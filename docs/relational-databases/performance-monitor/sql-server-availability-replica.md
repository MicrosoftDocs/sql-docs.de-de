---
title: SQL Server, Verfügbarkeitsreplikat | Microsoft-Dokumentation
description: Hier lernen Sie das Leistungsobjekt „SQLServer:Availability Replica“ kennen, das Leistungsindikatoren zu Verfügbarkeitsreplikaten in Always On-Verfügbarkeitsgruppen enthält.
ms.custom: ''
ms.date: 08/25/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- performance counters [SQL Server], AlwaysOn Availability Groups
- SQLServer:Availability Replica
- Availability Groups [SQL Server], performance counters
ms.assetid: e402f996-c1fb-484a-b804-45c49972f2e0
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 39df44f69e8a4cd05b5cefda27aefcd20d2100e1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338144"
---
# <a name="sql-server-availability-replica"></a>SQL Server, Verfügbarkeitsreplikat

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Das Leistungsobjekt **SQLServer:Availability Replica** enthält Leistungsindikatoren, die Informationen zu den Verfügbarkeitsreplikaten in Always On-Verfügbarkeitsgruppen in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]bereitstellen. Alle Leistungsindikatoren für Verfügbarkeitsreplikate gelten für das primäre Replikat und auch für die sekundären Replikate, wobei Sende-/Empfangs-Zähler das lokale Replikat wiedergeben. Normalerweise sendet das primäre Replikat die meisten Daten, und die sekundären Replikate empfangen die Daten. Sekundäre Replikate senden jedoch ACKs und weiteren Hintergrunddatenverkehr an die primären Replikate. Auf jedem Verfügbarkeitsreplikat zeigen einige Leistungsindikatoren in Abhängigkeit von der aktuellen Rolle des lokalen Replikats (primär oder sekundär) den Wert 0 an.  
  
|Name des Leistungsindikators|BESCHREIBUNG|  
|------------------|-----------------|  
|**Vom Replikat empfangene Bytes/s**|**In SQL Server 2012 und 2014:** Die tatsächliche Anzahl von Bytes (komprimiert), die pro Sekunde vom Verfügbarkeitsreplikat empfangen werden (synchron oder asynchron). Durch Pingbefehle und Statusupdates wird selbst für Datenbanken ohne Benutzerupdates Netzwerkdatenverkehr generiert. <BR/> <BR/> **SQL Server 2016 und höher:** Die tatsächliche Anzahl empfangener Bytes (komprimiert für asynchron, nicht komprimiert für synchron) aus dem Verfügbarkeitsreplikat pro Sekunde.|  
|**An das Replikat gesendete Bytes/s**|**In SQL Server 2012 und 2014:** Tatsächliche Anzahl von Bytes (komprimiert), die pro Sekunde über das Netzwerk an das Remoteverfügbarkeitsreplikat gesendet werden (synchron oder asynchron). Die Komprimierung ist standardmäßig für synchrone und asynchrone Replikate aktiviert. <BR/> <BR/> **In SQL Server 2016 und höher:** Anzahl von Bytes, die pro Sekunde an das Remoteverfügbarkeitsreplikat gesendet werden. Vor der Komprimierung für asynchrone Replikate. (Tatsächliche Anzahl von Bytes für das Sync-Replikat ohne Komprimierung)|  
|**An den Transport gesendete Bytes/s**|**In SQL Server 2012 und 2014:** Tatsächliche Anzahl von Bytes, die pro Sekunde (komprimiert) über das Netzwerk an das Remoteverfügbarkeitsreplikat gesendet werden (synchron oder asynchron). Die Komprimierung ist standardmäßig für synchrone und asynchrone Replikate aktiviert. <BR/> <BR/> **In SQL Server 2016 und höher:** Anzahl von Bytes, die pro Sekunde an das Remoteverfügbarkeitsreplikat gesendet werden (vor Komprimierung für das asynchrone Replikat). (Tatsächliche Anzahl von Bytes für das synchrone Replikat ohne Komprimierung)|  
|**Flussteuerungsdauer (ms/sek)**|Zeit in Millisekunden, die die Protokolldatenstrom-Meldungen in der letzten Sekunde auf Sendedatenfluss-Steuerung gewartet haben.|  
|**Flusssteuerung/s**|Häufigkeit der Initiierungsvorgänge für die Flusssteuerung in der letzten Sekunde. **Flussteuerungsdauer (ms/sek)** geteilt durch **Flusssteuerung/s** entspricht der durchschnittlichen Zeit pro Wartevorgang.|  
|**Replikat-Empfangsvorgänge/s**|Anzahl von Always On-Nachrichten, die pro Sekunde vom Replikat empfangen werden.|  
|**Erneut gesendete Nachrichten/s**|Anzahl von Always On-Nachrichten, die in der letzten Sekunde erneut gesendet wurden.|  
|**Replikat-Sendevorgänge/s**|Anzahl von Always On-Nachrichten, die pro Sekunde an dieses Verfügbarkeitsreplikat gesendet werden.|  
|**Transport-Sendevorgänge/s**|Tatsächliche Anzahl von Always On-Nachrichten, die pro Sekunde über das Netzwerk an das Remoteverfügbarkeitsreplikat gesendet werden. Auf dem primären Replikat ist dies die Anzahl von an das sekundäre Replikat gesendeten Nachrichten. Auf dem sekundären Replikat ist dies die Anzahl von an das primäre Replikat gesendeten Nachrichten.|  
  
## <a name="see-also"></a>Weitere Informationen 
 
 [Überwachen der Ressourcenverwendung &#40;Systemmonitor&#41;](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)   
 [SQL Server, Datenbankreplikat](../../relational-databases/performance-monitor/sql-server-database-replica.md)   
 [Always On-Verfügbarkeitsgruppen (SQL Server)](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)  
  
  
