---
title: Ändern des Proxykontos für den Hilfsprogramm-Sammlungssatz auf einer verwalteten Instanz von SQL Server | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie SQL Server Management Studio verwenden, um das Proxykonto für den Hilfsprogramm-Sammlungssatz auf einer verwalteten Instanz von SQL Server zu ändern.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
ms.assetid: ff37ba8b-a08c-4109-b6e2-5748c995a52c
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 103092d54d1b03405e91e5f1ae385bd396faebba
ms.sourcegitcommit: 4d370399f6f142e25075b3714e5c2ce056b1bfd0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91868638"
---
# <a name="change-proxy-account-for-utility-collection-on--managed-sql-server"></a>Ändern des Proxykontos für den Hilfsprogramm-Sammlungssatz auf einer verwalteten Instanz von SQL Server
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In diesem Thema wird beschrieben, wie Sie das Proxykonto für den Hilfsprogramm-Sammlungssatz auf einer verwalteten Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]ändern können.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
  
#### <a name="to-change-the-proxy-account-for-the-utility-collection-set-on-a-managed-instance-of-sql-server"></a>So ändern Sie das Proxykonto für den Hilfsprogramm-Sammlungssatz auf einer verwalteten Instanz von SQL Server  
  
1.  Entfernen Sie die verwaltete Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aus dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Hilfsprogramm.  
  
    -   Klicken Sie im **Hilfsprogramm-Explorer** in SSMS auf den Knoten **Verwaltete Instanzen** .  
  
    -   Klicken Sie in der Listenansicht **Hilfsprogramm-Explorer** mit der rechten Maustaste auf den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanznamen, und wählen Sie **Verwaltete Instanz entfernen** aus. Weitere Informationen finden Sie unter [Vorgehensweise: Entfernen einer Instanz von SQL Server aus dem SQL Server-Hilfsprogramm](../../relational-databases/manage/remove-an-instance-of-sql-server-from-the-sql-server-utility.md).  
  
2.  Registrieren Sie die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] erneut im [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Hilfsprogramm.  

    -   Klicken Sie im **Hilfsprogramm-Explorer** in SSMS mit der rechten Maustaste auf den Knoten **Verwaltete Instanzen**, und wählen Sie **Verwaltete Instanz hinzufügen** aus.  
  
    -   Befolgen Sie die Eingabeaufforderungen zum Abschließen des Assistenten, indem Sie das neue Proxykonto angeben.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Funktionen und Tasks im SQL Server-Hilfsprogramm](../../relational-databases/manage/sql-server-utility-features-and-tasks.md)   
 [Problembehandlung beim SQL Server-Hilfsprogramm](/previous-versions/sql/sql-server-2016/ee210592(v=sql.130))  
  
