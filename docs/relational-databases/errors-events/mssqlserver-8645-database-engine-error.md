---
description: MSSQLSERVER_8645
title: MSSQLSERVER_8645 | Microsoft-Dokumentation
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 8645 (Database Engine error)
ms.assetid: 63d6d6d7-3850-4061-8e96-b1fa665e3180
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 8f1772c8c73fb18173916b91ea14adf3cf997f4d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205673"
---
# <a name="mssqlserver_8645"></a>MSSQLSERVER_8645
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Details  
  
| attribute | Wert |  
| :-------- | :---- |  
|Produktname|SQL Server|  
|Ereignis-ID|8645|  
|Ereignisquelle|MSSQLSERVER|  
|Komponente|SQLEngine|  
|Symbolischer Name|MEMTIMEDOUT_ERR|  
|Meldungstext|Timeout beim Warten auf die Arbeitsspeicherressourcen für die Abfrageausführung. Führen Sie die Abfrage erneut aus.|  
  
## <a name="explanation"></a>Erklärung  
Timeout beim Warten auf die Arbeitsspeicherressourcen für die Abfrageausführung im Ressourcenpool 'default' (257).  
  
## <a name="user-action"></a>Benutzeraktion  
Wenn Sie keine Ressourcenkontrolle verwenden, empfehlen wir, dass Sie den gesamten Serverstatus und die Auslastung oder die Ressourcenpooleinstellungen bzw. Einstellungen für Arbeitsauslastungsgruppen überprüfen.  
  
In der folgenden Liste werden allgemeine Schritte erläutert, die bei der Problembehandlung von Arbeitsspeicherfehlern helfen:  
  
1.  Überprüfen Sie, ob andere Anwendungen oder Dienste Arbeitsspeicher auf dem Server beanspruchen. Rekonfigurieren Sie weniger kritische Anwendungen oder Dienste, damit sie weniger Speicher beanspruchen.  
  
2.  Beginnen Sie mit der Erfassung von Leistungsüberwachungsindikatoren für **SQL Server: Puffer-Manager**, **SQL Server: Speicher-Manager**.  
  
3.  Überprüfen Sie die folgenden SQL Server-Speicherkonfigurationsparameter:  
  
    -   **Max. Serverarbeitsspeicher**  
  
    -   **Min. Serverarbeitsspeicher**  
  
    -   **Min. Arbeitsspeicher pro Abfrage**  
  
    Achten Sie auf ungewöhnliche Einstellungen. Berichtigen Sie sie bei Bedarf. Konto für erhöhte Arbeitsspeicheranforderungen für [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. Die Standardeinstellungen werden unter "Festlegen von Serverkonfigurationsoptionen" in der SQL Server-Onlinedokumentation aufgeführt.  
  
4.  Verfolgen Sie die Ausgabe von DBCC MEMORYSTATUS und deren Veränderungen beim Anzeigen der Fehlermeldungen.  
  
5.  Überprüfen Sie die Arbeitsauslastung (z. B. Anzahl gleichzeitiger Sitzungen, derzeit ausgeführte Abfragen).  

Durch die folgenden Aktionen kann [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mehr Arbeitsspeicher zur Verfügung gestellt werden:  
  
-   Wenn Anwendungen außer SQL Server Ressourcen verbrauchen, sollten Sie diese Anwendungen beenden oder auf einem separaten Server ausführen. Dadurch fällt der Mangel an externem Arbeitsspeicher weg.  
  
-   Wenn Sie **Max. Serverarbeitsspeicher**  konfiguriert haben, erhöhen Sie dessen Wert.  
  
Führen Sie die folgenden DBCC-Befehle aus, um mehrere SQL Server-Speichercaches freizugeben.  
  
-   DBCC FREESYSTEMCACHE  
  
-   DBCC FREESESSIONCACHE  
  
-   DBCC FREEPROCCACHE  
  
Wenn das Problem weiterhin besteht, müssen Sie weitere Untersuchungen ausführen und möglicherweise die Arbeitsauslastung reduzieren.  
  
