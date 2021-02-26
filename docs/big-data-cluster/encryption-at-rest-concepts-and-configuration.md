---
title: Verschlüsselung ruhender Daten
titleSuffix: SQL Server Big Data Clusters
description: Erfahren Sie alles über Verschlüsselung ruhender Daten in einem SQL Server 2019 Big Data-Cluster.
author: dacoelho
ms.author: dacoelho
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 10/19/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 69749c0da2a5f7ef672a4673b5bc857898e9964f
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489184"
---
# <a name="encryption-at-rest-concepts-and-configuration-guide"></a>Konzepte- und Konfigurationsleitfaden für die Verschlüsselung ruhender Daten

Beginnend mit SQL Server Big Data-Clustern CU8 ist ein umfassender Funktionssatz für die Verschlüsselung ruhender Daten verfügbar, um Verschlüsselung auf Anwendungsebene für alle auf der Plattform gespeicherten Daten bereitzustellen. In diesem Leitfaden werden die Konzepte, die Architektur und die Konfiguration für den Funktionssatz für die Verschlüsselung ruhender Daten für SQL Server Big Data-Cluster dokumentiert.

In SQL Server Big Data-Clustern werden Daten an den folgenden zwei Orten gespeichert:

* __SQL Server__-Masterinstanz
* __HDFS__, von Speicherpool und Spark verwendet.

Um Daten in SQL Server Big Data-Clustern transparent verschlüsseln zu können, gibt es zwei mögliche Ansätze:

* __Volumeverschlüsselung__. Dieser Ansatz wird von der Kubernetes-Plattform unterstützt und als bewährte Methode für Big Data-Clusterbereitstellungen vorausgesetzt. Dieser Leitfaden behandelt die Volumeverschlüsselung nicht. Anleitungen zum ordnungsgemäßen Verschlüsseln von Volumes, die für SQL Server Big Data-Cluster verwendet werden sollen, finden Sie in der Dokumentation Ihrer Kubernetes-Plattform oder Appliance.
* __Verschlüsselung auf Anwendungsebene__. Diese Architektur bezieht sich auf die Verschlüsselung von Daten durch die Anwendung, die die Daten verarbeitet, bevor sie auf den Datenträger geschrieben werden. Für den Fall, dass die Volumes verfügbar gemacht werden, wäre ein Angreifer nicht in der Lage, Datenartefakte an anderer Stelle wiederherzustellen, es sei denn, das wurde ebenfalls mit denselben Verschlüsselungsschlüsseln konfiguriert.

Die __Featuregruppe zur Verschlüsselung ruhender Daten von SQL Server Big Data-Clustern__ unterstützt das Kernszenario der __Verschlüsselung auf Anwendungsebene__ für die __SQL Server__- und __HDFS__-Komponenten.

Folgende Funktionen werden bereitgestellt:

* __Systemseitig verwaltete Verschlüsselung ruhender Daten__. Diese Funktion ist in CU8 verfügbar.
* __Benutzerseitig verwaltete Verschlüsselung von ruhenden Daten (BYOK)__ mit Intergrationen von sowohl dienstseitig verwalteten als auch externen Schlüsselanbietern. Aktuell werden nur dienstseitig verwaltete, benutzerseitig erstellte Schlüssel unterstützt.

## <a name="key-definitions"></a>Wichtige Definitionen

### <a name="sql-server-big-data-clusters-key-management-service-kms"></a>Schlüsselverwaltungsdienst (KMS) von SQL Server Big Data-Clustern

Ein vom Controller gehosteter Dienst, der für die Verwaltung von Schlüsseln und Zertifikaten für den Funktionssatz für die Verschlüsselung ruhender Daten für den SQL Server BDC-Cluster zuständig ist. Es handelt sich um einen Dienst, der die folgenden Funktionen unterstützt:

* Sichere Verwaltung und Speicherung von Schlüsseln und Zertifikaten, die für die Verschlüsselung ruhender Daten verwendet werden.
* Kompatibilität mit Hadoop KMS. Er fungiert als Schlüsselverwaltungsdienst für die HDFS-Komponente im BDC.
* Verwaltung des SQL Server TDE-Zertifikats.

Im Rest dieses Dokuments bezeichnen wir diesen Dienst als __BDC KMS__. Außerdem wird der Begriff __BDC__ verwendet, um die Computingplattform __SQL Server Big Data-Cluster__ zu bezeichnen.

### <a name="system-managed-keys-and-certificates"></a>Systemseitig verwaltete Schlüssel und Zertifikate

Der BDC KMS-Dienst verwaltet alle Schlüssel und Zertifikate für SQL Server und HDFS.

### <a name="user-provided-certificates"></a>Benutzerseitig bereitgestellte Zertifikate

Benutzerseitig bereitgestellte Schlüssel und Zertifikate, die von BDC KMS verwaltet werden sollen, häufig als Bring Your Own Key (BYOK) bezeichnet.

### <a name="external-providers"></a>Externe Anbieter

Externe Schlüssellösungen, die mit BDC KMS kompatibel sind, für die externe Delegierung. Diese Funktion wird derzeit nicht unterstützt.

## <a name="encryption-at-rest-on-sql-server-big-data-clusters"></a>Verschlüsselung ruhender Daten auf SQL Server Big Data-Clustern

Lesen Sie dieses Dokument sorgfältig durch, um Ihr Szenario vollständig zu bewerten.

Der Funktionssatz führt den __BDC KMS-Controllerdienst__ ein, um systemseitig verwaltete Schlüssel und Zertifikate für die Verschlüsselung ruhender Daten sowohl in SQL Server als auch in HDFS bereitzustellen. Diese Schlüssel und Zertifikate werden vom Dienst verwaltet, und diese Dokumentation bietet eine Betriebsanleitung für die Interaktion mit dem Dienst.

* __SQL Server__-Instanzen nutzen die bestehende [Transparent Data Encryption](../relational-databases/security/encryption/transparent-data-encryption.md)-Funktion (TDE).
* __HDFS__ verwendet den nativen Hadoop KMS in jedem Pod für die Interaktion mit BDC KMS auf dem Controller. Dies ermöglicht HDFS-Verschlüsselungszonen, die sichere Pfade auf HDFS bereitstellen.

### <a name="sql-server-instances"></a>SQL Server-Instanzen

* Für die Verwendung mit TDE-Befehlen wird ein systemseitig generiertes Zertifikat auf SQL Server-Pods installiert. Der Benennungsstandard für das systemseitig verwaltete Zertifikat lautet `TDECertificate` + `timestamp`. Beispiel: `TDECertificate2020_09_15_22_46_27`.
* Auf der Masterinstanz des BDC bereitgestellte Datenbanken und Benutzerdatenbanken werden nicht automatisch verschlüsselt. DBAs können das installierte Zertifikat verwenden, um eine beliebige Datenbank zu verschlüsseln.
* Der Computepool und der Speicherpool werden automatisch mithilfe des systemseitig generierten Zertifikats verschlüsselt.
* Von der Verschlüsselung des Datenpools, wenn auch technisch möglich mithilfe von `EXECUTE AT`-T-SQL-Befehlen, wird abgeraten, außerdem wird sie auch zurzeit nicht unterstützt. Die Verwendung dieser Methode zur Verschlüsselung von Datenpool-Datenbanken ist möglicherweise nicht effektiv, und die Verschlüsselung erfolgt möglicherweise nicht im gewünschten Zustand. Ferner wird ein inkompatibler Upgradepfad zu den nächsten Releases erzeugt.
* Die SQL Server-Schlüsselrotation wird mithilfe von standardmäßigen T-SQL-Verwaltungsbefehlen erreicht. Eine umfassende Anleitung finden Sie im [Leitfaden zur Verwendung von SQL Server Transparent Data Encryption (transparente Datenverschlüsselung, TDE) für Big Data-Cluster](encryption-at-rest-sql-server-tde.md).
* Die Überwachung der Verschlüsselung erfolgt über vorhandene SQL Server-Standard-DMVs für TDE.
* Das Sichern und Wiederherstellen einer TDE-fähigen Datenbank im Cluster wird unterstützt.
* Hochverfügbarkeit wird unterstützt. Wenn eine Datenbank in der primären Instanz von SQL Server verschlüsselt ist, werden alle sekundären Replikate der Datenbank ebenfalls verschlüsselt.

### <a name="hdfs-encryption-zones"></a>HDFS-Verschlüsselungszonen

* [Active Directory-Integration](active-directory-prerequisites.md) ist erforderlich, um die Verschlüsselungszonenfunktion für HDFS zu aktivieren.
* Ein systemseitig generierter Schlüssel wird in Hadoop KMS bereitgestellt. Der Schlüsselname lautet `securelakekey`. In CU8 hat der Standardschlüssel 256 Bit, und wir unterstützen die Verschlüsselung mit 256-Bit-AES.
* Eine Standardverschlüsselungszone wird unter Verwendung des oben systemseitig generierten Schlüssels in einem Pfad namens `/securelake` bereitgestellt.
* Benutzer können mithilfe in diesem Leitfaden bereitgestellter spezifischer Anweisungen zusätzliche Schlüssel und Verschlüsselungszonen erstellen. Benutzer können während der Schlüsselerstellung zwischen den Schlüsselgrößen 128, 192 oder 256 auswählen.
* Bei HDFS-Verschlüsselungszonen wird die Schlüsselrotation mithilfe von azdata erreicht. Eine umfassende Anleitung finden Sie im [Leitfaden zur Verwendung von SQL Server HDFS-Verschlüsselungszonen für Big Data-Cluster](encryption-at-rest-hdfs-encryption-zones.md).
* Das Einbinden von HDFS-Tiering zusätzlich zu einer Verschlüsselungszone wird nicht unterstützt.

## <a name="encryption-at-rest-administration"></a>Verwaltung der Verschlüsselung ruhender Daten

Im Folgenden sind die Verwaltungsfunktionen für die Verschlüsselung ruhender Daten aufgeführt:

* [SQL Server TDE](encryption-at-rest-sql-server-tde.md) wird mit standardmäßigen T-SQL-Befehlen verwaltet.
* [HDFS-Verschlüsselungszonen](encryption-at-rest-hdfs-encryption-zones.md) und HDFS-Schlüssel werden mithilfe von azdata-Befehlen verwaltet.
* Die folgenden Verwaltungsfeatures werden mithilfe von [ausgeführten Notebooks](cluster-manage-notebooks.md) ausgeführt:
    - Sicherung und Wiederherstellung von HDFS-Schlüsseln
    - Löschen von HDFS-Schlüsseln

## <a name="configuration-guide"></a>Konfigurationsleitfaden

Während __neuer Bereitstellungen von SQL Server Big Data-Clustern__ (ab CU8) wird die __Verschlüsselung ruhender Daten standardmäßig aktiviert und konfiguriert__. Dies bedeutet:

* Die BDC KMS-Komponente wird im Controller bereitgestellt und generiert einen Standardsatz von Schlüsseln und Zertifikaten.
* SQL Server wird mit aktivierter TDE bereitgestellt, und Zertifikate werden vom Controller installiert.
* Hadoop KMS (für HDFS) wird für die Interaktion mit BDC KMS für Verschlüsselungsvorgänge konfiguriert. HDFS-Verschlüsselungszonen sind konfiguriert und einsatzbereit.

Die im vorherigen Abschnitt beschriebenen Anforderungen und Standardverhalten sind gültig.

__Bei einer neuen Bereitstellung von SQL Server BDC ab CU8 oder beim direkten Upgrade auf CU9 müssen keine weiteren Schritte durchgeführt werden__.

### <a name="upgrade-scenarios"></a>Upgradeszenarien

Bei vorhandenen Clustern wird vom Upgradeprozess keine neue Verschlüsselung oder Neuverschlüsselung von noch nicht verschlüsselten Benutzerdaten erzwungen. Dieses Verhalten ist entwurfsbedingt, und Folgendes muss pro Komponente berücksichtigt werden:

* __SQL Server__

    1. __SQL Server-Masterinstanz__. Der Upgradevorgang hat keine Auswirkungen auf Masterinstanz-Datenbanken und installierte TDE-Zertifikate. Es wird allerdings dringend empfohlen, Ihre Datenbanken sowie Ihre manuell installierten TDE-Zertifikate vor dem Upgradevorgang zu sichern. Ferner wird empfohlen, diese Artefakte außerhalb des SQL Server BDC-Clusters zu speichern.
    1. __Compute- und Speicherpool__. Diese Datenbanken werden systemseitig verwaltet, sind flüchtig und werden beim Clusterupgrade neu erstellt und automatisch verschlüsselt.
    1. __Datenpool__. Das Upgrade hat keine Auswirkungen auf Datenbanken im SQL Server-Instanzenteil des Datenpools.

* __HDFS__

    1. __HDFS__. Beim Upgradeprozess werden keine HDFS-Dateien und -Ordner außerhalb von Verschlüsselungszonen angefasst.

### <a name="upgrading-to-cu9-from-cu8-or-earlier"></a>Upgrade von einer Version bis CU8 auf CU9

Es sind keine weiteren Schritte erforderlich.

### <a name="upgrading-to-cu8-from-cu6-or-earlier"></a>Upgrade von einer Version bis CU6 auf CU8

   > [!CAUTION]
   > Führen Sie vor einem Upgrade auf SQL Server Big Data-Cluster CU8 eine vollständige Sicherung Ihrer Daten aus.


__Verschlüsselungszonen werden nicht konfiguriert__. Die Hadoop KMS-Komponente wird nicht für die Verwendung von BDC KMS konfiguriert. Befolgen Sie die Anleitungen im nächsten Abschnitt, um das HDFS-Verschlüsselungszonenfeature nach dem Upgrade zu konfigurieren und zu aktivieren.

#### <a name="enable-hdfs-encryption-zones-after-upgrade-to-cu8"></a>Aktivieren von HDFS-Verschlüsselungszonen nach einem Upgrade auf CU8

Wenn Sie ein Upgrade Ihres Cluster auf CU8 durchgeführt haben (`azdata upgrade`) und HDFS-Verschlüsselungszonen aktivieren möchten, gibt es zwei Möglichkeiten:

* Ausführung des [ausgeführten Notebooks](cluster-manage-notebooks.md) in Azure Data Studio mit dem Namen __SOP0128: Aktivieren von HDFS-Verschlüsselungszonen in Big Data-Clustern__ zum Durchführen der Konfiguration.
* Skriptausführung wie weiter unten beschrieben.

Anforderungen:

- Integrierter [Active Directory](active-directory-prerequisites.md)-Cluster.

- Im AD-Modus in den Cluster konfigurierte und protokollierte [!INCLUDE[azdata](../includes/azure-data-cli-azdata.md)].

Führen Sie das folgende Verfahren aus, um den Cluster mit Unterstützung für Verschlüsselungszonen neu zu konfigurieren.

1. Nachdem Sie das Upgrade mit `azdata` ausgeführt haben, speichern Sie die folgenden Skripts.

    Anforderungen für die Ausführung der Skripts:
        
    * Beide Skripts sollten sich im selben Verzeichnis befinden. 
    * `kubectl` in „PATH“
    * ```config```-Datei für Kubernetes im Ordner ```$HOME/.kube```
    
    Dieses Skript sollte __```run-key-provider-patch.sh```__ heißen:

    ```console
    #!/bin/bash
    
    if [[ -z "${BDC_DOMAIN}" ]]; then
      echo "BDC_DOMAIN environment variable with the domain name of the cluster is not defined."
      exit 1
    fi
    
    if [[ -z "${BDC_CLUSTER_NS}" ]]; then
      echo "BDC_CLUSTER_NS environment variable with the cluster namespace is not defined."
      exit 1
    fi
    
    kubectl get configmaps -n test
    
    diff <(kubectl get configmaps mssql-hadoop-storage-0-configmap -n $BDC_CLUSTER_NS -o json | ./updatekeyprovider.py) <(kubectl get configmaps mssql-hadoop-storage-0-configmap -n $BDC_CLUSTER_NS -o json | python -m json.tool)
    
    diff <(kubectl get configmaps mssql-hadoop-sparkhead-configmap -n $BDC_CLUSTER_NS -o json | ./updatekeyprovider.py) <(kubectl get configmaps mssql-hadoop-sparkhead-configmap -n $BDC_CLUSTER_NS -o json | python -m json.tool)
    
    # Replace the config maps.
    #
    kubectl replace -n $BDC_CLUSTER_NS -o json -f <(kubectl get configmaps mssql-hadoop-storage-0-configmap -n $BDC_CLUSTER_NS -o json | ./updatekeyprovider.py)
    
    kubectl replace -n $BDC_CLUSTER_NS -o json -f <(kubectl get configmaps mssql-hadoop-sparkhead-configmap -n $BDC_CLUSTER_NS -o json | ./updatekeyprovider.py)
    
    # Restart the pods which need to have the necessary changes with the core-site.xml
    kubectl delete pods -n $BDC_CLUSTER_NS nmnode-0-0
    kubectl delete pods -n $BDC_CLUSTER_NS storage-0-0
    kubectl delete pods -n $BDC_CLUSTER_NS storage-0-1
    
    # Wait for sometime for pods to restart
    #
    sleep 300
    
    # Check for the KMS process status.
    #
    kubectl exec -n $BDC_CLUSTER_NS -c hadoop nmnode-0-0 -- bash -c 'ps aux | grep kms'
    kubectl exec -n $BDC_CLUSTER_NS -c hadoop storage-0-0 -- bash -c 'ps aux | grep kms'
    kubectl exec -n $BDC_CLUSTER_NS -c hadoop storage-0-1 -- bash -c 'ps aux | grep kms'
    ```

    Dieses Skript sollte __```updatekeyprovider.py```__ heißen:

    ```python
    #!/usr/bin/env python3
    
    import json
    import re
    import sys
    import xml.etree.ElementTree as ET
    import os
    
    class CommentedTreeBuilder(ET.TreeBuilder):
        def comment(self, data):
            self.start(ET.Comment, {})
            self.data(data)
            self.end(ET.Comment)
    
    domain_name = os.environ['BDC_DOMAIN']
    
    parser = ET.XMLParser(target=CommentedTreeBuilder())
    
    core_site = 'core-site.xml'
    j = json.load(sys.stdin)
    cs = j['data'][core_site]
    csxml = ET.fromstring(cs, parser=parser)
    props = [prop.find('value').text for prop in csxml.findall(
        "./property/name/..[name='hadoop.security.key.provider.path']")]
    
    kms_provider_path=''
    
    for x in range(5):
        if len(kms_provider_path) != 0:
            kms_provider_path = kms_provider_path + ';'
        kms_provider_path = kms_provider_path + 'nmnode-0-0.' + domain_name
    
    if len(props) == 0:
        prop = ET.SubElement(csxml, 'property')
        name = ET.SubElement(prop, 'name')
        name.text = 'hadoop.security.key.provider.path'
        value = ET.SubElement(prop, 'value')
        value.text = 'kms://https@' + kms_provider_path + ':9600/kms'
        cs = ET.tostring(csxml, encoding='utf-8').decode('utf-8')
    
    j['data'][core_site] = cs
    
    kms_site = 'kms-site.xml.tmpl'
    ks = j['data'][kms_site]
    
    kp_uri_regex = re.compile('(<name>hadoop.kms.key.provider.uri</name>\s*<value>\s*)(.*)(\s*</value>)', re.MULTILINE)
    
    def replace_uri(match_obj):
        key_provider_uri = 'bdc://https@hdfsvault-svc.' + domain_name
        if match_obj.group(2) == 'jceks://file@/var/run/secrets/keystores/kms/kms.jceks' or match_obj.group(2) == key_provider_uri:
            return match_obj.group(1) + key_provider_uri + match_obj.group(3)
        return match_obj.group(0)
    
    ks = kp_uri_regex.sub(replace_uri, ks)
    
    j['data'][kms_site] = ks
    print(json.dumps(j, indent=4, sort_keys=True))
    ```

    Führen Sie __```run-key-provider-patch.sh```__ mit den entsprechenden Parametern aus. 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur wirkungsvollen Verwendung der Verschlüsselung ruhender Daten von SQL Server-Big Data-Clustern finden Sie in folgenden Artikeln:

- [Verschlüsselung ruhender Daten – SQL Server TDE](encryption-at-rest-sql-server-tde.md)
- [Verschlüsselung ruhender Daten – HDFS-Verschlüsselungszonen](encryption-at-rest-hdfs-encryption-zones.md)

Weitere Informationen zu [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] finden Sie in der folgenden Übersicht:

- [Was sind [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md)
