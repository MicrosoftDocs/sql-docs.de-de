---
title: 'Konfigurieren des iSCSI-Speichers für Failoverclusterinstanzen: SQL Server für Linux'
description: Hier erfahren Sie, wie Sie eine Failoverclusterinstanz mithilfe von iSCSI für SQL Server für Linux konfigurieren.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 06/30/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: dfd63aa749b6bf158ae5e9dcef06ba032c31ddee
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346331"
---
# <a name="configure-failover-cluster-instance---iscsi---sql-server-on-linux"></a>SQL Server für Linux: Konfigurieren einer Failoverclusterinstanz mit iSCSI

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

In diesem Artikel wird erklärt, wie Sie einen iSCSI-Speicher für eine Failoverclusterinstanz (FCI) unter Linux konfigurieren. 

## <a name="configure-iscsi"></a>Konfigurieren von iSCSI 
iSCSI verwendet Netzwerke, um Datenträger von einem Server darzustellen, der für andere Server als Zielserver gilt. Die Server, die eine Verbindung mit dem iSCSI-Ziel herstellen, erfordern einen konfigurierten iSCSI-Initiator. Die Datenträger auf dem Ziel erhalten explizite Berechtigungen, sodass nur die Initiatoren auf die Datenträger zugreifen können, die dies können sollen. Das Ziel selbst sollte hochverfügbar und zuverlässig sein.

### <a name="important-iscsi-target-information"></a>Wichtige Informationen zum iSCSI-Ziel
In diesem Abschnitt wird zwar nicht beschrieben, wie das iSCSI-Ziel konfiguriert wird, da dies von dem Typ der Quelle abhängt, die Sie verwenden möchten, jedoch wird sichergestellt, dass die Sicherheit für die von den Clusterknoten verwendeten Datenträger konfiguriert wird.  

Das Ziel sollte nie für FCI-Knoten konfiguriert werden, wenn ein auf Linux basierendes iSCSI-Ziel verwendet wird. iSCSI-Netzwerke sollten separat von denen sein, die vom regulären Netzwerkdatenverkehr der Quell- und Clientserver verwendet werden, um die Leistung und Verfügbarkeit zu verbessern. Die Netzwerke, die für iSCSI verwendet werden, sollten schnell sein. Denken daran, dass Netzwerke eine gewisse Prozessorbandbreite beanspruchen. Planen Sie daher entsprechend, wenn Sie einen herkömmlichen Server verwenden.
Das Wichtigste, was Sie auf dem Ziel sicherstellen sollten, ist, dass den erstellten Datenträgern die richtigen Berechtigungen zugewiesen werden, sodass nur die Server über Zugriffsberechtigungen verfügen, die an der Failoverclusterinstanz beteiligt sind. Im Folgenden wird ein Beispiel des iSCSI-Ziels von Microsoft gezeigt, in dem der Name „linuxnodes1“ erstellt wird, und (in diesem Fall) die IP-Adressen der Knoten so zugewiesen werden, dass „NewFCIDisk1.vhdx“ für sie angezeigt wird.

![Initiator][1]

### <a name="instructions"></a>Instructions

In diesem Abschnitt wird beschrieben, wie ein iSCSI-Initiator für die Server konfiguriert wird, die der Failoverclusterinstanz als Knoten dienen. Die Anweisungen sollten sowohl für RHEL als auch für Ubuntu funktionieren.

Weitere Informationen zum iSCSI-Initiator für die unterstützten Verteilungen finden Sie unter folgenden Links:
- [Red Hat](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Storage_Administration_Guide/iscsi-api.html)
- [SUSE](https://www.suse.com/documentation/sles11/stor_admin/data/sec_inst_system_iscsi_initiator.html) 
- [Ubuntu](https://help.ubuntu.com/lts/serverguide/iscsi-initiator.html)

1.  Wählen Sie einen der Server aus, der an der FCI-Konfiguration beteiligt sein wird. Es spielt keine Rolle, welchen Sie auswählen. iSCSI sollte sich auf einem dedizierten Netzwerk befinden, konfigurieren Sie iSCSI also so, dass das Netzwerk erkannt und verwendet wird. Führen Sie `sudo iscsiadm -m iface -I <iSCSIIfaceName> -o new` aus, wobei `<iSCSIIfaceName>` dem eindeutigen oder dem Anzeigenamen des Netzwerks entspricht. Im folgenden Beispiel wird `iSCSINIC` verwendet:
   
    ```bash
    sudo iscsiadm -m iface -I iSCSINIC -o new
    ```
    ![Screenshot: iface-Befehl und Antwort darauf][6]
 
2.  Bearbeiten Sie `/var/lib/iscsi/ifaces/iSCSIIfaceName`. Stellen Sie sicher, dass die folgenden Werte vollständig ausgefüllt sind:

    - iface.net_ifacename entspricht dem Namen der Netzwerkkarte, die im Betriebssystem angezeigt wird.
    - iface.hwaddress entspricht der MAC-Adresse des eindeutigen Namens, der unten für diese Schnittstelle erstellt wird.
    - iface.ipaddress
    - iface.subnet_Mask 

    Sehen Sie sich folgendes Beispiel an:

    ![Screenshot: Datei, in der die Werte vollständig ausgefüllt sind][2]

3.  Suchen Sie das iSCSI-Ziel.

    ```bash
    sudo iscsiadm -m discovery -t sendtargets -I <iSCSINetName> -p <TargetIPAddress>:<TargetPort>
    ```

     \<iSCSINetName> ist der eindeutige bzw. der Anzeigename des Netzwerks, \<TargetIPAddress> ist die IP-Adresse des iSCSI-Ziels, und \<TargetPort> ist der Port des iSCSI-Ziels. 

    ![Screenshot: discovery-Befehl und Antwort darauf][3]

 
4.  Melden Sie sich beim Zielserver an.

    ```bash
    sudo iscsiadm -m node -I <iSCSIIfaceName> -p TargetIPAddress -l
    ```

    \<iSCSIIfaceName> ist der eindeutige bzw. der Anzeigename des Netzwerks, und \<TargetIPAddress> ist die IP-Adresse des iSCSI-Ziels.

    ![iSCSITargetLogin][4]

5.  Überprüfen Sie, ob eine Verbindumg mit dem iSCSI-Ziel besteht.

    ```bash
    sudo iscsiadm -m session
    ```

    ![iSCSIVerify][5]

 
6.  Überprüfen Sie die angefügten iSCSI-Datenträger.

    ```bash
    sudo grep "Attached SCSI" /var/log/messages
    ```
    ![Screenshot: grep-Befehl und Antwort darauf mit den angefügten SCSI-Datenträgern][7]

7.  Erstellen Sie ein physisches Volume auf dem iSCSI-Datenträger.

    ```bash
    sudo pvcreate /dev/<devicename>
    ```

    \<devicename> ist der Name des Geräts aus dem vorherigen Schritt. 

 
8.  Erstellen Sie eine Volumegruppe auf dem iSCSI-Datenträger. Datenträger, die einer einzelnen Volumegruppe zugewiesen werden, werden als Pool oder Sammlung angezeigt. 

    ```bash
    sudo vgcreate <VolumeGroupName> /dev/devicename
    ```

    \<VolumeGroupName> ist der Name der Volumegruppe, und \<devicename> ist der Name des Geräts aus Schritt 6. 
 
9.  Erstellen und überprüfen Sie das logische Volume für den Datenträger.

    ```bash
    sudo lvcreate -Lsize -n <LogicalVolumeName> <VolumeGroupName>
    ```
    
    \<size> ist die Größe des zu erstellenden Volumes und kann in G (Gigabyte), T (Terabyte) usw. angegeben werden. \<LogicalVolumeName> ist der Name des logischen Volumes, und \<VolumeGroupName> ist der Name der Volumegruppe aus dem vorherigen Schritt. 

    Im folgenden Beispiel wird ein Volume mit 25 GB erstellt.
 
    ![Create25GBVol][10]

10. Führen Sie `sudo lvs` aus, um den erstellten LVM anzuzeigen.
 
11. Formatieren Sie das logische Volume mit einem unterstützten Dateisystem. Verwenden Sie das folgende Beispiel für EXT4:

    ```bash
    sudo mkfs.ext4 /dev/<VolumeGroupName>/<LogicalVolumeName>
    ```

    \<VolumeGroupName> ist der Name der Volumegruppe aus dem vorherigen Schritt. \<LogicalVolumeName> ist der Name des logischen Volumes aus dem vorherigen Schritt.  

12. Befolgen Sie für Systemdatenbanken oder Daten, die am Standardspeicherort gespeichert sind, die folgenden Schritte. Fahren Sie andernfalls mit Schritt 13 fort.

   * Stellen Sie sicher, dass SQL Server auf dem Server beendet wurde, auf dem Sie arbeiten.

        ```bash
        sudo systemctl stop mssql-server
        sudo systemctl status mssql-server
        ```
    
   * Wechseln Sie vollständig zum Superuser. Wenn dies erfolgreich war, erhalten Sie keine Bestätigung.

        ```bash
        sudo -i
        ```
    
   * Wechseln Sie zum mssql-Benutzer. Wenn dies erfolgreich war, erhalten Sie keine Bestätigung.

        ```bash
        su mssql
        ```
    
   * Erstellen Sie ein temporäres Verzeichnis, in dem Sie die SQL Server-Daten und -Protokolldateien speichern. Wenn dies erfolgreich war, erhalten Sie keine Bestätigung.

        ```bash
        mkdir <TempDir>
        ```
    
        \<TempDir> ist der Name des Ordners. Im folgenden Beispiel wird ein Ordner mit dem Namen „/var/opt/mssql/TempDir“ erstellt.

        ```bash
        mkdir /var/opt/mssql/TempDir
        ```
        
   * Kopieren Sie die SQL Server-Daten und -Protokolldateien in das temporäre Verzeichnis. Wenn dies erfolgreich war, erhalten Sie keine Bestätigung.

        ```bash
        cp /var/opt/mssql/data/* <TempDir>
        ```
    
        \<TempDir> ist der Name des Ordners aus dem vorherigen Schritt.
    
   * Stellen Sie sicher, dass sich die Dateien im Verzeichnis befinden.

        ```bash
        ls \<TempDir>
        ```
 
        \<TempDir> ist der Name des Ordners aus Schritt d.

   * Löschen Sie die Dateien aus dem vorhandenen SQL Server-Datenverzeichnis. Wenn dies erfolgreich war, erhalten Sie keine Bestätigung.

        ```bash
        rm - f /var/opt/mssql/data/*
        ```
    
   * Stellen Sie sicher, dass die Dateien gelöscht wurden. Im folgenden Beispiel wird die gesamte Sequenz von Schritt c bis h veranschaulicht.

        ```bash
        ls /var/opt/mssql/data
        ```
    
        ![Screenshot: ls-Befehl und Antwort darauf][8]

   * Geben Sie `exit` ein, um zum Root-Benutzer zurück zu wechseln.

   * Binden Sie das logische iSCSI-Volume in den SQL Server-Datenordner ein. Wenn dies erfolgreich war, erhalten Sie keine Bestätigung.

        ```bash
        mount /dev/<VolumeGroupName>/<LogicalVolumeName> /var/opt/mssql/data
        ```

        \<VolumeGroupName> ist der Name der Volumegruppe, und \<LogicalVolumeName> ist der Name des erstellten logischen Volumes. Die folgende Beispielsyntax entspricht der Volumegruppe und dem logischen Volume aus dem vorherigen Befehl.

        ```bash
        mount /dev/FCIDataVG1/FCIDataLV1 /var/opt/mssql/data
        ```

   * Ändern Sie den Besitzer der Einbindung in mssql. Wenn dies erfolgreich war, erhalten Sie keine Bestätigung.

        ```bash
        chown mssql /var/opt/mssql/data
        ```

   * Ändern Sie den Besitz der Gruppe der Einbindung in mssql. Wenn dies erfolgreich war, erhalten Sie keine Bestätigung.

        ```bash
        chgrp mssql /var/opt/mssql/data
        ```

   * Wechseln Sie zum mssql-Benutzer. Wenn dies erfolgreich war, erhalten Sie keine Bestätigung.

        ```bash
        su mssql
        ``` 

   * Kopieren Sie die Dateien aus dem temporären Verzeichnis „/var/opt/mssql/data“. Wenn dies erfolgreich war, erhalten Sie keine Bestätigung.

        ```bash
        cp /var/opt/mssql/TempDir/* /var/opt/mssql/data
        ``` 
    
   * Stellen Sie sicher, dass die Dateien vorhanden sind.

        ```bash
        ls /var/opt/mssql/data
        ``` 

   *    Geben Sie für `exit` nicht „mssql“ ein.

   *    Geben Sie für `exit` nicht „root“ ein.

   *    Starten Sie SQL Server. Wenn alles richtig kopiert wurde und die Sicherheitsmaßnahmen ordnungsgemäß angewendet wurden, sollte SQL Server als gestartet angezeigt werden.

        ```bash
        sudo systemctl start mssql-server
        sudo systemctl status mssql-server
        ``` 

   *    Beenden Sie SQL Server, und vergewissern Sie sich, dass die Serverinstanz heruntergefahren wurde.

        ```bash
        sudo systemctl stop mssql-server
        sudo systemctl status mssql-server
        ``` 

13. Führen Sie folgende Schritte aus, wenn es nicht um Systemdatenbanken, sondern um Benutzerdatenbanken oder Sicherungen geht. Wenn nur der Standardspeicherort verwendet wird, können Sie mit Schritt 14 fortfahren.

   *    Wechseln Sie zum Superuser. Wenn dies erfolgreich war, erhalten Sie keine Bestätigung.

        ```bash
        sudo -i
        ```

   *    Erstellen Sie einen Ordner, der von SQL Server verwendet wird. 

        ```bash
        mkdir <FolderName>
        ```

        \<FolderName> ist der Name des Ordners. Wenn sich der Ordner nicht am richtigen Speicherort befindet, muss der vollständige Pfad angegeben werden. Im folgenden Beispiel wird ein Ordner mit dem Namen „/var/opt/mssql/userdata“ erstellt.

        ```bash
        mkdir /var/opt/mssql/userdata
        ```

   *    Binden Sie das logische iSCSI-Volume in den Ordner ein, der im vorherigen Schritt erstellt wurde. Wenn dies erfolgreich war, erhalten Sie keine Bestätigung.

        ```bash
        mount /dev/<VolumeGroupName>/<LogicalVolumeName> <FolderName>
        ```

        \<VolumeGroupName> ist der Name der Volumegruppe, \<LogicalVolumeName> ist der Name des erstellten logischen Volumes, und \<FolderName> ist der Name des Ordners. Die Beispielsyntax wird unten gezeigt.

        ```bash
        mount /dev/FCIDataVG2/FCIDataLV2 /var/opt/mssql/userdata 
        ```

   *    Ändern Sie den Besitzer des Ordners, der in mssql erstellt wurde. Wenn dies erfolgreich war, erhalten Sie keine Bestätigung.

        ```bash
        chown mssql <FolderName>
        ```

        \<FolderName> ist der Name des erstellten Ordners. Ein entsprechendes Beispiel ist nachfolgend dargestellt.

        ```bash
        chown mssql /var/opt/mssql/userdata
        ```

   *    Ändern Sie die Gruppe des Ordners, der in mssql erstellt wurde. Wenn dies erfolgreich war, erhalten Sie keine Bestätigung.

        ```bash
        chown mssql <FolderName>
        ```

        \<FolderName> ist der Name des erstellten Ordners. Ein entsprechendes Beispiel ist nachfolgend dargestellt.

        ```bash
        chown mssql /var/opt/mssql/userdata
        ```

   *    Geben Sie `exit` ein, um nicht mehr der Superuser zu sein.

   *    Erstellen Sie eine Datenbank in diesem Ordner, um dies zu testen. Im folgenden Beispiel wird „sqlcmd“ verwendet, um eine Datenbank zu erstellen, den Kontext zu wechseln und zu überprüfen, ob die Dateien auf der Betriebssystemebene vorhanden sind. Anschließend wird der temporäre Speicherort gelöscht. Sie können SSMS verwenden.
  
        ![Screenshot: sqlcmd-Befehl und Antwort darauf][9]

   *    Heben Sie die Einbindung der Freigabe auf. 

        ```bash
        sudo umount /dev/<VolumeGroupName>/<LogicalVolumeName> <FolderName>
        ```

        \<VolumeGroupName> ist der Name der Volumegruppe, \<LogicalVolumeName> ist der Name des erstellten logischen Volumes, und \<FolderName> ist der Name des Ordners. Die Beispielsyntax wird unten gezeigt.

        ```bash
        sudo umount /dev/FCIDataVG2/FCIDataLV2 /var/opt/mssql/userdata 
        ```

14. Konfigurieren Sie den Server so, dass die Volumegruppe nur mit Pacemaker aktiviert werden kann.

    ```bash
    sudo lvmconf --enable-halvm --services -startstopservices
    ```

15. Generieren Sie eine Liste der Volumegruppen auf dem Server. Alle aufgeführten Gruppen, die nicht zum iSCSI-Datenträger gehören, werden vom System verwendet, z. B. für den Betriebssystemdatenträger.

    ```bash
    sudo vgs
    ```

16. Ändern Sie den Aktivierungskonfigurationsabschnitt der Datei unter /etc/lvm/lvm.conf. Konfigurieren Sie die folgende Zeile:

    ```bash
    volume_list = [ <ListOfVGsNotUsedByPacemaker> ]
    ```

    \<ListOfVGsNotUsedByPacemaker> ist die Liste der Volumegruppen aus der Ausgabe von Schritt 20, die nicht von der Failoverclusterinstanz verwendet wird. Setzen Sie sie in Anführungszeichen, und trennen Sie sie durch Kommas. Ein entsprechendes Beispiel ist nachfolgend dargestellt.

    ![Screenshot: Beispielwert für volume_list][11]

17. Wenn Linux gestartet wird, wird das Dateisystem eingebunden. Erstellen Sie das Image für das Stammdateisystem neu, um sicherzustellen, dass nur Pacemaker den iSCSI-Datenträger einbinden kann. 

    Führen Sie den folgenden Befehl aus. Dies sollte nur wenig Zeit beanspruchen. Wenn dies erfolgreich war, erhalten Sie keine Benachrichtigung.

    ```bash
    sudo dracut -H -f /boot/initramfs-$(uname -r).img $(uname -r)
    ```

18. Starten Sie den Server neu.

19. Führen Sie die Schritte 1-6 auf einem anderen Server aus, der an der Failoverclusterinstanz teilnimmt. Dadurch wird das iSCSI-Ziel für die SQL Server-Instanz angezeigt. 
 
20. Generieren Sie eine Liste der Volumegruppen auf dem Server. Nun sollte die zuvor erstellte Volumegruppe angezeigt werden. 

    ```bash
    sudo vgs
    ``` 

23. Starten Sie SQL Server, und überprüfen Sie, ob die Ausführung auf dem Server möglich ist.

    ```bash
    sudo systemctl start mssql-server
    sudo systemctl status mssql-server
    ```

24. Beenden Sie SQL Server, und vergewissern Sie sich, dass die Serverinstanz heruntergefahren wurde.

    ```bash
    sudo systemctl stop mssql-server
    sudo systemctl status mssql-server
    ```

25. Wiederholen Sie die Schritte 1-6 auf allen anderen Servern, die an der Failoverclusterinstanz teilnehmen sollen.

Sie können die Failoverclusterinstanz nun konfigurieren.

| Distribution | Thema |
| :----------- | :---- |
| Red Hat Enterprise Linux mit Add-On für Hochverfügbarkeit | [Konfigurieren](sql-server-linux-shared-disk-cluster-configure.md)<br/>[Ausführen](sql-server-linux-shared-disk-cluster-red-hat-7-operate.md) |
| SUSE Linux Enterprise Server mit Add-On für Hochverfügbarkeit | [Konfigurieren](sql-server-linux-shared-disk-cluster-sles-configure.md) |

## <a name="next-steps"></a>Nächste Schritte

[Konfigurieren einer Failoverclusterinstanz – SQL Server für Linux](sql-server-linux-shared-disk-cluster-configure.md)

<!--Image references-->
[1]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/05-initiator.png
[2]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/10-iscsiTargetSettings.png
[3]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/15-iSCSITargetResults.png
[4]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/20-iSCSITargetLogin.png
[5]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/25-iSCSIVerify.png
[6]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/7-setiscsinetwork.png
[7]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/30-iSCSIattachedDisks.png
[8]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/45-CopyMove.png
[9]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/50-ExampleCreateSSMS.png
[10]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/40-Create25GBVol.png
[11]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/55-ListOfVGs.png
