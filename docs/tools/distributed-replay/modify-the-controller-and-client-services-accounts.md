---
title: Ändern von Controller- und Clientdienstkonten
titleSuffix: SQL Server Distributed Replay
description: Erfahren Sie, wie Sie den Distributed Replay-Controller und die Clientdienstkonten ändern und anschließend die Zugriffssteuerungslisten erneut anwenden.
ms.prod: sql
ms.technology: tools-other
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 85a79a7af034f0a35f64f6d9a0cafeb2233e8418
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839291"
---
# <a name="modify-the-controller-and-client-services-accounts"></a>Ändern von Controller- und Clientdienstkonten

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

In diesem Thema werden Änderungen der Distributed Replay Controller- und Clientdienstkonten und die erneute Anwendung der Zugriffssteuerungslisten (Access Control List, ACL) behandelt.  
  
### <a name="to-start-or-stop-the-distributed-replay-services-using-computer-management"></a>So starten oder beenden Sie die Distributed Replay-Dienste über die Computerverwaltung  
  
1.  Geben Sie auf dem Computer mit den Distributed Replay-Diensten an der Eingabeaufforderung **dcomcnfg** ein.  
  
2.  Doppelklicken Sie auf **Dienste**, scrollen Sie nach unten, und klicken Sie mit der rechten Maustaste auf **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay \<service name>** . Klicken Sie anschließend auf **Start** oder auf **Beenden**.  
  
### <a name="to-modify-the-distributed-replay-controller-service"></a>So ändern Sie den Distributed Replay Controller-Dienst  
  
1.  Beenden Sie den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay Controller-Dienst auf dem Controllercomputer.  
  
2.  Klicken Sie unter **Dienste** mit der rechten Maustaste auf **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay Controller**, und wählen Sie dann **Eigenschaften** aus.  
  
3.  Wählen Sie im Fenster **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay Controller** auf der Registerkarte **Anmelden** die Option **Dieses Konto** aus, geben Sie das neue Anmeldekonto ein, oder klicken Sie auf **Durchsuchen** , um dieses zu suchen. Klicken Sie dann auf **OK**.  
  
     **Wichtig**: Wenn Sie den Distributed Replay-Controller konfigurieren, können Sie mindestens ein Benutzerkonto angeben, das zum Ausführen der Distributed Replay-Clientdienste verwendet wird. Die folgenden Kontotypen werden unterstützt:  
  
    -   Domänenbenutzerkonto  
  
    -   Vom Benutzer erstelltes lokales Benutzerkonto  
  
    -   Administrator  
  
    -   Virtuelles Konto und verwaltetes Dienstkonto (Managed Service Account, MSA)  
  
    -   Netzwerkdienste, lokale Dienste und System  
  
     Gruppenkonten (lokales oder Domänenbenutzerkonto) und andere integrierte Konten (wie "Jeder") werden nicht akzeptiert.  
  
4.  Starten Sie den Distributed Replay Controller-Dienst.  
  
### <a name="to-modify-the-distributed-replay-client-service"></a>So ändern Sie den Distributed Replay Client-Dienst  
  
1.  Bevor Sie den Distributed Replay Client-Dienst ändern, sollten Sie sicherstellen, dass das zu ändernde Clientdienstkonto (auf dem Controllercomputer im CTLRUSERS-Parameter) während des Setupvorgangs angegeben wurde. Wenn das Clientdienstkonto, das Sie ändern, während des Setups nicht angegeben wurde, müssen Sie zuerst die folgenden Schritte ausführen:  
  
    1.  Beenden Sie den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay-Controllerdienst.  
  
    2.  Geben Sie auf dem Controllercomputer, auf dem der Controllerdienst installiert ist, an der Eingabeaufforderung **dcomcnfg** ein.  
  
    3.  Navigieren Sie im Fenster **Komponentendienste** zu **Konsolenstamm > Komponentendienste > Computer > Arbeitsplatz > DCOM Config > DReplayController**.  
  
    4.  Klicken Sie mit der rechten Maustaste auf **DReplayController**, und klicken Sie anschließend auf **Eigenschaften**.  
  
    5.  Klicken Sie im Fenster **Eigenschaften von DReplayController** auf der Registerkarte **Sicherheit** im Abschnitt **Start- und Aktivierungsberechtigungen** auf **Bearbeiten** .  
  
    6.  Erteilen Sie die dem neuen Clientdienst-Anmeldekonto die Berechtigungen **Lokale Aktivierung** und Remoteaktivierung, und klicken Sie dann auf **OK**.  
  
    7.  Klicken Sie im Abschnitt **Zugriffsberechtigungen** auf **Bearbeiten** , gewähren Sie dem neuen Clientdienst-Anmeldekonto die Berechtigungen **Lokaler Zugriff** und Remotezugriff, und klicken Sie dann auf **OK**.  
  
    8.  Klicken Sie auf **OK** , um das Fenster **Eigenschaften von DReplayController** zu schließen.  
  
    9. Fügen Sie auf dem Controllercomputer der Gruppe **Distributed COM-Benutzer** das geänderte Clientdienst-Anmeldekonto hinzu.  
  
    10. Starten Sie den SQL Server Distributed Replay Controller-Dienst.  
  
2.  Beenden Sie den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay Client-Dienst.  
  
3.  Wählen Sie im Fenster **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay Client** auf der Registerkarte **Anmelden** die Option **Dieses Konto** aus, geben Sie das neue Anmeldekonto ein, oder klicken Sie auf **Durchsuchen** , um dieses zu suchen. Klicken Sie dann auf **OK**.  
  
4.  Starten Sie den Distributed Replay Client-Dienst.  
  
  
