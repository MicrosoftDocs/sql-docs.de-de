---
description: MSSQLSERVER_5242
title: MSSQLSERVER_5242 | Microsoft-Dokumentation
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 5242 (Database Engine error)
ms.assetid: 712b1a10-2f87-41df-a111-1ed9f14102d4
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 58ac27fac4af3c8d343102403549758ea428d494
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198130"
---
# <a name="mssqlserver_5242"></a>MSSQLSERVER_5242
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Details  
  
| attribute | Wert |  
| :-------- | :---- |  
|Produktname|SQL Server|  
|Ereignis-ID|5242|  
|Ereignisquelle|MSSQLSERVER|  
|Komponente|SQLEngine|  
|Symbolischer Name||  
|Meldungstext|Bei einem internen Vorgang wurde eine Inkonsistenz in der '%.*ls'-Datenbank (ID: %d) auf Seite %S_PGID gefunden. Wenden Sie sich an den technischen Support. Referenznummer %ld.|  
  
## <a name="explanation"></a>Erklärung  
Auf der Datenbankseite, die in der Fehlermeldung angegeben wird, ist eine strukturelle Inkonsistenz aufgetreten.  
  
## <a name="see-also"></a>Weitere Informationen  
[DBCC CHECKDB &#40;Transact-SQL&#41;](~/t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)  
[Datenbankdateien und Dateigruppen](~/relational-databases/databases/database-files-and-filegroups.md)  
  
