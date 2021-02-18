---
title: SQL Server Profiler-Dialogfelder
titleSuffix: SQL Server Profiler
description: In diesem Artikel erhalten Sie Informationen zu Befehlen und Einstellungen, die im SQL Server Profiler zur Verwendung in Ablaufverfolgungen, Vorlagen, Dateien, Tabellen, Leistungsindikatoren und Replays zur Verfügung stehen.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: reference
ms.assetid: e57b9160-4b78-4353-abb2-bfdbdf523d7a
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 07/07/2017
ms.openlocfilehash: 06711f4975bb84163f58857759c5470f004c52e9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338629"
---
# <a name="sql-server-profiler-dialog-boxes"></a>SQL Server Profiler-Dialogfelder

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Microsoft [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] ist ein Tool, das [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Ereignisse von einem Server aufzeichnet. Die Ereignisse werden in einer Ablaufverfolgungsdatei gespeichert, die später analysiert oder beim Versuch, ein Problem zu diagnostizieren, zur Wiedergabe einer bestimmten Reihe von Schritten verwendet werden kann. In der folgenden Tabelle finden Sie die Befehle und Einstellungen, die in den Dialogfeldern des [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] verfügbar sind.  
## <a name="trace-properties"></a>Ablaufverfolgungseigenschaften
### <a name="general-tab"></a>Registerkarte "Allgemein"
Mithilfe der Registerkarte **Allgemein** im Dialogfeld **Ablaufverfolgungseigenschaften** können Sie die Eigenschaften einer Ablaufverfolgung anzeigen und festlegen.  

|Element|BESCHREIBUNG
|---|---
|**Ablaufverfolgungsname** |Gibt den Namen der Ablaufverfolgung an.  
|**Name des Ablaufverfolgungsanbieters**|Zeigt den Namen der Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] an, für die die Ablaufverfolgung ausgeführt wird. Dieses Feld wird automatisch mit dem Namen des Servers ausgefüllt, den Sie bei der Verbindung angegeben haben. Um den Namen des Ablaufverfolgungsanbieters zu ändern, klicken Sie auf **Abbrechen** , um das Dialogfeld zu schließen, und starten Sie eine neue Ablaufverfolgung.  
|**Typ des Ablaufverfolgungsanbieters**|Zeigt den Servertyp an, der die Ablaufverfolgung bereitstellt. Das Feld **Typ des Ablaufverfolgungsanbieters** wird automatisch mithilfe der Datei mit den Ablaufverfolgungsdefinitionen ausgefüllt. Sie können diesen Eintrag nicht ändern.  
|**version**|Zeigt die Version des Servers an, der die Ablaufverfolgung bereitstellt. Das Feld **Version** wird automatisch mithilfe der Datei mit den Ablaufverfolgungsdefinitionen ausgefüllt. Sie können diesen Eintrag nicht ändern.  
|**Vorlage verwenden**|Wählt eine Vorlage aus dem Vorlagenverzeichnis aus. Das Verzeichnis enthält die Standardvorlagen und alle benutzerdefinierten Vorlagen, die für den aktuellen Typ des Ablaufverfolgungsanbieters erstellt wurden.  
|**In Datei speichern**|Zeichnet die Ablaufverfolgungsdaten in einer TRC-Datei auf. Das Speichern von Ablaufverfolgungsdaten ist für die spätere Überprüfung und Analyse nützlich.  
|**Maximale Dateigröße festlegen (MB)**|Wenn Sie die Ablaufverfolgungsdaten in einer Datei speichern, müssen Sie die maximale Größe der Ablaufverfolgungsdatei angeben. Der Standardwert ist 5 MB. Die maximale Größe ist nur durch das Dateisystem (NTFS, FAT) begrenzt, in dem die Datei gespeichert wird.  
|**Speichern unter**|Nachdem Sie die Option zum Speichern ausgewählt haben, können Sie dieses Symbol verwenden, um den Dateinamen zu ändern.  
|**Dateirollover aktivieren**|Wählen Sie diese Option, um das Erstellen zusätzlicher Dateien für Ablaufverfolgungsdaten zu ermöglichen, wenn die maximale Dateigröße erreicht wird. Jeder neue Dateiname besteht aus dem ursprünglichen Namen der TRC-Datei mit einer aufsteigenden Nummer. Wenn z. B. für die Datei **NewTrace.trc** die maximale Dateigröße erreicht ist, wird sie geschlossen und eine neue Datei mit dem Namen **NewTrace_1.trc** geöffnet, gefolgt von der Datei **NewTrace_2.trc** usw. Der Dateirollover wird standardmäßig aktiviert, wenn Sie eine Ablaufverfolgung in eine Datei speichern.  
|**Ablaufverfolgungsdaten von Serverprozessen**|Gibt an, dass der Server, der die Ablaufverfolgung ausführt, die Ablaufverfolgungsdaten verarbeiten soll. Mit dieser Option kann der Verarbeitungsaufwand reduziert werden, der durch die Ablaufverfolgung verursacht wird. Ist sie aktiviert, werden keine Ereignisse ausgelassen, auch nicht unter Belastungsbedingungen. Ist dieses Kontrollkästchen deaktiviert, übernimmt SQL Server Profiler die Verarbeitung. In diesem Fall besteht die Möglichkeit, dass einige Ereignisse bei hoher Belastung nicht verfolgt werden.  
|**In Tabelle speichern**|Zeichnet die Ablaufverfolgungsdaten in einer Datenbanktabelle auf. Das Speichern von Ablaufverfolgungsdaten ist für die spätere Überprüfung und Analyse nützlich. Durch das Speichern von Ablaufverfolgungsdaten in einer Tabelle kann es jedoch zu einem hohen Verarbeitungsaufwand auf dem Server kommen, auf dem die Ablaufverfolgung gespeichert wird. Vermeiden Sie daher wenn möglich, die Ablaufverfolgungstabelle auf demselben Server zu speichern, für den die Ablaufverfolgung ausgeführt wird.  
|**Zieltabelle**|Wenn Sie die Option zum Speichern der Ablaufverfolgungsdaten in einer Datenbanktabelle ausgewählt haben, können Sie auf dieses Symbol klicken, um den Tabellennamen zu ändern.  
| **Maximale Zeilenzahl festlegen (in Tausend)**|Gibt die Höchstanzahl der Zeilen an, in denen Daten gespeichert werden sollen. Die Standardeinstellung ist 1000 Zeilen. 
|**Beendigungszeit für Ablaufverfolgung aktivieren**|Legt das Datum und die Uhrzeit für das Ende und das Schließen der Ablaufverfolgung fest. 

### <a name="events-selection-tab"></a>Registerkarte „Ereignisauswahl“
Mithilfe der Registerkarte **Ereignisauswahl** im Dialogfeld **Ablaufverfolgungseigenschaften** können Sie die Ablaufverfolgung von Ereignissen oder Datenspalten anzeigen und angeben.  

|Element|BESCHREIBUNG
|---|---
|Spalte **Ereignisse**|Gibt die Ereignisse an, für die eine Ablaufverfolgung ausgeführt wird, indem das betreffende Kontrollkästchen in der Ereignisspalte aktiviert bzw. deaktiviert werden. Die **Ereignisse** sind nach Ereigniskategorien angeordnet. In der Vorlage angegebene Ereignisklassen werden automatisch ausgewählt. Weitere Informationen finden Sie unter [Ereignisklassen in SQL Server – Referenz](../../relational-databases/event-classes/sql-server-event-class-reference.md).  
|Datenspalten|Gibt die Datenspalten an, für die eine Ablaufverfolgung ausgeführt wird, indem das Kontrollkästchen für das Ereignis und die benötigte Datenspalte aktiviert wird. Alle relevanten Ereignisspalten sind für die in der Ablaufverfolgung enthaltenen Ereignisse standardmäßig aktiviert.  
|Filter|Sie können Filter festlegen, indem Sie auf die Spaltenüberschrift klicken und die Filterkriterien eingeben. Gefilterte Datenspalten sind im Dialogfeld **Filter bearbeiten** links neben der Spaltenbezeichnung durch ein Filtersymbol gekennzeichnet. Weitere Informationen finden Sie unter [SQL Server Profiler – Filter bearbeiten]().  
|**Alle Ereignisse anzeigen**|Zeigt alle verfügbaren Ereignisse an. Standardmäßig werden nur Zeilen im Raster **Ereignisauswahl** angezeigt, die ausgewählt sind. Deaktivieren Sie dieses Kontrollkästchen, um alle nicht ausgewählten Ereignisse im Raster **Ereignisauswahl** auszublenden.  
|**Alle Spalten anzeigen**|Zeigt alle verfügbaren Datenspalten an. Standardmäßig werden nur ausgewählte Datenspalten angezeigt. Deaktivieren Sie dieses Kontrollkästchen, um alle nicht ausgewählten Datenspalten im Raster **Ereignisauswahl** auszublenden.  
|**Spaltenfilter**|Öffnet das Dialogfeld **Filter bearbeiten** . Mithilfe dieses Dialogfelds können Sie Datenspaltenfilter bearbeiten.  
|**Spalten organisieren**|Ändert die Reihenfolge der Spalten in der Ablaufverfolgung und gruppiert die Ergebnisse in einer oder mehreren Spalten.  

## <a name="trace-template-properties"></a>Eigenschaften der Ablaufverfolgungsvorlage 
### <a name="new-general-tab"></a>Neu (die Registerkarte „Allgemein“)
Mithilfe der Registerkarte **Allgemein** des Dialogfelds **Eigenschaften der Ablaufverfolgungsvorlage** können Sie neue Ablaufverfolgungsvorlagen mithilfe der folgenden Optionen erstellen. Zeigen Sie im Menü **Datei** von [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] auf **Vorlagen**, und klicken Sie dann auf **Neu**, um dieses Dialogfeld zu öffnen.

|Element|BESCHREIBUNG
|---|---
|**Servertyp auswählen**|Geben Sie den Servertyp an, für den die Vorlage verwendet wird.  
|**Name der neuen Vorlage**|Geben Sie einen aussagekräftigen Namen für die Vorlage an.  
|**Neue Vorlage auf vorhandener basieren**|Verwenden Sie eine Vorlage aus der Liste als Basis für diese Vorlage. Alle ausgewählten Ereignisse, Datenspalten und Filter stimmen anfangs mit denen in der vorhandenen Vorlage überein und können dann nach Bedarf geändert werden.  
|**Als Standardvorlage für den ausgewählten Servertyp verwenden**|Verwenden Sie diese Vorlage standardmäßig für alle Ablaufverfolgungen, die für diesen Servertyp erstellt werden.  

### <a name="edit-general-tab"></a>Bearbeiten (die Registerkarte „Allgemein“)
 Mithilfe der Registerkarte **Allgemein** des Dialogfelds **Eigenschaften der Ablaufverfolgungsvorlage** können Sie vorhandene Ablaufverfolgungsvorlagen mithilfe der folgenden Optionen anzeigen oder bearbeiten. Sie können dieses Dialogfeld aufrufen, indem Sie erst im Menü **Datei** von [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] auf **Vorlagen** und dann auf **Vorlage bearbeiten** klicken.  

|Element|BESCHREIBUNG
|---|---
|**Servertyp auswählen**|Geben Sie den Servertyp an, für den die Vorlage verwendet wird.  
|**Vorlagennamen auswählen**|Wählen Sie die Vorlage aus, die Sie bearbeiten möchten.  
|**Als Standardvorlage für den ausgewählten Servertyp verwenden**|Verwenden Sie diese Vorlage standardmäßig für alle Ablaufverfolgungen, die für diesen Servertyp erstellt werden.  

### <a name="events-selection-tab"></a>Registerkarte „Ereignisauswahl“
Die Registerkarte **Ereignisauswahl** im Dialogfeld **Eigenschaften der Ablaufverfolgungsvorlage** wird zum Anzeigen, Bearbeiten und Angeben von Ereignisklassen und Datenspalten verwendet, die in eine [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] -Ablaufverfolgungsvorlage eingeschlossen werden sollen.  

|Element|BESCHREIBUNG
|---|---
|Spalte **Ereignisse**|Geben Sie die Ereignisse an, für die eine Ablaufverfolgung ausgeführt werden soll, indem Sie das betreffende Kontrollkästchen in der Ereignisspalte aktivieren bzw. deaktivieren. Die Ereignisse sind nach Ereigniskategorien angeordnet. Wenn Sie auf der Registerkarte **Allgemein** die Option **Neue Vorlage auf vorhandener basieren** auswählen, werden die Ereignisse der angegebenen Vorlage entsprechend automatisch ausgewählt. Weitere Informationen zu Ereignisklassen finden Sie unter [Ereignisklassen in SQL Server – Referenz](../../relational-databases/event-classes/sql-server-event-class-reference.md).  
|Datenspalten|Geben Sie die Datenspalten an, für die eine Ablaufverfolgung ausgeführt werden soll, indem Sie das Kontrollkästchen für das Ereignis und die benötigte Datenspalte aktivieren. Die relevanten Ereignisspalten werden standardmäßig für die einzelnen in die Ablaufverfolgung eingeschlossenen Ereignisse aktiviert, sofern das Kontrollkästchen des entsprechenden Ereignisses aktiviert ist. Wenn Sie auf der Registerkarte **Allgemein** die Option **Neue Vorlage auf vorhandener basieren** aktiviert haben, werden die Datenspalten und Filter der angegebenen Vorlage entsprechend automatisch ausgewählt.  
|Filter|Sie können Filter festlegen, indem Sie auf die Spaltenüberschrift klicken und die Filterkriterien eingeben. Gefilterte Datenspalten sind im Dialogfeld **Filter bearbeiten** links neben der Spaltenbezeichnung durch ein Filtersymbol gekennzeichnet.  
|**Alle Ereignisse anzeigen**|Zeigt alle verfügbaren Ereignisse an. Diese Option ist standardmäßig aktiviert, wenn Sie eine neue Vorlage erstellen, die nicht auf einer vorhandenen Vorlage basiert. Deaktivieren Sie diese Option, um alle nicht ausgewählten Ereignisse im Raster **Ereignisauswahl** auszublenden.  
|**Alle Spalten anzeigen**|Zeigt alle verfügbaren Datenspalten an. Diese Option ist standardmäßig aktiviert, wenn Sie eine neue Vorlage erstellen, die nicht auf einer vorhandenen Vorlage basiert. Deaktivieren Sie diese Option, um alle nicht ausgewählten Datenspalten im Raster **Ereignisauswahl** auszublenden.  
|**Spaltenfilter**|Öffnet das Dialogfeld **Filter bearbeiten**, in dem links neben der Datenspaltenbezeichnung ein Filtersymbol angezeigt wird. Mithilfe des Dialogfelds **Filter bearbeiten** können Sie Datenspaltenfilter bearbeiten.  
|**Spalten organisieren**|Ändert die Reihenfolge der Spalten in der Ablaufverfolgung und gruppiert die Ergebnisse in einer oder mehreren Spalten. 

## <a name="trace-file-properties"></a>Eigenschaften der Ablaufverfolgungsdatei 
### <a name="general-tab"></a>Registerkarte "Allgemein"
Mithilfe der Registerkarte **Allgemein** des Dialogfelds **Eigenschaften der Ablaufverfolgungsdatei** können Sie die Eigenschaften einer Ablaufverfolgungsdatei anzeigen.  
Öffnen Sie eine Ablaufverfolgungsdatei, um dieses Fenster anzuzeigen. Klicken Sie dann im Menü **Datei** auf **Eigenschaften**.  

|Element|BESCHREIBUNG
|---|---
|**Dateiname**|Der Pfad und der Name der angezeigten Ablaufverfolgungsdatei.  
|**Name des Ablaufverfolgungsanbieters**|Zeigt den Namen der Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] an, für die die Ablaufverfolgung ausgeführt wurde.  
|**Typ des Ablaufverfolgungsanbieters**|Zeigt den Servertyp an, der die Ablaufverfolgung bereitgestellt hat.  
|**version**|Zeigt die Version des Servers an, der die Ablaufverfolgung bereitgestellt hat.  
|**Dateigröße (KB)**|Die Größe der Ablaufverfolgungsdatei in Kilobyte (KB).  
|**Erstellt**|Datum und Uhrzeit der Erstellung der Ablaufverfolgungsdatei.  
|**Geändert** |Datum und Uhrzeit der Änderungen der Ablaufverfolgungsdatei.  

### <a name="events-selection-tab"></a>Registerkarte „Ereignisauswahl“
Mithilfe der Registerkarte **Ereignisauswahl** im Dialogfeld **Eigenschaften der Ablaufverfolgungsvorlage** können Sie die Spalteneigenschaften der Ablaufverfolgung anzeigen oder Datenspalten aus der Ablaufverfolgung entfernen.  
Öffnen Sie eine Ablaufverfolgungsdatei, um dieses Fenster anzuzeigen. Klicken Sie dann im Menü **Datei** auf **Eigenschaften** und dann auf die Registerkarte **Ereignisauswahl** .  

|Element|BESCHREIBUNG
|---|---
|Spalte **Ereignisse**|Zeigt verfolgte Ereignisse an, die nach Ereigniskategorien geordnet sind. Zunächst sind alle Ereignisse in der Ablaufverfolgung ausgewählt. Die Ereignisse können ausgewählt werden, indem das Kontrollkästchen oder eine Datenspalte für ein Ereignis aktiviert wird. Wenn das Ereigniskästchen aktiviert ist, werden alle für dieses Ereignis verfügbaren Datenspalten ausgewählt. Wenn die Datenspalte für ein Ereignis aktiviert ist, wird das Ereignis automatisch aktiviert und mit ihm alle weiteren erforderlichen Spalten. Wenn Sie eine Ablaufverfolgungsdatei oder -tabelle anzeigen, können Sie per Deaktivierung von Ereignissen oder Datenspalten die Menge der angezeigten Daten im Ablaufverfolgungsfenster zur besseren Übersicht reduzieren. Sie können auch Spaltenfilter ändern, um die Menge der sichtbaren Daten im Ablaufverfolgungsfenster zu reduzieren. Weitere Informationen zu Ereignisklassen finden Sie unter [Ereignisklassen in SQL Server – Referenz](../../relational-databases/event-classes/sql-server-event-class-reference.md).  
|Datenspalten|Zeigt Datenspalten der Ablaufverfolgung an. Alle relevanten Datenspalten in der Ablaufverfolgung sind standardmäßig für jedes in die Verfolgung einbezogene Ereignis aktiviert.  
|Filter|Sie können Filter festlegen, indem Sie auf die Spaltenüberschrift klicken und die Filterkriterien eingeben. Gefilterte Datenspalten sind im Dialogfeld **Filter bearbeiten** links neben der Spaltenbezeichnung durch ein Filtersymbol gekennzeichnet.  
|**Alle Ereignisse anzeigen**|Zeigt alle verfügbaren Ereignisse an. Standardmäßig werden nur Zeilen im Raster **Ereignisauswahl** angezeigt, die ausgewählt sind. Deaktivieren Sie dieses Kontrollkästchen, um alle nicht ausgewählten Ereignisse im Raster **Ereignisauswahl** auszublenden. Wenn die Option **Alle Ereignisse anzeigen** aktiviert ist und Sie eine Ablaufverfolgungsdatei oder -tabelle anzeigen, werden alle aufgezeichneten Ereignisse in der Ablaufanzeige des Ablaufverfolgungsfensters angezeigt.  
|**Alle Spalten anzeigen**|Zeigt alle verfügbaren Datenspalten an. Standardmäßig werden nur ausgewählte Datenspalten angezeigt. Deaktivieren Sie dieses Kontrollkästchen, um alle nicht ausgewählten Datenspalten im Raster **Ereignisauswahl** auszublenden.  
|**Spaltenfilter**|Öffnet das Dialogfeld **Filter bearbeiten** , durch das ein Filtersymbol auf der linken Seite des Spaltentitels für gefilterte Datenspalten angezeigt wird. Mithilfe des Dialogfelds **Filter bearbeiten** können Sie Datenspaltenfilter bearbeiten.  
|**Spalten organisieren**|Klicken Sie nach der Auswahl der zu verfolgenden **Ereignisse** und Datenspalten auf **Spalten organisieren**, um eine Reorganisation der Spalten im Raster des Fensters mit den Ablaufverfolgungsergebnissen zu erzwingen.  

## <a name="trace-table-properties"></a>Eigenschaften der Ablaufverfolgungstabelle
### <a name="events-selection-tab"></a>Registerkarte „Ereignisauswahl“
Mithilfe der Registerkarte **Ereignisauswahl** im Dialogfeld **Eigenschaften der Ablaufverfolgungstabelle** können Sie die Ereignisse und Datenspalteneigenschaften der Ablaufverfolgung anzeigen oder Ereignisse bzw. Spalten aus der Ablaufverfolgung entfernen.  
Öffnen Sie mit [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] eine Ablaufverfolgungstabelle, um dieses Fenster anzuzeigen. Klicken Sie dann im Menü **Datei** auf **Eigenschaften** und dann auf die Registerkarte **Ereignisauswahl** .  

|Element|BESCHREIBUNG
|---|---
|Spalte **Ereignisse**|Zeigt verfolgte Ereignisse an, die nach Ereigniskategorien geordnet sind. Die Ereignisse können ausgewählt werden, indem das Kontrollkästchen oder eine Datenspalte für ein Ereignis aktiviert wird. Wenn das Ereigniskästchen aktiviert ist, werden alle für dieses Ereignis verfügbaren Datenspalten ausgewählt. Wenn die Datenspalte für ein Ereignis aktiviert ist, wird das Ereignis automatisch aktiviert und mit ihm alle weiteren erforderlichen Spalten. Wenn Sie eine Ablaufverfolgungsdatei oder -tabelle anzeigen, können Sie per Deaktivierung von Ereignissen oder Datenspalten die Menge der angezeigten Daten im Ablaufverfolgungsfenster zur besseren Übersicht reduzieren. Sie können auch Spaltenfilter ändern, um die Menge der sichtbaren Daten im Ablaufverfolgungsfenster zu reduzieren. Weitere Informationen zu Ereignisklassen finden Sie unter [Ereignisklassen in SQL Server – Referenz](../../relational-databases/event-classes/sql-server-event-class-reference.md).  
|Sonstige Datenspalten|Zeigt Datenspalten der Ablaufverfolgung an. Alle relevanten Datenspalten in der Ablaufverfolgung sind standardmäßig für jedes in die Verfolgung einbezogene Ereignis aktiviert.  
|Filter|Sie können Filter festlegen, indem Sie auf die Spaltenüberschrift klicken und die Filterkriterien eingeben. Gefilterte Datenspalten sind im Dialogfeld **Filter bearbeiten** links neben der Spaltenbezeichnung durch ein Filtersymbol gekennzeichnet.  
|**Alle Ereignisse anzeigen**|Zeigt alle verfügbaren Ereignisse an. Standardmäßig werden nur Zeilen im Raster **Ereignisauswahl** angezeigt, die ausgewählt sind. Deaktivieren Sie dieses Kontrollkästchen, um alle nicht ausgewählten Ereignisse im Raster **Ereignisauswahl** auszublenden. Wenn die Option **Alle Ereignisse anzeigen** aktiviert ist und Sie eine Ablaufverfolgungsdatei oder -tabelle anzeigen, werden alle aufgezeichneten Ereignisse in der Ablaufanzeige des Ablaufverfolgungsfensters angezeigt.  
|**Alle Spalten anzeigen**|Zeigt alle verfügbaren Datenspalten an. Standardmäßig werden nur ausgewählte Datenspalten angezeigt. Deaktivieren Sie dieses Kontrollkästchen, um alle nicht ausgewählten Datenspalten im Raster **Ereignisauswahl** auszublenden.  
|**Spaltenfilter**|Öffnet das Dialogfeld **Filter** bearbeiten, in dem links neben der Spaltenbezeichnung ein Filtersymbol angezeigt wird. Mithilfe dieses Dialogfelds können Sie Datenspaltenfilter bearbeiten.  
|**Spalten organisieren** |Klicken Sie nach der Auswahl der zu verfolgenden **Ereignisse** und Datenspalten auf **Spalten organisieren**, um eine Reorganisation der Spalten im Raster des Fensters mit den Ablaufverfolgungsergebnissen zu erzwingen.  

## <a name="performance-counters-limit"></a>Beschränken der Leistungsindikatoren
Mithilfe des Dialogfelds zum Beschränken der Leistungsindikatoren, können Sie die Informationen aus einer Leistungsprotokolldatei des Systemmonitors beschränken, wenn er mit einer [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] -Ablaufverfolgung korreliert wird. Mithilfe dieses Dialogfelds können Sie Indikatoren auswählen, die angezeigt und für die Korrelation verwendet werden sollen.  
Das Dialogfeld zum **Beschränken der Leistungsindikatoren** wird mit den Leistungsobjekten und -indikatoren aufgefüllt, die in der Leistungsprotokolldatei enthalten sind.  
### <a name="to-select-performance-objects-and-counters-to-correlate-with-a-trace"></a>So wählen Sie Leistungsobjekte und -indikatoren aus, die mit einer Ablaufverfolgung korreliert werden sollen  
1.  Erweitern Sie ein Leistungsobjekt, um anzuzeigen, welche Indikatoren in der Leistungsprotokolldatei enthalten sind.  
2.  Aktivieren Sie die Indikatoren, die mit der [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] -Ablaufverfolgungsdatei korreliert werden sollen.  

Wenn Sie alle Indikatoren für ein Leistungsobjekt auswählen möchten, setzen Sie in das Feld neben dem Leistungsobjekt ein Häkchen. Wenn Sie den obersten Knoten aktivieren, der für den Computer steht, werden alle Leistungsobjekte und -indikatoren ausgewählt, die in der Leistungsprotokolldatei enthalten sind. 
## <a name="toolsoptions-general-options-page"></a>Extras/Optionen (Seite „Allgemeine Optionen“)
Verwenden Sie das Dialogfeld **Allgemeine Optionen** , um die folgenden Optionen anzuzeigen oder anzugeben.  
### <a name="display-options"></a>Anzeigeoptionen  

|Element|BESCHREIBUNG
|---|---
|**Schriftartname**|Zeigt den Namen der Schriftart an, die im Ergebnisraster der Ablaufverfolgung verwendet wird.  
|**Schriftgrad**|Zeigt die Größe der Schrift an, die im Ergebnisraster der Ablaufverfolgung verwendet wird.  
|**Schriftart auswählen**|Öffnet ein Dialogfeld, in dem die Schriftarteinstellungen geändert werden können.  
|**Einstellungen für Land/Region zum Anzeigen von Datums- und Uhrzeitwerten verwenden**|Zeigt Datums- und Uhrzeitwerte entsprechend den Einstellungen für das Land/die Region an, die für Ihren Computer konfiguriert sind. Wenn Sie diese Option nicht aktivieren, werden die Datums- und Uhrzeitwerte in dem festgelegten Format anzeigt, das von Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]verwendet wird und bei dem auch Millisekunden angezeigt werden. Beachten Sie, dass sich das Anzeigeformat von Zeitspalten wie **StartTime** und **EndTime** ändert, wenn das Kontrollkästchen aktiviert oder deaktiviert wird. Nicht geändert werden jedoch die **DateTime**-Wertparameter in den Sprachereignissen oder den Remoteprozeduraufrufen (Remote Procedure Calls, RPCs).  
|**Werte in der Spalte 'Dauer' in Mikrosekunden anzeigen**|Zeigt die Werte in der **Dauer** -Datenspalte bei Ablaufverfolgungen in Mikrosekunden an. Standardmäßig werden die Werte unter **Dauer** in Millisekunden angezeigt.  

### <a name="tracing-options"></a>Ablaufverfolgungsoptionen  

|Element|BESCHREIBUNG
|---|---
|**Ablaufverfolgung sofort nach dem Herstellen der Verbindung starten**|Startet eine Ablaufverfolgung mithilfe der Standardvorlage, sobald eine Verbindung hergestellt ist.  
|**Ablaufverfolgungsdefinition bei Änderung der Anbieterversion aktualisieren**|Wendet die neueste Ablaufverfolgungsdefinition auf [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] an, wenn der Anbieter aktualisiert wird. Diese Option ist standardmäßig nicht aktiviert. Dadurch ist [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] gezwungen, die Ablaufverfolgungsdefinition beim Server abzufragen und, sofern diese vorhanden ist, die Datei auf dem Datenträger neu zu erstellen.  

### <a name="file-rollover-options"></a>Dateirolloveroptionen  

|Element|BESCHREIBUNG
|---|---
|**Alle Rolloverdateien nacheinander ohne Eingabeaufforderung laden**|Lädt die Rolloverdateien automatisch, wenn eine Ablaufverfolgungsdatei geöffnet wird. Wenn mehrere Dateien bei der Ablaufverfolgung erstellt wurden, werden bei Auswahl dieser Option alle Rolloverdateien automatisch geladen.  
|**Bestätigung vor dem Laden von Rolloverdateien**|Legt fest, dass Sie beim Öffnen einer Ablaufverfolgungsdatei von [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] zur Bestätigung aufgefordert werden, bevor eine Rolloverdatei hinzugefügt wird.  
|**Nachfolgende Rolloverdateien niemals laden**|[!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] beim Öffnen einer Ablaufverfolgungsdatei nachfolgende Rolloverdateien lädt.  

### <a name="replay-options"></a>Wiedergabeoptionen  

|Element|BESCHREIBUNG
|---|---
|**Standardanzahl von Wiedergabethreads**|Gibt die Anzahl der Threads an, die gleichzeitig verwendet werden können. Eine größere Anzahl beansprucht mehr Ressourcen bei der Wiedergabe, erhöht aber die Wiedergabeparallelität.  
|**Standardwartezeit für Systemüberwachung (Sek.)**|Gibt die Wartezeit für die Wiedergabe in Sekunden an. Der Standardwert ist 3600 Sekunden (1 Stunde). Die Einstellung bestimmt den Zeitraum, in dem ein Thread ausgeführt werden kann, bevor er von der Systemüberwachung beendet wird.  
|**Standardabrufintervall für Systemüberwachung (Sek.)**|Gibt das Abrufinterval für die Systemüberwachung während der Wiedergabe in Sekunden an. Der Standardwert ist 60 Sekunden. Mit diesem Wert kann der Benutzer konfigurieren, wie oft die Systemüberwachung Informationen zu potenziell zu beendenden Vorgängen abruft.

## <a name="source-table-database-engine-tuning-advisor-select-workload-table"></a>Quelltabelle (Datenbankoptimierungsratgeber –Arbeitsauslastungstabelle auswählen)
In Microsoft SQL Server Profiler und im Optimierungsratgeber von Microsoft SQL Server wird dieses Dialogfeld verwendet, um Tabellen auszuwählen.  
- In Profiler können Sie mithilfe des Dialogfelds **Quelltabelle** eine Quelltabelle für eine Ablaufverfolgungstabelle angeben. Dabei handelt es sich um eine Tabelle, aus der eine Ablaufverfolgung geladen wird. Deren Inhalte können angezeigt oder zur Wiedergabe der Ablaufverfolgung verwendet werden.  
- Wählen Sie mithilfe des Dialogfelds **Arbeitsauslastungstabelle auswählen** im Optimierungsratgeber eine Datenbanktabelle mit Ablaufverfolgungsinformationen von Profiler aus, die als zu optimierende Arbeitsauslastung oder zur Vorschau der Tabelleninhalte vor dem Starten der Optimierungsanalyse verwendet werden sollen.  

|Element|BESCHREIBUNG
|---|---
|**SQL Server**|Gibt die Instanz von SQL Server an, mit der aktuell eine Verbindung besteht. Dieses Feld wird automatisch ausgefüllt und kann nicht aktualisiert werden.  
|**Datenbank**|Geben Sie die Datenbank an, in der die Ablaufverfolgungstabelle gespeichert ist.  
|**Besitzer**|Specifies the owner of the trace table. Dieses Feld wird automatisch mit **dbo** ausgefüllt.  
|**Table**|Geben Sie den Namen der Ablaufverfolgungstabelle an, aus der die Ablaufverfolgung gelesen werden soll.  

## <a name="destination-table"></a>Zieltabelle
Mithilfe des Dialogfelds **Zieltabelle** können Sie angeben, in welcher Tabelle die Ablaufverfolgung gespeichert werden soll.  

|Element|BESCHREIBUNG
|---|---
|**SQL Server**|Gibt die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] an, zu der zurzeit eine Verbindung besteht. Dieses Feld wird automatisch ausgefüllt und kann nicht aktualisiert werden. Wenn Sie den Server ändern möchten, klicken Sie auf **Abbrechen** , und stellen Sie eine Verbindung zu der Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] her, in der die Ablaufverfolgungstabelle gespeichert werden soll.  
|**Datenbank**|Geben Sie die Datenbank an, in der die Ablaufverfolgungstabelle gespeichert werden soll.  
|**Besitzer**|Specifies the owner of the trace table. Dieses Feld wird automatisch mit **dbo** ausgefüllt.  
|**Table**|Geben Sie den Namen der Tabelle an, in der die Ablaufverfolgung gespeichert werden soll.  

## <a name="replay-configuration"></a>Wiedergabekonfiguration
### <a name="basic-replay-options"></a>Grundlegende Wiedergabeoptionen
Mithilfe der Seite **Grundlegende Wiedergabeoptionen** des Dialogfelds **Wiedergabekonfiguration** können Sie angeben, auf welche Weise eine Ablaufverfolgungsdatei oder -tabelle wiedergegeben werden soll.  
Zum Anzeigen dieses Fensters öffnen Sie mithilfe von [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] eine Ablaufverfolgungsdatei oder -tabelle, die die zur Wiedergabe vorgesehenen Ereignisse enthält. Weitere Informationen finden Sie unter [Replay Requirements](../../tools/sql-server-profiler/replay-requirements.md). Wenn die Ablaufverfolgungsdatei oder -tabelle geöffnet ist, klicken Sie im Menü **Wiedergeben** auf **Start**, und stellen Sie dann eine Verbindung mit der Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] her, auf der die Ablaufverfolgung wiedergegeben werden soll.  

|Element|BESCHREIBUNG
|---|---
|**Wiedergabeserver**|Zeigt die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] an, mit der eine Verbindung für die Wiedergabe hergestellt wird.  
|**Ändern...**|Öffnet das Dialogfeld **Verbindung mit Server herstellen** , um eine Verbindung mit einem anderen Server herzustellen.  
|**In Datei speichern** |Speichert die Wiedergabeergebnisse in einer Datei. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] zeigt das Standarddialogfeld zum Speichern von Dateien an, in dem Sie einen Speicherort für die Datei angeben können.  
|**In Tabelle speichern**|Speichert die Wiedergabeergebnisse in einer Tabelle. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] wird ein Dialogfeld zum Auswählen der Tabelle angezeigt, in dem Sie einen Speicherort für die Tabelle angeben können.  
|**Anzahl von Wiedergabethreads**|Gibt die Anzahl der Threads an, die gleichzeitig verwendet werden können. Bei einer höheren Anzahl werden zwar mehr Ressourcen beansprucht, dafür erfolgt die Wiedergabe schneller und in höherem Maße gleichzeitig.  
|**Ereignisse in der Reihenfolge wiedergeben, in der ihr Ablauf verfolgt wurde**|Gibt Ereignisse sequenziell wieder. Verwenden Sie diese Option, wenn Sie eine Ablaufverfolgung zum Debuggen wiedergeben.  
|**Ereignisse mithilfe mehrerer Threads wiedergeben** |Gibt die Ereignisse gleichzeitig wieder. Diese Option ist zwar schneller als die sequenzielle Wiedergabe von Ereignissen, dafür ist kein Debugging verfügbar. Die Ereignisse werden je nach dazugehöriger Systemprozess-ID (SPID) geordnet.  
|**Wiedergabeergebnisse anzeigen**|Zeigt die Wiedergabeergebnisse in [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]an. 

### <a name="advanced-replay-options"></a>Erweiterte Wiedergabeoptionen
Mithilfe der Registerkarte **Erweiterte Wiedergabeoptionen** des Dialogfelds **Wiedergabekonfiguration** können Sie angeben, auf welche Weise eine Ablaufverfolgungsdatei wiedergegeben werden soll.  
Zum Anzeigen dieses Fensters öffnen Sie mithilfe von [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] eine Ablaufverfolgungsdatei oder -tabelle, die die zur Wiedergabe vorgesehenen Ereignisse enthält. Weitere Informationen finden Sie unter [Replay Requirements](../../tools/sql-server-profiler/replay-requirements.md). Wenn die Ablaufverfolgungsdatei oder -tabelle geöffnet ist, klicken Sie im Menü **Wiedergeben** auf **Start**, und stellen Sie dann eine Verbindung mit der Instanz von SQL Server her, auf der die Ablaufverfolgung wiedergegeben werden soll. Klicken Sie anschließend auf die Registerkarte **Erweiterte Wiedergabeoptionen** .  

|Element|BESCHREIBUNG
|---|---
|**System-SPIDs wiedergeben**|Gibt an, ob [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] die Systemprozess-IDs (SPIDs) wiedergibt.  
|**Nur eine SPID wiedergeben**|Gibt nur die Aktivität in der Ablaufverfolgungs-Quelldatei wieder, die mit der ausgewählten SPID in Beziehung steht.  
|**SPID für Wiedergabe**|Gibt an, welche SPID wiedergegeben werden soll.  
|**Wiedergabe nach Datum und Zeit beschränken**|Durch Aktivieren dieses Kontrollkästchens kann festgelegt werden, dass nur ein Teil der Ablaufverfolgungs-Quelldatei wiedergegeben wird.  
|**Startzeit**|Datum und Uhrzeit in der Ablaufverfolgungs-Quelldatei, zu der die Wiedergabe beginnen soll.  
|**Beendigungszeit**|Datum und Uhrzeit in der Ablaufverfolgungs-Quelldatei, zu der die Wiedergabe enden soll.  
|**Wartezeit für Systemüberwachung (Sek.)**|Gibt die Wartezeit für die Wiedergabe in Sekunden an. Der Standardwert ist 3600 Sekunden (1 Stunde). Die Einstellung bestimmt den Zeitraum, in dem ein Prozess ausgeführt werden kann, bevor er von der Systemüberwachung beendet wird.  
|**Abrufintervall für Systemüberwachung (Sek.)**|Gibt das Abrufinterval für die Systemüberwachung während der Wiedergabe in Sekunden an. Der Standardwert ist 60 Sekunden. Mit diesem Wert kann der Benutzer konfigurieren, wie oft die Systemüberwachung Informationen zu potenziell zu beendenden Vorgängen abruft.  
|**Überwachung blockierter SQL Server-Prozesse aktivieren**|Aktiviert einen Prozess, mit dem nach blockierten oder blockierenden Prozessen gesucht wird.  
|**Wartezeit für die Überwachung blockierter Prozesse (Sek.)**|Konfiguriert die Häufigkeit, mit der mithilfe der Überwachung für blockierte Prozesse nach blockierten oder blockierenden Prozessen gesucht wird.  

## <a name="find-dialog-box"></a>Suchen (Dialogfeld)
Mithilfe des Dialogfelds **Suchen** können Sie eine Ablaufverfolgung nach bestimmten Zeichen oder Wörtern durchsuchen. Um einen Suchvorgang abzubrechen, drücken Sie ESC.  
 Um das Dialogfeld in [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]zu öffnen, klicken Sie im Menü **Bearbeiten** auf **Suchen**.  

|Element|BESCHREIBUNG
|---|---
|**Suchen nach**|Geben Sie den Text ein, nach dem gesucht werden soll. Die Suche entspricht jeder Zeichenfolge, die die angegebene Zeichenfolge enthält. Die Suche nach „Completed“ entspricht beispielsweise „SQL:BatchCompleted“. Platzhalterzeichen (*, ? usw.) werden nicht unterstützt.  
|**Suchen in Spalte**|Klicken Sie auf eine zu durchsuchende Datenspalte, oder klicken Sie auf **\<All columns>** , um alle Datenspalten in der Ablaufverfolgung zu durchsuchen.  
|**Groß-/Kleinschreibung beachten**|Sucht nach Text, der die gleiche Groß-/Kleinschreibung besitzt wie der Text im Feld **Suchen nach** . Deaktivieren Sie dieses Kontrollkästchen, um in der Ablaufverfolgung nach Beispielen zu suchen, die groß oder klein geschrieben werden.  
|**Nur ganzes Wort suchen**|Schränkt die Suche auf ganze Wörter ein. Deaktivieren Sie das Kontrollkästchen **Nur ganzes Wort** suchen, um nach Zeichen innerhalb eines Worts zu suchen.  
|**Weitersuchen**|Sucht nach dem nächsten Beispiel der im Feld **Suchen nach** angegebenen Zeichen.  
|**Vorheriges suchen**|Sucht in der Ablaufverfolgung rückwärts nach dem vorherigen Beispiel der im Feld **Suchen nach** angegebenen Zeichen.  

 ## <a name="organize-columns"></a>Spalten organisieren
Verwenden Sie das Dialogfeld **Spalten organisieren** , um Datenspalten für das Gruppieren oder Aggregieren von Ereignissen auszuwählen, die in einer Ablaufverfolgung angezeigt werden. Dadurch können große Ablaufverfolgungsdateien und -tabellen einfacher gelesen und analysiert werden.  
- Durch das Aggregieren werden alle Ereignisse in der Ablaufverfolgung unter die jeweiligen Ereignisklassentypen verschoben und reduziert. Links neben dem Ereignisklassennamen wird ein Pluszeichen ( **+** ) angezeigt. Wenn Sie auf das Pluszeichen klicken, wird die Ereignisklasse expandiert, sodass alle Ereignisse dieses Typs angezeigt werden.  
- Mithilfe der Gruppierung werden alle Ereignisklassen eines bestimmten Typs im Ablaufverfolgungsfenster zusammengefasst angezeigt. Die Ereignisse werden jedoch nicht unter dem Ereignisklassentyp reduziert.  

Wenn Sie Ereignisse in der Anzeige des Ablaufverfolgungsfensters gruppieren oder aggregieren, bleiben die dafür ausgewählten Spalten im Anzeigefenster fixiert, aber Sie können einen Bildlauf nach links oder rechts ausführen, um alle anderen Datenspalten anzuzeigen.  
Sie können auf dieses Dialogfeld zugreifen, indem Sie eine vorhandene Ablaufverfolgungsdatei oder -tabelle öffnen und im Menü **Datei** von [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] auf **Eigenschaften** klicken. Klicken Sie im Dialogfeld **Ablaufverfolgungseigenschaften** auf die Registerkarte **Ereignisauswahl** , und dann auf **Spalten organisieren**. Sie können in der Registerkarte **Ereignisauswahl** auch auf **Spalten organisieren** klicken, wenn Sie eine neue Ablaufverfolgung erstellen.  
Verschieben Sie die Datenspaltennamen unter **Gruppen** , um Ereignisklassen im Ablaufverfolgungsfenster zu gruppieren oder aggregieren.
- Um Ereignisse zu aggregieren, verschieben Sie eine Datenspalte in die **Gruppen**. Dadurch werden alle Ereignisse eines bestimmten Typs unter dem Namen des Ereignisklassentyps in der Anzeige im Ablaufverfolgungsfenster reduziert. Links neben dem Ereignisklassennamen wird ein Pluszeichen ( **+** ) angezeigt. Klicken Sie auf das Pluszeichen, um den Ereignisklassentyp zu expandieren und alle Ereignisse anzuzeigen. Sie können die Aggregierung bzw. Gruppierung ein- und ausschalten, indem Sie im Menü **Ansicht** auf **Aggregierte Ansicht** bzw. **Gruppierte Ansicht** klicken.
- Um Ereignisse zu gruppieren, verschieben Sie mehrere Datenspalten in die **Gruppen**. Dadurch werden alle Ereignisse eines bestimmten Typs zusammen in der Anzeige des Ablaufverfolgungsfensters gruppiert, aber nicht unter dem Namen des jeweiligen Ereignisklassentyps reduziert. Sie können zwischen der gruppierten und der nicht gruppierten Ansicht wechseln, indem Sie im Menü Ansicht auf **Gruppierte Ansicht** klicken. Wenn mehr als eine Datenspalte in die **Gruppen** verschoben wird, ist die Option zum Wechseln in die **Aggregierte Ansicht** nicht verfügbar.

|Element|BESCHREIBUNG
|---|---
|**Spalten**|Liste der Datenspalten, die für das Verschieben in **Gruppen** verfügbar sind. Klicken Sie auf das Pluszeichen ( **+** ) links neben **Spalten** , um die Liste zu erweitern.  
|**Up**|Klicken Sie nach dem Auswählen einer Datenspalte auf **Nach oben** , um die Datenspalten nach oben in **Gruppen** zu verschieben. Sie können auch auf **Nach oben** klicken, um die Anzeige der Spalten im Ablaufverfolgungsfenster neu anzuordnen.  
|**Nach unten**|Klicken Sie nach dem Auswählen einer Datenspalte auf **Nach unten** , um die Datenspalten aus den **Gruppen** heraus zu verschieben. Sie können auch auf **Nach unten** klicken, um die Anzeige der Spalten im Ablaufverfolgungsfenster neu anzuordnen.  

## <a name="edit-filter"></a>Filter bearbeiten
Mithilfe des Dialogfelds **Filter bearbeiten** können Sie Filter für Datenspalten in einer Ablaufverfolgung erstellen und ändern. Klicken Sie auf einen Datenspaltenfilter in der Liste. Daraufhin werden die verfügbaren Filterkriterien für diese Datenspalte im angrenzenden Bereich angezeigt. Geben Sie die Filterkriterien ein, und klicken Sie auf **OK** , um die Filterkriterien auf die ausgewählte Datenspalte anzuwenden. Wenn links neben dem Datenspaltenname in der Liste ein Filtersymbol angezeigt wird, ist für diese Spalte bereits ein Filter konfiguriert.  
 >[!NOTE]
 >Für Spalten mit Zeichenfolgendaten werden die Filterkriterien als Zeichenfolgenwert LIKE oder NOT LIKE angezeigt.  

## <a name="select-template-name"></a>Vorlagennamen auswählen
Mit dem Dialogfeld **Vorlagenname auswählen** können Sie eine vorhandene [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] -Ablaufverfolgungsvorlage auswählen und in eine Datei auf dem Betriebssystem exportieren. Darüber hinaus können Sie dieses Dialogfeld dazu verwenden, beim Bearbeiten einer vorhandenen Ablaufverfolgungsvorlage einen anderen Namen auszuwählen oder einzugeben, unter dem die Ablaufverfolgungsvorlage gespeichert werden soll. Sie können beim Exportieren einer Vorlage auf dieses Dialogfeld zugreifen, indem Sie im Menü **Datei** von [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] auf **Vorlagen** zeigen, und dann auf **Vorlage exportieren** klicken. Um beim Ändern des Namens einer Vorlage auf dieses Dialogfeld zuzugreifen, zeigen Sie im Menü **Datei** auf **Vorlagen** und auf **Vorlage bearbeiten**, und klicken Sie dann auf **Speichern unter**.  

|Element|BESCHREIBUNG
|---|---
|**Servertyp**|Wählen Sie den Servertyp aus, auf dem Sie eine Vorlage auswählen möchten. Diese Option ist nur beim Exportieren einer Vorlage verfügbar.  
|**Vorlagenname**|Geben Sie einen neuen Vorlagennamen ein, oder wählen Sie einen Vorlagennamen aus der Liste aus. Beim Exportieren in eine Vorlage können Sie nur einen Vorlagennamen aus der Liste auswählen. 

## <a name="see-also"></a>Weitere Informationen 
[SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)   
[Überwachen der Serverleistung und -aktivität](../../relational-databases/performance/server-performance-and-activity-monitoring.md)  
  
