---
description: MSSQLSERVER_41365
title: MSSQLSERVER_41365 | Microsoft-Dokumentation
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 41365 (Database Engine error)
ms.assetid: 4fc7ec15-b722-4e3d-b7f9-3d39d171e96e
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 1d64a41b237933a62fea8ca7a944837c82dd6d6d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179509"
---
# <a name="mssqlserver_41365"></a>MSSQLSERVER_41365
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Details  
  
| attribute | Wert |  
| :-------- | :---- |  
|Produktname|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|Ereignis-ID|41365|  
|Ereignisquelle|MSSQLSERVER|  
|Komponente|SQLEngine|  
|Symbolischer Name|HK_MERGE_SCHEDULE_ERROR|  
|Meldungstext|Mergeanforderung für Transaktionsbereich [%ld, %ld] in Datenbank %.*ls wurde nicht geplant. Die Prüfpunktdateien, die den Bereich darstellen, sind entweder für Merge oder als Teil eines laufenden Mergevorgangs nicht verfügbar.|  
  
## <a name="explanation"></a>Erklärung  
Die Prüfpunktdateien, die den Bereich darstellen, sind entweder für Merge oder als Teil eines laufenden Mergevorgangs nicht verfügbar.  
  
## <a name="user-action"></a>Benutzeraktion  
Stellen Sie einen besseren Transaktionsbereich für den Merge-/Wartevorgang bereit, bevor Sie erneut die gleiche Anforderung ausgeben. Weitere Informationen finden Sie unter [In-Memory OLTP &#40;Arbeitsspeicheroptimierung&#41;](~/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).  
  
## <a name="see-also"></a>Weitere Informationen  
[In-Memory-OLTP &#40;Arbeitsspeicheroptimierung&#41;](~/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
