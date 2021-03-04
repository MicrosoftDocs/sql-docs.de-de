---
description: Analysieren der Ergebnisse
title: Analysieren der Ergebnisse | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: ''
author: rene-ye
ms.author: v-reye
manager: kenvh
ms.openlocfilehash: 7b56223594d1126c373b4fd8b24dabc1e81dab67
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837080"
---
# <a name="parsing-the-results"></a>Analysieren der Ergebnisse

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

In diesem Artikel wird beschrieben, wie die in einer SQL Server-Abfrage zurückgegebenen Ergebnisse von Benutzern verarbeitet werden sollten.

## <a name="update-counts-and-result-sets"></a>Anzahl von Updates und Resultsets

In diesem Abschnitt werden die beiden häufigsten Ergebnisse erläutert, die von SQL Server zurückgegeben werden: Anzahl von Updates und Resultsets. Im Allgemeinen führt jede von einem Benutzer ausgeführte Abfrage dazu, dass eines dieser Ergebnisse zurückgegeben wird. Es wird davon ausgegangen, dass Benutzer beide verarbeiten, wenn sie die Ergebnisse untersuchen.

Der folgende Code ist ein Beispiel dafür, wie ein Benutzer alle Ergebnisse des Servers durchlaufen kann:
```java
try (Connection connection = DriverManager.getConnection(URL); Statement statement = connection.createStatement()) {
    boolean resultsAvailable = statement.execute(USER_SQL);
    int updateCount = -2;
    while (true) {
        updateCount = statement.getUpdateCount();
        if (!resultsAvailable && updateCount == -1)
            break;
        if (resultsAvailable) {
            // handle ResultSet
        } else {
            // handle Update Count
        }
        resultsAvailable = statement.getMoreResults();
    }
}
```

## <a name="exceptions"></a>Ausnahmen
Wenn Sie eine Anweisung ausführen, die zu einem Fehler oder einer Informationsmeldung führt, reagiert SQL Server unterschiedlich. Die Reaktion hängt in diesem Fall davon ab, ob ein Ausführungsplan generiert werden kann oder nicht. Die Fehlermeldung kann unmittelbar nach der Ausführung der Anweisung angezeigt werden, oder es kann ein separates Resultset erforderlich sein. Im letzteren Fall müssen die Anwendungen das Resultset analysieren, um die Ausnahme abzurufen.

Wenn SQL Server keinen Ausführungsplan generieren kann, wird die Ausnahme sofort ausgelöst.

```java
String SQL = "SELECT * FROM nonexistentTable;";
try (Statement statement = connection.createStatement();) {
    statement.execute(SQL);
} catch (SQLException e) {
    e.printStackTrace();
}
```

Wenn SQL Server eine Fehlermeldung in einem Resultset zurückgibt, muss das Resultset verarbeitet werden, um die Ausnahme abzurufen.

```java
String SQL = "SELECT 1/0;";
try (Statement statement = connection.createStatement();) {
    boolean hasResult = statement.execute(SQL);
    if (hasResult) {
        try (ResultSet rs = statement.getResultSet()) {
            // Exception is thrown on next().
            while (rs.next()) {}
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

Wenn bei der Ausführung einer Anweisung mehrere Resultsets generiert werden, muss jedes Resultset verarbeitet werden, bis das Resultset mit der Ausnahme erreicht wird.

```java
String SQL = "SELECT 1; SELECT * FROM nonexistentTable;";
try (Statement statement = connection.createStatement();) {
    // Does not throw an exception on execute().
    boolean hasResult = statement.execute(SQL);
    while (hasResult) {
        try (ResultSet rs = statement.getResultSet()) {
            while (rs.next()) {
                System.out.println(rs.getString(1));
            }
        }
        // Moves the next result set that generates the exception.
        hasResult = statement.getMoreResults();
    }
} catch (SQLException e) {
    e.printStackTrace();
}
```

Im Fall von `String SQL = "SELECT * FROM nonexistentTable; SELECT 1;";` wird die Ausnahme unmittelbar bei `execute()` ausgelöst, und `SELECT 1` wird nicht mehr ausgeführt.

Wenn der von SQL Server zurückgegebene Fehler den Schweregrad `0` bis `9` aufweist, wird er als Informationsmeldung betrachtet und als `SQLWarning` zurückgegeben.

```java
String SQL = "RAISERROR ('WarningLevel5', 5, 2);";
try (Statement statement = connection.createStatement();) {
    boolean hasResult = statement.execute(SQL);
    SQLWarning warning = statement.getWarnings();
    System.out.println(warning);
}
```

## <a name="see-also"></a>Weitere Informationen

[Übersicht über den JDBC-Treiber](../../connect/jdbc/overview-of-the-jdbc-driver.md)
