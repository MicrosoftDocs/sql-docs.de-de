---
description: Erstellen von Aufträgen
title: Erstellen von Aufträgen
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- jobs [SQL Server Agent], creating
- SQL Server Agent jobs, creating
ms.assetid: 465fb7fc-7622-4252-a178-ea51691c935b
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 4a79b08a37c6fb21f8a1e6c1b3f16466c15d135b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342718"
---
# <a name="create-jobs"></a>Erstellen von Aufträgen
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance) werden derzeit die meisten, aber nicht alle, SQL Server-Agent-Features unterstützt. Details dazu finden Sie unter [T-SQL-Unterschiede zwischen Azure SQL Managed Instance und SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Ein Auftrag besteht aus einer festgelegten Folge von Operationen, die der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent der Reihenfolge nach ausführt. Über einen Auftrag können zahlreiche Aktivitäten ausgeführt werden, u. a. das Ausführen von [!INCLUDE[tsql](../../includes/tsql-md.md)] -Skripts, Eingabeaufforderungsanwendungen, Microsoft ActiveX-Skripts, Integration Services-Paketen, Analysis Services-Befehlen und -abfragen bzw. Replikationstasks. Aufträge können wiederholte oder planbare Tasks ausführen, und sie können Benutzer in Form von Warnungen hinsichtlich des Auftragsstatus benachrichtigen. Dies führt zu einer deutlichen Vereinfachung der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Verwaltung.  
  
Um einen Auftrag erstellen zu können, muss ein Benutzer Mitglied einer der festen Datenbankrollen des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agents oder Mitglied der festen Serverrolle **sysadmin** sein. Ein Auftrag kann nur von seinem Besitzer bzw. Mitgliedern der **sysadmin** -Rolle bearbeitet werden. Mitglieder der **sysadmin** -Rolle können anderen Benutzern den Auftragsbesitz zuweisen und sämtliche Aufträge ausführen, ungeachtet des Auftragsbesitzers. Weitere Informationen zu den festen Datenbankrollen des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agents finden Sie unter [Feste Datenbankrollen des SQL Server-Agents](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
Aufträge können so geschrieben werden, dass sie auf der lokalen Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder auf mehreren Instanzen in einem Unternehmen ausgeführt werden. Zum Ausführen von Aufträgen auf mehreren Servern müssen Sie mindestens einen Masterserver und einen oder mehrere Zielserver einrichten. Weitere Informationen zu Master- und Zielservern finden Sie unter [Automatisierte Verwaltung in einem Unternehmen](../../ssms/agent/automated-administration-across-an-enterprise.md).  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent zeichnet die Informationen von Aufträgen und Auftragsschritten im Auftragsverlauf auf.  
  
## <a name="related-tasks"></a>Related Tasks  
  
|BESCHREIBUNG|Thema|  
|-|-|  
|Beschreibt, wie ein [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent-Auftrag erstellt wird.|[Erstellen eines Auftrags](../../ssms/agent/create-a-job.md)|  
|Beschreibt, wie Sie den Besitz eines [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent-Auftrags einem anderen Benutzer neu zuweisen können.|[Give Others Ownership of a Job](../../ssms/agent/give-others-ownership-of-a-job.md)|  
|Beschreibt, wie Sie das Auftragsverlaufsprotokoll des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agents einrichten.|[Set Up the Job History Log](../../ssms/agent/set-up-the-job-history-log.md)|  
  
## <a name="see-also"></a>Weitere Informationen  
[Verwalten von Auftragsschritten](../../ssms/agent/manage-job-steps.md)  
[Automatisierte Verwaltung in einem Unternehmen](../../ssms/agent/automated-administration-across-an-enterprise.md)  
[Anlegen und Zuweisen von Zeitplänen zu Aufträgen](../../ssms/agent/create-and-attach-schedules-to-jobs.md)  
[Ausführen von Aufträgen](../../ssms/agent/run-jobs.md)  
[Anzeigen oder Ändern von Aufträgen](../../ssms/agent/view-or-modify-jobs.md)  
