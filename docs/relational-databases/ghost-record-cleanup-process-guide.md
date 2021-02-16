---
title: Leitfaden zum Prozess für das Cleanup inaktiver Datensätze | Microsoft-Dokumentation
description: Erfahren Sie mehr über das Cleanup inaktiver Datensätze, einen Hintergrundprozess, durch den in SQL Server zur Löschung gekennzeichnete Datensätze von Seiten gelöscht werden.
ms.custom: ''
ms.date: 05/02/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- ghost cleanup
- ghost records
- ghost clean up process
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: e228afb793e69ab7b9d36fae31e838f6d1bca15a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100059225"
---
# <a name="ghost-cleanup-process-guide"></a>Leitfaden zum Prozess für das Cleanup inaktiver Datensätze

Beim Cleanup inaktiver Datensätze handelt es sich um einen Singlethread-Hintergrundprozess, durch den zur Löschung gekennzeichnete Datensätze von Seiten gelöscht werden. Der folgende Artikel bietet einen Überblick über diesen Prozess.

## <a name="ghost-records"></a>Inaktive Datensätze

Datensätze, die auf der Blattebene einer Indexseite gelöscht werden, werden nicht von der Seite entfernt. Stattdessen werden die Datensätze mit „Zu löschen“ gekennzeichnet oder *verwaist*. Dies bedeutet, dass die Zeile weiterhin auf der Seite angezeigt wird. Jedoch wird in der Zeilenüberschrift eine geringfügige Änderung vorgenommen, die anzeigt, dass die Zeile verwaist ist. Dies soll zu einer Optimierung der Leistung während eines Löschvorgangs beitragen. Verwaiste Elemente sind für Sperren auf Zeilenebene, aber auch für Momentaufnahmeisolationen erforderlich, bei denen die älteren Zeilenversionen verwaltet werden müssen.

## <a name="ghost-record-cleanup-task"></a>Task für das Cleanup inaktiver Datensätze

Zum Löschen gekennzeichnete oder *verwaiste* Datensätze werden durch den Hintergrundprozess für das Cleanup inaktiver Datensätze bereinigt. Dieser Hintergrundprozess wird manchmal ausgeführt, nachdem die Löschtransaktion zugesichert wurde. Durch ihn werden verwaiste Datensätze von Seiten entfernt. Der Prozess für das Cleanup inaktiver Datensätze wird automatisch in regelmäßigen Zeitabständen ausgeführt (bei SQL Server 2012+ alle 5 Sekunden, bei SQL Server 2008/2008R2 alle 10 Sekunden) und prüft, ob alle Seiten, auf denen sich inaktive Datensätze befinden, entsprechend gekennzeichnet wurden. Findet der Prozess inaktive Datensätze, werden diese zur Löschung gekennzeichneten Datensätze gelöscht oder *verwaist*. Bei jeder Ausführung sind höchstens 10 Seiten betroffen.

Wenn ein Datensatz verwaist ist, wird die Datenbank entsprechend gekennzeichnet. Der Prozess für das Cleanup inaktiver Datensätze überprüft dann nur diese Datenbanken. Sobald sämtliche verwaisten Datensätze gelöscht wurden, kennzeichnet der Prozess für das Cleanup inaktiver Datensätze die Datenbank mit „having no ghosted records“ (enthält keine verwaisten Datensätze) und überspringt diese Datenbank bei der nächsten Ausführung. Darüber hinaus überspringt der Prozess Datenbanken, in denen er keine gemeinsame Sperre übernehmen kann, und wiederholt diesen Vorgang bei der nächsten Ausführung.

Mit der nachfolgenden Abfrage kann ermittelt werden, wie viele verwaiste Datensätze in einer einzelnen Datenbank enthalten sind. 

 ```sql
 SELECT sum(ghost_record_count) total_ghost_records, db_name(database_id) 
 FROM sys.dm_db_index_physical_stats (NULL, NULL, NULL, NULL, 'SAMPLED')
 group by database_id
 order by total_ghost_records desc
```

## <a name="disable-the-ghost-cleanup"></a>Deaktivieren des Cleanups inaktiver Datensätze

Auf Systemen mit hoher Belastung, auf denen viele Löschvorgänge durchgeführt werden, kann der Prozess für das Cleanup inaktiver Datensätze zu Leistungsproblemen bei der Beibehaltung von Seiten im Pufferpool und bei der E/A-Generierung führen. Folglich kann dieser Prozess mit dem [Ablaufverfolgungsflag 661](../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) deaktiviert werden. Eine Deaktivierung des Prozesses kann jedoch zu einer Beeinträchtigung der Leistung führen.

Durch das Deaktivieren des Prozesses für das Cleanup inaktiver Datensätze kann Ihre Datenbank unnötig groß werden. Dies kann zu Leistungsproblemen führen. Da durch das Cleanup als inaktiv gekennzeichnete Datensätze entfernt werden, bleiben diese Datensätze auf der Seite, wenn Sie den Prozess deaktivieren. Dadurch kann SQL Server diesen Speicherplatz nicht wiederverwenden. Folglich muss SQL Server stattdessen Daten zu neuen Seiten hinzufügen. Dies führt zu übermäßig großen Datenbankdateien und kann [Seitenteilungen](indexes/specify-fill-factor-for-an-index.md) verursachen. Seitenteilungen wiederum führen bei der Erstellung von Ausführungsplänen und bei der Durchführung von Prüfvorgängen zu Leistungsproblemen. 

Sobald der Prozess für das Cleanup inaktiver Datensätze deaktiviert wurde, müssen für die Entfernung der verwaisten Datensätze einige Maßnahmen ergriffen werden. Eine Möglichkeit besteht in einer Indexneuerstellung. Dabei werden Daten auf Seiten verschoben. Eine andere Möglichkeit besteht darin, [sp_clean_db_free_space](system-stored-procedures/sp-clean-db-free-space-transact-sql.md) (für die Bereinigung sämtlicher Datenbank-Datendateien) oder [sp_clean_db_file_free_space](system-stored-procedures/sp-clean-db-file-free-space-transact-sql.md) (für die Bereinigung einzelner Datenbank-Datendateien) manuell auszuführen. Dadurch werden verwaiste Datensätze gelöscht.

 >[!warning]
 > Generell wird von einer Deaktivierung des Prozesses für das Cleanup inaktiver Datensätze abgeraten. Sollten Sie dennoch eine Deaktivierung in Erwägung ziehen, prüfen Sie dies gründlich in einer kontrollierten Umgebung, bevor Sie sie dauerhaft in einer Produktionsumgebung implementieren.


## <a name="next-steps"></a>Nächste Schritte  
[Deaktivieren des Prozesses für die Bereinigung inaktiver Datensätze](https://support.microsoft.com/help/920093/tuning-options-for-sql-server-when-running-in-high-performance-workloa)
<br>[Entfernen inaktiver Datensätze aus einer einzelnen Datenbankdatei](system-stored-procedures/sp-clean-db-file-free-space-transact-sql.md)
<br>[Entfernen inaktiver Datensätze aus sämtlichen Datenbankdateien](system-stored-procedures/sp-clean-db-free-space-transact-sql.md)


