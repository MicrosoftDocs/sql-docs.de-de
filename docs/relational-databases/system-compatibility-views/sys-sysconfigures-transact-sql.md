---
description: sys.sysconfigures (Transact-SQL)
title: sys.syskonfiguriert (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.sysconfigures
- sysconfigures
- sys.sysconfigures_TSQL
- sysconfigures_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sysconfigures compatibility view
- sysconfigures system table
ms.assetid: 146bf10a-c898-4676-a2a1-673fb1cee7a2
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 109c17040f52392df2518ddccb47491d2b0a9527
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180705"
---
# <a name="syssysconfigures-transact-sql"></a>sys.sysconfigures (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Enthält eine Zeile für jede von einem Benutzer festgelegte Konfigurationsoption. **sysconfigures** enthält die vor dem letzten Start von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]definierten Konfigurationsoptionen sowie alle seitdem festgelegten dynamischen Konfigurationsoptionen.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**value**|**int**|Vom Benutzer änderbarer Wert für die Variable. Wird nur von [!INCLUDE[ssDE](../../includes/ssde-md.md)] verwendet, wenn RECONFIGURE ausgeführt wurde.|  
|**config**|**int**|Nummer der Konfigurationsvariablen.|  
|**comment**|**nvarchar(255)**|Erläuterung der Konfigurationsoption.|  
|**status**|**smallint**|Bitmuster, das den Status der Option anzeigt. Folgende Werte sind möglich:<br /><br /> 0 = Statisch Die Einstellung wird beim Neustart des Servers wirksam.<br /><br /> 1 = Dynamisch. Variable wird beim Ausführen der RECONFIGURE-Anweisung wirksam.<br /><br /> 2 = Erweitert. Variable wird nur angezeigt, wenn **Erweiterte Optionen anzeigen** festgelegt ist. Die Einstellung wird beim Neustart des Servers wirksam.<br /><br /> 3 = Dynamisch und erweitert.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Zuordnung von Systemtabellen zu System Sichten &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Kompatibilitätssichten &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
