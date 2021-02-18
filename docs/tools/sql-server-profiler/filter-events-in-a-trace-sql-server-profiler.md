---
title: Filtern von Ereignissen in einer Ablaufverfolgung
titleSuffix: SQL Server Profiler
description: In diesem Artikel wird erläutert, wie Sie einen Filter festlegen, um die Ereignisse einzugrenzen, die der SQL Server Profiler während einer Ablaufverfolgung erfasst. Außerdem erfahren Sie mehr über die Formate, die für bestimmte Filter erforderlich sind.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 0fd63573-3b35-4f67-9e1e-ed9aabee11a8
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: c80703ac5c23ba40007528c6285eb74e15e80264
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345059"
---
# <a name="filter-events-in-a-trace-sql-server-profiler"></a>Filtern von Ereignissen in einer Ablaufverfolgung (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Durch Filter werden die in einer Ablaufverfolgung aufgezeichneten Ereignisse eingeschränkt. Ist kein Filter eingerichtet, werden alle Ereignisse der ausgewählten Ereignisklassen in der Ablaufverfolgungsausgabe zurückgegeben. Es ist nicht obligatorisch, einen Filter für eine Ablaufverfolgung festzulegen. Jedoch wird durch Filter der bei der Ablaufverfolgung entstehende Verarbeitungsaufwand verringert.  
  
 Sie können Ablaufverfolgungsdefinitionen Filter hinzufügen, indem Sie die Registerkarte **Ereignisauswahl** des Dialogfelds **Ablaufverfolgungseigenschaften** oder des Dialogfelds **Eigenschaften der Ablaufverfolgungsvorlage** verwenden.  
  
### <a name="to-filter-events-in-a-trace"></a>So filtern Sie Ereignisse in einer Ablaufverfolgung  
  
1.  Klicken Sie im Dialogfeld **Ablaufverfolgungseigenschaften** oder **Eigenschaften der Ablaufverfolgungsvorlage** auf die Registerkarte **Ereignisauswahl** .  
  
     Die Registerkarte **Ereignisauswahl** enthält ein Rastersteuerelement. Bei dem Rastersteuerelement handelt es sich um eine Tabelle, die alle bei der Ablaufverfolgung zu berücksichtigenden Ereignisklassen enthält. Die Tabelle enthält für jede Ereignisklasse eine Zeile. Abhängig von dem Typ und der Version des Servers, zu dem eine Verbindung hergestellt wurde, können sich die Ereignisklassen geringfügig unterscheiden. Die Ereignisklassen werden in der Spalte **Ereignisse** des Rasters identifiziert und nach Ereigniskategorie gruppiert. In den übrigen Spalten sind die Datenspalten aufgeführt, die für jede Ereignisklasse zurückgegeben werden können.  
  
2.  Klicken Sie auf **Spaltenfilter.**  
  
     Daraufhin wird das Dialogfeld **Filter bearbeiten** angezeigt. Das Dialogfeld **Filter bearbeiten** enthält eine Liste von Vergleichsoperatoren, mit denen Sie Ereignisse in einer Ablaufverfolgung filtern können.  
  
3.  Wenn Sie einen Filter anwenden möchten, klicken Sie auf den Vergleichsoperator, und geben Sie den gewünschten Filterwert ein.  
  
4.  Klicken Sie auf **OK**.  
  
 **Überlegungen:**  
  
-   Wenn Sie Filterbedingungen für die Datenspalten **StartTime** und **EndTime** der Registerkarte Ereignisauswahl festlegen, stellen Sie Folgendes sicher:  
  
    -   Das von Ihnen eingegebene Datum entspricht diesem Format: `YYYY/MM/DD HH:mm:sec`.  
  
         ODER  
  
    -   Im Dialogfeld **Allgemeine Optionen** ist die Option **Einstellungen für Land/Region zum Anzeigen von Datums- und Uhrzeitwerten verwenden** aktiviert. Klicken Sie im [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]-Menü **Extras** auf **Optionen**, um das Dialogfeld **Allgemeine Optionen** anzuzeigen.  
  
         -UND-  
  
    -   Das von Ihnen eingegebene Datum liegt zwischen dem 1. Januar 1753 und dem 31. Dezember 9999.  
  
-   Wenn die Ablaufverfolgung für Ereignisse aus den Hilfsprogrammen **osql** oder **sqlcmd** ausgeführt wird, muss den Filtern in der **%** -Datenspalte immer **%** angehängt werden.  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)  
  
  
