---
title: Berichtsdatenbereich
description: In diesem Artikel erfahren Sie, wie Sie den Berichtsdatenbereich verwenden, um die aktuell definierten Parameter, Datenquellen, Datasets, Feldsammlungen und Bilder in Ihrem Bericht anzuzeigen.
author: maggiesMSFT
ms.author: maggies
ms.reviewer: ''
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
ms.custom: seodec18
ms.date: 12/14/2018
ms.openlocfilehash: c7afab0edc7afd86b16103e5364d17a93579d096
ms.sourcegitcommit: 6be9a0ff0717f412ece7f8ede07ef01f66ea2061
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/01/2020
ms.locfileid: "85812615"
---
# <a name="report-data-pane-in-sql-server-reporting-services-ssrs"></a>Berichtsdatenbereich in SQL Server Reporting Services (SSRS)

  Verwenden Sie den Bereich **Berichtsdaten** , um die aktuell definierten Parameter, Datenquellen, Datasets, Feldauflistungen und Bilder in Ihrem Bericht anzuzeigen. Die Berichtsdatenbereich stellt eine hierarchische Ansicht der Elemente zur Verfügung, die im Bericht Daten darstellen. Die Knoten der obersten Ebene stellen integrierte Felder, Parameter, Bilder und Datenquellenverweise dar. Erweitern Sie jeden Knoten, um die Datenelemente anzuzeigen. Wenn Sie z. B. einen Datenquellenknoten erweitern, werden die für diese Datenquelle definierten Datasets angezeigt. Wenn Sie ein Dataset erweitern, wird seine Feldauflistung angezeigt. Ziehen Sie Elemente in die Berichtsentwurfsoberfläche, um Daten mit Berichtselementen auf der Berichtsseite zu verknüpfen.  
  
## <a name="options"></a>Tastatur

 **Integrierte Felder**  
 Stellt Felder dar, die von Reporting Services bereitgestellt und in einem Bericht häufig verwendet werden, beispielsweise den Berichtsnamen oder Seitennummern. Weitere Informationen finden Sie unter [Integrierte Sammlungen in Ausdrücken &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/built-in-collections-in-expressions-report-builder.md).  
  
 **Parameter**  
 Stellt die Auflistung der Berichtsparameter dar, von denen jeder einwertig oder mehrwertig sein kann. Weitere Informationen finden Sie unter [Berichtsparameter &#40;Berichts-Generator und Berichts-Designer&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)" basiert.  
  
 **Images**  
 Stellt den Bildersatz dar, der im Bericht verwendet wird. Weitere Informationen finden Sie unter [Bilder &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/images-report-builder-and-ssrs.md).  
  
 **Datenquelle**  
 Stellt einen einzelnen Datenquellenverweis auf eine eingebettete Datenquelle oder eine freigegebene Datenquelle dar. In [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]werden freigegebene Datenquellen im Projektmappen-Explorer im Ordner Freigegebene Datenquellen angezeigt. Eine Datenquelle gibt einen der Datenquellentypen an, der von Reporting Services unterstützt wird. Eine Datenquelle ist der übergeordnete Knoten für die Auflistung der Datasets, die darauf basieren. Weitere Informationen finden Sie unter [Erstellen von Datenverbindungszeichenfolgen (Berichts-Generator und SSRS)](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md).  
  
 **Dataset**  
 Stellt ein einzelnes Dataset dar. Ein Dataset ist der übergeordnete Knoten für die Auflistung von Feldern, die durch die Abfrage und bestimmte berechnete Felder angegeben werden. Reporting Services unterstützt Abfrage-Designer, die Ihnen das Angeben einer Abfrage erleichtern. Weitere Informationen finden Sie unter [Berichtsdatasets (SSRS)](../../reporting-services/report-data/report-datasets-ssrs.md) und [Abfrageentwurfstools (SSRS)](../../reporting-services/report-data/query-design-tools-ssrs.md).  
  
## <a name="next-steps"></a>Nächste Schritte

 - [Datasetfelder-Sammlung &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-data/dataset-fields-collection-report-builder-and-ssrs.md)
 - [Gruppierungsbereich](../../reporting-services/tools/grouping-pane.md)