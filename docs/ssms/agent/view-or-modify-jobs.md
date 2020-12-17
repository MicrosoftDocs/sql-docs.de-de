---
description: Anzeigen oder Ändern von Aufträgen
title: Anzeigen oder Ändern von Aufträgen
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- jobs [SQL Server Agent], modifying
- jobs [SQL Server Agent], viewing
- modifying jobs
- viewing jobs
- SQL Server Agent jobs, viewing
- SQL Server Agent jobs, modifying
- displaying jobs
ms.assetid: 57f649b8-190c-4304-abd7-7ca5297deab7
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 7c16b835eff545c4623ccf9845a523f2e56bf865
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97422505"
---
# <a name="view-or-modify-jobs"></a>Anzeigen oder Ändern von Aufträgen
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance) werden derzeit die meisten, aber nicht alle, SQL Server-Agent-Features unterstützt. Details dazu finden Sie unter [T-SQL-Unterschiede zwischen Azure SQL Managed Instance und SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Jeden Auftrag, den Sie erstellt haben, können Sie anzeigen. Nachdem Sie einen Auftrag ausgeführt haben, können Sie auch den Auftragsverlauf anzeigen. Auf diese Weise erfahren Sie, wann der Auftrag ausgeführt wurde, den Status des Auftrags insgesamt sowie den Status jedes Auftragsschritts. Sie sehen, ob bei dem Auftrag in der Vergangenheit jemals ein Fehler aufgetreten ist, wann der Auftrag zuletzt erfolgreich ausgeführt wurde sowie welche Ausgabe bei jeder Ausführung des Auftrags erstellt wurde. Mitglieder der festen Serverrolle **sysadmin** können jeden Auftrag anzeigen oder ändern, unabhängig vom Besitzer.  
  
> [!NOTE]  
> Ein Auftrag muss mindestens einmal ausgeführt worden sein, damit ein Auftragsverlauf vorhanden ist. Sie können die Gesamtgröße des Auftragsverlaufsprotokolls und die Größe pro Auftrag begrenzen.  
  
Schließlich können Sie einen Auftrag an neue Anforderungen anpassen. Sie können Folgendes ändern:  
  
-   Benachrichtigungsoptionen  
  
-   Zeitpläne  
  
-   Auftragsschritte  
  
-   Besitz  
  
-   Auftragskategorie  
  
-   Zielserver (ausschließlich bei Multiserveraufträgen)  
  
Wenn Sie Änderungen an Multiserveraufträgen vornehmen, müssen Sie die Änderungen der Downloadliste bereitstellen, damit die Zielserver den aktualisierten Auftrag herunterladen können. Um sicherzustellen, dass die Zielserver über die aktuellsten Auftragsdefinitionen verfügen, führen Sie nach dem Aktualisieren des Multiserverauftrags wie folgt eine INSERT-Anweisung aus:  
  
```  
EXECUTE sp_post_msx_operation 'INSERT', 'JOB', '<job id>'  
```  
  
Weitere Informationen finden Sie unter [sp_purge_jobhistory (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-purge-jobhistory-transact-sql.md).  
  
Mitglieder der festen Serverrolle **sysadmin** können die Definition oder den Verlauf jedes Auftrags anzeigen und können jeden Auftrag ändern.  
  
## <a name="related-tasks"></a>Related Tasks  
  
|BESCHREIBUNG|Thema|  
|-|-|  
|Beschreibt, wie [!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent-Aufträge angezeigt werden.|[View a Job](../../ssms/agent/view-a-job.md)|  
|Beschreibt, wie das Auftragsverlaufsprotokoll des [!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agents angezeigt wird.|[Anzeigen des Auftragsverlaufs](../../ssms/agent/view-the-job-history.md)|  
|Beschreibt, wie der Inhalt des Auftragsverlaufsprotokolls des [!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agents gelöscht wird.|[Clear the Job History Log](../../ssms/agent/clear-the-job-history-log.md)|  
|Beschreibt, wie Größenbeschränkungen für Auftragsverlaufsprotokolle des [!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agents festgelegt werden.|[Resize the Job History Log](../../ssms/agent/resize-the-job-history-log.md)|  
|Beschreibt, wie die Eigenschaften von [!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent-Aufträgen geändert werden.|[Ändern eines Auftrags](../../ssms/agent/modify-a-job.md)|  
  
## <a name="see-also"></a>Weitere Informationen  
[sysjobhistory](../../relational-databases/system-tables/dbo-sysjobhistory-transact-sql.md)  
