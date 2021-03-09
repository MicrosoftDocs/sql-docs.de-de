---
description: sys.database_mirroring (Transact-SQL)
title: sys.database_mirroring (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.database_mirroring
- database_mirroring
- sys.database_mirroring_TSQL
- database_mirroring_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.database_mirroring catalog view
ms.assetid: 480de2b0-2c16-497d-a6a3-bf7f52a7c9a0
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: eeee0e1a401bfd1501249db9d2cda07e0e5648e9
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464838"
---
# <a name="sysdatabase_mirroring-transact-sql"></a>sys.database_mirroring (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Enthält eine Zeile für jede Datenbank in der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz. Wenn die Datenbank nicht ONLINE ist oder die Datenbankspiegelung nicht aktiviert ist, weisen alle Spalten, ausgenommen database_id, den Wert NULL auf.  
  
 Um die Zeile für eine andere Datenbank als die master- oder die tempdb-Datenbank anzeigen zu können, müssen Sie entweder der Datenbankbesitzer sein oder mindestens über die ALTER ANY DATABASE- oder die VIEW ANY DATABASE-Berechtigung auf Serverebene verfügen oder über die CREATE DATABASE-Berechtigung in der master-Datenbank verfügen. Um Werte ungleich NULL für eine Spiegel Datenbank anzuzeigen, müssen Sie ein Mitglied der festen Server Rolle **sysadmin** sein.  
  
> [!NOTE]  
>  Wenn eine Datenbank nicht an der Spiegelung beteiligt ist, enthalten alle Spalten mit dem Präfix "mirroring_" den Wert NULL.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|Die ID der Datenbank. Ist innerhalb einer Instanz von eindeutig [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**mirroring_guid**|**uniqueidentifier**|ID der Spiegelungspartnerschaft.<br /><br /> NULL = Datenbank ist nicht verfügbar oder wird nicht gespiegelt.<br /><br /> Hinweis: Wenn die Datenbank nicht an der Spiegelung beteiligt ist, sind alle Spalten mit dem Präfix "mirroring_" NULL.|  
|**mirroring_state**|**tinyint**|Status der Spiegeldatenbank und der Datenbank-Spiegelungssitzung.<br /><br /> 0 = angehalten<br /><br /> 1 = Getrennt vom anderen Partner<br /><br /> 2 = Wird synchronisiert<br /><br /> 3 = Ausstehendes Failover<br /><br /> 4 = Synchronisiert<br /><br /> 5 = Die Partner sind nicht synchronisiert. Failover ist jetzt nicht möglich.<br /><br /> 6 = Die Partner sind synchronisiert. Failover ist eventuell möglich. Informationen zu den Anforderungen für Failover finden Sie unter [Betriebsmodi für die Daten Bank Spiegelung](../../database-engine/database-mirroring/database-mirroring-operating-modes.md).<br /><br /> NULL = Auf die Datenbank kann nicht zugegriffen werden, oder die Datenbank wird nicht gespiegelt.|  
|**mirroring_state_desc**|**nvarchar(60)**|Beschreibung des Status der Spiegeldatenbank und der Datenbankspiegelungssitzung. Folgende Werte sind möglich:<br /><br /> DISCONNECTED<br /><br /> SYNCHRONIZED<br /><br /> SYNCHRONIZING<br /><br /> PENDING_FAILOVER<br /><br /> SUSPENDED<br /><br /> UNSYNCHRONIZED<br /><br /> SYNCHRONIZED<br /><br /> NULL<br /><br /> Weitere Informationen finden Sie unter [Spiegelungsstatus &#40;SQL Server&#41;](../../database-engine/database-mirroring/mirroring-states-sql-server.md).|  
|**mirroring_role**|**tinyint**|Aktuelle Rolle der lokalen Datenbank in der Datenbank-Spiegelungssitzung.<br /><br /> 1 = Prinzipal<br /><br /> 2 = Spiegel<br /><br /> NULL = Auf die Datenbank kann nicht zugegriffen werden, oder die Datenbank wird nicht gespiegelt.|  
|**mirroring_role_desc**|**nvarchar(60)**|Beschreibung der Rolle der lokalen Datenbank im Rahmen der Spiegelung. Folgende Werte sind möglich:<br /><br /> PRINCIPAL<br /><br /> MIRROR|  
|**mirroring_role_sequence**|**int**|Die Anzahl von Rollenwechseln zwischen den Spiegelungspartnern (Prinzipal und Spiegel) aufgrund eines Failovers oder eines erzwungenen Diensts.<br /><br /> NULL = Auf die Datenbank kann nicht zugegriffen werden, oder die Datenbank wird nicht gespiegelt.|  
|**mirroring_safety_level**|**tinyint**|Sicherheitseinstellung für Updates in der Spiegeldatenbank:<br /><br /> 0 = Unbekannter Status<br /><br /> 1 = Deaktiviert [asynchron]<br /><br /> 2 = Vollständige Sicherheit [synchron]<br /><br /> NULL = Auf die Datenbank kann nicht zugegriffen werden, oder die Datenbank wird nicht gespiegelt.|  
|**mirroring_safety_level_desc**|**nvarchar(60)**|Transaktionssicherheitseinstellung für Updates in der Spiegeldatenbank. Folgende Werte sind möglich:<br /><br /> UNKNOWN<br /><br /> OFF<br /><br /> FULL<br /><br /> NULL|  
|**mirroring_safety_sequence**|**int**|Updatesequenznummer für Änderungen an der Transaktionssicherheitsstufe.<br /><br /> NULL = Auf die Datenbank kann nicht zugegriffen werden, oder die Datenbank wird nicht gespiegelt.|  
|**mirroring_partner_name**|**nvarchar(128)**|Servername des Datenbank-Spiegelungspartners.<br /><br /> NULL = Auf die Datenbank kann nicht zugegriffen werden, oder die Datenbank wird nicht gespiegelt.|  
|**mirroring_partner_instance**|**nvarchar(128)**|Instanzname und Computername des anderen Partners. Clients benötigen diese Information, um die Verbindung mit dem Partner herstellen zu können, wenn dieser zum Prinzipalserver wird.<br /><br /> NULL = Auf die Datenbank kann nicht zugegriffen werden, oder die Datenbank wird nicht gespiegelt.|  
|**mirroring_witness_name**|**nvarchar(128)**|Servername des Datenbank-Spiegelungszeugen.<br /><br /> NULL = Es gibt keinen Zeugen.|  
|mirroring_witness_state|**tinyint**|Status des Zeugen in der Datenbankspiegelungssitzung der Datenbank. Folgende Werte sind möglich:<br /><br /> 0 = Unbekannt<br /><br /> 1 = Verbunden<br /><br /> 2 = Getrennt<br /><br /> NULL = Es ist kein Zeuge vorhanden, die Datenbank befindet sich nicht online, oder die Datenbank wird nicht gespiegelt.|  
|**mirroring_witness_state_desc**|**nvarchar(60)**|Beschreibung des Status. Folgende Werte sind möglich:<br /><br /> UNKNOWN<br /><br /> CONNECTED<br /><br /> DISCONNECTED<br /><br /> NULL|  
|**mirroring_failover_lsn**|**numeric(25,0)**|Protokollsequenznummer (Log Sequence Number, LSN) des aktuellsten Transaktionsprotokoll-Datensatzes, für den das Festschreiben auf der Festplatte auf beiden Partnern garantiert wird. Nach einem Failover wird der **mirroring_failover_lsn** von den Partnern als Punkt der Abstimmung verwendet, bei dem der neue Spiegel Server mit der Synchronisierung der neuen Spiegel Datenbank mit der neuen Prinzipal Datenbank beginnt.|  
|**mirroring_connection_timeout**|**int**|Timeout für die Spiegelungsverbindung in Sekunden. Dieser Wert entspricht der Anzahl von Sekunden, die auf eine Antwort vom Partner oder Zeugen gewartet wird, bis dieser als nicht verfügbar eingestuft wird. Standardmäßig beträgt der Timeoutwert 10 Sekunden.<br /><br /> NULL = Auf die Datenbank kann nicht zugegriffen werden, oder die Datenbank wird nicht gespiegelt.|  
|**mirroring_redo_queue**|**int**|Maximaler Umfang des Protokolls für die Wiederholung auf dem Spiegel. Wenn mirroring_redo_queue_type auf UNLIMITED festgelegt ist (Standardeinstellung), ist der Wert dieser Spalte NULL. Wenn die Datenbank nicht online ist, ist der Wert dieser Spalte ebenfalls NULL.<br /><br /> Andernfalls enthält diese Spalte den maximalen Protokollumfang in MB. Wenn das Maximum erreicht ist, wird das Protokoll auf dem Prinzipal vorübergehend angehalten, bis der Spiegelserver den aktuellen Stand erreicht hat. Durch diese Funktion wird die Failoverzeit begrenzt.<br /><br /> Weitere Informationen finden Sie unter [Einschätzen der Unterbrechung des Diensts während des Rollenwechsels &#40;Datenbankspiegelung&#41;](../../database-engine/database-mirroring/estimate-the-interruption-of-service-during-role-switching-database-mirroring.md)ausgetauscht werden.|  
|**mirroring_redo_queue_type**|**nvarchar(60)**|UNLIMITED zeigt an, dass die Wiederholungswarteschlange durch die Spiegelung nicht unterdrückt wird. Dies ist die Standardeinstellung.<br /><br /> MB zeigt an, dass die maximale Größe der Wiederholungswarteschlange (in MB) verwendet wird. Beachten Sie Folgendes: Wenn die Warteschlangengröße in KB oder GB angegeben wurde, wird der Wert vom [!INCLUDE[ssDE](../../includes/ssde-md.md)] in MB konvertiert.<br /><br /> Wenn die Datenbank nicht online ist, ist der Wert dieser Spalte NULL.|  
|**mirroring_end_of_log_lsn**|**numeric(25,0)**|Das lokale Protokollende, das auf den Datenträger geleert wurde. Dies ist vergleichbar mit der festgeschriebenen LSN vom Spiegel Server (Weitere Informationen finden Sie in der **mirroring_failover_lsn** -Spalte).|  
|**mirroring_replication_lsn**|**numeric(25,0)**|Die maximale LSN, die von der Replikation gesendet werden kann.|  
  
## <a name="permissions"></a>Berechtigungen  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Weitere Informationen finden Sie unter [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Katalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [sys.database_mirroring_witnesses &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/database-mirroring-witness-catalog-views-sys-database-mirroring-witnesses.md)   
 [sys.database_mirroring_endpoints (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md)   
 [Datenbanken und Dateikatalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/databases-and-files-catalog-views-transact-sql.md)   
 [FAQ: Abfragen des SQL Server-Systemkatalogs](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.yml)  
  
  
