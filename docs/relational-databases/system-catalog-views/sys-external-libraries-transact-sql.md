---
description: sys.external_libraries (Transact-SQL)
title: sys.external_libraries (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/25/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: machine-learning
ms.topic: reference
f1_keywords:
- external_libraries
- external_libraries_TSQL
- sys.external_libraries
- sys.external_libraries_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.external_libraries catalog view
author: dphansen
ms.author: davidph
manager: cgronlun
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 6370cfdd213c7670b22066395534928f980766c3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354199"
---
# <a name="sysexternal_libraries-transact-sql"></a>sys.external_libraries (Transact-SQL)  
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

Unterstützt die Verwaltung von Paket Bibliotheken im Zusammenhang mit externen Laufzeiten, wie z. b. R, Python und Java.

> [!NOTE]
> In SQL Server 2017 werden die R-Sprache und die Windows-Plattform unterstützt. R, Python und Java werden für die Windows- und die Linux-Plattform in SQL Server 2019 und höher unterstützt. In Azure SQL verwaltete Instanz werden R und python unterstützt.

## <a name="sysexternal_libraries"></a>sys.external_libraries

In der Katalog Sicht sys.external_libraries wird eine Zeile für jede externe Bibliothek aufgelistet, die in die Datenbank hochgeladen wurde.

|Spaltenname |Datentyp | BESCHREIBUNG|
|------|------|------|
|external_library_id |INT | ID des externen Bibliotheks Objekts. |
|name |sysname |Der Name der externen Bibliothek. Ist innerhalb der Datenbank pro Besitzer eindeutig.|
|principal_id |INT |ID des Prinzipals, der diese externe Bibliothek besitzt. |
|language | sysname | Der Name der Sprache oder Laufzeit, die die externe Bibliothek unterstützt. Gültige Werte sind ' R ', ' python ' und ' Java '. Weitere Laufzeiten werden möglicherweise in der Zukunft hinzugefügt.|
|scope |INT |0 für öffentlichen Bereich; 1 für privaten Bereich |  
|scope_desc |varchar (7) |Gibt an, ob das Paket öffentlich oder privat ist.|

## <a name="see-also"></a>Weitere Informationen  

+ [sys.external_library_files](sys-external-library-files-transact-sql.md)  
+ [externe Bibliothek erstellen](../../t-sql/statements/create-external-library-transact-sql.md)  
+ [Installieren neuer R-Pakete](../../machine-learning/package-management/install-additional-r-packages-on-sql-server.md)  
+ [Installieren neuer Python-Pakete](../../machine-learning/package-management/install-additional-python-packages-on-sql-server.md)  
