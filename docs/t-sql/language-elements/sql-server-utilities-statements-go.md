---
description: Anweisungen für SQL Server-Hilfsprogramme - GO
title: 'Anweisungen für SQL Server-Hilfsprogramme: GO | Microsoft-Dokumentation'
ms.custom: ''
ms.date: 07/27/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- GO
- GO_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- batches [SQL Server], ending
- ending batches [SQL Server]
- GO command
ms.assetid: b2ca6791-3a07-4209-ba8e-2248a92dd738
author: cawrites
ms.author: chadam
ms.openlocfilehash: 527a67471c502eaaabe57751f61bdd81db8ea752
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200478"
---
# <a name="sql-server-utilities-statements---go"></a>Anweisungen für SQL Server-Hilfsprogramme - GO
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] stellt Befehle zur Verfügung, die keine [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen sind, aber von den Hilfsprogrammen **sqlcmd** und **osql** und dem [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]-Code-Editor erkannt werden. Die Befehle können zur Vereinfachung der Lesbarkeit und Ausführung von Batches und Skripts verwendet werden.  
  
  GO signalisiert das Ende eines Batches von [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen an die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Hilfsprogramme.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql  
GO [count]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *count*  
 Eine positive ganze Zahl. Der Batch, der GO vorausgeht, wird so oft wie angegeben ausgeführt.  
  
## <a name="remarks"></a>Bemerkungen  
 GO ist keine [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung, sondern ein Befehl, der von den Hilfsprogrammen **sqlcmd** und **osql** und dem [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]-Code-Editor erkannt wird.  
  
 Die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Hilfsprogramme interpretieren GO als ein Signal zum Senden des aktuellen Batches von [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen an eine Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Der aktuelle Anweisungsbatch besteht aus allen Anweisungen, die seit dem letzten GO eingegeben wurden. Wenn dies das erste GO ist, besteht er aus allen Anweisungen, die seit dem Beginn der Ad-hoc-Sitzung oder des Skripts eingegeben wurden.  
  
 Eine [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung kann nicht dieselbe Zeile wie ein GO-Befehl belegen. Allerdings kann die Zeile Kommentare enthalten.  
  
 Benutzer müssen die Regeln für Batches befolgen. So muss z. B. jede Ausführung einer gespeicherten Prozedur nach der ersten Anweisung in einem Batch das EXECUTE-Schlüsselwort einschließen. Der Bereich von lokalen (benutzerdefinierten) Variablen ist auf einen Batch beschränkt, auf die nicht nach einem GO-Befehl verwiesen werden kann.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @MyMsg VARCHAR(50)  
SELECT @MyMsg = 'Hello, World.'  
GO -- @MyMsg is not valid after this GO ends the batch.  
  
-- Yields an error because @MyMsg not declared in this batch.  
PRINT @MyMsg  
GO  
  
SELECT @@VERSION;  
-- Yields an error: Must be EXEC sp_who if not first statement in   
-- batch.  
sp_who  
GO  
```  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anwendungen können zum Ausführen eines Batches mehrere [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen an eine Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] senden. Die Anweisungen im Batch werden dann in einen einzelnen Ausführungsplan kompiliert. Programmierer, die Ad-hoc-Anweisungen in den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Hilfsprogrammen ausführen oder aus [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen Skripts zur Ausführung durch die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Hilfsprogramme erstellen, verwenden GO, um das Ende eines Batches zu signalisieren.  
  
 Auf den ODBC- oder OLE DB-APIs basierende Anwendungen erhalten einen Syntaxfehler, wenn sie versuchen, einen GO-Befehl auszuführen. Die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Hilfsprogramme senden nie einen GO-Befehl an den Server.  
  
 Verwenden Sie kein Semikolon als Abschlusszeichen für Anweisung nach GO.
 
```sql
-- Yields an error because ; is not permitted after GO  
SELECT @@VERSION;  
GO;  
```
  
## <a name="permissions"></a>Berechtigungen  
 GO ist ein Hilfsprogrammbefehl, für den keine Berechtigungen erforderlich sind. Der Befehl kann von jedem Benutzer ausgeführt werden.    
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel werden zwei Batches erstellt. Der erste Batch enthält nur eine `USE AdventureWorks2012`-Anweisung zum Festlegen des Datenbankkontexts. Die verbleibenden Anweisungen verwenden eine lokale Variable. Daher müssen alle Deklarationen von lokalen Variablen in einem einzelnen Batch gruppiert werden. Dies wird erreicht, indem ein `GO`-Befehl erst nach der letzten Anweisung, die auf die Variable verweist, ausgeführt wird.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @NmbrPeople INT  
SELECT @NmbrPeople = COUNT(*)  
FROM Person.Person;  
PRINT 'The number of people as of ' +  
      CAST(GETDATE() AS CHAR(20)) + ' is ' +  
      CAST(@NmbrPeople AS CHAR(10));  
GO  
```  
  
 Im folgenden Beispiel werden die Anweisungen im Batch zweimal ausgeführt.  
  
```sql  
SELECT DB_NAME();  
SELECT USER_NAME();  
GO 2  
```  
  
  
