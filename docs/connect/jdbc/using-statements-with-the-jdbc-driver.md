---
title: Verwenden von Anweisungen mit dem JDBC-Treiber
description: Erfahren Sie, wie der Microsoft JDBC-Treiber für SQL Server zum Ausführen von SQL-Anweisungen und gespeicherten Prozeduren zum Durchführen von Datenbankoperationen verwendet werden kann.
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 7f8f3e8f-841e-4449-9154-b5366870121f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: de6f5340375a3a9a5c39e2405943729b997e9198
ms.sourcegitcommit: 129f8574eba201eb6ade1f1620c6b80dfe63b331
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2020
ms.locfileid: "87435295"
---
# <a name="using-statements-with-the-jdbc-driver"></a>Verwenden von Anweisungen mit dem JDBC-Treiber

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

[!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] bietet verschiedene Möglichkeiten, um Daten in einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbank zu verarbeiten. Mit dem JDBC-Treiber können SQL-Anweisungen für die Datenbank ausgeführt oder gespeicherte Prozeduren in der Datenbank aufgerufen werden, die sowohl Eingabe- als Ausgabeparameter verwenden. Der JDBC-Treiber unterstützt außerdem die Verwendung von SQL-Escapesequenzen, Updatezählungen, automatisch generierten Schlüsseln und die Ausführung von Updates in einer Batchoperation.  
  
Der JDBC-Treiber umfasst drei Klassen zum Abrufen von Daten aus einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbank:  
  
1. [SQLServerStatement](../../connect/jdbc/reference/sqlserverstatement-class.md): Ausführen von SQL-Anweisungen ohne Parameter.  
  
2. [SQLServerPreparedStatement](../../connect/jdbc/reference/sqlserverpreparedstatement-class.md): Ausführen von kompilierten SQL-Anweisungen, die IN-Parameter enthalten können (geerbt von SQLServerStatement).  
  
3. [SQLServerCallableStatement](../../connect/jdbc/reference/sqlservercallablestatement-class.md): Ausführen von gespeicherten Prozeduren, die IN- und/oder OUT-Parameter enthalten können (geerbt von SQLServerPreparedStatement).  
  
 In diesem Abschnitt wird beschrieben, wie Sie mit den drei Anweisungsklassen Daten in einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbank verarbeiten können.  
  
## <a name="in-this-section"></a>In diesem Abschnitt  

| Thema                                                                                                    | BESCHREIBUNG                                                                                                                                            |
| -------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| [Verwenden von Anweisungen mit SQL](../../connect/jdbc/using-statements-with-sql.md)                             | Beschreibt die Verwendung von SQL-Anweisungen mit dem JDBC-Treiber, um Daten in einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbank zu verarbeiten.    |
| [Verwenden von Anweisungen mit gespeicherten Prozeduren](../../connect/jdbc/using-statements-with-stored-procedures.md) | Beschreibt die Verwendung von gespeicherten Prozeduren mit dem JDBC-Treiber, um Daten in einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbank zu verarbeiten. |
| [Verwenden von mehreren Resultsets](../../connect/jdbc/using-multiple-result-sets.md)                           | Beschreibt die Verwendung des JDBC-Treibers, um Daten aus mehreren Resultsets abzurufen.                                                                       |
| [Verwenden von SQL-Escapesequenzen](../../connect/jdbc/using-sql-escape-sequences.md)                           | Beschreibt die Verwendung von SQL-Escapesequenzen, wie z. B. Datums- und Zeitliteralen und -funktionen.                                                               |
| [Verwenden von automatisch generierten Schlüsseln](../../connect/jdbc/using-auto-generated-keys.md)                             | Beschreibt die Verwendung von automatisch generierten Schlüsseln.                                                                                                     |
| [Ausführen von Batchvorgängen](../../connect/jdbc/performing-batch-operations.md)                         | Beschreibt die Verwendung des JDBC-Treibers, um Batchoperationen auszuführen.                                                                                      |
| [Verarbeiten komplexer Anweisungen](../../connect/jdbc/handling-complex-statements.md)                         | Beschreibt die Verwendung des JDBC-Treibers, um komplexe Anweisungen auszuführen, die vielfältige Tasks ausführen und verschiedene Datentypen zurückgeben können.               |
  
## <a name="see-also"></a>Weitere Informationen

[Übersicht über den JDBC-Treiber](../../connect/jdbc/overview-of-the-jdbc-driver.md)  
