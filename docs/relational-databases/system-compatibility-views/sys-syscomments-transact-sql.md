---
description: sys.syscomments (Transact-SQL)
title: sys.sysKommentare (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.syscomments_TSQL
- syscomments
- syscomments_TSQL
- sys.syscomments
dev_langs:
- TSQL
helpviewer_keywords:
- sys.syscomments compatibility view
- syscomments system table
ms.assetid: 767dd410-6bc9-4c4a-ab0f-6d2cf6163426
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 1d72b83ad031b426f403046ea6110da742c0b95e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171807"
---
# <a name="syssyscomments-transact-sql"></a>sys.syscomments (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Die Tabelle enthält Einträge für alle Sichten, Regeln, Standards, Trigger, CHECK-Einschränkungen, DEFAULT-Einschränkungen und gespeicherten Prozeduren innerhalb der Datenbank. Die **Text** -Spalte enthält die ursprünglichen SQL-Definitions Anweisungen.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Stattdessen wird die Verwendung von sys.sql_modules empfohlen. Weitere Informationen finden Sie unter [sys.sql_modules &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md).  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**id**|**int**|ID des Objekts, auf das sich der Text bezieht.|  
|**Zahl**|**smallint**|Nummer innerhalb der Prozedurgruppierung, wenn eine Gruppierung vorliegt.<br /><br /> 0 = Einträge sind keine Prozeduren.|  
|**ColId**|**smallint**|Zeilensequenznummer für Objektdefinitionen, die 4.000 Zeichen überschreiten.|  
|**status**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**ctext**|**varbinary(8000)**|Die Rohbytes der SQL-Definitionsanweisung.|  
|**TextType**|**smallint**|0 = Vom Benutzer anzugebender Kommentar<br /><br /> 1 = Vom System anzugebender Kommentar<br /><br /> 4 = Verschlüsselter Kommentar|  
|**language**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**.**|**bit**|Gibt an, ob die Prozedurdefinition verborgen ist.<br /><br /> 0 = Nicht verborgen<br /><br /> 1 = Verborgen<br /><br /> Wichtig um Definitionen gespeicherter Prozeduren zu verbergen, verwenden Sie CREATE PROCEDURE mit dem Schlüsselwort ENCRYPTION. **\* \* \* \***|  
|**komprimierte**|**bit**|Es wird immer 0 zurückgegeben. Zeigt an, ob die Prozedur komprimiert ist.|  
|**text**|**nvarchar(4000)**|Tatsächlicher Text der SQL-Definitionsanweisung.<br /><br /> Die Semantik des decodierten Ausdrucks entspricht dem ursprünglichen Text. Es gibt jedoch keine syntaktische Garantie. Leerzeichen werden beispielsweise aus dem decodierten Ausdruck entfernt.<br /><br /> Mit dieser [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] kompatiblen Ansicht werden Informationen aus aktuellen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Strukturen abgerufen, und es können mehr Zeichen als die **nvarchar (4000)** -Definition zurückgegeben werden. **sp_help** gibt **nvarchar (4000)** als Datentyp der Text Spalte zurück. Bei der Arbeit mit **syscomments** sollten Sie **nvarchar (max)** verwenden. Verwenden Sie für neue Entwicklungsaufgaben nicht **syscomments**.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Zuordnung von Systemtabellen zu System Sichten &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Kompatibilitätssichten &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
