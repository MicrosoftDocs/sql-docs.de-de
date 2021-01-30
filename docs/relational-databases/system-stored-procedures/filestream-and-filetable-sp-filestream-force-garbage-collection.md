---
description: sp_filestream_force_garbage_collection (Transact-SQL)
title: sp_filestream_force_garbage_collection (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_filestream_force_garbage_collection
- sp_filestream_force_garbage_collection_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- FILESTREAM [SQL Server]
- sp_filestream_force_garbage_collection
ms.assetid: 9d1efde6-8fa4-42ac-80e5-37456ffebd0b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0c234c5c03b00b25babbec0a4fbc197f235c966d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165248"
---
# <a name="sp_filestream_force_garbage_collection-transact-sql"></a>sp_filestream_force_garbage_collection (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Erzwingt die Ausführung des FILESTREAM-Garbage Collectors und löscht alle nicht benötigten FILESTREAM-Dateien.  
  
 Ein FILESTREAM-Container kann erst entfernt werden, wenn alle gelöschten Dateien darin vom Garbage Collector bereinigt wurden. Der FILESTREAM Garbage Collector wird automatisch ausgeführt. Wenn Sie jedoch einen Container entfernen müssen, bevor der Garbage Collector ausgeführt wurde, können Sie sp_filestream_force_garbage_collection verwenden, um die Garbage Collector manuell auszuführen.  
  
  
## <a name="syntax"></a>Syntax  
  
```  
sp_filestream_force_garbage_collection
    [ @dbname = ]  'database_name'
    [ , [ @filename = ] 'logical_file_name' ]
```  
  
## <a name="arguments"></a>Argumente  
 `[ @dbname = ]  'database_name'`  
 Gibt den Namen der Datenbank an, für die der Garbage Collector ausgeführt werden soll.  
  
> [!NOTE]  
> `@dbname` ist vom Typ **sysname**. Fehlt die Angabe, wird die aktuelle Datenbank zugrunde gelegt.  
  
 `[ @filename = ] 'logical_file_name'`  
 Gibt den logischen Namen des FILESTREAM-Containers an, für den der Garbage Collector ausgeführt werden soll. `@filename` ist optional. Wenn kein logischer Dateiname angegeben wird, bereinigt der Garbage Collector alle FILESTREAM-Container in der angegebenen Datenbank.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
  
| Wert | BESCHREIBUNG |
| ----- | ----------- |   
|0|Vorgang war erfolgreich.|  
|1|Fehler beim Vorgang.|  
  
## <a name="result-sets"></a>Resultsets  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|*file_name*|Gibt den Namen des FILESTREAM-Containers an|  
|*num_collected_items*|Gibt die Anzahl der FILESTREAM-Elemente (Dateien/Verzeichnisse) an, die vom Garbage Collector in diesem Container erfasst (gelöscht) wurden.|  
|*num_marked_for_collection_items*|Gibt die Anzahl der FILESTREAM-Elemente (Dateien/Verzeichnisse) an, die für den Garbage Collector in diesem Container markiert wurden. Diese Elemente wurden noch nicht gelöscht, können aber nach der Garbage Collection Phase gelöscht werden.|  
|*num_unprocessed_items*|Gibt die Anzahl der FILESTREAM-Elemente (Dateien oder Verzeichnisse) an, die nicht von der Garbage Collection in diesem FILESTREAM-Container erfasst wurden. Elemente können aus unterschiedlichen Gründen nicht verarbeitet werden:<br /><br /> Dateien, die festgesetzt werden müssen, da noch keine Protokollsicherung oder CheckPoint ausgeführt wurden.<br /><br /> Dateien im FULL- oder BULK_LOGGED-Wiederherstellungsmodell.<br /><br /> Es liegt eine aktive Transaktion mit langer Ausführungszeit vor.<br /><br /> Der Auftrag für den Replikations Protokoll Leser wurde nicht ausgeführt. Weitere Informationen finden Sie [im Whitepaper FILESTREAM-Speicher in SQL Server 2008](/previous-versions/sql/sql-server-2008/hh461480(v=msdn.10)) .|  
|*last_collected_xact_seqno*|Gibt die letzte Sequenznummer (LSN) für den entsprechenden FILESTREAM-Container an, bis zu der die Dateien von der Garbage Collection erfasst wurden.|  
  
## <a name="remarks"></a>Bemerkungen  
 Führt den FILESTREAM-Garbage Collector für die betreffende Datenbank (und den FILESTREAM-Container) vollständig aus. Dateien, die nicht mehr benötigt werden, werden vom Garbage Collection-Prozess entfernt. Die Zeit, die benötigt wird, damit dieser Vorgang abgeschlossen werden kann, hängt vom Umfang der FILESTREAM-Daten in dieser Datenbank oder in diesem Container sowie vom Ausmaß der DML-Aktivität im Zusammenhang mit den FILESTREAM-Daten in jüngster Zeit ab. Diese Vorgang kann auch ausgeführt werden, während die Datenbank online ist. Dies kann sich jedoch aufgrund verschiedener E/A-Aktivitäten im Rahmen der Garbage Collection auf die Leistung der Datenbank auswirken.  
  
> [!NOTE]  
>  Es wird empfohlen, diesen Vorgang nur bei Bedarf und außerhalb der üblichen Betriebsstunden auszuführen.  
  
Mehrere Aufrufe dieser gespeicherten Prozedur können nur in separaten Containern oder separaten Datenbanken gleichzeitig ausgeführt werden.  

Aufgrund von 2-Phasen-Vorgängen sollte die gespeicherte Prozedur zweimal ausgeführt werden, um zugrunde liegende FILESTREAM-Dateien tatsächlich zu löschen.  

Die Garbage Collection (GC) basiert auf Protokoll abkürzen. Wenn Dateien in der letzten Zeit in einer Datenbank mit dem vollständigen Wiederherstellungs Modell gelöscht wurden, werden Sie daher erst nach einer Protokoll Sicherung dieser Transaktionsprotokoll Teile und als inaktiv gekennzeichnet. In einer Datenbank, die das einfache Wiederherstellungs Modell verwendet, erfolgt eine Protokoll Kürzung, nachdem ein für `CHECKPOINT` die Datenbank ausgegeben wurde.  


## <a name="permissions"></a>Berechtigungen  
 Erfordert die Mitgliedschaft in der Datenbankrolle db_owner.  
  
## <a name="examples"></a>Beispiele  
 In den folgenden Beispielen wird der Garbage Collector für FILESTREAM-Container in der `FSDB`-Datenbank ausgeführt.  
  
### <a name="a-specifying-no-container"></a>A. Angeben keines Containers  
  
```sql  
USE FSDB;  
GO  
EXEC sp_filestream_force_garbage_collection @dbname = N'FSDB';  
```  
  
### <a name="b-specifying-a-container"></a>B. Angeben eines Containers  
  
```sql  
USE FSDB;  
GO  
EXEC sp_filestream_force_garbage_collection @dbname = N'FSDB',
    @filename = N'FSContainer';  
```  
  
## <a name="see-also"></a>Weitere Informationen  
[Filestream](../../relational-databases/blob/filestream-sql-server.md)
<br>[Filetables](../../relational-databases/blob/filetables-sql-server.md)
<br>[Dynamische Verwaltungssichten für Filestream und FileTable (Transact-SQL)](../system-dynamic-management-views/filestream-and-filetable-dynamic-management-views-transact-sql.md)
<br>[Katalogsichten für Filestream und FileTable (Transact-SQL)](../system-catalog-views/filestream-and-filetable-catalog-views-transact-sql.md)
<br>[sp_kill_filestream_non_transacted_handles (Transact-SQL)](filestream-and-filetable-sp-kill-filestream-non-transacted-handles.md)
  
