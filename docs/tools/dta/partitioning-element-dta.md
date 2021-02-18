---
title: Partitioning-Element (DTA)
description: Im Hilfsprogramm „DTA“ enthält das Element „Partitioning“ das Partitionierungsschema, das der Datenbankoptimierungsratgeber bei der Analyse verwenden soll.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
dev_langs:
- XML
helpviewer_keywords:
- Partitioning element
ms.assetid: 9bc5d1d5-27a7-4434-966f-c3935794af27
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 7dc261ca057aeebe07cf8dbd8cb4a735f05ffbce
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100335958"
---
# <a name="partitioning-element-dta"></a>Partitioning-Element (DTA)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Enthält das Partitionierungsschema, das der Datenbankoptimierungsratgeber bei der Analyse verwenden soll.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
<DTAInput>  
...code removed...  
    <TuningOptions>  
      <Partitioning>...</Partitioning>  
```  
  
## <a name="element-characteristics"></a>Elementmerkmale  
  
|Merkmal|BESCHREIBUNG|  
|--------------------|-----------------|  
|**Datentyp und -länge**|**string**, keine maximale Länge.|  
|**Zulässige Werte**|**NONE**<br /> Keine Partitionierung.<br /><br /> **FULL**<br /> Vollständige Partitionierung. (Erhöht die Leistung.)<br /><br /> **ALIGNED**<br /> Nur ausgerichtete Partitionierung. (Erleichtert die Verwaltbarkeit.)<br /><br /> Verwenden Sie nur einen dieser Werte mit diesem Element.<br /><br /> **ALIGNED** bedeutet, dass in der vom Datenbankoptimierungsratgeber generierten Empfehlung jeder vorgeschlagene Index auf exakt dieselbe Weise partitioniert wird wie die zugrunde liegende Tabelle, für die der Index definiert ist. Nicht gruppierte Indizes für eine indizierte Sicht werden mit der indizierten Sicht ausgerichtet.|  
|**Standardwert**|**NONE**|  
|**Vorkommen**|Einmalig erforderlich für das **TuningOptions** -Element, es sei denn, das **DropOnlyMode** -Element wird verwendet. Wird das **DropOnlyMode** -Element verwendet, kann das **Partitioning**-Element nicht verwendet werden. Diese Elemente schließen sich gegenseitig aus.|  
  
## <a name="element-relationships"></a>Elementbeziehungen  
  
|Beziehung|Elemente|  
|------------------|--------------|  
|**Übergeordnetes Element**|[TuningOptions-Element &#40;DTA&#41;](../../tools/dta/tuningoptions-element-dta.md)|  
|**Untergeordnete Elemente**|Keine.|  
  
## <a name="example"></a>Beispiel  
 Ein Beispiel für die Verwendung dieses Elements finden Sie unter [Beispiel für eine einfache XML-Eingabedatei &#40;DTA&#41;](../../tools/dta/simple-xml-input-file-sample-dta.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [XML-Eingabedateireferenz &#40;Datenbankoptimierungsratgeber&#41;](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  
