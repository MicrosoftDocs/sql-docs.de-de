---
title: Einrichten der Datenbankspiegelung (SQL Server) | Microsoft-Dokumentation
description: Hier erfahren Sie mehr über Voraussetzungen, Empfehlungen und Schritte zum Einrichten der Datenbankspiegelung in SQL Server, einschließlich einer Übersicht über eine Datenbankspiegelungssitzung.
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], deployment
ms.assetid: da45efed-55eb-4c71-be34-ac2589dfce8d
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: c085a7e1571e85e4fc3f3f9267e033e17302e09e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352279"
---
# <a name="setting-up-database-mirroring-sql-server"></a>Einrichten der Datenbankspiegelung (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In diesem Abschnitt werden die Voraussetzungen, Empfehlungen und Schritte zum Einrichten der Datenbankspiegelung beschrieben. Eine Einführung in die Datenbankspiegelung finden Sie unter [Datenbankspiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md).  
  
> [!IMPORTANT]  
>  Es empfiehlt sich, die Konfiguration der Datenbankspiegelung außerhalb der Spitzenbetriebszeiten durchzuführen, da sich die Konfiguration auf die Leistung auswirken kann.  
  
  
##  <a name="preparing-a-server-instance-to-host-a-mirror-server"></a><a name="PrepareInstances"></a> Vorbereiten einer Serverinstanz zum Hosten eines Spiegelservers  
 Für jede Datenbank-Spiegelungssitzung gilt:  
  
1.  Prinzipalserver, Spiegelserver und Zeuge müssen ggf. von separaten Serverinstanzen gehostet werden, die auf getrennten Hostsystemen ausgeführt werden. Jede der Serverinstanzen erfordert einen Datenbankspiegelungs-Endpunkt. Wenn Sie einen Datenbankspiegelungs-Endpunkt erstellen müssen, stellen Sie sicher, dass die anderen Serverinstanzen darauf zugreifen können.  
  
     Der für die Datenbankspiegelung von einer Serverinstanz verwendete Authentifizierungstyp ist eine Eigenschaft des Endpunkts der Datenbankspiegelung. Für die Datenbankspiegelung sind zwei Arten von Transportsicherheit verfügbar: die Windows-Authentifizierung oder die zertifikatbasierte Authentifizierung. Weitere Informationen finden Sie unter [Transportsicherheit für Datenbankspiegelung und Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md).  
  
     Die Anforderungen für den Netzwerkzugriff hängen von der Form der Authentifizierung wie folgt ab:  
  
    -   **Verwendung der Windows-Authentifizierung**  
  
         Wenn Serverinstanzen unter unterschiedlichen Domänenbenutzerkonten ausgeführt werden, erfordert jede Instanz eine Anmeldung bei der **master** -Datenbank der anderen Instanzen. Falls die Anmeldung nicht vorhanden ist, müssen Sie sie erstellen. Weitere Informationen finden Sie unter [Zulassen des Netzwerkzugriffs auf einen Datenbank-Spiegelungsendpunkt mithilfe der Windows-Authentifizierung &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-allow-network-access-windows-authentication.md).  
  
    -   **Verwendung von Zertifikaten**  
  
         Um die Zertifikatsauthentifizierung für die Datenbankspiegelung für eine bestimmte Serverinstanz zu aktivieren, muss der Systemadministrator jede Serverinstanz konfigurieren, um Zertifikate sowohl für ausgehende als auch für eingehende Verbindungen zu verwenden. Ausgehende Verbindungen müssen zuerst konfiguriert werden. Weitere Informationen finden Sie unter [Verwenden von Zertifikaten für einen Datenbankspiegelungs-Endpunkt &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md).  
  
2.  Stellen Sie sicher, dass auf dem Spiegelserver Anmeldungen für alle Datenbankbenutzer vorhanden sind. Weitere Informationen finden Sie unter [Einrichten von Anmeldekonten für die Datenbankspiegelung oder Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../database-engine/database-mirroring/set-up-login-accounts-database-mirroring-always-on-availability.md).  
  
3.  Legen Sie auf der Serverinstanz, auf der die Spiegeldatenbank gehostet wird, die übrigen Umgebungseinstellungen fest, die für die gespiegelte Datenbank erforderlich sind. Weitere Informationen finden Sie unter [Verwalten von Metadaten beim Bereitstellen einer Datenbank auf einer anderen Serverinstanz &#40;SQL Server&#41;](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md).  
  
##  <a name="overview-establishing-a-database-mirroring-session"></a><a name="EstablishUsingWinAuthentication"></a> Übersicht: Einrichten einer Datenbank-Spiegelungssitzung  
 Die grundlegenden Schritte zum Einrichten einer Spiegelungssitzung lauten wie folgt:  
  
1.  Erstellen Sie die Spiegeldatenbank, indem Sie die folgenden Sicherungen mithilfe von RESTORE WITH NORECOVERY für jeden Wiederherstellungsvorgang wiederherstellen:  
  
    1.  Stellen Sie die letzte vollständige Datenbanksicherung von der Prinzipaldatenbank wieder her, nachdem Sie sichergestellt haben, dass für die Prinzipaldatenbank bereits das vollständige Wiederherstellungsmodell verwendet wurde, als die Sicherung vorgenommen wurde. Die Spiegeldatenbank muss über den gleichen Namen wie die Prinzipaldatenbank verfügen.  
  
    2.  Wenn Sie seit der wiederhergestellten vollständigen Sicherung differenzielle Datenbanksicherungen vorgenommen haben, stellen Sie die letzte differenzielle Sicherung wieder her.  
  
    3.  Stellen Sie alle Protokollsicherungen wieder her, die seit der vollständigen oder differenziellen Datenbanksicherung ausgeführt wurden.  
  
     Weitere Informationen finden Sie unter [Vorbereiten einer Spiegeldatenbank auf die Spiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)verwendet.  
  
    > [!IMPORTANT]  
    >  Führen Sie die restlichen Einrichtungsschritte möglichst bald nach der Sicherung der Prinzipaldatenbank aus. Bevor Sie die Spiegelung auf den Partnern beginnen können, sollten Sie auf der ursprünglichen Datenbank eine aktuelle Protokollsicherung erstellen und in der zukünftigen Spiegelungsdatenbank wiederherstellen.  
  
2.  Sie können die Spiegelung entweder mit [!INCLUDE[tsql](../../includes/tsql-md.md)] oder dem Assistenten für die Datenbankspiegelung einrichten. Weitere Informationen finden Sie in den folgenden Themen:  
  
    -   [Einrichten einer Datenbank-Spiegelungssitzung mithilfe der Windows-Authentifizierung &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-establish-session-windows-authentication.md)  
  
    -   [Einrichten einer Datenbank-Spiegelungssitzung mithilfe der Windows-Authentifizierung &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
3.  Standardmäßig ist für eine Sitzung die Transaktionssicherheitsstufe FULL festgelegt (SAFETY ist auf FULL festgelegt), wodurch die Sitzung im synchronen Modus für hohe Sicherheit ohne automatisches Failover gestartet wird. Sie können die Sitzung wie folgt umkonfigurieren, sodass sie entweder im Modus für hohe Sicherheit mit automatischem Failover oder im asynchronen Modus für hohe Leistung ausgeführt wird:  
  
    -   **Modus für hohe Sicherheit mit automatischem Failover**  
  
         Wenn die Sitzung im Modus für hohe Sicherheit automatisches Failover unterstützen soll, fügen Sie eine Zeugenserverinstanz hinzu.  
  
         **So fügen Sie einen Zeugen hinzu**  
  
        -   [Hinzufügen eines Zeugen für die Datenbankspiegelung mithilfe der Windows-Authentifizierung &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/add-a-database-mirroring-witness-using-windows-authentication-transact-sql.md)  
  
        -   [Einrichten einer Datenbank-Spiegelungssitzung mithilfe der Windows-Authentifizierung &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
        > [!NOTE]  
        >  Der Datenbankbesitzer kann den Zeugen für eine Datenbank jederzeit deaktivieren. Das Deaktivieren des Zeugen ist gleichbedeutend mit dem Fehlen eines Zeugen, und das automatische Failover ist nicht möglich.  
  
    -   **Modus mit hoher Leistung**  
  
         Wenn Sie kein automatisches Failover wünschen oder wenn Ihnen Leistung wichtiger als hohe Verfügbarkeit ist, können Sie alternativ die Transaktionssicherheit deaktivieren. Weitere Informationen finden Sie unter [Ändern der Transaktionssicherheit in einer Datenbank-Spiegelungssitzung &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/change-transaction-safety-in-a-database-mirroring-session-transact-sql.md).  
  
        > [!NOTE]  
        >  Im Modus für hohe Leistung muss WITNESS auf OFF festgelegt sein. Weitere Informationen finden Sie unter [Quorum: Auswirkungen eines Zeugen auf die Datenbankverfügbarkeit &#40;Datenbankspiegelung&#41;](../../database-engine/database-mirroring/quorum-how-a-witness-affects-database-availability-database-mirroring.md).  
  
> [!NOTE]  
>  Ein Beispiel für das Verwenden von [!INCLUDE[tsql](../../includes/tsql-md.md)] zum Einrichten der Datenbankspiegelung mithilfe der Microsoft Windows-Authentifizierung finden Sie unter [Beispiel: Einrichten der Datenbankspiegelung mithilfe der Windows-Authentifizierung &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-windows-authentication-transact-sql.md).  
>   
>  Ein Beispiel für das Verwenden von [!INCLUDE[tsql](../../includes/tsql-md.md)] zum Einrichten der Datenbankspiegelung mithilfe der zertifikatbasierten Sicherheit finden Sie unter [Beispiel: Einrichten der Datenbankspiegelung mithilfe von Zertifikaten &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-certificates-transact-sql.md).  
  
  
##  <a name="in-this-section"></a><a name="InThisSection"></a> In diesem Abschnitt  
 [Vorbereiten einer Spiegeldatenbank auf die Spiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)  
 Fasst die Schritte zum Erstellen einer Spiegeldatenbank oder zum Vorbereiten einer Spiegeldatenbank vor dem Fortsetzen einer angehaltenen Sitzung zusammen. Darüber hinaus werden Links zu Themen zur Vorgehensweise bereitgestellt.  
  
 [Angeben einer Servernetzwerkadresse &#40;Datenbankspiegelung&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)  
 Beschreibt die Syntax einer Servernetzwerkadresse, wie die Adresse den Datenbankspiegelungs-Endpunkt der Serverinstanz identifiziert und wie der vollqualifizierte Domänenname eines Systems ermittelt werden kann.  
  
 [Einrichten einer Datenbank-Spiegelungssitzung mithilfe der Windows-Authentifizierung &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
 Beschreibt, wie der Assistent zum Konfigurieren der Sicherheit für die Datenbankspiegelung zum Starten der Datenbankspiegelung für eine Datenbank verwendet wird.  
  
 [Einrichten einer Datenbank-Spiegelungssitzung mithilfe der Windows-Authentifizierung &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-establish-session-windows-authentication.md)  
 Beschreibt die [!INCLUDE[tsql](../../includes/tsql-md.md)] -Schritte zum Einrichten der Datenbankspiegelung.  
  
 [Beispiel: Einrichten der Datenbankspiegelung mithilfe der Windows-Authentifizierung &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-windows-authentication-transact-sql.md)  
 Enthält ein Beispiel mit sämtlichen Schritten, die für das Erstellen einer Datenbank-Spiegelungssitzung mit einem Zeugen unter Verwendung der Windows-Authentifizierung erforderlich sind.  
  
 [Beispiel: Einrichten der Datenbankspiegelung mithilfe von Zertifikaten &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-certificates-transact-sql.md)  
 Enthält ein Beispiel mit sämtlichen Schritten, die für das Erstellen einer Datenbank-Spiegelungssitzung mit einem Zeugen unter Verwendung der zertifikatbasierten Authentifizierung erforderlich sind.  
  
 [Einrichten von Anmeldekonten für die Datenbankspiegelung oder Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../database-engine/database-mirroring/set-up-login-accounts-database-mirroring-always-on-availability.md)  
 Beschreibt, wie eine Anmeldung für eine Remoteserverinstanz erstellt wird, die ein anderes Konto als die lokale Serverinstanz verwendet.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
 **SQL Server Management Studio**  
  
-   [Starten des Assistenten zum Konfigurieren der Sicherheit für die Datenbankspiegelung &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-the-configuring-database-mirroring-security-wizard.md)  
  
-   [Einrichten einer Datenbank-Spiegelungssitzung mithilfe der Windows-Authentifizierung &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
 **Transact-SQL**  
  
-   [Zulassen des Netzwerkzugriffs auf einen Datenbank-Spiegelungsendpunkt mithilfe der Windows-Authentifizierung &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-allow-network-access-windows-authentication.md)  
  
-   [Ermöglichen des Verwendens von Zertifikaten für ausgehende Verbindungen für einen Datenbankspiegelungs-Endpunkt &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md)  
  
-   [Ermöglichen des Verwendens von Zertifikaten für eingehende Verbindungen für einen Datenbankspiegelungs-Endpunkt &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-inbound-connections.md)  
  
-   [Erstellen eines Endpunkts der Datenbankspiegelung für Windows-Authentifizierung &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)  
  
-   [Einrichten einer Datenbank-Spiegelungssitzung mithilfe der Windows-Authentifizierung &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-establish-session-windows-authentication.md)  
  
-   [Hinzufügen eines Zeugen für die Datenbankspiegelung mithilfe der Windows-Authentifizierung &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/add-a-database-mirroring-witness-using-windows-authentication-transact-sql.md)  
  
-   [Einrichten der TRUSTWORTHY-Eigenschaft für eine Spiegeldatenbank &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/set-up-a-mirror-database-to-use-the-trustworthy-property-transact-sql.md)  
  
 **Transact-SQL/SQL Server Management Studio**  
  
-   [Upgrading Mirrored Instances](../../database-engine/database-mirroring/upgrading-mirrored-instances.md)  
  
-   [Vorbereiten einer Spiegeldatenbank auf die Spiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)  
  
-   [Problembehandlung für die Datenbankspiegelungskonfiguration &#40;SQL Server&#41;](../../database-engine/database-mirroring/troubleshoot-database-mirroring-configuration-sql-server.md)  
  
  
## <a name="see-also"></a>Weitere Informationen  
 [Datenbankspiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [Datenbankspiegelung: Interoperabilität und gleichzeitige Verwendung &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-interoperability-and-coexistence-sql-server.md)   
 [Transportsicherheit für Datenbankspiegelung und Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)   
 [Angeben einer Servernetzwerkadresse &#40;Datenbankspiegelung&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)  
  
  
