---
description: DBCC CLONEDATABASE (Transact-SQL)
title: DBCC CLONEDATABASE (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 04/23/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CLONEDATABASE
- CLONE DATABASE
- DBCC_CLONEDATABASE_TSQL
- DBCC CLONEDATABASE
- DBCC CLONE DATABASE
- CLONEDATABASE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- database cloning [SQL Server]
- cloning databases
- clone databases
- cloning database
- clone database
- copying databases
- copy databases
- copying database
- copy database
- NO_STATISTICS option
- NO_QUERYSTORE option
- VERIFY_CLONEDB option
- BACKUP_CLONEDB option
- database copying [SQL Server]
- database cloning [SQL Server]
- DBCC CLONEDATABASE statement
ms.assetid: ''
author: bluefooted
ms.author: pamela
manager: amitban
ms.openlocfilehash: e0ddfa333535c48d3338eb26a078a67bbf7079d8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340652"
---
# <a name="dbcc-clonedatabase-transact-sql"></a>DBCC CLONEDATABASE (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Generiert einen reinen Schemaklon einer Datenbank, um Leistungsprobleme zu untersuchen, die mit dem Abfrageoptimierer zu tun haben.

![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
DBCC CLONEDATABASE   
(  
    source_database_name
    ,  target_database_name
)
    [ WITH { [ NO_STATISTICS ] [ , NO_QUERYSTORE ] [ , VERIFY_CLONEDB | SERVICEBROKER ] [ , BACKUP_CLONEDB ] } ]     
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
*source_database_name*  
Der Name der zu kopierenden Datenbank. 
  
*target_database_name*  
Der Name der Datenbank, in die die Quelldatenbank kopiert wird. Diese Datenbank wird von DBCC CLONEDATABASE erstellt und darf noch nicht vorhanden sein. 
  
NO_STATISTICS  
Gibt an, ob Tabellen-/Indexstatistiken aus dem Klon ausgeschlossen werden müssen. Ist diese Option nicht angegeben, werden Tabellen-/Indexstatistiken automatisch eingeschlossen. Diese Option wurde mit [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 CU3 und [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 eingeführt.

NO_QUERYSTORE<br>
Gibt an, ob Abfragespeicherdaten aus dem Klon ausgeschlossen werden müssen. Ist diese Option nicht angegeben, werden Abfragespeicherdaten in den Klon kopiert, wenn der Abfragespeicher in der Quelldatenbank aktiviert ist. Diese Option wurde mit [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 eingeführt.

VERIFY_CLONEDB  
Überprüft die Konsistenz der neuen Datenbank.  Diese Option ist erforderlich, wenn die geklonte Datenbank als Produktionsdatenbank verwendet werden soll.  Wird VERIFY_CLONEDB aktiviert, werden Statistiken und Abfragespeichersammlung ebenfalls deaktiviert, sodass dies mit einem Ausführen von VERIFY_CLONEDB NO_STATISTICS, NO_QUERYSTORE identisch ist.  Diese Option wurde mit [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP3, [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 und [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU8 eingeführt.

> [!NOTE]  
> Der folgende Befehl kann dazu verwendet werden sicherzustellen, dass die geklonte Datenbank als Produktionsdatenbank verwendet werden kann: <br/>`SELECT DATABASEPROPERTYEX('clone_database_name', 'IsVerifiedClone')`


SERVICEBROKER<br>
Gibt an, ob Systemkataloge, die mit dem Service Broker in Verbindung stehen, zum Klon hinzugefügt werden sollen.  Die SERVICEBROKER-Option kann nicht zusammen mit VERIFY_CLONEDB verwendet werden.  Diese Option wurde mit [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP3, [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 und [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU8 eingeführt.

BACKUP_CLONEDB  
Erstellt und überprüft eine Sicherung der Klondatenbank.  Wird dieses Argument zusammen mit VERIFY_CLONEDB verwendet, wird die Klondatenbank überprüft, bevor die Sicherung erstellt wird.  Diese Option wurde mit [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP3, [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 und [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU8 eingeführt.
  
## <a name="remarks"></a>Hinweise
Die folgenden Überprüfungen werden von DBCC CLONEDATABASE ausgeführt. Der Befehl schlägt fehl, wenn bei einer der Überprüfungen ein Fehler auftritt.
- Die Quelldatenbank muss eine Benutzerdatenbank sein. Ein Klonen von Systemdatenbanken (master, model, msdb, tempdb, distribution database usw.) ist nicht zulässig.
- Die Quelldatenbank muss online geschaltet oder lesbar sein.
- Es darf keine Datenbank vorhanden sein, die denselben Namen hat wie die Klondatenbank.
- Der Befehl befindet sich nicht in einer Benutzertransaktion.

Wenn alle Validierungen erfolgreich waren, wird das Klonen der Quelldatenbank in folgenden Vorgängen ausgeführt:
- Es wird eine neue Zieldatenbank erstellt, die dasselbe Dateilayout wie die Quelle, aber Standarddateigrößen aus der Modelldatenbank hat.
- Es wird eine interne Momentaufnahme der Quelldatenbank erstellt.
- Die Systemmetadaten aus der Quelldatenbank werden in die Zieldatenbank kopiert.
- Das gesamte Schema für alle Objekte wird aus der Quelldatenbank in die Zieldatenbank kopiert.
- Die Statistiken für alle Indizes werden aus der Quelldatenbank in die Zieldatenbank kopiert.

> [!NOTE]  
> Eine mit DBCC CLONEDATABASE generierte neue Datenbank ist hauptsächlich für Problembehandlungs- und Diagnosezwecke vorgesehen.  Soll die geklonte Datenbank zur Verwendung als Produktionsdatenbank unterstützt werden, muss die VERIFY_CLONEDB-Option verwendet werden.

Für alle Dateien in der Zieldatenbank werden die Größe- und Vergrößerungseigenschaften aus der Modelldatenbank übernommen. Für die Dateinamen für die Zieldatenbank wird die Konvention Name_der_Quelldatei_Unterstrich_Zufallszahl verwendet. Gibt es im Zielordner bereits eine Datei, die den generierten Dateinamen hat, schlägt DBCC CLONEDATABASE fehl.

DBCC CLONEDATABASE kann keinen Klon erstellen, wenn es irgendwelche Benutzerobjekte (Tabellen, Indizes, Schemas, Rollen usw.) gibt, die in der Modelldatenbank erstellt wurden. Wenn Benutzerobjekte in der Modelldatenbank vorhanden sind, schlägt das Klonen der Datenbank mit folgender Fehlermeldung fehl:

```
Msg 2601, Level 14, State 1, Line 1
Cannot insert duplicate key row in object <system table> with unique index 'index name'. The duplicate key value is <key value>   
```

> [!IMPORTANT]
> Sind Columnstore-Indizes vorhanden, müssen Sie die Columnstore-Statistikindizes gemäß [Überlegungen, wenn Sie die Abfragen mit Columnstore-Indizes für Klondatenbanken optimieren](https://techcommunity.microsoft.com/t5/SQL-Server/Considerations-when-tuning-your-queries-with-columnstore-indexes/ba-p/385294) aktualisieren, bevor Sie den **DBCC CLONEDATABASE**-Befehl ausführen.  Ab SQL Server 2019 sind die im oben genannten Artikel beschriebenen manuellen Schritte nicht mehr erforderlich, da der Befehl **DBCC CLONEDATABASE** diese Informationen automatisch erfasst.

<a name="ctp23"></a>

## <a name="stats-blob-for-columnstore-indexes"></a>Statistikblobs für Columnstore-Indizes

In [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] erfasst `DBCC CLONEDATABASE` automatisch die Statistikblobs für Columnstore-Indizes, sodass keine manuellen Schritte erforderlich sind.`DBCC CLONEDATABASE` erstellt eine rein schemabasierte Kopie einer Datenbank, die alle Elemente enthält, die zum Behandeln von Problemen mit der Abfrageleistung erforderlich sind, ohne das Daten kopiert werden. In älteren Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wurden beim Befehl nicht die Statistiken kopiert, die für die Problembehandlung bei Columnstore-Indexabfragen erforderlich sind, weshalb manuelle Schritte zur Erfassung dieser Informationen nötig waren.

Informationen bezüglich der Datensicherheit bei geklonten Datenbanken finden Sie unter [Understanding data security in cloned databases created using DBCC CLONEDATABASE](https://techcommunity.microsoft.com/t5/SQL-Server/Understanding-data-security-in-cloned-databases-created-using/ba-p/385287).

## <a name="internal-database-snapshot"></a>Interne Datenbankmomentaufnahme
In DBCC CLONEDATABASE wird eine interne Datenbankmomentaufnahme der Quelldatenbank verwendet, um die Transaktionskonsistenz zu erreichen, die zum Ausführen des Kopiervorgangs erforderlich ist. Durch Verwenden dieser Momentaufnahme werden beim Ausführen dieser Befehle Blockierungs- und Parallelitätsprobleme verhindert. Ist das Erstellen einer Momentaufnahme nicht möglich, schlägt DBCC CLONEDATABASE fehl. 

Sperren auf Datenbankebene sind während der folgenden Schritte des Kopiervorgang wirksam:
- Überprüfen der Quelldatenbank
- Abrufen der S-Sperre für die Quelldatenbank
- Erstellen der Momentaufnahme der Quelldatenbank
- Erstellen einer Klondatenbank (eine leere Datenbank, die aus der Modelldatenbank abgeleitet wird)
- Abrufen der X-Sperre für die Klondatenbank
- Kopieren der Metadaten in die Klondatenbank
- Aufheben aller DB-Sperren

Sobald der Befehl vollständig ausgeführt wurde, wird die interne Momentaufnahme gelöscht. Die Optionen TRUSTWORTHY und DB_CHAINING sind für eine geklonte Datenbank deaktiviert. 

## <a name="supported-objects"></a>Unterstützte Objekte
Nur die folgenden Objekte können in die Zieldatenbank geklont werden. Verschlüsselte Objekte werden geklont, können in der Klondatenbank aber nicht verwendet werden. Objekte, die nicht im folgenden Abschnitt aufgeführt sind, werden im Klon nicht unterstützt: 
- APPLICATION ROLE
- AVAILABILITY GROUP
- COLUMNSTORE INDEX
- CDB
- CDC
- CLR (ab [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 CU3, [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 und höheren Versionen)
- DATABASE PROPERTIES
- DEFAULT
- FILES AND FILEGROUPS
- Volltext (ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 CU2)
- FUNCTION
- INDEX
- LOGIN
- PARTITION FUNCTION
- PARTITION SCHEME
- PROCEDURE   
> [!NOTE]   
> [!INCLUDE[tsql](../../includes/tsql-md.md)]-Prozeduren werden in allen Versionen ab [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 unterstützt. CLR-Prozeduren werden ab [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 CU3 unterstützt. Systemintern kompilierte Prozeduren werden ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 unterstützt.  

- QUERY STORE (ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1)   
> [!NOTE]   
> Abfragespeicherdaten werden nur kopiert, wenn sie in der Quelldatenbank aktiviert sind. Sollen die neuesten Runtime-Statistiken als Teil des Abfragespeichers kopiert werden, führen Sie sp_query_store_flush_db aus, um die Runtime-Statistiken in den Abfragespeicher zu verschieben, bevor DBCC CLONEDATABASE ausgeführt wird.  

- ROLE
- RULE
- SCHEMA
- SEQUENCE
- SPATIAL INDEX
- STATISTICS
- SYNONYM
- TABLE
- MEMORY OPTIMIZED TABLES (nur in [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 und höheren Versionen)
- FILESTREAM AND FILETABLE OBJECTS (ab [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2-CU3, [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 und höheren Versionen) 
- TRIGGER
- TYPE
- UPGRADED DB
- USER
- VIEW
- XML INDEX
- XML SCHEMA COLLECTION  

## <a name="permissions"></a>Berechtigungen  
Erfordert die Mitgliedschaft in der festen Serverrolle **sysadmin** .

## <a name="error-log-messages"></a>Fehlerprotokollmeldungen
Die folgenden Meldungen zeigen ein Beispiel für die Meldungen, die während eines Klonvorgangs im Fehlerprotokoll protokolliert werden:

```
2018-03-26 15:33:56.05 spid53 Database cloning for 'sourcedb' has started with target as 'sourcedb_clone'.

2018-03-26 15:33:56.46 spid53 Starting up database 'sourcedb_clone'.

2018-03-26 15:33:57.80 spid53 Setting database option TRUSTWORTHY to OFF for database 'sourcedb_clone'.

2018-03-26 15:33:57.80 spid53 Setting database option DB_CHAINING to OFF for database 'sourcedb_clone'.

2018-03-26 15:33:57.88 spid53 Starting up database 'sourcedb_clone'.

2018-03-26 15:33:57.91 spid53 Database 'sourcedb_clone' is a cloned database. A cloned database should be used for diagnostic purposes only and is not supported for use in a production environment.

2018-03-26 15:33:57.92 spid53 Database cloning for 'sourcedb' has finished. Cloned database is 'sourcedb_clone'.
```

## <a name="database-properties"></a>Datenbankeigenschaften
`DATABASEPROPERTYEX('dbname', 'IsClone')` gibt 1 zurück, wenn die Datenbank mit DBCC CLONEDATABASE generiert wurde.

`DATABASEPROPERTYEX('dbname', 'IsVerifiedClone')` gibt 1 zurück, wenn die Datenbank mit VERIFY_CLONEDB erfolgreich überprüft wurde.

## <a name="examples"></a>Beispiele  
  
### <a name="a-creating-a-clone-of-a-database-that-includes-schema-statistics-and-query-store"></a>A. Erstellen eines Klons einer Datenbank, der Schema, Statistiken und Abfragespeicher enthält 
Im folgenden Beispiel wird ein Klon der AdventureWorks-Datenbank erstellt, der Schemas, Statistiken und Abfragespeicherdaten enthält ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 und höhere Versionen)

```sql  
DBCC CLONEDATABASE (AdventureWorks, AdventureWorks_Clone);    
GO 
```  
  
### <a name="b-creating-a-schema-only-clone-of-a-database-without-statistics"></a>B. Erstellen eines reinen Schemaklons einer Datenbank ohne Statistiken 
Im folgenden Beispiel wird ein Klon der AdventureWorks-Datenbank erstellt, der keine Statistiken enthält ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 CU3 und höhere Versionen)

```sql  
DBCC CLONEDATABASE (AdventureWorks, AdventureWorks_Clone) WITH NO_STATISTICS;    
GO 
```  

### <a name="c-creating-a-schema-only-clone-of-a-database-without-statistics-and-query-store"></a>C. Erstellen eines reinen Schemaklons einer Datenbank ohne Statistiken und Abfragespeicher 
Im folgenden Beispiel wird ein Klon der AdventureWorks-Datenbank erstellt, der keine Statistiken und Abfragespeicherdaten enthält ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 und höhere Versionen)

```sql  
DBCC CLONEDATABASE (AdventureWorks, AdventureWorks_Clone) WITH NO_STATISTICS, NO_QUERYSTORE;    
GO 
```  

### <a name="d-creating-a-clone-of-a-database-that-is-verified-for-production-use"></a>D. Erstellen eines Klons einer Datenbank, der zur Verwendung als Produktionsdatenbank überprüft wird
Im folgenden Beispiel wird ein reiner Schemaklon der AdventureWorks-Datenbank ohne Statistiken und Abfragespeicherdaten erstellt, der zur Verwendung als Produktionsdatenbank überprüft wird ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 und höhere Versionen).

```sql  
DBCC CLONEDATABASE (AdventureWorks, AdventureWorks_Clone) WITH VERIFY_CLONEDB;    
GO 
```  
  
### <a name="e-creating-a-clone-of-a-database-that-is-verified-for-production-use-that-includes-a-backup-of-the-cloned-database"></a>E. Erstellen eines Klons einer Datenbank, der zur Verwendung als Produktionsdatenbank überprüft wird, und einer Sicherung der geklonten Datenbank
Im folgenden Beispiel wird ein reiner Schemaklon der AdventureWorks-Datenbank ohne Statistiken und Abfragespeicherdaten erstellt, der zur Verwendung als Produktionsdatenbank überprüft wird.  Außerdem wird eine überprüfte Sicherung der geklonten Datenbank erstellt ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 und höhere Versionen).

```sql  
DBCC CLONEDATABASE (AdventureWorks, AdventureWorks_Clone) WITH VERIFY_CLONEDB, BACKUP_CLONEDB;    
GO 
```

## <a name="see-also"></a>Weitere Informationen
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)    
[Generieren eines Skripts für die notwendigen Datenbankmetadaten, um eine reine Statistikdatenbank in SQL Server zu erstellen](https://support.microsoft.com/help/914288)   

