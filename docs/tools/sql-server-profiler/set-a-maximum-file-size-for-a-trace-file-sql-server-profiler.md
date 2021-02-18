---
title: Festlegen der maximalen Dateigröße für eine Ablaufverfolgungsdatei
titleSuffix: SQL Server Profiler
description: Erfahren Sie, wie Sie die Größe der Ablaufverfolgungsdatei im SQL Server Profiler begrenzen und angeben, ob für die Datei ein Rollover durchgeführt werden soll, wenn sie die maximale Größe erreicht.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: e86dc4ce-5aa3-4c0d-acb5-c9e8871ed963
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 094df80b40338af09e7c4d9084ec3715a91a61c5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345804"
---
# <a name="set-a-maximum-file-size-for-a-trace-file-sql-server-profiler"></a>Festlegen einer maximalen Dateigröße für eine Ablaufverfolgungsdatei (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Verwenden Sie die folgende Prozedur, um die maximale Dateigröße für eine Ablaufverfolgungsdatei festzulegen.  
  
### <a name="to-set-a-maximum-file-size-for-a-trace-file"></a>So legen Sie die maximale Dateigröße für eine Ablaufverfolgungsdatei fest  
  
1.  Klicken Sie im Menü **Datei** auf **Neue Ablaufverfolgung**, und stellen Sie dann eine Verbindung zu einer Instanz von Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]her.  
  
     Das Dialogfeld **Ablaufverfolgungseigenschaften** wird angezeigt.  
  
    > [!NOTE]  
    >  Bei der Auswahl von **Ablaufverfolgung sofort nach dem Herstellen der Verbindung starten** wird das Dialogfeld **Ablaufverfolgungseigenschaften** nicht angezeigt. Stattdessen beginnt die Ablaufverfolgung. Um diese Einstellung zu deaktivieren, klicken Sie im Menü **Extras** auf **Optionen**, und deaktivieren Sie das Kontrollkästchen **Ablaufverfolgung sofort nach dem Herstellen der Verbindung starten** .  
  
2.  Geben Sie im Feld **Ablaufverfolgungsname** einen Namen für die Ablaufverfolgung ein.  
  
3.  Geben Sie im Feld **Vorlagenname** eine Ablaufverfolgungsvorlage aus.  
  
4.  Wählen Sie **In Datei speichern** aus, und geben Sie dann eine Datei zum Speichern der Ablaufverfolgungsinformationen an.  
  
5.  Aktivieren Sie das Kontrollkästchen **Maximale Dateigröße festlegen** , und geben Sie die maximale Dateigröße für die Ablaufverfolgung an. Erreicht die Datei die maximale Größe, werden die Ablaufverfolgungsereignisse in dieser Datei nicht mehr aufgezeichnet. Folgende Situation tritt ein, wenn Sie die Option **Dateirollover aktivieren** auswählen (dies ist standardmäßig ausgewählt):  
  
     Die Dateirolloveroption führt dazu, dass [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die aktuelle Datei schließt und eine neue Datei erstellt, wenn die maximale Dateigröße erreicht ist. Die neue Datei hat den gleichen Namen wie die vorige, sie wird jedoch zur Angabe der Abfolge um eine ganze Zahl ergänzt. Wenn z. B. der Name der ursprünglichen Ablaufverfolgungsdatei filename_1.trc lautet, lautet der Name der folgenden Ablaufverfolgungsdatei filename_2.trc usw. Wird der einer neuen Rolloverdatei zugewiesene Name bereits von einer vorhandenen Datei verwendet, wird die vorhandene Datei überschrieben, es sei denn, sie ist schreibgeschützt. Die Dateirolloveroption ist standardmäßig aktiviert, wenn Sie Ablaufverfolgungsdaten in einer Datei speichern.  
  
     Bei aktivierter Dateirolloveroption wird die Ablaufverfolgung fortgesetzt, bis sie auf andere Weise beendet wird. Um die Ablaufverfolgung zu beenden, nachdem die Dateigrößenbeschränkung erreicht wurde, deaktivieren Sie die Dateirolloveroption.  
  
    > [!NOTE]  
    >  Durch das FAT32-Dateisystem wird die Größe von Dateien auf knapp 4 Gigabyte (GB) beschränkt. Wenn die Ablaufverfolgungsdatei diese Größe erreicht, schlägt die Ablaufverfolgung mit dem Fehler „Nicht genügend Speicherplatz“ fehl. Verwenden Sie zum Erstellen größerer Dateien das NTFS-Dateisystem.  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)  
  
  
