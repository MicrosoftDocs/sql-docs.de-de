---
title: Berichtserstellung in der Azure Synapse Pathway-Vorschauversion
description: Azure Synapse Pathway bietet eine umfassende Berichterstellung für übersetzte Skripts.
author: anshul82-ms
ms.author: anrampal
ms.topic: tutorial
ms.prod: sql
ms.technology: tools-other
ms.date: 03/02/2021
monikerRange: =azure-sqldw-latest
ms.custom: template-tutorial
ms.openlocfilehash: 26701c33f713a1b82e3bab604a03e7dca054a36b
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186341"
---
# <a name="report-generation-for-azure-synapse-pathway-preview"></a>Berichterstellung für die Azure Synapse Pathway-Vorschauversion
[!INCLUDE [Azure Synapse Analytics](../../includes/applies-to-version/asa.md)]

Azure Synapse Pathway bietet einen umfassenden Bericht über die Anzahl der erfolgreich übersetzten Skripts. Der Bericht informiert außerdem über die Anzahl der Fehler und Warnungen für Anweisungen, die nicht übersetzt wurden.

## <a name="generate-report"></a>Erstellen des Berichts

Wenn Sie auf **Übersetzen** klicken, wird wie unten dargestellt ein Bericht angezeigt:

![Azure Synapse Pathway-Bericht](./media/report-generaration/report-overview.png)

## <a name="report-summary"></a>Zusammenfassung des Berichts

Unter „Conversion Success“ (Konvertierungserfolg) wird der Prozentsatz der erfolgreich übersetzten Skripts angezeigt.

![Azure Synapse Pathway](./media/report-generaration/conversion-success.png)

Im Abschnitt „Statement Distribution“ (Anweisungsverteilung) wird die Anzahl der DDL- und DML-Anweisungen angegeben, die übersetzt wurden, einschließlich der Anzahl der erstellten Schemas, Tabellen und Datenbanken.

![Azure Synapse-Bericht 1](./media/report-generaration/statement-distribution.png)

Die Einsparungen bei der Migration werden basierend auf der Anzahl und der Komplexität (einfach, hoch oder mittel) der übersetzten Objekte berechnet. Synapse Pathway geht von einem manuellen Aufwand von 30 Minuten pro Objektübersetzung aus.

![Azure Synapse-Bericht 2](./media/report-generaration/migration-savings.png)

## <a name="next-steps"></a>Nächste Schritte

[Azure Synapse Pathway herunterladen](synapse-pathway-download.md)