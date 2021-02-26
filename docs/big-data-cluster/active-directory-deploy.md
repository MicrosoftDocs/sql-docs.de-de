---
title: Bereitstellen im Active Directory-Modus
titleSuffix: SQL Server Big Data Cluster
description: Erfahren Sie, wie Sie für einen SQL Server-Big Data-Cluster in einer Active Directory-Domäne ein Upgrade durchführen.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 02/11/2021
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 799afc246b106c4b49d6aba44f8d26a761d6c2cc
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100343964"
---
# <a name="deploy-sql-server-big-data-cluster-in-active-directory-mode"></a>Bereitstellen eines SQL Server-Big Data-Clusters im Active Directory-Modus

In diesem Artikel wird die Bereitstellung von SQL Server Big Data-Clustern im Active Directory-Modus beschrieben. Für die in diesem Artikel erläuterten Schritte ist Zugriff auf eine Active Directory-Domäne erforderlich. Bevor Sie fortfahren, müssen Sie die unter [Bereitstellen von [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] im Active Directory Modus](active-directory-prerequisites.md) beschriebenen Anforderungen erfüllen.

## <a name="prepare-deployment"></a>Vorbereiten der Bereitstellung

Um einen BDC mit AD-Integration bereitzustellen, müssen einige zusätzliche Informationen zum Erstellen der BDC-bezogenen Objekte in AD angegeben werden.

Durch Verwendung des Profils `kubeadm-prod` (oder `openshift-prod` ab dem CU5-Release) verfügen Sie automatisch über Platzhalter für die sicherheits- und endpunktbezogenen Informationen, die für die AD-Integration benötigt werden.

Darüber hinaus müssen Sie Anmeldeinformationen angeben, die [!INCLUDE[big-data-clusters](../includes/ssbigdataclusters-nover.md)] zum Erstellen der erforderlichen Objekte in AD verwendet. Diese Anmeldeinformationen werden als Umgebungsvariablen angegeben.

## <a name="set-security-environment-variables"></a>Festlegen von sicherheitsbezogenen Umgebungsvariablen

Mit den folgenden Umgebungsvariablen werden die Anmeldeinformationen für das BDC-Domänendienstkonto angegeben, das zum Einrichten der AD-Integration verwendet wird. Dieses Konto wird vom BDC außerdem dazu verwendet, die BDC-bezogenen AD-Objekte im weiteren Verlauf zu verwalten.

```bash
export DOMAIN_SERVICE_ACCOUNT_USERNAME=<AD principal account name>
export DOMAIN_SERVICE_ACCOUNT_PASSWORD=<AD principal password>
```

## <a name="provide-security-and-endpoint-parameters"></a>Angeben von Sicherheits- und Endpunktparametern

Zusätzlich zu den Umgebungsvariablen für die Anmeldeinformationen müssen Sie Sicherheits- und Endpunktinformationen angeben, damit die AD-Integration funktioniert. Die erforderlichen Parameter sind im `kubeadm-prod`/`openshift-prod`-[Bereitstellungsprofil](deployment-guidance.md#configfile) bereits enthalten.

Für die AD-Integration sind die folgenden Parameter erforderlich. Fügen Sie diese Parameter zu den Dateien `control.json` und `bdc.json` hinzu, indem Sie die später in diesem Artikel beschriebenen `config replace`-Befehle verwenden. In allen nachstehenden Beispielen wird die Domäne `contoso.local` verwendet.

- `security.activeDirectory.ouDistinguishedName`: DN (Distinguished Name) einer Organisationseinheit (OE), in der alle während der Clusterbereitstellung erstellten AD-Konten gespeichert werden. Wenn die Domäne den Namen `contoso.local` aufweist, lautet der DN der Organisationseinheit `OU=BDC,DC=contoso,DC=local`.

- `security.activeDirectory.dnsIpAddresses` enthält die Liste der IP-Adressen der DNS-Server der Domäne. 

- `security.activeDirectory.domainControllerFullyQualifiedDns`: Liste der FQDNs der Domänencontroller. Der FQDN enthält den Computer-/Hostnamen eines Domänencontrollers. Wenn Sie über mehrere Domänencontroller verfügen, können Sie hier eine Liste angeben. Beispiel: `HOSTNAME.CONTOSO.LOCAL`.

  > [!IMPORTANT]
  > Wenn mehrere Domänencontroller eine Domäne bereitstellen, verwenden Sie den primären Domänencontroller als ersten Eintrag in der `domainControllerFullyQualifiedDns`-Liste der Sicherheitskonfiguration. Geben Sie `netdom query fsmo` zum Abrufen des Namens des primären Domänencontrollers in die Eingabeaufforderung ein, und drücken Sie dann die **EINGABETASTE**.

- **Optionaler Parameter** `security.activeDirectory.realm`: In den meisten Fällen entspricht der Bereich dem Domänennamen. Falls sich Bereich und Domänenname unterscheiden, verwenden Sie diesen Parameter zum Definieren des Bereichs (z. B. `CONTOSO.LOCAL`). Der für diesen Parameter angegebene Wert sollte vollqualifiziert sein.

- `security.activeDirectory.netbiosDomainName` **Optionaler Parameter**: Dies ist der NETBIOS-Name der AD-Domäne. In den meisten Fällen ist dies die erste Bezeichnung des AD-Domänennamens. Verwenden Sie bei Abweichungen diesen Parameter, um den NetBIOS-Domänennamen zu definieren. Dieser Wert darf keine Punkte enthalten. Dieser Name wird normalerweise verwendet, um die Benutzerkonten in der Domäne zu qualifizieren. Beispiel: Bei CONTOSO\user ist CONTOSO der NETBIOS-Domänenname.

  > [!NOTE]
  > Unterstützung mithilfe von *security.activeDirectory.netbiosDomainName* für eine Konfiguration, bei der sich der Active Directory-Domänenname vom **NETBIOS**-Namen der Active Directory-Domäne unterscheidet, wurde ab SQL Server 2019 CU9 aktiviert.

- `security.activeDirectory.domainDnsName`: Dies ist der Name Ihrer DNS-Domäne, die für den Cluster verwendet wird (z. B. `contoso.local`).

- `security.activeDirectory.clusterAdmins`: Dieser Parameter akzeptiert eine AD-Gruppe. Der AD-Gruppenbereich muss universell oder global sein. Mitglieder dieser Gruppe verfügen über die Clusterrolle `bdcAdmin`, über die sie die Administratorberechtigungen im Cluster erhalten. Das bedeutet, dass sie über [`sysadmin`-Berechtigungen in SQL Server](../relational-databases/security/authentication-access/server-level-roles.md#fixed-server-level-roles), [`superuser`-Berechtigungen in HDFS](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#The_Super-User) und Administratorberechtigungen bei bestehender Verbindung mit dem Controllerendpunkt verfügen.

  >[!IMPORTANT]
  >Erstellen Sie diese Gruppe in AD, bevor Sie mit der Bereitstellung beginnen. Wenn der Bereich für diese AD-Gruppe „domain local“ ist, schlägt die Bereitstellung fehl.

- `security.activeDirectory.clusterUsers`: Liste der AD-Gruppen, die als reguläre Benutzer (ohne Administratorberechtigungen) im Big Data-Cluster fungieren. Die Liste darf AD-Gruppen enthalten, die auf Gruppen der Kategorien „universal“ oder „domain global“ begrenzt sind. Dabei darf es sich nicht um lokale Domänengruppen handeln.

AD-Gruppen in dieser Liste werden der Big Data-Clusterrolle `bdcUser` zugeordnet. Ihnen muss der Zugriff auf SQL Server auf SQL Server (siehe [SQL Server-Berechtigungen](../relational-databases/security/permissions-hierarchy-database-engine.md)) oder HDFS (siehe [Anleitung zu HDFS-Berechtigungen](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#:~:text=Permission%20Checks%20%20%20%20Operation%20%20,%20%20N%2FA%20%2029%20more%20rows%20)) gewährt werden. Wenn eine Verbindung mit dem Controllerendpunkt besteht, können diese Benutzer mit dem Befehl `azdata bdc endpoint list` nur die Endpunkte auflisten, die im Cluster verfügbar sind.

Ausführliche Informationen zum Aktualisieren der AD-Gruppen für diese Einstellungen finden Sie unter [Verwalten des Zugriffs auf Big Data-Cluster im Active Directory-Modus](manage-user-access.md).

  >[!TIP]
  >Wenn die HDFS-Suchfunktion aktiviert werden soll, wenn eine Verbindung mit einem SQL Server-Master in Azure Data Studio besteht, muss einem Benutzer mit der Rolle „bdcUser“ die Berechtigung VIEW SERVER STATE zugewiesen werden, da Azure Data Studio die dynamische Verwaltungssicht `sys.dm_cluster_endpoints` verwendet, damit der erforderliche Knox-Gatewayendpunkt eine Verbindung mit HDFS herstellt.

  >[!IMPORTANT]
  >Erstellen Sie diese Gruppen in AD, bevor Sie mit der Bereitstellung beginnen. Wenn der Bereich für eine dieser AD-Gruppen „domain local“ ist, schlägt die Bereitstellung fehl.

  >[!IMPORTANT]
  >Wenn Ihre Domänenbenutzer über eine große Anzahl an Gruppenmitgliedschaften verfügen, sollten Sie die Werte für die Gatewayeinstellung `httpserver.requestHeaderBuffer` (der Standardwert ist `8192`) und die HDFS-Einstellung `hadoop.security.group.mapping.ldap.search.group.hierarchy.levels` (der Standardwert ist `10`) mithilfe der benutzerdefinierten Bereitstellungskonfigurationsdatei *bdc.json* anpassen. Dies ist eine bewährte Methode zum Vermeiden von Verbindungstimeouts des Gateways und/oder HTTP-Antworten mit dem Statuscode 431 (*Anforderungsheaderfelder zu groß*). Im folgenden Abschnitt der Konfigurationsdatei wird gezeigt, wie die Werte dieser Einstellungen definiert werden und welche Werte für eine höhere Anzahl an Gruppenmitgliedschaften empfohlen werden:

```json
{
    ...
    "spec": {
        "resources": {
            ...
            "gateway": {
                "spec": {
                    "replicas": 1,
                    "endpoints": [{...}],
                    "settings": {
                        "gateway-site.gateway.httpserver.requestHeaderBuffer": "65536"
                    }
                }
            },
            ...
        },
        "services": {
            ...
            "hdfs": {
                "resources": [...],
                "settings": {
                  "core-site.hadoop.security.group.mapping.ldap.search.group.hierarchy.levels": "4"
                }
            },
            ...
        }
    }
}
```

  >[!IMPORTANT]
  >Erstellen Sie die Gruppen für die unten aufgeführten Einstellungen in AD, bevor die Bereitstellung beginnt. Wenn der Bereich für eine dieser AD-Gruppen „domain local“ ist, schlägt die Bereitstellung fehl.

- **Optionaler Parameter** `security.activeDirectory.appOwners`: Liste der AD-Gruppen, die über Berechtigungen zum Erstellen, Löschen und Ausführen beliebiger Anwendungen verfügen. Die Liste darf AD-Gruppen enthalten, die auf Gruppen der Kategorien „universal“ oder „domain global“ begrenzt sind. Dabei darf es sich nicht um lokale Domänengruppen handeln.

- **Optionaler Parameter** `security.activeDirectory.appReaders`: Dieser Parameter ist eine Liste der AD-Gruppen, die über Berechtigungen zum Ausführen einer beliebigen Anwendung verfügen. Die Liste darf AD-Gruppen enthalten, die auf Gruppen der Kategorien „universal“ oder „domain global“ begrenzt sind. Dabei darf es sich nicht um lokale Domänengruppen handeln.

In der folgenden Tabelle wird das Autorisierungsmodell für die Anwendungsverwaltung gezeigt:

|   Autorisierte Rollen   |   [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]-Befehl   |
|----------------------|--------------------|
|   appOwner           | azdata app create  |
|   appOwner           | azdata app update  |
|   appOwner, appReader| azdata app list    |
|   appOwner, appReader| azdata app describe|
|   appOwner           | azdata app delete  |
|   appOwner, appReader| azdata app run     |

- `security.activeDirectory.subdomain`: Dieser **optionale Parameter** wurde mit dem Release von SQL Server 2019 CU5 eingeführt, um die Bereitstellung mehrerer Big Data-Cluster für dieselbe Domäne zu unterstützen. Mithilfe dieser Einstellung können Sie verschiedene DNS-Namen für jeden bereitgestellten Big Data-Cluster angeben. Wenn der Wert dieses Parameters nicht im Active Directory-Abschnitt der `control.json`-Datei angegeben ist, wird standardmäßig der Name des Big Data-Clusters (identisch mit dem Namen des Kubernetes-Namespace) verwendet, um den Wert der Unterdomäneneinstellung zu berechnen. 

  >[!NOTE]
  >Der Wert, der über die Unterdomäneneinstellung übergeben wird, ist keine neue AD-Domäne, sondern eine DNS-Domäne, die intern vom Big Data-Cluster verwendet wird.

  >[!IMPORTANT]
  >Sie müssen die neueste Version der **[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]** ab dem SQL Server 2019 CU5-Release installieren oder auf diese aktualisieren, um diese neuen Funktionen nutzen zu können und mehrere Big Data-Cluster in derselben Domäne bereitzustellen.

  Weitere Informationen über die Bereitstellung mehrerer Big Data-Cluster in derselben Active Directory-Domäne finden Sie unter [Konzept: Bereitstellen von [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] im Active Directory-Modus](active-directory-deployment-background.md).

- `security.activeDirectory.accountPrefix`: Dieser **optionale Parameter** wurde mit dem Release von SQL Server 2019 CU5 eingeführt, um die Bereitstellung mehrerer Big Data-Cluster für dieselbe Domäne zu unterstützen. Mit dieser Einstellung wird die Eindeutigkeit des Kontonamens für verschiedene Big Data-Clusterdienste gewährleistet, die sich zwischen zwei Clustern unterscheiden müssen. Das Anpassen des Namens für das Kontopräfix ist optional, standardmäßig wird der Unterdomänenname als Kontopräfix verwendet. Wenn der Unterdomänenname länger als 12 Zeichen ist, werden die ersten 12 Zeichen des Unterdomänennamens als Kontopräfix verwendet.  

  >[!NOTE]
  >Active Directory erfordert, dass Kontonamen auf 20 Zeichen eingeschränkt werden. Der Big Data-Cluster muss 8 Zeichen zum Unterscheiden von Pods und StatefulSets verwenden. Es verbleiben also 12 Zeichen als Grenzwert für das Kontopräfix.

[Überprüfen Sie den AD-Gruppenbereich](/powershell/module/addsadministration/get-adgroup), um zu ermitteln, ob es sich um eine DomainLocal-Gruppe handelt.

Wenn Sie Konfigurationsdatei für die Bereitstellung noch nicht initialisiert haben, können Sie diesen Befehl ausführen, um eine Kopie der Konfiguration abzurufen. In den folgenden Beispielen wird das Profil `kubeadm-prod` verwendet, dasselbe gilt für `openshift-prod`.

```bash
azdata bdc config init --source kubeadm-prod  --target custom-prod-kubeadm
```

Um die oben aufgeführten Parameter in der Datei `control.json` festzulegen, verwenden Sie die folgenden [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]-Befehle. Über diese Befehle werden die Konfigurationswerte vor der Bereitstellung durch Ihre eigenen Werte ersetzt.

> [!IMPORTANT]
> Im SQL Server 2019 CU2-Release hat sich die Struktur des Sicherheitskonfigurationsabschnitts im Bereitstellungsprofil leicht geändert, sodass nun alle Einstellungen im Zusammenhang mit Active Directory im neuen `activeDirectory` in der json-Struktur unter `security` in der Datei `control.json` zu finden sind.

>[!NOTE]
> Zusätzlich zum Bereitstellen von verschiedenen Werten für die Unterdomäne (wie in diesem Abschnitt beschrieben), müssen Sie auch verschiedene Portnummern für Endpunkte von Big Data-Clustern verwenden, wenn Sie mehrere Big Data-Cluster im gleichen Kubernetes-Cluster bereitstellen. Diese Portnummern sind zur Bereitstellungszeit über die Profile der [Bereitstellungskonfiguration](deployment-custom-configuration.md) konfigurierbar.

Im nachfolgenden Beispiel wird SQL Server 2019 CU2 verwendet. Dort wird gezeigt, wie Sie die AD-bezogenen Parameterwerte in der Bereitstellungskonfiguration ersetzen. Die Domänendetails unten sind Beispielwerte.

```bash
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.ouDistinguishedName=OU\=bdc\,DC\=contoso\,DC\=local"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.dnsIpAddresses=[\"10.100.10.100\"]"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.domainControllerFullyQualifiedDns=[\"HOSTNAME.CONTOSO.LOCAL\"]"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.domainDnsName=contoso.local"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.clusterAdmins=[\"bdcadminsgroup\"]"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.clusterUsers=[\"bdcusersgroup\"]"
#Example for providing multiple clusterUser groups: [\"bdcusergroup1\",\"bdcusergroup2\"]
```

Optional können Sie nur das SQL Server 2019 CU5-Release starten und die Standardwerte für die Einstellungen `subdomain` und `accountPrefix` überschreiben.

```bash
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.subdomain=[\"bdctest\"]"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.accountPrefix=[\"bdctest\"]"
```

In Releases vor SQL Server 2019 CU2 können Sie entsprechend Folgendes ausführen:

```bash
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.ouDistinguishedName=OU\=bdc\,DC\=contoso\,DC\=local"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.dnsIpAddresses=[\"10.100.10.100\"]"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.domainControllerFullyQualifiedDns=[\"HOSTNAME.CONTOSO.LOCAL\"]"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.domainDnsName=contoso.local"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.clusterAdmins=[\"bdcadminsgroup\"]"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.clusterUsers=[\"bdcusersgroup\"]"
#Example for providing multiple clusterUser groups: [\"bdcusergroup1\",\"bdcusergroup2\"]
```

Zusätzlich zu den Informationen oben müssen Sie DNS-Namen für die verschiedenen Clusterendpunkte angeben. Die DNS-Einträge mit den angegebenen DNS-Namen werden während der Bereitstellung automatisch in Ihrem DNS-Server erstellt. Sie verwenden diese Namen, wenn Sie eine Verbindung mit den verschiedenen Clusterendpunkten herstellen. Wenn der DNS-Name für die SQL-Masterinstanz beispielsweise `mastersql` lautet und da die Unterdomäne den Standardwert des Clusternamens in der Datei `control.json` verwendet, verwenden Sie entweder `mastersql.contoso.local,31433` oder `mastersql.mssql-cluster.contoso.local,31433` (je nachdem, welche Werte Sie in den Bereitstellungskonfigurationsdateien für die Endpunkt-DNS-Namen angegeben haben), um über die Tools eine Verbindung mit der Masterinstanz herzustellen. 

```bash
# DNS names for BDC services
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.spec.endpoints[0].dnsName=<controller DNS name>.contoso.local"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.spec.endpoints[1].dnsName=<monitoring services DNS name>.<Domain name. e.g. contoso.local>"
azdata bdc config replace -c custom-prod-kubeadm/bdc.json -j "$.spec.resources.master.spec.endpoints[0].dnsName=<SQL Master Primary DNS name>.<Domain name. e.g. contoso.local>"
azdata bdc config replace -c custom-prod-kubeadm/bdc.json -j "$.spec.resources.master.spec.endpoints[1].dnsName=<SQL Master Secondary DNS name>.<Domain name. e.g. contoso.local>"
azdata bdc config replace -c custom-prod-kubeadm/bdc.json -j "$.spec.resources.gateway.spec.endpoints[0].dnsName=<Gateway (Knox) DNS name>.<Domain name. e.g. contoso.local>"
azdata bdc config replace -c custom-prod-kubeadm/bdc.json -j "$.spec.resources.appproxy.spec.endpoints[0].dnsName=<app proxy DNS name>.<Domain name. e.g. contoso.local>"
```

> [!IMPORTANT]
> Sie können Endpunkt-DNS-Namen Ihrer Wahl verwenden, solange diese vollqualifiziert sind und zu keinem Konflikt zwischen zwei Big Data-Clustern führen, die in derselben Domäne bereitgestellt werden. Optional können Sie den Parameterwert `subdomain` verwenden, um sicherzustellen, dass DNS-Namen sich zwischen Clustern unterscheiden. Beispiel:

```bash
# DNS names for BDC services
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.spec.endpoints[0].dnsName=<controller DNS name>.<subdomain e.g. mssql-cluster>.contoso.local"
```

Sie finden hier ein Beispielskript für die [ Bereitstellung eines SQL Server-Big Data-Clusters in einem Einzelknoten-Kubernetes-Cluster (kubeadm) mit AD-Integration](https://github.com/microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/deployment/kubeadm/ubuntu-single-node-vm-ad).

> [!Note]
> Möglicherweise gibt es Szenarios, in denen Sie den neu eingeführten Parameter `subdomain` nicht unterstützen können. Beispielsweise wenn Sie ein Release vor CU5 bereitstellen müssen und die **[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]** bereits aktualisiert haben. Dies ist zwar sehr unwahrscheinlich, wenn Sie das Verhalten vor dem CU5-Release jedoch wiederherstellen müssen, können Sie im Active Directory-Abschnitt von `control.json` den Parameter `useSubdomain` auf `false` festlegen.  Der entsprechende Befehl lautet wie folgt:

```bash
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.useSubdomain=false"
```

Sie sollten jetzt alle erforderlichen Parameter für eine BDC-Bereitstellung mit Active Directory-Integration festgelegt haben.

Sie können nun den mit Azure Directory integrierten BDC-Cluster mithilfe des [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]-Befehls und des kubeadm-prod-Bereitstellungsprofils bereitstellen. Eine vollständige Dokumentation, wie Sie [!INCLUDE[big-data-clusters](../includes/ssbigdataclusters-nover.md)] bereitstellen, finden Sie unter [Bereitstellen von Big Data-Cluster für SQL Server in Kubernetes](deployment-guidance.md).

## <a name="verify-reverse-dns-entry-for-domain-controller"></a>Überprüfen der Reverse-DNS-Einträge für den Domänencontroller

Stellen Sie sicher, dass ein Reverse-DNS-Eintrag (PTR-Eintrag) für den Domänencontroller selbst im DNS-Server registriert ist. Sie können dies überprüfen, indem Sie `nslookup` für den Domänennamen im Domänencontroller ausführen, um sicherzustellen, dass der Name in die IP-Adresse des Domänencontrollers aufgelöst wird.

## <a name="known-issues-and-limitations"></a>Einschränkungen und bekannte Probleme

**Bekannte Einschränkungen in SQL Server 2019 CU5**

- Aktuell bieten das Dashboard für die Protokollsuche und das Metrikdashboard keine Unterstützung für die AD-Authentifizierung. Zur Authentifizierung bei diesen Dashboards kann die bei der Bereitstellung angegebene Kombination aus Benutzername und Kennwort für die Standardauthentifizierung verwendet werden. Alle weiteren Clusterendpunkte unterstützen die AD-Authentifizierung.

- Der sichere AD-Modus funktioniert aktuell nur für die Bereitstellungsumgebungen `kubeadm` und `openshift`, nicht für AKS oder ARO. Die Bereitstellungsprofile `kubeadm-prod` und `openshift-prod` enthalten standardmäßig die Sicherheitsabschnitte.

- Vor dem Release von SQL Server 2019 CU5 war nur ein Big Data-Cluster pro Domäne (Active Directory) zulässig. Das Aktivieren mehrerer Big Data-Cluster pro Domäne ist ab dem CU5-Release verfügbar.

- Keine der AD-Gruppen, die in Sicherheitskonfigurationen angegeben werden, können auf DomainLocal begrenzt werden. Sie können den Bereich einer AD-Gruppe überprüfen, indem Sie [diese Anweisungen befolgen](/powershell/module/addsadministration/get-adgroup).

- AD-Konten können für die Anmeldung beim Big Data-Cluster verwendet werden und werden über dieselbe Domäne zugelassen, die für den Big Data-Cluster konfiguriert wurde. Das Aktivieren von Anmeldungen über andere vertrauenswürdige Domänen wird nicht unterstützt.

## <a name="next-steps"></a>Nächste Schritte

[[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] verbinden: Active Directory-Modus](active-directory-connect.md)

[Behandeln von Problemen mit der Integration von Active Directory in SQL Server-Big Data-Clustern](troubleshoot-active-directory.md)

[Konzept: Bereitstellen von [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] im Active Directory-Modus](active-directory-deployment-background.md)
