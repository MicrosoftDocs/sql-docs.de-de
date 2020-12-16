---
title: Ereignisablaufverfolgung für Windows-Ziel
description: In diesem Artikel erfahren Sie, wie Sie die Ereignisablaufverfolgung für Windows (Event Tracing for Windows, ETW) als Ziel verwenden. Verwenden Sie ETW entweder in Verbindung mit erweiterten Ereignissen oder als Ereignisconsumer von erweiterten Ereignissen.
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xevents
ms.topic: conceptual
helpviewer_keywords:
- event tracing for windows target
- ETW target
- targets [SQL Server extended events], event tracing for windows target
ms.assetid: ca2bb295-b7f6-49c3-91ed-0ad4c39f89d5
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6e458356777d9dfb6784491ae4dbdca6482b943f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465581"
---
# <a name="event-tracing-for-windows-target"></a>Ereignisablaufverfolgung für Windows-Ziel

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Bevor Sie die Ereignisablaufverfolgung für Windows (Event Tracing for Windows, ETW) als Ziel verwenden, sollten Sie über ausreichende Kenntnisse der ETW verfügen. ETW wird entweder in Verbindung mit erweiterten Ereignissen oder als Ereignisconsumer für erweiterte Ereignisse verwendet. Über die folgenden externen Links erhalten Sie erste Hintergrundinformationen zur ETW:  
  
-   [Windows-Ereignisse](/windows/win32/events/windows-events)  
  
-   [Verbessertes Debugging und Leistungsoptimierung mit ETW](/archive/msdn-magazine/2007/april/event-tracing-improve-debugging-and-performance-tuning-with-etw)  
  
 Das ETW-Ziel ist ein Singletonziel, auch wenn es mehreren Sitzungen hinzugefügt werden kann. Auch wenn ein Ereignis in vielen Sitzungen ausgelöst wird, wird das Ereignis nur einmal je aufgetretenem Ereignis an das ETW-Ziel weitergeleitet. Die Engine für erweiterte Ereignisse ist pro Prozess auf eine einzelne Instanz beschränkt.  
  
> [!IMPORTANT]  
>  Das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Dienststartkonto muss Mitglied der Gruppe der Leistungsprotokollbenutzer sein, damit das ETW-Ziel funktionieren kann.  
  
 Die Konfiguration der Ereignisse in einer ETW-Sitzung wird von einem Prozess gesteuert, in dem die Engine für erweiterte Ereignisse gehostet ist. Die Engine steuert, welche Ereignisse ausgelöst werden und welche Bedingungen erfüllt sein müssen, damit ein Ereignis ausgelöst wird.  
  
 Nachdem ein ETW-Ziel an eine Sitzung für erweiterte Ereignisse gebunden wurde, in der das ETW-Ziel zum ersten Mal während der Lebenszeit eines Prozesses angefügt wird, öffnet das ETW-Ziel eine einzelne ETW-Sitzung für den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anbieter. Wenn bereits eine ETW-Sitzung vorhanden ist, erhält das ETW-Ziel einen Verweis auf die vorhandene Sitzung. Diese ETW-Sitzung ist für alle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanzen auf einem bestimmten Computer freigegeben. Diese ETW-Sitzung empfängt alle Ereignisse von Sitzungen, die das ETW-Ziel aufweisen.  
  
 Da für ETW Anbieter erforderlich sind, damit Ereignisse verwendet werden und an ETW fließen können, sind alle Pakete für erweiterte Ereignisse für die Sitzung aktiviert. Wenn ein Ereignis ausgelöst wird, sendet das ETW-Ziel das Ereignis an die Sitzung, in der der Anbieter für das Ereignis aktiviert ist.  
  
 Das ETW-Ziel unterstützt die synchrone Veröffentlichung von Ereignissen in dem Thread, der das Ereignis auslöst. Das ETW-Ziel unterstützt jedoch keine asynchrone Ereignisveröffentlichung.  
  
 Das ETW-Ziel unterstützt keine Steuerung durch externe ETW-Controller, z. B. Logman.exe. Für ETW-Ablaufverfolgungen muss eine Ereignissitzung mit dem ETW-Ziel erstellt werden. Weitere Informationen finden Sie unter [CREATE EVENT SESSION &#40;Transact-SQL&#41;](../../t-sql/statements/create-event-session-transact-sql.md).  
  
> [!NOTE]  
>  Bei Aktivieren des ETW-Ziels wird eine ETW-Sitzung mit dem Namen XE_DEFAULT_ETW_SESSION erstellt. Wenn eine Sitzung mit dem Namen XE_DEFAULT_ETW_SESSION bereits vorhanden ist, wird diese ohne Änderung der Eigenschaften der vorhandenen Sitzung verwendet. XE_DEFAULT_ETW_SESSION wird von allen Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]gemeinsam genutzt. Nachdem Sie XE_DEFAULT_ETW_SESSION gestartet haben, müssen Sie dies mit einem ETW-Controller beenden, z. B. dem Logman-Tool. Sie können zum Beispiel den folgenden Befehl an der Eingabeaufforderung ausführen: **logman stop XE_DEFAULT_ETW_SESSION -ets**.  
  
 In der folgenden Tabelle werden die verfügbaren Optionen für das Konfigurieren des ETW-Ziels beschrieben.  
  
|Option|Zulässige Werte|BESCHREIBUNG|  
|------------|--------------------|-----------------|  
|default_xe_session_name|Eine Zeichenfolge mit bis zu 256 Zeichen. Dieser Wert ist optional.|Der Name der Sitzung für erweiterte Ereignisse. Standardmäßig ist dies XE_DEFAULT_ETW_SESSION.|  
|default_etw_session_logfile_path|Eine Zeichenfolge mit bis zu 256 Zeichen. Dieser Wert ist optional.|Der Pfad der Protokolldatei für die Sitzung für erweiterte Ereignisse. Standardmäßig ist dies %TEMP%\XEEtw.etl.|  
|default_etw_session_logfile_size_mb|Eine beliebige Ganzzahl ohne Vorzeichen. Dieser Wert ist optional.|Die Protokolldateigröße in Megabyte (MB) für die Sitzung für erweiterte Ereignisse. Die Standardeinstellung ist 20 MB.|  
|default_etw_session_buffer_size_kb|Eine beliebige Ganzzahl ohne Vorzeichen. Dieser Wert ist optional.|Die Größe des Puffers im Arbeitsspeicher in Kilobyte (KB) für die Sitzung für erweiterte Ereignisse. Die Standardeinstellung ist 128 KB.|  
|retries|Eine beliebige Ganzzahl ohne Vorzeichen.|Die Anzahl erneuter Versuche, das Ereignis im ETW-Subsystem zu veröffentlichen, bevor es gelöscht wird. Die Standardeinstellung ist 0.|  
| &nbsp; | &nbsp; | &nbsp; |

 Die Konfiguration dieser Einstellungen ist optional. Das ETW-Ziel verwendet Standardwerte für diese Einstellungen.  
  
 Das ETW-Ziel führt die folgenden Aufgaben aus:  
  
-   Erstellen der ETW-Standardsitzung.  
  
-   Registrieren aller Pakete für erweiterte Ereignisse bei ETW. So kann sichergestellt werden, dass Ereignisse nicht von ETW gelöscht werden.  
  
-   Verwalten des Ereignisflusses zu ETW. Das ETW-Ziel erstellt ein ETW-Ereignis mit Daten für erweiterte Ereignisse und sendet es an die zugehörige ETW-Sitzung. Wenn das Ereignis die Puffergröße überschreitet oder die Daten für ein ETW-Ereignis zu groß sind, teilt ETW das Ereignis in Fragmente.  
  
-   Ständiges Beibehalten der Aktivierung von Paketen für erweiterte Ereignisse.  
  
 Die folgenden Standarddateispeicherorte werden von ETW verwendet:  
  
-   Die ETW-Ausgabedatei befindet sich im Ordner %TEMP%\XEEtw.etl.  
  
    > [!IMPORTANT]  
    >  Der Dateipfad kann nicht geändert werden, nachdem die erste Sitzung gestartet wurde.  
  
-   Die MOF-Dateien (Managed Object Format) befinden sich im Ordner *\<your install path>* \Microsoft SQL Server\Shared. Weitere Informationen finden Sie unter [Managed Object Format](/windows/win32/wmisdk/managed-object-format--mof-) auf der MSDN-Website.

<!-- ?LinkId=92851  ==  https://docs.microsoft.com/windows/desktop/WmiSdk/managed-object-format--mof-
-->

## <a name="adding-the-target-to-a-session"></a>Hinzufügen des Ziels zu einer Sitzung  
 Wenn Sie das ETW-Ziel einer Sitzung für erweiterte Ereignisse hinzufügen möchten, müssen Sie beim Erstellen oder Ändern einer Ereignissitzung die folgende Anweisung einschließen:  
  
```sql
ADD TARGET package0.etw_classic_sync_target  
```  
  
 Weitere Informationen zu einem vollständigen Beispiel, das zeigt, wie das ETW-Ziel verwendet wird und wie die Daten angezeigt werden, finden Sie unter [Überwachen der Systemaktivität mit erweiterten Ereignissen](../../relational-databases/extended-events/monitor-system-activity-using-extended-events.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Ziele für erweiterte Ereignisse von SQL Server](targets-for-extended-events-in-sql-server.md)   
 [sys.dm_xe_session_targets &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-xe-session-targets-transact-sql.md)   
 [CREATE EVENT SESSION &#40;Transact-SQL&#41;](../../t-sql/statements/create-event-session-transact-sql.md)   
 [ALTER EVENT SESSION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-event-session-transact-sql.md)  
  
