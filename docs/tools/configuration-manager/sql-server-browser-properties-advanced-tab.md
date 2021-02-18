---
title: Eigenschaften von SQL Server-Browser (Registerkarte Erweitert)
description: Hier erfahren Sie mehr über die Optionen auf der Registerkarte „Erweitert“ des Dialogfelds „SQL Server Browser Properties“ (Eigenschaften für den SQL Server-Browser), z. B. das Speicherverzeichnis und die Instanz-ID.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: ba79137a-cb72-4bf3-a650-e11d02cfce10
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: fc2075fe1875a8d74b101f79a691f5d88c26e04f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345227"
---
# <a name="sql-server-browser-properties-advanced-tab"></a>Eigenschaften von SQL Server-Browser (Registerkarte Erweitert)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Browser-Programm wird als Dienst auf dem Server ausgeführt. Der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Browser lauscht auf eingehende Anforderungen für [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Ressourcen und stellt Informationen zu [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanzen zur Verfügung, die auf dem Computer installiert sind.  
  
## <a name="options"></a>Tastatur  
 **Gruppiert**  
 Zeigt an, ob dieser Dienst als Ressource eines gruppierten Servers installiert ist.  
  
 **Berichterstellung für Kundenfeedback**  
 Gibt an, ob Service Quality Monitoring für diesen Dienst aktiviert wurde. Weitere Informationen zu Berichterstellung für Kundenfeedback finden Sie in der Onlinedokumentation unter "Einstellungen für Fehler- und Verwendungsberichte".  
  
 **Speicherverzeichnis**  
 Der Speicherort, an dem Speicherabbilder im Falle eines Fehlers abgelegt werden.  
  
 **Fehlerberichterstellung**  
 Mit der Einstellung **Ja** werden vom Programm Dr. Watson Informationen an [!INCLUDE[msCoName](../../includes/msconame-md.md)] oder den Fehlerberichtsserver weitergeleitet, wenn ein schwerwiegender Fehler auftritt. Weitere Informationen zur Fehlerberichterstellung finden Sie in der Onlinedokumentation unter "Einstellungen für Fehler- und Verwendungsberichte".  
  
 **Instanz-ID**  
 Gibt die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] an, die diese [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent-Instanz verwendet hat. Die Standardinstanz ist **MSSQL10_50.MSSQLSERVER**.  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQL Server-Browserdienst](../../tools/configuration-manager/sql-server-browser-service.md)  
  
  
