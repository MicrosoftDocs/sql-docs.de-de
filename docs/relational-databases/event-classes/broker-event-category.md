---
description: Broker (Ereigniskategorie)
title: Broker (Ereigniskategorie) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 05/24/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- SQL Server event classes, Broker event category
- Broker event category [SQL Server]
- event classes [SQL Server], Broker event category
ms.assetid: 470dc93c-0dda-4d89-829b-937738d59b31
author: stevestein
ms.author: sstein
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: bbf57d56e65e00221fba35449898c9bc23d70529
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99111823"
---
# <a name="broker-event-category"></a>Broker (Ereigniskategorie)

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

 Die **Broker**-Ereigniskategorie enthält allgemeine Service Broker-Ereignisse.  
  
## <a name="in-this-section"></a>In diesem Abschnitt  
  
|Thema|BESCHREIBUNG|  
|-----------|-----------------|  
|[Broker:Activation (Ereignisklasse)](../../relational-databases/event-classes/broker-activation-event-class.md)|Ein Ereignis, das generiert wird, wenn durch eine Warteschlangenüberwachung eine gespeicherte Aktivierungsprozedur gestartet wird.|  
|[Broker:Connection-Ereignisklasse](../../relational-databases/event-classes/broker-connection-event-class.md)|Ein Ereignis, das generiert wird, um den Status einer von Service Broker verwalteten Transportverbindung zu melden.|  
|[Broker:Conversation-Ereignisklasse](../../relational-databases/event-classes/broker-conversation-event-class.md)|Ein Ereignis, das generiert wird, um den Fortschritt einer Konversation zu melden.|  
|[Broker:Conversation Group-Ereignisklasse](../../relational-databases/event-classes/broker-conversation-group-event-class.md)|Ein Ereignis, das generiert wird, wenn die Datenbank eine Konversationsgruppe erstellt oder löscht.|  
|[Broker:Corrupted Message (Ereignisklasse)](../../relational-databases/event-classes/broker-corrupted-message-event-class.md)|Ein Ereignis, das zum Angeben einer fehlerhaften, von der Datenbank erhaltenen Nachricht generiert wird.|  
|[Broker:Forwarded Message Dropped-Ereignisklasse](../../relational-databases/event-classes/broker-forwarded-message-dropped-event-class.md)|Ein Ereignis, das generiert wird, wenn SQL Server eine Service Broker-Nachricht löscht, die hätte weitergeleitet werden sollen.|  
|[Broker:Forwarded Message Sent-Ereignisklasse](../../relational-databases/event-classes/broker-forwarded-message-sent-event-class.md)|Ein Ereignis, das generiert wird, wenn SQL Server eine Service Broker-Nachricht weiterleitet.|  
|[Broker:Message Classify (Ereignisklasse)](../../relational-databases/event-classes/broker-message-classify-event-class.md)|Ein Ereignis, das generiert wird, wenn Service Broker das Routing für eine Nachricht bestimmt.|  
|[Broker:Message Drop (Ereignisklasse)](../../relational-databases/event-classes/broker-message-drop-event-class.md)|Ein Ereignis, das generiert wird, wenn Service Broker eine empfangene Nachricht nicht beibehalten kann, die in dieser Instanz an einen Dienst weitergeleitet werden sollte.|  
|[Broker:Remote Message Ack (Ereignisklasse)](../../relational-databases/event-classes/broker-remote-message-ack-event-class.md)|Ein Ereignis, das generiert wird, wenn Service Broker eine Nachrichtenbestätigung sendet oder empfängt.|  
  
 Service Broker bietet auch zwei Sicherheitsüberwachungsereignisse. Weitere Informationen zu diesen Ereignissen finden Sie unter [Audit Broker Login (Ereignisklasse)](../../relational-databases/event-classes/audit-broker-login-event-class.md) und [Audit Broker Conversation (Ereignisklasse)](../../relational-databases/event-classes/audit-broker-conversation-event-class.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Sicherheitsüberwachung-Ereigniskategorie](/analysis-services/trace-events/security-audit-event-category)  
  
