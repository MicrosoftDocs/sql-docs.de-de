---
description: Degree of Parallelism (7.0 Insert)-Ereignisklasse
title: Degree of Parallelism (7.0 Insert)-Ereignisklasse | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Degree of Parallelism event class
ms.assetid: 6753ef30-890f-47a3-b0b6-8abb184e1d83
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 197dc975e6ca6e8119644188fac33aec77a541f2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182931"
---
# <a name="degree-of-parallelism-70-insert-event-class"></a>Degree of Parallelism (7.0 Insert)-Ereignisklasse
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
   Die Ereignisklasse **Degree of Parallelism (7.0 Insert)** tritt jedes Mal auf, wenn in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] eine SELECT-, INSERT-, UPDATE- oder DELETE-Anweisung ausgeführt wird.  
  
 Wenn diese Ereignisklasse in eine Ablaufverfolgung aufgenommen wird, wird die Leistung möglicherweise deutlich durch den Umfang des resultierendem Aufwands beeinträchtigt, wenn diese Ereignisse häufig auftreten. Wenn Sie den angefallenen Mehraufwand minimieren möchten, begrenzen Sie die Verwendung dieser Ereignisklasse auf Ablaufverfolgungen, die bestimmte Probleme nur kurzzeitig überwachen.  
  
## <a name="degree-of-parallelism-70-insert-event-class-data-columns"></a>Datenspalten der Degree of Parallelism (7.0 Insert)-Ereignisklasse  
  
|Datenspaltenname|Datentyp|BESCHREIBUNG|Column ID|Filterbar|  
|----------------------|---------------|-----------------|---------------|----------------|  
|**ApplicationName**|**nvarchar**|Name der Clientanwendung, die die Verbindung mit einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]hergestellt hat. Diese Spalte wird mit den Werten aufgefüllt, die von der Anwendung übergeben werden, und nicht mit dem angezeigten Namen des Programms.|10|Ja|  
|**BinaryData**|**image**|Anzahl der CPUs, die zum Abschließen des Prozesses auf der Grundlage der folgenden Werte verwendet werden:<br /><br /> Mit 0x00000000 wird ein serieller Plan angegeben, der in Serie ausgeführt wird.<br /><br /> Mit 0x01000000 wird ein paralleler Plan angegeben, der in Serie ausgeführt wird.<br /><br /> >= 0x02000000 gibt einen parallelen Plan an, der parallel ausgeführt wird.|2|Nein|  
|**ClientProcessID**|**int**|Die ID, die der Hostcomputer dem Prozess zuweist, in dem die Clientanwendung ausgeführt wird. Diese Datenspalte wird aufgefüllt, wenn die Clientprozess-ID durch den Client bereitgestellt wird.|9|Ja|  
|**DatabaseID**|**int**|Die ID der Datenbank, die durch die USE-Datenbankanweisung angegeben wurde, bzw. die ID der Standarddatenbank, wenn für eine bestimmte Instanz keine USE-Datenbankanweisung ausgegeben wurde. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] zeigt den Namen der Datenbank an, wenn die **ServerName** -Datenspalte in der Ablaufverfolgung aufgezeichnet wird und der Server verfügbar ist. Der Wert für eine Datenbank kann mithilfe der DB_ID-Funktion ermittelt werden.|3|Ja|  
|**DatabaseName**|**nvarchar**|Name der Datenbank, in der die Benutzeranweisung ausgeführt wird.|35|Ja|  
|**Ereignisklasse**|**int**|Ereignistyp = 28.|27|Nein|  
|**EventSequence**|**int**|Sequenz eines bestimmten Ereignisses innerhalb der Anforderung.|51|Nein|  
|**EventSubClass**|**int**|Gibt die ausgeführte Anweisung an, basierend auf den folgenden Werten:<br /><br /> 1 = Auswählen<br /><br /> 2 = Einfügen<br /><br /> 3 = Aktualisieren<br /><br /> 4 = Löschen|21|Nein|  
|**GroupID**|**int**|ID der Arbeitsauslastungsgruppe, in der das SQL-Ablaufverfolgungsereignis ausgelöst wird.|66|Ja|  
|**HostName**|**nvarchar**|Der Name des Computers, auf dem der Client ausgeführt wird. Diese Datenspalte wird aufgefüllt, wenn der Hostname durch den Client bereitgestellt wird. Der Hostname kann mithilfe der HOST_NAME-Funktion bestimmt werden.|8|Ja|  
|**Ganzzahlige Daten**|**int**|Der "Arbeitsbereichsspeicher" in KB, der der Abfrage zugeteilt wurde, um Hashing, Sortier- oder Indexerstellungsvorgänge auszuführen. Der Arbeitsspeicher wird während der Ausführung nach Bedarf zugeordnet.|25|Ja|  
|**IsSystem**|**int**|Gibt an, ob das Ereignis bei einem Systemprozess oder einem Benutzerprozess aufgetreten ist. 1 = System, 0 = Benutzer.|60|Ja|  
|**LoginName**|**nvarchar**|Anmeldename des Benutzers (Anmeldung der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Sicherheit oder Windows-Anmeldeinformationen im Format „DOMAIN\\*username*).|11|Ja|  
|**LoginSid**|**image**|Sicherheits-ID (SID) des angemeldeten Benutzers. Diese Informationen finden Sie in der sys.server_principals-Katalogsicht. Die SID ist für jede Anmeldung beim Server eindeutig.|41|Ja|  
|**NTDomainName**|**nvarchar**|Windows-Domäne, zu der der Benutzer gehört.|7|Ja|  
|**NTUserName**|**nvarchar**|Windows-Benutzername.|6|Ja|  
|**RequestID**|**int**|Die Anforderungs-ID, mit der die Volltextabfrage initiiert wurde.|49|Ja|  
|**ServerName**|**nvarchar**|Name der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz, für die eine Ablaufverfolgung ausgeführt wird.|26|Nein|  
|**SessionLoginName**|**nvarchar**|Der Anmeldename des Benutzers, der die Sitzung gestartet hat. Wenn Sie z. B. mit Login1 eine Verbindung zu [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] herstellen und mit Login2 eine Anweisung ausführen, zeigt **SessionLoginName** Login1 an, und **LoginName** zeigt Login2 an. Diese Spalte zeigt sowohl den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] - als auch den Windows-Anmeldenamen an.|64|Ja|  
|**SPID**|**int**|Die ID der Sitzung, in der das Ereignis aufgetreten ist.|12|Ja|  
|**StartTime**|**datetime**|Zeitpunkt, zu dem das Ereignis begonnen hat (falls vorhanden).|14|Ja|  
|**TransactionID**|**bigint**|Die vom System zugewiesene ID der Transaktion.|4|Ja|  
  
## <a name="see-also"></a>Weitere Informationen  
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)  
  
  
