---
description: sp_fulltext_table (Transact-SQL)
title: sp_fulltext_table (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_fulltext_table_TSQL
- sp_fulltext_table
dev_langs:
- TSQL
helpviewer_keywords:
- sp_fulltext_table
ms.assetid: a765f311-07fc-4af3-b74c-e9a027fbecce
author: markingmyname
ms.author: maghan
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 94ce485bc773b66010708034c6c6cd2b87f85d3e
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97439406"
---
# <a name="sp_fulltext_table-transact-sql"></a>sp_fulltext_table (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-asdw-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-asdw-xxx-md.md)]

  Markiert eine Tabelle für die Volltextindizierung oder hebt die Markierung auf.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Verwenden Sie stattdessen [CREATE FULLTEXT INDEX](../../t-sql/statements/create-fulltext-index-transact-sql.md), [ALTER FULLTEXT INDEX](../../t-sql/statements/alter-fulltext-index-transact-sql.md)und [DROP FULLTEXT INDEX](../../t-sql/statements/drop-fulltext-index-transact-sql.md) .  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_fulltext_table   
   [ @tabname= ] 'qualified_table_name'           
   , [ @action= ] 'action'   
   [   
   , [ @ftcat= ] 'fulltext_catalog_name'           
   , [ @keyname= ] 'unique_index_name'   
   ]  
```  
  
## <a name="arguments"></a>Argumente  
`[ @tabname = ] 'qualified_table_name'` Ein ein-oder zweiteilige Tabellenname. Die Tabelle muss in der aktuellen Datenbank vorhanden sein. *qualified_table_name* weist den Datentyp **nvarchar(517)** auf und hat keinen Standardwert.  
  
`[ @action = ] 'action'` Die auszuführende Aktion. *action* ist vom Datentyp **nvarchar(50)** und hat keinen Standardwert. Die folgenden Werte sind möglich:  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Erstellen**|Erstellt die Metadaten für einen Volltextindex für die Tabelle, auf die durch *qualified_table_name* verwiesen wird. Darüber hinaus wird angegeben, dass der Volltextindex für diese Tabelle in *fulltext_catalog_name* gespeichert werden soll. Diese Aktion legt außerdem die Verwendung von *unique_index_name* als Volltextschlüsselspalte fest. Dieser eindeutige Index muss bereits vorhanden sein und muss für eine Spalte der Tabelle definiert sein.<br /><br /> Eine Volltextsuche für diese Tabelle kann erst durchgeführt werden, wenn der Volltextkatalog aufgefüllt ist.|  
|**Dropdown**|Löscht die Metadaten für den Volltextindex für *qualified_table_name*. Ist der Volltextindex aktiviert, wird er vor dem Löschen automatisch deaktiviert. Es ist nicht erforderlich, Spalten zu entfernen, bevor der Volltextindex gelöscht wird.|  
|**Aktivieren**|Aktiviert die Möglichkeit, Volltextindexdaten für *qualified_table_name* nach der Deaktivierung zu sammeln. Es muss mindestens eine Spalte im Volltextindex vorhanden sein, damit er aktiviert werden kann.<br /><br /> Ein Volltextindex wird automatisch (für das Auffüllen) aktiviert, sobald die erste Spalte für die Indizierung hinzugefügt wird. Wenn die letzte Spalte aus dem Index gelöscht wird, wird der Index inaktiv. Wenn die Änderungsprotokollierung aktiviert ist, wird durch Aktivieren eines inaktiven Index ein neuer Auffüllvorgang gestartet.<br /><br /> Beachten Sie, dass dadurch nicht der eigentliche Volltextindex aufgefüllt wird, sondern nur die Tabelle im Volltextkatalog des Dateisystems registriert wird, um Zeilen von *qualified_table_name* während der nächsten Volltextindexauffüllung abrufen zu können.|  
|**Deaktivieren**|Deaktiviert den Volltextindex für *qualified_table_name* , sodass die Volltextindexdaten für *qualified_table_name* nicht mehr gesammelt werden können. Die Volltextindexmetadaten sind weiterhin vorhanden, und die Tabelle kann erneut aktiviert werden.<br /><br /> Wenn die Änderungsnachverfolgung aktiviert ist, wird der Status des Indexes durch die Deaktivierung eines aktiven Index eingefroren: derzeit ausgeführte Auffüllungsprozesse werden beendet, und dem Index werden keine Änderungen mehr hinzugefügt.|  
|**start_change_tracking**|Startet das inkrementelle Auffüllen des Volltextindex. Wenn die Tabelle nicht über einen Timestamp verfügt, wird das vollständige Auffüllen des Volltextindexes gestartet. Startet die Änderungsprotokollierung für die Tabelle.<br /><br /> Von der Volltextänderungsnachverfolgung werden keine WRITETEXT- oder UPDATETEXT-Operationen in diesen Spalten protokolliert, die den Typ **image**, **text** oder **ntext** aufweisen.|  
|**stop_change_tracking**|Beendet die Änderungsprotokollierung für die Tabelle.|  
|**update_index**|Gibt den aktuellen Satz der protokollierten Änderungen an den Volltextindex weiter.|  
|**Start_background_updateindex**|Startet den Vorgang, der protokollierte Änderungen an den Volltextindex weitergibt, sobald sie auftreten.|  
|**Stop_background_updateindex**|Beendet den Vorgang, der protokollierte Änderungen an den Volltextindex weitergibt, sobald sie auftreten.|  
|**start_full**|Startet eine vollständige Auffüllung des Volltextindexes für die Tabelle.|  
|**start_incremental**|Startet eine inkrementelle Auffüllung des Volltextindexes für die Tabelle.|  
|**Beenden**|Beendet das vollständige oder inkrementelle Auffüllen.|  
  
`[ @ftcat = ] 'fulltext_catalog_name'` Ist ein gültiger, vorhandener voll Text Katalog Name für eine **Create** -Aktion. Bei allen anderen Aktionen muss dieser Parameter NULL sein. *fulltext_catalog_name* ist vom Datentyp **sysname**. Der Standardwert ist NULL.  
  
`[ @keyname = ] 'unique_index_name'` Ist ein gültiger, eindeutigen Index, der keine NULL-Werte zulässt, für *qualified_table_name* für eine **Create** -Aktion. Bei allen anderen Aktionen muss dieser Parameter NULL sein. *unique_index_name* ist vom Datentyp **sysname**. Der Standardwert ist NULL.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 „0“ (erfolgreich) oder „1“ (fehlerhaft)  
  
## <a name="result-sets"></a>Resultsets  
 Keine  
  
## <a name="remarks"></a>Bemerkungen  
 Nachdem ein Volltextindex für eine bestimmte Tabelle deaktiviert wurde, bleibt der vorhandene Volltextindex bis zum nächsten vollständigen Auffüllen vorhanden. Dieser Index wird jedoch nicht verwendet, da [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Abfragen für deaktivierte Tabellen blockiert.  
  
 Wenn die Tabelle erneut aktiviert, der Index jedoch nicht erneut aufgefüllt wird, ist der alte Index weiterhin für Abfragen für verbleibende, aber nicht für neue, volltextfähige Spalten verfügbar. Für Daten aus gelöschten Spalten werden Übereinstimmungen in Abfragen gefunden, für die eine umfassende Volltextspaltensuche angegeben ist.  
  
 Nach der Definition einer Tabelle für die Volltextindizierung kann das Ändern des Datentyps der Volltextspalte für den eindeutigen Schlüssel, durch Ändern des Datentyps dieser Spalte oder durch Ändern der Volltextspalte für den eindeutigen Schlüssel, ohne ein vollständiges erneutes Auffüllen bei einer nachfolgenden Abfrage zu einem Fehler führen, wobei die folgende Fehlermeldung zurückgegeben wird: "Fehler beim Konvertieren des Volltextsuchschlüssel-Werts *data_type* in den Datentyp *key_value*." Um dies zu verhindern, löschen Sie die Volltextdefinition für diese Tabelle mithilfe der **drop** -Aktion von **sp_fulltext_table** , und definieren Sie sie neu mithilfe von **sp_fulltext_table** und **sp_fulltext_column**.  
  
 Die Volltextschlüsselspalte muss mit einer Größe von maximal 900 Byte definiert sein. Aus Gründen der Leistung sollte die Schlüsselspalte so klein wie möglich sein.  
  
## <a name="permissions"></a>Berechtigungen  
 Nur Mitglieder der festen Serverrolle **sysadmin** , der festen Datenbankrollen **db_owner** und **db_ddladmin** oder ein Benutzer mit Verweisberechtigungen für den Volltextkatalog können **sp_fulltext_table** ausführen.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-enabling-a-table-for-full-text-indexing"></a>A. Aktivieren der Volltextindizierung für eine Tabelle  
 Im folgenden Beispiel werden Volltextindexmetadaten für die `Document` -Tabelle der `AdventureWorks` -Datenbank erstellt. `Cat_Desc` ist ein voll Text Katalog. `PK_Document_DocumentID` ist ein eindeutiger, einspaltiger Index für `Document`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_fulltext_table 'Production.Document', 'create', 'Cat_Desc', 'PK_Document_DocumentID';  
--Add some columns  
EXEC sp_fulltext_column 'Production.Document','DocumentSummary','add';  
-- Activate the full-text index  
EXEC sp_fulltext_table 'Production.Document','activate';  
GO  
```  
  
### <a name="b-activating-and-propagating-track-changes"></a>B. Aktivieren und Weitergeben der Änderungsprotokollierung  
 Im folgenden Beispiel wird die Änderungsprotokollierung aktiviert und die sofortige Weitergabe von auftretenden Änderungen an den Volltextindex gestartet.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_fulltext_table 'Production.Document', 'Start_change_tracking';  
EXEC sp_fulltext_table 'Production.Document', 'Start_background_updateindex';  
GO  
```  
  
### <a name="c-removing-a-full-text-index"></a>C. Entfernen eines Volltextindexes  
 Im folgenden Beispiel werden Volltextindex-Metadaten für die `Document` -Tabelle der `AdventureWorks` -Datenbank entfernt.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_fulltext_table 'Production.Document', 'drop';  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [INDEXPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/indexproperty-transact-sql.md)   
 [OBJECTPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/objectproperty-transact-sql.md)   
 [sp_help_fulltext_tables &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-help-fulltext-tables-transact-sql.md)   
 [sp_help_fulltext_tables_cursor &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-help-fulltext-tables-cursor-transact-sql.md)   
 [sp_helpindex &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-helpindex-transact-sql.md)   
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Gespeicherte Prozeduren für die voll Text Suche und die semantische Suche &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/full-text-search-and-semantic-search-stored-procedures-transact-sql.md)  
  
  
