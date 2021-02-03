---
title: Sichern des Diensthauptschlüssels | Microsoft-Dokumentation
description: Erfahren Sie, wie der Diensthauptschlüssel in SQL Server mithilfe von Transact-SQL gesichert wird. Der Diensthauptschlüssel ist der Stamm der Verschlüsselungshierarchie.
ms.custom: ''
ms.date: 01/02/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- service master key [SQL Server], exporting
ms.assetid: f60b917c-6408-48be-b911-f93b05796904
author: jaszymas
ms.author: jaszymas
ms.openlocfilehash: 7c95feda5457c3623a893e77771dbdc10ea28257
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250684"
---
# <a name="back-up-the-service-master-key"></a>Sichern des Diensthauptschlüssels
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In diesem Artikel wird beschrieben, wie der Diensthauptschlüssel in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] mithilfe von [!INCLUDE[tsql](../../../includes/tsql-md.md)] gesichert wird. Der Diensthauptschlüssel ist der Stamm der Verschlüsselungshierarchie. Er sollte gesichert und an einem sicheren Ort außerhalb der Geschäftsräume aufbewahrt werden. Das Erstellen dieser Sicherung sollte eine der ersten administrativen Aktionen sein, die auf dem Server ausgeführt werden.  

## <a name="before-you-begin"></a>Vorbereitungen  
  
### <a name="limitations-and-restrictions"></a>Einschränkungen  

- Der Hauptschlüssel muss geöffnet und entschlüsselt sein, bevor er gesichert wird. Wenn er mit dem Diensthauptschlüssel verschlüsselt wird, muss der Hauptschlüssel nicht explizit geöffnet werden. Wenn der Hauptschlüssel jedoch nur mit einem Kennwort verschlüsselt wird, muss er explizit geöffnet werden.  
  
- Es wird empfohlen, dass Sie sofort nach der Erstellung eine Sicherung des Hauptschlüssels anlegen und diese an einem sicheren Ort außerhalb Ihrer Geschäftsräume aufbewahren.  
  
## <a name="security"></a>Sicherheit  
  
### <a name="permissions"></a>Berechtigungen
Erfordert die CONTROL-Berechtigung für die Datenbank.  
  
## <a name="using-transact-sql"></a>Verwenden von Transact-SQL  
  
### <a name="to-back-up-the-service-master-key"></a>So sichern Sie den Diensthauptschlüssel
  
1. Stellen Sie in [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]eine Verbindung mit der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Instanz her, die den zu sichernden Diensthauptschlüssel enthält.  
  
2. Wählen Sie das Kennwort, das zum Verschlüsseln des Diensthauptschlüssels auf dem Sicherungsmedium verwendet werden soll. Dieses Kennwort unterliegt Komplexitätsüberprüfungen. Weitere Informationen finden Sie unter [Password Policy](../../../relational-databases/security/password-policy.md).  
  
3. Verwenden Sie ein Wechselsicherungsmedium, auf dem eine Kopie des gesicherten Schlüssels gespeichert wird.  
  
4. Ermitteln Sie ein NTFS-Verzeichnis, in dem die Sicherung des Schlüssels erstellt werden soll. In diesem Verzeichnis erstellen Sie die im nächsten Schritt beschriebene Datei. Das Verzeichnis sollte durch stark einschränkende Zugriffssteuerungslisten (Access Control Lists, ACLs) geschützt sein.  
  
5. Stellen Sie im **Objekt-Explorer** eine Verbindung mit einer [!INCLUDE[ssDE](../../../includes/ssde-md.md)]-Instanz her.  
  
6. Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
7. Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**.  
  
    ```sql
    -- Creates a backup of the service master key.
    USE master;
    GO
    BACKUP SERVICE MASTER KEY TO FILE = 'c:\temp_backups\keys\service_master_ key'
        ENCRYPTION BY PASSWORD = '3dH85Hhk003GHk2597gheij4';
    GO
    ```  
  
    > [!NOTE]  
    > Der Dateipfad zum Schlüssel und das Kennwort (sofern es vorhanden ist) des Schlüssels unterscheiden sich von den obigen Informationen. Stellen Sie sicher, dass beide für den Server und die Schlüsseleinrichtung spezifisch sind.
  
8. Kopieren Sie die Datei auf das Sicherungsmedium, und überprüfen Sie die Kopie.  
  
9. Verwahren Sie die Sicherung an einem sicheren Ort außerhalb der Geschäftsräume.  

 Weitere Informationen finden Sie unter [OPEN MASTER KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/open-master-key-transact-sql.md) und [BACKUP MASTER KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/backup-master-key-transact-sql.md).  
