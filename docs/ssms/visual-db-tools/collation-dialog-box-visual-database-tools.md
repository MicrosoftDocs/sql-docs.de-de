---
description: Sortierreihenfolge (Dialogfeld) (Visual Database Tools)
title: Das Dialogfeld „Sortierung“
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- vdt.dlgbox.definecolumncollation
- vdtsql.chm:65561
ms.assetid: e4020f79-7abf-4839-b9b2-984ef7049817
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: c1e4c82b90c9b3c569f9f179ed16242c4ca76f40
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88314656"
---
# <a name="collation-dialog-box-visual-database-tools"></a>Sortierreihenfolge (Dialogfeld) (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Mit diesem Dialogfeld können Sie eine Sortierreihenfolge für die Spalte angeben. Die Sortierreihenfolge einer Spalte wird bei jedem Vorgang verwendet, bei dem die Werte der Spalte mit einer anderen Spalte oder mit konstanten Werten verglichen werden. Gleichzeitig wird dadurch das Verhalten einiger Zeichenfolgenfunktionen beeinflusst, z. B. SUBSTRING und CHARINDEX. Eine vollständige Liste der Auswirkungen der Einstellung der Sortierreihenfolge einer Spalte finden Sie in der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Dokumentation.  
  
Das Dialogfeld wird in den folgenden Fällen angezeigt:  
  
-   Wenn Sie auf der Registerkarte **Spalteneigenschaften** im Feld **Sortierung** einen ungültigen Sortierungsnamen eingeben.  
  
-   Wenn Sie in der Registerkarte **Spalteneigenschaften** in das Feld **Sortierreihenfolge** klicken, und dann auf die Schaltfläche mit den Auslassungspunkten ( **...** ) rechts neben dem Feld klicken.  
  
## <a name="options"></a>Optionen  
**SQL-Sortierung**  
Wählen Sie aus der Dropdownliste eine in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] definierte Sortierreihenfolge.  
  
**Windows-Sortierreihenfolge**  
Wählen Sie aus den Sortierreihenfolgen, die in Windows in der Dropdownliste definiert sind.  
  
**Binäre Sortierung**  
Verwenden Sie die Binärcodes der Zeichenwerte für Vergleiche. Wenn Sie diese Option auswählen, sind bestimmte Optionen der alphabetischen Vergleiche nicht verfügbar. Beispielsweise ist das Vergleichen ohne Unterscheidung von Groß- und Kleinschreibung nicht möglich, da Groß- und Kleinbuchstaben eine unterschiedliche binäre Codierung aufweisen. Diese Option wird nur verwendet, wenn Sie **Windows-Sortierreihenfolge**ausgewählt haben.  
  
**Lexikalische Sortierung**  
Verwenden Sie alphabetische Vergleichsoptionen. Diese Option wird nur verwendet, wenn Sie **Windows-Sortierreihenfolge**ausgewählt haben. Zu den alphabetischen Vergleichsoptionen gehören folgende:  
  
-   **Groß-/Kleinschreibung beachten** Aktivieren Sie diese Option, wenn beim Vergleichen die Groß- und Kleinschreibung berücksichtigt werden soll.  
  
-   **Akzente berücksichtigen** Aktivieren Sie diese Option, wenn beim Vergleichen zwischen Buchstaben mit Akzent und Buchstaben ohne Akzent unterschieden werden soll. Wenn Sie diese Option aktivieren, werden beim Vergleichen außerdem gleiche Buchstaben mit unterschiedlichen Akzenten unterschieden.  
  
-   **Kana berücksichtigen** Aktivieren Sie diese Option, wenn beim Vergleichen Unterschiede zwischen Katakana- und Hiragana-Japanisch berücksichtigt werden sollen.  
  
-   **Breite berücksichtigen** Aktivieren Sie diese Option, wenn beim Vergleichen der Unterschied zwischen Zeichen halber Breite und Zeichen normaler Breite berücksichtigt werden soll.  
  
**Schaltfläche Standard wiederherstellen**  
Wenden Sie die Standardsortierreihenfolge für die Datenbank auf die Spalte an.  
  
## <a name="see-also"></a>Weitere Informationen  
[Verwenden von Spalten in Aggregatabfragen &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/work-with-columns-in-aggregate-queries-visual-database-tools.md)  
  
