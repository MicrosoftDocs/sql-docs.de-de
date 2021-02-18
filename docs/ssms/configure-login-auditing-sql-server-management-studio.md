---
description: Konfigurieren der Anmeldungsüberwachung (SQL Server Management Studio)
title: Konfigurieren der Anmeldungsüberwachung (SQL Server Management Studio)
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- auditing [SQL Server]
- audits [SQL Server], logins
- logins [SQL Server], auditing
ms.assetid: 16961116-57ac-4eef-8037-791b26ade548
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 8e80dc413617cd98f1ff1bc293ef10b9a6b7b0f4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341078"
---
# <a name="configure-login-auditing-sql-server-management-studio"></a>Konfigurieren der Anmeldungsüberwachung (SQL Server Management Studio)
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]
In diesem Thema wird beschrieben, wie die Anmeldeüberwachung in [!INCLUDE[ssnoversion](../includes/ssnoversion-md.md)] konfiguriert wird, um die [!INCLUDE[ssDEnoversion](../includes/ssdenoversion_md.md)]-Anmeldeaktivität zu überwachen. Die Anmeldungsüberwachung kann so konfiguriert werden, dass die folgenden Ereignisse im Fehlerprotokoll aufgezeichnet werden.  
  
-   Fehlgeschlagene Anmeldungen  
  
-   Erfolgreiche Anmeldungen  
  
-   Erfolgreiche und fehlgeschlagene Anmeldungen  
  
Sie müssen [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] neu starten, damit die Option wirksam wird.  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Verwenden von SQL Server Management Studio  
  
#### <a name="to-configure-login-auditing"></a>So konfigurieren Sie die Anmeldungsüberwachung  
  
1.  Stellen Sie in [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]mithilfe des Objekt-Explorers eine Verbindung mit einer Instanz von [!INCLUDE[ssDEnoversion](../includes/ssdenoversion_md.md)] her.  
  
2.  Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf den Servernamen, und klicken Sie auf **Eigenschaften**.  
  
3.  Wählen Sie auf der Seite **Sicherheit** unter **Anmeldungsüberwachung** die gewünschte Option aus, und schließen Sie die Seite **Servereigenschaften** .  
  
4.  Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf den Servernamen, und klicken Sie dann auf **Neu starten**.  
  
