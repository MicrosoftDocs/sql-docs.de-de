---
title: FAQ zur Vorschauversion von Azure Synapse Pathway
description: Häufig gestellte Fragen zu Azure Synapse Pathway
author: anshul82-ms
ms.author: anrampal
ms.topic: overview
ms.date: 03/02/2021
ms.prod: sql
ms.technology: tools-other
monikerRange: =azure-sqldw-latest
ms.custom: template-overview
ms.openlocfilehash: 8352fb6a70c54ede61d544a147f970237404c9f5
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186322"
---
# <a name="azure-synapse-pathway-preview-faq"></a>FAQ zur Vorschauversion von Azure Synapse Pathway
[!INCLUDE [Azure Synapse Analytics](../../includes/applies-to-version/asa.md)]

Dieser Leitfaden enthält die am häufigsten gestellten Fragen zur Vorschauversion von Azure Synapse Pathway.

## <a name="general"></a>Allgemein

### <a name="q-what-is-azure-synapse-pathway"></a>Q. Was ist Azure Synapse Pathway?

A. Azure Synapse Pathway ist ein Codeübersetzungstool, das Sie dabei unterstützt, ein Upgrade auf eine moderne Data-Warehouse-Plattform durchzuführen. Hierfür automatisiert das Tool die Übersetzung des Codes für Ihr bestehendes Data Warehouse, um dafür zu sorgen, dass dieser mit dem T-SQL-basierten Azure Synapse SQL-System kompatibel ist.

### <a name="q-how-can-i-download-azure-synapse-pathway"></a>Q. Wie kann ich Azure Synapse Pathway herunterladen?

A. Synapse Pathway kann über das [Microsoft Download Center](https://aka.ms/synapse-pathway-download) heruntergeladen werden.

### <a name="q-how-much-does-azure-synapse-pathway-cost"></a>Q. Wie viel kostet Azure Synapse Pathway?

A. Für das Herunterladen des Tools und Ihre Codeübersetzungen mit Synapse Pathway fallen keine Kosten an.

### <a name="q-what-sourcetarget-pairs-does-azure-synapse-pathway-currently-support"></a>Q. Welche Quelle-Ziel-Paare werden derzeit von Azure Synapse Pathway unterstützt?

A. In dieser Vorschauversion von Synapse Pathway sind die folgenden Data Warehouses als Quellen verfügbar:
- Microsoft SQL Server
- Snowflake
- Netezza

### <a name="q-what-is-included-as-part-of-the-code-conversion"></a>Q. Wofür ist eine Codekonvertierung möglich?

A. Synapse Pathway unterstützt die Codeübersetzung für Tabellen, Schemas, Sichten und gespeicherte Prozeduren.

### <a name="q-can-it-also-scan-my-environment-and-provide-an-assessment-report-of-all-the-objects-that-need-to-be-convertedtranslated"></a>Q. Kann Synapse Pathway auch meine Umgebung überprüfen und einen Bewertungsbericht für alle Objekte bereitstellen, die konvertiert/übersetzt werden müssen?

A. In dieser Vorschauversion von Synapse Pathway müssen Sie den Link zu den DDL- und DML-Skripts angeben, die übersetzt werden müssen. Synapse Pathway überprüft nicht Ihre aktuelle Umgebung, um zu übersetzende Objekte zu identifizieren.

### <a name="q-what-information-does-azure-synapse-pathway-capture-about-my-current-data-warehouse-instance"></a>Q. Welche Informationen erfasst Azure Synapse Pathway zu meiner aktuellen Data-Warehouse-Instanz?

A. Da Sie Synapse Pathway in Ihrer lokalen Umgebung ausführen können, werden außer Ihrem Namen und Ihrer Organisation keine Daten erfasst. Diese Daten sind erforderlich, um die MSI-Datei im Microsoft Download Center herunterzuladen.

### <a name="q-where-can-i-raise-issues-encountered-in-azure-synapse-pathway"></a>Q. Wo kann ich Issues für Probleme in Azure Synapse Pathway erstellen?

A. Azure Synapse Pathway ist aktuell in der **Vorschauversion** verfügbar.   Support für Synapse Pathway erhalten Sie über den Microsoft-Supportkanal. Sie können das Ticket entweder im Azure-Portal erstellen oder in den Standardportalen (in der Regel lokaler Support).


> [!NOTE] 
> Wie bei anderen Azure-Diensten wird für alle Vorschauversionen von Diensten Support bereitgestellt, es gelten allerdings keine SLAs.

<!-- ### Troubleshooting and optimization

#### Q. Why do I see slow performance while running the code conversion?

#### Q. Translation of errors or unexpected results? -->

## <a name="next-steps"></a>Nächste Schritte

[Azure Synapse Pathway herunterladen](synapse-pathway-download.md)