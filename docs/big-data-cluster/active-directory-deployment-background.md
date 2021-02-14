---
title: Bereitstellen mehrerer SQL Server-Big Data-Cluster in einer Active Directory-Domäne
titleSuffix: SQL Server Big Data Cluster
description: Erfahren Sie, wie mehrere SQL Server Big Data-Cluster in einer einzelnen Active Directory-Domäne bereitgestellt werden.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 09/30/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: a8b82b4a5802467841ca345ac24c83e1356a79f8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040110"
---
# <a name="deploy-multiple-big-data-clusters-2019-in-the-same-active-directory-domain"></a>Bereitstellen mehrerer [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] in derselben Active Directory-Domäne

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

In diesem Artikel werden die Neuerungen im fünften kumulativen Update (Cumulative Update 2019, CU5) für SQL Server 2019 erläutert, die die Bereitstellung mehrerer SQL Server 2019-Big Data-Cluster in der gleichen Active Directory-Domäne (AD) sowie die Integration in dieselbe ermöglichen.

Vor SQL Server 2019 CU5 gab es zwei Probleme, die die Bereitstellung mehrerer Big Data-Cluster in einer AD-Domäne verhinderten.

- Namenskonflikt in Bezug auf Dienstprinzipalnamen und die DNS-Domäne
- Namen von Kontoprinzipalen in der Domäne

## <a name="object-name-collisions"></a>In Konflikte stehende Objektnamen

### <a name="service-principal-names-spn-and-dns-domain-naming-conflict"></a>Konflikt zwischen Dienstprinzipalnamen und dem DNS-Domänennamen

Der zur Bereitstellungszeit angegebene Domänenname wird als AD-DNS-Domäne verwendet. Dies bedeutet, dass die Pods im internen Netzwerk über diese DNS-Domäne eine Verbindung zueinander herstellen können. Zudem verwenden Benutzer diese DNS-Domäne, um eine Verbindung zu den BDC-Endpunkten herzustellen. Aus diesem Grund muss jeder Dienstprinzipalname (Service Principal Name, SPN), der innerhalb des BDC für einen Dienst erstellt wird, den Namen des Kubernetes-Pods, -Diensts oder -Endpunkts bei dieser AD-DNS-Domäne qualifizieren lassen. Wenn ein Benutzer einen zweiten Cluster in der Domäne bereitstellt, verfügt der generierte SPN über den gleichen vollqualifizierten Domänennamen, denn weder die Podnamen noch der DNS-Domänenname unterscheiden sich zwischen den beiden Clustern. Angenommen, die AD-DNS-Domäne lautet `contoso.local`. Einer der SPNs, die für die Masterpool-SQL Server-Instanz in Pod `master-0` generiert werden, lautet `MSSQLSvc/master-0.contoso.local:1433`. Der Podname für `master-0` ist im zweiten Cluster, den der Benutzer bereitstellt, identisch, und der Benutzer stellt dieselbe AD-DNS-Domäne (``contoso.local``) bereit. Dies ergibt die gleiche SPN-Zeichenfolge. In Active Directory ist die Erstellung eines in Konflikt stehenden Dienstprinzipalnames jedoch verboten, was zu einem Bereitstellungsfehler für den zweiten Cluster führt.

### <a name="domain-account-principal-names"></a>Namen von Kontoprinzipalen in der Domäne

Während einer Bereitstellung eines BDC in einer Active Directory-Domäne werden mehrere Kontoprinzipale für Dienste generiert, die im BDC ausgeführt werden. Dabei handelt es sich im Wesentlichen um AD-Benutzerkonten. Vor SQL Server 2019 CU5 waren die Namen dieser Konten unter Clustern nicht eindeutig. Dies zeigte sich, wenn der gleiche Benutzerkontoname für einen bestimmten Dienst im BDC in zwei verschiedenen Clustern erstellt werden sollte. Der zuletzt bereitgestellte Cluster erzeugte einen Konflikt in AD und konnte nicht erstellt werden.

## <a name="resolution-for-collisions"></a>Lösungen für Namenskonflikte

### <a name="solution-to-solve-the-problem-with-spns-and-dns-domain---sql-2019-cu5"></a>Lösung des Problems mit Dienstprinzipalnamen und DNS-Domäne – SQL Server 2019 CU5

Da sich die SPNs zwischen zwei Clustern unterscheiden müssen, muss sich auch der zur Bereitstellungszeit übergegebene DNS-Domänenname unterscheiden. Mit der neu in der Bereitstellungskonfigurationsdatei eingeführten Einstellung `subdomain` können Sie von nun an unterschiedliche DNS-Namen angeben. Wenn sich die Unterdomäne zwischen zwei Clustern unterscheidet und die interne Kommunikation über diese Unterdomäne erfolgen kann, enthalten die SPNs die Unterdomäne, wodurch die erforderliche Eindeutigkeit erreicht wird.

>[!NOTE]
>Der Wert, der über die Unterdomäneneinstellung übergeben wird, ist keine neue AD-Domäne, sondern eine intern verwendete DNS-Domäne.

Denken Sie noch einmal an das Beispiel mit dem SPN der Masterpool-SQL Server-Instanz. Wenn die Unterdomäne `bdc` lautet, ändert sich der zuvor erwähnte SPN in `MSSQLSvc/master-0.bdc.contoso.local:1433`.  

Die Wertanpassung des neu eingeführten Unterdomänenparameters in der Active Directory-Konfigurationsspezifikation ist optional. Standardmäßig wird der Name des Big Data-Clusters oder -Namespace verwendet, um den Wert der Unterdomäneneinstellung zu berechnen. Wenn Benutzer den Namen der Unterdomäne überschreiben möchten, können sie dazu den neuen Unterdomänenparameter verwenden, der in der Active Directory-Konfigurationsspezifikation eingeführt wird.

### <a name="solution-to-solve-the-problem-regarding-account-names-uniqueness"></a>Lösung: Problem in Bezug auf die Eindeutigkeit von Kontonamen

Damit Kontonamen auf ein Schema aktualisiert werden können, das Eindeutigkeit gewährleistet, wurde das Konzept des Kontopräfixes eingeführt. Das Kontopräfix ist ein Teil des Kontonamens, der über alle Cluster hinweg eindeutig ist. Der übrige Teil des Kontonamens bleibt für einen bestimmten Dienst immer identisch. Das neue Format des Kontonamens lautet wie folgt: `<prefix>-<name>-<podId>`. 

>[!NOTE]
>In Active Directory sind Kontonamen auf eine Länge von 20 Zeichen beschränkt. Der Big Data-Cluster muss 8 Zeichen zum Unterscheiden von Pods und StatefulSets verwenden. Für das Kontopräfix sind also maximal 12 Zeichen verfügbar.

Die Anpassung des Kontonamens ist optional. Verwenden Sie den Parameter `accountPrefix` aus der Active Directory-Konfigurationsspezifikation. `accountPrefix` wurde mit SQL Server 2019 CU5 in die Konfigurationsspezifikation eingeführt. Der Unterdomänenname wird standardmäßig als Kontopräfix verwendet. Wenn der Unterdomänenname länger als 12 Zeichen ist, werden die ersten 12 Zeichen des Unterdomänennamens als Kontopräfix verwendet.

Die Unterdomäne gilt nur für das DNS. Daher lautet der Name des neuen LDAP-Benutzerkontos `bdc-ldap@contoso.local`, nicht `bdc-ldap@bdc.contoso.local`.

## <a name="semantics"></a>Semantik

Nachstehend finden Sie eine Übersicht über die Semantik der Parameter, die in SQL Server 2019 CU5 für mehrere, in einer einzelnen Domäne bereitgestellte Cluster hinzugefügt wurden:

### `subdomain`

- Optionales Feld
- Datentyp: String
- Definition: Eine eindeutige DNS-Unterdomäne, die für diesen Big Data-Cluster verwendet werden soll. Dieser Wert muss für jeden Cluster, der in der Active Directory-Domäne bereitgestellt wird, unterschiedlich sein.  
- Standardwert: Wenn nicht angegeben, wird der Clustername als Standardwert verwendet.
- Maximale Länge: 63 Zeichen pro Bezeichnung (wobei jede durch einen Punkt getrennte Zeichenfolge als Bezeichnung gilt)
- Anmerkungen: Die DNS-Namen des Endpunkts sollten die Unterdomäne in ihrem vollqualifizierten Domänennamen verwenden.

### `accountPrefix`

- Optionales Feld
- Datentyp: String
- Definition: Ein eindeutiges Präfix für AD-Konten, das einen Big Data-Cluster generiert Dieser Wert muss für jeden Cluster, der in der Active Directory-Domäne bereitgestellt wird, unterschiedlich sein.
- Standardwert: Wenn nicht angegeben, wird die Unterdomäne als Standardwert verwendet. Wenn keine Unterdomäne bereitgestellt wird, wird der Clustername als Name der Unterdomäne verwendet. Daher wird der Clustername ebenfalls als accountPrefix geerbt. Wenn die Unterdomäne bereitgestellt wird und über einen mehrteiligen Namen verfügt (also mindestens einen Punkt enthält), muss der Benutzer accountPrefix angeben. 
- Maximale Länge: 12 Zeichen 

## <a name="impact-on-ad-domain-and-dns-server"></a>Auswirkung auf die AD-Domäne und den DNS-Server 

Es sind keine Änderungen in der AD-Domäne oder am Domänencontroller erforderlich, um die Bereitstellung mehrerer BDCs in der gleichen Active Directory-Domäne zu ermöglichen. Die DNS-Unterdomäne wird beim Registrieren externer Endpunkt-DNS-Namen automatisch auf dem DNS-Server erstellt. 

## <a name="impact-on-setting-up-the-deployment-configuration-file-used-for-the-bdc-deployment"></a>Auswirkung auf das Einrichten der Bereitstellungskonfigurationsdatei für die BDC-Bereitstellung 

Der Abschnitt *activeDirectory* in der Konfigurationsdatei der Steuerungsebene *control.json* enthält zwei neue optionale Parameter: `subdomain` und `accountPrefix`. Geben Sie für diese Einstellungen nur Werte an, wenn Sie das Standardverhalten überschreiben möchten. Gemäß dem Standardverhalten wird der Clustername für jede dieser Einstellungen verwendet. Der Clustername entspricht dem Namen des Namespace.

Zudem können Sie Endpunkt-DNS-Namen Ihrer Wahl verwenden, solange diese vollqualifiziert sind und zu keinem Konflikt zwischen zwei Big Data-Clustern führen, die in derselben Domäne bereitgestellt werden. Optional können Sie den Parameterwert der Unterdomäne verwenden, um sicherzustellen, dass DNS-Namen sich zwischen Clustern unterscheiden.  Ein Beispiel wäre der Gatewayendpunkt. Wenn Sie den Endpunkt `gateway` nennen und ihn im Rahmen der BDC-Bereitstellung automatisch auf dem DNS-Server registrieren möchten, verwenden Sie `gateway.bdc1.contoso.local` als DNS-Namen, wenn die Unterdomäne `bdc1` lautet und `contoso.local` der AD-DNS-Domänenname ist. Weitere zulässige Werte sind `gateway-bdc1.contoso.local` oder schlicht `gateway.contoso.local`.

## <a name="examples"></a>Beispiele

Im Folgenden finden Sie ein Beispiel für die Active Directory-Sicherheitskonfiguration, falls Sie die Unterdomäne und accountPrefix überschreiben möchten. 

```json
    "security": { 
        "activeDirectory": { 
            "ouDistinguishedName":"OU=contosoou,DC=contoso,DC=local", 
            "dnsIpAddresses": [ "10.10.10.10" ], 
            "domainControllerFullyQualifiedDns": [ "contoso-win2016-dc.contoso.local" ], 
            "domainDnsName":"contoso.local", 
            "subdomain": "bdc", 
            "accountPrefix": "myprefix", 
            "clusterAdmins": [ "contosoadmins" ], 
            "clusterUsers": [ "contosousers1", "contosousers2" ] 
        } 
    } 
  
```

Im Folgenden finden Sie ein Beispiel für die Endpunktspezifikation für Endpunkte der Steuerungsebene. Sie können beliebige Werte für DNS-Namen verwenden, solange diese eindeutig und vollqualifiziert sind:
  
```json
        "endpoints": [ 
            { 
                "serviceType": "NodePort", 
                "port": 30080, 
                "name": "Controller", 
                "dnsName": "control-bdc1.contoso.local" 
            }, 
            { 
                "serviceType": "NodePort", 
                "port": 30777, 
                "name": "ServiceProxy", 
                "dnsName": "monitor-bdc1.contoso.local" 
            } 
        ] 
  
```

## <a name="questions"></a>Fragen

### <a name="do-you-need-to-create-separate-ous-for-different-clusters"></a>Müssen separate Organisationseinheiten für verschiedene Cluster erstellt werden?

Dies ist nicht verpflichtend, jedoch empfehlenswert. Separate Organisationseinheiten für separate Cluster erleichtern Ihnen die Verwaltung der generierten Benutzerkonten.

### <a name="how-to-revert-back-to-the-pre-cu5-behavior-in-sql-2019"></a>Wie kann in SQL Server 2019 das Verhalten vor CU5 wiederhergestellt werden?

Möglicherweise gibt es Szenarios, in denen Sie den neu eingeführten Parameter `subdomain` nicht unterstützen können, beispielsweise wenn Sie ein Release vor CU5 bereitstellen müssen und die [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] bereits aktualisiert haben. Dies ist zwar sehr unwahrscheinlich, wenn Sie das Verhalten vor dem CU5-Release jedoch wiederherstellen müssen, können Sie im Active Directory-Abschnitt von `control.json` den Parameter `useSubdomain` auf `false` festlegen.

Im folgenden Beispiel wird `useSubdomain` in diesem Szenario auf `false` festgelegt.

```console
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.useSubdomain=false" 
```

## <a name="next-steps"></a>Nächste Schritte

[Behandeln von Problemen mit der Integration von Active Directory in SQL Server-Big Data-Clustern](troubleshoot-active-directory.md)
