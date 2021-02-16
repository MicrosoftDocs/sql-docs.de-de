---
description: catalog.set_execution_parameter_value (SSISDB-Datenbank)
title: catalog.set_execution_parameter_value (SSISDB-Datenbank) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 055d86c9-befd-4e63-acb1-6dfe833549d2
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 9c8c294aa9aefa326ad9b8d84ddb599f99f181cd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100273171"
---
# <a name="catalogset_execution_parameter_value-ssisdb-database"></a>catalog.set_execution_parameter_value (SSISDB-Datenbank)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Legt den Wert eines Parameters für eine Instanz der Ausführung im [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Katalog fest.  
  
 Ein Parameterwert kann nicht geändert werden, nachdem eine Instanz der Ausführung gestartet wurde.  
  
## <a name="syntax"></a>Syntax  
  
```sql  
catalog.set_execution_parameter_value [ @execution_id = execution_id  
    , [ @object_type = ] object_type  
    , [ @parameter_name = ] parameter_name  
    , [ @parameter_value = ] parameter_value  
```  
  
## <a name="arguments"></a>Argumente  
 [ @execution_id = ] *execution_id*  
 Der eindeutige Bezeichner für die Instanz der Ausführung. Der *execution_id* ist **bigint**.  
  
 [ @object_type = ] *object_type*  
 Der Typ des Parameters.  
  
 Legen Sie für die folgenden Parameter *object_type* auf „50“ fest.  
  
-   LOGGING_LEVEL  
  
-   CUSTOMIZED_LOGGING_LEVEL  
  
-   DUMP_ON_ERROR  
  
-   DUMP_ON_EVENT  
  
-   DUMP_EVENT_CODE  
  
-   CALLER_INFO  
  
-   SYNCHRONIZED  
  
 Verwenden Sie den Wert `20` , um einen Projektparameter anzugeben, oder den Wert `30` , um einen Paketparameter anzugeben.  
  
 *object_type* ist vom Typ **smallInt**.  
  
 [ @parameter_name = ] *parameter_name*  
 Der Name des Parameters. Der *parameter_name* ist **nvarchar(128)**.  
  
 [ @parameter_value = ] *parameter_value*  
 Der Wert des Parameters. Der *parameter_value* ist **sql_variant**.  
  
## <a name="remarks"></a>Hinweise  
 Um die Parameterwerte zu ermitteln, die für eine bestimmte Ausführung verwendet wurden, fragen Sie die catalog.execution_parameter_values-Sicht ab.  
  
 Um den Umfang der Informationen anzugeben, die während einer Paketausführung protokolliert werden, legen Sie *parameter_name* auf LOGGING_LEVEL und *parameter_value* auf einen der folgenden Werte fest.  
  
 Legen Sie den *object_type*-Parameter auf „50“ fest.  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|0|Keine<br /><br /> Die Protokollierung ist deaktiviert. Nur der Status der Ausführung von Paketen wird protokolliert.|  
|1|Basic<br /><br /> Alle Ereignisse werden protokolliert, außer benutzerdefinierten und Diagnose-Ereignissen. Dies ist der Standardwert.|  
|2|Leistung<br /><br /> Nur Leistungsstatistiken sowie OnError- und OnWarning-Ereignisse werden protokolliert.|  
|3|Ausführlich<br /><br /> Alle Ereignisse werden protokolliert, einschließlich benutzerdefinierter Ereignisse und Diagnose-Ereignissen. <br />Zu den benutzerdefinierten Ereignissen zählen auch von Integration Services-Tasks protokollierte Ereignisse. Weitere Informationen finden Sie unter [Benutzerdefinierte Meldungen für die Protokollierung](../../integration-services/performance/integration-services-ssis-logging.md#custom_messages).|  
|4|Runtimeherkunft<br /><br /> Sammelt die Daten, die zum Nachverfolgen der Datenherkunft im Datenfluss benötigt werden.|  
|100|Benutzerdefinierter Protokolliergrad<br /><br /> Legen Sie die Einstellungen im CUSTOMIZED_LOGGING_LEVEL-Parameter fest. Weitere Informationen zu den Werten, die Sie angeben können, finden Sie unter [catalog.create_customized_logging_level](../../integration-services/system-stored-procedures/catalog-create-customized-logging-level.md).<br /><br /> Weitere Informationen zu benutzerdefinierten Protokolliergraden finden Sie unter [Aktivieren der Protokollierung für die Paketausführung auf dem SSIS-Server](../../integration-services/performance/integration-services-ssis-logging.md#server_logging).|  
  
 Wenn Sie festlegen möchten, dass der Integration Services-Server Dumpdateien generiert, wenn während einer Paketausführung ein Fehler auftritt, legen Sie die folgenden Parameterwerte für eine Ausführungsinstanz fest, die nicht ausgeführt wurde.  
  
|Parameter|Wert|  
|---------------|-----------|  
|*execution_id*|Der eindeutige Bezeichner für die Instanz der Ausführung|  
|*object_type*|50|  
|*parameter_name*|'DUMP_ON_ERROR|  
|*parameter_value*|1|  
  
 Wenn Sie festlegen möchten, dass der Integration Services-Server Dumpdateien generiert, wenn während einer Paketausführung Ereignisse auftreten, legen Sie die folgenden Parameterwerte für eine Ausführungsinstanz fest, die nicht ausgeführt wurde.  
  
|Parameter|Wert|  
|---------------|-----------|  
|*execution_id*|Der eindeutige Bezeichner für die Instanz der Ausführung|  
|*object_type*|50|  
|*parameter_name*|'DUMP_ON_EVENT|  
|*parameter_value*|1|  
  
 Wenn Sie die während einer Paketausführung auftretenden Ereignisse festlegen möchten, die Integration Services-Server zum Generieren von Dumpdateien veranlassen, legen Sie die folgenden Parameterwerte für eine Ausführungsinstanz fest, die nicht ausgeführt wurde. Trennen Sie mehrere Ereigniscodes mithilfe eines Semikolons.  
  
|Parameter|Wert|  
|---------------|-----------|  
|*execution_id*|Der eindeutige Bezeichner für die Instanz der Ausführung|  
|*object_type*|50|  
|*parameter_name*|DUMP_EVENT_CODE|  
|*parameter_value*|Ein oder mehrere Ereigniscodes|  
  
## <a name="examples"></a>Beispiele  

### <a name="a-generate-dump-files-for-errors"></a>A. Generieren von Sicherungsdateien bei Fehlern

 Im folgenden Beispiel wird angegeben, dass der Integration Services-Server Dumpdateien generiert, wenn während einer Paketausführung ein Fehler auftritt.  
  
```sql
exec catalog.create_execution  'TR2','Recurring ETL', 'Dim_DCVendor.dtsx',NULL, 0,@execution_id out  
exec catalog.set_execution_parameter_value  @execution_id, 50, 'DUMP_ON_ERROR',1  
```  
  
### <a name="b-generate-dump-files-for-events"></a>B. Generieren von Sicherungsdateien bei Ereignissen

 Im folgenden Beispiel wird angegeben, dass der Integration Services-Server Dumpdateien generiert, wenn während einer Paketausführung Ereignisse auftreten. Zudem wird das Ereignis angegeben, das den Server zum Generieren der Dateien veranlasst.  
  
```sql
exec catalog.create_execution  'TR2','Recurring ETL', 'Dim_DCVendor.dtsx',NULL, 0,@execution_id out  
exec catalog.set_execution_parameter_value  @execution_id, 50, 'DUMP_ON_EVENT',1  
  
declare @event_code nvarchar(50)  
set @event_code = '0xC020801C'  
exec catalog.set_execution_parameter_value  @execution_id, 50, 'DUMP_EVENT_CODE', @event_code  
```  
  
## <a name="return-code-value"></a>Rückgabecodewert  
 0 (Erfolg)  
  
## <a name="result-sets"></a>Resultsets  
 Keine  
  
## <a name="permissions"></a>Berechtigungen  
 Diese gespeicherte Prozedur erfordert eine der folgenden Berechtigungen:  
  
-   READ- und MODIFY-Berechtigungen für die Instanz der Ausführung  
  
-   Mitgliedschaft in der Datenbankrolle **ssis_admin**  
  
-   Mitgliedschaft in der Serverrolle **sysadmin**  
  
## <a name="errors-and-warnings"></a>Fehler und Warnungen  
 In der folgenden Liste werden einige Bedingungen beschrieben, die möglicherweise einen Fehler oder eine Warnung auslösen:  
  
-   Der Benutzer verfügt nicht über die entsprechenden Berechtigungen.  
  
-   Der Ausführungsbezeichner ist ungültig.  
  
-   Der Parametername ist ungültig.  
  
-   Der Datentyp des Parameterwerts stimmt nicht mit dem Datentyp des Parameters überein.  
  
## <a name="see-also"></a>Weitere Informationen  
 [catalog.execution_parameter_values &#40;SSISDB-Datenbank&#41;](../../integration-services/system-views/catalog-execution-parameter-values-ssisdb-database.md)   
 [Generieren von Dumpdateien für die Paketausführung](../../integration-services/troubleshooting/generating-dump-files-for-package-execution.md)  
  
  
