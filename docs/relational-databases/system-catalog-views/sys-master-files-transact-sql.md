---
description: sys.master_files (Transact-SQL)
title: sys.master_files (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/10/2016
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.master_files
- master_files_TSQL
- sys.master_files_TSQL
- master_files
dev_langs:
- TSQL
helpviewer_keywords:
- sys.master_files catalog view
ms.assetid: 803b22f2-0016-436b-a561-ce6f023d6b6a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 59258c52e24e5b9d85f9096d6eb2d24fa3d48df3
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489454"
---
# <a name="sysmaster_files-transact-sql"></a>sys.master_files (Transact-SQL)
[!INCLUDE [sql-asdbmi-pdw](../../includes/applies-to-version/sql-asdbmi-pdw.md)]

  Enthält eine Zeile pro Datei einer Datenbank, die als master-Datenbank gespeichert ist. Dies ist eine einzelne, systemweite Sicht.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|database_id|**int**|ID der Datenbank, auf die sich diese Datei bezieht Der masterdatabase_id ist immer 1.|  
|file_id|**int**|ID der Datei in der Datenbank Die ID der primären Datei ist immer 1.|  
|file_guid|**uniqueidentifier**|Der eindeutige Bezeichner der Datei.<br /><br /> NULL = die Datenbank wurde von einer früheren Version von aktualisiert [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (gültig für SQL Server 2005 und früher).|  
|Typ|**tinyint**|Dateityp:<br /><br /> 0 = Zeilen<br /><br /> 1 = Protokoll<br /><br /> 2 = FILESTREAM<br /><br /> 3 = [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> 4 = Volltext (Volltextkataloge vor [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]; Volltextkataloge, die auf [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] oder höher aktualisiert oder darin erstellt wurden, geben den Dateityp 0 zurück.)|  
|type_desc|**nvarchar(60)**|Beschreibung des Dateityps:<br /><br /> ROWS<br /><br /> PROTOKOLL<br /><br /> FILESTREAM<br /><br /> FULLTEXT (Volltextkataloge vor [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)].)|  
|data_space_id|**int**|Die ID des Datenspeicherplatzes, zu dem diese Datei gehört. Der Datenspeicherplatz ist eine Dateigruppe.<br /><br /> 0 = Protokolldateien|  
|name|**sysname**|Logischer Name der Datei in der Datenbank|  
|physical_name|**nvarchar(260)**|Betriebssystem-Dateiname|  
|state|**tinyint**|Dateistatus:<br /><br /> 0 = ONLINE<br /><br /> 1 = RESTORING<br /><br /> 2 = RECOVERING<br /><br /> 3 = RECOVERY_PENDING<br /><br /> 4 = SUSPECT<br /><br /> 5 = [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> 6 = OFFLINE<br /><br /> 7 = DEFUNCT|  
|state_desc|**nvarchar(60)**|Beschreibung des Dateistatus:<br /><br /> ONLINE<br /><br /> RESTORING<br /><br /> RECOVERING<br /><br /> RECOVERY_PENDING<br /><br /> SUSPECT<br /><br /> OFFLINE<br /><br /> DEFUNCT<br /><br /> Weitere Informationen finden Sie im Abschnitt [Dateistatus](../../relational-databases/databases/file-states.md).|  
|size|**int**|Die aktuelle Dateigröße in Seiten mit einer Größe von 8 KB. Für eine Datenbankmomentaufnahme gibt size den Speicherplatz an, den die Momentaufnahme maximal für die Datei verwenden kann.<br /><br /> Hinweis: Dieses Feld wird für FILESTREAM-Container als 0 (null) aufgefüllt. Fragen Sie die *sys.database_files* -Katalog Sicht nach der tatsächlichen Größe der FILESTREAM-Container ab.|  
|max_size|**int**|Maximale Dateigröße in Seiten mit einer Größe von 8 KB:<br /><br /> 0 = Keine Vergrößerung zulässig.<br /><br /> -1 = Datei wird vergrößert, bis der Datenträger voll ist.<br /><br /> 268435456 = Protokolldatei wird bis zu einer maximalen Größe von 2 TB vergrößert.<br /><br /> Hinweis: Datenbanken, die mit einer unbegrenzten Protokolldatei Größe aktualisiert werden, melden-1 für die maximale Größe der Protokolldatei.|  
|growth|**int**|0 = Die Datei hat eine feste Größe und wird nicht vergrößert.<br /><br /> >0 = die Datei wird automatisch vergrößert.<br /><br /> Falls is_percent_growth = 0, erfolgt die Vergrößerung in Schritten von 8-KB-Seiten, gerundet auf die nächsten 64 KB.<br /><br /> Falls is_percent_growth = 1, erfolgt die Vergrößerung als ganzzahliger Prozentwert.|  
|is_media_read_onlyF|**bit**|1 = Die Datei befindet sich auf einem schreibgeschützten Medium.<br /><br /> 0 = Die Datei befindet sich auf einem Medium mit Lese-/Schreibzugriff.|  
|is_read_only|**bit**|1 = Die Datei ist als schreibgeschützt gekennzeichnet.<br /><br /> 0 = Die Datei ermöglicht den Lese-/Schreibzugriff.|  
|is_sparse|**bit**|1 = Die Datei ist eine Sparsedatei.<br /><br /> 0 = Die Datei ist keine Sparsedatei.<br /><br /> Weitere Informationen finden Sie unter [Anzeigen der Größe der Datei mit geringer Dichte einer Datenbank-Momentaufnahme &#40;Transact-SQL&#41;](../../relational-databases/databases/view-the-size-of-the-sparse-file-of-a-database-snapshot-transact-sql.md).|  
|is_percent_growth|**bit**|1 = Die Vergrößerung der Datei erfolgt prozentual.<br /><br /> 0 = Absolute Vergrößerung in Seiten.|  
|is_name_reserved|**bit**|1 = Der gelöschte Dateiname kann wiederverwendet werden. Eine Protokollsicherung muss erstellt werden, bevor der Name (name oder physical_name) für einen neuen Dateinamen wiederverwendet werden kann.<br /><br /> 0 = Der Dateiname kann nicht wiederverwendet werden.|  
|create_lsn|**numeric(25,0)**|Protokollfolgenummer (LSN, Log Sequence Number), bei der die Datei erstellt wurde|  
|drop_lsn|**numeric(25,0)**|LSN, bei der die Datei gelöscht wurde|  
|read_only_lsn|**numeric(25,0)**|LSN, bei der die Dateigruppe mit der Datei von Lesen/Schreiben in Schreibgeschützt geändert wurde (letzte Änderung)|  
|read_write_lsn|**numeric(25,0)**|LSN, bei der die Dateigruppe mit der Datei von Schreibgeschützt in Lesen/Schreiben geändert wurde (letzte Änderung)|  
|differential_base_lsn|**numeric(25,0)**|Die Basis für differenzielle Sicherungen. Datenblöcke, die nach dieser LSN geändert wurden, werden in eine differenzielle Sicherung eingeschlossen.|  
|differential_base_guid|**uniqueidentifier**|Der eindeutige Bezeichner der Basissicherung, auf der eine differenzielle Sicherung basiert.|  
|differential_base_time|**datetime**|Zeit, die differential_base_lsn entspricht|  
|redo_start_lsn|**numeric(25,0)**|LSN, bei der das nächste Rollforward beginnen muss.<br /><br /> Ist NULL, außer state = RESTORING oder state = RECOVERY_PENDING.|  
|redo_start_fork_guid|**uniqueidentifier**|Eindeutiger Bezeichner des Verzweigungspunkts. Die first_fork_guid der nächsten wiederhergestellten Protokollsicherung muss mit diesem Wert übereinstimmen. Dies stellt den aktuellen Status des Containers dar.|  
|redo_target_lsn|**numeric(25,0)**|Die LSN, bei der das Onlinerollforward für diese Datei beendet werden kann.<br /><br /> Ist NULL, außer state = RESTORING oder state = RECOVERY_PENDING.|  
|redo_target_fork_guid|**uniqueidentifier**|Die Wiederherstellungsverzweigung, bei der der Container wiederhergestellt werden kann. Hierzu gehört redo_target_lsn.|  
|backup_lsn|**numeric(25,0)**|Die LSN der letzten Datensicherung oder differenziellen Sicherung der Datei.|  
|credential_id|**int**|Der, der `credential_id` `sys.credentials` zum Speichern der Datei verwendet wird. Wenn beispielsweise [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf einem virtuellen Azure-Computer ausgeführt wird und die Datenbankdateien in Azure BLOB Storage gespeichert werden, werden Anmelde Informationen mit den Anmelde Informationen für den Zugriff auf den Speicherort konfiguriert.|  
  
> [!NOTE]  
>  Wenn Sie große Indizes löschen oder neu erstellen bzw. wenn Sie große Tabellen löschen oder abschneiden, verzögert [!INCLUDE[ssDE](../../includes/ssde-md.md)] die Aufhebung der aktuellen Seitenzuordnungen sowie die zugehörigen Sperren, bis für die Transaktion ein Commit ausgeführt wird. Bei verzögerten Löschvorgängen wird der zugeordnete Speicherplatz nicht sofort freigegeben. Deshalb geben die von sys.master_files unmittelbar nach dem Löschen oder Abschneiden eines umfangreichen Objekts zurückgegebenen Werte möglicherweise nicht den tatsächlich verfügbaren Speicherplatz an.  

> [!NOTE]  
>  Für tempdb zeigt View sys.master_files die anfängliche tempdb-Größe an. Die Werte werden als Vorlage für die Erstellung von tempdb beim Start von SQL Server verwendet. Wenn tempdb wächst, wird es daher nicht in der Ansicht widergespiegelt. Um die aktuelle Größe der tempdb-Dateien zu erhalten, Fragen Sie ab `tempdb.sys.database_files` .
  
## <a name="permissions"></a>Berechtigungen  
 Zum Anzeigen der entsprechenden Zeile ist mindestens eine der Berechtigungen CREATE DATABASE, ALTER ANY DATABASE oder VIEW ANY DEFINITION erforderlich.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Datenbanken und Dateikatalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/databases-and-files-catalog-views-transact-sql.md)   
 [Dateistatus](../../relational-databases/databases/file-states.md)   
 [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [Datenbankdateien und Dateigruppen](../../relational-databases/databases/database-files-and-filegroups.md)  
  
  
