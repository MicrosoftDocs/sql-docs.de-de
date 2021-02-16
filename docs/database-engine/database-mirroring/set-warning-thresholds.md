---
title: Festlegen von Warnungsschwellenwerten | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie im Dialogfeld „Schwellenwerte für Warnung festlegen“ die Warnungsschwellenwerte für ausgewählte gespiegelte Datenbanken in SQL Server aktivieren und konfigurieren.
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
f1_keywords:
- sql13.swb.dbmmonitor.setwarningthreshold.f1
ms.assetid: 17f93147-e7d9-4092-b4c2-c11b38051171
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: c5433b64027311fbf98b8c62bd8d25d1fbc3fdfb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352255"
---
# <a name="set-warning-thresholds"></a>Schwellenwerte für Warnung festlegen
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Mithilfe dieses Dialogfelds können Sie einen oder mehrere Warnungsschwellenwerte für die in der Navigationsstruktur des Dialogfelds **Datenbankspiegelungs-Monitor** ausgewählte Datenbank aktivieren und konfigurieren.  
  
 Mit dem Dialogfeld wird versucht, eine Verbindung mit beiden Serverinstanzen herzustellen. Diese Verbindungen werden asynchron hergestellt. Das Dialogfeld zeigt den Verbindungsstatus von jedem Partner an. Wenn der Partner nicht verbunden ist, können Sie auf **Verbinden** klicken.  
  
 **So verwenden Sie SQL Server Management Studio zum Überwachen der Datenbankspiegelung**  
  
-   [Starten des Datenbankspiegelungs-Monitors &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)  
  
## <a name="options"></a>Tastatur  
 *Serverinstanz und zugehöriger Verbindungsstatus*  
 Name einer Partnerserverinstanz in der Form *SYSTEM* **\\** _INSTANZNAME_. Für eine Standardserverinstanz wird nur der Systemname angezeigt.  
  
 Dieses Feld zeigt auch an, ob der Monitor zum aktuellen Zeitpunkt mit der Serverinstanz verbunden ist. Folgende Werte sind für den Verbindungsstatus möglich:  
  
-   **Nicht verbunden mit** *Name_der_Serverinstanz*  
  
-   **Verbindung wird hergestellt** *Name_der_Serverinstanz*  
  
-   **Verbunden mit***Name_der_Serverinstanz*  
  
    > [!NOTE]  
    >  Wenn Sie kein Mitglied der festen Serverrolle **sysadmin** sind, lautet dieser Statuts **Verbunden mit** *Name_der_Serverinstanz* **(Begrenzte Berechtigungen)** .  
  
 Der Name jeder der Partnerserverinstanzen wird in einem separaten Feld für die *Serverinstanz und den zugehörigen Verbindungsstatus* angezeigt. Das oberste Feld listet den Prinzipalserver auf, wenn die Ausführung des Monitors gestartet wurde.  
  
 **Verbinden**/**Abbrechen**  
 Eine Schaltfläche **Verbinden**/**Abbrechen** ist jedem Feld für die *Serverinstanz und den zugehörigen Verbindungsstatus* zugeordnet. Der Status der Schaltfläche ist abhängig vom Verbindungsstatus:  
  
-   Wenn keine Verbindung mit der Serverinstanz besteht, lautet der Schaltflächentext **Verbinden**. Klicken Sie auf die Schaltfläche, um eine Verbindung mit der Serverinstanz herzustellen.  
  
-   Wenn gerade ein Verbindungsversuch ausgeführt wird, lautet der Schaltflächentext **Abbrechen**. Klicken Sie auf die Schaltfläche, um den Verbindungsversuch abzubrechen.  
  
-   Wenn der Server verbunden ist, lautet der Schaltflächentext **Verbunden**, und die Schaltfläche ist abgeblendet.  
  
 **Schwellenwerte**  
 Das Raster **Schwellenwerte** zeigt die Warnungseinstellungen für beide Serverinstanzen an.  
  
> [!NOTE]  
>  Wenn eine Serverinstanz nicht verbunden ist, werden die Spalten mit leeren Zellen und grauem Hintergrund angezeigt. Wenn die Verbindung geöffnet wird, zeigt das Raster automatisch den Inhalt der Instanz an.  
  
 Das Raster enthält die folgenden Spalten:  
  
 **Warnungen**  
 Listet die unterstützten Warnungen auf:  
  
|Warnung|BESCHREIBUNG|  
|-------------|-----------------|  
|**Warnhinweis anzeigen, wenn das nicht gesendete Protokoll den Schwellenwert überschreitet.**|Der Schwellenwert gibt die Anzahl der Kilobytes (KB) des nicht gesendeten Protokolls in der Sendewarteschlange auf dem Prinzipal an.|  
|**Warnhinweis anzeigen, wenn das nicht wiederhergestellte Protokoll den Schwellenwert überschreitet.**|Der Schwellenwert gibt die Anzahl an KB der Wiederholungswarteschlange auf der Spiegelserverinstanz an.|  
|**Warnhinweis anzeigen, wenn das Alter der ältesten, nicht gesendeten Transaktion den Schwellenwert überschreitet.**|Der Schwellenwert gibt die Anzahl der Transaktionsminuten an, die noch nicht aus der Sendewarteschlange an die Spiegelserverinstanz gesendet wurden. Dieser Wert hilft, das Datenverlustrisiko in Bezug auf die Zeit zu messen.|  
|**Warnhinweis anzeigen, wenn der Spiegelungscommitaufwand den Schwellenwert überschreitet.**|Der Schwellenwert gibt die Anzahl der Verzögerung pro Transaktion in Millisekunden an. Dieser Wert ist nur im Modus für hohe Sicherheit relevant. Hierbei handelt es sich um die Verzögerung, die entsteht, während die Prinzipalserverinstanz darauf wartet, dass die Spiegelserverinstanz den Transaktionsprotokolldatensatz in die Wiederholungswarteschlange schreibt.|  
  
 **Aktiviert für '** *\<server instance>* **'**  
 Ein leeres Kontrollkästchen gibt an, dass die Warnung zum jetzigen Zeitpunkt auf der Serverinstanz deaktiviert ist. Klicken Sie auf das zugehörige Kontrollkästchen, um eine Warnung zu aktivieren.  
  
 **Schwellenwert bei '** *\<server instance>* **'**  
 Legen Sie den Schwellenwert auf der linken Seite der Spalte fest, wenn eine Warnung aktiviert ist. Ein Ereignis tritt beim Erreichen des angegebenen Schwellenwerts auf, wenn die Statustabelle aktualisiert wird. Wenn Sie einen Schwellenwert nach dem Konfigurieren eines Werts deaktivieren, verbleibt der betreffende Wert in diesem Feld und wird verwendet, sobald Sie die Warnung wieder aktivieren.  
  
 Wenn eine Warnung nicht aktiviert ist, ist dieses Feld inaktiv.  
  
 **OK**  
 Wenn Sie auf **OK** klicken, wird das Dialogfeld geschlossen, und die aktuell angegebenen Warnungsschwellenwerte werden im Raster **Schwellenwerte** auf der Seite **Warnungen** angezeigt.  
  
## <a name="remarks"></a>Bemerkungen  
 Ein Schwellenwert gilt jeweils nur auf einem Partner. Es ist jedoch empfehlenswert, einen Schwellenwert für ein bestimmtes Ereignis auf beiden Partnern festzulegen, um sicherzustellen, dass die Warnung weiterhin angezeigt wird, wenn ein Failover der Datenbank erfolgt. Der geeignete Schwellenwert für jeden Partner ist abhängig von der Leistungsfähigkeit des Systems des betreffenden Partners.  
  
 Ein Ereignis wird nur dann in das Ereignisprotokoll für eine Leistung geschrieben, sofern der Wert seinen Schwellenwert erreicht oder überschreitet, wenn die Statustabelle aktualisiert wird. Wenn ein Spitzenwert den Schwellenwert vorübergehend zwischen zwei Statusupdates erreicht, wird dieser Spitzenwert nicht erkannt.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Starten des Datenbankspiegelungs-Monitors &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)   
 [Überwachen der Datenbankspiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)   
 [Starten des Assistenten zum Konfigurieren der Sicherheit für die Datenbankspiegelung &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-the-configuring-database-mirroring-security-wizard.md)  
  
  
