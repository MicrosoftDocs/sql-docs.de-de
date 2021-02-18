---
title: Ändern eines Filters
titleSuffix: SQL Server Profiler
description: Hier erfahren Sie, wie Sie einen Ablaufverfolgungsfilter im SQL Server Profiler ändern, um benötigte Informationen zu sammeln. Hier erfahren Sie, wie Ablaufverfolgungsfilter die Leistung der Datenbank beeinflussen.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 8b317813-4918-4485-b930-77b1951aa00c
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: dc2171ea2aaf418eae0fa277a8f4144dc4e7ca9d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345046"
---
# <a name="modify-a-filter-sql-server-profiler"></a>Ändern eines Filters (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Sie können Ablaufverfolgungsvorlagen, die Ablaufverfolgungsdefinitionen enthalten, Filter hinzufügen, um die Anzahl der von einer Ablaufverfolgung gesammelten Ereignisse zu beschränken. Durch die Beschränkung der gesammelten Ereignisse können die bei der Ablaufverfolgung entstehenden Leistungseinbußen reduziert werden. Wenn Sie Filter für eine Ablaufverfolgungsvorlage einrichten und feststellen, dass bei der Ablaufverfolgung nicht die von Ihnen benötigten Informationen gesammelt werden, können Sie den Filter bearbeiten.  
  
### <a name="to-modify-a-filter"></a>So ändern Sie einen Filter  
  
1.  Öffnen Sie in [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]die Vorlage für den Ablaufverfolgungsfilter, den Sie ändern möchten. Klicken Sie im Menü **Datei** auf **Vorlagen**, und wählen Sie dann **Vorlage bearbeiten** aus.  
  
2.  Wählen Sie auf der Registerkarte **Allgemein** des Dialogfelds **Eigenschaften der Ablaufverfolgungsvorlage** eine Vorlage aus der Liste **Vorlagennamen auswählen** aus.  
  
3.  Klicken Sie auf die Registerkarte **Ereignisauswahl** .  
  
     Die Registerkarte **Ereignisauswahl** enthält ein Rastersteuerelement. Bei dem Rastersteuerelement handelt es sich um eine Tabelle, die alle bei der Ablaufverfolgung zu berücksichtigenden Ereignisklassen enthält. Die Tabelle enthält für jede Ereignisklasse eine Zeile. Abhängig von dem Typ und der Version des Servers, zu dem eine Verbindung hergestellt wurde, können sich die Ereignisklassen geringfügig unterscheiden. Die Ereignisklassen werden in der Spalte **Events** des Rasters identifiziert und nach Ereigniskategorie gruppiert. In den übrigen Spalten sind die Datenspalten aufgeführt, die für jede Ereignisklasse zurückgegeben werden können.  
  
4.  Klicken Sie auf **Spaltenfilter**.  
  
5.  Klicken Sie im Dialogfeld **Filter bearbeiten** auf den Wert neben dem zu bearbeitenden Vergleichsoperator, und geben Sie einen neuen Wert ein bzw. löschen Sie einen Wert. Darüber hinaus können Sie zusätzliche Filter hinzufügen.  
  
6.  Klicken Sie auf **OK** , und speichern Sie die Vorlage.  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)  
  
  
