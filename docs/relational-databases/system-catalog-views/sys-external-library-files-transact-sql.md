---
description: sys.external_library_files (Transact-SQL)
title: sys.external_library_files (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/25/2020
ms.prod: sql
ms.technology: machine-learning
ms.topic: reference
f1_keywords:
- external_library_files
- external_library_files_TSQL
- sys.external_library_files
- sys.external_library_files_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.external_library_files catalog view
author: dphansen
ms.author: davidph
manager: cgronlun
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: b716b8a3264152c3f03a454f22613f56241279d1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351745"
---
# <a name="sysexternal_library_files-transact-sql"></a>sys.external_library_files (Transact-SQL)  
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

Listet eine Zeile für jede Datei auf, die eine externe Bibliothek bildet.

|Spaltenname |Datentyp |BESCHREIBUNG|
|------|------|-----|
|external_library_id | INT |ID des externen Bibliotheks Objekts. |
|Inhalt |varbinary(max) |Inhalt des externen Bibliotheksdatei Artefakts. |
|platform |TINYINT |ID der Host Plattform, auf der SQL Server installiert ist. |
|platform_desc | nvarchar(60) |Der Name der Host Plattform. Gültige Werte sind "Windows", "Linux". |

### <a name="see-also"></a>Weitere Informationen  

[sys.external_libraries](sys-external-libraries-transact-sql.md)  
[externe Bibliothek erstellen](../../t-sql/statements/create-external-library-transact-sql.md)  
