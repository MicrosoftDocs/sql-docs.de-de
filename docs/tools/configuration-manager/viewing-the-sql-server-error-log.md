---
title: Anzeigen des SQL Server-Fehlerprotokolls
description: Hier finden Sie Informationen dazu, wie Sie Unterstützung beim Erkennen von Problemen in SQL Server erhalten, indem Sie im aktuellen Fehlerprotokoll oder in Sicherungen vorheriger Protokolle überprüfen, ob Prozesse erfolgreich abgeschlossen wurden.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- cycling SQL Server error log
- viewing SQL Server error log
- errors [SQL Server], logs
- SQL Server error log
- displaying SQL Server error log
- logs [SQL Server], SQL Server error logs
ms.assetid: 6908c21a-65e3-458f-a272-fee256d86448
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 20827337064ab864d435fab314330345b3d54e7a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345975"
---
# <a name="viewing-the-sql-server-error-log"></a>Anzeigen des SQL Server-Fehlerprotokolls
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Zeigen Sie das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Fehlerprotokoll an, um sicherzustellen, dass bestimmte Prozesse erfolgreich abgeschlossen wurden (z.B. Sicherungs- und Wiederherstellungsvorgänge, Batchbefehle oder andere Skripts und Prozesse). Dies ist hilfreich, um aktuelle oder potenzielle Problembereiche zu ermitteln, einschließlich automatischer Wiederherstellungsmeldungen (insbesondere, wenn eine Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] angehalten und neu gestartet wurde), Kernelmeldungen oder anderer Fehlermeldungen auf Serverebene.  
  
 Um das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Fehlerprotokoll anzuzeigen, können Sie [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder einen beliebigen Text-Editor verwenden. Weitere Informationen zum Anzeigen des Fehlerprotokolls finden Sie unter [Open Log File Viewer](../../relational-databases/logs/open-log-file-viewer.md). Standardmäßig befindet sich das Fehlerprotokoll unter `Program Files\Microsoft SQL Server\MSSQL.`*n*`\MSSQL\LOG\ERRORLOG` und `ERRORLOG.`*n* .  
  
 Es wird immer dann ein neues Fehlerprotokoll erstellt, wenn eine Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gestartet wird, obwohl die gespeicherte Systemprozedur [sp_cycle_errorlog](../../relational-databases/system-stored-procedures/sp-cycle-errorlog-transact-sql.md) zum Durchlaufen der Fehlerprotokolldateien verwendet werden kann, ohne dass die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]neu gestartet werden muss. In der Regel behält [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Sicherungen der letzten sechs Protokolle bei. Dabei erhält die letzte Sicherung die Erweiterung .1, die zweitletzte Sicherung die Erweiterung .2 usw. Das aktuelle Fehlerprotokoll hat keine Erweiterung.  
  
 Beachten Sie, dass Sie auch das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Fehlerprotokoll zu Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] anzeigen können, die offline sind oder nicht gestartet werden können. Weitere Informationen finden Sie unter [Anzeigen von Offlineprotokolldateien](../../relational-databases/logs/view-offline-log-files.md).  
  
  
