---
description: Geschäftsregelbedingungen (Master Data Services)
title: Geschäftsregelbedingungen
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: d2e0a8c3-4c2e-407c-856e-68d95ebda9ed
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 4a9d3bbdefd894f82f84f66727873aedadc4e14a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341511"
---
# <a name="business-rule-conditions-master-data-services"></a>Geschäftsregelbedingungen (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  In [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]bestimmen Geschäftsregelbedingungen die Bedingungen, die für eine oder mehrere Aktionen, die ausgeführt werden sollen, den Wert "true" haben müssen.  
  
> [!NOTE]  
>  Bedingungen sind optional. Wenn Sie keine Bedingung angeben, werden immer dann Aktionen ausgeführt, wenn Daten gegen Geschäftsregeln überprüft werden.  
  
## <a name="business-rule-conditions"></a>Geschäftsregelbedingungen  
  
|Bedingungsname|Beschreibung|  
|--------------------|-----------------|  
|**ist gleich**|Das ausgewählte Attribut **ist gleich** einem bestimmten Attribut oder Attributwert bzw. ist leer.<br /><br /> Diese Bedingung ist für Text-, Zahlen-, Datums- und Linkwerte gültig.|  
|**ist nicht gleich**|Das ausgewählte Attribut **ist nicht gleich** einem bestimmten Attribut oder Attributwert bzw. ist leer.<br /><br /> Diese Bedingung ist für Text-, Zahlen-, Datums- und Linkwerte gültig.|  
|**ist größer als**|Das ausgewählte Attribut **ist größer als** ein bestimmtes Attribut oder ein bestimmter Attributwert bzw. ist leer.<br /><br /> Diese Bedingung ist für Text-, Zahlen- und Datumswerte gültig.|  
|**ist größer als oder gleich**|Das ausgewählte Attribut **ist größer als oder gleich** einem bestimmten Attribut oder Attributwert bzw. ist leer.<br /><br /> Diese Bedingung ist für Text-, Zahlen- und Datumswerte gültig.|  
|**ist kleiner als**|Das ausgewählte Attribut **ist kleiner als** ein bestimmtes Attribut oder ein bestimmter Attributwert bzw. ist leer.<br /><br /> Diese Bedingung ist für Text-, Zahlen- und Datumswerte gültig.|  
|**ist kleiner als oder gleich**|Das ausgewählte Attribut **ist kleiner als oder gleich** einem bestimmten Attribut oder Attributwert bzw. ist leer.<br /><br /> Diese Bedingung ist für Text-, Zahlen- und Datumswerte gültig.|  
|**beginnt mit**|Das ausgewählte Attribut **beginnt mit** einem bestimmten Attribut oder Attributwert bzw. ist leer.<br /><br /> Diese Bedingung ist für Text- und Linkwerte gültig.|  
|**beginnt nicht mit**|Das ausgewählte Attribut **beginnt nicht mit** einem bestimmten Attribut oder Attributwert bzw. ist leer.<br /><br /> Diese Bedingung ist für Text- und Linkwerte gültig.|  
|**endet mit**|Das ausgewählte Attribut **endet mit** einem bestimmten Attribut oder Attributwert bzw. ist leer.<br /><br /> Diese Bedingung ist für Text- und Linkwerte gültig.|  
|**endet nicht mit**|Das ausgewählte Attribut **endet nicht mit** einem bestimmten Attribut oder Attributwert bzw. ist leer.<br /><br /> Diese Bedingung ist für Text- und Linkwerte gültig.|  
|**contains**|Das ausgewählte Attribut **enthält** ein bestimmtes Attribut oder einen bestimmten Attributwert bzw. ist leer.<br /><br /> Diese Bedingung ist für Text- und Linkwerte gültig.|  
|**enthält nicht**|Das ausgewählte Attribut **enthält nicht** ein bestimmtes Attribut oder einen Attributwert bzw. ist leer.<br /><br /> Diese Bedingung ist für Text- und Linkwerte gültig.|  
|**Enthält das Muster**|Das ausgewählte Attribut **enthält das Muster** eines bestimmten Attributs oder Attributwerts bzw. ist leer. Verwenden Sie reguläre Ausdrücke von .NET Framework, um das Muster anzugeben.<br /><br /> Weitere Informationen zu regulären Ausdrücken finden Sie unter [Sprachelemente für reguläre Ausdrücke](/dotnet/standard/base-types/regular-expression-language-quick-reference) in der MSDN Library.<br /><br /> Diese Bedingung ist für Text- und Linkwerte gültig.|  
|**enthält nicht das Muster**|Das ausgewählte Attribut **enthält nicht das Muster** eines bestimmten Attributs oder Attributwerts bzw. ist leer. Verwenden Sie reguläre Ausdrücke von .NET Framework, um das Muster anzugeben.<br /><br /> Weitere Informationen zu regulären Ausdrücken finden Sie unter [Sprachelemente für reguläre Ausdrücke](/dotnet/standard/base-types/regular-expression-language-quick-reference) in der MSDN Library.<br /><br /> Diese Bedingung ist für Text- und Linkwerte gültig.|  
|**Enthält die Teilmenge**|Das ausgewählte Attribut **enthält die Teilmenge** eines bestimmten Attributs oder Attributwerts. Sie müssen die Startposition für die Suche angeben (1 bedeutet z. B., dass die Suche beim ersten Zeichen beginnt).<br /><br /> Diese Bedingung ist für Text- und Linkwerte gültig.|  
|**enthält nicht die Teilmenge**|Das ausgewählte Attribut **enthält nicht die Teilmenge** eines bestimmten Attributs oder Attributwerts. Sie müssen die Startposition für die Suche angeben (1 bedeutet z. B., dass die Suche beim ersten Zeichen beginnt).<br /><br /> Diese Bedingung ist für Text- und Linkwerte gültig.|  
|**hat sich geändert**|Das ausgewählte Attribut **wurde geändert** , seit das letzte Mal Geschäftsregeln auf das Element angewendet wurden. Sie müssen die Änderungsgruppe angeben, zu der das Attribut gehört.<br /><br /> Weitere Informationen zu Änderungsnachverfolgungsgruppen finden Sie unter [Hinzufügen von Attributen zu einer Änderungsnachverfolgungsgruppe &#40;Master Data Services&#41;](../master-data-services/add-attributes-to-a-change-tracking-group-master-data-services.md).<br /><br /> Diese Bedingung ist für Text-, Zahlen-, Datums- und Linkwerte gültig.|  
|**wurde nicht geändert**|Das ausgewählte Attribut **wurde nicht geändert** , seit zuletzt Geschäftsregeln auf das Element angewendet wurden. Sie müssen die Änderungsgruppe angeben, zu der das Attribut gehört.<br /><br /> Weitere Informationen zu Änderungsnachverfolgungsgruppen finden Sie unter [Hinzufügen von Attributen zu einer Änderungsnachverfolgungsgruppe &#40;Master Data Services&#41;](../master-data-services/add-attributes-to-a-change-tracking-group-master-data-services.md).<br /><br /> Diese Bedingung ist für Text-, Zahlen-, Datums- und Linkwerte gültig.|  
|**Liegt zwischen**|Das ausgewählte Attribut **liegt zwischen** zwei bestimmten Attributwerten.<br /><br /> Diese Bedingung ist für Text-, Zahlen- und Datumswerte gültig.|  
|**ist nicht zwischen**|Das ausgewählte Attribut **ist nicht zwischen** zwei bestimmten Attributwerten.<br /><br /> Diese Bedingung ist für Text-, Zahlen- und Datumswerte gültig.|  
  
> [!NOTE]  
>  Wenn eine Geschäftsregel eine Bedingung enthält, die zwei Werte vergleicht, und die Regel auf ein Element angewendet wird, wofür beide Werte NULL sind, besteht dieses Element keine Überprüfung.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Geschäftsregel Aktionen &#40;Master Data Services&#41;](../master-data-services/business-rule-actions-master-data-services.md)   
 [Master Data Services von Geschäftsregeln &#40;&#41;](../master-data-services/business-rules-master-data-services.md)   
 [Erstellen und Veröffentlichen einer Geschäftsregel &#40;Master Data Services&#41;](../master-data-services/create-and-publish-a-business-rule-master-data-services.md)  
  
