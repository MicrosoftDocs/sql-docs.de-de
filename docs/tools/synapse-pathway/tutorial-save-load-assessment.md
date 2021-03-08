---
title: Speichern und Laden von Bewertungen mit der Vorschauversion von Azure Synapse Pathway
description: Speichern und Laden von Data Warehouse-Bewertungen mit der Vorschauversion von Azure Synapse Pathway
author: anshul82-ms
ms.author: anrampal
ms.prod: sql
ms.technology: tools-other
ms.topic: tutorial
ms.date: 03/02/2021
monikerRange: =azure-sqldw-latest
ms.openlocfilehash: fbaa518e2f2a5485f85fc7812291beb88896ac6e
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186226"
---
# <a name="save-and-load-assessments-with-azure-synapse-pathway-preview"></a>Speichern und Laden von Bewertungen mit der Vorschauversion von Azure Synapse Pathway
[!INCLUDE [Azure Synapse Analytics](../../includes/applies-to-version/asa.md)]

Die folgenden ausführlichen Anleitungen veranschaulichen, wie Sie eine Data Warehouse-Bewertung mithilfe von Azure Synapse Pathway aus einer Datei speichern und hochladen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Speichern einer Bewertung in einer Datei
> * Laden einer Bewertung aus einer Datei

## <a name="prerequisites"></a>Voraussetzungen

Sie müssen [Azure Synapse Pathway](synapse-pathway-download.md) installiert haben, um dieses Tutorial abzuschließen. Weitere Informationen zu diesem Tool finden Sie in der [Übersicht zu Azure Synapse Pathway](azure-synapse-pathway-overview.md).

## <a name="saving-an-assessment-to-a-file"></a>Speichern einer Bewertung in einer Datei

1. Sobald Sie die Übersetzung ausgeführt haben, sollte der Bericht angezeigt werden, in dem die Codeübersetzung zusammengefasst ist. ![Übersicht über den Bericht zur Azure Synapse Pathway-Bewertung](./media/tutorial-save-load-assessment/report-overview.png)
1. Klicken Sie auf die Schaltfläche **Bewertung speichern**, geben Sie den Namen der Datei an, und wählen Sie dann **Speichern** aus.
![Azure Synapse Pathway-Bewertung](./media/tutorial-save-load-assessment/save-assessment.png)

1. Eine ASMPRJ-Datei wird am angegebenen Ziel erstellt.

## <a name="loading-an-assessment-from-a-file"></a>Laden einer Bewertung aus einer Datei

1. Wenn Sie die Bewertung öffnen möchten, klicken Sie auf **Bewertung laden**, und geben Sie den ASMPRJ-Dateinamen an. ![Browsen zum Speicherort der Azure Synapse Pathway-Bewertung](./media/tutorial-save-load-assessment/browse-location.png)

1. Die Quell-, Eingabe- und Ausgabeordner werden basierend auf der ausgewählten Bewertung aufgefüllt.
![Konfiguration der Azure Synapse Pathway-Bewertung mit dem Übersetzungstyp, dem Eingabeverzeichnis und dem Ausgabeverzeichnis](./media/tutorial-save-load-assessment/load-assessment.png)
1. Wählen Sie **Übersetzen** aus, um die Codeübersetzung noch mal auszuführen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Berichterstellung](report-generation.md)
