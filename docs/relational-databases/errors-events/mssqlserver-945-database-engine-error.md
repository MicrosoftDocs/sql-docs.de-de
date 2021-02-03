---
description: MSSQLSERVER_945
title: MSSQLSERVER_945 | Microsoft-Dokumentation
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 945 (Database Engine error)
ms.assetid: ee501d13-0bd9-4627-896c-ed5b1bdb88b3
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: f9f8a4016736e24b56ee199baa67f761a9dec12d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187639"
---
# <a name="mssqlserver_945"></a>MSSQLSERVER_945
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Details  
  
| attribute | Wert |  
| :-------- | :---- |  
|Produktname|SQL Server|  
|Ereignis-ID|945|  
|Ereignisquelle|MSSQLSERVER|  
|Komponente|SQLEngine|  
|Symbolischer Name|DB_IS_SHUTDOWN|  
|Meldungstext|Die '%.*ls'-Datenbank kann nicht geöffnet werden, da auf einige Dateien nicht zugegriffen werden kann oder nicht genügend Platz im Arbeitsspeicher oder auf dem Datenträger zur Verfügung steht.  Ausführliche Informationen finden Sie im SQL Server-Fehlerprotokoll.|  
  
## <a name="explanation"></a>Erklärung  
Der Zugriff auf die Datenbank war nicht möglich, da Dateien oder andere Ressourcen fehlen.  
  
## <a name="user-action"></a>Benutzeraktion  
Zusätzliche Informationen zum Platz im Arbeitsspeicher, auf dem Datenträger und zum Berechtigungsfehler finden Sie im Fehlerprotokoll. Bestätigen Sie den Speicherort der MDF- und NDF-Dateien für die entsprechende Datenbank, und bestätigen Sie, dass das von [!INCLUDE[ssDE](../../includes/ssde-md.md)] verwendete Konto über eine Berechtigung für den Zugriff auf diese Dateien verfügt. Starten Sie die Datenbank neu, nachdem das Problem behoben wurde. Verwenden Sie dazu ALTER DATABASE, und legen Sie die Datenbank auf ONLINE fest.  
  
