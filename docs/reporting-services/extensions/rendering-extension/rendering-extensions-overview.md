---
title: Übersicht über Renderingerweiterungen | Microsoft-Dokumentation
description: In diesem Artikel werden die in den Reporting Services enthaltenen Datenrenderingerweiterungen behandelt. Dabei erfahren Sie, wie Sie benutzerdefinierte Renderingerweiterungen hinzufügen, um Berichte in anderen Formaten zu erstellen.
ms.date: 12/7/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- formats [Reporting Services], rendering extensions
- rendering extensions [Reporting Services], about extensions
ms.assetid: 909356a0-4709-43e5-b597-33bd9bb22882
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: b3d3dc5fac6915faafb48b045b70c5b57f6e6ad8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100071120"
---
# <a name="rendering-extensions-overview"></a>Übersicht über Renderingerweiterungen
  Eine Renderingerweiterung stellt eine Komponente oder ein Modul eines Berichtsservers dar, mit dem Daten- und Layoutinformationen in ein gerätespezifisches Format umgewandelt werden. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] umfasst sieben Renderingerweiterungen: HTML, Excel, Word, CSV oder Text, XML, Image und PDF. Sie können zusätzliche Renderingerweiterungen erstellen, um Berichte in anderen Formaten zu generieren.  
  
> [!NOTE]  
>  Welche Renderingerweiterungen zur Verfügung stehen, sehen Sie auf der Liste der installierten Erweiterungen in der Datei RSReportServer.config.  
  
 In der folgenden Tabelle werden die Renderingerweiterungen beschrieben, die in [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] enthalten sind.  
  
|Name der Erweiterung|BESCHREIBUNG|  
|--------------------|-----------------|  
|**XML**|Rendert einen Bericht im XML-Format. Der Bericht wird in einem Browser geöffnet. Weitere Umwandlungen, die auf dieses XML-Format angewandt werden, können eine kosteneffiziente Methode darstellen, die Entwicklung einer eigenen Renderingerweiterung zu vermeiden.|  
|**CSV**|Rendert einen Bericht in einem durch Trennzeichen getrennten Format. Der Bericht wird in einem Anzeigetool für Dateien im CSV-Format geöffnet.|  
|**IMAGE**|Rendert einen Bericht in einem seitenbasierten Format. Das Format wird als **TIFF** im Export-Dropdownfeld der Berichtssymbolleiste angezeigt.|  
|**PDF**|Rendert einen Bericht in Adobe Acrobat Reader. Das Format wird als **PDF-Datei (Acrobat)** im Export-Dropdownfeld der Berichtssymbolleiste angezeigt.|  
|**EXCEL**|Rendert einen Bericht in [!INCLUDE[ofprexcel](../../../includes/ofprexcel-md.md)].|  
|**WORD**|Rendert einen Bericht in [!INCLUDE[ofprword](../../../includes/ofprword-md.md)].|  
|**HTML 4.0** (Teil der HTML-Renderingerweiterung)|HTML ist das Format, in dem der Bericht ursprünglich gerendert wird. Wenn Ihr Browser HTML 4.0 unterstützt, wird dieses Format verwendet. Andernfalls wird HTML 3.2 verwendet.|  
|**MHTML** (Teil der HTML-Renderingerweiterung)|Rendert einen Bericht im MHTML-Format. Der Bericht wird in Internet Explorer geöffnet. Das Format wird als **Webarchiv** im Export-Dropdownfeld der Berichtssymbolleiste angezeigt.|  
|**NULL**|Rendert einen Bericht in keinem bestimmten Format. Diese Renderingerweiterung ist dann sinnvoll, wenn Berichte im Cache eingefügt werden sollen. NULL-Rendering sollte in Verbindung mit einer geplanten Ausführung oder Übermittlung verwendet werden.|  
  
 Weitere Informationen zu den empfohlenen Formaten und deren Verwendung finden Sie unter [Exportieren von Berichten (Berichts-Generator und SSRS)](../../../reporting-services/report-builder/export-reports-report-builder-and-ssrs.md).  
  
 Jede Renderingerweiterung, die von [!INCLUDE[msCoName](../../../includes/msconame-md.md)] implementiert und mit [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] geliefert wird, verwendet eine Reihe gemeinsamer Schnittstellen. Damit wird sichergestellt, dass jeder Erweiterung vergleichbare Funktionen implementiert und die Komplexität des Renderingcodes im Kern des Berichtsservers reduziert.  
  
## <a name="rendering-object-model"></a>Renderingobjektmodell  
 Wenn ein Bericht verarbeitet wird, ist das Ergebnis ein öffentliches verfügbares Objektmodell, das als Renderingobjektmodell (ROM) bezeichnet wird. Das Renderingobjektmodell ist eine Auflistung von Klassen, die Inhalte, Layout und Daten eines verarbeiteten Berichts definieren. Das ROM steht Entwicklern zur Verfügung, die benutzerdefinierte Renderingerweiterungen für [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] entwerfen, entwickeln und bereitstellen möchten. Ein ROM wird erstellt, wenn der Berichtsserver die XML-Definition eines Berichts zusammen mit den benutzerdefinierten Berichtsdaten verarbeitet. Wenn die Verarbeitung abgeschlossen ist, wird das öffentliche Modell von einer Renderingerweiterung zur Definition der Berichtsausgabe verwendet. Die verfügbaren öffentlichen Klassen des ROMs sind im **Microsoft.ReportingServices.OnDemandReportRendering**-Namespace definiert.  
  
## <a name="writing-custom-rendering-extensions"></a>Schreiben von benutzerdefinierten Renderingerweiterungen  
 Bevor Sie sich dazu entscheiden, eine benutzerdefinierte Renderingerweiterung zu erstellen, sollten Sie einfachere Alternativen prüfen. Ihre Möglichkeiten:  
  
-   Passen Sie eine gerenderte Ausgabe an, indem Sie die Geräteinformationseinstellungen für vorhandene Erweiterungen angeben.  
  
-   Fügen Sie benutzerdefinierte Formatierungs- und Präsentationsfunktionen hinzu, indem Sie XSL-Transformationen (XSLT) mit der Ausgabe des XML-Renderingformats kombinieren.  
  
 Das Schreiben von benutzerdefinierten Renderingerweiterungen ist nicht ganz einfach. Eine Renderingerweiterung muss normalerweise alle möglichen Kombinationen von Berichtselementen unterstützen und erfordert, dass Sie Hunderte von Klassen, Schnittstellen, Methoden und Eigenschaften implementieren. Wenn Sie einen Bericht in einem Format rendern müssen, das nicht in [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] enthalten ist, und sich entscheiden, eine eigene Implementierung einer Renderingerweiterung mit verwaltetem Code zu schreiben, dann muss die Renderingerweiterung die vom Berichtsserver benötigte **Microsoft.ReportingServices.OnDemandReportRendering.IRenderingExtension**-Schnittstelle implementieren.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Implementing a Rendering Extension (Implementieren von Renderingerweiterungen)](../../../reporting-services/extensions/rendering-extension/implementing-a-rendering-extension.md)   
 [Reporting Services Extension Library (Reporting Services-Erweiterungsbibliothek)](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
