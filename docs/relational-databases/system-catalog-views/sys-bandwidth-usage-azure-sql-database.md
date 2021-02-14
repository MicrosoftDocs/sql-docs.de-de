---
description: sys.bandwidth_usage (Azure SQL Database)
title: sys.bandwidth_usage (Azure SQL-Datenbank) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/28/2019
ms.service: sql-database
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- bandwidth_usage
- sys.bandwidth_usage
- bandwidth_usage_TSQL
- sys.bandwidth_usage_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.bandwidth_usage
- bandwidth_usage
ms.assetid: 43ed8435-f059-4907-b5c0-193a258b394a
author: julieMSFT
ms.author: jrasnick
monikerRange: = azuresqldb-current
ms.openlocfilehash: fcf2ac0f0336a7567987d056d0a568d6892b5d1e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338907"
---
# <a name="sysbandwidth_usage-azure-sql-database"></a>sys.bandwidth_usage (Azure SQL Database)

[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

> [!NOTE]
> Dies gilt nur für [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] v11. * *  
  
 Gibt Informationen zur Netzwerkbandbreite zurück, die von den einzelnen Datenbanken auf einem **[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] v11-Datenbankserver** verwendet wird. Jede Zeile, die für eine bestimmte Datenbank zurückgegeben wird, gibt eine einzelne Richtung und eine Verwendungsklasse über eine Stunde hinweg an.  
  
 **Dies ist in einer veraltet [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] .**  
  
 Die **sys.bandwidth_usage** Sicht enthält die folgenden Spalten.  
  
|Spaltenname|Beschreibung|  
|-----------------|-----------------|  
|**time**|Die Stunde, als die Bandbreite verwendet wurde. Die Zeilen in dieser Sicht enthalten stündliche Angaben. Beispielsweise bedeutet 2009-09-19 02:00:00.000, dass die Bandbreite am 19. September 2009 zwischen 2:00 Uhr und 3:00 Uhr verwendet wurde.|  
|**database_name**|Der Name der Datenbank, die Bandbreite verwendet hat.|  
|**direction**|Der Typ der Bandbreite, der verwendet wurde. Dies kann eine der folgenden Optionen sein:<br /><br /> Input: Daten, die in den verschoben werden [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] .<br /><br /> Ausgang: Daten, die aus der verschoben werden [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] .|  
|**class**|Die Klasse der Bandbreite, die verwendet wurde. Dies kann eine der folgenden Optionen sein:<br />Intern: Daten, die innerhalb der Azure-Plattform verschoben werden.<br />Extern: Daten, die aus der Azure-Plattform heraus verschoben werden.<br /><br /> Diese Klasse wird nur zurückgegeben, wenn die Datenbank in einer kontinuierlichen Kopienbeziehung zwischen Regionen ([!INCLUDE[ssGeoDR](../../includes/ssgeodr-md.md)]) ist. Wenn eine bestimmte Datenbank nicht an einer fortlaufenden Kopier Beziehung teilnimmt, werden keine Interlink-Zeilen zurückgegeben. Weitere Informationen finden Sie im Abschnitt "Hinweise" weiter unten in diesem Thema.|  
|**time_period**|Der Zeitraum, in dem die Verwendung aufgetreten ist, ist entweder Peak oder OffPeak. Die Spitzenzeit basiert auf der Region, in der der Server erstellt wurde. Wenn ein -Server beispielsweise in der Region "US_Northwest" erstellt wurde, liegt die Spitzenzeit laut Definition zwischen 10:00 Uhr und 18:00 Uhr PST.|  
|**quantity**|Die Menge der verwendeten Bandbreite in KB.|  
  
## <a name="permissions"></a>Berechtigungen

 Diese Ansicht ist nur in der **Master** -Datenbank für den Prinzipal Anmelde Namen auf Serverebene verfügbar.  
  
## <a name="remarks"></a>Bemerkungen  
  
### <a name="external-and-internal-classes"></a>Externe und interne Klassen

 Für jede Datenbank, die zu einem bestimmten Zeitpunkt verwendet wird, gibt die **sys.bandwidth_usage** Sicht Zeilen zurück, die die Klasse und die Richtung der Bandbreiten Verwendung anzeigen. Das folgende Beispiel zeigt Daten, die für eine bestimmte Datenbank verfügbar gemacht werden können. In diesem Beispiel ist der Zeitpunkt 2012-04-21 17:00:00, der im Zeitraum mit Spitzenwerten liegt. Der Datenbankname ist Db1. In diesem Beispiel hat **sys.bandwidth_usage** eine Zeile für alle vier Kombinationen der Eingangs-und Ausgangs Richtung sowie externer und interner Klassen wie folgt zurückgegeben:  
  
|time|database_name|direction|class|time_period|quantity|  
|----------|--------------------|---------------|-----------|------------------|--------------|  
|2012-04-21 17:00:00|Db1|Eingehende Daten|Extern|Peak|66|  
|2012-04-21 17:00:00|Db1|Ausgehende Daten|Extern|Peak|741|  
|2012-04-21 17:00:00|Db1|Eingehende Daten|Intern|Peak|1052|  
|2012-04-21 17:00:00|Db1|Ausgehende Daten|Intern|Peak|3525|  
  
### <a name="interpreting-data-direction-for-ssgeodr"></a>Interpretieren der Datenrichtung für [!INCLUDE[ssGeoDR](../../includes/ssgeodr-md.md)]

 Bei [!INCLUDE[ssGeoDR](../../includes/ssgeodr-md.md)] sind die Bandbreitenverwendungsdaten in der logischen Masterdatenbank auf beiden Seiten einer kontinuierlichen Kopienbeziehung sichtbar. Daher müssen Sie die Indikatoren für die Eingangs-und Ausgangs Richtung aus der Perspektive der Datenbanken interpretieren, die Sie Abfragen. Betrachten Sie beispielsweise einen Replikationsdatenstrom, der 1 MB Daten vom Quell- auf den Zielserver überträgt. In diesem Fall wird auf dem Quell Server die 1-MB-Anzahl in Bezug auf die Gesamtanzahl der gesendeten Daten und auf dem Zielserver als empfangene Daten aufgezeichnet.  
  
> [!NOTE]  
> Die vom Quell- auf den Zielserver übertragenen Massendaten werden in Richtung des Benutzerdatendatenflusses übertragen. Manche Daten müssen jedoch in die andere Richtung übertragen werden.  
