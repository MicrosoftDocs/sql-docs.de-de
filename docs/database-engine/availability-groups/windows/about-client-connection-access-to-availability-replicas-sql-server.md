---
title: Typen von Clientverbindungen zu Replikaten in einer Verfügbarkeitsgruppe
description: Erfahren Sie, welche Verbindungstypen Clients entweder mit primären oder sekundären Replikaten einer Always On-Verfügbarkeitsgruppe in SQL Server erstellen können.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], readable secondary replicas
- active secondary replicas [SQL Server], read-only access to
- Availability Groups [SQL Server], configuring
- Availability Groups [SQL Server], client connectivity
- Availability Groups [SQL Server], active secondary replicas
ms.assetid: 29027e46-43e4-4b45-b650-c4cdeacdf552
author: cawrites
ms.author: chadam
ms.openlocfilehash: b05793948960a5981813095413dc48dfe23df3ae
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339268"
---
# <a name="types-of-client-connections-to-replicas-within-an-always-on-availability-group"></a>Typen von Clientverbindungen zu Replikaten in einer Always On-Verfügbarkeitsgruppe
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In einer Always On-Verfügbarkeitsgruppe können Sie mindestens ein Verfügbarkeitsreplikat konfigurieren, um schreibgeschützte Verbindungen zuzulassen, wenn es unter der sekundären Rolle ausgeführt wird (d h. bei Ausführung als sekundäres Replikat). Sie können auch jedes Verfügbarkeitsreplikat konfigurieren, um schreibgeschützte Verbindungen bei der Ausführung unter der primären Rolle zuzulassen oder auszuschließen (d. h. bei Ausführung als das primäre Replikat).  
  
 Um den Clientzugriff auf primäre oder sekundäre Datenbanken einer bestimmten Verfügbarkeitsgruppe zu erleichtern, sollten Sie einen Verfügbarkeitsgruppenlistener erstellen. Standardmäßig werden eingehende Verbindungen vom Verfügbarkeitsgruppenlistener an das primäre Replikat weitergeleitet. Sie können jedoch eine Verfügbarkeitsgruppe konfigurieren, um schreibgeschütztes Routing zu unterstützen, das seinem Verfügbarkeitsgruppenlistener ermöglicht, die Verbindungsanforderungen von Anwendungen für beabsichtigte Lesevorgänge an ein lesbares, sekundäres Replikat umzuleiten. Weitere Informationen finden Sie unter [Konfigurieren des schreibgeschützten Routing für eine Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md).  
  
 Während eines Failovers geht ein sekundäres Zielreplikat in die primäre Rolle über, und das frühere primäre Replikat geht in die sekundäre Rolle über. Während des Failoverprozesses werden alle Clientverbindungen zum primären Replikat und zu den sekundären Replikaten beendet. Nach dem Failover, wenn ein Client die Verbindung mit dem Verfügbarkeitsgruppenlistener wiederherstellt, verbindet der Listener den Client erneut mit dem neuen primären Replikat, sofern es sich dabei nicht um eine Verbindungsanforderung für beabsichtigte Lesevorgänge handelt. Wenn schreibgeschütztes Routing auf dem Client und den Serverinstanzen konfiguriert wird, die das neue primäre Replikat hosten, und auf mindestens einem lesbaren sekundären Replikat, werden die Verbindungsanforderungen für beabsichtigte Lesevorgänge an ein sekundäres Replikat weitergeleitet, das den angeforderten Verbindungszugriffstyp des Clients unterstützt. Um nach einem Failover ordnungsgemäße Clientfunktionen sicherzustellen, ist es wichtig, den Verbindungszugriff für sekundäre und primäre Rollen jedes Verfügbarkeitsreplikats zu konfigurieren.  
  
> [!NOTE]  
>  Weitere Informationen über den Verfügbarkeitsgruppenlistener, der Verbindungsanforderungen von Clients verarbeitet, finden Sie unter [Verfügbarkeitsgruppenlistener, Clientkonnektivität und Anwendungsfailover &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md).  
  
##  <a name="types-of-connection-access-supported-by-the-secondary-role"></a><a name="ConnectAccessForSecondary"></a> Von der sekundären Rolle unterstützte Verbindungszugriffstypen  
 Die sekundäre Rolle unterstützt die folgenden drei Alternativen für Clientverbindungen:  
  
 Keine Verbindungen  
 Es werden keine Benutzerverbindungen zugelassen. Sekundäre Datenbanken sind nicht für Lesezugriff verfügbar. Dies ist das Standardverhalten in der sekundären Rolle.  
  
 Nur Verbindungen für beabsichtigte Lesevorgänge  
 Die sekundären Datenbanken sind nur für Verbindungen verfügbar, für die die **Anwendungszweck** -Verbindungseigenschaft auf **ReadOnly** (*Verbindungen für beabsichtigte Lesevorgänge*) festgelegt ist.  
  
 Weitere Informationen zu dieser Verbindungseigenschaft finden Sie unter [SQL Server Native Client-Unterstützung für hohe Verfügbarkeit, Wiederherstellung im Notfall](../../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md).  
  
 Schreibgeschützte Verbindungen zulassen  
 Die sekundären Datenbanken sind alle für Lesezugriffsverbindungen verfügbar. Diese Option ermöglicht es Clients mit älteren Versionen, eine Verbindung herzustellen.  
  
 Weitere Informationen finden Sie unter [Konfigurieren des schreibgeschützten Zugriffs auf ein Verfügbarkeitsreplikat &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)besitzen.  
  
##  <a name="types-of-connection-access-supported-by-the-primary-role"></a><a name="ConnectAccessForPrimary"></a> Von der primären Rolle unterstützte Verbindungszugriffstypen  
 Die primäre Rolle unterstützt die folgenden zwei Alternativen für Clientverbindungen:  
  
 Alle Verbindungen sind zugelassen  
 Sowohl Verbindungen mit Lese-/Schreibzugriff als auch schreibgeschützte Verbindungen sind für primäre Datenbanken zugelassen. Dies ist das Standardverhalten für die primäre Rolle.  
  
 Nur Verbindungen mit Lese-/Schreibzugriff zulassen  
 Wenn die **Anwendungszweck** -Verbindungseigenschaft auf **ReadWrite** oder nicht festgelegt ist, wird die Verbindung zugelassen. Verbindungen, für die das **Anwendungszweck** -Verbindungszeichenfolgen-Schlüsselwort auf **ReadOnly** festgelegt ist, werden nicht zugelassen. Durch das Zulassen nur von Verbindungen mit Lese-/Schreibzugriff kann verhindert werden, dass die Kunden mit dem primären Replikat versehentlich eine leseintensive Arbeitsauslastung verbinden.  
  
 Weitere Informationen zu dieser Verbindungseigenschaft finden Sie unter [Using Connection String Keywords with SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md).  
  
 Weitere Informationen finden Sie unter [Konfigurieren des schreibgeschützten Zugriffs auf ein Verfügbarkeitsreplikat &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)besitzen.  
  
##  <a name="how-the-connection-access-configuration-affects-client-connectivity"></a><a name="HowConnectionAccessAffectsConnectivity"></a> Auswirkungen der Verbindungszugriffskonfiguration auf die Clientkonnektivität  
 Die Verbindungszugriffseinstellungen eines Replikats bestimmen, ob ein Verbindungsversuch fehlschlägt oder erfolgreich ist. In der folgenden Tabelle wird für jede Verbindungszugriffseinstellung zusammengefasst, ob ein bestimmter Verbindungsversuch erfolgreich ist oder fehlschlägt.  
  
|Replikatrolle|Auf Replikat unterstützter Verbindungszugriff|Verbindungsabsicht|Ergebnis des Verbindungsversuchs|  
|------------------|--------------------------------------------|-----------------------|--------------------------------|  
|Secondary|All|Beabsichtigte Lesevorgänge, Lese-/Schreibzugriff oder keine Verbindungsabsicht angegeben|Erfolg|  
|Secondary|Keine (dies ist der sekundäre Standardverhalten)|Beabsichtigte Lesevorgänge, Lese-/Schreibzugriff oder keine Verbindungsabsicht angegeben|Fehler|  
|Secondary|Nur beabsichtigte Lesevorgänge|Beabsichtigte Lesevorgänge|Erfolg|  
|Secondary|Nur beabsichtigte Lesevorgänge|Lese-/Schreibzugriff oder keine Verbindungsabsicht angegeben|Fehler|  
|Primär|Alle (dies ist das primäre Standardverhalten)|Schreibgeschützt, Lese-/Schreibzugriff oder keine Verbindungsabsicht angegeben|Erfolg|  
|Primär|Lese-/Schreibzugriff|Nur beabsichtigte Lesevorgänge|Fehler|  
|Primär|Lese-/Schreibzugriff|Lese-/Schreibzugriff oder keine Verbindungsabsicht angegeben|Erfolg|  
  
 Informationen darüber, wie Sie die Verfügbarkeitsgruppe konfigurieren müssen, damit diese Clientverbindungen zu ihren Replikaten akzeptiert, finden Sie unter [Verfügbarkeitsgruppenlistener, Clientkonnektivität und Anwendungsfailover &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md).  
  
### <a name="example-connection-access-configuration"></a>Beispiel für Verbindungszugriffskonfiguration  
 Abhängig von der unterschiedlichen Konfiguration von Verfügbarkeitsreplikaten für den Verbindungszugriff kann sich die Unterstützung für Clientverbindungen nach dem Failover einer Verfügbarkeitsgruppe ändern. Betrachten Sie z. B. eine Verfügbarkeitsgruppe, für die die Berichterstellung auf sekundären Remotereplikaten mit asynchronem Commit ausgeführt wird. Für alle schreibgeschützten Anwendungen für die Datenbanken in dieser Verfügbarkeitsgruppe ist die **Anwendungszweck** -Verbindungseigenschaft auf **ReadOnly** festgelegt, damit alle schreibgeschützten Verbindungen Verbindungen für beabsichtigte Lesevorgänge sind.  
  
 Diese Beispielverfügbarkeitsgruppe besitzt zwei Replikate mit synchronem Commit im Hauptrechencenter und zwei Replikate mit asynchronem Commit an einem Satellitenstandort. Für die primäre Rolle sind alle Replikate für Lese-/Schreibzugriff konfiguriert, wodurch Verbindungen für beabsichtigte Lesevorgänge zum primären Replikat in allen Situationen verhindert werden. Die sekundäre Rolle mit synchronem Commit verwendet die Standard-Verbindungszugriffskonfiguration ("keine"), die alle Clientverbindungen unter der sekundären Rolle verhindert.  Im Gegensatz dazu werden die Replikate mit asynchronem Commit konfiguriert, um Verbindungen für beabsichtigte Lesevorgänge unter der sekundären Rolle zuzulassen. In der folgenden Tabelle wird diese Beispielkonfiguration zusammengefasst:  
  
|Replikat|Commit-Modus|Anfangsrolle|Verbindungszugriff für sekundäre Rolle|Verbindungszugriff für primäre Rolle|  
|-------------|-----------------|------------------|------------------------------------------|----------------------------------------|  
|Replikat1|Synchron|Primär|Keine|Lese-/Schreibzugriff|  
|Replikat2|Synchron|Secondary|Keine|Lese-/Schreibzugriff|  
|Replikat3|Asynchron|Secondary|Nur beabsichtigte Lesevorgänge|Lese-/Schreibzugriff|  
|Replikat4|Asynchron|Secondary|Nur beabsichtigte Lesevorgänge|Lese-/Schreibzugriff|  
  
 In der Regel treten in diesem Beispielszenario Failover nur zwischen den Replikaten mit synchronem Commit auf, und sofort nach dem Failover können Anwendungen für beabsichtigte Lesevorgänge erneut eine Verbindung mit einem der sekundären Replikate mit asynchronem Commit herstellen. Bei einem Notfall im Hauptrechencenter gehen jedoch beide Replikate mit synchronem Commit verloren. Der Datenbankadministrator am Satellitenstandort reagiert, indem er ein erzwungenes manuelles Failover zu einem sekundären Replikat mit asynchronem Commit ausführt. Die sekundären Datenbanken auf dem verbleibenden sekundären Replikat werden vom erzwungenen Failover angehalten und dadurch für schreibgeschützte Arbeitsauslastungen nicht verfügbar. Das für Lese-/Schreibverbindungen konfigurierte neue primäre Replikat verhindert, dass die Arbeitsauslastung für beabsichtigte Lesevorgänge mit der Auslastung für Lese-/Schreibzugriff konkurriert. Dies bedeutet, dass bis der Datenbankadministrator die sekundären Datenbanken auf dem verbleibenden sekundären Replikat mit asynchronem Commit fortsetzt, Clients für beabsichtigte Lesevorgänge keine Verbindung mit einem Verfügbarkeitsreplikat herstellen können.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
  
-   [Konfigurieren des schreibgeschützten Zugriffs auf ein Verfügbarkeitsreplikat &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)  
  
-   [Konfigurieren des schreibgeschützten Routing für eine Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md)  
  
-   [Überwachen von Verfügbarkeitsgruppen &#40;Transact-SQL&#41;](../../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)  
  
-   [Anzeigen von Verfügbarkeitsreplikateigenschaften &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/view-availability-replica-properties-sql-server.md)  
  
-   [Verwenden des Dialogfelds Neue Verfügbarkeitsgruppe &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Verwandte Inhalte  
  
-   [Microsoft SQL Server Always On-Lösungshandbuch zu hoher Verfügbarkeit und Notfallwiederherstellung](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))  
  
-   [SQL Server Always On-Teamblog: Der offizielle SQL Server Always On-Teamblog](/archive/blogs/sqlalwayson/)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Übersicht über AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Verfügbarkeitsgruppenlistener, Clientkonnektivität und Anwendungsfailover &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)   
 [Statistik](../../../relational-databases/statistics/statistics.md)  
  
