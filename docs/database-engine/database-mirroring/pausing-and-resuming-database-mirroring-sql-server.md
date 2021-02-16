---
title: Anhalten und Fortsetzen einer Datenbankspiegelung
description: Hier erfahren Sie, wie Sie eine SQL Server-Datenbankspiegelungssitzung anhalten und später fortsetzen, um den Sitzungszustand beizubehalten, während die Spiegelung angehalten ist.
ms.custom: seo-lt-2019
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- sessions [SQL Server], database mirroring
- resuming database mirroring
- database mirroring [SQL Server], pausing
- database mirroring [SQL Server], resuming
- pausing database mirroring
ms.assetid: c67802c6-ee8c-4cbd-a6d4-f7b80413a4ab
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: c565c8ca660c0616c061ba70f3c24d3a7d347191
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343735"
---
# <a name="pausing-and-resuming-database-mirroring-sql-server"></a>Anhalten und Fortsetzen der Datenbankspiegelung (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Der Datenbankbesitzer kann eine Datenbank-Spiegelungssitzung jederzeit anhalten und später fortsetzen. Durch Anhalten bleibt der Sitzungsstatus erhalten, während die Spiegelung unterbrochen wird. Bei Engpässen ist das Anhalten möglicherweise nützlich, um die Leistung auf dem Prinzipalserver zu verbessern.  
  
 Wenn eine Sitzung angehalten wird, bleibt die Prinzipaldatenbank weiterhin verfügbar. Durch das Anhalten wird der Status der Spiegelungssitzung auf SUSPENDED festgelegt, und die Spiegeldatenbank hält nicht mehr Schritt mit der Prinzipaldatenbank. Dadurch wird die Prinzipaldatenbank fehleranfällig ausgeführt.  
  
 Es empfiehlt sich, eine angehaltene Sitzung rasch fortzusetzen, weil das Transaktionsprotokoll nicht gekürzt werden kann, während eine Datenbank-Spiegelungssitzung angehalten ist. Wenn die Datenbank-Spiegelungssitzung zu lange angehalten wird, kann es somit sein, dass das Transaktionsprotokoll vollständig aufgefüllt wird und die Datenbank nicht mehr verfügbar ist. Eine Erläuterung dazu finden Sie unter "Auswirkung des Anhaltens und Fortsetzens auf die Protokollkürzung" weiter unten in diesem Thema.  
  
> [!IMPORTANT]  
>  Nach einem erzwungenen Dienst, wenn der ursprüngliche Prinzipalserver die Verbindung wiederherstellt, wird die Spiegelung angehalten. Das Fortsetzen der Spiegelung in dieser Situation könnte auf dem ursprünglichen Prinzipalserver zu Datenverlust führen. Weitere Informationen zum Verwalten des potenziellen Datenverlusts finden Sie unter [Database Mirroring Operating Modes](../../database-engine/database-mirroring/database-mirroring-operating-modes.md).  
  
 **In diesem Thema:**  
  
-   [Auswirkung des Anhaltens und Fortsetzens auf die Protokollkürzung](#EffectOnLogTrunc)  
  
-   [Vermeiden eines vollen Transaktionsprotokolls](#AvoidFullLog)  
  
-   [Verwandte Aufgaben](#RelatedTasks)  
  
##  <a name="how-pausing-and-resuming-affect-log-truncation"></a><a name="EffectOnLogTrunc"></a> Auswirkung des Anhaltens und Fortsetzens auf die Protokollkürzung  
 Wenn ein automatischer Prüfpunkt für eine Datenbank ausgeführt wird, wird normalerweise das zugehörige Transaktionsprotokoll nach der nächsten Protokollsicherung auf diesen Prüfpunkt gekürzt. Während eine Datenbank-Spiegelungssitzung angehalten ist, bleiben alle aktuellen Protokolldatensätze aktiv, weil der Prinzipalserver darauf wartet, sie an den Spiegelserver zu senden. Die ungesendeten Protokolldatensätze sammeln sich im Transaktionsprotokoll der Prinzipaldatenbank an, bis die Sitzung fortgesetzt wird und der Prinzipalserver die Protokolldatensätze an den Spiegelserver gesendet hat.  
  
 Wenn die Sitzung fortgesetzt wird, beginnt der Prinzipalserver sofort damit, die akkumulierten Protokolldatensätze an den Spiegelserver zu senden. Nachdem der Spiegelserver bestätigt hat, dass der dem ältesten automatischen Prüfpunkt entsprechende Protokolldatensatz in die Warteschlange gestellt wurde, kürzt der Prinzipalserver das Protokoll der Prinzipaldatenbank auf diesen Prüfpunkt. Der Spiegelserver kürzt die Wiederholungswarteschlange auf denselben Protokolldatensatz. Da dieser Prozess für jeden sukzessiven Prüfpunkt wiederholt wird, wird das Protokoll schrittweise, von Prüfpunkt zu Prüfpunkt, gekürzt.  
  
> [!NOTE]  
>  Weitere Informationen zu Prüfpunkten und der Protokollkürzung finden Sie unter [Datenbankprüfpunkte &#40;SQL Server&#41;](../../relational-databases/logs/database-checkpoints-sql-server.md).  
  
##  <a name="avoid-a-full-transaction-log"></a><a name="AvoidFullLog"></a> Vermeiden eines vollen Transaktionsprotokolls  
 Wenn das Protokoll voll ist (weil die maximale Größe erreicht wurde oder weil für die Serverinstanz der Speicherplatz nicht ausreicht), kann die Datenbank keine Updates mehr ausführen. Es gibt zwei Möglichkeiten, um dieses Problem zu vermeiden:  
  
-   Setzen Sie die Datenbank-Spiegelungssitzung fort, bevor das Protokoll voll ist, oder fügen Sie mehr Protokollspeicher hinzu. Durch das Fortsetzen der Datenbankspiegelung kann der Prinzipalserver das angesammelte aktive Protokoll an den Spiegelserver senden, und die Spiegeldatenbank erhält den Status SYNCHRONIZING. Der Spiegelserver kann dann das Protokoll auf den Datenträger schreiben und damit beginnen, es zu wiederholen.  
  
-   Beenden Sie die Datenbank-Spiegelungssitzung durch Entfernen der Spiegelung.  
  
     Im Gegensatz zum Anhalten einer Sitzung werden beim Entfernen der Spiegelung alle Informationen zur Spiegelungssitzung gelöscht. Jede Partnerserverinstanz behält eine eigene Kopie der Datenbank. Wenn die frühere Spiegelkopie wiederhergestellt wird, weicht sie von der früheren Prinzipalkopie ab und liegt um die Zeitspanne zurück, die seit dem Anhalten der Sitzung vergangen ist. Weitere Informationen finden Sie unter [Entfernen der Datenbankspiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/removing-database-mirroring-sql-server.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
 **So halten Sie eine Datenbankspiegelung an bzw. setzen sie fort**  
  
-   [Anhalten oder Fortsetzen einer Datenbank-Spiegelungssitzung &#40;SQL Server&#41;](../../database-engine/database-mirroring/pause-or-resume-a-database-mirroring-session-sql-server.md)  
  
 **So beenden Sie die Datenbankspiegelung**  
  
-   [Entfernen der Datenbankspiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/remove-database-mirroring-sql-server.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [Datenbankspiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [Entfernen der Datenbankspiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/removing-database-mirroring-sql-server.md)  
  
  
