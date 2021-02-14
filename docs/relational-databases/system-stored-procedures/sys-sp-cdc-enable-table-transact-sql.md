---
description: sys.sp_cdc_enable_table (Transact-SQL)
title: sys.sp_cdc_enable_table (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.sp_cdc_enable_table_TSQL
- sp_cdc_enable_table_TSQL
- sys.sp_cdc_enable_table
- sp_cdc_enable_table
dev_langs:
- TSQL
helpviewer_keywords:
- change data capture [SQL Server], enabling tables
- sys.sp_cdc_enable_table
- sp_cdc_enable_table
ms.assetid: 26150c09-2dca-46ad-bb01-3cb3165bcc5d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f499a26e4884518e002b5d33d9f9cb25b7c1457e
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100339392"
---
# <a name="syssp_cdc_enable_table-transact-sql"></a>sys.sp_cdc_enable_table (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Aktiviert Change Data Capture für die angegebene Quelltabelle in der aktuellen Datenbank. Wenn eine Tabelle für Change Data Capture aktiviert ist, wird für jeden Vorgang der Datenbearbeitungssprache (Data Manipulation Language, DML), der auf die Tabelle angewendet wird, ein Datensatz in das Transaktionsprotokoll geschrieben. Der Change Data Capture-Prozess ruft diese Informationen aus dem Protokoll ab und schreibt sie in die Änderungstabellen, auf die über eine Reihe von Funktionen zugegriffen wird.  
  
 Change Data Capture ist nicht in jeder Edition von [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Eine Liste der Funktionen, die von den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Editionen unterstützt werden, finden Sie unter [Von den SQL Server 2016-Editionen unterstützte Funktionen](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sys.sp_cdc_enable_table   
  [ @source_schema = ] 'source_schema',   
  [ @source_name = ] 'source_name' ,  [,[ @capture_instance = ] 'capture_instance' ]  
  [,[ @supports_net_changes = ] supports_net_changes ]  
  , [ @role_name = ] 'role_name'  
  [,[ @index_name = ] 'index_name' ]  
  [,[ @captured_column_list = ] 'captured_column_list' ]  
  [,[ @filegroup_name = ] 'filegroup_name' ]  
  [,[ @allow_partition_switch = ] 'allow_partition_switch' ]  
  [;]  
```  
  
## <a name="arguments"></a>Argumente  
`[ @source_schema = ] 'source_schema'` Der Name des Schemas, zu dem die Quell Tabelle gehört. *source_schema* ist vom **Datentyp vom Datentyp sysname** und hat keinen Standardwert und darf nicht NULL sein.  
  
`[ @source_name = ] 'source_name'` Der Name der Quell Tabelle, für die Change Data Capture aktiviert werden soll. *source_name* ist vom **Datentyp vom Datentyp sysname** und hat keinen Standardwert und darf nicht NULL sein.  
  
 *source_name* muss in der aktuellen Datenbank vorhanden sein. Tabellen im **CDC** -Schema können nicht für Change Data Capture aktiviert werden.  
  
`[ @role_name = ] 'role_name'` Der Name der Daten Bank Rolle, die verwendet wird, um den Zugriff auf Änderungs Daten zu ändern. *role_name* ist vom **Datentyp vom Datentyp sysname** und muss angegeben werden. Wenn der Parameter explizit auf NULL festgelegt ist, wird keine Gatingrolle verwendet, um den Zugriff auf Änderungsdaten einzuschränken.  
  
 Wenn die Rolle derzeit vorhanden ist, wird sie verwendet. Ist die Rolle nicht vorhanden, wird versucht, eine Datenbankrolle mit dem angegebenen Namen zu erstellen. Vor dem Versuch zur Erstellung der Rolle wird der Rollenname um die in der Zeichenfolge rechts befindlichen Leerstellen gekürzt. Wenn der Aufrufer nicht berechtigt ist, Rollen in der Datenbank zu erstellen, tritt bei der gespeicherten Prozedur ein Fehler auf.  
  
`[ @capture_instance = ] 'capture_instance'` Der Name der Aufzeichnungs Instanz, die verwendet wird, um instanzspezifische Change Data Capture Objekte zu benennen. *capture_instance* ist vom **Datentyp vom Datentyp sysname** und darf nicht NULL sein.  
  
 Wenn nicht angegeben, wird der Name vom Quell Schema Namen und dem Quell Tabellennamen im Format *schemaname_sourcename* abgeleitet. *capture_instance* darf nicht länger als 100 Zeichen sein und muss innerhalb der Datenbank eindeutig sein. Unabhängig davon, ob Sie angegeben oder abgeleitet sind, wird *capture_instance* auf ein Leerzeichen rechts von der Zeichenfolge gekürzt.  
  
 Eine Quelltabelle kann maximal zwei Aufzeichnungsinstanzen aufweisen. Weitere Informationen finden Sie unter [sys.sp_cdc_help_change_data_capture &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-change-data-capture-transact-sql.md).  
  
`[ @supports_net_changes = ] supports_net_changes` Gibt an, ob die Unterstützung für das Abfragen von Netto Änderungen für diese Aufzeichnungs Instanz aktiviert werden soll. *supports_net_changes* ist vom Typ **Bit** und hat den Standardwert 1, wenn die Tabelle über einen Primärschlüssel verfügt oder die Tabelle über einen eindeutigen Index verfügt, der mithilfe des- @index_name Parameters identifiziert wurde. Andernfalls hat der Parameter den Standardwert 0.  
  
 Bei 0 werden nur die Unterstützungsfunktionen zum Abfragen aller Änderungen generiert.  
  
 Bei 1 werden auch die Funktionen generiert, die zum Abfragen der Nettoänderungen erforderlich sind.  
  
 Wenn *supports_net_changes* auf 1 festgelegt ist, muss *index_name* angegeben werden, oder die Quell Tabelle muss über einen definierten Primärschlüssel verfügen.  
  
`[ @index_name = ] 'index_name_'` Der Name eines eindeutigen Indexes, der zum eindeutigen Identifizieren von Zeilen in der Quell Tabelle verwendet werden soll. *index_name* ist vom **Datentyp vom Datentyp sysname** und kann NULL sein. Wenn angegeben, muss *index_name* ein gültiger eindeutiger Index für die Quell Tabelle sein. Wenn *index_name* angegeben wird, haben die identifizierten Index Spalten Vorrang vor allen definierten Primärschlüssel Spalten als eindeutige Zeilen Bezeichner für die Tabelle.  
  
`[ @captured_column_list = ] 'captured_column_list'` Identifiziert die Quell Tabellenspalten, die in die Änderungs Tabelle eingeschlossen werden sollen. *captured_column_list* ist vom Datentyp **nvarchar (max)** und kann NULL sein. Wenn der Wert NULL ist, werden alle Spalten in der Änderungstabelle eingeschlossen.  
  
 Spaltennamen müssen gültige Spalten in der Quelltabelle sein. In einem Primärschlüssel Index definierte Spalten oder Spalten, auf die von *index_name* verwiesen wird, müssen eingeschlossen werden.  
  
 *captured_column_list* ist eine durch Trennzeichen getrennte Liste mit Spaltennamen. Einzelne Spaltennamen innerhalb der Liste können optional mit doppelten Anführungszeichen ("") oder eckigen Klammern ([]) angegeben werden. Wenn ein Spaltenname ein eingebettetes Komma enthält, muss er in Anführungszeichen eingeschlossen sein.  
  
 *captured_column_list* dürfen die folgenden reservierten Spaltennamen nicht enthalten: **__ $ start_lsn**, **__ $ end_lsn**, **__ $**, **__ $ Operation** und **__ $ update_mask**.  
  
`[ @filegroup_name = ] 'filegroup_name'` Die Datei Gruppe, die für die Änderungs Tabelle verwendet werden soll, die für die Aufzeichnungs Instanz erstellt wurde. *filegroup_name* ist vom **Datentyp vom Datentyp sysname** und kann NULL sein. Wenn angegeben, müssen *filegroup_name* für die aktuelle Datenbank definiert werden. Wenn der Wert NULL ist, wird die Standarddateigruppe verwendet.  
  
 Es wird empfohlen, eine separate Dateigruppe für Change Data Capture-Änderungstabellen zu erstellen.  
  
`[ @allow_partition_switch = ] 'allow_partition_switch'` Gibt an, ob der Switch Partition-Befehl von ALTER TABLE für eine Tabelle ausgeführt werden kann, die für Change Data Capture aktiviert ist. *allow_partition_switch* ist vom Typ **Bit** und hat den Standardwert 1.  
  
 Bei nicht partitionierten Tabellen lautet die Schaltereinstellung immer 1, und die tatsächliche Einstellung wird ignoriert. Wenn der Schalter für eine nicht partitionierte Tabelle explizit auf 0 festgelegt ist, wird Warnung 22857 ausgegeben. Dies zeigt an, dass die Schaltereinstellung ignoriert wurde. Wenn der Schalter für eine partitionierte Tabelle explizit auf 0 festgelegt ist, wird die Warnung 22356 ausgegeben. Diese zeigt an, dass SWITCH PARTITION-Vorgänge für die Quelltabelle nicht zulässig sind. Wenn die Schaltereinstellung entweder explizit auf 1 festgelegt und der Standardwert 1 zugelassen und die aktivierte Tabelle partitioniert ist, wird die Warnung 22855 ausgegeben. Diese zeigt an, dass Partitionsschalter nicht blockiert werden. Falls Partitionsschalter auftreten, werden die aus dem Schalter resultierenden Änderungen von Change Data Capture nicht nachverfolgt. Dies führt bei der Nutzung der Änderungsdaten zu Inkonsistenzen.  
  
> [!IMPORTANT]  
>  SWITCH PARTITION ist ein Metadatenvorgang, verursacht jedoch Datenänderungen. Die mit diesem Vorgang verbundenen Datenänderungen werden nicht in den Change Data Capture-Änderungstabellen aufgezeichnet. Beispiel: An einer Tabelle mit drei Partitionen werden Änderungen vorgenommen. Mit dem Aufzeichnungsprozess werden Einfüge-, Update- und Löschvorgänge verfolgt, die Benutzer in der Tabelle ausführen. Wenn jedoch eine Partition in eine andere Tabelle ausgelagert wird (z. B. zur Durchführung einer Massenlöschung), werden die verschobenen Zeilen in der Änderungstabelle nicht als gelöschte Zeilen aufgezeichnet. Wenn der Tabelle eine neue Partition mit vorab ausgefüllten Zeilen hinzugefügt wird, werden diese Zeilen auch nicht in der Änderungstabelle erfasst. Dies kann zu inkonsistenten Daten führen, wenn die Änderungen von einer Anwendung belegt und auf ein Ziel angewendet werden.  
 
> [!NOTE] 
> Bevor Sie einen Split-oder Merge-Vorgang für eine CDC-fähige Tabelle ausführen, stellen Sie sicher, dass für die Partition keine ausstehenden replizierten Befehle vorhanden sind und dass während der Split-oder Merge-Vorgänge keine DML-Vorgänge ausgeführt werden. Nicht verarbeitete Transaktionen oder DML-Vorgänge können zu Verarbeitungsfehlern `Error 608: No catalog entry found for partitionID` beim Protokoll Lese-Agent oder CDC-Aufzeichnungs Auftrag führen. Möglicherweise muss CDC für die Tabelle deaktiviert werden, damit der Split-oder Merge-Vorgang fortgesetzt werden kann. 



## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="result-sets"></a>Resultsets  
 Keine  
  
## <a name="remarks"></a>Bemerkungen  
 Bevor Sie eine Tabelle für Change Data Capture aktivieren können, muss die Datenbank aktiviert sein. Um zu ermitteln, ob die Datenbank für Change Data Capture aktiviert ist, Fragen Sie die **is_cdc_enabled** Spalte in der [sys. Database](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) -Katalog Sicht ab. Verwenden Sie die gespeicherte Prozedur [sys.sp_cdc_enable_db](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-db-transact-sql.md) , um die Datenbank zu aktivieren.  
  
 Wenn Change Data Capture für eine Tabelle aktiviert wird, werden eine Änderungstabelle und eine oder zwei Abfragefunktionen generiert. Die Änderungstabelle dient als Repository für die Änderungen der Quelltabelle, die durch den Aufzeichnungsprozess aus dem Transaktionsprotokoll extrahiert wurden. Die Abfragefunktionen werden verwendet, um Daten aus der Änderungstabelle zu extrahieren. Die Namen dieser Funktionen werden auf folgende Weise vom *capture_instance* -Parameter abgeleitet:  
  
-   Funktion für alle Änderungen: **CDC.fn_cdc_get_all_changes_<capture_instance>**  
  
-   NET Changes-Funktion: **CDC.fn_cdc_get_net_changes_<capture_instance>**  
  
 **sys.sp_cdc_enable_table** erstellt außerdem die Aufzeichnungs-und Cleanupaufträge für die Datenbank, wenn die Quell Tabelle die erste Tabelle in der Datenbank ist, die für Change Data Capture aktiviert werden soll, und für die Datenbank keine Transaktions Veröffentlichungen vorhanden sind. Die **is_tracked_by_cdc** Spalte in der [sys. Tables](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md) -Katalog Sicht wird auf 1 festgelegt.  
  
> [!NOTE]  
>  Der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent muss nicht aktiv sein, wenn Change Data Capture für eine Tabelle aktiviert wird. Das Transaktionsprotokoll und in die Änderungstabelle geschriebene Einträge werden jedoch erst vom Aufzeichnungsprozess verarbeitet, wenn der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent ausgeführt wird.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die Mitgliedschaft in der **db_owner** Fixed-Daten Bank Rolle.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-enabling-change-data-capture-by-specifying-only-required-parameters"></a>A. Aktivieren von Change Data Capture durch das Angeben von nur erforderlichen Parametern  
 Im folgenden Beispiel wird Change Data Capture für die `HumanResources.Employee`-Tabelle aktiviert. Nur die erforderlichen Parameter werden angegeben.  
  
```  
USE AdventureWorks2012;  
GO  
EXECUTE sys.sp_cdc_enable_table  
    @source_schema = N'HumanResources'  
  , @source_name = N'Employee'  
  , @role_name = N'cdc_Admin';  
GO  
```  
  
### <a name="b-enabling-change-data-capture-by-specifying-additional-optional-parameters"></a>B. Aktivieren von Change Data Capture durch das Angeben zusätzlicher optionaler Parameter  
 Im folgenden Beispiel wird Change Data Capture für die `HumanResources.Department`-Tabelle aktiviert. Alle Parameter außer `@allow_partition_switch` werden angegeben.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sys.sp_cdc_enable_table  
    @source_schema = N'HumanResources'  
  , @source_name = N'Department'  
  , @role_name = N'cdc_admin'  
  , @capture_instance = N'HR_Department'   
  , @supports_net_changes = 1  
  , @index_name = N'AK_Department_Name'   
  , @captured_column_list = N'DepartmentID, Name, GroupName'   
  , @filegroup_name = N'PRIMARY';  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [sys.sp_cdc_disable_table &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-disable-table-transact-sql.md)   
 [sys.sp_cdc_help_change_data_capture &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-change-data-capture-transact-sql.md)   
 [CDC.fn_cdc_get_all_changes_&#60;capture_instance&#62;  &#40;Transact-SQL-&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-all-changes-capture-instance-transact-sql.md)   
 [CDC.fn_cdc_get_net_changes_&#60;capture_instance&#62; &#40;Transact-SQL-&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-net-changes-capture-instance-transact-sql.md)   
 [sys.sp_cdc_help_jobs &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-jobs-transact-sql.md)  
  
  
