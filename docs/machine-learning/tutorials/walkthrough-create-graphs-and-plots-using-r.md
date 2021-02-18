---
title: 'R-Tutorial: Erstellen von Diagrammen und Plots'
description: Erlernen Sie Techniken zum Generieren von Plots und Karten mithilfe der Programmiersprache R mit SQL Server-Daten. Erstellen Sie ein einfaches Histogramm und anschließend einen komplexerer Kartenplot.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 06/13/2019
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: edf65095f245e5185b9eeadbd1d8b6c8e87e49cd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272951"
---
# <a name="create-graphs-and-plots-using-sql-and-r-walkthrough"></a>Erstellen von Graphen und Plots mithilfe von SQL und R (exemplarische Vorgehensweise)
[!INCLUDE [SQL Server 2016](../../includes/applies-to-version/sqlserver2016.md)]

In diesem Teil der exemplarischen Vorgehensweise lernen Sie Techniken zum Erstellen von Plots und Karten mithilfe von R und SQL Server-Daten kennen. Sie erstellen zunächst ein einfaches Histogramm und anschließend eine komplexere Kartenzeichnung.

## <a name="prerequisites"></a>Voraussetzungen

Bei diesem Schritt wird eine laufende R-Sitzung basierend auf den vorherigen Schritten in dieser Vorgehensweise angenommen. Es werden die Verbindungszeichenfolgen und Datenquellenobjekte verwendet, die in diesen Schritten erstellt wurden. Die folgenden Tools und Pakete werden zum Ausführen des Skripts verwendet.

+ Rgui.exe zum Ausführen von R-Befehlen
+ Management Studio zum Ausführen von T-SQL
+ googMap
+ ggmap-Paket
+ mapproj-Paket

## <a name="create-a-histogram"></a>Erstellen eines Histogramms

1. Erstellen Sie die erste Zeichnung mithilfe der [rxHistogram](/r-server/r-reference/revoscaler/rxdatasource) -Funktion.  Die rxHistogram-Funktion bietet ähnliche Funktionen wie Open-Source-R-Pakete, kann aber in einem Remoteausführungskontext ausgeführt werden.

    ```R
    # Plot fare amount on SQL Server and return the plot
    start.time <- proc.time()
    rxHistogram(~fare_amount, data = inDataSource, title = "Fare Amount Histogram")
    used.time <- proc.time() - start.time
    print(paste("It takes CPU Time=", round(used.time[1]+used.time[2],2), " seconds, Elapsed Time=", round(used.time[3],2), " seconds to generate plot.", sep=""))
    ```

2. Das Bild wird im Grafikgerät von R für Ihre Entwicklungsumgebung zurückgegeben.  Klicken Sie z.B. in RStudio auf das Fenster **Plot** (Diagramm).  In [!INCLUDE[rsql_rtvs](../../includes/rsql-rtvs-md.md)]wird ein separates Grafikfenster geöffnet.

    ![Verwenden von rxHistogram zum Darstellen von Fahrpreisen](media/rsql-e2e-rxhistogramresult.png "Verwenden von rxHistogram zum Darstellen von Fahrpreisen")

    > [!NOTE]
    > Sieht Ihr Graph anders aus?
    >  
    > Dies liegt daran, dass _inDataSource_ nur die ersten 1000 Zeilen verwendet. Die Reihenfolge der Zeilen, die TOP ohne eine ORDER BY-Klausel verwenden, ist nicht deterministisch. Daher ist zu erwarten, dass möglicherweise andere Daten und resultierende Graphen angezeigt werden.
    > Dieses bestimmte Bild wurde mithilfe von ca. 10.000 Datenzeilen generiert. Wir empfehlen daher, mit einer unterschiedlichen Anzahl von Zeilen zu experimentieren, um verschiedene Diagramme zu erhalten. Beachten Sie, wie lange es dauert, bis die Ergebnisse in Ihrer Umgebung zurückgegeben werden.

## <a name="create-a-map-plot"></a>Erstellen einer Kartenzeichnung

Datenbankserver blockieren in der Regel den Internetzugriff. Dies ist möglicherweise unpraktisch, wenn R-Pakete verwendet werden, die zum Generieren von Plots Karten oder andere Bilder herunterladen müssen. Es gibt jedoch eine Problemumgehung, die Sie beim Entwickeln einer eigenen Anwendung möglicherweise als hilfreich erachten. Im Prinzip generieren Sie die Kartendarstellung auf dem Client und legen anschließend die als Attribute in der SQL Server-Tabelle gespeicherten Punkte über die Karte.

1. Definieren Sie die Funktion, die das R-Plotobjekt erstellt. Die benutzerdefinierte Funktion *mapPlot* erstellt ein Punktdiagramm, das die Abholorte des Taxis verwendet und die Anzahl der Fahrten von jedem Abholort aus zeichnet. Die Funktion verwendet die Pakete **ggplot2** und **ggmap**, die bereits [installiert und geladen](walkthrough-data-science-end-to-end-walkthrough.md#add-packages) sein müssen.

    ```R
    mapPlot <- function(inDataSource, googMap){
        library(ggmap)
        library(mapproj)
        ds <- rxImport(inDataSource)
        p <- ggmap(googMap)+
        geom_point(aes(x = pickup_longitude, y =pickup_latitude ), data=ds, alpha =.5,
    color="darkred", size = 1.5)
        return(list(myplot=p))
    }
    ```

    + Die benutzerdefinierte Funktion *mapPlot* hat zwei Argumente: ein vorhandenes Datenobjekt, das Sie zuvor mit RxSqlServerData definiert haben, sowie die Kartendarstellung, die vom Client übergeben wurde.
    + In der Zeile, die mit der Variablen *ds* beginnt, wird rxImport verwendet, um Daten aus der zuvor erstellten Datenquelle *inDataSource* in den Arbeitsspeicher zu laden. (Diese Datenquelle enthält 1000 Datensätze. Wenn Sie eine Karte mit mehr Datenpunkten erstellen möchten, können Sie eine andere Datenquelle ersetzen.)
    + Wenn Sie Open Source-Funktionen von R verwenden, müssen Daten in Datenrahmen im lokalen Arbeitsspeicher geladen werden. Durch Aufruf der Funktion [rxImport](/r-server/r-reference/revoscaler/rximport) kann die Ausführung im Arbeitsspeicher des Remotecomputekontexts erfolgen.

2. Ändern Sie den Computekontext in „lokal“, und laden Sie die zum Erstellen der Karten erforderlichen Bibliotheken.

    ```R
    rxSetComputeContext("local")
    library(ggmap)
    library(mapproj)
    gc <- geocode("Times Square", source = "google")
    googMap <- get_googlemap(center = as.numeric(gc), zoom = 12, maptype = 'roadmap', color = 'color');
    ```

    + Die Variable `gc` speichert mehrere Koordinaten für den Times Square, NY.

    + Die Zeile, die mit `googmap` beginnt, generiert eine Karte mit den angegebenen Koordinaten in der Mitte.

3. Wechseln Sie zum SQL Server-Computekontext, und rendern Sie die Ergebnisse, indem Sie die Plotfunktion in [rxExec](/r-server/r-reference/revoscaler/rxexec) wie hier dargestellt umschließen. Die rxExec-Funktion wird im **RevoScaleR**-Paket eingeschlossen und unterstützt die Ausführung beliebiger R-Funktionen in einem Remotecomputekontext.

    ```R
    rxSetComputeContext(sqlcc)
    myplots <- rxExec(mapPlot, inDataSource, googMap, timesToRun = 1)
    plot(myplots[[1]][["myplot"]]);
    ````

    + Die Kartendaten in `googMap` werden als Argument an die remote ausgeführte *mapPlot*-Funktion übergeben. Da die Karten in Ihrer lokalen Umgebung generiert wurden, müssen sie an die Funktion übergeben werden, damit der Plot im Kontext von SQL Server erstellt werden kann.

    + Wenn die Zeile, die mit `plot` beginnt, ausgeführt wird, werden die gerenderten Daten serialisiert in die lokale R-Umgebung zurückgeschrieben, sodass Sie sie in Ihrem R-Client anzeigen können.

    > [!NOTE]
    > Wenn Sie SQL Server in einem virtuellen Azure-Computer verwenden, wird an dieser Stelle möglicherweise ein Fehler angezeigt. Dies ist dann der Fall, wenn die standardmäßige Firewallregel in Azure den Netzwerkzugriff durch R-Code blockiert. Ausführliche Informationen zum Beheben dieses Fehlers finden Sie unter [Installieren von Machine Learning (R) Services auf einer Azure-VM](../install/sql-machine-learning-azure-virtual-machine.md).

4. In der folgenden Abbildung ist das ausgegebene Diagramm dargestellt. Die Taxi-Abholorte wurden zur Karte als rote Punkte hinzugefügt. Je nachdem, wie viele Abholorte in der verwendeten Datenquelle gespeichert sind, sieht Ihre Abbildung möglicherweise anders aus.

    ![Zeichnen von Taxifahrten mithilfe einer benutzerdefinierten R-Funktion](media/rsql-e2e-mapplot.png "Zeichnen von Taxifahrten mithilfe einer benutzerdefinierten R-Funktion")

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen von Datenfunktionen mit R und SQL](walkthrough-create-data-features.md)