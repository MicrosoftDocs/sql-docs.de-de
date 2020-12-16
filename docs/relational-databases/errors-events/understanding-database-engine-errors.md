---
title: Grundlegendes zu Datenbank-Engine-Fehlern | Microsoft-Dokumentation
description: Hier erfahren Sie mehr über die Attribute von Fehlern, die von der SQL Server-Datenbank-Engine ausgelöst werden, und wie Sie auf alle Systemfehlermeldungen und benutzerdefinierten Fehlermeldungen von „sys.messages“ zugreifen können.
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- errors [SQL Server], about errors
- errors [SQL Server], Database Engine
- errors [SQL Server]
- Database Engine [SQL Server], errors
ms.assetid: ddaca9d3-956f-46a5-8cd3-a7a15ec75878
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 479250170d4630a6f2b7716750e85e5af23e84d0
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465661"
---
# <a name="understanding-database-engine-errors"></a>Grundlegendes zu Datenbank-Engine-Fehlern
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Von [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] ausgelöste Fehler weisen die in der folgenden Tabelle beschriebenen Attribute auf.  
  
|attribute|BESCHREIBUNG|  
|---------------|-----------------|  
|Fehlernummer|Jede Fehlermeldung besitzt eine eindeutige Fehlernummer.|  
|Fehlermeldungs-Zeichenfolge|Die Fehlermeldung enthält Diagnoseinformationen über die Ursache des Fehlers. Zahlreiche Fehlermeldungen haben Ersetzungsvariablen, in die Informationen, wie z. B. der Name des Objekts, das den Fehler generiert, eingefügt werden.|  
|severity|Der Schweregrad zeigt an, wie schwerwiegend der Fehler ist. Fehler mit einem niedrigen Schweregrad, wie z. B. 1 oder 2, sind Informationsmeldungen oder Warnungen auf niedriger Ebene. Fehler mit einem hohen Schweregrad deuten auf Probleme hin, die so bald wie möglich behoben werden sollten. Weitere Informationen zu den Schweregraden finden Sie unter [Schweregrade von Datenbank-Engine-Fehlern](../../relational-databases/errors-events/database-engine-error-severities.md).|  
|State|Einige Fehlermeldungen können für mehrere Elemente im Code für [!INCLUDE[ssDE](../../includes/ssde-md.md)]ausgelöst werden. So kann z. B. der Fehler 1105 aufgrund verschiedener Bedingungen ausgelöst werden. Jeder Bedingung, die einen Fehler auslöst, wird ein eindeutiger Statuscode zugewiesen.<br /><br /> Beim Anzeigen von Datenbanken von bekannten Problemen, wie z. B. der [!INCLUDE[msCoName](../../includes/msconame-md.md)] Knowledge Base, können Sie mithilfe der Statusnummer bestimmen, ob das aufgezeichnete Problem mit dem aufgetretenen Fehler übereinstimmt. Wenn z. B. ein Knowledge Base-Artikel den Fehler 1105 mit dem Status 2 erläutert und die von Ihnen empfangene Fehlermeldung 1105 den Status 3 aufwies, ist der Fehler möglicherweise auf eine andere als die im Artikel gemeldete Ursache zurückzuführen.<br /><br /> Der [!INCLUDE[msCoName](../../includes/msconame-md.md)] Software Service kann auch den Statuscode von einem Fehler verwenden, um den Ort im Quellcode zu finden, wo der Fehler entstanden ist. Diese Informationen stellen möglicherweise zusätzliche Ideen zur Problemdiagnose bereit.|  
|Name der Prozedur|Ist der Name der gespeicherten Prozedur oder des Triggers, in der oder in dem der Fehler aufgetreten ist.|  
|Zeilennummer|Zeigt an, welche Anweisung in einem Batch, einer gespeicherten Prozedur, einem Trigger oder einer Funktion den Fehler generiert hat.|  
  
 Alle System- und benutzerdefinierten Fehlermeldungen in einer [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Instanz sind in der Katalogsicht **sys.messages** enthalten. Die RAISERROR-Anweisung kann zur Rückgabe von benutzerdefinierten Fehlern an eine Anwendung verwendet werden.  
  
 Alle Datenbank-APIs, z. B. das [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] **SQLClient**-Namespace, ActiveX Data Objects (ADO), OLE DB und Open Database Connectivity (ODBC), melden die grundlegenden Fehlerattribute. Diese Information schließt die Fehlernummer und die Meldungszeichenfolge ein. Nicht alle APIs berichten jedoch alle anderen Fehlerattribute.  
  
 Informationen zu einem Fehler, der innerhalb des Bereichs des TRY-Blocks eines TRY...CATCH-Konstrukts auftritt, können im [!INCLUDE[tsql](../../includes/tsql-md.md)]-Code mithilfe von Funktionen wie ERROR_LINE, ERROR_MESSAGE, ERROR_NUMBER, ERROR_PROCEDURE, ERROR_SEVERITY und ERROR_STATE innerhalb des Bereichs des zugehörigen CATCH-Blocks abgerufen werden. Weitere Informationen finden Sie unter [TRY...CATCH &#40;Transact-SQL&#41;](../../t-sql/language-elements/try-catch-transact-sql.md).  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird die `sys.messages` -Katalogsicht abgefragt, um eine Liste aller system- und benutzerdefinierten Fehlermeldungen im [!INCLUDE[ssDE](../../includes/ssde-md.md)] zurückzugeben, die über englischen Text verfügen (`1033`).  
  
```  
SELECT  
    message_id,  
    language_id,  
    severity,  
    is_event_logged,  
    text  
  FROM sys.messages  
  WHERE language_id = 1033;  
```  
  
 Weitere Informationen finden Sie unter [sys.messages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [sys.messages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md)   
 [RAISERROR &#40;Transact-SQL&#41;](../../t-sql/language-elements/raiserror-transact-sql.md)   
 [@@ERROR &#40;Transact-SQL&#41;](../../t-sql/functions/error-transact-sql.md)   
 [TRY...CATCH &#40;Transact-SQL&#41;](../../t-sql/language-elements/try-catch-transact-sql.md)   
 [ERROR_LINE &#40;Transact-SQL&#41;](../../t-sql/functions/error-line-transact-sql.md)   
 [ERROR_MESSAGE &#40;Transact-SQL&#41;](../../t-sql/functions/error-message-transact-sql.md)   
 [ERROR_NUMBER &#40;Transact-SQL&#41;](../../t-sql/functions/error-number-transact-sql.md)   
 [ERROR_PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/functions/error-procedure-transact-sql.md)   
 [ERROR_SEVERITY &#40;Transact-SQL&#41;](../../t-sql/functions/error-severity-transact-sql.md)   
 [ERROR_STATE &#40;Transact-SQL&#41;](../../t-sql/functions/error-state-transact-sql.md)  
  
  
