---
description: Verbessern des Zugriffs auf Ablaufverfolgungsdaten
title: Verbessern des Zugriffs auf Ablaufverfolgungsdaten | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- Profiler [SQL Server Profiler], space
- SQL Server Profiler, space
- space [SQL Server], SQL Server Profiler
ms.assetid: c260c000-fd53-4831-993f-df6894f3228b
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: f2c85d7c440f32aee7b6b6fb76019fdb2eef2c2b
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/26/2020
ms.locfileid: "88324878"
---
# <a name="improve-access-to-trace-data"></a>Verbessern des Zugriffs auf Ablaufverfolgungsdaten
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] verwendet Leerzeichen im Verzeichnis **temp**, um den Zugriff auf Ablaufverfolgungsdaten zu verbessern. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] erfordert mindestens 10 MB freien Speicherplatz. Wenn der verfügbare Speicherplatz beim Verwenden von [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]weniger als 10 MB beträgt, werden alle [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] -Funktionen beendet.  
  
 Wenn [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] Speicherplatz im **temp** -Verzeichnis verwendet, kann das **temp** -Verzeichnis durch diese Speicherverwendung schnell anwachsen. Um Probleme mit der Dateivergrößerung zu vermeiden, können Sie das **temp** -Verzeichnis auf einem Laufwerk platzieren, das kein Systemlaufwerk ist. Ändern Sie dazu den Wert für die TEMP-Umgebungsvariable.  
  
 Im folgenden Verfahren wird beschrieben, wie der Wert für die TEMP-Umgebungsvariable bei den meisten Microsoft Windows-Betriebssystemen geändert wird. Weitere Informationen zum Festlegen von Umgebungsvariablen finden Sie in der Dokumentation des Windows-Betriebssystems.  
  
### <a name="to-change-the-temp-environment-variable-in-windows-operating-systems"></a>So ändern Sie die TEMP-Umgebungsvariable in Windows-Betriebssystemen  
  
1.  Wählen Sie im Menü **Start** die Option **Systemsteuerung** aus, und klicken Sie dann auf **System**.  
  
2.  Klicken Sie im Dialogfeld **Systemeigenschaften** auf die Registerkarte **Erweitert** , und klicken Sie dann auf **Umgebungsvariablen**.  
  
3.  Führen Sie in der Liste **Systemvariablen** einen Bildlauf nach unten durch, wählen Sie die Zeile für die **TEMP** -Variable aus, und klicken Sie auf **Bearbeiten**.  
  
4.  Geben Sie im Dialogfeld **Systemvariable bearbeiten** den Pfad und den Namen des Laufwerks und des Verzeichnisses für das **temp** -Verzeichnis ein.  
  
5.  Klicken Sie auf **OK** , um die Änderung zu speichern.  
  
## <a name="see-also"></a>Siehe auch  
 [Starten von SQL Server Profiler](../../tools/sql-server-profiler/start-sql-server-profiler.md)   
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)  
  
  
