---
title: Starten des Datenbankspiegelungs-Monitors (SSMS)
description: Hier wird beschrieben, wie der Datenbankspiegelungs-Monitor in der grafischen Benutzeroberfläche von SQL Server Management Studio (SSMS) gestartet wird.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- monitoring database mirroring [SQL Server]
- Database Mirroring Monitor [SQL Server], starting
- database mirroring [SQL Server], monitoring
ms.assetid: 53165335-97ca-4f88-8e78-22f1839dee98
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: bb2ffa0d767bd6105a83322f9adf2c3803ba6d50
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352238"
---
# <a name="start-database-mirroring-monitor-sql-server-management-studio"></a>Starten des Datenbankspiegelungs-Monitors (SQL Server Management Studio)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Der Datenbankspiegelungs-Monitor ist Teil des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Monitors, der aus [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]gestartet wird.  
  
> [!NOTE]
>  Der Datenbankspiegelungs-Monitor ist nicht in jeder Edition von [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Eine Liste der Funktionen, die von den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Editionen unterstützt werden, finden Sie unter [Von den SQL Server 2016-Editionen unterstützte Funktionen](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
### <a name="to-launch-the-database-mirroring-monitor"></a>So starten Sie den Datenbankspiegelungs-Monitor  
  
1.  Nachdem Sie eine Verbindung mit der Prinzipalserverinstanz hergestellt haben, klicken Sie im Objekt-Explorer auf den Servernamen, um die Serverstruktur zu erweitern.  
  
2.  Erweitern Sie **Datenbanken**, und wählen Sie die zu überwachende Datenbank aus.  
  
3.  Klicken Sie mit der rechten Maustaste auf die Datenbank, wählen Sie **Tasks** aus, und klicken Sie dann auf **Datenbankspiegelungs-Monitor starten**.  
  
4.  Klicken Sie im Dialogfeld **Datenbankspiegelungs-Monitor** auf **Gespiegelte Datenbank registrieren** , um eine oder mehrere gespiegelte Datenbanken zu registrieren.  
  
    > [!NOTE]  
    >  Wenn Sie eine Datenbank bei einem der Partner registrieren, wird sie automatisch auch bei dem anderen Partner registriert. Wenn der Monitor bereits über Verbindungsanmeldeinformationen für die andere Partnerinstanz verfügt, werden diese für die Herstellung der Verbindung verwendet. Andernfalls versucht der Monitor, die Verbindung mithilfe der Windows-Authentifizierung herzustellen. Wenn Sie für das Herstellen der Verbindung mit einer der Serverinstanzen andere Anmeldeinformationen verwenden möchten, klicken Sie auf **Beim Klicken auf 'OK' das Dialogfeld 'Serververbindungen verwalten' anzeigen**.  
  
 Weitere Informationen zur Datenbankspiegelung finden Sie unter [Datenbankspiegelungs-Monitor (Übersicht)](../../database-engine/database-mirroring/database-mirroring-monitor-overview.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Datenbankspiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [Einrichten einer Datenbank-Spiegelungssitzung mithilfe der Windows-Authentifizierung &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
  
