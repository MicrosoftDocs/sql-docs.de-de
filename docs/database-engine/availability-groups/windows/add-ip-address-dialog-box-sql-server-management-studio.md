---
title: 'Assistent für Verfügbarkeitsgruppen: Hinzufügen einer IP-Adresse'
description: 'In diesem Artikel werden die Optionen im Dialogfeld „IP-Adresse hinzufügen“ beschrieben, das sich auf der Seite „Replikate angeben“ im Assistenten für Verfügbarkeitsgruppen von SQL Server Management Studio befindet. '
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: end-user-help
f1_keywords:
- sql13.swb.availabilitygrouplistener.addipaddress.f1
ms.assetid: 98c9ad3b-ff3c-4c1d-b344-59a72fca137c
author: cawrites
ms.author: chadam
ms.openlocfilehash: c3ad49873fa56984cbbaf7d4504c5c84cb24be4e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343993"
---
# <a name="add-ip-address-dialog-box-sql-server-management-studio"></a>Dialogfeld IP-Adresse hinzufügen (SQL Server Management Studio)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In diesem F1-Hilfethema werden die Optionen des Dialogfelds **IP-Adresse hinzufügen** beschrieben. Auf dieses Dialogfeld wird über das Dialogfeld **Neuer Verfügbarkeitsgruppenlistener** und über die Registerkarte **Listener** der Seite **Replikate angeben** des [!INCLUDE[ssAoNewAgWiz](../../../includes/ssaonewagwiz-md.md)] oder des [!INCLUDE[ssAoAddRepWiz](../../../includes/ssaoaddrepwiz-md.md)] von [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]zugegriffen.  
  
## <a name="prerequisites"></a>Voraussetzungen  
 Bevor Sie beginnen, einem Verfügbarkeitsgruppenlistener Subnetze hinzuzufügen, stellen Sie sicher, dass Sie die IP-Adresse für jedes Subnetz und bei einer IPv4-Adresse die Subnetzmaske kennen.  
  
##  <a name="add-ip-address-options"></a><a name="PageOptions"></a> Optionen für "IP-Adresse hinzufügen"  
 **Subnetz**  
 Verwenden Sie die Dropdownliste, um eine Adresse für das Subnetz auszuwählen, das Sie dem Verfügbarkeitsgruppenlistener hinzufügen. Standardmäßig besitzt ein Subnetz sowohl eine IPv4-Adresse als auch eine IPv6-Adresse. Beim ersten Verwenden des Dialogfelds **IP-Adresse hinzufügen** zeigt die Dropdownliste **Subnetz** beide Subnetzadressen für jedes Subnetz an, das ein Replikat für die Verfügbarkeitsgruppe hostet. Um dem Listener ein angegebenes Subnetz hinzuzufügen, wählen Sie eine der Subnetzadressen aus.  
  
 Nachdem Sie das Dialogfeld **IP-Adresse hinzufügen** abgeschlossen und auf **OK** geklickt haben, um dem Listener eine ausgewählte Subnetzadresse hinzuzufügen, filtert die Dropdownliste **Subnetz** diese Subnetzadresse heraus. Alle nicht ausgewählten Subnetzadressen verbleiben in der Dropdownliste. Stellen Sie sicher, dass Sie dem Listener nur genau eine Subnetzadresse pro Subnetz hinzufügen, anderenfalls schlägt die Listenererstellung fehl.  
  
 **Adressen**  
 Verwenden Sie dieses Feld, um eine statische IP-Adresse für die ausgewählte Subnetzadresse einzugeben. Wenden Sie sich an Ihren Netzwerkadministrator, um diese IP-Adresse zu erhalten. Stellen Sie sicher, dass Sie eine gültige Adresse für die ausgewählte Subnetzadresse eingeben, anderenfalls schlägt die Listenererstellung fehl.  
  
 **IPv4-Adresse**  
 Wenn Sie die IPv4-Subnetzadresse eines Subnetzes ausgewählt haben, geben Sie hier eine gültige statische IPv4-Adresse ein.  
  
 **Subnetzmaske**  
 Bei einer IPv4-Adresse zeigt dieses schreibgeschützte Feld die Subnetzmaske des ausgewählten Subnetzes an.  
  
 **IPv6-Adresse**  
 Wenn Sie die IPv6-Subnetzadresse eines Subnetzes ausgewählt haben, geben Sie hier eine gültige statische IPv6-Adresse ein.  
  
 **OK**  
 Klicken Sie hier, um das Subnetz, dessen Adresse Sie ausgewählt haben, mit der angegebenen statischen IP-Adresse hinzuzufügen. Dem Subnetzraster des Dialogfelds **Neuer Verfügbarkeitsgruppenlistener** oder **Replikate angeben** wird eine Zeile mit diesen Werten hinzugefügt.  
  
> [!IMPORTANT]  
>  Im Dialogfeld **IP-Adresse hinzufügen** wird die IP-Adresse nicht überprüft. Auch verhindert das Dialogfeld nicht, dass Sie die zweite Subnetzadresse für ein Subnetz hinzufügen, das Sie bereits dem Verfügbarkeitsgruppenlistener hinzugefügt haben.  
  
 **Abbrechen**  
 Klicken Sie, um die Auswahl abzubrechen und zum Dialogfeld **Neuer Verfügbarkeitsgruppenlistener** oder zur Registerkarte **Listener** zurückzukehren, ohne eine statische IP-Adresse für ein Subnetz hinzuzufügen.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
  
-   [Erstellen oder Konfigurieren eines Verfügbarkeitsgruppenlisteners &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
-   [Verwenden des Dialogfelds Neue Verfügbarkeitsgruppe &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
-   [Verwenden des Assistenten zum Hinzufügen von Replikaten zu Verfügbarkeitsgruppen &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Übersicht über AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Verfügbarkeitsgruppenlistener, Clientkonnektivität und Anwendungsfailover &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)   
 [AlwaysOn-Clientkonnektivität &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-client-connectivity-sql-server.md)  
  
  
