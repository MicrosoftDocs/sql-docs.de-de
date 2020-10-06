---
description: Exists (DMX)
title: Vorhanden (DMX) | Microsoft-Dokumentation
ms.date: 06/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: dmx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: 1f45a4a1d0e709c6b8eb9bb7217d268420f31d07
ms.sourcegitcommit: c7f40918dc3ecdb0ed2ef5c237a3996cb4cd268d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "91726231"
---
# <a name="exists-dmx"></a>Exists (DMX)
[!INCLUDE[ssas](../includes/applies-to-version/ssas.md)]

  Gibt **true** zurück, wenn die angegebene Unterabfrage mindestens eine Zeile zurückgibt.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
EXISTS(<subquery>)  
```  
  
## <a name="arguments"></a>Argumente  
 *subquery*  
 Eine SELECT-Anweisung des Formulars SELECT * FROM \<column name> [WHERE \<predicate list> ].  
  
## <a name="result-type"></a>Ergebnistyp  
 Gibt " **true** " zurück, wenn das von der Unterabfrage zurückgegebene Resultset mindestens eine Zeile enthält. Andernfalls wird **false**zurückgegeben.  
  
## <a name="remarks"></a>Bemerkungen  
 Sie können das NOT-Schlüsselwort vor EXISTS verwenden, z. B. `WHERE NOT EXISTS (<subquery>)`.  
  
 Die Spaltenliste, die zum Unterabfrageargument EXISTS hinzugefügt wird, ist nicht relevant. Die Funktion überprüft lediglich, ob eine Zeile existiert, auf die die Bedingung zutrifft.  
  
## <a name="examples"></a>Beispiele  
 Sie können mit EXISTS und NOT EXISTS überprüfen, ob Bedingungen in einer geschachtelten Tabelle zutreffen. Dies ist hilfreich beim Erstellen eines Filters zum Überprüfen der Daten, die zum Trainieren oder Testen eines Data Mining-Modells verwendet werden. Weitere Informationen finden Sie unter [Filter für Miningmodelle &#40;Analysis Services – Data Mining&#41;](/analysis-services/data-mining/filters-for-mining-models-analysis-services-data-mining).  
  
 Das folgende Beispiel basiert auf der `[Association]` Mining Struktur und dem Mining Modell, die Sie im Lernprogramm zu [Data Mining-Grundlagen](/previous-versions/sql/sql-server-2016/ms167167(v=sql.130))erstellt haben. Die Abfrage gibt nur die Fälle zurück, in denen der Kunde mindestens ein Patchkit gekauft hat.  
  
```  
SELECT * FROM [Association].CASES  
WHERE EXISTS  
(  
SELECT * FROM [v Assoc Seq Line Numbers]  
WHERE [[Model] = 'Patch kit'  
)  
```  
  
 Eine weitere Möglichkeit zum Anzeigen der Daten, die von dieser Abfrage zurückgegeben werden, besteht darin, das Modell im Zuordnungs-Viewer zu öffnen, mit der rechten Maustaste auf das Itemset **Patch Kit = vorhandene**zu klicken, die Option **Drillthrough** auszuwählen und dann **nur Modell Fälle**auszuwählen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Funktionen &#40;DMX-&#41;](../dmx/functions-dmx.md)   
 [Modellfiltersyntax und Beispiele &#40;Analysis Services – Data Mining&#41;](/analysis-services/data-mining/model-filter-syntax-and-examples-analysis-services-data-mining)  
  
