---
description: Tabellen
title: Tabellen | Microsoft-Dokumentation
ms.custom: ''
ms.date: 09/18/2019
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- tables [SQL Server]
- table components [SQL Server]
ms.assetid: 82d7819c-b801-4309-a849-baa63083e83f
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6a12d62cd0494fdb28ae9f02b8124230c2a33cf1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351141"
---
# <a name="tables"></a>Tabellen
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

Tabellen sind Datenbankobjekte, die sämtliche in einer Datenbank enthaltenen Daten umfassen. Die Daten in den Tabellen sind, ähnlich wie in einer Kalkulationstabelle, logisch in Zeilen und Spalten angeordnet. Jede Zeile stellt einen eindeutigen Datensatz und jede Spalte ein Feld im Datensatz dar. Eine Tabelle, die z. B. die Angestelltendaten für ein Unternehmen enthält, könnte eine Zeile für jeden Angestellten sowie Spalten enthalten, die Informationen zu einzelnen Angestellten angeben, wie z. B. die Mitarbeiternummer, den Namen, die Adresse, die Berufsbezeichnung und die private Telefonnummer. 

- Die Anzahl der Tabellen in einer Datenbank ist nur durch die in einer Datenbank zulässige Anzahl an Objekten beschränkt (2.147.483.647). Eine standardmäßige benutzerdefinierte Tabelle kann bis zu 1.024 Spalten enthalten. Die Anzahl der Zeilen in der Tabelle wird nur durch die Speicherkapazität des Servers beschränkt. 

- Sie können der Tabelle und jeder Spalte in der Tabelle Eigenschaften zuweisen, um die zulässigen Daten und weitere Eigenschaften zu steuern. Sie können z. B. Einschränkungen für eine Spalte erstellen, um keine NULL-Werte zuzulassen, oder einen Standardwert bereitstellen, wenn ein Wert nicht angegeben wird. Sie können außerdem eine Schlüsseleinschränkung für die Tabelle zuweisen, die Eindeutigkeit erzwingt oder eine Beziehung zwischen Tabellen definiert. 

- Die Daten in der Tabelle können entweder nach Zeile oder Seite komprimiert werden. Durch die Datenkomprimierung können mehrere Zeilen auf einer Seite gespeichert werden. Weitere Informationen finden Sie unter [Data Compression](../../relational-databases/data-compression/data-compression.md). 

## <a name="types-of-tables"></a>Typen von Tabellen
 Neben der Standardrolle der grundlegenden benutzerdefinierten Tabellen bietet [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die folgenden Typen von Tabellen, die innerhalb einer Datenbank speziellen Zwecken dienen: 

### <a name="partitioned-tables"></a>Partitionierte Tabellen

Partitionierte Tabellen sind Tabellen, deren Daten horizontal in Einheiten aufgeteilt sind, die über mehrere Dateigruppen innerhalb einer Datenbank verteilt sein können. Durch die Partitionierung sind große Tabellen oder Indizes leichter zu verwalten, denn Sie können dadurch schnell und effizient auf Datenteilmengen zugreifen und sie verwalten, während die Integrität der gesamten Sammlung erhalten bleibt. Standardmäßig unterstützt [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] bis zu 15.000 Partitionen. Weitere Informationen finden Sie unter [partitionierte Tabellen und Indizes](../../relational-databases/partitions/partitioned-tables-and-indexes.md).

### <a name="temporary-tables"></a>Temporäre Tabellen

Temporäre Tabellen werden in **tempdb** gespeichert. Es gibt zwei Arten von temporären Tabellen: lokale und globale temporäre Tabellen. Sie unterscheiden sich hinsichtlich ihrer Namen, ihrer Sichtbarkeit und ihrer Verfügbarkeit. Lokale temporäre Tabellen weisen als erstes Zeichen ihres Namens ein einzelnes Nummernzeichen (#) auf. Sie sind nur im Rahmen der aktuellen Verbindung des Benutzers sichtbar und werden gelöscht, sobald der Benutzer die Verbindung mit der Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]trennt. Globale temporäre Tabellen weisen als erste Zeichen ihres Namens zwei Nummernzeichen (##) auf. Nachdem sie erstellt wurden, sind sie für jeden Benutzer sichtbar, und sie werden gelöscht, nachdem alle Benutzer, die auf diese Tabelle verweisen, die Verbindung mit der Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]getrennt haben. 


#### <a name="reduced-recompilations-for-workloads-using-temporary-tables-across-multiple-scopes"></a><a name="ctp23"></a> Weniger Neukompilierungen für Workloads mit temporären Tabellen für mehrere Bereiche

Bei allen Datenbank-Kompatibilitätsgraden verringert [!INCLUDE[ss2019](../../includes/sssql19-md.md)] die Neukompilierungen für Arbeitsauslastungen mit temporären Tabellen für mehrere Bereiche. Dieses Feature ist in Azure SQL-Datenbank beim Datenbank-Kompatibilitätsgrad 150 für alle Bereitstellungsmodelle ebenfalls aktiviert.  Vor der Einführung dieses Features, konnte das Verweisen auf eine temporäre Tabelle mit einer DML-Anweisung (`SELECT`, `INSERT`, `UPDATE`, `DELETE`) zu einer Neukompilierung der DML-Anweisung bei jeder Ausführung führen, wenn die temporäre Tabelle in einem Batch im äußeren Bereich erstellt wurde. Durch dieses Update führt SQL Server zusätzliche einfache Überprüfungen durch, um unnötige Neukompilierungen zu vermeiden:

- Überprüfung, ob es sich bei dem Modul im äußeren Bereich, das zur Kompilierzeit zur Erstellung der temporären Tabelle verwendet wurde, um das gleiche Modul handelt, das für nachfolgende Ausführungen verwendet wurde 
- Nachverfolgung von Änderungen an der Datendefinitionssprache (Data Definition Language, DDL), die bei der ersten Kompilierung vorgenommen wurden, und Vergleich mit den DDL-Vorgängen für nachfolgende Ausführungen

Dadurch können überflüssige Neukompilierungen vermieden und der CPU-Aufwand reduziert werden.

### <a name="system-tables"></a>Systemtabellen

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] speichert die Daten, welche die Konfiguration des Servers sowie alle seine Tabellen definieren, in einem bestimmten Satz von Tabellen, die als Systemtabellen bezeichnet werden. Benutzer können die Systemtabellen nicht direkt abfragen oder aktualisieren. Die in den Systemtabellen enthaltenen Informationen werden über die Systemsichten zur Verfügung gestellt. Weitere Informationen finden Sie unter [Systemsichten &#40;Transact-SQL&#41;](../../t-sql/language-reference.md). 
 
### <a name="wide-tables"></a>Breite Tabellen

Breite Tabellen verwenden [Sparsespalten](../../relational-databases/tables/use-sparse-columns.md) , um die möglichen Spalten einer Tabelle auf 30.000 zu erhöhen. Spalten mit geringer Dichte sind gewöhnliche Spalten, die einen optimierten Speicher für NULL-Werte haben. Spalten mit geringer Dichte reduzieren die Speicherplatzanforderungen von NULL-Werten auf Kosten eines erhöhten Aufwands, um Werte ungleich NULL abzurufen. Eine breite Tabelle besitzt einen definierten [Spaltensatz](../../relational-databases/tables/use-column-sets.md), bei dem es sich um eine nicht typisierte XML-Darstellung handelt, die alle Sparsespalten einer Tabelle in einer strukturierten Ausgabe kombiniert. Die Anzahl der Indizes und Statistiken wird ebenfalls auf 1.000 bzw. 30.000 erhöht. Die maximale Größe einer breiten Tabelle beträgt normalerweise 8.019 Byte. Deshalb sollten die meisten Daten einer Zeile NULL sein. Für eine breite Tabelle beträgt die maximale Anzahl von Spalten ohne geringe Dichte zuzüglich der berechneten Spalten weiterhin 1.024. 

Bei breiten Tabellen treten folgende Leistungsauswirkungen auf:

- Breite Tabellen können die Kosten für die Verwaltung der Indizes in der Tabelle erhöhen. Es ist ratsam, die Anzahl an Indizes für eine breite Tabelle auf die Indizes zu beschränken, die für die Geschäftslogik erforderlich sind. Wenn die Anzahl von Indizes zunimmt, gilt dies auch für die Anforderungen an DML-Kompilierzeit und Arbeitsspeicher. Bei nicht gruppierten Indizes sollte es sich um gefilterte Indizes handeln, die auf Datenteilmengen angewendet werden. Weitere Informationen finden Sie unter [erstellen gefilterter Indizes](../../relational-databases/indexes/create-filtered-indexes.md). 

- Anwendungen können breiten Tabellen dynamisch Spalten hinzufügen und daraus entfernen. Wenn Spalten hinzugefügt oder entfernt werden, werden kompilierte Abfragepläne ebenfalls als ungültig erklärt. Es ist ratsam, eine Anwendung gemäß der vorhergesagten Arbeitsauslastung zu entwerfen, damit Schemaänderungen minimiert werden. 

- Wenn Daten einer breiten Tabelle hinzugefügt oder daraus entfernt werden, kann dies die Leistung beeinträchtigen. Anwendungen müssen für die vorhergesagte Arbeitsauslastung entworfen werden, damit Änderungen an den Tabellendaten minimiert werden. 

- Beschränken Sie die Ausführung von DML-Anweisungen für eine breite Tabelle, bei denen mehrere Zeilen eines Gruppierungsschlüssels aktualisiert werden. Diese Anweisungen können für die Kompilierung und Ausführung beträchtliche Speicherressourcen erfordern. 

- Partitionswechselvorgänge für breite Tabellen können langsam sein und für die Verarbeitung ggf. eine große Menge an Arbeitsspeicher erfordern. Die Leistungs- und Arbeitsspeicheranforderungen sind proportional zur Gesamtzahl der Spalten in der Quell- und Zielpartition. 

- Updatecursor, die bestimmte Spalten in einer breiten Tabelle aktualisieren, sollten die Spalten explizit in der FOR UPDATE-Klausel aufführen. Dies trägt zur Optimierung der Leistung bei der Verwendung von Cursorn bei. 

## <a name="common-table-tasks"></a>Allgemeine Tabellentasks
 Die folgende Tabelle enthält Links zu häufigen Tasks, die dem Erstellen oder Ändern einer Tabelle zugeordnet sind. 

|Tabellentasks|Thema|
|-----------------|-----------|
|Beschreibt, wie eine Tabelle erstellt wird.|[Erstellen von Tabellen &#40;Datenbank-Engine&#41;](../../relational-databases/tables/create-tables-database-engine.md)|
|Beschreibt, wie eine Tabelle gelöscht wird.|[Löschen von Tabellen &#40;Datenbank-Engine&#41;](../../relational-databases/tables/delete-tables-database-engine.md)|
|Beschreibt, wie eine neue Tabelle erstellt wird, die einige oder alle Spalten einer vorhandenen Tabelle enthalten.|[Duplizieren von Tabellen](../../relational-databases/tables/duplicate-tables.md)|
|Beschreibt, wie eine Tabelle umbenannt wird.|[Umbenennen von Tabellen &#40;Datenbank-Engine&#41;](../../relational-databases/tables/rename-tables-database-engine.md)|
|Beschreibt, wie die Eigenschaften der Tabelle angezeigt werden.|[Anzeigen der Tabellendefinition](../../relational-databases/tables/view-the-table-definition.md)|
|Beschreibt, wie ermittelt wird, ob andere Objekte, z. B. eine Sicht oder gespeicherte Prozedur, von einer Tabelle abhängen.|[Anzeigen der Abhängigkeiten einer Tabelle](../../relational-databases/tables/view-the-dependencies-of-a-table.md)|

 Die folgende Tabelle enthält Links zu häufigen Tasks, die dem Erstellen oder Ändern von Spalten in einer Tabelle zugeordnet sind. 

|Spaltentasks|Thema|
|------------------|-----------|
|Beschreibt, wie einer vorhandenen Tabelle Spalten hinzugefügt werden.|[Hinzufügen von Spalten zu einer Tabelle &#40;Datenbank-Engine&#41;](../../relational-databases/tables/add-columns-to-a-table-database-engine.md)|
|Beschreibt, wie Spalten aus einer Tabelle gelöscht werden.|[Spalten aus einer Tabelle löschen](../../relational-databases/tables/delete-columns-from-a-table.md)|
|Beschreibt, wie der Name einer Spalte geändert wird.|[Umbenennen von Spalten &#40;Datenbank-Engine&#41;](../../relational-databases/tables/rename-columns-database-engine.md)|
|Beschreibt, wie Spalten einer Tabelle in eine andere Tabelle kopiert werden. Sie können entweder nur die Spaltendefinition oder die Definition und Daten kopieren.|[Kopieren von Spalten aus einer Tabelle in eine andere &#40;Datenbank-Engine&#41;](../../relational-databases/tables/copy-columns-from-one-table-to-another-database-engine.md)|
|Beschreibt, wie eine Spaltendefinition durch Ändern des Datentyps oder anderer Eigenschaften geändert wird.|[Ändern von Spalten &#40;Datenbank-Engine&#41;](../../relational-databases/tables/modify-columns-database-engine.md)|
|Beschreibt, wie die Reihenfolge, in der die Spalten angezeigt werden, geändert wird.|[Ändern der Spaltenreihenfolge einer Tabelle](../../relational-databases/tables/change-column-order-in-a-table.md)|
|Beschreibt, wie eine berechnete Spalte in einer Tabelle erstellt wird.|[Angeben von berechneten Spalten in einer Tabelle](../../relational-databases/tables/specify-computed-columns-in-a-table.md)|
|Beschreibt, wie ein Standardwert für eine Spalte angegeben wird. Dieser Wert wird verwendet, wenn kein anderer Wert angegeben wird.|[Angeben von Standardwerten für Spalten](../../relational-databases/tables/specify-default-values-for-columns.md)|

## <a name="see-also"></a>Weitere Informationen
 [Primärschlüssel- und Fremdschlüsseleinschränkungen](../../relational-databases/tables/primary-and-foreign-key-constraints.md) [UNIQUE- und CHECK-Einschränkungen](../../relational-databases/tables/unique-constraints-and-check-constraints.md)