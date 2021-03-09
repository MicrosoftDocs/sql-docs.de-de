---
title: Sicherheit auf Zeilenebene
description: Mithilfe der Sicherheit auf Zeilenebene können Sie den Zugriff auf Zeilen in einer Datenbanktabelle in SQL Server anhand der Gruppenmitgliedschaft oder des Ausführungskontext steuern.
ms.custom: ''
ms.date: 09/01/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- access control predicates
- row level security
- security [SQL Server], predicate based access control
- row level security described
- predicate based security
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3f54ca9be0a75912c716f12b56c8b3848b151358
ms.sourcegitcommit: 0bcda4ce24de716f158a3b652c9c84c8f801677a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2021
ms.locfileid: "102247538"
---
# <a name="row-level-security"></a>Sicherheit auf Zeilenebene

[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  ![Grafik zur Sicherheit auf Zeilenebene](../../relational-databases/security/media/row-level-security-graphic.png "Grafik zur Sicherheit auf Zeilenebene")  
  
Mithilfe der Sicherheit auf Zeilenebene können Sie den Zugriff auf Zeilen in einer Datenbanktabelle anhand der Gruppenmitgliedschaft oder des Ausführungskontext steuern.
  
Eine zeilenbasierte Sicherheit vereinfacht den Entwurf und die Sicherheitscodierung in Ihrer Anwendung. Mithilfe der Sicherheit auf Zeilenebene (Row-Level Security, RLS) können Sie Einschränkungen in den Datenzeilenzugriff implementieren. Beispielsweise können Sie sicherstellen, dass Mitarbeiter nur auf Datenzeilen zugreifen können, die für ihre Abteilung relevant sind. Ein weiteres Beispiel: Sie können den Zugriff von Kunden auf Daten beschränken, die für ihr Unternehmen von Bedeutung sind.  
  
Die Datenbeschränkungszugriffslogik befindet sich auf der Datenbankebene, statt fern der Daten auf einer anderen Anwendungsebene. Das Datenbanksystem wendet die Zugriffsbeschränkungen bei jedem Zugriffsversuch auf Daten aus einer beliebigen Ebene an. Dadurch bietet Ihr Sicherheitssystem eine geringere Angriffsfläche und ist zuverlässiger und robuster.  
  
Implementieren Sie RLS, indem Sie die [CREATE SECURITY POLICY](../../t-sql/statements/create-security-policy-transact-sql.md)[!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung und Prädikate verwenden, die als [Inline-Tabellenwertfunktionen](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md) erstellt werden.  

[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)], [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] ([Abrufen](/azure/azure-sql/database/features-comparison?WT.mc_id=TSQL_GetItTag)), [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].
  
> [!NOTE]
> Azure Synapse unterstützt nur Filterprädikate. Blockprädikate werden in Azure Synapse derzeit nicht unterstützt.

## <a name="description"></a><a name="Description"></a> Beschreibung

RLS unterstützt zwei Arten von Sicherheitsprädikaten.  
  
- FILTER-Prädikate filtern automatisch die Zeilen, die für Lesevorgänge (SELECT, UPDATE und DELETE) zur Verfügung stehen.  
  
- BLOCK-Prädikate blockieren explizit Schreibvorgänge (AFTER INSERT, AFTER UPDATE, BEFORE UPDATE, BEFORE DELETE), die gegen das Prädikat verstoßen.  
  
 Der Zugriff auf Daten auf Zeilenebene in einer Tabelle wird durch ein Sicherheitsprädikat beschränkt, das als Inline-Tabellenwertfunktion definiert ist. Die Funktion wird dann aufgerufen und durch eine Sicherheitsrichtlinie erzwungen. Für Filterprädikate gilt: Der Anwendung ist nicht „bewusst“, dass Zeilen aus dem Resultset herausgefiltert wurden. Wenn alle Zeilen gefiltert wurden, wird eine NULL-Menge zurückgegeben. Für BLOCK-Prädikate gilt: Alle Vorgänge, die gegen das Prädikat verstoßen, misslingen mit einem Fehler.  
  
 Filterprädikate werden beim Lesen von Daten aus der Basistabelle angewendet. Sie betreffen alle GET-Vorgänge: **SELECT**, **DELETE** und **UPDATE**. Benutzer können gefilterte Zeilen weder auswählen noch löschen. Der Benutzer kann gefilterte Zeilen nicht aktualisieren. Zeilen können jedoch so aktualisiert werden, dass sie im Anschluss gefiltert werden. BLOCK-Prädikate betreffen alle Schreibvorgänge.  
  
- Die Prädikate AFTER INSERT und AFTER UPDATE können Benutzer am Ändern von Zeilen in Werte hindern, die gegen das Prädikat verstoßen.  
  
- Prädikate des Typs BEFORE UPDATE können Benutzer am Ändern von Zeilen in Werte hindern, die derzeit gegen das Prädikat verstoßen.  
  
- Prädikate des Typs BEFORE DELETE können Löschvorgänge blockieren.  
  
 FILTER- und BLOCK-Prädikate und Sicherheitsrichtlinien weisen folgendes Verhalten auf:  
  
- Sie können eine Prädikatfunktion definieren, die mit einer anderen Tabelle verknüpft wird und/oder eine Funktion aufruft. Wenn die Sicherheitsrichtlinie mit `SCHEMABINDING = ON` (Standardeinstellung) erstellt wird, ist die Verknüpfung oder Funktion über die Abfrage erreichbar und funktioniert wie erwartet, ohne dass zusätzliche Berechtigungsüberprüfungen durchgeführt werden zu müssen. Wenn die Sicherheitsrichtlinie mit `SCHEMABINDING = OFF` erstellt wird, benötigen Benutzer **SELECT**-Berechtigungen für diese zusätzlichen Tabellen und Funktionen, um die Zieltabelle abfragen zu können. Wenn die Prädikatfunktion eine CLR-Skalarwertfunktion aufruft, wird zusätzlich die **EXECUTE**-Berechtigung benötigt.
  
- Sie können eine Abfrage auf eine Tabelle anwenden, für die ein Sicherheitsprädikat zwar definiert, jedoch deaktiviert ist. Zeilen, die gefiltert oder gesperrt wurden, sind nicht betroffen.  
  
- Wenn ein DBO-Benutzer, ein Mitglied der **db_owner**-Rolle oder der Tabellenbesitzer eine Tabelle abfragt, für die eine Sicherheitsrichtlinie definiert und aktiviert ist, werden die Zeilen gemäß der definierten Sicherheitsrichtlinie gefiltert oder gesperrt.  
  
- Versuche, das Schema einer Tabelle zu ändern, die durch eine Sicherheitsrichtlinie an ein Schema gebunden ist, führen zu einem Fehler. Allerdings können vom Prädikat nicht referenzierte Spalten geändert werden.  
  
- Wenn einer Tabelle ein Prädikat hinzugefügt wird, in der für den angegebenen Vorgang bereits ein Prädikat definiert ist, wird ein Fehler verursacht. Dies geschieht unabhängig davon, ob das Prädikat aktiviert ist oder nicht.
  
- Beim Versuch eine Funktion zu ändern, die innerhalb einer schemagebundenen Sicherheitsrichtlinie als Prädikat für eine Tabelle verwendet wird, wird ein Fehler verursacht.  
  
- Das Definieren mehrerer aktiver Sicherheitsrichtlinien, die nicht überlappende Prädikate enthalten, kann erfolgreich ausgeführt werden.  
  
 FILTER-Prädikate weisen folgendes Verhalten auf:  
  
- Definieren Sie eine Sicherheitsrichtlinie, die die Zeilen einer Tabelle filtert. Die Anwendung beachtet keine Zeilen, die nach **SELECT**-, **UPDATE**- und **DELETE**-Vorgängen gefiltert wurden. Dies gilt selbst dann, wenn alle Zeilen gefiltert wurden. Die Anwendung kann mit **INSERT** selbst dann Zeilen einfügen, wenn sie bei einem anderen Vorgang gefiltert werden.  
  
 BLOCK-Prädikate weisen folgendes Verhalten auf:  
  
- BLOCK-Prädikate für UPDATE werden in getrennte Vorgänge für BEFORE und AFTER unterteilt. Folglich können Benutzer beispielsweise nicht daran gehindert werden, eine Zeile in einen Wert zu ändern, der höher als der aktuelle ist. Wenn diese Art von Logik erforderlich ist, müssen Sie Trigger mit den Zwischentabellen des Typs [DELETED und INSERTED](../triggers/use-the-inserted-and-deleted-tables.md) verwenden, um gemeinsam auf die alten und neuen Werte zu verweisen.  
  
- Der Optimierer überprüft kein Blockprädikat des Typs AFTER UPDATE, wenn keine der von der Prädikatfunktion verwendeten Spalten geändert wurde. Beispiel: Alice darf kein Gehalt in einen Wert über 100.000 ändern. Alice kann die Adresse eines Mitarbeiters ändern, dessen Gehalt bereits über 100.000 liegt, solange die Spalten, auf die im Prädikat verwiesen wird, nicht geändert wurden.  
  
- An den APIs für Massenvorgänge, einschließlich BULK INSERT, sind keine Änderungen erfolgt. Dies bedeutet, dass BLOCK-Prädikate des Typs AFTER INSERT für Masseneinfügevorgänge genauso wie für herkömmliche Einfügevorgänge gelten.  
  
## <a name="use-cases"></a><a name="UseCases"></a> Einsatzgebiete

 Hier sind Entwurfsbeispiele dazu, wie RLS verwendet werden kann:  
  
- Für ein Krankenhaus kann eine Sicherheitsrichtlinie definiert werden, die dem Pflegepersonal ausschließlich die Anzeige von Datenzeilen ermöglicht, die sich auf ihre Patienten beziehen.  
  
- Für eine Bank kann eine Richtlinie definiert werden, die den Zugriff auf Finanzdatenzeilen basierend auf dem Geschäftsbereich oder der Rolle eines Mitarbeiters innerhalb des Unternehmens beschränkt.  
  
- Eine Anwendung mit mehreren Mandanten kann eine Richtlinie zum Erzwingen einer logischen Trennung der einzelnen Datenzeilen der Mandanten aus jeder anderen Mandanten-Zeile erstellen. Effizienzen werden durch den Datenspeicher für viele Mandanten in einer einzelnen Tabelle erreicht. Jeder Mandant kann nur die eigenen Datenzeilen anzeigen.  
  
 RLS-Filterprädikate sind funktional äquivalent zum Anhängen einer **WHERE** -Klausel. Bei dem Prädikat kann es sich um komplexe Geschäftsabläufe handeln oder die Klausel kann so einfach sein wie das `WHERE TenantId = 42`.  
  
 Formaler ausgedrückt führt RLS eine prädikatbasierte Zugriffssteuerung ein. Sie ermöglicht eine flexible, zentrale und prädikatbasierte Auswertung. Das Prädikat kann auf Metadaten oder beliebigen anderen Kriterien basieren, die der Administrator für geeignet hält. Das Prädikat wird als Kriterium verwendet, um zu bestimmen, ob der Benutzer über die entsprechenden Zugriffsberechtigungen für die Daten basierend auf den Benutzerattributen verfügt. Mithilfe der prädikatbasierten Zugriffssteuerung kann eine bezeichnerbasierte Zugriffssteuerung implementiert werden.  
  
## <a name="permissions"></a><a name="Permissions"></a> Berechtigungen

 Für das Erstellen, Ändern oder Löschen von Sicherheitsrichtlinien ist die **ALTER ANY SECURITY POLICY** -Berechtigung erforderlich. Für das Erstellen oder Löschen einer Sicherheitsrichtlinie ist bei dem Schema die **ALTER** -Berechtigung erforderlich.  
  
 Darüber hinaus sind die folgenden Berechtigungen für jedes hinzugefügte Prädikat erforderlich:  
  
- **SELECT** - und **REFERENCES** -Berechtigungen für die Funktion, die als Prädikat verwendet wird.  
  
- **REFERENCES** -Berechtigung für die Zieltabelle, die an die Richtlinie gebunden wird.  
  
- **REFERENCES** -Berechtigung für jede Spalte in der Zieltabelle, die als Argument verwendet wird.  
  
 Sicherheitsrichtlinien gelten für alle Benutzer, einschließlich der Dbo-Benutzer in der Datenbank. Dbo-Benutzer können Sicherheitsrichtlinien ändern oder löschen, ihre Änderungen an Sicherheitsrichtlinien können jedoch überwacht werden. Wenn Benutzer mit ausgedehnten Berechtigungen (z. B. „sysadmin“ oder „db_owner“) alle Zeilen sehen müssen, um Datenprobleme zu beheben oder Daten zu überprüfen, muss die Sicherheitsrichtlinie entsprechend definiert werden.  
  
 Wenn eine Sicherheitsrichtlinie mit `SCHEMABINDING = OFF`erstellt wird, benötigen die Benutzer zum Abfragen der Zieltabelle die  **SELECT** - oder **EXECUTE** -Berechtigung für die Prädikatfunktion und alle weiteren Tabellen, Sichten oder Funktionen, die innerhalb der Prädikatfunktion verwendet werden. Wenn eine Sicherheitsrichtlinie mit `SCHEMABINDING = ON` erstellt wird (Standard), werden diese Berechtigungsprüfungen umgangen, wenn Benutzer die Zieltabelle abfragen.  
  
## <a name="best-practices"></a><a name="Best"></a> Bewährte Methoden  
  
- Es wird dringend empfohlen, ein separates Schema für die RLS-Objekte zu erstellen: Prädikatfunktionen und Sicherheitsrichtlinien. So lassen sich die Berechtigungen, die für diese speziellen Objekte erforderlich sind, von den Zieltabellen trennen. Eine weitere Trennung für verschiedene Richtlinien und Prädikatfunktionen ist möglicherweise in mehrinstanzenfähigen Datenbanken erforderlich, aber nicht als Standardeinstellung für jeden Fall.
  
- Die **ALTER ANY SECURITY POLICY**-Berechtigung sollte nur für Benutzer mit erhöhten Berechtigungen (z.B. ein Sicherheitsrichtlinienmanager) verwendet werden. Der Sicherheitsrichtlinienmanager benötigt keine **SELECT**-Berechtigung für die geschützten Tabellen.  
  
- Vermeiden Sie Konvertierungen in Prädikatfunktionen, um potenzielle Laufzeitfehler zu vermeiden.  
  
- Vermeiden Sie nach Möglichkeit Rekursionen in Prädikatfunktionen, um Leistungseinbußen zu vermeiden. Der Abfrageoptimierer versucht, direkte Rekursionen zu erkennen. Indirekte Rekursionen werden jedoch nicht zuverlässig gefunden. Eine indirekte Rekursion liegt vor, wenn eine zweite Funktion die Prädikatfunktion aufruft.  
  
- Vermeiden Sie übermäßige Tabellenverknüpfungen Prädikatfunktionen, um die Leistung zu maximieren.  
  
 Vermeiden Sie Prädikatlogik, die von sitzungsspezifischen [SET-Optionen](../../t-sql/statements/set-statements-transact-sql.md) abhängig: Wenngleich ihre Verwendung in der Praxis eher unwahrscheinlich ist, können Prädikatfunktionen, deren Logik von bestimmten sitzungsspezifischen **SET**-Optionen abhängt, Informationen preisgeben, wenn Benutzer in der Lage sind, beliebige Abfragen auszuführen. Beispiel: Eine Prädikatfunktion, die eine Zeichenfolge implizit in **datetime** konvertiert, kann unterschiedliche Zeilen basierend auf der Option **SET DATEFORMAT** für die aktuelle Sitzung filtern. Im Allgemeinen sollten Prädikatfunktionen die folgenden Regeln einhalten:  
  
- Prädikatfunktionen dürfen Zeichenfolgen nicht implizit in **date**, **smalldatetime**, **datetime**, **datetime2** oder **datetimeoffset** bzw. umgekehrt konvertieren, da diese Konvertierungen von den Optionen [SET DATEFORMAT &#40;Transact-SQL&#41;](../../t-sql/statements/set-dateformat-transact-sql.md) und [SET LANGUAGE &#40;Transact-SQL&#41;](../../t-sql/statements/set-language-transact-sql.md) beeinflusst werden. Verwenden Sie stattdessen die **CONVERT** -Funktion, und geben Sie den „style“-Parameter explizit an.  
  
- Prädikatfunktionen dürfen nicht vom Wert des ersten Tages der Woche abhängig sein, da dieser Wert von der Option [SET DATEFIRST &#40;Transact-SQL&#41;](../../t-sql/statements/set-datefirst-transact-sql.md) beeinflusst wird.  
  
- Prädikatfunktionen dürfen nicht von arithmetischen oder Aggregationsausdrücken abhängig sein, die bei einem Fehler (wie z. B. Überlauf oder Division durch 0) **NULL** zurückgeben, da dieses Verhalten von den Optionen [SET ANSI_WARNINGS &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-warnings-transact-sql.md), [SET NUMERIC_ROUNDABORT &#40;Transact-SQL&#41;](../../t-sql/statements/set-numeric-roundabort-transact-sql.md) und [SET ARITHABORT &#40;Transact-SQL&#41;](../../t-sql/statements/set-arithabort-transact-sql.md) beeinflusst wird.  
  
- Prädikatfunktionen dürfen verkettete Zeichenfolgen nicht mit **NULL** vergleichen, da dieses Verhalten von der Option [SET CONCAT_NULL_YIELDS_NULL &#40;Transact-SQL&#41;](../../t-sql/statements/set-concat-null-yields-null-transact-sql.md) beeinflusst wird.  

## <a name="security-note-side-channel-attacks"></a><a name="SecNote"></a>Sicherheitshinweis: Seitenkanalangriffe

### <a name="malicious-security-policy-manager"></a>Schädliche Sicherheitsrichtlinien-Manager

Es ist wichtig zu beachten, dass ein schädlicher Sicherheitsrichtlinien-Manager mit ausreichenden Berechtigungen zum Erstellen einer Sicherheitsrichtlinie auf eine vertrauliche Spalte und der Berechtigung zum Erstellen oder Ändern von Inline-Tabellenwertfunktionen mit einem anderen Benutzer zusammenwirken kann, der über ausgewählte Berechtigungen für eine Tabelle verfügt, um eine Datenexfiltration durchzuführen, indem schädliche Inline-Tabellenwertfunktionen erstellt werden, die dazu dienen sollen, Seitenkanalangriffe zu verwenden, um Daten abzuleiten. Solche Angriffe sind möglich, wenn sich Benutzer abgesprochen haben, oder wenn einem böswilligen Benutzer zu hohe Berechtigungen erteilt wurden. Zudem sind vermutlich mehrere Iterationen zum Anpassen der Richtlinie vonnöten. Dazu sind Berechtigungen zum Entfernen das Prädikats erforderlich, um die Schemabindung aufheben zu können. Gleichzeitig müssen die Inline-Tabellenwertfunktionen angepasst und SELECT-Anweisungen wiederholt für die Zieltabelle ausgeführt werden. Es wird empfohlen, Berechtigungen nach Bedarf zu beschränken und das System auf verdächtige Aktivitäten zu überwachen. Aktivitäten wie das ständige Ändern von Richtlinien und Inline-Tabellenwertfunktionen im Zusammenhang mit der Sicherheit auf Zeilenebene sollten überwacht werden.  
  
### <a name="carefully-crafted-queries"></a>Sorgfältig erstellte Abfragen

Es ist möglich, die Offenlegung von Informationen durch die Verwendung von sorgfältig erstellten Abfragen zu verursachen. Beispiel: `SELECT 1/(SALARY-100000) FROM PAYROLL WHERE NAME='John Doe'` würde einen schädlichen Benutzer wissen lassen, dass das Gehalt von John Doe 100.000 USD beträgt. Auch wenn ein Sicherheitsprädikat eingerichtet ist, um zu verhindern, dass ein schädlicher Benutzer die Gehälter anderer Personen direkt abfragen kann, kann der Benutzer bestimmen, wann die Abfrage eine Division-durch-Null-Ausnahme zurückgibt.  

## <a name="cross-feature-compatibility"></a><a name="Limitations"></a> Featureübergreifende Kompatibilität

 Im Allgemeinen funktioniert Sicherheit auf Zeilenebene featureübergreifend wie erwartet. Es gibt jedoch einige Ausnahmen. In diesem Abschnitt finden Sie verschiedene Hinweise und Vorsichtsmaßnahmen bei Verwenden von Sicherheit auf Zeilenebene mit bestimmten anderen Features von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
- **DBCC SHOW_STATISTICS** meldet Statistiken zu ungefilterten Daten, was zur Offenlegung von Informationen führen kann, die ansonsten durch eine Sicherheitsrichtlinie geschützt sind. Aus diesem Grund ist die Anzeige eines Statistikobjekts für eine Tabelle mit einer Richtlinie für Sicherheit auf Zeilenebene beschränkt. Der Benutzer muss die Tabelle besitzen oder Mitglied der festen Serverrolle „sysadmin“ bzw. der festen Datenbankrolle „db_owner“ oder „db_ddladmin“ sein.  
  
- **Filestream:** RLS ist nicht mit Filestream kompatibel.  
  
- **PolyBase:** RLS wird mit externen Tabellen in Azure Synapse und SQL Server 2019 CU1 oder höher unterstützt. 

- **Speicheroptimierte Tabellen:** Die Inline-Tabellenwertfunktion, die als Sicherheitsprädikat für eine speicheroptimierte Tabelle verwendet wird, muss mit der Option `WITH NATIVE_COMPILATION` definiert werden. Bei dieser Option werden von speicheroptimierten Tabellen nicht unterstützte Sprachfeatures gesperrt, und zur Erstellungszeit wird der entsprechende Fehler ausgelöst. Weitere Informationen finden Sie im Abschnitt **Sicherheit auf Zeilenebene** in [Einführung in speicheroptimierte Tabellen](../../relational-databases/in-memory-oltp/introduction-to-memory-optimized-tables.md).  
  
- **Indizierte Sichten:** Im allgemeinen können Sicherheitsrichtlinien basierend auf Sichten erstellt werden. Sichten können basierend auf Tabellen erstellt werden, die durch Sicherheitsrichtlinien gebunden sind. Allerdings können indizierte Sichten nicht basierend auf Tabellen erstellt werden, für die eine Sicherheitsrichtlinie gilt, da die Richtlinie bei Zeilensuchvorgängen über den Index umgangen würde.  
  
- **Change Data Capture:** Change Data Capture kann ganze Zeilen offenlegen, die für Mitglieder von **db_owner** oder Benutzer gefiltert werden sollen, die Mitglieder der Gating-Rolle sind. Diese wird angegeben, wenn CDC für eine Tabelle aktiviert ist (Hinweis: Sie können diese Funktion explizit auf **NULL** festlegen, damit alle Benutzer auf die Änderungsdaten zugreifen können). Im Endeffekt können **db_owner** und Mitglieder dieser Gating-Rolle alle Datenänderungen für eine Tabelle anzeigen, auch wenn für die Tabelle eine Sicherheitsrichtlinie gilt.  
  
- **Änderungsnachverfolgung:** Die Änderungsnachverfolgung kann zur Offenlegung des Primärschlüssels von Zeilen führen, die für Benutzer mit den Berechtigungen **SELECT** und **VIEW CHANGE TRACKING** gefiltert werden sollen. Tatsächliche Datenwerte werden nicht preisgegeben, sondern nur dass Spalte A für die Spalte mit dem Primärschlüssel B aktualisiert/eingefügt/gelöscht wurde. Dies ist problematisch, wenn der primäre Schlüssel ein vertrauliches Element enthält, z. B. eine US-Sozialversicherungsnummer. In der Praxis ist **CHANGETABLE** jedoch fast immer mit der ursprünglichen Tabelle verknüpft, um die neuesten Daten abzurufen.  
  
- **Volltextsuche:** Für Abfragen, die die folgenden Funktionen für Volltextsuche und semantische Suche verwenden, wird eine Leistungsbeeinträchtigung erwartet. Der Grund ist ein zusätzlicher Join, der eingeführt wird, um Sicherheit auf Zeilenebene anzuwenden und die Offenlegung der Primärschlüssel von Zeilen zu vermeiden, die gefiltert werden sollen: **CONTAINSTABLE**, **FREETEXTTABLE**, semantickeyphrasetable, semanticsimilaritydetailstable und semanticsimilaritytable.  
  
- **Columnstore-Indizes:** RLS ist sowohl mit gruppierten als auch nicht gruppierten Columnstore-Indizes kompatibel. Da für die Sicherheit auf Zeilenebene jedoch eine Funktion angewendet wird, ist es möglich, dass der Optimierer den Abfrageplan so ändert, dass nicht der Batchmodus verwendet wird.  
  
- **Partitionierte Sichten:** Blockprädikate können nicht für partitionierte Sichten definiert werden, und partitionierte Sichten können nicht auf Grundlage von Tabellen erstellt werden, die Blockprädikate verwenden. FILTER-Prädikate sind kompatibel mit partitionierten Sichten.  
  
- **Temporale Tabellen:** Temporale Tabellen sind mit RLS kompatibel. Sicherheitsprädikate für die aktuelle Tabelle werden jedoch nicht automatisch in die Verlaufstabelle repliziert. Um eine Sicherheitsrichtlinie auf die aktuellen und Verlaufstabellen anzuwenden, müssen Sie für jede Tabelle ein Sicherheitsprädikat einzeln hinzufügen.  
  
## <a name="examples"></a><a name="CodeExamples"></a> Beispiele  
  
### <a name="a-scenario-for-users-who-authenticate-to-the-database"></a><a name="Typical"></a> A. Szenario für Benutzer, die sich bei der Datenbank authentifizieren

 In diesem Beispiel werden drei Benutzer und eine Tabelle erstellt, die sechs Zeilen enthält. Anschließend werden eine Inline-Tabellenwertfunktion und eine Sicherheitsrichtlinie für die Tabelle erstellt. Im Beispiel wird gezeigt, wie ausgewählte Anweisungen für verschiedene Benutzer gefiltert werden.  
  
 Erstellen Sie drei Benutzerkonten, anhand derer unterschiedliche Zugriffsmöglichkeiten vorgeführt werden.  


```sql  
CREATE USER Manager WITHOUT LOGIN;  
CREATE USER Sales1 WITHOUT LOGIN;  
CREATE USER Sales2 WITHOUT LOGIN;  
```

Erstellen Sie eine Tabelle zum Speichern von Daten.  

```sql
CREATE TABLE Sales  
    (  
    OrderID int,  
    SalesRep sysname,  
    Product varchar(10),  
    Qty int  
    );  
```

 Füllen Sie die Tabelle mit sechs Datenzeilen auf, sodass drei Bestellungen pro Vertriebsmitarbeiter enthalten sind.  

```sql
INSERT INTO Sales VALUES (1, 'Sales1', 'Valve', 5);
INSERT INTO Sales VALUES (2, 'Sales1', 'Wheel', 2);
INSERT INTO Sales VALUES (3, 'Sales1', 'Valve', 4);
INSERT INTO Sales VALUES (4, 'Sales2', 'Bracket', 2);
INSERT INTO Sales VALUES (5, 'Sales2', 'Wheel', 5);
INSERT INTO Sales VALUES (6, 'Sales2', 'Seat', 5);
-- View the 6 rows in the table  
SELECT * FROM Sales;
```

Gewähren Sie den Benutzern Lesezugriff auf die Tabelle.  

```sql
GRANT SELECT ON Sales TO Manager;  
GRANT SELECT ON Sales TO Sales1;  
GRANT SELECT ON Sales TO Sales2;  
```

Erstellen Sie ein neues Schema und eine Inline-Tabellenwertfunktion. Die Funktion gibt 1 zurück, wenn eine Zeile in der Spalte SalesRep dem Benutzer entspricht, der die Abfrage ausführt (`@SalesRep = USER_NAME()`) oder wenn der Benutzer, der die Abfrage ausführt, der Manager-Benutzer ist (`USER_NAME() = 'Manager'`).

```sql
CREATE SCHEMA Security;  
GO  
  
CREATE FUNCTION Security.fn_securitypredicate(@SalesRep AS sysname)  
    RETURNS TABLE  
WITH SCHEMABINDING  
AS  
    RETURN SELECT 1 AS fn_securitypredicate_result
WHERE @SalesRep = USER_NAME() OR USER_NAME() = 'Manager';  
```

Erstellen Sie eine Sicherheitsrichtlinie, die die Funktion als Filterprädikat hinzufügt. Der Status muss auf ON festgelegt werden, um die Richtlinie zu aktivieren.

```sql
CREATE SECURITY POLICY SalesFilter  
ADD FILTER PREDICATE Security.fn_securitypredicate(SalesRep)
ON dbo.Sales  
WITH (STATE = ON);  
```

Zulassen der SELECT-Berechtigungen für die fn_securitypredicate-Funktion 
```sql
GRANT SELECT ON security.fn_securitypredicate TO Manager;  
GRANT SELECT ON security.fn_securitypredicate TO Sales1;  
GRANT SELECT ON security.fn_securitypredicate TO Sales2;  
```

Testen Sie jetzt das Filterungsprädikat, indem Sie sie als jeweiliger Benutzer aus der Sales-Tabelle auswählen.

```sql
EXECUTE AS USER = 'Sales1';  
SELECT * FROM Sales;
REVERT;  
  
EXECUTE AS USER = 'Sales2';  
SELECT * FROM Sales;
REVERT;  
  
EXECUTE AS USER = 'Manager';  
SELECT * FROM Sales;
REVERT;  
```

Dem Manager sollten alle sechs Zeilen angezeigt werden. Den Benutzern Sales1 und Sales2 sollten nur ihre eigenen Verkäufe angezeigt werden.

Ändern Sie die Sicherheitsrichtlinie, um die Richtlinie zu deaktivieren.

```sql
ALTER SECURITY POLICY SalesFilter  
WITH (STATE = OFF);  
```

Jetzt können die Benutzer Sales1 und Sales2 alle sechs Zeilen sehen.

Stellen Sie eine Verbindung mit der SQL-Datenbank her, um Ressourcen zu bereinigen.

```sql
DROP USER Sales1;
DROP USER Sales2;
DROP USER Manager;

DROP SECURITY POLICY SalesFilter;
DROP TABLE Sales;
DROP FUNCTION Security.fn_securitypredicate;
DROP SCHEMA Security;
```

### <a name="b-scenarios-for-using-row-level-security-on-an-azure-synapse-external-table"></a><a name="external"></a> B. Szenarios für die Verwendung von Sicherheit auf Zeilenebene in einer externen Azure Synapse-Tabelle

In diesem kurzen Beispiel werden drei Benutzer und eine externe Tabelle mit sechs Zeilen erstellt. Anschließend werden eine Inline-Tabellenwertfunktion und eine Sicherheitsrichtlinie für die externe Tabelle erstellt. Im Beispiel wird gezeigt, wie ausgewählte Anweisungen für verschiedene Benutzer gefiltert werden. 

### <a name="prerequisites"></a>Voraussetzungen

1. Sie besitzen einen dedizierten SQL-Pool. Siehe [Erstellen eines dedizierten SQL-Pools](/azure/synapse-analytics/sql-data-warehouse/create-data-warehouse-portal).
1. Der Server, auf dem der dedizierte SQL-Pool gehostet wird, muss bei AAD registriert sein, und Sie müssen über ein Azure-Speicherkonto mit den Berechtigungen eines Speicherblogdaten-Mitwirkenden verfügen. Führen Sie [die hier beschriebenen Schritte](/azure/azure-sql/database/vnet-service-endpoint-rule-overview#steps) aus.
1. Erstellen Sie ein Dateisystem fürIhr Azure Storage-Konto. Zeigen Sie Ihr Speicherkonto mit Storage-Explorer an. Klicken Sie mit der rechten Maustaste auf Container, und wählen Sie *Dateisystem erstellen* aus.  

Sobald Sie die Voraussetzungen eingerichtet haben, erstellen Sie drei Benutzerkonten, anhand derer unterschiedliche Zugriffsmöglichkeiten gezeigt werden.

```sql
--run in master
CREATE LOGIN Manager WITH PASSWORD = '<user_password>'
GO
CREATE LOGIN Sales1 WITH PASSWORD = '<user_password>'
GO
CREATE LOGIN Sales2 WITH PASSWORD = '<user_password>'
GO

--run in master and your dedicated SQL pool database
CREATE USER Manager FOR LOGIN Manager;  
CREATE USER Sales1  FOR LOGIN Sales1;  
CREATE USER Sales2  FOR LOGIN Sales2 ;
```

Erstellen Sie eine Tabelle zum Speichern von Daten.  

```sql
CREATE TABLE Sales  
    (  
    OrderID int,  
    SalesRep sysname,  
    Product varchar(10),  
    Qty int  
    );  
```

Füllen Sie die Tabelle mit sechs Datenzeilen auf, sodass drei Bestellungen pro Vertriebsmitarbeiter enthalten sind.  

```sql
INSERT INTO Sales VALUES (1, 'Sales1', 'Valve', 5);
INSERT INTO Sales VALUES (2, 'Sales1', 'Wheel', 2);
INSERT INTO Sales VALUES (3, 'Sales1', 'Valve', 4);
INSERT INTO Sales VALUES (4, 'Sales2', 'Bracket', 2);
INSERT INTO Sales VALUES (5, 'Sales2', 'Wheel', 5);
INSERT INTO Sales VALUES (6, 'Sales2', 'Seat', 5);
-- View the 6 rows in the table  
SELECT * FROM Sales;
```

Erstellen Sie aus der Sales-Tabelle, die Sie gerade erstellt haben, eine externe Azure Synapse-Tabelle.

```sql
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<user_password>';

CREATE DATABASE SCOPED CREDENTIAL msi_cred WITH IDENTITY = 'Managed Service Identity';

CREATE EXTERNAL DATA SOURCE ext_datasource_with_abfss WITH (TYPE = hadoop, LOCATION = 'abfss://<file_system_name@storage_account>.dfs.core.windows.net', CREDENTIAL = msi_cred);

CREATE EXTERNAL FILE FORMAT MSIFormat  WITH (FORMAT_TYPE=DELIMITEDTEXT);
  
CREATE EXTERNAL TABLE Sales_ext WITH (LOCATION='<your_table_name>', DATA_SOURCE=ext_datasource_with_abfss, FILE_FORMAT=MSIFormat, REJECT_TYPE=Percentage, REJECT_SAMPLE_VALUE=100, REJECT_VALUE=100)
AS SELECT * FROM sales;
```

Erteilen Sie den drei Benutzern SELECT-Berechtigungen für die externe Tabelle „Sales_ext“, die Sie erstellt haben.

```sql
GRANT SELECT ON Sales_ext TO Sales1;  
GRANT SELECT ON Sales_ext TO Sales2;  
GRANT SELECT ON Sales_ext TO Manager;
```

Erstellen Sie ein neues Schema und eine Inline-Tabellenwertfunktion. In Beispiel A haben Sie diesen Vorgang möglicherweise bereits abgeschlossen. Die Funktion gibt 1 zurück, wenn eine Zeile in der Spalte SalesRep dem Benutzer entspricht, der die Abfrage ausführt (`@SalesRep = USER_NAME()`), oder wenn der Benutzer, der die Abfrage ausführt, der Manager-Benutzer ist (`USER_NAME() = 'Manager'`).

```sql
CREATE SCHEMA Security;  
GO  
  
CREATE FUNCTION Security.fn_securitypredicate(@SalesRep AS sysname)  
    RETURNS TABLE  
WITH SCHEMABINDING  
AS  
    RETURN SELECT 1 AS fn_securitypredicate_result
WHERE @SalesRep = USER_NAME() OR USER_NAME() = 'Manager';  
```

Erstellen Sie eine Sicherheitsrichtlinie für die externe Tabelle, wobei Sie die Inline-Tabellenwertfunktion als Filterprädikat verwenden. Der Status muss auf ON festgelegt werden, um die Richtlinie zu aktivieren.

```sql
CREATE SECURITY POLICY SalesFilter_ext
ADD FILTER PREDICATE Security.fn_securitypredicate(SalesRep)
ON dbo.Sales_ext  
WITH (STATE = ON);
```

Testen Sie jetzt das Filterprädikat, indem Sie in der externen Sales_ext-Tabelle eine Auswahl treffen. Melden Sie sich unter jedem Benutzernamen an – Sales1, Sales2 und Manager. Führen Sie den folgenden Befehl als jeweiliger Benutzer aus.

```sql
SELECT * FROM Sales_ext;
```

Dem Manager sollten alle sechs Zeilen angezeigt werden. Die Benutzer Sales1 und Sales2 sollten nur ihre eigenen Verkaufsdaten sehen.

Ändern Sie die Sicherheitsrichtlinie, um die Richtlinie zu deaktivieren.

```sql
ALTER SECURITY POLICY SalesFilter_ext  
WITH (STATE = OFF);  
```

Jetzt werden den Sales1- und Sales2-Benutzern alle sechs Zeilen angezeigt.

Stellen Sie eine Verbindung mit der Azure Synapse-Datenbank her, um Ressourcen zu bereinigen.

```sql
DROP USER Sales1;
DROP USER Sales2;
DROP USER Manager;

DROP SECURITY POLICY SalesFilter_ext;
DROP TABLE Sales;
DROP EXTERNAL TABLE Sales_ext;
DROP EXTERNAL DATA SOURCE ext_datasource_with_abfss ;
DROP EXTERNAL FILE FORMAT MSIFormat;
DROP DATABASE SCOPED CREDENTIAL msi_cred; 
DROP MASTER KEY;
```

Stellen Sie eine Verbindung mit der logischen Masterdatenbank her, um Ressourcen zu bereinigen.

```sql
DROP LOGIN Sales1;
DROP LOGIN Sales2;
DROP LOGIN Manager;
```

### <a name="c-scenario-for-users-who-connect-to-the-database-through-a-middle-tier-application"></a><a name="MidTier"></a> C. Szenario für Benutzer, die sich über eine Middle-Tier Application mit der Datenbank verbinden

> [!NOTE]
> In diesem Beispiel wird die Funktionalität zum Blockieren von Prädikaten für Azure Synapse momentan nicht unterstützt, sodass das Einfügen von Zeilen für die falsche Benutzer-ID bei Azure Synapse nicht blockiert wird.

Dieses Beispiel zeigt, wie eine Anwendung der mittleren Schicht eine Verbindungsfilterung implementieren kann, bei der Anwendungsbenutzer (oder Mandanten) den gleichen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Benutzer (die Anwendung) gemeinsam verwenden. Die Anwendung legt nach dem Verbinden mit der Datenbank die aktuelle Anwendungsbenutzer-ID in [SESSION_CONTEXT &#40;Transact-SQL&#41;](../../t-sql/functions/session-context-transact-sql.md) fest. Dann sorgen Sicherheitsrichtlinien dafür, dass Zeilen transparent herausgefiltert werden, die für diese ID nicht sichtbar sein sollen. Außerdem wird der Benutzer am Einfügen von Zeilen für die falsche Benutzer-ID gehindert. Es sind keine weiteren App-Änderungen erforderlich.  
  
 Erstellen Sie eine Tabelle zum Speichern von Daten.

```sql
CREATE TABLE Sales (  
    OrderId int,  
    AppUserId int,  
    Product varchar(10),  
    Qty int  
);  
```

Füllen Sie die Tabelle mit sechs Datenzeilen auf, sodass drei Bestellungen pro Anwendungsbenutzer enthalten sind.

```sql
INSERT Sales VALUES
    (1, 1, 'Valve', 5),
    (2, 1, 'Wheel', 2),
    (3, 1, 'Valve', 4),  
    (4, 2, 'Bracket', 2),
    (5, 2, 'Wheel', 5),
    (6, 2, 'Seat', 5);  
```

Erstellen Sie ein Benutzerkonto mit geringen Berechtigungen, das die Anwendung zum Herstellen der Verbindung verwendet.

```sql
-- Without login only for demo  
CREATE USER AppUser WITHOUT LOGIN;
GRANT SELECT, INSERT, UPDATE, DELETE ON Sales TO AppUser;  
  
-- Never allow updates on this column  
DENY UPDATE ON Sales(AppUserId) TO AppUser;  
```

Erstellen Sie ein neues Schema und eine Prädikatfunktion, die die Anwendungsbenutzer-ID verwendet, die in **SESSION_CONTEXT** zum Filtern von Zeilen gespeichert ist.

```sql
CREATE SCHEMA Security;  
GO  
  
CREATE FUNCTION Security.fn_securitypredicate(@AppUserId int)  
    RETURNS TABLE  
    WITH SCHEMABINDING  
AS  
    RETURN SELECT 1 AS fn_securitypredicate_result  
    WHERE  
        DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('AppUser')
        AND CAST(SESSION_CONTEXT(N'UserId') AS int) = @AppUserId;
GO  
```

Erstellen Sie eine Sicherheitsrichtlinie, die diese Funktion als FILTER-Prädikat und BLOCK-Prädikat für `Sales`hinzufügt. Das BLOCK-Prädikat benötigt nur **AFTER INSERT**, da **BEFORE UPDATE** und **BEFORE DELETE** bereits gefiltert sind und **AFTER UPDATE** unnötig ist, da die Spalte `AppUserId` aufgrund von zuvor festgelegten Spaltenberechtigungen nicht in andere Werte geändert werden kann.

```sql
CREATE SECURITY POLICY Security.SalesFilter  
    ADD FILTER PREDICATE Security.fn_securitypredicate(AppUserId)
        ON dbo.Sales,  
    ADD BLOCK PREDICATE Security.fn_securitypredicate(AppUserId)
        ON dbo.Sales AFTER INSERT
    WITH (STATE = ON);  
```

Nun können wir die Verbindungsfilterung durch Auswahl der Tabelle `Sales` simulieren, nachdem in **SESSION_CONTEXT** andere Benutzer-IDs festgelegt wurden. In der Praxis ist die Anwendung nach Öffnen einer Verbindung zuständig für das Festlegen der aktuellen Benutzer-ID in **SESSION_CONTEXT** .

```sql
EXECUTE AS USER = 'AppUser';  
EXEC sp_set_session_context @key=N'UserId', @value=1;  
SELECT * FROM Sales;  
GO  
  
/* Note: @read_only prevents the value from changing again until the connection is closed (returned to the connection pool)*/
EXEC sp_set_session_context @key=N'UserId', @value=2, @read_only=1;
  
SELECT * FROM Sales;  
GO  
  
INSERT INTO Sales VALUES (7, 1, 'Seat', 12); -- error: blocked from inserting row for the wrong user ID  
GO  
  
REVERT;  
GO  
```

Bereinigen Sie Datenbankressourcen.

```sql
DROP USER AppUser;

DROP SECURITY POLICY Security.SalesFilter;
DROP TABLE Sales;
DROP FUNCTION Security.fn_securitypredicate;
DROP SCHEMA Security;
```

## <a name="see-also"></a>Weitere Informationen

 [CREATE SECURITY POLICY &#40;Transact-SQL&#41;](../../t-sql/statements/create-security-policy-transact-sql.md)</br>
 [ALTER SECURITY POLICY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-security-policy-transact-sql.md)</br>
 [DROP SECURITY POLICY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-security-policy-transact-sql.md)</br>
 [CREATE FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-function-transact-sql.md)</br>
 [SESSION_CONTEXT &#40;Transact-SQL&#41;](../../t-sql/functions/session-context-transact-sql.md)</br>
 [sp_set_session_context &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-set-session-context-transact-sql.md)</br>
 [sys.security_policies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-security-policies-transact-sql.md)</br>
 [sys.security_predicates &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-security-predicates-transact-sql.md)</br>
 [Erstellen benutzerdefinierter Funktionen &#40;Datenbank-Engine&#41;](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md)
