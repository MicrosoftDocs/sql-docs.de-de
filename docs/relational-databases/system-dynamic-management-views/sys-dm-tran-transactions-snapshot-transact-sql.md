---
description: sys.dm_tran_transactions_snapshot (Transact-SQL)
title: sys.dm_tran_transactions_snapshot (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_tran_transactions_snapshot
- dm_tran_transactions_snapshot
- sys.dm_tran_transactions_snapshot_TSQL
- dm_tran_transactions_snapshot_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_tran_transactions_snapshot dynamic management view
ms.assetid: 03f64883-07ad-4092-8be0-31973348c647
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 236a53de756755b16c47b36e316ffc873f4fb2e7
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100347641"
---
# <a name="sysdm_tran_transactions_snapshot-transact-sql"></a>sys.dm_tran_transactions_snapshot (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Gibt eine virtuelle Tabelle für die **sequence_number** von Transaktionen zurück, die beim Starten der einzelnen Momentaufnahmetransaktionen aktiv sind. Die von dieser Sicht zurückgegebenen Informationen können Ihnen bei folgenden Aufgaben helfen:  
  
-   Finden der Anzahl derzeit aktiver Momentaufnahmetransaktionen.  
  
-   Identifizieren von Datenänderungen, die von einer bestimmten Momentaufnahmetransaktion ignoriert werden. Für eine Transaktion, die beim Start einer Momentaufnahmetransaktion aktiviert ist, werden alle Datenänderungen durch diese Transaktion, selbst nach dem Commit dieser Transaktion, von der Momentaufnahmetransaktion ignoriert.  
  
 Betrachten Sie beispielsweise die folgende Ausgabe von **sys.dm_tran_transactions_snapshot**:  
  
```  
transaction_sequence_num snapshot_id snapshot_sequence_num  
------------------------ ----------- ---------------------  
59                       0           57  
59                       0           58  
60                       0           57  
60                       0           58  
60                       0           59  
60                       3           57  
60                       3           58  
60                       3           59  
60                       3           60  
```  
  
 Die `transaction_sequence_num` -Spalte identifiziert die Transaktionssequenznummer (XSN) der aktuellen Momentaufnahmetransaktionen. Die Ausgabe zeigt zwei Transaktionssequenznummern: `59` und `60`. Die `snapshot_sequence_num` -Spalte identifiziert die Transaktionssequenznummer der Transaktionen, die beim Start jeder Momentaufnahmetransaktion aktiviert sind.  
  
 Die Ausgabe zeigt, dass Momentaufnahmetransaktion XSN-59 startet, während zwei aktive Transaktionen, XSN-57 und XSN-58, ausgeführt werden. Wenn XSN-57 oder XSN-58 Datenänderungen ausführt, ignoriert XSN-59 die Änderungen und verwendet die Zeilenversionsverwaltung, um eine hinsichtlich der Transaktion konsistente Sicht der Datenbank bereitzustellen.  
  
 Momentaufnahmetransaktion XSN-60 ignoriert Datenänderungen, die von XSN-57 und XSN-58 sowie XSN 59 ausgeführt werden.  
  
## <a name="table-returned"></a>Zurückgegebene Tabelle  
  
|Spaltenname|Datentyp|Beschreibung|  
|-----------------|---------------|-----------------|  
|**transaction_sequence_num**|**bigint**|Transaktionssequenznummer (XSN) einer Momentaufnahmetransaktion.|  
|**snapshot_id**|**int**|Momentaufnahme-ID für jede [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisung, die unter der Snapshotoption READ COMMITTED mit Zeilenversionsverwaltung gestartet wurde. Mit diesem Wert wird eine im Hinblick auf Transaktionen konsistente Sicht der Datenbank generiert, die jede Abfrage unterstützt, die unter der Snapshotoption READ COMMITTED mit Zeilenversionsverwaltung ausgeführt wird.|  
|**snapshot_sequence_num**|**bigint**|Die Transaktionssequenznummer einer Transaktion, die beim Starten der Momentaufnahmetransaktion aktiviert war.|  
  
## <a name="permissions"></a>Berechtigungen

In [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ist die- `VIEW SERVER STATE` Berechtigung erforderlich.   
Bei den Dienst Zielen "Basic", "S0" und "S1" in SQL-Datenbank ist für Datenbanken in Pools für elastische Datenbanken das [Server Administrator](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) Konto oder das [Azure Active Directory Administrator](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) Konto erforderlich. Für alle anderen SQL-Datenbank-Dienst Ziele `VIEW DATABASE STATE` ist die Berechtigung in der Datenbank erforderlich.   
  
## <a name="remarks"></a>Bemerkungen  
 Wenn eine Momentaufnahmetransaktion gestartet wird, zeichnet [!INCLUDE[ssDE](../../includes/ssde-md.md)] alle Transaktionen auf, die zu dieser Zeit aktiv sind. **sys.dm_tran_transactions_snapshot** erfasst diese Informationen für alle derzeit aktiven Momentaufnahmetransaktionen.  
  
 Jede Transaktion wird durch eine Transaktionssequenznummer identifiziert, die zu Transaktionsbeginn zugewiesen wird. Transaktionen starten zu dem Zeitpunkt, zu dem eine BEGIN TRANSACTION- oder BEGIN WORK-Anweisung ausgeführt wird. [!INCLUDE[ssDE](../../includes/ssde-md.md)] ordnet hingegen die Transaktionssequenznummer mit der Ausführung der ersten [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisung zu, die nach der BEGIN TRANSACTION- oder BEGIN WORK-Anweisung auf Daten zugreift. Transaktionssequenznummern werden um eins erhöht.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Dynamische Verwaltungssichten und Funktionen in Verbindung mit Transaktionen &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  

