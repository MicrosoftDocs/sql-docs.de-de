---
description: catalog.object_parameters (SSISDB-Datenbank)
title: catalog.object_parameters (SSISDB-Datenbank) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: d7b04903-2d61-4159-9456-475942d1f732
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 423f96cba79018230b7121bf25f93337a48a2fd6
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835183"
---
# <a name="catalogobject_parameters-ssisdb-database"></a>catalog.object_parameters (SSISDB-Datenbank)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  Zeigt die Parameter für alle Pakete und Projekte im [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Katalog an.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|parameter_id|**bigint**|Der eindeutige Bezeichner (ID) des Parameters.|  
|project_id|**bigint**|Die eindeutige ID des Projekts.|  
|object_type|**smallint**|Der Typ des Parameters. Für einen Projektparameter ist der Wert `20` , und für einen Paketparameter ist der Wert `30` .|  
|object_name|**sysname**|Der Name des entsprechenden Projekts oder Pakets.|  
|parameter_name|**sysname(nvarchar(128))**|Der Name des Parameters.|  
|data_type|**nvarchar(128)**|Der Datentyp des Parameters.|  
|Erforderlich|**bit**|Wenn der Wert `1`lautet, ist der Parameterwert erforderlich, um die Ausführung zu starten. Wenn der Wert `0`lautet, ist der Parameterwert nicht erforderlich, um die Ausführung zu starten.|  
|sensitive|**bit**|Wenn der Wert `1`lautet, ist der Parameterwert vertraulich. Wenn der Wert `0`lautet, ist der Parameterwert nicht vertraulich.|  
|description|**nvarchar(1024)**|Eine optionale Beschreibung des Pakets.|  
|design_default_value|**sql_variant**|Der Standardwert für den Parameter, der während des Entwurfs des Projekts oder Pakets zugewiesen wurde.|  
|default_value|**sql_variant**|Der Standardwert, der derzeit auf dem Server verwendet wird.|  
|value_type|**char(1)**|Gibt den Typ des Parameterwerts an. In diesem Feld wird `V` angezeigt, wenn „parameter_value“ ein Literalwert ist, und `R` wird angezeigt, wenn der Wert durch Verweis auf eine Umgebungsvariable zugewiesen wird.|  
|value_set|**bit**|Wenn der Wert `1`ist, wurde der Parameterwert zugewiesen. Wenn der Wert `0`ist, wurde der Parameterwert nicht zugewiesen.|  
|referenced_variable_name|**nvarchar(128)**|Der Name der Umgebungsvariablen, die dem Wert des Parameters zugewiesen wird. Der Standardwert ist **NULL**.|  
|validation_status|**char(1)**|Nur für Informationszwecke identifiziert. Wird in [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)] nicht verwendet.|  
|last_validation_time|**datetimeoffset(7)**|Nur für Informationszwecke identifiziert. Wird in [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)] nicht verwendet.|  
  
## <a name="permissions"></a>Berechtigungen  
 Zum Anzeigen von Zeilen in dieser Sicht benötigten Sie eine der folgenden Berechtigungen:  
  
-   READ-Berechtigung für das Projekt  
  
-   Mitgliedschaft in der Datenbankrolle **ssis_admin**  
  
-   Mitgliedschaft in der Serverrolle **sysadmin**  
  
 Sicherheit auf Zeilenebene wird erzwungen. Es werden nur Zeilen angezeigt, zu deren Anzeige Sie berechtigt sind.  
  
  
