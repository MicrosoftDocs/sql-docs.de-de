---
description: Abonnementsichtformate (Master Data Services)
title: Abonnementsichtformate
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: ff1e2566-ac8f-467d-a6d9-12c3f13879b9
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: b94496efb521fce2099a59d7e0e99b90438de361
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336242"
---
# <a name="subscription-view-formats-master-data-services"></a>Abonnementsichtformate (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Auf Grundlage der ausgewählten Entität oder abgeleiteten Hierarchie sind die folgenden Formate für die Abonnementansicht verfügbar.  
  
## <a name="subscription-view-formats"></a>Abonnementsichtformate  
  
|Name|Beschreibung|  
|----------|-----------------|  
|**Blattelemente**|Enthält Blattelemente und ihre zugeordneten Attributwerte.|  
|**Verlauf von Blattelementen**|Enthält Verlaufsdaten zu Blattelementen und die zugeordneten Attributwerte. Das Anzeigeformat ist Slowly Changing Dimension Typ 4.|  
|**Blattelemente SCD-Typ 2**|Enthält Verlaufsdaten und aktuelle Daten zu Blattelementen sowie die zugeordneten Attributwerte. Das Anzeigeformat ist Slowly Changing Dimension Typ 2.|  
|**Konsolidierte Elemente**|Enthält konsolidierte Elemente und ihre zugeordneten Attributwerte.|  
|**Verlauf von konsolidierten Elementen**|Enthält Verlaufsdaten zu konsolidierten Elementen und die zugeordneten Attributwerte. Das Anzeigeformat ist Slowly Changing Dimension Typ 4.|  
|**Konsolidierte Elemente SCD-Typ 2**|Enthält Verlaufsdaten und aktuelle Daten zu konsolidierten Elementen und die zugeordneten Attributwerte. Das Anzeigeformat ist Slowly Changing Dimension Typ 2.|  
|**Zugehörigkeit zu Sammlungen**|Enthält eine Liste von Auflistungen und ihre zugeordneten Attributwerte.|  
|**Sammlungen**|Enthält eine Liste von Auflistungen und die zugehörigen Elemente sowie Gewichtungswerte und Sortierreihenfolge.|  
|**Verlauf von Sammelelementen**|Enthält Verlaufsdaten zu Sammlungselementen und die zugeordneten Attributwerte. Das Anzeigeformat ist Slowly Changing Dimension Typ 4.|  
|**Sammelelemente SCD-Typ 2**|Enthält Verlaufsdaten und aktuelle Daten zu Sammlungselementen und die zugeordneten Attributwerte. Das Anzeigeformat ist Slowly Changing Dimension Typ 2.|  
|**Explizite Strukturen über- und untergeordneter Elemente**|Enthält Strukturen expliziter Hierarchien für eine Entität im Format über- und untergeordneter Elemente.|  
|**Explizite Ebenen**|Enthält Strukturen expliziter Hierarchien für eine Entität im Ebenenformat.|  
|**Abgeleitete Hierarchie über- und untergeordneter Elemente (Sicht der abgeleiteten Hierarchie)**|Enthält die Struktur einer abgeleiteten Hierarchie im Format über- und untergeordneter Elemente.|  
|**Abgeleitete Ebenen (Sicht der abgeleiteten Hierarchie)**|Enthält die Struktur einer abgeleiteten Hierarchie im Ebenenformat.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Übersicht: Exportieren von Daten &#40;Master Data Services&#41;](../master-data-services/overview-exporting-data-master-data-services.md)   
 [Erstellen einer Abonnementsicht zum Exportieren von Daten &#40;Master Data Services&#41;](../master-data-services/create-a-subscription-view-to-export-data-master-data-services.md)  
  
  
