---
description: catalog.projects (SSISDB-Datenbank)
title: catalog.projects (SSISDB-Datenbank) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: a6b595e1-5227-47ce-8ee2-a28c1e1d5645
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 1d23141832f90f458d484266468b7f76ffd29f75
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835190"
---
# <a name="catalogprojects-ssisdb-database"></a>catalog.projects (SSISDB-Datenbank)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  Zeigt die Details für alle Projekte an, die im **SSISDB** -Katalog angezeigt werden.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|project_id|**bigint**|Der eindeutige Bezeichner (ID) des Projekts.|  
|folder_id|**bigint**|Die eindeutige ID des Ordners, in dem sich das Projekt befindet.|  
|name|**sysname**|Der Name des Projekts.|  
|description|**nvarchar(1024)**|Die optionale Beschreibung des Projekts.|  
|project_format_version|**int**|Die Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], mit der das Projekt entwickelt wurde.|  
|deployed_by_sid|**varbinary(85)**|Die Sicherheits-ID (SID) des Benutzers, der das Projekt installiert hat.|  
|deployed_by_sid|**nvarchar(128)**|Der Name des Benutzers, der das Projekt installiert hat.|  
|last_deployed_time|**datetimeoffset(7)**|Datum und Uhrzeit der Bereitstellung oder erneuten Bereitstellung des Projekts.|  
|created_time|**datetimeoffset(7)**|Datum und Uhrzeit der Projekterstellung.|  
|object_version_lsn|**bigint**|Die Version des Projekts. Diese Nummer ist nicht unbedingt eine fortlaufende Nummer.|  
|validation_status|**char(1)**|Der Überprüfungsstatus.|  
|last_validation_time|**datetimeoffset(7)**|Der Zeitpunkt der letzten Überprüfung.|  
  
## <a name="remarks"></a>Hinweise  
 In dieser Sicht wird eine Zeile für jedes Projekt im Katalog angezeigt.  
  
## <a name="permissions"></a>Berechtigungen  
 Diese Sicht erfordert eine der folgenden Berechtigungen:  
  
-   READ-Berechtigung für das Projekt  
  
-   Mitgliedschaft in der Datenbankrolle **ssis_admin**  
  
-   Mitgliedschaft in der Serverrolle **sysadmin**  
  
> [!NOTE]  
>  Wenn Sie über die READ-Berechtigung für ein Projekt verfügen, verfügen Sie auch über die READ-Berechtigung für alle Pakete und Umgebungsverweise, die diesem Projekt zugeordnet sind. Sicherheit auf Zeilenebene wird erzwungen. Es werden nur Zeilen angezeigt, zu deren Anzeige Sie berechtigt sind.  
  
  
