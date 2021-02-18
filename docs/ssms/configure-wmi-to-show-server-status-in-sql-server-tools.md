---
description: Konfigurieren von WMI zum Anzeigen des Serverstatus in SQL Server-Tools
title: Konfigurieren von WMI zum Anzeigen des Serverstatus in SQL Server-Tools
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- WMI Provider for Server Events, setting permissions
- WMI permissions [SQL Server]
ms.assetid: 7e97197b-ed4d-40d1-9a52-9ab1d92401d7
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a39ef785c7f0208d34f10bf31d5749f403e63723
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344204"
---
# <a name="configure-wmi-to-show-server-status-in-sql-server-tools"></a>Konfigurieren von WMI zum Anzeigen des Serverstatus in SQL Server-Tools
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
In diesem Thema wird beschrieben, wie WMI konfiguriert wird, um den Serverstatus in SQL Server-Tools in [!INCLUDE[ssnoversion](../includes/ssnoversion-md.md)]anzuzeigen. Bei der Verbindungsherstellung mit Servern wird von den Komponenten Registrierte Server und Objekt-Explorer von [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]sowie auch vom [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Konfigurations-Manager Windows Management Instrumentation (WMI) zum Abrufen des Status der [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Dienste (MSSQLSERVER) und [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Agent-Dienste (MSSQLSERVER) verwendet. Zum Anzeigen des Dienststatus muss der Benutzer über Remotezugriffsrechte für das WMI-Objekt verfügen. Zum Konfigurieren dieser Berechtigung muss auf dem Server WMI installiert sein.  
  
## <a name="to-configure-wmi-permission"></a><a name="SSMSProcedure"></a>So konfigurieren Sie die WMI-Berechtigung  
  
1.  Klicken Sie auf dem Remoteserver im Menü **Start** auf **Ausführen**.  
  
2.  Geben Sie im Feld **Öffnen** die Zeichenfolge **wmimgmt.msc** ein, und klicken Sie dann auf **OK**.  
  
3.  Klicken Sie im Programm **Windows-Verwaltungsinfrastruktur** mit der rechten Maustaste auf **WMI-Steuerung (Lokal)**, und klicken Sie dann auf **Eigenschaften**.  
  
4.  Erweitern Sie im Dialogfeld **Eigenschaften von WMI-Steuerung (Lokal)** auf der Registerkarte **Sicherheit** den Eintrag **Root**, und klicken Sie dann auf **CIMV2**.  
  
5.  Klicken Sie auf **Sicherheit** , um das Dialogfeld **Sicherheit für ROOT\CIMV2** zu öffnen.  
  
6.  Fügen Sie zum Feld **Gruppen- oder Benutzernamen** eine Gruppe oder einen Benutzer hinzu, und markieren Sie die Gruppe bzw. den Benutzer.  
  
7.  Wählen Sie im Feld **Berechtigungen für** _<group or user>_ die Spalte **Zulassen** für die Berechtigung **Remoteaktivierung** für Benutzer aus, durch die der Dienststatus remote erkannt werden soll.  
  
## <a name="see-also"></a>Weitere Informationen  
[Starten, Beenden oder Anhalten des SQL Server-Agent-Diensts](../ssms/agent/start-stop-or-pause-the-sql-server-agent-service.md)  
  
