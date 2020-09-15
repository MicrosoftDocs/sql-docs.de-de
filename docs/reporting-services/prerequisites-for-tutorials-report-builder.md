---
title: Voraussetzungen für Tutorials (Berichts-Generator) | Microsoft-Dokumentation
description: Informieren Sie sich über die Voraussetzungen zum Abschließen der Berichts-Generator-Tutorials.
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
ms.assetid: 9b8346a6-f4f4-4ad3-bc98-8f2be342ef2d
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 15ca9251c5cb9f541710d7a18b8c10864cd24b8c
ms.sourcegitcommit: e8f6c51d4702c0046aec1394109bc0503ca182f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2020
ms.locfileid: "87945516"
---
# <a name="prerequisites-for-tutorials-report-builder"></a>Voraussetzungen für Lernprogramme (Berichts-Generator)

Die Tutorials zum Berichts-Generator setzen voraus, dass Sie paginierte Berichte aus [!INCLUDE[ssRSCurrent](../includes/ssrscurrent-md.md)] auf einem Berichtsserver oder einer in einen Berichtsserver integrierten SharePoint-Website anzeigen und speichern können. Für Daten werden in allen Tutorials wörtliche Abfragen verwendet, die von einer SQL Server-Instanz verarbeitet werden müssen.  
  
Wenn Sie keinen Zugriff auf einen Berichtsserver, eine Website oder eine Datenquelle haben, können Sie sich mit Berichts-Generator vertraut machen, indem Sie einen Offlinebericht erstellen. Siehe [Tutorial: Erstellen eines Quick-Diagrammberichts offline &#40;Berichts-Generator&#41;](../reporting-services/report-builder/tutorial-create-a-quick-chart-report-offline-report-builder.md).  

## <a name="requirements"></a>Requirements (Anforderungen)

Für die Ausführung der Lernprogramme zum Berichts-Generator gelten die folgenden Voraussetzungen:  
  
-   Zugriff auf den Berichts-Generator Sie können den Berichts-Generator auf einem [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] -Berichtsserver oder einem [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] -Berichtsserver im integrierten SharePoint-Modus ausführen. Nur der erste Schritt ist auf den verschiedenen Servern anders, also das Öffnen des Berichts-Generators.  
  
    Wählen Sie auf einem Berichtsserver **Neu** > **Paginierter Bericht**.
  
    Wählen Sie auf einem Berichtsserver im integrierten SharePoint-Modus in der Registerkarte **Dokumente** die Option **Neues Dokument**und wählen Sie aus der Dropdown-Liste **Berichts-Generator-Bericht**. Beispiel: `https://<servername>/sites/mySite/reports`. Der SharePoint-Administrator muss die Funktion "Berichts-Generator-Bericht" für jede Dokumentbibliothek aktivieren.  
  
-   Sie benötigen die URL zu einem [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] -Berichtsserver oder einer in einen [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] -Berichtsserver integrierten SharePoint-Website. Sie benötigen Berechtigungen zum Speichern und Anzeigen von Berichten, freigegebenen Datenquellen, freigegebenen Datasets, Berichtsteilen und Modellen. Standardmäßig ist die URL für den Berichtsserver `https://<servername>/reportserver`. Standardmäßig ist die URL für eine SharePoint-Website `https://<sitename>` oder `https://<server>/site`.  
  
-   Den Namen einer SQL Server-Instanz und entsprechende Anmeldeinformationen für den schreibgeschützten Zugriff auf eine beliebige Datenbank. In den Datasetabfragen im Tutorial werden zwar Literaldaten verwendet, die Abfrage muss jedoch von einer SQL Server-Instanz verarbeitet werden, um die für ein Berichtsdataset erforderlichen Metadaten zurückzugeben. Die folgende Verbindungszeichenfolge gibt z. B. nur einen Server an: `data source=<servername>`. Sie müssen Lesezugriff auf die Standarddatenbank besitzen, die Ihnen vom Systemadministrator zugewiesen wird, der Ihnen die Berechtigung für den Zugriff auf den Server gewährt. Sie können auch eine Datenbank entsprechend der folgenden Verbindungszeichenfolge angeben: `data source=<servername>;initial catalog=<database>`.  
  
-   Für das [Tutorial: Kartenbericht (Berichts-Generator)](tutorial-map-report-report-builder.md) muss der Berichtsserver für die Unterstützung von Bing-Karten als Hintergrund konfiguriert werden. Weitere Informationen finden Sie unter [Planen der Unterstützung für Kartenberichte](https://docs.microsoft.com/sql/reporting-services/report-design/plan-a-map-report-report-builder-and-ssrs).   

-   Das [Tutorial: Erstellen von Drillthrough- und Hauptberichten (Berichts-Generator)](tutorial-creating-drillthrough-and-main-reports-report-builder.md) erfordert Zugriff auf den Contoso Sales-Cube. Weitere Informationen finden Sie im Tutorial. 
  
Der Berichtsserveradministrator muss Ihnen die erforderlichen Berechtigungen für den Berichtsserver erteilen und die Orte für die [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] -Ordner sowie die Standardoptionen für den Berichts-Generator konfigurieren. Weitere Informationen finden Sie unter [Installieren des Berichts-Generators](install-windows/install-report-builder.md).  

## <a name="next-steps"></a>Nächste Schritte

[Tutorials (Berichts-Generator)](../reporting-services/report-builder-tutorials.md)  

Haben Sie dazu Fragen? [Stellen Sie eine Frage im Reporting Services-Forum](https://go.microsoft.com/fwlink/?LinkId=620231)
