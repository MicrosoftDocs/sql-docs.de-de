---
description: Auto Stats-Ereignisklasse
title: Auto Stats-Ereignisklasse | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- Auto Stats event class
ms.assetid: cd613fce-01e1-4d8f-86cc-7ffbf0759f9e
author: stevestein
ms.author: sstein
ms.openlocfilehash: f9e20179799dbc61dae0a1e02247fe5eb0df2628
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/25/2020
ms.locfileid: "96126715"
---
# <a name="auto-stats-event-class"></a>Auto Stats-Ereignisklasse
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
   Die **Auto Stats**-Ereignisklasse zeigt an, dass ein automatisches Update der Index- und Spaltenstatistiken ausgeführt wurde.  **Auto Stats** wird außerdem ausgelöst, wenn Statistiken geladen werden, damit der Optimierer sie verwenden kann.
  
## <a name="auto-stats-event-class-data-columns"></a>Datenspalten der Auto Stats-Ereignisklasse  
  
|Datenspaltenname|Datentyp|BESCHREIBUNG|Column ID|Filterbar|  
|----------------------|---------------|-----------------|---------------|----------------|  
|**ApplicationName**|**nvarchar**|Name der Clientanwendung, die die Verbindung mit einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]hergestellt hat. Diese Spalte wird mit den Werten aufgefüllt, die von der Anwendung übergeben werden, und nicht mit dem angezeigten Namen des Programms.|10|Ja|  
|**ClientProcessID**|**int**|Die ID, die der Hostcomputer dem Prozess zuweist, in dem die Clientanwendung ausgeführt wird. Diese Datenspalte wird aufgefüllt, wenn der Client die Clientprozess-ID angibt.|9|Ja|  
|**DatabaseID**|**int**|Die ID der Datenbank, die durch die USE *database* -Anweisung angegeben wurde, bzw. die ID der Standarddatenbank, wenn für eine bestimmte Instanz keine USE *database* -Anweisung ausgegeben wurde. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] zeigt den Namen der Datenbank an, wenn die **ServerName** -Datenspalte in der Ablaufverfolgung aufgezeichnet wird und der Server verfügbar ist. Der Wert für eine Datenbank kann mithilfe der DB_ID-Funktion ermittelt werden.|3|Ja|  
|**DatabaseName**|**nvarchar**|Name der Datenbank, in der die Benutzeranweisung ausgeführt wird.|35|Ja|  
|**Duration**|**bigint**|Die Zeit (in Mikrosekunden), die für das Ereignis benötigt wurde.|13|Ja|  
|**EndTime**|**datetime**|Der Zeitpunkt, zu dem das Ereignis beendet wurde.|15|Ja|  
|**Fehler**|**int**|Fehlernummer eines bestimmten Ereignisses. Dies ist häufig die in der **sys.messages** -Katalogsicht gespeicherte Fehlernummer.|31|Ja|  
|**EventClass**|**int**|Ereignistyp = 58.|27|Nein|  
|**EventSequence**|**int**|Sequenz eines bestimmten Ereignisses innerhalb der Anforderung.|51|Nein|  
|**EventSubClass**|**int**|Der Typ der Ereignisunterklasse:<br /><br /> 1: Synchron erstellte/aktualisierte Statistiken. Die **TextData** -Spalte gibt an, welche Statistiken vorhanden sind, und ob sie erstellt oder aktualisiert wurden.<br /><br /> 2: Asynchrones Statistikupdate; Auftrag in Warteschlange.<br /><br /> 3: Asynchrones Statistikupdate; Auftrag wird gestartet.<br /><br /> 4: Asynchrones Statistikupdate; Auftrag abgeschlossen.|21|Ja|  
|**GroupID**|**int**|ID der Arbeitsauslastungsgruppe, in der das SQL-Ablaufverfolgungsereignis ausgelöst wird.|66|Ja|  
|**HostName**|**nvarchar**|Der Name des Computers, auf dem der Client ausgeführt wird. Diese Datenspalte wird aufgefüllt, wenn der Hostname durch den Client bereitgestellt wird. Der Hostname kann mithilfe der HOST_NAME-Funktion bestimmt werden.|8|Ja|  
|**IndexID**|**int**|Die ID für den Index-/Statistikeintrag des Objekts, das von dem Ereignis betroffen ist. Um die Index-ID für ein Objekt zu ermitteln, verwenden Sie die **index_id** -Spalte der Katalogsicht **sys.indexes** .|24|Ja|  
|**IntegerData**|**int**|Anzahl von Statistikauflistungen, die erfolgreich aktualisiert wurden|25|Ja|  
|**IntegerData2**|**int**|Auftragssequenznummer.|55|Ja|  
|**IsSystem**|**int**|Gibt an, ob das Ereignis bei einem Systemprozess oder einem Benutzerprozess aufgetreten ist. 1 = System, 0 = Benutzer.|60|Ja|  
|**LoginName**|**nvarchar**|Anmeldename des Benutzers ( [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Sicherheitsanmeldung oder Windows-Anmeldeinformationen im Format DOMAIN\username).|11|Ja|  
|**LoginSid**|**image**|Sicherheits-ID (SID) des angemeldeten Benutzers. Diese Informationen finden Sie in der **sys.server_principals** -Katalogsicht. Die SID ist für jede Anmeldung beim Server eindeutig.|41|Ja|  
|**NTDomainName**|**nvarchar**|Windows-Domäne, zu der der Benutzer gehört.|7|Ja|  
|**NTUserName**|**nvarchar**|Windows-Benutzername.|6|Ja|  
|**ObjectID**|**int**|Vom System zugewiesene ID des Objekts.|22|Ja|  
|**RequestID**|**int**|Die ID der Anforderung, die die Anweisung enthält.|49|Ja|  
|**ServerName**|**nvarchar**|Name der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz, für die eine Ablaufverfolgung ausgeführt wird.|26|Nein|  
|**SessionLoginName**|**nvarchar**|Der Anmeldename des Benutzers, der die Sitzung geöffnet hat. Wenn Sie z. B. mit Login1 eine Verbindung zu [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] herstellen und mit Login2 eine Anweisung ausführen, zeigt **SessionLoginName** Login1 an, und **LoginName** zeigt Login2 an. Diese Spalte zeigt sowohl den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] - als auch den Windows-Anmeldenamen an.|64|Ja|  
|**SPID**|**int**|Die ID der Sitzung, in der das Ereignis aufgetreten ist.|12|Ja|  
|**StartTime**|**datetime**|Zeitpunkt, zu dem das Ereignis begonnen hat (falls vorhanden).|14|Ja|  
|**Erfolgreich**|**int**|0 = Fehler<br /><br /> 1 = Erfolg<br /><br /> 2 = Aufgrund von Servereinschränkung ausgelassen (MSDE)|23|Ja|  
|**TextData**|**ntext**|Der Inhalt dieser Spalte hängt davon ab, ob die Statistiken synchron (**EventSubClass** 1) oder asynchron (**EventSubClass** 2, 3 oder 4) aktualisiert wurden:<br /><br /> 1: Listet auf, welche Statistiken aktualisiert/erstellt wurden<br /><br /> 2, 3 oder 4: NULL; **IndexID** -Spalte wird mit der Index-/Statistik-ID für aktualisierte Statistiken gefüllt.|1|Ja|  
|**TransactionID**|**bigint**|Die vom System zugewiesene ID der Transaktion.|4|Ja|  
|**Type**|**int**|Typ des Auftrags.|57|Ja|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Erweiterte Ereignisse](../../relational-databases/extended-events/extended-events.md)   
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)  
  
  
