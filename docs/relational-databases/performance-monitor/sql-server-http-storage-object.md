---
title: SQL Server, HTTP_STORAGE_OBJECT | Microsoft-Dokumentation
description: Hier lernen Sie das „SQLServer:HTTP-Speicher“-Leistungsobjekt kennen, das Leistungsindikatoren enthält, mit denen ein Azure-Speicherkonto überwacht wird.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
ms.assetid: ae849f79-c581-42a5-a5cc-0a9ebea171b9
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 6ca729c6097b02142e4459a0c11e5ad77741ea37
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505706"
---
# <a name="sql-server-http-storage"></a>SQL Server, HTTP-Speicher
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Das **SQLServer:HTTP-Speicher**-Leistungsobjekt umfasst Leistungsindikatoren, mit denen ein Microsoft Azure-Speicherkonto überwacht wird. Mithilfe des Features [SQL Server-Datendateien in Microsoft Azure](../../relational-databases/databases/sql-server-data-files-in-microsoft-azure.md) können Sie Datenbankdateien in Azure Storage Blobs speichern. Dieses Leistungsobjekt behandelt jedes Azure Storage-Konto als getrenntes Laufwerk.  
  
|Name des Leistungsindikators|BESCHREIBUNG|  
|------------------|-----------------|  
|**Mittlere Bytes/Lesevorgang**|Die durchschnittliche Anzahl von Bytes, die pro Lesevorgang aus dem HTTP-Speicher übertragen wurde.|  
|**Mittlere Bytes/Übertragung**|Die durchschnittliche Anzahl von Bytes, die während Lese- oder Schreibvorgängen aus dem HTTP-Speicher übertragen wurde.|  
|**Mittlere Bytes/Schreibvorgang**|Die durchschnittliche Anzahl von Bytes, die pro Schreibvorgang aus dem HTTP-Speicher übertragen wurde.|  
|**Durchschn. Mikrosekunden/Lesevorgang**|Die durchschnittliche Anzahl von Mikrosekunden, die ein Lesevorgang aus dem HTTP-Speicher dauert.|  
|**Durchschn. Mikrosekunden/Lesevorgang (Abschluss)**|Die durchschnittliche Anzahl Mikrosekunden, die der Abschluss eines HTTP-Lesezugriffs auf den Speicher benötigt.| 
|**Durchschn. Mikrosekunden/Übertragung**|Die durchschnittliche Anzahl von Mikrosekunden, die eine Übertragung in den HTTP-Speicher dauert.|  
|**Durchschn. Mikrosekunden/Schreibvorgang**|Die durchschnittliche Anzahl von Mikrosekunden, die ein Schreibvorgang in den HTTP-Speicher dauert.|  
|**Durchschn. Mikrosekunden/Schreibvorgang (Abschluss)**|Die durchschnittliche Anzahl Mikrosekunden, die der Abschluss eines HTTP-Schreibzugriffs auf den Speicher benötigt.|  
|**E/A-Zugriffsfehler auf den HTTP-Speicher/s**|Die Anzahl der Schreibanforderungen mit Fehler, die pro Sekunde an den HTTP-Speicher gesendet werden.| 
|**Wiederholungen für HTTP-Speicher-E/A-Vorgänge/s**|Die Anzahl der an den HTTP-Speicher pro Sekunde gesendeten Wiederholungsanforderungen.|  
|**Ausstehende HTTP-Speicher-E/A-Vorgänge**|Die Gesamtanzahl ausstehender E/A-Vorgänge für einen HTTP-Speicher.|  
|**Gelesene Bytes/s**|Die bei Lesevorgängen pro Sekunde aus dem HTTP-Speicher übertragene Datenmenge.|  
|**Lesevorgänge/s**|Die Anzahl der Lesevorgänge im HTTP-Speicher pro Sekunde.|  
|**Gesamtanzahl Bytes/s**|Die bei Lese- oder Schreibvorgängen pro Sekunde aus dem HTTP-Speicher übertragene Datenmenge.|  
|**Übertragungen/s**|Die Anzahl der Lese- und Schreibvorgänge im HTTP-Speicher pro Sekunde.|  
|**Geschriebene Bytes/s**|Die bei Schreibvorgängen pro Sekunde aus dem HTTP-Speicher übertragene Datenmenge.|  
|**Schreibvorgänge/s**|Die Anzahl der Schreibvorgänge im HTTP-Speicher pro Sekunde.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Überwachen der Ressourcenverwendung &#40;Systemmonitor&#41;](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)  
  
  
