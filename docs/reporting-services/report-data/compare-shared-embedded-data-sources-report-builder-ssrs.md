---
description: 'Vergleichen von freigegebenen und eingebetteten Datenquellen: Berichts-Generator und Reporting Services (SSRS)'
title: 'Vergleichen von freigegebenen und eingebetteten Datenquellen: Berichts-Generator und Reporting Services | Microsoft-Dokumentation'
ms.date: 11/18/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 5e2c72ad95392be7571c05e1dddcee1ed35c3d6a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100042220"
---
# <a name="compare-shared-and-embedded-data-sources---report-builder--reporting-services-ssrs"></a>Vergleichen von freigegebenen und eingebetteten Datenquellen: Berichts-Generator und Reporting Services (SSRS)

[!INCLUDE [ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE [ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)]
 
Sie können eine Verbindung mit Daten entweder über eine freigegebene oder eine eingebettete Datenquelle herstellen. Eine *freigegebene Datenquelle* wird unabhängig von jeglichem Bericht definiert. Sie können diese in mehreren Berichten auf einem Berichtsserver oder einer SharePoint-Website verwenden. Eine *eingebettete Datenquelle* wird in einem Bericht definiert. Sie können sie nur in diesem Bericht verwenden. 

 Freigegebene Datenquellen sind hilfreich für Datenquellen, die Sie häufig verwenden. Es wird empfohlen, so oft wie möglich freigegebene Datenquellen zu erstellen und zu verwenden. Mit ihnen können Berichte und der Berichtszugriff einfacher verwaltet und Berichte und die Datenquellen, auf die sie zugreifen, sicherer gemacht werden. Wenn Sie eine freigegebene Datenquelle benötigen, müssen Sie sich möglicherweise an den Systemadministrator wenden, damit er eine für Sie erstellt.  
  
 Bei einer eingebetteten Datenquelle, die auch als *berichtsspezifische Datenquelle* bezeichnet wird, handelt es sich um eine Datenverbindung, die in der Berichtsdefinition gespeichert wird. Verbindungsinformationen zu einer eingebetteten Datenquelle können nur von dem Bericht verwendet werden, in den diese eingebettet wurde. Verwenden Sie das Dialogfeld **Datenquelleneigenschaften** , um eingebettete Datenquellen zu definieren und zu verwalten.  
  
 Der Unterschied zwischen eingebetteten und freigegebenen Datenquellen liegt in der Art der Erstellung, Speicherung und Verwaltung.  
  
-   In Berichts-Designer werden eingebettete oder freigegebene Datenquellen als Teil eines [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] -Projekts erstellt. Sie können steuern, ob Sie sie für die Vorschau lokal verwenden oder sie als Teil des Projekts auf einem Berichtsserver oder einer SharePoint-Website bereitstellen möchten. Sie können benutzerdefinierte Datenerweiterungen verwenden, die auf dem Computer und dem Berichtsserver oder der SharePoint-Website installiert wurden, auf dem bzw. der die Berichte bereitgestellt werden.  
  
     Systemadministratoren können zusätzliche Datenverarbeitungserweiterungen und .NET Framework-Datenanbieter installieren und konfigurieren. Weitere Informationen finden Sie unter [Datenverarbeitungserweiterungen und .NET Framework-Datenanbieter &#40;SSRS&#41;](../../reporting-services/report-data/data-processing-extensions-and-net-framework-data-providers-ssrs.md).  
  
     Entwickler können mithilfe der <xref:Microsoft.ReportingServices.DataProcessing> -API Datenverarbeitungserweiterungen erstellen, durch die weitere Datenquellentypen unterstützt werden.  
  
-   Wechseln Sie im [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)]zu einem Berichtsserver oder zu einer SharePoint-Website, und wählen Sie freigegebene Datenquellen aus, oder erstellen Sie eingebettete Datenquellen im Bericht. Freigegebene Datenquellen können nicht im [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] erstellt werden. Sie können keine benutzerdefinierten Datenerweiterungen im [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] verwenden.  

## <a name="summary-of-differences"></a>Zusammenfassung der Unterschiede
  
 In der folgenden Tabelle werden die Unterschiede zwischen eingebetteten und freigegebenen Datenquellen zusammengefasst.  
  
|BESCHREIBUNG|Eingebettet<br /><br /> Data source|Shared<br /><br /> Data source|  
|-----------------|------------------------------|----------------------------|  
|Die Datenverbindung ist in die Berichtsdefinition eingebettet.|![Verfügbar](../../reporting-services/report-data/media/greencheck.gif "Verfügbar")||  
|Der Zeiger auf die Datenverbindung auf dem Berichtsserver ist in die Berichtsdefinition eingebettet.||![Verfügbar](../../reporting-services/report-data/media/greencheck.gif "Verfügbar")|  
|Wird auf dem Berichtsserver verwaltet.|![Verfügbar](../../reporting-services/report-data/media/greencheck.gif "Verfügbar")|![Verfügbar](../../reporting-services/report-data/media/greencheck.gif "Verfügbar")|  
|Ist für freigegebene Datasets erforderlich.||![Verfügbar](../../reporting-services/report-data/media/greencheck.gif "Verfügbar")|  
|Ist für Komponenten erforderlich.||![Verfügbar](../../reporting-services/report-data/media/greencheck.gif "Verfügbar")|  

## <a name="next-steps"></a>Nächste Schritte

[Erstellen, Ändern und Löschen von freigegebenen Datenquellen (SSRS)](../../reporting-services/report-data/create-modify-and-delete-shared-data-sources-ssrs.md)   
[Erstellen und Verwenden eingebetteter Datenquellen](../../reporting-services/report-data/create-and-modify-embedded-data-sources.md)   
[Festlegen von Bereitstellungseigenschaften](../../reporting-services/tools/set-deployment-properties-reporting-services.md)   
[Angeben der Anmeldeinformationen und Verbindungsinformationen für Berichtsdatenquellen](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md)  

Haben Sie dazu Fragen? [Stellen Sie eine Frage im Reporting Services-Forum](https://go.microsoft.com/fwlink/?LinkId=620231)
