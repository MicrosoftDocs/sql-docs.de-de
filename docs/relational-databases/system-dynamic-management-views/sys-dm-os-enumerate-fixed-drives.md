---
description: sys. dm_os_enumerate_fixed_drives (Transact-SQL)
title: sys. dm_os_enumerate_fixed_drives (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 09/18/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_os_enumerate_fixed_drives
- sys.dm_os_enumerate_fixed_drives_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_enumerate_fixed_drives dynamic management view
ms.assetid: 2e27489e-cf69-4a89-9036-77723ac3de66
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c35d783b8db1abe5803a34dd1a4c401444897207
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89539310"
---
# <a name="sysdm_os_enumerate_fixed_drives-transact-sql"></a>sys. dm_os_enumerate_fixed_drives (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Eingeführt in SQL Server 2019.

Listet Volumes auf, die auf Laufwerk Buchstaben wie bereitgestellt werden `C:\` .

|Spaltenname|Datentyp|BESCHREIBUNG|
|-----------------|---------------|-----------------|  
|`fixed_drive_path`|`nvarchar(512)`|Der Pfad zum Volume, wie z `C:\` . b..|  
|`drive_type`|`int`|Code für den Laufwerkstyp. Siehe [ `GetDriveTypeW` Function](/windows/win32/api/fileapi/nf-fileapi-getdrivetypew).|
|`drive_type_desc`|`nvarchar(512)`|Die Beschreibung des Laufwerks Typs. Siehe [ `GetDriveTypeW` Function](/windows/win32/api/fileapi/nf-fileapi-getdrivetypew).|
|`free_space_in_bytes`|`bigint`|Freier Speicherplatz auf dem Datenträger in Byte.|

## <a name="permissions"></a>Berechtigungen

Der Benutzer muss `VIEW SERVER STATE` über die-Berechtigung auf dem Server verfügen.

## <a name="see-also"></a>Weitere Informationen  

 [Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Mit e/a verbundene dynamische Verwaltungs Sichten und Funktionen &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/i-o-related-dynamic-management-views-and-functions-transact-sql.md)  
