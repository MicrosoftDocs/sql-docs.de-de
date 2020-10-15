---
title: Power Pivot-Verbindungstyp | Microsoft-Dokumentation
description: Mithilfe der Informationen in diesem Artikel zum Power Pivot-Verbindungstyp erfahren Sie, wie Sie eine Datenquelle erstellen.
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
ms.assetid: a104c3c7-f118-4d02-9a0f-6859f1469d11
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: c83a33e56bca1cba9fab1c5e3ef873e9aa312f8a
ms.sourcegitcommit: 783b35f6478006d654491cb52f6edf108acf2482
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91891730"
---
# <a name="power-pivot-connection-type-ssrs"></a>Power Pivot-Verbindungstyp (SSRS)
  Sie können Daten mithilfe der SQL Server Analysis Services-Datenverarbeitungserweiterung aus einer [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] -Arbeitsmappe abrufen, die in einem SharePoint- [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] -Katalog veröffentlicht ist.  
  
 Verwenden Sie die Informationen in diesem Thema, um eine Datenquelle zu erstellen. Eine Schritt-für-Schritt-Anleitung finden Sie unter [Hinzufügen und Prüfen einer Datenverbindung &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-data/add-and-verify-a-data-connection-report-builder-and-ssrs.md).  
  
## <a name="prerequisites"></a>Voraussetzungen  
 Die [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] -Datenquelle muss in einem [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] -Katalog auf einer SharePoint-Website veröffentlicht sein.  
  
 Zur Unterstützung von Verbindungen vom Berichts-Generator mit einer [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] -Arbeitsmappe muss SQL Server 2008 R2 ADOMD.NET auf Ihrem Arbeitsstationscomputer installiert sein. Diese Clientbibliothek wird mit [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] für Excel installiert. Wenn Sie jedoch einen Computer verwenden, der nicht über diese Anwendung verfügt, müssen Sie ADOMD.NET von der Seite [SQL Server 2008 R2 Feature Pack](https://www.microsoft.com/download/details.aspx?id=44272)herunterladen und installieren.  
  
## <a name="data-source-type"></a>Datenquellentyp  
 Verwenden Sie den Berichtsdatenquellentyp **Microsoft SQL Server Analysis Services**.  
  
## <a name="connection-string"></a>Verbindungszeichenfolge  
 Die Verbindungszeichenfolge ist die URL der [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]-Arbeitsmappe, die auf SharePoint im [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]-Katalog oder einer anderen Bibliothek veröffentlicht wurde, z.B. `https://contoso-srv/subsite/PowerPivotLibrary/ContosoSales.xlsx`.  
  
## <a name="credentials"></a>Anmeldeinformationen  
 Geben Sie die Anmeldeinformationen an, die Sie benötigen, um auf die [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] -Arbeitsmappe und die SharePoint-Website zuzugreifen, z.B. die Windows-Authentifizierung (Integrierte Sicherheit). Weitere Informationen finden Sie unter [Erstellen von Datenverbindungszeichenfolgen (Berichts-Generator und SSRS)](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md) oder [Angeben der Anmeldeinformationen und Verbindungsinformationen für Berichtsdatenquellen](specify-credential-and-connection-information-for-report-data-sources.md).  
  
## <a name="queries"></a>Abfragen  
 Nachdem Sie eine Verbindung mit der [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] -Datenquelle hergestellt haben, verwenden Sie die grafische MDX-Abfrage, um durch Durchsuchen und Auswählen aus den zugrunde liegenden Datenstrukturen eine Abfrage zu erstellen. Nach dem Erstellen einer Abfrage können Sie die Abfrage so ausführen, dass die Beispieldaten im Ergebnisbereich angezeigt werden.  
  
 Mit dem Abfrage-Designer wird die Abfrage analysiert, um die Datasetfelder zu bestimmen. Sie können die Datasetfeldauflistung im Bereich **Berichtsdaten** auch manuell bearbeiten. Weitere Informationen finden Sie unter [Hinzufügen, Bearbeiten und Aktualisieren von Feldern im Berichtsdatenbereich &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-data/add-edit-refresh-fields-in-the-report-data-pane-report-builder-and-ssrs.md).  
  
## <a name="filters"></a>Filter  
 Geben Sie im Bereich "Filter" Dimensionen und Elemente an, die ausgefiltert oder in die Abfrageergebnisse eingeschlossen werden sollen.  
  
## <a name="parameters"></a>Parameter  
 Aktivieren Sie im Bereich "Filter" die Option **Parameter** für einen Filter, um automatisch einen Berichtsparameter mit verfügbaren Werten zu erstellen, die der Filterauswahl entsprechen.  
  
## <a name="remarks"></a>Bemerkungen  
 Wenn Sie den Berichts-Generator aus der [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] -Arbeitsmappe in einem [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] -Katalog öffnen, werden PivotTables, PivotCharts, Slicer und andere Layout- und analytische Funktionen aus der [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] -Arbeitsmappe im Bericht nicht neu erstellt. Stattdessen enthält der leere Bericht eine vorkonfigurierte Datenquelle, die auf die Daten in der [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] -Arbeitsmappe verweist. Berichte auf Grundlage einer [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] -Arbeitsmappe zu entwerfen, kann arbeitsintensiv und zeitaufwändig sein, abhängig von der Anzahl von Slicern, Filtern und Tabellen oder Diagrammen, die Sie im Bericht erneut erstellen möchten. Ein besserer Ansatz ist, die Präsentation der Daten zu planen, die Sie unabhängig vom [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] -Entwurf in einem Bericht haben möchten.  
  
 Die Daten in einer [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] -Arbeitsmappe sind stark komprimiert; aus der [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] -Arbeitsmappe für einen Bericht abgerufene Daten sind nicht komprimiert. Geben Sie im Abfrage-Designer Filter und Parameter an, um die Daten auf die für den Bericht erforderliche Menge zu begrenzen.  
  
 Im Gegensatz zu einer Verbindung mit einem Analysis Services-Cube hat ein [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] -Modell keine Hierarchien. Um ähnliche Funktionalität wie die von verwandten Slicern in der Arbeitsmappe bereitzustellen, müssen Sie kaskadierende Parameter im Bericht erstellen. Weitere Informationen finden Sie unter [Hinzufügen von kaskadierenden Parametern zu einem Bericht &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/add-cascading-parameters-to-a-report-report-builder-and-ssrs.md)erstellen.  
  
 In einigen Fällen müssen Sie möglicherweise Ausdrücke an die zugrunde liegenden Datenwerte aus dem [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] -Modell anpassen. Sie müssen möglicherweise Ausdrücke ändern, um Daten in den richtigen Datentyp zu konvertieren oder eine Aggregatfunktion hinzuzufügen oder zu entfernen. Verwenden Sie zum Konvertieren eines Datentyps aus einer Zeichenfolge in eine ganze Zahl beispielsweise `=CInt`. Überprüfen Sie vor der Veröffentlichung des Berichts stets, ob der Bericht die erwarteten Werte aus den Daten im [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] -Modell anzeigt.  
  
 Vorschaubilder eines Berichts in einem [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] -Katalog werden nur generiert, wenn die folgenden Bedingungen erfüllt sind:  
  
-   Der Bericht und die [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] -Arbeitsmappe mit den Daten müssen in einem gemeinsamen [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] -Katalog gespeichert sein.  
  
-   Der Bericht enthält nur [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] -Daten aus einer [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] -Datenquelle.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Benutzeroberfläche des MDX-Abfrage-Designers für Analysis Services &#40;Berichts-Generator&#41;](/previous-versions/sql/)   
 [Ausdrücke &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)  
  
