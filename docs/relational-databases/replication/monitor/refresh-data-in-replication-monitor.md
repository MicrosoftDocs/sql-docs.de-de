---
description: Aktualisieren von Daten in Replikationsmonitor
title: Aktualisieren von Daten in Replikationsmonitor | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- refreshing data
ms.assetid: e9582244-7d00-45f4-be16-020a65c76a5e
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: adc29367fae4cd4e32a4cced3684f16771765c18
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97432828"
---
# <a name="refresh-data-in-replication-monitor"></a>Aktualisieren von Daten in Replikationsmonitor
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  Im Replikationsmonitor können das Hauptfenster und die Detailfenster (die Fenster, die vom Hauptfenster aus aufrufbar sind) sowohl automatisch als auch manuell aktualisiert werden. Zum manuellen Aktualisieren eines Fensters drücken Sie F5. Das Hauptfenster wird standardmäßig alle fünf Sekunden aktualisiert. Diese Zeiteinstellung kann aber für jeden Verleger individuell angepasst werden.  
  
 Die im Replikationsmonitor angezeigten Daten werden aus einem Zwischenspeicher abgerufen. Informationen zum Zusammenhang zwischen dem Zwischenspeicher und dem Aktualisieren des Replikationsmonitors finden Sie unter [Zwischenspeichern, Aktualisieren und Leistung des Replikationsmonitors](../../../relational-databases/replication/monitor/caching-refresh-and-replication-monitor-performance.md). Informationen zum Starten des Replikationsmonitors finden Sie unter [Starten des Replikationsmonitors](../../../relational-databases/replication/monitor/start-the-replication-monitor.md).  
  
### <a name="to-set-refresh-options-for-replication-monitor"></a>So legen Sie die Aktualisierungseinstellungen für den Replikationsmonitor fest
  
1.  Klicken Sie mit der rechten Maustaste im linken Bereich des Replikationsmonitors auf einen Verleger, und klicken Sie dann auf **Verlegereinstellungen**.  
  
2.  Legen Sie im Dialogfeld **Verlegereinstellungen** für die Optionen **Automatisch aktualisieren** und **Aktualisierungsrate** eine Einstellung fest. Die Einstellung für **Automatisch aktualisieren** wirkt sich auf das Hauptfenster im Replikationsmonitor aus. Die Einstellung **Aktualisierungsrate** hat darüber hinaus Auswirkungen auf die Detailfenster, für die eine automatische Aktualisierung festgelegt wird (Änderungen an dieser Einstellung gelten nur für die Detailfenster, die nach dieser Änderung geöffnet werden).  
  
3.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  

### <a name="to-specify-that-a-detail-window-should-automatically-refresh"></a>So geben Sie an, dass ein Detailfenster automatisch aktualisiert wird  
  
1.  Öffnen Sie das gewünschte Detailfenster im Replikationsmonitor. Beispiel:  
  
    1.  Erweitern Sie im linken Bereich eine Verlegergruppe, erweitern Sie einen Verleger, und klicken Sie dann auf eine Veröffentlichung.  
  
    2.  Klicken Sie auf die Registerkarte **Alle Abonnements** .  
  
    3.  Klicken Sie mit der rechten Maustaste auf ein Abonnement, und klicken Sie dann auf **Details anzeigen**.  
  
2.  Klicken Sie im Detailfenster **Abonnement \<SubscriptionName>** auf **Aktion** und anschließend auf **Automatisch aktualisieren**. Die Häufigkeit der Aktualisierung richtet sich nach der Einstellung **Aktualisierungsrate** im Dialogfeld **Verlegereinstellungen** .  
  
## <a name="see-also"></a>Weitere Informationen  
 [Überwachen der Replikation](../../../relational-databases/replication/monitor/monitoring-replication.md)  
  
  
