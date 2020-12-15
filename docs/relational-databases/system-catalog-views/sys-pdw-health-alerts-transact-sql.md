---
description: sys.pdw_health_alerts (Transact-SQL)
title: sys.pdw_health_alerts (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
ms.assetid: 49c01e5f-ee47-41a0-871d-35a759f50851
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016'
ms.openlocfilehash: fcdb396016c58f82f3e67f08af2b5489adb40731
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97472941"
---
# <a name="syspdw_health_alerts-transact-sql"></a>sys.pdw_health_alerts (Transact-SQL)
[!INCLUDE [pdw](../../includes/applies-to-version/pdw.md)]

  Speichert die Eigenschaften für die verschiedenen Warnungen, die auf dem System auftreten können. Dies ist eine Katalog Tabelle für Warnungen.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|Range|  
|-----------------|---------------|-----------------|-----------|  
|alert_id|**int**|Eindeutiger Bezeichner der Warnung.<br /><br /> Der Schlüssel für diese Ansicht.|NOT NULL|  
|component_id|**int**|Die ID der Komponente, für die diese Warnung gilt. Die Komponente ist ein allgemeiner Komponenten Bezeichner, z. b. "Netzteil", und ist nicht spezifisch für eine Installation. Weitere Informationen finden Sie unter [sys.pdw_health_components &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-pdw-health-components-transact-sql.md).|NOT NULL|  
|alert_name|**nvarchar(255)**|Name der Warnung.|NOT NULL|  
|state|**nvarchar(32)**|Status der Warnung|NOT NULL<br /><br /> Mögliche Werte:<br /><br /> Betriebs<br /><br /> "Nicht funktionstüchtig"<br /><br /> Zerstört<br /><br /> Erreicht|  
|severity|**nvarchar(32)**|Schweregrad der Warnung|NOT NULL<br /><br /> Mögliche Werte:<br /><br /> Ellen<br /><br /> Davor<br /><br /> Zeit|  
|Typ|**nvarchar(32)**|Warnungstyp.|NOT NULL<br /><br /> Mögliche Werte:<br /><br /> Status schange-der Gerätestatus hat sich geändert.<br /><br /> Schwellenwert: ein Wert hat den Schwellenwert überschritten.|  
|description|**nvarchar(4000)**|Beschreibung der Warnung.|NOT NULL|  
|condition|**nvarchar(255)**|Wird verwendet, wenn Type = Threshold ist. Definiert, wie der Warnungs Schwellenwert berechnet wird.|NULL|  
|status|**nvarchar(32)**|Warnungsstatus|NULL|  
|condition_value|**bit**|Gibt an, ob die Warnung während des System Vorgangs auftreten darf.|NULL<br /><br /> Mögliche Werte<br /><br /> 0-Warnung wird nicht generiert.<br /><br /> 1-Warnung wird generiert.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Katalogsichten von Azure Synapse Analytics und Parallel Data Warehouse Catalog](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)  
  
  
