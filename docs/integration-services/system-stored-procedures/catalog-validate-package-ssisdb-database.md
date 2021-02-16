---
description: catalog.validate_package (SSISDB-Datenbank)
title: catalog.validate_package (SSISDB-Datenbank) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
helpviewer_keywords:
- validate_package stored procedure [Integration Services]
- catalog.validate_package stored procedure [Integration Services]
ms.assetid: 0dc03df1-b793-408f-af4c-c11188729abf
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 2aca1e5c0b1ace9e1020b58464459415a8939ce1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350260"
---
# <a name="catalogvalidate_package-ssisdb-database"></a>catalog.validate_package (SSISDB-Datenbank)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Überprüft asynchron ein Paket im [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Katalog.  
  
## <a name="syntax"></a>Syntax  
  
```sql
catalog.validate_package [ @folder_name = ] folder_name  
    , [ @project_name = ] project_name  
    , [ @package_name = ] package_name  
    , [ @validation_id = ] validation_id OUTPUT  
 [  , [ @use32bitruntime = ] use32bitruntime ]  
 [  , [ @environment_scope = ] environment_scope ]  
 [  , [ @reference_id = ] reference_id ]  
```  
  
## <a name="arguments"></a>Argumente  
 [ @folder_name = ] *folder_name*  
 Der Name des Ordners, der das Paket enthält. Der *folder_name* ist **nvarchar(128)** .  
  
 [ @project_name = ] *project_name*  
 Der Name des Projekts, das das Paket enthält. Der *project_name* ist **nvarchar(128)** .  
  
 [ @package_name = ] *package_name*  
 Der Name des Pakets. Der *package_name* ist **nvarchar(260)**.  
  
 [ @validation_id = ] *validation_id*  
 Gibt den eindeutigen Bezeichner (ID) der Überprüfung zurück. Das Argument *validation_id* ist vom Typ **bigint**.  
  
 [ @use32bitruntime = ] *use32bitruntime*  
 Gibt an, ob die 32-Bit-Laufzeit verwendet werden soll, um das Paket unter einem 64-Bit-Betriebssystem auszuführen. Verwenden Sie den Wert `1`, um das Paket mit der 32-Bit-Runtime auszuführen, wenn die Ausführung unter einem 64-Bit-Betriebssystem erfolgt. Verwenden Sie den Wert `0`, um das Paket mit der 64-Bit-Laufzeit auszuführen, wenn die Ausführung unter einem 64-Bit-Betriebssystem erfolgt. Dieser Parameter ist optional. Das Argument *use32bitruntime* ist vom Typ **bit**.  
  
 [ @environment_scope = ] *environment_scope*  
 Gibt die Umgebungsverweise an, die bei der Überprüfung beachtet werden. Wenn der Wert `A` ist, werden alle dem Projekt zugeordneten Umgebungsverweise in die Überprüfung eingeschlossen. Wenn der Wert `S` ist, wird nur ein einzelner Umgebungsverweis eingeschlossen. Wenn der Wert `D` ist, werden keine Umgebungsverweise eingeschlossen, und jeder Parameter muss für eine erfolgreiche Überprüfung über einen Standardliteralwert verfügen. Dieser Parameter ist optional. Standardmäßig wird das Zeichen `D` verwendet. Das Argument *environment_scope* ist vom Typ **Char(1)**.  
  
 [ @reference_id = ] *reference_id*  
 Die eindeutige ID des Umgebungsverweises. Dieser Parameter ist nur erforderlich, wenn *environment_scope* den Wert `S` aufweist und daher nur ein einzelner Umgebungsverweis in die Überprüfung eingeschlossen wird. Der *reference_id* ist **bigint**.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 0 (Erfolg)  
  
## <a name="result-sets"></a>Resultsets  
 Keine  
  
## <a name="permissions"></a>Berechtigungen  
 Diese gespeicherte Prozedur erfordert eine der folgenden Berechtigungen:  
  
-   READ-Berechtigungen für das Projekt und ggf. READ-Berechtigungen für die Umgebungen, auf die verwiesen wird  
  
-   Mitgliedschaft in der Datenbankrolle **ssis_admin**  
  
-   Mitgliedschaft in der Serverrolle **sysadmin**  
  
## <a name="errors-and-warnings"></a>Fehler und Warnungen  
 In der folgenden Liste werden einige Bedingungen beschrieben, die möglicherweise einen Fehler oder eine Warnung auslösen:  
  
-   Der Projekt- oder Paketname ist nicht gültig.  
  
-   Der Benutzer verfügt nicht über die entsprechenden Berechtigungen.  
  
-   Eine oder mehrere der in die Überprüfung eingeschlossenen Umgebungen, auf die verwiesen wird, enthalten keine Variablen, auf die verwiesen wird.  
  
-   Bei der Überprüfung des Pakets tritt ein Fehler auf.  
  
-   Die Umgebung, auf die verwiesen wird, ist nicht vorhanden.  
  
-   In den in der Überprüfung enthaltenen Umgebungen, auf die verwiesen wird, können Variablen nicht gefunden werden, auf die verwiesen wird.  
  
-   In den Paketparametern wird auf Variablen verwiesen, aber in der Überprüfung wurden keine Umgebungen eingeschlossen, auf die verwiesen wird.  
  
## <a name="remarks"></a>Bemerkungen  
 Die Überprüfung vereinfacht das Identifizieren von Problemen, die das erfolgreiche Ausführen des Pakets verhindern. Verwenden Sie die [catalog.validations](../../integration-services/system-views/catalog-validations-ssisdb-database.md)- oder [catalog.operations](../../integration-services/system-views/catalog-operations-ssisdb-database.md)-Sicht, um den Überprüfungszustand zu überwachen.  
  
  
