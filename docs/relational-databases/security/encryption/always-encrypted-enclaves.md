---
title: Always Encrypted mit Secure Enclaves
description: Erfahren Sie mehr zu Always Encrypted mit dem Feature „Secure Enclaves“ für SQL Server.
ms.custom: seo-lt-2019
ms.date: 01/15/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: cbec7eb15ed8acad746e3fcb2013ee8100bcdd26
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837699"
---
# <a name="always-encrypted-with-secure-enclaves"></a>Always Encrypted mit Secure Enclaves

[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

Always Encrypted mit Secure Enclaves stellt eine Erweiterung zu den Computingfunktionen von [Always Encrypted](always-encrypted-database-engine.md) dar und umfasst die direkte Verschlüsselung und umfangreichere vertrauliche Abfragen. Always Encrypted mit Secure Enclaves ist in [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)] und [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] verfügbar (Vorschau).

Always Encrypted wurde 2015 in [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] und in [!INCLUDE[sssql16](../../../includes/sssql16-md.md)] eingeführt und schützt vertrauliche Daten vor Schadsoftware und dem Zugriff durch nicht *autorisierte* Benutzer mit umfangreichen Rechten: Datenbankadministratoren, Computeradministratoren, Cloudadministratoren sowie alle anderen Benutzer, die rechtmäßig auf Serverinstanzen, Hardware und Ähnliches zugreifen, aber keinen Zugriff auf einige oder alle tatsächlichen Daten haben sollten.  

Ohne die in diesem Artikel erläuterten Erweiterungen schützt Always Encrypted die Daten, indem diese auf Clientseite verschlüsselt werden und *niemals* zugelassen wird, dass die Daten oder die zugehörigen kryptografischen Schlüssel in Klartextform in der [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] angezeigt werden. Daher ist die Funktionalität von verschlüsselten Spalten innerhalb der Daten stark eingeschränkt. Die einzigen Vorgänge, die [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] für verschlüsselte Daten ausführen kann, sind Gleichheitsvergleiche (nur bei [deterministischer Verschlüsselung](always-encrypted-database-engine.md#selecting--deterministic-or-randomized-encryption) verfügbar). Alle anderen Vorgänge, beispielweise kryptografische Vorgänge (anfängliche Datenverschlüsselung oder Schlüsselrotation) sowie umfangreichere Abfragen (z. B. Musterabgleich) werden innerhalb der Datenbank nicht unterstützt. Benutzer müssen ihre Daten aus der Datenbank verschieben, um diese Vorgänge auf Clientseite auszuführen.

Always Encrypted *mit Secure Enclaves* beseitigt diese Einschränkungen, indem einige Berechnungen für Klartextdaten innerhalb einer Secure Enclave auf Serverseite zugelassen werden. Eine Secure Enclave ist ein geschützter Speicherbereich innerhalb des [!INCLUDE[ssde-md](../../../includes/ssde-md.md)]-Prozesses. Die Secure Enclave wird dem Rest der [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] und anderen Prozessen auf dem Hostcomputer als undurchsichtiger Behälter angezeigt. Es gibt keine Möglichkeit, Daten oder Code innerhalb der Enclave von außen anzuzeigen, auch nicht mit einem Debugger. Diese Eigenschaften machen die Secure Enclave zu einem *Trusted Execution Environment*, das sicher auf kryptografische Schlüssel und vertrauliche Daten in Klartextform zugreifen kann, ohne die Vertraulichkeit der Daten zu gefährden.

Always Encrypted verwendet Secure Enclaves wie im folgenden Diagramm veranschaulicht:

![Datenfluss](./media/always-encrypted-enclaves/ae-data-flow.png)

Beim Analysieren einer Transact-SQL-Anweisung, die von einer Anwendung gesendet wird, ermittelt die [!INCLUDE[ssde-md](../../../includes/ssde-md.md)], ob die Anweisung Vorgänge für verschlüsselte Daten enthält, für die die Verwendung der Secure Enclave erforderlich ist. Für diese Anweisungen gilt:

- Der Clienttreiber sendet die für die Vorgänge erforderlichen Spaltenverschlüsselungsschlüssel an die Secure Enclave (über einen sicheren Kanal) und übermittelt die Anweisung, die ausgeführt werden soll.

- Bei der Verarbeitung der Anweisung delegiert die [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] Kryptografievorgänge oder Berechnungen für verschlüsselte Spalten an die Secure Enclave. Bei Bedarf entschlüsselt die Enclave die Daten und führt Berechnungen in Klartextform aus.

Während der Verarbeitung der Anweisung werden weder die Daten noch die Spaltenverschlüsselungsschlüssel außerhalb der Secure Enclave in Klartextform in der [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] verfügbar gemacht.

## <a name="supported-enclave-technologies"></a>Unterstützte Enclavetechnologien

In [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)] verwendet Always Encrypted über [Virtualisierungsbasierte Sicherheit (VBS)](https://www.microsoft.com/security/blog/2018/06/05/virtualization-based-security-vbs-memory-enclaves-data-protection-through-isolation/) abgesicherte Secure Enclaves (auch als VSM-Enclaves bezeichnet) im Windows-Arbeitsspeicher.

In [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] verwendet Always Encrypted mit Secure Enclaves [Intel Software Guard Extensions-Enclaves (Intel SGX)](https://itpeernetwork.intel.com/microsoft-azure-confidential-computing/). Intel SGX ist eine hardwarebasierte Trusted Execution Environment-Technologie, die in Datenbanken mit der Hardwarekonfiguration [DC-series](/azure/azure-sql/database/service-tiers-vcore?tabs=azure-portal#dc-series) unterstützt wird.

## <a name="secure-enclave-attestation"></a>Nachweis von Secure Enclaves

Die Secure Enclave innerhalb der [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] hat Zugriff auf vertrauliche Daten und die Spaltenverschlüsselungsschlüssel in Klartextform. Deshalb muss der Clienttreiber, bevor er eine Anweisung sendet, die Enclaveberechnungen für die [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] umfasst, innerhalb der Anwendung überprüfen, dass es sich um eine echte Secure Enclave vom Typ VBS oder SGX handelt und dass der Code, der in der Secure Enclave ausgeführt wird, die echte Always Encrypted-Bibliothek darstellt, die kryptografische Always Encrypted-Algorithmen für die direkte Verschlüsselung und für Vorgänge implementiert, die in vertraulichen Abfragen unterstützt werden.

Der Prozess der Überprüfung der Enclave wird als **Enclavenachweis** bezeichnet und erfordert, dass sowohl ein Clienttreiber innerhalb der Anwendung als auch eine [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] einen externen Nachweisdienst kontaktieren. Die genauen Merkmale des Nachweisprozesses richten sich nach dem Enclavetyp (VBS oder SGX) und dem Nachweisdienst.

Der Nachweisprozess für Secure Enclaves vom Typ VBS in [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)] ist der [Runtimenachweis für die Windows Defender-Systemüberwachung](https://www.microsoft.com/security/blog/2018/06/05/virtualization-based-security-vbs-memory-enclaves-data-protection-through-isolation/), bei dem der Host-Überwachungsdienst als Nachweisdienst verwendet wird. 

Für die Durchführung des Nachweises für Intel SGX-Enclaves in [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] benötigen Sie [Microsoft Azure Attestation](/azure/attestation/overview).

> [!NOTE]
> [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)] unterstützt Microsoft Azure Attestation nicht. Der Host-Überwachungsdienst ist die einzige Nachweislösung, die für VBS-Enclaves in [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)] unterstützt wird.

## <a name="supported-client-drivers"></a>Unterstützte Clienttreiber

Um Always Encrypted mit Secure Enclaves verwenden zu können, muss eine Anwendung einen Clienttreiber nutzen, der dieses Feature unterstützt. Konfigurieren Sie die Anwendung sowie den Clienttreiber, um Enclaveberechnungen und Enclavenachweise zu aktivieren. Weitere Informationen, einschließlich der Liste der unterstützten Clienttreiber, finden Sie unter [Entwickeln von Anwendungen mithilfe von Always Encrypted](always-encrypted-client-development.md).

## <a name="terminology"></a>Begriff

### <a name="enclave-enabled-keys"></a>Enclave-fähige Schlüssel

Always Encrypted mit Secure Enclave führt das Konzept der Enclave-fähigen Schlüssel ein:

- **Enclave-fähiger Spaltenhauptschlüssel:** Ein Spaltenhauptschlüssel, der mit der im Metadatenobjekt des Spaltenhauptschlüssels in der Datenbank angegebenen Eigenschaft `ENCLAVE_COMPUTATIONS` erstellt wird. Das Spaltenhauptschlüssel-Metadatenobjekt muss auch eine gültige Signatur der Metadateneigenschaften enthalten. Weitere Informationen finden Sie unter [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md).
- **Enclave-fähiger Spaltenverschlüsselungsschlüssel**: Ein Spaltenverschlüsselungsschlüssel, der mit einem Enclave-fähigen Spaltenhauptschlüssel verschlüsselt ist. Nur Enclave-fähige Spaltenverschlüsselungsschlüssel können für Berechnungen innerhalb der Secure Enclave verwendet werden. 

Weitere Informationen finden Sie unter [Verwalten von Schlüsseln für Always Encrypted mit Secure Enclaves](always-encrypted-enclaves-manage-keys.md).

### <a name="enclave-enabled-columns"></a>Enclave-fähige Spalten

Eine Enclave-fähige Spalte ist eine Datenbankspalte, die mit einem Enclave-fähigen Spaltenverschlüsselungsschlüssel verschlüsselt ist.

## <a name="confidential-computing-capabilities-for-enclave-enabled-columns"></a>Funktionen für vertrauliches Computing für Enclave-fähige Spalten

Die zwei wichtigsten Vorteile von Always Encrypted mit Secure Enclaves sind die direkte Verschlüsselung und die umfassenden vertraulichen Abfragen.

### <a name="in-place-encryption"></a>Direkte Verschlüsselung

Die direkte Verschlüsselung ermöglicht kryptografische Vorgänge für Datenbankspalten innerhalb der Secure Enclave, ohne die Daten aus der Datenbank zu verschieben. Die direkte Verschlüsselung verbessert die Leistung und die Zuverlässigkeit der Verschlüsselung. Sie können eine direkte Verschlüsselung mithilfe der Anweisung [ALTER TABLE (Transact-SQL)](../../../t-sql/statements/alter-table-transact-sql.md) durchführen. 

Die unterstützten Kryptografievorgänge lauten:

- Verschlüsseln einer Klartextspalte mit einem Enclave-fähigen Spaltenverschlüsselungsschlüssel
- Erneutes Verschlüsseln einer verschlüsselten Enclave-fähigen Spalte, um:
  - einen Spaltenverschlüsselungsschlüssel zu rotieren: Hierbei wird die Spalte mit einem Enclave-fähigen Spaltenverschlüsselungsschlüssel erneut verschlüsselt.
  - den Verschlüsselungstyp einer Enclave-fähigen Spalte zu ändern, z. B. von deterministisch zu zufällig
- Entschlüsseln von Daten, die in einer Enclave-fähigen Spalte gespeichert sind (Konvertieren der Spalte in eine Klartextspalte)

Eine direkte Verschlüsselung ist sowohl bei der deterministischen als auch bei der zufälligen Verschlüsselung zulässig, solange die an einem Kryptografievorgang beteiligten Spaltenverschlüsselungsschlüssel Enclave-fähig sind.

### <a name="confidential-queries"></a>Vertrauliche Abfragen

Vertrauliche Abfragen sind [DML-Abfragen](../../../t-sql/queries/queries.md), die Vorgänge für Enclave-fähige Spalten umfassen, die in der Secure Enclave ausgeführt werden.

In Secure Enclaves werden folgende Vorgänge unterstützt:

| Vorgang| [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)] | [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] |
|:---|:---|:---|
| [Comparison Operators (Vergleichsoperatoren)](../../../mdx/comparison-operators.md) | Unterstützt | Unterstützt |
| [BETWEEN (Transact-SQL)](../../../t-sql/language-elements/between-transact-sql.md) | Unterstützt | Unterstützt |
| [IN (Transact-SQL)](../../../t-sql/language-elements/in-transact-sql.md) | Unterstützt | Unterstützt |
| [LIKE (Transact-SQL)](../../../t-sql/language-elements/like-transact-sql.md) | Unterstützt | Unterstützt |
| [DISTINCT](../../../t-sql/queries/select-transact-sql.md#c-using-distinct-with-select) | Unterstützt | Unterstützt |
| [Joins](../../performance/joins.md) | Unterstützt nur Joins geschachtelter Schleifen | Unterstützt |
| [SELECT - ORDER BY-Klausel (Transact-SQL)](../../../t-sql/queries/select-order-by-clause-transact-sql.md) | Nicht unterstützt | Unterstützt |
| [SELECT – GROUP BY – Transact-SQL](../../../t-sql/queries/select-group-by-transact-sql.md) | Nicht unterstützt | Unterstützt |

> [!NOTE]
> Die obigen Vorgänge werden in Secure Enclaves nur für Enclave-fähige Spalten unterstützt, für die die **zufällige** anstatt der deterministischen Verschlüsselung verwendet wird. Der Gleichheitsvergleich bleibt die einzige Berechnung, die in Spalten mit deterministischer Verschlüsselung unterstützt wird. Gleichheitsvergleiche erfolgen durch den Vergleich von Chiffretext außerhalb der Enclave, unabhängig davon, ob die Spalte Enclave-fähig ist oder nicht. Die deterministische Verschlüsselung unterstützt die folgenden Vorgänge im Zusammenhang mit Gleichheitsvergleichen: 
> - [= (Gleich)](../../../t-sql/language-elements/equals-transact-sql.md) in Punktsuchen, Suchvorgängen und Joins
> - [IN](../../../t-sql/language-elements/in-transact-sql.md)
> - [SELECT – GROUP BY](../../../t-sql/queries/select-group-by-transact-sql.md)
> - [DISTINCT](../../../t-sql/queries/select-transact-sql.md#c-using-distinct-with-select)
>
> In [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)] muss für vertrauliche Abfragen, die Enclaves in Zeichenfolgenspalten (`char`, `nchar`) verwenden, eine binary2-Sortierung (BIN2) für die Spalte verwendet werden. In [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] muss für vertrauliche Abfragen in Zeichenfolgen eine BIN2- oder eine UTF-8-Sortierung verwendet werden. 

### <a name="indexes-on-enclave-enabled-columns"></a>Indizes in Enclave-fähigen Spalten

Mithilfe der zufälligen Verschlüsselung können Sie nicht gruppierte Indizes für Enclave-fähige Spalten erstellen, um vertrauliche DML-Abfragen mithilfe der Secure Enclave schneller auszuführen.

Damit sichergestellt wird, dass eine mit zufälliger Verschlüsselung verschlüsselte Spalte keine vertraulichen Daten offenlegt, müssen die Schlüsselwerte in der Indexdatenstruktur (B-Struktur) basierend auf ihren Klartextwerten verschlüsselt und sortiert werden. Das Sortieren nach dem Klartextwert ist ebenfalls nützlich, um Abfragen innerhalb der Enclave zu verarbeiten. Wenn der Abfrageexecutor in der [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] einen Index auf einer verschlüsselten Spalte für Berechnungen innerhalb der Enclave verwendet, durchsucht er den Index nach bestimmten in der Spalte gespeicherten Werten. Bei jeder Suche werden möglicherweise mehrere Vergleiche ausgeführt. Der Abfrageexecutor delegiert jeden Vergleich an die Enclave, die einen in der Spalte gespeicherten Wert und den zu vergleichenden verschlüsselten Indexschlüsselwert entschlüsselt, den Vergleich im Klartext durchführt und das Ergebnis des Vergleichs an den Executor zurückgibt.

Das Erstellen von Indizes auf Spalten, die zufällige Verschlüsselung verwenden und nicht Enclave-fähig sind, wird weiterhin nicht unterstützt.

Ein Index für eine Spalte mit deterministischer Verschlüsselung wird basierend auf Chiffretext (nicht Klartext) sortiert, unabhängig davon, ob die Spalte Enclave-fähig ist oder nicht.

Weitere Informationen finden Sie unter [Erstellen und Verwenden von Indizes in Spalten mithilfe von Always Encrypted mit Secure Enclaves](always-encrypted-enclaves-create-use-indexes.md). Allgemeine Informationen zur Funktionsweise der Indizierung in [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] finden Sie im Artikel [Beschreibung von gruppierten und nicht gruppierten Indizes](../../indexes/clustered-and-nonclustered-indexes-described.md).

### <a name="database-recovery"></a>Datenbankwiederherstellung

Wenn eine SQL Server-Instanz ausfällt, können ihre Datenbanken in einem Zustand verbleiben, in dem die Datendateien einige Änderungen durch unvollständige Transaktionen enthalten können. Wenn die Instanz gestartet wird, führt sie einen Prozess namens [Datenbankwiederherstellung](../../logs/the-transaction-log-sql-server.md#recovery-of-all-incomplete-transactions-when--is-started) aus, bei dem jede im Transaktionsprotokoll gefundene unvollständige Transaktion zurückgesetzt wird, um sicherzustellen, dass die Integrität der Datenbank erhalten bleibt. Wenn eine unvollständige Transaktion Änderungen an einem Index vorgenommen hat, müssen diese ebenfalls rückgängig gemacht werden. Beispielsweise müssen einige Schlüsselwerte im Index entfernt oder neu eingefügt werden.

> [!IMPORTANT]
> Microsoft empfiehlt dringend, die [Schnellere Datenbankwiederherstellung (ADR)](../../backup-restore/restore-and-recovery-overview-sql-server.md#adr) für Ihre Datenbank zu aktivieren, **bevor** Sie den ersten Index auf einer Enclave-fähigen Spalte verwenden, die mit zufälliger Verschlüsselung verschlüsselt wurde. Die ADR ist standardmäßig in [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] aktiviert, in [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)] jedoch nicht.

Beim herkömmlichen [Datenbankwiederherstellungsprozess](/azure/sql-database/sql-database-accelerated-database-recovery#the-current-database-recovery-process) (gemäß dem [ARIES](https://people.eecs.berkeley.edu/~brewer/cs262/Aries.pdf)-Wiederherstellungsmodell) muss SQL Server zum Rückgängigmachen einer Änderung an einem Index warten, bis eine Anwendung den Spaltenverschlüsselungsschlüssel für die Spalte an die Enclave übermittelt. Dies kann sehr lange dauern. Die ADR reduziert die Anzahl der Vorgänge zum Rückgängigmachen von Änderungen, die verschoben werden müssen, da ein Spaltenverschlüsselungsschlüssel im Cache innerhalb der Enclave nicht verfügbar ist. Infolgedessen wird die Verfügbarkeit der Datenbank erheblich erhöht, indem die Wahrscheinlichkeit, dass eine neue Transaktion blockiert wird, minimiert wird. Wenn ADR aktiviert ist, benötigt SQL Server möglicherweise immer noch einen Spaltenverschlüsselungsschlüssel, um die Bereinigung alter Datenversionen abzuschließen, aber er erledigt dies als Hintergrundaufgabe, die sich nicht auf die Verfügbarkeit der Datenbank oder Benutzertransaktionen auswirkt. Es kann jedoch vorkommen, dass Fehlermeldungen im Fehlerprotokoll angezeigt werden, die auf fehlgeschlagene Bereinigungsvorgänge aufgrund eines fehlenden Spaltenverschlüsselungsschlüssels hinweisen.

## <a name="security-considerations"></a>Sicherheitshinweise

Die folgenden Sicherheitsaspekte sind für Always Encrypted mit Secure Enclaves zu beachten.

- Die Sicherheit Ihrer Daten innerhalb der Enclave hängt von einem Nachweisprotokoll und einem Nachweisdienst ab. Daher müssen Sie sicherstellen, dass der Nachweisdienst und die Nachweisrichtlinien, die der Dienst durchsetzt, von einem vertrauenswürdigen Administrator verwaltet werden. Darüber hinaus unterstützen die Nachweisdienste in der Regel verschiedene Richtlinien und Nachweisprotokolle, von denen einige eine minimale Überprüfung der Enclave und ihrer Umgebung durchführen und für Test- und Entwicklungszwecke konzipiert sind. Halten Sie sich genau an die für Ihren Nachweisdienst spezifischen Richtlinien, um sicherzustellen, dass Sie die empfohlenen Konfigurationen und Richtlinien für Ihre Produktionsumgebungen verwenden. 
- Die Verschlüsselung einer Spalte unter Verwendung von zufälliger Verschlüsselung mit einem Enclave-fähigen Spaltenverschlüsselungsschlüssel kann dazu führen, dass die Reihenfolge der in der Spalte gespeicherten Daten verloren geht, da solche Spalten Bereichsvergleiche unterstützen. Wenn beispielsweise eine verschlüsselte Spalte, die Gehälter von Mitarbeitern enthält, einen Index verwendet, könnte ein böswilliger DBA den Index scannen, um den maximalen verschlüsselten Wert für das Gehalt zu finden und eine Person mit dem maximalen Gehalt zu identifizieren (vorausgesetzt, der Name der Person ist nicht verschlüsselt). 
- Wenn Sie Always Encrypted verwenden, um sensible Daten vor unbefugtem Zugriff durch DBAs zu schützen, sollten Sie die Spaltenhauptschlüssel oder Spaltenverschlüsselungsschlüssel nicht an die DBAs weitergeben. Ein DBA kann Indizes auf verschlüsselten Spalten verwalten, ohne direkten Zugriff auf die Schlüssel zu haben, indem er den Cache der Spaltenverschlüsselungsschlüssel innerhalb der Enclave nutzt.

## <a name="considerations-for-business-continuity-disaster-recovery-and-data-migration"></a><a name="anchorname-1-considerations-availability-groups-db-migration"></a> Überlegungen zu Business Continuity & Disaster Recovery sowie zur Datenmigration

Stellen Sie beim Konfigurieren einer Lösung für Hochverfügbarkeit oder Notfallwiederherstellung für eine Datenbank, die Always Encrypted mit Secure Enclaves verwendet, sicher, dass alle Datenbankreplikate eine Secure Enclave verwenden können. Wenn eine Enclave für das primäre, aber nicht für das sekundäre Replikat verfügbar ist, löst jede Anweisung, die versucht, die Funktionen von Always Encrypted mit Secure Enclaves zu verwenden, nach dem Failover einen Fehler aus.

Wenn Sie eine Datenbank mithilfe von Always Encrypted mit Secure Enclaves kopieren oder migrieren, achten Sie darauf, dass die Zielumgebung Enclaves immer unterstützt. Andernfalls funktionieren Anweisungen mit Enclaves nicht für Kopien oder Migrationen der Datenbank.

Folgendes sollten Sie berücksichtigen:

- **SQL Server**
  - Stellen Sie beim Konfigurieren von [Always On-Verfügbarkeitsgruppen](../../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md) sicher, dass jede SQL Server-Instanz, die eine Datenbank in der Verfügbarkeitsgruppe hostet, Always Encrypted mit Secure Enclaves unterstützt und sowohl eine Enclave als auch ein Nachweis für diese konfiguriert ist.
  - Wenn Sie eine Sicherungsdatei einer Datenbank wiederherstellen, die die Funktionen von Always Encrypted mit Secure Enclaves auf einer SQL Server-Instanz verwendet, für die die Enclave nicht konfiguriert ist, wird der Wiederherstellungsvorgang erfolgreich durchgeführt, und alle Funktionen, die nicht auf die Enclave angewiesen sind, sind verfügbar. Alle nachfolgenden Anweisungen, die die Enclave-Funktionen verwenden, schlagen jedoch fehl, und Indizes für Enclave-fähige Spalten mit zufälliger Verschlüsselung werden ungültig. Dasselbe gilt, wenn Sie eine Datenbank mit Always Encrypted mit Secure Enclaves an die Instanz anfügen, für die die Enclave nicht konfiguriert ist.
  - Wenn Ihre Datenbank Indizes für Enclave-fähige Spalten mit zufälliger Verschlüsselung enthält, stellen Sie sicher, dass Sie die [ADR](../../backup-restore/restore-and-recovery-overview-sql-server.md#adr) in der Datenbank aktivieren, bevor Sie eine Datenbanksicherung erstellen. Mit ADR wird sichergestellt, dass die Datenbank, einschließlich der Indizes, sofort nach der Wiederherstellung der Datenbank verfügbar ist. Weitere Informationen finden Sie unter [Datenbankwiederherstellung](#database-recovery).
  
- **Azure SQL-Datenbank**
  - Wenn Sie die [aktive Georeplikation](/azure/azure-sql/database/active-geo-replication-overview) konfigurieren, stellen Sie sicher, dass die sekundäre Datenbank Secure Enclaves unterstützt, wenn auch die primäre Datenbank dies tut.

Wenn Sie in SQL Server oder Azure SQL-Datenbank Ihre Datenbank mithilfe einer BACPAC-Datei migrieren, müssen Sie sicherstellen, dass Sie alle Indizes für Enclave-fähige Spalten mit zufälliger Verschlüsselung löschen, bevor Sie die BACPAC-Datei erstellen.

## <a name="known-limitations"></a>Bekannte Einschränkungen

Mit Always Encrypted mit Secure Enclaves werden einige Einschränkungen von Always Encrypted behoben, da die direkte Verschlüsselung und umfangreichere vertrauliche Abfragen mit Indizes wie im Abschnitt [Funktionen für vertrauliches Computing für Enclave-fähige Spalten](#confidential-computing-capabilities-for-enclave-enabled-columns) beschrieben unterstützt werden.

Alle anderen Einschränkungen für Always Encrypted, die unter den [Featuredetails](always-encrypted-database-engine.md#feature-details) aufgeführt sind, gelten auch für Always Encrypted mit Secure Enclaves.

Die folgenden Einschränkungen sind für Always Encrypted mit Secure Enclaves zu beachten:

- Gruppierte Indizes können nicht auf Enclave-fähigen Spalten mit zufälliger Verschlüsselung erstellt werden.
- Enclave-fähige Spalten mit zufälliger Verschlüsselung können keine Primärschlüsselspalten sein und können nicht durch Fremdschlüsselbeschränkungen oder eindeutige Schlüsselbeschränkungen referenziert werden.
- In [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)] (diese Einschränkung gilt nicht für [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)]) werden nur Joins geschachtelter Schleifen (mit Indizes, falls verfügbar) für Enclave-fähige Spalten mit zufälliger Verschlüsselung unterstützt. Informationen zu anderen Unterschieden zwischen den verschiedenen Produkten finden Sie unter [Vertrauliche Abfragen](#confidential-queries).
- Direkte Verschlüsselungsvorgänge können nicht mit anderen Änderungen an Spaltenmetadaten kombiniert werden. Ausgenommen hiervon sind Änderungen einer Sortierung innerhalb derselben Codeseite und der NULL-Zulässigkeit. Beispiel: Sie können nicht in einer einzigen `ALTER TABLE`/`ALTER COLUMN`-Transact-SQL-Anweisung eine Spalte verschlüsseln, erneut verschlüsseln oder entschlüsseln UND den Datentyp der Spalte ändern. Sie müssen zwei separate Anweisungen verwenden.
- Die Verwendung von Enclave-fähigen Schlüsseln für Spalten in In-Memory-Tabellen wird nicht unterstützt.
- Ausdrücke, die berechnete Spalten definieren, können keine Berechnungen für Enclave-fähige Spalten mit zufälliger Verschlüsselung durchführen – selbst dann nicht, wenn die Berechnungen zu den unter [Vertrauliche Abfragen](#confidential-queries) aufgeführten Vorgängen gehören.
- Escapezeichen werden in Parametern des LIKE-Operators in Enclave-fähigen Spalten, die zufällige Verschlüsselung verwenden, nicht unterstützt.
- Abfragen mit dem LIKE-Operator oder einem Vergleichsoperator, der einen Abfrageparameter mit einem der folgenden Datentypen hat (die nach der Verschlüsselung zu großen Objekten werden), ignorieren Indizes und führen Tabellenscans durch.
  - `nchar[n]` und `nvarchar[n]`, wenn n größer als 3967 ist.
  - `char[n]`, `varchar[n]`, `binary[n]`, `varbinary[n]`, wenn n größer als 7935 ist.
- Tooleinschränkungen:
  - Die einzigen unterstützten Schlüsselspeicher für Enclave-fähige Spaltenhauptschlüssel sind Windows Certificate Store und Azure Key Vault.
  - Importieren/Exportieren von Datenbanken mit Enclave-fähigen Schlüsseln wird nicht unterstützt.
  - Um einen direkten kryptografischen Vorgang über eine `ALTER TABLE`/`ALTER COLUMN`-Anweisung auszulösen, müssen Sie die Anweisung über ein Abfragefenster in SSMS ausgeben. Alternativ dazu können Sie selbst ein Programm schreiben, das die Anweisung ausgibt. Das Cmdlet „Set-SqlColumnEncryption“ im SQL Server-PowerShell-Modul und der Always Encrypted-Assistent in SQL Server Management Studio unterstützen derzeit die direkte Verschlüsselung nicht. Beide Tools verschieben derzeit die Daten aus der Datenbank, um kryptografische Vorgänge auszuführen – auch dann, wenn die für die Vorgänge verwendeten Spaltenverschlüsselungsschlüssel Enclave-fähig sind.

## <a name="next-steps"></a>Nächste Schritte

- [Tutorial: Erste Schritte mit Always Encrypted mit Secure Enclaves in SQL Server](../tutorial-getting-started-with-always-encrypted-enclaves.md)
- [Tutorial: Erste Schritte mit Always Encrypted mit Secure Enclaves in Azure SQL-Datenbank](/azure/azure-sql/database/always-encrypted-enclaves-getting-started)
- [Konfigurieren und Verwenden von Always Encrypted mit Secure Enclaves](configure-always-encrypted-enclaves.md)

## <a name="see-also"></a>Weitere Informationen

- [Verwalten von Schlüsseln für Always Encrypted mit Secure Enclaves](always-encrypted-enclaves-manage-keys.md)