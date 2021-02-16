---
description: Nicht deterministische Konvertierung von Datumsliteralen in DATE-Werte
title: Nicht deterministische Konvertierung von Datumsliteralen | Microsoft-Dokumentation
ms.custom: ''
ms.date: 11/19/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: genemi
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 45540a538982f75033f35ad759782317983dc958
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100071814"
---
# <a name="nondeterministic-conversion-of-literal-date-strings-into-date-values"></a>Nicht deterministische Konvertierung von Datumsliteralen in DATE-Werte

Seien Sie vorsichtig, wenn Sie die Konvertierung Ihrer CHARACTER-Zeichenfolgen in DATE-Datentypen zulassen. Der Grund dafür ist, dass solche Konvertierungen oft _nicht deterministisch_ sind.

Sie steuern diese nicht deterministischen Konvertierungen, indem Sie die Einstellungen von [SET LANGUAGE](../statements/set-language-transact-sql.md) und [SET DATEFORMAT](../statements/set-dateformat-transact-sql.md) berücksichtigen.



## <a name="set-language-example-month-name-in-polish"></a>Beispiel für SET LANGUAGE: Monatsname in polnischer Sprache

- `SET LANGUAGE Polish;`

Eine Zeichenfolge kann der Name eines Monats sein. Aber ist der Name in englischer, polnischer, kroatischer oder einer anderen Sprache? Und wird die Sitzung des Benutzers auf die richtige entsprechende SPRACHE festgelegt?

Nehmen Sie z. B. das Wort _listopad_, das der Name eines Monats ist. Aber welcher Monat es ist, hängt von der Sprache ab, von der das SQL Server-System glaubt, dass sie verwendet wird:
- Falls Polnisch, steht _listopad_ für den 11. Monat (_November_ auf Englisch).
- Falls Kroatisch, steht _listopad_ für den 10. Monat (_Oktober_ auf Englisch).

#### <a name="code-example-of-set-language"></a>Codebeispiel für SET LANGUAGE

```sql
--SELECT alias FROM sys.syslanguages ORDER BY alias;

DECLARE @yourInputDate  NVARCHAR(32) = '28 listopad 2018';

SET LANGUAGE Polish;
SELECT CONVERT(DATE, @yourInputDate) AS [SL_Polish];

SET LANGUAGE Croatian;
SELECT CONVERT(DATE, @yourInputDate) AS [SL_Croatian];

SET LANGUAGE English;


/***  Actual output:  For the two months, note the 11 versus the 10.
SL_Polish
2018-11-28

SL_Croatian
2018-10-28
***/
```



## <a name="set-dateformat-example"></a>Beispiel für SET DATEFORMAT

- `SET DATEFORMAT dmy;`

Das voranstehende **dmy**-Format besagt, dass z. B. die Datumszeichenfolge '01-03-2018' so interpretiert wird, dass sie _den ersten Tag im März im Jahr 2018_ bedeutet.

Wenn stattdessen **mdy** angegeben wird, würde die gleiche Zeichenfolge '01-03-2018' _den dritten Tag im Januar 2018_ bedeuten.

Und wenn **ymd** angegeben würde, gibt es keine Garantie dafür, wie die Ausgabe aussehen würde. Der numerische Wert '2018' ist zu groß für einen Tag.
<!--
The preceding claim of "no guarantee" might be incorrect, in the minds of the SQL query engine Developer team?
-->

#### <a name="specific-countries"></a>Bestimmte Länder

In Japan und China wird das DATEFORMAT **ymd** verwendet. Die Teile des Formats befinden sich in einer sinnvollen Reihenfolge von der größten Einheit zur kleinsten. Daher lässt sich dieses Format gut sortieren. Dieses Format gilt als _internationales_ Format. Es ist international, weil die vier Ziffern des Jahres eindeutig sind und kein Land der Erde das archaische Format **ydm** verwendet.

In anderen Ländern wie Deutschland und Frankreich ist das DATEFORMAT **dmy**, was **'TT.MM.JJJJ'** bedeutet. Das **dmy**-Format lässt sich nicht gut sortieren, aber es ist eine sinnvolle Reihenfolge von der kleinsten bis zur größten Einheit.

Die USA und die Föderierten Staaten von Mikronesien sind die einzigen Länder, die **mdy** verwenden, was sich nicht sortieren lässt. Die gemischte Reihenfolge des Formats entspricht einem Muster mündlicher Sprache in gesprochenen Datumsangaben.

#### <a name="code-example-of-set-dateformat-mdy-versus-dmy"></a>Codebeispiel für die SET DATEFORMAT: *mdy* im Vergleich zu *dmy*

Das folgende Transact-SQL-Codebeispiel verwendet die gleiche Datumszeichenfolge mit drei unterschiedlichen DATEFORMAT-Einstellungen. Bei Ausführung des Codes wird die im Kommentar gezeigte Ausgabe generiert:

```sql
DECLARE @yourDateString NVARCHAR(10) = '12-09-2018';
PRINT @yourDateString + '  = the input.';

SET DATEFORMAT dmy;
SELECT CONVERT(DATE, @yourDateString) AS [DMY-Interpretation-of-input-format];

SET DATEFORMAT mdy;
SELECT CONVERT(DATE, @yourDateString) AS [MDY-Interpretation-of-input-format];

SET DATEFORMAT ymd;
SELECT CONVERT(DATE, @yourDateString) AS [YMD-Interpretation--?--NotGuaranteed];


/***  Actual output:
12-09-2018  = the input.

DMY-Interpretation-of-input-format
2018-09-12

MDY-Interpretation-of-input-format
2018-12-09

YMD-Interpretation--?--NotGuaranteed
2018-12-09
***/
```

Im vorhergehenden Codebeispiel weist das letzte Beispiel eine Nichtübereinstimmung zwischen dem Format **ymd** und der Eingabezeichenfolge auf. Der dritte Knoten der Eingabezeichenfolge stellt einen Zahlenwert dar, der für einen Tag zu groß ist. Bei solchen Nichtübereinstimmungen garantiert Microsoft nicht den Ausgabewert.

#### <a name="convert-offers-explicit-codes-for-_deterministic_-control-of-date-formats"></a>CONVERT bietet explizite Codes für die _deterministische_ Steuerung von Datumsformaten.

Unser Dokumentationsartikel zu CAST und CONVERT enthält explizite Codes, die Sie mit der CONVERT-Funktion verwenden können, um Datumskonvertierungen _deterministisch_ zu steuern. Jeden Monat hat der Artikel eine der höchsten Zugriffszahlen.

- [CAST und CONVERT (Transact-SQL): Datums- und Uhrzeitformate](../functions/cast-and-convert-transact-sql.md#date-and-time-styles)
- [CAST und CONVERT (Transact-SQL): Bestimmte DATETIME-Konvertierungen sind nicht deterministisch](../functions/cast-and-convert-transact-sql.md#certain-datetime-conversions-are-nondeterministic)



## <a name="compatibility-level-90-and-above"></a>Kompatibilitätsgrad 90 und höher

Bei SQL Server 2000 war der Kompatibilitätsgrad 80. Für Gradeinstellungen von 80 oder niedriger waren implizite Datumskonvertierungen deterministisch.

Ab SQL Server 2005 und dessen Kompatibilitätsgrad 90 wurden implizite Datumskonvertierungen nicht mehr deterministisch. Ab Grad 90 wurden die Datumskonvertierungen von SET LANGUAGE und SET DATEFORMAT abhängig.

#### <a name="unicode"></a>Unicode

<!-- The next live sentence needs an explanatory example!  N'somethingHere?'.
-->
Die Konvertierung von Nicht-Unicode-Zeichendaten zwischen Sortierungen wird auch als nicht deterministisch angesehen.



## <a name="see-also"></a>Weitere Informationen

- [Festlegen einer Sitzungssprache](../../relational-databases/collations/set-a-session-language.md)
- [Datums- und Uhrzeitdatentypen und zugehörige Funktionen (Transact-SQL)](../functions/date-and-time-data-types-and-functions-transact-sql.md)
- [FORMAT (Transact-SQL)](../functions/format-transact-sql.md)
- [ISDATE (Transact-SQL)](../functions/isdate-transact-sql.md)



<!--
This new article is linked-to by the following articles (at least initially on 2018/11/19).....
...
* docs/relational-databases/views/create-indexed-views.md
* docs/relational-databases/indexes/indexes-on-computed-columns.md
* docs/t-sql/functions/cast-and-convert-transact-sql.md
...
As a reaction to public PR 1279, this approach of creating a new article to link to is a better alternative than a docs/includes/ approach.
GeneMi (MightyPen), 2018/11/19
-->

