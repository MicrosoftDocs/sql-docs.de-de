---
title: Als veraltet markierte Features der Datenbank-Engine in SQL Server 2017 | Microsoft-Dokumentation
titleSuffix: SQL Server 2019
description: Erfahren Sie mehr über die veralteten Features der Datenbank-Engine, die weiterhin in SQL Server 2017 (14.x) verfügbar sind, aber nicht in neuen Anwendungen verwendet werden sollten.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: release-landing
ms.topic: conceptual
helpviewer_keywords:
- deprecated features [SQL Server]
- Database Engine [SQL Server], backward compatibility
- deprecation [SQL Server], feature list
ms.assetid: ''
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017'
ms.openlocfilehash: e14f354812c99b9ed3e8be86734e3cb8e07db751
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489474"
---
# <a name="deprecated-database-engine-features-in-sql-server-2017"></a>Als veraltet markierte Funktionen der Datenbank-Engine in SQL Server 2017

[!INCLUDE[SQL Server 2017](../includes/applies-to-version/sqlserver2017.md)]

  In diesem Thema werden die als veraltet markierten Funktionen von [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] beschrieben, die in [!INCLUDE[sssql17-md](../includes/sssql17-md.md)]noch verfügbar sind. Als veraltet markierte Funktionen sollten in neuen Anwendungen nicht verwendet werden.  
  
Wenn eine Funktion als veraltet markiert ist, bedeutet dies:

- Die Funktion ist ausschließlich im Wartungsmodus. Es werden keine weiteren Änderungen vorgenommen, auch solche nicht, die mit der Interoperabilität mit neuen Funktionen zu tun haben.
- Wir bemühen uns, veraltete Funktionen in zukünftigen Versionen zu belassen, um Upgrades zu vereinfachen. In seltenen Fällen kann es jedoch vorkommen, dass eine veraltete Funktion aus [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] entfernt wird, weil sie zukünftige Innovationen beschränkt.
- Für neue Entwicklungen wird empfohlen, in diesen keine veralteten Funktionen zu verwenden.      

Sie können die Nutzung als veraltet markierter Funktionen mithilfe des [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Objektleistungsindikators "Als veraltet markierte Funktion" und Ablaufverfolgungsereignissen überwachen. Weitere Informationen finden Sie unter [Verwenden von SQL Server-Objekten](../relational-databases/performance-monitor/use-sql-server-objects.md).  

Die Werte dieser Zähler sind auch durch Ausführung der folgenden Anweisung verfügbar:  

```sql
SELECT * FROM sys.dm_os_performance_counters
WHERE object_name = 'SQLServer:Deprecated Features';
```

> [!NOTE]
> Diese Liste ist identisch mit der [!INCLUDE[sssql15-md](../includes/sssql16-md.md)]-Liste. Es gibt keine Datenbank-Engine-Funktionen, die neu für [!INCLUDE[sssql17-md](../includes/sssql17-md.md)] als veraltet markiert oder eingestellt wurden.

## <a name="features-deprecated-in-the-next-version-of-sql-server"></a>Funktionen, die ab der nächsten Version von SQL Server veraltet sind

Die folgenden Features von SQL Server-Datenbank-Engine werden in der nächsten Version von SQL Server als veraltet eingestuft. Verwenden Sie diese Funktionen nicht zum Entwickeln neuer Anwendungen, und ändern Sie so bald wie möglich die Anwendungen, in denen diese Funktionen zurzeit verwendet werden. Der **Featurename**-Wert wird in Ablaufverfolgungsereignissen als Objektname und in Leistungsindikatoren sowie `sys.dm_os_performance_counters` als Instanzname angezeigt. Der **Feature ID** -Wert wird in Ablaufverfolgungsereignissen als ObjectId angezeigt.

### <a name="back-up-and-restore"></a>Sichern und Wiederherstellen

| Als veraltet markierte Funktion | Ersatz | Feature name | Feature ID |
|--------------------|-------------|--------------|------------|
| RESTORE { DATABASE &#124; LOG } WITH [MEDIA]PASSWORD ist weiterhin veraltet.<br /><br />BACKUP { DATABASE &#124; LOG } WITH PASSWORD und BACKUP { DATABASE &#124; LOG } WITH MEDIAPASSWORD werden eingestellt. | Keine. | BACKUP DATABASE oder LOG WITH PASSWORD<br /><br />BACKUP DATABASE oder LOG WITH MEDIAPASSWORD | 104<br /><br /> 103 |

### <a name="compatibility-levels"></a>Kompatibilitätsgrade

| Als veraltet markierte Funktion | Ersatz | Feature name | Feature ID |
|--------------------|-------------|--------------|------------|
Upgrade von Version 100 (SQL Server 2008 und SQL Server 2008 R2) | Wenn die [Unterstützung](/lifecycle/products/?products=sql-server) für eine SQL Server-Version eingestellt wird, wird der zugehörige Datenbank-Kompatibilitätsgrad als veraltet markiert. Anwendungen, die für einen der unterstützten Datenbank-Kompatibilitätsgrade zertifiziert sind, werden jedoch so lange wie möglich unterstützt, um die Upgrades zu vereinfachen. Weitere Informationen zu den Kompatibilitätsgraden finden Sie unter [ALTER DATABASE-Kompatibilitätsgrad &#40;Transact-SQL&#41;](../t-sql/statements/alter-database-transact-sql-compatibility-level.md). | Datenbank-Kompatibilitätsgrad 100 | 108 |

### <a name="database-objects"></a>Datenbankobjekte

| Als veraltet markierte Funktion | Ersatz | Feature name | Feature ID |
|--------------------|-------------|--------------|------------|
| Funktionalität zum Zurückgeben von Resultsets von Triggern | Keine | Zurückgeben von Ergebnissen aus Triggern | 12 |

### <a name="encryption"></a>Verschlüsselung

| Als veraltet markierte Funktion | Ersatz | Feature name | Feature ID |
|--------------------|-------------|--------------|------------|
| Die Verschlüsselung mit RC4 oder RC4_128 ist veraltet. Die Entfernung ist für die nächste Version geplant. Die Entschlüsselung von RC4 und RC4_128 ist nicht veraltet. | Verwenden Sie einen anderen Verschlüsselungsalgorithmus, z. B. AES. | Veralteter Verschlüsselungsalgorithmus | 253 |
| Die Verwendung von MD2, MD4, MD5, SHA und SHA1 ist veraltet. | Verwenden Sie stattdessen SHA2_256 oder SHA2_512. Ältere Algorithmen funktionieren zwar weiterhin, lösen jedoch ein Ereignis aus, das auf die Veraltung hinweist. |Veralteter Hashalgorithmus | Keine |

### <a name="remote-servers"></a>Remoteserver

| Als veraltet markierte Funktion | Ersatz | Feature name | Feature ID |
|--------------------|-------------|--------------|------------|
| sp_addremotelogin<br /><br />sp_addserver <br /><br /> sp_dropremotelogin <br /><br /> sp_helpremotelogin <br /><br /> sp_remoteoption|Ersetzen Sie Remoteserver mithilfe von Verbindungsservern. sp_addserver kann nur mit der lokalen Option verwendet werden. | sp_addremotelogin<br /><br />sp_addserver <br /><br /> sp_dropremotelogin <br /><br /> sp_helpremotelogin <br /><br > sp_remoteoption | 70 <br /><br /> 69 <br /><br /> 71 <br /><br /> 72 <br /><br /> 73 |
| \@\@remserver | Ersetzen Sie Remoteserver mithilfe von Verbindungsservern. | Keine | Keine |
| SET REMOTE_PROC_TRANSACTIONS|Ersetzen Sie Remoteserver mithilfe von Verbindungsservern. | SET REMOTE_PROC_TRANSACTIONS | 110 |

### <a name="transact-sql"></a>Transact-SQL

| Als veraltet markierte Funktion | Ersatz | Feature name | Feature ID |
|--------------------|-------------|--------------|------------|
| **SET ROWCOUNT** für die **INSERT**-, **UPDATE**-Anweisung und die **DELETE** -Anweisung | TOP-Schlüsselwort | SET ROWCOUNT | 109 |
| HOLDLOCK-Tabellenhinweis ohne Klammern. | Verwenden Sie HOLDLOCK mit Klammern. | HOLDLOCK-Tabellenhinweis ohne Klammern | 167 |

## <a name="features-deprecated-in-a-future-version-of-sql-server"></a>Funktionen, die ab einer zukünftigen Version von SQL Server veraltet sind

Die folgenden Features von SQL Server-Datenbank-Engine werden in der nächsten Version von SQL Server unterstützt. Die genaue SQL Server-Version wurde jedoch noch nicht festgelegt.

### <a name="back-up-and-restore"></a>Sichern und Wiederherstellen

| Als veraltet markierte Funktion | Ersatz | Feature name |
|--------------------|-------------|--------------|
| BACKUP { DATABASE &#124; LOG } TO TAPE <br /><br /> BACKUP { DATABASE &#124; LOG } TO *Bandgerät*|BACKUP { DATABASE &#124; LOG } TO DISK <br /><br /> BACKUP { DATABASE &#124; LOG } TO *Datenträger* | BACKUP DATABASE oder LOG TO TAPE |
| sp_addumpdevice '**tape**' | sp_addumpdevice '**disk**' | ADDING TAPE DEVICE |
| sp_helpdevice | sys.backup_devices | sp_helpdevice |

### <a name="compatibility-levels"></a>Kompatibilitätsgrade

| Als veraltet markierte Funktion | Ersatz | Feature name |
|--------------------|-------------|--------------|
| sp_dbcmptlevel|ALTER DATABASE ... SET COMPATIBILITY_LEVEL. Weitere Informationen finden Sie unter [ALTER DATABASE-Kompatibilitätsgrad &#40;Transact-SQL&#41;](../t-sql/statements/alter-database-transact-sql-compatibility-level.md). | sp_dbcmptlevel |
| Datenbank-Kompatibilitätsgrad 110 und 120 | Planen Sie, in einer zukünftigen Version die Datenbank und die Anwendung zu aktualisieren. Anwendungen, die für einen der unterstützten Datenbank-Kompatibilitätsgrade zertifiziert sind, werden jedoch so lange wie möglich unterstützt, um die Upgrades zu vereinfachen. Weitere Informationen zu den Kompatibilitätsgraden finden Sie unter [ALTER DATABASE-Kompatibilitätsgrad &#40;Transact-SQL&#41;](../t-sql/statements/alter-database-transact-sql-compatibility-level.md). | Datenbank-Kompatibilitätsgrad 110 <br /><br /> Datenbank-Kompatibilitätsgrad 120 |

### <a name="collations"></a>Sortierungen

| Als veraltet markierte Funktion | Ersatz | Feature name |
|--------------------|-------------|--------------|
| Korean_Wansung_Unicode <br /><br /> Lithuanian_Classic <br /><br /> SQL_AltDiction_CP1253_CS_AS | Keine. Diese Sortierungen sind in SQL Server 2005 (9.x) vorhanden, aber nicht über fn_helpcollations sichtbar. | Korean_Wansung_Unicode <br /><br /> Lithuanian_Classic <br /><br /> SQL_AltDiction_CP1253_CS_AS |
| Hindi <br /><br /> Mazedonisch | Diese Sortierungen sind in SQL Server 2005 (9.x) und höher vorhanden, aber nicht über fn_helpcollations sichtbar. Verwenden Sie stattdessen Macedonian_FYROM_90 und Indic_General_90.|Hindi <br /><br /> Mazedonisch |
| Azeri_Latin_90 <br /><br /> Azeri_Cyrilllic_90 | Azeri_Latin_100 <br /><br /> Azeri_Cyrilllic_100 | Azeri_Latin_90 <br /><br /> Azeri_Cyrilllic_90 |

### <a name="data-types"></a>Datentypen

| Als veraltet markierte Funktion | Ersatz | Feature name |
|--------------------|-------------|--------------|
| sp_addtype <br /><br /> sp_droptype|CREATE TYPE<br /><br /> DROP TYPE | sp_addtype<br /><br /> sp_droptype |
| **timestamp** -Syntax für **rowversion** -Datentyp | **rowversion** -Datentypsyntax | timestamp |
| Fähigkeit, NULL-Werte in **timestamp** -Spalten einzufügen | Verwenden Sie stattdessen DEFAULT. | INSERT NULL in TIMESTAMP-Spalten |
| Tabellenoption 'text in row'|Verwenden Sie stattdessen die Datentypen **varchar(max)** , **nvarchar(max)** und **varbinary(max)** . Weitere Informationen finden Sie unter [sp_tableoption &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md).|Tabellenoption 'text in row' |
| Datentypen:<br /><br /> **text**<br /><br /> **ntext**<br /><br /> **image**|Verwenden Sie stattdessen die Datentypen **varchar(max)** , **nvarchar(max)** und **varbinary(max)** .|Datentypen: **text**, **ntext** oder **image** |

### <a name="database-management"></a>Datenbankverwaltung

| Als veraltet markierte Funktion | Ersatz | Feature name |
|--------------------|-------------|--------------|
| sp_attach_db <br /><br /> sp_attach_single_file_db|CREATE DATABASE-Anweisung mit der FOR ATTACH-Option. Verwenden Sie die FOR ATTACH_REBUILD_LOG-Option, um mehrere Protokolldateien neu zu erstellen, wenn mindestens eine Datei einen neuen Speicherort aufweist. | sp_attach_db <br /><br /> sp_attach_single_file_db |
| sp_certify_removable<br /><br /> sp_create_removable|sp_detach_db|sp_certify_removable<br /><br /> sp_create_removable |
| sp_dbremove | DROP DATABASE | sp_dbremove |
| sp_renamedb | MODIFY NAME in ALTER DATABASE | sp_renamedb |

### <a name="database-objects"></a>Datenbankobjekte

| Als veraltet markierte Funktion | Ersatz | Feature name |
|--------------------|-------------|--------------|
| CREATE DEFAULT<br /><br /> DROP DEFAULT<br /><br /> sp_bindefault<br /><br /> sp_unbindefault|DEFAULT-Schlüsselwort in CREATE TABLE und ALTER TABLE|CREATE_DROP_DEFAULT<br /><br /> sp_bindefault<br /><br /> sp_unbindefault |
| CREATE RULE<br /><br /> DROP RULE<br /><br /> sp_bindrule<br /><br /> sp_unbindrule | CHECK-Schlüsselwort in CREATE TABLE und ALTER TABLE | CREATE_DROP_RULE<br /><br /> sp_bindrule<br /><br /> sp_unbindrule |
| sp_change_users_login | Verwenden Sie ALTER USER. | sp_change_users_login |
| sp_depends | sys.dm_sql_referencing_entities und sys.dm_sql_referenced_entities | sp_depends |
| sp_getbindtoken | Verwenden Sie MARS oder verteilte Transaktionen. | sp_getbindtoken |

### <a name="database-options"></a>Datenbankoptionen

| Als veraltet markierte Funktion | Ersatz | Feature name |
|--------------------|-------------|--------------|
| sp_bindsession | Verwenden Sie MARS oder verteilte Transaktionen. | sp_bindsession |
| sp_resetstatus | ALTER DATABASE SET { ONLINE &#124; EMERGENCY } | sp_resetstatus
| Option TORN_PAGE_DETECTION von ALTER DATABASE|Option PAGE_VERIFY TORN_PAGE DETECTION von ALTER DATABASE | ALTER DATABASE WITH TORN_PAGE_DETECTION |

### <a name="dbcc"></a>DBCC

| Als veraltet markierte Funktion | Ersatz | Feature name |
|--------------------|-------------|--------------|
| DBCC DBREINDEX|Option REBUILD von ALTER INDEX | DBCC DBREINDEX |
| DBCC INDEXDEFRAG|Option REORGANIZE von ALTER INDEX | DBCC INDEXDEFRAG |
| DBCC SHOWCONTIG|sys.dm_db_index_physical_stats | DBCC SHOWCONTIG |
| DBCC PINTABLE<br /><br /> DBCC UNPINTABLE | Hat keinerlei Auswirkungen. | DBCC [UN]PINTABLE |

### <a name="extended-properties"></a>Erweiterte Eigenschaften

| Als veraltet markierte Funktion | Ersatz | Feature name |
|--------------------|-------------|--------------|
| Level0type = 'type' und Level0type = 'USER', um erweiterte Eigenschaften Objekten auf der ersten oder zweiten Ebene hinzuzufügen. | Verwenden Sie Level0type = 'USER' nur, um eine erweiterte Eigenschaft direkt einem Benutzer oder einer Rolle hinzuzufügen.<br /><br /> Verwenden Sie Level0type = 'SCHEMA' zum Hinzufügen einer erweiterten Eigenschaft zu Objekten auf der ersten Ebene, wie TABLE oder VIEW, bzw. zu Objekten auf der zweiten Ebene, wie COLUMN oder TRIGGER. Weitere Informationen finden Sie unter [sp_addextendedproperty &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql.md). | EXTPROP_LEVEL0TYPE<br /><br /> EXTPROP_LEVEL0USER |

### <a name="extended-stored-procedures"></a>Erweiterte gespeicherte Prozeduren

| Als veraltet markierte Funktion | Ersatz | Feature name |
|--------------------|-------------|--------------|
| xp_grantlogin<br /><br /> xp_revokelogin<br /><br /> xp_loginConfig|Verwenden Sie CREATE_LOGIN<br /><br /> Verwenden Sie DROP LOGIN und das IsIntegratedSecurityOnly-Argument von SERVERPROPERTY.|xp_grantlogin<br /><br /> xp_revokelogin<br /><br /> xp_loginConfig |

### <a name="extended-stored-procedures-programming"></a>Programmieren erweiterter gespeicherter Prozeduren

| Als veraltet markierte Funktion | Ersatz | Feature name |
|--------------------|-------------|--------------|
| srv_alloc<br /><br /> srv_convert<br /><br /> srv_describe<br /><br /> srv_getbindtoken<br /><br /> srv_got_attention<br /><br /> srv_message_handler<br /><br /> srv_paramdata<br /><br /> srv_paraminfo<br /><br /> srv_paramlen<br /><br /> srv_parammaxlen<br /><br /> srv_paramname<br /><br /> srv_paramnumber<br /><br /> srv_paramset<br /><br /> srv_paramsetoutput<br /><br /> srv_paramstatus<br /><br /> srv_paramtype<br /><br /> srv_pfield<br /><br /> srv_pfieldex<br /><br /> srv_rpcdb<br /><br /> srv_rpcname<br /><br /> srv_rpcnumber<br /><br /> srv_rpcoptions<br /><br /> srv_rpcowner<br /><br /> srv_rpcparams<br /><br /> srv_senddone<br /><br /> srv_sendmsg<br /><br /> srv_sendrow<br /><br /> srv_setcoldata<br /><br /> srv_setcollen<br /><br /> srv_setutype<br /><br /> srv_willconvert<br /><br /> srv_wsendmsg | Verwenden Sie stattdessen die CLR-Integration. | XP_API |
| sp_addextendedproc<br /><br /> sp_dropextendedproc<br /><br /> sp_helpextendedproc | Verwenden Sie stattdessen die CLR-Integration. | sp_addextendedproc<br /><br /> sp_dropextendedproc<br /><br /> sp_helpextendedproc |
| xp_grantlogin<br /><br /> xp_revokelogin<br /><br /> xp_loginConfig|Verwenden Sie CREATE_LOGIN<br /><br /> Verwenden Sie DROP LOGIN und das IsIntegratedSecurityOnly-Argument von SERVERPROPERTY. | xp_grantlogin<br /><br /> xp_revokelogin<br /><br /> xp_loginConfig |

### <a name="high-availability"></a>Hochverfügbarkeit

| Als veraltet markierte Funktion | Ersatz | Feature name |
|--------------------|-------------|--------------|
| Datenbankspiegelung | Always On-Verfügbarkeitsgruppen<br /><br /> Wenn Ihre SQL Server-Version keine Always On-Verfügbarkeitsgruppen unterstützt, verwenden Sie den Protokollversand. | DATABASE_MIRRORING |

### <a name="index-options"></a>Indexoptionen

| Als veraltet markierte Funktion | Ersatz | Feature name |
|--------------------|-------------|--------------|
| sp_indexoption | ALTER INDEX | sp_indexoption |
| CREATE TABLE-, ALTER TABLE- oder CREATE INDEX-Syntax ohne Klammern um die Optionen | Schreiben Sie Anweisung so um, dass sie die aktuelle Syntax verwendet. | INDEX_OPTION |

### <a name="instance-options"></a>Instanzoptionen

| Als veraltet markierte Funktion | Ersatz | Feature name |
|--------------------|-------------|--------------|
| sp_configure-Option 'Updates zulassen'|Systemtabellen sind nicht mehr aktualisierbar. Die Einstellung hat keine Auswirkungen. | sp_configure 'allow updates' |
| sp_configure-Optionen: <br /><br /> 'locks' <br /><br /> 'Geöffnete Objekte'<br /><br /> 'Festgelegte Workingsetgröße' | Wird jetzt automatisch konfiguriert. Die Einstellung hat keine Auswirkungen. | sp_configure 'locks'<br /><br /> sp_configure 'open objects'<br /><br /> sp_configure 'set working set size' |
| sp_configure-Option 'Prioritätserhöhung' | Systemtabellen sind nicht mehr aktualisierbar. Die Einstellung hat keine Auswirkungen. Verwenden Sie stattdessen die Windows-Option „start /high … program.exe“. | sp_configure 'priority boost' |
| sp_configure-Option 'remote proc trans' | Systemtabellen sind nicht mehr aktualisierbar. Die Einstellung hat keine Auswirkungen. | sp_configure 'remote proc trans' |

### <a name="linked-servers"></a>Verbindungsserver

| Als veraltet markierte Funktion | Ersatz | Feature name |
|--------------------|-------------|--------------|
| Angeben des SQLOLEDB-Anbieters für Verbindungsserver. | SQL Server Native Client (SQLNCLI) | SQLOLEDDB für Verbindungsserver |

### <a name="metadata"></a>Metadaten

| Als veraltet markierte Funktion | Ersatz | Feature name |
|--------------------|-------------|--------------|
| FILE_ID<br /><br /> INDEXKEY_PROPERTY | FILE_IDEX<br /><br /> sys.index_columns | FILE_ID<br /><br /> INDEXKEY_PROPERTY |

### <a name="native-xml-web-services"></a>Systemeigene XML-Webdienste

| Als veraltet markierte Funktion | Ersatz | Feature name |
|--------------------|-------------|--------------|
| Die CREATE ENDPOINT-Anweisung oder ALTER ENDPOINT-Anweisung mit der FOR SOAP-Option.<br /><br /> sys.endpoint_webmethods<br /><br /> sys.soap_endpoints|Verwenden Sie stattdessen Windows Communications Foundation (WCF) oder ASP.NET. | CREATE/ALTER ENDPOINT<br /><br /> sys.endpoint_webmethods<br /><br /> EXT_soap_endpoints<br /><br /> sys.soap_endpoints |

### <a name="other"></a>Andere

| Als veraltet markierte Funktion | Ersatz | Feature name |
|--------------------|-------------|--------------|
| DB-Library<br /><br />Embedded SQL für C|Obwohl Datenbank-Engine weiterhin Verbindungen von vorhandenen Anwendungen unterstützt, die die DB-Library- und Embedded SQL-APIs verwenden, gehören die Dateien bzw. die Dokumentationen, die zum Programmieren von Anwendungen erforderlich sind, die diese APIs verwenden, nicht mehr zum Lieferumfang. In einer der kommenden Versionen von SQL Server-Datenbank-Engine wird die Unterstützung für Verbindungen von DB-Library- oder Embedded SQL-Anwendungen eingestellt. Verwenden Sie DB-Library bzw. Embedded SQL nicht zum Entwickeln neuer Anwendungen. Entfernen Sie alle Abhängigkeiten von DB-Library bzw. Embedded SQL, wenn Sie vorhandene Anwendungen ändern. Verwenden Sie anstelle dieser APIs den SQLClient-Namespace oder eine API wie ODBC. SQL Server 2019 (15.x) enthält nicht die DB-Library-DLL, die zum Ausführen dieser Anwendungen erforderlich ist. Zum Ausführen von DB-Library- oder Embedded SQL-Anwendungen muss die DB-Library-DLL aus SQL Server-Version 6.5, 7.0 oder 2000 (8.x) verfügbar sein. | Keine |

### <a name="security"></a>Sicherheit

| Als veraltet markierte Funktion | Ersatz | Feature name |
|--------------------|-------------|--------------|
| Die ALTER LOGIN WITH SET CREDENTIAL-Syntax | Wurde durch die neue ALTER LOGIN ADD- und DROP CREDENTIAL-Syntax ersetzt | ALTER LOGIN WITH SET CREDENTIAL |
| sp_addapprole<br /><br /> sp_dropapprole | CREATE APPLICATION ROLE<br /><br /> DROP APPLICATION ROLE | sp_addapprole<br /><br /> sp_dropapprole |
| sp_addlogin<br /><br /> sp_droplogin | CREATE LOGIN<br /><br /> DROP LOGIN|sp_addlogin<br /><br /> sp_droplogin |
| sp_adduser<br /><br /> sp_dropuser | CREATE USER<br /><br /> DROP USER|sp_adduser<br /><br /> sp_dropuser |
| sp_grantdbaccess<br /><br /> sp_revokedbaccess|CREATE USER<br /><br /> DROP USER|sp_grantdbaccess<br /><br /> sp_revokedbaccess |
| sp_addrole<br /><br /> sp_droprole | CREATE ROLE<br /><br /> DROP ROLE|sp_addrole<br /><br /> sp_droprole |
| sp_approlepassword<br /><br /> sp_password | ALTER APPLICATION ROLE<br /><br /> ALTER LOGIN|sp_approlepassword<br /><br /> sp_password |
| sp_changedbowner|ALTER AUTHORIZATION | sp_changedbowner |
| sp_changeobjectowner|ALTER SCHEMA oder ALTER AUTHORIZATION | sp_changeobjectowner |
| sp_control_dbmasterkey_password | Es muss ein Hauptschlüssel vorhanden sein, und das richtige Kennwort muss angegeben werden.|sp_control_dbmasterkey_password |
| sp_defaultdb<br /><br /> sp_defaultlanguage | ALTER LOGIN|sp_defaultdb<br /><br /> sp_defaultlanguage |
| sp_denylogin<br /><br /> sp_grantlogin<br /><br /> sp_revokelogin|ALTER LOGIN DISABLE<br /><br /> CREATE LOGIN<br /><br /> DROP LOGIN|sp_denylogin<br /><br /> sp_grantlogin<br /><br /> sp_revokelogin |
| USER_ID|DATABASE_PRINCIPAL_ID | USER_ID |
| sp_srvrolepermission<br /><br /> sp_dbfixedrolepermission|Diese gespeicherten Prozeduren geben Informationen zurück, die in [!INCLUDE[ssVersion2000](../includes/ssversion2000-md.md)]richtig waren. Die Ausgabe gibt nicht die Änderungen an der Berechtigungshierarchie wieder, die in SQL Server 2008 implementiert wurde. Weitere Informationen finden Sie unter [Berechtigungen fester Serverrollen](https://msdn.microsoft.com/library/ms175892\(SQL.100\).aspx). | sp_srvrolepermission<br /><br /> sp_dbfixedrolepermission |
| GRANT ALL<br /><br /> DENY ALL<br /><br /> REVOKE ALL|GRANT-, DENY- und REVOKE-bezogene Berechtigungen|ALL-Berechtigung |
| Intrinsische PERMISSIONS-Funktion | Fragen Sie stattdessen sys.fn_my_permissions ab. | PERMISSIONS |
| SETUSER | EXECUTE AS | SETUSER |
| RC4- und DESX-Verschlüsselungsalgorithmen|Verwenden Sie einen anderen Algorithmus, z. B. AES. | DESX-Algorithmus |

### <a name="server-configuration-options"></a>Serverkonfigurationsoptionen

| Als veraltet markierte Funktion | Ersatz | Feature name |
|--------------------|-------------|--------------|
| C2-Überwachungsoption „Standardablaufverfolgung aktiviert“<br /><br /> Standardablaufverfolgung aktiviert (Option) | [Common Criteria-Kompatibilität aktiviert (Serverkonfigurationsoption)](../database-engine/configure-windows/common-criteria-compliance-enabled-server-configuration-option.md)<br /><br /> [Erweiterte Ereignisse](../relational-databases/extended-events/extended-events.md) | sp_configure 'c2 audit mode'<br /><br />sp_configure 'default trace enabled' |

### <a name="smo-classes"></a>SMO-Klassen

| Als veraltet markierte Funktion | Ersatz | Feature name |
|--------------------|-------------|--------------|
| *Microsoft.SQLServer. Management.Smo.Information*-Klasse<br /><br />*Microsoft.SQLServer. Management.Smo.Settings*-Klasse<br /><br />*Microsoft.SQLServer.Management. Smo.DatabaseOptions*-Klasse<br /><br />*Microsoft.SqlServer.Management.Smo. DatabaseDdlTrigger.NotForReplication*-Eigenschaft | *Microsoft.SQLServer.  Management.Smo.Server*-Klasse<br /><br />**Microsoft.SqlServer.  Management.Smo.Server* class<br /><br />* Microsoft.SqlServer. Management.Smo.Database*-Klasse<br /><br />Keine | Keine |

### <a name="sql-server-agent"></a>SQL Server-Agent

| Als veraltet markierte Funktion | Ersatz | Feature name |
|--------------------|-------------|--------------|
| **net send** -Benachrichtigung<br /><br />Pagerbenachrichtigung | E-Mail-Benachrichtigung<br /><br />E-Mail-Benachrichtigung | Keine |

### <a name="sql-server-management-studio"></a>SQL Server Management Studio

| Als veraltet markierte Funktion | Ersatz | Feature name |
|--------------------|-------------|--------------|
| Projektmappen-Explorer in SQL Server Management Studio | | Keine |

### <a name="system-stored-procedures-and-functions"></a>Gespeicherte Systemprozeduren und Funktionen

| Als veraltet markierte Funktion | Ersatz | Feature name |
|--------------------|-------------|--------------|
| sp_db_increased_partitions | Keine. In SQL Server 2019 (15.x) wird standardmäßig wird eine höhere Anzahl von Partitionen unterstützt. | sp_db_increased_partitions |
| fn_virtualservernodes<br /><br />fn_servershareddrives | sys.dm_os_cluster_nodes<br /><br />sys.dm_io_cluster_shared_drives | fn_virtualservernodes<br /><br /> fn_servershareddrives |
| fn_get_sql | sys.dm_exec_sql_text | fn_get_sql |
| sp_lock | sys.dm_tran_locks | sp_lock |

### <a name="system-tables"></a>Systemtabellen

| Als veraltet markierte Funktion | Ersatz | Feature name |
|--------------------|-------------|--------------|
| sysaltfiles<br /><br />syscacheobjects<br /><br />syscolumns<br /><br />syscomments<br /><br />sysconfigures<br /><br />sysconstraints<br /><br />syscurconfigs<br /><br />sysdatabases<br /><br />sysdepends<br /><br />sysdevices<br /><br />sysfilegroups<br /><br />sysfiles<br /><br />sysforeignkeys<br /><br />sysfulltextcatalogs<br /><br />sysindexes<br /><br />sysindexkeys<br /><br />syslockinfo<br /><br />syslogins<br /><br />sysmembers<br /><br />sysmessages<br /><br />sysobjects<br /><br />sysoledbusers<br /><br />sysopentapes<br /><br />sysperfinfo<br /><br />syspermissions<br /><br />sysprocesses<br /><br />sysprotects<br /><br />sysreferences<br /><br />sysremotelogins<br /><br />sysservers<br /><br />systypes<br /><br />sysusers|Kompatibilitätssichten Weitere Informationen finden Sie unter [Kompatibilitätssichten &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md).<br /><br />**Wichtig:** Die Kompatibilitätssichten machen keine Metadaten für in SQL Server 2005 (9.x) eingeführte Features verfügbar. Es wird empfohlen, die Anwendungen für die Verwendung von Katalogsichten zu aktualisieren. Weitere Informationen finden Sie unter [Katalogsichten &#40;Transact-SQL&#41;](../relational-databases/system-catalog-views/catalog-views-transact-sql.md). | sysaltfiles<br /><br />syscacheobjects<br /><br />syscolumns<br /><br />syscomments<br /><br />sysconfigures<br /><br />sysconstraints<br /><br />syscurconfigs<br /><br />sysdatabases<br /><br />sysdepends<br /><br />sysdevices<br /><br />sysfilegroups<br /><br />sysfiles<br /><br />sysforeignkeys<br /><br />sysfulltextcatalogs<br /><br />sysindexes<br /><br />sysindexkeys<br /><br />syslockinfo<br /><br />syslogins<br /><br />sysmembers<br /><br />sysmessages<br /><br />sysobjects<br /><br />sysoledbusers<br /><br />sysopentapes<br /><br />sysperfinfo<br /><br />syspermissions<br /><br />sysprocesses<br /><br />sysprotects<br /><br />sysreferences<br /><br />sysremotelogins<br /><br />sysservers<br /><br />systypes<br /><br />sysusers |
| sys.numbered_procedures<br /><br />sys.numbered_procedure_parameters | Keine | numbered_procedures<br /><br />numbered_procedure_parameters |

### <a name="sql-trace-stored-procedures-functions-and-catalog-views"></a>SQL-Ablaufverfolgung - gespeicherte Prozeduren, Funktionen und Katalogsichten

| Als veraltet markierte Funktion | Ersatz | Feature name |
|--------------------|-------------|--------------|
| sp_trace_create<br /><br />sp_trace_setevent<br /><br />sp_trace_setfilter<br /><br />sp_trace_setstatus<br /><br />fn_trace_geteventinfo<br /><br />fn_trace_getfilterinfo<br /><br />fn_trace_getinfo<br /><br />fn_trace_gettable<br /><br />sys.traces<br /><br />sys.trace_events<br /><br />sys.trace_event_bindings<br /><br />sys.trace_categories<br /><br />sys.trace_columns<br /><br />sys.trace_subclass_values|[Erweiterte Ereignisse](../relational-databases/extended-events/extended-events.md) | sp_trace_create<br /><br />sp_trace_setevent<br /><br />sp_trace_setfilter<br /><br />sp_trace_setstatus<br /><br />fn_trace_geteventinfo<br /><br />fn_trace_getfilterinfo<br /><br />fn_trace_getinfo<br /><br />fn_trace_gettable<br /><br />sys.traces<br /><br />sys.trace_events<br /><br />sys.trace_event_bindings<br /><br />sys.trace_categories<br /><br />sys.trace_columns<br /><br />sys.trace_subclass_values |

### <a name="system-views"></a>Systemsichten

| Als veraltet markierte Funktion | Ersatz | Feature name |
|--------------------|-------------|--------------|
| sys.sql_dependencies|sys.sql_expression_dependencies|sys.sql_dependencies |

### <a name="table-compression"></a>Tabellenkomprimierung

| Als veraltet markierte Funktion | Ersatz | Feature name |
|--------------------|-------------|--------------|
| Verwendung des vardecimal-Speicherformats | Das Vardecimal-Speicherformat ist veraltet. Bei der Datenkomprimierung in SQL Server 2019 (15.x) werden Dezimalwerte sowie andere Datentypen komprimiert. Es wird empfohlen, dass Sie die Datenkomprimierung statt des vardecimal-Speicherformats verwenden. | Vardecimal-Speicherformat |
| Verwenden Sie die sp_db_vardecimal_storage_format-Prozedur.|Das Vardecimal-Speicherformat ist veraltet. Bei der Datenkomprimierung in SQL Server 2019 (15.x) werden Dezimalwerte sowie andere Datentypen komprimiert. Es wird empfohlen, dass Sie die Datenkomprimierung statt des vardecimal-Speicherformats verwenden. | sp_db_vardecimal_storage_format |
| sp_estimated_rowsize_reduction_for_vardecimal (Transact-SQL)|Verwenden Sie stattdessen die Datenkomprimierung und die sp_estimate_data_compression_savings-Prozedur. |sp_estimated_rowsize_reduction_for_vardecimal |

### <a name="text-pointers"></a>Textzeiger

| Als veraltet markierte Funktion | Ersatz | Feature name |
|--------------------|-------------|--------------|
| WRITETEXT<br /><br />UPDATETEXT<br /><br />READTEXT|Keine|UPDATETEXT oder WRITETEXT<br /><br />READTEXT |
| TEXTPTR()<br /><br />TEXTVALID() | Keine | TEXTPTR<br /><br />TEXTVALID |

### <a name="transact-sql"></a>Transact-SQL

| Als veraltet markierte Funktion | Ersatz | Feature name |
|--------------------|-------------|--------------|
| :: Funktionsaufrufsequenz | Ersetzt durch SELECT *Spaltenliste* FROM sys.\<*function_name*>().<br /><br />Ersetzen Sie beispielsweise `SELECT * FROM ::fn_virtualfilestats(2,1)`durch `SELECT * FROM sys.fn_virtualfilestats(2,1)`. | Funktionsaufrufsyntax '::' |
| Spaltenverweise mit 3 Teilen und 4 Teilen. | Zweiteilige Namen sind das standardkonforme Verhalten.|Mehr als zweiteiliger Spaltenname |
| Eine in Anführungszeichen eingeschlossene Zeichenfolge, die als Spaltenalias für einen Ausdruck in einer SELECT-Liste verwendet wird:<br /><br />'*Zeichenfolgenalias*' = *Ausdruck* | *Ausdruck* [AS] *Spaltenalias*<br /><br />*Ausdruck* [AS] [*Spaltenalias*]<br /><br />*Ausdruck* [AS] "*Spaltenalias*"<br /><br />*Ausdruck* [AS] '*Spaltenalias*'<br /><br />*Spaltenalias* = *Ausdruck* | Zeichenfolgenliterale als Spaltenaliase |
| Nummerierte Prozeduren | Keine. Darf nicht verwendet werden. | ProcNums |
| Syntax für *Tabellenname.Indexname* in DROP INDEX|Syntax für *Indexname* ON *Tabellenname* in DROP INDEX.|DROP INDEX mit zweiteiligem Namen |
| Nicht auf ein Semikolon endende Transact-SQL-Anweisungen|Transact-SQL-Anweisungen müssen mit einem Semikolon ( ; ) beendet werden. | Keine |
| GROUP BY ALL|Verwenden Sie eine benutzerdefinierte einzelfallspezifische Lösung mit UNION oder abgeleiteter Tabelle. | GROUP BY ALL |
| ROWGUIDCOL als Spaltenname in DML-Anweisungen|Verwenden Sie $rowguid.|ROWGUIDCOL |
| IDENTITYCOL als Spaltenname in DML-Anweisungen|Verwenden Sie $identity.|IDENTITYCOL |
| Verwendung von #, ## als Name für eine temporäre Tabelle oder eine temporäre gespeicherte Prozedur | Verwenden Sie mindestens ein zusätzliches Zeichen.|'#' und '##' als Namen von temporären Tabellen und gespeicherten Prozeduren
| Verwendung von \@, \@\@ oder \@\@ als Transact-SQL-Bezeichner | \@, \@\@ oder Namen, die mit \@\@ beginnen, dürfen nicht als Bezeichner verwendet werden. | '\@' und Namen, die mit '\@\@' beginnen, als Transact-SQL-Bezeichner |
| Verwendung des DEFAULT-Schlüsselworts als Standardwert.|Verwenden Sie das Wort DEFAULT nicht als Standardwert. | DEFAULT-Schlüsselwort als Standardwert |
| Verwendung eines Leerzeichens als Trennzeichen zwischen Tabellenhinweisen|Trennen Sie die Tabellenhinweise durch Kommas. | Mehrere Tabellenhinweise ohne Komma |
| Die Auswahlliste einer indizierten Aggregatsicht muss im Kompatibilitätsmodus 90 COUNT_BIG (\*) enthalten. | Verwenden Sie COUNT_BIG(\*). | Indexsicht-Auswahlliste ohne COUNT_BIG(\*) |
| Das indirekte Anwenden von Tabellenhinweisen auf einen Aufruf einer Tabellenwertfunktion (Table Valued Function, TVF) mit mehreren Anweisungen über eine Sicht.|Keine.|Indirekte TVF-Hinweise |
| ALTER DATABASE-Syntax:<br /><br />MODIFY FILEGROUP READONLY<br /><br />MODIFY FILEGROUP READWRITE | MODIFY FILEGROUP READ_ONLY<br /><br />MODIFY FILEGROUP READ_WRITE | MODIFY FILEGROUP READONLY<br /><br />MODIFY FILEGROUP READWRITE |
| SET ANSI_NULLS OFF und ANSI_NULLS OFF-Datenbankoption<br /><br />SET ANSI_PADDING OFF und ANSI_PADDING OFF-Datenbankoption<br /><br />SET CONCAT_NULL_YIELDS_NULL OFF und CONCAT_NULL_YIELDS_NULL OFF-Datenbankoption<br /><br />SET OFFSETS | Keine. <br /><br /> ANSI_NULLS, ANSI_PADDING und CONCAT_NULLS_YIELDS_NULL werden stets auf ON festgelegt. SET OFFSETS ist nicht verfügbar. | SET ANSI_NULLS OFF <br /><br /> SET ANSI_PADDING OFF<br /><br />SET CONCAT_NULL_YIELDS_NULL OFF<br /><br />SET OFFSETS<br /><br />ALTER DATABASE SET ANSI_NULLS OFF<br /><br />ALTER DATABASE SET ANSI_PADDING OFF <br /><br /> ALTER DATABASE SET CONCAT_NULL_YIELDS_NULL OFF |
| SET FMTONLY | [sys.dm_exec_describe_first_result_set &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-transact-sql.md), [sys.dm_exec_describe_first_result_set_for_object &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-for-object-transact-sql.md), [sp_describe_first_result_set &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md) und [sp_describe_undeclared_parameters &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-describe-undeclared-parameters-transact-sql.md). | SET FMTONLY |
| Angeben von NOLOCK oder READUNCOMMITTED in der FROM-Klausel einer UPDATE- oder DELETE-Anweisung | Entfernen Sie die NOLOCK- oder READUNCOMMITTED-Tabellenhinweise aus der FROM-Klausel. | NOLOCK oder READUNCOMMITTED in UPDATE oder DELETE |
| Angeben von Tabellenhinweisen ohne das WITH-Schlüsselwort | Verwenden Sie WITH. | Tabellenhinweis ohne WITH |
| INSERT_HINTS | | INSERT_HINTS |

### <a name="tools"></a>Tools

| Als veraltet markierte Funktion | Ersatz | Feature name |
|--------------------|-------------|--------------|
| SQL Server Profiler für die Ablaufverfolgungssammlung | Verwenden Sie den in SQL Server Management Studio eingebetteten Profiler für erweiterte Ereignisse.|SQL Server Profiler |
| SQL Server Profiler für die Ablaufverfolgungswiedergabe | [SQL Server Distributed Replay](../tools/distributed-replay/sql-server-distributed-replay.md) |

### <a name="trace-management-objects"></a>Verwaltungsobjekte für die Ablaufverfolgung

| Als veraltet markierte Funktion | Ersatz | Feature name |
|--------------------|-------------|--------------|
| Microsoft.SqlServer.Management.Trace-Namespace (enthält die APIs für die SQL Server-Ablaufverfolgungsobjekte und -Wiedergabeobjekte)|Ablaufverfolgungskonfiguration: <xref:Microsoft.SqlServer.Management.XEvent><br /><br />Ablaufverfolgungslesevorgänge: <xref:Microsoft.SqlServer.XEvent.Linq><br /><br />Ablaufverfolgungswiedergabe: Keine | |

### <a name="xml"></a>XML

| Als veraltet markierte Funktion | Ersatz | Feature name |
|--------------------|-------------|--------------|
| XDR-Inlineschemagenerierung | Die XMLDATA-Direktive zur FOR XML-Option ist veraltet. Verwenden Sie XSD-Generierung für RAW- und AUTO-Modus. Es gibt keinen Ersatz für die XMLDATA-Direktive im EXPLICIT-Modus. | XMLDATA |

> [!NOTE]
> Der **OUTPUT** -Cookieparameter für **sp_setapprole** ist zurzeit als **varbinary(8000)** dokumentiert, was der korrekten maximalen Länge entspricht. Die aktuelle Implementierung gibt jedoch **varbinary(50)** zurück. Wenn Entwickler **varbinary(50)** zugeordnet haben, erfordert die Anwendung möglicherweise Änderungen, wenn die Cookierückgabegröße in einer zukünftigen Version steigt. Obwohl es sich nicht um ein Veraltungsproblem handelt, wird dies in diesem Thema erwähnt, da die Anwendungsanpassungen ähnlich sind. Weitere Informationen finden Sie unter [sp_setapprole &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-setapprole-transact-sql.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Nicht mehr unterstützte Datenbank-Engine-Funktionalität in SQL Server 2016](./discontinued-database-engine-functionality-in-sql-server.md)  
