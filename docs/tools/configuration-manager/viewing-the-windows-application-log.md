---
title: Anzeigen des Windows-Anwendungsprotokolls
description: In diesem Artikel erfahren Sie, wie Sie das Windows-Anwendungsprotokoll anzeigen und verwalten. Außerdem erfahren Sie, wie Sie SQL Server so konfigurieren, dass Informationen zu Ereignissen in dieses Protokoll geschrieben werden.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- application logs [SQL Server]
- Windows application logs [SQL Server]
- viewing Windows application logs
- errors [SQL Server], logs
- system logs [SQL Server]
- security logs [SQL Server]
- displaying Windows application logs
- logs [SQL Server], Windows application logs
ms.assetid: f9853b74-7db7-47cc-b957-e49ed5bc0a1a
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 5cec58c54da985712838efeca3ee22401a420ae0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345959"
---
# <a name="viewing-the-windows-application-log"></a>Anzeigen des Windows-Anwendungsprotokolls
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Wenn [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] so konfiguriert wurde, dass das Microsoft Windows-Anwendungsprotokoll verwendet wird, schreibt jede [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Sitzung neue Ereignisse in dieses Protokoll. Im Gegensatz zum Fehlerprotokoll von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wird beim Starten einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz nicht jedes Mal ein neues Anwendungsprotokoll erstellt.  
  
 Das Windows-Anwendungsprotokoll können Sie mit der Windows-Ereignisanzeige oder dem Protokoll-Viewer in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]anzeigen und verwalten.  
  
 Es gibt drei Protokolle, die mit der Ereignisanzeige angezeigt werden können.  
  
|Windows-Protokolltyp|BESCHREIBUNG|  
|----------------------|-----------------|  
|Systemprotokoll|Zeichnet Ereignisse auf, die von den Windows-Betriebssystemkomponenten protokolliert werden. Wenn beispielsweise ein Treiber oder eine andere Systemkomponente beim Starten nicht geladen wird, wird dies im Systemprotokoll aufgezeichnet.|  
|Sicherheitsprotokoll|Zeichnet Sicherheitsereignisse auf, wie z. B. fehlgeschlagene Anmeldeversuche. Dadurch können Änderungen am Sicherheitssystem nachverfolgt und mögliche Verstöße gegen die Sicherheit erkannt werden. So können z. B. Versuche einer Anmeldung am System im Sicherheitsprotokoll aufgezeichnet werden, je nach Überwachungseinstellungen im Benutzer-Manager.<br /><br /> Ausschließlich Mitglieder der festen Serverrolle **sysadmin** können das Sicherheitsprotokoll anzeigen.|  
|Anwendungsprotokoll|Zeichnet Ereignisse auf, die von Anwendungen protokolliert werden. So kann eine Datenbankanwendung beispielsweise einen Dateifehler im Anwendungsprotokoll aufzeichnen.|  
  
 Weitere Informationen zum Verwenden der Ereignisanzeige und zum Verwalten des Anwendungsprotokolls sowie Erklärungen zu den angezeigten Informationen finden Sie in der Windows-Dokumentation.  
  
 **So zeigen Sie das Anwendungsprotokoll von Windows an**  
  
 [Anzeigen des Anwendungsprotokolls von Windows &#40;Windows&#41;](../../relational-databases/performance/view-the-windows-application-log-windows-10.md)  
  
  
