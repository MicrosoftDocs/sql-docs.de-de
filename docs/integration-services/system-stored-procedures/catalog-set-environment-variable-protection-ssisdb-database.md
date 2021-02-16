---
description: catalog.set_environment_variable_protection (SSISDB-Datenbank)
title: catalog.set_environment_variable_protection (SSISDB-Datenbank) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 005b6b2f-a5d9-4ea4-8d4e-beed6ab33c0d
author: chugugrace
ms.author: chugu
ms.openlocfilehash: a14d8ad6fdc2e48868e31a12ff82d7b445eb6145
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100273191"
---
# <a name="catalogset_environment_variable_protection-ssisdb-database"></a>catalog.set_environment_variable_protection (SSISDB-Datenbank)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Legt das Vertraulichkeitsbit einer Umgebungsvariablen im [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Katalog fest.  
  
## <a name="syntax"></a>Syntax  
  
```sql  
catalog.set_environment_variable_protection [ @folder_name = ] folder_name  
    , [ @environment_name = ] environment_name  
    , [ @variable_name = ] variable_name  
    , [ @sensitive = ] sensitive  
```  
  
## <a name="arguments"></a>Argumente  
 [ @folder_name = ] *folder_name*  
 Der Name des Ordners, der die Umgebung enthält. Der *folder_name* ist **nvarchar(128)** .  
  
 [ @environment_name = ] *environment_name*  
 Der Name der Umgebung. Der *environment_name* ist **nvarchar(128)** .  
  
 [ @variable_name = ] *variable_name*  
 Der Name der Umgebungsvariablen. Der *variable_name* ist **nvarchar(128)**.  
  
 [ @sensitive = ] *sensitive*  
 Gibt an, ob die Variable einen vertraulichen Wert enthält. Verwenden Sie den Wert `1` , um anzugeben, dass der Wert der Umgebungsvariablen vertraulich ist, oder den Wert `0` , um anzugeben, dass er nicht vertraulich ist. Ein vertraulicher Wert wird verschlüsselt, wenn er gespeichert wird. Ein Wert, der nicht vertraulich ist, wird als Nur-Text gespeichert. Der *sensitive* -Parameter ist **bit**.  
  
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
  
-   Der Ordnername ist ungültig.  
  
-   Der Umgebungsname ist ungültig.  
  
-   Der Umgebungsvariablenname ist ungültig.  
  
-   Der Benutzer verfügt nicht über die entsprechenden Berechtigungen.  
  
  
