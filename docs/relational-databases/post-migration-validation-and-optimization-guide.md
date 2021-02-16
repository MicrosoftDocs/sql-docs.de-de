---
description: Handbuch für die Überprüfung und Optimierung nach der Migration
title: Handbuch für die Überprüfung und Optimierung nach der Migration | Microsoft-Dokumtenation
ms.date: 01/09/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- post-migration validation and optimization
- guide, post-migration validation and optimization
ms.assetid: 11f8017e-5bc3-4bab-8060-c16282cfbac1
author: pelopes
ms.author: harinid
ms.openlocfilehash: 397e5b7acd88f8e10dea05ce3ab2dc2f7dd105c2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350655"
---
# <a name="post-migration-validation-and-optimization-guide"></a>Handbuch für die Überprüfung und Optimierung nach der Migration

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

Die Schritte in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], die nach der der Migration ausgeführt werden, sind sehr wichtig für das Abgleichen der Genauigkeit und der Vollständigkeit der Daten sowie für das Aufdecken von Leistungsproblemen mit der Arbeitsauslastung.

## <a name="common-performance-scenarios"></a>Allgemeine Leistungsszenarios

Im Folgenden sind einige der häufigsten Leistungsszenarios aufgelistet, die nach der Migration zur Plattform [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] auftreten, und wie sie behoben werden können. Hierzu gehören auch Szenarios, die für die Migration von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] zu [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] (ältere Version zu neuere Version) sowie für die Migration von foreign-Plattformen (z.B. Oracle, DB2, MySQL und Sybase) zu [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] spezifisch sind.

## <a name="query-regressions-due-to-change-in-ce-version"></a><a name="CEUpgrade"></a> Abfrageregressionen aufgrund einer Änderung in der CE-Version

**Gilt für: Migration von** [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] zu [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

Wenn Sie von einer älteren [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Version zu [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] oder neuer migrieren und ein Upgrade auf den aktuellen [Datenbankkompatibilitätsgrad](../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md) durchführen, kann es bei einer Arbeitsauslastung womöglich zu einem Leistungsrückgang kommen.

Seit [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] sind daher alle Änderungen des Abfrageoptimierers an den neuesten [Datenbank-Kompatibilitätsgrad](../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md) gebunden, sodass Pläne nicht sofort im Moment des Upgrades geändert werden, sondern erst, wenn ein Benutzer die Datenbankoption `COMPATIBILITY_LEVEL` auf die neuste aktualisiert. Diese Möglichkeit gibt Ihnen in Kombination mit dem Abfragespeicher ein großes Maß an Kontrolle über die Abfrageleistung im Upgradeprozess. 

Weitere Informationen zu Änderungen des Abfrageoptimierers, der in [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] eingeführt wurde, finden Sie unter [Optimizing Your Query Plans with the SQL Server 2014 Cardinality Estimator (Optimieren Ihrer Abfragepläne mit der Kardinalitätsschätzung von SQL Server 2014)](/previous-versions/dn673537(v=msdn.10)).

### <a name="steps-to-resolve"></a>Schritte zum Beheben

Ändern Sie den [Datenbank-Kompatibilitätsgrad](../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md) auf die Quellversion, und befolgen Sie den empfohlenen Upgradeworkflow wie in folgendem Bild gezeigt:

![Diagramm: Empfohlener Upgradeworkflow](../relational-databases/performance/media/query-store-usage-5.png "query-store-usage-5")  

Weitere Informationen zu diesem Thema finden Sie unter [Aufrechterhalten einer stabilen Leistung während des Upgrades auf SQL Server 2016](../relational-databases/performance/query-store-usage-scenarios.md#CEUpgrade).

## <a name="sensitivity-to-parameter-sniffing"></a><a name="ParameterSniffing"></a> Sensitivität für die Parameterermittlung

**Gilt für:** die Migration von Drittanbieter-Plattformen (z.B. Oracle, DB2, MySQL und Sybase) zu [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]

> [!NOTE]
> Bei Migrationen von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] zu [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] wird dieses Szenario bei einer Migration auf eine neuere, unveränderte Version von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] nicht berücksichtigt, wenn das Problem auf dem Quell-[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] bereits bestand. 

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] kompiliert Abfragepläne in gespeicherten Prozeduren, indem die Eingabeparameter bei der ersten Kompilierung ermittelt werden. Anschließend wird ein parametrisierter und wiederverwendeter Plan generiert, der für diese Verteilung von Eingabedaten optimiert ist. Die meisten Anweisungen, die triviale Pläne generieren, werden parametrisiert, wenn auch nicht in gespeicherten Prozeduren. Nachdem Sie ein Plan zuerst zwischengespeichert wird, wird jede spätere Ausführung einem zuvor zwischengespeicherten Plan zugeordnet.
Ein mögliches Problem tritt auf, wenn diese erste Kompilierung möglicherweise nicht die am häufigsten verwendeten Parametersätze für die übliche Arbeitsauslastung verwendet hat. Bei anderen Parametern wird derselbe Ausführungsplans ineffizient. Weitere Informationen zu diesem Thema finden Sie unter [Parameterermittlung](../relational-databases/query-processing-architecture-guide.md#ParamSniffing).

### <a name="steps-to-resolve"></a>Schritte zum Beheben

1.  Verwenden Sie den `RECOMPILE`-Hinweis. Ein Plan wird jedes Mal je nach Parameterwert berechnet.
2.  Schreiben Sie die gespeicherte Prozedur neu, sodass die Option `(OPTIMIZE FOR(<input parameter> = <value>))` verwendet wird. Entscheiden Sie, welcher Wert verwendet werden soll, der die am besten zu den meisten der relevanten Arbeitsauslastungen passt und einen Plan erstellt und verwaltet, der für die parametrisierten Werte effizient wird.
3.  Schreiben Sie die gespeicherten Prozeduren mithilfe der lokalen Variablen innerhalb der Prozedur neu. Nun verwendet der Optimierer den Dichtevektor für Einschätzungen, was zu dem gleichen Plan führt, unabhängig vom Parameterwert.
4.  Schreiben Sie die gespeicherte Prozedur neu, sodass die Option `(OPTIMIZE FOR UNKNOWN)` verwendet wird. Dies hat dieselbe Wirkung wie die Verwendung der lokalen Variablen.
5.  Schreiben Sie die Abfrage neu, sodass der Hinweis `DISABLE_PARAMETER_SNIFFING` verwendet wird. Dies hat denselben Effekt wie die Verwendung der lokalen Variablen: Die Parameterermittlung wird vollständig deaktiviert, es sei denn, `OPTION(RECOMPILE)`, `WITH RECOMPILE` oder `OPTIMIZE FOR <value>` wird verwendet.

> [!TIP] 
> Nutzen Sie die Vorteile der [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)]-Plananalyse, um schnell zu ermitteln, ob es sich um ein Problem handelt. Weitere Informationen dazu finden Sie [unter diesem Link](/archive/blogs/sql_server_team/new-in-ssms-query-performance-troubleshooting-made-easier).

## <a name="missing-indexes"></a><a name="MissingIndexes"></a> Fehlende Indizes

**Gilt für:** die Migration von Drittanbieter-Plattformen (z.B. Oracle, DB2, MySQL und Sybase) und die Migration von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] zu [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]

Falsche oder fehlende Indizes führt zu zusätzlicher Eingabe/Ausgabe, wegen der zusätzlicher Arbeitsspeicher und CPU verschwendet wird. Dies kann daran liegen, dass das Arbeitsauslastungsprofil geändert wurde, also z.B. die Verwendung anderer Prädikate, die den vorhandenen Indexentwurf ungültig machen. Anzeichen einer schlechten Indizierungsstrategie oder Änderungen am Arbeitsauslastungsprofil sind z.B. folgende:
-   Doppelte, redundante, selten verwendete und vollständig nicht verwendete Indizes
-   Nicht verwendete Indizes mit Updates. Dabei ist besondere Sorgfalt geboten.

### <a name="steps-to-resolve"></a>Schritte zum Beheben

1.  Nutzen Sie den grafischen Ausführungsplan für „Fehlender Index“-Verweise.
2.  Indizieren Sie generierte Vorschläge mit dem [Datenbankoptimierungsratgeber](../tools/dta/tutorial-database-engine-tuning-advisor.md).
3.  Nutzen Sie die [Fehlende Indizes-DMV](../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-details-transact-sql.md) oder das [SQL Server-Leistungsdashboard](https://www.microsoft.com/download/details.aspx?id=29063).
4.  Nutzen Sie bereits vorhandene Skripts, die vorhandene DMVs verwenden können, um einen Einblick in alle fehlenden, doppelten, redundante, selten verwendeten und nicht vollständig verwendeten Indizes zu bieten. Verwenden Sie diese Skripts auch, wenn Indexverweise in vorhandenen Prozeduren und Funktionen in der Datenbank mit einem Hinweis versehen/hartcodiert wurden. 

> [!TIP] 
> Beispiele für bereits vorhandene Skripts [Index-Creation](https://github.com/Microsoft/tigertoolbox/tree/master/Index-Creation) und [Index-Information](https://github.com/Microsoft/tigertoolbox/tree/master/Index-Information). 

## <a name="inability-to-use-predicates-to-filter-data"></a><a name="InabilityPredicates"></a> Unmöglichkeit, mit Prädikaten Daten zu filtern

**Gilt für:** die Migration von Drittanbieter-Plattformen (z.B. Oracle, DB2, MySQL und Sybase) und die Migration von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] zu [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]

> [!NOTE]
> Bei Migrationen von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] zu [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] wird dieses Szenario bei einer Migration auf eine neuere, unveränderte Version von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] nicht berücksichtigt, wenn das Problem auf dem Quell-[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] bereits bestand.

Der [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Abfrageoptimierer kann nur Informationen berücksichtigen, die zum Zeitpunkt der Kompilierung bekannt sind. Wenn eine Arbeitsauslastung Prädikate nutzt, die nur zum Zeitpunkt der Ausführung bekannt sein können, erhöht sich das Risiko einer schlechten Planauswahl. Für einen qualitativ noch hochwertigeren Plan zu erhalten, müssen Prädikate **SARGable**, oder „**S** earch **Arg** ument **able**“ sein.

Einige Beispiele für nicht SARGable-Prädikate sind:
-   Implizite Datenkonvertierungen wie VARCHAR, NVARCHAR oder INT zu VARCHAR. Suchen Sie in den tatsächlichen Ausführungsplänen nach CONVERT_IMPLICIT-Laufzeitwarnungen. Das Konvertieren von einem Typ in einen anderen kann auch zu einem Genauigkeitsverlust führen.
-   Komplexe unbestimmte Ausdrücke wie `WHERE UnitPrice + 1 < 3.975`, aber nicht `WHERE UnitPrice < 320 * 200 * 32`.
-   Ausdrücke mit Funktionen wie `WHERE ABS(ProductID) = 771` oder`WHERE UPPER(LastName) = 'Smith'`
-   Zeichenfolgen mit einem führenden Platzhalterzeichen wie `WHERE LastName LIKE '%Smith'`, aber nicht `WHERE LastName LIKE 'Smith%'`

### <a name="steps-to-resolve"></a>Schritte zum Beheben

1. Deklarieren Sie Variablen/Parameter immer als vorgesehenen [Zieldatentyp](../t-sql/data-types/data-types-transact-sql.md). 
  -   Dazu kann das Vergleichen aller benutzerdefinierten Codekonstrukte gehören, die in der Datenbank gespeichert sind (z.B. gespeicherte Prozeduren, benutzerdefinierte Funktionen oder Sichten), mit Systemtabellen, die Informationen zu Datentypen beinhalten, die in den zugrunde liegenden Tabellen verwendet werden (z.B. [sys.columns](../relational-databases/system-catalog-views/sys-columns-transact-sql.md)).
2. Wenn der gesamte Code nicht bis zum vorherigen Punkt durchsucht werden kann, ändern Sie zum gleichen Zweck den Datentyp für die Tabelle entsprechend einer Variablen-/Parameterdeklaration.
3. Gründe für die Nützlichkeit der folgenden Konstrukte:

  -   Funktionen werden als Prädikate verwendet
  -   Platzhaltersuchen
  -   Komplexe Ausdrücke auf Grundlage von spaltenbasierten Daten. Bewerten Sie die Notwendigkeit, stattdessen persistente berechnete Spalten zu erstellen, die indiziert werden können

> [!NOTE] 
> Alles, was oben aufgeführt ist, kann programmgesteuert ausgeführt werden.

## <a name="use-of-table-valued-functions-multi-statement-vs-inline"></a><a name="TableValuedFunctions"></a> Verwendung von Tabellenwertfunktionen (Funktionen mit mehreren Anweisungen oder Inlinefunktionen)

**Gilt für:** die Migration von Drittanbieter-Plattformen (z.B. Oracle, DB2, MySQL und Sybase) und die Migration von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] zu [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]

> [!NOTE]
> Bei Migrationen von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] zu [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] wird dieses Szenario bei einer Migration auf eine neuere, unveränderte Version von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] nicht berücksichtigt, wenn das Problem auf dem Quell-[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] bereits bestand.

Tabellenwertfunktionen geben einen table-Datentyp zurück, der eine Alternative zu Ansichten sein kann. Ansichten sind auf eine einzelne `SELECT`-Anweisung beschränkt, während benutzerdefinierte Funktionen zusätzliche Anweisungen enthalten können, die mehr Logik als ansichten ermöglichen.

> [!IMPORTANT] 
> Da die Ausgabetabelle einer Tabellenwertfunktion mit mehreren Anweisungen (Multi-Statement Table Valued Function, MSTVF) nicht zur Kompilierzeit erstellt wird, verwendet der [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Abfrageoptimierer Heuristik und keine tatsächliche Statistik, um Zeileneinschätzungen zu bestimmen. Auch wenn den Basistabellen Indizes hinzugefügt werden, wird dies nicht helfen. Für MSTVFs verwendet [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] eine feste Schätzung von 1 für die Anzahl der Zeilen, die von einer MSTVF zurückgegeben werden sollen (beginnend mit [!INCLUDE[ssSQL14](../includes/sssql14-md.md)], wo die behobene Schätzung 100 Zeilen beträgt).

### <a name="steps-to-resolve"></a>Schritte zum Beheben

1.  Wenn die Tabellenwertfunktion mit mehreren Anweisungen nur eine einzelne Anweisung enthält, konvertieren Sie zu einer Inline-Tabellenwertfunktion.

    ```sql
    CREATE FUNCTION dbo.tfnGetRecentAddress(@ID int)
    RETURNS @tblAddress TABLE
    ([Address] VARCHAR(60) NOT NULL)
    AS
    BEGIN
      INSERT INTO @tblAddress ([Address])
      SELECT TOP 1 [AddressLine1]
      FROM [Person].[Address]
      WHERE  AddressID = @ID
      ORDER BY [ModifiedDate] DESC
    RETURN
    END
    ```

    Es folgt das Beispiel für das Inline-Format.

    ```sql
    CREATE FUNCTION dbo.tfnGetRecentAddress_inline(@ID int)
    RETURNS TABLE
    AS
    RETURN (
      SELECT TOP 1 [AddressLine1] AS [Address]
      FROM [Person].[Address]
      WHERE  AddressID = @ID
      ORDER BY [ModifiedDate] DESC
    )
    ```

2.  Wenn sie komplexer ist, sollten Sie die Zwischenergebnisse verwenden, die in speicheroptimierten Tabellen oder in temporären Tabellen gespeichert sind.

##  <a name="additional-reading"></a><a name="Additional_Reading"></a> Zusätzliches Lesematerial

 [Bewährte Methoden für den Abfragespeicher](../relational-databases/performance/best-practice-with-the-query-store.md)  
[Speicheroptimierte Tabellen](./in-memory-oltp/sample-database-for-in-memory-oltp.md)  
[Benutzerdefinierte Funktionen](../relational-databases/user-defined-functions/user-defined-functions.md)  
[Table Variables and Row Estimations - Part 1 (Tabellenvariablen und Zeilenschätzungen – Teil 1)](/archive/blogs/blogdoezequiel/table-variables-and-row-estimations-part-1)  
[Table Variables and Row Estimations - Part 1 (Tabellenvariablen und Zeilenschätzungen – Teil 2)](/archive/blogs/blogdoezequiel/table-variables-and-row-estimations-part-2)  
[Zwischenspeichern und Wiederverwenden von Ausführungsplänen](../relational-databases/query-processing-architecture-guide.md#execution-plan-caching-and-reuse)