---
title: Create-Element (DTA)
description: Im dta-Hilfsprogramm enthält das Create-Element Informationen zu Indizes, Statistiken oder Heapstrukturen in einer benutzerdefinierten Konfiguration.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
dev_langs:
- XML
helpviewer_keywords:
- Create element (DTA)
ms.assetid: 9d076c90-f933-45f4-b6d9-447793f6528b
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 281fd59192deeedbc32d7e543fdb95875929801d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342084"
---
# <a name="create-element-dta"></a>Create-Element (DTA)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Enthält Informationen zu den Indizes, Statistiken oder Heapstrukturen in einer benutzerspezifischen Konfiguration.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
<Recommendation>  
    <Create>  
    ...code removed here...  
    </Create>  
</Recommendation>  
```  
  
## <a name="element-characteristics"></a>Elementmerkmale  
  
|Merkmal|BESCHREIBUNG|  
|--------------------|-----------------|  
|**Datentyp und -länge**|Keine.|  
|**Standardwert**|Keine.|  
|**Vorkommen**|Einmalig erforderlich pro physischem Entwurfsstrukturtyp (Indizes, Statistiken oder Heapstrukturen).|  
  
## <a name="element-relationships"></a>Elementbeziehungen  
  
|Beziehung|Elemente|  
|------------------|--------------|  
|**Übergeordnetes Element**|[Recommendation-Element &#40;DTA&#41;](../../tools/dta/recommendation-element-dta.md)|  
|**Untergeordnete Elemente**|[Index-Element &#40;DTA&#41;](../../tools/dta/index-element-dta.md)<br /><br /> **Statistics**-Element (weitere Informationen finden Sie im Thema zum [XML-Schema für den Datenbankoptimierungsratgeber](https://schemas.microsoft.com/sqlserver/))<br /><br /> **Heap** -Element (weitere Informationen finden Sie im Thema zum [XML-Schema für den Datenbankoptimierungsratgeber](https://schemas.microsoft.com/sqlserver/) )|  
  
## <a name="remarks"></a>Bemerkungen  
 Dieses Element hat den Namen **CreateTypecomplexType** im XML-Schema des Datenbankoptimierungsratgebers. Es wird zum Erstellen von Indizes, Statistiken und Heapstrukturen für eine benutzerspezifische Konfiguration verwendet. Dieses **Create** -Element ist nicht mit den anderen Typen identisch, mit denen Sichten (**CreateViewType**) oder Partitionierungen (**CreatePType**) erstellt werden können. Unter [XML-Schema für den Datenbankoptimierungsratgeber](https://schemas.microsoft.com/sqlserver/) finden Sie Informationen zu diesen anderen **Create** -Elementtypen.  
  
## <a name="example"></a>Beispiel  
 Ein Beispiel für die Verwendung dieses Elements finden Sie unter [Beispiel für eine XML-Eingabedatei mit benutzerdefinierter Konfiguration &#40;DTA&#41;](../../tools/dta/xml-input-file-sample-with-user-specified-configuration-dta.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [XML-Eingabedateireferenz &#40;Datenbankoptimierungsratgeber&#41;](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  
