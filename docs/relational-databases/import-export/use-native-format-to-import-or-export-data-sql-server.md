---
title: Verwenden des nativen Formats zum Importieren und Exportieren von Daten
description: Beim Importieren oder Exportieren in SQL Server enthält das native Format die nativen Datentypen einer Datenbank für die Hochgeschwindigkeitsübertragung von Daten zwischen SQL Server-Tabellen.
ms.date: 09/30/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- native data format [SQL Server]
- data formats [SQL Server], native
ms.assetid: eb279b2f-0f1f-428f-9b8f-2a7fc495b79f
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: 4a1802cc2bcd64141c35dcc1e15f4609848e7c79
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465551"
---
# <a name="use-native-format-to-import-or-export-data-sql-server"></a>Verwenden des nativen Formats zum Importieren oder Exportieren von Daten (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Das systemeigene Format wird für die Massenübertragung von Daten zwischen mehreren Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mithilfe einer Datendatei empfohlen, die keinen erweiterten oder Doppelbyte-Zeichensatz (Double-Byte Character Set, DBCS) enthält.  

> [!NOTE]
>  Für die Massenübertragung von Daten zwischen mehreren Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mithilfe einer Datendatei, die erweiterte Zeichen oder DBCS-Zeichen enthält, sollten Sie das systemeigene Unicode-Format verwenden. Weitere Informationen finden Sie unter [Verwenden des nativen Unicode-Formats zum Importieren oder Exportieren von Daten &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md).

Das systemeigene Format erhält die systemeigenen Datentypen einer Datenbank. Das systemeigene Format ist für die Hochgeschwindigkeitsübertragung von Daten zwischen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Tabellen konzipiert. Wenn Sie eine Formatdatei verwenden, müssen Quell- und Zieltabelle nicht identisch sein. Die Datenübertragung besteht aus zwei Schritten:  
  
1.  Massenexportieren der Daten aus einer Quelltabelle in eine Datendatei  
  
2.  Massenimportieren der Daten aus der Datendatei in die Zieltabelle  

Durch die Verwendung des systemeigenen Formats zwischen identischen Tabellen wird die unnötige Konvertierung von Datentypen in das und aus dem Zeichenformat vermieden und somit Zeit und Speicherplatz gespart. Um eine optimale Übertragungsrate zu erreichen, werden jedoch wenige Überprüfungen der Datenformatierung vorgenommen. Berücksichtigen Sie, um Probleme mit den geladenen Daten zu verhindern, die folgende Einschränkungsliste.  


## <a name="restrictions"></a>Beschränkungen
Um Daten im systemeigenen Format erfolgreich zu importieren, müssen folgende Punkte sichergestellt sein:  
  
-   Die Datendatei liegt im systemeigenen Format vor.  
  
-   Die Zieltabelle muss mit der Datendatei kompatibel sein (Spaltenanzahl, Datentyp, Länge, NULL-Status usw. müssen richtig sein), oder Sie müssen eine Formatdatei verwenden, um jedes Feld den entsprechenden Spalten zuzuordnen.  
  
    > [!NOTE]
    >  Wenn Sie Daten aus einer Datei importieren, die nicht mit der Zieltabelle übereinstimmt, kann der Importvorgang zwar erfolgreich sein, aber die in die Zieltabelle eingefügten Datenwerte sind wahrscheinlich falsch. Das liegt daran, dass die Daten aus der Datei mithilfe des Formats der Zieltabelle interpretiert werden. Daher hat jede fehlende Übereinstimmung die Einfügung falscher Werte zur Folge. Eine derartige fehlende Übereinstimmung kann jedoch unter keinen Umständen logische oder physische Inkonsistenzen in der Datenbank verursachen.  
  
     Weitere Informationen zur Verwendung von Formatdateien finden Sie unter [Formatdateien zum Importieren oder Exportieren von Daten &#40;SQL Server&#41;](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md).  
  
 Ein erfolgreicher Import beschädigt die Zieltabelle nicht.  
  
## <a name="how-bcp-handles-data-in-native-format"></a>Verarbeitung von Daten im nativen Format durch bcp
 Dieser Abschnitt enthält spezielle Überlegungen zum Exportieren und Importieren von Daten im systemeigenen Format durch das Hilfsprogramm **bcp** .  
  
-   Nicht auf Zeichen basierende Daten  
  
     Das [Hilfsprogramm „bcp“](../../tools/bcp-utility.md) verwendet das interne binäre Datenformat von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , um auf Nichtzeichen basierende Daten aus einer Tabelle in eine Datendatei zu schreiben.  
  
-   Daten vom Typ [char](../../t-sql/data-types/char-and-varchar-transact-sql.md) oder [varchar](../../t-sql/data-types/char-and-varchar-transact-sql.md)  
  
     Am Anfang jedes [char](../../t-sql/data-types/char-and-varchar-transact-sql.md) - oder [varchar](../../t-sql/data-types/char-and-varchar-transact-sql.md) -Felds fügt [bcp](../../tools/bcp-utility.md) die Präfixlänge hinzu.  
  
    > [!IMPORTANT]
    >  Wenn der einheitliche Modus verwendet wird, konvertiert das [Hilfsprogramm „bcp“](../../tools/bcp-utility.md) standardmäßig Zeichen aus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in OEM-Zeichen, bevor sie in eine Datendatei kopiert werden. Das [Hilfsprogramm „bcp“](../../tools/bcp-utility.md) konvertiert Zeichen aus einer Datendatei in ANSI-Zeichen, bevor der Massenimport der Zeichen in eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Tabelle ausgeführt wird. Während dieser Konvertierungen kann es zum Verlust von Daten mit erweiterten Zeichen kommen. Verwenden Sie für erweiterte Zeichen entweder das systemeigene Unicode-Format, oder geben Sie eine Codepage an.
  
-   [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md) -Datentyp  
  
     Wenn [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md) -Daten als SQLVARIANT in einer Datendatei im nativen Format gespeichert werden, behalten die Daten alle Merkmale. Die Metadaten, die den Datentyp jedes Datenwerts aufzeichnen, werden zusammen mit dem Datenwert gespeichert. Diese Metadaten werden verwendet, um den Datenwert mit dem gleichen Datentyp in einer [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md) -Zielspalte neu zu erstellen.  
  
     Wenn der Datentyp der Zielspalte nicht [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md)ist, werden alle Datenwerte unter Einhaltung der üblichen Regeln der impliziten Datenkonvertierung in den Datentyp der Zielspalte konvertiert. Wenn während der Datenkonvertierung ein Fehler auftritt, wird für den aktuellen Batch ein Rollback ausgeführt. Bei Werten vom Typ [char](../../t-sql/data-types/char-and-varchar-transact-sql.md) und [varchar](../../t-sql/data-types/char-and-varchar-transact-sql.md), die zwischen [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md)-Spalten übertragen werden, treten möglicherweise Probleme bei der Codepagekonvertierung auf.  
  
     Weitere Informationen zur Datenkonvertierung finden Sie unter [Datentypkonvertierung &#40;Datenbank-Engine&#41;](../../t-sql/data-types/data-type-conversion-database-engine.md).  
  
## <a name="command-options-for-native-format"></a>Befehlsoptionen für das native Format
Sie können Daten im nativen Format importieren, unter Verwendung von [BCP](../../tools/bcp-utility.md), [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) oder [INSERT ... SELECT * FROM OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md).  Für einen [bcp](../../tools/bcp-utility.md) -Befehl oder eine [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) -Anweisung können Sie das Datenformat in der Anweisung angeben.  Für eine [INSERT ... SELECT * FROM OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md)-Anweisung müssen Sie das Datenformat in einer Formatdatei angeben.  

Das native Format wird durch die folgenden Befehlsoptionen unterstützt:  

|Get-Help|Option|BESCHREIBUNG|  
|-------------|------------|-----------------|  
|bcp|**-n**|Veranlasst das Hilfsprogramm „bcp“, die nativen Datentypen der Daten zu verwenden.*|  
|BULK INSERT|DATAFILETYPE **='native'**|Verwendet die systemeigenen Datentypen (native oder widenative) der Daten. Beachten Sie, dass DATAFILETYPE nicht erforderlich ist, wenn eine Formatdatei die Datentypen angibt.|  
|OPENROWSET|–|Muss eine Formatdatei verwenden|

  
 \*Verwenden Sie zum Laden nativer Daten ( **-n**) in ein Format, das mit früheren Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Clients kompatibel ist, den Schalter **-V** . Weitere Informationen finden Sie unter [Importieren von Daten aus früheren SQL Server-Versionen im nativen Format oder im Zeichenformat](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md).  
  
> [!NOTE]
>  Alternativ können Sie die Formatierung pro Feld in einer Formatdatei angeben. Weitere Informationen finden Sie unter [Formatdateien zum Importieren oder Exportieren von Daten &#40;SQL Server&#41;](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md)erforderlich.
  

## <a name="example-test-conditions"></a>Beispieltestbedingungen
Die in diesem Thema beschriebenen Beispiele basieren auf einer Tabelle und einer Formatdatei, die nachstehend definiert werden.

### <a name="sample-table"></a>Beispieltabelle
Das folgende Skript erstellt eine Testdatenbank sowie eine Tabelle namens `myNative` und füllt die Tabelle mit einigen ursprünglichen Werten auf.  Führen Sie den folgenden Transact-SQL-Befehl in Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) aus:

```sql
CREATE DATABASE TestDatabase;
GO

USE TestDatabase;
CREATE TABLE dbo.myNative ( 
   PersonID smallint NOT NULL,
   FirstName varchar(25) NOT NULL,
   LastName varchar(30) NOT NULL,
   BirthDate date,
   AnnualSalary money
   );

-- Populate table
INSERT TestDatabase.dbo.myNative
VALUES 
(1, 'Anthony', 'Grosse', '1980-02-23', 65000.00),
(2, 'Alica', 'Fatnowna', '1963-11-14', 45000.00),
(3, 'Stella', 'Rossenhain', '1992-03-02', 120000.00);

-- Review Data
SELECT * FROM TestDatabase.dbo.myNative;
```

### <a name="sample-non-xml-format-file"></a>Beispiel einer Nicht-XML-Formatdatei
SQL Server unterstützt zwei Typen von Formatdateien: Nicht-XML- und XML-Format.  Nicht-XML ist das ursprüngliche Format, das von früheren Versionen von SQL Server unterstützt wird.  Ausführliche Informationen finden Sie unter [Nicht-XML-Formatdateien (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md) .  Im folgenden Befehl wird das [bcp-Hilfsprogramm](../../tools/bcp-utility.md) verwendet, um die Nicht-XML-Formatdatei `myNative.fmt`zu erstellen, die auf dem Schema von `myNative`basiert.  Geben Sie bei der Ausführung eines [bcp](../../tools/bcp-utility.md) -Befehls zum Erstellen einer Formatdatei das **format** -Argument an, und verwenden Sie **nul** anstatt eines Datendateipfads.  Die Option „format“ erfordert außerdem die Option **-f** .  Zusätzlich wird in diesem Beispiel der Qualifizierer **c** verwendet, um Zeichendaten anzugeben, und **T** , um eine vertrauenswürdige Verbindung anzugeben, für die integrierte Sicherheit verwendet wird.  Geben Sie folgende Befehle an der Eingabeaufforderung ein:

```cmd
bcp TestDatabase.dbo.myNative format nul -f D:\BCP\myNative.fmt -T 

REM Review file
Notepad D:\BCP\myNative.fmt
```

> [!IMPORTANT]
> Stellen Sie sicher, dass Ihre Nicht-XML-Formatdatei mit einem Wagenrücklauf/Zeilenvorschub endet.  Andernfalls wird Ihnen möglicherweise die folgende Fehlermeldung angezeigt:
> 
> `SQLState = S1000, NativeError = 0`  
> `Error = [Microsoft][ODBC Driver 13 for SQL Server]I/O error while reading BCP format file`

## <a name="examples"></a>Beispiele
In dem folgenden Beispiel werden die Datenbank und die Formatdateien verwendet, die oben erstellt wurden.

### <a name="using-bcp-and-native-format-to-export-data"></a>Verwenden von bcp und des nativen Formats zum Exportieren von Daten
Der Schalter **-n** und der **OUT** -Befehl.  Hinweis: Die in diesem Beispiel erstellte Datendatei wird auch in allen nachfolgenden Beispielen verwendet.  Geben Sie folgende Befehle an der Eingabeaufforderung ein:

```cmd
bcp TestDatabase.dbo.myNative OUT D:\BCP\myNative.bcp -T -n

REM Review results
NOTEPAD D:\BCP\myNative.bcp
```

### <a name="using-bcp-and-native-format-to-import-data-without-a-format-file"></a>Verwenden von bcp und des nativen Formats zum Importieren von Daten ohne eine Formatdatei
Der Schalter **-n** und der **IN** -Befehl.  Geben Sie folgende Befehle an der Eingabeaufforderung ein:

```cmd
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myNative;"

REM Import data
bcp TestDatabase.dbo.myNative IN D:\BCP\myNative.bcp -T -n

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myNative;"
```

### <a name="using-bcp-and-native-format-to-import-data-with-a-non-xml-format-file"></a>Verwenden von bcp und des nativen Formats zum Importieren von Daten mit einer Nicht-XML-Formatdatei
Die Schalter **-n** und **-f** switches und **IN** commund.  Geben Sie folgende Befehle an der Eingabeaufforderung ein:

```cmd
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myNative;"

REM Import data
bcp TestDatabase.dbo.myNative IN D:\BCP\myNative.bcp -f D:\BCP\myNative.fmt -T

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myNative;"
```

### <a name="using-bulk-insert-and-native-format-without-a-format-file"></a>Verwenden von BULK INSERT und des nativen Formats ohne eine Formatdatei
**DATAFILETYPE** -Argument.  Führen Sie den folgenden Transact-SQL-Befehl in Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) aus:

```sql
TRUNCATE TABLE TestDatabase.dbo.myNative; -- for testing
BULK INSERT TestDatabase.dbo.myNative
    FROM 'D:\BCP\myNative.bcp'
    WITH (
        DATAFILETYPE = 'native'
        );

-- review results
SELECT * FROM TestDatabase.dbo.myNative;
```

### <a name="using-bulk-insert-and-native-format-with-a-non-xml-format-file"></a>Verwenden von BULK INSERT und des nativen Formats mit einer Nicht-XML-Formatdatei
Argument **FORMATFILE** .  Führen Sie den folgenden Transact-SQL-Befehl in Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) aus:

```sql
TRUNCATE TABLE TestDatabase.dbo.myNative; -- for testing
BULK INSERT TestDatabase.dbo.myNative
   FROM 'D:\BCP\myNative.bcp'
   WITH (
        FORMATFILE = 'D:\BCP\myNative.fmt'
        );

-- review results
SELECT * FROM TestDatabase.dbo.myNative;
```

### <a name="using-openrowset-and-native-format-with-a-non-xml-format-file"></a>Verwenden von OPENROWSET und des nativen Formats mit einer Nicht-XML-Formatdatei
Argument **FORMATFILE** .  Führen Sie den folgenden Transact-SQL-Befehl in Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) aus:

```sql
TRUNCATE TABLE TestDatabase.dbo.myNative;  -- for testing
INSERT INTO TestDatabase.dbo.myNative
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myNative.bcp', 
        FORMATFILE = 'D:\BCP\myNative.fmt'  
        ) AS t1;

-- review results
SELECT * FROM TestDatabase.dbo.myNative;
```
  
## <a name="related-tasks"></a>Zugehörige Aufgaben
So verwenden Sie Datenformate für Massenimport oder Massenexport 
  
-   [Importieren von Daten aus früheren SQL Server-Versionen im nativen Format oder im Zeichenformat](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md)  
  
-   [Verwenden des Zeichenformats zum Importieren und Exportieren von Daten &#40;SQL Server&#41;](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)  
  
-   [Verwenden des Unicode-Zeichenformats zum Importieren und Exportieren von Daten &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)  
  
-   [Verwenden des nativen Unicode-Formats zum Importieren oder Exportieren von Daten &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [bcp (Hilfsprogramm)](../../tools/bcp-utility.md)   
 [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [Datentypen &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [sql_variant &#40;Transact-SQL&#41;](../../t-sql/data-types/sql-variant-transact-sql.md)   
 [Importieren von Daten aus früheren SQL Server-Versionen im nativen Format oder im Zeichenformat](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md)   
 [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)   
 [Verwenden des nativen Unicode-Formats zum Importieren oder Exportieren von Daten &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)  
  
  
