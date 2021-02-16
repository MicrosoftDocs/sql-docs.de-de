---
title: Transparente Datenverschlüsselung (TDE) | Microsoft-Dokumentation
description: Hier finden Sie Informationen zur Transparent Data Encryption-Technologie, die SQL Server-, Azure SQL-Datenbank- und Azure Synapse Analytics-Daten verschlüsselt. Dieser Vorgang wird als „Verschlüsselung ruhender Daten“ bezeichnet.
ms.custom: ''
ms.date: 05/09/2019
ms.prod: sql
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Transparent Data Encryption
- database encryption key, about
- TDE
- database encryption key
- TDE, about
- Transparent Data Encryption, about
- encryption [SQL Server], transparent data encryption
ms.assetid: c75d0d4b-4008-4e71-9a9d-cee2a566bd3b
author: jaszymas
ms.author: jaszymas
ms.reviewer: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ef973df188c8df74ccddb2415094afb4e25c399c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345396"
---
# <a name="transparent-data-encryption-tde"></a>TDE (Transparent Data Encryption)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

*Transparente Datenverschlüsselung* (Transparent Data Encryption, TDE) verschlüsselt [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-, [!INCLUDE[ssSDSFull](../../../includes/sssdsfull-md.md)]- und [!INCLUDE[ssSDWfull](../../../includes/sssdwfull-md.md)]-Datendateien. Diese Verschlüsselung ist auch als Verschlüsselung von ruhenden Daten bekannt.

Sie können die folgenden Vorkehrungen treffen, um eine Datenbank zu sichern:

* Entwerfen eines sicheren Systems
* Verschlüsseln vertraulicher Ressourcen
* Erstellen einer Firewall für die Datenbankserver

Jedoch kann eine böswillige Partei, die physische Medien wie Datenträger oder Sicherungsbänder stiehlt, die Datenbank wiederherstellen oder anfügen und ihre Daten durchsuchen.

Eine Lösung dieses Problems besteht darin, die sensiblen Daten in einer Datenbank zu verschlüsseln und ein Zertifikat zum Schützen der Schlüssel zu verwenden, die die Daten verschlüsseln. Mit dieser Lösung wird verhindert, dass Personen ohne die Schlüssel die Daten verwenden können. Diese Art von Schutz müssen Sie jedoch im Voraus planen.

TDE führt die E/A-Verschlüsselung und -Entschlüsselung von Daten- und Protokolldateien in Echtzeit durch. Die Verschlüsselung nutzt einen DEK (Database Encryption Key, Datenbankverschlüsselungsschlüssel). Der Datenbankstartdatensatz speichert den Schlüssel, damit er für die Wiederherstellung verfügbar ist. Der DEK ist ein symmetrischer Schlüssel. Dieser wird von einem Zertifikat, das in der Masterdatenbank des Servers gespeichert wird, oder von einem asymmetrischen Schlüssel geschützt, der von einem EKM-Modul geschützt wird.

TDE schützt ruhende Daten, also die Daten- und Protokolldateien. Sie entspricht den in vielen Branchen etablierten Gesetzen, Bestimmungen und Richtlinien. Dadurch können Softwareentwickler Daten mithilfe der AES- und 3DES-Verschlüsselungsalgorithmen verschlüsseln, ohne vorhandene Anwendungen ändern zu müssen.

> [!IMPORTANT]
> TDE stellt keine Verschlüsselung über Kommunikationskanäle bereit. Weitere Informationen zum Verschlüsseln von Daten über Kommunikationskanäle finden Sie unter [Aktivieren von verschlüsselten Verbindungen für die Datenbank-Engine (SQL Server-Konfigurations-Manager)](../../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).
>
>**Verwandte Themen:**
>
> - [Transparent Data Encryption mit Azure SQL-Datenbank](/azure/azure-sql/database/transparent-data-encryption-tde-overview)
> - [Erste Schritte mit Transparent Data Encryption (TDE) in Azure Synapse Analytics](/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-encryption-tde-tsql)
> - [Verschieben einer TDE-geschützten Datenbank auf einen anderen SQL-Server](../../../relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server.md)
> - [Aktivieren von TDE in SQL Server mithilfe von EKM](../../../relational-databases/security/encryption/enable-tde-on-sql-server-using-ekm.md)
> - [Verwenden von SQL Server-Connector mit SQL-Verschlüsselungsfunktionen](../../../relational-databases/security/encryption/use-sql-server-connector-with-sql-encryption-features.md)
> - [SQL Server Security Blog on TDE with FAQ (SQL Server Blog zu Sicherheitsthemen über TDE mit FAQ)](/archive/blogs/sqlsecurity/feature-spotlight-transparent-data-encryption-tde)

## <a name="about-tde"></a>Informationen zu TDE

Die Verschlüsselung der Datenbankdatei erfolgt auf Seitenebene. In einer verschlüsselten Datenbank werden die Seiten verschlüsselt, bevor sie auf den Datenträger geschrieben werden, und entschlüsselt, wenn sie in den Arbeitsspeicher gelesen werden. TDE erhöht nicht die Größe der verschlüsselten Datenbank.

### <a name="information-applicable-to-sssds"></a>Informationen zu [!INCLUDE[ssSDS](../../../includes/sssds-md.md)]

Wenn Sie TDE mit [!INCLUDE[sqldbesa](../../../includes/sqldbesa-md.md)] V12 verwenden, erstellt [!INCLUDE[ssSDS](../../../includes/sssds-md.md)] automatisch das Zertifikat auf Serverebene, das in der Masterdatenbank gespeichert wird. Beim Verschieben einer TDE-Datenbank nach [!INCLUDE[ssSDS](../../../includes/sssds-md.md)] muss die Datenbank für den Verschiebevorgang nicht entschlüsselt werden. Weitere Informationen zur Verwendung von TDE mit [!INCLUDE[ssSDS](../../../includes/sssds-md.md)] finden Sie unter [Transparente Datenverschlüsselung in Azure SQL-Datenbank](/azure/azure-sql/database/transparent-data-encryption-tde-overview).

### <a name="information-applicable-to-ssnoversion"></a>Informationen zu [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]

Nachdem Sie eine Datenbank gesichert haben, können Sie diese mithilfe des richtigen Zertifikats wiederherstellen. Weitere Informationen zu Zertifikaten finden Sie unter [SQL Server Certificates and Asymmetric Keys](../../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md).

Sobald Sie TDE aktiviert haben, sollten Sie umgehend das Zertifikat und den zugehörigen privaten Schlüssel sichern. Wenn das Zertifikat nicht mehr verfügbar ist oder Sie die Datenbank auf einem anderen Server wiederherstellen oder anfügen, benötigen Sie Sicherungen des Zertifikats und des privaten Schlüssels. Andernfalls können Sie die Datenbank nicht öffnen.

Behalten Sie das verschlüsselnde Zertifikat, selbst wenn Sie TDE für die Datenbank deaktiviert haben. Obwohl die Datenbank nicht verschlüsselt wird, werden Teile des Transaktionsprotokolls möglicherweise weiterhin geschützt. Möglicherweise benötigen Sie das Zertifikat auch für einige Vorgänge, bis Sie eine vollständige Datenbanksicherung durchgeführt haben.

Sie können ein Zertifikat, dessen Ablaufdatum überschritten wurde, weiterhin zum Verschlüsseln und Entschlüsseln von Daten mit TDE verwenden.

### <a name="encryption-hierarchy"></a>Verschlüsselungshierarchie

Die folgende Abbildung zeigt die Architektur der TDE-Verschlüsselung. Nur die Datenbankebenenelemente (der Datenbankverschlüsselungsschlüssel und die ALTER DATABASE-Teile) können bei der Verwendung von TDE in [!INCLUDE[ssSDS](../../../includes/sssds-md.md)] vom Benutzer konfiguriert werden.

![TDE-Architektur](../../../relational-databases/security/encryption/media/tde-architecture.png)

## <a name="enable-tde"></a>Aktivieren von TDE

Führen Sie folgende Schritte aus, um TDE zu verwenden:

**Gilt für**: [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].

1. Erstellen Sie einen Hautschlüssel.

1. Erstellen oder beziehen Sie ein vom Hauptschlüssel geschütztes Zertifikat.

1. Erstellen Sie einen Verschlüsselungsschlüssel für die Datenbank, und schützen Sie ihn mithilfe des Zertifikats.

1. Legen Sie die Verschlüsselung für die Datenbank fest.

Im folgenden Beispiel wird die Verschlüsselung und Entschlüsselung der `AdventureWorks2012`-Datenbank mithilfe eines Zertifikats namens `MyServerCert` veranschaulicht, das auf dem Server installiert ist.

```sql
USE master;
GO
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<UseStrongPasswordHere>';
go
CREATE CERTIFICATE MyServerCert WITH SUBJECT = 'My DEK Certificate';
go
USE AdventureWorks2012;
GO
CREATE DATABASE ENCRYPTION KEY
WITH ALGORITHM = AES_128
ENCRYPTION BY SERVER CERTIFICATE MyServerCert;
GO
ALTER DATABASE AdventureWorks2012
SET ENCRYPTION ON;
GO
```

Die Verschlüsselungs- und Entschlüsselungsvorgänge werden in von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]geplanten Hintergrundthreads ausgeführt. Verwenden Sie die Katalogsichten und dynamischen Verwaltungssichten der Tabelle, die im späteren Verlauf dieses Artikels gezeigt wird, um den Status dieser Vorgänge anzuzeigen.

> [!CAUTION]
> Sicherungsdateien für Datenbanken, für die TDE aktiviert wurde, werden ebenfalls mit dem Datenbankverschlüsselungsschlüssel verschlüsselt. Darum muss bei der Wiederherstellung dieser Sicherungen das Zertifikat, das zum Verschlüsseln des Verschlüsselungsschlüssels für die Datenbank verwendet wurde, verfügbar sein. Stellen Sie daher zusätzlich zum Sichern der Datenbank sicher, dass Sie Sicherungen der Serverzertifikate anlegen. Wenn die Zertifikate nicht mehr verfügbar sind, kann es zu Datenverlust kommen.
>
> Weitere Informationen finden Sie unter [SQL Server Certificates and Asymmetric Keys](../../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md).

## <a name="commands-and-functions"></a>Befehle und Funktionen

Damit die folgenden Anweisungen TDE-Zertifikate akzeptieren, verwenden Sie einen Datenbankhauptschlüssel, um sie zu verschlüsseln. Wenn Sie diese nur mit einem Kennwort verschlüsseln, lehnen die Anweisungen die Verschlüsselungen ab.

> [!IMPORTANT]
> Wenn Sie das Zertifikatkennwort schützen, nachdem es von TDE verwendet wird, kann nach einem Neustart nicht auf die Datenbank zugegriffen werden.

Die folgende Tabelle bietet Links und Erläuterungen zu den Befehlen und Funktionen von TDE:

|Befehl oder Funktion|Zweck|
|-------------------------|-------------|
|[CREATE DATABASE ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-database-encryption-key-transact-sql.md)|Erstellt einen Schlüssel, der die Datenbank verschlüsselt| 
|[ALTER DATABASE ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/alter-database-encryption-key-transact-sql.md)|Ändert den Schlüssel, der die Datenbank verschlüsselt|
|[DROP DATABASE ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/drop-database-encryption-key-transact-sql.md)|Entfernt den Schlüssel, der die Datenbank verschlüsselt|
|[ALTER DATABASE SET-Optionen (Transact-SQL)](../../../t-sql/statements/alter-database-transact-sql-set-options.md)|Erklärt die **ALTER DATABASE**-Option, mit der TDE aktiviert wird|

## <a name="catalog-views-and-dynamic-management-views"></a>Katalogsichten und dynamische Verwaltungssichten

 In der folgenden Tabelle werden die Katalogsichten und die dynamischen Verwaltungssichten von TDE erläutert.

|Katalogsicht oder dynamische Verwaltungssicht|Zweck|
|---------------------------------------------|-------------|
|[sys.databases (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)|Katalogsicht, die Datenbankinformationen anzeigt|
|[sys.certificates (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-certificates-transact-sql.md)|Katalogsicht, die die Zertifikate in einer Datenbank anzeigt|
|[sys.dm_database_encryption_keys (Transact-SQL)](../../../relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql.md)|Dynamische Verwaltungssicht, die Informationen zu den Verschlüsselungsschlüsseln einer Datenbank und dem Zustand der Verschlüsselung bereitstellt|

## <a name="permissions"></a>Berechtigungen

Jede Funktion und jeder Befehl von TDE erfordert bestimmte Berechtigungen, die in den zuvor gezeigten Tabellen beschrieben wurden.

Zum Anzeigen der Metadaten, die im Zusammenhang mit TDE stehen, ist die VIEW DEFINITION-Berechtigung für ein Zertifikat erforderlich.

## <a name="considerations"></a>Überlegungen

Während eine erneute Verschlüsselungsprüfung für einen Datenbankverschlüsselungsvorgang ausgeführt wird, sind Wartungsvorgänge für die Datenbank deaktiviert. Sie können den Einzelbenutzermodus für die Datenbank verwenden, um Wartungsvorgänge durchzuführen. Weitere Informationen finden Sie unter [So legen Sie den Einzelbenutzermodus für eine Datenbank fest](../../../relational-databases/databases/set-a-database-to-single-user-mode.md).

Verwenden Sie die dynamische Verwaltungssicht „sys.dm_database_encryption_keys“, um den Zustand der Datenbankverschlüsselung zu ermitteln. Weitere Informationen finden Sie im Abschnitt „Katalogsichten und dynamische Verwaltungssichten“ weiter oben in diesem Artikel.

Bei TDE werden alle Dateien und Dateigruppen in einer Datenbank verschlüsselt. Wenn eine Dateigruppe in einer Datenbank als READ ONLY gekennzeichnet ist, schlägt der Datenbankverschlüsselungsvorgang fehl.

Wenn Sie eine Datenbank bei der Datenbankspiegelung oder beim Protokollversand verwenden, werden beide Datenbanken verschlüsselt. Die Protokolltransaktionen werden für die Übertragung zwischen den Datenbanken verschlüsselt.

> [!IMPORTANT]
> Volltextindizes werden verschlüsselt, wenn für eine Datenbank die Verschlüsselung festgelegt ist. Solche Indizes, die in einer SQL Server-Version vor SQL Server 2008 erstellt wurden, werden von SQL Server 2008 oder höher in die Datenbank importiert und von TDE verschlüsselt.

> [!TIP]
> Verwenden Sie SQL Server Audit oder SQL-Datenbanküberwachung, um die Veränderungen im TDE-Status einer Datenbank zu überwachen. Bei SQL Server wird TDE unter der Überwachungsaktionsgruppe DATABASE_CHANGE_GROUP nachverfolgt, die Sie in [SQL Server Audit-Aktionsgruppen und -Aktionen](../../../relational-databases/security/auditing/sql-server-audit-action-groups-and-actions.md) finden können.

## <a name="restrictions"></a>Beschränkungen

Die folgende Vorgänge sind während der ersten Datenbankverschlüsselung, einer Schlüsseländerung oder der Datenbankentschlüsselung nicht erlaubt:

- Löschen einer Datei aus einer Dateigruppe in einer Datenbank

- Löschen einer Datenbank

- Offlineschalten einer Datenbank

- Trennen einer Datenbank

- Versetzen einer Datenbank oder einer Dateigruppe in den READ ONLY-Zustand

Die folgenden Vorgänge sind während der Ausführung der Anweisungen CREATE DATABASE ENCRYPTION KEY, ALTER DATABASE ENCRYPTION KEY, DROP DATABASE ENCRYPTION KEY und ALTER DATABASE...SET ENCRYPTION nicht erlaubt:

- Löschen einer Datei aus einer Dateigruppe in einer Datenbank

- Löschen einer Datenbank

- Offlineschalten einer Datenbank

- Trennen einer Datenbank

- Versetzen einer Datenbank oder einer Dateigruppe in den READ ONLY-Zustand

- Verwenden eines ALTER DATABASE-Befehls

- Starten einer Datenbank- oder Datenbankdateisicherung

- Starten einer Datenbank- oder Datenbankdateiwiederherstellung

- Erstellen einer Momentaufnahme

Die folgenden Vorgänge oder Bedingungen verhindern die Anweisungen CREATE DATABASE ENCRYPTION KEY, ALTER DATABASE ENCRYPTION KEY, DROP DATABASE ENCRYPTION KEY und ALTER DATABASE...SET ENCRYPTION:

- Eine Datenbank ist schreibgeschützt oder enthält schreibgeschützte Dateigruppen.

- Ein ALTER DATABASE-Befehl wird ausgeführt.

- Eine Datensicherung wird ausgeführt.

- Eine Datenbank ist offline geschaltet oder wird wiederhergestellt.

- Eine Momentaufnahme wird ausgeführt.

- Es werden Datenbankwartungstasks ausgeführt.

Wenn TDE aktiviert ist, ist die Dateiinitialisierung beim Erstellen von Datenbankdateien nicht verfügbar.

Der asymmetrische Schlüssel muss sich auf einem erweiterbaren Schlüsselverwaltungsanbieter befinden, um einen Datenbankverschlüsselungsschlüssel mit einem asymmetrischen Schlüssel zu verschlüsseln.

## <a name="tde-scan"></a>TDE-Überprüfung

[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] muss einen Verschlüsselungsscan durchführen, damit TDE auf einer Datenbank aktiviert werden kann. Der Scan liest alle Seiten der Datendateien in den Pufferpool und schreib die verschlüsselten Seiten dann wieder auf den Datenträger.

Damit Sie über mehr Kontrolle über den Verschlüsselungsscan verfügen, wurde mit [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)] der TDE-Scan eingeführt, der über eine Syntax zum Anhalten und Fortsetzen verfügt. Wenn die Arbeitsauslastung des Systems sehr hoch ist oder während geschäftskritischen Zeiten, können Sie den Scan pausieren und später fortsetzen.

Verwenden Sie die folgende Syntax, um den TDE-Verschlüsselungsscan anzuhalten:

```sql
ALTER DATABASE <db_name> SET ENCRYPTION SUSPEND;
```

Auf ähnliche Weise können Sie den TDE-Verschlüsselungsscan mithilfe der folgenden Syntax fortsetzen:

```sql
ALTER DATABASE <db_name> SET ENCRYPTION RESUME;
```

Die Spalte „encryption_scan_state“ wurde zur dynamischen Verwaltungssicht „sys.dm_database_encryption_keys“ hinzugefügt. Diese zeigt den aktuellen Zustand des Verschlüsselungsscans an. Außerdem wurde eine neue Spalte namens „encryption_scan_modify_date“ hinzugefügt, die das Datum und die Uhrzeit der letzten Änderung des Status des Verschlüsselungsscans enthält.

Wenn die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Instanz neu gestartet wird, während ihr Verschlüsselungsscan angehalten ist, wird beim Start eine Meldung im Fehlerprotokoll protokolliert. Die Meldung gibt an, dass ein bestehender Scan angehalten wurde.

## <a name="tde-and-transaction-logs"></a>TDE und Transaktionsprotokolle

Wenn eine Datenbank TDE verwendet, wird der restliche Teil des aktuellen virtuellen Transaktionsprotokolls entfernt. Die Entfernung erzwingt die Erstellung des nächsten Transaktionsprotokolls. Dieses Verhalten gewährleistet, dass kein Klartext in en Protokollen verbleibt, nachdem die Datenbank für die Verschlüsselung eingerichtet wurde.

Den Status der Protokolldateiverschlüsselung finden Sie wie im folgenden Beispiel gezeigt in der `encryption_state`-Spalte der `sys.dm_database_encryption_keys`-Sicht:

```sql
USE AdventureWorks2012;
GO
/* The value 3 represents an encrypted state
   on the database and transaction logs. */
SELECT *
FROM sys.dm_database_encryption_keys
WHERE encryption_state = 3;
GO
```

Weitere Informationen zur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Protokolldateiarchitektur finden Sie unter [Das Transaktionsprotokoll (SQL Server)](../../../relational-databases/logs/the-transaction-log-sql-server.md).

Bevor ein Datenbankverschlüsselungsschlüssel geändert wird, verschlüsselt der vorherige Datenbankverschlüsselungsschlüssel alle Daten, die in das Transaktionsprotokoll geschrieben werden.

Wenn Sie einen Datenbankverschlüsselungsschlüssel zweimal ändern, müssen Sie eine Protokollsicherung anlegen, bevor Sie den Datenbankverschlüsselungsschlüssel wieder ändern können.

## <a name="tde-and-the-tempdb-system-database"></a>TDE und die tempdb-Systemdatenbank

Die **tempdb**-Systemdatenbank wird verschlüsselt, wenn eine beliebige andere Datenbank in der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Instanz mithilfe von TDE verschlüsselt wird. Diese Verschlüsselung kann sich auf die Leistung unverschlüsselter Datenbanken der gleichen [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Instanz auswirken. Weitere Informationen über die **tempdb**-Systemdatenbank finden Sie unter [tempdb-Datenbank](../../../relational-databases/databases/tempdb-database.md).

## <a name="tde-and-replication"></a>TDE und Replikation

Daten aus einer TDE-aktivierten Datenbank werden bei der Replikation nicht automatisch in einer verschlüsselten Form repliziert. Sie müssen TDE separat aktivieren, wenn Sie die Verteilungs- und Abonnentendatenbanken schützen möchten.

Die Momentaufnahmereplikation kann Daten in nicht verschlüsselten Zwischendateien wie BCP-Dateien speichern. Dies gilt auch für die erste Datenverteilung für Transaktionsdaten und die Mergereplikation. Während einer solchen Replikation können Sie die Verschlüsselung aktivieren, um den Kommunikationskanal zu schützen.

Weitere Informationen finden Sie unter [Aktivieren von verschlüsselten Verbindungen zur Datenbank-Engine (SQL Server-Konfigurations-Manager)](../../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).

## <a name="tde-and-always-on"></a>TDE und Always On    
Sie können [einer Always On-Verfügbarkeitsgruppe eine verschlüsselte Datenbank hinzufügen](../../../database-engine/availability-groups/windows/encrypted-databases-with-always-on-availability-groups-sql-server.md).  

Erstellen Sie den Hauptschlüssel und Zertifikate oder einen asymmetrischen Schlüssel (EKM) auf allen sekundären Replikaten, um die Datenbanken zu verschlüsseln, die Mitglied einer Verfügbarkeitsgruppe sind, bevor Sie den [Datenbankverschlüsselungsschlüssel](../../../t-sql/statements/create-database-encryption-key-transact-sql.md) auf dem primären Replikat erstellen.  

Wenn zum Schutz des Datenbankverschlüsselungsschlüssels (DEK) ein Zertifikat verwendet wird, [sichern Sie das auf dem primären Replikat erstellte Zertifikat](../../../t-sql/statements/backup-certificate-transact-sql.md), und [erstellen Sie dann das Zertifikat aus einer Datei](../../../t-sql/statements/create-certificate-transact-sql.md) auf allen sekundären Replikaten, bevor Sie den Datenbankverschlüsselungsschlüssel auf dem primären Replikat erstellen. 

## <a name="tde-and-filestream-data"></a>TDE und FILESTREAM-Daten

**FILESTREAM**-Daten werden nicht verschlüsselt, selbst wenn Sie TDE aktivieren.

<a name="scan-suspend-resume"></a>

## <a name="remove-tde"></a>Entfernen von TDE

Mithilfe der `ALTER DATABASE`-Anweisung können Sie die Verschlüsselung einer Datenbank entfernen.

```sql
ALTER DATABASE <db_name> SET ENCRYPTION OFF;
```

Der Status der Datenbank wird mit der dynamischen Verwaltungssicht [sys.dm_database_encryption_keys](../../../relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql.md) angezeigt.

Warten Sie, bis die Entschlüsselung abgeschlossen ist, bevor Sie den Datenbankverschlüsselungsschlüssel mithilfe von [DROP DATABASE ENCRYPTION KEY](../../../t-sql/statements/drop-database-encryption-key-transact-sql.md) entfernen.

> [!IMPORTANT]
> Sichern Sie Hauptschlüssel und Zertifikat, die für TDE verwendet werden, an einem sicheren Speicherort. Der Hauptschlüssel und das Zertifikat sind erforderlich, um Sicherungen wiederherzustellen, die erstellt wurden, solange die Datenbank mit TDE verschlüsselt war. Nach Entfernung des Datenbankverschlüsselungsschlüssels erstellen Sie eine Protokollsicherung und dann eine neue vollständige Sicherung der verschlüsselten Datenbank. 

## <a name="tde-and-buffer-pool-extension"></a>TDE und Pufferpoolerweiterung

Wenn Sie eine Datenbank mit TDE verschlüsseln, werden Dateien, die mit der Pufferpoolerweiterung zusammenhängen, nicht verschlüsselt. Verwenden Sie für diese Dateien Verschlüsselungstools wie BitLocker oder EFS auf Dateisystemebene.

## <a name="tde-and-in-memory-oltp"></a>TDE und In-Memory-OLTP

Sie können TDE für eine Datenbank aktivieren, die über In-Memory OLTP-Objekte verfügt. In-Memory-OLTP-Protokolldatensätze in [!INCLUDE[sssql16-md](../../../includes/sssql16-md.md)] und [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] werden verschlüsselt, wenn Sie TDE aktivieren. In-Memory-OLTP-Protokolldatensätze in [!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)] werden verschlüsselt, wenn Sie TDE aktivieren, Dateien in der MEMORY_OPTIMIZED_DATA-Dateigruppe werden jedoch nicht verschlüsselt.

## <a name="related-tasks"></a>Zugehörige Aufgaben

[Verschieben einer TDE-geschützten Datenbank auf einen anderen SQL-Server](../../../relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server.md)  
[Aktivieren von TDE in SQL Server mithilfe von EKM](../../../relational-databases/security/encryption/enable-tde-on-sql-server-using-ekm.md)  
[Erweiterbare Schlüsselverwaltung mit Azure Key Vault (SQL Server)](../../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)  

## <a name="related-content"></a>Verwandte Inhalte

[Transparent Data Encryption mit Azure SQL-Datenbank](/azure/azure-sql/database/transparent-data-encryption-tde-overview)  
[Erste Schritte mit Transparent Data Encryption (TDE) in Azure Synapse Analytics](/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-encryption-tde-tsql)  
[SQL Server-Verschlüsselung](../../../relational-databases/security/encryption/sql-server-encryption.md)  
[Verschlüsselungsschlüssel für SQL Server und SQL-Datenbank (Datenbank-Engine)](../../../relational-databases/security/encryption/sql-server-and-database-encryption-keys-database-engine.md)  

## <a name="see-also"></a>Weitere Informationen

[Sicherheitscenter für SQL Server-Datenbank-Engine und Azure SQL-Datenbank](../../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)  
[FILESTREAM (SQL Server)](../../../relational-databases/blob/filestream-sql-server.md)