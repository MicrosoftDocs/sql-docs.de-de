---
title: Konvertieren von Python- und SQL-Datentypen
description: Überprüfen Sie die implizite und explizite Konvertierung von Datentypen zwischen Python und SQL Server in Data Science- und Machine Learning-Lösungen.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/06/2020
ms.topic: conceptual
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 99a677874a29b3784ed36f29876a332add21c920
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344431"
---
# <a name="data-type-mappings-between-python-and-sql-server"></a>Zuordnungen von Datentypen zwischen Python und SQL Server
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

In diesem Artikel werden die unterstützten Datentypen aufgelistet und die Datentypkonvertierungen ausgeführt, die für die Verwendung des Features zur Python-Integration in SQL Server Machine Learning Services gelten.

Verglichen mit SQL Server unterstützt Python nur eine begrenzte Anzahl von Datentypen. Daher ist es möglich, dass SQL-Daten aus SQL Server bei der Verwendung in Python-Skripts implizit in einen kompatiblen Python-Datentyp konvertiert werden. Häufig kann jedoch keine exakte automatische Konvertierung durchgeführt werden, dann wird ein Fehler zurückgegeben.

## <a name="python-and-sql-data-types"></a>Python- und SQL-Datentypen

In der folgenden Tabelle werden die impliziten Konvertierungen aufgeführt, die bereitgestellt werden. Andere Datentypen werden nicht unterstützt.

| SQL-Typ             | Python-Typ | BESCHREIBUNG |
|----------------------|-------------|-------------|
| **bigint**           | `float64`   |
| **binary**           | `bytes`     |
| **bit**              | `bool`      |
| **char**             | `str`       |
| **date**             | `datetime`  |
| **datetime**         |`datetime`   | Dieser Typ wird in SQL Server 2017 CU6 und höher (mit **NumPy**-Arrays vom Typ `datetime.datetime` oder **Pandas** `pandas.Timestamp`) unterstützt. `sp_execute_external_script` unterstützt jetzt `datetime`-Typen mit Sekundenbruchteilen.|
| **float**            | `float64`   |
| **nchar**            | `str`       |
| **nvarchar**         | `str`       |
| **nvarchar(max)**    | `str`       |
| **real**             | `float64`   |
| **smalldatetime**    | `datetime`  |
| **smallint**         | `int32`     |
| **tinyint**          | `int32`     |
| **uniqueidentifier** | `str`       |
| **varbinary**        | `bytes`     |
| **varbinary(max)**   | `bytes`     |
| **varchar(n)**       | `str`       |
| **varchar(max)**     | `str`       |

## <a name="see-also"></a>Weitere Informationen

+ [Zuordnungen von Datentypen zwischen R und SQL Server](../r/r-libraries-and-data-types.md)
