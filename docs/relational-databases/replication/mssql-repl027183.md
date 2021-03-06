---
description: MSSQL_REPL027183
title: MSSQL_REPL027183 | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
helpviewer_keywords:
- MSSQL_REPL027183 error
ms.assetid: 52c271ac-1a0e-43d5-85d4-35886d1efd32
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 32112470b6740eaac429895a394a3ae5cbdec110
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206149"
---
# <a name="mssql_repl027183"></a>MSSQL_REPL027183
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>Meldungsdetails  
  
|attribute|Wert|  
|-|-|  
|Produktname|SQL Server|  
|Ereignis-ID|27183|  
|Ereignisquelle|MSSQLSERVER|  
|Komponente|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Symbolischer Name||  
|Meldungstext|Vom Mergeprozess konnten Änderungen in Artikeln mit parametrisierten Zeilenfiltern nicht aufgezählt werden. Falls dieser Fehler weiterhin auftritt, erhöhen Sie das Abfragetimeout für diesen Prozess, reduzieren Sie die Beibehaltungsdauer für die Veröffentlichung, und verbessern Sie die Indizes für veröffentlichte Tabellen.|  
  
## <a name="explanation"></a>Erklärung  
 Dieser Fehler wird ausgelöst, wenn bei der Verarbeitung von Änderungen in einer gefilterten Veröffentlichung ein Merge-Agent-Timeout auftritt. Das Timeout kann durch eines der folgenden Probleme verursacht werden:  
  
-   Die Optimierung für vorausberechnete Partitionen wird nicht verwendet.  
  
-   In den beim Filtern verwendeten Spalten liegt eine Indexfragmentierung vor.  
  
-   Es sind umfangreiche Tabellen mit Mergemetadaten vorhanden, beispielsweise **MSmerge_tombstone**, **MSmerge_contents** und **MSmerge_genhistory**.  
  
-   Es sind gefilterte Tabellen, die nicht über einen eindeutigen Schlüssel verknüpft sind, und Joinsfilter mit einer großen Anzahl an Tabellen vorhanden.  
  
## <a name="user-action"></a>Benutzeraktion  
 So lösen Sie das Problem:  
  
-   Setzen Sie den Wert des **-QueryTimeOut** -Parameters für den Merge-Agent herauf, damit die Verarbeitung fortgesetzt werden kann und Sie sich der eigentlichen Ursache des Fehlers widmen können. Agentparameter können in den Agentprofilen und in der Befehlszeile angegeben werden. Weitere Informationen finden Sie unter  
  
    -   [Arbeiten mit Replikations-Agent-Profilen](../../relational-databases/replication/agents/work-with-replication-agent-profiles.md)  
  
    -   [Anzeigen und Ändern von Befehlszeilenparametern des Replikations-Agents &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/agents/view-and-modify-replication-agent-command-prompt-parameters.md)  
  
    -   [Replication Agent Executables Concepts](../../relational-databases/replication/concepts/replication-agent-executables-concepts.md)zugeordnet ist.  
  
-   Verwenden Sie, sofern möglich, die Optimierung für vorausberechnete Partitionen. Diese Optimierung wird standardmäßig verwendet, wenn mehrere Veröffentlichungsanforderungen erfüllt sind. Weitere Informationen zu diesen Anforderungen finden Sie unter [Optimieren der Leistung parametrisierter Filter mithilfe vorausberechneter Partitionen](../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md). Wenn die Veröffentlichung diese Anforderungen nicht erfüllt, sollten Sie einen erneuten Entwurf der Veröffentlichung in Betracht ziehen.  
  
-   Geben Sie die kürzestmögliche Einstellung für die Beibehaltungsdauer der Veröffentlichung an, da die Replikation keinen Cleanup der Metadaten in den Veröffentlichungs- und den Abonnementdatenbanken ausführen kann, bevor die Beibehaltungsdauer erreicht wurde. Weitere Informationen finden Sie unter [Subscription Expiration and Deactivation](../../relational-databases/replication/subscription-expiration-and-deactivation.md).  
  
-   Als Teil der Wartung für die Mergereplikation überprüfen Sie gelegentlich die Vergrößerung der Systemtabellen, die mit der Mergereplikation verbunden sind: **MSmerge_contents**, **MSmerge_genhistory**, **MSmerge_tombstone**, **MSmerge_current_partition_mappings** und **MSmerge_past_partition_mappings**. Führen Sie eine regelmäßige Neuindizierung dieser Tabellen durch. Weitere Informationen finden Sie unter [Neuorganisieren und Neuerstellen von Indizes](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md).  
  
-   Stellen Sie sicher, dass die zum Filtern verwendeten Spalten richtig indiziert sind, und erstellen Sie die entsprechenden Indizes gegebenenfalls erneut. Weitere Informationen finden Sie unter [Neuorganisieren und Neuerstellen von Indizes](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md).  
  
-   Legen Sie für Joinsfilter, die auf eindeutigen Spalten basieren, die **join_unique_key** -Eigenschaft fest. Weitere Informationen finden Sie unter [Join Filters](../../relational-databases/replication/merge/join-filters.md).  
  
-   Begrenzen Sie die Anzahl der Tabellen in der Joinsfilterhierarchie. Wenn Sie Joinsfilter für fünf oder mehr Tabellen erstellen, sollten Sie andere Lösungen in Betracht ziehen: Kleinere Tabellen, Tabellen, die nicht geändert werden, oder Tabellen, bei denen es sich primär um Nachschlagetabellen handelt, sollten in diesem Fall nicht gefiltert werden. Verwenden Sie Joinsfilter nur zwischen Tabellen, für die eine Partitionierung auf Abonnements erfolgen muss.  
  
-   Nehmen Sie zwischen Synchronisierungen weniger Änderungen an gefilterten Tabellen vor, oder führen Sie den Merge-Agent häufiger aus. Weitere Informationen zum Angeben von Synchronisierungszeitplänen finden Sie unter [Specify Synchronization Schedules](../../relational-databases/replication/specify-synchronization-schedules.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Fehler- und Ereignisreferenz &#40;Replikation&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
