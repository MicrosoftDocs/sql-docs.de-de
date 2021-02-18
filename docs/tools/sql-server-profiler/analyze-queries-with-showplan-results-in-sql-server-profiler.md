---
title: Analysieren von Abfragen mit Showplanergebnissen
titleSuffix: SQL Server Profiler
description: In diesem Artikel erfahren Sie, wie Sie Showplanergebnisse aus einer Ablaufverfolgung extrahieren, damit der SQL Server Profiler Informationen zum Abfrageplan anzeigt. Außerdem erfahren Sie, wie Sie eine Tabelle der Showplanablaufverfolgungsereignisse anzeigen.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 6a2f7727-141c-4f59-8613-2e452bc78467
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 31771ddc584d72dee2b3c859b349dd5752e4d4d5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349455"
---
# <a name="analyze-queries-with-showplan-results-in-sql-server-profiler"></a>Analysieren von Abfragen mit SHOWPLAN-Ergebnissen in SQL Server Profiler

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Sie können einer Ablaufverfolgungsdefinition Showplan-Ereignisklassen hinzufügen, die bewirken, dass von [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] Abfrageplaninformationen in der Ablaufverfolgung gesammelt und angezeigt werden. Darüber hinaus können Showplanereignisse aus anderen, in dieser Ablaufverfolgung gesammelten Ereignissen extrahiert und in einer separaten XML-Datei gespeichert werden.  
  
 Beim Extrahieren von Showplanereignissen aus der Ablaufverfolgung kann eine der folgenden Methoden angewendet werden:  
  
-   Beim Konfigurieren der Ablaufverfolgung über die Registerkarte **Ereignisextraktionseinstellungen** . Beachten Sie, dass diese Registerkarte nur angezeigt wird, wenn Sie auf der Registerkarte **Ereignisauswahl** ein Showplanereignis ausgewählt haben.  
  
-   Mithilfe der Option **SQL Server-Ereignisse extrahieren** im Menü **Datei**  
  
-   Extrahieren und Speichern einzelner Ereignisse, indem Sie mit der rechten Maustaste auf ein bestimmtes Ereignis klicken und **Ereignisdaten extrahieren** auswählen.  
  
## <a name="showplan-events"></a>Showplanereignisse  
 Die Showplan-Ablaufverfolgungsereignisse werden in der folgenden Tabelle aufgelistet und beschrieben.  
  
|Ereignisname|BESCHREIBUNG|  
|----------------|-----------------|  
|**Performance Statistics**|Gibt an, wann ein kompilierter Showplan zum ersten Mal zwischengespeichert wurde, wann er erneut kompiliert wurde und wann er aus dem Plancache entfernt wurde. Die **TextData** -Spalte enthält den Showplan im XML-Format. Weitere Informationen finden Sie unter [Performance Statistics-Ereignisklasse](../../relational-databases/event-classes/performance-statistics-event-class.md).|  
|**Showplan All**|Zeigt den Abfrageplan mit sämtlichen Kompilierungsdetails für die ausgeführte [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisung an. Beispielsweise können Kostenschätzungen und Spaltenlisten angezeigt werden. Weitere Informationen finden Sie unter [Showplan All Event Class](../../relational-databases/event-classes/showplan-all-event-class.md).|  
|**Showplan All For Query Compile**|Tritt auf, wenn eine Abfrage in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]kompiliert oder erneut kompiliert wird. Dies ist die Kompilierzeitentsprechung des **Showplan All** -Ereignisses. **Showplan All** tritt auf, wenn eine Abfrage ausgeführt wird. **Showplan All For Query Compile** tritt auf, wenn eine Abfrage kompiliert wird. Weitere Informationen finden Sie unter [Showplan All for Query Compile Event Class](../../relational-databases/event-classes/showplan-all-for-query-compile-event-class.md).|  
|**Showplan Statistics Profile**|Zeigt den Abfrageplan mit vollständigen Laufzeitdetails der ausgeführten [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisung an, einschließlich der tatsächlichen Anzahl von Zeilen, die an jeden Vorgang übergeben werden. Weitere Informationen finden Sie unter [Showplan Statistics Profile Event Class](../../relational-databases/event-classes/showplan-statistics-profile-event-class.md).|  
|**Showplan Text**|Zeigt die Abfrageplanstruktur der ausgeführten [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisung als binäre Daten an. Weitere Informationen finden Sie unter [Showplan Text Event Class](../../relational-databases/event-classes/showplan-text-event-class.md).|  
|**Showplan Text (Unencoded)**|Zeigt die Abfrageplanstruktur der ausgeführten [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisung als Text an. Diese Ereignisklasse zeigt die gleichen Informationen an wie Showplan Text, abgesehen davon, dass diese Ereignisklasse statt binärer Daten Text anzeigt. Weitere Informationen finden Sie unter [Showplan Text &#40;Unencoded&#41;-Ereignisklasse](../../relational-databases/event-classes/showplan-text-unencoded-event-class.md).|  
|**Showplan XML**|Zeigt den Abfrageplan mit allen während der Abfrageoptimierung gesammelten Daten an. Dieses Ereignis wird nur generiert, wenn ein Abfrageplan optimiert wird. Weitere Informationen finden Sie unter [Showplan XML Event Class](../../relational-databases/event-classes/showplan-xml-event-class.md).|  
|**Showplan XML For Query Compile**|Zeigt den Abfrageplan an, wenn die Abfrage kompiliert wird. Weitere Informationen finden Sie unter [Showplan XML for Query Compile Event Class](../../relational-databases/event-classes/showplan-xml-for-query-compile-event-class.md).|  
|**Showplan XML Statistics Profile**|Zeigt den Abfrageplan mit vollständigen Laufzeitdetails im XML-Format an. Diese Ereignisklasse zeichnet z. B. die Anzahl von Zeilen auf, die an jeden Operator der ausgeführten [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisung übergeben werden. Weitere Informationen finden Sie unter [Showplan XML Statistics Profile Event Class](../../relational-databases/event-classes/showplan-xml-statistics-profile-event-class.md).|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Leistung (Ereigniskategorie)](../../relational-databases/event-classes/performance-event-category.md)  
  
  
