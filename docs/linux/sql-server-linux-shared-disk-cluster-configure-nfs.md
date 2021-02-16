---
title: Konfigurieren einer Failoverclusterinstanz mithilfe von NFS-Speicher – SQL Server für Linux
description: In diesem Artikel erfahren Sie, wie Sie eine Failoverclusterinstanz (FCI) mithilfe von NFS-Speicher für SQL Server unter Linux konfigurieren.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 08/28/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 90240b383e19fefd254cd5f28b6d113b40b20d98
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346316"
---
# <a name="configure-failover-cluster-instance---nfs---sql-server-on-linux"></a>Konfigurieren einer Failoverclusterinstanz (NFS): SQL Server für Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

In diesem Artikel wird erklärt, wie Sie NFS-Speicher für eine Failoverclusterinstanz (Failover Cluster Instance, FCI) unter Linux konfigurieren. 

Ein Network File System (NFS) ist ein gängiger Weg zum Freigeben von Datenträgern unter Linux, jedoch nicht unter Windows. Ähnlich wie iSCSI kann ein NFS auf einem Server, auf einem Gerät oder einer Speichereinheit konfiguriert werden, sofern die Speicheranforderungen für SQL Server eingehalten werden.

## <a name="important-nfs-server-information"></a>Wichtige Informationen zum NFS-Server

Die Quelle, die das NFS hostet, z. B. ein Linux-Server, muss Version 4.2 oder höher verwenden oder damit kompatibel sein. Frühere Versionen funktionieren mit SQL Server für Linux nicht.

Beachten Sie beim Konfigurieren des Ordners/der Ordner, die auf dem NFS-Server freigegeben werden sollen, Folgendes:
- `rw` ist erforderlich, damit aus dem Ordner gelesen und in den Ordner geschrieben werden kann
- `sync` ist erforderlich für garantierte Schreibvorgänge in den Ordner
- Verwenden Sie `no_root_squash` nicht als Option, da dies ein Sicherheitsrisiko darstellt
- Für den Ordner muss Vollzugriff (777) eingerichtet sein

Stellen Sie sicher, dass Ihre Sicherheitsstandards für den Zugriff erzwungen werden. Achten Sie beim Konfigurieren des Ordners darauf, dass nur die an der FCI beteiligten Server den NFS-Ordner sehen können. Unten ist als Beispiel eine angepasste „/etc/exports“-Datei auf einer Linux-basierten NFS-Lösung zu sehen. Der Ordner ist auf FCIN1 und FCIN2 beschränkt.

![Screenshot: Beispiel für eine angepasste Version von /etc/exports auf einer Linux-basierten NFS-Lösung, bei der der Ordner auf FCIN1 und FCIN2 beschränkt ist][1]

## <a name="instructions"></a>Instructions

1. Wählen Sie einen der Server aus, der an der FCI-Konfiguration beteiligt sein wird. Es spielt keine Rolle, welchen Sie auswählen. 

2. Stellen Sie sicher, dass der Server die Einbindung/en auf dem NFS-Server sehen kann.

    ```bash
    sudo showmount -e <IPAddressOfNFSServer>
    ```

    \<IPAddressOfNFSServer> ist die IP-Adresse des NFS-Servers, den Sie verwenden werden.

3. Befolgen Sie für Systemdatenbanken oder Daten, die am Standardspeicherort gespeichert sind, die folgenden Schritte. Fahren Sie andernfalls mit Schritt 4 fort.
 
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

    Der Name des Ordners lautet \<TempDir>. Im folgenden Beispiel wird ein Ordner mit dem Namen „/var/opt/mssql/tmp“ erstellt.

    ```bash
    mkdir /var/opt/mssql/tmp
    ```

   * Kopieren Sie die SQL Server-Daten und -Protokolldateien in das temporäre Verzeichnis. Wenn dies erfolgreich war, erhalten Sie keine Bestätigung.
    
    ```bash
    cp /var/opt/mssql/data/* <TempDir>
    ```

    \<TempDir> ist der Name des Ordners aus dem vorherigen Schritt.

   * Stellen Sie sicher, dass sich die Dateien im Verzeichnis befinden.

    ```bash
    ls TempDir
    ```

    \<TempDir> ist der Name des Ordners aus Schritt d.

   * Löschen Sie die Dateien aus dem vorhandenen SQL Server-Datenverzeichnis. Wenn dies erfolgreich war, erhalten Sie keine Bestätigung.

    ```bash
    rm - f /var/opt/mssql/data/*
    ```

   * Stellen Sie sicher, dass die Dateien gelöscht wurden. 

    ```bash
    ls /var/opt/mssql/data
    ```
    
   * Geben Sie „exit“ ein, um zum Root-Benutzer zurück zu wechseln.

   * Binden Sie die NFS-Freigabe in den SQL Server-Datenordner ein. Wenn dies erfolgreich war, erhalten Sie keine Bestätigung.

    ```bash
    mount -t nfs4 <IPAddressOfNFSServer>:<FolderOnNFSServer> /var/opt/mssql/data -o nfsvers=4.2,timeo=14,intr
    ```

    \<IPAddressOfNFSServer> ist die IP-Adresse des NFS-Servers, den Sie verwenden werden. 

    \<FolderOnNFSServer> ist der Name der NFS-Freigabe. Die folgende Beispielsyntax entspricht den NFS-Informationen aus Schritt 2.

    ```bash
    mount -t nfs4 200.201.202.63:/var/nfs/fci1 /var/opt/mssql/data -o nfsvers=4.2,timeo=14,intr
    ```

   * Überprüfen Sie, ob die Einbindung erfolgreich war, indem Sie „mount“ ohne Parameter ausgeben.

    ```bash
    mount
    ```

    ![Screenshot: mount-Befehl und Antwort darauf ohne Wechsel][2]

   * Wechseln Sie zum mssql-Benutzer. Wenn dies erfolgreich war, erhalten Sie keine Bestätigung.

    ```bash
    su mssql
    ```

   * Kopieren Sie die Dateien aus dem temporären Verzeichnis „/var/opt/mssql/data“. Wenn dies erfolgreich war, erhalten Sie keine Bestätigung.

    ```bash
    cp /var/opt/mssql/tmp/* /var/opt/mssqldata
    ```

   * Stellen Sie sicher, dass die Dateien vorhanden sind.

    ```bash
    ls /var/opt/mssql/data
    ```

   * Geben Sie für „exit“ nicht „mssql“ ein. 
    
   * Geben Sie für „exit“ nicht „root“ ein.

   * Starten Sie SQL Server. Wenn alles richtig kopiert wurde und die Sicherheitsmaßnahmen ordnungsgemäß angewendet wurden, sollte SQL Server als gestartet angezeigt werden.

    ```bash
    sudo systemctl start mssql-server
    sudo systemctl status mssql-server
    ```
    
   * Erstellen Sie eine Datenbank, um zu testen, ob die Sicherheit richtig eingerichtet wurde. Im folgenden Beispiel geschieht dies über Transact-SQL, es ist jedoch auch über SSMS möglich.
 
    ![CreateTestdatabase][3]

   * Beenden Sie SQL Server, und vergewissern Sie sich, dass die Serverinstanz heruntergefahren wurde.

    ```bash
    sudo systemctl stop mssql-server
    sudo systemctl status mssql-server
    ```

   * Heben Sie die Einbindung der Freigabe auf, wenn Sie keine weiteren NFS-Freigaben erstellen. Heben Sie andernfalls die Freigabe nicht auf.

    ```bash
    sudo umount <IPAddressOfNFSServer>:<FolderOnNFSServer> <FolderToMountIn>
    ```

    \<IPAddressOfNFSServer> ist die IP-Adresse des NFS-Servers, den Sie verwenden werden.

    \<FolderOnNFSServer> ist der Name der NFS-Freigabe.

    \<FolderMountedIn> ist der Ordner, der im vorherigen Schritt erstellt wurde. 

4. Führen Sie folgende Schritte aus, wenn es nicht um Systemdatenbanken, sondern um Benutzerdatenbanken oder Sicherungen geht. Wenn nur der Standardspeicherort verwendet wird, können Sie mit Schritt 5 fortfahren.

   * Wechseln Sie zum Superuser. Wenn dies erfolgreich war, erhalten Sie keine Bestätigung.

    ```bash
    sudo -i
    ```

   * Erstellen Sie einen Ordner, der von SQL Server verwendet wird. 

    ```bash
    mkdir <FolderName>
    ```

    \<FolderName> ist der Name des Ordners. Wenn sich der Ordner nicht am richtigen Speicherort befindet, muss der vollständige Pfad angegeben werden. Im folgenden Beispiel wird ein Ordner mit dem Namen „/var/opt/mssql/userdata“ erstellt.

    ```bash
    mkdir /var/opt/mssql/userdata
    ```

   * Binden Sie die NFS-Freigabe in den Ordner ein, der im vorherigen Schritt erstellt wurde. Wenn dies erfolgreich war, erhalten Sie keine Bestätigung.

    ```bash
    Mount -t nfs4 <IPAddressOfNFSServer>:<FolderOnNFSServer> <FolderToMountIn> -o nfsvers=4.2,timeo=14,intr
    ```

    \<IPAddressOfNFSServer> ist die IP-Adresse des NFS-Servers, den Sie verwenden werden.

    \<FolderOnNFSServer> ist der Name der NFS-Freigabe.

    \<FolderToMountIn> ist der Ordner, der im vorherigen Schritt erstellt wurde. Unten finden Sie ein Beispiel. 

    ```bash
    mount -t nfs4 200.201.202.63:/var/nfs/fci2 /var/opt/mssql/userdata -o nfsvers=4.2,timeo=14,intr
    ```

   * Überprüfen Sie, ob die Einbindung erfolgreich war, indem Sie „mount“ ohne Parameter ausgeben.
  
   * Geben Sie „exit“ ein, um nicht mehr der Superuser zu sein.

   * Erstellen Sie eine Datenbank in diesem Ordner, um dies zu testen. Im folgenden Beispiel wird „sqlcmd“ verwendet, um eine Datenbank zu erstellen, den Kontext zu wechseln und zu überprüfen, ob die Dateien auf der Betriebssystemebene vorhanden sind. Anschließend wird der temporäre Speicherort gelöscht. Sie können SSMS verwenden.

    ![Screenshot: sqlcmd-Befehl und Antwort darauf][4]
 
   * Heben Sie die Einbindung der Freigabe auf. 

    ```bash
    sudo umount <IPAddressOfNFSServer>:<FolderOnNFSServer> <FolderToMountIn>
    ```

    \<IPAddressOfNFSServer> ist die IP-Adresse des NFS-Servers, den Sie verwenden werden.
    
    \<FolderOnNFSServer> ist der Name der NFS-Freigabe.

    \<FolderMountedIn> ist der Ordner, der im vorherigen Schritt erstellt wurde. Unten finden Sie ein Beispiel. 
 
5. Wiederholen Sie die Schritte auf dem oder den anderen Knoten.


## <a name="next-steps"></a>Nächste Schritte

[Konfigurieren einer Failoverclusterinstanz – SQL Server für Linux](sql-server-linux-shared-disk-cluster-configure.md)

<!--Image references-->
[1]: ./media/sql-server-linux-shared-disk-cluster-configure-nfs/05-nfsacl.png
[2]: ./media/sql-server-linux-shared-disk-cluster-configure-nfs/10-mountnoswitches.png
[3]: ./media/sql-server-linux-shared-disk-cluster-configure-nfs/20-createtestdatabase.png
[4]: ./media/sql-server-linux-shared-disk-cluster-configure-nfs/15-createtestdatabase.png
