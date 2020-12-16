---
title: Engine für erweiterte Ereignisse von SQL Server
description: Die Engine für erweiterte Ereignisse von SQL Server ermöglicht die Definition von Ereignissen und die Verarbeitung von Ereignisdaten, verwaltet erweiterte Ereignisse sowie eine Liste von Sitzungen.
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xevents
ms.topic: conceptual
helpviewer_keywords:
- extended events [SQL Server], engine
ms.assetid: d74642a5-42b9-4a15-aa3d-f98bfe695050
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 339e6ada7ae707d4bb48f5b1d3c1eb3b52e94e72
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97481371"
---
# <a name="sql-server-extended-events-engine"></a>Engine für erweiterte Ereignisse von SQL Server

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Extended Events-Engine ist eine Sammlung von Diensten und Objekten, die:  
  
-   die Definition von Ereignissen ermöglichen,  
  
-   die Verarbeitung von Ereignisdaten ermöglichen,  
  
-   Dienste und Objekte für erweiterte Ereignisse im System verwalten und  
  
-   eine Liste von Sitzungen für erweiterte Ereignisse führen und den Zugriff auf diese Liste verwalten.  
  
 Die Engine für erweiterte Ereignisse selbst stellt keine Ereignisse oder beim Auslösen eines Ereignisses erforderlichen Aktionen bereit. Die Prozesse, die die Engine für erweiterte Ereignisse verwenden, definieren die Interaktion mit der Engine. Diese Prozesse fügen Ereignispunkte hinzu und stellen die bei Auslösung eines Ereignisses erforderlichen Aktionen bereit.  
  
 Die folgende Abbildung zeigt eine vereinfachte Ansicht einer Sitzung für erweiterte Ereignisse. Weitere Informationen finden Sie unter [SQL Server Extended Events Sessions](../../relational-databases/extended-events/sql-server-extended-events-sessions.md).  
  
 ![Detaillierte Architektur von erweiterten Ereignissen](../../relational-databases/extended-events/media/xearchitecturedetailed.gif "Detaillierte Architektur von erweiterten Ereignissen")  
  
 Beachten Sie Folgendes:  
  
-   Jeder Windows-Prozess kann über ein oder mehrere Module (**Win32-Prozess**, **Win32-Modul**) verfügen. Diese werden auch als *Binärdateien* oder *ausführbare Module* bezeichnet.  
  
-   Jedes Windows-Prozessmodul kann mindestens ein Paket für erweiterte Ereignisse (**Paket**) enthalten, das wiederum mindestens ein Objekt für erweiterte Ereignisse (**Typ**, **Ziel**, **Aktion**, **Zuordnung**, **Prädikat** und **Ereignis**) enthalten kann.  
  
-   Ein Hostprozess kann nur eine Instanz der Engine für erweiterte Ereignisse (**Engine für erweiterte Ereignisse**) aufweisen. Dieses führt die folgenden Aufgaben aus:  
  
    -   Es verwaltet einige Aspekte der Sitzung (z. B. das Aufzählen von Sitzungen).  
  
    -   Es übernimmt die Verteilung (**Verteiler**). Dies ist mit einem Threadpool vergleichbar.  
  
    -   Es verarbeitet Speicherpuffer (**Puffer**) für Ereignisse. Wenn Puffer aufgefüllt sind, werden sie an Ziele verteilt.  
  
-   Wenn eine Sitzung erstellt wurde und optional Ereignisse an die Sitzung (**Sitzungskontext**) gebunden wurden:  
  
    -   Es können auch Instanzen von Zielen (**Zielinstanz**) erstellt und der Sitzung hinzugefügt werden.  
  
    -   Wenn Puffer aufgefüllt sind, werden diese an Ziele verteilt.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Erweiterte Ereignisse](../../relational-databases/extended-events/extended-events.md)  
  
  
