---
title: Ableiten einer Vorlage aus einer Ablaufverfolgungsdatei oder Ablaufverfolgungstabelle
titleSuffix: SQL Server Profiler
description: In diesem Artikel erfahren Sie, wie Sie den SQL Server Profiler verwenden, um eine Ablaufverfolgungsvorlage aus einer vorhandenen Ablaufverfolgungsdatei oder aus einer in einer Datenbank gespeicherten Ablaufverfolgungstabelle erstellen.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 305817b7-4d23-49fb-9e6c-4d34359877bf
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 02af487c0d7301ce7d5d71ff046b90fb25efc8ab
ms.sourcegitcommit: a9f16d7819ed0e2b7ad8f4a7d4d2397437b2bbb2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2020
ms.locfileid: "88713708"
---
# <a name="derive-a-template-from-a-trace-file-or-trace-table-sql-server-profiler"></a>Ableiten einer Vorlage von einer Ablaufverfolgungsdatei oder Ablaufverfolgungstabelle (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In diesem Thema wird beschrieben, wie Sie mithilfe von [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]eine Ablaufverfolgungsvorlage aus einer vorhandenen Ablaufverfolgungsdatei oder Ablaufverfolgungstabelle erstellen.  
  
### <a name="to-derive-a-template-from-a-trace-file-or-trace-table"></a>So leiten Sie eine Vorlage von einer Ablaufverfolgungsdatei oder Ablaufverfolgungstabelle ab  
  
1.  Öffnen Sie die Ablaufverfolgungsdatei oder Ablaufverfolgungstabelle, die Sie als Grundlage für die Vorlage verwenden möchten. Weitere Informationen finden Sie unter [Öffnen einer Ablaufverfolgungsdatei &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/open-a-trace-file-sql-server-profiler.md) oder den Optimierungsratgeber von [Öffnen einer Ablaufverfolgungstabelle &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/open-a-trace-table-sql-server-profiler.md)die richtigen Ereignisse und Spalten aufgezeichnet werden.  
  
2.  Zeigen Sie im Menü **Datei** auf **Speichern unter**, und klicken Sie dann auf **Ablaufverfolgungsvorlage**.  
  
3.  Geben Sie einen Namen ein, oder wählen Sie einen aus der Liste aus. Klicken Sie auf **OK**.  
  
> [!NOTE]  
>  Bei Verwendung einer vorhandenen Vorlagendatei werden Sie gefragt, ob Sie die Datei überschreiben möchten.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Erstellen einer Ablaufverfolgungsvorlage &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/create-a-trace-template-sql-server-profiler.md)   
 [Modify a Trace Template &#40;SQL Server Profiler&#41; (Ändern einer Ablaufverfolgungsvorlage &#40;SQL Server Profiler&#41)](./modify-trace-templates.md?view=sql-server-ver15)   
 [Ableiten einer Vorlage von einer zurzeit ausgeführten Ablaufverfolgung &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/derive-a-template-from-a-running-trace-sql-server-profiler.md)   
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)  
  
