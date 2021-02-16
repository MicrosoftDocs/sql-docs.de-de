---
title: Konfigurieren einer Failoverclusterinstanz mithilfe von SMB-Speicher – SQL Server für Linux
description: In diesem Artikel erfahren Sie, wie Sie eine Failoverclusterinstanz (FCI) mithilfe von SMB-Speicher für SQL Server für Linux konfigurieren.
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 08/28/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 6cc84164c2829d2cf6d5e66f40fd13921fdb105b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346322"
---
# <a name="configure-failover-cluster-instance---smb---sql-server-on-linux"></a>Konfigurieren einer Failoverclusterinstanz (SMB): SQL Server für Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

In diesem Artikel wird erklärt, wie Sie einen SMB-Speicher für eine Failoverclusterinstanz (FCI) unter Linux konfigurieren. 
 
Bei Anwendern, die kein Windows verwenden, wird SMB oft als CIFS-Freigabe (Common Internet File System) bezeichnet und über Samba implementiert. In der Windows-Welt erfolgt der Zugriff auf eine SMB-Freigabe folgendermaßen: \\SERVERNAME\SHARENAME. Bei Linux-basierten SQL Server-Installationen muss die SMB-Freigabe als Ordner eingebunden werden.

## <a name="important-source-and-server-information"></a>Wichtige Informationen zu Quelle und Server

Im Folgenden finden Sie einige Tipps und Hinweise für eine erfolgreiche Verwendung von SMB:
- Die SMB-Freigabe kann sich auf Windows, Linux oder sogar einer Appliance befinden, solange sie SMB 3.0 oder höher verwendet. Weitere Informationen zu Samba und SMB 3.0 finden Sie unter [SMB 3.0](https://wiki.samba.org/index.php/Samba3/SMB2#SMB_3.0). Dort erfahren Sie auch, ob Ihre Samba-Implementierung mit SMB 3.0 kompatibel ist.
- Die SMB-Freigabe sollte hoch verfügbar sein.
- Die Sicherheit muss in der SMB-Freigabe ordnungsgemäß festgelegt werden. Im Folgenden finden Sie ein Beispiel aus „/etc/samba/smb.conf“, wobei SQLData1 der Name der Freigabe ist.

![Screenshot, der zeigt, dass SQLData1 der Name der Freigabe ist][1]

## <a name="instructions"></a>Instructions

1. Wählen Sie einen der Server aus, der an der FCI-Konfiguration beteiligt sein wird. Es spielt keine Rolle, welchen Sie auswählen.
   
1. Rufen Sie Informationen zum mssql-Benutzer ab.
   
   ```bash
    sudo id mssql
   ```
   
   Notieren Sie sich die Benutzer-ID, die Gruppen-ID und die Gruppen. 
   
1. Führen Sie `sudo smbclient -L //NameOrIP/ShareName -U User` aus.
   
   \<NameOrIP> ist der DNS-Name oder die IP-Adresse des Servers, auf dem die SMB-Freigabe gehostet wird.
   
   \<ShareName> ist der Name der SMB-Freigabe. 
   
1. Befolgen Sie für Systemdatenbanken oder Daten, die am Standardspeicherort gespeichert sind, die folgenden Schritte. Fahren Sie andernfalls mit Schritt 5 fort. 
   
   1. Stellen Sie sicher, dass SQL Server auf dem Server beendet wurde, auf dem Sie arbeiten.
      ```bash
      sudo systemctl stop mssql-server
      sudo systemctl status mssql-server
      ```
      
   1. Wechseln Sie vollständig zum Superuser. Wenn dies erfolgreich war, erhalten Sie keine Bestätigung.
      
      ```bash
      sudo -i
      ```
      
   1. Wechseln Sie zum mssql-Benutzer. Wenn dies erfolgreich war, erhalten Sie keine Bestätigung.
      
      ```bash
      su mssql
      ```
      
   1. Erstellen Sie ein temporäres Verzeichnis, in dem Sie die SQL Server-Daten und -Protokolldateien speichern. Wenn dies erfolgreich war, erhalten Sie keine Bestätigung.
      
      ```bash
      mkdir <TempDir>
      ```
      
      \<TempDir> ist der Name des Ordners. Im folgenden Beispiel wird ein Ordner mit dem Namen „/var/opt/mssql/tmp“ erstellt.
      
      ```bash
      mkdir /var/opt/mssql/tmp
      ```
      
   1. Kopieren Sie die SQL Server-Daten und -Protokolldateien in das temporäre Verzeichnis. Wenn dies erfolgreich war, erhalten Sie keine Bestätigung.
      
      ```bash
      cp /var/opt/mssql/data/* <TempDir>
      ```
      
      \<TempDir> ist der Name des Ordners aus dem vorherigen Schritt.
      
   1. Stellen Sie sicher, dass sich die Dateien im Verzeichnis befinden.
      
      ```bash
      ls <TempDir>
      ```
      
      \<TempDir> ist der Name des Ordners aus Schritt d.
      
   1. Löschen Sie die Dateien aus dem vorhandenen SQL Server-Datenverzeichnis. Wenn dies erfolgreich war, erhalten Sie keine Bestätigung.
      
      ```bash
      rm - f /var/opt/mssql/data/*
      ```
      
   1. Stellen Sie sicher, dass die Dateien gelöscht wurden. 
      
      ```bash
      ls /var/opt/mssql/data
      ```
      
   1. Geben Sie „exit“ ein, um zum Root-Benutzer zurück zu wechseln.
      
   1. Binden Sie die SMB-Freigabe in den SQL Server-Datenordner ein. Wenn dies erfolgreich war, erhalten Sie keine Bestätigung. Dieses Beispiel zeigt die Syntax zum Herstellen einer Verbindung mit einer Windows Server-basierten SMB 3.0-Freigabe.
      
      ```bash
      Mount -t cifs //<ServerName>/<ShareName> /var/opt/mssql/data -o vers=3.0,username=<UserName>,password=<Password>,domain=<domain>,uid=<mssqlUID>,gid=<mssqlGID>,file_mode=0777,dir_mode=0777
      ```
      
      \<ServerName> ist der Name des Servers mit der SMB-Freigabe.
      
      \<ShareName> ist der Name der Freigabe.
      
      \<UserName> ist der Name des Benutzers, der auf die Freigabe zugreifen soll.
      
      \<Password> ist das Kennwort des Benutzers.
      
      \<domain> ist der Name der Active Directory-Domäne.
      
      \<mssqlUID> ist die Benutzer-ID des mssql-Benutzers. 
      
      \<mssqlGID> ist die Gruppen-ID des mssql-Benutzers.
      
   1. Überprüfen Sie, ob die Einbindung erfolgreich war, indem Sie „mount“ ohne Parameter ausgeben.
      
      ```bash
      mount
      ```
      
   1. Wechseln Sie zum mssql-Benutzer. Wenn dies erfolgreich war, erhalten Sie keine Bestätigung.
      
      ```bash
      su mssql
      ```
      
   1. Kopieren Sie die Dateien aus dem temporären Verzeichnis „/var/opt/mssql/data“. Wenn dies erfolgreich war, erhalten Sie keine Bestätigung.
      
      ```bash
      cp /var/opt/mssql/tmp/* /var/opt/mssql/data
      ```
      
   1. Stellen Sie sicher, dass die Dateien vorhanden sind.
      
      ```bash
      ls /var/opt/mssql/data
      ```
      
   1. Geben Sie für „exit“ nicht „mssql“ ein. 
      
   1. Geben Sie für „exit“ nicht „root“ ein.
   
   1. Starten Sie SQL Server. Wenn alles richtig kopiert wurde und die Sicherheitsmaßnahmen ordnungsgemäß angewendet wurden, sollte SQL Server als gestartet angezeigt werden.
      
      ```bash
      sudo systemctl start mssql-server
      sudo systemctl status mssql-server
      ```
      
   1. Erstellen Sie zum weiteren Testen eine Datenbank, um sicherzustellen, dass die Berechtigungen in Ordnung sind. Im folgenden Beispiel wird Transact-SQL verwendet. Sie können SSMS verwenden.
      
      ![10_testcreatedb][2] 
      
   1. Beenden Sie SQL Server, und vergewissern Sie sich, dass die Serverinstanz heruntergefahren wurde. Wenn Sie weitere Datenträger hinzufügen oder testen möchten, fahren Sie SQL Server erst herunter, wenn diese hinzugefügt und getestet wurden.
      
      ```bash
      sudo systemctl stop mssql-server
      sudo systemctl status mssql-server
      ```
      
   1. Heben Sie die Einbindung der Freigabe nur auf, wenn Sie fertig sind. Falls nicht, heben Sie die Einbindung auf, nachdem Sie mit dem Testen bzw. Hinzufügen weiterer Datenträger fertig sind.
      
      ```bash
      sudo umount //<IPAddressorServerName>/<ShareName /<FolderMountedIn>
      ```
      
      \<IPAddressOrServerName> ist die IP-Adresse oder der Name des SMB-Hosts.
      
      \<ShareName> ist der Name der Freigabe.
      
      \<FolderMountedIn> ist der Name des Ordners, in den SMB eingebunden ist.
      
5. Führen Sie folgende Schritte aus, wenn es nicht um Systemdatenbanken, sondern um Benutzerdatenbanken oder Sicherungen geht. Wenn nur der Standardspeicherort verwendet wird, können Sie mit Schritt 14 fortfahren.
   
   1. Wechseln Sie zum Superuser. Wenn dies erfolgreich war, erhalten Sie keine Bestätigung.
      
      ```bash
      sudo -i
      ```
      
   1. Erstellen Sie einen Ordner, der von SQL Server verwendet wird. 
      
      ```bash
      mkdir <FolderName>
      ```
      
      \<FolderName> ist der Name des Ordners. Wenn sich der Ordner nicht am richtigen Speicherort befindet, muss der vollständige Pfad angegeben werden. Im folgenden Beispiel wird ein Ordner mit dem Namen „/var/opt/mssql/userdata“ erstellt.
      
      ```bash
      mkdir /var/opt/mssql/userdata
      ```
      
   1. Binden Sie die SMB-Freigabe in den SQL Server-Datenordner ein. Wenn dies erfolgreich war, erhalten Sie keine Bestätigung. Dieses Beispiel zeigt die Syntax zum Herstellen einer Verbindung mit einer Samba-basierten SMB 3.0-Freigabe.
      
      ```bash
      Mount -t cifs //<ServerName>/<ShareName> <FolderName> -o vers=3.0,username=<UserName>,password=<Password>,uid=<mssqlUID>,gid=<mssqlGID>,file_mode=0777,dir_mode=0777
      ```
      
      \<ServerName> ist der Name des Servers mit der SMB-Freigabe.
      
      \<ShareName> ist der Name der Freigabe.
      
      \<FolderName> ist der Name des Ordners, der im letzten Schritt erstellt wurde.  
      
      \<UserName> ist der Name des Benutzers, der auf die Freigabe zugreifen soll.
      
      \<Password> ist das Kennwort des Benutzers.
      
      \<mssqlUID> ist die Benutzer-ID des mssql-Benutzers.
      
      \<mssqlGID> ist die Gruppen-ID des mssql-Benutzers.
      
   1. Überprüfen Sie, ob die Einbindung erfolgreich war, indem Sie „mount“ ohne Parameter ausgeben.
   
   1. Geben Sie „exit“ ein, um nicht mehr der Superuser zu sein.
   
   1. Erstellen Sie eine Datenbank in diesem Ordner, um dies zu testen. Im folgenden Beispiel wird „sqlcmd“ verwendet, um eine Datenbank zu erstellen, den Kontext zu wechseln und zu überprüfen, ob die Dateien auf der Betriebssystemebene vorhanden sind. Anschließend wird der temporäre Speicherort gelöscht. Sie können SSMS verwenden.
   
   1. Heben Sie die Einbindung der Freigabe auf. 
      
      ```bash
      sudo umount //<IPAddressorServerName>/<ShareName> /<FolderMountedIn>
      ```
      
      \<IPAddressOrServerName> ist die IP-Adresse oder der Name des SMB-Hosts.
      
      \<ShareName> ist der Name der Freigabe.
      
      \<FolderMountedIn> ist der Name des Ordners, in den SMB eingebunden ist.
   
1. Wiederholen Sie die Schritte auf dem oder den anderen Knoten.

Sie können die Failoverclusterinstanz nun konfigurieren.

## <a name="next-steps"></a>Nächste Schritte

[Konfigurieren einer Failoverclusterinstanz – SQL Server für Linux](sql-server-linux-shared-disk-cluster-configure.md)

<!--Image references-->
[1]: ./media/sql-server-linux-shared-disk-cluster-configure-smb/05-smbsource.png 
[2]: ./media/sql-server-linux-shared-disk-cluster-configure-smb/10-testcreatedb.png 
