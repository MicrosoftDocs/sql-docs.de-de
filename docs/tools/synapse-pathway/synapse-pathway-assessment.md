---
title: Bewertung in der Vorschauversion von Azure Synapse Pathway
description: Durchführen einer Codeübersetzung für ein Data Warehouse mit Azure Synapse Pathway
author: anshul82-ms
ms.author: anrampal
ms.prod: sql
ms.technology: Azure Synapse Pathway
ms.topic: tutorial
ms.date: 03/02/2021
monikerRange: =azure-sqldw-latest
ms.custom: template-tutorial
ms.openlocfilehash: b76fecf9a8a7eafc84a1b9eebd746287dddf3af9
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101873107"
---
# <a name="tutorial-to-perform-your-first-code-translation-with-azure-synapse-pathway-preview"></a>Tutorial: Durchführen Ihrer ersten Codeübersetzung mit der Vorschauversion von Azure Synapse Pathway
[!INCLUDE [Azure Synapse Analytics](../../includes/applies-to-version/asa.md)]

Die Vorschauversion von Azure Synapse Pathway bietet Unterstützung für die Übersetzung von Schemas, Tabellen, Sichten, Funktionen usw. aus **Netezza**, **Snowflake** und **Microsoft SQL Server** in T-SQL-konformen Code, mit dem die Migration zu Azure Synapse Analytics automatisiert wird.

Weitere Informationen finden Sie unter [Vorschauversion von Azure Synapse Pathway](azure-synapse-pathway-overview).

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Durchführen Ihrer ersten Übersetzung von SQL-Skripts aus Ihrem bestehenden Data Warehouse in T-SQL-Skripts für Azure Synpase SQL 
> * Auswählen einer der verfügbaren Quellen
> * Anzeigen der Fehler und Warnungen für Objekte, die nicht übersetzt wurden

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie sicher, dass Sie [Azure Synapse Pathway](synapse-pathway-download.md) installiert haben, um die Schritte in diesem Tutorial ausführen zu können. Wenn Sie eine Einführung benötigen, finden Sie diese unter [Vorschauversion von Azure Synapse Pathway](azure-synapse-pathway-overview.md).

## <a name="run-the-translation"></a>Durchführen der Übersetzung

1. Starten Sie die Azure Synapse Pathway-MSI-Datei. 

1. Wählen Sie eine der verfügbaren Quellen aus. Die Quellen, die bald noch ergänzt werden, sind derzeit noch ausgegraut.
1. Klicken Sie unter „Input directory“ (Eingabeverzeichnis) auf „Durchsuchen“, und teilen Sie dem Tool den Speicherort des Ordners mit, in dem sich die **DDL**- und **DML**-Skripts befinden, die übersetzt werden müssen.

    > [!Note]
    > Nur Dateien mit der Erweiterung „.sql“ können als Eingabequelle angegeben werden. Wenn der Benutzer DDL- und DML-Skripts in .txt-Dateien bereitstellt, führt das Tool keine Übersetzung durch.

1. Wählen Sie zur Übersetzung von Netezza-Code für Azure Synapse Analytics in der Dropdownliste „Translation Type“ (Übersetzungstyp) die Option „IBM Netezza“ aus.
  ![Eingabe für die Azure Synapse-Bewertung](./media/perform-assessment/assessment-input.png)

1. Klicken Sie zum Auswählen des Ausgabeverzeichnisses auf „Durchsuchen“, um den Ort anzugeben, an dem die Ausgabe generiert werden soll.
 ![Ausgabeverzeichnis für Azure Synapse](./media/perform-assessment/output-directory.png)

1. Klicken Sie auf **Übersetzen**, um die Übersetzung zu starten.

## <a name="view-results"></a>Anzeigen der Ergebnisse

1. Die Dauer der Bewertung hängt von der Anzahl der hinzugefügten Datenbanken und der Größe der Schemas der einzelnen Datenbanken ab. Die Ergebnisse für die einzelnen Datenbanken werden angezeigt, sobald sie verfügbar sind.
 ![Azure Synapse-Bewertungsbericht](./media/perform-assessment/assessment-report.png)

1. Durch Klicken auf „Ergebnisse anzeigen“ gelangen Sie zu dem im vorherigen Schritt angegebenen Ausgabeverzeichnis, in dem die übersetzte(n) Skriptdatei(en) basierend auf Ihrer Eingabeverzeichnisstruktur angezeigt werden.

1. Das Verzeichnis enthält die Projektstruktur, die Sie problemlos in Ihr GitHub-Repository committen können.
  
1. Eine Ergebnisdatei, die eine Liste mit Fehlern und Warnungen enthält, wird ebenfalls in das Ausgabeverzeichnis geladen.

## <a name="next-steps"></a>Nächste Schritte

[Informieren Sie sich darüber, wie Sie die Bewertung speichern und laden.](tutorial-save-load-assessment.md)
