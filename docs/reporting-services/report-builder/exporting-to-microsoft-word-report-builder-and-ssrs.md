---
title: Exportieren nach Microsoft Word (Berichts-Generator) | Microsoft-Dokumentation
ms.date: 12/06/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-builder
description: Die Word-Renderingerweiterung rendert paginierte Berichte im Microsoft Word-Format (.docx). Das Format ist Office Open XML.
ms.custom: seodec18
ms.topic: conceptual
ms.assetid: 0cd8ae26-4682-4473-8f15-af084951defd
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: e1a9fd855d1b0bc8defd0e4355f490a2824fead6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100015260"
---
# <a name="exporting-to-microsoft-word-report-builder-and-ssrs"></a>Exportieren nach Microsoft Word (Berichts-Generator und SSRS)

  Die Word-Renderingerweiterung rendert paginierte Berichte im Microsoft Word-Format (.docx). Das Format ist Office Open XML.  
  
 Der Inhaltstyp von Dateien, die vom Renderer generiert werden, ist **application/vnd.openxmlformats-officedocument.wordprocessingml.document** , und die Dateinamenerweiterung lautet „.docx“.  
  
 Weitere Informationen zum Exportieren nach Word finden Sie unter [Exportieren von Berichten (Berichts-Generator und SSRS)](../../reporting-services/report-builder/export-reports-report-builder-and-ssrs.md).  
  
 Nachdem Sie den Bericht in ein Word-Dokument exportiert haben, können Sie den Inhalt bearbeiten und dokumentartige Berichte wie Adressetiketten, Bestellungen oder Serienbriefe entwerfen.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
##  <a name="report-items-in-word"></a><a name="ReportItemsWord"></a> Berichtselemente in Word  
 In Word exportierte Berichte werden als geschachtelte Tabelle angezeigt, in der der Berichtshauptteil dargestellt wird. Ein Tablix-Datenbereich wird als geschachtelte Tabelle gerendert, die die Struktur des Datenbereichs im Bericht widerspiegelt. Textfelder und Rechtecke werden jeweils als Zelle innerhalb der Tabelle gerendert. Der Textfeldwert wird in der Zelle angezeigt.  
  
 Bilder, Diagramme, Datenbalken, Sparklines, Karten, Indikatoren und Messgeräte werden als statische Bilder innerhalb einer Tabellenzelle gerendert. Links und Drillthroughlinks zu diesen Berichtselementen werden gerendert. Zuordnungen und Bereiche, auf die innerhalb eines Diagramms geklickt werden kann, werden nicht unterstützt.  
  
 Spaltenberichte im Newsletterformat werden nicht in Word gerendert. Der Berichtshauptteil, Seitenhintergrundbilder und Farben werden nicht gerendert.  
  
##  <a name="pagination"></a><a name="Pagination"></a> Paginierung  
 Nachdem der Bericht in Word geöffnet wurde, wird er auf Grundlage des Seitenformats erneut paginiert. Durch die erneute Paginierung können Seitenumbrüche an unerwünschten Stellen eingefügt werden. In einigen Fällen kann der exportierte Bericht zwei aufeinander folgende Seitenumbrüche in einer Zeile oder hinzugefügte leere Seiten enthalten. Sie können versuchen, die Paginierung von Word zu ändern, indem Sie die Seitenränder anpassen.  
  
 Dieser Renderer unterstützt nur logische Seitenumbrüche.  
  
### <a name="page-sizing"></a>Anpassen der Seitengröße  
 Wenn der Bericht gerendert wird, wird die Seitenhöhe und -breite in Word von folgenden RDL-Eigenschaften bestimmt: Höhe und Breite des Papierformats, linker und rechter Seitenrand sowie oberer und unterer Seitenrand.  
  
### <a name="page-width"></a>Seitenbreite  
 Word unterstützt Seitenbreiten von bis zu 22 Zoll. Ist der Bericht breiter als 22 Zoll, wird der Bericht zwar dennoch vom Renderer gerendert, der Inhalt des Berichts wird jedoch von Word nicht in der Seitenlayoutansicht oder im Lesemoduslayout angezeigt. Um die Daten anzuzeigen, wechseln Sie zur Normalansicht oder Weblayoutansicht. In diesen Ansichten reduziert Word die Menge an Leerzeichen und zeigt dadurch mehr vom Berichtsinhalt an.  
  
 Beim Rendern wird der Bericht auf die erforderliche Breite (maximal 22 Zoll) geändert, um den Inhalt anzuzeigen. Die minimale Breite des Berichts basiert auf der RDL-Width-Eigenschaft im Bereich „Eigenschaften“.  
  
##  <a name="document-properties"></a><a name="DocumentProperties"></a> Dokumenteigenschaften  
 Der Word-Renderer schreibt die folgenden Metadaten in die DOCX-Datei.  
  
|Berichtselementeigenschaften|BESCHREIBUNG|  
|-------------------------------|-----------------|  
|Berichtstitel (Berichtstitel)|Titel|  
|Autor des Berichts|Autor|  
|Berichtsbeschreibung|Kommentare|  
  
##  <a name="page-headers-and-footers"></a><a name="ReportHeadersFooters"></a> Seitenkopfzeilen und -fußzeilen  
 Kopf- und Fußzeilen werden als Kopf- und Fußzeilenbereiche in Word gerendert. Wenn eine Berichtsseitennummer oder ein Ausdruck, der die Gesamtzahl der Berichtsseiten angibt, in der Kopf- oder Fußzeile erscheint, wird diese bzw. dieser in ein Word-Feld übersetzt, sodass die genaue Seitenzahl im gerenderten Bericht angezeigt wird. Wenn die Kopf- oder Fußzeilenhöhe im Bericht festgelegt wird, kann Word diese Einstellung nicht unterstützen. Die PrintOnFirstPage-Eigenschaft kann unter bestimmten Umständen angeben, ob in einer Kopf- bzw. Fußzeile auf der ersten Seite eines Berichts Text enthalten ist. Wenn der gerenderte Bericht mehrere Seiten umfasst und jede Seite lediglich einen einzelnen Abschnitt enthält, können Sie PrintOnFirstPage auf FALSE festlegen, und der Text wird auf der ersten und letzten Seite unterdrückt. Andernfalls ist der Text enthalten, unabhängig vom Wert der PrintOnFirstPage-Eigenschaft.  
  
 Der Word-Renderer analysiert alle Ausdrücke in Kopf- und Fußzeilen, wenn Berichte nach Word exportiert werden. Viele Ausdrücke werden erfolgreich analysiert, und die erwarteten Werte werden in den Kopf- und Fußzeilen aller Berichtseiten angezeigt.  
  
 Wenn eine Kopf- oder Fußzeile jedoch einen komplexen Ausdruck enthält, der auf unterschiedlichen Berichtseiten unterschiedliche Werte ergibt, wird auf allen Berichtseiten ggf. der gleiche Wert angezeigt. Die Seitenzahlen der folgenden beiden Ausdrücke werden im exportierten Bericht nicht erhöht. Die Seitenzahl ergibt auf allen Berichtseiten den gleichen Wert.  
  
-   `="Page: " + Globals!PageNumber.ToString + " of " + Globals!TotalPages.ToString`  
  
-   `=Avg(Fields!YTDPurchase.Value, "Sales") & " Page Number " & Globals!PageNumber`  
  
 Dies liegt daran, dass der Word-Renderer den Bericht für Felder analysiert, die mit der Paginierung zusammenhängen, wie **PageNumber** und **TotalPages** , und nur einfache Verweise verarbeitet und keine Funktionsaufrufe. In diesem Fall wird vom Ausdruck die **ToString** -Funktion aufgerufen. Die folgenden beiden Ausdrücke sind gleichwertig und werden beide ordnungsgemäß gerendert, wenn Sie die Vorschau für den Bericht im Berichts-Generator oder Berichts-Designer anzeigen. Sie können den veröffentlichten Bericht aber auch in einem [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Webportal oder einer SharePoint-Bibliothek rendern. Der Word-Renderer analysiert jedoch nur den zweiten Ausdruck erfolgreich und rendert die richtigen Seitenzahlen.  
  
-   **Komplexer Ausdruck:**  Der Ausdruck ist `="Average Sales " & Avg(Fields!YTDPurchase.Value, "Sales") & " Page Number " & Globals!PageNumber`  
  
-   **Ausdruck mit Textausführungen:** Text, **Average Sales** und der Ausdruck `=Avg(Fields!YTDPurchase.Value, "Sales)` und Text, **Page Number** und der Ausdruck `=Globals!PageNumber`  
  
 Um dieses Problem zu umgehen, verwenden Sie mehrere Textausführungen statt eines komplexen Ausdrucks, wenn Sie Ausdrücke in Fuß- und Kopfzeilen verwenden. Die folgenden beiden Ausdrücke sind äquivalent. Der Erste ist ein komplexer Ausdruck, der Zweite verwendet Textausführungen. Der Word-Renderer analysiert nur den zweiten Ausdruck erfolgreich.  
  
##  <a name="interactivity"></a><a name="Interactivity"></a> Interaktivität  
 Einige interaktive Elemente werden in Word unterstützt. Im Folgenden werden spezifische Funktionsweisen beschrieben.  
  
### <a name="show-and-hide"></a>Einblenden und Ausblenden  
 Der Word-Renderer rendert Berichtselemente auf Grundlage ihres Status beim Rendern. Wenn der Status eines Berichtselements ausgeblendet wird, wird das Berichtselement nicht im Word-Dokument gerendert. Wird der Status eines Berichtselements angezeigt, wird das Berichtselement im Word-Dokument gerendert. Die Funktion zum Wechseln zwischen beiden Möglichkeiten wird in Word nicht unterstützt.  
  
### <a name="document-map"></a>Dokumentstruktur  
 Wenn Dokumentstrukturbezeichnungen im Bericht vorhanden sind, werden sie als Word-Inhaltsverzeichnisbezeichnungen in den entsprechenden Berichtselementen und -gruppen gerendert. Die Dokumentstrukturbezeichnung wird als Bezeichnungstext für das Inhaltsverzeichnis verwendet. Der Ziellink wird in der Nähe des Elements, auf dem die Bezeichnung festgelegt wird, positioniert. Wenn kein Inhaltsverzeichnis im Word-Dokument erstellt wird, können Sie mit den im Bericht gerenderten Dokumentstrukturbezeichnungen ein eigenes Inhaltsverzeichnis erstellen.  
  
### <a name="hyperlink-and-drillthrough-links"></a>Links und Drillthroughlinks  
 Links und Drillthroughlinks im Textfeld und Bildberichtselemente werden als Links im Word-Dokument gerendert. Wenn Sie auf den Link klicken, wird der Standardwebbrowser geöffnet und navigiert zur URL. Beim Klicken auf den Drillthroughlink wird auf den ursprünglichen Berichtsserver zugegriffen.  
  
### <a name="interactive-sorting"></a>Interaktives Sortieren  
 Der Berichtsinhalt wird auf der Basis der aktuellen Sortierung innerhalb des Berichtsdatenbereichs gerendert. Word unterstützt keine interaktive Sortierung. Nachdem der Bericht gerendert wurde, können Sie die Tabellensortierung in Word anwenden.  
  
### <a name="bookmarks"></a>Lesezeichen  
 Lesezeichen im Bericht werden als Word-Lesezeichen gerendert. Lesezeichenlinks werden als Links zu den Lesezeichenbezeichnungen innerhalb des Dokuments gerendert. Lesezeichenbezeichnungen müssen weniger als 40 Zeichen lang sein. Das einzige Sonderzeichen, das in einer Lesezeichenbezeichnung verwendet werden kann, ist ein Unterstrich (_). Nicht unterstützte Sonderzeichen werden aus dem Namen der Lesezeichenbezeichnung entfernt. Wenn der Name mehr als 40 Zeichen umfasst, wird er gekürzt. Bei doppelt vorhandenen Lesezeichennamen im Bericht werden die Lesezeichen nicht in Word gerendert.  
  
##  <a name="word-style-rendering"></a><a name="WordStyleRendering"></a> Rendern von Formaten in Word  
 Im Folgenden wird kurz beschrieben, wie Formate in Word gerendert werden.  
  
### <a name="color-palette"></a>Farbpalette  
 Im Bericht gerenderte Farben werden im Word-Dokument gerendert.  
  
### <a name="border"></a>Rahmen  
 Rahmen für Berichtselemente, mit Ausnahme des Seitenrands, werden als Word-Tabellenzellrahmen gerendert.  
  
##  <a name="squiggly-lines-in-exported-reports"></a><a name="SquigglyLines"></a> Wellenlinien in exportierten Berichten  
 Exportierte Berichtsdaten und -konstanten, die in Word angezeigt werden, können mit roten der grünen Wellenlinien unterlegt sein. Die roten Wellenlinien weisen auf Rechtschreibfehler, die grünen Wellenlinien auf Grammatikfehler hin. Dieser Fall tritt immer dann ein, wenn Berichte Wörter enthalten, die nicht die Korrekturhilferichtlinien (Rechtschreibung und Grammatik) der angegebenen Bearbeitungssprache in Word erfüllen. So werden z. B. englische Spaltentitel mit hoher Wahrscheinlichkeit mit roten Wellenlinien unterlegt, wenn der Bericht mit einer spanischen Word-Version gerendert wird. Rechtschreibfehler sind in Berichten häufiger anzutreffen als Grammatikfehler, da Berichte in der Regel nur kurze Texte und keine vollständigen Sätze oder Absätze enthalten.  
  
 Die Wellenlinien in den Berichten suggerieren, dass der Bericht Fehler enthält, was aber meistens nicht der Fall ist. Sie können die Wellenlinien entfernen, indem Sie die Korrekturhilfensprache für den Bericht ändern. Um die Sprache für die Korrekturhilfen zu ändern, wählen Sie den Inhalt des Berichts aus, und geben Sie anschließend die gewünschte Sprache an. Sie können den gesamten Inhalt oder nur einen Teil des Inhalts auswählen. In Word befindet sich die Sprachoption **Sprache für die Korrekturhilfen festlegen** im Bereich **Sprache** auf der Registerkarte **Überprüfen** . Nachdem Sie den Inhalt aktualisiert haben, müssen Sie das Dokument neu speichern.  
  
 Je nachdem welche Sprachversion von Office Sie verwenden, sind die Korrekturhilfen (z. B. Wörterbücher) der ausgewählten Sprache entweder bereits im Programm enthalten oder müssen separat als [!INCLUDE[msCoName](../../includes/msconame-md.md)] Office-Sprachpaket erworben werden.  
  
 Die folgenden Themen enthalten weitere Informationen zum Einstellen der Office- und Word-Optionen.  
  
-   Ändern Sie die Bearbeitungssprache in den **Microsoft Office-Spracheinstellungen** oder im Dialogfeld **Word-Optionen** in Word. Weitere Informationen finden Sie unter [Festlegen der Bearbeitungs-, Anzeige- oder Hilfespracheinstellungen](https://office.microsoft.com/word-help/enable-the-use-of-other-languages-in-your-office-programs-HA010354783.aspx?CTT=1).  
  
-   Fügen Sie die Office-Sprachpakete hinzu, und ändern Sie anschließend die Bearbeitungssprache. Weitere Informationen finden Sie unter [Festlegen der Bearbeitungs-, Anzeige- oder Hilfespracheinstellungen](https://office.microsoft.com/word-help/enable-the-use-of-other-languages-in-your-office-programs-HA010354783.aspx?CTT=1) und [Office-Sprachoptionen](https://office.microsoft.com/language/).  
  
> [!NOTE]  
>  Wenn Sie die Bearbeitungssprache in den **Microsoft Office-Spracheinstellungen** oder im Dialogfeld **Word-Optionen** in Word ändern, gilt die Änderung für alle Office-Programme.  
  
##  <a name="word-limitations"></a><a name="WordLimitations"></a> Beschränkungen in Word  
 Die folgenden Beschränkungen gelten für [!INCLUDE[ofprword](../../includes/ofprword-md.md)]:  
  
-   Word-Tabellen unterstützen maximal 63 Spalten. Wenn Sie einen Bericht mit mehr als 63 Spalten erstellen und versuchen, diesen zu rendert, wird die Tabelle von Word geteilt. Die zusätzlichen Spalten werden angrenzend an die im Hauptteil des Berichts angezeigten 63 Spalten platziert. Daher werden die Berichtsspalten möglicherweise nicht in der erwarteten Reihenfolge angezeigt.  
  
-   Word unterstützt ein Seitenformat von maximal 22 Zoll Breite und 22 Zoll Höhe. Wenn der Inhalt über 22 Zoll hinausgeht, werden einige Daten unter Umständen nicht in der Seitenlayoutansicht angezeigt.  
  
-   Word ignoriert Einstellungen für die Höhe der Kopf- und Fußzeilen.  
  
-   Nachdem der Bericht exportiert wurde, paginiert Word den Bericht erneut. Dadurch werden dem gerenderten Bericht möglicherweise zusätzliche Seitenumbrüche hinzugefügt.  
  
-   Word wiederholt keine Kopfzeilen ab der zweiten Seite, auch nicht, wenn Sie die RepeatOnNewPage-Eigenschaft der statischen Kopfzeile in einem Tablix (Tabelle, Matrix oder Liste) auf **TRUE** festgelegt haben. Sie können explizite Seitenumbrüche im Bericht definieren, um die Anzeige von Kopfzeilen auf neuen Seiten zu erzwingen. Da Word jedoch eine eigene Paginierung auf den nach Word exportierten, gerenderten Bericht anwendet, können die Ergebnisse abweichen, und die Kopfzeile wird möglicherweise nicht erwartungsgemäß wiederholt. Die statische Kopfzeile ist die Zeile, die die Spaltenüberschriften enthält.  
  
-   Textfelder werden größer, wenn sie geschützte Leerzeichen enthalten.  
  
-   Wenn Text nach Word exportiert wird, kann der Text mit Schriftdekorationen bei einigen Schriftarten zu unerwarteten oder fehlenden Symbolen im gerenderten Bericht führen.  
  
##  <a name="benefits-of-using-the-word-renderer"></a><a name="WordBenefits"></a> Vorteile der Verwendung des Word-Renderers  
 Der Word-Renderer ermöglicht nicht nur den Zugriff auf die neuen Funktionen, die in [!INCLUDE[ofprword](../../includes/ofprword-md.md)] .docx-Dateien für exportierte Berichte verfügbar sind, sondern stellt mit dem *.docx-Format meistens auch kleinere Dateien zur Verfügung. Die mit dem Word-Renderer exportierten Berichte sind normalerweise deutlich kleiner als die gleichen Berichte, die mit dem Word 2003-Renderer exportiert werden.  
  
## <a name="backward-compatibility-of-exported-reports"></a>Abwärtskompatibilität exportierter Berichte  
 Sie können einen Word-Kompatibilitätsmodus auswählen und die Kompatibilitätsoptionen festlegen. Der Word-Renderer erstellt Dokumente, für die der Kompatibilitätsmodus aktiviert ist. Wenn die Dokumente mit deaktiviertem Kompatibilitätsmodus erneut gespeichert werden, kann sich dies auf das Dokumentlayout auswirken.  
  
 Wenn Sie den Kompatibilitätsmodus deaktivieren und einen Bericht erneut speichern, kann sich das Berichtslayout auf unerwartete Weise ändern.  
  
##  <a name="the-word-2003-renderer"></a><a name="AvailabilityWord"></a> Der Word 2003-Renderer  
  
> [!IMPORTANT]  
>  Die [!INCLUDE[ofprword](../../includes/ofprword-md.md)] 2003-Renderingerweiterung (.doc) ist veraltet. Weitere Informationen finden Sie unter [Als veraltet markierte Funktionen in SQL Server Reporting Services in SQL Server 2016](~/reporting-services/deprecated-features-in-sql-server-reporting-services-ssrs.md).  
  
 Der Word-Renderer ist kompatibel mit [!INCLUDE[ofprword](../../includes/ofprword-md.md)] 2003 sowie [!INCLUDE[msCoName](../../includes/msconame-md.md)] mit dem installierten Office Compatibility Pack für Word, Excel und PowerPoint. Weitere Informationen finden Sie unter [Microsoft Office Compatibility Pack für Word, Excel und PowerPoint](https://www.microsoft.com/download/office.aspx).
  
 Die frühere Version der Word-Renderingerweiterung, die kompatibel mit [!INCLUDE[ofprword](../../includes/ofprword-md.md)] 2003 ist, wird in Word 2003 umbenannt. Nur die Word-Renderingerweiterung ist standardmäßig verfügbar. Sie müssen die Reporting Services-Konfigurationsdateien aktualisieren, um die Word 2003-Renderingerweiterung verfügbar zu machen. Der Inhaltstyp von Dateien, die vom Word 2003-Renderer generiert werden, ist **application/vnd.ms-word** , und die Dateinamenerweiterung der Dateien lautet „.doc“.  
  
 In SQL Server Reporting Services entspricht der Word-Standardrenderer der Version, die im Format von [!INCLUDE[ofprword](../../includes/ofprword-md.md)] (.docx) rendert. Dies ist die **Word** -Option, die in den Menüs **Exportieren** in eine [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Webportal- und SharePoint-Liste aufgeführt ist. Die frühere, nur mit [!INCLUDE[ofprword](../../includes/ofprword-md.md)] 2003 kompatible Version heißt jetzt Word 2003 und ist in Menüs aufgeführt, die diesen Namen verwenden. Die Menüoption **Word 2003** ist standardmäßig nicht sichtbar, ein Administrator kann sie jedoch sichtbar machen, indem er die RSReportServer-Konfigurationsdatei aktualisiert. Um Berichte aus [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] mit dem Word 2003-Renderer zu exportieren, aktualisieren Sie die RSReportDesigner-Konfigurationsdatei. Aber auch wenn der Word 2003-Renderer sichtbar gemacht wird, steht er nicht in allen Szenarien zur Verfügung. Da sich die RSReportServer-Konfigurationsdatei auf dem Berichtsserver befindet, müssen die Tools oder Produkten, aus denen Sie Berichte exportieren, mit einem Berichtsserver verbunden sein, um die Konfigurationsdatei zu lesen. Wenn Sie Tools oder Produkte im getrennten oder lokalen Modus verwenden, hat das Sichtbarmachen des Word 2003-Renderers keine Auswirkungen. Die Menüoption **Word 2003** ist weiterhin nicht verfügbar. Wenn Sie den Word 2003-Renderer in der RSReportDesigner-Konfigurationsdatei sichtbar machen, ist die Menüoption **Word 2003** in der [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] -Berichtsvorschau immer verfügbar.  
  
 Die Menüoption **Word 2003** ist in den folgenden Szenarien nie sichtbar:  
  
-   Berichts-Generator im getrennten Modus, und Sie zeigen einen Bericht im Berichts-Generator in der Vorschau an.  
  
-   Berichts-Viewer-Webpart im lokalen Modus, und die SharePoint-Farm ist nicht in einen [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Berichtsserver integriert. Weitere Informationen zum lokalen Modus finden Sie unter [Berichte im lokalen Modus im Vergleich zu Berichten im verbundenen Modus im Berichts-Viewer (Reporting Services im SharePoint-Modus)](../../reporting-services/report-server-sharepoint/local-mode-vs-connected-mode-reports-in-the-report-viewer.md)  
  
 Wenn der **Word 2003** -Renderer für Sichtbarkeit konfiguriert ist, ist in den folgenden Szenarien sowohl die **Word** - als auch die **Word 2003** -Menüoption verfügbar:  
  
-   [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Webportal, wenn Reporting Services im einheitlichen Modus installiert ist.  
  
-   SharePoint-Website, wenn Reporting Services im integrierten SharePoint-Modus installiert ist.  
  
-   [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] , und Sie zeigen Berichte in der Vorschau an.  
  
-   Berichts-Generator, der mit einem Berichtsserver verbunden ist.  
  
-   Das Berichts-Viewer-Webpart im Remotemodus.  
  
 Die folgende XML zeigt die Elemente für die beiden Word-Renderingerweiterungen in der RSReportServer- und der RSReportDesigner-Konfigurationsdatei an:  
  
 `<Extension Name="WORDOPENXML" Type="Microsoft.ReportingServices.Rendering.WordRenderer.WordOpenXmlRenderer.WordOpenXmlDocumentRenderer,Microsoft.ReportingServices.WordRendering"/>`  
  
 `<Extension Name="WORD" Type="Microsoft.ReportingServices.Rendering.WordRenderer.WordDocumentRenderer,Microsoft.ReportingServices.WordRendering" Visible="false"/>`  
  
 Die WORDOPENXML-Erweiterung definiert den Word-Renderer für [!INCLUDE[ofprword](../../includes/ofprword-md.md)] -.docx-Dateien. Die WORD-Erweiterung definiert die [!INCLUDE[ofprword](../../includes/ofprword-md.md)] 2003-Version. `Visible = "false"` gibt an, dass der Word 2003-Renderer ausgeblendet wird. Weitere Informationen finden Sie unter [RSReportServer.config-Konfigurationsdatei](../../reporting-services/report-server/rsreportserver-config-configuration-file.md) und [RSReportDesigner-Konfigurationsdatei](../../reporting-services/report-server/rsreportdesigner-configuration-file.md).  
  
### <a name="differences-between-the-word-and-word-2003-renderers"></a>Unterschiede zwischen dem Word-Renderer und dem Word 2003-Renderer  
 Es gibt keinen visuellen Unterschied zwischen Berichten, die mit dem Word- bzw. Word 2003-Renderer gerendert werden. Möglicherweise fallen Ihnen jedoch kleinere Unterschiede zwischen dem Word- und dem Word 2003-Format auf.  
  
##  <a name="device-information-settings"></a><a name="DeviceInfo"></a> Geräteinformationseinstellungen  
 Sie können einige Standardeinstellungen für diesen Renderer ändern, beispielsweise Hyperlinks und Drillthroughlinks auslassen oder alle Elemente erweitern, die unabhängig vom ursprünglichen Status des Elements beim Rendern aktiviert bzw. deaktiviert werden können. Ändern Sie dazu die Geräteinformationseinstellungen. Weitere Informationen finden Sie unter [Word Device Information Settings](../../reporting-services/word-device-information-settings.md).  

## <a name="next-steps"></a>Nächste Schritte

[Paginierung in Reporting Services](../../reporting-services/report-design/pagination-in-reporting-services-report-builder-and-ssrs.md)   
[Renderingverhalten](../../reporting-services/report-design/rendering-behaviors-report-builder-and-ssrs.md)   
[Interaktive Funktionalität für verschiedene Berichtsrenderingerweiterungen](../../reporting-services/report-builder/interactive-functionality-different-report-rendering-extensions.md)   
[Rendern von Berichtselementen](../../reporting-services/report-design/rendering-report-items-report-builder-and-ssrs.md)   
[Tables, Matrices, and Lists (Tabellen, Matrizen und Listen)](../../reporting-services/report-design/tables-matrices-and-lists-report-builder-and-ssrs.md)  

Haben Sie dazu Fragen? [Stellen Sie eine Frage im Reporting Services-Forum](https://go.microsoft.com/fwlink/?LinkId=620231)
