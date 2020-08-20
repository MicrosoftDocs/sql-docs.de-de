---
description: MSSQLSERVER_7308
title: MSSQLSERVER_7308 | Microsoft-Dokumentation
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 7308 (Database Engine error)
- single-threaded apartment mode
ms.assetid: cca9eab9-afb9-463d-abfd-0802257e2c99
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 568cf7ae707b3e0b8da28b0a6c0ae9b076267c6f
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88487028"
---
# <a name="mssqlserver_7308"></a>MSSQLSERVER_7308
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Details  
  
| attribute | Wert |  
| :-------- | :---- |  
|Produktname|SQL Server|  
|Ereignis-ID|7308|  
|Ereignisquelle|MSSQLSERVER|  
|Komponente|SQLEngine|  
|Symbolischer Name|RMT_STA_DISABLED|  
|Meldungstext|Der OLE DB-Anbieter ‚%ls’ kann nicht für verteilte Abfragen verwendet werden, weil der Anbieter so konfiguriert ist, dass er im Singlethread-Apartment-Modus ausgeführt wird.|  
  
## <a name="explanation"></a>Erklärung  
Sie haben einen OLE DB-Anbieter verwendet, der so konfiguriert ist, dass er im Singlethread-Apartment-Modus (STA) ausgeführt wird. OLE DB-Anbieter, die im Singlethread-Apartment-Modus (STA) ausgeführt werden, können nicht für verteilte Abfragen verwendet werden.  
  
## <a name="user-action"></a>Benutzeraktion  
Um diesen Fehler zu beheben, konfigurieren Sie den Anbieter so, dass er im Multithread-Apartment-Modus (MTA) ausgeführt wird. Wenn der Anbieter den MTA nicht unterstützt und der Anbieter nicht auf eine Version aktualisiert werden kann, die MTA unterstützt, ziehen Sie in Erwägung, den Anbieter so zu konfigurieren, dass er außerhalb des Prozesses ausgeführt wird. Der Anbieterhersteller kann Ihnen mitteilen, ob der Anbieter den MTA oder die Ausführung außerhalb des Prozesses unterstützt.  
  
