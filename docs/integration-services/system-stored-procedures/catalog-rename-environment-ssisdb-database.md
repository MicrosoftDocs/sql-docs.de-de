---
description: catalog.rename_environment (SSISDB-Datenbank)
title: catalog.rename_environment (SSISDB-Datenbank) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: c73d7452-31c5-4f4e-afcc-e9eca760c826
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 5cff0cdf01fbfbb0af768a18ab54614c2a46f5bb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346510"
---
# <a name="catalogrename_environment-ssisdb-database"></a>catalog.rename_environment (SSISDB-Datenbank)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Benennt eine Umgebung im [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Katalog um.  
  
## <a name="syntax"></a>Syntax  
  
```sql  
catalog.rename_environment [ @folder_name = ] folder_name  
    , [ @environment_name = ] environment_name  
    , [ @new_environment_name= ] new_environment_name  
```  
  
## <a name="arguments"></a>Argumente  
 [ @folder_name = ] *folder_name*  
 Der Name des Ordners, der die Umgebung enthält. Der *folder_name* ist **nvarchar(128)** .  
  
 [ @environment_name = ] *environment_name*  
 Der ursprüngliche Name der Umgebung. Der *environment_name* ist **nvarchar(128)** .  
  
 [ @new_environment_name = ] *new_environment_name*  
 Der neue Name der Umgebung. Der *new_environment_name* ist **nvarchar(128)**.  
  
## <a name="return-code-value"></a>Rückgabecodewert  
 0 (Erfolg)  
  
## <a name="result-sets"></a>Resultsets  
 Keine  
  
## <a name="permissions"></a>Berechtigungen  
 Diese gespeicherte Prozedur erfordert eine der folgenden Berechtigungen:  
  
-   MODIFY-Berechtigung für die Umgebung  
  
-   Mitgliedschaft in der Datenbankrolle **ssis_admin**  
  
-   Mitgliedschaft in der Serverrolle **sysadmin**  
  
## <a name="errors-and-warnings"></a>Fehler und Warnungen  
 In der folgenden Liste werden einige Bedingungen beschrieben, die möglicherweise einen Fehler oder eine Warnung auslösen:  
  
-   Der ursprüngliche Umgebungsname ist ungültig.  
  
-   Der neue Name wurde bereits für eine vorhandene Umgebung verwendet.  
  
## <a name="remarks"></a>Bemerkungen  
 Umgebungsverweise aus Projekten werden nicht automatisch aktualisiert, wenn Sie die Umgebung umbenennen. Umgebungsverweise müssen entsprechend aktualisiert werden. Diese gespeicherte Prozedur wird auch dann erfolgreich ausgeführt, wenn Umgebungsverweise durch das Ändern des Umgebungsnamens beschädigt werden. Nach Abschluss dieser gespeicherten Prozedur müssen Umgebungsverweise aktualisiert werden.  
  
> [!NOTE]  
>  Wenn ein Umgebungsverweis ungültig ist, schlagen Überprüfung und Ausführung der entsprechenden Pakete fehl, von denen diese Verweise verwendet werden.  
  
  
