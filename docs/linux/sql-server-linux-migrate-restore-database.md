---
title: Migrieren einer SQL Server-Datenbank von Windows zu Linux
description: In diesem Tutorial erfahren Sie, wie Sie eine SQL Server-Datenbanksicherung von Windows auf einem Linux-Computer mit SQL Server wiederherstellen.
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 08/16/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 9ac64d1a-9fe5-446e-93c3-d17b8f55a28f
ms.openlocfilehash: 2ae765c8bc1f274ab9b9f62978a85b5f09f6ca58
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349086"
---
# <a name="migrate-a-sql-server-database-from-windows-to-linux-using-backup-and-restore"></a>Migrieren einer SQL Server-Datenbank von Windows zu Linux mithilfe der Funktion Sichern und Wiederherstellen

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Wenn Sie eine Datenbank von SQL Server unter Windows zu SQL Server unter Linux migrieren möchten, wird die Verwendung der SQL Server-Funktion Sichern und Wiederherstellen empfohlen. Dieses Tutorial enthält die erforderlichen Schritte, um eine Datenbank mithilfe von Sicherungs- und Wiederherstellungsmethoden zu Linux zu verschieben.

> [!div class="checklist"]
> * Erstellen einer Sicherungsdatei unter Windows mit SSMS
> * Installieren einer Bash-Shell unter Windows
> * Verschieben der Sicherungsdatei von der Bash-Shell zu Linux
> * Wiederherstellen der Sicherungsdatei unter Linux mit Transact-SQL
> * Ausführen einer Abfrage zur Überprüfung der Migration

Eine weitere Möglichkeit zur Migration einer SQL Server-Datenbank von Windows zu Linux besteht darin, eine SQL Server Always On-Verfügbarkeitsgruppe zu erstellen. Informationen hierzu finden Sie unter [sql-server-linux-availability-group-cross-platform](sql-server-linux-availability-group-cross-platform.md).

## <a name="prerequisites"></a>Voraussetzungen

Zur Durchführung dieses Tutorials ist Folgendes erforderlich:

* Ein Windows-Computer mit folgenden Komponenten:
  * Installierte [SQL Server](https://www.microsoft.com/sql-server/sql-server-downloads)-Instanz
  * Installiertes [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md)
  * Zieldatenbank, die migriert werden soll

* Ein Linux-Computer, auf dem folgende Komponenten installiert sind:
  * SQL Server ([RHEL](quickstart-install-connect-red-hat.md), [SLES](quickstart-install-connect-suse.md) oder [Ubuntu](quickstart-install-connect-ubuntu.md)) mit Befehlszeilentools.

## <a name="create-a-backup-on-windows"></a>Erstellen einer Sicherung unter Windows

Es gibt mehrere Möglichkeiten, unter Windows eine Sicherungsdatei einer Datenbank zu erstellen. In den folgenden Schritten wird hierzu SQL Server Management Studio (SSMS) verwendet:

1. Starten Sie auf Ihrem Windows-Computer **SQL Server Management Studio**.

1. Geben Sie im Verbindungsdialogfeld **localhost** ein.

1. Erweitern Sie im Objekt-Explorer den Eintrag **Datenbanken**.

1. Klicken Sie mit der rechten Maustaste auf die Zieldatenbank, wählen Sie **Aufgaben** aus, und klicken Sie dann auf **Sichern...** .

   ![Erstellen einer Sicherungsdatei mit SSMS](./media/sql-server-linux-migrate-restore-database/ssms-create-backup.png)

1. Vergewissern Sie sich, dass im Dialogfeld **Datenbank sichern** die Option **Sicherungstyp** auf **Vollständig** und die Option **Sichern auf** auf **Datenträger** festgelegt ist. Notieren Sie sich Name und Speicherort der Datei. Eine Datenbank mit dem Namen **YourDB** in SQL Server 2016 besitzt beispielsweise den Standardsicherungspfad `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Backup\YourDB.bak`.

1. Klicken Sie auf **OK**, um die Datenbank zu sichern.

> [!NOTE]
> Eine weitere Möglichkeit zum Erstellen der Sicherungsdatei besteht darin, eine Transact-SQL-Abfrage auszuführen. Mit dem folgenden Transact-SQL-Befehl werden für eine Datenbank namens **YourDB** dieselben Aktionen ausgeführt wie durch die vorherigen Schritte:
>
> ```sql
> BACKUP DATABASE [YourDB] TO  DISK =
> N'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Backup\YourDB.bak'
> WITH NOFORMAT, NOINIT, NAME = N'YourDB-Full Database Backup',
> SKIP, NOREWIND, NOUNLOAD, STATS = 10
> GO
> ```

## <a name="install-a-bash-shell-on-windows"></a>Installieren einer Bash-Shell unter Windows

Übertragen Sie zum Wiederherstellen der Datenbank zunächst die Sicherungsdatei vom Windows-Computer auf den Linux-Zielcomputer. In diesem Tutorial wird die Datei aus einer Bash-Shell (Terminalfenster), die unter Windows ausgeführt wird, zu Linux verschoben.

1. Installieren Sie auf Ihrem Windows-Computer eine Bash-Shell, die die Befehle **scp** (sicheres Kopieren) und **ssh** (Remoteanmeldung) unterstützt. Zwei Beispiele hierfür sind:

   * [Windows-Subsystem für Linux](/windows/wsl/about) (Windows 10)
   * Bash-Shell von Git ([https://git-scm.com/downloads](https://git-scm.com/downloads))

1. Öffnen Sie eine Bash-Sitzung unter Windows.

## <a name="copy-the-backup-file-to-linux"></a><a id="scp"></a> Kopieren der Sicherungsdatei zu Linux

1. Navigieren Sie in Ihrer Bash-Sitzung zum Verzeichnis, das die Sicherungsdatei enthält. Beispiel:

   ```bash
   cd 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Backup\'
   ```

1. Übertragen Sie mit dem Befehl **scp** die Datei auf den Linux-Zielcomputer. Im folgenden Beispiel wird die Datei **YourDB.bak** in das Basisverzeichnis von *user1* auf dem Linux-Server mit der IP-Adresse *192.0.2.9* übertragen:

   ```bash
   scp YourDB.bak user1@192.0.2.9:./
   ```
   ![Befehl „scp“](./media/sql-server-linux-migrate-restore-database/scp-command.png)

> [!TIP]
> Für die Übertragung der Datei gibt es weitere Möglichkeiten. So können Sie beispielsweise mit [Samba](https://help.ubuntu.com/community/Samba) eine SMB-Netzwerkfreigabe zwischen Windows und Linux konfigurieren. Eine exemplarische Vorgehensweise unter Ubuntu finden Sie unter [How to Create a Network Share Via Samba (Erstellen einer Netzwerkfreigabe mit Samba)](https://help.ubuntu.com/community/How%20to%20Create%20a%20Network%20Share%20Via%20Samba%20Via%20CLI%20%28Command-line%20interface/Linux%20Terminal%29%20-%20Uncomplicated,%20Simple%20and%20Brief%20Way!). Nachdem Sie die Freigabe erstellt haben, können Sie darauf von Windows aus wie auf eine Netzwerkdateifreigabe zugreifen (z. B. **\\\\machinenameorip\\share**).

## <a name="move-the-backup-file-before-restoring"></a>Verschieben der Sicherungsdatei vor der Wiederherstellung

Nun befindet sich die Sicherungsdatei auf dem Linux-Server im Basisverzeichnis Ihres Benutzers. Bevor Sie die Datenbank in SQL Server wiederherstellen, müssen Sie die Sicherung in ein Unterverzeichnis von **/var/opt/mssql** verschieben.

1. Stellen Sie in derselben Bash-Sitzung unter Windows eine Remoteverbindung mit Ihrem Linux-Zielcomputer über **ssh** her. Im folgenden Beispiel wird eine Verbindung mit dem Linux-Computer **192.0.2.9** für den Benutzer **user1** hergestellt:

   ```bash
   ssh user1@192.0.2.9
   ```

   Jetzt führen Sie Befehle auf dem Linux-Remoteserver aus.

1. Wechseln Sie in den Administratormodus.

   ```bash
   sudo su
   ```

1. Erstellen Sie ein neues Sicherungsverzeichnis. Der Parameter „-p“ hat keine Auswirkungen, sollte das Verzeichnis bereits vorhanden sein.

   ```bash
   mkdir -p /var/opt/mssql/backup
   ```

1. Verschieben Sie die Sicherungsdatei in dieses Verzeichnis. Im folgenden Beispiel befindet sich die Sicherungsdatei im Basisverzeichnis von *user1*. Ändern Sie den Befehl so, dass er mit dem Speicherort und dem Dateinamen Ihrer Sicherungsdatei übereinstimmt.

   ```bash
   mv /home/user1/YourDB.bak /var/opt/mssql/backup/
   ```

1. Beenden Sie den Administratormodus.

   ```bash
   exit
   ```

## <a name="restore-your-database-on-linux"></a>Wiederherstellen der Datenbank unter Linux

Verwenden Sie zum Wiederherstellen der Datenbanksicherung den Transact-SQL-Befehl (TQL) **RESTORE DATABASE**.

> [!NOTE]
> In den folgenden Schritten wird das Tool **sqlcmd** verwendet. Wenn Sie SQL Server-Tools noch nicht installiert haben, finden Sie Informationen zur Vorgehensweise unter [Install SQL Server command-line tools on Linux (Installieren von SQL Server-Befehlszeilentools unter Linux)](sql-server-linux-setup-tools.md).

1. Starten Sie im selben Terminal das Tool **sqlcmd**. Im folgenden Beispiel wird für den **SA**-Benutzer eine Verbindung mit der lokalen SQL Server-Instanz hergestellt. Geben Sie das Kennwort ein, wenn Sie dazu aufgefordert werden. Alternativ dazu können Sie das Kennwort angeben, indem Sie den Parameter **-P** hinzufügen.

   ```bash
   sqlcmd -S localhost -U SA
   ```

1. Geben Sie bei der Eingabeaufforderung `>1` den folgenden Befehl vom Typ **RESTORE DATABASE** ein, und drücken Sie nach jeder Zeile die EINGABETASTE (der mehrzeilige Befehl kann nicht auf einmal kopiert und eingefügt werden). Ersetzen Sie `YourDB` jedes Mal durch den Namen Ihrer Datenbank.

   ```sql
   RESTORE DATABASE YourDB
   FROM DISK = '/var/opt/mssql/backup/YourDB.bak'
   WITH MOVE 'YourDB' TO '/var/opt/mssql/data/YourDB.mdf',
   MOVE 'YourDB_Log' TO '/var/opt/mssql/data/YourDB_Log.ldf'
   GO
   ```

   Nun sollten Sie die Meldung erhalten, dass die Datenbank erfolgreich wiederhergestellt wurde.

   Möglicherweise gibt `RESTORE DATABASE` einen Fehler zurück, der etwa wie folgt aussieht:

   ```bash
   File 'YourDB_Product' cannot be restored to 'Z:\Microsoft SQL Server\MSSQL11.GLOBAL\MSSQL\Data\YourDB\YourDB_Product.ndf'. Use WITH MOVE to identify a valid location for the file.
   Msg 5133, Level 16, State 1, Server servername, Line 1
   Directory lookup for the file "Z:\Microsoft SQL Server\MSSQL11.GLOBAL\MSSQL\Data\YourDB\YourDB_Product.ndf" failed with the operating system error 2(The system cannot find the file specified.).
   ```
   
   In diesem Fall enthält die Datenbank sekundäre Dateien. Sind diese Dateien nicht in der `MOVE`-Klausel von `RESTORE DATABASE` angegeben, wird im Wiederherstellungsverfahren versucht, sie im gleichen Pfad wie der ursprüngliche Server zu erstellen. 

   Sie können alle in der Sicherung enthaltenen Dateien auflisten:
   ```sql
   RESTORE FILELISTONLY FROM DISK = '/var/opt/mssql/backup/YourDB.bak'
   GO
   ```
   Die angezeigte Liste sollte in etwa folgendermaßen aussehen (nur die beiden ersten Spalten werden angezeigt):
   ```sql
   LogicalName         PhysicalName                                                                 ..............
   ----------------------------------------------------------------------------------------------------------------------
   YourDB              Z:\Microsoft SQL Server\MSSQL11.GLOBAL\MSSQL\Data\YourDB\YourDB.mdf          ..............
   YourDB_Product      Z:\Microsoft SQL Server\MSSQL11.GLOBAL\MSSQL\Data\YourDB\YourDB_Product.ndf  ..............
   YourDB_Customer     Z:\Microsoft SQL Server\MSSQL11.GLOBAL\MSSQL\Data\YourDB\YourDB_Customer.ndf ..............
   YourDB_log          Z:\Microsoft SQL Server\MSSQL11.GLOBAL\MSSQL\Data\YourDB\YourDB_Log.ldf      ..............
   ```
   
   Mit dieser Liste können Sie `MOVE`-Klauseln für die zusätzlichen Dateien erstellen. In diesem Beispiel sieht `RESTORE DATABASE` folgendermaßen aus:

   ```sql
   RESTORE DATABASE YourDB
   FROM DISK = '/var/opt/mssql/backup/YourDB.bak'
   WITH MOVE 'YourDB' TO '/var/opt/mssql/data/YourDB.mdf',
   MOVE 'YourDB_Product' TO '/var/opt/mssql/data/YourDB_Product.ndf',
   MOVE 'YourDB_Customer' TO '/var/opt/mssql/data/YourDB_Customer.ndf',
   MOVE 'YourDB_Log' TO '/var/opt/mssql/data/YourDB_Log.ldf'
   GO
   ```


1. Überprüfen Sie die Wiederherstellung, indem Sie alle Datenbanken auf dem Server auflisten. Dabei sollte die wiederhergestellte Datenbank aufgeführt werden.

   ```sql
   SELECT Name FROM sys.Databases
   GO
   ```

1. Führen Sie für die migrierte Datenbank weitere Abfragen aus. Mit dem folgenden Befehl wird ein Kontextwechsel zur Datenbank **YourDB** durchgeführt und Zeilen aus einer der Tabellen daraus ausgewählt.

   ```sql
   USE YourDB
   SELECT * FROM YourTable
   GO
   ```

1. Wenn Sie das Tool **sqlcmd** beenden möchten, geben Sie `exit` ein.

1. Wenn Sie die Remotesitzung über **ssh** beenden möchten, geben Sie noch einmal `exit` ein.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Sie eine Datenbank unter Windows sichern und auf einen Linux-Server mit SQL Server verschieben. Sie haben Folgendes gelernt:
> [!div class="checklist"]
> * Erstellen einer Sicherungsdatei unter Windows mit SSMS und Transact-SQL
> * Installieren einer Bash-Shell unter Windows
> * Verschieben von Sicherungsdateien von Windows zu Linux mit **scp**
> * Herstellen einer Remoteverbindung mit Ihrem Linux-Computer über **ssh**
> * Verschieben der Sicherungsdatei als Vorbereitung für die Wiederherstellung
> * Ausführen von Transact-SQL-Befehlen mit **sqlcmd**
> * Wiederherstellen der Datenbanksicherung mit dem Befehl **RESTORE DATABASE** 
> * Ausführen der Abfrage zur Überprüfung der Migration

Lernen Sie als Nächstes weitere Migrationsszenarios für SQL Server unter Linux kennen. 

> [!div class="nextstepaction"]
>[Migrate databases to SQL Server on Linux (Migrieren von Datenbanken zu SQL Server unter Linux)](sql-server-linux-migrate-overview.md)