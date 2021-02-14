---
description: sys.dm_exec_plan_attributes (Transact-SQL)
title: sys.dm_exec_plan_attributes (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 10/20/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_exec_plan_attributes_TSQL
- dm_exec_plan_attributes_TSQL
- dm_exec_plan_attributes
- sys.dm_exec_plan_attributes
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_plan_attributes dynamic management function
ms.assetid: dacf3ab3-f214-482e-aab5-0dab9f0a3648
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 0bbf64478b1ce114419df3e65c26ff453b3cfc5e
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100342972"
---
# <a name="sysdm_exec_plan_attributes-transact-sql"></a>sys.dm_exec_plan_attributes (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt eine Zeile pro Planattribut für den vom Planhandle angegebenen Plan zurück. Mit dieser Tabellenwertfunktion können Sie Details zu einem bestimmten Plan abrufen, z. B. die Cacheschlüsselwerte oder die Anzahl der aktuellen, gleichzeitigen Ausführungen des Plans.  
  
> [!NOTE]  
>  Einige der Informationen, die über diese Funktion zurückgegeben werden, werden der [sys.sysCache Objects](../../relational-databases/system-compatibility-views/sys-syscacheobjects-transact-sql.md) -abwärts Kompatibilitäts Ansicht zugeordnet.

## <a name="syntax"></a>Syntax  
```  
sys.dm_exec_plan_attributes ( plan_handle )  
```  
  
## <a name="arguments"></a>Argumente  
 *plan_handle*  
 Führt eine eindeutige Identifizierung eines Abfrageplans für einen ausgeführten Batch aus, dessen Plan sich im Plancache befindet. *plan_handle* ist **varbinary(64)** Das Plan Handle kann aus der dynamischen Verwaltungs Sicht [sys.dm_exec_cached_plans](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md) abgerufen werden.  
  
## <a name="table-returned"></a>Zurückgegebene Tabelle  
  
|Spaltenname|Datentyp|Beschreibung|  
|-----------------|---------------|-----------------|  
|Attribut|**varchar(128)**|Name des Attributs, das diesem Plan zugeordnet ist. In der folgenden Tabelle sind die möglichen Attribute, deren Datentypen und ihre Beschreibungen aufgeführt.|  
|value|**sql_variant**|Wert des Attributs, das diesem Plan zugeordnet ist.|  
|is_cache_key|**bit**|Gibt an, ob das Attribut als Teil des Cachesuchschlüssels für den Plan verwendet wird.|  

In der obigen Tabelle kann das- **Attribut** die folgenden Werte aufweisen:

|attribute|Datentyp|Beschreibung|  
|---------------|---------------|-----------------|  
|set_options|**int**|Gibt die Optionswerte an, mit denen der Plan kompiliert wurde.|  
|objectid|**int**|Einer der Hauptschlüssel zur Suche nach einem Objekt im Cache. Dies ist die Objekt-ID, die in [sys. Objects](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md) für Datenbankobjekte (Prozeduren, Sichten, Trigger usw.) gespeichert ist. Für Pläne vom Typ "Adhoc" oder "Prepared" ist dies ein interner Hash des Batchtexts.|  
|dbid|**int**|Die ID der Datenbank, welche die Entität enthält, auf die der Plan verweist.<br /><br /> Für Ad-hoc-Pläne oder vorbereitete Pläne ist dies die Datenbank-ID, von der der Batch ausgeführt wird.|  
|dbid_execute|**int**|Für Systemobjekte, die in der **Ressourcen** Datenbank gespeichert sind, die Datenbank-ID, aus der der zwischengespeicherte Plan ausgeführt wird. In allen anderen Fällen ist der Wert gleich 0.|  
|user_id|**int**|Mit dem Wert -2 wird angegeben, dass der abgesendete Batch nicht von der impliziten Namensauflösung abhängt und von verschiedenen Benutzern gemeinsam verwendet werden kann. Dies ist die bevorzugte Methode. Jeder andere Wert stellt den Benutzernamen des Benutzers dar, der die Abfrage in der Datenbank absendet.| 
|language_id|**smallint**|ID der Sprache der Verbindung, die das Cacheobjekt erstellt hat. Weitere Informationen finden Sie unter [sys.sysSprachen &#40;Transact-SQL-&#41;](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql.md).|  
|date_format|**smallint**|Datumsformat der Verbindung, die das Cacheobjekt erstellt hat. Weitere Informationen finden Sie unter [SET DATEFORMAT &#40;Transact-SQL&#41;](../../t-sql/statements/set-dateformat-transact-sql.md).|  
|date_first|**tinyint**|Erster Datumswert. Weitere Informationen finden Sie unter [SET DATEFIRST &#40;Transact-SQL&#41;](../../t-sql/statements/set-datefirst-transact-sql.md).|  
|status|**int**|Interne Statusbits, die Teil des Cachesuchschlüssels sind.|  
|required_cursor_options|**int**|Vom Benutzer angegebene Cursoroptionen, z. B. der Cursortyp.|  
|acceptable_cursor_options|**int**|Cursoroptionen, in die von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] eine implizite Konvertierung vorgenommen werden kann, um die Ausführung der Anweisung zu unterstützen. Beispielsweise kann der Benutzer einen dynamischen Cursor angeben, doch kann dieser Cursortyp vom Abfrageoptimierer in einen statischen Cursor konvertiert werden.|  
|inuse_exec_context|**int**|Die Anzahl der derzeit ausgeführten Batches, die den Abfrageplan verwenden.|  
|free_exec_context|**int**|Die Anzahl der zwischengespeicherten Ausführungskontexte für den Abfrageplan, die derzeit nicht verwendet werden.|  
|hits_exec_context|**int**|Die Anzahl der Vorgänge, bei denen der Ausführungskontext aus dem Plancache abgerufen und wiederverwendet wurde, wodurch der Aufwand zum erneuten Kompilieren der SQL-Anweisung eingespart wird. Der Wert ist ein aggregierter Wert für alle bisherigen Batchausführungen.|  
|misses_exec_context|**int**|Die Anzahl der Vorgänge, bei denen ein Ausführungskontext im Plancache nicht gefunden wurde, was zum Erstellen eines neuen Ausführungskontexts für die Batchausführung führt.|  
|removed_exec_context|**int**|Die Anzahl der Ausführungskontexte, die aufgrund ungenügenden Arbeitsspeichers für den zwischengespeicherten Plan entfernt wurden.|  
|inuse_cursors|**int**|Die Anzahl der derzeit ausgeführten Batches, die einen oder mehrere Cursor enthalten, die den zwischengespeicherten Plan verwenden.|  
|free_cursors|**int**|Die Anzahl der im Leerlauf befindlichen oder freien Cursor für den zwischengespeicherten Plan.|  
|hits_cursors|**int**|Die Anzahl der Vorgänge, bei denen ein inaktiver Cursor aus dem zwischengespeicherten Plan abgerufen und wiederverwendet wurde. Der Wert ist ein aggregierter Wert für alle bisherigen Batchausführungen.|  
|misses_cursors|**int**|Die Anzahl der Vorgänge, bei denen im Cache kein inaktiver Cursor gefunden werden konnte.|  
|removed_cursors|**int**|Die Anzahl der Cursor, die aufgrund ungenügenden Arbeitsspeichers für den zwischengespeicherten Plan entfernt wurden.|  
|sql_handle|**varbinary**(64)|Das SQL-Handle für den Batch.|  
|merge_action_type|**smallint**|Der Typ des Triggerausführungsplans, der als Ergebnis einer MERGE-Anweisung verwendet wird.<br /><br /> 0 gibt einen Nicht-Triggerplan an, einen Triggerplan, der nicht als Ergebnis einer MERGE-Anweisung ausgeführt wird, oder einen Triggerplan, der als Ergebnis einer MERGE-Anweisung ausgeführt wird, die nur eine DELETE-Aktion angibt.<br /><br /> 1 gibt einen INSERT-Triggerplan an, der als Ergebnis einer MERGE-Anweisung ausgeführt wird.<br /><br /> 2 gibt einen UPDATE-Triggerplan an, der als Ergebnis einer MERGE-Anweisung ausgeführt wird.<br /><br /> 3 gibt einen DELETE-Triggerplan an, der als Ergebnis einer MERGE-Anweisung ausgeführt wird, die eine entsprechende INSERT- oder UPDATE-Aktion enthält.<br /><br /> Bei geschachtelten Triggern, die durch kaskadierende Aktionen ausgeführt werden, ist dieser Wert die Aktion der MERGE-Anweisung, durch die das Kaskadieren verursacht wurde.|  
  
## <a name="permissions"></a>Berechtigungen  

In [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ist die- `VIEW SERVER STATE` Berechtigung erforderlich.   
Bei den Dienst Zielen "Basic", "S0" und "S1" in SQL-Datenbank ist für Datenbanken in Pools für elastische Datenbanken das [Server Administrator](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) Konto oder das [Azure Active Directory Administrator](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) Konto erforderlich. Für alle anderen SQL-Datenbank-Dienst Ziele `VIEW DATABASE STATE` ist die Berechtigung in der Datenbank erforderlich.   

## <a name="remarks"></a>Bemerkungen  
  
## <a name="set-options"></a>SET-Optionen  
 Kopien desselben kompilierten Plans können sich nur durch den Wert in der Spalte **set_options** unterscheiden. Dies weist darauf hin, dass verschiedene Verbindungen für die gleiche Abfrage unterschiedliche Sätze von SET-Optionen verwenden. Die Verwendung unterschiedlicher Sätze von Optionen ist meist unerwünscht, da dies zusätzliche Kompilierungen, einen geringeren Anteil von Wiederverwendungen von Plänen sowie, da im Cache mehrere Pläne vorhanden sind, eine Vergrößerung des Plancaches verursachen kann.  
  
### <a name="evaluating-set-options"></a>Auswerten von SET-Optionen  
 Um den in **set_options** zurückgegebenen Wert in die Optionen zu übersetzen, mit denen der Plan kompiliert wurde, subtrahieren Sie die Werte vom **set_options** Wert, beginnend mit dem größtmöglichen Wert, bis Sie 0 erreichen. Jeder subtrahierte Wert entspricht einer Option, die im Abfrageplan verwendet wurde. Wenn der Wert in **set_options** beispielsweise 251 ist, werden die Optionen, mit denen der Plan kompiliert wurde, ANSI_NULL_DFLT_ON (128), QUOTED_IDENTIFIER (64), ANSI_NULLS (32), ANSI_WARNINGS (16), CONCAT_NULL_YIELDS_NULL (8), paralleler Plan (2) und ANSI_PADDING (1).  
  
|Option|Wert|  
|------------|-----------|  
|ANSI_PADDING|1|  
|Parallelplan<br /><br /> Gibt an, dass die Optionen für den Plan Parallelismus geändert wurden.|2|  
|FORCEPLAN|4|  
|CONCAT_NULL_YIELDS_NULL|8|  
|ANSI_WARNINGS|16|  
|ANSI_NULLS|32|  
|QUOTED_IDENTIFIER|64|  
|ANSI_NULL_DFLT_ON|128|  
|ANSI_NULL_DFLT_OFF|256|  
|NoBrowseTable<br /><br /> Gibt an, dass im Plan keine Arbeitstabelle verwendet wird, um einen FOR BROWSE-Vorgang zu implementieren.|512|  
|TriggerOneRow<br /><br /> Gibt an, dass der Plan Optimierungen einzelner Zeilen für Deltatabellen von AFTER-Triggern umfasst.|1024|  
|ResyncQuery<br /><br /> Gibt an, dass die Abfrage von internen gespeicherten Systemprozeduren übermittelt wurde.|2048|  
|ARITH_ABORT|4096|  
|NUMERIC_ROUNDABORT|8192|  
|DATEFIRST|16384|  
|DATEFORMAT|32768|  
|LanguageID|65536|  
|UPON<br /><br /> Gibt an, dass die Datenbankoption PARAMETERIZATION beim Kompilieren des Plans auf FORCED festgelegt wurde.|131072|  
|ROWCOUNT|**Gilt für:** [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] An [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]<br /><br /> 262144|  
  
## <a name="cursors"></a>Cursor  
 Inaktive Cursor werden in einem kompilierten Plan zwischengespeichert, sodass der zum Speichern des Cursors verwendete Arbeitsspeicher von gleichzeitigen Benutzern des Cursors wiederverwendet werden kann. Angenommen, dass ein Cursor von einem Batch deklariert und verwendet wird, ohne dass seine Zuordnung aufgehoben wird. Wenn zwei Benutzer denselben Batch ausführen, sind zwei aktive Cursor vorhanden. Sobald die Zuordnung der Cursor aufgehoben ist (möglicherweise in unterschiedlichen Batches), wird der Arbeitsspeicher zum Speichern des Cursors zwischengespeichert und nicht freigegeben. Der Liste der inaktiven Cursor wird im kompilierten Plan beibehalten. Bei der nächsten Ausführung des Batches durch einen Benutzer wird der zwischengespeicherte Arbeitsspeicher für den Cursor wiederverwendet und als aktiver Cursor ordnungsgemäß initialisiert.  
  
### <a name="evaluating-cursor-options"></a>Auswerten von Cursoroptionen  
 Subtrahieren Sie die Werte vom Spaltenwert, beginnend mit dem größtmöglichen Wert, bis Sie 0 erreichen, um den in **required_cursor_options** zurückgegebenen Wert und **acceptable_cursor_options** auf die Optionen zu übersetzen, mit denen der Plan kompiliert wurde. Jeder subtrahierte Wert entspricht einer Cursoroption, die im Abfrageplan verwendet wurde.  
  
|Option|Wert|  
|------------|-----------|  
|Keine|0|  
|INSENSITIVE|1|  
|SCROLL|2|  
|READ ONLY|4|  
|FOR UPDATE|8|  
|LOCAL|16|  
|GLOBAL|32|  
|FORWARD_ONLY|64|  
|KEYSET|128|  
|DYNAMIC|256|  
|SCROLL_LOCKS|512|  
|OPTIMISTIC|1024|  
|STATIC|2048|  
|FAST_FORWARD|4096|  
|IN PLACE|8192|  
|FOR *select_statement*|16384|  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-returning-the-attributes-for-a-specific-plan"></a>A. Zurückgeben der Attribute für einen bestimmten Plan  
 Im folgenden Beispiel werden alle Planattribute für einen angegebenen Plan zurückgegeben. Die dynamische Verwaltungssicht `sys.dm_exec_cached_plans` wird zuerst abgefragt, um das Planhandle für den angegebenen Plan abzurufen. In der zweiten Abfrage ersetzen Sie `<plan_handle>` durch einen Planhandlewert aus der ersten Abfrage.  
  
```sql  
SELECT plan_handle, refcounts, usecounts, size_in_bytes, cacheobjtype, objtype   
FROM sys.dm_exec_cached_plans;  
GO  
SELECT attribute, value, is_cache_key  
FROM sys.dm_exec_plan_attributes(<plan_handle>);  
GO  
```  
  
### <a name="b-returning-the-set-options-for-compiled-plans-and-the-sql-handle-for-cached-plans"></a>B. Zurückgeben der SET-Optionen für kompilierte Pläne und des SQL-Handles für zwischengespeicherte Pläne  
 Im folgenden Beispiel wird ein Wert zurückgegeben, der die Optionen darstellt, mit denen die einzelnen Pläne kompiliert wurden. Außerdem wird das SQL-Handle für alle zwischengespeicherten Pläne zurückgegeben.  
  
```sql  
SELECT plan_handle, pvt.set_options, pvt.sql_handle  
FROM (  
    SELECT plan_handle, epa.attribute, epa.value   
    FROM sys.dm_exec_cached_plans   
        OUTER APPLY sys.dm_exec_plan_attributes(plan_handle) AS epa  
    WHERE cacheobjtype = 'Compiled Plan') AS ecpa   
PIVOT (MAX(ecpa.value) FOR ecpa.attribute IN ("set_options", "sql_handle")) AS pvt;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Dynamische Verwaltungs Sichten und-Funktionen im Zusammenhang mit der Ausführung &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
 [sys.dm_exec_cached_plans &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)   
 [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)  
  
  

