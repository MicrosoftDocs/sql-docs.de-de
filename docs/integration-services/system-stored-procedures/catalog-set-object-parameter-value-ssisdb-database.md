---
description: catalog.set_object_parameter_value (SSISDB-Datenbank)
title: catalog.set_object_parameter_value (SSISDB-Datenbank) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: fb887543-f92f-404d-9495-a1dd23a6716e
author: chugugrace
ms.author: chugu
ms.openlocfilehash: bdc1a5ff8944a29d7e2a2fcdeec297973c9849f8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355299"
---
# <a name="catalogset_object_parameter_value-ssisdb-database"></a>catalog.set_object_parameter_value (SSISDB-Datenbank)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Legt den Wert eines Parameters im [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Katalog fest. Ordnet einer Umgebungsvariable den Wert zu oder weist einen Literalwert zu, der standardmäßig verwendet wird, wenn keine anderen Werte zugewiesen werden.  
  
## <a name="syntax"></a>Syntax  
  
```sql  
catalog.set_object_parameter_value [ @object_type = ] object_type   
    , [ @folder_name = ] folder_name   
    , [ @project_name = ] project_name   
    , [ @parameter_name = ] parameter_name   
    , [ @parameter_value = ] parameter_value   
 [  , [ @object_name = ] object_name ]  
 [  , [ @value_type = ] value_type ]  
```  
  
## <a name="arguments"></a>Argumente  
 [@object_type =] *object_type*  
 Der Typ des Parameters. Verwenden Sie den Wert `20` , um einen Projektparameter anzugeben, oder den Wert `30` , um einen Paketparameter anzugeben. Der *object_type* ist **smallInt**.  
  
 [@folder_name =] *folder_name*  
 Der Name des Ordners, der den Parameter enthält. Der *folder_name* ist **nvarchar(128)** .  
  
 [@project_name =] *project_name*  
 Der Name des Projekts, das den Parameter enthält. Der *project_name* ist **nvarchar(128)** .  
  
 [@parameter_name =] *parameter_name*  
 Der Name des Parameters. Der *parameter_name* ist **nvarchar(128)**.  
  
 [@parameter_value =] *parameter_value*  
 Der Wert des Parameters. Der *parameter_value* ist **sql_variant**.  
  
 [@object_name =] *object_name*  
 Der Name des Pakets. Dieses Argument ist erforderlich, wenn der Parameter ein Paketparameter ist. Der *object_name* ist **nvarchar(260)**.  
  
 [@value_type =] *value_type*  
 Der Typ des Parameterwerts. Verwenden Sie das Zeichen `V`, um anzugeben, dass *parameter_value* ein Literalwert ist, der standardmäßig verwendet wird, wenn vor der Ausführung keine anderen Werte zugewiesen werden. Verwenden Sie das Zeichen `R` , um anzugeben, dass *parameter_value* ein Wert ist, auf den verwiesen wird und der auf den Namen einer Umgebungsvariablen festgelegt wurde. Dieses Argument ist optional. Das Zeichen `V` wird standardmäßig verwendet. Der *value_type* ist **char(1)**.  
  
## <a name="return-code-value"></a>Rückgabecodewert  
 0 (Erfolg)  
  
## <a name="result-sets"></a>Resultsets  
 Keine  
  
## <a name="permissions"></a>Berechtigungen  
 Diese gespeicherte Prozedur erfordert eine der folgenden Berechtigungen:  
  
-   READ-Berechtigung und MODIFY-Berechtigung für das Projekt  
  
-   Mitgliedschaft in der Datenbankrolle **ssis_admin**  
  
-   Mitgliedschaft in der Serverrolle **sysadmin**  
  
## <a name="errors-and-warnings"></a>Fehler und Warnungen  
 In der folgenden Liste werden Bedingungen beschrieben, die möglicherweise bewirken, dass die gespeicherte Prozedur einen Fehler auslöst:  
  
-   Der Parametertyp ist ungültig.  
  
-   Der Projektname ist ungültig.  
  
-   Für Paketparameter ist der Paketname ungültig.  
  
-   Der Werttyp ist ungültig.  
  
-   Der Benutzer verfügt nicht über die entsprechenden Berechtigungen.  
  
## <a name="remarks"></a>Bemerkungen  
  
-   Wenn kein *value_type* angegeben ist, wird standardmäßig ein Literalwert für *parameter_value* verwendet. Wenn ein Literalwert verwendet wird, wird *value_set* in der Sicht [object_parameters](../../integration-services/system-views/catalog-object-parameters-ssisdb-database.md) auf `1` festgelegt. Ein NULL-Parameterwert ist nicht zulässig.  
  
-   Wenn *value_type* das Zeichen `R`enthält, das einen Wert bezeichnet, auf den verwiesen wird, verweist *parameter_value* auf den Namen einer Umgebungsvariablen.  
  
-   Für `20` kann der Wert *object_type* verwendet werden, um einen Projektparameter anzugeben. In diesem Fall ist für *object_name* kein Wert erforderlich, und jeder für *object_name* angegebene Wert wird ignoriert. Dieser Wert wird verwendet, wenn der Benutzer einen Projektparameter festlegen möchte.  
  
-   Für `30` kann der Wert *object_type* verwendet werden, um einen Paketparameter anzugeben. In diesem Fall wird ein Wert für *object_name* verwendet, um das entsprechende Paket anzugeben. Wenn *object_name* nicht angegeben wird, gibt die gespeicherte Prozedur einen Fehler zurück und wird beendet.  
  
  
