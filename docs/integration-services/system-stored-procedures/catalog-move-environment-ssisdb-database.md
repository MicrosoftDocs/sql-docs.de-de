---
description: catalog.move_environment (SSISDB-Datenbank)
title: catalog.move_environment (SSISDB-Datenbank) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: b3fb5242-3c4c-4a87-b3e5-beb22fbab053
author: chugugrace
ms.author: chugu
ms.openlocfilehash: cba8586605a55d438d55b6954640bfb2366b1001
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342205"
---
# <a name="catalogmove_environment-ssisdb-database"></a>catalog.move_environment (SSISDB-Datenbank)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Verschiebt eine Umgebung von einem Ordner im [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Katalog in einen anderen.  
  
## <a name="syntax"></a>Syntax  
  
```sql  
catalog.move_environment [ @source_folder = ] source_folder  
    , [ @environment_name = ] environment_name  
    , [ @destination_folder = ] destination_folder  
```  
  
## <a name="arguments"></a>Argumente  
 [ @source_folder = ] *source_folder*  
 Der Name des Quellordners, in dem sich die Umgebung vor dem Verschieben befindet. Der *source_folder* ist **nvarchar(128)**.  
  
 [ @environment_name = ] *environment_name*  
 Der Name der Umgebung, die verschoben werden soll. Der *environment_name* ist **nvarchar(128)** .  
  
 [ @destination_folder = ] *destination_folder*  
 Der Name des Zielordners, in dem sich die Umgebung nach dem Verschieben befindet. Der *destination_folder* ist **nvarchar(128)**.  
  
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
  
-   Die Umgebung ist im Quellordner nicht vorhanden.  
  
-   Für den Zielordner ist bereits eine Umgebung mit dem gleichen Namen vorhanden.  
  
-   Der Benutzer verfügt nicht über die entsprechenden Berechtigungen.  
  
## <a name="remarks"></a>Bemerkungen  
 Umgebungsverweise aus Projekten werden nicht während des Verschiebens der Umgebung entsprechend angepasst. Umgebungsverweise müssen entsprechend aktualisiert werden. Diese gespeicherte Prozedur wird auch dann erfolgreich ausgeführt, wenn Umgebungsverweise durch das Verschieben einer Umgebung beschädigt werden. Nach Abschluss dieser gespeicherten Prozedur müssen Umgebungsverweise aktualisiert werden.  
  
> [!NOTE]  
>  Ein Projekt kann über relative oder absolute Umgebungsverweise verfügen. Relative Verweise verweisen mit dem Namen auf die Umgebung und erfordern, dass sich die Umgebung im gleichen Ordner wie das Projekt befindet. Absolute Verweise verweisen mit Name und Ordner auf die Umgebung, und sie verweisen auf Umgebungen, die sich in einem anderen Ordner als dem Ordner des Projekts befinden.  
  
  
