---
title: Hardware für SQL Server und In-Memory-OLTP | Microsoft-Dokumentation
description: Erfahren Sie mehr über Hardwareaspekte für die In-Memory-OLTP-Leistung in SQL Server. In-Memory-OLTP verwendet Arbeitsspeicher und Datenträger anders als datenträgerbasierte Tabellen.
ms.custom: ''
ms.date: 03/28/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||=azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: ba34a1b550d9726c3de479c56f92e40adf404214
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063706"
---
# <a name="hardware-considerations-for-in-memory-oltp-in-sql-server"></a>Überlegungen zur Hardware für In-Memory-OLTP in SQL Server

In-Memory-OLTP verwendet Arbeitsspeicher und Datenträger anders als herkömmliche datenträgerbasierte Tabellen. Die Leistungsverbesserung, die Sie mit In-Memory-OLTP erzielen können, hängt von der verwendeten Hardware ab. In diesem Blogbeitrag erörtern wir eine Reihe allgemeiner Hardware-Aspekte und bieten allgemeingültige Richtlinien für Hardware zur Verwendung mit In-Memory-OLTP.

> [!NOTE]
> Dieser Artikel war ein Blog, der am 1. August 2013 vom Microsoft SQL Server 2014-Team veröffentlicht wurde. Die Webseite des Blogs wird außer Betrieb gesetzt.
>
> [SQL Server In-Memory-OLTP](./in-memory-oltp-in-memory-optimization.md)

<!--
    Here was the link to the blog. This blog was captured into this new article on 2018/11/30, by GeneMi (MightyPen).
    https://cloudblogs.microsoft.com/sqlserver/2013/08/01/hardware-considerations-for-in-memory-oltp-in-sql-server-2014/
    At least one pre-existing article that contained the obsolete blog link was:
        relational-databases\in-memory-oltp\sample-database-for-in-memory-oltp.md
-->

## <a name="cpu"></a>CPU

In-Memory-OLTP erfordert keinen High-End-Server, um eine OLTP-Workload mit hohem Durchsatz zu unterstützen. Wir empfehlen die Verwendung eines Mittelklasseservers mit 2 CPU-Sockets. Aufgrund des erhöhten Durchsatzes, der durch In-Memory-OLTP ermöglicht wird, sind zwei Sockets für Ihre Geschäftsanforderungen wahrscheinlich ausreichend.

Es wird empfohlen, Hyperthreading mit In-Memory-OLTP zu aktivieren. Bei einigen OLTP-Workloads konnten wir bei Verwendung von Hyperthreading Leistungssteigerungen von bis zu 40 % verzeichnen.

## <a name="memory"></a>Arbeitsspeicher

Alle speicheroptimierten Tabellen befinden sich vollständig im Arbeitsspeicher. Daher müssen Sie über genügend physischen Arbeitsspeicher für die Tabellen selbst verfügen, um die in der Datenbank ausgeführte Workload zu unterstützen. Wie viel Arbeitsspeicher Sie tatsächlich benötigen, hängt ganz von der Workload ab. Als Ausgangspunkt benötigen Sie jedoch wahrscheinlich genügend verfügbaren Arbeitsspeicher für etwa die doppelte Datengröße. Sie benötigen auch genügend Arbeitsspeicher für den Pufferpool, falls die Workload auch mit herkömmlichen datenträgerbasierten Tabellen arbeitet.

Um festzustellen, wie viel Arbeitsspeicher eine bestimmte speicheroptimierte Tabelle belegt, führen Sie die folgende Abfrage aus:

```sql
select object_name(object_id), * from sys.dm_db_xtp_table_memory_stats;
```

Die Ergebnisse zeigen den für speicheroptimierte Tabellen und deren Indizes belegten Arbeitsspeicher. Zu den Tabellendaten gehören die Benutzerdaten sowie alle älteren Zeilenversionen, die für laufende Transaktionen noch benötigt werden oder vom System noch nicht bereinigt wurden. Der von Hashindizes belegte Arbeitsspeicher ist konstant und hängt nicht von der Anzahl der Zeilen in der Tabelle ab.

Es ist wichtig zu beachten, dass bei Verwendung von In-Memory-OLTP die gesamte Datenbank nicht in den Arbeitsspeicher passen muss. Sie können über eine mehrere Terabyte große Datenbank verfügen und trotzdem von In-Memory-OLTP profitieren, solange die Größe Ihrer häufig abgerufenen Daten (d.h. der speicheroptimierten Tabellen) 256 GB nicht überschreitet. Die maximale Anzahl der Prüfpunktdatendateien, die SQL Server für eine einzelne Datenbank verwalten kann, ist 4.000, wobei jede Datei 128 MB groß ist. Obwohl dies einen theoretischen Maximalwert von 512 GB ergeben würde, unterstützen wir bis zu 256 GB, um zu gewährleisten, dass SQL Server mit dem Zusammenführen von Prüfpunktdateien Schritt halten kann und nicht das Limit von 4.000 Dateien erreicht. Beachten Sie, dass dieser Grenzwert nur für die speicheroptimierten Tabellen gilt. Für die herkömmlichen datenträgerbasierten Tabellen in derselben SQL Server-Datenbank gilt keine solche Größenbeschränkung.

Nicht dauerhafte speicheroptimierte Tabellen mit der Einstellung DURABILITY=SCHEMA_ONLY werden nicht auf dem Datenträger beibehalten. Obwohl dauerhafte speicheroptimierte Tabellen nicht durch die Anzahl der Prüfpunktdateien beschränkt sind, werden nur 256 GB unterstützt. Die Überlegungen zu Protokoll- und Datenlaufwerken im weiteren Verlauf dieses Beitrags gelten für nicht dauerhafte Tabellen nicht, da die Daten nur im Arbeitsspeicher vorhanden sind.

## <a name="log-drive"></a>Protokolllaufwerk

Protokolldatensätze zu speicheroptimierten Tabellen werden zusammen mit den anderen SQL Server-Protokolldatensätzen in das Transaktionsprotokoll der Datenbank geschrieben.

Es ist immer wichtig, die Protokolldatei auf einem Laufwerk mit niedriger Wartezeit abzulegen, damit Transaktionen nicht zu lange warten müssen und Konflikte bei der Protokoll-E/A vermieden werden. Ihr System wird so schnell wie Ihre langsamste Komponente ausgeführt (Amdahlsches Gesetz). Sie müssen sicherstellen, dass Ihr Gerät für die Protokoll-E/A beim Ausführen von In-Memory-OLTP kein Engpass wird. Wir empfehlen die Verwendung eines Speichergeräts mit niedriger Wartezeit, mindestens SSD.

Beachten Sie, dass speicheroptimierte Tabellen weniger Protokollbandbreite benötigen als datenträgerbasierte Tabellen, da sie keine Indexvorgänge und UNDO-Datensätze protokollieren. Dies kann helfen, Konflikte bei der Protokoll-E/A zu reduzieren.

## <a name="data-drive"></a>Datenlaufwerk

Zur Persistenz von speicheroptimierten Tabellen mit Prüfpunktdateien wird Streaming-E/A verwendet. Daher benötigen diese Dateien kein Laufwerk mit niedriger Wartezeit oder schneller zufälliger E/A. Stattdessen ist der Hauptfaktor für diese Laufwerke die Geschwindigkeit der sequentiellen E/A und die Bandbreite des Hostbusadapters (HBA). Somit benötigen Sie für Prüfpunktdateien keine SSD-Laufwerke. Sie können sie auf Hochleistungsspindeln (z.B. SAS) platzieren, sofern ihre sequentielle E/A-Geschwindigkeit Ihren Anforderungen entspricht.

Der größte Faktor beim Bestimmen der Geschwindigkeitsanforderung ist Ihr RTO (Recovery Time Objective) beim Serverneustart. Während der Wiederherstellung der Datenbank müssen alle Daten in den speicheroptimierten Tabellen vom Datenträger in den Arbeitsspeicher gelesen werden. Die Wiederherstellung der Datenbank erfolgt mit der sequenziellen Lesegeschwindigkeit Ihres E/A-Subsystems. Der Datenträger ist der Engpass.

Um strenge RTO-Anforderungen zu erfüllen, empfehlen wir, die Prüfpunktdateien auf mehrere Datenträger zu verteilen, indem Sie mehrere Container zur Dateigruppe MEMORY_OPTIMIZED_DATA hinzufügen. SQL Server unterstützt das parallele Laden von Prüfpunktdateien von mehreren Laufwerken. Die Wiederherstellung erfolgt mit der aggregierten Geschwindigkeit der Laufwerke.

Hinsichtlich der Datenträgerkapazität empfehlen wir die zwei- bis dreifache Größe der speicheroptimierten Tabellen zur Verfügung zu haben.

## <a name="see-also"></a>Weitere Informationen

[Beispieldatenbank für In-Memory OLTP](sample-database-for-in-memory-oltp.md)