---
title: SQL Server, Datenbankreplikat | Microsoft-Dokumentation
description: Hier lernen Sie das „SQLServer:Datenbankreplikat“-Leistungsobjekt kennen, das Leistungsindikatoren enthält zu den sekundären Datenbanken einer Always On-Verfügbarkeitsgruppe enthält.
ms.custom: ''
ms.date: 08/24/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- SQLServer:Database Replica
- performance counters [SQL Server], AlwaysOn Availability Groups
- Availability Groups [SQL Server], performance counters
ms.assetid: a5f6bdce-2b13-4924-aaeb-b50b57d624d8
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: 280fae3d260995a15a7baa9e40a861cb2cfdecc4
ms.sourcegitcommit: 783b35f6478006d654491cb52f6edf108acf2482
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91892230"
---
# <a name="sql-server-database-replica"></a>SQL Server, Datenbankreplikat

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Das **SQLServer:Datenbankreplikat** -Leistungsobjekt enthält Leistungsindikatoren, die Informationen zu den sekundären Datenbanken einer Always On-Verfügbarkeitsgruppe in [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]bereitstellen. Dieses Objekt ist auf nur einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz gültig, die ein sekundäres Replikat hostet.  
  
|Name des Leistungsindikators|BESCHREIBUNG|Anzeige für...|  
|------------------|-----------------|--------------|  
|**Empfangene Dateibytes/s**|Die Menge der FILESTREAM-Daten, die vom sekundären Replikat für die sekundäre Datenbank in der letzten Sekunde empfangen wurde.|Sekundäres Replikat|  
|**Warteschlange für ausstehende Protokollanwendung**|Anzahl der Protokollblöcke, die darauf warten, in das Datenbankreplikat übernommen zu werden.|Sekundäres Replikat|
|**Warteschlange für bereite Protokollanwendung**|Anzahl der Protokollblöcke, die bereit sind und darauf warten, in das Datenbankreplikat übernommen zu werden.|Sekundäres Replikat|
|**Empfangene Protokollbytes/s**|Die Menge der Protokolldatensätze, die vom sekundären Replikat für die Datenbank in der letzten Sekunde empfangen wurde.|Sekundäres Replikat|  
|**Verbleibendes Protokoll für Rollbackphase**|Entspricht der Menge an Protokollen in Kilobytes, die für das Ausführen der Rollbackphase verbleiben.|Sekundäres Replikat|  
|**Protokollsendewarteschlange**|Die Menge der Protokolldatensätze in den Protokolldateien der primären Datenbank in Kilobytes, die nicht an das sekundäre Replikat gesendet wurden. Dieser Wert wird vom primären Replikat an das sekundäre Replikat gesendet. Die Warteschlangengröße umfasst keine FILESTREAM-Dateien, die an ein sekundäres Replikat gesendet wurden.|Sekundäres Replikat|  
|**Gespiegelte Schreibtransaktionen/Sekunde**|Anzahl von Transaktionen, die in die primäre Datenbank geschrieben und beim Senden des Protokolls an die sekundäre Datenbank commitet wurden (in der letzten Sekunde).|Primäres Replikat|  
|**Wiederherstellungswarteschlange**|Die Menge der Protokolldatensätze in den Protokolldateien des sekundären Replikats, die nicht wiederholt wurde.|Sekundäres Replikat|  
|**Blockierte Wiederholungen/s**|Häufigkeit der Blockierung des REDO-Threads für Sperren von Lesern der Datenbank.|Sekundäres Replikat|  
|**Verbleibende Wiederholungsbytes**|Die zur Wiederholung verbleibende Protokollmenge in Kilobytes bis zum Abschließen der Wiederherstellungsphase.|Sekundäres Replikat|  
|**Wiederholte Bytes/s**|Menge an Protokolldatensätzen, die in der letzten Sekunde auf der sekundären Datenbank wiederholt wurden.|Sekundäres Replikat|  
|**Rückgängig zu machendes Gesamtprotokoll**|Gesamtanzahl an Kilobytes von Protokollen, die rückgängig zu machen sind.|Sekundäres Replikat|  
|**Transaktionsverzögerung**|Verzögerung beim Warten auf die Bestätigung eines nicht abgeschlossenen Commits für alle aktuellen Transaktionen in Millisekunden. Dividieren Sie den Wert durch *Gespiegelte Schreibtransaktionen/Sekunde*, um die *Durchschnittliche Transaktionsverzögerung* zu erhalten. Weitere Informationen finden Sie unter [SQL Server 2012 AlwaysOn – Teil 12 – Leistungsaspekte und Lesitungsüberwachung II](/archive/blogs/saponsqlserver/sql-server-2012-alwayson-part-12-performance-aspects-and-performance-monitoring-ii).|Primäres Replikat|  
  
## <a name="see-also"></a>Weitere Informationen
  
 [Überwachen der Ressourcenverwendung &#40;Systemmonitor&#41;](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)   
 [SQL Server, Verfügbarkeitsreplikat](../../relational-databases/performance-monitor/sql-server-availability-replica.md)   
 [SQL Server, Datenbanken-Objekt](../../relational-databases/performance-monitor/sql-server-databases-object.md)   
 [Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)  
