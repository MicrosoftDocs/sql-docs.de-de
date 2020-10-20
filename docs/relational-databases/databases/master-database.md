---
description: master-Datenbank
title: Master-Datenbank | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/28/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- master database [SQL Server], about
- master database [SQL Server]
ms.assetid: 660e909f-61eb-406b-bbce-8864dd629ba0
author: stevestein
ms.author: sstein
ms.openlocfilehash: bdc1669274577d7d75f0d50ab2f5b9ec736799cc
ms.sourcegitcommit: cfa04a73b26312bf18d8f6296891679166e2754d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2020
ms.locfileid: "92194372"
---
# <a name="master-database"></a>master-Datenbank

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  In der **master** -Datenbank werden alle Systemebeneninformationen für ein [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -System aufgezeichnet. Dazu gehören instanzweite Metadaten wie Anmeldekonten, Endpunkte, Verbindungsserver und Systemkonfigurationseinstellungen. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]werden Systemobjekte nicht mehr in der **master** -Datenbank gespeichert. Stattdessen werden sie in der [Resource-Datenbank](../../relational-databases/databases/resource-database.md)gespeichert. Die **master** -Datenbank bezeichnet die Datenbank, die das Vorhandensein aller anderen Datenbanken, einschließlich der Speicherorte der Datenbankdateien, sowie die Initialisierungsinformationen für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]aufzeichnet. Deshalb kann [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nicht starten, wenn die **master** -Datenbank nicht verfügbar ist.  

> [!IMPORTANT]
> Für Azure SQL-Datenbank Singletons und Pools für elastische Datenbanken gelten nur die Masterdatenbank und die tempdb-Datenbank. Weitere Informationen finden Sie unter [Was ist ein Azure SQL-Datenbank-Server](/azure/sql-database/sql-database-servers#what-is-an-azure-sql-database-server). Eine Erläuterung von tempdb im Kontext von Azure SQL-Datenbank finden Sie unter [tempdb-Datenbank in Azure SQL-Datenbank](tempdb-database.md#tempdb-database-in-sql-database). Für Azure SQL Managed Instance gelten alle Systemdatenbanken. Weitere Informationen zu verwalteten Instanzen in Azure SQL-Datenbank finden Sie unter [Was ist eine verwaltete Instanz](/azure/sql-database/sql-database-managed-instance).
  
## <a name="physical-properties-of-master"></a>physische Eigenschaften der master-Datenbank

Die folgende Tabelle zeigt die Anfangskonfigurationswerte der **master**-Daten und -Protokolldateien für SQL Server und Azure SQL Managed Instance. Die Größe dieser Dateien kann sich in den verschiedenen Editionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]geringfügig unterscheiden.  
  
|Datei|Logischer Name (logical name)|Physischer Name (physical name)|Dateivergrößerung (file growth)|  
|----------|------------------|-------------------|-----------------|  
|Primäre Daten|master|master.mdf|Automatische Vergrößerung um 10 Prozent, bis der Speicherplatz auf dem Datenträger erschöpft ist.|  
|Log|mastlog|mastlog.ldf|Automatische Vergrößerung um 10 %, bis der Maximalwert von 2 TB erreicht wird.|  
  
Informationen zum Verschieben der **master** -Daten und -Protokolldateien finden Sie unter [Verschieben von Systemdatenbanken](../../relational-databases/databases/move-system-databases.md).  

> [!IMPORTANT]
> Beim Azure SQL-Datenbank-Server hat der Benutzer keine Kontrolle über die Größe der **master**-Datenbank.
  
### <a name="database-options"></a>Datenbankoptionen

Die folgende Tabelle zeigt den Standardwert jeder Datenbankoption in der **master**-Datenbank für SQL Server und Azure SQL Managed Instance und gibt an, ob die Option geändert werden kann. Zum Anzeigen der aktuellen Einstellungen dieser Optionen verwenden Sie die Katalogsicht [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) .  
  
> [!IMPORTANT]
> Bei Azure SQL-Datenbank Singletons und Pools für elastische Datenbanken hat der Benutzer keine Kontrolle über diese Datenbankoptionen.

|Datenbankoption|Standardwert|Kann geändert werden.|  
|---------------------|-------------------|---------------------|  
|ALLOW_SNAPSHOT_ISOLATION|EIN|Nein|  
|ANSI_NULL_DEFAULT|OFF|Ja|  
|ANSI_NULLS|OFF|Ja|  
|ANSI_PADDING|OFF|Ja|  
|ANSI_WARNINGS|OFF|Ja|  
|ARITHABORT|OFF|Ja|  
|AUTO_CLOSE|OFF|Nein|  
|AUTO_CREATE_STATISTICS|EIN|Ja|  
|AUTO_SHRINK|OFF|Nein|  
|AUTO_UPDATE_STATISTICS|EIN|Ja|  
|AUTO_UPDATE_STATISTICS_ASYNC|OFF|Ja|  
|CHANGE_TRACKING|OFF|Nein|  
|CONCAT_NULL_YIELDS_NULL|OFF|Ja|  
|CURSOR_CLOSE_ON_COMMIT|OFF|Ja|  
|CURSOR_DEFAULT|GLOBAL|Ja|  
|Datenbankverfügbarkeitsoptionen|ONLINE<br /><br /> MULTI_USER<br /><br /> READ_WRITE|Nein<br /><br /> Nein<br /><br /> Nein|  
|DATE_CORRELATION_OPTIMIZATION|OFF|Ja|  
|DB_CHAINING|EIN|Nein|  
|ENCRYPTION|OFF|Nein|  
|MIXED_PAGE_ALLOCATION|EIN|Nein|  
|NUMERIC_ROUNDABORT|OFF|Ja|  
|PAGE_VERIFY|CHECKSUM|Ja|  
|PARAMETERIZATION|SIMPLE|Ja|  
|QUOTED_IDENTIFIER|OFF|Ja|  
|READ_COMMITTED_SNAPSHOT|OFF|Nein|  
|RECOVERY|SIMPLE|Ja|  
|RECURSIVE_TRIGGERS|OFF|Ja|  
|Service Broker-Optionen|DISABLE_BROKER|Nein|  
|TRUSTWORTHY|OFF|Ja|  
  
Eine Beschreibung dieser Datenbankoptionen finden Sie unter [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  
  
## <a name="restrictions"></a>Beschränkungen  
Die folgenden Operationen können an der **master** -Datenbank nicht ausgeführt werden:  
  
- Hinzufügen von Dateien oder Dateigruppen.  
- Bei Sicherungen kann für die Masterdatenbank nur eine vollständige Datenbanksicherung ausgeführt werden.
- Ändern der Sortierung. Die Standardsortierung entspricht der Serversortierung.  
- Ändern des Datenbankbesitzers Der Besitzer von**master** ist **sa**.  
- Erstellen eines Volltextkatalogs oder Volltextindex.  
- Erstellen von Triggern für Systemtabellen in der Datenbank.  
- Löschen der Datenbank.  
- Löschen des **guest** -Benutzers aus der Datenbank.  
- Aktivieren von Change Data Capture  
- Teilnehmen an der Datenbankspiegelung.  
- Entfernen der primären Dateigruppe, der primären Datendatei oder der Protokolldatei.  
- Umbenennen der Datenbank oder der primären Dateigruppe.  
- Versetzen der Datenbank in den OFFLINE-Modus.  
- Versetzen der Datenbank oder der primären Dateigruppe in den READ_ONLY-Modus.  
  
## <a name="recommendations"></a>Empfehlungen  
Beim Arbeiten mit der **master** -Datenbank sollten Sie die folgenden Empfehlungen beachten:  
  
- Stellen Sie sicher, dass Sie jederzeit über eine aktuelle Sicherungskopie der **master** -Datenbank verfügen.  
- Erstellen Sie nach den folgenden Operationen sobald wie möglich eine Sicherungskopie der **master** -Datenbank:  
  
  - Erstellen, Ändern oder Löschen einer Datenbank  
  - Ändern der Server- oder Datenbankkonfigurationswerte  
  - Ändern oder Hinzufügen von Anmeldekonten  
  
- Erstellen Sie in der **master**-Datenbank keine Benutzerobjekte. Andernfalls muss die **master** -Datenbank häufiger gesichert werden.  
- Legen Sie die Option TRUSTWORTHY für die **master** -Datenbank nicht auf ON fest.  
  
## <a name="what-to-do-if-master-becomes-unusable"></a>Erforderliche Aktionen, wenn "master" nicht mehr verwendbar ist  
 Wenn **master** nicht mehr verwendet werden kann, gibt es mehrere Methoden, um sie in einen verwendbaren Zustand zurückzuversetzen:  
  
- Wiederherstellen der **master** -Datenbank von einer aktuellen Datenbanksicherung.  
  
  Wenn Sie die Serverinstanz starten können, sollten Sie in der Lage sein, **master** aus einer vollständigen Datenbanksicherung wiederherzustellen. Weitere Informationen finden Sie unter [Wiederherstellen der master-Datenbank &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/restore-the-master-database-transact-sql.md).  
  
- Vollständige Neuerstellung der **master** -Datenbank.  
  
  Falls ernsthafte Schäden an der **master** -Datenbank das Starten von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]verhindern, müssen Sie die **master**-Datenbank neu erstellen. Weitere Informationen finden Sie unter [Neuerstellen von Systemdatenbanken](../../relational-databases/databases/rebuild-system-databases.md).  
  
  > [!IMPORTANT]  
  >  Beim Neuerstellen der **master** -Datenbank werden alle Systemdatenbanken neu erstellt.  
  
## <a name="related-content"></a>Verwandte Inhalte  
- [Neuerstellen von Systemdatenbanken](../../relational-databases/databases/rebuild-system-databases.md)  
- [Systemdatenbanken](../../relational-databases/databases/system-databases.md)  
- [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)  
- [sys.master_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)  
- [Verschieben von Datenbankdateien](../../relational-databases/databases/move-database-files.md)