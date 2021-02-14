---
title: Debuggen und Diagnostizieren von Spark-Anwendungen
titleSuffix: SQL Server Big Data Clusters
description: Verwenden Sie Spark History Server, um Spark-Anwendungen zu debuggen und zu diagnostizieren, die auf Big Data-Clustern unter SQL Server 2019 ausgeführt werden.
author: jejiang
ms.author: jejiang
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 43a450b21ab7d025f61f77b1391f9484e1cb2991
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045920"
---
# <a name="debug-and-diagnose-spark-applications-on-big-data-clusters-2019-in-spark-history-server"></a>Debuggen und Diagnostizieren von Spark-Anwendungen in [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] in Spark History Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Dieser Artikel enthält Anleitungen zur Verwendung der erweiterten Version von Spark History Server zum Debuggen und Diagnostizieren von Spark-Anwendungen in Big-Data-Clustern für SQL Server. Diese Debug- und Diagnosefunktionen sind in den Spark History Server integriert und werden von Microsoft unterstützt. Die Erweiterung umfasst die Registerkarten „Data“ (Daten), „Graph“ (Diagramm) und „Diagnosis“ (Diagnose). Auf der Registerkarte „Data“ (Daten) können Benutzer die Eingabe- und Ausgabedaten des Spark-Auftrags überprüfen. Auf der Registerkarte „Graph“ (Diagramm) können Benutzer den Datenfluss überprüfen und das Auftragsdiagramm wiedergeben. Auf der Registerkarte „Diagnosis“ (Diagnose) findet der Benutzer Informationen zur Datenschiefe, Zeitabweichung und eine Analyse zur Executorauslastung.

## <a name="get-access-to-spark-history-server"></a>Zugreifen auf Spark History Server

Die Benutzeroberfläche der Open-Source-Version von Spark History Server wird mit Informationen wie auftragsspezifischen Daten und einer interaktiven Visualisierung des Auftragsdiagramms und von Datenflüssen für Big-Data-Cluster erweitert. 

### <a name="open-the-spark-history-server-web-ui-by-url"></a>Öffnen der Webbenutzeroberfläche von Spark History Server über eine URL
Öffnen Sie Spark History Server, indem Sie die folgende URL aufrufen. Ersetzen Sie dabei `<Ipaddress>` und `<Port>` durch genaue Informationen zu den Big-Data-Clustern. Bei Clustern, die vor SQL Server 2019 CU 5 mit Einrichtung eines Big Data-Clusters in einer Standardauthentifizierung (Benutzername/Kennwort) bereitgestellt wurden, müssen Sie **root**-Benutzerangaben machen, wenn Sie bei der Anmeldung bei Gatewayendpunkten (Knox) dazu aufgefordert werden. Weitere Informationen finden Sie unter [Verwenden eines Python-Skripts zum Bereitstellen eines Big-Data-Clusters für SQL Server in Azure Kubernetes Service (AKS)](quickstart-big-data-cluster-deploy.md) [!INCLUDE [big-data-cluster-root-user](../includes/big-data-cluster-root-user.md)]

```
https://<Ipaddress>:<Port>/gateway/default/sparkhistory
```

Die Webbenutzeroberfläche von Spark History Server sieht wie folgt aus:

![Spark History Server](./media/apache-azure-spark-history-server/spark-history-server.png)


## <a name="data-tab-in-spark-history-server"></a>Die Registerkarte „Data“ (Daten) in Spark History Server
Wählen Sie die Auftrags-ID aus, und klicken Sie im Toolmenü auf **Data** (Daten), um die Datenansicht aufzurufen.

+ Überprüfen Sie die Registerkarten **Inputs** (Eingaben), **Outputs** (Ausgaben) und **Table Operations** (Tabellenvorgänge) einzeln.

    ![Registerkarten zu den Daten in Spark History Server](./media/apache-azure-spark-history-server/sparkui-data-tabs.png)

+ Kopieren Sie alle Zeilen, indem Sie auf die Schaltfläche **Copy** (Kopieren) klicken.

    ![Kopiert alle Zeilen](./media/apache-azure-spark-history-server/sparkui-data-copy.png)

+ Speichern Sie alle Daten in einer CSV-Datei, indem Sie auf die Schaltfläche **csv** klicken.

    ![Speichern von Daten als CSV-Dateien](./media/apache-azure-spark-history-server/sparkui-data-save.png)

+ Wenn Sie Suchbegriffe in das Feld **Search** (Suchen) eingeben, werden die Suchergebnisse sofort angezeigt.

    ![Begriffe suchen](./media/apache-azure-spark-history-server/sparkui-data-search.png)

+ Klicken Sie erst auf den Spaltenheader, um die Tabelle zu sortieren, und dann auf das Pluszeichen, um eine Zeile zu erweitern und weitere Details anzuzeigen, oder auf das Minuszeichen, um eine Zeile zuzuklappen.

    ![Funktionen der Datentabelle](./media/apache-azure-spark-history-server/sparkui-data-table.png)

+ Wenn Sie eine Datei herunterladen möchten, klicken Sie auf der rechten Seite auf die Schaltfläche **Partial Download** (Unvollständiger Download). Dann wird die ausgewählte Datei heruntergeladen und lokal gespeichert. Wenn die Datei nicht mehr vorhanden ist, wird eine neue Registerkarte geöffnet, auf der die Fehlermeldungen angezeigt werden.

    ![Eine Datenzeile herunterladen](./media/apache-azure-spark-history-server/sparkui-data-download-row.png)

+ Kopieren Sie den vollständigen oder den relativen Pfad, indem Sie entsprechend auf die Optionen **Copy Full Path** (Vollständigen Pfad kopieren) oder **Copy Relative Path** (Relativen Pfad kopieren) klicken, die über das Downloadmenü verfügbar sind. Wenn Sie bei Azure Data Lake Storage-Dateien die Option **Open in Azure Storage Explorer** (Im Azure Storage-Explorer öffnen) auswählen, wird der Azure Storage-Explorer geöffnet. Sie werden dann bei der Anmeldung zum genauen Ordner weitergeleitet.

    ![Vollständigen oder relativen Pfad kopieren](./media/apache-azure-spark-history-server/sparkui-data-copy-path.png)

+ Klicken Sie auf die Zahl unterhalb der Tabelle, um zwischen den einzelnen Seiten zu navigieren, wenn mehr Zeilen vorhanden sind als auf eine Seite passen. 

    ![Datenseite](./media/apache-azure-spark-history-server/sparkui-data-page.png)

+ Zeigen Sie mit der Maus auf das Fragezeichen neben „Data“ (Daten), um die QuickInfo anzuzeigen, oder klicken Sie auf das Fragezeichen, um weitere Informationen zu erhalten.

    ![Weitere Informationen zu den Daten](./media/apache-azure-spark-history-server/sparkui-data-more-info.png)

+ Senden Sie Feedback zu Problemen, indem Sie auf **Provide us feedback** (Feedback übermitteln) klicken.

    ![Feedback zur Registerkarte „Graph“ (Diagramm)](./media/apache-azure-spark-history-server/sparkui-graph-feedback.png)

## <a name="graph-tab-in-spark-history-server"></a>Die Registerkarte „Graph“ (Diagramm) in Spark History Server

Wählen Sie die Auftrags-ID aus, und klicken Sie im Menü des Tools auf **Graph** (Diagramm), um die Ansicht des Auftragsdiagramms aufzurufen.

+ Im daraufhin erstellten Auftragsdiagramm erhalten Sie eine Übersicht über Ihren Auftrag. 

+ Standardmäßig werden alle Aufträge angezeigt. Diese können nach **Job ID** (Auftrags-ID) gefiltert werden.

    ![„Graph“ (Diagramm): Job ID (Auftrags-ID)](./media/apache-azure-spark-history-server/sparkui-graph-jobid.png)

+ **Progress** (Status) wird als Standardwert belassen. Der Benutzer kann den Datenfluss überprüfen, indem er **Read** (Lesen) oder **Written** (Geschrieben) in der Dropdown-Liste von **Display** (Anzeigen) auswählt.

    ![„Graph“ (Diagramm): „Display“ (Anzeige)](./media/apache-azure-spark-history-server/sparkui-graph-display.png)

    Wärmebild der Diagrammknotenanzeige in Farbe

    ![„Graph“ (Diagramm): Wärmebild](./media/apache-azure-spark-history-server/sparkui-graph-heatmap.png)

+ Sie können den Auftrag wiedergeben, indem Sie auf die Schaltfläche **Playback** (Wiedergabe) klicken, und jederzeit wieder anhalten, indem Sie auf „Stop“ (Beenden) klicken. Die Auftragsanzeige in Farbe zur Darstellung verschiedener Statusangaben bei der Wiedergabe:

    + Grün für erfolgreiche Aufträge, die abgeschlossen wurden.
    + Orange bei Auftragsinstanzen, die nicht erfolgreich waren, aber das Endergebnis des Auftrags nicht beeinträchtigen. Diese Aufträge enthielten doppelte oder wiederholte Instanzen, werden später aber möglicherweise erfolgreich ausgeführt.
    + Blau für Aufträge, die gerade ausgeführt werden.
    + Weiß für Aufträge, deren Ausführung noch ansteht oder bei denen eine Phase übersprungen wurde.
    + Rot für fehlgeschlagene Aufträge.

    ![„Graph“ (Diagramm): Farbbeispiel für eine Ausführung](./media/apache-azure-spark-history-server/sparkui-graph-color-running.png)
 
    Übersprungene Phasen werden in weiß angezeigt.
    ![„Graph“ (Diagramm): Farbbeispiel für eine übersprungene Phase](./media/apache-azure-spark-history-server/sparkui-graph-color-skip.png)

    ![„Graph“ (Diagramm): Farbbeispiel für einen Fehler](./media/apache-azure-spark-history-server/sparkui-graph-color-failed.png)
 
    > [!NOTE]
    > Jeder Auftrag darf wiedergegeben werden. Die Wiedergabe von unvollständigen Aufträgen wird allerdings nicht unterstützt.


+ Scrollen Sie mit der Maus, um das Auftragsdiagramm zu vergrößern bzw. zu verkleinern, oder klicken Sie auf **Zoom to fit** (Mit Zoom anpassen), um es an die Bildschirmgröße anzupassen.
 
    ![„Graph“ (Diagramm): „Zoom to fit“ (Mit Zoom anpassen)](./media/apache-azure-spark-history-server/sparkui-graph-zoom2fit.png)

+ Zeigen Sie mit der Maus auf den Diagrammknoten, um bei fehlgeschlagenen Aufträgen die QuickInfo anzuzeigen, und klicken Sie auf eine Phase, um die Seite zu dieser Phase zu öffnen.

    ![„Graph“ (Diagramm): QuickInfo](./media/apache-azure-spark-history-server/sparkui-graph-tooltip.png)

+ Auf der Registerkarte „Job graph“ (Auftragsdiagramm) werden QuickInfos und kleine Symbole angezeigt, wenn es Aufträge gibt, die die folgenden Bedingungen erfüllen:
    + Datenschiefe: Lesegröße der Daten > durchschnittliche Lesegröße der Daten aller Aufträge innerhalb dieser Phase × 2 und Lesegröße der Daten > 10 MB
    + Zeitabweichung: Ausführungszeit > durchschnittliche Ausführungszeit aller Aufgaben in dieser Phase × 2 und Ausführungszeit > 2 Minuten

    ![„Graph“ (Diagramm): Symbol für eine Zeitabweichung](./media/apache-azure-spark-history-server/sparkui-graph-skew-icon.png)

+ Der Auftragsdiagrammknoten zeigt die folgenden Informationen zu den einzelnen Phasen an:
    + ID
    + Name oder Beschreibung
    + Anzahl der Aufträge insgesamt
    + Daten lesen: die Summe der Eingabegröße und Lesegröße der Zufallswiedergabe
    + Daten schreiben: die Summe der Ausgabegröße und der Schreibgröße der Zufallswiedergabe
    + Ausführungszeit: die Zeit zwischen der Startzeit des ersten Versuchs und der Endzeit des letzten Versuchs
    + Zeilenanzahl: die Summe der Eingabe und Ausgabedatensätze sowie die Datensätze der zufälligen Lese- und Schreibvorgänge
    + Status

    > [!NOTE]
    > Standardmäßig zeigt der Auftragsdiagrammknoten Informationen zum letzten Versuch der einzelnen Phasen an (mit Ausnahme der Phase „Ausführungszeit“), während der Wiedergabediagrammknoten Informationen zu jedem Versuch bereitstellt.

    > [!NOTE]
    > Für die Datengröße des Lese-und Schreibvorgangs wird 1 MB = 1000 KB = 1000 × 1000 Bytes verwendet.

+ Senden Sie Feedback zu Problemen, indem Sie auf **Provide us feedback** (Feedback übermitteln) klicken.

    ![Feedback zur Registerkarte „Graph“ (Diagramm)](./media/apache-azure-spark-history-server/sparkui-graph-feedback.png)


## <a name="diagnosis-tab-in-spark-history-server"></a>Die Registerkarte „Diagnosis“ (Diagnose) in Spark History Server
Wählen Sie die Auftrags-ID aus, und klicken Sie im Toolmenü auf **Diagnosis** (Diagnose), um die Ansicht der Auftragsdiagnose aufzurufen. Die Registerkarte „Diagnosis“ (Diagnose) umfasst die folgenden weiteren Registerkarten: **Data Skew** (Datenschiefe), **Time Skew** (Zeitabweichung) und **Executor Usage Analysis** (Analyse zur Executorauslastung).
    
+ Überprüfen Sie die **Datenschiefe**, **Zeitabweichung** und **Executor-Nutzungsanalyse**, indem Sie die entsprechenden Registerkarten auswählen.

    ![Registerkarten unter „Diagnosis“ (Diagnose)](./media/apache-azure-spark-history-server/sparkui-diagnosis-tabs.png)

### <a name="data-skew"></a>Datenschiefe
Klicken Sie auf die Registerkarte **Data Skew** (Datenschiefe). Dann werden basierend auf den angegebenen Parametern die entsprechenden schiefen Aufträge angezeigt. 

+ **Parameter festlegen:** Im ersten Abschnitt werden die Parameter angezeigt, die zum Erkennen von Datenschiefe verwendet werden. Es gilt die folgende Regel: Task-Datenlesevorgänge > 3 durchschnittliche Task-Datenlesevorgänge und Task-Datenlesevorgänge > 10 MB. Wenn Sie eine eigene Regel für Aufträge mit Datenschiefe definieren möchten, können Sie Ihre Parameter und die **Skewed Stage** (Phase mit Datenschiefe) auswählen. Dann wird der Abschnitt **Skew Chart** (Diagramm zur Datenschiefe) entsprechend aktualisiert. 

+ **Schiefe Phase:** Im zweiten Abschnitt werden die Phasen angezeigt, die Aufgaben mit Abweichungen entsprechend den oben angegebenen Kriterien enthalten. Wenn in einer Stufe mehr als ein schiefer Auftrag vorhanden ist, werden in der Tabelle mit den schiefen Aufträgen nur die schiefsten Aufträge angezeigt (z. B. der größte Datensatz für die Datenschiefe). 

    ![Abschnitt 2: Datenschiefe](./media/apache-azure-spark-history-server/sparkui-diagnosis-dataskew-section2.png)

+ **Skew Chart** (Diagramm zur Datenschiefe): Wenn eine Zeile in der Tabelle zu den Phasen mit Datenschiefe ausgewählt ist, werden im Diagramm zur Datenschiefe basierend auf den gelesenen Daten und der Ausführungszeit weitere Details zur Auftragsverteilung angezeigt. Die schiefen Aufträge sind rot und die normalen Aufträge sind blau markiert. Aus Leistungsgründen werden im Diagramm nur bis zu 100 Beispielaufträge angezeigt. Die Auftragsdetails werden im unteren Bereich auf der rechten Seite angezeigt.

    ![Abschnitt 3: Datenschiefe](./media/apache-azure-spark-history-server/sparkui-diagnosis-dataskew-section3.png)

### <a name="time-skew"></a>Zeitabweichung
Auf der Registerkarte **Time Skew** (Zeitabweichung) werden schiefe Aufträge basierend auf ihrer Ausführungszeit angezeigt. 

+ **Parameter festlegen:** Im ersten Abschnitt werden die Parameter angezeigt, die zum Erkennen der Zeitabweichung verwendet werden. Die Standardkriterien zum Erkennen von Zeitabweichungen sind: Ausführungszeit der Aufgabe > 3-mal durchschnittliche Ausführungszeit und Ausführungszeit der Aufgabe > 30 Sekunden. Sie können die Parameter Ihren Anforderungen entsprechend anpassen. Unter **Skewed Stage** (Phase mit Datenschiefe) und **Skew Chart** (Diagramm zur Datenschiefe) werden wie auf der obenstehenden Registerkarte **Data Skew** (Datenschiefe) die Informationen zu den einzelnen Phasen und Aufträgen angezeigt.

+ Klicken Sie auf **Time Skew** (Zeitabweichung). Dann werden im Abschnitt **Skewed Stage** (Phase mit Datenschiefe) entsprechend den im Abschnitt **Specify Parameters** (Parameter angeben) festgelegten Parametern die gefilterten Ergebnisse angezeigt. Klicken Sie im Abschnitt **Skewed Stage** (Phase mit Datenschiefe) auf ein Element. Dann wird in Abschnitt 3 ein Entwurf des entsprechenden Diagramms angezeigt, und die Auftragsdetails finden Sie im unteren Bereich auf der rechten Seite.

    ![Abschnitt 2: Zeitabweichung](./media/apache-azure-spark-history-server/sparkui-diagnosis-timeskew-section2.png)

### <a name="executor-usage-analysis"></a>Analyse zur Executorauslastung
Im Diagramm zur Executorauslastung wird die tatsächliche Executorzuweisung und der Ausführungsstatus der Spark-Auftrags angezeigt.  

+ Klicken Sie auf **Executor Usage Analysis** (Analyse zur Executorauslastung). Dann werden die folgenden vier Kurven zur Executorauslastung angezeigt: **Allocated Executors** (Zugewiesene Executors), **Running Executors** (Ausgeführte Executors), **Idle Executors** (Excutors im Leerlauf) und **Max Executor Instances** (Höchstanzahl der Executorinstanzen). Durch jedes Ereignis, bei dem Executors hinzugefügt oder entfernt werden, wird die Anzahl der zugewiesenen Executors erhöht bzw. verringert. Sie können sich auf der Registerkarte „Jobs“ (Aufträge) den Ereignisverlauf ansehen, falls Sie sich für weitere Vergleiche interessieren.

    ![Registerkarte „Executors“](./media/apache-azure-spark-history-server/sparkui-diagnosis-executors.png)

+ Klicken Sie auf das Farbsymbol, um den entsprechenden Inhalt in allen Entwürfen auszuwählen oder zu deaktivieren.

    ![Diagramm auswählen](./media/apache-azure-spark-history-server/sparkui-diagnosis-select-chart.png)
    
## <a name="spark--yarn-logs"></a>Spark-/Yarn-Protokolle
Die Protokolle für Spark und Yarn finden Sie neben dem Spark History Server auch hier:
* Spark-Ereignisprotokolle: hdfs:///system/spark-events
* Yarn-Protokolle: hdfs:///tmp/logs/root/logs-tfile

Hinweis: Für beide Protokolle gilt ein Standardaufbewahrungszeitraum von 7 Tagen. Informationen zum Ändern des Aufbewahrungszeitraums finden Sie auf der Seite [Konfigurieren von Apache Spark und Apache Hadoop in Big Data-Clustern](configure-spark-hdfs.md). Der Speicherort **kann nicht** geändert werden.

## <a name="known-issues"></a>Bekannte Probleme
Folgende Probleme sind für Spark History Server bekannt:

+ Die Erweiterung funktioniert derzeit für Spark 2.3-Cluster.

+ Eingabe- und Ausgabedaten werden unter Verwendung von RDD nicht auf der Registerkarte „Data“ (Daten) angezeigt.

## <a name="next-steps"></a>Nächste Schritte

* [Erste Schritte mit Big-Data-Clustern von SQL Server](./deploy-get-started.md)
* Konfigurieren von Spark-Einstellungen
* [Konfigurieren von Spark-Einstellungen](/azure/hdinsight/spark/apache-spark-settings/)