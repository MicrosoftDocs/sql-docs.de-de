---
title: MSSQLSERVER_233 | Microsoft-Dokumentation
description: Der SQL Server-Client kann keine Verbindung mit dem Server herstellen. Hier finden Sie eine Erläuterung des Fehlers 233 sowie mögliche Lösungen.
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: language-reference
f1_keywords:
- "233"
helpviewer_keywords:
- 233 (Database Engine error)
ms.assetid: 201665dc-7ac8-4c19-90d3-33354c5caa72
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: bf1a3e2f94c221caa4b70105aaccc331c6082b97
ms.sourcegitcommit: 36fe62a3ccf34979bfde3e192cfa778505add465
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2020
ms.locfileid: "94521070"
---
# <a name="mssqlserver_233"></a>MSSQLSERVER_233
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Details  
  
| attribute | Wert |  
| :-------- | :---- |  
|Produktname|SQL Server|  
|Ereignis-ID|233|  
|Ereignisquelle|MSSQLSERVER|  
|Komponente|SQLEngine|  
|Symbolischer Name||  
|Meldungstext|Eine Verbindung mit dem Server wurde erfolgreich hergestellt, aber dann trat während des Anmeldevorgangs ein Fehler auf. (Anbieter: Shared Memory-Anbieter, Fehler: 0 - No process is on the other end of the pipe.) (0 – Kein Prozess ist am anderen Ende der Pipe.) (Microsoft SQL Server, Fehler: 233)|  
  
## <a name="explanation"></a>Erklärung  
Der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Client kann keine Verbindung mit dem Server herstellen. Dieser Fehler ist möglicherweise darauf zurückzuführen, dass der Server nicht für die Annahme von Remoteverbindungen konfiguriert ist.  
  
## <a name="user-action"></a>Benutzeraktion  
Verwenden Sie das Tool [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Konfigurations-Manager, um zuzulassen, dass in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Remoteverbindungen angenommen werden.  
  
## <a name="see-also"></a>Weitere Informationen  
[Netzwerkprotokolle und Netzwerkbibliotheken](~/sql-server/install/network-protocols-and-network-libraries.md)  
[Client-Netzwerkkonfiguration](~/database-engine/configure-windows/client-network-configuration.md)  
[Konfigurieren von Clientprotokollen](~/database-engine/configure-windows/configure-client-protocols.md)  
[Aktivieren oder Deaktivieren eines Servernetzwerkprotokolls](~/database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md)  
  
