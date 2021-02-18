---
title: Festlegen globaler Ablaufverfolgungsoptionen
titleSuffix: SQL Server Profiler
description: Hier erfahren Sie, wie Sie globale Ablaufverfolgungsoptionen festlegen, und erhalten Informationen zu den Optionen, die eine bestimmte SQL Server Profiler-Instanz auf alle Ablaufverfolgungen anwenden kann.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 2854608a-c3c7-4eb8-b567-034bfec4b1a9
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 0394038ecd114f30c1a32005e126a4aa20b548a2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345815"
---
# <a name="set-global-trace-options-sql-server-profiler"></a>Festlegen globaler Ablaufverfolgungsoptionen (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

In diesem Thema wird beschrieben, wie Sie die Optionen festlegen können, die für alle Ablaufverfolgungen gelten, die mit einer bestimmten Instanz von [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]erstellt werden.  
  
### <a name="to-set-global-trace-options"></a>So legen Sie globale Ablaufverfolgungsoptionen fest  
  
1.  Klicken Sie im Menü **Extras** auf **Optionen**.  
  
2.  Klicken Sie im Dialogfeld **Allgemeine Optionen** auf **Schriftart auswählen**, um die Anzeigeoptionen zu ändern, und klicken Sie anschließend auf **OK**.  
  
3.  Sie können optional auch **Ablaufverfolgung sofort nach Herstellen der Verbindung starten** auswählen.  
  
4.  Sie können optional auch **Ablaufverfolgungsdefinition bei Änderung der Anbieterversion aktualisieren** auswählen. Diese Option wird empfohlen, und sie ist standardmäßig aktiviert. Wenn diese Option aktiviert ist, wird die Ablaufverfolgungsdefinition automatisch auf die aktuelle Version des Servers aktualisiert, auf dem die Ablaufverfolgung ausgeführt wird.  
  
5.  Sie können optional angeben, wie der Server mit Rolloverdateien umgehen soll:  
  
    -   Wählen Sie **Alle Rolloverdateien nacheinander ohne Eingabeaufforderung laden** aus, damit Rolloverdateien während der Wiedergabe automatisch geladen werden.  
  
    -   Wählen sie **Bestätigung vor dem Laden von Rolloverdateien** aus, um Rolloverdateien während der Wiedergabe steuern zu können.  
  
    -   Wählen Sie **Nachfolgende Rolloverdateien niemals laden** aus, um jeweils nur eine Datei wiederzugeben.  
  
6.  Optional können Sie die Wiedergabeoptionen festlegen:  
  
    -   **Standardanzahl von Wiedergabethreads** wird die Anzahl von Prozessorthreads kontrolliert, die während der Wiedergabe verwendet werden sollen. Durch eine höhere Anzahl von Threads wird die Wiedergabe schneller abgeschlossen. Die Serverleistung wird während der Wiedergabe jedoch beeinträchtigt. Die empfohlene Einstellung lautet **4**. Die folgende Tabelle enthält die verfügbaren Optionen:  
  
        |Wert|BESCHREIBUNG|  
        |-----------|-----------------|  
        |**2**|Minimalwert. Verwenden von zwei Threads für die Wiedergabe.|  
        |**4**|Standardwert.|  
        |**255**|Maximalwert. Durch Einstellung eines Maximalwerts wird die für andere Prozesse verfügbare Leistung eingeschränkt.|  
  
    -   Mit **Standardwartezeit für Systemüberwachung (Sek.)** wird die maximale Zeitspanne in Sekunden angegeben, über die ein Wiedergabethread einen anderen Prozess blockieren kann. In der folgenden Tabelle werden die einzelnen Werten näher erläutert.  
  
        |Wert|BESCHREIBUNG|  
        |-----------|-----------------|  
        |**0**|Minimalwert. Die Einstellung **0** bedeutet, dass [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] einen blockierenden Prozess in keinem Fall beendet.|  
        |**3600**|Standardwert. Lässt blockierende Prozesse zu, die nicht länger als **3600** Sekunden oder eine Stunde dauern.|  
        |**86400**|Maximalwert. Lässt blockierende Prozesse zu, die nicht länger als **86400** Sekunden oder einen Tag dauern.|  
  
    -   Mit **Standardabrufintervall für Systemüberwachung (Sek.)** wird die Frequenz festgelegt, mit der Wiedergabethreads für blockierende Prozesse abgerufen werden. In der folgenden Tabelle werden die einzelnen Werten näher erläutert.  
  
        |Wert|BESCHREIBUNG|  
        |-----------|-----------------|  
        |**1**|Minimalwert. Eine Einstellung von **1** bedeutet, dass [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] blockierende Prozesse einmal pro Sekunde abruft.|  
        |**60**|Standardwert. Abrufen von blockierenden Prozessen einmal pro Minute.|  
        |**86400**|Maximalwert. Abrufen von blockierenden Prozessen einmal pro **86400** Sekunden, oder einmal pro Tag.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Festlegen der Standardeinstellungen für die Ablaufverfolgungsanzeige &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/set-trace-display-defaults-sql-server-profiler.md)   
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)  
  
  
