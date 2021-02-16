---
description: catalog.create_customized_logging_level
title: catalog.create_customized_logging_level | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 20b3ba0a-126f-49bf-b70f-61b2a0fcb750
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 5fbedb41369300353d7b0f8ce64da355592b6eaf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346845"
---
# <a name="catalogcreate_customized_logging_level"></a>catalog.create_customized_logging_level 

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Erstellt einen neuen benutzerdefinierten Protokolliergrad. Weitere Informationen zu benutzerdefinierten Protokolliergraden finden Sie unter [Integration Services &#40;SSIS&#41; Logging (SSIS-Protokollierung [SQL Server Integration Services])](../../integration-services/performance/integration-services-ssis-logging.md).  
  
## <a name="syntax"></a>Syntax  
  
```sql  
catalog.create_customized_logging_level [ @level_name = ] level_name  
    , [ @level_description = ] level_description  
    , [ @profile_value = ] profile_value  
    , [ @events_value = ] events_value  
    , [ @level_id = ] level_id OUT   
```  
  
## <a name="arguments"></a>Argumente  
 [ @level_name = ] *level_name*  
 Der Name für den neuen benutzerdefinierten Protokolliergrad.  
  
 Das Argument *level_name* ist vom Typ **nvarchar(128)**.  
  
 [ @level_description = ] *level_description*  
 Die Beschreibung für den neuen benutzerdefinierten Protokolliergrad.  
  
 Das Argument *level_description* ist vom Typ **nvarchar(max)**.  
  
 [ @profile_value = ] *profile_value*  
 Die Statistiken, die durch den neuen Protokolliergrad protokolliert werden sollen.  
  
 Die gültigen Werte für Statistiken werden im Folgenden aufgeführt. Diese Werte entsprechen denjenigen auf der Registerkarte **Statistik** des Dialogfelds **Verwaltung des angepassten Protokolliergrads**.  
  
-   Ausführung = 0  
  
-   Volume = 1  
  
-   Leistung = 2    
  
 Das Argument *profile_value* ist vom Typ **bigint**.  
  
 [ @events_value = ] *events_value*  
 Die Ereignisse, die durch den neuen Protokolliergrad protokolliert werden sollen.  
  
 Die gültigen Werte für Ereignisse werden im Folgenden aufgeführt. Diese Werte entsprechen denjenigen auf der Registerkarte **Ereignisse** des Dialogfelds **Verwaltung des angepassten Protokolliergrads**.  
  
|Ereignisse ohne Ereigniskontext|Ereignisse mit Ereigniskontext|  
|----------------------------------|-------------------------------|  
|OnVariableValueChanged = 0<br /><br /> OnExecutionStatusChanged = 1<br /><br /> OnPreExecute = 2<br /><br /> OnPostExecute = 3<br /><br /> OnPreValidate = 4<br /><br /> OnPostValidate = 5<br /><br /> OnWarning = 6<br /><br /> OnInformation = 7<br /><br /> OnError = 8<br /><br /> OnTaskFailed = 9<br /><br /> OnProgress = 10<br /><br /> OnQueryCancel = 11<br /><br /> OnBreakpointHit = 12<br /><br /> OnCustomEvent = 13<br /><br /> Diagnostic = 14<br /><br /> DiagnosticEx = 15<br /><br /> NonDiagnostic = 16|OnVariableValueChanged_IncludeContext = 32<br /><br /> OnExecutionStatusChanged_IncludeContext = 33<br /><br /> OnPreExecute_IncludeContext = 34<br /><br /> OnPostExecute_IncludeContext = 35<br /><br /> OnPreValidate_IncludeContext = 36<br /><br /> OnPostValidate_IncludeContext = 37<br /><br /> OnWarning_IncludeContext = 38<br /><br /> OnInformation_IncludeContext = 39<br /><br /> OnError_IncludeContext = 40<br /><br /> OnTaskFailed_IncludeContext = 41<br /><br /> OnProgress_IncludeContext = 42<br /><br /> OnQueryCancel_IncludeContext= 43<br /><br /> OnBreakpointHit_IncludeContext = 44<br /><br /> OnCustomEvent_IncludeContext = 45<br /><br /> Diagnostic_IncludeContext = 46<br /><br /> DiagnosticEx_IncludeContext = 47<br /><br /> NonDiagnostic_IncludeContext = 48|  
  
 Das Argument *events_value* ist vom Typ **bigint**.  
  
 [ @level_id = ] *level_id* OUT  
 Die ID des neuen benutzerdefinierten Protokolliergrads.  
  
 Das Argument *level_id* ist vom Typ **bigint**.  
  
## <a name="remarks"></a>Hinweise  
 Gehen Sie zum Kombinieren mehrerer Werte in Transact-SQL für das Argument *profile_value* oder *events_value* wie im folgenden Beispiel vor. Verwenden Sie zur Erfassung der Ereignisse OnError (8) und DiagnosticEx (15) als Berechnungsformel für *events_value* die Formel `2^8 + 2^15 = 33024`.  
  
## <a name="return-codes"></a>Rückgabecodes  
 0 (Erfolg)  
  
 Wenn die gespeicherte Prozedur fehlschlägt, wird ein Fehler ausgelöst.  
  
## <a name="result-set"></a>Resultset  
 Keine  
  
## <a name="permissions"></a>Berechtigungen  
 Diese gespeicherte Prozedur erfordert eine der folgenden Berechtigungen:  
  
-   Mitgliedschaft in der Datenbankrolle **ssis_admin**  
  
-   Mitgliedschaft in der Serverrolle **sysadmin**  
  
## <a name="errors-and-warnings"></a>Fehler und Warnungen  
 Die folgende Liste beschreibt Bedingungen, unter denen die gespeicherte Prozedur fehlschlägt.  
  
-   Der Benutzer verfügt nicht über die erforderlichen Berechtigungen.  
  
  
