---
title: Anzeigen von Datenunterschieden
description: In diesem Artikel erfahren Sie, wie Sie zwei Datenbanken vergleichen und sich dann ansehen, inwiefern sich die Datenbankobjekte unterscheiden. Außerdem erfahren Sie, wie Sie Datensätze innerhalb von Objekten anzeigen und wie Sie die Ansicht filtern.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.datacompare.f1
ms.assetid: f88d3350-2eaf-44cc-96a8-84008b6cd071
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 6516a9c39debe29befb106340c90920a70798044
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017970"
---
# <a name="how-to-view-data-differences"></a>Gewusst wie: Anzeigen von Datenunterschieden

Nach dem Vergleich der Daten in zwei Datenbanken werden alle verglichenen *Datenbankobjekte* sowie deren Status aufgeführt. Sie können auch Ergebnisse für die Datensätze in den einzelnen Objekten (gruppiert nach Status) anzeigen.  
  
Nachdem Sie sich die Unterschiede angesehen haben, können Sie einige oder alle unterschiedlichen, fehlenden oder neuen Objekte oder Datensätze des *Ziels* aktualisieren, sodass sie der *Quelle* entsprechen.  
  
### <a name="to-view-data-differences"></a>So zeigen Sie Datenunterschiede an  
  
1.  Vergleichen Sie die Daten einer Quelle und eines Ziels.  
  
2.  (Optional) Führen Sie einen oder beide der folgenden Schritte aus:  
  
    -   Standardmäßig werden die Ergebnisse für alle Objekte (unabhängig vom jeweiligen Status) angezeigt. Wenn nur Objekte mit einem bestimmten Status angezeigt werden sollen, klicken Sie auf eine Option in der Liste **Filter**.  
  
    -   Wenn Sie Ergebnisse für Datensätze innerhalb eines bestimmten Objekts anzeigen möchten, klicken Sie im Hauptergebnisbereich auf das gewünschte Objekt, und klicken Sie anschließend im Bereich mit der Datensatzansicht auf eine Registerkarte. Jede Registerkarte zeigt sämtliche Datensätze innerhalb des Objekts, die einen bestimmten Status besitzen: Unterschiedliche Datensätze, Nur in der Quelle, Nur im Ziel oder Identische Datensätze. Die Daten sind nach Datensatz und Spalte sortiert.  
  
## <a name="see-also"></a>Weitere Informationen  
[Vorgehensweise: Vergleichen verschiedener Datenbankdefinitionen mithilfe des Schemavergleichs](../ssdt/how-to-use-schema-compare-to-compare-different-database-definitions.md)  
  
