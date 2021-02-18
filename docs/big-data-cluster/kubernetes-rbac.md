---
title: RBAC in Kubernetes
titleSuffix: SQL Server big data clusters
description: In diesem Artikel wird beschrieben, wie für Big Data-Cluster in SQL Server RBAC mit Kubernetes verwendet wird.
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 02/11/2021
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: f8759714fe2846a86f53a4974960b511f4b4c3dd
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100343891"
---
# <a name="kubernetes-rbac-model--impact-on-users-and-service-accounts-managing-bdc"></a>RBAC-Modell in Kubernetes und Auswirkungen auf Benutzer und Dienstkonten, die Big Data-Cluster verwalten

In diesem Artikel werden die Berechtigungsanforderungen für Big Data-Cluster verwaltende Benutzer und die Semantik für das Standarddienstkonto und den Kubernetes-Zugriff innerhalb des Big Data Clusters beschrieben.

> [!NOTE]
> Weitere Ressourcen zum RBAC-Modell in Kubernetes finden Sie unter [Verwenden der RBAC-Autorisierung](https://kubernetes.io/docs/reference/access-authn-authz/rbac/) und [Verwenden von RBAC zum Definieren und Anwenden von Berechtigungen](https://docs.openshift.com/container-platform/4.4/authentication/using-rbac.html).

## <a name="role-required-for-deployment"></a>Für die Bereitstellung erforderliche Rolle

Der Big Data-Cluster verwendet Dienstkonten (z. B. `sa-mssql-controller` oder `master`), um z. B. die Bereitstellung der Clusterpods, der Dienste, der Hochverfügbarkeit und der Überwachung zu orchestrieren. Wenn die Bereitstellung des Big Data-Clusters beginnt (z. B. `azdata bdc create`), übernimmt [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] folgende Aufgaben:

1. azdata überprüft, ob der bereitgestellte Namespace vorhanden ist.
2. Ist dies nicht der Fall, wird ein Namespace erstellt und die `MSSQL_CLUSTER`-Bezeichnung darauf angewendet.
3. azdata erstellt das `sa-mssql-controller`-Dienstkonto.
4. azdata erstellt eine `<namespaced>-admin`-Rolle mit vollständigen Berechtigungen für den Namespace oder das Projekt, jedoch ohne Berechtigungen auf Clusterebene.
5. azdata erstellt eine Rollenzuweisung dieses Dienstkontos zu der Rolle.

Sobald diese Schritte abgeschlossen sind, werden die Pods der Steuerungsebene bereitgestellt, und das Dienstkonto stellt den Rest des Big Data-Clusters bereit.  

Als Konsequenz benötigt der bereitstellende Benutzer Berechtigungen für die folgenden Aufgaben:

- Auflisten der Namespaces im Cluster (1)
- Patchen des neuen oder des vorhandenen Namespace mit der Bezeichnung (2)
- Erstellen des Dienstkontos `sa-mssql-controller`, der `<namespaced>-admin`-Rolle und der Rollenbindung (3–5)

Die `admin`-Standardrolle verfügt nicht über diese Berechtigungen, der den Big Data-Cluster bereitstellende Benutzer benötigt also mindestens Administratorberechtigungen auf Ebene des Namespace.

## <a name="cluster-role-required-for-pods-and-nodes-metrics-collection"></a>Zum Sammeln von Pod- und Knotenmetriken erforderliche Clusterrolle

Ab SQL Server 2019 CU5 ist für Telegraf ein Dienstkonto mit Berechtigungen für Rollen auf dem gesamten Cluster erforderlich, um die Pod- und Knotenmetriken zu erfassen. Bei der Bereitstellung (oder beim Upgrade für vorhandene Bereitstellungen) wird versucht, das erforderliche Dienstkonto und die Clusterrolle zu erstellen, aber wenn der Benutzer, der den Cluster bereitstellt oder das Upgrade durchführt, nicht die erforderlichen Berechtigungen hat, wird die Bereitstellung bzw. das Upgrade mit einer Warnung fortgesetzt und erfolgreich abgeschlossen. Allerdings werden die Pod- und Knotenmetriken in diesem Fall nicht gesammelt. Der den Cluster bereitstellende Benutzer muss einen Clusteradministrator bitten, die Rolle und das Dienstkonto zu erstellen (vor oder nach der Bereitstellung bzw. dem Upgrade). Nach der Erstellung der Rolle und des Dienstkontos werden diese vom Big Data-Cluster verwendet. 

Nachfolgend finden Sie die auszuführenden Schritte zum Erstellen der erforderlichen Artefakte:

1. Erstellen Sie die Datei *metrics-role.yaml* mit folgendem Inhalt. Stellen Sie sicher, dass Sie die Platzhalter *<clusterName>* durch den Namen Ihres Big Data-Clusters ersetzen.

   ```yaml
   apiVersion: rbac.authorization.k8s.io/v1
   kind: ClusterRole
   metadata:
     name: <clusterName>:cr-mssql-metricsdc-reader
   rules:
   - apiGroups:
     - '*'
     resources:
     - pods
     - nodes/stats
     - nodes/proxy
     verbs:
     - get
   ---
   apiVersion: rbac.authorization.k8s.io/v1
   kind: ClusterRoleBinding
   metadata:
     name: <clusterName>:crb-mssql-metricsdc-reader
   roleRef:
     apiGroup: rbac.authorization.k8s.io
     kind: ClusterRole
     name: <clusterName>:cr-mssql-metricsdc-reader
   subjects:
   - kind: ServiceAccount
     name: sa-mssql-metricsdc-reader
     namespace: <clusterName>
   ```

2. Erstellen Sie die Clusterrolle und die Clusterrollenbindung:

   ```bash
   kubectl create -f metrics-role.yaml
   ```

Das Dienstkonto, die Clusterrolle und die Clusterrollenbindung können entweder vor oder nach der Bereitstellung des Big Data-Clusters erstellt werden. Kubernetes aktualisiert die Berechtigung für das Telegraf-Dienstkonto automatisch. Wenn die Erstellung als Podbereitstellung erfolgt, tritt bei der Erfassung der Pod- und Knotenmetriken eine Verzögerung von einigen Minuten auf.

> [!NOTE]
> SQL Server 2019 CU5 führt zwei Funktionsparameter ein, um die Sammlung von Pod- und Knotenmetriken zu steuern. Standardmäßig sind diese Parameter in allen Umgebungszielen auf „True“ festgelegt, mit Ausnahme von OpenShift. Hier wird der Standardwert überschrieben. 

Sie können diese Einstellungen im Sicherheitsabschnitt in der Bereitstellungskonfigurationsdatei `control.json` anpassen:

```json
  "security": {
    …
    "allowNodeMetricsCollection": false,
    "allowPodMetricsCollection": false
  }
```

Wenn diese Einstellungen auf `false` festgelegt werden, wird im Workflow für die Bereitstellung des Big Data-Clusters nicht versucht, das Dienstkonto, die Clusterrolle und die Bindung für Telegraf zu erstellen.

## <a name="default-service-account-usage-from-within-a-bdc-pod"></a>Verwendung des Standarddienstkontos innerhalb eines BDC-Pods

Zugunsten eines strengeren Sicherheitsmodells deaktivierte SQL Server 2019 CU5 die standardmäßige Einbindung von Anmeldeinformationen für das Kubernetes-Standarddienstkonto in den BDC-Pods. Dies gilt sowohl für neue als auch für aktualisierte Bereitstellungen in CU5 oder höheren Versionen.
Das Anmeldeinformationstoken in den Pods kann für den Zugriff auf den Kubernetes-API-Server verwendet werden, und die Berechtigungsebene hängt von den Kubernetes-Autorisierungsrichtlinieneinstellungen ab. Wenn Sie über bestimmte Anwendungsfälle verfügen, für die das vorherige CU5-Verhalten wiederhergestellt werden muss, wird in CU6 ein neuer Funktionsschalter eingeführt, damit Sie die automatische Einbindung nur zum Zeitpunkt der Bereitstellung aktivieren können. Hierzu können Sie die Bereitstellungskonfigurationsdatei „control.json“ verwenden und *automountServiceAccountToken* auf *true* festlegen. Führen Sie den folgenden Befehl aus, um diese Einstellung in Ihrer benutzerdefinierten Konfigurationsdatei *control.json* mithilfe der [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] zu aktualisieren: 

``` bash
azdata bdc config replace -c custom-bdc/control.json -j "$.security.automountServiceAccountToken=true"
```
