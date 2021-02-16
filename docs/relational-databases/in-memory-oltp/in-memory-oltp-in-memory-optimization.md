---
title: In-Memory OLTP (Arbeitsspeicheroptimierung) | Microsoft-Dokumentation
description: Verwenden Sie diese Beispiele und Ressourcen für In-Memory OLTP, das die Leistung in SQL Server deutlich verbessern kann.
ms.custom: ''
ms.date: 11/21/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
helpviewer_keywords:
- In-Memory OLTP
- memory-optimized tables
ms.assetid: e1d03d74-2572-4a55-afd6-7edf0bc28bdb
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8d1dfd8b3d09adb4c3fdb0ad0d2bd02b1044f69a
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2021
ms.locfileid: "100351255"
---
# <a name="in-memory-oltp-and-memory-optimization"></a>In-Memory-OLTP und Arbeitsspeicheroptimierung

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

 [!INCLUDE[hek_2](../../includes/hek-2-md.md)] kann die Leistung beim Verarbeiten von Transaktionen, Erfassen und Laden von Daten sowie in vorübergehenden Datenszenarien erheblich verbessern.  Informationen zum grundlegenden Code und Wissen, den bzw. das Sie für einen schnellen Test Ihrer eigenen speicheroptimierten Tabelle und nativ kompilierten gespeicherten Prozedur benötigen, finden Sie unter
 -  [Schnellstart 1: In-Memory-OLTP-Technologien für höhere Transact-SQL-Leistung](../../relational-databases/in-memory-oltp/survey-of-initial-areas-in-in-memory-oltp.md).  
 
Wir haben ein [**17-minütiges Video**](#anchorname-17minute-video) auf YouTube hochgeladen, das In-Memory-OLTP für SQL Server erläutert und die Leistungsvorteile aufzeigt.

Eine detailliertere Übersicht über In-Memory-OLTP und die Szenarien, in denen die Technologie Leistungsvorteile bringt, finden Sie unter:

- [Übersicht und Verwendungsszenarien](../../relational-databases/in-memory-oltp/overview-and-usage-scenarios.md)
 
 Beachten Sie, dass [!INCLUDE[hek_2](../../includes/hek-2-md.md)] die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Technologie zur Verbesserung der Leistung der Transaktionsverarbeitung ist. Zur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Technologie, die die Leistung von Berichts- und Analyseabfragen verbessert, siehe [Beschreibung von Columnstore-Indizes](../../relational-databases/indexes/columnstore-indexes-overview.md).
  
 Mehrere Verbesserungen wurden an In-Memory-OLTP sowohl in [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] als auch in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] vorgenommen. Die Transact-SQL-Oberfläche wurde verbessert, um das Migrieren von Datenbanken zu vereinfachen. Unterstützung für die Ausführung von ALTER-Vorgängen für speicheroptimierte Tabellen und systemintern kompilierte gespeicherte Prozeduren wurde hinzugefügt, um Anwendungen einfacher zu verwalten.
  
> [!NOTE]  
>  **Ausprobieren**  
>   
>  In-Memory OLTP ist in den SQL-Datenbank- und elastischen Pool-Tarifen Premium und Unternehmenskritisch verfügbar. Zum Einstieg in In-Memory-OLTP sowie Columnstore in Azure SQL-Datenbank finden Sie Informationen unter [Optimieren der Leistung mithilfe von In-Memory-Technologien in SQL-Datenbank](/azure/azure-sql/in-memory-oltp-overview).  
  

## <a name="in-this-section"></a>In diesem Abschnitt  
 Dieser Abschnitt enthält die folgenden Themen:  
  
|Thema|BESCHREIBUNG|  
|-----------|-----------------|  
|[Schnellstart 1: In-Memory-OLTP-Technologien für höhere Transact-SQL-Leistung](../../relational-databases/in-memory-oltp/survey-of-initial-areas-in-in-memory-oltp.md)|Tauchen Sie direkt in In-Memory-OLTP ein.|
|[Übersicht und Verwendungsszenarien](../../relational-databases/in-memory-oltp/overview-and-usage-scenarios.md)|Übersicht über die Aufgaben von In-Memory-OLTP und die Szenarien, die hinsichtlich Leistung davon profitieren.|
|[Anforderungen für die Verwendung speicheroptimierter Tabellen](../../relational-databases/in-memory-oltp/requirements-for-using-memory-optimized-tables.md)|Erläutert Hardware- und Softwareanforderungen und Richtlinien zum Verwenden von speicheroptimierten Tabellen.|  
|[Codebeispiele für In-Memory OLTP](./sample-database-for-in-memory-oltp.md)|Enthält Codebeispiele, die das Erstellen und Verwenden einer speicheroptimierten Tabelle veranschaulichen.|  
|[Speicheroptimierte Tabellen](./sample-database-for-in-memory-oltp.md)|Bietet eine Einführung in speicheroptimierte Tabellen.|  
|[Speicheroptimierte Tabellenvariablen](./faster-temp-table-and-table-variable-by-using-memory-optimization.md)|Ein Codebeispiel, das veranschaulicht, wie eine speicheroptimierte Tabellenvariable anstelle einer herkömmlichen Tabellenvariable verwendet wird, um die Verwendung von tempdb zu reduzieren.|  
|[Indizes für speicheroptimierte Tabellen](./indexes-for-memory-optimized-tables.md)|Bietet eine Einführung in speicheroptimierte Indizes.|  
|[Nativ kompilierte gespeicherte Prozeduren](./a-guide-to-query-processing-for-memory-optimized-tables.md)|Führt systemintern kompilierte gespeicherte Prozeduren ein.|  
|[Verwalten des Arbeitsspeichers für In-Memory-OLTP](/previous-versions/sql/sql-server-2016/dn465872(v=sql.130))|Erläutert die Funktionsweise und Verwaltung der Speicherverwendung im System.|  
|[Erstellen und Verwalten von Speicher für speicheroptimierte Objekte](../../relational-databases/in-memory-oltp/creating-and-managing-storage-for-memory-optimized-objects.md)|Erläutert Daten- und Änderungsdateien, die Informationen zu Transaktionen in speicheroptimierten Tabellen speichern.|  
|[Sichern und Wiederherstellen speicheroptimierter Tabellen](/previous-versions/sql/sql-server-2016/dn624160(v=sql.130))|Erläutert die Sicherung und Wiederherstellung von speicheroptimierten Tabellen.|  
|[Transact-SQL-Unterstützung für OLTP im Arbeitsspeicher](../../relational-databases/in-memory-oltp/transact-sql-support-for-in-memory-oltp.md)|Erläutert die [!INCLUDE[tsql](../../includes/tsql-md.md)] -Unterstützung für [!INCLUDE[hek_2](../../includes/hek-2-md.md)].|  
|[Unterstützung für Hochverfügbarkeit für In-Memory OLTP-Datenbanken](../../relational-databases/in-memory-oltp/high-availability-support-for-in-memory-oltp-databases.md)|Erläutert Verfügbarkeitsgruppen und Failoverclustering in [!INCLUDE[hek_2](../../includes/hek-2-md.md)].|  
|[SQL Server-Unterstützung für In-Memory OLTP](./transact-sql-support-for-in-memory-oltp.md)|Listet neue und aktualisierte Syntax und Funktionen auf, die speicheroptimierte Tabellen unterstützen.|  
|[Migrieren zu In-Memory OLTP](./plan-your-adoption-of-in-memory-oltp-features-in-sql-server.md)|Erläutert, wie datenträgerbasierte Tabellen zu speicheroptimierten Tabellen migriert werden.|  
| &nbsp; | &nbsp; |

## <a name="links-to-other-websites"></a>Links zu anderen Websites

Dieser Abschnitt enthält Links zu anderen Websites, die Informationen zu In-Memory-OLTP für SQL Server enthalten.

- [**Video**, in dem In-Memory-OLTP erläutert wird und Leistungsvorteile veranschaulicht werden](#anchorname-17minute-video)

- [In-Memory OLTP Performance Demo v1.0](https://github.com/Microsoft/sql-server-samples/releases/tag/in-memory-oltp-demo-v1.0)

-   [SQL Server In-Memory-OLTP: Internes technisches Whitepaper](./sql-server-in-memory-oltp-internals-for-sql-server-2016.md)  

-   [Vergleich zwischen SQL Server In-Memory-OLTP und Columnstore-Funktion](https://download.microsoft.com/download/D/0/0/D0075580-6D72-403D-8B4D-C3BD88D58CE4/SQL_Server_2016_In_Memory_OLTP_and_Columnstore_Comparison_White_Paper.pdf)

-   Neuigkeiten bei In-Memory-OLTP in SQL Server 2016 – [Teil 1](/archive/blogs/sqlserverstorageengine/in-memory-oltp-whats-new-in-sql2016-ctp3) und [Teil 2](/archive/blogs/sqlserverstorageengine/whats-new-for-in-memory-oltp-in-sql-server-2016-since-ctp3)
  
-   [In-Memory OLTP - Common Workload Patterns and Migration Considerations (In-Memory-OLTP: Allgemeine Workloadmuster und Überlegungen zur Migration)](/previous-versions/dn673538(v=msdn.10))  
  
-   [In-Memory-OLTP-Blog](https://cloudblogs.microsoft.com/sqlserver/2013/06/26/sql-server-2014-in-memory-technologies-blog-series-introduction/)  

## <a name="17-minute-video-indexed"></a><a name="anchorname-17minute-video"></a>17-minütiges Video, indiziert

- _Videotitel:_ &nbsp; **In-Memory-OLTP in SQL Server 2016**
- _Veröffentlichungsdatum:_ &nbsp; 10.03.2019, auf `YouTube.com`.
- _Dauer:_ &nbsp; 17:32 &nbsp; &nbsp; (Links zum Video finden Sie im folgenden [**Index**](#anchorname-index-17minute-video).)
- _Gehostet von:_ &nbsp; Jos de Bruijn, Senior Program Manager für SQL Server

### <a name="demo-can-be-downloaded"></a>Die Demo kann heruntergeladen werden.

Bei Zeitmarke 08:09 führt das Video eine Demonstration zwei Mal aus. Sie können den Quellcode für die ausführbare Leistungsdemo, die im Video verwendet wird, über den folgenden Link herunterladen:

- [In-Memory OLTP Performance Demo v1.0, Quellcode](https://github.com/Microsoft/sql-server-samples/releases/tag/in-memory-oltp-demo-v1.0)

Die folgenden allgemeinen Schritte sind im Video zu sehen:

1. Zunächst wird die Demo mit einer regulären Tabelle ausgeführt.
2. Als nächstes sehen wir eine speicheroptimierte Version der Tabelle, die mit wenigen Mausklicks erstellt und in SQL Server Management Studio („SSMS. exe“) mit Daten aufgefüllt wird.
3. Anschließend wird die Demo mit der speicheroptimierten Tabelle erneut ausgeführt. Es wird eine enorme Geschwindigkeitsverbesserung gemessen.

### <a name="index-to-each-section-in-the-video"></a><a name="anchorname-index-17minute-video"></a>Index für jeden Abschnitt im Video

| Link zu Zeitmarken | Titel des Abschnitts |
| :------------- | :------------ |
| A.&nbsp; [00:00](https://www.youtube.com/watch?v=l5l5eophmK4&t=0) | Die Einführung. |
| <br/>B.&nbsp; [00:56](https://www.youtube.com/watch?v=l5l5eophmK4&t=56) | <br/>Warum sich Kunden mit In-Memory-OLTP auseinandersetzen sollten. |
| &nbsp; &nbsp; [01:03](https://www.youtube.com/watch?v=l5l5eophmK4&t=63) | Moderne Hardware erfordert eine moderne Architektur des Datenbanksystems. |
| &nbsp; &nbsp; [02:10](https://www.youtube.com/watch?v=l5l5eophmK4&t=130) | Explosion der generierten Daten. Vorgänge müssen sofort (geringe Latenzzeit) ausgeführt werden. |
| &nbsp; &nbsp; [03:19](https://www.youtube.com/watch?v=l5l5eophmK4&t=199) | Verringerung der Gesamtkosten: Nutzen Sie die vorhandenen Ressourcen effizienter. |
| <br/>C.&nbsp; [03:33](https://www.youtube.com/watch?v=l5l5eophmK4&t=213) | <br/>Vorstellung von In-Memory-OLTP.<br/>Leistungsoptimierung durch Verwenden von speicheroptimierten Technologien. |
| &nbsp; &nbsp; [05:03](https://www.youtube.com/watch?v=l5l5eophmK4&t=303) | Bis zu 30-fach schnellere Transaktionsverarbeitung. |
| &nbsp; &nbsp; [05:22](https://www.youtube.com/watch?v=l5l5eophmK4&t=322) | Vollständig dauerhaft: Daten überstehen Serverfehler. |
| &nbsp; &nbsp; [06:15](https://www.youtube.com/watch?v=l5l5eophmK4&t=375) | Vollständig in SQL Server integriert. Daher sind keine neuen Sprachen oder Tools zu erlernen. |
| &nbsp; &nbsp; [07:22](https://www.youtube.com/watch?v=l5l5eophmK4&t=442) | Zuerst in SQL Server 2014 veröffentlicht, wichtige Verbesserungen in 2016. |
| &nbsp; &nbsp; [07:58](https://www.youtube.com/watch?v=l5l5eophmK4&t=558) | Aich in Azure SQL-Datenbank (in der Cloud) verfügbar. |
| <br/>D.&nbsp; [08:09](https://www.youtube.com/watch?v=l5l5eophmK4&t=489) | <br/>Leistungsdemonstration.<br/> Ausführen der Demo mit einer regulären Tabelle. |
| &nbsp; &nbsp; [09:11](https://www.youtube.com/watch?v=l5l5eophmK4&t=551) | SSMS-Kontextmenü: **Berichte** &gt; **Transaktionsleistungsanalyse** |
| &nbsp; &nbsp; [10:38](https://www.youtube.com/watch?v=l5l5eophmK4&t=638) | SSMS-Kontextmenü: **Advisor für die Speicheroptimierung**<br/> &nbsp; &nbsp; Tatsächliches Erstellen einer speicheroptimierten Tabelle aus einer regulären Tabelle und Migrieren der Daten. |
| &nbsp; &nbsp; [11:28](https://www.youtube.com/watch?v=l5l5eophmK4&t=688) | Führen Sie die Demo erneut aus. Sie erkennen eine 45-fache Leistungsverbesserung. |
| <br/>E.&nbsp; [12:17](https://www.youtube.com/watch?v=l5l5eophmK4&t=737) | <br/>Einfachere Verwendung von In-Memory-OLTP in SQL Server 2016 (im Vergleich zu 2014). |
| &nbsp; &nbsp; [12:43](https://www.youtube.com/watch?v=l5l5eophmK4&t=763) | Vereinfachte Analyse zur Unterstützung der App-Migration. |
| &nbsp; &nbsp; [13:03](https://www.youtube.com/watch?v=l5l5eophmK4&t=783) | Geringere Komplexität der App-Migration durch erweiterte Transact-SQL-Sprachunterstützung (z.B. mit Fremdschlüsseln und Triggern). |
| &nbsp; &nbsp; [13:56](https://www.youtube.com/watch?v=l5l5eophmK4&t=836) | Verbesserte Verwaltbarkeit.<br/> &nbsp; &nbsp; Beispiele: Ändern von Schema und Indizes, automatische Aktualisierung von Statistiken. |
| <br/>F.&nbsp; [14:46](https://www.youtube.com/watch?v=l5l5eophmK4&t=886) | <br/>Verbesserte Skalierbarkeit. |
| &nbsp; &nbsp; [15:12](https://www.youtube.com/watch?v=l5l5eophmK4&t=912) | Große speicheroptimierte Tabellen (bis zu 2 TB pro Datenbank). |
| &nbsp; &nbsp; [15:34](https://www.youtube.com/watch?v=l5l5eophmK4&t=934) | Noch bessere Skalierung. |
| &nbsp; &nbsp; [16:41](https://www.youtube.com/watch?v=l5l5eophmK4&t=1001) | Nutzen Sie die vorhandenen Ressourcen effizienter! |
| <br/>G.&nbsp; [16:53](https://www.youtube.com/watch?v=l5l5eophmK4&t=1013) | <br/>Abschließende Kommentare. (Endet bei 17:32.) |
| &nbsp; | &nbsp; |

## <a name="see-also"></a>Weitere Informationen  
 [Datenbankfunktionen](../databases/databases.md)  
