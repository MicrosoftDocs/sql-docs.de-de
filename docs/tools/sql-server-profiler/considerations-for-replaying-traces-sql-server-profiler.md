---
title: Überlegungen zum Wiedergeben von Ablaufverfolgungen
titleSuffix: SQL Server Profiler
description: Finden Sie heraus, welche Vorgänge, gespeicherten Prozeduren, Vorlagen und Protokollaktivitäten den SQL Server Profiler an der Wiedergabe von Ablaufverfolgungen hindern.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 73fa339f-b71a-4be4-97ca-d4ae84c8b90b
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/06/2017
ms.openlocfilehash: d06b9769a76d7e6555e0dc671782fcd062b4574f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349424"
---
# <a name="considerations-for-replaying-traces-sql-server-profiler"></a>Überlegungen zum Wiedergeben von Ablaufverfolgungen (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] können die folgenden Arten von Ablaufverfolgungen nicht wiedergegeben werden:  
  
-   Ablaufverfolgungen, die die Transaktionsreplikation und andere Transaktionsprotokollaktivität enthalten. Diese Ereignisse werden ausgelassen. Andere Replikationsarten markieren nicht das Transaktionsprotokoll und sind nicht betroffen.  
  
-   Ablaufverfolgungen, die Vorgänge mit global eindeutigen Bezeichnern (Globally Unique Identifier, GUID) enthalten. Diese Ereignisse werden ausgelassen.  
  
-   Ablaufverfolgungen, die Vorgänge mit den Spalten **text**, **ntext** und **image** mit dem Hilfsprogramm **bcp** , den Anweisungen BULK INSERT, READTEXT, WRITETEXT und UPDATETEXT und Volltextvorgängen enthalten. Diese Ereignisse werden ausgelassen.  
  
-   Ablaufverfolgungen, die eine Sitzungsbindung enthalten: gespeicherte Systemprozeduren **sp_getbindtoken** und **sp_bindsession** . Diese Ereignisse werden ausgelassen.  
  
> [!NOTE]  
>  Wenn Sie nicht die vorkonfigurierte Wiedergabevorlage (**TSQL_Replay**) verwenden und nicht alle erforderlichen Daten erfassen, wird die Ablaufverfolgung von [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] nicht wiedergegeben. Weitere Informationen finden Sie unter [Replay Requirements](../../tools/sql-server-profiler/replay-requirements.md).  
  
 Informationen zu den Berechtigungen, die zum Wiedergeben einer Ablaufverfolgung erforderlich sind, finden Sie unter [Permissions Required to Run SQL Server Profiler](../../tools/sql-server-profiler/permissions-required-to-run-sql-server-profiler.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [bcp (Hilfsprogramm)](../../tools/bcp-utility.md)   
 [Ereignisklassen in SQL Server: Referenz](../../relational-databases/event-classes/sql-server-event-class-reference.md)   
 [sp_getbindtoken &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-getbindtoken-transact-sql.md)   
 [sp_bindsession (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-bindsession-transact-sql.md)   
 [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [READTEXT &#40;Transact-SQL&#41;](../../t-sql/queries/readtext-transact-sql.md)   
 [WRITETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/writetext-transact-sql.md)   
 [UPDATETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/updatetext-transact-sql.md)  
  
  
