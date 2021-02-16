---
title: Weiterleiten von Lese-/Schreibverbindungen mit primären Replikaten
description: In diesem Artikel erfahren Sie, wie Lese-/Schreibverbindungen unabhängig vom in der Verbindungszeichenfolge angegebenen Zielserver an das primäre Replikat einer Always On-Verfügbarkeitsgruppe weitergeleitet werden.
ms.custom: seo-lt-2019
ms.date: 01/09/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: article
helpviewer_keywords:
- connection access to availability replicas
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], readable secondary replicas
- active secondary replicas [SQL Server], read-only access to
- readable secondary replicas
- Availability Groups [SQL Server], active secondary replicas
ms.assetid: ''
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 5694ab7203b0285c4ef275fa3d5c3b70463b0ba5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100060019"
---
# <a name="secondary-to-primary-replica-readwrite-connection-redirection-always-on-availability-groups"></a>Umleitung von Lese-/Schreibverbindungen vom sekundären zum primären Replikat (Always On-Verfügbarkeitsgruppen)

[!INCLUDE[appliesto](../../../includes/applies-to-version/sqlserver2019.md)]

In [!INCLUDE[sssql19-md](../../../includes/sssql19-md.md)] CTP 2.0 wird eine *Umleitung von Lese-/Schreibverbindungen vom sekundären zum primären Replikat* für Always On-Verfügbarkeitsgruppen eingeführt. Die Umleitung von Lese-/Schreibverbindungen ist auf jeder Betriebssystemplattform verfügbar. Durch dieses Feature können Clientanwendungsverbindungen zum primären Replikat weitergeleitet werden, unabhängig davon, ob der Zielserver in der Verbindungszeichenfolge angegeben ist. 

In der Verbindungszeichenfolge kann beispielsweise ein sekundäres Replikat als Ziel angegeben sein. Je nach Konfiguration des Verfügbarkeitsgruppenreplikats und den Einstellungen in der Verbindungszeichenfolge kann die Verbindung automatisch an das primäre Replikat umgeleitet werden. 

## <a name="use-cases"></a>Anwendungsfälle

Vor [!INCLUDE[sssql19-md](../../../includes/sssql19-md.md)] leiten der Verfügbarkeitgruppenlistener und die entsprechende Clusterressource den Benutzerdatenverkehr an das primäre Replikat weiter, um die Verbindungswiederherstellung nach einem Failover sicherzustellen. [!INCLUDE[sssql19-md](../../../includes/sssql19-md.md)] unterstützt die Funktion des Verfügbarkeitgruppenlisteners weiterhin und fügt die Umleitung von Replikatverbindungen für Szenarien hinzu, in denen keine Listener verwendet werden kann. Beispiel:

* Die Clustertechnologie, in die SQL Server-Verfügbarkeitsgruppen integriert sind, bietet keine Funktion, die einem Listener ähnelt. 
* Eine Konfiguration mit mehreren Subnetzen wie z.B. die Cloud oder Floating IP mit Pacemaker mit mehreren Subnetzen – solche Konfigurationen können aufgrund der Menge an beteiligten Komponenten sehr komplex, fehleranfällig und schwer zu korrigieren sein.
* Horizontale Leseskalierung oder eine Notfallwiederherstellung mit Clustertyp `NONE`, da es keinen einfachen Mechanismus gibt, um eine transparente Wiederherstellung der Verbindung nach einem manuellen Failover gibt.

## <a name="requirement"></a>Anforderung

Damit ein sekundäres Replikat Lese-/Schreibverbindungsanforderungen umleiten kann, müssen folgende Voraussetzungen erfüllt sein:
* Das sekundäre Replikat muss online sein. 
* Die Replikatspezifikation `PRIMARY_ROLE` muss `READ_WRITE_ROUTING_URL` enthalten.
* Die Verbindungszeichenfolge muss auf `ReadWrite` festgelegt werden, indem `ApplicationIntent` als `ReadWrite` definiert wird oder indem `ApplicationIntent` nicht festgelegt wird, sodass standardmäßig `ReadWrite` verwendet wird.

## <a name="set-read_write_routing_url-option"></a>Festlegen der READ_WRITE_ROUTING_URL-Option

Um die Umleitung von Lese-/Schreibverbindungen zu konfigurieren, legen Sie beim Erstellen der Verfügbarkeitsgruppe `READ_WRITE_ROUTING_URL` für das primäre Replikat fest. 

In [!INCLUDE[sssql19-md](../../../includes/sssql19-md.md)] wurde `READ_WRITE_ROUTING_URL` zur `<add_replica_option>`-Spezifikation hinzugefügt. Weitere Informationen finden Sie in den folgenden Artikeln: 

* [CREATE AVAILABILITY GROUP](../../../t-sql/statements/create-availability-group-transact-sql.md)
* [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md)


### <a name="primary_roleread_write_routing_url-not-set-default"></a>PRIMARY_ROLE(READ_WRITE_ROUTING_URL) nicht festgelegt (Standardeinstellung) 

Standardmäßig ist die Umleitung von Lese-/Schreibverbindungen für ein Replikat nicht festgelegt. Wie ein sekundäres Replikat Verbindungsanforderungen behandelt, richtet sich danach, ob das Zulassen von Verbindungen für das sekundäre Replikat festgelegt ist, und nach den `ApplicationIntent`-Einstellung in der Verbindungszeichenfolge. Die folgende Tabelle zeigt, wie ein sekundäres Replikat basierend auf `SECONDARY_ROLE (ALLOW CONNECTIONS = )` und `ApplicationIntent` Verbindungen behandelt.

|Wert vom Typ <code>ApplicationIntent</code>|<code>SECONDARY_ROLE (ALLOW CONNECTIONS = NO)</code>|<code>SECONDARY_ROLE (ALLOW CONNECTIONS = READ_ONLY)</code>|<code>SECONDARY_ROLE (ALLOW CONNECTIONS = ALL)</code>|
|-----|-----|-----|-----|
|`ApplicationIntent=ReadWrite`<br/> Standard|Verbindungen werden nicht hergestellt|Verbindungen werden nicht hergestellt|Verbindungen werden erfolgreich hergestellt<br/>Lesevorgänge werden erfolgreich durchgeführt<br/>Schreibvorgänge werden nicht durchgeführt|
|`ApplicationIntent=ReadOnly`|Verbindungen werden nicht hergestellt|Verbindungen werden erfolgreich hergestellt|Verbindungen werden erfolgreich hergestellt

Die oben gezeigte Tabelle veranschaulicht das Standardverhalten – dies ist das gleiche Verwalten wie in den SQL Server-Versionen vor[!INCLUDE[sssql19-md](../../../includes/sssql19-md.md)]. 

### <a name="primary_roleread_write_routing_url-set"></a>PRIMARY_ROLE(READ_WRITE_ROUTING_URL) festgelegt 

Nachdem Sie die Umleitung von Lese-/Schreibverbindungen festgelegt haben, behandelt das Replikat Verbindungsanforderungen anders. Das Verbindungsverhalten richtet sich weiterhin nach den Einstellungen für `SECONDARY_ROLE (ALLOW CONNECTIONS = )` und `ApplicationIntent`. Die folgende Tabelle zeigt, wie ein sekundäres Replikat mit festgelegtem `READ_WRITE_ROUTING` basierend auf `SECONDARY_ROLE (ALLOW CONNECTIONS = )` und `ApplicationIntent` Verbindungen behandelt.

|Wert vom Typ <code>ApplicationIntent</code>|<code>SECONDARY_ROLE (ALLOW CONNECTIONS = NO)</code>|<code>SECONDARY_ROLE (ALLOW CONNECTIONS = READ_ONLY)</code>|<code>SECONDARY_ROLE (ALLOW CONNECTIONS = ALL)</code>|
|-----|-----|-----|-----|
|`ApplicationIntent=ReadWrite`<br/>Standard|Verbindungen werden nicht hergestellt|Verbindungen werden nicht hergestellt|Verbindungen werden an das primäre Replikat geleitet|
|`ApplicationIntent=ReadOnly`|Verbindungen werden nicht hergestellt|Verbindungen werden erfolgreich hergestellt|Verbindungen werden erfolgreich hergestellt

Die oben stehende Tabelle zeigt Folgendes: Bei festgelegter `READ_WRITE_ROUTING_URL`-Option für das primäre Replikat leitet das sekundäre Replikat Verbindungen an das primäre Replikat um, wenn `SECONDARY_ROLE (ALLOW CONNECTIONS = ALL)` festgelegt ist. Die Verbindung gibt `ReadWrite` an.

## <a name="example"></a>Beispiel 

In diesem Beispiel weist die Verfügbarkeitsgruppe drei Replikat auf:
* Ein primäres Replikat auf COMPUTER01
* Ein synchrones sekundäres Replikat auf COMPUTER02
* Ein asynchrones sekundäres Replikat auf COMPUTER03

Die folgende Abbildung zeigt die Verfügbarkeitsgruppe.

![Verfügbarkeitsgruppe mit primärem, sekundärem und asynchronen sekundärem Replikat](media/replica-connection-redirection-always-on-availability-groups/01_originalAG.png)

Das folgende Transact-SQL-Skript erstellt diese Verfügbarkeitsgruppe. In diesem Beispiel gibt jedes Replikat die `READ_WRITE_ROUTING_URL` an.
```sql
CREATE AVAILABILITY GROUP MyAg   
     WITH ( CLUSTER_TYPE =  NONE )  
   FOR   
     DATABASE  <Database1>   
   REPLICA ON   
      'COMPUTER01' WITH   
         (  
         ENDPOINT_URL = 'TCP://COMPUTER01.<domain>.<tld>:5022',  
         AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,  
         FAILOVER_MODE = MANUAL,  
         SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL,   
            READ_ONLY_ROUTING_URL = 'TCP://COMPUTER01.<domain>.<tld>:1433' ),
         PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE,   
            READ_ONLY_ROUTING_LIST = (COMPUTER02, COMPUTER03),
            READ_WRITE_ROUTING_URL = 'TCP://COMPUTER01.<domain>.<tld>:1433' )   
         SESSION_TIMEOUT = 10  
         ),   
      'COMPUTER02' WITH   
         (  
         ENDPOINT_URL = 'TCP://COMPUTER02.<domain>.<tld>:5022',  
         AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,  
         FAILOVER_MODE = MANUAL, 
         SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL,   
            READ_ONLY_ROUTING_URL = 'TCP://COMPUTER02.<domain>.<tld>:1433' ),  
         PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE,   
            READ_ONLY_ROUTING_LIST = (COMPUTER01, COMPUTER03),  
            READ_WRITE_ROUTING_URL = 'TCP://COMPUTER02.<domain>.<tld>:1433' )   
         SESSION_TIMEOUT = 10  
         ),   
      'COMPUTER03' WITH   
         (  
         ENDPOINT_URL = 'TCP://COMPUTER03.<domain>.<tld>:5022',  
         AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,  
         FAILOVER_MODE = MANUAL,  
         SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL,   
            READ_ONLY_ROUTING_URL = 'TCP://COMPUTER03.<domain>.<tld>:1433' ),  
         PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE,   
            READ_ONLY_ROUTING_LIST = (COMPUTER01, COMPUTER02),  
            READ_WRITE_ROUTING_URL = 'TCP://COMPUTER03.<domain>.<tld>:1433' )  
         SESSION_TIMEOUT = 10  
         );
GO  
```
   - `<domain>.<tld>`
      - Domäne und Top-Level-Domäne des vollqualifizierten Domänennamens. Beispiel: `corporation.com`.


### <a name="connection-behaviors"></a>Verbindungsverhalten

Im folgenden Diagramm stellt eine Clientanwendung mit `ApplicationIntent=ReadWrite` eine Verbindung mit COMPUTER02 her. Die Verbindung wird an das primäre Replikat umgeleitet. 

![Die Verbindung zu Computer 2 wird wieder zum primären Replikat weitergeleitet.](media/replica-connection-redirection-always-on-availability-groups/02_redirectionAG.png)

Das sekundäre Replikat leitet Lese-/Schreibaufrufe an das primäre Replikat um. Eine Schreibverbindung an eines der Replikate wird an das primäre Replikat umgeleitet. 

Im folgenden Diagramm wurde für das primäre Replikat ein manuelles Failover zu COMPUTER02 ausgeführt. Eine Clientanwendung stellt mit `ApplicationIntent=ReadWrite` eine Verbindung mit COMPUTER01 her. Die Verbindung wird an das primäre Replikat umgeleitet. 

![Die Verbindung wird zum neuen primären Replikat auf Computer 2 weitergeleitet.](media/replica-connection-redirection-always-on-availability-groups/03_redirectionAG.png)

## <a name="see-also"></a>Weitere Informationen

[Übersicht zu AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 
[Informationen zum Clientverbindungszugriff auf Verfügbarkeitsreplikate (SQL Server)](../../../database-engine/availability-groups/windows/about-client-connection-access-to-availability-replicas-sql-server.md)   

[Verfügbarkeitsgruppenlistener, Clientkonnektivität und Anwendungsfailover &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)
