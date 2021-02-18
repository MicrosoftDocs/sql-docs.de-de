---
title: Speichern von Ablaufverfolgungsergebnissen in einer Datei
titleSuffix: SQL Server Profiler
description: Erfahren Sie, wie Sie aufgezeichnete Ereignisdaten in einer Ablaufverfolgungsdatei speichern, eine maximale Größe der Ablaufverfolgungsdatei angeben und die Dateirolloveroption in SQL Server Profiler aktivieren.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: ac528747-0c19-4f3d-96f5-44c762a4abed
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: b9cd6bc990ccd82ece2e7a9b9aabb9831fbc8f5b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345782"
---
# <a name="save-trace-results-to-a-file-sql-server-profiler"></a>Speichern von Ablaufverfolgungsergebnissen in einer Datei (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

In diesem Thema wird beschrieben, wie Ablaufverfolgungsergebnisse mithilfe von [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]in einer Datei gespeichert werden.  
  
### <a name="to-save-trace-results-to-a-file"></a>So speichern Sie Ablaufverfolgungsergebnisse in einer Datei  
  
1.  Klicken Sie im Menü **Datei** auf **Neue Ablaufverfolgung**, und stellen Sie dann eine Verbindung mit einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]her.  
  
     Das Dialogfeld **Ablaufverfolgungseigenschaften** wird angezeigt.  
  
    > [!NOTE]  
    >  Bei der Auswahl von **Ablaufverfolgung sofort nach dem Herstellen der Verbindung starten** wird das Dialogfeld **Ablaufverfolgungseigenschaften** nicht angezeigt. Stattdessen beginnt die Ablaufverfolgung. Um diese Einstellung zu deaktivieren, klicken Sie im Menü **Extras** auf **Optionen**, und deaktivieren Sie das Kontrollkästchen **Ablaufverfolgung sofort nach dem Herstellen der Verbindung starten** .  
  
2.  Geben Sie im Feld **Ablaufverfolgungsname** einen Namen für die Ablaufverfolgung ein.  
  
3.  Aktivieren Sie das Kontrollkästchen **In Datei speichern** .  
  
     Das Dialogfeld **Speichern unter** wird angezeigt.  
  
4.  Geben Sie im Dialogfeld **Speichern unter** den Pfad und den Dateinamen an. Klicken Sie auf **Speichern**.  
  
    > [!NOTE]  
    >  Stellen Sie sicher, dass der SQL Server-Dienst die entsprechenden Berechtigungen hat, um in eine Datei im angegebenen Verzeichnis zu schreiben.  
  
5.  Geben Sie im Dialogfeld **Ablaufverfolgungseigenschaften** die maximale Dateigröße in das Textfeld **Maximale Dateigröße festlegen (MB)** ein. Der Standardwert ist 5 MB.  
  
6.  Geben Sie optional die folgenden Optionen an:  
  
    -   Aktivieren Sie das Kontrollkästchen **Dateirollover aktivieren** , damit [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] neue Dateien für Ablaufverfolgungsdaten erstellt, wenn die maximale Dateigröße erreicht wird. Diese Option ist standardmäßig aktiviert.  
  
    -   Aktivieren Sie das Kontrollkästchen **Ablaufverfolgungsdaten von Serverprozessen** , um sicherzustellen, dass der Server jedes Ablaufverfolgungsereignis aufzeichnet.  
  
        > [!NOTE]  
        >  Wenn **Ablaufverfolgungsdaten von Serverprozessen** deaktiviert ist, zeichnet der Server keine Ereignisse auf, falls dadurch die Leistung erheblich beeinträchtigt wird.  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)  
  
  
