---
title: 'SQL Server: Erzwingen eines Failovers für Verfügbarkeitsgruppen'
description: Erzwingen eines Failovers für Verfügbarkeitsgruppen mit dem Clustertyp „NONE“
services: ''
author: MikeRayMSFT
ms.topic: include
ms.date: 02/05/2018
ms.author: mikeray
ms.custom: include file
ms.openlocfilehash: 6e4eb6f9cf7b58a18f42d0b99531084d7fb2cd59
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343389"
---
Jede Verfügbarkeitsgruppe hat nur ein primäres Replikat. Das primäre Replikat lässt Lese- und Schreibvorgänge zu. Sie können ein Failover ausführen, um das primäre Replikat zu ändern. In einer typischen Verfügbarkeitsgruppe automatisiert der Cluster-Manager den Failoverprozess. In einer Verfügbarkeitsgruppe mit dem Clustertyp „NONE“ erfolgt der Failovervorgang manuell.

Es gibt zwei Möglichkeiten, ein Failover für ein primäres Replikat in einer Verfügbarkeitsgruppe mit dem Clustertyp „NONE“ auszuführen:

- Manuelles Failover ohne Datenverlust
- Erzwungenes manuelles Failover mit Datenverlust


### <a name="manual-failover-without-data-loss"></a>Manuelles Failover ohne Datenverlust

Verwenden Sie diese Methode, wenn das primäre Replikat verfügbar ist. Dabei müssen Sie allerdings vorübergehend oder dauerhaft ändern, welche Instanz das primäre Replikat hostet.
Um potenzielle Datenverluste zu vermeiden, stellen Sie vor der Ausführung eines manuellen Failovers sicher, dass das sekundäre Zielreplikat aktuell ist.

So führen ein manuelles Failover ohne Datenverlust aus:

1. Verwenden Sie `SYNCHRONOUS_COMMIT` für das aktuelle primäre Replikat und das sekundäre Zielreplikat.

   ```SQL
   ALTER AVAILABILITY GROUP [AGRScale] 
        MODIFY REPLICA ON N'<node2>' 
        WITH (AVAILABILITY_MODE = SYNCHRONOUS_COMMIT);
   ```

1. Um zu ermitteln, ob für aktive Transaktionen ein Commit auf das primäre Replikat und gleichzeitig auf mindestens ein synchrones sekundäres Replikat ausgeführt wurde, führen Sie die folgende Abfrage aus:

   ```SQL
   SELECT ag.name, 
      drs.database_id, 
      drs.group_id, 
      drs.replica_id, 
      drs.synchronization_state_desc, 
      ag.sequence_number
   FROM sys.dm_hadr_database_replica_states drs, sys.availability_groups ag
   WHERE drs.group_id = ag.group_id; 
   ```

   Das sekundäre Replikat wird synchronisiert, wenn `synchronization_state_desc``SYNCHRONIZED` ist.

1. Aktualisieren Sie `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` auf 1.

   Das folgende Skript legt `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` auf „1“ für eine Verfügbarkeitsgruppe mit dem Namen `ag1` fest. Ersetzen Sie `ag1` vor der Ausführung des Skripts durch den Namen Ihrer Verfügbarkeitsgruppe:

   ```SQL
   ALTER AVAILABILITY GROUP [AGRScale] 
        SET (REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT = 1);
   ```

   Diese Einstellung stellt sicher, dass für jede aktive Transaktion ein Commit auf das primäre Replikat und auf mindestens ein synchrones sekundäres Replikat ausgeführt wurde.
   >[!NOTE]
   >Diese Einstellung ist nicht failoverspezifisch und sollte anhand der Umgebungsanforderungen festgelegt werden.

1. Schalten Sie das primäre Replikat offline, um die Rollenänderung vorzubereiten: 

   ```SQL
   ALTER AVAILABILITY GROUP [AGRScale] OFFLINE
   ```

1. Stufen Sie das sekundäre Zielreplikat auf ein primäres hoch.

   ```SQL
   ALTER AVAILABILITY GROUP AGRScale FORCE_FAILOVER_ALLOW_DATA_LOSS; 
   ```

1. Aktualisieren Sie die Rolle des alten Replikats auf `SECONDARY`, und führen Sie den folgenden Befehl auf der SQL Server-Instanz aus, die das alte primäre Replikat hostet:

   ```SQL
   ALTER AVAILABILITY GROUP [AGRScale] 
        SET (ROLE = SECONDARY); 
   ```

   > [!NOTE]
   > Verwenden Sie [DROP AVAILABILITY GROUP](../t-sql/statements/drop-availability-group-transact-sql.md) (Verfügbarkeitsgruppe löschen), um eine Verfügbarkeitsgruppe zu löschen. Führen Sie bei Verfügbarkeitsgruppen, die mit dem Clustertyp „NONE“ oder „EXTERNAL“ erstellt wurden, den Befehl für alle Replikate aus, die der Verfügbarkeitsgruppe angehören.

1. Setzen Sie die Datenverschiebung fort, und führen Sie folgenden Befehl für jede Datenbank in der Verfügbarkeitsgruppe auf der SQL Server-Instanz aus, die das primäre Replikat hostet:

   ```SQL
   ALTER DATABASE [db1]
        SET HADR RESUME
   ```

1. Erstellen Sie jeden Listener neu, den Sie für Leseskalierungszwecke erstellt haben und der nicht von einem Cluster-Manager verwaltet wird. Wenn der ursprüngliche Listener auf das alte primäre Replikat zeigt, löschen Sie ihn, und erstellen Sie ihn neu, um auf das neue primäre Replikat zu verweisen.

### <a name="forced-manual-failover-with-data-loss"></a>Erzwungenes manuelles Failover mit Datenverlust

Wenn das primäre Replikat nicht verfügbar ist und nicht sofort wieder hergestellt werden kann, müssen Sie ein Failover auf das sekundäre Replikat mit Datenverlust erzwingen. Wenn das ursprüngliche primäre Replikat jedoch nach einem Failover wieder hergestellt wird, nimmt es die primäre Rolle an. Um zu vermeiden, dass jedes Replikat einen anderen Zustand aufweist, entfernen Sie das ursprüngliche primäre Replikat nach einem erzwungenen Failover mit Datenverlust aus der Verfügbarkeitsgruppe. Wenn das ursprüngliche primäre Replikat wieder online ist, entfernen Sie die Verfügbarkeitsgruppe vollständig. 

Gehen Sie folgendermaßen vor, um ein manuelles Failover mit Datenverlust vom primären Replikat N1 zum sekundären Replikat N2 zu erzwingen: 

1. Initiieren Sie auf dem sekundären Replikat (N2) das erzwungene Failover: 

    ```SQL
    ALTER AVAILABILITY GROUP [AGRScale] FORCE_FAILOVER_ALLOW_DATA_LOSS;
    ```
    
1. Entfernen Sie auf dem neuen primären Replikat (N2) das ursprüngliche primäre Replikat (N1): 

    ```SQL
    ALTER AVAILABILITY GROUP [AGRScale]
    REMOVE REPLICA ON N'N1';
    ```
    
1. Überprüfen Sie, ob der gesamte Anwendungsdatenverkehr auf den Listener und/oder das neue primäre Replikat verweist. 
1. Wenn das ursprüngliche primäre Replikat (N1) online geschaltet wird, schalten Sie die Verfügbarkeitsgruppe „AGRScale“ auf der ursprünglichen primären Datenbank (N1) offline:

   ```SQL
   ALTER AVAILABILITY GROUP [AGRScale] OFFLINE
   ```
1. Wenn Daten oder nicht synchronisierte Änderungen vorhanden sind, behalten Sie diese Datei über Sicherungskopien oder andere Datenreplikationsoptionen bei, die Ihren Geschäftsanforderungen entsprechen.     
1. Entfernen Sie als Nächstes die Verfügbarkeitsgruppe aus der ursprünglichen primären Datenbank (N1):

    ```SQL
    DROP AVAILABILITY GROUP [AGRScale];
    ```
1. Löschen Sie die Datenbank der Verfügbarkeitsgruppe auf dem ursprünglichen primären Replikat (N1): 

    ```SQL
    USE [master]
    GO
    DROP DATABASE [AGDBRScale]
    GO
    ```
    
 1. (Optional) Sie können, wenn gewünscht, N1 jetzt wieder als neues sekundäres Replikat der Verfügbarkeitsgruppe „AGRScale“ hinzufügen.
