---
title: Bereitstellen in OpenShift
titleSuffix: SQL Server Big Data Cluster
description: Erfahren Sie, wie Sie für SQL Server-Big Data-Cluster in OpenShift ein Upgrade vornehmen.
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: d78e229bcbf2a088d42431abdf02bec3f9e51eab
ms.sourcegitcommit: 62c7b972db0ac28e3ae457ce44a4566ebd3bbdee
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2021
ms.locfileid: "103231498"
---
# <a name="deploy-big-data-clusters-2019-on-openshift-on-premises-and-azure-red-hat-openshift"></a>Bereitstellen von [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] lokal auf OpenShift und Azure Red Hat OpenShift

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

In diesem Artikel wird erläutert, wie Sie einen SQL Server-Big Data-Cluster (BDC) in OpenShift-Umgebungen, lokal oder in Azure Red Hat OpenShift (ARO) bereitstellen.

> [!TIP]
> Eine schnelle Möglichkeit zum Bootstrapping einer Beispielumgebung mithilfe von ARO und der anschließenden Bereitstellung eines BDCs auf dieser Plattform ist die Verwendung des Python-Skripts, das [hier](quickstart-big-data-cluster-deploy-aro.md) verfügbar ist.


SQL Server 2019 CU5 bietet Unterstützung für SQL Server-Big Data-Cluster in OpenShift. Sie können Big Data-Cluster für lokale OpenShift-Instanzen oder auf Azure Red Hat OpenShift (ARO) bereitstellen. Die Bereitstellung erfordert mindestens die OpenShift-Clusterversion 4.3. Obwohl der Bereitstellungsworkflow ähnlich wie bei anderen auf Kubernetes basierten Plattformen ist ([kubeadm](deploy-with-kubeadm.md) und [AKS](deploy-on-aks.md)), gibt es dennoch Unterschiede. Der Unterschied besteht hauptsächlich in Bezug auf das Ausführen von Anwendungen als nicht Stammbenutzer und den Sicherheitskontext, der für den Namespace verwendet wird, in dem BDC bereitgestellt wird.

Informationen zum lokalen Bereitstellen des OpenShift-Clusters finden Sie in der [Red Hat OpenShift-Dokumentation](https://docs.openshift.com/container-platform/4.3/release_notes/ocp-4-3-release-notes.html#ocp-4-3-installation-and-upgrade). Informationen zu den Bereitstellungen von ARO finden Sie in der [Azure Red Hat OpenShift-Dokumentation](/azure/openshift/intro-openshift).

In diesem Artikel werden die Bereitstellungsschritte beschrieben, die für die OpenShift-Plattform spezifisch sind. Dabei werden die Optionen für den Zugriff auf die Zielumgebung und der Namespace aufgeführt, den Sie zum Bereitstellen des Big Data-Clusters verwenden.

## <a name="pre-requisites"></a>Voraussetzungen

> [!IMPORTANT]
> Die folgenden Voraussetzungen müssen von einem OpenShift-Clusteradministrator (Clusterrolle „Clusteradministrator“) ausgeführt werden, der über ausreichende Berechtigungen zum Erstellen dieser Clusterebenenobjekte verfügt. Weitere Informationen zu Clusterrollen in OpenShift finden Sie unter [Verwenden von RBAC zum Definieren und Anwenden von Berechtigungen](https://docs.openshift.com/container-platform/4.4/authentication/using-rbac.html).

1. Stellen Sie sicher, dass die `pidsLimit`-Einstellung auf OpenShift aktualisiert wurde, um SQL Server-Workloads zu unterstützen. Der Standardwert in OpenShift ist zu niedrig für Produktionsumgebungen wie Workloads. Beginnen Sie mindestens mit `4096`. Der optimale Wert hängt jedoch von der Einstellung von `max worker threads` in SQL Server und der Anzahl der CPU-Prozessoren auf dem OpenShift-Hostknoten ab. 
    - Befolgen Sie `pidsLimit`, um herauszufinden, wie Sie das [pidsLimit]( https://github.com/openshift/machine-config-operator/blob/master/docs/ContainerRuntimeConfigDesign.md) für Ihren OpenShift-Cluster aktualisieren. Beachten Sie, dass OpenShift-Versionen vor `4.3.5` einen Fehler aufweisen, der dazu führt, dass der aktualisierte Wert nicht übernommen wird. Aktualisieren Sie OpenShift daher unbedingt auf die neueste Version. 
    - Wenn Sie den optimalen Wert abhängig von Ihrer Umgebung und den geplanten SQL Server-Workloads berechnen möchten, können Sie die folgende Schätzung und die folgenden Beispiele verwenden:

    |Anzahl der Prozessoren|Max. Anzahl von Arbeitsthreads|Standardworker pro Prozessor|Minimaler pidsLimit-Wert|
    |--------------------|--------------------------|-----------------------------|-----------------------|
    |          64        |           512            |             16              | 512 + (64 × 16) = 1536 |
    |         128        |           512            |             32              | 512 + (128 × 32) = 4608 |

    > [!NOTE]
    > Andere Prozesse (z. B. Sicherungen, CLR, Fulltext, SQLAgent) führen ebenfalls zu erhöhtem Aufwand. Fügen Sie also dem geschätzten Wert einen Puffer hinzu.

1. Laden Sie die benutzerdefinierte Sicherheitskontexteinschränkung (Security Context Constraint, SCC) [`bdc-scc.yaml`](#bdc-sccyaml-file)herunter:

    ```console
    curl https://raw.githubusercontent.com/microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/deployment/openshift/bdc-scc.yaml -o bdc-scc.yaml
    ```

1. Wenden Sie die SCC auf den Cluster an.

    ```console
    oc apply -f bdc-scc.yaml
    ```

    > [!NOTE]
    > Die benutzerdefinierte SCC für BDC basiert auf der integrierten `nonroot`-SCC in OpenShift mit zusätzlichen Berechtigungen. Weitere Informationen zu Sicherheitskontexteinschränkungen in OpenShift finden Sie unter [Verwalten von Sicherheitskontexteinschränkungen](https://docs.openshift.com/container-platform/4.3/authentication/managing-security-context-constraints.html). Ausführliche Informationen darüber, welche Berechtigungen für Big Data-Cluster zusätzlich zur `nonroot`-SCC erforderlich sind, finden Sie in [diesem Whitepaper](https://aka.ms/sql-bdc-openshift-security).

3. Erstellen eines Namespace/Projekts:

   ```console
   oc new-project <namespaceName>
   ```

4. Weisen Sie die benutzerdefinierte SCC den Dienstkonten für Benutzer innerhalb des Namespace zu, in dem BDC bereitgestellt wird:

   ```console
   oc create rolebinding bdc-rbac --clusterrole=system:scc:bdc-scc --group=system:serviceaccounts:<namespace>
   ```

5. Weisen Sie den Benutzern, die BDC bereitstellen, die entsprechende Berechtigung zu. Führen Sie einen der folgenden Schritte aus: 

   - Wenn der Benutzer, der BDC bereitstellt, über eine Clusteradministratorrolle verfügt, fahren Sie mit dem [Bereitstellen eines Big Data-Clusters](#deploy-big-data-cluster) fort.

   - Wenn der Benutzer, der BDC bereitstellt, ein Namespaceadministrator ist, weisen Sie diesem die lokale Rolle „Clusteradministrator“ für den erstellten Namespace zu. Dies ist die bevorzugte Option für den Benutzer, der den Big Data-Cluster bereitstellt und verwaltet, um Administratorberechtigungen auf Namespaceebene zu erhalten.

   ```console
   oc adm policy add-role-to-user cluster-admin <deployingUser> -n <namespaceName>
   ```

   Der Benutzer, der den Big Data-Cluster bereitstellt, muss sich dann bei der OpenShift-Konsole anmelden:

   ```console
   oc login -u <deployingUser> -p <password>
   ```

## <a name="deploy-big-data-cluster"></a>Bereitstellen eines Big Data-Clusters

1. Installieren Sie das neueste [azdata](../azdata/install/deploy-install-azdata.md)-Hilfsprogramm.

1. Klonen Sie eine der integrierten Konfigurationsdateien für OpenShift, abhängig von Ihrer Zielumgebung (OpenShift lokal oder ARO) und dem Bereitstellungsszenario. Informationen zu den Einstellungen, die für OpenShift in den integrierten Konfigurationsdateien spezifisch sind, finden Sie weiter unten im Abschnitt *Für OpenShift spezifische Einstellung in Bereitstellungskonfigurationsdateien*. Weitere Informationen zu verfügbaren Konfigurationsdateien finden Sie im [Bereitstellungsleitfaden](deployment-guidance.md).

   Listen Sie nun alle verfügbaren integrierten Konfigurationsdateien auf.

   ```console
   azdata bdc config list
   ```

   Führen Sie den folgenden Befehl aus, um eine der integrierten Konfigurationsdateien zu klonen. (Alternativ können Sie auch das auf Ihrer Zielplattform bzw. Ihrem Zielszenario basierende Profil ersetzen):

   ```console
   azdata bdc config init --source openshift-dev-test --target custom-openshift
   ```

   Beginnen Sie für eine Bereitstellung unter ARO mit einem der `aro-`-Profile, das für diese Umgebung geeignete Standardwerte für `serviceType` und `storageClass` enthält. Beispiel:

   ```console
   azdata bdc config init --source aro-dev-test --target custom-openshift
   ```

1. Passen Sie die Konfigurationsdateien „control.json“ und „bdc.json“ an. Im Folgenden finden Sie einige zusätzliche Ressourcen, die Sie durch die für verschiedene Anwendungsfälle unterstützten Anpassungen führen:

   - [Storage](concept-data-persistence.md)
   - [AD-bezogene Einstellungen](active-directory-deploy.md)
   - [Weitere Anpassungen](deployment-custom-configuration.md)

   > [!NOTE]
   > Die Integration mit Azure Active Directory für BDC wird nicht unterstützt. Daher können Sie diese Authentifizierungsmethode nicht verwenden, wenn Sie in ARO bereitstellen.

1. Legen Sie [Umgebungsvariablen](deployment-guidance.md#env) fest.

1. Stellen Sie den Big Data-Cluster bereit.

   ```console
   azdata bdc create --config custom-openshift --accept-eula yes
   ```

1. Bei erfolgreicher Bereitstellung können Sie sich anmelden und die externen Clusterendpunkte auflisten:

   ```console
      azdata login -n mssql-cluster
      azdata bdc endpoint list
   ```

## <a name="openshift-specific-settings-in-the-deployment-configuration-files"></a>OpenShift-spezifische Einstellungen in den Bereitstellungskonfigurationsdateien

SQL Server 2019 CU5 führt zwei Funktionsparameter ein, um die Sammlung von Pod- und Knotenmetriken zu steuern. Diese Parameter werden in den integrierten Profilen für OpenShift standardmäßig auf `false` festgelegt, da für die Überwachungscontainer [privilegierter Sicherheitskontext](https://www.openshift.com/blog/managing-sccs-in-openshift) erforderlich ist. Dadurch werden einige der Sicherheitseinschränkungen für den Namespace gelockert, auf dem BDC bereitgestellt ist.

```json
    "security": {
      "allowNodeMetricsCollection": false,
      "allowPodMetricsCollection": false
}
```

Der Name der Standardspeicherklasse in ARO ist „managed-premium“ (im Gegensatz zu AKS, bei dem die Standardspeicherklasse „default“, also Standard, genannt wird). Diese Informationen finden Sie in der `control.json`-Datei, die `aro-dev-test` und `aro-dev-test-ha` entspricht:

```json
    },
    "storage": {
      "data": {
        "className": "managed-premium",
        "accessMode": "ReadWriteOnce",
        "size": "15Gi"
      },
      "logs": {
        "className": "managed-premium",
        "accessMode": "ReadWriteOnce",
        "size": "10Gi"
      }
```

## <a name="bdc-sccyaml-file"></a>`bdc-scc.yaml`-Datei

Die SCC-Datei für diese Bereitstellung ist:

:::code language="yaml" source="../../sql-server-samples/samples/features/sql-big-data-cluster/deployment/openshift/bdc-scc.yaml":::

## <a name="next-steps"></a>Nächste Schritte

[Tutorial: Laden von Beispieldaten in einen Big Data-Cluster für SQL Server](tutorial-load-sample-data.md)
