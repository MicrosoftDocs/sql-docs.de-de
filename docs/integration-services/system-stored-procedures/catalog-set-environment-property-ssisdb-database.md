---
description: catalog.set_environment_property (SSISDB-Datenbank)
title: catalog.set_environment_property (SSISDB-Datenbank) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: a345675b-d32e-4624-96cf-ec656730b114
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 892823abe1f7607a034f9cc22b34dd7a81c12ffc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100273221"
---
# <a name="catalogset_environment_property-ssisdb-database"></a>catalog.set_environment_property (SSISDB-Datenbank)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Legt die Eigenschaft einer Umgebung im [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Katalog fest.  
  
## <a name="syntax"></a>Syntax  
  
```sql  
catalog.set_environment_property [ @folder_name = ] folder_name  
    , [ @environment_name = ] environment_name  
    , [ @property_name = ] property_name  
    , [ @property_value = ] property_value  
```  
  
## <a name="arguments"></a>Argumente  
 [ @folder_name = ] *folder_name*  
 Der Name des Ordners, der die Umgebung enthält. Der *folder_name* ist **nvarchar(128)** .  
  
 [ @environment_name = ] *environment_name*  
 Der Name der Umgebung. Der *environment_name* ist **nvarchar(128)** .  
  
 [ @property_name = ] *property_name*  
 Der Name einer Umgebungseigenschaft. Der *property_name* ist **nvarchar(128)**.  
  
 [ @property_value = ] *property_value*  
 Der Wert der Umgebungseigenschaft. Der *property_value* ist **nvarchar(1024)**.  
  
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
  
-   Der Eigenschaftsname ist ungültig.  
  
-   Der Umgebungsname ist ungültig.  
  
## <a name="remarks"></a>Hinweise  
 In dieser Version kann nur die `Description` -Eigenschaft festgelegt werden. Der Wert für die `Description` -Eigenschaft darf 4000 Zeichen nicht überschreiten.  
  
  
