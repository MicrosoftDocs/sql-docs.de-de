---
description: Banner-Element (ssbdiagnose)
title: Banner-Element
diagnose: In SQL Server, the Banner element identifies which utility generated the ssbdiagnose output XML file.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- banner element
- XML output file format [ssbdiagnose], banner element
- ssbdiagnose
ms.assetid: cc6cd49a-acf0-4cfb-8c6a-554692b89de2
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: dae521477b4255c6e4f697631960d4647e736f01
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88484943"
---
# <a name="banner-element-ssbdiagnose"></a>Banner-Element (ssbdiagnose)


 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Identifiziert das Hilfsprogramm, das die XML-Ausgabedatei von **ssbdiagnose** generiert hat.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
<Banner  
    title="..."   
    product="..."   
    version="..." />  
```  
  
## <a name="element-attributes"></a>Elementattribute  
  
|attribute|BESCHREIBUNG|  
|---------------|-----------------|  
|**title**|Identifiziert das Hilfsprogramm, das die XML-Ausgabedatei von **ssbdiagnose** generiert hat.|  
|**product**|Identifiziert das Produkt, das die XML-Ausgabedatei von **ssbdiagnose** generiert hat.|  
|**version**|Identifiziert die Version des Hilfsprogramms, das die XML-Ausgabedatei generiert hat.|  
  
## <a name="element-characteristics"></a>Elementmerkmale  
  
|Merkmal|BESCHREIBUNG|  
|--------------------|-----------------|  
|**Datentyp und -länge**|Keine.|  
|**Standardwert**|Keine.|  
|**Vorkommen**|Einmal pro XML-Ausgabedatei von **ssbdiagnose**|  
  
## <a name="element-relationships"></a>Elementbeziehungen  
  
|Beziehung|Elemente|  
|------------------|--------------|  
|**Übergeordnetes Element**|[DiagnosticInformation-Element &#40;ssbdiagnose&#41;](../../tools/ssbdiagnose/diagnosticinformation-element-ssbdiagnose.md)|  
|**Untergeordnete Elemente**|Keine.|  
  
## <a name="example"></a>Beispiel  
 Das folgende Beispiel zeigt ein Banner-Element.  
  
```  
<Banner title="Service Broker Diagnostics Utility" product="Microsoft SQL Server" version="10.0.1073.0" />  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [ssbdiagnose-Hilfsprogramm &#40;Service Broker&#41;](../../tools/ssbdiagnose/ssbdiagnose-utility-service-broker.md)  
  
  
