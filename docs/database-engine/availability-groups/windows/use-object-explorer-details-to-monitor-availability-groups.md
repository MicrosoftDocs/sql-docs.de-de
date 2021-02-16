---
title: Überwachen von Verfügbarkeitsgruppen anhand der Details im Objekt-Explorer | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie SQL Server Management Studio zum Überwachen und Verwalten vorhandener AlwaysOn-Verfügbarkeitsgruppen, Verfügbarkeitsreplikate und Verfügbarkeitsdatenbanken verwenden.
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.availabilitygroup.OEdetails.f1
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- Availability Groups [SQL Server], databases
- Availability Groups [SQL Server]
ms.assetid: 84affc47-40e0-43d9-855e-468967068c35
author: cawrites
ms.author: chadam
ms.openlocfilehash: acf2d400610afdaf695f696ae8f63edfc6fd0a24
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344511"
---
# <a name="use-object-explorer-details-to-monitor-availability-groups"></a>Überwachen von Verfügbarkeitsgruppen anhand der Details im Objekt-Explorer
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In diesem Thema wird beschrieben, wie der Bereich **Details zum Objekt-Explorer** von [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] verwendet wird, um vorhandene Always On-Verfügbarkeitsgruppen, -Verfügbarkeitsreplikate und -Verfügbarkeitsdatenbanken zu überwachen und zu verwalten.  
  
> [!NOTE]  
>  Informationen zum Verwenden des Bereichs mit Details zum Objekt-Explorer finden Sie unter [Detailbereich des Objekt-Explorers](../../../ssms/object/object-explorer-details-pane.md).  
  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Voraussetzungen  
 Sie müssen mit der Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (Serverinstanz) verbunden sein, die entweder das primäre Replikat oder ein sekundäres Replikat hostet.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
 **So überwachen Sie Verfügbarkeitsgruppen, Verfügbarkeitsreplikate und Verfügbarkeitsdatenbanken**  
  
1.  Klicken Sie im Menü **Ansicht** auf **Details zum Objekt-Explorer** , oder drücken Sie F7.  
  
2.  Stellen Sie im Objekt-Explorer eine Verbindung mit der Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] her, auf der Sie eine Verfügbarkeitsgruppe überwachen möchten, und klicken Sie auf den Servernamen, um die Serverstruktur zu erweitern.  
  
3.  Erweitern Sie den Knoten **Hohe Verfügbarkeit (immer aktiviert)** und den Knoten **Verfügbarkeitsgruppen** .  
  
4.  Der Bereich **Details zum Objekt-Explorer** zeigt jede Verfügbarkeitsgruppe an, für die die verbundene Serverinstanz ein Replikat hostet. Für jede Verfügbarkeitsgruppe zeigt die Spalte **Serverinstanz (primär)** den Namen der Serverinstanz an, die derzeit das primäre Replikat hostet.  Um weitere Informationen zu einer bestimmten Verfügbarkeitsgruppe anzuzeigen, wählen Sie diese im Objekt-Explorer aus.  
  
5.  Der Bereich **Details zum Objekt-Explorer** zeigt dann die Knoten **Verfügbarkeitsreplikate** und **Verfügbarkeitsdatenbanken** für diese Verfügbarkeitsgruppe an:  
  
    -   Wenn Sie den Knoten **Verfügbarkeitsgruppe** im Objekt-Explorer erweitern und den Knoten **Verfügbarkeitsreplikate** auswählen, werden im Bereich **Details zum Objekt-Explorer** Informationen zu jedem Verfügbarkeitsreplikat in der Gruppe angezeigt. Weitere Informationen finden Sie weiter unten in diesem Thema unter [Verfügbarkeitsreplikatdetails](#AvReplicaDetails).  
  
         Um Vorgänge auf mehreren Verfügbarkeitsreplikaten auszuführen, wählen Sie diese aus, und klicken Sie mit der rechten Maustaste, um ein Kontextmenü zu öffnen, das die verfügbaren Befehle auflistet.  
  
    -   Wenn Sie den Knoten **Verfügbarkeitsgruppe** im Objekt-Explorer erweitern und den Knoten **Verfügbarkeitsdatenbanken** auswählen, werden im Bereich **Details zum Objekt-Explorer** Informationen zu jeder Verfügbarkeitsdatenbank in der Gruppe angezeigt. Weitere Informationen finden Sie weiter unten in diesem Thema unter [Verfügbarkeitsdatenbank-Details](#AvDbDetails).  
  
         Um Vorgänge auf mehreren Verfügbarkeitsdatenbanken auszuführen, wählen Sie diese aus, und klicken Sie mit der rechten Maustaste, um ein Kontextmenü zu öffnen, das die verfügbaren Befehle auflistet.  
  
##  <a name="availability-groups-details"></a><a name="AvGroupsDetails"></a> Verfügbarkeitsgruppendetails  
 Im Detailbildschirm **Verfügbarkeitsgruppen** werden die folgenden Spalten angezeigt:  
  
 **Name**  
 Führt die Listenerordner **Verfügbarkeitsreplikate**, **Verfügbarkeitsdatenbanken** und **Verfügbarkeitsgruppe** der ausgewählten Verfügbarkeitsgruppe auf.  
  
##  <a name="availability-replica-details"></a><a name="AvReplicaDetails"></a> Verfügbarkeitsreplikatdetails  
 Im Detailbildschirm **Verfügbarkeitsreplikat** werden die folgenden Spalten angezeigt:  
  
 **Serverinstanz**  
 Zeigt den Namen der Serverinstanz an, die das Verfügbarkeitsreplikat hostet, zusammen mit einem Symbol, das den aktuellen Verbindungsstatus der Serverinstanz zur lokalen Serverinstanz angibt.  
  
 **Rolle**  
 Gibt die aktuelle Rolle des Verfügbarkeitsreplikats an, entweder **Primär** oder **Sekundär**. Informationen über [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]-Rollen finden Sie unter [Übersicht über Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).  
  
 **Verbindungsmodus in sekundärer Rolle**  
 Gibt an, ob die Datenbanken eines bestimmten Verfügbarkeitsreplikats, das die sekundäre Rolle einnimmt (das heißt, als sekundäres Replikat dient), Verbindungen von Clients akzeptieren können.  
  
 Die folgenden Werte sind möglich:  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Keine Verbindungen zulassen**|Es sind keine direkten Verbindungen zu den Verfügbarkeitsdatenbanken zulässig, wenn dieses Verfügbarkeitsreplikat als sekundäres Replikat dient. Sekundäre Datenbanken sind nicht für Lesezugriff verfügbar.|  
|**Nur Verbindungen für beabsichtigte Lesevorgänge zulassen**|Es sind nur direkte, schreibgeschützte Verbindungen zulässig, wenn dieses Replikat als sekundäres Replikat dient. Alle Datenbanken im Replikat sind für den Lesezugriff verfügbar.|  
|**Alle Verbindungen zulassen**|Es sind alle Verbindungen zu diesen Datenbanken für den schreibgeschützten Zugriff zulässig, wenn dieses Replikat als sekundäres Replikat dient.|  
  
 **Verbindungsstatus**  
 Gibt an, ob ein sekundäres Replikat derzeit mit dem primären Replikat verbunden ist. Die folgenden Werte sind möglich:  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Nicht verbunden**|Gibt bei einem Remoteverfügbarkeitsreplikat an, dass es vom lokalen Verfügbarkeitsreplikat getrennt ist. Die Antwort des lokalen Replikats auf den Status "Getrennt" hängt wie folgt von dessen Rolle ab:<br /><br /> Wenn auf dem primären Replikat ein sekundäres Replikat getrennt ist, werden die sekundären Datenbanken auf dem primären Replikat als **Nicht synchronisiert** gekennzeichnet, und das primäre Replikat wartet, bis das sekundäre Replikat wieder verbunden ist.<br /><br /> Wird erkannt, dass das sekundäre Replikat getrennt ist, wird versucht, die Verbindung des sekundären Replikats mit dem primären Replikat wiederherzustellen.|  
|**Verbunden**|Ein Remoteverfügbarkeitsreplikat, das derzeit mit dem lokalen Replikat verbunden ist.|  
|**NULL**|Wenn das lokale Replikat ein sekundäres Replikat ist, ist dieser Wert für andere sekundäre Replikate NULL.|  
  
 **Synchronisierungsstatus**  
 Gibt an, ob ein sekundäres Replikat gerade mit dem primärem Replikat synchronisiert wird. Die folgenden Werte sind möglich:  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Nicht synchronisiert**|Die Datenbank ist nicht synchronisiert oder wurde noch nicht mit der Verfügbarkeitsgruppe verknüpft.|  
|**Synchronisiert**|Die Datenbank ist mit der primären Datenbank auf dem aktuellen primären Replikat (falls vorhanden) oder auf dem letzten primären Replikat synchronisiert.<br /><br /> Hinweis: Im Leistungsmodus befindet sich die Datenbank nie im Status „Synchronisiert“.|  
|**NULL**|Unbekannter Status. Dieser Wert tritt auf, wenn die lokale Serverinstanz nicht mit dem WSFC-Failovercluster kommunizieren kann (d. h., der lokale Knoten ist nicht Teil des WSFC-Quorums).|  
  
> [!NOTE]  
>  Informationen zu Leistungsindikatoren für Verfügbarkeitsreplikate finden Sie unter [SQL Server, Verfügbarkeitsreplikat](../../../relational-databases/performance-monitor/sql-server-availability-replica.md).  
  
##  <a name="availability-database-details"></a><a name="AvDbDetails"></a> Verfügbarkeitsdatenbank-Details  
 Im Detailbildschirm **Verfügbarkeitsdatenbank** werden die folgenden Eigenschaften der Verfügbarkeitsdatenbanken in einer bestimmten Verfügbarkeitsgruppe angezeigt:  
  
 **Name**  
 Der Name der Verfügbarkeitsdatenbank.  
  
 **Synchronisierungsstatus**  
 Gibt an, ob die Verfügbarkeitsdatenbank gerade mit dem primärem Replikat synchronisiert wird.  
  
 Folgende Werte sind für den Synchronisierungsstatus möglich:  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|Wird synchronisiert|Die sekundäre Datenbank hat die Transaktionsprotokoll-Datensätze für die primäre Datenbank empfangen, die noch nicht auf den Datenträger geschrieben (festgeschrieben) wurden.<br /><br /> Hinweis: Im Modus mit asynchronem Commit lautet der Synchronisierungsstatus immer **Wird synchronisiert**.|  
|||  
  
 **Angehalten**  
 Gibt an, ob die Verfügbarkeitsdatenbank gerade online ist. Die folgenden Werte sind möglich:  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Angehalten**|Dieser Status gibt an, dass die Datenbank lokal angehalten wird und manuell fortgesetzt werden muss.<br /><br /> Auf dem primären Replikat ist der Wert für eine sekundäre Datenbank unzuverlässig. Um zuverlässig zu bestimmen, ob eine sekundäre Datenbank angehalten wird, fragen Sie sie auf dem sekundären Replikat ab, das die Datenbank hostet.|  
|**Nicht verknüpft**|Gibt an, dass die sekundäre Datenbank entweder nicht mit der Verfügbarkeitsgruppe verknüpft wurde oder aus der Gruppe entfernt wurde.|  
|**Online**|Gibt an, dass die Datenbank nicht auf dem lokalen Verfügbarkeitsreplikat angehalten wird und dass die Datenbank verbunden ist.|  
|**Nicht verbunden**|Gibt an, dass das sekundäre Replikat derzeit keine Verbindung mit dem primären Replikat herstellen kann.|  
  
> [!NOTE]  
>  Weitere Informationen zu Leistungsindikatoren für Verfügbarkeitsdatenbanken finden Sie unter [SQL Server, Datenbankreplikat](../../../relational-databases/performance-monitor/sql-server-database-replica.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [sys.dm_os_performance_counters &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md)   
 [Verwenden des Alway On-Dashboards (SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)   
 [Anzeigen von Verfügbarkeitsgruppeneigenschaften (SQL Server)](../../../database-engine/availability-groups/windows/view-availability-group-properties-sql-server.md)   
 [Anzeigen von Verfügbarkeitsreplikateigenschaften &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/view-availability-replica-properties-sql-server.md)  
  
  
