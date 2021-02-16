---
description: DATABASEPROPERTYEX (Transact-SQL)
title: DATABASEPROPERTYEX (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 04/23/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DATABASEPROPERTYEX
- DATABASEPROPERTYEX_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- DATABASEPROPERTYEX function
- displaying database properties
- database properties [SQL Server]
ms.assetid: 8a9e0ffb-28b5-4640-95b2-a54e3e5ad941
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ec28104c0d178c50bc468a9e9012e71c89cc21db
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2021
ms.locfileid: "100346611"
---
# <a name="databasepropertyex-transact-sql"></a>DATABASEPROPERTYEX (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Für eine angegebene Datenbank in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gibt diese Funktion die aktuelle Einstellung der angegebenen Datenbankoption oder -eigenschaft zurück.
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
DATABASEPROPERTYEX ( database , property )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
*database*  
Ein Ausdruck, der den Namen der Datenbank angibt, für die `DATABASEPROPERTYEX` Informationen über die angegebene Eigenschaft zurückgibt. *database* hat den Datentyp **nvarchar(128)** .  

Für [!INCLUDE[ssSDS](../../includes/sssds-md.md)] erfordert `DATABASEPROPERTYEX` den Namen der aktuellen Datenbank. Die Funktion gibt NULL für alle Eigenschaften zurück, wenn ein anderer Datenbankname angegeben wird.
  
*property*  
Ein Ausdruck, der den Namen der zurückzugebenden Datenbankeigenschaft angibt. *property* hat den Datentyp **varchar(128)** und unterstützt einen der Werte in dieser Tabelle:
  
> [!NOTE]  
>  Wenn die Datenbank noch nicht gestartet wurde, wird für Aufrufe von `DATABASEPROPERTYEX` NULL zurückgegeben, wenn `DATABASEPROPERTYEX` diese Werte durch direkten Datenbankzugriff abruft, statt sie aus den Metadaten abzurufen. Eine Datenbank, für die AUTO_CLOSE auf ON festgelegt ist, ist als „nicht gestartet“ definiert.  
  
|Eigenschaft|BESCHREIBUNG|Zurückgegebener Wert|  
|---|---|---|
|Sortierung|Standardsortierungsname der Datenbank|Sortierungsname<br /><br /> NULL: Die Datenbank wurde nicht gestartet.<br /><br /> Basisdatentyp: **nvarchar(128)**|  
|ComparisonStyle|Die Windows-Vergleichsart der Sortierung. Verwenden Sie die folgenden Stilwerte, um eine Bitmap für den schlussendlichen ComparisonStyle-Wert zu erstellen:<br /><br /> Groß-/Kleinschreibung ignorieren: 1<br /><br /> Akzente ignorieren: 2<br /><br /> Kana ignorieren: 65536<br /><br /> Breite ignorieren: 131072<br /><br /> <br /><br /> Der Standardwert 196.609 ist beispielsweise das Ergebnis der Kombination der Optionen Groß-/Kleinschreibung ignorieren, Kana ignorieren und Breite ignorieren.|Gibt die Vergleichsart zurück.<br /><br /> Gibt für alle binären Sortierungen 0 zurück.<br /><br /> Basisdatentyp: **int**|  
|Edition|Die Edition oder Dienstebene der Datenbank.|**Gilt für**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].<br /><br /> <br /><br /> Universell<br /><br /> Unternehmenskritisch<br /><br /> Basic<br /><br /> Standard<br /><br /> Premium<br /><br /> System (für die master-Datenbank)<br /><br /> NULL: Die Datenbank wurde nicht gestartet.<br /><br /> Basisdatentyp: **nvarchar**(64)|  
|IsAnsiNullDefault|Die Datenbank befolgt ISO-Regeln für das Zulassen von NULL-Werten.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Ungültige Eingabe<br /><br /> Basisdatentyp: **int**|  
|IsAnsiNullsEnabled|Alle Vergleiche mit einem NULL-Wert werden zu einem unbekannten Wert ausgewertet.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Ungültige Eingabe<br /><br /> Basisdatentyp: **int**|  
|IsAnsiPaddingEnabled|Zeichenfolgen werden vor dem Vergleich oder Einfügen auf dieselbe Länge aufgefüllt.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Ungültige Eingabe<br /><br /> Basisdatentyp: **int**|  
|IsAnsiWarningsEnabled|SQL Server gibt Fehler- oder Warnmeldungen aus, wenn Standardfehlerbedingungen auftreten.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Ungültige Eingabe<br /><br /> Basisdatentyp: **int**|  
|IsArithmeticAbortEnabled|Abfragen werden beendet, wenn während der Abfrageausführung ein Überlauffehler oder ein Fehler aufgrund einer Division durch Null auftritt.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Ungültige Eingabe<br /><br /> Basisdatentyp: **int**|  
|IsAutoClose|Die Datenbank wird ordnungsgemäß heruntergefahren, und Ressourcen werden freigegeben, nachdem der letzte Benutzer die Anwendung beendet hat.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Ungültige Eingabe<br /><br /> Basisdatentyp: **int**|  
|IsAutoCreateStatistics|Der Abfrageoptimierer erstellt Statistiken für einzelne Spalten nach Bedarf, um die Abfrageleistung zu verbessern.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Ungültige Eingabe<br /><br /> Basisdatentyp: **int**|  
|IsAutoCreateStatisticsIncremental|Automatisch erstellte Statistiken für einzelne Spalten sind inkrementell, falls möglich.|**Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.<br /><br /> 1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Ungültige Eingabe<br /><br /> Basisdatentyp: **int**|  
|IsAutoShrink|Datenbankdateien sind Kandidaten für das automatische periodische Verkleinern.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Ungültige Eingabe<br /><br /> Basisdatentyp: **int**|  
|IsAutoUpdateStatistics|Wenn in einer Abfrage vorhandene Statistiken verwendet werden, die möglicherweise veraltet sind, aktualisiert der Abfrageoptimierer diese Statistiken.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Ungültige Eingabe<br /><br /> Basisdatentyp: **int**|
|IsClone|Die Datenbank ist nur eine Kopie der Schemas und Statistiken einer Benutzerdatenbank, die mit DBCC CLONEDATABASE erstellt wurde. Weitere Informationen finden Sie in diesem [Microsoft Support-Artikel](https://support.microsoft.com/help/3177838).|**Gilt für**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 und höher.<br /><br /> 1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Ungültige Eingabe<br /><br /> Basisdatentyp: **int**| 
|IsCloseCursorsOnCommitEnabled|Wenn für eine Transaktion ein Commit ausgeführt wird, werden alle geöffneten Cursor geschlossen.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Ungültige Eingabe<br /><br /> Basisdatentyp: **int**|  
|IsFulltextEnabled|Die Datenbank ist für die Volltext- und semantische Indizierung aktiviert.|**Gilt für**:  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.<br /><br /> <br /><br /> 1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Ungültige Eingabe<br /><br /> Basisdatentyp: **int**<br /><br /> **Hinweis:** Der Wert dieser Eigenschaft hat jetzt keine Auswirkungen. In Benutzerdatenbanken ist die Volltextsuche standardmäßig aktiviert. In einer zukünftigen Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wird diese Eigenschaft entfernt. Verwenden Sie diese Eigenschaft nicht beim Entwickeln neuer Anwendungen, und ändern Sie Anwendungen, in denen diese Eigenschaft zurzeit verwendet wird, so bald wie möglich.|  
|IsInStandBy|Die Datenbank ist im Schreibschutzmodus online. Die Wiederherstellung des Protokolls ist zulässig.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Ungültige Eingabe<br /><br /> Basisdatentyp: **int**|  
|IsLocalCursorsDefault|Cursordeklarationen werden standardmäßig auf LOCAL festgelegt.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Ungültige Eingabe<br /><br /> Basisdatentyp: **int**|  
|IsMemoryOptimizedElevateToSnapshotEnabled|Auf speicheroptimierte Tabellen wird mit der SNAPSHOT-Isolation zugegriffen, wenn die Sitzungseinstellung TRANSACTION ISOLATION LEVEL auf READ COMMITTED, READ UNCOMMITTED oder eine niedrigere Isolationsstufe festgelegt ist.|**Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.<br /><br /> <br /><br /> 1: TRUE<br /><br /> 0: FALSE<br /><br /> Basisdatentyp: **int**|  
|IsMergePublished|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unterstützt Datenbanktabellenveröffentlichung für Mergereplikation, wenn die Replikation installiert ist.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Ungültige Eingabe<br /><br /> Basisdatentyp: **int**|  
|IsNullConcat|Der Operand für die NULL-Verkettung ergibt NULL.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Ungültige Eingabe<br /><br /> Basisdatentyp: **int**|  
|IsNumericRoundAbortEnabled|Fehler werden generiert, wenn ein Genauigkeitsverlust in Ausdrücken auftritt.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Ungültige Eingabe<br /><br /> Basisdatentyp: **int**|  
|IsParameterizationForced|Die SET-Option PARAMETERIZATION wurde für die Datenbank auf FORCED festgelegt.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Ungültige Eingabe|  
|IsQuotedIdentifiersEnabled|Für Bezeichner sind doppelte Anführungszeichen zulässig.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Ungültige Eingabe<br /><br /> Basisdatentyp: **int**|  
|IsPublished|Wenn Replikation installiert ist, unterstützt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Datenbanktabellenveröffentlichung für Momentaufnahme- oder Transaktionsreplikation.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Ungültige Eingabe<br /><br /> Basisdatentyp: **int**|  
|IsRecursiveTriggersEnabled|Das rekursive Auslösen von Triggern ist aktiviert.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Ungültige Eingabe<br /><br /> Basisdatentyp: **int**|  
|IsSubscribed|Die Datenbank wurde für eine Veröffentlichung abonniert.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Ungültige Eingabe<br /><br /> Basisdatentyp: **int**|  
|IsSyncWithBackup|Die Datenbank ist entweder eine veröffentlichte oder eine Verteilungsdatenbank, und für sie wird ein Wiederherstellen unterstützt, bei dem Transaktionsreplikation nicht unterbrochen wird.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Ungültige Eingabe<br /><br /> Basisdatentyp: **int**|  
|IsTornPageDetectionEnabled|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] erkennt unvollständige E/A-Vorgänge, die durch Stromausfälle oder andere Systemunterbrechungen verursacht wurden.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Ungültige Eingabe<br /><br /> Basisdatentyp: **int**| 
|IsVerifiedClone|Die Datenbank ist nur eine Kopie der Schemas und Statistiken einer Benutzerdatenbank, die mit der DBBC CLONEDATABASE-Option WITH VERIFY_CLONEDB erstellt wurde. Weitere Informationen finden Sie im folgenden [Microsoft Support-Artikel](https://support.microsoft.com/help/3177838).|**Gilt für**: Ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2.<br /><br /> <br /><br /> 1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Ungültige Eingabe<br /><br /> Basisdatentyp: **int**| 
|IsXTPSupported|Gibt an, ob die Datenbank In-Memory-OLTP unterstützt, d.h. das Erstellen und Verwenden von speicheroptimierten Tabellen und nativ kompilierten Modulen.<br /><br /> Spezifisch für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:<br /><br /> IsXTPSupported ist unabhängig von der Existenz von MEMORY_OPTIMIZED_DATA-Dateigruppen, die für die Erstellung von In-Memory-OLTP-Objekten benötigt werden.|**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].<br /><br /> 1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Ungültige Eingabe, ein Fehler oder nicht anwendbar<br /><br /> Basisdatentyp: **int**|  
|LastGoodCheckDbTime|Datum und Uhrzeit der letzten Ausführung von DBCC CHECKDB in der angegebenen Datenbank. <sup>1</sup> Wenn DBCC CHECKDB in einer Datenbank noch nicht ausgeführt wurde, wird 1900-01-01 00:00:00.000 zurückgegeben.|**Gilt für:** [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] ab SP2.</br>[!INCLUDE[sssql17](../../includes/sssql17-md.md)] ab CU9</br>[!INCLUDE[sssql19](../../includes/sssql19-md.md)] oder höher.</br>Azure SQL-Datenbank.<br/><br/>Ein datetime-Wert<br /><br /> NULL: Ungültige Eingabe<br /><br /> Basisdatentyp: **datetime**| 
|LCID|Der zur Sortierung verwendete Windows-Gebietsschemabezeichner (LCID, Locale Identifier).|LCID-Wert (im Dezimalformat).<br /><br /> Basisdatentyp: **int**|  
|MaxSizeInBytes|Maximale Datenbankgröße in Bytes.|**Gilt für**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].<br /><br />[Azure SQL-Datenbank und Azure Synapse Analytics](/azure/sql-database/sql-database-single-database-scale#dtu-based-purchasing-model): Der Wert basiert auf SLO, sofern kein zusätzlicher Speicher erworben wurde.<br /><br />[vCore](/azure/sql-database/sql-database-single-database-scale#vcore-based-purchasing-model): Der Wert wird in Inkrementen von 1 GB bis zur Maximalgröße erhöht.<br /><br />NULL: Die Datenbank wurde nicht gestartet.<br /><br /> Basisdatentyp: **bigint**|  
|Wiederherstellung|Datenbank-Wiederherstellungsmodell|FULL: Vollständiges Wiederherstellungsmodell<br /><br /> BULK_LOGGED: Massenprotokolliertes Modell<br /><br /> SIMPLE: Einfaches Wiederherstellungsmodell<br /><br /> Basisdatentyp: **nvarchar(128)**|  
|ServiceObjective|Beschreibt die Leistungsstufe der Datenbank in [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] oder [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].|Einer der folgenden:<br /><br /> NULL = Die Datenbank wurde nicht gestartet.<br /><br /> Freigegeben (für Web/Business-Editionen)<br /><br /> Basic<br /><br /> S0<br /><br /> S1<br /><br /> S2<br /><br /> S3<br /><br /> P1<br /><br /> P2<br /><br /> P3<br /><br /> ElasticPool<br /><br /> System (für die master-Datenbank)<br /><br /> Basisdatentyp: **nvarchar(32)**|  
|ServiceObjectiveId|Die ID des Dienstziels in [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].|**uniqueidentifier**, die das Dienstziel identifiziert.|  
|SQLSortOrder|Die ID der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Sortierreihenfolge, die in früheren Versionen von SQL Server unterstützt wurde.|0: In der Datenbank wird Windows-Sortierung verwendet.<br /><br /> >0: ID der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Sortierreihenfolge<br /><br /> NULL: Ungültige Eingabe, oder die Datenbank wurde nicht gestartet.<br /><br /> Basisdatentyp: **tinyint**|  
|Status|Der Status der Datenbank.|ONLINE: Die Datenbank ist für Abfragen verfügbar.<br /><br /> **Hinweis:** Die Funktion gibt möglicherweise den ONLINE-Status zurück, während die Datenbank geöffnet wird und noch nicht wiederhergestellt wurde. Fragen Sie die Collation-Eigenschaft von **DATABASEPROPERTYEX** ab, um zu ermitteln, ob eine ONLINE-Datenbank Verbindungen akzeptieren kann. Die ONLINE-Datenbank kann Verbindungen akzeptieren, wenn die Datenbanksortierung einen Wert ungleich NULL zurückgibt. Fragen Sie bei Always On-Datenbanken die „database_state“- oder die „database_state_desc“-Spalte von `sys.dm_hadr_database_replica_states` ab.<br /><br /> OFFLINE: Die Datenbank wurde explizit offline geschaltet.<br /><br /> RESTORING: Die Wiederherstellung der Datenbank wurde gestartet.<br /><br /> RECOVERING: Die Wiederherstellung der Datenbank wurde gestartet, aber die Datenbank ist noch nicht für Abfragen bereit.<br /><br /> SUSPECT: Die Datenbank wurde nicht wiederhergestellt.<br /><br /> EMERGENCY: Die Datenbank befindet sich im schreibgeschützten Notfallmodus. Der Zugriff ist auf sysadmin-Mitglieder beschränkt.<br /><br /> Basisdatentyp: **nvarchar(128)**|  
|Updateability|Zeigt an, ob Daten geändert werden können.|READ_ONLY: Für die Datenbank werden Datenlese- aber keine Datenänderungsvorgänge unterstützt.<br /><br /> READ_WRITE: Für die Datenbank werden Datenlese- und Datenänderungsvorgänge unterstützt.<br /><br /> Basisdatentyp: **nvarchar(128)**|  
|UserAccess|Zeigt an, welche Benutzer auf die Datenbank zugreifen können.|SINGLE_USER: Immer nur jeweils ein „db_owner“-, „dbcreator“- oder „sysadmin“-Benutzer<br /><br /> RESTRICTED_USER: Nur Mitglieder aus einer der Rollen „db_owner“, „dbcreator“ oder „sysadmin“<br /><br /> MULTI_USER: Alle Benutzer<br /><br /> Basisdatentyp: **nvarchar(128)**|  
|Version|Die interne Versionsnummer des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Codes, mit dem die Datenbank erstellt wurde. [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|Versionsnummer: Die Datenbank ist geöffnet.<br /><br /> NULL: Die Datenbank wurde nicht gestartet.<br /><br /> Basisdatentyp: **int**| 

<br/>   

> [!NOTE]  
> <sup>1</sup> Für Datenbanken, die Teil einer Verfügbarkeitsgruppe sind, gibt `LastGoodCheckDbTime` das Datum und die Uhrzeit der letzten erfolgreichen Ausführung von DBCC CHECKDB auf dem primären Replikat zurück, egal von welchem Replikat Sie den Befehl ausführen. 

## <a name="return-types"></a>Rückgabetypen
**sql_variant**
  
## <a name="exceptions"></a>Ausnahmen  
Gibt NULL bei einem Fehler zurück oder wenn ein Aufrufer nicht über Berechtigungen zum Anzeigen des Objekts verfügt.
  
In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] kann ein Benutzer nur die Metadaten sicherungsfähiger Elemente anzeigen, bei denen der Benutzer entweder der Besitzer ist oder für die dem Benutzer eine Berechtigung erteilt wurde. Dies bedeutet, dass Metadaten ausgebende integrierte Funktionen, z.B. `OBJECT_ID`, möglicherweise NULL zurückgeben, wenn der Benutzer keine Berechtigungen für das Objekt hat. Weitere Informationen finden Sie unter [Konfigurieren der Sichtbarkeit von Metadaten](../../relational-databases/security/metadata-visibility-configuration.md).
  
## <a name="remarks"></a>Bemerkungen  
`DATABASEPROPERTYEX` gibt immer nur jeweils eine Eigenschaftseinstellung zurück. Verwenden Sie die [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht, um mehrere Eigenschaftseinstellungen anzuzeigen.
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-retrieving-the-status-of-the-auto_shrink-database-option"></a>A. Abrufen des Status der Datenbankoption AUTO_SHRINK  
In diesem Beispiel wird der Status der AUTO_SHRINK-Datenbankoption für die `AdventureWorks`-Datenbank zurückgegeben.
  
```sql
SELECT DATABASEPROPERTYEX('AdventureWorks2014', 'IsAutoShrink');  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)] Dies gibt an, dass AUTO_SHRINK auf OFF festgelegt ist.
  
```
------------------  
0  
```  
  
### <a name="b-retrieving-the-default-collation-for-a-database"></a>B. Abrufen der Standardsortierung für eine Datenbank  
In diesem Beispiel werden mehrere Attribute der `AdventureWorks`-Datenbank zurückgegeben.
  
```sql
SELECT   
    DATABASEPROPERTYEX('AdventureWorks2014', 'Collation') AS Collation,  
    DATABASEPROPERTYEX('AdventureWorks2014', 'Edition') AS Edition,  
    DATABASEPROPERTYEX('AdventureWorks2014', 'ServiceObjective') AS ServiceObjective,  
    DATABASEPROPERTYEX('AdventureWorks2014', 'MaxSizeInBytes') AS MaxSizeInBytes  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Collation                     Edition        ServiceObjective  MaxSizeInBytes  
----------------------------  -------------  ----------------  --------------  
SQL_Latin1_General_CP1_CI_AS  DataWarehouse  DW1000            5368709120  
```  
  
## <a name="see-also"></a>Weitere Informationen
[ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)  
[Datenbankstatus](../../relational-databases/databases/database-states.md)  
[sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)  
[sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)  
[SERVERPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/serverproperty-transact-sql.md)
  
  
