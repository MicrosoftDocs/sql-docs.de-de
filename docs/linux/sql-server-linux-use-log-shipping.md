---
title: Konfigurieren des Protokollversands für SQL Server für Linux
description: Dieses Tutorial zeigt ein einfaches Beispiel für die Replikation einer SQL Server-Instanz unter Linux auf einer sekundären Instanz mithilfe des Protokollversands.
author: VanMSFT
ms.author: vanto
ms.date: 07/01/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 9385bcc04935c90ff060201d38e676b6da226e33
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346209"
---
# <a name="get-started-with-log-shipping-on-linux"></a>Erste Schritte mit dem Protokollversand unter Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Der SQL Server-Protokollversand ist eine Hochverfügbarkeitskonfiguration, bei der eine Datenbank von einem primären Server auf einem oder mehreren sekundären Servern repliziert wird. Kurz gesagt, wird eine Sicherung der Quelldatenbank auf dem sekundären Server wiederhergestellt. Der primäre Server erstellt dann regelmäßig Transaktionsprotokollsicherungen, die von den sekundären Servern wiederhergestellt werden, wobei die sekundäre Kopie der Datenbank aktualisiert wird. 

  ![Das Diagramm zeigt den Workflow für den Protokollversand.](https://preview.ibb.co/hr5Ri5/logshipping.png)

Wie in dieser Abbildung gezeigt, umfasst eine Protokollversandsitzung die folgenden Schritte:

- Sichern der Transaktionsprotokolldatei auf der primären SQL Server-Instanz.
- Kopieren der Transaktionsprotokoll-Sicherungsdatei über das Netzwerk in eine oder mehrere sekundäre SQL Server-Instanzen.
- Wiederherstellen der Transaktionsprotokoll-Sicherungsdatei auf der sekundären SQL Server-Instanz.

## <a name="prerequisites"></a>Voraussetzungen
- [Installieren des SQL Server-Agent unter Linux](./sql-server-linux-setup-sql-agent.md)

## <a name="setup-a-network-share-for-log-shipping-using-cifs"></a>Einrichten einer Netzwerkfreigabe für den Protokollversand mithilfe von CIFS 

> [!NOTE] 
> In diesem Tutorial werden CIFS und Samba zum Einrichten der Netzwerkfreigabe verwendet. Wenn Sie NFS verwenden möchten, hinterlassen Sie einen Kommentar, und wir fügen ihn dem Dokument hinzu.       

### <a name="configure-primary-server"></a>Konfigurieren des primären Servers
-   Führen Sie Folgendes aus, um Samba zu installieren.

    ```bash
    sudo apt-get install samba #For Ubuntu
    sudo yum -y install samba #For RHEL/CentOS
    ```
-   Erstellen Sie ein Verzeichnis zum Speichern der Protokolle für den Protokollversand, und gewähren Sie MSSQL die erforderlichen Berechtigungen.

    ```bash
    mkdir /var/opt/mssql/tlogs
    chown mssql:mssql /var/opt/mssql/tlogs
    chmod 0700 /var/opt/mssql/tlogs
    ```

-   Bearbeiten Sie die Datei „/etc/samba/smb.conf“ (Sie benötigen hierfür Stammverzeichnisberechtigungen), und fügen Sie den folgenden Abschnitt hinzu:

    ```bash
    [tlogs]
    path=/var/opt/mssql/tlogs
    available=yes
    read only=yes
    browsable=yes
    public=yes
    writable=no
    ```

-   Erstellen eines MSSQL-Benutzers für Samba

    ```bash
    sudo smbpasswd -a mssql
    ```

-   Neustart der Samba-Dienste
    ```bash
    sudo systemctl restart smbd.service nmbd.service
    ```
 
### <a name="configure-secondary-server"></a>Konfigurieren des sekundären Servers

-   Führen Sie Folgendes aus, um den CIFS-Client zu installieren.
    ```bash   
    sudo apt-get install cifs-utils #For Ubuntu
    sudo yum -y install cifs-utils #For RHEL/CentOS
    ```

-   Erstellen Sie eine Datei zum Speichern Ihrer Anmeldeinformationen. Verwenden Sie das Kennwort, das Sie kürzlich für Ihr MSSQL-Samba-Konto festgelegt haben. 

    ```console
        vim /var/opt/mssql/.tlogcreds
        #Paste the following in .tlogcreds
        username=mssql
        domain=<domain>
        password=<password>
    ```

-   Führen Sie die folgenden Befehle aus, um ein leeres Verzeichnis zum Einbinden zu erstellen und Berechtigung und Besitz richtig festzulegen.
    ```bash   
    mkdir /var/opt/mssql/tlogs
    sudo chown root:root /var/opt/mssql/tlogs
    sudo chmod 0550 /var/opt/mssql/tlogs
    sudo chown root:root /var/opt/mssql/.tlogcreds
    sudo chmod 0660 /var/opt/mssql/.tlogcreds
    ```

-   Fügen Sie „etc/fstab“ die Zeile hinzu, um die Freigabe beizubehalten. 

    ```console
        //<ip_address_of_primary_server>/tlogs /var/opt/mssql/tlogs cifs credentials=/var/opt/mssql/.tlogcreds,ro,uid=mssql,gid=mssql 0 0
    ```

-   Binden Sie die Freigaben ein.
    ```bash   
    sudo mount -a
    ```
       
## <a name="setup-log-shipping-via-t-sql"></a>Einrichten des Protokollversands über T-SQL

- Führen Sie dieses Skript auf Ihrem primären Server aus.

    ```sql
    BACKUP DATABASE SampleDB
    TO DISK = '/var/opt/mssql/tlogs/SampleDB.bak'
    GO
    ```

    ```sql
    DECLARE @LS_BackupJobId AS uniqueidentifier 
    DECLARE @LS_PrimaryId   AS uniqueidentifier 
    DECLARE @SP_Add_RetCode As int 
    EXECUTE @SP_Add_RetCode = master.dbo.sp_add_log_shipping_primary_database 
             @database = N'SampleDB' 
            ,@backup_directory = N'/var/opt/mssql/tlogs' 
            ,@backup_share = N'/var/opt/mssql/tlogs' 
            ,@backup_job_name = N'LSBackup_SampleDB' 
            ,@backup_retention_period = 4320
            ,@backup_compression = 2
            ,@backup_threshold = 60 
            ,@threshold_alert_enabled = 1
            ,@history_retention_period = 5760 
            ,@backup_job_id = @LS_BackupJobId OUTPUT 
            ,@primary_id = @LS_PrimaryId OUTPUT 
            ,@overwrite = 1 

    IF (@@ERROR = 0 AND @SP_Add_RetCode = 0) 
    BEGIN 

    DECLARE @LS_BackUpScheduleUID   As uniqueidentifier 
    DECLARE @LS_BackUpScheduleID    AS int 

    EXECUTE msdb.dbo.sp_add_schedule 
            @schedule_name =N'LSBackupSchedule' 
            ,@enabled = 1 
            ,@freq_type = 4 
            ,@freq_interval = 1 
            ,@freq_subday_type = 4 
            ,@freq_subday_interval = 15 
            ,@freq_recurrence_factor = 0 
            ,@active_start_date = 20170418 
            ,@active_end_date = 99991231 
            ,@active_start_time = 0 
            ,@active_end_time = 235900 
            ,@schedule_uid = @LS_BackUpScheduleUID OUTPUT 
            ,@schedule_id = @LS_BackUpScheduleID OUTPUT 

    EXECUTE msdb.dbo.sp_attach_schedule 
            @job_id = @LS_BackupJobId 
            ,@schedule_id = @LS_BackUpScheduleID  

    EXECUTE msdb.dbo.sp_update_job 
            @job_id = @LS_BackupJobId 
            ,@enabled = 1 
            
    END 

    EXECUTE master.dbo.sp_add_log_shipping_alert_job 

    EXECUTE master.dbo.sp_add_log_shipping_primary_secondary 
            @primary_database = N'SampleDB' 
            ,@secondary_server = N'<ip_address_of_secondary_server>' 
            ,@secondary_database = N'SampleDB' 
            ,@overwrite = 1 
    ```


- Führen Sie dieses Skript auf Ihrem sekundären Server aus.

    ```sql
    RESTORE DATABASE SampleDB FROM DISK = '/var/opt/mssql/tlogs/SampleDB.bak'
    WITH NORECOVERY;
    ```
    
    ```sql
    DECLARE @LS_Secondary__CopyJobId    AS uniqueidentifier 
    DECLARE @LS_Secondary__RestoreJobId AS uniqueidentifier 
    DECLARE @LS_Secondary__SecondaryId  AS uniqueidentifier 
    DECLARE @LS_Add_RetCode As int 

    EXECUTE @LS_Add_RetCode = master.dbo.sp_add_log_shipping_secondary_primary 
            @primary_server = N'<ip_address_of_primary_server>' 
            ,@primary_database = N'SampleDB' 
            ,@backup_source_directory = N'/var/opt/mssql/tlogs/' 
            ,@backup_destination_directory = N'/var/opt/mssql/tlogs/' 
            ,@copy_job_name = N'LSCopy_SampleDB' 
            ,@restore_job_name = N'LSRestore_SampleDB' 
            ,@file_retention_period = 4320 
            ,@overwrite = 1 
            ,@copy_job_id = @LS_Secondary__CopyJobId OUTPUT 
            ,@restore_job_id = @LS_Secondary__RestoreJobId OUTPUT 
            ,@secondary_id = @LS_Secondary__SecondaryId OUTPUT 

    IF (@@ERROR = 0 AND @LS_Add_RetCode = 0) 
    BEGIN 

    DECLARE @LS_SecondaryCopyJobScheduleUID As uniqueidentifier 
    DECLARE @LS_SecondaryCopyJobScheduleID  AS int 

    EXECUTE msdb.dbo.sp_add_schedule 
            @schedule_name =N'DefaultCopyJobSchedule' 
            ,@enabled = 1 
            ,@freq_type = 4 
            ,@freq_interval = 1 
            ,@freq_subday_type = 4 
            ,@freq_subday_interval = 15 
            ,@freq_recurrence_factor = 0 
            ,@active_start_date = 20170418 
            ,@active_end_date = 99991231 
            ,@active_start_time = 0 
            ,@active_end_time = 235900 
            ,@schedule_uid = @LS_SecondaryCopyJobScheduleUID OUTPUT 
            ,@schedule_id = @LS_SecondaryCopyJobScheduleID OUTPUT 

    EXECUTE msdb.dbo.sp_attach_schedule 
            @job_id = @LS_Secondary__CopyJobId 
            ,@schedule_id = @LS_SecondaryCopyJobScheduleID  

    DECLARE @LS_SecondaryRestoreJobScheduleUID  As uniqueidentifier 
    DECLARE @LS_SecondaryRestoreJobScheduleID   AS int 

    EXECUTE msdb.dbo.sp_add_schedule 
            @schedule_name =N'DefaultRestoreJobSchedule' 
            ,@enabled = 1 
            ,@freq_type = 4 
            ,@freq_interval = 1 
            ,@freq_subday_type = 4 
            ,@freq_subday_interval = 15 
            ,@freq_recurrence_factor = 0 
            ,@active_start_date = 20170418 
            ,@active_end_date = 99991231 
            ,@active_start_time = 0 
            ,@active_end_time = 235900 
            ,@schedule_uid = @LS_SecondaryRestoreJobScheduleUID OUTPUT 
            ,@schedule_id = @LS_SecondaryRestoreJobScheduleID OUTPUT 

    EXECUTE msdb.dbo.sp_attach_schedule 
            @job_id = @LS_Secondary__RestoreJobId 
            ,@schedule_id = @LS_SecondaryRestoreJobScheduleID  
            
    END 
    DECLARE @LS_Add_RetCode2    As int 
    IF (@@ERROR = 0 AND @LS_Add_RetCode = 0) 
    BEGIN 

    EXECUTE @LS_Add_RetCode2 = master.dbo.sp_add_log_shipping_secondary_database 
            @secondary_database = N'SampleDB' 
            ,@primary_server = N'<ip_address_of_primary_server>' 
            ,@primary_database = N'SampleDB' 
            ,@restore_delay = 0 
            ,@restore_mode = 0 
            ,@disconnect_users  = 0 
            ,@restore_threshold = 45   
            ,@threshold_alert_enabled = 1 
            ,@history_retention_period  = 5760 
            ,@overwrite = 1 

    END 

    IF (@@error = 0 AND @LS_Add_RetCode = 0) 
    BEGIN 

    EXECUTE msdb.dbo.sp_update_job 
            @job_id = @LS_Secondary__CopyJobId 
            ,@enabled = 1 

    EXECUTE msdb.dbo.sp_update_job 
            @job_id = @LS_Secondary__RestoreJobId 
            ,@enabled = 1 

    END 
    ```

## <a name="verify-log-shipping-works"></a>Überprüfen der Funktion des Protokollversands

- Überprüfen Sie, ob der Protokollversand funktioniert, indem Sie den folgenden Auftrag auf dem primären Server starten.

    ```sql
    USE msdb ;  
    GO  

    EXECUTE dbo.sp_start_job N'LSBackup_SampleDB' ;  
    GO  
    ```

- Überprüfen Sie, ob der Protokollversand funktioniert, indem Sie den folgenden Auftrag auf dem sekundären Server starten.
 
    ```sql
    USE msdb ;  
    GO  

    EXECUTE dbo.sp_start_job N'LSCopy_SampleDB' ;  
    GO  
    EXECUTE dbo.sp_start_job N'LSRestore_SampleDB' ;  
    GO  
    ```
 - Überprüfen Sie, ob das Failover für den Protokollversand funktioniert, indem Sie den folgenden Befehl ausführen:
 
    > [!WARNING]
    > Mit diesem Befehl wird die sekundäre Datenbank online geschaltet, und die Protokollversandkonfiguration wird unterbrochen. Nach dem Ausführen dieses Befehls müssen Sie den Protokollversand neu konfigurieren.
 
    ```sql
    RESTORE DATABASE SampleDB WITH RECOVERY;
    ```