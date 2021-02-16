---
title: Speichern von JSON-Dokumenten in SQL Server oder SQL-Datenbank
description: In diesem Artikel wird beschrieben, warum und wie sie JSON-Dokumente in SQL Server oder SQL-Datenbank speichern und indizieren und wie Sie Abfragen für diese JSON-Dokumente optimieren.
ms.date: 06/03/2020
ms.prod: sql
ms.technology: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: 30d51162b36d787e8976b2c6aa48e533fd476f8e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100015060"
---
# <a name="store-json-documents-in-sql-server-or-sql-database"></a>Speichern von JSON-Dokumenten in SQL Server oder SQL-Datenbank
SQL Server und Azure SQL-Datenbank verfügen über native JSON-Funktionen, mit denen Sie JSON-Dokumente durch die standardmäßige SQL-Sprache analysieren können. Sie können JSON-Dokumente in SQL Server oder SQL-Datenbank speichern und JSON-Daten wie in einer NoSQL-Datenbank abfragen. Dieser Artikel beschreibt die Optionen zum Speichern von JSON-Dokumenten in SQL Server oder SQL-Datenbank.

## <a name="json-storage-format"></a>JSON-Speicherformat

Die erste Entscheidung zum Speicherentwurf betrifft das Speichern von JSON-Dokumenten in den Tabellen. Es stehen zwei Optionen zur Verfügung:
- **LOB-Speicher**: JSON-Dokumente können unverändert in `NVARCHAR`-Spalten gespeichert werden. Dies ist die beste Möglichkeit für schnelles Laden von Daten und Erfassen, da die Ladegeschwindigkeit dem Laden von Zeichenfolgenspalten entspricht. Dieser Ansatz kann zu einer zusätzlichen Leistungseinbuße bei der Abfrage-/Analysezeit führen, wenn die Indizierung von JSON-Werten nicht durchgeführt wird, da die unformatierten JSON-Dokumente analysiert werden müssen, während die Abfragen ausgeführt werden. 
- **Relationaler Speicher**: JSON-Dokumente können analysiert werden, während sie mithilfe von `OPENJSON`-, `JSON_VALUE`- oder `JSON_QUERY`-Funktionen in die Tabelle eingefügt werden. Fragmente aus den eingegebenen JSON-Dokumenten können in den SQL-Datentypspalten oder in NVARCHAR-Spalten gespeichert werden, die JSON-Unterelemente enthalten. Diese Vorgehensweise erhöht die Ladezeit, da die JSON-Analyse beim Laden erfolgt. Die Abfrageleistung entspricht jedoch der Leistung bei klassischen Abfragen relationaler Daten.

## <a name="classic-tables"></a>Klassische Tabellen

Die einfachste Möglichkeit zum Speichern von JSON-Dokumenten in SQL Server oder SQL-Datenbank stellt die Erstellung einer zweispaltigen Tabelle dar, die die ID und den Inhalt des Dokuments enthält. Beispiel:

```sql
create table WebSite.Logs (
    _id bigint primary key identity,
    log nvarchar(max)
);
```

Diese Struktur entspricht den Sammlungen, die Sie in klassischen Dokumentdatenbanken finden. Der Primärschlüssel `_id` ist ein automatisch inkrementierter Wert, der einen eindeutigen Bezeichner für jedes Dokument bereitstellt und schnelle Suchvorgänge ermöglicht. Diese Struktur ist eine gute Wahl für die klassischen NoSQL-Szenarios, in denen Sie ein Dokument mit der ID abrufen oder ein gespeichertes Dokument mit einer bestimmten ID aktualisieren möchten.

Mit dem Datentyp „nvarchar(max)“ können Sie JSON-Dokumente mit einer Größe von bis zu 2 GB speichern. Wenn Sie jedoch sicher sind, dass Ihre JSON-Dokumente nicht größer als 8 KB sind, wird empfohlen, aus Leistungsgründen NVARCHAR(4000) anstelle von NVARCHAR(max) zu verwenden.

Bei der im vorherigen Beispiel erstellten Beispieltabelle wird davon ausgegangen, dass gültige JSON-Dokumente in der Spalte `log` gespeichert sind. Wenn Sie sichergehen wollen, dass ein gültiges JSON-Objekt in der Spalte `log` gespeichert ist, können Sie für die Spalte eine CHECK-Einschränkung hinzufügen. Beispiel:

```sql
ALTER TABLE WebSite.Logs
    ADD CONSTRAINT [Log record should be formatted as JSON]
                   CHECK (ISJSON(log)=1)
```

Jedes Mal, wenn ein Dokument in die Tabelle eingefügt oder in der Tabelle aktualisiert wird, überprüft diese Einschränkung, ob das JSON-Dokument korrekt formatiert ist. Ohne diese Einschränkung ist die Tabelle für Einfügungen optimiert, da jedes JSON-Dokument ohne weitere Verarbeitung direkt zur Spalte hinzugefügt wird.

Wenn Sie die JSON-Dokumente in der Tabelle speichern, können Sie zum Abfragen der Dokumente die Standardsprache Transact-SQL verwenden. Beispiel:

```sql
SELECT TOP 100 JSON_VALUE(log, '$.severity'), AVG( CAST( JSON_VALUE(log,'$.duration') as float))
 FROM WebSite.Logs
 WHERE CAST( JSON_VALUE(log,'$.date') as datetime) > @datetime
 GROUP BY JSON_VALUE(log, '$.severity')
 HAVING AVG( CAST( JSON_VALUE(log,'$.duration') as float) ) > 100
 ORDER BY AVG( CAST( JSON_VALUE(log,'$.duration') as float) ) DESC
```

Die Möglichkeit, *jede* T-SQL-Funktion und Abfrageklausel zum Abfragen von JSON-Dokumenten verwenden zu können, ist besonders nützlich. SQL Server und SQL-Datenbank führen in den Abfragen, die Sie zur Analyse von JSON-Dokumenten verwenden können, keine Einschränkungen ein. Sie können mit der Funktion `JSON_VALUE` Werte aus einem JSON-Dokument extrahieren und wie jeden anderen Wert in der Abfrage verwenden.

Diese Fähigkeit, umfassende T-SQL-Abfragesyntax zu verwenden, ist der entscheidende Unterschied zwischen SQL Server und SQL-Datenbank und klassischen NoSQL-Datenbanken – bei Transact-SQL sind aller Wahrscheinlichkeit nach alle Funktionen vorhanden, die Sie zur Verarbeitung von JSON-Daten benötigen.

## <a name="indexes"></a>Indizes

Wenn Sie feststellen, dass Ihre Abfragen häufig Dokumente nach bestimmten Eigenschaften durchsuchen (z.B. nach einer `severity`-Eigenschaft in einem JSON-Dokument), können Sie einen klassischen NONCLUSTERED-Index für die Eigenschaft hinzufügen, um die Abfragen zu beschleunigen.

Sie können eine berechnete Spalte erstellen, die JSON-Werte von den JSON-Spalten für den angegebenen Pfad (d.h. für den Pfad `$.severity`) verfügbar macht, und einen Standardindex für diese berechnete Spalte erstellen. Beispiel:

```sql
create table WebSite.Logs (
    _id bigint primary key identity,
    log nvarchar(max),

    severity AS JSON_VALUE(log, '$.severity'),
    index ix_severity (severity)
);
```

Bei der in diesem Beispiel verwendeten berechneten Spalte handelt es sich um eine nicht persistierte oder virtuelle Spalte, die den Speicherplatz der Tabelle nicht erweitert. Diese wird vom Index `ix_severity` verwendet, um wie im folgenden Beispiel die Leistung von Abfragen zu verbessern:

```sql
SELECT log
FROM Website.Logs
WHERE JSON_VALUE(log, '$.severity') = 'P4'
```

Ein wichtiges Merkmal dieses Index ist, dass er sich sortieren lässt. Wenn Ihre ursprüngliche NVARCHAR-Spalte über eine COLLATION-Eigenschaft verfügt (z.B. Groß-/Kleinschreibung oder Japanisch), wird der Index nach den Sprach- oder Groß-/Kleinschreibungsregeln der NVARCHAR-Spalte organisiert. Die Unterstützung dieser Sortierung kann sich für die Entwicklung von Anwendungen für globale Märkte, die benutzerdefinierte Sprachregeln bei der Verarbeitung von JSON-Dokumenten erfordern, als wichtiges Feature erweisen.

## <a name="large-tables--columnstore-format"></a>Große Formate für Tabellen und Columnstore-Indizes

Wenn Sie mit einer großen Anzahl von JSON-Dokumenten in Ihrer Sammlung rechnen, empfehlen wir Ihnen, einen CLUSTERED COLUMNSTORE-Index für die Sammlung hinzuzufügen, wie im folgenden Beispiel gezeigt wird:

```sql
create sequence WebSite.LogID as bigint;
go
create table WebSite.Logs (
    _id bigint default(next value for WebSite.LogID),
    log nvarchar(max),

    INDEX cci CLUSTERED COLUMNSTORE
);
```

Ein CLUSTERED COLUMNSTORE-Index ermöglicht eine hohe (bis zu 25-fache) Datenkomprimierung, wodurch Sie Ihren Speicherplatzbedarf deutlich reduzieren, die Speicherkosten senken und die E/A-Leistung Ihrer Workloads erhöhen können. Außerdem sind CLUSTERED COLUMNSTORE-Indizes für Tabellenscans und Analysen für Ihre JSON-Dokumente optimiert, sodass dieser Indextyp die beste Option für Protokollanalysen darstellen kann.

Im vorhergehende Beispiel wird ein Sequenzobjekt verwendet, um der Spalte `_id` Werte zuzuweisen. Sowohl Sequenzen als auch Identitäten sind gültige Optionen für die ID-Spalte.

## <a name="frequently-changing-documents--memory-optimized-tables"></a>Häufig geänderte Dokumente und speicheroptimierte Tabellen

Wenn Sie mit einer großen Anzahl von Aktualisierungs-, Einfüge- und Löschvorgängen in Ihren Sammlungen rechnen, können Sie Ihre JSON-Dokumente in speicheroptimierten Tabellen speichern. Speicheroptimierte JSON-Sammlungen verwalten die Daten immer im Arbeitsspeicher, sodass kein Speicher-E/A-Overhead erzeugt wird. Darüber hinaus sind speicheroptimierte JSON-Sammlungen komplett sperrenfrei, d. h., andere Vorgänge werden durch Aktionen für Dokumente nicht blockiert.

Um eine klassische Sammlung in eine speicheroptimierte Sammlung zu konvertieren, müssen Sie lediglich die Option **with (memory_optimized=on)** nach der Tabellendefinition angeben, wie im folgenden Beispiel gezeigt wird. Dann haben Sie eine speicheroptimierte Version der JSON-Sammlung erstellt.

```sql
create table WebSite.Logs (
  _id bigint identity primary key nonclustered,
  log nvarchar(4000)
) with (memory_optimized=on)
```

Eine speicheroptimierte Tabelle ist die beste Option für häufig geänderte Dokumente. Wenn Sie speicheroptimierte Tabellen in Erwägung ziehen, sollten Sie auch die Leistung berücksichtigen. Verwenden Sie für die JSON-Dokumente in Ihren speicheroptimierten Sammlungen möglichst NVARCHAR(4000) anstelle von NVARCHAR(max), da die Leistung hierdurch deutlich verbessert werden kann.

Wie bei klassischen Tabellen können Sie durch berechnete Spalten Indizes für die Felder, die Sie in speicheroptimierten Tabellen verfügbar machen, hinzufügen. Beispiel:

```sql
create table WebSite.Logs (

  _id bigint identity primary key nonclustered,
  log nvarchar(4000),

  severity AS cast(JSON_VALUE(log, '$.severity') as tinyint) persisted,
  index ix_severity (severity)

) with (memory_optimized=on)
```

Um die Leistung zu maximieren, sollten Sie den JSON-Wert in den kleinstmöglichen Typ umwandeln, der zum Speichern des Eigenschaftswerts verwendet werden kann. Im vorherigen Beispiel wurde **tinyint** verwendet.

Sie können auch SQL-Abfragen, die JSON-Dokumente aktualisieren, in gespeicherte Prozeduren einbinden, um von den Vorteilen der nativen Kompilierung zu profitieren. Beispiel:

```sql
CREATE PROCEDURE WebSite.UpdateData(@Id int, @Property nvarchar(100), @Value nvarchar(100))
WITH SCHEMABINDING, NATIVE_COMPILATION

AS BEGIN
    ATOMIC WITH (transaction isolation level = snapshot,  language = N'English')

    UPDATE WebSite.Logs
    SET log = JSON_MODIFY(log, @Property, @Value)
    WHERE _id = @Id;

END
```

Diese nativ kompilierte Prozedur erstellt anhand der Abfrage einen DLL-Code, der die Abfrage ausführt. Mit einer nativ kompilierten Prozedur können Sie schneller Daten abfragen und aktualisieren.

## <a name="conclusion"></a>Zusammenfassung

Durch native JSON-Funktionen in SQL Server und SQL-Datenbank können JSON-Dokumente wie in NoSQL-Datenbanken verarbeitet werden. Datenbanken – ganz gleich, ob relationale oder NoSQL-Datenbanken – weisen Vor- und Nachteile hinsichtlich der Verarbeitung von JSON-Daten auf. Der größte Vorteil bei der Speicherung von JSON-Dokumenten in SQL Server oder SQL-Datenbank besteht in der vollständigen Unterstützung für SQL-Sprachen. Sie können mithilfe der umfangreichen Transact-SQL-Sprache Daten verarbeiten und eine Vielzahl von Speicheroptionen konfigurieren (von Columnstore-Indizes für umfangreiche Komprimierungen und schnelle Analysen bis hin zu speicheroptimierten Tabellen für eine Verarbeitung ohne Sperren). Gleichzeitig profitieren Sie von ausgereiften Sicherheits- und Internationalisierungsfeatures, die Sie in Ihrem NoSQL-Szenario einfach wiederverwenden können. Die in diesem Artikel beschriebenen Gründe sind überzeugende Argumente dafür, die Speicherung von JSON-Dokumenten in SQL Server oder SQL-Datenbank in Erwägung zu ziehen.

## <a name="learn-more-about-json-in-sql-server-and-azure-sql-database"></a>Weitere Informationen zu JSON in SQL Server und Azure SQL-Datenbank  
  
### <a name="microsoft-videos"></a>Microsoft-Videos

Eine visuelle Einführung in die JSON-Unterstützung, die in SQL Server und Azure SQL-Datenbank integriert ist, finden Sie in den folgenden Videos:

-   [SQL Server 2016 and JSON Support](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support)

-   [Using JSON in SQL Server 2016 and Azure SQL Database](https://channel9.msdn.com/Shows/Data-Exposed/Using-JSON-in-SQL-Server-2016-and-Azure-SQL-Database)

-   [JSON as a bridge between NoSQL and relational worlds](https://channel9.msdn.com/events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds)
