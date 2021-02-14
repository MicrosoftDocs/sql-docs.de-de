---
description: sys.dm_hadr_availability_replica_states (Transact-SQL)
title: sys.dm_hadr_availability_replica_states (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 10/16/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_hadr_availability_replica_states
- sys.dm_hadr_availability_replica_states_TSQL
- sys.dm_hadr_availability_replica_states
- dm_hadr_availability_replica_states_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- sys.dm_hadr_availability_replica_states dynamic management view
ms.assetid: d2e678bb-51e8-4a61-b223-5c0b8d08b8b1
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 2c725e1758b1ff0197d6b4498cef6f5f54809d89
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354138"
---
# <a name="sysdm_hadr_availability_replica_states-transact-sql"></a>sys.dm_hadr_availability_replica_states (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt eine Zeile für jedes lokale Replikat und eine Zeile für jedes Remotereplikat zurück, das sich in derselben Always On-Verfügbarkeitsgruppe wie ein lokales Replikat befindet. Jede Zeile enthält Informationen zum Zustand eines angegebenen Replikats.  
  
> [!IMPORTANT]  
>  Um Informationen zu jedem Replikat in einer bestimmten Verfügbarkeits Gruppe abzurufen, Fragen Sie **sys.dm_hadr_availability_replica_states** auf der Serverinstanz ab, die das primäre Replikat hostet. Findet die Abfrage in einer Serverinstanz statt, die ein sekundäres Replikat einer Verfügbarkeitsgruppe hostet, gibt diese dynamische Verwaltungssicht nur lokale Informationen für die Verfügbarkeitsgruppe zurück.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**replica_id**|**uniqueidentifier**|Eindeutiger Bezeichner des Replikats.|  
|**group_id**|**uniqueidentifier**|Eindeutiger Bezeichner der Verfügbarkeitsgruppe.|  
|**is_local**|**bit**|Ob das Replikat lokal ist, eines von:<br /><br /> 0 = Gibt ein sekundäres Remotereplikat in einer Verfügbarkeitsgruppe an, deren primäres Replikat von der lokalen Serverinstanz gehostet wird. Dieser Wert kommt nur am primären Replikatspeicherort vor.<br /><br /> 1 = gibt ein lokales Replikat an. Auf sekundären Replikaten ist dies der einzige verfügbare Wert für die Verfügbarkeitsgruppe, zu der das Replikat gehört.|  
|**role**|**tinyint**|Aktuelle [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] Rolle eines lokalen Replikats oder eines verbundenen Remote Replikats, eines der folgenden:<br /><br /> 0 = Wird aufgelöst<br /><br /> 1 = Primär<br /><br /> 2 = Sekundär<br /><br /> Informationen über [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]-Rollen finden Sie unter [Übersicht über Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).|  
|**role_desc**|**nvarchar(60)**|Beschreibung der **Rolle**, eine der folgenden:<br /><br /> RESOLVING<br /><br /> PRIMARY<br /><br /> SECONDARY|  
|**operational_state**|**tinyint**|Aktueller Betriebsstatus des Replikats, eines der folgenden:<br /><br /> 0 = Ausstehendes Failover<br /><br /> 1 = ausstehend<br /><br /> 2 = Online<br /><br /> 3 = offline<br /><br /> 4 = fehlgeschlagen<br /><br /> 5 = Fehler, kein Quorum<br /><br /> NULL = Das Replikat ist nicht lokal.<br /><br /> Weitere Informationen finden Sie unter [Rollen und Betriebszustände](#RolesAndOperationalStates)weiter unten in diesem Thema.|  
|**Betriebs \_ Status " \_ Entsc"**|**nvarchar(60)**|Beschreibung des **Betriebs \_ Status**, eine der folgenden:<br /><br /> PENDING_FAILOVER<br /><br /> PENDING (AUSSTEHEND)<br /><br /> ONLINE<br /><br /> OFFLINE<br /><br /> FAILED<br /><br /> FAILED_NO_QUORUM<br /><br /> NULL|  
|**Wiederherstellungs \_ Zustand**|**tinyint**|Rollup der **Daten Bank \_ Status** -Spalte der dynamischen Verwaltungs Sicht [sys.dm_hadr_database_replica_states](../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md) . Im folgenden sind die möglichen Werte und ihre Beschreibungen aufgeführt.<br /><br /> 0: in Bearbeitung.  Mindestens eine verbundene Datenbank verfügt über einen anderen Daten Bank Status als Online (der **Daten Bank \_ Status** ist nicht 0).<br /><br /> 1: Online. Alle verbundenen Datenbanken haben den Daten Bank Status Online (**database_state** ist 0).<br /><br /> NULL: **is_local** = 0|  
|**recovery_health_desc**|**nvarchar(60)**|Beschreibung der **recovery_health**, eine der folgenden:<br /><br /> ONLINE_IN_PROGRESS<br /><br /> ONLINE<br /><br /> NULL|  
|**Synchronisierungs \_ Zustand**|**tinyint**|Gibt ein Rollup des Daten Bank Synchronisierungs Status (**synchronization_state**) aller verbundenen Verfügbarkeits Datenbanken (auch als *Replikate* bezeichnet) und den Verfügbarkeits Modus des Replikats (synchroner Commit oder asynchroner Commit-Modus) wieder. Der Rollup zeigt den am wenigsten fehlerfreien akkumulierten Zustand der Datenbanken auf dem Replikat an. Unten sind die möglichen Werte und ihre Beschreibungen aufgeführt.<br /><br /> 0: nicht fehlerfrei.   Mindestens eine verknüpfte Datenbank weist den Status NOT SYNCHRONIZING auf.<br /><br /> 1: teilweise fehlerfrei. Einige Replikate befinden sich nicht im Zielsynchronisierungsstatus: Replikate mit synchronem Commit sollten synchronisiert sein, und Replikate mit asynchronem Commit sollten synchronisiert werden.<br /><br /> 2: fehlerfrei. Alle Replikate befinden sich im Zielsynchronisierungsstatus: Replikate mit synchronem Commit sind synchronisiert, und Replikate mit asynchronem Commit werden synchronisiert.|  
|**synchronization_health_desc**|**nvarchar(60)**|Beschreibung der **synchronization_health**, eine der folgenden:<br /><br /> NOT_HEALTHY<br /><br /> PARTIALLY_HEALTHY<br /><br /> HEALTHY|  
|**connected_state**|**tinyint**|Gibt an, ob ein sekundäres Replikat derzeit mit dem primären Replikat Die möglichen Werte werden unten mit ihren Beschreibungen angezeigt.<br /><br /> 0: getrennt. Die Antwort eines Verfügbarkeits Replikats auf den Status "getrennt" hängt von seiner Rolle ab: Wenn ein sekundäres Replikat getrennt ist, werden die sekundären Datenbanken auf dem primären Replikat als nicht synchronisiert gekennzeichnet, was darauf wartet, dass das sekundäre Replikat erneut verbunden wird. Wenn auf einem sekundären Replikat festgestellt wird, dass die Verbindung getrennt wurde, versucht das sekundäre Replikat, die Verbindung mit dem primären Replikat<br /><br /> 1: verbunden.<br /><br /> Jedes primäre Replikat verfolgt den Verbindungsstatus für jedes sekundäre Replikat in der gleichen Verfügbarkeitsgruppe nach. Sekundäre Replikate verfolgen nur den Verbindungsstatus des primären Replikats nach.|  
|**connected_state_desc**|**nvarchar(60)**|Beschreibung der **connection_state**, eine der folgenden:<br /><br /> DISCONNECTED<br /><br /> CONNECTED|  
|**last_connect_error_number**|**int**|Die Nummer des letzten Verbindungsfehlers.|  
|**last_connect_error_description**|**nvarchar(1024)**|Der Text der **last_connect_error_number** Meldung.|  
|**last_connect_error_timestamp**|**datetime**|Datums-und Uhrzeit Zeitstempel, der angibt, wann der **last_connect_error_number** Fehler aufgetreten ist.|  
  
##  <a name="roles-and-operational-states"></a><a name="RolesAndOperationalStates"></a> Rollen und Betriebszustände  
 Die Rolle ( **Rolle**) gibt den Status eines gegebenen Verfügbarkeits Replikats und den Betriebsstatus ( **operational_state**) an, ob das Replikat für die Verarbeitung von Client Anforderungen für die gesamte Datenbank des Verfügbarkeits Replikats bereit ist. Im folgenden finden Sie eine Zusammenfassung der Betriebszustände, die für jede Rolle möglich sind: auflösen, Primär und sekundär.  
  
 **Auflösen:** Wenn sich ein Verfügbarkeits Replikat in der Rolle "auflösen" befindet, sind die möglichen Betriebszustände wie in der folgenden Tabelle dargestellt.  
  
|Betriebsstatus|BESCHREIBUNG|  
|-----------------------|-----------------|  
|PENDING_FAILOVER|Derzeit wird ein Failoverbefehl für die Verfügbarkeitsgruppe verarbeitet.|  
|OFFLINE|Alle Konfigurationsdaten für das Verfügbarkeitsreplikat wurden im WSFC-Cluster und auch in den lokalen Metadaten aktualisiert, aber in der Verfügbarkeitsgruppe fehlt derzeit ein primäres Replikat.|  
|FAILED|Beim Versuch, Informationen aus dem WSFC-Cluster abzurufen, ist ein Lesefehler aufgetreten.|  
|FAILED_NO_QUORUM|Der lokale wsfc-Knoten verfügt über kein Quorum. Dies ist ein abgeleiteter Status.|  
  
 **Primär:** Wenn ein Verfügbarkeits Replikat die primäre Rolle ausführt, ist es derzeit das primäre Replikat. Die möglichen Betriebszustände sind wie in der folgenden Tabelle dargestellt.  
  
|Betriebsstatus|BESCHREIBUNG|  
|-----------------------|-----------------|  
|PENDING (AUSSTEHEND)|Dies ist ein vorübergehender Status, aber ein primäres Replikat kann in diesem Status hangen bleiben, wenn keine Arbeitsthreads zum Verarbeiten der Anforderungen verfügbar sind.|  
|ONLINE|Die Verfügbarkeitsgruppenressource ist online, und alle Datenbankarbeitsthreads wurden abgerufen.|  
|FAILED|Das Verfügbarkeitsreplikat kann nicht aus dem WSFC-Cluster lesen oder in den WSFC-Cluster schreiben.|  
  
 **Sekundär:** Wenn ein Verfügbarkeits Replikat die sekundäre Rolle ausführt, ist es derzeit ein sekundäres Replikat. Die möglichen Betriebszustände sind wie in der folgenden Tabelle dargestellt.  
  
|Betriebsstatus|BESCHREIBUNG|  
|-----------------------|-----------------|  
|ONLINE|Das lokale sekundäre Replikat ist mit dem primären Replikat verbunden.|  
|FAILED|Das lokale sekundäre Replikat kann nicht aus dem WSFC-Cluster lesen oder in den WSFC-Cluster schreiben.|  
|NULL|Auf einem primären Replikat wird dieser Wert zurückgegeben, wenn sich die Zeile auf ein sekundäres Replikat bezieht.|  
  
## <a name="security"></a>Sicherheit  
  
### <a name="permissions"></a>Berechtigungen  
 Erfordert die VIEW SERVER STATE-Berechtigung auf dem Server.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Übersicht über AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Überwachen von Verfügbarkeitsgruppen &#40;Transact-SQL&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)  
  
  
