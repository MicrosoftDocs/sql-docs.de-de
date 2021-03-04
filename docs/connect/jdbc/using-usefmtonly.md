---
description: Abrufen von ParameterMetaData über useFmtOnly
title: Abrufen von ParameterMetaData über useFmtOnly | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: ''
caps.latest.revision: ''
author: rene-ye
ms.author: v-reye
manager: kenvh
ms.openlocfilehash: a88707cc4679343c2e29a86ecc3a9efb8a9719dd
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837450"
---
# <a name="retrieving-parametermetadata-via-usefmtonly"></a>Abrufen von ParameterMetaData über useFmtOnly
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Der Microsoft JDBC-Treiber für SQL Server bietet eine alternative Möglichkeit zum Abfragen von Parametermetadaten vom Server: **useFmtOnly**. Diese Funktion wurde erstmals in Version 7.4 des Treibers eingeführt und ist als Problemumgehung für bekannte Probleme in `sp_describe_undeclared_parameters` erforderlich.
  
  Der Treiber verwendet primär die gespeicherte Prozedur `sp_describe_undeclared_parameters`, um Parametermetadaten abzufragen. Dies ist in den meisten Fällen die empfohlene Vorgehensweise für das Abrufen von Parametermetadaten. Bei folgenden Anwendungsfällen wird die Ausführung der gespeicherten Prozedur jedoch aktuell mit einem Fehler abgebrochen:
  
-   Bei Always Encrypted-Spalten
  
-   Für temporäre Tabellen und Tabellenvariablen
  
-   Bei Ansichten 
  
  In diesen Fällen sollte die SQL-Abfrage des Benutzers auf Parameter und Tabellenziele analysiert und anschließend eine `SELECT`-Abfrage mit aktiviertem `FMTONLY` ausgeführt werden. Der folgende Ausschnitt veranschaulicht diese Funktion:
  
```sql
--create a normal table 'Foo' and a temporary table 'Bar'
CREATE TABLE Foo(c1 int);
CREATE TABLE #Bar(c1 int);

EXEC sp_describe_undeclared_parameters N'SELECT * FROM Foo WHERE c1 = @p0' --works fine
EXEC sp_describe_undeclared_parameters N'SELECT * FROM #Bar WHERE c1 = @p0' --fails with "Invalid object name '#Bar'"

SET FMTONLY ON;
SELECT c1 FROM Foo; --works
SET FMTONLY OFF;
SET FMTONLY ON;
SELECT c1 FROM #Bar; --works
SET FMTONLY OFF;
```
 
## <a name="turning-the-feature-onoff"></a>Aktivieren/Deaktivieren der Funktion 
 Die Funktion **useFmtOnly** ist standardmäßig deaktiviert. Benutzer können sie über die Verbindungszeichenfolge aktivieren, indem sie `useFmtOnly=true` angeben. Beispiel: `jdbc:sqlserver://<server>:<port>;databaseName=<databaseName>;user=<user>;password=<password>;useFmtOnly=true;`.
 
 Alternativ ist die Funktion über `SQLServerDataSource` verfügbar.
 ```java
SQLServerDataSource ds = new SQLServerDataSource();
ds.setServerName(<server>);
ds.setPortNumber(<port>);
ds.setDatabaseName("<databaseName>");
ds.setUser("<user>");
ds.setPassword("<password>");
ds.setUseFmtOnly(true);
try (Connection c = ds.getConnection()) {
    // do work with connection
}
 ```
 
 Die Funktion ist auch auf Anweisungsebene verfügbar. Benutzer können sie über `PreparedStatement.setUseFmtOnly(boolean)` aktivieren/deaktivieren.
> [!NOTE]  
>  Der Treiber priorisiert die Eigenschaft auf Anweisungsebene gegenüber der Eigenschaft auf Verbindungsebene.

## <a name="using-the-feature"></a>Verwenden der Funktion
  Wenn die Funktion aktiviert ist, verwendet der Treiber sie intern beim Abfragen von Parametermetadaten anstelle von `sp_describe_undeclared_parameters`. Der Endbenutzer muss keine weiteren Schritte ausführen.
```java
final String sql = "INSERT INTO #Bar VALUES (?)";
try (Connection c = DriverManager.getConnection(URL, USERNAME, PASSWORD)) {
    try (Statement s = c.createStatement()) {
        s.execute("CREATE TABLE #Bar(c1 int)");
    }
    try (PreparedStatement p1 = c.prepareStatement(sql); PreparedStatement p2 = c.prepareStatement(sql)) {
        ((SQLServerPreparedStatement) p1).setUseFmtOnly(true);
        ParameterMetaData pmd1 = p1.getParameterMetaData();
        System.out.println(pmd1.getParameterTypeName(1)); // prints int
        ParameterMetaData pmd2 = p2.getParameterMetaData(); // throws exception, Invalid object name '#Bar'
    }
}
```
> [!NOTE]  
>  Die Funktion unterstützt ausschließlich `SELECT/INSERT/UPDATE/DELETE`-Abfragen. Abfragen sollten mit einem der vier unterstützten Schlüsselwörter oder einem [allgemeinen Tabellenausdruck](../../t-sql/queries/with-common-table-expression-transact-sql.md) beginnen, auf das bzw. den eine der unterstützten Abfragen folgt. Parameter innerhalb von allgemeine Tabellenausdrücken werden nicht unterstützt.

## <a name="known-issues"></a>Bekannte Probleme
  Es gibt derzeit einige Probleme mit diesem Feature, die auf Mängel in der SQL-Analyselogik zurückzuführen sind. Diese Probleme werden möglicherweise in einem zukünftigen Update der Funktion behoben und sind im Folgenden mit entsprechenden Vorschlägen für Problemumgehungen dokumentiert.
  
A. Verwenden eines „vorwärts deklarierten“ Alias
```sql
CREATE TABLE Foo(c1 int)

DELETE fooAlias FROM Foo fooAlias WHERE c1 > ?; --Invalid object name 'fooAlias'

--Workaround #1: Specify AS keyword
DELETE fooAlias FROM Foo AS fooAlias WHERE c1 > ?;
--Workaround #2: Use the table name
DELETE Foo FROM Foo fooAlias WHERE c1 > ?;
```

B. Mehrdeutiger Spaltenname, wenn Tabellen gemeinsame Spaltennamen aufweisen
```sql
CREATE TABLE Foo(c1 int, c2 int, c3 int)
CREATE TABLE Bar(c1 int, c2 int, c3 int)

SELECT c1,c2 FROM Foo WHERE c3 IN (SELECT c3 FROM Bar WHERE c1 > ? and c2 < ? and c3 = ?); --Ambiguous Column Name

--Workaround: Use aliases
SELECT c1,c2 FROM Foo WHERE c3 IN (SELECT c3 FROM Bar b WHERE b.c1 = ? and b.c2 = ? and b.c3 = ?);
```

C. SELECT aus einer Unterabfrage mit Parametern
```sql

CREATE TABLE Foo(c1 int)

SELECT * FROM (SELECT * FROM Foo WHERE c1 = ?) WHERE c1 = ?; --Incorrect syntax near '?'

--Workaround: N/A
```

D: Unterabfragen in einer SET-Klausel
```sql
CREATE TABLE Foo(c1 int)

UPDATE Foo SET c1 = (SELECT c1 FROM Foo) WHERE c1 = ?; --Incorrect syntax near ')'

--Workaround: Add a 'delimiting' condition
UPDATE Foo SET c1 = (SELECT c1 FROM Foo HAVING (HASH JOIN)) WHERE c1 = ?;
```

## <a name="see-also"></a>Weitere Informationen  
 [Festlegen von Verbindungseigenschaften](../../connect/jdbc/setting-the-connection-properties.md)  
  
