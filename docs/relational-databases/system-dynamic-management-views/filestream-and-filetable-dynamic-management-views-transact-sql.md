---
description: Dynamische Verwaltungssichten für Filestream und FileTable (Transact-SQL)
title: Dynamische Verwaltungs Sichten für FILESTREAM und FILETABLE (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- FileTables [SQL Server], dynamic management views
ms.assetid: e50a135d-6644-42a4-a0df-1c7a2b722051
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 3291b20888ba330496c6cf3d5784520ce02bc906
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99146110"
---
# <a name="filestream-and-filetable-dynamic-management-views-transact-sql"></a>Dynamische Verwaltungssichten für Filestream und FileTable (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  In diesem Abschnitt werden die dynamischen Verwaltungssichten im Zusammenhang mit den FILESTREAM- und FileTable-Funktionen beschrieben.  
  
## <a name="filestream-dynamic-management-views-and-functions"></a>Filestream: Dynamische Verwaltungssichten und Funktionen  
 [sys.dm_filestream_file_io_handles &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-filestream-file-io-handles-transact-sql.md)  
 Zeigt die aktuell geöffneten Transaktionsdateihandles an.  
  
 [sys.dm_filestream_file_io_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-filestream-file-io-requests-transact-sql.md)  
 Zeigt aktuelle Dateieingabe- und Dateiausgabeanforderungen an.  
  
## <a name="filetable-dynamic-management-views-and-functions"></a>FileTable: Dynamische Verwaltungssichten und Funktionen  
 [sys.dm_filestream_non_transacted_handles &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-filestream-non-transacted-handles-transact-sql.md)  
 Zeigt die derzeit geöffneten nicht transaktionsgebundenen Dateihandles für FileTable-Daten an.  

## <a name="see-also"></a>Weitere Informationen
[Filestream](../../relational-databases/blob/filestream-sql-server.md)
<br>[Filetables](../../relational-databases/blob/filetables-sql-server.md)
<br>[Katalogsichten für Filestream und FileTable (Transact-SQL)](../system-catalog-views/filestream-and-filetable-catalog-views-transact-sql.md)
<br>[Gespeicherte Systemprozeduren für Filestream und FileTable (Transact-SQL)](../system-stored-procedures/filestream-and-filetable-system-stored-procedures.md)
  
