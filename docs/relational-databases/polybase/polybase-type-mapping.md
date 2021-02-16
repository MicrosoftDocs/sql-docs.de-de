---
title: Typzuordnung mit PolyBase | Microsoft-Dokumentation
description: In den Tabellen in diesem Artikel wird die Zuordnung zwischen externen PolyBase-Datenquellen und SQL Server beschrieben. Definieren Sie externe Tabellen mit dem Transact-SQL-Befehl CREATE EXTERNAL TABLE.
ms.date: 09/24/2018
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
ms.openlocfilehash: ddb0c0ac0c1afd4e4a3f94504256fa787dceaf9d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080002"
---
# <a name="type-mapping-with-polybase"></a>Typzuordnung mit PolyBase

[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Dieser Artikel beschreibt die Zuordnung zwischen externen PolyBase-Datenquellen und SQL Server. Sie können diese Informationen verwenden, um externe Tabellen mit dem Transact-SQL-Befehl [CREATE EXTERNAL TABLE](../../t-sql/statements/create-external-table-transact-sql.md) richtig zu definieren.

## <a name="overview"></a>Übersicht

Beim Erstellen einer externen Tabelle mit PolyBase müssen die Spaltendefinitionen, einschließlich der Datentypen und die Anzahl der Spalten mit den Daten in externen Dateien übereinstimmen. Wenn ein Konflikt besteht, werden die Zeilen der Datei beim Abfragen der tatsächlichen Daten zurückgewiesen.  
  
Für externe Tabellen, die auf Dateien in externen Datenquellen verweisen, müssen die Spalten- und Typdefinitionen dem genauen Schema der externen Datei zugeordnet werden. Beim Definieren von Datentypen, die auf Daten verweisen, die in Hadoop/Hive gespeichert sind, verwenden Sie die folgenden Zuordnungen zwischen Datentypen von SQL und Hive und wandeln Sie den Typ in einen SQL-Datentyp um, wenn Sie daraus auswählen. Die Typen umfassen alle Versionen von Hive, sofern nicht anders angegeben.

> [!NOTE]  
> SQL Server unterstützt den Datenwert *unendlich* von Hive nicht in beliebigen Konvertierungen. PolyBase schlägt mit einem Datentypkonvertierungsfehler fehl.

## <a name="hadoop-type-mapping-reference"></a>Hadoop-Typzuordnungsverweis

| SQL-Datentyp | .NET-Datentyp            | Hive-Datentyp | Hadoop/Java-Datentyp | Kommentare                       |
| ------------- | ------------------------- | -------------- | --------------------- | ------------------------------ |
| TINYINT       | Byte                      | TINYINT        | ByteWritable          | Nur für unsignierte Zahlen.     |
| SMALLINT      | Int16                     | SMALLINT       | ShortWritable         |
| INT           | Int32                     | INT            | IntWritable           |
| BIGINT        | Int64                     | BIGINT         | LongWritable          |
| bit           | Boolean                   | boolean        | BooleanWritable       |
| float         | Double                    | double         | DoubleWritable        |
| real          | Single                    | float          | FloatWritable         |
| money         | Decimal                   | double         | DoubleWritable        |
| SMALLMONEY    | Decimal                   | double         | DoubleWritable        |
| NCHAR         | String<br /><br /> Char[] | Zeichenfolge         | Varchar               |
| NVARCHAR      | String<br /><br /> Char[] | Zeichenfolge         | Varchar               |
| char          | String<br /><br /> Char[] | Zeichenfolge         | Varchar               |
| varchar       | String<br /><br /> Char[] | Zeichenfolge         | Varchar               |
| BINARY        | Byte[]                    | BINARY         | BytesWritable         | Gilt für Hive 0.8 und höher. |
| varbinary     | Byte[]                    | BINARY         | BytesWritable         | Gilt für Hive 0.8 und höher. |
| date          | Datetime                  | timestamp      | TimestampWritable     |
| smalldatetime | Datetime                  | timestamp      | TimestampWritable     |
| datetime2     | Datetime                  | timestamp      | TimestampWritable     |
| datetime      | Datetime                  | timestamp      | TimestampWritable     |
| time          | TimeSpan                  | timestamp      | TimestampWritable     |
| Decimal       | Decimal                   | Decimal        | BigDecimalWritable    | Gilt für Hive 0.11 und höher. |

<!--SQL Server 2019-->
::: moniker range=">= sql-server-ver15 "

## <a name="oracle-type-mapping-reference"></a>Oracle-Typzuordnungsverweis

| Oracle-Datentyp | SQL Server-Typ | 
| -------------    | --------------- |
|Float             |Float            |
|NUMBER            |Float            |
|NUMBER (p,s)      |Decimal (p, s)   |
|LONG              |nvarchar         |
|BINARY_FLOAT      |Real             | 
|BINARY_DOUBLE     |Float            | 
|CHAR              |Char             |
|VARCHAR2          |Varchar          | 
|NVARCHAR2         |nvarchar         | 
|RAW               |Varbinary        |
|LONG RAW          |Varbinary        | 
|BLOB              |Varbinary        |
|CLOB              |Varchar          |
|NCLOB             | nvarchar        | 
|ROWID             |Varchar          |
|UROWID            |Varchar          | 
|DATE              |Datetime2        |
|timestamp         |Datetime2        | 

**Typenkonflikt:** 

**Gleitkomma:** Oracle unterstützt eine Gleitkommagenauigkeit von 126, was niedriger ist als die von SQL-Server (53). Daher kann **Float (1-53)** direkt zugeordnet werden, aber darüber hinaus kommt es zu Datenverlust durch Abschneidung.

**Zeitstempel:** „Timestamp“ und „Timestamp with local timezone“ in Oracle unterstützen eine Genauigkeit in Sekundenbruchteilen von 9, während SQL Server bei „DateTime2“ nur eine Genauigkeit in Sekundenbruchteilen von 7 unterstützt. 




## <a name="mongodb-type-mapping"></a>MongoDB-Typzuordnung

| BSON-Datentyp     | SQL Server-Typ |
| ------------------ | --------------- |
| Double             | Float           |
| Zeichenfolge             | nvarchar        |
| Binärdaten        | nvarchar        |
| ObjectID          | nvarchar        |
| Boolean            | bit             |
| Date               | Datetime2       |
| 32-bit integer     | Int             |
| Timestamp          | nvarchar        |
| 64-Bit-Integer     | BigInt          |
|Decimal 128         | Decimal         | 
| DBPointer          | nvarchar        |
| JavaScript         | nvarchar        |
| Max Key            | nvarchar        |
| Min Key            | nvarchar        |
| Symbol             | nvarchar        |
| Regular Expression | nvarchar        |
| Undefined/NULL     | nvarchar        |


MongoDB verwendet BSON-Dokumente zur Speicherung von Datensätzen. Im Gegensatz zu den vorherigen Szenarios weist BSON kein Schema auf und unterstützt das Einbetten von Dokumenten und Arrays in andere Dokumente. Dadurch ist der Benutzer flexibler. 


## <a name="teradata-type-mapping-reference"></a>Teradata-Typzuordnungsverweis

| Teradata-Datentyp | SQL Server-Typ | 
| -------------      | -------------   |
|INTEGER             |Int              |
|SMALLINT            |SmallInt         |
|bigint              |BigInt           |
|BYTEINT             |SmallInt         |
|DECIMAL             |Decimal          |
|GLEITKOMMAZAHL               |Decimal          |
|BYTE                |Binary           |
|VARBYTE             |Varbinary        |
|BLOB                |varbinary        |
|CHAR                |Nchar            |
|CLOB                |nvarchar         |
|VARCHAR             |nvarchar         |
|Graphic             |Nchar            |
|JSON                |nvarchar         |
|VARGRAPHIC          |nvarchar         |
|DATE                |Date             |
|timestamp           |Datetime2        |
|TIME                |Time             |
|TIME WITH TIME ZONE |Time             |
|TIMESTAMP WITH TIME ZONE|Zeit         |

::: moniker-end

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Verwendung finden Sie im Transact-SQL-Referenzartikel für [CREATE EXTERNAL TABLE](../../t-sql/statements/create-external-table-transact-sql.md).
