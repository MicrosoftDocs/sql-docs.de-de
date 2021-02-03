---
description: MSSQLSERVER_1105
title: MSSQLSERVER_1105 | Microsoft-Dokumentation
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 1105 (Database Engine error)
ms.assetid: e7f4ad02-8c7f-4bb9-9781-2c86253f2138
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 26b6adb58381b9dd9e02f378743405f602795bbd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197202"
---
# <a name="mssqlserver_1105"></a>MSSQLSERVER_1105
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Details  
  
| attribute | Wert |  
| :-------- | :---- |  
|Produktname|SQL Server|  
|Ereignis-ID|1105|  
|Ereignisquelle|MSSQLSERVER|  
|Komponente|SQLEngine|  
|Symbolischer Name|NO_MORE_SPACE_IN_FG|  
|Meldungstext|Speicherplatz für das '%.*ls'%.\*ls-Objekt in der '%.\*ls'-Datenbank konnte nicht zugeordnet werden, da die '%.\*ls'-Dateigruppe voll ist. Speicherplatz kann durch Löschen nicht benötigter Dateien, Löschen von Objekten in der Dateigruppe, Hinzufügen von Dateien zur Dateigruppe oder Festlegen der automatischen Vergrößerung für vorhandene Dateien in der Dateigruppe gewonnen werden.|  
  
## <a name="explanation"></a>Erklärung  
In einer Dateigruppe ist kein Speicherplatz verfügbar.  
  
## <a name="user-action"></a>Benutzeraktion  
Durch die folgenden Aktionen kann Speicherplatz in der Dateigruppe verfügbar gemacht werden:  
  
-   Aktivieren Sie die automatische Vergrößerung.  
  
-   Fügen Sie der Dateigruppe weitere Dateien hinzu.  
  
-   Geben Sie Speicherplatz frei, indem Sie Indizes oder Tabellen löschen, die nicht mehr benötigt werden.  
  
-   Weitere Informationen finden Sie unter "Problembehandlung bei unzureichendem Speicherplatz" in der SQL Server-Onlinedokumentation.  
  
> [!NOTE]  
> Wenn sich ein Index in mehreren Dateien befindet, kann **ALTER INDEX REORGANIZE** den Fehler 1105 zurückgeben, wenn eine der Dateien voll ist. Der Neuorganisationsvorgang wird blockiert, wenn während des Prozesses versucht wird, Zeilen in die volle Datei zu verschieben. Um diese Einschränkung zu umgehen, führen Sie **ALTER INDEX REBUILD** anstelle von **ALTER INDEX REORGANIZE** durch, oder erhöhen Sie die Dateivergrößerungsgrenze für alle Dateien, die voll sind.  
  
