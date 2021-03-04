---
description: sys.dm_tran_active_snapshot_database_transactions (Transact-SQL)
title: sys.dm_tran_active_snapshot_database_transactions (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_tran_active_snapshot_database_transactions_TSQL
- dm_tran_active_snapshot_database_transactions
- sys.dm_tran_active_snapshot_database_transactions
- dm_tran_active_snapshot_database_transactions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_tran_active_snapshot_database_transactions dynamic management view
ms.assetid: 55b83f9c-da10-4e65-9846-f4ef3c0c0f36
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c70cab7ea44fc1cadd226605a3b74f38d52ce7f7
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839129"
---
# <a name="sysdm_tran_active_snapshot_database_transactions-transact-sql"></a>sys.dm_tran_active_snapshot_database_transactions (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  In einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz gibt diese dynamische Verwaltungssicht eine virtuelle Tabelle für alle aktiven Transaktionen zurück, die Zeilenversionen generieren oder potenziell auf sie zugreifen. Transaktionen für mindestens eine der folgenden Bedingungen sind enthalten:  
  
-   Wenn eine oder beide der Datenbankoptionen ALLOW_SNAPSHOT_ISOLATION und READ_COMMITTED_SNAPSHOT auf ON festgelegt sind:  
  
    -   Es gibt eine Zeile für jede Transaktion, die unter der Momentaufnahmeisolationsstufe oder der READ COMMITTED-Isolationsstufe, die die Zeilenversionsverwaltung verwendet, ausgeführt wird.  
  
    -   Es gibt eine Zeile für jede Transaktion, die bewirkt, dass eine Zeilenversion in der aktuellen Datenbank erstellt wird. Beispielsweise generiert die Transaktion eine Zeilenversion durch Aktualisieren oder Löschen einer Zeile in der aktuellen Datenbank.  
  
-   Wenn ein Trigger ausgelöst wird, gibt es eine Zeile für die Transaktion, unter der der Trigger ausgeführt wird.  
  
-   Wenn eine Onlineindizierungsprozedur ausgeführt wird, gibt es eine Zeile für die Transaktion, die den Index erstellt.  
  
-   Wenn eine MARS-Sitzung (Multiple Active Result Sets) aktiviert ist, gibt es eine Zeile für jede Transaktion, die auf Zeilenversionen zugreift.  
  
 Diese dynamische Verwaltungssicht schließt keine Systemtransaktionen ein.  
  
> [!NOTE]  
>  Um dies von oder aus aufzurufen [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , verwenden Sie den Namen **sys.dm_pdw_nodes_tran_active_snapshot_database_transactions**.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sys.dm_tran_active_snapshot_database_transactions  
```  
  
## <a name="table-returned"></a>Zurückgegebene Tabelle  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**transaction_id**|**bigint**|Eindeutige, der Transaktion zugewiesene ID. Die Transaktions-ID wird in erster Linie zum Identifizieren der Transaktion in Sperrvorgängen verwendet.|  
|**transaction_sequence_num**|**bigint**|Transaktionssequenznummer. Hierbei handelt es sich um eine eindeutige, der Transaktion beim Start zugewiesene Sequenznummer. Transaktionen, die keine Versionsdatensätze generieren und keine Momentaufnahmescans verwenden, erhalten keine Transaktionssequenznummer.|  
|**commit_sequence_num**|**bigint**|Sequenznummer, die das Ende (durch Commit oder Anhalten) der Transaktion angibt. Bei aktiven Transaktionen ist der Wert NULL.|  
|**is_snapshot**|**int**|0 = Keine Momentaufnahmeisolationstransaktion<br /><br /> 1 = Momentaufnahmeisolationstransaktion|  
|**session_id**|**int**|ID der Sitzung, die die Transaktion gestartet hat.|  
|**first_snapshot_sequence_num**|**bigint**|Niedrigste Transaktionssequenznummer der Transaktionen, die beim Erstellen einer Momentaufnahme aktiviert waren. Bei der Ausführung einer Momentaufnahmetransaktion wird eine Momentaufnahme aller zu diesem Zeitpunkt aktiven Transaktionen erstellt. Für NonSnapshot-Transaktionen wird in dieser Spalte 0 angezeigt.|  
|**max_version_chain_traversed**|**int**|Maximale Länge der Versionskette, die durchsucht wird, um die hinsichtlich der Transaktion konsistente Version zu finden.|  
|**average_version_chain_traversed**|**real**|Durchschnittliche Anzahl von Zeilenversionen in den durchsuchten Versionsketten.|  
|**elapsed_time_seconds**|**bigint**|Zeitraum, der verstrichen ist, seitdem die Transaktion ihre Transaktionssequenznummer erhalten hat.|  
|**pdw_node_id**|**int**|**Gilt für**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Der Bezeichner für den Knoten, auf dem sich diese Distribution befindet.|  
  
## <a name="permissions"></a>Berechtigungen

In [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ist die- `VIEW SERVER STATE` Berechtigung erforderlich.   
Bei den Dienst Zielen "Basic", "S0" und "S1" in SQL-Datenbank ist für Datenbanken in Pools für elastische Datenbanken das [Server Administrator](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) Konto oder das [Azure Active Directory Administrator](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) Konto erforderlich. Für alle anderen SQL-Datenbank-Dienst Ziele `VIEW DATABASE STATE` ist die Berechtigung in der Datenbank erforderlich.   

## <a name="remarks"></a>Bemerkungen  
 **sys.dm_tran_active_snapshot_database_transactions** meldet Transaktionen, denen eine Transaktions Sequenznummer (XSN) zugewiesen ist. Die XSN wird zugewiesen, wenn die Transaktion zum ersten Mal auf den Versionsspeicher zugreift. In den folgenden Beispielen wird gezeigt, wann in einer Datenbank, die für die Momentaufnahmeisolation oder die READ COMMITTED-Isolation aktiviert ist, die die Zeilenversionsverwaltung verwendet, einer Transaktion eine XSN zugewiesen wird:  
  
-   Wenn eine Transaktion unter der serialisierbaren Isolationsstufe ausgeführt wird, wird eine XSN zugewiesen, wenn die Transaktion zum ersten Mal eine Anweisung ausführt, die die Erstellung einer Zeilenversion verursacht, z. B. einen UPDATE-Vorgang.  
  
-   Wenn eine Transaktion unter der Momentaufnahmeisolation ausgeführt wird, wird eine XSN zugewiesen, wenn eine Anweisung in der Datenbearbeitungssprache (Data Manipulation Language, DML), einschließlich eines SELECT-Vorgangs, ausgeführt wird.  
  
 Transaktionssequenznummern werden bei jeder Transaktion, die in einer [!INCLUDE[ssDE](../../includes/ssde-md.md)]-Instanz gestartet wird, seriell inkrementiert.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird ein Testszenario verwendet, in dem vier gleichzeitige Transaktionen, die jeweils durch eine Transaktionssequenznummer (XSN) identifiziert werden, in einer Datenbank ausgeführt werden, für die die Optionen ALLOW_SNAPSHOT_ISOLATION und READ_COMMITTED_SNAPSHOT auf ON festgelegt sind. Die folgenden Transaktionen werden ausgeführt:  
  
-   XSN-57 ist ein Updatevorgang auf der serialisierbaren Isolationsstufe.  
  
-   XSN-58 entspricht XSN-57.  
  
-   Bei XSN-59 handelt es sich um einen SELECT-Vorgang unter der Momentaufnahmeisolation.  
  
-   XSN-60 entspricht XSN-59.  
  
 Die folgende Abfrage wird ausgeführt.  
  
```  
SELECT   
    transaction_id,  
    transaction_sequence_num,  
    commit_sequence_num,  
    is_snapshot session_id,  
    first_snapshot_sequence_num,  
    max_version_chain_traversed,  
    average_version_chain_traversed,  
    elapsed_time_seconds  
  FROM sys.dm_tran_active_snapshot_database_transactions;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
transaction_id  transaction_sequence_num  commit_sequence_num  
--------------  ------------------------  -------------------  
9295            57                        NULL  
9324            58                        NULL  
9387            59                        NULL  
9400            60                        NULL  
  
is_snapshot  session_id   first_snapshot_sequence_num  
-----------  -----------  ---------------------------  
0            54           0  
0            53           0  
1            52           57  
1            51           57  
  
max_version_chain_traversed  average_version_chain_traversed  
---------------------------  -------------------------------  
0                            0  
0                            0  
1                            1  
1                            1  
  
elapsed_time_seconds  
--------------------  
419  
397  
359  
333  
```  
  
 In den folgenden Informationen werden die Ergebnisse aus **sys.dm_tran_active_snapshot_database_transactions** ausgewertet:  
  
-   XSN-57: da diese Transaktion nicht unter der Momentaufnahme Isolation ausgeführt wird, ist der `is_snapshot` Wert und `first_snapshot_sequence_num` `0` . `transaction_sequence_num` zeigt an, dass der Transaktion eine Transaktionsnummer zugewiesen wurde, da die Datenbankoption ALLOW_SNAPSHOT_ISOLATION und/oder die Datenbankoption READ_COMMITTED_SNAPSHOT aktiviert sind (ON).  
  
-   XSN-58: Diese Transaktion wird nicht unter der Momentaufnahmeisolation ausgeführt. Es gelten die gleichen Informationen wie für XSN-57.  
  
-   XSN-59: Dies ist die erste aktive Transaktion, die unter der Momentaufnahmeisolation ausgeführt wird. Diese Transaktion liest Daten, für die vor XSN-57 ein Commit ausgeführt wird, wie dies durch `first_snapshot_sequence_num` angezeigt wird. Die Ausgabe für diese Transaktion zeigt außerdem an, dass die maximale Versionskette, die für eine Zeile durchsucht wird, `1` beträgt und dass für jede Zeile, auf die zugegriffen wird, durchschnittlich `1` Version durchsucht wurde. Dies bedeutet, dass die Transaktionen XSN-57, XSN-58 und XSN-60 keine Zeilen geändert und kein Commit ausgeführt haben.  
  
-   XSN-60: Dies ist die zweite Transaktion, die unter der Momentaufnahmeisolation ausgeführt wird. Die Ausgabe zeigt die gleichen Informationen an wie für XSN-59.  
  
## <a name="see-also"></a>Weitere Informationen  
 [SET TRANSACTION ISOLATION LEVEL &#40;Transact-SQL&#41;](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)   
 [Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Dynamische Verwaltungssichten und Funktionen in Verbindung mit Transaktionen &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)  
  
