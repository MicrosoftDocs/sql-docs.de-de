---
description: ALTER DATABASE-Kompatibilitätsgrad (Transact-SQL)
title: ALTER DATABASE-Kompatibilitätsgrad (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/22/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- COMPATIBILITY_LEVEL_TSQL
- COMPATIBILITY_LEVEL
dev_langs:
- TSQL
helpviewer_keywords:
- 80 compatibility level
- ALTER DATABASE statement, compatibility levels
- 90 compatibility level
- compatibility levels [SQL Server]
- 100 compatibility level
- db compatibility level
- db compat level
ms.assetid: ca5fd220-d5ea-4182-8950-55d4101a86f6
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4a025086b2b45804a82801fceef9032c388cf2c2
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2021
ms.locfileid: "100348518"
---
# <a name="alter-database-transact-sql-compatibility-level"></a>ALTER DATABASE-Kompatibilitätsgrad (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Legt für [!INCLUDE[tsql](../../includes/tsql-md.md)] und Verhalten bei der Abfrageverarbeitung fest, dass sie mit der angegebenen Version der SQL-Engine kompatibel sein müssen. Informationen zu anderen ALTER DATABASE-Optionen finden Sie unter [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md).  

Weitere Informationen zu Syntaxkonventionen finden Sie unter [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

## <a name="syntax"></a>Syntax

```syntaxsql
ALTER DATABASE database_name
SET COMPATIBILITY_LEVEL = { 150 | 140 | 130 | 120 | 110 | 100 | 90 }
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente

*database_name* Der Name der Datenbank, die geändert werden soll.

COMPATIBILITY_LEVEL {150 \| 140 \| 130 \| 120 \| 110 \| 100 \| 90 \| 80} entspricht der Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], mit der die Datenbank kompatibel gemacht werden soll. Die folgenden Kompatibilitätsgradwerte können konfiguriert werden (nicht alle Versionen unterstützen alle oben genannten Kompatibilitätsgrade):

<a name="supported-dbcompats"></a>

|Produkt|Version der Datenbank-Engine|Bestimmung des Kompatibilitätsgrads mit Standards|Unterstützte Kompatibilitätsgradwerte|
|-------------|-----------------------------|-------------------------------------|------------------------------------------|
|[!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]|15|150|150, 140, 130, 120, 110, 100|
|[!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|14|140|140, 130, 120, 110, 100|
|[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|12|150|150, 140, 130, 120, 110, 100|
|Verwaltete [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]-Instanz|12|150|150, 140, 130, 120, 110, 100|
|[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]|13|130|130, 120, 110, 100|
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]|12|120|120, 110, 100|
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]|11|110|110, 100, 90|
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]|10,5|100|100, 90, 80|
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]|10|100|100, 90, 80|
|[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]|9|90|90, 80|
|[!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)]|8|80|80|

> [!IMPORTANT]
> Die Versionsnummern der Datenbank-Engine für SQL Server und Azure SQL-Datenbank sind nicht miteinander vergleichbar, sondern sind interne Buildnummern für diese separaten Produkte. Die Datenbank-Engine für Azure SQL-Datenbank basiert auf der gleichen Codebasis wie die SQL Server-Datenbank-Engine. Entscheidend ist dabei, dass die Datenbank-Engine in Azure SQL-Datenbank immer über die neuesten SQL-Datenbank-Engine-Bits verfügt. Die Version 12 von Azure SQL-Datenbank ist neuer als die Version 15 von SQL Server.

## <a name="remarks"></a>Bemerkungen
Bei allen Installationen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wird der Standardkompatibilitätsgrad von der Version von [!INCLUDE[ssDE](../../includes/ssde-md.md)] abgeleitet. Neue Datenbanken sind auf diesen Grad festgelegt, sofern der Kompatibilitätsgrad der **model**-Datenbank nicht niedriger ist. Wenn Datenbanken aus einer früheren Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] angefügt oder wiederhergestellt werden, behält die Datenbank ihren vorhandenen Kompatibilitätsgrad, wenn es sich dabei um den zulässigen Mindestwert für die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz oder um einen höheren Wert handelt. Bei einem Upgrade einer Datenbank, deren Kompatibilitätsgrad unterhalb des von [!INCLUDE[ssde_md](../../includes/ssde_md.md)] festgelegten zulässigen Grads liegt, wird die Datenbank automatisch auf den niedrigsten zulässigen Kompatibilitätsgrad festgelegt. Dies gilt sowohl für die System- als auch für die Benutzerdatenbanken.

Die folgenden Verhaltensweisen werden für [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] erwartet, wenn eine Datenbank angefügt oder wiederhergestellt wird, und werden nach einem direkten Upgrade erwartet:

- War der Kompatibilitätsgrad einer Benutzerdatenbank vor dem Upgrade 100 oder höher, wird er nach dem Upgrade beibehalten.
- War der Kompatibilitätsgrad einer Benutzerdatenbank vor dem Upgrade 90, wird er auf 100 gesetzt, was dem niedrigsten unterstützten Kompatibilitätsgrad in [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] entspricht.
- Der Kompatibilitätsgrad der Datenbanken „tempdb“, „model“, „msdb“ und „Resource“ wird auf den Standardkompatibilitätsgrad für eine bestimmte [!INCLUDE[ssDE](../../includes/ssde-md.md)]-Version festgelegt. 
- Die „master“-Systemdatenbank behält den Kompatibilitätsgrad von vor dem Upgrade bei. Dies wirkt sich nicht auf das Verhalten der Benutzerdatenbank aus. 

Bei vorhandenen Datenbanken mit niedrigeren Kompatibilitätsgraden gilt Folgendes: Solange eine Anwendung keine Verbesserungen nutzen muss, die nur bei einem höheren Kompatibilitätsgrad verfügbar sind, ist es sinnvoll, den vorherigen Kompatibilitätsgrad beizubehalten. Bei Neuentwicklungen oder falls eine vorhandene Anwendung neue Features wie die [intelligente Abfrageverarbeitung](../../relational-databases/performance/intelligent-query-processing.md) sowie neue [!INCLUDE[tsql](../../includes/tsql-md.md)]-Abfragen nutzen muss, sollten Sie ein Upgrade auf den neuesten verfügbaren Datenbank-Kompatibilitätsgrad planen. Weitere Informationen finden Sie unter [Kompatibilitätsgrade und Upgrades der Datenbank-Engine](../../database-engine/install-windows/compatibility-certification.md#compatibility-levels-and-database-engine-upgrades).     

> [!NOTE]
> Wenn keine Benutzerobjekte und Abhängigkeiten vorhanden sind, ist ein Upgrade auf den Standardkompatibilitätsgrad in aller Regel sicher. Weitere Informationen finden Sie unter [master-Datenbank – Empfehlungen](../../relational-databases/databases/master-database.md#recommendations).

Verwenden Sie `ALTER DATABASE`, um den Kompatibilitätsgrad der Datenbank zu ändern. Die neue Kompatibilitätsgradeinstellung für eine Datenbank wird wirksam, wenn ein `USE <database>`-Befehl ausgegeben oder eine neue Anmeldung mit dieser Datenbank als Standarddatenbankkontext verarbeitet wird.
Fragen Sie die Spalte `compatibility_level` in der Katalogsicht [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) ab, um den aktuellen Kompatibilitätsgrad einer Datenbank anzuzeigen.

> [!NOTE]
> Eine [Verteilungsdatenbank](../../relational-databases/replication/distribution-database.md), die in einer früheren Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] erstellt wurde und auf [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] RTM oder Service Pack 1 aktualisiert wurde, verfügt über einen Kompatibilitätsgrad von „90“, was für andere Datenbanken nicht unterstützt wird. Dies wirkt sich nicht auf die Funktionstüchtigkeit der Replikation aus. Ein Upgrade auf ein späteres Service Pack oder eine spätere Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] führt dazu, dass der Kompatibilitätsgrad der Verteilungsdatenbank auf denjenigen der **Master** datenbank erhöht wird.

> [!NOTE]
> Seit **November 2019** ist der Standardkompatibilitätsgrad für neu erstellte Datenbanken in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] gleich „150“. [!INCLUDE[msCoName](../../includes/msconame-md.md)] passt bei vorhandenen Datenbanken den Datenbank-Kompatibilitätsgrad nicht an. Kunden können diesen nach Ihren eigenen Bedürfnissen anpassen.        
> [!INCLUDE[msCoName](../../includes/msconame-md.md)] empfiehlt Kunden dringend, ein Upgrade auf den aktuellen Kompatibilitätsgrad durchzuführen, um von den neuesten Verbesserungen bei der Abfrageoptimierung profitieren zu können.        

Wenn Sie den Datenbank-Kompatibilitätsgrad 120 oder höher für Ihre gesamte Datenbank nutzen möchten, aber das [**Kardinalitätsschätzungsmodell**](../../relational-databases/performance/cardinality-estimation-sql-server.md) von [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] bevorzugen, sodass eine Zuordnung zum Datenbank-Kompatibilitätsgrad 110 erfolgt, sollten Sie die Dokumentation zur Anweisung [ALTER DATABASE SCOPED CONFIGURATION](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md) und insbesondere den Abschnitt zu deren Schlüsselwort `LEGACY_CARDINALITY_ESTIMATION = ON` lesen.

Weitere Einzelheiten zur Bewertung der Leistungsunterschiede bei Ihren wichtigsten Abfragen zwischen zwei Kompatibilitätsgraden in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] finden Sie unter [Verbesserte Abfrageleistung bei Kompatibilitätsgrad 130 in Azure SQL-Datenbank](/archive/blogs/sqlserverstorageengine/improved-query-performance-with-compatibility-level-130-in-azure-sql-database). Beachten Sie, dass dieser Artikel sich auf den Kompatibilitätsgrad 130 und [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bezieht. Es gelten jedoch die gleichen Vorgehensweisen für Upgrades auf den Kompatibilitätsgrad 140 oder höher in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Führen Sie die folgende Abfrage aus, um die [!INCLUDE[ssDE](../../includes/ssde-md.md)]-Version zu bestimmen, mit der Sie verbunden sind.

```sql
SELECT SERVERPROPERTY('ProductVersion');
```

> [!NOTE]
> [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] unterstützt nicht alle Features, die nach Kompatibilitätsgrad variieren.

Führen Sie eine Abfrage für die Spalte **compatibility_level** von [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) aus, um die aktuellen Kompatibilitätsgrad zu ermitteln.

```sql
SELECT name, compatibility_level FROM sys.databases;
```

## <a name="compatibility-levels-and-database-engine-upgrades"></a>Kompatibilitätsgrade und Upgrades der Datenbank-Engine
Der Datenbank-Kompatibilitätsgrad ist ein wichtiges Tool zur Unterstützung der Datenbankmodernisierung, denn damit können Upgrades für [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] durchgeführt werden, während der funktionale Status von anbindenden Anwendungen erhalten bleibt, indem der vor einem Upgrade wirksame Kompatibilitätsgrad beibehalten wird. Für eine ältere Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (z. B. [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) kann daher ein Upgrade auf [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] oder [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] (einschließlich verwalteter Instanzen) durchgeführt werden, ohne dass Änderungen an Anwendungen (mit Ausnahme von Datenbankverbindungen) erforderlich sind. Weitere Informationen finden Sie unter [Kompatibilitätszertifizierung](../../database-engine/install-windows/compatibility-certification.md).

Solange für die jeweilige Anwendung keine Verbesserungen genutzt werden müssen, die nur in einem höheren Datenbank-Kompatibilitätsgrad verfügbar sind, ist dieser Ansatz zum Durchführen eines Upgrades für [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] und zum Beibehalten des vorherigen Datenbank-Kompatibilitätsgrads geeignet. Weitere Informationen zum Verwenden eines Kompatibilitätsgrads für Abwärtskompatibilität finden Sie unter [Kompatibilitätszertifizierung](../../database-engine/install-windows/compatibility-certification.md).

## <a name="best-practices-for-upgrading-database-compatibility-level"></a>Bewährte Methoden zum Aktualisieren des Datenbank-Kompatibilitätsgrads
Den empfohlenen Workflow für ein Upgrade des Kompatibilitätsgrads finden Sie unter [Ändern des Datenbank-Kompatibilitätsmodus und Verwenden des Abfragespeichers](../../database-engine/install-windows/change-the-database-compatibility-mode-and-use-the-query-store.md). Unterstützung beim Upgrade des Datenbank-Kompatibilitätsgrads finden Sie außerdem unter [Aktualisieren von Datenbanken mit dem Abfrageoptimierung-Assistenten](../../relational-databases/performance/upgrade-dbcompat-using-qta.md).

## <a name="compatibility-levels-and-stored-procedures"></a>Kompatibilitätsgrade und gespeicherte Prozeduren
Wenn eine gespeicherte Prozedur ausgeführt wird, verwendet sie den aktuellen Kompatibilitätsgrad der Datenbank, in der sie definiert ist. Wenn die Kompatibilitätseinstellung einer Datenbank geändert wird, werden alle zugehörigen gespeicherten Prozeduren automatisch entsprechend neu kompiliert.

## <a name="using-compatibility-level-for-backward-compatibility"></a><a name="backwardCompat"></a> Verwenden des Kompatibilitätsgrads für Abwärtskompatibilität
Die Einstellung [Datenbank-Kompatibilitätsgrad](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md) bietet Abwärtskompatibilität mit früheren Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in Bezug auf das Verhalten von [!INCLUDE[tsql](../../includes/tsql-md.md)] und der Abfrageoptimierung. Dies gilt allerdings ausschließlich für die angegebene Datenbank und nicht für den gesamten Server.  

Beginnend mit dem Kompatibilitätsmodus 130 wurden alle neuen Fixes und Features, die Auswirkungen auf einen Abfrageplan haben, ausdrücklich nur zum neuen Kompatibilitätsmodus hinzugefügt. Dadurch sollte das Risiko während der Upgrades minimiert werden, die durch Leistungseinbußen aufgrund von Abfrageplanänderungen entstanden sind, die möglicherweise auf neue Verhaltensweisen der Abfrageoptimierung zurückgeführt werden können.      

Verwenden Sie für Anwendungen den niedrigeren Kompatibilitätsgrad als sichereren Migrationspfad, um versionsbedingte Unterschiede in den Verhalten zu umgehen, die über die jeweilige Einstellung für den Kompatibilitätsgrad gesteuert werden. Das Ziel sollte weiterhin darin bestehen, zu einem späteren Zeitpunkt ein Upgrade auf den neuesten Kompatibilitätsgrad durchzuführen, damit einige der neuen Features wie die [intelligente Abfrageverarbeitung](../../relational-databases/performance/intelligent-query-processing.md) auf kontrollierte Weise übernommen werden können. 

Ausführlichere Informationen einschließlich des empfohlenen Workflows für ein Upgrade des Datenbank-Kompatibilitätsgrads finden Sie unter [Bewährte Methoden zum Aktualisieren des Datenbank-Kompatibilitätsgrads](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#best-practices-for-upgrading-database-compatibility-level).

> [!IMPORTANT]
> **Nicht mehr unterstützte** Funktionen, die in einer bestimmten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Version eingeführt wurden, werden durch den Kompatibilitätsgrad **nicht** geschützt. Dies bezieht sich auf Funktionalität, die aus der [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] entfernt wurde.
> Der `FASTFIRSTROW`-Hinweis wurde beispielweise in [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] nicht mehr unterstützt und durch den `OPTION (FAST n )`-Hinweis ersetzt. Wenn der Datenbank-Kompatibilitätsgrad auf 110 festgelegt wird, wird der nicht mehr unterstützte Hinweis nicht wiederhergestellt.  
>  
> Weitere Informationen zu nicht mehr unterstützten Funktionen finden Sie unter [Nicht mehr unterstützte Datenbank-Engine-Funktionen in SQL Server](../../database-engine/discontinued-database-engine-functionality-in-sql-server.md) und [Nicht mehr unterstützte Datenbank-Engine-Funktionen in SQL Server 2014](/previous-versions/sql/2014/database-engine/discontinued-database-engine-functionality-in-sql-server-2016?preserve-view=true&view=sql-server-2014).

> [!IMPORTANT]
> **Breaking Changes**, die in einer bestimmten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Version eingeführt wurden, werden **möglicherweise nicht** durch den Kompatibilitätsgrad geschützt. Dies bezieht sich auf Verhaltensänderungen zwischen Versionen der [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. Das Verhalten von [!INCLUDE[tsql](../../includes/tsql-md.md)] wird normalerweise durch den Kompatibilitätsgrad geschützt. Geänderte oder entfernte Systemobjekte werden jedoch **nicht** durch den Kompatibilitätsgrad geschützt.
>
> Ein Beispiel für eine wichtige Änderung, die durch den Kompatibilitätsgrad **geschützt** wird, ist eine implizite Konvertierung vom Datentyp „DateTime“ in den Datentyp „DateTime2“. Unter dem Datenbank-Kompatibilitätsgrad 130 ergibt sich daraus eine verbesserte Genauigkeit, indem die Bruchteile von Millisekunden berücksichtigt werden, wodurch sich unterschiedliche konvertierte Werte ergeben. Legen Sie zum Wiederherstellen des vorherigen Konvertierungsverhaltens den Datenbank-Kompatibilitätsgrad auf 120 oder niedriger fest.
>
> Zu Beispielen für wichtige Änderungen, die **nicht** durch den Kompatibilitätsgrad geschützt sind, zählen die folgenden:
>
> - Geänderte Spaltennamen in Systemobjekten. In [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] wurde die Spalte *single_pages_kb* in sys.dm_os_sys_info in *pages_kb* umbenannt. Unabhängig vom Kompatibilitätsgrad erzeugt die Abfrage `SELECT single_pages_kb FROM sys.dm_os_sys_info` den Fehler 207 (ungültiger Spaltenname).
> - Entfernte Systemobjekte. In [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] wurde `sp_dboption` entfernt. Unabhängig vom Kompatibilitätsgrad erzeugt die Anweisung `EXEC sp_dboption 'AdventureWorks2016', 'autoshrink', 'FALSE';` den Fehler 2812 (gespeicherte Prozedur „sp_dboption“ konnte nicht gefunden werden).
>
> Weitere Informationen zu wichtigen Änderungen finden Sie unter [Wichtige Änderungen für Datenbank-Engine-Features in SQL Server 2019](../../database-engine/breaking-changes-to-database-engine-features-in-sql-server-version-15.md), [Wichtige Änderungen für Datenbank-Engine-Features in SQL Server 2017](../../database-engine/breaking-changes-to-database-engine-features-in-sql-server-2017.md), [Wichtige Änderungen für Datenbank-Engine-Features in SQL Server 2016](../../database-engine/breaking-changes-to-database-engine-features-in-sql-server-2016.md) und [Wichtige Änderungen für Datenbank-Engine-Features in SQL Server 2014](/previous-versions/sql/2014/database-engine/breaking-changes-to-database-engine-features-in-sql-server-2016?preserve-view=true&view=sql-server-2014).

## <a name="differences-between-compatibility-levels"></a>Unterschiede zwischen Kompatibilitätsgraden
Bei allen Installationen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wird der Standardkompatibilitätsgrad von der Version von [!INCLUDE[ssDE](../../includes/ssde-md.md)] abgeleitet. Dies wird in [dieser Tabelle](#supported-dbcompats) veranschaulicht. Planen Sie für neue Entwicklungsprojekte immer die Zertifizierung der Anwendungen auf den aktuellsten Datenbank-Kompatibilitätsgrad.

Die [!INCLUDE[tsql](../../includes/tsql-md.md)]-Syntax wird nicht durch den Datenbank-Kompatibilitätsgrad abgegrenzt, es sei denn, vorhandene Anwendungen können unterbrochen werden, indem ein Konflikt mit [!INCLUDE[tsql](../../includes/tsql-md.md)]-Benutzercode erstellt werden kann. Diese Ausnahmen werden in den nächsten Abschnitten dieses Artikels dokumentiert. In diesen werden die Unterschiede zwischen bestimmten Kompatibilitätsgraden erläutert.

Der Datenbank-Kompatibilitätsgrad bietet zudem Abwärtskompatibilität mit früheren Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], da angefügte oder aus älteren Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wiederhergestellte Datenbanken ihren vorhandenen Kompatibilitätsgrad beibehalten, sofern dieser dem mindestens zulässigen Kompatibilitätsgrad oder höher entspricht. Diese Vorgehensweise wurde im Abschnitt [Verwenden des Kompatibilitätsgrads für Abwärtskompatibilität](#backwardCompat) bereits erläutert.

Ab Datenbank-Kompatibilitätsgrad 130 werden alle neuen Fixes und Features, die sich auf Abfragepläne auswirken, nur zum aktuellsten Kompatibilitätsgrad hinzugefügt. Dieser wird auch als „Standardkompatibilitätsgrad“ bezeichnet. Dadurch sollte das Risiko während der Upgrades minimiert werden, die durch Leistungseinbußen aufgrund von Abfrageplanänderungen entstanden sind und möglicherweise auf neue Verhaltensweisen der Abfrageoptimierung zurückgeführt werden können. 

Diese grundlegenden Änderungen, die sich auf den Plan auswirken, werden nur zum Standardkompatibilitätsgrad einer neuen Version von [!INCLUDE[ssDE](../../includes/ssde-md.md)] hinzugefügt:

1.  **Fixes für den Abfrageoptimierer, die für vorherige Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unter dem Ablaufverfolgungsflag 4199 veröffentlicht wurden, werden im Standardkompatibilitätsgrad einer neueren Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] automatisch aktiviert.** **Anwendungsbereich:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

    Als [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] veröffentlicht wurde, wurden beispielsweise alle Fixes für den Abfrageoptimierer, die für vorherige Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (und die Kompatibilitätsgrade 100 bis 120) veröffentlicht wurden, automatisch für Datenbanken aktiviert, die den Standardkompatibilitätsgrad von [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] (130) verwenden. Nur Fixes für den Abfrageoptimierer, die für Versionen nach RTM gelten, müssen explizit aktiviert werden.
    
    > [!NOTE]
    > Sie können folgende Methoden verwenden, um Fixes für den Abfrageoptimierer zu aktivieren:    
    >
    > - auf Serverebene mit dem [Ablaufverfolgungsflag 4199](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md#4199)
    > - auf Datenbankebene mit der `QUERY_OPTIMIZER_HOTFIXES`-Option in [ALTER DATABASE SCOPED CONFIGURATION (Transact-SQL)](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)
    > - auf Abfrageebene mit dem [Abfragehinweis](../../t-sql/queries/hints-transact-sql-query.md#use_hint) `USE HINT 'ENABLE_QUERY_OPTIMIZER_HOTFIXES'`
    
    Als [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] dann veröffentlicht wurde, wurden alle Fixes für den Abfrageoptimierer, die nach [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] RTM veröffentlicht wurden, automatisch für Datenbanken aktiviert, die den Standardkompatibilitätsgrad von [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] (140) verwenden. Dabei handelt es sich um kumulatives Verhalten, das auch alle früheren Versionen der Fixes enthält. Auch hier müssen nur Fixes für den Abfrageoptimierer, die für Post-RTM gelten, explizit aktiviert werden.  
    
    In der folgenden Tabelle wird dieses Verhalten zusammengefasst:
    
    |Version der Datenbank-Engine|Datenbank-Kompatibilitätsgrad|Ablaufverfolgungsflag 4199|Abfrageoptimierer-Änderungen aus früheren Datenbank-Kompatibilitätsgraden|Abfrageoptimierer-Änderungen für Versionen der Datenbank-Engine nach RTM|
    |----------|----------|---|------------|--------|
    |13 ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)])|100 bis 120<br /><br /><br />130|Aus<br />Andererseits<br /><br />Aus<br />Andererseits|**Disabled**<br />Aktiviert<br /><br />**Aktiviert**<br />Aktiviert|Disabled<br />Aktiviert<br /><br />Disabled<br />Aktiviert|
    |14 ([!INCLUDE[ssSQL17](../../includes/sssql17-md.md)])|100 bis 120<br /><br /><br />130<br /><br /><br />140|Aus<br />Andererseits<br /><br />Aus<br />Andererseits<br /><br />Aus<br />Andererseits|**Disabled**<br />Aktiviert<br /><br />**Aktiviert**<br />Aktiviert<br /><br />**Aktiviert**<br />Aktiviert|Disabled<br />Aktiviert<br /><br />Disabled<br />Aktiviert<br /><br />Disabled<br />Aktiviert|
    |15 ([!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]) und 12 ([!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)])|100 bis 120<br /><br /><br />130 bis 140<br /><br /><br />150|Aus<br />Andererseits<br /><br />Aus<br />Andererseits<br /><br />Aus<br />Andererseits|**Disabled**<br />Aktiviert<br /><br />**Aktiviert**<br />Aktiviert<br /><br />**Aktiviert**<br />Aktiviert|Disabled<br />Aktiviert<br /><br />Disabled<br />Aktiviert<br /><br />Disabled<br />Aktiviert|
    
    > [!IMPORTANT]
    > Fixes für den Abfrageoptimierer, die falsche Ergebnisse oder Fehler durch Zugriffsverletzungen beheben, werden nicht durch das Ablaufverfolgungsflag 4199 geschützt. Diese Fixes sind nicht optional.
 
2.  **Änderungen an der [Kardinalitätsschätzung](../../relational-databases/performance/cardinality-estimation-sql-server.md), die in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] veröffentlicht wurden, werden nur im Standardkompatibilitätsgrad einer neuen Version von [!INCLUDE[ssDE](../../includes/ssde-md.md)]** , nicht jedoch in früheren Kompatibilitätsgraden veröffentlicht. 

    Als [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] veröffentlicht wurde, waren Änderungen an der Kardinalitätsschätzung beispielsweise nur für Datenbanken verfügbar, die den Standardkompatibilitätsgrad von [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] (130) verwendet haben. Für frühere Kompatibilitätsgrade wurde das Verhalten der Kardinalitätsschätzung beibehalten, das vor [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] verfügbar war. 
    
    Als [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] später veröffentlicht wurde, waren neuere Änderungen an der Kardinalitätsschätzung beispielsweise nur für Datenbanken verfügbar, die den Standardkompatibilitätsgrad von [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] (140) verwendet haben. Für den Datenbank-Kompatibilitätsgrad 130 wurde das Verhalten der Kardinalitätsschätzung von [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] beibehalten.
    
    In der folgenden Tabelle wird dieses Verhalten zusammengefasst:
    
    |Version der Datenbank-Engine|Datenbank-Kompatibilitätsgrad|Änderungen an neueren Versionen der Kardinalitätsschätzung|
    |----------|--------|-------------|
    |13 ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)])|130<br />130|Disabled<br />Aktiviert|
    |14 ([!INCLUDE[ssSQL17](../../includes/sssql17-md.md)])<sup>1</sup>|< 140<br />140|Disabled<br />Aktiviert|
    |15 ([!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)])<sup>1</sup>|< 150<br />150|Disabled<br />Aktiviert|
    
    <sup>1</sup> Gilt auch für [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
    
> [!IMPORTANT]
> Weitere Unterschiede zwischen den einzelnen Kompatibilitätsgraden werden in den nächsten Abschnitten dieses Artikels behandelt.

## <a name="differences-between-compatibility-level-140-and-level-150"></a>Unterschiede zwischen Kompatibilitätsgrad 140 und Kompatibilitätsgrad 150
In diesem Abschnitt werden neue mit Kompatibilitätsgrad 150 eingeführte Verhaltensweisen beschrieben.

|Kompatibilitätsgradeinstellung 140 oder niedriger|Kompatibilitätsgradeinstellung 150|
|--------------------------------------------------|-----------------------------------------|
|Relationale Data Warehouse- und Analyseworkloads können Columnstore-Indizes möglicherweise aufgrund des Mehraufwands für OLTP, fehlender Unterstützung durch den Hersteller oder anderer Einschränkungen nicht unten.  Ohne Columnstore-Indizes können diese Workloads nicht vom Batchausführungsmodus profitieren.|Der Batchausführungsmodus ist nun für Analyseworkloads verfügbar, ohne dass Columnstore-Indizes erforderlich sind. Weitere Informationen finden Sie unter [Batchmodus bei Rowstore](../../relational-databases/performance/intelligent-query-processing.md#batch-mode-on-rowstore).|
|Abfragen im Zeilenmodus, die nicht genügend Speicherzuweisungen anfordern, was zu einem Überlauf auf Datenträger führt, haben möglicherweise weiterhin Probleme mit aufeinanderfolgenden Ausführungen.|Abfragen im Zeilenmodus, die nicht genügend Arbeitsspeicherzuweisungen anfordern, was zu einem Überlauf auf Datenträger führt, weisen bei aufeinanderfolgenden Ausführungen möglicherweise eine Leistungssteigerung auf. Weitere Informationen finden Sie unter [Feedback zur Speicherzuweisung im Zeilenmodus](../../relational-databases/performance/intelligent-query-processing.md#row-mode-memory-grant-feedback).|
|Abfragen im Zeilenmodus, die eine übermäßige Speicherzuweisung anfordern, was zu Parallelitätsproblemen führt, haben möglicherweise weiterhin Probleme mit aufeinanderfolgenden Ausführungen.|Abfragen im Batchmodus, die eine übermäßige Speicherzuweisung anfordern, was zu Parallelitätsproblemen führt, weisen bei aufeinanderfolgenden Ausführungen möglicherweise eine verbesserte Parallelität auf. Weitere Informationen finden Sie unter [Feedback zur Speicherzuweisung im Zeilenmodus](../../relational-databases/performance/intelligent-query-processing.md#row-mode-memory-grant-feedback).|
|Abfragen, die auf TSQL_SCALAR_UDFs verweisen, verwenden einen iterativen Aufruf, verursachen keine Kosten und erzwingen die serielle Ausführung. |TSQL_SCALAR_UDFs werden in äquivalente relationale Ausdrücke transformiert, die per „Inlining“ durch die aufrufende Abfrage ersetzt werden, was oft zu erheblichen Leistungssteigerungen führt. Weitere Informationen finden Sie unter [TSQL_SCALAR_UDF_INLINING](../../relational-databases/performance/intelligent-query-processing.md#scalar-udf-inlining).|
|Tabellenvariablen nutzen für die Kardinalitätsschätzung eine festgelegte Schätzung.  Wenn die tatsächliche Anzahl der Zeilen sehr viel höher als der geschätzter Wert ist, kann die Leistung nachgelagerter Vorgänge beeinträchtigt werden. |Neue Pläne verwenden anstelle einer festgelegten Schätzung die tatsächliche Kardinalität der Tabellenvariablen, die bei der ersten Kompilierung vorgefunden wird. Weitere Informationen finden Sie unter [Verzögerte Kompilierung von Tabellenvariablen](../../relational-databases/performance/intelligent-query-processing.md#table-variable-deferred-compilation).|

Weitere Informationen zu Abfrageverarbeitungsfeatures, die im Datenbank-Kompatibilitätsgrad 150 aktiviert sind, finden Sie unter [Neuigkeiten zu SQL Server 2019](../../sql-server/what-s-new-in-sql-server-ver15.md) und [Intelligente Abfrageverarbeitung in SQL Server-Datenbanken](../../relational-databases/performance/intelligent-query-processing.md).

## <a name="differences-between-compatibility-level-130-and-level-140"></a>Unterschiede zwischen Kompatibilitätsgrad 130 und Kompatibilitätsgrad 140

In diesem Abschnitt werden neue mit Kompatibilitätsgrad 140 eingeführte Verhaltensweisen beschrieben.

|Kompatibilitätsgradeinstellung 130 oder niedriger|Kompatibilitätsgradeinstellung 140|
|--------------------------------------------------|-----------------------------------------|
|Bei Kardinalitätsschätzungen für Anweisungen, die auf Tabellenwertfunktionen mit mehreren Anweisungen verweisen, wird eine feste Zeilenvorhersage verwendet.|Bei Kardinalitätsschätzungen für zulässige Anweisungen, die auf Tabellenwertfunktionen mit mehreren Anweisungen verweisen, wird die tatsächliche Kardinalität der Funktionsausgabe verwendet. Dies wird über die **geschachtelte Ausführung** für Tabellenwertfunktionen mit mehreren Anweisungen aktiviert.|
|Abfragen im Batchmodus, die nicht genügend Arbeitsspeicherzuweisungen anfordern, was zu einem Überlauf der Datenträger führt, haben möglicherweise weiterhin Probleme mit aufeinanderfolgenden Ausführungen.|Abfragen im Batchmodus, die nicht genügend Arbeitsspeicherzuweisungen anfordern, was zu einem Überlauf der Datenträger führt, weisen bei aufeinanderfolgenden Ausführungen möglicherweise eine Leistungssteigerung auf. Dies wird über das **Feedback zur Speicherzuweisung im Batchmodus** ermöglicht, das die Arbeitsspeicherzuweisung eines zwischengespeicherten Plans aktualisiert, wenn es bei Operatoren im Batchmodus zu Überläufen kommt. |
|Abfragen im Batchmodus, die eine übermäßige Arbeitsspeicherzuweisung anfordern, die zu Parallelitätsproblemen führt, haben möglicherweise weiterhin Probleme mit aufeinanderfolgenden Ausführungen.|Abfragen im Batchmodus, die eine übermäßige Arbeitsspeicherzuweisung anfordern, die zu Parallelitätsproblemen führt, weisen bei aufeinanderfolgenden Ausführungen möglicherweise eine verbesserte Parallelität auf. Dies wird über das **Feedback zur Speicherzuweisung im Batchmodus** ermöglicht, das die Arbeitsspeicherzuweisung eines zwischengespeicherten Plans aktualisiert, wenn ursprünglich eine übermäßige Speicherkapazität angefordert wurde.|
|Abfragen im Batchmodus, die Verknüpfungsoperatorrn enthalten, sind für drei physische LOIN-Algorithmen geeignet, zu denen geschachtelte Schleifen, Hashjoins und Mergejoins zählen. Wenn Kardinalitätsschätzungen für Join-Eingaben falsch sind, wird möglicherweise ein ungeeigneter JOIN-Algorithmus ausgewählt. Dies hat negative Auswirkungen auf die Leistung. Zudem wird der ungeeignete JOIN-Algorithmus so lange weiter verwendet, bis der zwischengespeicherte Plan erneut kompiliert wurde.|Es gibt einen weiteren Verknüpfungsoperator mit dem Namen **adaptiver Join**. Wenn Kardinalitätsschätzungen für die äußere erstellte Join-Eingaben falsch sind, wird möglicherweise ein ungeeigneter Join-Algorithmus ausgewählt. Wenn dieser Fall eintritt und die Anweisung für einen adaptiven Join geeignet ist, wird auf dynamische Weise für kleinere Join-Eingaben eine geschachtelte Schleife und für umfangreichere Join-Eingaben ein Hashjoin verwendet, ohne dass eine Rekompilierung erforderlich ist. |
|Triviale Pläne, die auf Columnstore-Indizes verweisen, sind für eine Ausführung im Batchmodus nicht geeignet. |Ein trivialer Plan, der auf Columnstore-Indizes verweist, wird zugunsten eines Plans gelöscht, der für eine Ausführung im Batchmodus geeignet ist.|
|Der `sp_execute_external_script`-UDX-Operator kann nur im Zeilenmodus ausgeführt werden.|Der `sp_execute_external_script`-UDX-Operator ist für eine Ausführung im Batchmodus geeignet.|
|Tabellenwertfunktionen mit mehreren Anweisungen verfügen über keine verschachtelte Ausführung |Verschachtelte Ausführung für Tabellenwertfunktionen mit mehreren Anweisungen zur Verbesserung der Planqualität.|

Fixes unter dem Ablaufverfolgungsflag 4199 in früheren Versionen von SQL Server vor SQL Server 2017 sind jetzt standardmäßig aktiviert. Mit Kompatibilitätsmodus 140. Das Ablaufverfolgungsflag 4199 gilt weiterhin für Fixes für den neuen Abfrageoptimierer, die nach SQL Server 2017 veröffentlicht werden. Informationen zum Ablaufverfolgungsflag 4199 finden Sie unter [Ablaufverfolgungsflag 4199](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md#4199).

## <a name="differences-between-compatibility-level-120-and-level-130"></a>Unterschiede zwischen Kompatibilitätsgrad 120 und Kompatibilitätsgrad 130

In diesem Abschnitt werden neue mit Kompatibilitätsgrad 130 eingeführte Verhaltensweisen beschrieben.

|Kompatibilitätsgradeinstellung 120 oder niedriger|Kompatibilitätsgradeinstellung 130|
|--------------------------------------------------|-----------------------------------------|
|Die INSERT-Anweisung in einer INSERT-SELECT-Anweisung erhält einen einzelnen Thread.|Die INSERT-Anweisung in einer INSERT-SELECT-Anweisung erhält mehrere Threads oder kann einen parallelen Plan aufweisen.|
|Abfragen für eine speicheroptimierte Tabelle werden in einem einzelnen Thread ausgeführt.|Abfragen für eine speicheroptimierte Tabelle können jetzt parallele Pläne aufweisen.|
|Einführung der SQL 2014-Kardinalitätsschätzung **CardinalityEstimationModelVersion="120"**|Weitere Verbesserungen der Kardinalitätsschätzung im Kardinalitätsschätzungsmodell 130, das in einem Abfrageplan angezeigt werden kann. **CardinalityEstimationModelVersion="130"**|
|Änderungen des Batchmodus im Vergleich zu Änderungen des Zeilenmodus mit Columnstore-Indizes:<br /><ul><li>Sortierungen in einer Tabelle mit Columnstore-Index weisen einen Zeilenmodus auf <li>Fensterfunktionsaggregate werden in einem Zeilenmodus wie `LAG` oder `LEAD` ausgeführt <li>Abfragen in Columnstore-Tabellen mit mehreren unterschiedlichen, im Zeilenmodus ausgeführten Klauseln <li>Abfragen, die unter MAXDOP 1 oder mit einem seriellen Plan im Zeilenmodus ausgeführt werden</li></ul>| Änderungen des Batchmodus im Vergleich zu Änderungen des Zeilenmodus mit Columnstore-Indizes:<br /><ul><li>Sortierungen in einer Tabelle mit einem Columnstore-Index weisen jetzt einen Batchmodus auf <li>Fensteraggregate werden jetzt in einem Batchmodus wie `LAG` oder `LEAD` ausgeführt <li>Abfragen für Columnstore-Tabellen mit mehreren unterschiedlichen, im Batchmodus ausgeführten Klauseln <li>Abfragen unter MAXDOP 1 oder mit einem seriellen Plan werden im Batchmodus ausgeführt</li></ul>|
|Statistiken können automatisch aktualisiert werden. | Die Logik, die Statistiken automatisch aktualisiert, ist bei umfangreichen Tabellen aggressiver. In der Praxis soll dies Fälle reduzieren, in denen Kunden bei Abfragen Leistungsprobleme feststellen, bei denen häufig neu eingefügte Zeilen abgefragt werden, bei denen die Statistiken jedoch nicht entsprechend aktualisiert wurden und diese Werte noch nicht enthalten sind. |
|Die Ablaufverfolgung 2371 ist in [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] standardmäßig auf OFF festgelegt. | Die [Ablaufverfolgung 2371](/archive/blogs/psssql/default-auto-statistics-update-threshold-change-for-sql-server-2016) ist in [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] standardmäßig auf ON festgelegt. Das Ablaufverfolgungsflag 2371 weist den automatischen Statistikupdater an, in einer Tabelle mit vielen Zeilen Stichproben von einer kleineren, aber sinnvolleren Teilmenge von Zeilen durchzuführen. <br/> <br/> Eine Verbesserung besteht darin, in der Stichprobe mehr Zeilen einzubeziehen, die kürzlich eingefügt wurden. <br/> <br/> Eine weitere Verbesserung besteht darin, Abfragen während des Prozesses der Statistikaktualisierung nicht zu blockieren, sondern weiter auszuführen. |
|Für den Kompatibilitätsgrad 120 werden in einem Singlethreadprozess Stichproben aus Statistiken entnommen.|Für den Kompatibilitätsgrad 130 werden in einem Multithreadprozess Stichproben aus Statistiken entnommen. |
|Der Grenzwert liegt bei 253 eingehenden Fremdschlüsseln.| Bis zu 10.000 eingehende Fremdschlüssel oder vergleichbare Referenzen können auf eine bestimmte Tabelle verweisen. Einschränkungen finden Sie unter [Create Foreign Key Relationships](../../relational-databases/tables/create-foreign-key-relationships.md). |
|Die als veraltet markierten Hashalgorithmen MD2, MD4, MD5, SHA und SHA1 sind zulässig.|Nur die Hashalgorithmen SHA2_256 und SHA2_512 sind zulässig.|
||[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] schließt Verbesserungen bei einigen Datentypkonvertierungen und einigen Vorgängen (eher selten) ein. Weitere Einzelheiten finden Sie unter [SQL Server 2016 improvements in handling some data types and uncommon operations](https://support.microsoft.com/help/4010261/sql-server-2016-improvements-in-handling-some-data-types-and-uncommon) (Verbesserungen der Verarbeitung einiger Datentypen und seltener Vorgänge in SQL Server 2016).|
|Die Funktion `STRING_SPLIT` ist nicht verfügbar.|Die Funktion `STRING_SPLIT` steht für den Kompatibilitätsgrad 130 oder höher zur Verfügung. Wenn der Kompatibilitätsgrad Ihrer Datenbank niedriger als 130 ist, kann [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die `STRING_SPLIT`-Funktion nicht suchen und ausführen.|

Fixes unter dem Ablaufverfolgungsflag 4199 in früheren Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vor [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] sind jetzt standardmäßig aktiviert. Mit Kompatibilitätsmodus 130. Das Ablaufverfolgungsflag 4199 gilt weiterhin für Fixes für den neuen Abfrageoptimierer, die nach [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] veröffentlicht werden. Für die Verwendung des älteren Abfrageoptimierers in [!INCLUDE[ssSDS](../../includes/sssds-md.md)] müssen Sie den Kompatibilitätsgrad 110 auswählen. Informationen zum Ablaufverfolgungsflag 4199 finden Sie unter [Ablaufverfolgungsflag 4199](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md#4199).

## <a name="differences-between-lower-compatibility-levels-and-level-120"></a>Unterschiede zwischen niedrigeren Kompatibilitätsgraden und Kompatibilitätsgrad 120

In diesem Abschnitt werden neue mit Kompatibilitätsgrad 120 eingeführte Verhaltensweisen beschrieben.

|Kompatibilitätsgradeinstellung 110 oder niedriger|Kompatibilitätsgradeinstellung 120|
|--------------------------------------------------|-----------------------------------------|
|Der ältere Abfrageoptimierer wird verwendet.|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] schließt deutliche Verbesserungen der Komponente zum Erstellen und Optimieren von Abfrageplänen ein. Diese neue Funktion des Abfrageoptimierers ist nur bei Verwendung des Datenbank-Kompatibilitätsgrads 120 verfügbar. Damit diese Verbesserungen optimal genutzt werden können, sollten neue Datenbankanwendungen mit dem Datenbank-Kompatibilitätsgrad 120 entwickelt werden. Von früheren [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Versionen migrierte Anwendungen sollten sorgfältig daraufhin überprüft werden, ob die bisherige gute Leistung aufrechterhalten bzw. verbessert wird. Wird die Leistung beeinträchtigt, können Sie den Kompatibilitätsgrad der Datenbank auf 110 oder einen niedrigeren Wert festlegen, um die ältere Methodologie des Abfrageoptimierers zu nutzen.<br /><br /> Der Datenbank-Kompatibilitätsgrad 120 verwendet eine neue Kardinalitätsschätzung, die für moderne Data-Warehousing- und OLTP-Arbeitsauslastungen optimiert ist. Bevor Sie den Datenbank-Kompatibilitätsgrad aufgrund von Leistungsproblemen auf 110 festlegen, sollten Sie die Empfehlungen im Abschnitt *Abfragepläne* des [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]-Artikels [Neues in der Datenbank-Engine](../../sql-server/what-s-new-in-sql-server-2016.md) lesen.|
|Bei Kompatibilitätsgraden unter 120 wird die Spracheinstellung beim Konvertieren eines **date**-Werts in einen Zeichenfolgenwert ignoriert. Beachten Sie, dass dieses Verhalten nur für den **date**-Typ spezifisch ist. Siehe Beispiel B im Abschnitt „Beispiele“ weiter unten.|Die Spracheinstellung wird nicht ignoriert, wenn ein **date**-Wert in eine Zeichenfolge konvertiert wird.|
|Rekursive Verweise auf der rechten Seite einer `EXCEPT`-Klausel erzeugen eine Endlosschleife. In Beispiel C im nachfolgenden Abschnitt „Beispiele“ wird dieses Verhalten veranschaulicht.|Rekursive Verweise in einer `EXCEPT`-Klausel generieren gemäß dem ANSI SQL-Standard einen Fehler.|
|Der rekursive allgemeine Tabellenausdruck (Common Table Expression, CTE) lässt doppelte Spaltennamen zu.|Der rekursive CTE lässt keine doppelten Spaltennamen zu.|
|Deaktivierte Trigger werden aktiviert, wenn die Trigger geändert werden.|Das Ändern eines Triggers ändert nicht den Status (deaktiviert oder aktiviert) des Triggers.|
|Die OUTPUT INTO-Tabellenklausel ingoiert die `IDENTITY_INSERT SETTING = OFF` und lässt zu, dass explizite Werte eingefügt werden.|Sie können keine expliziten Werte für eine Identitätsspalte in einer Tabelle einfügen, wenn `IDENTITY_INSERT` auf OFF festgelegt ist.|
|Wenn die Datenbankkapselung auf PARTIAL festgelegt ist, kann die Überprüfung des `$action`-Felds in der `OUTPUT`-Klausel einer `MERGE`-Anweisung einen Sortierungsfehler zurückgeben.|Die von der `$action`-Klausel einer `MERGE`-Anweisung zurückgegebene Sortierung der Werte entspricht der Datenbanksortierung anstelle der Serversortierung. Es wird kein Sortierungskonfliktfehler zurückgegeben.|
|Durch eine `SELECT INTO`-Anweisung wird immer ein Singlethread-Einfügevorgang erstellt.|Durch eine `SELECT INTO`-Anweisung kann ein paralleler Einfügevorgang erstellt werden. Beim Einfügen einer großer Anzahl von Zeilen kann die Leistung durch den parallelen Vorgang verbessert werden.|

## <a name="differences-between-lower-compatibility-levels-and-levels-100-and-110"></a>Unterschiede zwischen niedrigeren Kompatibilitätsgraden und den Kompatibilitätsgraden 100 und 110

In diesem Abschnitt werden neue mit Kompatibilitätsgrad 110 eingeführte Verhaltensweisen beschrieben. Dieser Abschnitt bezieht sich auch auf höhere Kompatibilitätsgrade als 110.

|Kompatibilitätsgradeinstellung 100 oder niedriger|Kompatibilitätsgradeinstellung 110 oder höher|
|--------------------------------------------------|--------------------------------------------------|
|Common Language Runtime (CLR)-Datenbankobjekte werden mit Version 4 der CLR ausgeführt. Einige in Version 4 der CLR eingeführte Verhaltensänderungen werden jedoch vermieden. Weitere Informationen finden Sie unter [Neues in der CLR-Integration](../../relational-databases/clr-integration/clr-integration-what-s-new.md).|CLR-Datenbankobjekte werden mit Version 4 der CLR ausgeführt.|
|Die XQuery-Funktionen **string-length** und **substring** betrachten jedes Ersatzzeichen als zwei Zeichen.|Die XQuery-Funktionen **string-length** und **substring** betrachten jedes Ersatzzeichen als ein Zeichen.|
|`PIVOT` ist in einer Abfrage für einen rekursiven allgemeinen Tabellenausdruck (Common Table Expression, CTE) zulässig. Die Abfrage gibt jedoch falsche Ergebnisse zurück, wenn mehrere Zeilen pro Gruppierung vorhanden sind.|`PIVOT` ist in einer Abfrage für einen rekursiven allgemeinen Tabellenausdruck (Common Table Expression, CTE) nicht zulässig. Es wird ein Fehler zurückgegeben.|
|Der RC4-Algorithmus wird nur aus Gründen der Abwärtskompatibilität unterstützt. Neues Material kann nur mit RC4 oder RC4_128 verschlüsselt werden, wenn die Datenbank den Kompatibilitätsgrad 90 oder 100 besitzt. (Nicht empfohlen.) In [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] kann mit RC4 oder RC4_128 verschlüsseltes Material in jedem Kompatibilitätsgrad entschlüsselt werden.|Neues Material kann nicht mit RC4 oder RC4_128 verschlüsselt werden. Verwenden Sie stattdessen einen neueren Algorithmus, z. B. einen der AES-Algorithmen. In [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] kann mit RC4 oder RC4_128 verschlüsseltes Material in jedem Kompatibilitätsgrad entschlüsselt werden.|
|Das Standardformat für `CAST`- und `CONVERT`-Vorgänge bei den Datentypen **time** und **datetime2** ist 121, es sei denn, einer der Typen wird in einem berechneten Spaltenausdruck verwendet. Für berechnete Spalten ist das Standardformat 0. Dieses Verhalten wirkt sich auf berechnete Spalten aus, wenn sie erstellt werden und in Abfragen mit automatischer Parametrisierung oder in Einschränkungsdefinitionen verwendet werden.<br /><br /> In Beispiel D im nachfolgenden Abschnitt „Beispiele“ wird der Unterschied zwischen den Formaten 0 und 121 veranschaulicht. Das oben beschriebene Verhalten wird nicht veranschaulicht. Weitere Informationen zu Datums- und Zeitformaten finden Sie unter [CAST und CONVERT](../../t-sql/functions/cast-and-convert-transact-sql.md).|Unter dem Kompatibilitätsgrad 110 ist das Standardformat für `CAST`- und `CONVERT`-Vorgänge im Fall der Datentypen **time** und **datetime2** immer 121. Basiert die Abfrage auf dem alten Verhalten, verwenden Sie einen Kompatibilitätsgrad unter 110, oder geben Sie in der betroffenen Abfrage explizit das Format 0 an.<br /><br /> Ein Update der Datenbank auf Kompatibilitätsgrad 110 ändert keine Benutzerdaten, die auf dem Datenträger gespeichert wurden. Sie müssen diese Daten entsprechend manuell korrigieren. Haben Sie beispielsweise `SELECT INTO` zum Erstellen einer Tabelle von einer Quelle verwendet, die einen Ausdruck für eine berechnete Spalte (oben beschrieben) beinhaltete, werden die Daten mit dem Format 0 anstelle der Definition der berechneten Spalte an sich gespeichert. Sie müssen diese Daten manuell aktualisieren, um sie an das Format 121 anzupassen.|
|Alle Spalten in Remotetabellen des Typs **smalldatetime**, auf die in einer partitionierten Ansicht verwiesen wird, werden als **datetime** zugeordnet. Entsprechende Spalten in lokalen Tabellen (in derselben Ordnungsposition in der Auswahlliste) müssen den Typ **datetime** aufweisen.|Alle Spalten in Remotetabellen des Typs **smalldatetime**, auf die in einer partitionierten Ansicht verwiesen wird, werden als **smalldatetime** zugeordnet. Entsprechende Spalten in lokalen Tabellen (in derselben Ordnungsposition in der Auswahlliste) müssen den Typ **smalldatetime** aufweisen.<br /><br /> Nach dem Upgrade auf 110 schlägt die verteilte partitionierte Sicht wegen des Datentypkonflikts fehl. Sie können dieses Problem beheben, indem Sie den Datentyp in der Remotetabelle in **datetime** ändern oder den Kompatibilitätsgrad der lokalen Datenbank auf höchstens 100 festlegen.|
|Die `SOUNDEX`-Funktion implementiert die folgenden Regeln:<br /><br /> 1) H oder W in Großbuchstaben werden beim Trennen von zwei Konsonanten ignoriert, welche die gleiche Zahl im `SOUNDEX`-Code aufweisen.<br /><br /> 2) Wenn die ersten beiden Zeichen von *character_expression* die gleiche Zahl im `SOUNDEX`-Code aufweisen, werden beide Zeichen eingeschlossen. Wenn ein Satz nebeneinander liegender Konsonanten die gleiche Zahl im `SOUNDEX`-Code aufweist, werden andernfalls alle Zeichen außer dem ersten Zeichen ausgeschlossen.|Die `SOUNDEX`-Funktion implementiert die folgenden Regeln:<br /><br /> 1) Wenn H oder W in Großbuchstaben zwei Konsonanten trennt, welche die gleiche Zahl im `SOUNDEX`-Code aufweisen, wird der rechts stehende Konsonant ignoriert.<br /><br /> 2) Wenn ein Satz nebeneinander liegender Konsonanten die gleiche Zahl im `SOUNDEX`-Code aufweist, werden andernfalls alle Zeichen außer dem ersten Zeichen ausgeschlossen.<br /><br /> <br /><br /> Die zusätzlichen Regeln führen möglicherweise dazu, dass die von der `SOUNDEX`-Funktion berechneten Werte sich von den unter früheren Kompatibilitätsgraden berechneten Werten unterscheiden. Möglicherweise sind die Indizes, Heaps oder CHECK-Einschränkungen, welche die `SOUNDEX`-Funktion verwenden, nach dem Upgrade auf Kompatibilitätsgrad 110 erneut zu erstellen. Weitere Informationen finden Sie unter [SOUNDEX](../../t-sql/functions/soundex-transact-sql.md).|

## <a name="differences-between-compatibility-level-90-and-level-100"></a>Unterschiede zwischen Kompatibilitätsgrad 90 und Kompatibilitätsgrad 100

In diesem Abschnitt werden neue mit Kompatibilitätsgrad 100 eingeführte Verhaltensweisen beschrieben.

|Kompatibilitätsgradeinstellung 90|Kompatibilitätsgradeinstellung 100|Mögliche Auswirkung|
|----------------------------------------|-----------------------------------------|---------------------------|
|Die Einstellung QUOTED_IDENTIFER ist für Tabellenwertfunktionen mit mehreren Anweisungen immer auf ON festgelegt, wenn diese erstellt werden, unabhängig von der Einstellung auf Sitzungsebene.|Die QUOTED IDENTIFIER-Sitzungseinstellung wird berücksichtigt, wenn Tabellenwertfunktionen mit mehreren Anweisungen erstellt werden.|Medium|
|Wenn Sie eine Partitionsfunktion erstellen oder ändern, werden die Literale **datetime** und **smalldatetime** in der Funktion ausgewertet, wobei von US_Englisch als Spracheinstellung ausgegangen wird.|Die aktuelle Spracheinstellung wird verwendet, um die Literale **datetime** und **smalldatetime** in der Partitionsfunktion auszuwerten.|Medium|
|Die `FOR BROWSE`-Klausel ist in `INSERT`- und `SELECT INTO`-Anweisungen zulässig (und wird ignoriert).|Die `FOR BROWSE`-Klausel ist in `INSERT`- und `SELECT INTO`-Anweisungen nicht zulässig.|Medium|
|Volltextprädikate sind in der `OUTPUT`-Klausel zulässig.|Volltextprädikate sind in der `OUTPUT`-Klausel nicht zulässig.|Niedrig|
|`CREATE FULLTEXT STOPLIST`, `ALTER FULLTEXT STOPLIST` und `DROP FULLTEXT STOPLIST` werden nicht unterstützt. Die Systemstoppliste wird automatisch neuen Volltextindizes zugeordnet.|`CREATE FULLTEXT STOPLIST`, `ALTER FULLTEXT STOPLIST` und `DROP FULLTEXT STOPLIST` werden unterstützt.|Niedrig|
|`MERGE` wird nicht als reserviertes Schlüsselwort erzwungen.|MERGE ist ein vollständig reserviertes Schlüsselwort. Die `MERGE`-Anweisung wird bei den Kompatibilitätsgraden 100 und 90 unterstützt.|Niedrig|
|Die Verwendung des \<dml_table_source>-Arguments der INSERT-Anweisung löst einen Syntaxfehler aus.|Sie können die Ergebnisse einer OUTPUT-Klausel in einer geschachtelten INSERT-, UPDATE-, DELETE- oder MERGE-Anweisung aufzeichnen und diese Ergebnisse in eine Zieltabelle oder -sicht einfügen. Dies erfolgt über das \<dml_table_source>-Argument der INSERT-Anweisung.|Niedrig|
|Sofern `NOINDEX` nicht angegeben ist, führt `DBCC CHECKDB` oder `DBCC CHECKTABLE` sowohl physische als auch logische Konsistenzprüfungen für eine einzelne Tabelle oder indizierte Ansicht und alle zugehörigen, nicht gruppierten Indizes und XML-Indizes aus. Räumliche Indizes werden nicht unterstützt.|Sofern `NOINDEX` nicht angegeben ist, führt `DBCC CHECKDB` oder `DBCC CHECKTABLE` sowohl physische als auch logische Konsistenzprüfungen für eine einzelne Tabelle und alle zugehörigen, nicht gruppierten Indizes aus. Allerdings werden für XML-Indizes, räumliche Indizes und indizierte Sichten standardmäßig nur physische Konsistenzprüfungen ausgeführt.<br /><br /> Bei Angabe von `WITH EXTENDED_LOGICAL_CHECKS` werden logische Konsistenzprüfungen an indizierten Ansichten, XML-Indizes und räumlichen Indizes (sofern vorhanden) ausgeführt. Standardmäßig werden physische Konsistenzprüfungen vor den logischen Konsistenzprüfungen ausgeführt. Wenn `NOINDEX` ebenfalls angegeben ist, werden nur die logischen Prüfungen ausgeführt.|Niedrig|
|Wenn eine OUTPUT-Klausel mit einer DML-Anweisung (Data Manipulation Language, Datenbearbeitungssprache) verwendet wird und während der Ausführung der Anweisung ein Laufzeitfehler auftritt, wird die gesamte Transaktion beendet, und es wird ein Rollback für sie ausgeführt.|Wenn eine `OUTPUT`-Klausel mit einer DML-Anweisung (DML = Data Manipulation Language, Datenbearbeitungssprache) verwendet wird und während der Ausführung der Anweisung ein Laufzeitfehler auftritt, hängt das Verhalten von der `SET XACT_ABORT`-Einstellung ab. Wenn `SET XACT_ABORT` auf OFF festgelegt ist, führt ein Anweisungsabbruchfehler, der von der DML-Anweisung bei Verwendung der `OUTPUT`-Klausel generiert wird, zum Abbruch der Anweisung. Die Ausführung des Batches wird jedoch fortgesetzt, und es wird kein Rollback für die Transaktion ausgeführt. Wenn `SET XACT_ABORT` auf ON festgelegt ist, bewirken alle Laufzeitfehler, die von der DML-Anweisung bei Verwendung der OUTPUT-Klausel generiert werden, dass der Batch abgebrochen und für die Transaktion ein Rollback ausgeführt wird.|Niedrig|
|CUBE und ROLLUP werden nicht als reservierte Schlüsselwörter erzwungen.|`CUBE` und `ROLLUP` sind reservierte Schlüsselwörter innerhalb der GROUP BY-Klausel.|Niedrig|
|Für Elemente des **anyType**-XML-Datentyps wird die strict-Überprüfung angewendet.|Für Elemente des **anyType**-Datentyps wird die Lax-Überprüfung angewendet. Weitere Informationen finden Sie unter [Platzhalterkomponenten und Inhaltsüberprüfung](../../relational-databases/xml/wildcard-components-and-content-validation.md).|Niedrig|
|Die speziellen Attribute **xsi:nil** und **xsi:type** können durch DML-Anweisungen nicht abgefragt oder geändert werden.<br /><br /> Dies hat zur Folge, dass `/e/@xsi:nil` fehlschlägt, während `/e/@*` die Attribute **xsi:nil** und **xsi:type** ignoriert. `/e` gibt jedoch die Attribute **xsi:nil** und **xsi:type** aus Gründen der Konsistenz mit `SELECT xmlCol` zurück, sogar wenn `xsi:nil = "false"`.|Die speziellen Attribute **xsi:nil** und **xsi:type** werden als reguläre Attribute gespeichert und können abgefragt und geändert werden.<br /><br /> Beispielsweise werden bei der Ausführung der Abfrage `SELECT x.query('a/b/@*')` alle Attribute einschließlich **xsi:nil** und **xsi:type** zurückgegeben. Zum Ausschließen dieser Typen in der Abfrage müssen Sie `@*` durch `@*[namespace-uri(.) != "`*insert xsi namespace uri*`"` und nicht `(local-name(.) = "type"` oder `local-name(.) ="nil".` ersetzen.|Niedrig|
|Eine benutzerdefinierte Funktion, die eine XML-Zeichenfolgenkonstante in einen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-datetime-Typ konvertiert, wird als deterministisch gekennzeichnet.|Eine benutzerdefinierte Funktion, die eine XML-Zeichenfolgenkonstante in einen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-datetime-Typ konvertiert, wird als nicht deterministisch gekennzeichnet.|Niedrig|
|Die XML-Datentypen "union" und "list" werden nicht vollständig unterstützt.|Die XML-Datentypen "union" und "list" werden vollständig unterstützt, einschließlich der folgenden Funktionalität:<br /><br /> "union" von "list"<br /><br /> "union" von "union"<br /><br /> Liste der atomaren Typen<br /><br /> "list" von "union"|Niedrig|
|Die für eine xQuery-Methode erforderlichen SET-Optionen werden nicht überprüft, wenn die Methode in einer Sicht oder Inline-Tabellenwertfunktion enthalten ist.|Die für eine xQuery-Methode erforderlichen SET-Optionen werden überprüft, wenn die Methode in einer Sicht oder Inline-Tabellenwertfunktion enthalten ist. Wenn die SET-Optionen der Methode nicht ordnungsgemäß festgelegt sind, wird ein Fehler ausgegeben.|Niedrig|
|XML-Attributwerte, die Zeilenendezeichen enthalten (Wagenrücklauf und Zeilenvorschub) werden nicht gemäß dem XML-Standard normalisiert. Somit werden anstelle eines einzelnen Zeilenvorschubzeichens beide Zeichen zurückgegeben.|XML-Attributwerte, die Zeilenendezeichen enthalten (Wagenrücklauf und Zeilenvorschub) werden gemäß dem XML-Standard normalisiert. Somit werden alle Zeilenumbrüche in extern analysierten Entitäten (einschließlich der Dokumententität) bei Eingabe normalisiert, indem sowohl die aus zwei Zeichen bestehende Folge #xD #xA als auch alle Vorkommnisse von #xD, die nicht von #xA gefolgt werden, in das einzelne Zeichen #xA übersetzt werden.<br /><br /> Anwendungen, die mithilfe von Attributen Zeichenfolgenwerte transportieren, die Zeilenendezeichen enthalten, erhalten diese Zeichen nicht wie gesendet zurück. Um die Normalisierung zu verhindern, müssen für die Codierung aller Zeilenendezeichen numerische XML-Zeichenentitäten verwendet werden.|Niedrig|
|Die Spalteneigenschaften `ROWGUIDCOL` und `IDENTITY` können als Einschränkung falsch benannt werden. Beispielsweise wird die Anweisung `CREATE TABLE T (C1 int CONSTRAINT MyConstraint IDENTITY)` ausgeführt, doch wird der Einschränkungsname nicht beibehalten und ist für den Benutzer nicht verfügbar.|Die Spalteneigenschaften `ROWGUIDCOL` und `IDENTITY` können nicht als Einschränkung benannt werden. Der Fehler 156 wird zurückgegeben.|Niedrig|
|Die Aktualisierung von Spalten mithilfe einer bidirektionalen Zuweisung, wie z.B. `UPDATE T1 SET @v = column_name = <expression>`, kann zu unerwarteten Ergebnissen führen, weil während der Ausführung der Anweisung anstelle des Anfangswerts der aktuelle Wert der Variablen in anderen Klauseln verwendet werden kann, wie z.B. in der `WHERE`- und `ON`-Klausel. Dies kann dazu führen, dass sich die Bedeutungen der Prädikate für jede Zeile einzeln unvorhersehbar ändern.<br /><br /> Dieses Verhalten gilt nur, wenn der Kompatibilitätsgrad auf 90 festgelegt ist.|Die Aktualisierung von Spalten mithilfe einer zweiseitigen Zuweisung liefert die erwarteten Ergebnisse, weil während der Ausführung der Anweisung nur auf den Anweisungsanfangswert der Spalte zugegriffen wird.|Niedrig|
|Weitere Informationen finden Sie im Beispiel E weiter unten im Abschnitt „Beispiele“.|Siehe Beispiel F im nachfolgenden Abschnitt „Beispiele“.|Niedrig|
|Die ODBC-Funktion &lt;legacyBold&gt;{fn CONVERT ()}&lt;/legacyBold&gt; verwendet das Standarddatumsformat der Sprache. Bei einigen Sprachen lautet das Standardformat YDM. Dies kann zu Konvertierungsfehlern führen, wenn CONVERT() mit anderen Funktionen kombiniert wird, die ein YMD-Format erwarten, wie z.B. `{fn CURDATE()}`.|Die ODBC-Funktion `{fn CONVERT()}` verwendet für die Konvertierung in die ODBC-Datentypen SQL_TIMESTAMP, SQL_DATE, SQL_TIME, SQLDATE, SQL_TYPE_TIME und SQL_TYPE_TIMESTAMP das sprachenunabhängige XMD-Format im Format 121.|Niedrig|
|Für systeminterne datetime-Funktionen, wie z.&#160;B. DATEPART, ist es nicht erforderlich, dass Zeichenfolgeneingabewerte gültige datetime-Literale sind. `SELECT DATEPART (year, '2007/05-30')` wird beispielsweise erfolgreich kompiliert.|Für systeminterne datetime-Funktionen wie z.B. `DATEPART` ist es erforderlich, dass Zeichenfolgeneingabewerte gültige datetime-Literale sind. Fehler 241 wird zurückgegeben, wenn ein ungültiges datetime-Literal verwendet wird.|Niedrig|
|Nachstehende Leerzeichen, die im erstem Eingabeparameter der REPLACE-Funktion angegeben werden, werden gekürzt, wenn der Parameter den Typ „Char“ aufweist. In der folgenden Anweisung wird der Wert 'ABC ' beispielsweise fälschlicherweise als 'ABC' ausgewertet: SELECT '<' + REPLACE(CONVERT(char(6), 'ABC '), ' ', 'L') + '>'.|Nachstehende Leerzeichen werden immer beibehalten. Für Anwendungen, die sich auf das frühere Verhalten der Funktion stützen, verwenden Sie die RTRIM-Funktion bei der Angabe der ersten Eingabeparameter der Funktion. Die folgende Syntax reproduziert das Verhalten von SQL Server 2005: SELECT '<' + REPLACE(RTRIM(CONVERT(char(6), 'ABC ')), ' ', 'L') + '>'.|Niedrig|

## <a name="reserved-keywords"></a>Reservierte Schlüsselwörter

Die Kompatibilitätseinstellung bestimmt außerdem die für das [!INCLUDE[ssDE](../../includes/ssde-md.md)] reservierten Schlüsselwörter. In der folgenden Tabelle sind die reservierten Schlüsselwörter aufgeführt, die mit den einzelnen Kompatibilitätsgraden eingeführt werden.

|Kompatibilitätsgradeinstellung|Reservierte Schlüsselwörter|
|----------------------------------|-----------------------|
|130|Bestimmung erforderlich.|
|120|Keine.|
|110|`WITHIN GROUP`, `TRY_CONVERT`, `SEMANTICKEYPHRASETABLE`, `SEMANTICSIMILARITYDETAILSTABLE`, `SEMANTICSIMILARITYTABLE`|
|100|`CUBE`, `MERGE`, `ROLLUP`|
|90|`EXTERNAL`, `PIVOT`, `UNPIVOT`, `REVERT`, `TABLESAMPLE`|

Bei einem bestimmten Kompatibilitätsgrad schließen die reservierten Schlüsselwörter alle Schlüsselwörter ein, die mit diesem oder einem niedrigeren Grad eingeführt wurden. So stellen z. B. bei Anwendungen mit dem Kompatibilitätsgrad 110 alle in der vorherigen Tabelle aufgeführten Schlüsselwörter reservierte Schlüsselwörter dar. Bei den niedrigeren Kompatibilitätsgraden stellen die Schlüsselwörter von Grad 100 weiterhin gültige Objektnamen dar; die Sprachfunktionen von Grad 110, die diesen Schlüsselwörtern entsprechen, sind jedoch nicht verfügbar.

Nach der Einführung bleibt ein Schlüsselwort reserviert. So stellt z. B. das reservierte Schlüsselwort PIVOT, das mit dem Kompatibilitätsgrad 90 eingeführt wurde, auch bei Grad 100, 110 und 120 ein reserviertes Schlüsselwort dar.

Wenn eine Anwendung einen Bezeichner verwendet, der für den Kompatibilitätsgrad der Anwendung als Schlüsselwort reserviert ist, erzeugt die Anwendung einen Fehler. Sie können dies umgehen, indem Sie den Bezeichner in eckige Klammern ( **[]** ) oder Anführungszeichen( **""** ) einschließen. Wenn Sie z. B. eine Anwendung, die den Bezeichner `EXTERNAL` verwendet, auf den Kompatibilitätsgrad 90 aktualisieren möchten, könnten Sie den Bezeichner ändern, sodass er anschließend entweder `[EXTERNAL]` oder `"EXTERNAL"` lautet.

Weitere Informationen finden Sie unter [Reservierte Schlüsselwörter](../../t-sql/language-elements/reserved-keywords-transact-sql.md).

## <a name="permissions"></a>Berechtigungen

Erfordert die `ALTER`-Berechtigung für die Datenbank.

## <a name="examples"></a>Beispiele

### <a name="a-changing-the-compatibility-level"></a>A. Ändern des Kompatibilitätsgrads

Im folgenden Beispiel wird der Kompatibilitätsgrad der [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank auf 110 festgelegt (Standard für [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]).

```sql
ALTER DATABASE AdventureWorks2012
SET COMPATIBILITY_LEVEL = 110;
GO
```

Im folgenden Beispiel wird der Kompatibilitätsgrad der aktuellen Datenbank zurückgegeben.

```sql
SELECT name, compatibility_level
FROM sys.databases
WHERE name = db_name();
```

### <a name="b-ignoring-the-set-language-statement-except-under-compatibility-level-120"></a>B. Ignorieren der SET LANGUAGE-Anweisung, es sei denn, es ist ein Kompatibilitätsgrad unter 120 festgelegt

Die folgende Abfrage ignoriert die `SET LANGUAGE`-Anweisung, es sei denn, es ist ein Kompatibilitätsgrad unter 120 festgelegt.

```sql
SET DATEFORMAT dmy;
DECLARE @t2 date = '12/5/2011' ;
SET LANGUAGE dutch;
SELECT CONVERT(varchar(11), @t2, 106);

-- Results when the compatibility level is less than 120.
12 May 2011

-- Results when the compatibility level is set to 120).
12 mei 2011
```

### <a name="c-for-compatibility-level-setting-of-110-or-lower-recursive-references-on-the-right-hand-side-of-an-except-clause-create-an-infinite-loop"></a>C. Rekursive Verweise auf der rechten Seite einer EXCEPT-Klausel erzeugen bei der Kompatibilitätsgradeinstellung von 110 oder niedriger eine Endlosschleife.

```sql
WITH
cte AS (SELECT * FROM (VALUES (1),(2),(3)) v (a)),
r
AS (SELECT a FROM Table1
UNION ALL
(SELECT a FROM Table1 EXCEPT SELECT a FROM r) )
SELECT a
FROM r;

```

### <a name="d-the-difference-between-styles-0-and-121"></a>D: Der Unterschied zwischen den Formaten 0 und 121

Weitere Informationen zu Datums- und Zeitformaten finden Sie unter [CAST und CONVERT](../../t-sql/functions/cast-and-convert-transact-sql.md).

```sql
CREATE TABLE t1 (c1 time(7), c2 datetime2);

INSERT t1 (c1,c2) VALUES (GETDATE(), GETDATE());

SELECT CONVERT(nvarchar(16),c1,0) AS TimeStyle0
       ,CONVERT(nvarchar(16),c1,121)AS TimeStyle121
       ,CONVERT(nvarchar(32),c2,0) AS Datetime2Style0
       ,CONVERT(nvarchar(32),c2,121)AS Datetime2Style121
FROM t1;

-- Returns values such as the following.
TimeStyle0       TimeStyle121
Datetime2Style0      Datetime2Style121
---------------- ----------------
-------------------- --------------------------
3:15PM           15:15:35.8100000
Jun  7 2011  3:15PM  2011-06-07 15:15:35.8130000
```

### <a name="e-variable-assignment---top-level-union-operator"></a>E. Variablenzuweisung – UNION-Operator auf der obersten Ebene
Die Variablenzuweisung ist in einer Anweisung mit einem UNION-Operator auf der obersten Ebene zulässig, kann jedoch unerwartete Ergebnisse zurückgeben. Beispielsweise wird in den folgenden Anweisungen der lokalen Variable `@v` der Wert der Spalte `BusinessEntityID` aus der Vereinigung von zwei Tabellen zugewiesen. Wenn die SELECT-Anweisung mehr als einen Wert zurückgibt, wird der Variablen definitionsgemäß der zuletzt zurückgegebene Wert zugewiesen. In diesem Fall wird der Variablen der letzte Wert richtig zugewiesen, es wird jedoch auch das Resultset der SELECT UNION-Anweisung zurückgegeben.

```sql
ALTER DATABASE AdventureWorks2012
SET compatibility_level = 110;
GO
USE AdventureWorks2012;
GO
DECLARE @v int;
SELECT @v = BusinessEntityID FROM HumanResources.Employee
UNION ALL
SELECT @v = BusinessEntityID FROM HumanResources.EmployeeAddress;
SELECT @v;
```

Die Variablenzuweisung ist in einer Anweisung mit einem UNION-Operator auf der obersten Ebene nicht zulässig. Der Fehler 10734 wird zurückgegeben. Um den Fehler zu beheben, müssen Sie die Abfrage umschreiben, wie im folgenden Beispiel gezeigt.

```sql
DECLARE @v int;
SELECT @v = BusinessEntityID FROM
    (SELECT BusinessEntityID FROM HumanResources.Employee
     UNION ALL
     SELECT BusinessEntityID FROM HumanResources.EmployeeAddress) AS Test;
SELECT @v;
```

## <a name="see-also"></a>Weitere Informationen 
[Kompatibilitätszertifizierung](../../database-engine/install-windows/compatibility-certification.md)       
[ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md)       
[Reservierte Schlüsselwörter](../../t-sql/language-elements/reserved-keywords-transact-sql.md)       
[CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)       
[DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)       
[sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)       
[sys.database_files](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)       
[Anzeigen oder Ändern des Kompatibilitätsgrads einer Datenbank](../../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md)       
[Ändern des Datenbank-Kompatibilitätsmodus und Verwenden des Abfragespeichers](../../database-engine/install-windows/change-the-database-compatibility-mode-and-use-the-query-store.md)       
[Upgraden von Datenbanken mit dem Abfrageoptimierungs-Assistenten](../../relational-databases/performance/upgrade-dbcompat-using-qta.md)