---
description: catalog.deploy_project (SSISDB-Datenbank)
title: catalog.deploy_project (SSISDB-Datenbank) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/09/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 2e3439b4-7226-4b61-a993-7a1d161eac7e
author: chugugrace
ms.author: chugu
ms.openlocfilehash: ef918ad60dd685cf90fc043eba72dcc6dd7987ee
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342251"
---
# <a name="catalogdeploy_project-ssisdb-database"></a>catalog.deploy_project (SSISDB-Datenbank)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Stellt ein Projekt in einem Ordner im [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Katalog bereit oder aktualisiert ein vorhandenes Projekt, das zuvor bereitgestellt wurde.  
  
## <a name="syntax"></a>Syntax  
  
```sql  
catalog.deploy_project [@folder_name =] folder_name   
      , [ @project_name = ] project_name   
      , [ @project_stream = ] projectstream   
    [ , [ @operation_id = ] operation_id OUTPUT ]   
```  
  
## <a name="arguments"></a>Argumente  
 [@folder_name =] *folder_name*  
 Der Name des Ordners, in dem das Projekt bereitgestellt wird. Der *folder_name* ist **nvarchar(128)** .  
  
 [@project_name =] *project_name*  
 Der Name des neuen oder aktualisierten Projekts im Ordner. Der *project_name* ist **nvarchar(128)** .  
  
 [@projectstream =] *projectstream*  
 Der binäre Inhalt einer [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Projektbereitstellungsdatei (Erweiterung ISPAC).  
  
 Sie können eine SELECT-Anweisung mit der OPENROWSET-Funktion und dem BULK-Rowsetanbieter verwenden, um binäre Dateiinhalte abzurufen. Ein Beispiel finden Sie unter [Bereitstellen von SQL Server Integration Services-Projekten und Paketen (SSIS)](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md). Weitere Informationen zu OPENROWSET finden Sie unter [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md).  
  
 *projectstream* ist **varbinary(MAX)**.  
  
 [@operation_id =] *operation_id*  
 Gibt den eindeutigen Bezeichner für den Bereitstellungsvorgang zurück. Der *operation_id* ist **bigint**.  
  
## <a name="return-code-value"></a>Rückgabecodewert  
 0 (Erfolg)  
  
## <a name="result-sets"></a>Resultsets  
 Keine  
  
## <a name="permissions"></a>Berechtigungen  
 Diese gespeicherte Prozedur erfordert eine der folgenden Berechtigungen:  
  
-   CREATE_OBJECTS-Berechtigungen für den Ordner, um ein neues Projekt bereitzustellen, oder MODIFY-Berechtigungen für das Projekt, um ein Projekt zu aktualisieren  
  
-   Mitgliedschaft in der Datenbankrolle **ssis_admin**  
  
-   Mitgliedschaft in der Serverrolle **sysadmin**  
  
## <a name="errors-and-warnings"></a>Fehler und Warnungen  
 In der folgenden Liste werden Bedingungen beschrieben, die möglicherweise bewirken, dass diese gespeicherte Prozedur einen Fehler auslöst:  
  
-   Ein Parameter verweist auf ein Objekt, das nicht vorhanden ist, ein Parameter versucht, ein bereits vorhandenes Objekt zu erstellen, oder ein Parameter ist aus anderen Gründen ungültig  
  
-   Der Wert des Parameters *\@project_name* stimmt nicht mit dem Namen des Projekts in der Bereitstellungsdatei überein  
  
-   Der Benutzer verfügt nicht über ausreichende Berechtigungen  
  
## <a name="remarks"></a>Bemerkungen  
 Während einer Projektbereitstellung oder eines Projektupdates überprüft die gespeicherte Prozedur nicht die Schutzebene einzelner Pakete im Projekt.  
  
  
