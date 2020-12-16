---
description: Entwicklerhandbuch (Replikation)
title: Entwicklerhandbuch (Replikation) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
helpviewer_keywords:
- developer's guide [SQL Server replication]
- programming [SQL Server replication]
- replication [SQL Server], development
ms.assetid: 7ee134ae-1cab-4a35-8017-8ac6d8fc64b6
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: f3cd2af326a38168860d072303f3f572959f73d7
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97460293"
---
# <a name="replication-developer-documentation"></a>Entwicklerhandbuch (Replikation)
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]

  Durch die Fähigkeit, eine Replikationstopologie programmgesteuert zu konfigurieren, zu warten und zu überwachen, können Sie häufig anfallende Replikationstasks vereinfachen und die Benutzerfreundlichkeit Ihrer replikationsbasierten Anwendungen verbessern. Durch die Programmierung der Replikation können den Endbenutzern benutzerdefinierte Replikationsfunktionen bereitgestellt werden, ohne dass diese mit gespeicherten Replikationsprozeduren und ausführbaren Dateien von Replikations-Agents vertraut sein müssen oder die von [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] implementierte Replikationsbenutzeroberfläche verwenden müssen.  
  
 Im Folgenden werden Szenarien vorgestellt, in denen Ihre Anwendungen von einem programmgesteuerten Zugriff auf Replikationsdienste profitieren können:  
  
-   Hinzufügen von Replikationsfunktionen zu einer vorhandenen Endbenutzeranwendung, z. B. Synchronisierung eines Pullabonnements, wenn der Benutzer auf eine Schaltfläche klickt.  
  
-   Erstellen einer webbasierten Benutzeroberfläche zur Remoteverwaltung der Replikation.  
  
-   Erstellen einer benutzerdefinierten Benutzeroberfläche, die nur einen Teil der Verwaltungsfunktionen zur Verfügung stellt und zur Remoteverwaltung mehrerer Replikationstopologien von einem einzelnen Speicherort aus verwendet werden kann oder Verwaltungs- und Synchronisierungsfunktionen kombiniert.  
  
-   Verbessern eines vorhandenen Überwachungstools durch Hinzufügen der Funktion zur Überwachung des Status einer Veröffentlichung, eines Abonnements oder des Verteilers.  
  
-   Erstellen einer benutzerdefinierten Anwendung zum Verwalten oder Synchronisieren von Abonnements für einen Oracle-Verleger.  
  
-   Schreiben von benutzerdefinierten Geschäftsregeln, die ausgeführt werden, wenn ein Mergeabonnement synchronisiert wird.  
  
-   Generieren von [!INCLUDE[tsql](../../../includes/tsql-md.md)]-Skripts, die beim Konfigurieren neuer Abonnenten wiederholt ausgeführt werden können.  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ermöglicht es Ihnen, Replikations-Agents programmgesteuert zu kontrollieren und eine Replikationstopologie programmgesteuert zu verwalten und zu überwachen. Weitere Informationen zum Programmieren der Replikation finden Sie unter [Konzepte für die Replikationsprogrammierung](../../../relational-databases/replication/concepts/replication-programming-concepts.md).  
  
## <a name="in-this-section"></a>In diesem Abschnitt  
 [Konzepte für die Replikationsprogrammierung](../../../relational-databases/replication/concepts/replication-programming-concepts.md)  
 Beschreibt die Planungsschritte zur Entwicklung einer Anwendung, die die Replikation verwendet.  
  
 [Replication System Stored Procedures Concepts](../../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)  
 Beschreibt, wie gespeicherte Systemprozeduren verwendet werden können, um programmgesteuerten Zugriff in einer Replikationstopologie bereitzustellen.  
  
 [Replication Management Objects Concepts](../../../relational-databases/replication/concepts/replication-management-objects-concepts.md)  
 Erläutert die Konzepte zum Verwenden von Replikationsverwaltungsobjekten (RMO). Hierbei handelt es sich um eine verwaltete Codeassembly, die Replikationsfunktionen für [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] kapselt.  
  
 [Ausführbare Konzepte für die Programmierung von Replikations-Agent](../../../relational-databases/replication/concepts/replication-agent-executables-concepts.md)  
 Beschreibt die Verwendung von ausführbaren Dateien von Replikations-Agents.  
  
  
  
