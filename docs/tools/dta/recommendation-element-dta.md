---
title: Recommendation-Element (DTA)
description: Das Recommendation-Element im dta-Hilfsprogramm enthält Informationen zu den hypothetischen Indizes, die Teil einer benutzerdefinierten Konfiguration sind.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
dev_langs:
- XML
helpviewer_keywords:
- Recommendation element
ms.assetid: 679ea535-865a-4633-a4d3-5b3090515158
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 96684c5962d6417f87999411131c924045933dd0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345871"
---
# <a name="recommendation-element-dta"></a>Recommendation-Element (DTA)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Enthält Informationen zu den hypothetischen Indizes, die Teil einer benutzerspezifischen Konfiguration sind.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
<Configuration>  
    ...code removed here...  
    <Table>  
      <Name>...</Name>  
      <Recommendation>  
      ...code removed here...  
      </Recommendation>  
```  
  
## <a name="element-characteristics"></a>Elementmerkmale  
  
|Merkmal|BESCHREIBUNG|  
|--------------------|-----------------|  
|**Datentyp und -länge**|Keine.|  
|**Standardwert**|Keine.|  
|**Vorkommen**|Optional. Einmalige Verwendung pro **Table** -Element möglich.|  
  
## <a name="element-relationships"></a>Elementbeziehungen  
  
|Beziehung|Elemente|  
|------------------|--------------|  
|**Übergeordnetes Element**|[Table-Element für Schema &#40;DTA&#41;](../../tools/dta/table-element-for-schema-dta.md)|  
|**Untergeordnete Elemente**|[Create Element &#40;DTA&#41;](../../tools/dta/create-element-dta.md)<br /><br /> **Drop** -Element. Weitere Informationen finden Sie im [XML-Schema des Datenbankoptimierungsratgebers](https://go.microsoft.com/fwlink/?linkid=43100).|  
  
## <a name="remarks"></a>Bemerkungen  
 Dieses Element hat den Namen **CreateTypecomplexType** im XML-Schema des Datenbankoptimierungsratgebers. Es wird zur Angabe von Indizes für eine hypothetische Konfiguration verwendet. Dieses **Recommendation** -Element ist nicht mit den anderen Typen identisch, die zum Angeben von Partitionierungen (**RecommendationPType**) oder Sichten (**RecommendationViewType**) verwendet werden können. Unter **XML-Schema des Datenbankoptimierungsratgebers** finden Sie Informationen zu diesen anderen [Recommendation](https://go.microsoft.com/fwlink/?linkid=43100)-Elementtypen.  
  
## <a name="example"></a>Beispiel  
 Ein Beispiel für die Verwendung dieses Elements finden Sie unter [Beispiel für eine XML-Eingabedatei mit benutzerdefinierter Konfiguration &#40;DTA&#41;](../../tools/dta/xml-input-file-sample-with-user-specified-configuration-dta.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [XML-Eingabedateireferenz &#40;Datenbankoptimierungsratgeber&#41;](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  
