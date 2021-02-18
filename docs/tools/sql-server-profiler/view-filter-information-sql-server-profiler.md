---
title: Anzeigen von Filterinformationen
titleSuffix: SQL Server Profiler
description: Erfahren Sie, wie Sie die Filter anzeigen, die der SQL Server Profiler derzeit auf Datenspalten anwendet, um die Ereignisse zu begrenzen, deren Ablauf verfolgt wird.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 8d002dea-376a-452c-b3ca-3e93656ed75f
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: c4031cd865c84b9bc6171b25d40de21ab29379ef
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338605"
---
# <a name="view-filter-information-sql-server-profiler"></a>Anzeigen von Filterinformationen (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

In diesem Thema wird beschrieben, wie Filter von Datenspalten für Ereignisklassen mithilfe von [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]angezeigt werden können.  
  
### <a name="to-view-filter-information"></a>So zeigen Sie Filterinformationen an  
  
1.  Öffnen Sie eine Ablaufverfolgungsdatei, eine Ablaufverfolgungstabelle oder ein SQL-Skript, und klicken Sie im Menü **Datei** auf **Eigenschaften**. Wenn Sie eine Ablaufverfolgungsvorlage bearbeiten oder eine neue Ablaufverfolgung erstellen, können Sie diesen Schritt auslassen.  
  
2.  Klicken Sie auf der Registerkarte **Ereignisauswahl** mit der rechten Maustaste auf den Namen der Datenspalte für den anzuzeigenden Filter, und klicken Sie dann auf **Spaltenfilter bearbeiten**.  
  
3.  Erweitern Sie im Dialogfeld **Filter bearbeiten** die Vergleichsoperatoren für den Filter, um den zugewiesenen Wert für das angegebene Kriterium anzuzeigen. Wiederholen Sie Schritt 2 und 3 für alle Spalten, für die Sie Filterinformationen anzeigen möchten.  
  
> [!NOTE]  
>  Vergleichsoperatoren mit zugewiesenen Werten sind fett formatiert.  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)  
  
  
