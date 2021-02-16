---
title: Angeben der Endpunkt-URL für Verfügbarkeitsreplikate
description: In diesem Artikel erfahren Sie, wie Sie die Endpunkt-URL angeben, wenn Sie ein Replikat in einer Always On-Verfügbarkeitsgruppe in SQL Server hinzufügen oder ändern.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- endpoints [SQL Server], AlwaysOn Availability Groups
- Availability Groups [SQL Server], configuring
- Availability Groups [SQL Server], endpoint
- Endpoint URLs (HADR)
ms.assetid: d7520c13-a8ee-4ddc-9e9a-54cd3d27ef1c
author: cawrites
ms.author: chadam
ms.openlocfilehash: 0bee045cec3e11e7ae4f511b7e7b630aee208cd9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349260"
---
# <a name="specify-endpoint-url---adding-or-modifying-availability-replica"></a>Angeben der Endpunkt-URL: Hinzufügen oder Ändern von Verfügbarkeitsreplikaten
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Um ein Verfügbarkeitsreplikat für eine Verfügbarkeitsgruppe zu hosten, muss eine Serverinstanz einen Datenbankspiegelungs-Endpunkt besitzen. Die Serverinstanz überwacht [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] -Meldungen von Verfügbarkeitsreplikaten, die von anderen Serverinstanzen gehostet wurden, mithilfe dieses Endpunkts. Um ein Verfügbarkeitsreplikat für eine Verfügbarkeitsgruppe zu definieren, müssen Sie die Endpunkt-URL der Serverinstanz angeben, die das Replikat hostet. Die *Endpunkt-URL* identifiziert das Transportprotokoll des Datenbankspiegelungs-Endpunkt-TCP, die Systemadresse der Serverinstanz und die dem Endpunkt zugeordnete Portnummer.  
  
> [!NOTE]  
>  Die Begriff "Endpunkt-URL" ist ein Synonym für den Begriff "Servernetzwerkadresse", der von der Datenbankspiegelungsbenutzeroberfläche und in der Dokumentation verwendet wird.  
  
  
##  <a name="syntax-for-an-endpoint-url"></a><a name="SyntaxOfURL"></a> Syntax für eine Endpunkt-URL  
 Die Syntax für eine Endpunkt-URL hat folgende Form:  
  
 TCP <strong>://</strong> *\<system-address>* <strong>:</strong> *\<port>*  
  
 Hierbei gilt:  
  
-   *\<system-address>* ist eine Zeichenfolge, die das Zielcomputersystem eindeutig identifiziert. In der Regel handelt es sich bei der Serveradresse um einen Systemnamen (wenn sich die Systeme in derselben Domäne befinden), einen vollqualifizierten Domänennamen oder eine IP-Adresse:  
  
    -   Da die Knoten des WSFC-Clusters (Windows Server-Failoverclustering) sich in derselben Domäne befinden, können Sie den Namen des Computersystems verwenden, z. B. `SYSTEM46`.  
  
    -   Wenn Sie eine IP-Adresse verwenden möchten, muss diese in Ihrer Umgebung eindeutig sein. Wir empfehlen die Verwendung einer IP-Adresse nur, wenn diese statisch ist. Die IP-Adresse kann im IPv4-Format (IP Version 4) oder im IPv6-Format (IP Version) vorliegen. Eine IPv6-Adresse muss in eckige Klammern gesetzt werden, z.B. **[** _<IPv6-Adresse>_ **]** .  
  
         Um die IP-Adresse eines Systems zu ermitteln, geben Sie an der Windows-Eingabeaufforderung den Befehl **ipconfig** ein.  
  
    -   Der vollqualifizierte Domänenname funktioniert auf alle Fälle. Hierbei handelt es sich um eine lokal definierte Adresszeichenfolge, die an unterschiedlichen Stellen unterschiedliche Formen annimmt. Häufig, jedoch nicht immer, ist ein vollqualifizierter Domänenname ein zusammengesetzter Name, der den Computernamen und eine Reihe von Domänensegmenten enthält, die durch Punkte voneinander getrennt sind, z. B.:  
  
         _Computername_ **.** _Domänensegment_[... **.** _Domänensegment_]  
  
         Dabei steht *Computername* für den Netzwerknamen des Computers, auf dem die Serverinstanz ausgeführt wird, und *Domänensegment*[... **.** _Domänensegment_] für die übrigen Domäneninformationen des Servers. Beispiel: `localinfo.corp.Adventure-Works.com`.  
  
         Inhalt und Anzahl von Domänenelementen werden innerhalb des Unternehmens oder der Organisation bestimmt. Weitere Informationen finden Sie in [Ermitteln des vollqualifizierten Domänennamens](#Finding_FQDN)weiter unten in diesem Thema.  
  
-   *\<port>* ist die Portnummer, die vom Spiegelungsendpunkt der Partnerserverinstanz verwendet wird.  
  
     Ein Datenbankspiegelungs-Endpunkt kann jeden verfügbaren Port im Computersystem verwenden. Jede Portnummer darf nur einem Endpunkt zugeordnet werden, und jeder Endpunkt ist einer einzelnen Serverinstanz zugeordnet. Daher lauschen unterschiedliche Serverinstanzen auf dem gleichen Server an unterschiedliche Endpunkten mit unterschiedlichen Ports. Daher leitet der Port, den Sie in der Endpunkt-URL beim Festlegen eines Verfügbarkeitsreplikat angeben, eingehende Meldungen immer an die Serverinstanz weiter, deren Endpunkt diesem Port zugeordnet ist.  
  
     In der Endpunkt-URL identifiziert nur die Nummer des Ports die Serverinstanz, die dem Spiegelungsendpunkt auf dem Zielcomputer zugeordnet ist. Die folgende Abbildung veranschaulicht die Endpunkt-URLS von zwei Serverinstanzen auf einem Computer. In der Standardinstanz wird Port `7022` verwendet, und in der benannten Instanz wird Port `7033`verwendet. Die Endpunkt-URL für diese beiden Serverinstanzen lauten `TCP://MYSYSTEM.Adventure-works.MyDomain.com:7022` bzw. `TCP://MYSYSTEM.Adventure-works.MyDomain.com:7033`. Beachten Sie, dass die Adresse nicht den Namen der Serverinstanz enthält.  
  
     ![Servernetzwerkadressen einer Standardinstanz](../../../database-engine/availability-groups/windows/media/dbm-2-instances-ports-1-system.gif "Servernetzwerkadressen einer Standardinstanz")  
  
     Verwenden Sie die folgende [!INCLUDE[tsql](../../../includes/tsql-md.md)] -Anweisung, um den Port zu identifizieren, der derzeit dem Endpunkt der Datenbankspiegelung einer Serverinstanz zugeordnet ist:  
  
    ```  
    SELECT type_desc, port FROM sys.TCP_endpoints  
    ```  
  
     Suchen Sie die Zeile, deren **type_desc** -Wert „DATABASE_MIRRORING“ lautet, und verwenden Sie die entsprechende Portnummer.  
  
### <a name="examples"></a>Beispiele  
  
#### <a name="a-using-a-system-name"></a>A. Verwenden eines Systemnamens  
 Die folgende Endpunkt-URL gibt einen Systemnamen, `SYSTEM46`und Port `7022`an.  
  
 `TCP://SYSTEM46:7022`  
  
#### <a name="b-using-a-fully-qualified-domain-name"></a>B. Verwenden eines vollqualifizierten Domänennamens  
 Die folgende Endpunkt-URL gibt den vollqualifizierten Domänennamen `DBSERVER8.manufacturing.Adventure-Works.com`und Port `7024`an.  
  
 `TCP://DBSERVER8.manufacturing.Adventure-Works.com:7024`  
  
#### <a name="c-using-ipv4"></a>C. Verwenden von IPv4  
 Die folgende Endpunkt-URL gibt die IPv4-Adresse `10.193.9.134`und Port `7023`an.  
  
 `TCP://10.193.9.134:7023`  
  
#### <a name="d-using-ipv6"></a>D: Verwenden von IPv6  
 Die folgende Endpunkt-URL gibt die IPv6-Adresse `2001:4898:23:1002:20f:1fff:feff:b3a3`und Port `7022`an.  
  
 `TCP://[2001:4898:23:1002:20f:1fff:feff:b3a3]:7022`  
  
##  <a name="finding-the-fully-qualified-domain-name-of-a-system"></a><a name="Finding_FQDN"></a> Ermitteln des vollqualifizierten Domänennamens eines Systems  
 Um den vollqualifizierten Domänennamen eines Systems zu ermitteln, geben Sie an der Windows-Eingabeaufforderung des Systems den folgenden Befehl ein:  
  
 **IPCONFIG /ALL**  
  
 Wenn Sie den vollqualifizierten Domänennamen bilden möchten, verketten Sie die Werte von *<host_name>* bzw. *<Primary_Dns_Suffix>* wie folgt:  
  
 _<host_name>_ **.** _<Primary_Dns_Suffix>_  
  
 Beispielsweise entspricht die IP-Konfiguration  
  
 `Host Name  .  .  .  .  .  .  : MYSERVER`  
  
 `Primary Dns Suffix  .  .  .  : mydomain.Adventure-Works.com`  
  
 dem folgenden vollqualifizierten Domänennamen:  
  
 `MYSERVER.mydomain.Adventure-Works.com`  
  
> [!NOTE]  
>  Wenn Sie weitere Informationen zu einem vollqualifizierten Domänennamen benötigen, wenden Sie sich an den Systemadministrator.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
 **So konfigurieren Sie einen Datenbankspiegelungs-Endpunkt**  
  
-   [Erstellen eines Datenbankspiegelungs-Endpunkts für Always On-Verfügbarkeitsgruppen &#40;SQL Server PowerShell&#41;](../../../database-engine/availability-groups/windows/database-mirroring-always-on-availability-groups-powershell.md)  
  
-   [Erstellen eines Endpunkts der Datenbankspiegelung für Windows-Authentifizierung &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)  
  
-   [Verwenden von Zertifikaten für einen Datenbankspiegelungs-Endpunkt &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)  
  
    -   [Ermöglichen des Verwendens von Zertifikaten für ausgehende Verbindungen für einen Datenbankspiegelungs-Endpunkt &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md)  
  
    -   [Ermöglichen des Verwendens von Zertifikaten für eingehende Verbindungen für einen Datenbankspiegelungs-Endpunkt &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/database-mirroring-use-certificates-for-inbound-connections.md)  
  
-   [Angeben einer Servernetzwerkadresse &#40;Datenbankspiegelung&#41;](../../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)  
  
-   Angeben der Endpunkt-URL beim Hinzufügen oder Ändern eines Verfügbarkeitsreplikats (SQL Server)  
  
-   [Problembehandlung für die AlwaysOn-Verfügbarkeitsgruppenkonfiguration &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md)  
  
 **So zeigen Sie Informationen zum Datenbankspiegelungs-Endpunkt an**  
  
-   [sys.database_mirroring_endpoints &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md)  
  
 **So fügen Sie ein Verfügbarkeitsreplikat hinzu**  
  
-   [Hinzufügen eines sekundären Replikats zu einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
-   [Verknüpfen eines sekundären Replikats mit einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Verwandte Inhalte  
  
-   [Microsoft SQL Server Always On-Lösungshandbuch zu hoher Verfügbarkeit und Notfallwiederherstellung](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))  
  
## <a name="see-also"></a>Weitere Informationen  
 [Erstellung und Konfiguration von Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md)   
 [Übersicht über AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../../t-sql/statements/create-endpoint-transact-sql.md)  
  
