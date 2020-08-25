---
description: Überlegungen und Einschränkungen zu temporalen Tabellen
title: Überlegungen und Einschränkungen zu temporalen Tabellen | Microsoft-Dokumentation
ms.custom: ''
ms.date: 09/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: c8a21481-0f0e-41e3-a1ad-49a84091b422
author: CarlRabeler
ms.author: carlrab
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b3d276d144730bfeb39b0cde13720da61de72b67
ms.sourcegitcommit: 331b8495e4ab37266945c81ff5b93d250bdaa6da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2020
ms.locfileid: "88645827"
---
# <a name="temporal-table-considerations-and-limitations"></a>Überlegungen und Einschränkungen zu temporalen Tabellen


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]


Aufgrund der Eigenschaften der Systemversionsverwaltung gibt es einige Überlegungen und Einschränkungen, derer Sie sich bei der Arbeit mit temporalen Tabellen bewusst sein müssen.

Beachten Sie Folgendes, wenn Sie mit temporalen Tabellen arbeiten:

- Für eine temporale Tabelle muss ein Primärschlüssel definiert sein, um Datensätze zwischen der aktuellen Tabelle und den Verlaufstabellen zu korrelieren. Für die Verlaufstabelle kann kein Primärschlüssel definiert sein.
- Die SYSTEM_TIME-Zeitraumspalten, die verwendet werden, um die Werte **SysStartTime** und **SysEndTime** aufzuzeichnen, müssen mit dem Datentyp „datetime2“ definiert sein.
- Falls der Name einer Verlaufstabelle während der Erstellung der Verlaufstabelle angegeben wird, müssen Sie das Schema und den Tabellennamen angeben.
- Standardmäßig ist die Verlaufstabelle **PAGE** -komprimiert.
- Falls die aktuelle Tabelle partitioniert wurde, kann die Verlaufstabelle auf der Standarddateigruppe erstellt werden, da die Partitionierungskonfiguration nicht automatisch von der aktuellen auf die Verlaufstabelle repliziert wird.
- Temporale Tabellen und Verlaufstabellen können nicht **FILETABLE** sein und können Spalten aller unterstützten Datentypen außer **FILESTREAM** enthalten, da **FILETABLE** und **FILESTREAM** die Datenbearbeitung außerhalb von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ermöglichen. Daher kann die Systemversionsverwaltung nicht garantiert werden.
- Eine Knoten- oder Edgetabelle kann nicht als temporale Tabelle erstellt oder in eine solche geändert werden.
- Temporale Tabellen unterstützen zwar Blobdatentypen wie **(n)varchar(max)** , **varbinary(max)** , **(n)text** und **image**, ziehen jedoch signifikante Speicherkosten auf sich und wirken sich aufgrund ihrer Größe auf die Leistung aus. Daher sollten Sie beim Entwerfen Ihres Systems vorsichtig sein, wenn Sie diese Datentypen verwenden.
- Die Verlaufstabelle muss in der gleichen Datenbank wie die aktuelle Tabelle erstellt werden. Temporale Abfragen über **Linked Server** werden nicht unterstützt.
- Die Verlaufstabelle darf keine Einschränkungen aufweisen (Primärschlüssel, Fremdschlüssel, Tabellen- oder Spalteneinschränkungen).
- Indizierte Sichten werden nicht zusätzlich zu temporalen Abfragen (Abfragen, die die Klausel **FOR SYSTEM_TIME** verwenden) unterstützt.
- Die Onlineoption (**WITH (ONLINE = ON**) hat keine Auswirkungen auf **ALTER TABLE ALTER COLUMN** , wenn es sich um eine temporale Tabelle mit Systemversionsverwaltung handelt. ALTER COLUMN wird nicht im Modus „online“ durchgeführt. Dies gilt unabhängig vom Wert, der für die Option ONLINE festgelegt wurde.
- **INSERT** - und **UPDATE** -Anweisungen können nicht auf die SYSTEM_TIME-Zeitraumspalten verweisen. Jeder Versuch, Werte direkt in diese Spalten einzufügen, wird blockiert.
- **TRUNCATE TABLE** wird nicht unterstützt, während **SYSTEM_VERSIONING** auf **ON**
- Die direkte Änderung von Daten in einer Verlaufstabelle ist nicht zulässig.
- **ON DELETE CASCADE** und **ON UPDATE CASCADE** sind in der aktuellen Tabelle nicht zulässig. Das heißt, dass in den Fällen, wenn die temporale Tabelle als verweisende Tabelle in der Fremdschlüsselbeziehung (entspricht *parent_object_id* in sys.foreign_keys) fungiert, keine CASCADE-Optionen zulässig sind. Verwenden Sie Anwendungslogik oder AFTER-Trigger, um die Konsistenz beim Löschen in der Primärschlüsseltabelle (entspricht *referenced_object_id* in sys.foreign_keys) beizubehalten, wenn Sie diese Einschränkung umgehen möchten. Falls die Primärschlüsseltabelle temporal und die verweisende Tabelle nicht temporal ist, gibt es keine solche Einschränkung.

  > [!NOTE]
  > Diese Einschränkung gilt nur für SQL Server 2016. CASCADE-Optionen werden in [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] und in SQL Server-2017 ab CTP 2.0 unterstützt.

- **INSTEAD OF** -Trigger sind weder bei der aktuellen noch bei der Verlaufstabelle zulässig, um zu verhindern, dass die DML-Logik ungültig wird. **AFTER** -Trigger sind nur in der aktuellen Tabelle zulässig. In der Verlaufstabelle werden sie blockiert, um zu vermeiden, dass die DML-Logik blockiert wird.
- Die Verwendung der Replikationstechniken ist eingeschränkt:

  - **Always On:** Vollständig unterstützt.
  - **Change Data Capture und Änderungsnachverfolgung:** Nur für die aktuelle Tabelle unterstützt.
  - **Momentaufnahme- und Transaktionsreplikation**: Nur unterstützt für einen Verleger mit deaktivierten temporalen Tabellen und einen Abonnenten mit aktivierten temporalen Tabellen. In diesem Fall wird der Verleger für eine OLTP-Arbeitsauslastung verwendet, während der Abonnent zum Verlagern der Berichtserstellung dient (einschließlich „AS OF“-Abfragen). Wenn der Verteilungs-Agent startet, öffnet er eine Transaktion, die geöffnet bleibt, bis der Agent angehalten wird. Aufgrund dieses Verhaltens werden SysStartTime und SysEndTime mit der Startzeit der ersten Transaktion aufgefüllt, die vom Verteilungs-Agent gestartet wird. Folglich ist es möglicherweise besser, den Verteilungs-Agent nach einem Zeitplan auszuführen, statt das Standardverhalten zu verwenden und ihn fortlaufend auszuführen, wenn für Ihre Anwendung oder Organisation wichtig ist, dass SysStartTime und SysEndTime mit einer Zeit aufgefüllt werden, die nur wenig von der aktuellen Systemzeit abweicht. Die Verwendung von mehreren Abonnenten wird nicht unterstützt, da dies aufgrund der Abhängigkeit von der lokalen Systemuhr zu inkonsistenten temporalen Daten führen könnte.
  - **Mergereplikation:** Für temporale Tabellen nicht unterstützt.

- Reguläre Abfragen betreffen nur Daten in der aktuellen Tabelle. Zum Abfragen von Daten in der Verlaufstabelle müssen Sie zeitliche Abfragen verwenden. Diese werden weiter unten in diesem Thema im Abschnitt „Abfragen temporaler Daten“ behandelt.
- Eine optimale Indizierungsstrategie enthält einen gruppierten Columnstore-Index und/oder einen B-Strukturindex auf der aktuellen Tabelle und einen Columnstore-Index auf der Verlaufstabelle für die optimale Speichergröße und -Leistung. Falls Sie Ihre eigene Verlaufstabelle erstellen oder verwenden, empfehlen wir Ihnen dringend, diese Art Index zu erstellen. Dieser Index sollte aus Zeitraumspalten bestehen und mit der Spalte „Ende des Zeitraums“ beginnen, um die temporalen Abfragen sowie die Abfragen, die Teil der Datenkonsistenzüberprüfung sind, zu beschleunigen. Die Standardverlaufstabelle hat einen für Sie erstellten gruppierten Rowstore-Index, der auf den Zeitraumspalten (Ende, Start) basiert. Ein nicht gruppierter Rowstore-Index wird mindestens empfohlen.
- Die folgenden Objekte bzw. Eigenschaften werden beim Erstellen der Verlaufstabelle nicht aus der aktuellen Tabelle in die Verlaufstabelle repliziert:

  - Periodendefinition
  - Identitätsdefinition
  - Indizes
  - Statistik
  - Check-Einschränkungen
  - Trigger
  - Partitionierungskonfiguration
  - Berechtigungen
  - Prädikate für die Sicherheit auf Zeilenebene

- Eine Verlaufstabelle kann nicht als aktuelle Tabelle in einer Kette von Verlaufstabellen konfiguriert werden.

## <a name="see-also"></a>Weitere Informationen

- [Temporale Tabellen](../../relational-databases/tables/temporal-tables.md)
- [Erste Schritte mit temporalen Tabellen mit Systemversionsverwaltung](../../relational-databases/tables/getting-started-with-system-versioned-temporal-tables.md)
- [Systemkonsistenzprüfungen von temporalen Tabellen](../../relational-databases/tables/temporal-table-system-consistency-checks.md)
- [Partitionierung mit temporären Tabellen](../../relational-databases/tables/partitioning-with-temporal-tables.md)
- [Sicherheit bei temporale Tabellen](../../relational-databases/tables/temporal-table-security.md)
- [Verwalten der Beibehaltung von Verlaufsdaten in temporalen Tabellen mit Systemversionsverwaltung](../../relational-databases/tables/manage-retention-of-historical-data-in-system-versioned-temporal-tables.md)
- [Temporale Tabellen mit Systemversionsverwaltung und speicheroptimierten Tabellen](../../relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables.md)
- [Metadatenansichten und Funktionen für temporale Tabellen](../../relational-databases/tables/temporal-table-metadata-views-and-functions.md)
