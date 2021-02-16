---
description: catalog.deny_permission (SSISDB-Datenbank)
title: catalog.deny_permission (SSISDB-Datenbank) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: de310bac-2ddc-4ef9-8783-43dcb02a94f1
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d32af051552c211592bcc4deb2bcd2c1c1c20079
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342258"
---
# <a name="catalogdeny_permission-ssisdb-database"></a>catalog.deny_permission (SSISDB-Datenbank)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Verweigert eine Berechtigung für ein sicherungsfähiges Objekt im [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Katalog.  
  
## <a name="syntax"></a>Syntax  
  
```sql
catalog.deny_permission [ @object_type = ] object_type  
    , [ @object_id = ] object_id  
    , [ @principal_id = ] principal_id  
    , [ @permission_type = ] permission_type  
```  
  
## <a name="arguments"></a>Argumente  
 [ @object_type = ] *object_type*  
 Der Typ des sicherungsfähigen Objekts. Die Typen von sicherungsfähigen Objekten umfassen Ordner (`1`), Projekt (`2`), Umgebung (`3`) und Vorgang (`4`). Das Argument *object_type* ist vom Typ **smallint**_._  
  
 [ @object_id = ] *object_id*  
 Der eindeutige Bezeichner (ID) oder der Primärschlüssel des sicherungsfähigen Objekts. Das Argument *object_id* ist vom Typ **bigint**.  
  
 [ @principal_id = ] *principal_id*  
 Die ID des Prinzipals, der verweigert werden soll. Das Argument *principal_id* ist vom Typ **int**.  
  
 [ @permission_type = ] *permission_type*  
 Der Typ der Berechtigung, die verweigert werden soll. Das Argument *permission_type* ist vom Typ **smallint**.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 0 (Erfolg)  
  
 1 („object_class“ ist ungültig)  
  
 2 („object_id“ ist nicht vorhanden)  
  
 3 („principal“ ist nicht vorhanden)  
  
 4 („permission“ ist ungültig)  
  
 5 (anderer Fehler)  
  
## <a name="result-sets"></a>Resultsets  
 Keine  
  
## <a name="permissions"></a>Berechtigungen  
 Diese gespeicherte Prozedur erfordert eine der folgenden Berechtigungen:  
  
-   MANAGE_PERMISSIONS-Berechtigung für das Objekt  
  
-   Mitgliedschaft in der Datenbankrolle **ssis_admin**  
  
-   Mitgliedschaft in der Serverrolle **sysadmin**  
  
## <a name="remarks"></a>Bemerkungen  
 Mit dieser gespeicherten Prozedur können Sie die in der folgenden Tabelle beschriebenen Typen von Berechtigungen verweigern:  
  
|permission_type-Wert|Berechtigungsname|Berechtigungsbeschreibung|Anwendbare Objekttypen|  
|----------------------------|---------------------|----------------------------|-----------------------------|  
|`1`|READ|Ermöglicht es dem Prinzipal, Informationen zu lesen, die als Teil des Objekts angesehen werden, z. B. Eigenschaften. Ermöglicht es dem Prinzipal nicht, den Inhalt von anderen Objekten innerhalb des Objekts aufzuzählen oder zu lesen.|Ordner, Projekt, Umgebung, Vorgang|  
|`2`|MODIFY|Ermöglicht es dem Prinzipal, Informationen zu ändern, die als Teil des Objekts angesehen werden, z. B. Eigenschaften. Ermöglicht es dem Prinzipal nicht, andere Objekte innerhalb des Objekts zu ändern.|Ordner, Projekt, Umgebung, Vorgang|  
|`3`|Führen Sie|Ermöglicht es dem Prinzipal, alle Pakete im Projekt auszuführen.|Projekt|  
|`4`|MANAGE_PERMISSIONS|Ermöglicht es dem Prinzipal, den Objekten Berechtigungen zuzuweisen.|Ordner, Projekt, Umgebung, Vorgang|  
|`100`|CREATE_OBJECTS|Ermöglicht es dem Prinzipal, Objekte im Ordner zu erstellen.|Ordner|  
|`101`|READ_OBJECTS|Ermöglicht es dem Prinzipal, alle Objekte im Ordner zu lesen.|Ordner|  
|`102`|MODIFY_OBJECTS|Ermöglicht es dem Prinzipal, alle Objekte im Ordner zu ändern.|Ordner|  
|`103`|EXECUTE_OBJECTS|Ermöglicht es dem Prinzipal, alle Pakete aus allen Projekten im Ordner auszuführen.|Ordner|  
|`104`|MANAGE_OBJECT_PERMISSIONS|Ermöglicht es dem Prinzipal, Berechtigungen für alle Objekte im Ordner zu verwalten.|Ordner|  
  
## <a name="errors-and-warnings"></a>Fehler und Warnungen  
 In der folgenden Liste werden einige Bedingungen beschrieben, die möglicherweise einen Fehler oder eine Warnung auslösen:  
  
-   Wenn „permission_type“ angegeben wird, verweigert die Prozedur die angegebene Berechtigung, die dem angegebenen Prinzipal für das angegebene Objekt explizit zugewiesen wurde. Auch wenn keine solchen Instanzen vorhanden sind, gibt die Prozedur dennoch einen Erfolgscodewert (`0`) zurück.  
  
-   Wenn „permission_type“ ausgelassen wird, verweigert die Prozedur alle Berechtigungen für den angegebenen Prinzipal des angegebenen Objekts.  
  
  
