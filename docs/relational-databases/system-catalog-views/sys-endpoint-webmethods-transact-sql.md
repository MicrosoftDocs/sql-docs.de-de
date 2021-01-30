---
description: sys.endpoint_webmethods (Transact-SQL)
title: sys.endpoint_webmethods (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.endpoint_webmethods_TSQL
- sys.endpoint_webmethods
- endpoint_webmethods_TSQL
- sys.http_soap_methods_TSQL
- endpoint_webmethods
- sys.http_soap_methods
dev_langs:
- TSQL
helpviewer_keywords:
- sys.endpoint_webmethods catalog view
ms.assetid: 7dad0cf6-eafa-47cf-98cc-75ba8d3c7959
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7d528284dd7a8cb3efcbbd84d66a38362c0141f9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203915"
---
# <a name="sysendpoint_webmethods-transact-sql"></a>sys.endpoint_webmethods (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 Enthält eine Zeile mit einer FOR EACH SOAP-Methode, die für einen SOAP-aktivierten HTTP-Endpunkt definiert ist. Die Kombination der endpoint_id-Spalte und der namespace-Spalte ist eindeutig.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|endpoint_id|**int**|ID des Endpunktes, für die die Webmethode definiert ist|  
|Namespace|**nvarchar (384)**|Namespace für die Webmethode|  
|method_alias|**nvarchar (64)**|Alias für die Methode<br /><br /> Hinweis: Bezeichner [!INCLUDE[tsql](../../includes/tsql-md.md)] lassen Zeichen zu, die in WSDL-Methodennamen nicht zulässig sind.<br /><br /> Mit dem Alias wird der in der WSDL-Beschreibung des Endpunktes verfügbar gemachte Name dem eigentlichen zugrunde liegenden ausführbaren [!INCLUDE[tsql](../../includes/tsql-md.md)]-Objekt zugeordnet, das beim Starten der Webmethode aufgerufen wird.|  
|object_name|**nvarchar (776)**|Der Name des Objekts gemäß der Option NAME =, an das die Webmethode umgeleitet wird. Namens Teile sind durch einen Zeitraum (.) getrennt und durch eckige Klammern getrennt `[``]` .<br /><br /> Der Objektname muss gemäß der WSDL-Option dreiteilig sein.|  
|result_schema|**tinyint**|Option, die bestimmt, welches XSD ggf. mit einer Antwort zurückgesendet wird.<br /><br /> 0 = Keine<br /><br /> 1 = Standard<br /><br /> 2 = Standardwert|  
|result_schema_desc|**nvarchar(60)**|Beschreibung der Option, die bestimmt, welches XSD ggf. mit einer Antwort zurückgesendet wird.<br /><br /> Keine<br /><br /> STANDARD<br /><br /> DEFAULT|  
|result_format|**tinyint**|Option, die bestimmt, wie Ergebnisse in der Antwort formatiert werden.<br /><br /> 1 = ALL_RESULTS<br /><br /> 2 = ROWSETS_ONLY<br /><br /> 3 = NONE|  
|result_format_desc|**nvarchar(60)**|Beschreibung der Option, die bestimmt, wie Ergebnisse in der Antwort formatiert werden.<br /><br /> ALL_RESULTS<br /><br /> ROWSETS_ONLY<br /><br /> Keine|  
  
## <a name="permissions"></a>Berechtigungen  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Weitere Informationen finden Sie unter [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Endpoints Catalog Views &#40;Transact-SQL&#41; (Katalogsichten für Endpunkte &#40;Transact-SQL&#41;)](../../relational-databases/system-catalog-views/endpoints-catalog-views-transact-sql.md)   
 [Katalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
