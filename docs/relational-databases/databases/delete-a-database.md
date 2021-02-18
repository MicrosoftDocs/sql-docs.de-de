---
title: Löschen einer Datenbank | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie eine benutzerdefinierte Datenbank in SQL Server mithilfe von SQL Server Management Studio oder Transact-SQL löschen.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- database removal [SQL Server], SQL Server Management Studio
- removing databases
- deleting databases
- dropping databases
- databases [SQL Server], dropping
- database removal [SQL Server]
ms.assetid: 1fd8c0f5-03e1-449a-af45-b8cacb479d9c
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e6835865e78147a11878b12ae8ce9c465b66f0e7
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489424"
---
# <a name="delete-a-database"></a>Löschen einer Datenbank
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  In diesem Artikel wird beschrieben, wie eine benutzerdefinierte Datenbank unter [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mit [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[tsql](../../includes/tsql-md.md)] gelöscht wird.  
  
 **In diesem Thema**  
  
-   **Vorbereitungen:**  
  
     [Einschränkungen](#Restrictions)  
  
     [Voraussetzungen](#Prerequisites)  
  
     [Empfehlungen](#Recommendations)  
  
     [Security](#Security)  
  
-   **So löschen Sie eine Datenbank mit:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Nachverfolgung:**  [Nach dem Löschen einer Datenbank](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Einschränkungen  
  
-   Systemdatenbanken können nicht gelöscht werden.  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Voraussetzungen  
  
-   Löschen Sie alle Datenbankmomentaufnahmen, die in der Datenbank vorhanden sind. Weitere Informationen finden Sie unter [Löschen einer Datenbank-Momentaufnahme &#40;Transact-SQL&#41;](../../relational-databases/databases/drop-a-database-snapshot-transact-sql.md)angefügt werden.  
  
-   Wenn für die Datenbank der Protokollversand konfiguriert ist, entfernen Sie diesen.  
  
-   Wenn die Datenbank für die Transaktionsreplikation oder für die Mergereplikation veröffentlicht ist bzw. Abonnent der Mergereplikation ist, entfernen Sie die Replikation aus der Datenbank.  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Empfehlungen  
  
-   Ziehen Sie in Betracht, eine vollständige Sicherung der Datenbank vorzunehmen. Eine gelöschte Datenbank kann nur neu erstellt werden, indem eine Sicherungskopie wiederhergestellt wird.  
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Für die Ausführung von DROP DATABASE benötigt ein Benutzer zumindest die CONTROL-Berechtigung für die Datenbank.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
  
#### <a name="to-delete-a-database"></a>So löschen Sie eine Datenbank  
  
1.  Stellen Sie im **Objekt-Explorer** eine Verbindung zu einer Instanz von [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]her, und erweitern Sie dann diese Instanz.  
  
2.  Erweitern Sie **Datenbanken**, klicken Sie mit der rechten Maustaste auf die zu löschende Datenbank, und klicken Sie dann auf **Löschen**.  
  
3.  Bestätigen Sie, dass die richtige Datenbank ausgewählt ist, und klicken Sie dann auf **OK**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
  
#### <a name="to-delete-a-database"></a>So löschen Sie eine Datenbank  
  
1.  Stellen Sie eine Verbindung mit dem [!INCLUDE[ssDE](../../includes/ssde-md.md)]her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**. Im Beispiel werden die Datenbanken `Sales` und `NewSales` entfernt.  
  
```sql  
USE master ;  
GO  
DROP DATABASE Sales, NewSales ;  
GO  
```  
  
##  <a name="follow-up-after-deleting-a-database"></a><a name="FollowUp"></a>Nächster Schritt: Nach dem Löschen einer Datenbank  
 Sichern Sie die **master** -Datenbank. Wenn die **master** -Datenbank wiederhergestellt werden muss, können Fehlermeldungen auftreten, falls in den Systemkatalogsichten weiterhin Verweise auf Datenbanken bestehen, die seit der letzten Sicherung der **master** -Datenbank gelöscht wurden.  
  
## <a name="see-also"></a>Weitere Informationen  
 [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-transact-sql.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)  
  
