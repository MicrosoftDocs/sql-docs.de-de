---
title: Separates Speichern von Showplan XML-Ereignissen
description: Erfahren Sie, wie Sie Showplan XML-Ereignisse, die in Ablaufverfolgungen erfasst wurden, mit SQL Server Profiler in gesonderten Dateien speichern. Öffnen Sie die Dateien in SQL Server Management Studio.
titleSuffix: SQL Server Profiler
ms.custom: seo-dt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Showplan XML events
- saving Showplan XML events
- events [SQL Server], Showplan XML
ms.assetid: 33320a7a-36e8-401c-876d-5b82c49abd85
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 34c79b9f3dad13a8927296d424d827eb693ad1dd
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505014"
---
# <a name="save-showplan-xml-events-separately-sql-server-profiler"></a>Separates Speichern von Showplan XML-Ereignissen (SQL Server Profiler)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In diesem Thema wird beschrieben, wie **Showplan XML** -Ereignisse, die in Ablaufverfolgungen erfasst sind, mithilfe von [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]in separaten .SQLPlan-Dateien gespeichert werden können. Sie können die **Showplan XML**-Ereignisdateien in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] öffnen, um den grafischen Ausführungsplan für jedes Ereignis anzuzeigen.  
  
## <a name="save-showplan-xml-events-separately"></a>Separates Speichern von Showplan XML-Ereignissen  
  
1. Wählen Sie im Menü **Datei** die Option **Neue Ablaufverfolgung** aus, und stellen Sie dann eine Verbindung mit einer Instanz von SQL Server her.  
  
     Das Dialogfeld **Ablaufverfolgungseigenschaften** wird angezeigt.  
  
    > [!NOTE]  
    >  Wenn Sie **Ablaufverfolgung sofort nach dem Herstellen der Verbindung starten** auswählen, wird das Dialogfeld **Ablaufverfolgungseigenschaften** nicht angezeigt. Stattdessen beginnt die Ablaufverfolgung. Um diese Einstellung zu deaktivieren, wählen Sie im Menü **Extras** **Optionen** aus und deaktivieren das Kontrollkästchen **Ablaufverfolgung sofort nach dem Herstellen der Verbindung starten**.  
  
2. Geben Sie im Dialogfeld **Ablaufverfolgungseigenschaften** im Feld **Ablaufverfolgungsname** einen Namen für die Ablaufverfolgung ein.  
  
3. Wählen Sie in der Liste **Vorlage verwenden** eine Nachverfolgungsvorlage aus, auf der die Nachverfolgung basieren soll. Wenn Sie keine Vorlage verwenden möchten, wählen Sie **Leer** aus.  
  
4. Führen Sie eines der folgenden Verfahren aus:  
  
    -   Aktivieren Sie das Kontrollkästchen **In Datei speichern**, um die Ablaufverfolgung in einer Datei aufzuzeichnen. Geben Sie einen Wert für **Maximale Dateigröße festlegen** an. 
    
        Optional aktivieren Sie die Kontrollkästchen **Dateirollover aktivieren** und **Ablaufverfolgungsdaten von Serverprozessen** .  
  
    -   Aktivieren Sie das Kontrollkästchen **In Tabelle speichern**, um die Ablaufverfolgung in einer Datenbanktabelle aufzuzeichnen. 
    
        Wählen Sie optional **Maximale Zeilenzahl festlegen** aus, und geben Sie einen Wert an.  
  
5. Aktivieren Sie optional das Kontrollkästchen **Beendigungszeit für Ablaufverfolgung aktivieren** , und geben Sie das Datum und die Uhrzeit zum Beenden der Ablaufverfolgung an. 
  
6. Wählen Sie die Registerkarte **Ereignisauswahl** aus.  
  
7. Erweitern Sie in der Datenspalte **Ereignisse** die Ereigniskategorie **Leistung**, und aktivieren Sie dann das Kontrollkästchen **Showplan XML**. Wenn die **Performance**-Ereigniskategorie nicht verfügbar ist, wählen Sie **Alle Ereignisse anzeigen** aus, um sie anzuzeigen.  
  
     Die Registerkarte **Ereignisextraktionseinstellungen** wird dem Dialogfeld **Ablaufverfolgungseigenschaften** hinzugefügt.  
  
8. Wählen Sie auf der Registerkarte **Ereignisextraktionseinstellungen** die Option **XML-Showplanereignisse separat speichern** aus.  
  
9. Geben Sie in das Dialogfeld **Speichern unter** den Namen der Datei ein, in die die **Showplan XML** -Ereignisse gespeichert werden sollen.  
  
10. Wählen Sie **Alle XML-Showplanbatches in einer einzelnen Datei** aus, um alle **Showplan XML**-Ereignisse in einer einzelnen XML-Datei zu speichern. Wählen Sie alternativ **Jeder XML-Showplanbatch in einer eigenen Datei** aus, um für jedes **Showplan XML**-Ereignis eine neue XML-Datei zu erstellen.  
  
11. Um die **Showplan XML**-Ereignisdatei in SQL Server Management Studio anzeigen zu lassen, zeigen Sie im Menü **Datei** auf **Öffnen**, und wählen Sie **Datei** aus. Navigieren Sie zu dem Verzeichnis, in dem Sie die **Showplan XML**-Ereignisdatei(en) gespeichert hatten, wählen Sie eine Datei aus, und öffnen Sie diese. **Showplan XML** -Ereignisdateien besitzen die Dateierweiterung .SQLPlan.  

## <a name="see-also"></a>Weitere Informationen  
 [Analysieren von Abfragen mit Showplan-Ergebnissen in SQL Server Profiler](../../tools/sql-server-profiler/analyze-queries-with-showplan-results-in-sql-server-profiler.md)  
  
  
