---
description: Warnungseigenschaften – Neue Warnung (Seite „Antwort“)
title: Warnungseigenschaften – Neue Warnung (Seite „Antwort“)
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.ag.alert.response.f1
ms.assetid: 72daf008-f9ea-4077-b217-5048e7759d3e
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 5f8796a3103400a457559efc33ac295429562695
ms.sourcegitcommit: 22dacedeb6e8721e7cdb6279a946d4002cfb5da3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2020
ms.locfileid: "92036509"
---
# <a name="alert-properties---new-alert-response-page"></a>Warnungseigenschaften – Neue Warnung (Seite „Antwort“)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance) werden derzeit die meisten, aber nicht alle, SQL Server-Agent-Features unterstützt. Details dazu finden Sie unter [T-SQL-Unterschiede zwischen Azure SQL Managed Instance und SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Auf dieser Seite können Sie einen Auftrag festlegen, der ausgeführt werden und eine Liste von Operatoren abrufen soll, die zur Reaktion auf eine [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent-Warnung benachrichtigt werden sollen.  

## <a name="options"></a>Optionen  
**Auftrag ausführen**  
Aktiviert die Optionen **Auftragsliste**, **Neuer Auftrag** und **Auftrag anzeigen** .  
  
**Neuer Auftrag**  
Öffnet das Dialogfeld **Neuer Auftrag** . Diese Schaltfläche ist nicht verfügbar, wenn **Auftrag ausführen** nicht ausgewählt ist.  
  
**Auftrag anzeigen**  
Zeigen Sie den ausgewählten Auftrag an oder ändern Sie ihn. Diese Option ist nicht verfügbar, wenn **Auftrag ausführen** nicht ausgewählt ist.  
  
**Operatoren benachrichtigen**  
Aktiviert die Steuerelemente, mit denen Sie Operatoren hinzufügen, entfernen oder ändern können.  
  
**Operatorliste**  
Enthält die Operatoren, die benachrichtigt werden, wenn eine Warnung auftritt. Aktivieren Sie das Kontrollkästchen **E-Mail**, **Pager**oder **NET SEND** hinter dem Operatornamen, um eine Benachrichtigungsmethode festzulegen. Diese Option ist nicht verfügbar, wenn **Operatoren benachrichtigen** nicht ausgewählt ist.  
  
**E-Mail**  
Verwenden Sie E-Mail zum Benachrichtigen des Operators.  
  
**Pager**  
Verwenden Sie die Pager-E-Mail-Adresse zum Benachrichtigen des Operators.  
  
**NET SEND**  
Verwenden Sie zum Benachrichtigen des Operators die Option **NET SEND** .  
  
**Neuer Operator**  
Zeigt das Dialogfeld **Neuer Operator** an, in dem Sie einen neuen Operator erstellen können.  
  
**Operator anzeigen**  
Zeigt das Dialogfeld **Eigenschaften** für den derzeit ausgewählten Operator an. Über das Dialogfeld **Operator-Eigenschaften** können Sie die Operator-Eigenschaften anzeigen und ändern.  
  
## <a name="see-also"></a>Weitere Informationen  
[Warnungen](../../ssms/agent/alerts.md)  
[Create an Alert Using Severity Level](../../ssms/agent/create-an-alert-using-severity-level.md)  
[Warnungen](../../ssms/agent/alerts.md)  
[Edit an Alert](../../ssms/agent/edit-an-alert.md)  
[Delete an Alert](../../ssms/agent/delete-an-alert.md)  
