---
title: 'R-Tutorial: Untersuchen und Visualisieren von Daten'
titleSuffix: SQL machine learning
description: Untersuchen Sie Beispieldaten, und generieren Sie einige Plots als Vorbereitung für die Verwendung der binären Klassifizierung in R mit SQL Machine Learning.
ms.prod: sql
ms.technology: machine-learning
ms.date: 10/15/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||>=azuresqldb-mi-current'
ms.openlocfilehash: 8e2e8994f92bfb96fa66c2c6a08c7db198902b45
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354208"
---
# <a name="r-tutorial-explore-and-visualize-data"></a>R-Tutorial: Untersuchen und Visualisieren von Daten
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

Im zweiten Teil dieser fünfteiligen Tutorialreihe werden Sie die Beispieldaten untersuchen und einige Plots erstellen. Im späteren Verlauf erfahren Sie, wie Sie Grafikobjekte in Python serialisieren, anschließend deserialisieren und Plots erstellen.

Im zweiten Teil dieser fünfteiligen Tutorialreihe überprüfen Sie die Beispieldaten und generieren dann einige Plots mit den generischen Funktionen `barplot` und `hist` in Basis-R.

Ein wichtiges Ziel dieses Artikels besteht darin, das Aufrufen von R-Funktionen von [!INCLUDE[tsql](../../includes/tsql-md.md)] in gespeicherten Prozeduren zu veranschaulichen und die Ergebnisse in Anwendungsdateiformaten zu speichern:

+ Erstellen Sie eine gespeicherte Prozedur mithilfe von `barplot`, um einen R-Plot als varbinary-Daten zu generieren. Verwenden Sie **bcp**, um den binären Stream in eine Bilddatei zu exportieren.
+ Erstellen Sie mithilfe von `hist` eine gespeicherte Prozedur, um einen Plot zu generieren. Die Ergebnisse werden als JPG- und PDF-Ausgabe gespeichert.

> [!NOTE]
> Da die Visualisierung ein leistungsfähiges Tool zum Verständnis der Datenform und -verteilung ist, bietet R zahlreiche Funktionen und Pakete zum Generieren von Histogrammen, Streudiagrammen, Boxplots und anderen Diagrammen zum Durchsuchen von Daten. R erstellt Bilder üblicherweise mithilfe eines R-Geräts für grafische Ausgaben, die Sie als **varbinary**-Datentyp für das Rendern in Anwendungen erfassen und speichern können. Sie können die Bilder auch in einem beliebigen unterstützten Dateiformat (z. B. JPG oder PDF) speichern.

In diesem Artikel führen Sie Folgendes durch:

> [!div class="checklist"]
> + Überprüfen der Beispieldaten
> + Erstellen von Plots mit R in T-SQL
> + Ausgeben von Plots in mehreren Dateiformaten

In [Teil 1](r-taxi-classification-introduction.md) haben Sie die Voraussetzungen installiert und die Beispieldatenbank wiederhergestellt.

In [Teil drei](r-taxi-classification-create-features.md) erfahren Sie, wie Sie mithilfe einer Transact-SQL-Funktion aus Rohdaten Features erstellen. Sie rufen anschließend die Funktion aus einer gespeicherten Prozedur auf, um eine Tabelle zu erstellen, die die Funktionswerte enthält.

In [Teil vier](r-taxi-classification-train-model.md) laden Sie die Module und rufen die erforderlichen Funktionen auf, um das Modell mithilfe einer gespeicherten SQL Server-Prozedur zu erstellen und zu trainieren.

In Teil fünf erfahren Sie, wie Sie die Modelle [operationalisieren](r-taxi-classification-deploy-model.md) können, die Sie in Teil vier trainiert und gespeichert haben.

## <a name="review-the-data"></a>Überprüfen der Daten

Das Entwickeln einer Data Science-Lösung bringt normalerweise die intensive Untersuchung und Visualisierung von Daten mit sich. Nehmen Sie sich zunächst ein paar Minuten Zeit, um die Beispieldaten zu überprüfen, falls Sie das noch nicht getan haben.

Im ursprünglichen öffentlichen Dataset wurden die Taxi-IDs und die Fahrtendatensätze in separaten Dateien bereitgestellt. Die beiden ursprünglichen Datasets wurden in den Spalten _medallion_, _hack\_license_ und _pickup\_datetime_ zusammengefügt, damit die Beispieldaten einfacher verwendet werden können.  Die Datensätze wurden ebenso auf Stichproben reduziert, um nur 1 % der ursprünglichen Anzahl der Datensätze zu erhalten. Der auf Stichproben reduzierte Datensatz hat 1.703.957 Zeilen und 23 Spalten.

**Taxi-IDs**
  
+ Die Spalte _medallion_ stellt die eindeutige ID-Nummer des Taxis dar.
  
+ Die Spalte _hack\_license_ enthält die (anonymisierte) Lizenznummer des Taxifahrers.
  
**Datensätze von Fahrten und Fahrpreisen**
  
+ Jeder Datensatz für Fahrten enthält den genauen Abhol- und Zielort und die Zeit sowie die Fahrstrecke.
  
+ Jeder Fahrpreisdatensatz enthält die Zahlungsinformationen wie die Zahlungsart, der Gesamtbetrag und den Fahrtpreis.
  
+ Die letzten drei Spalten können für verschiedene Machine Learning-Tasks verwendet werden. Die Spalte _tip\_amount_ enthält fortlaufende numerische Werte und kann als **label**-Spalte für die Regressionsanalyse verwendet werden. Die Spalte _tipped_ verfügt nur über Ja/Nein-Werte und wird für die binäre Klassifikation verwendet. Die _tip\_class_-Spalte weist mehrere **Klassenbezeichnungen** auf und kann deshalb als Bezeichnung für mehrklassige Klassifizierungsaufgaben verwendet werden.
  
  Diese exemplarische Vorgehensweise enthält nur die binäre Klassifizierungsaufgabe. Sie können gerne versuchen, Modelle für die anderen beiden Machine Learning-Tasks und für mehrklassige Klassifizierung zu erstellen.
  
+ Die Werte für die Bezeichnungsspalten basieren alle auf der _tip\_amount_-Spalte und verwenden folgende Geschäftsregeln:
  
  |Name der abgeleiteten Spalte|Regel|
  |-|-|
  |tipped|If tip_amount > 0, tipped = 1, otherwise tipped = 0|
  |tip_class|Class 0: tip_amount = $0<br /><br />Class 1: tip_amount > $0 and tip_amount <= $5<br /><br />Class 2: tip_amount > $5 and tip_amount <= $10<br /><br />Class 3: tip_amount > $10 and tip_amount <= $20<br /><br />Class 4: tip_amount > $20|

## <a name="create-plots-using-r-in-t-sql"></a>Erstellen von Plots mit R in T-SQL

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
> [!IMPORTANT]
> Ab SQL Server 2019 erfordert der Isolationsmechanismus, dass Sie für das Verzeichnis, in dem die Plotdatei gespeichert ist, entsprechende Berechtigungen erteilen. Informationen zum Festlegen dieser Berechtigungen finden Sie im [Abschnitt zu Dateiberechtigungen unter „SQL Server 2019 unter Windows: Isolationsänderungen für Machine Learning Services“](../install/sql-server-machine-learning-services-2019.md#file-permissions).
::: moniker-end

Um den Plot zu erstellen, verwenden Sie die R-Funktion `barplot`. In diesem Schritt wird ein Histogramm anhand der Daten aus einer [!INCLUDE[tsql](../../includes/tsql-md.md)]-Abfrage gezeichnet. Sie können diese Funktion in eine gespeicherte Prozedur wie **RPlotHistogram** einschließen.

1. Klicken Sie in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] im Objekt-Explorer mit der rechten Maustaste auf die Datenbank **NYCTaxi_Sample**, und wählen Sie **Neue Abfrage** aus. Alternativ können Sie in Azure Data Studio im Menü **Datei** die Option **Neues Notebook** auswählen und eine Verbindung mit der Datenbank herstellen.

2. Fügen Sie das folgende Skript ein, um eine gespeicherte Prozedur zu erstellen, die das Histogramm zeichnet. Dieses Beispiel heißt **RPlotHistogram**.

   ```sql
   CREATE PROCEDURE [dbo].[RPlotHistogram]
   AS
   BEGIN
     SET NOCOUNT ON;
     DECLARE @query nvarchar(max) =  
     N'SELECT tipped FROM [dbo].[nyctaxi_sample]'  
     EXECUTE sp_execute_external_script @language = N'R',  
                                        @script = N'  
      image_file = tempfile();  
      jpeg(filename = image_file);  
      #Plot histogram  
      barplot(table(InputDataSet$tipped), main = "Tip Histogram", col="lightgreen", xlab="Tipped or not", ylab = "Counts", space=0)
      dev.off();  
      OutputDataSet <- data.frame(data=readBin(file(image_file, "rb"), what=raw(), n=1e6));  
      ',  
      @input_data_1 = @query  
      WITH RESULT SETS ((plot varbinary(max)));  
   END
   GO
   ```

Folgende Punkte sind bei diesem Skript besonders wichtig:
  
+ Die Variable `@query` definiert den Abfragetext (`'SELECT tipped FROM nyctaxi_sample'`), der an das R-Skript als das Argument für die Skripteingabevariable, `@input_data_1`, übergeben wird. Wenn R-Skripts als externe Prozesse ausgeführt werden, sollte eine 1:1-Zuordnung zwischen den Eingaben in Ihr Skript und den Ausgaben in die gespeicherte Systemprozedur [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) bestehen, die die R-Sitzung in SQL Server startet.
  
+ Innerhalb des R-Skripts wird die Variable `image_file` definiert, um das Bild zu speichern.

+ Die `barplot`-Funktion wird aufgerufen, um den Plot zu generieren.
  
+ Das R-Gerät wird auf **off** festgelegt, da Sie diesen Befehl als externes Skript in SQL Server ausführen. Wenn Sie in R einen Zeichenbefehl für hohe Ebenen ausgeben, öffnet R in der Regel ein Grafikfenster, das als *device* bezeichnet wird. Sie können das device-Fenster deaktivieren, wenn Sie in eine Datei schreiben oder die Ausgabe anders verarbeiten.
  
+ Das R-Grafikobjekt wird zu einem R-Datenrahmen für die Ausgabe serialisiert.

### <a name="execute-the-stored-procedure-and-use-bcp-to-export-binary-data-to-an-image-file"></a>Ausführen der gespeicherten Prozedur und Verwenden von bcp zum Exportieren von Binärdaten in eine Bilddatei

Die gespeicherte Prozedur gibt das Bild als Strom von varbinary-Daten zurück, die Sie natürlich nicht direkt anzeigen können. Sie können jedoch das Hilfsprogramm **bcp** verwenden, um die varbinary-Daten abzurufen und sie als Bilddatei auf einem Clientcomputer zu speichern.
  
1. Führen Sie die folgende Anweisung in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]aus:
  
    ```sql
    EXEC [dbo].[RPlotHistogram]
    ```
  
    **Ergebnisse**
    
    *plot* *0xFFD8FFE000104A4649...*
  
2. Öffnen Sie eine PowerShell-Eingabeaufforderung, führen Sie den folgenden Befehl aus, und stellen Sie den erforderlichen Instanznamen, Datenbanknamen, Benutzernamen und die Anmeldeinformationen als Argumente bereit. Bei Windows-Identitäten können Sie **-U** und **-P** durch **-T** ersetzen.
  
   ```powershell
   bcp "exec RPlotHistogram" queryout "plot.jpg" -S <SQL Server instance name> -d  NYCTaxi_Sample  -U <user name> -P <password> -T
   ```

   > [!NOTE]
   > Bei Befehlsoptionen für bcp wird die Groß- und Kleinschreibung beachtet.
  
3. Wenn die Verbindung erfolgreich hergestellt wurde, werden Sie dazu aufgefordert, weitere Informationen über das Format der Grafikdatei einzugeben.

   Drücken Sie bei jeder Eingabeaufforderung die EINGABETASTE, um die bestehenden Angaben zu akzeptieren, außer der folgenden Änderungen:

   + Geben Sie für **prefix-length of field plot** 0 ein
  
   + Geben Sie **Y** ein, wenn Sie die Ausgabeparameter für den späteren Gebrauch speichern möchten.
  
   ```text
   Enter the file storage type of field plot [varbinary(max)]: 
   Enter prefix-length of field plot [8]: 0
   Enter length of field plot [0]:
   Enter field terminator [none]:
  
   Do you want to save this format information in a file? [Y/n]
   Host filename [bcp.fmt]:
   ```
  
   **Ergebnisse**

   ```text
   Starting copy...
   1 rows copied.
   Network packet size (bytes): 4096
   Clock Time (ms.) Total     : 3922   Average : (0.25 rows per sec.)
   ```

   > [!TIP]
   > Wenn Sie die Formatinformationen in einer Datei (bcp.fmt) speichern, generiert das Hilfsprogramm **bcp** eine Formatdefinition, die Sie zukünftig auf ähnliche Befehle anwenden können, ohne zur Eingabe von Formatoptionen für Grafikdateien aufgefordert zu werden. Fügen Sie zum Verwenden der Formatdatei `-f bcp.fmt` an das Ende einer beliebigen Befehlszeile nach dem Argument „Kennwort“ ein.
  
4. Die Ausgabedatei wird im gleichen Verzeichnis erstellt, in dem Sie den PowerShell-Befehl ausgeführt haben. Um die Grafik anzuzeigen, öffnen Sie einfach die Datei plot.jpg.
  
   ![Taxifahrten mit und ohne Trinkgeld](media/rsql-devtut-tippedornot.jpg "Taxifahrten mit und ohne Trinkgeld")  
  
## <a name="create-a-stored-procedure-using-hist"></a>Erstellen einer gespeicherten Prozedur mit `hist`

In der Regel generieren Data Scientists mehrere Datenvisualisierungen, um Einblicke in die Daten aus verschiedenen Perspektiven zu erhalten. In diesem Beispiel erstellen Sie eine gespeicherte Prozedur mit dem Namen **RPlotHist**, um Histogramme, Streudiagramme und andere R-Grafiken in das JPG- und PDF-Format zu schreiben.

Diese gespeicherte Prozedur verwendet die `hist`-Funktion, um das Histogramm zu erstellen, wobei die Binärdaten in beliebte Formate wie JPG, PDF und PNG exportiert werden.

1. Klicken Sie in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] im Objekt-Explorer mit der rechten Maustaste auf die Datenbank **NYCTaxi_Sample**, und wählen Sie **Neue Abfrage** aus.

2. Fügen Sie das folgende Skript ein, um eine gespeicherte Prozedur zu erstellen, die das Histogramm zeichnet. Dieses Beispiel heißt **RPlotHist**.
  
   ```sql
   CREATE PROCEDURE [dbo].[RPlotHist]  
   AS  
   BEGIN  
     SET NOCOUNT ON;  
     DECLARE @query nvarchar(max) =  
     N'SELECT cast(tipped as int) as tipped, tip_amount, fare_amount FROM [dbo].[nyctaxi_sample]'  
     EXECUTE sp_execute_external_script @language = N'R',  
     @script = N'  
      # Set output directory for files and check for existing files with same names   
       mainDir <- ''C:\\temp\\plots''  
       dir.create(mainDir, recursive = TRUE, showWarnings = FALSE)  
       setwd(mainDir);  
       print("Creating output plot files:", quote=FALSE)
       
       # Open a jpeg file and output histogram of tipped variable in that file.  
       dest_filename = tempfile(pattern = ''rHistogram_Tipped_'', tmpdir = mainDir)  
       dest_filename = paste(dest_filename, ''.jpg'',sep="")  
       print(dest_filename, quote=FALSE);  
       jpeg(filename=dest_filename);  
       hist(InputDataSet$tipped, col = ''lightgreen'', xlab=''Tipped'',   
           ylab = ''Counts'', main = ''Histogram, Tipped'');  
        dev.off();  

       # Open a pdf file and output histograms of tip amount and fare amount.   
       # Outputs two plots in one row  
       dest_filename = tempfile(pattern = ''rHistograms_Tip_and_Fare_Amount_'', tmpdir = mainDir)  
       dest_filename = paste(dest_filename, ''.pdf'',sep="")  
       print(dest_filename, quote=FALSE);  
       pdf(file=dest_filename, height=4, width=7);  
       par(mfrow=c(1,2));  
       hist(InputDataSet$tip_amount, col = ''lightgreen'',   
           xlab=''Tip amount ($)'',   
           ylab = ''Counts'',   
           main = ''Histogram, Tip amount'', xlim = c(0,40), 100);  
       hist(InputDataSet$fare_amount, col = ''lightgreen'',   
           xlab=''Fare amount ($)'',   
           ylab = ''Counts'',   
           main = ''Histogram,   
           Fare amount'',   
           xlim = c(0,100), 100);  
       dev.off();  
  
       # Open a pdf file and output an xyplot of tip amount vs. fare amount using lattice;  
       # Only 10,000 sampled observations are plotted here, otherwise file is large.  
       dest_filename = tempfile(pattern = ''rXYPlots_Tip_vs_Fare_Amount_'', tmpdir = mainDir)  
       dest_filename = paste(dest_filename, ''.pdf'',sep="")  
       print(dest_filename, quote=FALSE);  
       pdf(file=dest_filename, height=4, width=4);  
       plot(tip_amount ~ fare_amount,   
           data = InputDataSet[sample(nrow(InputDataSet), 10000), ],   
           ylim = c(0,50),   
           xlim = c(0,150),   
           cex=.5,   
           pch=19,   
           col=''darkgreen'',    
           main = ''Tip amount by Fare amount'',   
           xlab=''Fare Amount ($)'',   
           ylab = ''Tip Amount ($)'');   
       dev.off();',  
     @input_data_1 = @query  
   END
   ```
  
Folgende Punkte sind bei diesem Skript besonders wichtig:

+ Die Ausgabe der SELECT-Abfrage in der gespeicherten Prozedur wird im Standarddatenrahmen von R, `InputDataSet`, gespeichert. Anschließend können verschiedene Zeichenfunktionen von R aufgerufen werden, um die tatsächlichen Grafikdateien zu generieren. Die meisten der eingebetteten R-Skripts stellen Optionen für diese Grafikfunktionen dar, z.B. `plot` oder `hist`.
  
+ Das R-Gerät wird auf **off** festgelegt, da Sie diesen Befehl als externes Skript in SQL Server ausführen. Wenn Sie in R einen Zeichenbefehl für hohe Ebenen ausgeben, öffnet R in der Regel ein Grafikfenster, das als *device* bezeichnet wird. Sie können das device-Fenster deaktivieren, wenn Sie in eine Datei schreiben oder die Ausgabe anders verarbeiten.
  
+ Alle Dateien werden im lokalen Ordner C:\temp\Plots gespeichert. Der Zielordner wird durch die Argumente für das R-Skript als Teil der gespeicherten Prozedur definiert. Um die Dateien in einem anderen Ordner auszugeben, ändern Sie den Wert der `mainDir` -Variable im R-Skript, das in der gespeicherten Prozedur gespeichert ist. Sie können auch das Skript ändern, damit es verschiedene Formate, mehr Dateien usw. ausgibt.

### <a name="execute-the-stored-procedure"></a>Ausführen der gespeicherten Prozedur

Führen Sie die folgende Anweisung aus, um binäre Plotdaten in JPEG- und PDF-Dateiformate zu exportieren.

```sql
EXEC RPlotHist
```

**Ergebnisse**

```text
STDOUT message(s) from external script:
[1] Creating output plot files:[1] C:\temp\plots\rHistogram_Tipped_18887f6265d4.jpg[1] 

C:\temp\plots\rHistograms_Tip_and_Fare_Amount_1888441e542c.pdf[1]

C:\temp\plots\rXYPlots_Tip_vs_Fare_Amount_18887c9d517b.pdf
```

Die Zahlen in den Dateinamen werden nach dem Zufallsprinzip generiert, um sicherzustellen, dass kein Fehler auftritt, weil Sie in eine bereits vorhandene Datei schreiben.

### <a name="view-output"></a>Anzeigen der Ausgabe

Öffnen Sie den Zielordner, und überprüfen Sie die Dateien, die vom R-Code in der gespeicherten Prozedur erstellt wurden, um den Plot anzuzeigen.

1. Wechseln Sie zum in der STDOUT-Meldung angegebenen Ordner (in diesem Beispiel C:\temp\plots\).

2. Öffnen Sie `rHistogram_Tipped.jpg`, um die Anzahl der Fahrten mit und ohne Trinkgeld anzuzeigen (dieses Histogramm ähnelt dem, das Sie im vorherigen Schritt generiert haben).

3. Öffnen Sie `rHistograms_Tip_and_Fare_Amount.pdf`, um die Verteilung des Trinkgeldbetrags im Vergleich zum Fahrpreis anzuzeigen.

   ![Histogramm mit Trinkgeldbetrag und Fahrpreis](media/rsql-devtut-tipamtfareamt.PNG "Histogramm mit Trinkgeldbetrag und Fahrpreis")

4. Öffnen Sie `rXYPlots_Tip_vs_Fare_Amount.pdf`, um ein Punktdiagramm mit der Höhe des Fahrpreises auf der X-Achse und der Höhe des Trinkgelds auf der Y-Achse anzuzeigen.

   ![Trinkgeldbetrag über dem Fahrpreis](media/rsql-devtut-tipamtbyfareamt.PNG "Trinkgeldbetrag über dem Fahrpreis")

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel führen Sie folgende Schritte aus:

> [!div class="checklist"]
> + Sie haben die Beispieldaten überprüft.
> + Sie haben mithilfe von R in T-SQL Plots erstellt.
> + Ausgeben von Plots in mehreren Dateiformaten

> [!div class="nextstepaction"]
> [R-Tutorial: Erstellen von Datenfeatures](r-taxi-classification-create-features.md)