---
title: Transact-SQL-Breakpoints
description: Beim Debuggen können Sie Breakpoints verwenden, um die Ausführung bei Bedarf anzuhalten. Hier finden Sie eine Liste mit Breakpointaufgaben mit Links zu Artikeln, in denen diese beschrieben werden.
titleSuffix: T-SQL debugger
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Transact-SQL debugger, breakpoints
ms.assetid: c234430f-bd94-4d0d-9e74-2bf11681fa50
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 12/04/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 75452eb30bce3f8d4df8727527a021c7a0417276
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339324"
---
# <a name="transact-sql-breakpoints"></a>Transact-SQL-Breakpoints

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Haltepunkte legen fest, dass der [!INCLUDE[tsql](../../includes/tsql-md.md)] -Debugger die Ausführung bei einer bestimmten [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisung unterbricht. Sie können dann den Status der Codeelemente an diesem Punkt anzeigen.

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]

## <a name="breakpoints"></a>Breakpoints

Wenn Sie den [!INCLUDE[tsql](../../includes/tsql-md.md)] -Debugger ausführen, können Sie einen Haltepunkt auf bestimmten Anweisungen umschalten. Wenn die Ausführung eine Anweisung mit einem Haltepunkt erreicht, hält der Debugger die Ausführung an, damit Sie Debuginformationen anzeigen können, z. B. die in Variablen und Parametern vorhandenen Werte.

Sie können Haltepunkte im Editorfenster einzeln verwalten, oder alle zusammen im Fenster **Haltepunkte** . Sie können Haltepunkte bearbeiten, um bestimmte Elemente festzulegen, wie z. B. die speziellen Bedingungen, unter denen der Haltepunkt die Ausführung anhalten soll, oder die Aktionen, die beim Ausführen des Haltepunkts durchgeführt werden sollen.

## <a name="breakpoint-tasks"></a>Haltepunkttasks  
  
|Taskbeschreibung|Thema|  
|----------------------|-----------|  
|Beschreibt, wie die [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisung angegeben wird, bei der der Debugger angehalten werden soll.|[Ein- und Ausschalten eines Breakpoints](./toggle-a-breakpoint.md)|  
|Beschreibt, wie ein Haltepunkt vorübergehend deaktiviert und später erneut aktiviert wird. Beschreibt darüber hinaus, wie ein Haltepunkt gelöscht wird.|[Aktivieren, Deaktivieren und Löschen von Breakpoints](./enable-disable-and-delete-breakpoints.md)|  
|Beschreibt, wie eine Bedingung angegeben wird, die definiert, ob Haltepunkte auf Grundlage der Auswertung eines angegebenen Transact-SQL-Ausdrucks unterbrochen werden.|[Angeben einer Breakpointbedingung](./specify-a-breakpoint-condition.md)|  
|Beschreibt, wie eine Trefferanzahl festgelegt wird, die dazu führt, dass ein Haltepunkt nur dann unterbrochen wird, wenn die Anweisung, die den Haltepunkt enthält, mit einer festgelegten Häufigkeit ausgeführt wurde.|[Angeben einer Trefferanzahl](./specify-a-hit-count.md)|  
|Beschreibt, wie ein Filter festgelegt wird, der bewirkt, dass ein Haltepunkt nur für angegebene Prozesse oder Threads unterbrochen wird.|[Angeben eines Breakpointfilters](./specify-a-breakpoint-filter.md)|  
|Beschreibt, wie eine **Bei Treffer** -Aktion festgelegt wird, bei der es sich um einen Vorgang handelt, der beim Ausführen der Haltepunktanweisung durchgeführt wird. Ein Beispiel hierfür ist das Drucken einer Meldung.|[Angeben einer Breakpointaktion](./specify-a-breakpoint-action.md)|  
|Beschreibt, wie die Position eines Haltepunkts bearbeitet wird.|[Bearbeiten einer Breakpointposition](./edit-a-breakpoint-location.md)|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Transact-SQL-Debuggerinformationen](./transact-sql-debugger-information.md)  
  
