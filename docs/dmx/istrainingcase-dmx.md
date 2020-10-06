---
description: IsTrainingCase (DMX)
title: IsTrainingCase (DMX) | Microsoft-Dokumentation
ms.date: 06/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: dmx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: 5eb68d0aaa0d19fb903154b8d5c4d4135b57883e
ms.sourcegitcommit: c7f40918dc3ecdb0ed2ef5c237a3996cb4cd268d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "91726146"
---
# <a name="istrainingcase-dmx"></a>IsTrainingCase (DMX)
[!INCLUDE[ssas](../includes/applies-to-version/ssas.md)]

  Gibt an, ob ein Fall als Trainingsfall für ein bestimmtes Data Mining-Modell oder eine bestimmte Data Mining-Struktur verwendet wird.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
IsTrainingCase()  
```  
  
## <a name="result-type"></a>Ergebnistyp  
 Gibt " **true** " zurück, wenn der Fall ein Teil des Trainings Datasets ist. andernfalls **false**.  
  
## <a name="remarks"></a>Bemerkungen  
 Wenn Sie eine Miningstruktur und ein damit verknüpftes Miningmodell mit dem Data Mining-Assistenten erstellen, werden standardmäßig 30 Prozent der Fälle zur Verwendung als Testdataset zurückgehalten. Die verbleibenden Fälle in der angegebenen Datenquelle werden zum Trainieren des Modells verwendet. Wenn Sie das Miningmodell jedoch mithilfe von DMX-Anweisungen (Data Mining-Erweiterungen) erstellen, werden standardmäßig alle Daten zum Trainieren des Modells verwendet, und es wird kein Testsatz erstellt. Damit ein Testdataset erstellt werden kann, müssen Sie die Parameter der WITH HOLDOUT-Klausel festlegen.  
  
 Sie können ermitteln, ob die Daten in einer bestimmten Miningstruktur in Test- und Trainingssätze geteilt wurden, indem Sie den Wert der Eigenschaften von <xref:Microsoft.AnalysisServices.MiningStructure.HoldoutMaxCases%2A> und <xref:Microsoft.AnalysisServices.MiningStructure.HoldoutMaxPercent%2A> anzeigen.  
  
> [!NOTE]  
>  Drillthrough muss für das Modell aktiviert sein, wenn Sie die IsTrainingCase-Funktion oder die IsTestCase-Funktion verwenden möchten, um Details zu den Fällen im Modell zurückzugeben. Weitere Informationen finden Sie unter [Aktivieren von Drillthrough für ein Miningmodell](/analysis-services/data-mining/enable-drillthrough-for-a-mining-model).  
  
 Um Fälle zurückzugeben, die Teil des Test Datasets sind, verwenden Sie die Funktion [IsTestCase &#40;DMX-&#41;](../dmx/istestcase-dmx.md).  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird das Clustering-Data Mining Modell aus dem Ziel Mailing-Szenario im Lernprogramm zu [Data Mining-Grundlagen](/previous-versions/sql/sql-server-2016/ms167167(v=sql.130))verwendet. Die Abfrage gibt nur die Fälle zurück, die zum Trainieren des Miningmodells verwendet wurden. Darüber hinaus werden die Trainingsfälle auf Kunden unter 40 eingeschränkt.  
  
```  
SELECT *  
FROM [TM Clustering].CASES  
WHERE IsTrainingCase()  
AND [Age] <40  
```  
  
 Weitere Beispiele für die Abfrage der in Data Mining verwendeten Fälle finden Sie unter [Select from &#60;Model&#62;. Fälle &#40;DMX-&#41;](../dmx/select-from-model-cases-dmx.md) und [Wählen aus &#60;Struktur&#62;. Fälle](../dmx/select-from-structure-cases.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Trainings-und Test Datasets](/analysis-services/data-mining/training-and-testing-data-sets)   
 [Funktionen &#40;DMX-&#41;](../dmx/functions-dmx.md)   
 [Data Mining-Abfragen](/analysis-services/data-mining/data-mining-queries)  
  
