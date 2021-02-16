---
description: catalog.stop_operation (SSISDB-Datenbank)
title: catalog.stop_operation (SSISDB-Datenbank) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 97fd9d22-03dd-4eda-8f6c-ba8b67acec68
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 1d215f7d53fcc525fdc6aca43c43df25a827ed6e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346744"
---
# <a name="catalogstop_operation-ssisdb-database"></a>catalog.stop_operation (SSISDB-Datenbank)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Beendet eine Überprüfung oder eine Ausführungsinstanz im [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Katalog.  
  
## <a name="syntax"></a>Syntax  
  
```sql  
catalog.stop_operation [ @operation_id = ] operation_id  
```  
  
## <a name="arguments"></a>Argumente  
 [ @operation_id = ] *operation_id*  
 Der eindeutige Bezeichner der Überprüfung oder Instanz der Ausführung. Der *operation_id* ist **bigint**.  
  
## <a name="return-code-value"></a>Rückgabecodewert  
 0 (Erfolg)  
  
## <a name="result-sets"></a>Resultsets  
 Keine  
  
## <a name="permissions"></a>Berechtigungen  
 Diese gespeicherte Prozedur erfordert eine der folgenden Berechtigungen:  
  
-   READ-Berechtigung und MODIFY-Berechtigung für die Überprüfung oder Instanz der Ausführung  
  
-   Mitgliedschaft in der Datenbankrolle **ssis_admin**  
  
-   Mitgliedschaft in der Serverrolle **sysadmin**  
  
## <a name="errors-and-warnings"></a>Fehler und Warnungen  
 In der folgenden Liste werden einige Bedingungen beschrieben, die möglicherweise einen Fehler oder eine Warnung auslösen:  
  
-   Der Benutzer verfügt nicht über die entsprechenden Berechtigungen.  
  
-   Der Vorgangsbezeichner ist ungültig.  
  
-   Der Vorgang wurde bereits beendet.  
  
## <a name="remarks"></a>Hinweise  
 Ein Vorgang im [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Katalog darf immer nur von jeweils einem Benutzer beendet werden. Wenn mehrere Benutzer versuchen, den Vorgang zu beenden, gibt die gespeicherte Prozedur beim ersten Versuch Erfolg (der Wert `0`) zurück, bei anschließenden Versuchen wird jedoch ein Fehler ausgelöst.  
  
  
