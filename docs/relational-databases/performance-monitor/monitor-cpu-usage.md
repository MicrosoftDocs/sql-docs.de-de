---
title: Überwachen der CPU-Auslastung | Microsoft-Dokumentation
description: Überwachen einer SQL Server-Instanz, um zu bestimmen, ob die CPU-Auslastungsraten in einem normalen Bereich liegen. Verwenden Sie den Systemmonitor, um festzustellen, wie lange die CPU für das Ausführen eines Threads benötigt.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- monitoring performance [SQL Server], CPU usage
- tuning databases [SQL Server], CPU usage
- processors [SQL Server], monitoring usage
- database performance [SQL Server], CPU usage
- monitoring CPU usage [SQL Server]
- server performance [SQL Server], CPU usage
- database monitoring [SQL Server], CPU usage
- monitoring [SQL Server], CPU usage
- processors [SQL Server]
- CPU [SQL Server], monitoring
- monitoring server performance [SQL Server], CPU usage
ms.assetid: 2a02a3b6-07b2-4ad0-8a24-670414d19812
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 885375f935459acc0d699870a95db706f38ecd87
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505993"
---
# <a name="monitor-cpu-usage"></a>Überwachen der CPU-Auslastung
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Überwachen Sie eine Instanz von Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in regelmäßigen Abständen, um sicherzustellen, dass sich die CPU-Nutzungsraten im Normalbereich bewegen. Eine konstant hohe CPU-Nutzungsrate kann ein Anzeichen dafür sein, dass die CPU aktualisiert werden muss oder weitere Prozessoren hinzugefügt werden müssen. Darüber hinaus kann eine hohe CPU-Nutzungsrate auf eine schlecht angepasste oder entwickelte Anwendung hinweisen. Eine Optimierung der Anwendung kann die CPU-Nutzung senken.  
  
 Um die CPU-Nutzungsrate festzustellen, rufen Sie am besten im Systemmonitor den Leistungsindikator **Prozessor: Prozessorzeit (%)** auf. Dieser Leistungsindikator überwacht die Zeit, die die CPU zur Verarbeitung eines Threads benötigt, der sich nicht im Leerlauf befindet. Ein konstanter Status von 80-90 % kann darauf hinweisen, dass ein CPU-Upgrade notwendig ist oder weitere Prozessoren hinzugefügt werden müssen. Bei Multiprozessorsystemen sollte für jeden Prozessor eine separate Instanz dieses Leistungsindikators überwacht werden. Dieser Wert stellt die Summe der Prozessorzeit für einen bestimmten Prozessor dar. Um den Durchschnitt für alle Prozessoren zu ermitteln, müssen Sie dagegen den Leistungsindikator **System: Gesamtprozessorzeit (%)** aufrufen.  
  
 Darüber hinaus können Sie die Prozessornutzung aber auch über die folgenden Leistungsindikatoren überwachen:  
  
-   **Prozessor: Privilegierte Zeit (%)**  
  
     Gibt den prozentualen Zeitanteil an der Gesamtzeit an, die der Prozessor benötigt, um Microsoft Windows-Kernelbefehle, wie die Verarbeitung von E/A-Anforderungen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , auszuführen. Sollte dieser Leistungsindikator bei hohen Werten für die Leistungsindikatoren **Physischer Datenträger** gleich bleibend hoch sein, sollten Sie die Installation eines schnelleren oder effizienteren Datenträgersubsystems in Erwägung ziehen.  
  
    > [!NOTE]  
    >  Unterschiedliche Datenträgercontroller und -treiber benötigen unterschiedlich viel Zeit für die Kernelverarbeitung. Effiziente Controller und Treiber beanspruchen weniger privilegierte Zeit und überlassen Benutzeranwendungen so mehr Verarbeitungszeit, wodurch der Durchsatz insgesamt steigt.  
  
-   **Prozessor: Benutzerzeit (%)**  
  
     Gibt den prozentualen Zeitanteil an der Gesamtzeit an, die der Prozessor benötigt, um Benutzerprozesse wie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]auszuführen.  
  
-   **System: Prozessor-Warteschlangenlänge**  
  
     Gibt die Anzahl der Threads an, die auf Prozessorzeit warten. Ein Prozessorengpass entsteht, wenn die Threads eines Prozesses mehr Prozessorzyklen benötigen, als zur Verfügung stehen. Wenn viele Prozesse versuchen, Prozessorzeit zu beanspruchen, müssen Sie ggf. einen schnelleren Prozessor installieren. Bei einem Multiprozessorsystem könnten Sie auch einen Prozessor hinzufügen.  
  
 Bei der Überprüfung der Prozessornutzung müssen Sie auch die Art der Vorgänge berücksichtigen, die durch die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ausgeführt werden. Wenn in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viele Berechnungen ausgeführt werden, wie etwa Abfragen, die Aggregate enthalten, oder arbeitsspeichergestützte Abfragen, bei denen keine Datenträger-E/A notwendig ist, können durchaus 100 % der Prozessorzeit beansprucht werden. Wenn dies bewirkt, dass die Leistung anderer Anwendungen leidet, versuchen Sie, die Arbeitsauslastung zu ändern. Reservieren Sie beispielsweise den Computer für das Ausführen der Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Nutzungsraten um 100 %, wenn viele Clientanforderungen verarbeitet werden, können darauf hinweisen, dass Prozesse in Warteschlangen angeordnet werden, auf Prozessorzeit warten und einen Engpass verursachen. Lösen Sie das Problem, indem Sie das System durch leistungsstärkere Prozessoren ergänzen.  
  
  
