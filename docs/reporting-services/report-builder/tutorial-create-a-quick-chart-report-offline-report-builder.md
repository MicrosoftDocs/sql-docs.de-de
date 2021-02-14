---
title: 'Tutorial: Schnelles Erstellen eines Diagrammberichts offline (Berichts-Generator) | Microsoft-Dokumentation'
description: In diesem Tutorial erstellen Sie mit dem Berichts-Generator ein Kreisdiagramm in einem paginierten Reporting Services-Bericht. Anschließend fügen Sie Prozentwerte hinzu und nehmen Änderungen am Kreisdiagramm vor.
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-builder
ms.topic: conceptual
helpviewer_keywords:
- reports, creating
- tutorials, getting started
- creating reports
ms.assetid: 6b1db67a-cf75-494c-b70c-09f1e6a8d414
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 2f7fb9781583a56a2765f066462e62717c1d32d6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100070550"
---
# <a name="tutorial-create-a-quick-chart-report-offline-report-builder"></a>Tutorial: Erstellen eines Quick-Diagrammberichts offline (Berichts-Generator)

  In diesem Tutorial verwenden Sie einen Assistenten, um ein Kreisdiagramm in einem paginierten [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]-Bericht im [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] zu erstellen. Anschließend fügen Sie Prozentwerte hinzu und nehmen geringfügige Änderungen am Kreisdiagramm vor. 
  
Dieses Lernprogramm kann auf zwei unterschiedliche Arten absolviert werden. Beide Methoden liefern das gleiche Ergebnis – ein Kreisdiagramm entsprechend der folgenden Abbildung:  
  
 ![Schnelles Kreisdiagramm im Berichts-Generator](../../reporting-services/report-builder/media/report-builder-quick-pie-chart.png "Schnelles Kreisdiagramm im Berichts-Generator")  
  
## <a name="prerequisites"></a>Voraussetzungen  
 Sowohl bei Verwendung von XML-Daten als auch bei einer [!INCLUDE[tsql](../../includes/tsql-md.md)]-Abfrage benötigen Sie Zugriff auf den Berichts-Generator. Sie können [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] auf einem [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Berichtsserver starten (entweder im nativem Modus oder im integrierten SharePoint-Modus), oder Sie können [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] aus dem Microsoft Download Center herunterladen. Weitere Informationen finden Sie unter [Installieren des Berichts-Generators](../../reporting-services/install-windows/install-report-builder.md).  
  
##  <a name="two-ways-to-do-this-tutorial"></a><a name="TwoWays"></a> Zwei Möglichkeiten zum Absolvieren des Lernprogramms  
  
-   [Erstellen des Kreisdiagramms mit XML-Daten](#CreatePieChartXML)  
  
-   [Erstellen des Kreisdiagramms mit einer Transact-SQL-Abfrage, die Daten enthält](#CreatePieQueryData)  
  
### <a name="using-xml-data-for-this-tutorial"></a>Verwenden von XML-Daten für dieses Lernprogramm  
 Sie können XML-Daten verwenden, die Sie aus diesem Thema kopieren und in den Assistenten einfügen. Es muss keine Verbindung zu einem [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]-Berichtsserver im einheitlichen Modus oder im integrierten SharePoint-Modus bestehen, und Sie benötigen keinen Zugriff auf eine SQL Server-Instanz.  
  
 [Erstellen des Kreisdiagramms mit XML-Daten](#CreatePieChartXML)  
  
### <a name="using-a-tsql-query-that-contains-data-for-this-tutorial"></a>Verwenden einer [!INCLUDE[tsql](../../includes/tsql-md.md)] -Abfrage, die Daten für dieses Tutorial enthält  
 Sie können aus diesem Thema eine Abfrage mit Daten kopieren und diese in den Assistenten einfügen. Sie benötigen den Namen einer SQL Server-Instanz sowie Anmeldeinformationen für den schreibgeschützten Zugriff auf eine beliebige Datenbank. In der Datasetabfrage im Tutorial werden zwar Literaldaten verwendet, jedoch muss die Abfrage durch eine SQL Server-Instanz verarbeitet werden, um die für ein Berichtsdataset erforderlichen Metadaten zurückzugeben.  
  
 Der Vorteil der Verwendung der [!INCLUDE[tsql](../../includes/tsql-md.md)] -Abfrage besteht darin, dass in allen anderen [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] -Tutorials die gleiche Methode verwendet wird. Sie wissen daher bereits, was zu tun ist, wenn Sie die anderen Tutorials durcharbeiten.  
  
 Für die [!INCLUDE[tsql](../../includes/tsql-md.md)] -Abfrage müssen noch einige andere Voraussetzungen erfüllt sein. Weitere Informationen finden Sie unter [Voraussetzungen für Lernprogramme &#40;Berichts-Generator&#41;](../../reporting-services/prerequisites-for-tutorials-report-builder.md).  
  
 [Erstellen des Kreisdiagramms mit einer Transact-SQL-Abfrage, die Daten enthält](#CreatePieQueryData)  
  
##  <a name="creating-the-pie-chart-with-xml-data"></a><a name="CreatePieChartXML"></a> Erstellen des Kreisdiagramms mit XML-Daten  
  
1.  [Starten Sie den Berichts-Generator](../../reporting-services/report-builder/start-report-builder.md) aus dem [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Webportal, vom Berichtsserver im integrierten SharePoint-Modus oder von Ihrem Computer.  
  
     Das Dialogfeld **Erste Schritte** wird angezeigt.  
  
     ![Erste Schritte mit dem Berichts-Generator](../../reporting-services/media/rb-getstarted.png "Erste Schritte mit dem Berichts-Generator")  
  
     Falls das Dialogfeld **Erste Schritte** nicht angezeigt wird, klicken Sie auf **Datei** >**Neu** zu erstellen. Das Dialogfeld **Neuer Bericht oder neues Dataset** verfügt größtenteils über den gleichen Inhalt wie das Dialogfeld **Erste Schritte** .  
  
2.  Vergewissern Sie sich, dass im linken Bereich **Neuer Bericht** ausgewählt ist.  
  
3.  Klicken Sie im rechten Bereich auf **Diagramm-Assistent** und anschließend auf **Erstellen**.  
  
4.  Klicken Sie auf der Seite **Dataset auswählen** auf **Dataset erstellen** und anschließend auf **Weiter**.  
  
5.  Klicken Sie auf der Seite **Verbindung mit einer Datenquelle auswählen** auf **Neu**.  
  
     Das Dialogfeld **Datenquelleneigenschaften** wird angezeigt.  
  
6.  Sie können jedes gewünschte Element als Datenquelle bezeichnen. Geben Sie im Feld **Name** den Namen **MyPieChart** ein.  
  
7.  Klicken Sie im Dialogfeld **Verbindungstyp auswählen** auf **XML**.  
  
8.  Klicken Sie zunächst auf die Registerkarte **Anmeldeinformationen** und anschließend auf **Aktuellen Windows-Benutzer verwenden. Möglicherweise ist eine Kerberos-Delegierung erforderlich**. Klicken Sie anschließend auf **OK**.  
  
9. Klicken Sie auf der Seite **Verbindung mit einer Datenquelle auswählen** auf **MyPieChart** und anschließend auf **Weiter**.  
  
10. Kopieren Sie den folgenden Text, und fügen Sie ihn in das große Feld im oberen Bereich der Seite **Abfrage entwerfen** ein.  
  
    ```  
    <Query>  
    <ElementPath>Root /S  {@Sales (Integer)} /C {@FullName} </ElementPath>  
    <XmlData>  
    <Root>  
    <S Sales="150">  
      <C FullName="Jae Pak" />  
    </S>  
    <S Sales="350">  
      <C FullName="Jillian  Carson" />  
    </S>  
    <S Sales="250">  
      <C FullName="Linda C Mitchell" />  
    </S>  
    <S Sales="500">  
      <C FullName="Michael Blythe" />  
    </S>  
    <S Sales="450">  
      <C FullName="Ranjit Varkey" />  
    </S>  
    </Root>  
    </XmlData>  
    </Query>  
    ```  
  
11. (Optional) Klicken Sie auf die Schaltfläche **Ausführen** ( **!** ), um die Daten anzuzeigen, auf denen das Diagramm basiert.  
  
     ![Berichts-Generator, Abfrageentwurf](../../reporting-services/report-builder/media/rb-designquery.png "Berichts-Generator, Abfrageentwurf")  
  
12. Klicken Sie auf **Weiter**.  
  
13. Klicken Sie auf der Seite **Diagrammtyp auswählen** auf **Kreisdiagramm**. Danach klicken Sie auf **Weiter**.  
  
14. Doppelklicken Sie auf der Seite **Diagrammfelder anordnen** auf das Feld **Vertrieb** im Feld **Verfügbare Felder**.  
  
     Beachten Sie, dass der Wert automatisch in das Feld **Werte** verschoben wird, da es sich um einen numerischen Wert handelt.  
  
     ![Berichts-Generator, Diagrammfelder anordnen](../../reporting-services/report-builder/media/rb-wizarrangefields.png "Berichts-Generator, Diagrammfelder anordnen")  
  
15. Ziehen Sie das Feld **FullName** aus dem Feld **Verfügbare Felder** in das Feld **Kategorien** (oder doppelklicken Sie auf das Feld, damit es in das Feld **Kategorien** verschoben wird). Klicken Sie anschließend auf **Weiter**.  
  
     Die Vorschauseite zeigt Ihr neues Kreisdiagramm mit aussagekräftigen Daten. Anstelle der Namen der Vertriebsmitarbeiter enthält die Legende "Full Name 1", "Full Name 2" usw.; außerdem ist die Größe der Kreissegmente nicht exakt. Diese Darstellung soll nur eine Vorstellung vom Aussehen des Berichts vermitteln.  
  
     ![Berichts-Generator, neues Diagramm, Vorschau](../../reporting-services/report-builder/media/rb-newchartpreview.png "Berichts-Generator, neues Diagramm, Vorschau")  
  
16. Klicken Sie auf **Fertig stellen**.  
  
     Jetzt sehen Sie Ihren neuen Kreisdiagrammbericht in der Entwurfsansicht, immer noch mit aussagekräftigen Daten.  
  
     ![Berichts-Generator, neues Kreisdiagramm in der Entwurfsansicht](../../reporting-services/report-builder/media/rb-newpiedesign.png "Berichts-Generator, neues Kreisdiagramm in der Entwurfsansicht")  
  
17. Klicken Sie auf der Registerkarte **Home** des Menübands auf **Ausführen** , um das tatsächliche Kreisdiagramm anzuzeigen.  
  
     ![Berichts-Generator, neues Diagramm, Ausführung](../../reporting-services/report-builder/media/rb-newchartrun.png "Berichts-Generator, neues Diagramm, Ausführung")  
  
18. Fahren Sie mit [Nach der Ausführung des Assistenten](#AfterWizard) in diesem Artikel fort, um Ihr Kreisdiagramm weiter zu ändern.  
  
##  <a name="creating-the-pie-chart-with-a-tsql-query"></a><a name="CreatePieQueryData"></a> Erstellen des Kreisdiagramms mit einer [!INCLUDE[tsql](../../includes/tsql-md.md)] -Abfrage  
  
1.  [Starten Sie den Berichts-Generator](../../reporting-services/report-builder/start-report-builder.md) aus dem [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Webportal, vom Berichtsserver im integrierten SharePoint-Modus oder von Ihrem Computer.  
  
     Das Dialogfeld **Erste Schritte** wird angezeigt.  
  
    > [!NOTE]  
    >  Falls das Dialogfeld **Erste Schritte** nicht angezeigt wird, klicken Sie auf **Datei** >**Neu** zu erstellen. Das Dialogfeld **Neuer Bericht oder neues Dataset** verfügt größtenteils über den gleichen Inhalt wie das Dialogfeld **Erste Schritte** .  
  
2.  Vergewissern Sie sich, dass im linken Bereich **Neuer Bericht** ausgewählt ist.  
  
3.  Klicken Sie im rechten Bereich auf **Diagramm-Assistent** und anschließend auf **Erstellen**.  
  
4.  Klicken Sie auf der Seite **Dataset auswählen** auf **Dataset erstellen** und anschließend auf **Weiter**.  
  
5.  Wählen Sie auf der Seite **Verbindung mit einer Datenquelle auswählen** eine vorhandene Datenquelle aus, oder navigieren Sie zum Berichtsserver, und wählen Sie eine Datenquelle aus. Klicken Sie anschließend auf **Weiter**. Möglicherweise müssen Benutzername und Kennwort eingegeben werden.  
  
    > [!NOTE]  
    >  Welche Datenquelle Sie auswählen, ist unwichtig, solange Sie über ausreichende Berechtigungen verfügen. Aus der Datenquelle werden keine Daten abgerufen. Weitere Informationen finden Sie unter [Voraussetzungen für Lernprogramme &#40;Berichts-Generator&#41;](../../reporting-services/prerequisites-for-tutorials-report-builder.md).  
  
6.  Klicken Sie auf der Seite **Abfrage entwerfen** auf **Als Text bearbeiten**.  
  
7.  Fügen Sie die folgende Abfrage in den Abfragebereich ein:  
  
    ```  
    SELECT 150 AS Sales, 'Jae Pak' AS FullName   
    UNION SELECT 350 AS Sales, 'Jillian Carson' AS FullName   
    UNION SELECT 250 AS Sales, 'Linda C Mitchell' AS FullName   
    UNION SELECT 500 AS Sales, 'Michael Blythe' AS FullName   
    UNION SELECT 450 AS Sales, 'Ranjit Varkey' AS FullName   
    ```  
  
8.  (Optional) Klicken Sie auf die Schaltfläche „Ausführen“ ( **!** ), um die Daten anzuzeigen, auf denen das Diagramm basiert.  
  
9. Klicken Sie auf **Weiter**.  
  
10. Klicken Sie auf der Seite **Diagrammtyp auswählen** auf **Kreisdiagramm**. Danach klicken Sie auf **Weiter**.  
  
11. Doppelklicken Sie auf der Seite **Diagrammfelder anordnen** auf das Feld **Vertrieb** im Feld **Verfügbare Felder**.  
  
     Beachten Sie, dass der Wert automatisch in das Feld **Werte** verschoben wird, da er ein numerischer Wert ist.  
  
12. Ziehen Sie das Feld **FullName** aus dem Feld **Verfügbare Felder** in das Feld **Kategorien** (oder doppelklicken Sie auf das Feld, damit es in das Feld **Kategorien** verschoben wird). Klicken Sie anschließend auf **Weiter**.  
  
13. Klicken Sie auf **Fertig stellen**.  
  
     Nun sehen Sie den neuen Kreisdiagrammbericht auf der Entwurfsoberfläche. Was Sie sehen, ist symbolisch. Anstelle der Namen der Vertriebsmitarbeiter enthält die Legende "Full Name 1", "Full Name 2" usw.; außerdem ist die Größe der Kreissegmente nicht exakt. Diese Darstellung soll nur eine Vorstellung vom Aussehen des Berichts vermitteln.  
  
15. Klicken Sie auf der Registerkarte **Home** des Menübands auf **Ausführen** , um das tatsächliche Kreisdiagramm anzuzeigen.  
 
##  <a name="after-you-run-the-wizard"></a><a name="AfterWizard"></a> Nach der Ausführung des Assistenten  
 Nachdem Sie den Kreisdiagrammbericht erstellt haben, können Sie etwas experimentieren. Klicken Sie auf der Registerkarte **Ausführen** des Menübands auf **Entwurf**, um das Diagramm weiter ändern zu können.  
  
## <a name="make-the-chart-bigger"></a>Vergrößern des Diagramms  
 Das Kreisdiagramm muss unter Umständen vergrößert werden. 
 
*  Klicken Sie auf das Diagramm, um es auszuwählen. Klicken Sie dabei jedoch nicht auf ein Element des Diagramms. Ziehen Sie anschließend die rechte untere Ecke, um die Größe zu ändern.  

Beachten Sie, dass sich die Entwurfsoberfläche beim Ziehen vergrößert.
  
## <a name="add-a-report-title"></a>Hinzufügen eines Berichtstitels  
1. Markieren Sie am oberen Rand des Diagramms den Text **Diagrammtitel** , und geben Sie anschließend einen Titel (beispielsweise **Sales Pie Chart**) ein.  
2. Ändern Sie bei ausgewähltem Titel im Bereich „Eigenschaften“ folgende Werte: **Farbe** in **Schwarz** und **Schriftgrad** in **12 Punkt**.
  
## <a name="add-percentages"></a>Hinzufügen von Prozentsätzen  
 
1.  Klicken Sie mit der rechten Maustaste auf das Kreisdiagramm und anschließend auf **Datenbezeichnungen anzeigen**. Die Datenbezeichnungen sollten innerhalb jedes Segments des Kreisdiagramms angezeigt werden.  
  
2.  Klicken Sie mit der rechten Maustaste auf die Bezeichnungen und anschließend auf **Reihenbezeichnungseigenschaften**. Das Dialogfeld **Reihenbezeichnungseigenschaften** wird angezeigt.  
  
3.  Geben Sie den Typ **#PERCENT{P0}** für das Feld **Bezeichnungsdaten** ein.  
  
     Wenn Sie **{P0}** eingeben, erhalten Sie den Prozentsatz ohne Dezimalstellen. Wenn Sie nur **#PERCENT** eingeben, haben die Zahlen zwei Dezimalstellen. **#PERCENT** ist ein Schlüsselwort, das eine Berechnung oder eine Funktion für Sie ausführt. Dies ist nur ein Beispiel unter vielen.  
     
4. Klicken Sie auf **Ja** , um zu bestätigen, dass **UseValueAsLabel** auf **False** festgelegt werden soll.

5. Wählen Sie auf der Registerkarte **Schriftgrad** die Option **Fett** , und ändern Sie den Wert von **Farbe** in **Weiß**.

6. Klicken Sie auf **OK**.     
  
 Weitere Informationen zum Anpassen von Diagrammbezeichnungen und -legenden finden Sie unter [Anzeigen von Prozentwerten in einem Kreisdiagramm &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/display-percentage-values-on-a-pie-chart-report-builder-and-ssrs.md) sowie unter [Ändern des Texts eines Legendenelements &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/chart-legend-change-item-text-report-builder.md).  
  
##  <a name="whats-next"></a><a name="WhatsNext"></a> Wie geht es weiter?  
 Nachdem Sie nun Ihren ersten Bericht in [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)]erstellt haben, können Sie die anderen Tutorials durcharbeiten und die ersten Berichte mit Ihren eigenen Daten erstellen. Zur Ausführung von [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] müssen Sie berechtigt sein, mithilfe einer *Verbindungszeichenfolge*, durch die die Verbindung mit der Datenquelle hergestellt wird, auf Ihre Datenquellen (beispielsweise Datenbanken) zuzugreifen. Der Systemadministrator hat diese Informationen und kann Ihnen bei der Einrichtung helfen.  
  
 Zur Bearbeitung der anderen Tutorials benötigen Sie den Namen einer SQL Server-Instanz sowie Anmeldeinformationen für den schreibgeschützten Zugriff auf eine beliebige Datenbank. Auch dabei können Sie sich an den Systemadministrator wenden.  
  
 Schließlich benötigen Sie die URL und die Berechtigungen, um die Berichte auf einem Berichtsserver oder einer SharePoint-Website zu speichern, die mit einem Berichtsserver integriert ist. Sie können jeden Bericht, den Sie erstellen, direkt auf Ihrem Computer ausführen. Berichte haben jedoch mehr Funktionalität, wenn sie vom Berichtsserver oder von einer SharePoint-Website aus ausgeführt werden. Sie benötigen Berechtigungen zum Ausführen Ihrer Berichte oder anderer Berichte auf dem Berichtsserver oder einer SharePoint-Website, auf der sie veröffentlicht werden. Wenden Sie sich an den Systemadministrator, um Zugriff zu erhalten.  
  
 Außerdem sollten Sie sich zunächst mit einigen Grundlagen und Begriffen vertraut machen. Siehe [Konzepte von Reporting Services (SSRS)](../reporting-services-concepts-ssrs.md). Außerdem sollten Sie das Erstellen Ihres ersten Berichts sorgfältig planen. Der Aufwand lohnt sich. Weitere Informationen finden Sie unter [Planen eines Berichts &#40;Berichts-Generator&#41;](../../reporting-services/report-design/planning-a-report-report-builder.md).  

## <a name="next-steps"></a>Nächste Schritte

[Lernprogramme für den Berichts-Generator](../../reporting-services/report-builder-tutorials.md)   
[Berichts-Generator in SQL Server](../../reporting-services/report-builder/report-builder-in-sql-server-2016.md)  

Haben Sie dazu Fragen? [Stellen Sie eine Frage im Reporting Services-Forum](https://go.microsoft.com/fwlink/?LinkId=620231)
