---
description: DROP EXTERNAL LIBRARY (Transact-SQL)
title: DROP EXTERNAL LIBRARY (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/26/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: machine-learning
ms.topic: reference
f1_keywords:
- DROP EXTERNAL LIBRARY
- DROP_EXTERNAL_LIBRARY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- DROP EXTERNAL LIBRARY
author: dphansen
ms.author: davidph
manager: cgronlund
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 3b7e6509cabb65a9e6dd09a6ccbe76f33bff5bc7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346597"
---
# <a name="drop-external-library-transact-sql"></a>DROP EXTERNAL LIBRARY (Transact-SQL)  
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

Löscht eine vorhandene Paketbibliothek. Paketbibliotheken werden von unterstützten externen Runtimes wie R, Python oder Java verwendet.

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
> [!NOTE]
> In SQL Server 2017 werden die R-Sprache und die Windows-Plattform unterstützt. R, Python und Java werden für die Windows- und die Linux-Plattform in SQL Server 2019 und höher unterstützt.
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
> [!NOTE]
> In Azure SQL Managed Instance werden die Sprachen R und Python unterstützt.
::: moniker-end

## <a name="syntax"></a>Syntax

```syntaxsql
DROP EXTERNAL LIBRARY library_name
[ AUTHORIZATION owner_name ];
```

### <a name="arguments"></a>Argumente

**library_name**

Gibt den Namen einer vorhandenen Paketbibliothek an.

Bibliotheken umfassen nur den Benutzer. Bibliotheksnamen müssen innerhalb des Kontexts eines bestimmten Benutzers oder Besitzers eindeutig sein.

**owner_name**

Gibt den Namen eines Benutzers oder einer Rolle an, der oder die die externe Bibliothek besitzt.

Datenbankbesitzer können Bibliotheken löschen, die von anderen Benutzern erstellt wurden.

## <a name="permissions"></a>Berechtigungen

Zum Löschen einer Bibliothek ist die Berechtigung ALTER ANY EXTERNAL LIBRARY erforderlich. Standardmäßig kann jeder Datenbankbesitzer oder der Besitzer des Objekts auch eine externe Bibliothek löschen.

### <a name="return-values"></a>Rückgabewerte

Eine Informationsmeldung wird zurückgegeben, wenn die Anweisung erfolgreich war.

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Bemerkungen

Im Gegensatz zu anderen `DROP`-Anweisungen in SQL Server unterstützt diese Anweisung das Angeben einer optionalen Autorisierungsklausel. Dies ermöglicht einem **dbo** oder Benutzer mit der Rolle **db_owner**, eine Paketbibliothek zu löschen, die von einem normalen Benutzer in die Datenbank hochgeladen wurde.

In einer SQL-Instanz sind einige Pakete vorinstalliert. Diese werden als *Systempakete* bezeichnet. Systempakete können vom Benutzer weder hinzugefügt noch aktualisiert oder entfernt werden.

## <a name="examples"></a>Beispiele

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
Hinzufügen des benutzerdefinierten R-Pakets `customPackage` zu einer Datenbank:

```sql
CREATE EXTERNAL LIBRARY customPackage 
FROM (CONTENT = 'C:\temp\customPackage_v1.1.zip')
WITH (LANGUAGE = 'R');
GO
```
::: moniker-end

Löschen der `customPackage`-Bibliothek:

```sql
DROP EXTERNAL LIBRARY customPackage;
```

## <a name="see-also"></a>Weitere Informationen

[CREATE EXTERNAL LIBRARY (Transact-SQL)](create-external-library-transact-sql.md)  
[ALTER EXTERNAL LIBRARY (Transact-SQL)](alter-external-library-transact-sql.md)  
[sys.external_library_files](../../relational-databases/system-catalog-views/sys-external-library-files-transact-sql.md)  
[sys.external_libraries](../../relational-databases/system-catalog-views/sys-external-libraries-transact-sql.md)  
