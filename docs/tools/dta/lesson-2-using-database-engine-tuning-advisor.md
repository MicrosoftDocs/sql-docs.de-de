---
title: Verwenden des Datenbankoptimierungsratgebers
description: Hier erfahren Sie, wie die GUI des SQL Server-Datenbankoptimierungsratgebers Datenbanken optimiert, Optimierungssitzungen verwaltet und Optimierungsempfehlungen anzeigt.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Database Engine [SQL Server], tutorials
ms.assetid: 3317d4f8-ed9e-4f2e-b5f1-a6bf3a9d6c8d
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 10d32bb3267e79f4f97ce7a4832c1a999751904c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353515"
---
# <a name="lesson-2-using-database-engine-tuning-advisor"></a>Lektion 2: Verwenden des Datenbankoptimierungsratgebers

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Der Datenbankoptimierungsratgeber ermöglicht Ihnen das Optimieren von Datenbanken, Verwalten von Optimierungssitzungen und das Anzeigen von Empfehlungen zur Optimierung. Benutzer, die erweiterte Kenntnisse in physischen Entwurfsstrukturen haben, können mit diesem Tool eine Analyse der Datenbankoptimierung ausführen. Benutzer, die mit der Datenbankoptimierung noch nicht vertraut sind, können mithilfe dieses Tools die beste Konfiguration physischer Entwurfsstrukturen für die Arbeitsauslastung herausfinden, die sie optimieren. Diese Lektion umfasst grundlegende, praktische Hinweise für Datenbankadministratoren, die mit der grafischen Benutzeroberfläche des Datenbankoptimierungsratgebers noch nicht vertraut sind, sowie für Systemadministratoren, die möglicherweise keine umfassenden Kenntnisse physischer Entwurfsstrukturen haben.  

## <a name="prerequisites"></a>Voraussetzungen 

Zur Durchführung dieses Tutorials benötigen Sie SQL Server Management Studio, Zugriff auf einen Server, auf dem SQL-Server ausgeführt wird, und eine AdventureWorks-Datenbank.

- Installieren Sie [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md).
- Installieren Sie die [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads).
- Laden Sie die [AdventureWorks 2017-Beispieldatenbank](../../samples/adventureworks-install-configure.md) herunter.


Anweisungen zum Wiederherstellen von Datenbanken in SSMS finden Sie hier: [Wiederherstellen einer Datenbank](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md).

  >[!NOTE]
  > Dieses Tutorial richtet sich an Benutzer, die mit der Verwendung von SQL Server Management Studio und mit grundlegenden Datenbankverwaltungsaufgaben vertraut sind. 
  
## <a name="tuning-a-workload"></a>Optimieren einer Arbeitsauslastung
Der Datenbankoptimierungsratgeber dient dazu, den optimalen Entwurf für eine physische Datenbank hinsichtlich der Abfrageleistung für die Datenbanken zu ermitteln, die Sie für die Optimierung auswählen.  

1.  Kopieren Sie eine Beispiel-[SELECT](../../t-sql/queries/select-examples-transact-sql.md)-Anweisung, und fügen Sie diese in den Abfrage-Editor von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ein. Speichern Sie die Datei unter dem Namen **MyScript.sql** in einem Verzeichnis, in dem Sie sie leicht wieder auffinden. Im Folgenden finden Sie ein Beispiel unter Verwendung der AdventureWorks2017-Datenbank.  

 ```sql
 Use [Adventureworks2017]; -- may need to modify database name to match database
 GO
 SELECT DISTINCT pp.LastName, pp.FirstName 
 FROM Person.Person pp JOIN HumanResources.Employee e
 ON e.BusinessEntityID = pp.BusinessEntityID WHERE pp.BusinessEntityID IN 
 (SELECT SalesPersonID 
 FROM Sales.SalesOrderHeader
 WHERE SalesOrderID IN 
 (SELECT SalesOrderID 
 FROM Sales.SalesOrderDetail
 WHERE ProductID IN 
 (SELECT ProductID 
 FROM Production.Product p 
 WHERE ProductNumber = 'BK-M68B-42')));
 GO
 ```

  ![Speichern der SQL-Abfrage](media/dta-tutorials/dta-save-query.png)
  
2.  Starten Sie den Datenbankoptimierungsratgeber. Wählen Sie in SQL Server Management Studio (SSMS) im Menü **Tools** den **Datenbankoptimierungsratgeber** aus.  Weitere Informationen finden Sie unter [Starten eines Datenbankoptimierungsratgebers](lesson-1-basic-navigation-in-database-engine-tuning-advisor.md#launch-database-tuning-advisor). Stellen Sie über das Dialogfeld **Verbindung mit Server herstellen** eine Verbindung mit Ihrer SQL Server-Instanz her.  
  
3.  Geben Sie auf der Registerkarte **Allgemein** im rechten Bereich der GUI des Datenbankoptimierungsratgebers im Feld **Sitzungsname** den Namen **MySession** ein. 
  
4.  Wählen Sie **Datei** für Ihre **Arbeitsauslastung** aus, und klicken Sie auf das Fernglassymbol, um **nach einer Arbeitsauslastungsdatei zu suchen**. Suchen Sie nach der Datei **MyScript.sql**, die Sie in Schritt 1 gespeichert haben.  

   ![Suchen nach dem zuvor gespeicherten Skript](media/dta-tutorials/dta-script.png)
  
5.  Wählen Sie in der Liste **Datenbank für Arbeitsauslastungsanalyse** den Eintrag „AdventureWorks2017“ aus, wählen Sie dann im Raster **Zu optimierende Datenbanken und Tabellen auswählen** den Eintrag „AdventureWorks2017“ aus, und belassen Sie **Optimierungsprotokoll speichern** aktiviert. **Datenbank für Arbeitsauslastungsanalyse** gibt die erste Datenbank an, mit der der Datenbankoptimierungsratgeber beim Optimieren einer Arbeitsauslastung eine Verbindung herstellt. Nach dem Beginn der Optimierung stellt der Datenbankoptimierungsratgeber Verbindungen mit den Datenbanken her, die über die `USE DATABASE` -Anweisungen in der Arbeitsauslastung angegeben sind.  

  ![Optionen des Datenbankoptimierungsratgebers für die Datenbank](media/dta-tutorials/dta-select-db.png)
  
6.  Klicken Sie auf die Registerkarte **Optimierungsoptionen** . In dieser Übung werden Sie keine Optimierungsoptionen festlegen. Aber nehmen Sie sich die Zeit, und überprüfen Sie die Standardoptimierungsoptionen. Drücken Sie F1, um die Hilfe zu dieser Seite im Registerformat anzuzeigen. Klicken Sie auf **Erweiterte Optionen** , um weitere Optimierungsoptionen anzuzeigen. Klicken Sie im Dialogfeld **Erweiterte Optimierungsoptionen** auf **Hilfe** , um weitere Informationen zu den angezeigten Optimierungsoptionen aufzurufen. Klicken Sie auf **Abbrechen** , um das Dialogfeld **Erweiterte Optimierungsoptionen** zu schließen und die Standardoptionen beizubehalten.  

  ![DTA-Optimierungsoptionen](media/dta-tutorials/dta-tuning-options.png)
  
7.  Klicken Sie auf der Symbolleiste auf die Schaltfläche **Analyse starten** . Während der Datenbankoptimierungsratgeber die Arbeitsauslastung analysiert, können Sie den Status auf der Registerkarte **Fortschritt** überwachen. Wenn das Optimieren abgeschlossen ist, wird die Registerkarte **Empfehlungen** angezeigt.  
  
    Wenn Sie einen Fehler zum Enddatum und zur Beendigungszeit für die Optimierung erhalten, aktivieren Sie das Kontrollkästchen **Beenden am** auf der Registerkarte **Optimierungsoptionen** . Überprüfen Sie, ob die in **Beenden am** für Datum und Uhrzeit angegebenen Werte größer sind als das aktuelle Datum und die aktuelle Uhrzeit, und ändern Sie die Werte nach Bedarf.  

  ![Starten der Analyse des Datenbankoptimierungsratgebers](media/dta-tutorials/dta-start-analysis.png)

  
8.  Speichern Sie die Empfehlungen nach Ende der Analyse als [!INCLUDE[tsql](../../includes/tsql-md.md)] -Skript. Klicken Sie dazu im Menü **Aktionen** auf **Empfehlungen speichern** . Navigieren Sie im Dialogfeld **Speichern unter** zu dem Verzeichnis, in dem das Skript mit Empfehlungen gespeichert werden soll, und geben Sie als Dateinamen **MyRecommendations** an.  

  ![Speichern der Empfehlungen des Datenbankoptimierungsratgebers](media/dta-tutorials/dta-save-recommendations.png)

## <a name="view-tuning-recommendations"></a>Optimierungsempfehlungen anzeigen
  
1.  Verwenden Sie auf der Registerkarte **Empfehlungen** die Bildlaufleiste unten auf der Seite im Registerformat, um alle Spalten zu **Indexempfehlungen** anzuzeigen. Jede Zeile steht für ein Datenbankobjekt (Indizes oder indizierte Sichten), für das der [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Optimierungsratgeber die Empfehlung abgibt, es zu löschen oder anzulegen. Führen Sie einen Bildlauf zur Spalte ganz rechts durch, und klicken Sie auf **Definition**. [!INCLUDE[ssDE](../../includes/ssde-md.md)] Der Optimierungsratgeber zeigt das Fenster **SQL-Skriptvorschau** an, in dem das [!INCLUDE[tsql](../../includes/tsql-md.md)] -Skript angezeigt werden kann, das das Datenbankobjekt in dieser Zeile anlegt oder löscht. Klicken Sie auf **Schließen** , um das Vorschaufenster zu schließen.  
  
    Wenn Sie Probleme haben, eine **Definition** zu finden, die einen Link enthält, klicken Sie am unteren Rand der Seite im Registerformat auf das Kontrollkästchen **Vorhandene Objekte anzeigen** , um es zu deaktivieren. Damit wird die Anzahl dargestellter Zeilen reduziert. Wenn Sie das Kontrollkästchen deaktivieren, zeigt der [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Optimierungsratgeber nur die Objekte an, für die eine Empfehlung generiert wurde. Aktivieren Sie das Kontrollkästchen **Vorhandene Objekte anzeigen** , um alle Datenbankobjekte anzuzeigen, die derzeit in der [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] -Datenbank vorhanden sind. Zum Anzeigen aller Objekte verwenden Sie die Bildlaufleiste rechts auf der Seite im Registerformat.

  ![Indexempfehlung des Datenbankoptimierungsratgebers](media/dta-tutorials/dta-recommendation.png)  
  
2.  Klicken Sie mit der rechten Maustaste auf das Raster im Bereich **Indexempfehlungen** . Im daraufhin angezeigten Kontextmenü können Sie Empfehlungen auswählen oder deren Auswahl aufheben. Außerdem können Sie die Schriftart des Rastertexts ändern.  
 
   ![Auswahlmenü für die Indexempfehlung](media/dta-tutorials/dta-index-recommendation-options.png)
  
3.  Klicken Sie im Menü **Aktionen** auf **Empfehlungen speichern** , um alle Empfehlungen in einem [!INCLUDE[tsql](../../includes/tsql-md.md)] -Skript zu speichern. Weisen Sie dem Skript den Namen **MySessionRecommendations.sql** zu.  
  
    Öffnen Sie das Skript MySessionRecommendations.sql im Abfrage-Editor von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , um es anzuzeigen. Sie könnten jetzt die Empfehlungen auf die Beispieldatenbank [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] anwenden, indem Sie das Skript im Abfrage-Editor ausführen, aber tun Sie das jetzt nicht. Schließen Sie das Skript im Abfrage-Editor, ohne es auszuführen.  
  
    Optional können Sie die Empfehlungen auch anwenden, indem Sie im Menü **Aktionen** des **-Optimierungsratgebers auf** Empfehlungen anwenden [!INCLUDE[ssDE](../../includes/ssde-md.md)] klicken. Wenden Sie jedoch die Empfehlungen an dieser Stelle in der Übung nicht an.  
  
4.  Wenn auf der Registerkarte **Empfehlungen** mehrere Empfehlungen vorhanden sind, deaktivieren Sie einige der Zeilen, in denen Datenbankobjekte im Raster **Indexempfehlungen** aufgelistet sind.  
  
5.  Klicken Sie im Menü **Aktionen** auf **Empfehlungen bewerten**. [!INCLUDE[ssDE](../../includes/ssde-md.md)] Der Optimierungsratgeber erstellt eine neue Optimierungssitzung, in der Sie eine Untergruppe der ursprünglichen Empfehlungen aus MySession auswerten können.  
  
6.  Geben Sie als Namen für die neue Sitzung im Feld **Sitzungsname** den Wert **EvaluateMySession** ein, und klicken Sie auf der Symbolleiste auf die Schaltfläche **Analyse starten** . Zum Anzeigen der Ergebnisse dieser neuen Optimierungssitzung können Sie die Schritte 2 und 3 wiederholen.  
  
### <a name="summary"></a>Zusammenfassung  
Das Auswerten einer Untergruppe von Optimierungsempfehlungen kann erforderlich sein, wenn Sie feststellen, dass Sie nach dem Ausführen einer Sitzung die Optimierungsoptionen noch ändern müssen. Beispiel: Sie legen im [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Optimierungsratgeber in den Optimierungsoptionen für eine Sitzung fest, dass indizierte Sichten berücksichtigt werden sollen. Nachdem die Empfehlung erstellt wurde, beschließen Sie jedoch, indizierte Sichten nicht zu berücksichtigen. Sie können anschließend im Menü **Aktionen** den **-Optimierungsratgeber mithilfe der Option** Empfehlungen bewerten [!INCLUDE[ssDE](../../includes/ssde-md.md)] anweisen, die Sitzung neu zu bewerten, ohne dabei indizierte Sichten zu berücksichtigen. Wenn Sie die Option **Empfehlungen auswerten** verwenden, werden für die zweite Optimierungssitzung die vorher generierten Empfehlungen hypothetisch auf den aktuellen physischen Entwurf angewendet, um den physischen Entwurf für die zweite Optimierungssitzung zu erstellen.  
  
Auf der Registerkarte **Berichte** können Sie weitere Ergebnisse der Optimierung anzeigen. Darauf wird in der nächsten Aufgabe dieser Lektion näher eingegangen.  

## <a name="view-tuning-reports"></a>Anzeigen von Optimierungsberichten
Es kann nützlich sein, die Skripts anzuzeigen, die zum Implementieren der Optimierungsergebnisse verwendet werden können. Daneben bietet der Datenbankoptimierungsratgeber jedoch noch viele weitere nützliche Berichte, die Sie ebenfalls anzeigen können. Diese Berichte umfassen Informationen zu den vorhandenen physischen Entwurfsstrukturen in der zu optimierenden Datenbank sowie über die empfohlenen Strukturen. Zum Anzeigen der Optimierungsberichte klicken Sie auf die Registerkarte **Berichte** , wie in der folgenden Übung beschrieben.


1. Wählen Sie im Datenbankoptimierungsratgeber die Registerkarte **Berichte** aus. 
2. Im Bereich **Optimierungszusammenfassung** können Sie Informationen zu dieser Optimierungssitzung anzeigen. Verwenden Sie die Bildlaufleiste, um den gesamten Inhalt des Bereichs anzuzeigen. Beachten Sie die Felder **Prozentsatz für die erwartete Verbesserung** und **Von der Empfehlung verwendeter Speicherplatz (MB)** . Beim Festlegen der Optionen für die Optimierung kann der Platz eingeschränkt werden, der den Empfehlungen zur Verfügung steht. Wählen Sie auf der Registerkarte **Optimierungsoptionen** die Option **Erweiterte Optionen** aus. Aktivieren Sie **Max. Speicherplatz für Empfehlungen definieren (MB)** und geben Sie den maximal zulässigen Speicherplatz für eine empfohlene Konfiguration in Megabytes ein. Mit der Schaltfläche **Zurück** kehren Sie vom Browser der Hilfe zu diesem Tutorial zurück. 

    ![DTA-Optimierungszusammenfassung](media/dta-tutorials/dta-tuning-summary.png)
  
3.  Klicken Sie im Bereich **Optimierungsberichte** in der Liste **Bericht auswählen** auf **Anweisungskostenbericht** . Wenn Sie für die Anzeige des Berichts mehr Platz brauchen, ziehen Sie den Rand des Bereichs **Sitzungsmonitor** nach links. Mit jeder [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisung, die für eine Tabelle in Ihrer Datenbank ausgeführt wird, sind Leistungskosten verknüpft. Diese Leistungskosten können reduziert werden, indem für Spalten in einer Tabelle, auf die häufig zugegriffen wird, effektive Indizes erstellt werden. Dieser Bericht zeigt, wie hoch die geschätzte Verbesserung in Prozent ist, die bei einer Implementierung der Optimierungsempfehlungen erreicht werden kann, im Vergleich zu den ursprünglichen Kosten bei Ausführen der Anweisung in der Arbeitsauslastung. Beachten Sie, dass die Menge der im Bericht enthaltenen Informationen von der Länge und Komplexität der Arbeitsauslastung abhängt.  

    ![Bericht des Datenbankoptimierungsratgebers zu Anweisungskosten](media/dta-tutorials/dta-statement-cost.png)
  
4.  Klicken Sie mit der rechten Maustaste im Raster auf den Bereich **Anweisungskostenbericht** und anschließend auf **In Datei exportieren**. Speichern Sie den Bericht unter dem Namen **MyReport**. Dem Dateinamen wird automatisch die Erweiterung .xml angehängt. Sie können die Datei MyReport.xml in Ihrem bevorzugten XML-Editor oder in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] öffnen, um den Inhalt des Berichts anzuzeigen.  
  
5.  Kehren Sie zur Registerkarte **Berichte** des Datenbankoptimierungsratgebers zurück, und klicken Sie anschließend noch einmal mit der rechten Maustaste auf **Anweisungskostenbericht** . Überprüfen Sie die weiteren Optionen, die zur Verfügung stehen. Beachten Sie, dass Sie die Schriftart des Berichts, den Sie anzeigen, ändern können. Wenn Sie die Schriftart hier ändern, wird sie auch auf den anderen Seiten im Registerformat geändert.  
  
6.  Klicken Sie auf weitere Berichte in der Liste **Bericht auswählen** , um sich mit diesen vertraut zu machen.  
  
### <a name="summary"></a>Zusammenfassung  
Sie haben jetzt die Registerkarte **Berichte** der Benutzeroberfläche des Datenbankoptimierungsratgebers für die Optimierungssitzung MySession kennen gelernt. Die gleichen Schritte können Sie ausführen, um die Berichte zu prüfen, die für die Optimierungssitzung EvaluateMySession erstellt wurden. Doppelklicken Sie dazu im Bereich **Sitzungsmonitor** auf **EvaluateMySession** .  


 ## <a name="next-lesson"></a>Nächste Lektion  
[Lektion 3: Verwenden des Befehlszeilenprogramms dta](../../tools/dta/lesson-3-using-the-dta-command-prompt-utility.md)  
   
  
