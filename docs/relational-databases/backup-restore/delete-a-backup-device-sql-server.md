---
title: Löschen eines Sicherungsmediums (SQL Server) | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie ein Sicherungsmedium in SQL Server mithilfe von SQL Server Management Studio oder Transact-SQL entfernen.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- database backups [SQL Server], deleting devices
- backup devices [SQL Server], deleting
- deleting backup devices
- removing backup devices
- backing up databases [SQL Server], backup devices
ms.assetid: 7be62480-ed6a-4262-a071-1feba73b1c02
author: cawrites
ms.author: chadam
ms.openlocfilehash: ea8bb759da76fef8336efd8493aee1bd8d692452
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076282"
---
# <a name="delete-a-backup-device-sql-server"></a>Löschen eines Sicherungsmediums (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In diesem Thema wird beschrieben, wie Sie ein Sicherungsmedium in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[tsql](../../includes/tsql-md.md)]löschen können.  
  
 **In diesem Thema**  
  
-   **Vorbereitungen:**  
  
     [Security](#Security)  
  
-   **So löschen Sie ein Sicherungsmedium mit**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Erfordert die Mitgliedschaft in der festen Serverrolle **diskadmin** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
  
#### <a name="to-delete-a-backup-device"></a>So löschen Sie ein Sicherungsmedium  
  
1.  Stellen Sie eine Verbindung mit der entsprechenden Instanz von [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]her, und klicken Sie danach im Objekt-Explorer auf den Servernamen, um die Serverstruktur zu erweitern.  
  
2.  Erweitern Sie **Serverobjekte** und anschließend **Sicherungsmedien**.  
  
3.  Klicken Sie mit der rechten Maustaste auf das gewünschte Medium, und klicken Sie dann auf **Löschen**.  
  
4.  Überprüfen Sie im Dialogfeld **Objekt löschen** , ob in der Spalte **Objektname** der richtige Medienname angezeigt wird.  
  
5.  Klicken Sie auf **OK**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
  
#### <a name="to-delete-a-backup-device"></a>So löschen Sie ein Sicherungsmedium  
  
1.  Stellen Sie eine Verbindung mit dem [!INCLUDE[ssDE](../../includes/ssde-md.md)]her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Kopieren Sie das folgende Beispiel, und fügen Sie es in das Abfragefenster ein. In diesem Beispiel wird gezeigt, wie [sp_dropdevice](../../relational-databases/system-stored-procedures/sp-dropdevice-transact-sql.md) verwendet werden muss, um ein Sicherungsmedium zu löschen. Führen Sie das erste Beispiel aus, um das `mybackupdisk` -Sicherungsmedium und den physischen Namen `c:\backup\backup1.bak`zu erstellen. Führen Sie **sp_dropdevice** aus, um das `mybackupdisk` -Sicherungsmedium zu löschen. Der physische Namen wird vom `delfile` -Parameter gelöscht.  
  
```sql  
--Define a backup device and physical name.   
USE AdventureWorks2012 ;  
GO  
EXEC sp_addumpdevice 'disk', 'mybackupdisk', 'c:\backup\backup1.bak' ;  
GO  
--Delete the backup device and the physical name.  
USE AdventureWorks2012 ;  
GO  
EXEC sp_dropdevice ' mybackupdisk ', 'delfile' ;  
GO  
  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Anzeigen der Eigenschaften und des Inhalts eines logischen Sicherungsmediums &#40;SQL Server&#41;](../../relational-databases/backup-restore/view-the-properties-and-contents-of-a-logical-backup-device-sql-server.md)   
 [sys.backup_devices &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-backup-devices-transact-sql.md)   
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [Sicherungsmedien &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md)   
 [sp_addumpdevice &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md)  
  
  
