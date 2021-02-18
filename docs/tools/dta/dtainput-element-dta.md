---
title: DTAInput-Element (DTA)
description: Im dta-Hilfsprogramm enthält das DTAInput-Element die Definition der XML-Eingabe für den Datenbankoptimierungsratgeber.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
dev_langs:
- XML
helpviewer_keywords:
- DTAInput element
ms.assetid: 40c19abf-ded5-43de-be96-5b43b1b81b03
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 275bd84346c334e0c92c73a7750660338c957fdf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338727"
---
# <a name="dtainput-element-dta"></a>DTAInput-Element (DTA)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Enthält die Definition der XML-Eingabe für den Datenbankoptimierungsratgeber.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
<DTAXML>  
    <DTAInput>  
    ...code removed here...  
    </DTAInput>  
```  
  
## <a name="element-characteristics"></a>Elementmerkmale  
  
|Merkmale|BESCHREIBUNG|  
|---------------------|-----------------|  
|**Datentyp und -länge**|Keine.|  
|**Standardwert**|Keine.|  
|**Vorkommen**|Optional einmalig pro **DTAXML** -Element.|  
  
## <a name="element-relationships"></a>Elementbeziehungen  
  
|Beziehung|Elemente|  
|------------------|--------------|  
|**Übergeordnetes Element**|[DTAXML-Element &#40;DTA&#41;](../../tools/dta/dtaxml-element-dta.md)|  
|**Untergeordnete Elemente**|[Server-Element &#40;DTA&#41;](../../tools/dta/server-element-dta.md)<br /><br /> [Workload-Element &#40;DTA&#41;](../../tools/dta/workload-element-dta.md)<br /><br /> [TuningOptions-Element &#40;DTA&#41;](../../tools/dta/tuningoptions-element-dta.md)<br /><br /> [Configuration-Element &#40;DTA&#41;](../../tools/dta/configuration-element-dta.md)|  
  
## <a name="remarks"></a>Bemerkungen  
 Dieses Element ist der Stamm der Eingabeschemahierarchie des Datenbankoptimierungsratgebers. Bei Eingaben in den Datenbankoptimierungsratgeber kann es sich um Argumente handeln, mit denen die Server angegeben werden, deren Datenbanken Sie optimieren möchten, oder auch Arbeitsauslastungen, Optimierungsoptionen bzw. eine benutzerspezifische Konfiguration.  
  
## <a name="example"></a>Beispiel  
 Ein Beispiel für die Verwendung des **DTAInput**-Elements finden Sie unter [Beispiel für eine einfache XML-Eingabedatei &#40;DTA&#41;](../../tools/dta/simple-xml-input-file-sample-dta.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [XML-Eingabedateireferenz &#40;Datenbankoptimierungsratgeber&#41;](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  
