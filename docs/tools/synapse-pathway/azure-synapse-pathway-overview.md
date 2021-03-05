---
title: Übersicht über die Vorschauversion von Azure Synapse Pathway
description: Azure Synapse Pathway ist ein Tool für die Migration von Data Warehouses zu Azure Synapse Analytics.
ms.author: anrampal
ms.topic: overview
ms.date: 03/02/2021
ms.prod: sql
ms.technology: Azure Synapse Pathway
monikerRange: =azure-sqldw-latest
ms.custom: template-overview
ms.openlocfilehash: d7289d2bfe099dad7bbc91ccd5060797f7aad997
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101873131"
---
# <a name="azure-synapse-pathway-preview"></a>Vorschauversion von Azure Synapse Pathway
[!INCLUDE [Azure Synapse Analytics](../../includes/applies-to-version/asa.md)]

Wenn Kunden die Modernisierung ihrer Data-Warehouse-Systeme in Erwägung ziehen, ist eines der wichtigsten Hindernisse das Übersetzen ihres SQL-Codes. Der vorhandene Code wurde für das aktuelle System geschrieben und optimiert, muss nun jedoch für das neue System optimiert werden, zu dem migriert wird.

Auf der ganzen Welt möchten Organisationen ihre Analyseplattformen modernisieren, um die Gesamtkosten zu senken und von Innovationsvorteilen zu profitieren. Die Kunden haben jedoch Tausende von Arbeitsstunden und mehrere Millionen Dollar in einige Zehntausend für ihre Data Warehouses geschriebene Codezeilen investiert.
 
Für die Übersetzung dieses wichtigen SQL-Codes müssen Kunden entweder den vorhandenen SQL-Code selbst manuell umschreiben oder große Teile ihres Budgets in das Umschreiben oder Konvertieren ihres Codes durch einen externen Dienstleister investieren.

> [!IMPORTANT]
> Azure Synapse Pathway befindet sich aktuell in der öffentlichen Vorschauphase.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
 
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). 

**Azure Synapse Pathway** unterstützt Sie beim Upgrade auf eine moderne Data-Warehouse-Plattform durch Automatisierung der Übersetzung von Code für Ihr bestehendes Data Warehouse. Es handelt sich dabei um ein kostenloses, intuitives und leicht zu verwendendes Tool, das die Codeübersetzung automatisiert und dadurch eine schnellere Migration zu Azure Synapse Analytics ermöglicht.

 ![Übersicht über Azure Synapse Pathway](./media/pathway-overview/synapse-pathway-overview.png) 

Synapse Pathway übersetzt DDL-Anweisungen (Data Definition Language, Datendefinitionssprache) und DML-Anweisungen (Data Manipulation Language, Datenbearbeitungssprache) in eine T-SQL-konforme Sprache, die mit Azure Synapse SQL kompatibel ist.

## <a name="behind-the-scenes"></a>Abläufe im Hintergrund

Weitere Informationen zu Azure Synapse Pathway finden Sie im Artikel [Hintergrundinformationen zur Vorschauversion von Azure Synapse Pathway](synapse-pathway-behind-the-scenes.md), in dem die Funktionsweise von Azure Synapse Pathway ausführlich erläutert wird.

## <a name="get-azure-synapse-pathway"></a>Abrufen von Azure Synapse Pathway

Die Voraussetzungen für die Installation von Synapse Pathway sowie einen Link zum Herunterladen der aktuellen Version finden Sie unter [Herunterladen der Vorschauversion von Azure Synapse Pathway](synapse-pathway-download.md).

## <a name="supported-sources"></a>Unterstützte Quellen

Azure Synapse Pathway unterstützt die Konvertierung von Code für Datenbanken, Schemas und Tabellen für die folgenden Quellen:
- **IBM Netezza** 
- **Microsoft SQL Server**
- **Snowflake**

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

Weitere Informationen zu Azure Synapse Pathway finden Sie auf der [FAQ-Seite](pathway-faq.md).

## <a name="next-steps"></a>Nächste Schritte

- [Führen Sie Ihre erste Übersetzung mit Azure Synapse Pathway durch.](synapse-pathway-assessment.md)
- Ankündigungsblog: [Ankündigung von Azure Synapse Pathway: Vorantreiben Ihrer Data-Warehouse-Migration – Microsoft Tech Community](https://techcommunity.microsoft.com/t5/azure-synapse-analytics/announcing-azure-synapse-pathway-turbocharge-your-data-warehouse/ba-p/2176630)


