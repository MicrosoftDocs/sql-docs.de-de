---
title: Konfigurieren vordefinierter Replikationswarnungen (SSMS)
description: Hier erfahren Sie, wie Sie mit SQL Server Management Studio (SSMS) vordefinierte Replikationswarnungen konfigurieren.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- alerts [SQL Server replication]
- predefined replication alerts [SQL Server replication]
ms.assetid: c0414147-7ffe-4f9a-908c-71c1b5201584
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||=sqlallproducts-allversions
ms.openlocfilehash: 1dd36895604f81e378b37d2d711c814251ee7cd7
ms.sourcegitcommit: 783b35f6478006d654491cb52f6edf108acf2482
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91892420"
---
# <a name="configure-predefined-replication-alerts-sql-server-management-studio"></a>Konfigurieren von vordefinierten Replikationswarnungen (SQL Server Management Studio)
[!INCLUDE[applies-to-version/_ssnoversion.md](../../../includes/applies-to-version/sqlserver.md)]
  Die Replikation bietet die folgenden vordefinierten Warnungen, die für das Reagieren auf die folgenden Replikationsereignisse konfiguriert werden können:  
  
-   **Replikation: Der Agent war erfolgreich.**  
  
-   **Replikation: Fehler beim Agent.**  
  
-   **Replikation: Wiederholung des Agents.**  
  
-   **Replikation: Abgelaufenes Abonnement wurde gelöscht.**  
  
-   **Replikation: Das Abonnement wurde nach einem Überprüfungsfehler neu initialisiert.**  
  
-   **Replikation: Fehler bei der Datenüberprüfung auf dem Abonnenten.**  
  
-   **Replikation: Der Abonnent hat die Datenüberprüfung erfolgreich durchlaufen.**  
  
-   **Replikation: Der Agent wurde benutzerdefiniert heruntergefahren.**  
  
 Konfigurieren Sie diese Warnungen im Ordner **Warnungen** in [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] oder auf der Registerkarte **Warnungen** im Replikationsmonitor. Weitere Informationen zum Zugreifen auf diese Registerkarte finden Sie unter [View information and perform tasks using Replication Monitor (Anzeigen von Informationen und Ausführen von Aufgaben mit dem Replikationsmonitor)](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md).  
  
 Zusätzlich zu diesen Warnungen bietet der Replikationsmonitor eine Reihe von status- und leistungsbezogenen Warnungen. Weitere Informationen finden Sie unter [Set Thresholds and Warnings in Replication Monitor](../../../relational-databases/replication/monitor/set-thresholds-and-warnings-in-replication-monitor.md). Mithilfe der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Infrastruktur für Warnungen können Sie außerdem auch Warnungen für andere Replikationsereignisse definieren. Weitere Informationen finden Sie unter [Erstellen eines benutzerdefinierten Ereignisses](../../../ssms/agent/create-a-user-defined-event.md).  
  
### <a name="to-configure-a-predefined-replication-alert-in-management-studio"></a>So konfigurieren Sie eine vordefinierte Replikationswarnung in SQL Server Management Studio  
  
1.  Stellen Sie in [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)]eine Verbindung mit dem Verteiler her, und erweitern Sie dann den Serverknoten.  
  
2.  Erweitern Sie den Ordner **SQL Server-Agent** , und erweitern Sie dann den Ordner **Warnungen** .  
  
3.  Klicken Sie mit der rechten Maustaste auf eine Replikationswarnung, und klicken Sie dann auf **Eigenschaften**.  
  
4.  Legen Sie im Dialogfeld **Eigenschaften von Warnung "\<AlertName>"** die gewünschten Optionen fest:  
  
    -   Klicken Sie auf der Seite **Allgemein** auf **Aktivieren**, und geben Sie an, für welche Datenbank die Warnung gelten soll.  
  
    -   Geben Sie auf der Seite **Antwort** an, ob eine E-Mail gesendet und/oder ein Auftrag ausgeführt werden soll.  
  
         Wenn die Warnung **Replikation: Fehler bei der Datenüberprüfung auf dem Abonnenten.** lautet, können Sie den Auftrag festlegen, den die Replikation als Antwort auf diese Warnung bereitstellt: Klicken Sie auf **Auftrag ausführen** und anschließend auf die Schaltfläche zum Durchsuchen ( **...** ). Klicken Sie im Dialogfeld **Auftrag suchen** auf **Durchsuchen**. Wählen Sie im Dialogfeld **Nach Objekten suchen** die Option **Abonnements mit Datenüberprüfungsfehlern neu initialisieren**aus. Klicken Sie in beiden geöffneten Dialogfeldern auf **OK** . Wenn der Auftrag ausgeführt wird, wird ein Remoteprozeduraufruf (Remote Procedure Call, RPC) einer gespeicherten Prozedur verwendet, mit dem das Abonnement erneut initialisiert wird. Wenn der Verleger einen Remoteverteiler verwendet, müssen Sie auf dem Verleger eine Remoteserveranmeldung definieren, damit der RPC des Verteilers an den Verleger ausgeführt werden kann.  
  
    -   Passen Sie auf der Seite **Optionen** den Text der Antwort an.  
  
5.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  

### <a name="to-configure-an-alert-for-a-threshold-in-replication-monitor"></a>So konfigurieren Sie eine Warnung für einen Schwellenwert im Replikationsmonitor  
  
1.  Klicken Sie auf der Registerkarte **Warnungen** auf **Warnungen konfigurieren**.  
  
2.  Wählen Sie im Dialogfeld **Replikationswarnungen konfigurieren** eine Warnung aus, und klicken Sie dann auf **Konfigurieren**.  
  
3.  Legen Sie im Dialogfeld **Eigenschaften von Warnung "\<AlertName>"** die gewünschten Optionen fest:  
  
    -   Klicken Sie auf der Seite **Allgemein** auf **Aktivieren**, und geben Sie an, für welche Datenbank die Warnung gelten soll.  
  
    -   Geben Sie auf der Seite **Antwort** an, ob eine E-Mail gesendet und/oder ein Auftrag ausgeführt werden soll.  
  
         Wenn die Warnung **Replikation: Fehler bei der Datenüberprüfung auf dem Abonnenten.** lautet, können Sie den Auftrag festlegen, den die Replikation als Antwort auf diese Warnung bereitstellt: Klicken Sie auf **Auftrag ausführen** und anschließend auf die Schaltfläche zum Durchsuchen ( **...** ). Klicken Sie im Dialogfeld **Auftrag suchen** auf **Durchsuchen**. Wählen Sie im Dialogfeld **Nach Objekten suchen** die Option **Abonnements mit Datenüberprüfungsfehlern neu initialisieren**aus. Klicken Sie in beiden geöffneten Dialogfeldern auf **OK** . Wenn der Auftrag ausgeführt wird, wird ein Remoteprozeduraufruf (Remote Procedure Call, RPC) einer gespeicherten Prozedur verwendet, mit dem das Abonnement erneut initialisiert wird. Wenn der Verleger einen Remoteverteiler verwendet, müssen Sie auf dem Verleger eine Remoteserveranmeldung definieren, damit der RPC des Verteilers an den Verleger ausgeführt werden kann.  
  
    -   Passen Sie auf der Seite **Optionen** den Text der Antwort an.  
  
4.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
5.  Klicken Sie auf **Schließen**.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Verwenden von Warnungen für Replikations-Agentereignisse](../../../relational-databases/replication/agents/use-alerts-for-replication-agent-events.md)  
  
