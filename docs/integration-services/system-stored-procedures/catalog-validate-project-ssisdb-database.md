---
description: catalog.validate_project (SSISDB-Datenbank)
title: catalog.validate_project (SSISDB-Datenbank) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 5270689a-46d4-4847-b41f-3bed1899e955
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f57af2d5c29af5d3fd904c04637c0ba585b107a0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355296"
---
# <a name="catalogvalidate_project-ssisdb-database"></a>catalog.validate_project (SSISDB-Datenbank)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Überprüft asynchron ein Projekt im [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Katalog.  
  
## <a name="syntax"></a>Syntax  
  
```sql
catalog.validate_project [ @folder_name = ] folder_name  
    , [ @project_name = ] project_name  
    , [ @validate_type = ] validate_type  
    , [ @validation_id = ] validation_id OUTPUT  
 [  , [ @use32bitruntime = ] use32bitruntime ]  
 [  , [ @environment_scope = ] environment_scope ]  
 [  , [ @reference_id = ] reference_id ]  
```  
  
## <a name="arguments"></a>Argumente  
 [ @folder_name = ] *folder_name*  
 Der Name eines Ordners, der das Projekt enthält. Der *folder_name* ist **nvarchar(128)** .  
  
 [ @project_name = ] *project_name*  
 Der Name des Projekts. Der *project_name* ist **nvarchar(128)** .  
  
 [ @validate_type = ] *validate_type*  
 Gibt den Typ der auszuführenden Überprüfung an. Führen Sie mithilfe des Zeichens `F` eine vollständige Überprüfung aus. Dieser Parameter ist optional. Das Zeichen `F` wird standardmäßig verwendet. Das Argument *value_type* ist vom Typ **char(1)**.  
  
 [ @validation_id = ] *validation_id*  
 Gibt den eindeutigen Bezeichner (ID) der Überprüfung zurück. Das Argument *validation_id* ist vom Typ **bigint**.  
  
 [ @use32bitruntime = ] *use32bitruntime*  
 Gibt an, ob die 32-Bit-Laufzeit verwendet werden soll, um das Paket unter einem 64-Bit-Betriebssystem auszuführen. Verwenden Sie den Wert `1`, um das Paket mit der 32-Bit-Runtime auszuführen, wenn die Ausführung unter einem 64-Bit-Betriebssystem erfolgt. Verwenden Sie den Wert `0`, um das Paket mit der 64-Bit-Laufzeit auszuführen, wenn die Ausführung unter einem 64-Bit-Betriebssystem erfolgt. Dieser Parameter ist optional. Das Argument *use32bitruntime* ist vom Typ **bit**.  
  
 [ @environment_scope = ] *environment_scope*  
 Gibt die Umgebungsverweise an, die bei der Überprüfung beachtet werden. Wenn der Wert `A` ist, werden alle dem Projekt zugeordneten Umgebungsverweise in die Überprüfung eingeschlossen. Wenn der Wert `S` ist, wird nur ein einzelner Umgebungsverweis eingeschlossen. Wenn der Wert `D` ist, werden keine Umgebungsverweise eingeschlossen, und jeder Parameter muss für eine erfolgreiche Überprüfung über einen Standardliteralwert verfügen. Dieser Parameter ist optional. Das Zeichen `D` wird standardmäßig verwendet. Das Argument *environment_scope* ist vom Typ **Char(1)**.  
  
 [ @reference_id = ] *reference_id*  
 Die eindeutige ID des Umgebungsverweises. Dieser Parameter ist nur erforderlich, wenn *environment_scope* den Wert `S` aufweist und daher nur ein einzelner Umgebungsverweis in die Überprüfung eingeschlossen wird. Der *reference_id* ist **bigint**.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 0 (Erfolg)  
  
## <a name="result-sets"></a>Resultsets  
 Die Ausgabe der Überprüfungsschritte wird als unterschiedliche Abschnitte des Resultsets zurückgegeben.  
  
## <a name="permissions"></a>Berechtigungen  
 Diese gespeicherte Prozedur erfordert eine der folgenden Berechtigungen:  
  
-   READ-Berechtigungen für das Projekt und ggf. READ-Berechtigungen für die Umgebungen, auf die verwiesen wird  
  
-   Mitgliedschaft in der Datenbankrolle **ssis_admin**  
  
-   Mitgliedschaft in der Serverrolle **sysadmin**  
  
## <a name="errors-and-warnings"></a>Fehler und Warnungen  
 In der folgenden Liste werden einige Bedingungen angegeben, die möglicherweise einen Fehler oder eine Warnung auslösen:  
  
-   Die Überprüfung schlägt für ein oder mehrere Pakete im Projekt fehl.  
  
-   Die Überprüfung schlägt fehl, wenn eine oder mehrere der in die Überprüfung eingeschlossenen Umgebungen, auf die verwiesen wird, keine Variablen enthalten, auf die verwiesen wird.  
  
-   Der angegebene Überprüfungstyp ist ungültig.  
  
-   Der Projektname oder die Umgebungsverweis-ID ist ungültig.  
  
-   Der Benutzer verfügt nicht über die entsprechenden Berechtigungen.  
  
## <a name="remarks"></a>Bemerkungen  
 Die Überprüfung erleichtert das Identifizieren von Problemen, die das erfolgreiche Ausführen der Pakete im Projekt verhindern. Verwenden Sie die [catalog.validations](../../integration-services/system-views/catalog-validations-ssisdb-database.md)- oder [catalog.operations](../../integration-services/system-views/catalog-operations-ssisdb-database.md)-Sicht, um den Überprüfungszustand zu überwachen.  
  
 In der Überprüfung können nur Umgebungen verwendet werden, auf die vom Benutzer zugegriffen werden kann. Die Ausgabe der Überprüfung wird als Resultset an den Client gesendet.  
  
 In dieser Version unterstützt die Projektüberprüfung keine Abhängigkeitsüberprüfung.  
  
 Eine vollständige Überprüfung bestätigt, dass alle Umgebungsvariablen, auf die verwiesen wird, in den in die Überprüfung eingeschlossenen Umgebungen, auf die verwiesen wird, vorhanden sind. In den Ergebnissen der vollständigen Überprüfung werden Umgebungsverweise aufgeführt, die ungültig sind, sowie Umgebungsvariablen, auf die verwiesen wird und die in keiner der in die Überprüfung eingeschlossenen Umgebungen, auf die verwiesen wird, gefunden werden konnten.  
  
  
