---
description: catalog.get_parameter_values (SSISDB-Datenbank)
title: catalog.get_parameter_values (SSISDB-Datenbank) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 5b1aeaf7-c938-4aef-bafc-e4d7a82eb578
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 176fde267c490d41a41003e58cf1e201862a110b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342242"
---
# <a name="catalogget_parameter_values-ssisdb-database"></a>catalog.get_parameter_values (SSISDB-Datenbank)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Ermittelt die Standardparameterwerte und ruft diese aus einem Projekt und den entsprechenden Paketen im [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Katalog ab.  
  
## <a name="syntax"></a>Syntax  
  
```sql  
catalog.get_parameter_values [ @folder_name = ] folder_name  
     , [ @project_name = ] project_name  
     , [ @package_name = ] package_name  
  [  , [ @reference_id = ] reference_id  ]  
  
```  
  
## <a name="arguments"></a>Argumente  
 [ @folder_name = ] *folder_name*  
 Der Name des Ordners, der das Projekt enthält. Der *folder_name* ist **nvarchar(128)** .  
  
 [ @project_name = ] *project_name*  
 Der Name des Projekts, in dem sich die Parameter befinden. Der *project_name* ist **nvarchar(128)** .  
  
 [ @package_name = ] *package_name*  
 Der Name des Pakets. Geben Sie den Paketnamen an, um alle Projektparameter und die Parameter aus einem bestimmten Paket abzurufen. Der *package_name* ist **nvarchar(260)**.  
  
 [ @reference_id = ] *reference_id*  
 Der eindeutige Bezeichner eines Umgebungsverweises. Dieser Parameter ist optional. Der *reference_id* ist **bigint**.  
  
## <a name="return-code-value"></a>Rückgabecodewert  
 0 (Erfolg)  
  
## <a name="result-sets"></a>Resultsets  
 Gibt eine Tabelle zurück, die das folgende Format aufweist:  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|object_type|**smallint**|Der Typ des Parameters. Für einen Projektparameter ist der Wert `20` , und für einen Paketparameter ist der Wert `30` .|  
|parameter_data_type|**nvarchar(128)**|Der Datentyp des Parameters.|  
|parameter_name|**sysname**|Der Name des Parameters.|  
|parameter_value|**sql_variant**|Der Wert des Parameters.|  
|sensitive|**bit**|Wenn der Wert `1`lautet, ist der Parameterwert vertraulich. Wenn der Wert `0`lautet, ist der Parameterwert nicht vertraulich.|  
|Erforderlich|**bit**|Wenn der Wert `1`lautet, ist der Parameterwert erforderlich, um die Ausführung zu starten. Wenn der Wert `0`lautet, ist der Parameterwert nicht erforderlich, um die Ausführung zu starten.|  
|value_set|**bit**|Wenn der Wert `1`ist, wurde der Parameterwert zugewiesen. Wenn der Wert `0`ist, wurde der Parameterwert nicht zugewiesen.|  
  
> [!NOTE]  
>  Literalwerte werden als Nur-Text angezeigt. Anstelle vertraulicher Werte wird **NULL** angezeigt.  
  
## <a name="permissions"></a>Berechtigungen  
 Diese gespeicherte Prozedur erfordert eine der folgenden Berechtigungen:  
  
-   READ-Berechtigungen für das Projekt und ggf. READ-Berechtigung für die Umgebung, auf die verwiesen wird  
  
-   Mitgliedschaft in der Datenbankrolle **ssis_admin**  
  
-   Mitgliedschaft in der Serverrolle **sysadmin**  
  
## <a name="errors-and-warnings"></a>Fehler und Warnungen  
 In der folgenden Liste werden einige Bedingungen beschrieben, die möglicherweise einen Fehler oder eine Warnung auslösen:  
  
-   Das Paket kann im angegebenen Ordner oder Projekt nicht gefunden werden.  
  
-   Der Benutzer verfügt nicht über die entsprechenden Berechtigungen.  
  
-   Der angegebene Umgebungsverweis ist nicht vorhanden.  
  
  
