---
description: TopCount (DMX)
title: TopCount (DMX) | Microsoft-Dokumentation
ms.date: 06/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: dmx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: 23a9a1594c31bda040a22ad3914af8b8e7d3603f
ms.sourcegitcommit: c7f40918dc3ecdb0ed2ef5c237a3996cb4cd268d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "91726081"
---
# <a name="topcount-dmx"></a>TopCount (DMX)
[!INCLUDE[ssas](../includes/applies-to-version/ssas.md)]

  Gibt die durch einen Ausdruck angegebene Anzahl von obersten Zeilen in absteigender Rangreihenfolge zurück.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
TopCount(<table expression>, <rank expression>, <count>)  
```  
  
## <a name="applies-to"></a>Gilt für  
 Ein Ausdruck, der eine Tabelle zurückgibt, wie z \<table column reference> . b., oder eine Funktion, die eine Tabelle zurückgibt.  
  
## <a name="return-type"></a>Rückgabetyp  
 \<table expression>  
  
## <a name="remarks"></a>Bemerkungen  
 Der Wert, der vom-Argument bereitgestellt wird \<rank expression> , bestimmt die absteigende Rangfolge für die Zeilen, die im- \<table expression> Argument angegeben sind, und die Anzahl der obersten Zeilen, die im-Argument angegeben werden, \<count> wird zurückgegeben.  
  
 Die TopCount-Funktion wurde ursprünglich eingeführt, um assoziative Vorhersagen zu ermöglichen, und im Allgemeinen werden dieselben Ergebnisse wie eine Anweisung erzeugt, die **Select Top** -und **Order by** -Klauseln enthält. Sie erhalten eine bessere Leistung für assoziative Vorhersagen, wenn Sie die **Vorhersagefunktion (DMX)** verwenden, die die Angabe einer Reihe von Vorhersagen unterstützt.  
  
 Es gibt jedoch Situationen, in denen Sie möglicherweise weiterhin TopCount verwenden müssen. DMX unterstützt z. b. den **Top** -Qualifizierer in einer untergeordneten SELECT-Anweisung nicht. Die Funktion " [präthistogram &#40;DMX-&#41;](../dmx/predicthistogram-dmx.md) " unterstützt auch das Hinzufügen von **Top**nicht.  
  
## <a name="examples"></a>Beispiele  
 Die folgenden Beispiele sind Vorhersage Abfragen für das Association-Modell, das Sie mit dem Lernprogramm zu [Data Mining-Grundlagen](/previous-versions/sql/sql-server-2016/ms167167(v=sql.130))erstellen. Die Abfragen geben die gleichen Ergebnisse zurück, im ersten Beispiel wird jedoch TopCount verwendet, und im zweiten Beispiel wird die Vorhersagefunktion verwendet.  
  
 Um zu verstehen, wie TopCount funktioniert, ist es möglicherweise hilfreich, zunächst eine Vorhersage Abfrage auszuführen, die nur die geclusterte Tabelle zurückgibt.  
  
```  
SELECT Predict ([Association].[v Assoc Seq Line Items], INCLUDE_STATISTICS, 10)  
FROM   
     [Association]  
NATURAL PREDICTION JOIN  
SELECT (SELECT 'Women''s Mountain Shorts' as [Model]) AS [v Assoc Seq Line Items]) AS t  
```  
  
> [!NOTE]  
>  In diesem Beispiel enthält der als Eingabe bereitgestellte Wert ein einzelnes Anführungszeichen und muss daher mit Escapezeichen versehen werden, indem ihm ein weiteres einzelnes Anführungszeichen vorangestellt wird. Wenn Sie über die Syntax zum Einfügen von Escapezeichen nicht sicher sind, können Sie den Generator für Vorhersageabfragen verwenden, um die Abfrage zu erstellen. Wenn Sie den Wert aus der Dropdownliste auswählen, wird das erforderliche Escapezeichen automatisch eingefügt. Weitere Informationen finden Sie unter [Erstellen einer SINGLETON-Abfrage im Data Mining-Designer](/analysis-services/data-mining/create-a-singleton-query-in-the-data-mining-designer).  
  
 Beispielergebnisse:  
  
|Modell|$SUPPORT|$PROBABILITY|$ADJUSTEDPROBABILITY|  
|-----------|--------------|------------------|--------------------------|  
|Sport-100|4334|0,291283016|0,252695851|  
|Water Bottle|2866|0,192620472|0,175205052|  
|Patchkit|2113|0,142012232|0,132389356|  
|Mountain Tire Tube|1992|0,133879965|0,125304948|  
|Mountain-200|1755|0,117951475|0,111260823|  
|Road Tire Tube|1588|0,106727603|0,101229538|  
|Cycling Cap|1473|0,098998589|0,094256014|  
|Fender Set – Mountain|1415|0,095100477|0,090718432|  
|Mountain Bottle Cage|1367|0,091874454|0,087780332|  
|Road Bottle Cage|1195|0,080314537|0,077173962|  
  
 Die TopCount-Funktion nimmt die Ergebnisse dieser Abfrage und gibt die angegebene Anzahl von Zeilen mit dem kleinsten Wert zurück.  
  
```  
SELECT   
TopCount  
    (  
    Predict ([Association].[v Assoc Seq Line Items],INCLUDE_STATISTICS,10),  
    $SUPPORT,  
    3)  
FROM   
     [Association]  
NATURAL PREDICTION JOIN  
(SELECT (SELECT 'Women''s Mountain Shorts' as [Model]) AS [v Assoc Seq Line Items]) AS t  
```  
  
 Das erste Argument für die TopCount-Funktion ist der Name einer Tabellenspalte. In diesem Beispiel wird die-Tabelle zurückgegeben, indem die Vorhersagefunktion aufgerufen und das INCLUDE_STATISTICS-Argument verwendet wird.  
  
 Das zweite Argument für die TopCount-Funktion ist die Spalte in der Tabelle, die Sie zum Sortieren der Ergebnisse verwenden. In diesem Beispiel gibt die INCLUDE_STATISTICS-Option die Spalten $SUPPORT, $PROBABILTY und $ADJUSTED PROBABILITY zurück. In diesem Beispiel werden die Ergebnisse mithilfe von $SUPPORT sortiert.  
  
 Das dritte Argument für die TopCount-Funktion gibt die Anzahl von Zeilen an, die als ganze Zahl zurückgegeben werden. Für die obersten drei Produkte, sortiert nach $ SUPPORT, geben Sie 3 ein.  
  
 Beispielergebnisse:  
  
|Modell|$SUPPORT|$PROBABILITY|$ADJUSTEDPROBABILITY|  
|-----------|--------------|------------------|--------------------------|  
|Sport-100|4334|0,29...|0,25...|  
|Water Bottle|2866|0,19...|0,17...|  
|Patchkit|2113|0,14...|0,13...|  
  
 Dieser Abfragetyp kann jedoch die Leistung in einer Produktionseinstellung beeinträchtigen. Das liegt daran, dass die Abfrage einen Satz aller Vorhersagen für den Algorithmus zurückgibt, diese Vorhersagen sortiert und die obersten 3 Vorhersagen zurückgibt.  
  
 Das folgende Beispiel enthält eine alternative Anweisung, die die gleichen Ergebnisse zurückgibt, aber bedeutend schneller ausgeführt wird. In diesem Beispiel wird TopCount durch die Vorhersagefunktion ersetzt, die eine Reihe von Vorhersagen als Argument akzeptiert. In diesem Beispiel wird auch das **$Support** -Schlüsselwort verwendet, um die Spalte der Spalte mit den Spalten direkt abzurufen.  
  
```  
SELECT Predict ([Association].[v Assoc Seq Line Items], INCLUDE_STATISTICS, 3, $SUPPORT)  
```  
  
 Die Ergebnisse enthalten die obersten 3 Vorhersagen sortiert nach dem Unterstützungswert. Sie können $SUPPORT durch $PROBABILITY oder $ADJUSTED_PROBABILITY ersetzen, um nach Wahrscheinlichkeit oder angepasster Wahrscheinlichkeit sortierte Vorhersagen zurückzugeben. Weitere Informationen finden Sie unter **Vorhersagen (DMX)**.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Funktionen &#40;DMX-&#41;](../dmx/functions-dmx.md)   
 [Allgemeine Vorhersagefunktionen &#40;DMX-&#41;](../dmx/general-prediction-functions-dmx.md)   
 [BottomCount &#40;DMX-&#41;](../dmx/bottomcount-dmx.md)   
 [Topprozent &#40;DMX-&#41;](../dmx/toppercent-dmx.md)   
 [TopSum &#40;DMX-&#41;](../dmx/topsum-dmx.md)  
  
