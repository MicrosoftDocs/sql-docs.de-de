---
title: Verwenden des RSClientPrint-Steuerelements in benutzerdefinierten Anwendungen | Microsoft-Dokumentation
description: RSPrintClient sorgt für clientseitigen Druck von Berichten, die in einem HTML-Viewer angezeigt werden. Der Berichtsserver rendert den Bericht in die Renderingerweiterung für Bilder (EMF).
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service
ms.topic: reference
helpviewer_keywords:
- RSPrintClient control
- print controls [Reporting Services]
- custom printing [Reporting Services]
- client-side printing
ms.assetid: 8c0bdd18-8905-4e22-9774-a240fc81a8a7
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 1c977799cb20d3735b4cc77d2608c0412ce51769
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100022283"
---
# <a name="using-the-rsclientprint-control-in-custom-applications"></a>Verwenden des RSClientPrint-Steuerelements in benutzerdefinierten Anwendungen
  Das [!INCLUDE[msCoName](../../../includes/msconame-md.md)]-ActiveX-Steuerelement **RSPrintClient** sorgt für clientseitigen Druck von Berichten, die in einem HTML-Viewer angezeigt werden. Es enthält das Dialogfeld **Drucken**, mit dessen Hilfe ein Benutzer Druckaufträge initiieren, Berichte in der Vorschau anzeigen, Seiten zum Drucken angeben und Ränder ändern kann. Während eines clientseitigen Druckvorgangs rendert der Berichtsserver den Bericht mithilfe der Bildrenderingerweiterung (EMF) und verwendet die Druckfunktionen des Betriebssystems zum Erstellen des Druckauftrags und zum Senden des Auftrags an einen Drucker.  
  
 Mithilfe der clientseitigen Druckfunktion kann die Qualität eines Druckes für einen HTML-Bericht gesteuert und verbessert werden, indem die Druckereinstellungen des Browsers auf dem Computer des Benutzers übergangen und stattdessen die Seitengrößen, Ränder sowie der Kopf- und Fußzeilentext des Berichts zum Erstellen der Druckausgabe verwendet werden. Vom Drucksteuerelement werden Eigenschaftswerte des Berichts gelesen, um die Seitengröße und Ränder festzulegen.  
  
 Entwickler, die die clientseitige Druckfunktion auf Symbolleisten oder in Viewern von Drittanbietern aktivieren möchten, können über das COM-Objekt **RSClientPrint** auf das ActiveX-Steuerelement zugreifen. Das Steuerelement kann frei verteilt werden. In der folgenden Liste sind Empfehlungen für die Verwendung des Steuerelements enthalten:  
  
-   Verwenden Sie das Steuerelement zum Verbessern des Druckes von webbasierten Berichten. Sie können das Objekt in jeder mit [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] kompatiblen Programmiersprache oder in einem Skript angeben. In [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows Forms-Anwendungen kann das Steuerelement nicht verwendet werden.  
  
-   Kopieren Sie die CAB-Datei aus den [!INCLUDE[ssRSNoVersion](../../../includes/ssrsnoversion-md.md)]-Programmdateien, und fügen Sie sie zur Codebasis Ihrer benutzerdefinierten Anwendung hinzu.  
 
-   Verwenden Sie ein \<OBJECT>-Tag, um das Steuerelement anzugeben.  
  
-   Geben Sie eine relative oder vollqualifizierte URL zur CAB-Datei im OBJECT CODEBASE-Attribut an.  
  
-   Geben Sie eigene Informationen zur Anwendungsversion der CAB-Datei an, damit Sie nachverfolgen können, welche Version in der Anwendung verwendet wird.  
    
  
## <a name="rsprintclient-overview"></a>Übersicht über RSPrintClient  
 Vom Steuerelement wird ein benutzerdefiniertes Dialogfeld zum Drucken angezeigt, das die in Dialogfeldern zum Drucken üblichen Funktionen enthält. Dazu zählen Druckvorschau, Seitenauswahl zum Angeben bestimmter Seiten und Bereiche, Seitenränder und Ausrichtung. Das Steuerelement ist in eine CAB-Datei verpackt. Der Text im Dialogfeld **Drucken** ist in allen Sprachen lokalisiert, die in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] unterstützt werden. Das ActiveX-Steuerelement **RSPrintClient** druckt den Bericht mithilfe der Bildrenderingerweiterung (EMF). Die folgenden EMF-Geräteinformationen werden verwendet: StartPage, EndPage, MarginBottom, MarginLeft, MarginTop, MarginRight, PageHeight und PageWidth. Andere Einstellungen für Geräteinformationen werden für Bildrendering nicht unterstützt.  
  
### <a name="language-support"></a>Sprachunterstützung  
 Das Drucksteuerelement stellt Text in der Benutzeroberfläche in unterschiedlichen Sprachen bereit und akzeptiert Eingabewerte mit einer Kalibrierung für unterschiedliche Maßsysteme. Welches Sprach- und Maßsystem verwendet wird, wird durch die Eigenschaften **Culture** und **UICulture** bestimmt. Beide Eigenschaften akzeptieren LCID-Werte. Wenn Sie einen LCID-Wert für eine Sprache angeben, die eine Variante einer unterstützten Sprache ist, wird die Sprache verwendet, die am nächsten zu Ihrer Angabe liegt. Falls Sie einen LCID-Wert angeben, der nicht unterstützt wird und auch keine enge Übereinstimmung mit einem anderen LCID-Wert aufweist, wird Englisch (Vereinigte Staaten) verwendet.  
  
## <a name="using-rsclientprint-in-code"></a>Verwenden von "RSClientPrint" im Code  
 Das **RSClientPrint**-Objekt wird zum programmseitigen Zugriff auf das ActiveX-Steuerelement und seiner Methoden und Eigenschaften verwendet. Das Steuerelement stellt einen modalen Dialog für die Druckvorschau bereit.  
  
### <a name="specifying-default-values"></a>Angeben von Standardwerten  
 Sie können das Dialogfeld **Drucken** mit den Rand- und Seitenwerten des Berichts initialisieren. Standardmäßig wird das Dialogfeld **Drucken** mit Werten aus der Berichtsdefinition initialisiert. Sie können die Standardwerte verwenden oder andere Werte angeben, indem Sie die Eigenschaften des Objekts festlegen.  
  
 Alle Dimensionen werden in Millimetern festgelegt. Die Maßkonvertierung findet zur Laufzeit statt, wenn **Culture** und **UICulture** auf Gebietsschemas festgelegt sind, in denen keine metrischen Maße verwendet werden.  
  
 Sie können erfahren, welche Werte für Seitengrößen und -ränder verwendet werden, wenn Sie mithilfe der **GetProperties**-Methode die Standardwerte abrufen:  
  
-   Mit **PageHeight** und **PageWidth** werden die Standardseitenhöhe und -breite angegeben. Beim Starten des Drucksteuerelements werden diese Eigenschaftswerte zur Auswahl der nächsten verfügbaren Papiergröße für den aktuell ausgewählten Drucker verwendet. Falls der Wert für **PageWidth** größer als der Wert für **PageHeight** ist, wird die Ausrichtung auf Querformat festgelegt. Andernfalls ist sie auf Hochformat festgelegt.  
  
-   **LeftMargin**, **RightMargin**, **TopMargin**, und **BottomMargin** werden standardmäßig auf jeweils 12,2 mm festgelegt.  
  
 Diese Eigenschaften werden in der **Item**-Eigenschaftsauflistung auf dem Berichtsserver gespeichert. Die Werte werden bei jedem Update der Berichtsdefinition überschrieben.  
  
### <a name="rsclientprint-properties"></a>RSClientPrint-Eigenschaften  
  
|Eigenschaft|type|RW|Standard|BESCHREIBUNG|  
|--------------|----------|--------|-------------|-----------------|  
|MarginLeft|Double|RW|Berichteinstellung|Ruft den linken Rand ab bzw. legt ihn fest. Wird kein Wert vom Entwickler festgelegt oder im Bericht angegeben, wird 12,2 Millimeter als Standardwert verwendet.|  
|MarginRight|Double|RW|Berichteinstellung|Ruft den rechten Rand ab bzw. legt ihn fest. Wird kein Wert vom Entwickler festgelegt oder im Bericht angegeben, wird 12,2 Millimeter als Standardwert verwendet.|  
|MarginTop|Double|RW|Berichteinstellung|Ruft den oberen Rand ab bzw. legt ihn fest. Wird kein Wert vom Entwickler festgelegt oder im Bericht angegeben, wird 12,2 Millimeter als Standardwert verwendet.|  
|MarginBottom|Double|RW|Berichteinstellung|Ruft den unteren Rand ab bzw. legt ihn fest. Wird kein Wert vom Entwickler festgelegt oder im Bericht angegeben, wird 12,2 Millimeter als Standardwert verwendet.|  
|PageWidth|Double|RW|Berichteinstellung|Ruft die Seitenbreite ab bzw. legt sie fest. Wird kein Wert vom Entwickler oder in der Berichtsdefinition festgelegt, wird 215,9 Millimeter als Standardwert verwendet.|  
|PageHeight|Double|RW|Berichteinstellung|Ruft die Seitenhöhe ab bzw. legt sie fest. Wird kein Wert vom Entwickler oder in der Berichtsdefinition festgelegt, wird 279,4 Millimeter als Standardwert verwendet.|  
|Kultur|Int32|RW|Browsergebietsschema|Gibt den Gebietsschemabezeichner (LCID) an. Mit diesem Wert wird die Maßeinheit für die Benutzereingaben bestimmt. Wenn ein Benutzer z.B. **3** eingibt, wird der Wert in Millimetern gemessen, falls die Sprache auf Französisch festgelegt wird. Ist die Sprache auf Englisch (Vereinigte Staaten) festgelegt, wird Zoll verwendet. Zu den gültigen Werten zählen folgende: 1028, 1031, 1033, 1036, 1040, 1041, 1042, 2052, 3082.|  
|UICulture|String|RW|Clientkultur|Gibt die Zeichenfolgenlokalisierung des Dialogfelds an. Der Text im Dialogfeld Drucken ist in folgende Sprachen lokalisiert: Vereinfachtes Chinesisch, Traditionelles Chinesisch, Englisch, Französisch, Deutsch, Italienisch, Japanisch, Koreanisch und Spanisch. Zu den gültigen Werten zählen folgende: 1028, 1031, 1033, 1036, 1040, 1041, 1042, 2052, 3082.|  
|Authenticate|Boolean|RW|False|Gibt an, ob das Steuerelement einen GET-Befehl für den Berichtsserver ausgibt, um eine Verbindung für das Drucken außerhalb der Sitzung zu initiieren.|  
  
### <a name="when-to-set-the-authenticate-property"></a>Festlegen der Authenticate-Eigenschaft  
 Wenn Sie von einer Browsersitzung heraus drucken, muss die **Authenticate**-Eigenschaft nicht festgelegt werden. Im Kontext einer aktiven Sitzung werden alle Anforderungen vom Drucksteuerelement an den Berichtsserver über den Browser verarbeitet. Der Browser legt die für die Kommunikation mit dem Berichtsserver erforderlichen Sitzungsvariablen fest.  
  
 Wenn Sie außerhalb der Sitzung drucken (wenn Sie z.B. einen Bericht direkt an einen Drucker senden, ohne ihn zuvor zu öffnen), muss das Drucksteuerelement eine HTTP **GET**-Anforderung ausgeben, um die Sitzung mit dem Berichtsserver einzurichten. Legen Sie **Authenticate** auf **TRUE** fest, um die **GET**-Anforderung auszugeben.  
  
 Sie müssen die **GET**-Anforderung nur dann ausgeben, wenn Sie die integrierte Sicherheit von Windows oder die Standardauthentifizierung verwenden. Beim Verwenden der Formularauthentifizierung wird die **Authenticate**-Eigenschaft ignoriert. Der Anwendungscode muss die Sitzung festlegen und den Benutzer mithilfe der von Ihnen angegebenen benutzerdefinierten Sicherheitserweiterung authentifizieren. Wenn Sie die Formularauthentifizierung verwenden, müssen Sie den Ablaufwert im Authentifizierungscookie so festlegen, dass Sitzungen eine angemessene Zeit lang beibehalten werden. Ist der Wert zu niedrig, werden die Benutzer jedes Mal, wenn das Cookie abläuft, zur Eingabe ihrer Anmeldeinformationen aufgefordert.  
  
### <a name="clsids"></a>CLSIDs  
 Wenn Sie den Bericht lokal ausführen, verwenden Sie die folgenden CLSID-Werte.  
  
-   41861299-EAB2-4DCC-986C-802AE12AC499  
  
-   5554DCB0-700B-498D-9B58-4E40E5814405  
  
-   60677965-AB8B-464f-9B04-4BA871A2F17F  
  
 Wenn Sie den Bericht in Azure SQL Reporting ausführen, verwenden Sie die folgenden CLSID-Werte.  
  
-   3DD32426-554D-48C0-A200-65D3BF880E38  
  
-   05662494-ACF9-446A-BE4C-7D3F7EA7F62F  
  
### <a name="rsprintclient-support-for-the-print-method"></a>RSPrintClient-Unterstützung für die Print-Methode  
 Das **RSClientPrint**-Objekt unterstützt die zum Starten des Dialogfelds „Drucken“ verwendete **Print**-Methode. Die **Print**-Methode besitzt die folgenden Argumente.  
  
|Argument|E/A|type|BESCHREIBUNG|  
|--------------|----------|----------|-----------------|  
|ServerPath|Geben Sie in|String|Gibt das virtuelle Verzeichnis des Berichtsservers an (z.B. `https://adventure-works/reportserver`).|  
|ReportPathParameters|Geben Sie in|String|Gibt den vollständigen Namen zum Bericht im Ordnernamespace des Berichtsservers einschließlich Parameter an. Berichte werden über den URL-Zugriff abgerufen. Beispiel: "/AdventureWorks Sample Reports/Employee Sales Summary&EmpID=1234"|  
|ReportName|Geben Sie in|String|Der kurze Name des Berichts (im Beispiel oben lautet der kurze Name Employee Sales Summary). Dieser Name wird im Dialogfeld Drucken und in der Druckerwarteschlange angezeigt.|  
  
### <a name="example"></a>Beispiel  
 Im folgenden HTML-Beispiel wird das Angeben der CAB-Datei, der **Print**-Methode und der Eigenschaften in JavaScript angezeigt:  
  
 `<BODY onload="Print()">`  
  
 `<OBJECT ID="RSClientPrint" CLASSID="CLSID: 5554DCB0-700B-498D-9B58-4E40E5814405D3" CODEBASE="<URL to the .CAB file>#Version=<your application version information>" VIEWASTEXT></OBJECT>`  
  
 `<script language="javascript">`  
  
 `function Print()`  
  
 `{`  
  
 `RSClientPrint.MarginLeft = 12.7;`  
  
 `RSClientPrint.MarginTop = 12.7;`  
  
 `RSClientPrint.MarginRight = 12.7;`  
  
 `RSClientPrint.MarginBottom = 12.7;`  
  
 `RSClientPrint.Culture = 1033;`  
  
 `RSClientPrint.UICulture = 9;`  
  
 `RSClientPrint.Print('https://localhost/rtm', '%2fEmployee_Sales_Summary&ReportMonth=6&ReportYear=2004&EmpID=20', 'Employee_Sales_Summary')`  
  
 `}`  
  
 `</script>`  
  
 `</BODY>`  
  
## <a name="see-also"></a>Weitere Informationen  
 [Drucken von Berichten in einem Browser mit dem Drucksteuerelement (Berichts-Generator und SSRS)](../../../reporting-services/report-builder/print-reports-from-a-browser-with-the-print-control-report-builder-and-ssrs.md)   
 [Drucken von Berichten &#40;Berichts-Generator und SSRS&#41;](../../../reporting-services/report-builder/print-reports-report-builder-and-ssrs.md)   
 [Geräteinformationseinstellungen für Bilder](../../../reporting-services/image-device-information-settings.md)  
  
  
