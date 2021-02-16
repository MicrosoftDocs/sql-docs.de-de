---
description: In-Memory-Datenbanksysteme und -Technologien
title: Features und Technologien für In-Memory-Datenbanksysteme
ms.date: 10/30/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- in-memory systems
- in-memory technologies
- in-memory features
- database, in-memory database
- system, in-memory system
- features, in-memory features
- in-memory
ms.assetid: 11f8017e-5bc3-4bab-8060-c16282cfbac1
author: briancarrig
ms.author: brcarrig
ms.openlocfilehash: 96f175f4eb6f913c7870f573e65201e1124dffad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351280"
---
# <a name="in-memory-database-systems-and-technologies"></a>In-Memory-Datenbanksysteme und -Technologien

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

Diese Seite ist als Referenzseite für In-Memory-Features und -Technologien in SQL Server gedacht. Ein In-Memory-Datenbanksystem ist ein Datenbanksystem, das so konzipiert ist, dass es die in modernen Datenbanksystemen verfügbaren größeren Arbeitsspeicherkapazitäten nutzt. In-Memory-Datenbanken können relational oder nicht relational sein.

Häufig wird angenommen, dass die bessere Leistung von In-Memory-Datenbanksystemen hauptsächlich darauf zurückzuführen ist, dass auf Daten im Arbeitsspeicher (um ein Vielfaches) schneller zugegriffen werden kann als auf Daten auf dem schnellsten Datenträgersubsystem. Allerdings passt bei vielen SQL Server-Arbeitsauslastungen der gesamte Arbeitssatz in den verfügbaren Arbeitsspeicher. Viele In-Memory-Datenbanksysteme können Daten persistent auf Datenträgern speichern, und möglicherweise passt nicht immer das gesamte Dataset in den verfügbaren Arbeitsspeicher.

Bei Arbeitsauslastungen von relationalen Datenbanken ist ein schneller flüchtiger Cache vor deutlich langsameren, aber langlebigen Medien üblich. Hierfür sind bestimmte Ansätze bei der Arbeitsauslastungsverwaltung erforderlich. Die Möglichkeiten, die sich durch schnellere Übertragungsraten von Arbeitsspeichern, größere Kapazität oder sogar persistenten Speicher ergeben, erleichtern die Entwicklung von neuen Features und Technologien, die neue Ansätze für die Arbeitsauslastungsverwaltung von relationalen Datenbanken vorantreiben können.

## <a name="hybrid-buffer-pool"></a>Hybrider Pufferpool

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

Bei einem [hybriden Pufferpool](../database-engine/configure-windows/hybrid-buffer-pool.md) wird der Pufferpool für Datenbankdateien, die auf byteadressierbaren Speichergeräten mit persistentem Speicher liegen, für Windows- und Linuxplattformen mit [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] erweitert.

## <a name="memory-optimized-tempdb-metadata"></a>Speicheroptimierte `tempdb`-Metadaten

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

Mit [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] wird eine neue Funktion für [speicheroptimierte tempdb-Metadaten](./databases/tempdb-database.md#memory-optimized-tempdb-metadata) eingeführt, durch die einige Engpässe aufgrund von Konflikten effektiv behoben werden und sich eine neue Ebene der Skalierbarkeit für tempdb-intensive Workloads ergibt.

## <a name="in-memory-oltp"></a>In-Memory-OLTP

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

[In-Memory-OLTP](./in-memory-oltp/in-memory-oltp-in-memory-optimization.md) ist eine in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] und [!INCLUDE[ssSDS](../includes/sssds-md.md)] verfügbare Datenbanktechnologie für die Optimierung der Transaktionsverarbeitung, des Erfassens und Ladens von Daten sowie vorübergehender Datenszenarios.

## <a name="configuring-persistent-memory-support-for-linux"></a>Unterstützung für persistenten Speicher konfigurieren unter Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

[!INCLUDE[sqlv15](../includes/sssql19-md.md)] beschreibt, wie Sie persistenten Speicher unter Verwendung des [persistenten Speichers](../linux/sql-server-linux-configure-pmem.md) des Hilfsprogramms `ndctl` konfigurieren.

## <a name="persisted-log-buffer"></a>Persistenter Protokollpuffer

Mit dem Service Pack 1 von [!INCLUDE[ssSQL16](../includes/sssql16-md.md)] wurde die Leistung bei schreibintensiven Arbeitsauslastungen, die mit WRITELOG-Wartevorgängen verbunden waren, optimiert. Der Protokollpuffer wird im persistenten Speicher gespeichert. Dieser kleine Puffer (20 MB pro Benutzerdatenbank) muss auf einen Datenträger geleert werden, damit die im Transaktionsprotokoll erfassten Transaktionen festgeschrieben werden. Bei schreibintensiven OLTP-Arbeitsauslastungen kann dieser Leerungsmechanismus zu einem Engpass werden. Mit dem Protokollpuffer im persistenten Speicher wird die Anzahl von für das Festschreiben des Protokolls erforderlichen Vorgängen verringert, wodurch die Gesamtdauer von Transaktionen optimiert und die Leistung der Arbeitsauslastungen verbessert wird. Dieser Prozess wurde ursprünglich als [Tail of Log Caching]( https://blogs.msdn.microsoft.com/bobsql/2016/11/08/how-it-works-it-just-runs-faster-non-volatile-memory-sql-server-tail-of-log-caching-on-nvdimm/) (Protokollfragmentzwischenspeicherung) eingeführt. Dies wurde allerdings als Widerspruch zur [Protokollfragmentsicherung](./backup-restore/tail-log-backups-sql-server.md) sowie zu der traditionellen Auffassung, dass ein Protokollfragment der Teil eines Transaktionsprotokolls ist, der festgeschrieben, aber noch nicht gesichert wurde, wahrgenommen. Da die offizielle Bezeichnung für das Feature „Persisted Log Buffer“ (persistenter Protokollpuffer) ist, wird diese Bezeichnung hier verwendet.

Weitere Informationen finden Sie unter [Hinzufügen eines persistenten Protokollpuffers zu einer Datenbank](./databases/add-persisted-log-buffer.md).
