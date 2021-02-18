---
title: Organisieren der in einer Ablaufverfolgung angezeigten Spalten
titleSuffix: SQL Server Profiler
description: In diesem Artikel erfahren Sie, wie Sie die Analyse der Ausgabe der SQL Server Profiler-Ablaufverfolgung durch Gruppierung und Aggregierung von Ereignissen beim Definieren oder Anzeigen einer Ablaufverfolgung vereinfachen können.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 6b923f94-0eb1-467e-82f6-ceed43f77017
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 2727aa7ed5e95881e21eddb73a94ddef97795c08
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353408"
---
# <a name="organize-columns-displayed-in-a-trace-sql-server-profiler"></a>Organisieren von in einer Ablaufverfolgung angezeigten Spalten (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

 Sie können Datenspalten in einer Ablaufverfolgung gruppieren, wenn Sie eine Ablaufverfolgung definieren oder wenn Sie in der Ablaufverfolgungstabelle oder im Dialogfeld **Eigenschaften der Ablaufverfolgungsdatei** die Option **Spalten organisieren** auswählen. Durch das Gruppieren der Datenspalten können Sie die [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] -Ablaufverfolgungsausgabe besser analysieren. Weitere Informationen finden Sie unter [Anzeigen und Analysieren von Ablaufverfolgungen mit SQL Server Profiler](../../tools/sql-server-profiler/view-and-analyze-traces-with-sql-server-profiler.md).  
  
 **Spalten organisieren** ermöglicht Ihnen das Gruppieren der Ablaufverfolgungsereignisse oder das Gruppieren und Aggregieren dieser Ereignisse anhand der von Ihnen ausgewählten Datenspalten.  
  
-   Wählen Sie mehrere Datenspalten zum Gruppieren aus, um nur Ablaufverfolgungsereignisse zu gruppieren. Wenn Sie mehrere Datenspalten zum Gruppieren auswählen, zeigt das Ablaufverfolgungsfenster Ereignisse gruppiert nach den Werten in den Datenspalten an, die Sie zum Gruppieren ausgewählt haben. Das folgende Beispiel zeigt, wie das Raster für das Ablaufverfolgungsfenster aussieht, wenn Sie die Datenspalten **Duration** und **StartTime** zum Gruppieren auswählen. Beachten Sie, dass die Werte der **Duration** -Spalte in aufsteigender Reihenfolge angezeigt werden, gefolgt von den **StartTime** -Werten.  
  
|Duration|StartTime|EventClass|ClientProcessID|  
|--------------|---------------|----------------|---------------------|  
||12/12/2006 3:16:43 PM|SQL:StmtStarting|2124|  
|0|12/12/2006 5:39:23 PM|Audit Login|648|  
|1|12/12/2006 5:24:44 PM|SQL:StmtStarting|2124|  
|25|12/12/2006 5:24:44 PM|SQL:StmtCompleted|648|  
  
-   Wählen Sie nur eine Spalte zum Gruppieren aus, um Ablaufverfolgungsereignisse zu gruppieren und zu aggregieren. Wenn Sie nur eine Datenspalte zum Gruppieren auswählen, zeigt das Ablaufverfolgungsfenster Ereignisse gruppiert nach den Werten in dieser Datenspalte an, und alle Ereignisse darunter werden reduziert. Ein Pluszeichen ( **+** ) wird links neben dem Ereignis in der zum Gruppieren ausgewählten Datenspalte angezeigt, und die Anzahl der darunter reduzierten Ereignisse wird in Klammern rechts neben dem Ereignis angezeigt. Das folgende Beispiel zeigt, wie das Raster für das Ablaufverfolgungsfenster aussieht, wenn Sie nur die **EventClass** -Datenspalte zum Gruppieren auswählen. Beachten Sie, dass alle Ereignisse unter der **EventClass** -Datenspalte organisiert sind. Klicken Sie zum Anzeigen aller Ereignisse auf das Pluszeichen, um alle Ereignisklassen dieses Typs zu erweitern und anzuzeigen.  
  
|EventClass|StartTime|Duration|ClientProcessID|  
|----------------|---------------|--------------|---------------------|  
|+ ExistingConnection (6)||||  
|+ SQL:BatchStarting (25)||||  
|+ SQL:StmtCompleted (11)||||  
|+ SQL:SmtStarting (21)||||  
  
### <a name="to-group-data-columns-displayed-in-a-trace"></a>So gruppieren Sie in einer Ablaufverfolgung angezeigte Datenspalten  
  
1.  Öffnen Sie eine vorhandene Ablaufverfolgungsdatei oder -tabelle.  
  
2.  Klicken Sie im Menü **Datei** auf **Eigenschaften**.  
  
3.  Klicken Sie im Dialogfeld **Eigenschaften der Ablaufverfolgungsdatei** oder **Eigenschaften der Ablaufverfolgungstabelle** auf die Registerkarte **Ereignisauswahl** .  
  
4.  Klicken Sie auf der Registerkarte **Ereignisauswahl** auf **Spalten organisieren**.  
  
5.  Wählen Sie im Dialogfeld **Spalten organisieren** die Spalten aus, die in einer Gruppe angezeigt werden sollen, und klicken Sie auf **Nach oben** , um sie in die Liste **Gruppen** zu verschieben. Wenn Sie alle gewünschten Spalten in die Liste **Gruppen** verschoben haben, können Sie die Schaltflächen **Nach oben** und **Nach unten** verwenden, um ihre Reihenfolge zu ändern.  
  
     Das Verschieben der Datenspaltennamen in die Liste **Gruppen** hat zur Folge, dass die angezeigte Ablaufverfolgung zunächst nach den Werten in der obersten Datenspalte in der Liste **Gruppen** organisiert wird, dann nach der zweiten Datenspalte in der Liste **Gruppen** usw.  
  
6.  Klicken Sie im Dialogfeld **Spalten organisieren** auf **OK** , und klicken Sie dann im Dialogfeld **Eigenschaften der Ablaufverfolgungstabelle** oder **Eigenschaften der Ablaufverfolgungsdatei** auf **OK** .  
  
     Nachdem Sie im Dialogfeld **Eigenschaften der Ablaufverfolgungstabelle** oder **Eigenschaften der Ablaufverfolgungsdatei** auf **OK** geklickt haben, werden die Datenspalten in der angezeigten Ablaufverfolgung neu organisiert. Die Datenspalte, die Sie an die oberste Position in der Liste **Gruppen** verschoben haben, befindet sich an erster Stelle in der Ablaufverfolgungsanzeige, wenn Sie das Raster von links nach rechts lesen. Die Zeilen in der Ablaufverfolgung sind in aufsteigender Reihenfolge nach den Werten organisiert, die in den Datenspalten enthalten sind, die Sie in die Liste **Gruppen** aufgenommen haben. Die zum Gruppieren ausgewählten Spalten bleiben fest in der Anzeige, Sie können jedoch einen Bildlauf nach rechts oder links durchführen, um die anderen Spalten anzuzeigen.  
  
7.  Klicken Sie zum Aufheben der Gruppierung der angezeigten Ablaufverfolgungsdaten im Menü **Ansicht** auf **Gruppierte Ansicht** , um die Auswahl aufzuheben. Wenn Sie zur gruppierten Ansicht zurückkehren möchten, klicken Sie im Menü **Ansicht** erneut auf **Gruppierte Ansicht** , um den Befehl wieder auszuwählen.  
  
### <a name="to-group-and-aggregate-data-columns-in-a-trace"></a>So gruppieren und aggregieren Sie Datenspalten in einer Ablaufverfolgung  
  
1.  Öffnen Sie eine vorhandene Ablaufverfolgungsdatei oder -tabelle.  
  
2.  Klicken Sie im Menü **Datei** auf **Eigenschaften**.  
  
3.  Klicken Sie im Dialogfeld **Eigenschaften der Ablaufverfolgungsdatei** oder **Eigenschaften der Ablaufverfolgungstabelle** auf die Registerkarte **Ereignisauswahl** .  
  
4.  Klicken Sie auf der Registerkarte **Ereignisauswahl** auf **Spalten organisieren**.  
  
5.  Wählen Sie im Dialogfeld **Spalten organisieren** eine Spalte aus, nach der die angezeigten Ablaufverfolgungsereignisse gruppiert und aggregiert werden sollen. Klicken Sie auf **Nach oben** , um den Spaltennamen in die Liste **Gruppen** zu verschieben. Sie können die Schaltflächen **Nach oben** und **Nach unten** verwenden, um die übrigen Spalten unter **Spalten** bei Bedarf neu anzuordnen.  
  
6.  Klicken Sie im Dialogfeld **Spalten organisieren** auf **OK** , und klicken Sie dann im Dialogfeld **Eigenschaften der Ablaufverfolgungstabelle** oder **Eigenschaften der Ablaufverfolgungsdatei** auf **OK** .  
  
     Nachdem Sie im Dialogfeld **Eigenschaften der Ablaufverfolgungstabelle** oder **Eigenschaften der Ablaufverfolgungsdatei** auf **OK** geklickt haben, werden die Datenspalten in der angezeigten Ablaufverfolgung neu organisiert. Alle anderen Datenspaltenereignisse werden unter der Datenspalte aggregiert, die Sie in die Liste **Gruppen** verschoben haben. Klicken Sie auf das Pluszeichen ( **+** ) links neben dem Ereignis in der für die Aggregation ausgewählten Datenspalte, um es zu erweitern und alle Ereignisse dieses Typs anzuzeigen. Die für die Aggregation ausgewählte Spalte bleibt fest in der Anzeige, Sie können jedoch einen Bildlauf nach rechts oder links durchführen, um die anderen Spalten anzuzeigen.  
  
7.  Um zu einer normalen Ansicht der Ablaufverfolgungsdaten zurückzukehren, klicken Sie im Menü **Ansicht** auf **Aggregierte Ansicht** , sodass die Auswahl aufgehoben wird. Wenn Sie zur aggregierten Ansicht zurückkehren möchten, klicken Sie im Menü **Ansicht** erneut auf **Aggregierte Ansicht** , um den Befehl wieder auszuwählen. Beachten Sie, dass Sie auch im Menü **Ansicht** auf **Gruppierte Ansicht** klicken können, um die gruppierten Ablaufverfolgungsereignisse anzuzeigen, ohne sie zu reduzieren.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Erstellen einer Ablaufverfolgung &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/create-a-trace-sql-server-profiler.md)   
 [Öffnen einer Ablaufverfolgungstabelle &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/open-a-trace-table-sql-server-profiler.md)   
 [Öffnen einer Ablaufverfolgungsdatei &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/open-a-trace-file-sql-server-profiler.md)  
  
  
