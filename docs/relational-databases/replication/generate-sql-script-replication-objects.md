---
description: SQL-Skript generieren (Replikationsobjekte)
title: SQL-Skript generieren (Replikationsobjekte) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.generatesqlscript.f1
helpviewer_keywords:
- Generate SQL Script dialog box
ms.assetid: b7ccc34e-1c22-44b8-8eb5-f6423af3164e
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 36c275237e7770092e9795759659ad4d7326e550
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97464761"
---
# <a name="generate-sql-script-replication-objects"></a>SQL-Skript generieren (Replikationsobjekte)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  Ein Replikationsskript enthält die gespeicherten [!INCLUDE[tsql](../../includes/tsql-md.md)] -Systemprozeduren, die zur Implementierung der Replikationskomponenten, für die Skripts erstellt wurden, erforderlich sind. Dazu gehören eine Veröffentlichung oder ein Abonnement. Für die Replikationskomponenten in einer Topologie sollten im Rahmen des Plans zur Wiederherstellung im Notfall Skripts erstellt werden; diese können dann auch zur Automatisierung sich wiederholender Tasks verwendet werden. Die Replikation stellt zum Erstellen von Skripts für Replikationsobjekte zwei Dialogfelder bereit:  
  
-   **SQL-Skript generieren:** Dieses Dialogfeld kann über das Kontextmenü des Ordners **Replikation** sowie über alle untergeordneten Ordner in [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] aufgerufen werden. Mithilfe dieses Dialogfelds können Sie Skripts für alle Replikationsobjekte einer Instanz von [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] erstellen.  
  
-   **SQL-Skript generieren\<ObjectName>** : Dieses Dialogfeld kann über das Kontextmenü für Veröffentlichungen und Abonnements aufgerufen werden. Mithilfe dieses Dialogfelds können Sie Skripts für einzelne Objekte erstellen.  
  
 In diesen Dialogfeldern werden Skripts für Objekte einer einzelnen Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]erstellt; es werden keine Verbindungen mit anderen Instanzen hergestellt, um Skripts für verknüpfte Objekte zu erstellen.  
  
## <a name="generate-sql-script-options"></a>SQL-Skript generieren (Optionen)  
 **Verteilereigenschaften**  
 Wählen Sie diese Option aus, um Skripts für gespeicherte Prozeduren zu erstellen, um: den Verteiler aktivieren oder deaktivieren, mit dem Verteiler verknüpfte Verleger hinzufügen oder löschen sowie die Verteilungsdatenbank erstellen oder löschen zu können.  
  
 **Veröffentlichungen in den folgenden Datenquellen**  
 Wählen Sie diese Option aus, um Skripts für gespeicherte Prozeduren zu erstellen, um: die Veröffentlichung aktivieren oder deaktivieren sowie Veröffentlichungen, Artikel, Pushabonnements und Replikationsaufträge erstellen oder löschen zu können.  
  
 **Abonnements in den folgenden Datenquellen**  
 Durch Auswählen dieser Option können Sie Skripts für gespeicherte Prozeduren erstellen, um Pullabonnements und Replikationsaufträge zu erstellen oder zu löschen.  
  
 **Um Komponenten zu erstellen oder zu aktivieren** und **Um Komponenten zu löschen oder zu deaktivieren**  
 Geben Sie an, ob das Skript Befehle zum Erstellen oder Löschen eines Replikationsobjekts enthalten soll. [!INCLUDE[msCoName](../../includes/msconame-md.md)] empfiehlt, das Dialogfeld zu verwenden, um eine Reihe von Skripts zum Aktivieren und Deaktivieren von Komponenten zu erstellen.  
  
 **Replikationsaufträge**  
 Wählen Sie diese Option aus, um neben Skripts für Aufrufe gespeicherter Prozeduren auch Skripts für Replikationsaufträge zu erstellen. Diese Option ist nur verfügbar, wenn Skripts von einem Verteiler aus erstellt werden.  
  
 Bei der Ausführung gespeicherter Replikationsprozeduren werden die erforderlichen Aufträge erstellt, d. h., die Option muss nicht ausgewählt werden. Einen Datensatz mit den erstellten Aufträgen zur Verfügung zu haben, kann sich jedoch als nützlich erweisen, falls ein einzelner Auftrag erneut erstellt werden muss.  
  
## <a name="generate-sql-script-objectname-options"></a>SQL-Skript generieren – Optionen für \<ObjectName>  
 **Um Komponenten zu erstellen oder zu aktivieren** und **Um Komponenten zu löschen oder zu deaktivieren**  
 Geben Sie an, ob das Skript Befehle zum Erstellen oder Löschen eines Replikationsobjekts enthalten soll. [!INCLUDE[msCoName](../../includes/msconame-md.md)] empfiehlt, das Dialogfeld zu verwenden, um eine Reihe von Skripts zum Aktivieren und Deaktivieren von Komponenten zu erstellen.  
  
 **Replikationsaufträge**  
 Auf diese Option kann im Dialogfeld **SQL-Skript generieren** zugegriffen werden.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Erstellen von Skripts für die Replikation](../../relational-databases/replication/scripting-replication.md)  
  
  
