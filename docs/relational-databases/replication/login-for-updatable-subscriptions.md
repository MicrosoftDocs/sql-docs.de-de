---
description: Anmeldename für aktualisierbare Abonnements
title: Anmeldename für aktualisierbare Abonnements | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/25/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.newsubwizard.updatablesubscriptionslogin.f1
ms.assetid: 301ea227-0455-40ba-9009-d38f8676b325
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 81a455310e419eaac0136721d021e700fa29f136
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88493929"
---
# <a name="login-for-updatable-subscriptions"></a>Anmeldename für aktualisierbare Abonnements
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Wenn Sie im Assistenten auf der Seite **Aktualisierbare Abonnements** die Option **Replizieren** ausgewählt haben, müssen Sie mit dem Abonnenten ein Konto angeben, unter dem die Verbindungen mit dem Verleger für das sofortige Update hergestellt werden. 
  
 Die Verbindungen werden durch die Trigger verwendet, die auf dem Abonnenten ausgelöst werden und die Änderungen zum Verleger weitergeben. Dieses Konto ist erforderlich, auch wenn Sie **Änderungen in die Warteschlange einreihen und Commit baldmöglichst ausführen** auf der Seite **Aktualisierbare Abonnements** ausgewählt haben. Der Assistent für neue Abonnements konfiguriert standardmäßig verzögerte Updates über eine Warteschlange mit der Möglichkeit, zur sofortigen Aktualisierung zu wechseln.  
  
> **WICHTIG!** Dem für die Verbindung angegebenen Konto sollten nur die Berechtigung zum Einfügen, Aktualisieren und Löschen der Daten in den durch die Replikation in der Veröffentlichungsdatenbank erstellten Sichten erteilt werden. Darüber hinaus sollte das Konto über keine weiteren Berechtigungen verfügen. Gewähren Sie dem Konto, das Sie für den jeweiligen Abonnenten konfiguriert haben, Berechtigungen für die Sichten in der Veröffentlichungsdatenbank, deren Benennung das Format **syncobj_** _\<HexadecimalNumber>_ aufweist.  
  
 Für den Typ der Verbindung gibt es drei Optionen:  
  
-   Ein vordefinierter Verbindungsserver oder Remoteserver.  
  
-   Ein durch die Replikation erstellter Verbindungsserver. Die Verbindung wird mit den Anmeldeinformationen hergestellt, die Sie im Assistenten angeben.  
  
-   Ein durch die Replikation erstellter Verbindungsserver. Die Verbindung wird mit den Anmeldeinformationen des Benutzers hergestellt, der die Änderung auf dem Abonnenten vornimmt.  
  
 Die ersten beiden Optionen können im Assistenten angegeben werden. Die letzte Option kann nur mithilfe von [sp_link_publication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-link-publication-transact-sql.md) angegeben werden. Geben Sie für den Parameter `@security_mode` den Wert **1** an.  
  
## <a name="options"></a>Tastatur  
 **Erstellen Sie einen Verbindungsserver, der die Verbindung mithilfe des folgenden Anmeldenamens für die SQL Server-Authentifizierung herstellt:**  
 Durch die Replikation wird ein Verbindungsserver mithilfe der in den Feldern **Anmeldename** und **Kennwort** angegebenen Anmeldeinformationen erstellt.  
  
 **Anmeldung**  
 Geben Sie einen [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldenamen ein, der nur die in diesem Thema beschriebenen Berechtigungen aufweist.  
  
 **Kennwort**  
 Geben Sie ein sicheres Kennwort für den in **Anmeldename**angegebenen Anmeldenamen ein.  
    
 **Vordefinierten Verbindungsserver oder Remoteserver verwenden**  
 Bei dieser Option ist ein bereits von Ihnen definierter Verbindungsserver oder Remoteserver erforderlich. Weitere Informationen finden Sie unter [Verbindungsserver &#40;Datenbank-Engine&#41;](../../relational-databases/linked-servers/linked-servers-database-engine.md) und [Remoteserver](../../database-engine/configure-windows/remote-servers.md). Stellen Sie sicher, dass der für den Verbindungsserver oder Remoteserver verwendete Anmeldename ein sicheres Kennwort sowie ausschließlich die in diesem Thema beschriebenen Berechtigungen aufweist.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Create an Updatable Subscription to a Transactional Publication](publish/create-an-updatable-subscription-to-a-transactional-publication.md)   
 [Anzeigen und Ändern von Replikationssicherheitseinstellungen](../../relational-databases/replication/security/view-and-modify-replication-security-settings.md)   
 [Updatable Subscriptions for Transactional Replication](../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md)   
 [Abonnieren von Veröffentlichungen](../../relational-databases/replication/subscribe-to-publications.md)  
  
  
