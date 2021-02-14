---
title: Exportieren von Berichten (Berichts-Generator) | Microsoft-Dokumentation
description: Sie können einen Bericht in den Reporting Services in ein anderes Dateiformat exportieren, z. B. in das PowerPoint- oder PDF-Format. Dafür können Sie den Berichts-Generator, den Berichts-Designer oder den Berichtsserver verwenden.
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-builder
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: d82c6f45d6cd8e4e35735291433d3e054afe5481
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100024106"
---
# <a name="export-reports-report-builder-and-ssrs"></a>Exportieren von Berichten (Berichts-Generator und SSRS)

  Sie können einen [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Bericht in ein anderes Dateiformat, z.B. PowerPoint, [!INCLUDE[ofprword](../../includes/ofprword-md.md)]oder [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] , oder in eine PDF- oder Bilddatei exportieren. Sie können den Bericht auch durch Generieren eines Atom-Dienstdokuments exportieren, wobei die im Bericht verfügbaren Atom-kompatiblen Datenfeeds aufgelistet werden. Sie können Ihren Bericht aus dem Berichts-Generator, dem Berichts-Designer ([!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]) oder dem Berichtsserver exportieren.  
  
 Exportieren Sie einen Bericht, um folgende Aufgaben auszuführen:  
  
-   **Verwenden der Berichtsdaten in einer anderen Anwendung.** Sie können den Bericht z. B. nach Excel exportieren und anschließend in Excel mit den Daten arbeiten.  
  
-   **Drucken des Berichts in einem anderen Format.** Sie können den Bericht z. B. in das PDF-Dateiformat exportieren und anschließend ausdrucken.  
  
-   **Speichern einer Kopie des Berichts in einem anderen Dateiformat.** Sie können einen Bericht z. B. nach Word exportieren und speichern, um eine Kopie des Berichts zu erstellen.  
  
-   **Verwenden von Berichtsdaten als Datenfeeds in Anwendungen.** Sie können z.B. Atom-kompatible Datenfeeds generieren, die von Power Pivot oder Power BI verwendet werden können, und anschließend in Power Pivot oder Power BI mit den Daten arbeiten. Weitere Informationen finden Sie unter [Generieren von Datenfeeds aus Berichten](../../reporting-services/report-builder/generate-data-feeds-from-a-report-report-builder-and-ssrs.md).  
  
-   Das Rendern des Berichts auf dem Berichtsserver ist sinnvoll, wenn Sie Abonnements einrichten, Berichte per E-Mail bereitstellen oder einen Bericht speichern möchten, der auf dem Berichtsserver verfügbar ist. Weitere Informationen finden Sie unter [Abonnements und Übermittlung &#40;Reporting Services&#41;](../../reporting-services/subscriptions/subscriptions-and-delivery-reporting-services.md).  
  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] bietet viele Renderingerweiterungen und unterstützt den Export von Berichten in gebräuchliche Dateiformate. Die Renderingerweiterungen unterstützen Dateiformate mit bedingten Umbrüchen (z. B. Word oder Excel), festen Seitenumbrüche (z. B. PDF oder TIFF) oder nur Daten (z. B. CSV oder Atom-kompatibles XML).  
  
 Unter Umständen bestehen Auswirkungen auf die Berichtspaginierung, wenn Sie einen Bericht in ein anderes Format exportieren. In der Vorschau wird ein Bericht so angezeigt, wie er von der HTML-Renderingerweiterung gerendert wird, in der Regeln für bedingte Seitenumbrüche angewendet werden. Wenn Sie einen Bericht in ein anderes Dateiformat exportieren (z. B. Adobe Acrobat, PDF), basiert die Paginierung auf der physischen Seitengröße und folgt Regeln für feste Seitenumbrüche. Die Seiten können auch durch logische Seitenumbrüche getrennt werden, die Sie zu einem Bericht hinzufügen. Die tatsächliche Länge einer Seite variiert jedoch je nach dem verwendeten Renderertyp. Sie sollten mit dem Paginierungsverhalten der ausgewählten Renderingerweiterung vertraut sein, wenn Sie die Paginierung Ihres Berichts ändern möchten. Möglicherweise müssen Sie den Entwurf des Berichtslayouts für diese Renderingerweiterung anpassen. Weitere Informationen finden Sie unter [Seitenlayout und Rendering](../../reporting-services/report-design/page-layout-and-rendering-report-builder-and-ssrs.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]

## <a name="to-export-a-report-from-report-builder"></a><a name="bkmk_export_from_rb"></a> So exportieren Sie einen Bericht aus dem Berichts-Generator

1.  Führen Sie den Bericht aus, oder zeigen Sie eine Vorschau des Berichts an.  
  
2.  Klicken Sie im Menüband auf **Exportieren**.  
  
     ![Exportieren im Berichts-Generator](../../reporting-services/report-builder/media/ssrb-export.png "Exportieren im Berichts-Generator")  
  
3.  Wählen Sie das Format aus, das Sie verwenden möchten.  
  
     Das Dialogfeld **Speichern unter** wird geöffnet. Standardmäßig entspricht der Dateiname dem Namen des exportierten Berichts. Optional können Sie den Dateinamen ändern.  
  
##  <a name="to-export-a-report-from-the-ssrsnoversion-web-portal"></a><a name="bkmk_export_from_rm"></a> So exportieren Sie einen Bericht aus dem [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Webportal  
  
1.  Navigieren Sie auf der Seite [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Home **vom** -Webportal aus zum Bericht, den Sie exportieren möchten.  
  
2.  Klicken Sie auf den Bericht, um ihn zu rendern und eine Vorschau des Berichts anzuzeigen.  
  
3.  Klicken Sie auf der Berichts-Viewer-Symbolleiste auf den Dropdownpfeil **Exportieren** .  
  
     ![Exportieren im Reporting Services-Webportal](../../reporting-services/report-builder/media/ssrsportal-export.png "Exportieren im Reporting Services-Webportal")  
  
4.  Wählen Sie das Format aus, das Sie verwenden möchten.  
  
5.  Klicken Sie auf **Exportieren**. Ein Dialogfeld wird angezeigt, und Sie werden gefragt, ob Sie die Datei öffnen oder speichern möchten.  
  
6.  Klicken Sie auf **Öffnen**, um den Bericht im ausgewählten Exportformat anzuzeigen.  
  
     \- oder –  
  
     Klicken Sie auf **Speichern**, um den Bericht sofort im ausgewählten Exportformat zu speichern.  
  
     Der Bericht wird mit der Anwendung, die dem von Ihnen gewählten Format zugeordnet ist, entweder angezeigt oder gespeichert. Wenn Sie auf **Speichern** klicken, werden Sie aufgefordert, einen Speicherort für Ihren Bericht anzugeben.  
  
##  <a name="to-export-a-report-from-a-sharepoint-library"></a><a name="bkmk_export_from_sharepoint"></a> So exportieren Sie einen Bericht aus einer SharePoint-Bibliothek  
  
1.  Zeigen Sie eine Vorschau des Berichts an.  
  
2.  Klicken Sie auf der Symbolleiste auf **Aktionen**, zeigen Sie auf **Exportieren**, und wählen Sie dann das Format aus, das Sie verwenden möchten.  
  
     Das Dialogfeld **Dateidownload** wird geöffnet.  
  
3.  Klicken Sie auf **Öffnen**, um den Bericht im ausgewählten Exportformat anzuzeigen.  
  
     \- oder –  
  
     Klicken Sie auf **Speichern**, um den Bericht sofort im ausgewählten Exportformat zu speichern.  
  
     Der Bericht wird mit der Anwendung, die dem von Ihnen gewählten Format zugeordnet ist, entweder angezeigt oder gespeichert. Wenn Sie auf **Speichern** klicken, werden Sie aufgefordert, einen Speicherort für Ihren Bericht anzugeben.  
  
     Sie können den Dateinamen des exportierten Berichts ändern.  
  
     **Hinweis** Wenn das Programm den Bericht nicht im gewählten Format öffnen kann, weil diesem Dateityp kein Programm zugeordnet ist, werden Sie aufgefordert, den exportierten Bericht zu speichern oder im Internet ein Programm zum Öffnen des Berichts zu suchen.  
  
##  <a name="rendering-extension-types"></a><a name="RendererTypes"></a> Renderingerweiterungstypen  
 Drei Arten von [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Renderingerweiterungen sind verfügbar:  
  
-   **Datenrenderererweiterungen:** Datenrenderingerweiterungen entfernen alle Formatierungs- und Layoutinformationen aus dem Bericht und zeigen nur die Daten an. Die mithilfe dieser Option erstellte Datei kann zum Importieren der Rohberichtsdaten in einen anderen Dateityp verwendet werden, z. B. Excel, eine andere Datenbank, eine XML-Datennachricht oder eine benutzerdefinierte Anwendung. Datenrenderer unterstützen keine Seitenumbrüche.  
  
     Die folgenden Datenrenderingerweiterungen werden unterstützt: CSV, XML und Atom.  
  
-   **Renderererweiterungen mit bedingtem Seitenumbruch:** Renderingerweiterungen mit bedingtem Seitenumbruch behalten das Berichtslayout und die Formatierung bei. Die mithilfe dieser Option erstellte Datei wird für die Bildschirmanzeige und -bereitstellung optimiert, z.B. auf einer Webseite oder in den **ReportViewer** -Steuerelementen.  
  
     Die folgenden Renderingerweiterungen für bedingten Seitenumbruch werden unterstützt: [!INCLUDE[msCoName](../../includes/msconame-md.md)] Excel, [!INCLUDE[msCoName](../../includes/msconame-md.md)] Word und Webarchiv (MHTML).  
  
-   **Renderingerweiterungen mit festem Seitenumbruch:** Renderererweiterungen mit festem Seitenumbruch behalten das Berichtslayout und die Formatierung bei. Die mithilfe dieser Option erstellte Datei wird für einen konsistenten Druck oder für die Onlineanzeige in einem Buchformat optimiert.  
  
     Die folgenden Renderingerweiterungen für harten Seitenumbruch werden unterstützt: TIFF und PDF.  
  
##  <a name="formats-you-can-export-while-viewing-reports"></a><a name="ExportFormats"></a> Formate, die Sie beim Anzeigen von Berichten exportieren können  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] stellt Renderingerweiterungen bereit, die Berichte in anderen Formaten rendern. Sie sollten den Berichtsentwurf für das gewählte Dateiformat optimieren.  In der folgenden Tabelle sind die Formate aufgeführt, in die Sie über die Benutzeroberfläche exportieren können.  Es gibt zusätzliche Formate, die Sie mit [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Abonnements oder beim Exportieren per URL-Zugriff verwenden können.  Gehen Sie zum Abschnitt [Weitere Methoden zum Exportieren von Berichten](#OtherWaysExportingReports)in diesem Thema.  
  
|Format|Renderingerweiterungstyp|BESCHREIBUNG|  
|------------|------------------------------|-----------------|  
|Acrobat-Datei (PDF-Datei)|Fester Seitenumbruch|Die PDF-Renderingerweiterung rendert Berichte in einem Dateiformat, das in Adobe Acrobat und anderen PDF-Viewern von Drittanbietern geöffnet werden kann, die das Format PDF 1.3 unterstützen. Obwohl PDF 1.3 mit Adobe Acrobat 4.0 oder höher kompatibel ist, wird Adobe Acrobat von [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] erst ab Version 6 unterstützt. Die Renderingerweiterung erfordert keine Adobe-Software, um Berichte zu rendern. Zum Anzeigen oder Drucken von Berichten im PDF-Format sind allerdings PDF-Viewer wie Adobe Acrobat erforderlich.<br /><br /> Weitere Informationen finden Sie unter [Exportieren als PDF-Datei](../../reporting-services/report-builder/exporting-to-a-pdf-file-report-builder-and-ssrs.md).|  
|Atom|Daten|Die Atom-Renderingerweiterung generiert Atom-kompatible Datenfeeds aus Berichten. Die Datenfeeds sind mit Anwendungen lesbar und austauschbar, die Atom-kompatible Datenfeeds nutzen können, z.B. Power Pivot oder Power BI.<br /><br /> Die Ausgabe ist ein Atom-Dienstdokument, in dem die in einem Bericht verfügbaren Datenfeeds aufgeführt sind. Mindestens ein Datenfeed wird für jeden Datenbereich in einem Bericht erstellt. Abhängig vom Typ des Datenbereichs und den darin angezeigten Daten können mehrere Datenfeeds generiert werden.<br /><br /> Weitere Informationen finden Sie unter [Generieren von Datenfeeds aus Berichten](../../reporting-services/report-builder/generating-data-feeds-from-reports-report-builder-and-ssrs.md).|  
|CSV|Daten|Die durch Trennzeichen getrennte CSV (Comma-Separated Value)-Renderingerweiterung rendert Berichte als vereinfachte Darstellung der Daten eines Berichts in einem standardisierten Nur-Text-Format, das leicht lesbar und mit anderen Anwendungen austauschbar ist.<br /><br /> Weitere Informationen finden Sie unter [Exportieren als CSV-Datei](../../reporting-services/report-builder/exporting-to-a-csv-file-report-builder-and-ssrs.md).|  
|EXCELOPENXML|Bedingter Seitenumbruch|Wird beim Überprüfen von Berichten als „Excel“ in den Exportmenüs angezeigt. Die Excel-Renderingerweiterung rendert einen Bericht als Excel-Dokument (.xlsx), das mit [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] 2013 kompatibel ist.  Weitere Informationen finden Sie unter [Exportieren nach Microsoft Excel](../../reporting-services/report-builder/exporting-to-microsoft-excel-report-builder-and-ssrs.md).|  
|PowerPoint|Fester Seitenumbruch|Die PowerPoint-Renderingerweiterung rendert einen Bericht als PowerPoint-Dokument (.pptx), das mit PowerPoint 2013 kompatibel ist.|  
|TIFF-Datei|Fester Seitenumbruch|Die Bildrenderingerweiterung rendert einen Bericht als Bitmap oder Metadatei. Standardmäßig erstellt die Bildrenderingerweiterung eine TIFF-Datei des Berichts, die auf mehreren Seiten angezeigt werden kann. Nachdem der Client das Bild erhalten hat, kann es in einem Image Viewer angezeigt und gedruckt werden.<br /><br /> Die Bildrenderingerweiterung kann Dateien in allen von [!INCLUDE[ndptecgdiplus](../../includes/ndptecgdiplus-md.md)] unterstützten Formaten erstellen: BMP, EMF, EMFPlus, GIF, JPEG, PNG und TIFF.<br /><br /> Weitere Informationen finden Sie unter [Exportieren in eine Bilddatei](../../reporting-services/report-builder/exporting-to-an-image-file-report-builder-and-ssrs.md).|  
|Webarchiv|Bedingter Seitenumbruch|Die HTML-Renderingerweiterung rendert einen Bericht im HTML-Format. Die Renderingerweiterung kann außerdem vollständige HTML-Seiten oder HTML-Fragmente zum Einbetten in andere HTML-Seiten erstellen. HTML wird stets mit UTF-8-Codierung erstellt.<br /><br /> Die HTML-Renderingerweiterung ist die Standardrenderingerweiterung für Berichte, die im Berichts-Generator in der Vorschau angezeigt und in einem Browser angezeigt werden. Dies gilt auch bei der Ausführung im [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Webportal.<br /><br /> Weitere Informationen finden Sie unter [Rendern in das HTML-Format](../../reporting-services/report-builder/rendering-to-html-report-builder-and-ssrs.md).|  
|WORDOPENXML|Bedingter Seitenumbruch|Beim Anzeigen von Berichten als „Word“ im Exportmenü angezeigt. Die Word-Renderingerweiterung rendert einen Bericht als Word-Dokument (.docx), das mit [!INCLUDE[ofprword](../../includes/ofprword-md.md)] 2013 kompatibel ist.  Weitere Informationen finden Sie unter [Exportieren nach Microsoft Word](../../reporting-services/report-builder/exporting-to-microsoft-word-report-builder-and-ssrs.md).|  
|XML|Daten|Die XML-Renderingerweiterung gibt einen Bericht im XML-Format zurück. Das Schema der Bericht-XML-Ausgabe hängt vom jeweiligen Bericht ab und enthält nur Daten. Layoutinformationen werden von der XML-Renderingerweiterung nicht gerendert, und die Paginierung wird nicht beibehalten. Der von dieser Erweiterung generierte XML-Code kann in eine Datenbank importiert, als XML-Datennachricht verwendet oder an eine benutzerdefinierte Anwendung gesendet werden.<br/><br/> Weitere Informationen finden Sie unter [Exportieren nach XML](../../reporting-services/report-builder/exporting-to-xml-report-builder-and-ssrs.md).|  
  
##  <a name="generating-data-feeds-from-a-report"></a><a name="GeneratingDataFeedsFromReport"></a> Generieren von Datenfeeds aus einem Bericht  
 Führen Sie den Bericht im [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Webportal aus, und klicken Sie anschließend auf der Symbolleiste des Webportals auf das Symbol **Datenfeed generieren** , um Datenfeeds aus einem Bericht zu generieren. In einer Eingabeaufforderung werden Sie gefragt, ob die Datei gespeichert oder geöffnet werden soll. Wenn Sie **Öffnen** auswählen, wird das Atom-Dienstdokument in der Anwendung geöffnet, die der Dateierweiterung ".atomsvc" zugeordnet ist. Wenn Sie **Speichern** auswählen, wird das Dokument als ATOMSVC-Datei gespeichert. Standardmäßig wird der Name des Berichts als Dateiname verwendet. Sie können den Namen ändern, um einen sinnvolleren Namen anzugeben.  
  
 Das Atom-Dienstdokument wird auf dem Computer gespeichert. Sie können es später auf einen Berichtsserver oder einen anderen Server hochladen, um es für andere Benutzer verfügbar zu machen. Weitere Informationen finden Sie unter [Generieren von Datenfeeds aus Berichten](../../reporting-services/report-builder/generating-data-feeds-from-reports-report-builder-and-ssrs.md) und [Generieren von Datenfeeds aus einem Bericht](../../reporting-services/report-builder/generate-data-feeds-from-a-report-report-builder-and-ssrs.md).  
  
##  <a name="troubleshooting-exported-reports"></a><a name="Troubleshooting"></a> Problembehandlung bei exportierten Berichten  
 Gelegentlich sehen die Berichte anders aus, oder sie funktionieren nach dem Exportieren in ein anderes Format nicht wunschgemäß. Dies liegt daran, dass für den Renderer möglicherweise bestimmte Regeln und Einschränkungen gelten. Sie können vielen Einschränkungen dadurch begegnen, dass Sie sie beim Erstellen des Berichts berücksichtigen. Sie müssen im Bericht ggf. ein etwas anderes Layout verwenden, die Elemente im Bericht sorgfältig ausrichten, die Fußzeilen im Bericht auf eine Textzeile beschränken usw.  
  
 Falls Ihr Bericht Unicode-Text mit arabischen Zahlen oder arabische Daten enthält, werden die Daten und Zahlen nicht korrekt gerendert, wenn Sie den Bericht in eines der folgenden Formate exportieren oder ausdrucken.  
  
-   PDF  
  
-   Wort  
  
-   Excel  
  
-   Image/TIFF  
  
 Wenn Sie den Bericht als HTML exportieren, werden die Daten und Zahlen korrekt gerendert.  
  
 In den Themen zu bestimmten Renderern werden neben dem Rendern von Berichtselementen und Datenbereichen auch die Einschränkungen und Lösungen für jeden Renderer beschrieben.  
  
-   [Exportieren als CSV-Datei &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-builder/exporting-to-a-csv-file-report-builder-and-ssrs.md)  
  
-   [Exporting to Microsoft Excel &#40;Report Builder and SSRS&#41; (Exportieren nach Microsoft Excel (Berichts-Generator und SSRS))](../../reporting-services/report-builder/exporting-to-microsoft-excel-report-builder-and-ssrs.md)  
  
-   [Exportieren nach Microsoft Word &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-builder/exporting-to-microsoft-word-report-builder-and-ssrs.md)  
  
-   [Rendern in das HTML-Format &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-builder/rendering-to-html-report-builder-and-ssrs.md)  
  
-   [Exportieren als PDF-Datei &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-builder/exporting-to-a-pdf-file-report-builder-and-ssrs.md)  
  
-   [Exportieren in eine Bilddatei &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-builder/exporting-to-an-image-file-report-builder-and-ssrs.md)  
  
-   [Exportieren nach XML &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-builder/exporting-to-xml-report-builder-and-ssrs.md)  
  
-   [Generieren von Datenfeeds aus Berichten &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-builder/generating-data-feeds-from-reports-report-builder-and-ssrs.md)  
  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] bietet zusätzliche Funktionen, mit denen Berichte erstellt werden können, die in anderen Formaten problemlos funktionieren. Durch Seitenumbrüche in Tablix-Datenbereichen (Tabelle, Matrix und Liste), Gruppen und Rechtecke lässt sich die Berichtspaginierung besser steuern. Durch Seitenumbrüche begrenzte Berichtsseiten können über unterschiedliche Seitennamen verfügen und die Seitennummerierung zurücksetzen. Mithilfe von Ausdrücken können die Seitennamen und Seitenzahlen bei Ausführung des Berichts dynamisch aktualisiert werden. Weitere Informationen finden Sie unter [Paginierung in Reporting Services](../../reporting-services/report-design/pagination-in-reporting-services-report-builder-and-ssrs.md).  
  
 Zudem können Sie mit dem integrierten globalen Objekt in RenderFormat bedingt unterschiedliche Berichtslayouts für verschiedene Renderer übernehmen. Weitere Informationen finden Sie unter [Integrierte globale Werte und Benutzerverweise](../../reporting-services/report-design/built-in-collections-built-in-globals-and-users-references-report-builder.md).

##  <a name="other-ways-of-exporting-reports"></a><a name="OtherWaysExportingReports"></a> Weitere Methoden zum Exportieren von Berichten  
 Beim Exportieren eines Berichts handelt es sich um eine bedarfsgesteuerte Aufgabe, die Sie für einen im [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Webportal oder Berichts-Generator geöffneten Bericht ausführen. Falls Sie einen Exportvorgang automatisieren möchten (z. B. um einen Bericht in einem wiederkehrenden Zeitplan als spezifischen Dateityp in einen freigegebenen Ordner zu exportieren), erstellen Sie ein Abonnement, durch das der Bericht an einen freigegebenen Ordner übermittelt wird. Weitere Informationen finden Sie unter [File Share Delivery in Reporting Services](../../reporting-services/subscriptions/file-share-delivery-in-reporting-services.md).  
  
 Berichte, die mit den Berichtstools in der Vorschau angezeigt oder in einer Browseranwendung wie dem [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Webportal geöffnet werden, werden stets zuerst in HTML gerendert. Sie können keine andere Renderingerweiterung als Standarderweiterung für die Anzeige angeben. Sie können jedoch ein Abonnement erstellen, durch das ein Bericht im gewünschten Renderingformat für die nachfolgende Übermittlung an ein E-Mail-Postfach oder an einen freigegebenen Ordner erstellt wird. Weitere Informationen finden Sie unter [Erstellen und Verwalten von Abonnements für Berichtsserver im einheitlichen Modus](../../reporting-services/subscriptions/create-and-manage-subscriptions-for-native-mode-report-servers.md) und [Erstellen, Ändern und Löschen von datengesteuerten Abonnements](../../reporting-services/subscriptions/create-modify-and-delete-data-driven-subscriptions.md).  
  
 Sie können auch über eine URL, in der eine Renderingerweiterung als URL-Parameter angegeben ist, auf einen Bericht zugreifen und ihn direkt im angegebenen Format rendern, ohne ihn zuerst in HTML zu rendern. Im folgenden Beispiel wird ein Bericht im Excel-Format gerendert:  
  
```  
https://<Report Server Name>/reportserver?/Sales/YearlySalesSummary&rs:Format=Excel&rs:Command=Render  
```  
  
 Und im folgenden Beispiel wird ein PowerPoint-Bericht aus einer benannten Instanz gerendert:  
  
```  
https://<Report Server Name/ReportServer_THESQLINSTANCE/Pages/ReportViewer.aspx?%2freportfolder%2freport+name+with+spaces&rs:Format=pptx  
```  
  
 Weitere Informationen finden Sie unter [Exportieren von Berichten über URL-Zugriff](../../reporting-services/export-a-report-using-url-access.md).  

## <a name="next-steps"></a>Nächste Schritte

[Steuern von Seitenumbrüchen, Überschriften, Spalten und Zeilen &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/controlling-page-breaks-headings-columns-and-rows-report-builder-and-ssrs.md)   
[Suchen, Anzeigen und Verwalten von Berichten (Berichts-Generator und SSRS)](../../reporting-services/report-builder/finding-viewing-and-managing-reports-report-builder-and-ssrs.md)   
[Drucken von Berichten &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-builder/print-reports-report-builder-and-ssrs.md)   
[Speichern von Berichten &#40;Berichts-Generator&#41;](../../reporting-services/report-builder/saving-reports-report-builder.md)  

Haben Sie dazu Fragen? [Stellen Sie eine Frage im Reporting Services-Forum](https://go.microsoft.com/fwlink/?LinkId=620231)
