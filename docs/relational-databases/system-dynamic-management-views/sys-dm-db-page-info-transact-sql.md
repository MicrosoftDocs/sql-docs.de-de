---
description: sys.dm_db_page_info (Transact-SQL)
title: sys.dm_db_page_info (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 09/18/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
f1_keywords:
- sys.dm_db_page_info
- sys.dm_db_page_info_TSQL
- dm_db_page_info
- dm_db_page_info_TSQL
- dbcc page
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_page_info dynamic management view
author: bluefooted
ms.author: pamela
manager: amitban
monikerRange: '>=sql-server-ver15'
ms.openlocfilehash: df904c938418b6d57bc5aac286e482311de4554a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100071423"
---
# <a name="sysdm_db_page_info-transact-sql"></a>sys.dm_db_page_info (Transact-SQL)

[!INCLUDE[tsql-appliesto-ssver15-asdb-xxxx-xxx](../../includes/tsql-appliesto-ssver15-asdb-xxxx-xxx.md)]

Gibt Informationen zu einer Seite in einer Datenbank zurück.  Die-Funktion gibt eine Zeile zurück, die die Header Informationen der Seite enthält, einschließlich `object_id` , `index_id` und `partition_id` .  Dank dieser Funktion ist die Verwendung von `DBCC PAGE` in den meisten Fällen nicht mehr erforderlich.

> [!NOTE]
> `sys.dm_db_page_info` wird derzeit nur in und höher unterstützt [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] .


## <a name="syntax"></a>Syntax   
```  
sys.dm_db_page_info ( DatabaseId, FileId, PageId, Mode )  
``` 

## <a name="arguments"></a>Argumente  
*DatabaseID* | NULL | Vorgegebene     
Die ID der Datenbank. *DatabaseID* ist vom Datentyp **smallint**. Eine gültige Eingabe ist die ID einer Datenbank. Der Standardwert ist NULL. das Senden eines NULL-Werts für diesen Parameter führt jedoch zu einem Fehler.
 
 Mit der NULL | Vorgegebene   
Die ID der Datei. " *Fleid* " ist " **int**"  Eine gültige Eingabe ist die ID-Nummer einer Datei in der Datenbank, die von *DatabaseID* angegeben wird. Der Standardwert ist NULL. das Senden eines NULL-Werts für diesen Parameter führt jedoch zu einem Fehler.

*PageID* | NULL | Vorgegebene   
Die ID der Seite.  *PageID* ist vom Datentyp **int**.  Eine gültige Eingabe ist die ID einer Seite in der Datei, die von " *fleid*" angegeben wird. Der Standardwert ist NULL. das Senden eines NULL-Werts für diesen Parameter führt jedoch zu einem Fehler.

*Modus* | NULL | Vorgegebene   
Bestimmt die Detailebene in der Ausgabe der Funktion. "Limited" gibt NULL-Werte für alle Beschreibungs Spalten zurück, "ausführlich" füllt Beschreibungs Spalten auf.  Der Standardwert ist ' Limited '.

## <a name="table-returned"></a>Zurückgegebene Tabelle  

|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|database_id |INT |Datenbank-ID |
|file_id |INT |Datei-ID |
|page_id |INT |Seiten-ID |
|page_header_version |INT |Seitenkopf Version |
|page_type |INT |Seitentyp |
|page_type_desc |nvarchar (64) |Beschreibung des Seiten Typs |
|page_type_flag_bits |nvarchar (64) |Typflag "Bits" in Seitenkopf |
|page_type_flag_bits_desc |nvarchar (64) |Typflag "Bits Description" im Seitenkopf |
|page_flag_bits |nvarchar (64) |Bits im Seitenkopf markieren |
|page_flag_bits_desc |nvarchar(256) |Bits-Beschreibung im Seitenkopf markieren |
|page_lsn |nvarchar (64) |Protokoll Sequenznummer/Zeitstempel |
|page_level |INT |Ebene der Seite in Index (Blatt = 0) |
|object_id |INT |ID des Objekts, das die Seite besitzt |
|index_id |INT |ID des Indexes (0 für Heap Datenseiten) |
|partition_id |BIGINT |Die ID der Partition. |
|alloc_unit_id |BIGINT |ID der Zuordnungs Einheit |
|is_encrypted |bit |Bit, das angibt, ob die Seite verschlüsselt ist |
|has_checksum |bit |Bit, um anzugeben, ob die Seite einen Prüfsummen Wert hat |
|Prüfsumme |INT |Speichert den Prüfsummen Wert, mit dem Daten beschädigt erkannt werden. |
|is_iam_pg |bit |Bit, um anzugeben, ob es sich bei der Seite um eine IAM-Seite handelt  |
|is_mixed_ext |bit |Bit, das angibt, ob es in einem gemischten Block zugeordnet ist |
|has_ghost_records |bit |Bit, das angibt, ob die Seite inaktive Datensätze enthält <br> Ein inaktive Datensatz ist ein Datensatz, der zum Löschen markiert wurde, aber noch entfernt werden muss.|
|has_version_records |bit |Bit, das angibt, ob die Seite für die [Beschleunigte Daten Bank Wiederherstellung](../backup-restore/restore-and-recovery-overview-sql-server.md#adr) verwendete Versionsdaten Sätze enthält |
|pfs_page_id |INT |Seiten-ID der entsprechenden PFS-Seite |
|pfs_is_allocated |bit |Bit, um anzugeben, ob die Seite auf der entsprechenden PFS-Seite als zugeordnet gekennzeichnet ist |
|pfs_alloc_percent |INT |Belegungs Prozentsatz, wie durch das entsprechende PFS-Byte angegeben |
|pfs_status |nvarchar (64) |PFS-Byte |
|pfs_status_desc |nvarchar (64) |Die Beschreibung des PFS-bytes. |
|gam_page_id |INT |Seiten-ID der entsprechenden GAM-Seite |
|gam_status |bit |Bit, das angibt, ob es in GAM zugeordnet ist |
|gam_status_desc |nvarchar (64) |Beschreibung des GAM-Status Bit |
|sgam_page_id |INT |Seiten-ID der entsprechenden SGAM-Seite |
|sgam_status |bit |Bit, das angibt, ob es in SGAM zugeordnet ist |
|sgam_status_desc |nvarchar (64) |Beschreibung des SGAM-Status Bit |
|diff_map_page_id |INT |Seiten-ID der entsprechenden differenziellen Bitmap-Seite |
|diff_status |bit |Bit, das angibt, ob der diff-Status geändert wird |
|diff_status_desc |nvarchar (64) |Beschreibung des diff-Statusbits |
|ml_map_page_id |INT |Seiten-ID der entsprechenden Bitmap-Seite der minimalen Protokollierung |
|ml_status |bit |Bit, das angibt, ob die Seite minimal protokolliert wird |
|ml_status_desc |nvarchar (64) |Beschreibung des Bits mit minimaler Protokollierungs Status |
|prev_page_file_id |SMALLINT |Datei-ID der vorherigen Seite |
|prev_page_page_id |INT |Seiten-ID der vorherigen Seite |
|next_page_file_id |SMALLINT |Datei-ID der nächsten Seite |
|next_page_page_id |INT |ID der nächsten Seiten Seite |
|fixed_length |SMALLINT |Länge von Zeilen mit fester Größe |
|slot_count |SMALLINT |Gesamtanzahl der Slots (verwendet und nicht verwendet) <br> Bei einer Datenseite entspricht diese Zahl der Anzahl von Zeilen. |
|ghost_rec_count |SMALLINT |Anzahl von Datensätzen, die auf der Seite als "Ghost" gekennzeichnet <br> Ein inaktive Datensatz ist ein Datensatz, der zum Löschen markiert wurde, aber noch entfernt werden muss. |
|free_bytes |SMALLINT |Anzahl der freien Bytes auf der Seite |
|free_data_offset |INT |Offset des freien Speicherplatzes am Ende des Datenbereichs |
|reserved_bytes |SMALLINT |Anzahl von freien bytes, die von allen Transaktionen (bei Heap) reserviert werden <br> Anzahl der ggf. gehosteten Zeilen (bei Index Blatt) |
|reserved_bytes_by_xdes_id |SMALLINT |Von m_xdesID beigetragener Speicherplatz m_reservedCnt <br> Nur zu Debuggingzwecken |
|xdes_id |nvarchar (64) |Letzte von m_reserved beigetragene Transaktion <br> Nur zu Debuggingzwecken |
||||

## <a name="remarks"></a>Bemerkungen
Die `sys.dm_db_page_info` dynamische Verwaltungsfunktion gibt Seiten Informationen wie `page_id` , `file_id` , `index_id` `object_id` usw. zurück, die in einem Seitenkopf vorhanden sind. Diese Informationen sind für die Problembehandlung und das Debuggen verschiedener Leistungsprobleme (Sperr-und Latchkonflikte) und Beschädigungs Probleme nützlich.

`sys.dm_db_page_info` kann anstelle der- `DBCC PAGE` Anweisung in vielen Fällen verwendet werden, aber es werden nur die Seitenheader Informationen und nicht der Text der Seite zurückgegeben. `DBCC PAGE` wird immer noch für Anwendungsfälle benötigt, in denen der gesamte Inhalt der Seite erforderlich ist.

## <a name="using-in-conjunction-with-other-dmvs"></a>Verwenden von in Verbindung mit anderen DMVs
Einer der wichtigen Anwendungsfälle von `sys.dm_db_page_info` besteht darin, ihn mit anderen DMVs zu verknüpfen, die Seiten Informationen verfügbar machen.  Um diesen Anwendungsfall zu vereinfachen, wurde eine neue Spalte namens `page_resource` hinzugefügt, die Seiten Informationen in einem 8-Byte-Hexadezimal Format verfügbar macht. Diese Spalte wurde zu und hinzu `sys.dm_exec_requests` gefügt `sys.sysprocesses` und wird in Zukunft nach Bedarf anderen DMVs hinzugefügt.

Eine neue Funktion, `sys.fn_PageResCracker` , übernimmt `page_resource` als Eingabe und gibt eine einzelne Zeile aus, die `database_id` , `file_id` und enthält `page_id` .  Diese Funktion kann dann verwendet werden, um Joins zwischen `sys.dm_exec_requests` oder `sys.sysprocesses` und zu vereinfachen `sys.dm_db_page_info` .

## <a name="permissions"></a>Berechtigungen  
Erfordert die- `VIEW DATABASE STATE` Berechtigung in der Datenbank.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-displaying-all-the-properties-of-a-page"></a>A. Anzeigen aller Eigenschaften einer Seite
Die folgende Abfrage gibt eine Zeile mit allen Seiten Informationen für eine bestimmte `database_id` `file_id` `page_id` Kombination aus,, mit dem Standardmodus (' Limited ') zurück.

```sql
SELECT *  
FROM sys.dm_db_page_info (5, 1, 15, DEFAULT)
```

### <a name="b-using-sysdm_db_page_info-with-other-dmvs"></a>B. Verwenden von sys.dm_db_page_info mit anderen DMVs 

Die folgende Abfrage gibt eine Zeile zurück, die von verfügbar gemacht `wait_resource` wird, `sys.dm_exec_requests` Wenn die Zeile einen nicht-NULL-Wert enthält `page_resource`

```sql
SELECT page_info.* 
FROM sys.dm_exec_requests AS d  
CROSS APPLY sys.fn_PageResCracker (d.page_resource) AS r  
CROSS APPLY sys.dm_db_page_info(r.db_id, r.file_id, r.page_id, 'LIMITED') AS page_info
```

## <a name="see-also"></a>Weitere Informationen  
[Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
[Dynamische Verwaltungs Sichten im Zusammenhang mit der Datenbank &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)   
[sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)     
[sys.fn_PageResCracker](../../relational-databases/system-functions/sys-fn-pagerescracker-transact-sql.md)


