---
description: Index neu erstellen (Task)
title: Index neu erstellen (Task) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.rebuildindextask.f1
helpviewer_keywords:
- rebuilding indexes
- indexes [Integration Services]
- Rebuild Index task
ms.assetid: 021884dd-e72d-47b2-99e8-b741410509c3
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 7ea6820033ad5b545cc73ce79741b52953bd5caa
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/26/2020
ms.locfileid: "92197155"
---
# <a name="rebuild-index-task"></a>Index neu erstellen (Task)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Mit dem Task Index neu erstellen werden Indizes in Tabellen und Sichten von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datenbanken neu erstellt. Weitere Informationen zum Verwalten von Indizes finden Sie unter [Neuorganisieren und Neuerstellen von Indizes](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md).  
  
 Mithilfe des Tasks Index neu erstellen kann ein Paket Indizes in einer einzelnen Datenbank oder mehreren Datenbanken neu erstellen. Falls mit dem Task nur die Indizes in einer einzelnen Datenbank neu erstellt werden, können Sie die Sichten und Tabellen auswählen, deren Indizes neu erstellt werden.  
  
 Dieser Task kapselt eine ALTER INDEX REBUILD-Anweisung mit den folgenden Indexneuerstellungsoptionen:  
  
-   Geben Sie einen Prozentsatz für FILLFACTOR an, oder verwenden Sie den ursprünglichen Wert für FILLFACTOR.  
  
-   Legen Sie SORT_IN_TEMPDB = ON fest, um das Zwischenergebnis der Sortierung zu speichern, mit dem der Index in tempdb neu erstellt wird. Wenn diese Option auf OFF festgelegt ist, wird das Ergebnis in derselben Datenbank wie der Index gespeichert.  
  
-   Legen Sie PAD_INDEX = ON fest, um den mit FILLFACTOR angegebenen freien Speicherplatz den Zwischenebenenseiten des Indexes zuzuordnen.  
  
-   Legen Sie IGNORE_DUP_KEY = ON fest, damit ein Einfügevorgang für mehrere Zeilen, der Datensätze einschließt, die UNIQUE-Einschränkungen verletzen, diejenigen Datensätze einfügen kann, die keine UNIQUE-Einschränkungen verletzen.  
  
-   Legen Sie ONLINE = ON fest, um keine Tabellensperren zu verwenden, damit Abfragen oder Updates für die zugrunde liegende Tabelle während der Neuindizierung fortgesetzt werden können.  
  
    > [!NOTE]  
    >  Onlineindexvorgänge sind nicht in jeder Edition von [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]verfügbar. Eine Liste der Funktionen, die von den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Editionen unterstützt werden, finden Sie unter [Von den SQL Server 2016-Editionen unterstützte Funktionen](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
-   Geben Sie einen Wert für MAXDOP an, um die Anzahl der Prozessoren zu begrenzen, die bei der Ausführung paralleler Pläne verwendet werden.  
  
-   Geben Sie WAIT_AT_LOW_PRIORITY, MAX_DURATION und ABORT_AFTER_WAIT an, um zu steuern, wie lange der Indexvorgang auf Sperren mit niedriger Priorität wartet.  
  
 Weitere Informationen zur ALTER INDEX-Anweisung und zu den Indexneuerstellungsoptionen finden Sie unter [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md).  
  
> [!IMPORTANT]  
>  Die Zeit, die der Task zum Erstellen der vom Task ausgeführten [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisung benötigt, ist proportional zur Anzahl der vom Task neu erstellten Indizes. Falls für den Task konfiguriert ist, dass die Indizes in allen Tabellen und Sichten in einer Datenbank mit vielen Indizes neu erstellt werden, oder dass Indizes in mehreren Datenbanken neu erstellt werden, kann das Generieren der Transact-SQL-Anweisung lange dauern.  
  
## <a name="configuration-of-the-rebuild-index-task"></a>Konfiguration des Tasks "Index neu erstellen"  
 Eigenschaften können Sie mit dem [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer festlegen. Dieser Task ist im **-Designer in der** Toolbox **im Abschnitt** Wartungsplantasks [!INCLUDE[ssIS](../../includes/ssis-md.md)] enthalten.  
  
 Klicken Sie auf das folgende Thema, um weitere Informationen zu den Eigenschaften zu erhalten, die Sie im [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer festlegen können:  
  
 [Task „Index neu erstellen“ &#40;Wartungsplan&#41;](../../relational-databases/maintenance-plans/rebuild-index-task-maintenance-plan.md)  
  
## <a name="related-tasks"></a>Related Tasks  
 Informationen zum Festlegen dieser Eigenschaften im [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer finden Sie unter [Festlegen der Eigenschaften eines Tasks oder Containers](./add-or-delete-a-task-or-a-container-in-a-control-flow.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Integration Services-Tasks](../../integration-services/control-flow/integration-services-tasks.md)   
 [Ablaufsteuerung](../../integration-services/control-flow/control-flow.md)  
  
