---
description: catalog.move_project (SSISDB-Datenbank)
title: catalog.move_project (SSISDB-Datenbank) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: ef3b0325-d8e9-472b-bf11-7d3efa6312ff
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 4c7cf1e3c23eacbabfd59ba3b8948740dc1a0543
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342191"
---
# <a name="catalogmove_project---ssisdb-database"></a>catalog.move_project (SSISDB-Datenbank)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Verschiebt ein Projekt von einem Ordner im [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Katalog in einen anderen.  
  
## <a name="syntax"></a>Syntax  
  
```sql  
catalog.move_project [ @source_folder = ] source_folder  
    , [ @project_name = ] project_name  
    , [ @destination_folder = ] destination_folder  
```  
  
## <a name="arguments"></a>Argumente  
 [ @source_folder = ] *source_folder*  
 Der Name des Quellordners, in dem sich das Projekt vor dem Verschieben befindet. Der *source_folder* ist **nvarchar(128)**.  
  
 [ @project_name = ] *project_name*  
 Der Name des Projekts, das verschoben werden soll. Der *project_name* ist **nvarchar(128)** .  
  
 [ @destination_folder = ] *destination_folder*  
 Der Name des Zielordners, in dem sich das Projekt nach dem Verschieben befindet. Der *destination_folder* ist **nvarchar(128)**.  
  
## <a name="return-code-value"></a>Rückgabecodewert  
 0 (Erfolg)  
  
## <a name="result-sets"></a>Resultsets  
 Keine  
  
## <a name="permissions"></a>Berechtigungen  
 Diese gespeicherte Prozedur erfordert eine der folgenden Berechtigungen:  
  
-   READ-Berechtigung und MODIFY-Berechtigung für das Projekt, das verschoben werden soll, und CREATE_OBJECTS-Berechtigung für den Zielordner  
  
-   Mitgliedschaft in der Datenbankrolle **ssis_admin**  
  
-   Mitgliedschaft in der Serverrolle **sysadmin**  
  
## <a name="errors-and-warnings"></a>Fehler und Warnungen  
 In der folgenden Liste werden Bedingungen beschrieben, die möglicherweise bewirken, dass diese gespeicherte Prozedur einen Fehler auslöst:  
  
-   Das Projekt ist nicht vorhanden.  
  
-   Der Quellordner nicht vorhanden.  
  
-   Der Zielordner ist nicht vorhanden, oder der Zielordner enthält bereits ein Projekt mit dem gleichen Namen.  
  
-   Der Benutzer verfügt nicht über die entsprechenden Berechtigungen.  
  
## <a name="remarks"></a>Bemerkungen  
 Wenn ein Projekt von einem Quellordner in einen Zielordner verschoben wird, werden das Projekt im Quellordner und entsprechende Umgebungsverweise gelöscht. Im Zielordner werden ein identisches Projekt und Umgebungsverweise erstellt. Relative Umgebungsverweise verweisen nach dem Verschieben auf einen anderen Ordner. Absolute Verweise verweisen nach dem Verschieben auf den gleichen Ordner.  
  
> [!NOTE]  
>  Ein Projekt kann über relative oder absolute Umgebungsverweise verfügen. Relative Verweise verweisen mit dem Namen auf die Umgebung und erfordern, dass sich die Umgebung im gleichen Ordner wie das Projekt befindet. Absolute Verweise verweisen mit Name und Ordner auf die Umgebung, und sie verweisen auf Umgebungen, die sich in einem anderen Ordner als dem Ordner des Projekts befinden.  
  
  
