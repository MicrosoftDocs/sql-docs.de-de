---
description: sys.fn_xe_file_target_read_file (Transact-SQL)
title: sys.fn_xe_file_target_read_file (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_xe_file_target_read_file_TSQL
- fn_xe_file_target_read_file
- sys.fn_xe_file_target_read_file_TSQL
- sys.fn_xe_file_target_read_file
dev_langs:
- TSQL
helpviewer_keywords:
- extended events [SQL Server], functions
- fn_xe_file_target_read_file function
- sys.fn_xe_file_target_read_file function
ms.assetid: cc0351ae-4882-4b67-b0d8-bd235d20c901
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: daa5c2bb81ea9f89ebf415e242e881732ad08389
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2021
ms.locfileid: "100338099"
---
# <a name="sysfn_xe_file_target_read_file-transact-sql"></a>sys.fn_xe_file_target_read_file (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Liest Dateien, die vom asynchronen Dateiziel der erweiterten Ereignisse erstellt werden. Pro Zeile wird ein Ereignis im XML-Format zurückgegeben.  
  
> [!WARNING]  
>  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] akzeptieren Ablauf Verfolgungs Ergebnisse, die im xel-und xem-Format generiert werden. [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] Erweiterte Ereignisse unterstützen nur Ablauf Verfolgungs Ergebnisse im XEL-Format. Verwenden Sie SQL Server Management Studio, um Ablaufverfolgungsergebnisse im XEL-Format lesen zu können.    
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sys.fn_xe_file_target_read_file ( path, mdpath, initial_file_name, initial_offset )  
```  
  
## <a name="arguments"></a>Argumente  
 *path*  
 Der Pfad zu den zu lesenden Dateien. der *Pfad* kann Platzhalter enthalten und den Namen einer Datei enthalten. *Pfad ist vom Datentyp* **nvarchar (260)**. Es gibt keinen Standardwert. Im Kontext von Azure SQL-Datenbank handelt es sich bei diesem Wert um eine HTTP-URL zu einer Datei in Azure Storage.
  
 *mdpath*  
 Der Pfad zur Metadatendatei, die der Datei oder den Dateien entspricht, die durch das *path* -Argument angegeben werden. *mdpath ist vom Datentyp* **nvarchar (260)**. Es gibt keinen Standardwert. Ab SQL Server 2016 kann dieser Parameter als NULL angegeben werden.
  
> [!NOTE]  
>  [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] der *mdpath* -Parameter ist nicht erforderlich. Er wird jedoch beibehalten, um die Abwärtskompatibilität für in früheren Versionen von SQL Server erstellte Protokolldateien aufrechtzuerhalten.  
  
 *initial_file_name*  
 Die erste Datei, die aus dem *Pfad* gelesen werden soll. *initial_file_name* ist vom Datentyp **nvarchar (260)**. Es gibt keinen Standardwert. Wenn **null** als Argument angegeben wird, werden alle im *Pfad* gefundenen Dateien gelesen.  
  
> [!NOTE]  
>  *initial_file_name* und *initial_offset* sind paarweise Verknüpfungs Argumente. Wenn Sie einen Wert für eines der beiden Argumente angeben, müssen Sie auch einen Wert für das andere Argument angeben.  
  
 *initial_offset*  
 Wird verwendet, um den letzten zuvor gelesenen Offset anzugeben und überspringt alle Ereignisse bis (einschließlich) des Offsets. Die Ereignisenumeration startet nach dem angegebenen Offset. *initial_offset* ist **bigint**. Wenn **null** als Argument angegeben wird, wird die gesamte Datei gelesen.  
  
## <a name="table-returned"></a>Zurückgegebene Tabelle  
  
|Spaltenname|Datentyp|Beschreibung|  
|-----------------|---------------|-----------------|  
|module_guid|**uniqueidentifier**|Die Ereignismodul-GUID. Lässt keine NULL-Werte zu.|  
|package_guid|**uniqueidentifier**|Die Ereignispaket-GUID. Lässt keine NULL-Werte zu.|  
|object_name|**nvarchar(256)**|Der Name des Ereignisses. Lässt keine NULL-Werte zu.|  
|event_data|**nvarchar(max)**|Der Ereignisinhalt im XML-Format. Lässt keine NULL-Werte zu.|  
|file_name|**nvarchar(260)**|Der Name der Datei, die das Ereignis enthält. Lässt keine NULL-Werte zu.|  
|file_offset|**bigint**|Der Offset des Blocks in der Datei, der das Ereignis enthält. Lässt keine NULL-Werte zu.|  
|timestamp_utc|**datetime2**|**Gilt für:** [!INCLUDE[sssql17](../../includes/sssql17-md.md)] und höher und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].<br /><br />Das Datum und die Uhrzeit (UTC-Zeitzone) des Ereignisses. Lässt keine NULL-Werte zu.|  

  
## <a name="remarks"></a>Bemerkungen  
 Das Lesen umfangreicher Resultsets durch Ausführen von **sys.fn_xe_file_target_read_file** in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] kann zu einem Fehler führen. Verwenden Sie das **Ergebnis im Datei** Modus (**STRG + UMSCHALT + F**), um große Resultsets in eine Datei zu exportieren und die Datei stattdessen mit einem anderen Tool zu lesen.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die VIEW SERVER STATE-Berechtigung auf dem Server.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-retrieving-data-from-file-targets"></a>A. Abrufen von Daten aus Dateizielen  
 Im folgenden Beispiel werden alle Zeilen aus allen Dateien abgerufen. In dieser Beispieldatei befinden sich die Dateiziele und Metadateien im Ablaufverfolgungsordner auf dem Laufwerk C:\.  
  
```  
SELECT * FROM sys.fn_xe_file_target_read_file('C:\traces\*.xel', 'C:\traces\metafile.xem', null, null);  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungs Sichten für erweiterte Ereignisse](../../relational-databases/system-dynamic-management-views/extended-events-dynamic-management-views.md)   
 [Katalog Sichten für erweiterte Ereignisse &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/extended-events-catalog-views-transact-sql.md)   
 [Erweiterte Ereignisse](../../relational-databases/extended-events/extended-events.md)  
  
  
