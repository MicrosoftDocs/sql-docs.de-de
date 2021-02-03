---
title: Erteilen von Berechtigungen für eine gespeicherte Prozedur | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie einer gespeicherten Prozedur mithilfe von SQL Server Management Studio oder Transact-SQL in SQL Server 2019 (15.x) Berechtigungen gewähren.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: conceptual
helpviewer_keywords:
- stored procedures [SQL Server], permissions
ms.assetid: a7d15816-a788-4099-ad91-dc4b26618299
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3b41f60b80dcb13d9e61b854666e3298b2b295cf
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250292"
---
# <a name="grant-permissions-on-a-stored-procedure"></a>Erteilen von Berechtigungen für eine gespeicherte Prozedur
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  In diesem Thema wird beschrieben, wie Sie Berechtigungen für eine gespeicherte Prozedur in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[tsql](../../includes/tsql-md.md)]erteilen. Berechtigungen können einem vorhandenen Benutzer, einer Datenbankrolle oder einer Anwendungsrolle in der Datenbank erteilt werden.  
  
 **In diesem Thema**  
  
-   **Vorbereitungen:**  
  
     [Einschränkungen](#Restrictions)  
  
     [Security](#Security)  
  
-   **Erteilen von Berechtigungen für eine gespeicherte Prozedur mit:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Einschränkungen  
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] kann nicht verwendet werden, um Berechtigungen für Systemprozeduren oder Systemfunktionen zu erteilen. Verwenden Sie stattdessen [GRANT-Objektberechtigungen](../../t-sql/statements/grant-object-permissions-transact-sql.md) .  
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Der Berechtigende (oder der mit der AS-Option angegebene Prinzipal) benötigt entweder die Berechtigung selbst mit GRANT OPTION oder eine höhere Berechtigung, die die erteilte Berechtigung impliziert. Erfordert die ALTER-Berechtigung im Schema, zu der die Prozedur gehört, oder die CONTROL-Berechtigung für die Prozedur. Weitere Informationen finden Sie unter [GRANT (Objektberechtigungen) &#40;Transact-SQL&#41;](../../t-sql/statements/grant-object-permissions-transact-sql.md)erteilen.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
  
#### <a name="to-grant-permissions-on-a-stored-procedure"></a>So erteilen Sie Berechtigungen für eine gespeicherte Prozedur  
  
1.  Stellen Sie im Objekt-Explorer eine Verbindung mit einer Instanz von [!INCLUDE[ssDE](../../includes/ssde-md.md)] her, und erweitern Sie dann diese Instanz.  
  
2.  Erweitern Sie **Datenbanken**, erweitern Sie die Datenbank, zu der die Prozedur gehört, und erweitern Sie dann **Programmierbarkeit**.  
  
3.  Erweitern Sie **Gespeicherte Prozeduren**, klicken Sie mit der rechten Maustaste auf die Prozedur, für die Sie Berechtigungen erteilen möchten, und klicken Sie anschließend auf **Eigenschaften**.  
  
4.  Wählen Sie in **Eigenschaften der gespeicherten Prozedur** die Seite **Berechtigungen** aus.  
  
5.  Klicken Sie auf **Suchen**, um einem Benutzer, einer Datenbankrolle oder einer Anwendungsrolle Berechtigungen zu erteilen.  
  
6.  Klicken Sie in **Benutzer oder Rollen auswählen** auf **Objekttypen** , um die gewünschten Benutzer und Rollen hinzuzufügen bzw. zu löschen.  
  
7.  Klicken Sie auf **Durchsuchen** , um die Liste der Benutzer oder Rollen anzuzeigen. Wählen Sie die Benutzer bzw. Rollen aus, denen Berechtigungen gewährt werden sollen.  
  
8.  Wählen Sie im Raster **Explizite Berechtigungen** die Berechtigungen aus, die Sie dem angegebenen Benutzer bzw. der angegebenen Rolle erteilen möchten. Eine Beschreibung der Berechtigungen finden Sie unter [Berechtigungen &#40;Datenbank-Engine&#41;](../../relational-databases/security/permissions-database-engine.md).  

 Durch Auswahl von **Erteilen** wird angegeben, dass der Empfänger die angegebene Berechtigung erhält. Durch Auswahl von **Mit Erteilung** wird angegeben, dass der Empfänger außerdem die angegebene Berechtigung anderen Prinzipalen erteilen kann.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
  
#### <a name="to-grant-permissions-on-a-stored-procedure"></a>So erteilen Sie Berechtigungen für eine gespeicherte Prozedur  
  
1.  Stellen Sie eine Verbindung mit dem [!INCLUDE[ssDE](../../includes/ssde-md.md)]her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**. Im folgenden Beispiel wird die `EXECUTE` -Berechtigung für die gespeicherte Prozedur `HumanResources.uspUpdateEmployeeHireInfo` einer Anwendungsrolle mit dem Namen `Recruiting11`erteilt.  
  
```sql  
USE AdventureWorks2012;   
GRANT EXECUTE ON OBJECT::HumanResources.uspUpdateEmployeeHireInfo  
    TO Recruiting11;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [sys.fn_builtin_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [GRANT (Objektberechtigungen) &#40;Transact-SQL&#41;](../../t-sql/statements/grant-object-permissions-transact-sql.md)   
 [Erstellen einer gespeicherten Prozedur](../../relational-databases/stored-procedures/create-a-stored-procedure.md)   
 [Ändern einer gespeicherten Prozedur](../../relational-databases/stored-procedures/modify-a-stored-procedure.md)   
 [Löschen einer gespeicherten Prozedur](../../relational-databases/stored-procedures/delete-a-stored-procedure.md)   
 [Umbenennen einer gespeicherten Prozedur](../../relational-databases/stored-procedures/rename-a-stored-procedure.md)  
  
  
