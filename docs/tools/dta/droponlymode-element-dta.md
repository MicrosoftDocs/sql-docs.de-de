---
title: DropOnlyMode-Element (DTA)
description: Im dta-Hilfsprogramm gibt das DropOnlyMode-Element an, dass der Datenbankoptimierungsratgeber nur das Löschen von vorhandenen Indizes, indizierten Sichten und Partitionen in Betracht ziehen soll.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
dev_langs:
- XML
helpviewer_keywords:
- DropOnlyMode element
ms.assetid: 80960676-7581-4074-889b-80ee665963dd
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 27d38719f0ee9572a86a7b196655489b33824071
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351102"
---
# <a name="droponlymode-element-dta"></a>DropOnlyMode-Element (DTA)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Gibt an, dass der Datenbankoptimierungsratgeber während der Optimierungssitzung nur vorhandene Indizes, indizierte Sichten oder Partitionen löschen soll. Wenn diese Optimierungsoption angegeben ist, werden keine neuen physischen Entwurfsstrukturen berücksichtigt.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
<DTAInput>  
...code removed...  
    <TuningOptions>  
      <DropOnlyMode>...</DropOnlyMode>  
```  
  
## <a name="element-characteristics"></a>Elementmerkmale  
 **Datentyp und -länge**  
  
 **Standardwert**  
  
 **Häufigkeit**: Optional. Kann für jedes **TuningOptions** -Element nur einmal verwendet werden. Keine Verwendung möglich, wenn im **TuningOptions** -Element die folgenden Elemente angegeben sind:  
  
-   [FeatureSet-Element &#40;DTA&#41;](../../tools/dta/featureset-element-dta.md)  
  
-   [Partitioning-Element &#40;DTA&#41;](../../tools/dta/partitioning-element-dta.md)  
  
-   [KeepExisting-Element &#40;DTA&#41;](../../tools/dta/keepexisting-element-dta.md) ist auf **ALL** festgelegt  
  
## <a name="element-relationships"></a>Elementbeziehungen  
 **Übergeordnetes Element:** [TuningOptions-Element &#40;DTA&#41;](../../tools/dta/tuningoptions-element-dta.md)  
  
 **Untergeordnete Elemente**  
  
## <a name="example"></a>Beispiel  
 Das folgende Beispiel veranschaulicht den **TuningOptions** -Abschnitt einer XML-Eingabedatei des Datenbankoptimierungsratgebers, wobei **DropOnlyMode** angegeben ist. In diesem Beispiel ist die Optimierungszeit auf 24 Stunden (1440 Minuten) begrenzt, und alle vorhandenen gruppierten und nicht gruppierten Indizes sollen gelöscht werden:  
  
```xml  
<TuningOptions  
  <TuningTimeInMin>1440</Name>  
  <KeepExisting>ALIGNED</KeepExisting>  
  <DropOnlyMode />  
</TuningOptions>  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [XML-Eingabedateireferenz &#40;Datenbankoptimierungsratgeber&#41;](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  
