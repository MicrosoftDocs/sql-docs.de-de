---
description: catalog.create_environment_variable (SSISDB-Datenbank)
title: catalog.create_environment_variable (SSISDB-Datenbank) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 91ed017b-6567-4bf2-b9f1-e2b5c70a5343
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 7a3b2b55c5a0efbc2acf8152b89a292adf4cc298
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346813"
---
# <a name="catalogcreate_environment_variable-ssisdb-database"></a>catalog.create_environment_variable (SSISDB-Datenbank)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Erstellt eine Umgebungsvariable im [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Katalog.  
  
## <a name="syntax"></a>Syntax  
  
```sql  
catalog.create_environment_variable [ @folder_name = ] folder_name  
    , [ @environment_name = ] environment_name  
    , [ @variable_name = ] variable_name  
    , [ @data_type = ] data_type  
    , [ @sensitive = ] sensitive  
    , [ @value = ] value  
    , [ @description = ] description  
```  
  
## <a name="arguments"></a>Argumente  
 [@folder_name =] *folder_name*  
 Der Name des Ordners, der die Umgebung enthält. Der *folder_name* ist **nvarchar(128)** .  
  
 [@environment_name =] *environment_name*  
 Der Name der Umgebung. Der *environment_name* ist **nvarchar(128)** .  
  
 [@variable_name =] *variable_name*  
 Der Name der Umgebungsvariablen. Der *variable_name* ist **nvarchar(128)**.  
  
 [@data_type =] *data_type*  
 Der Datentyp der Variablen. Zu den unterstützten Umgebungsvariablen-Datentypen zählen **Boolean**, **Byte**, **DateTime**, **Double**, **Int16**, **Int32**, **Int64**, **Single**, **String**, **UInt32** und **UInt64**. Die Umgebungsvariablen-Datentypen **Char**, **DBNull**, **Object** und **Sbyte** werden nicht unterstützt. Der *data_type*-Parameter ist vom Typ **nvarchar(128)**.  
  
 [@sensitive =] *sensitive*  
 Gibt an, ob die Variable einen vertraulichen Wert enthält. Verwenden Sie den Wert `1` , um anzugeben, dass der Wert der Umgebungsvariablen vertraulich ist, oder den Wert `0` , um anzugeben, dass er nicht vertraulich ist. Ein vertraulicher Wert wird verschlüsselt, wenn er gespeichert wird. Ein Wert, der nicht vertraulich ist, wird als Nur-Text-Wert gespeichert. *Sensitive* ist vom Typ **bit**.  
  
 [@value =] *value*  
 Der Wert der Umgebungsvariablen. Der *value* ist **sql_variant**.  
  
 [@description =] *description*  
 Die Beschreibung der Umgebungsvariablen. *value* ist vom Typ **nvarchar(1024)**.  
  
## <a name="return-code-value"></a>Rückgabecodewert  
 0 (Erfolg)  
  
## <a name="result-sets"></a>Resultsets  
 Keine  
  
## <a name="permissions"></a>Berechtigungen  
 Diese gespeicherte Prozedur erfordert eine der folgenden Berechtigungen:  
  
-   READ-Berechtigung und MODIFY-Berechtigung für die Umgebung  
  
-   Mitgliedschaft in der Datenbankrolle **ssis_admin**  
  
-   Mitgliedschaft in der Serverrolle **sysadmin**  
  
## <a name="errors-and-warnings"></a>Fehler und Warnungen  
 In der folgenden Liste werden einige Bedingungen beschrieben, die möglicherweise einen Fehler oder eine Warnung auslösen:  
  
-   Der Name des Ordners, der Umgebung oder der Umgebungsvariablen ist ungültig.  
  
-   Der Variablenname ist bereits in der Umgebung vorhanden.  
  
-   Der Benutzer verfügt nicht über die entsprechenden Berechtigungen.  
  
## <a name="remarks"></a>Bemerkungen  
 Mit einer Umgebungsvariablen kann zur Ausführung eines Pakets einem Projektparameter oder Paketparameter effizient ein Wert zugewiesen werden. Umgebungsvariablen ermöglichen die Organisation von Parameterwerten. Variablennamen müssen innerhalb einer Umgebung eindeutig sein.  
  
 Die gespeicherte Prozedur überprüft den Datentyp der Variablen, um sicherzustellen, dass sie vom [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Katalog unterstützt wird.  
  
> [!TIP]  
>  Eventuell sollten Sie anstelle des nicht unterstützten Datentyps **Sbyte** den Datentyp **Int16** in [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] verwenden.  
  
 Der Wert, der dieser gespeicherten Prozedur mit dem Parameter *value* übergeben wurde, wird gemäß der folgenden Tabelle von einem [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Datentyp in einen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datentyp konvertiert:  
  
|Integration Services-Datentyp|SQL Server-Datentyp|  
|------------------------------------|--------------------------|  
|**Boolescher Wert**|**bit**|  
|**Byte**|**binary**, **varbinary**|  
|**DateTime**|**datetime**, **datetime2**, **datetimeoffset**, **smalldatetime**|  
|**Double**|Genauer numerischer Ausdruck: **decimal**, **numeric**; ungefährer numerischer Ausdruck: **float**, **real**|  
|**Int16**|**smallint**|  
|**Int32**|**int**|  
|**Int64**|**bigint**|  
|**Single**|Genauer numerischer Ausdruck: **decimal**, **numeric**; ungefährer numerischer Ausdruck: **float**, **real**|  
|**String**|**varchar**, **nvarchar**, **char**|  
|**UInt32**|**int** (**int** ist die nächste verfügbare Zuordnung zu **Uint32**.)|  
|**UInt64**|**bigint** (**int** ist die nächste verfügbare Zuordnung zu **Uint64**.)|  
  
  
