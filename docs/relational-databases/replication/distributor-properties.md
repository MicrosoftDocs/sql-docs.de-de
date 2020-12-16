---
title: Verteilereigenschaften (Dialogfeld)
description: Hier werden die unterschiedlichen Seiten des Dialogfelds „Verteilereigenschaften“ in SQL Server Management Studio (SSMS) beschrieben.
ms.custom: seo-lt-2019
ms.date: 11/20/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.configdistwizard.distdbproperties.f1
- sql13.rep.configdistwizard.distproperties.general.f1
- sql13.rep.configdistwizard.distproperties.publishers.f1
ms.assetid: f643c7c3-f238-4835-b81e-2c2b3b53b23f
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 51e617c26aad4261d0d337ff2fd2a3dcd19c79b8
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97484862"
---
# <a name="sql-server-replication-distributor-properties-dialog-box"></a>Dialogfeld „Verteilereigenschaften“ für die SQL Server-Replikation 
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

Auf dieser Seite werden die Seiten im Dialogfeld „Verteilereigenschaften“ beschrieben. 

## <a name="general"></a>Allgemein
Mithilfe der Seite **Allgemein** des Dialogfelds **Verteilereigenschaften** können Sie Verteilungsdatenbanken hinzufügen und löschen sowie Eigenschaften von Verteilungsdatenbanken festlegen.  
  
 Die Verteilungsdatenbank speichert Metadaten und Verlaufsdaten für alle Replikationstypen und Transaktionen für die Transaktionsreplikation. In vielen Fällen reicht eine Verteilungsdatenbank aus. Wenn jedoch mehrere Verleger einen Verteiler verwenden, sollten Sie die Erstellung einer Verteilungsdatenbank für jeden Verleger in Betracht ziehen. Auf diese Weise stellen Sie sicher, dass die durch jede Verteilungsdatenbank fließenden Daten eindeutig sind.  

 **Datenbanken**  
 Das Eigenschaftenraster **Datenbanken** zeigt den Namen und die Beibehaltungseigenschaften der Verteilungsdatenbanken auf dem Verteiler an. **Transaktionsbeibehaltung** ist die Dauer, die Transaktionen für die Transaktionsreplikation gespeichert werden (die Transaktionsbeibehaltung wird auch als Verteilungsbeibehaltung bezeichnet). **Verlaufsbeibehaltung** ist die Dauer, die Metadaten des Verlaufs für jede Art von Replikation gespeichert werden. Weitere Informationen zu Beibehaltungsdauer finden Sie unter [Abonnementablauf und -deaktivierung](../../relational-databases/replication/subscription-expiration-and-deactivation.md).  
  
 Klicken Sie im Eigenschaftenraster **Datenbanken** auf die Eigenschaftenschaltfläche (die Schaltfläche mit den **drei Punkten**), um das Dialogfeld **Eigenschaften der Verteilungsdatenbank** zu öffnen.  
  
 **Neu**  
 Erstellt eine neue Verteilungsdatenbank  
  
 **Löschen**  
 Wählen Sie im Eigenschaftenraster **Datenbanken** eine vorhandene Verteilungsdatenbank aus, und klicken Sie auf **Löschen** , um die Datenbank zu löschen. Die Verteilungsdatenbank kann nicht gelöscht werden, wenn es die einzige Datenbank dieser Art ist. Jeder Verteiler muss zumindest eine Verteilungsdatenbank besitzen. Zum Löschen aller Verteilungsdatenbanken müssen Sie die Verteilung auf dem Computer deaktivieren. Weitere Informationen finden Sie unter [Deaktivieren der Veröffentlichung und Verteilung](../../relational-databases/replication/disable-publishing-and-distribution.md).  
  
 **Profilstandards**  
 Klicken Sie auf diese Option, um auf die Replikations-Agentprofile im Dialogfeld **Agentprofile** zuzugreifen. Weitere Informationen zu Profilen finden Sie unter [Replication Agent Profiles](../../relational-databases/replication/agents/replication-agent-profiles.md).  

## <a name="publishers"></a>Herausgeber
Mithilfe der Seite **Verleger** des Dialogfelds **Verteilereigenschaften** können Sie es Verlegern ermöglichen, diesen Verteiler zu verwenden. Sie können auch zu diesen Verlegern gehörige Eigenschaften festlegen. Beachten Sie, dass dieser Server nicht zu einem Verleger wird, wenn Sie es einem Verleger ermöglichen, diesen Server als Verteiler zu verwenden. Sie müssen eine Verbindung mit dem Verleger herstellen, ihn für das Veröffentlichen konfigurieren und diesen Server als Verteiler auswählen. Sie können den Verleger konfigurieren und mithilfe des Assistenten für neue Veröffentlichung einen Verteiler auswählen.  
  
 **Verleger**  
 Wählen Sie die Server aus, die diesen Verteiler verwenden dürfen. Klicken Sie auf die Eigenschaftenschaltfläche (die Schaltfläche mit den **drei Punkten**) neben einem Verleger, um zusätzliche Eigenschaften anzuzeigen und festzulegen.  
  
 **Add (Hinzufügen)**  
 Wenn der von Ihnen gewünschte Server nicht in der Liste enthalten ist, klicken Sie auf **Hinzufügen**, um einen [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Verleger oder einen Oracle-Verleger zur Liste der verfügbaren Verleger hinzuzufügen. Wenn der von Ihnen hinzugefügte Server der erste Server ist, der diesen Verteiler als Remoteverteiler verwendet, werden Sie aufgefordert, ein Kennwort für administrative Verbindungen einzugeben.  
  
 **Kennwort für administrative Verbindung**  
 Verwenden Sie diese Option, um das Kennwort für die Verbindung zu aktualisieren, die die Replikation zwischen dem Verleger und dem Remoteverteiler mithilfe der **distributor_admin** -Anmeldung herstellt:  
  
-   Wenn der Verteiler nur als lokaler Verteiler dient, wird dieses Kennwort per Zufallsgenerator erstellt und automatisch konfiguriert.   
-   Wenn der Verteiler bereits einen Remoteverleger besitzt, wurde ein Kennwort bereits ursprünglich auf dieser Seite oder auf der Seite **Verteilerkennwort** des Verteilungskonfigurations-Assistenten angegeben.    
-   Wenn Sie den ersten Remoteverleger für diesen Verleger aktivieren, werden Sie aufgefordert, ein Kennwort einzugeben.  Weitere Informationen zur Sicherheit für Verteiler finden Sie unter [Schützen des Verteilers](../../relational-databases/replication/security/secure-the-distributor.md).  

## <a name="distribution-database"></a>Verteilungsdatenbank 
 Mithilfe des Dialogfelds **Eigenschaften der Verteilungsdatenbank** können Sie eine Reihe von Eigenschaften anzeigen und die Transaktionsbeibehaltungsdauer und die Aufbewahrungsdauer für den Verlauf für die Datenbank festlegen.  
  
 **Name**  
 Der Name der Verteilungsdatenbank. Der Standardwert ist 'distribution' (schreibgeschützt).  
  
 **Dateipfade**  
 Der Speicherort der Datenbankdatei und der Protokolldatei (schreibgeschützt).  
  
 **Transaktionsbeibehaltungsdauer**  
 Wird auch als Beibehaltungsdauer für die Verteilung bezeichnet. Der Zeitraum, für den Transaktionen für die Transaktionsreplikation gespeichert werden. Weitere Informationen finden Sie unter [Subscription Expiration and Deactivation](../../relational-databases/replication/subscription-expiration-and-deactivation.md).  
  
 **Aufbewahrungsdauer für Verlauf**  
 Der Zeitraum, für den die Metadaten des Verlaufs für jede Art von Replikation gespeichert werden.  
  
 **Sicherheit für den Warteschlangenlese-Agent**  
 Der Warteschlangenlese-Agent wird von Transaktionsreplikationen verwendet, die Abonnements mit verzögertem Update über eine Warteschlange zulassen. Ein Warteschlangenlese-Agent wird automatisch erstellt, wenn Sie die Option **Transaktionsveröffentlichung mit Updateabonnements** auf der Seite **Veröffentlichungstyp** des Assistenten für neue Veröffentlichung auswählen. Klicken Sie auf **Sicherheitseinstellungen…** , um das Konto zu ändern, unter dem der Agent ausgeführt wird und Verbindungen mit dem Verteiler herstellt.  
  
 Eine Warteschlangenlese-Agent kann auch erstellt werden, indem Sie auf dieser Seite die Option **Warteschlangenlese-Agent erstellen** auswählen (diese Option ist nicht verfügbar, wenn der Agent bereits erstellt wurde).  
  
 Für den Warteschlangenlese-Agent werden zusätzliche Verbindungsinformationen an zwei Stellen angegeben:    
-   Die Verbindung mit dem Verleger stellt der Agent mithilfe der Anmeldeinformationen her, die im Dialogfeld **Verlegereigenschaften** angegeben sind. Es ist auf der Seite **Verleger** des Dialogfelds **Verteilereigenschaften** verfügbar.    
-   Die Verbindung mit dem Abonnenten stellt der Agent mithilfe der Anmeldeinformationen her, die für den Verteilungs-Agent im Assistenten für neue Abonnements angegeben sind.  Weitere Informationen finden Sie unter  [Replication Agent Security Model](../../relational-databases/replication/security/replication-agent-security-model.md). 
  
## <a name="see-also"></a>Weitere Informationen  
 [Verteilung konfigurieren](../../relational-databases/replication/configure-distribution.md)   
 [Anzeigen und Ändern der Verteiler- und Verlegereigenschaften](../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md)   
  
  
