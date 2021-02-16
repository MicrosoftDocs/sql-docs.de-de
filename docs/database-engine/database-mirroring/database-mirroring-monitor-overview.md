---
title: Datenbankspiegelungs-Monitor (Übersicht) | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie den Datenbankspiegelungs-Monitor zum Überwachen gespiegelter Datenbanken verwenden, um den Datenfluss in der Datenbankspiegelungssitzung und für die Problembehandlung zu überprüfen.
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
f1_keywords:
- sql13.swb.dbmmonitor.main.f1
helpviewer_keywords:
- Database Mirroring Monitor [SQL Server], interface
ms.assetid: 8ebbdcd6-565a-498f-b674-289c84b985eb
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 1bbe6002daa25cc29a91194abad022fce5c5ff92
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353124"
---
# <a name="database-mirroring-monitor-overview"></a>Datenbankspiegelungs-Monitor (Übersicht)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Wenn Sie über die richtigen Berechtigungen verfügen, können Sie den Datenbankspiegelungs-Monitor verwenden, um eine beliebige Teilmenge der gespiegelten Datenbanken auf einer Serverinstanz zu überwachen. Durch das Überwachen können Sie das Vorhandensein und die Qualität des Datenflusses in der Datenbank-Spiegelungssitzung überprüfen. Der Datenbankspiegelungs-Monitor ist auch bei der Problembehandlung der Ursachen für reduzierten Datenfluss hilfreich.  
  
 Sie können jede der gespiegelten Datenbanken für die Überwachung auf jedem der Failoverpartner einzeln registrieren. Wenn Sie eine Datenbank registrieren, werden die folgenden Informationen zur Datenbank vom Datenbankspiegelungs-Monitor zwischengespeichert:  
  
-   Datenbankname  
  
-   Die Namen der beiden Partnerserverinstanzen  
  
-   Die letzten bekannten Rollen jedes Partners (Prinzipal oder Spiegel)  
  
## <a name="permissions"></a>Berechtigungen  
 Um Datenbankspiegelung zu überwachen, müssen Sie entweder Mitglied der festen Serverrolle **sysadmin** oder der festen Datenbankrolle **dbm_monitor** in der **msdb** -Datenbank auf der Serverinstanz sein. Wenn Sie Mitglied von **sysadmin** oder **dbm_monitor** auf nur einer der Partnerserverinstanzen sind, kann der Monitor nur mit diesem Partner eine Verbindung herstellen. Der Monitor kann keine Informationen von dem anderen Partner abrufen.  
  
 Wenn Sie nur Mitglied von **dbm_monitor** auf einer Serverinstanz sind, haben Sie eingeschränkte Berechtigungen auf dieser Serverinstanz. Sie können nur die neueste Statuszeile anzeigen. Wenn Sie mithilfe von **dbm_monitor** -Berechtigungen eine Verbindung mit einer Serverinstanz herstellen, werden Sie vom Datenbankspiegelungs-Monitor informiert, dass Sie eingeschränkte Berechtigungen haben.  
  
> [!IMPORTANT]  
>  Die feste Datenbankrolle **dbm_monitor** wird in der **msdb** -Datenbank erstellt, wenn die erste Datenbank im Datenbankspiegelungs-Monitor registriert wird. Die neue **dbm_monitor** -Rolle hat keine Mitglieder, bis ein Systemadministrator der Rolle Benutzer zuweist.  
  
## <a name="navigation-tree"></a>Navigationsstruktur  
 Wenn Datenbanken für die Überwachung durch den Datenbankspiegelungs-Monitor registriert wurden, wird eine Liste registrierter Datenbanken in der Navigationsstruktur angezeigt. Die Struktur wird alle 30 Sekunden automatisch aktualisiert. Wählen Sie eine registrierte Datenbank aus, um ihren Status anzuzeigen. Weitere Informationen finden Sie weiter unten unter "Detailbereich".  
  
 Für jede registrierte Datenbank werden die folgenden Informationen angezeigt:  
  
 _<Database_name>_ **(** _\<Status>_ **,** _<PRINCIPAL_SERVER>_ **->** _<MIRROR_SERVER>_ **)**  
  
 *<Database_name>*  
 Der Name einer gespiegelten Datenbank, die für den Datenbankspiegelungs-Monitor registriert ist.  
  
 *\<Status>*  
 Im Folgenden sind die möglichen Statuswerte und die zugehörigen Symbole aufgeführt:  
  
|Symbol|Status|BESCHREIBUNG|  
|----------|------------|-----------------|  
|Warnungssymbol|**Unbekannt**|Der Monitor ist mit keinem der beiden Partner verbunden. Es sind nur die Informationen verfügbar, die vom Monitor zwischengespeichert wurden.|  
|Warnungssymbol|**Wird synchronisiert**|Der Inhalt der Spiegeldatenbank liegt zeitlich hinter dem Inhalt der Prinzipaldatenbank. Die Prinzipalserverinstanz sendet Protokolldatensätze an die Spiegelserverinstanz, die die Änderungen auf die Spiegeldatenbank anwendet, um ein Rollforward dafür auszuführen.<br /><br /> Beim Start einer Datenbank-Spiegelungssitzung befinden sich Spiegel- und Prinzipaldatenbank in diesem Status.|  
|Standard-Datenbankzylinder|**Synchronisiert**|Wenn der Spiegelserver den Stand des Prinzipalservers erreicht hat, wechselt der Datenbankstatus zu **Synchronisiert**. Die Datenbank behält diesen Status so lange bei, wie der Prinzipalserver Änderungen an den Spiegelserver sendet und der Spiegelserver Änderungen auf die Spiegeldatenbank anwendet.<br /><br /> Im Modus für hohe Sicherheit ist sowohl das automatische Failover als auch das manuelle Failover ohne Datenverlust möglich.<br /><br /> Im Modus für hohe Leistung ist immer ein gewisser Datenverlust möglich, sogar im Status **Synchronisiert** .|  
|Warnungssymbol|**Angehalten**|Die Prinzipaldatenbank ist verfügbar, sendet aber keine Protokolle an den Spiegelserver.|  
|Fehlersymbol|**Nicht verbunden**|Die Serverinstanz kann keine Verbindung mit ihrem Partner herstellen.|  
  
 *<PRINCIPAL_SERVER>*  
 Der Name des Partners, der derzeit die Prinzipalserverinstanz ist. Der Name weist folgendes Format auf:  
  
 *<SYSTEM_NAME>* [ **\\** _<instance_name>_ ]  
  
 Dabei ist *<SYSTEM_NAME>* der Name des Systems, auf dem sich die Serverinstanz befindet. Für eine nicht standardmäßige Serverinstanz wird auch der Instanzname angezeigt: _<SYSTEM_NAME>_ **\\** _<instance_name>_ .  
  
 *<MIRROR_SERVER>*  
 Der Name des Partners, der derzeit die Spiegelserverinstanz ist. Das Format ist identisch mit dem Format für den Prinzipalserver.  
  
## <a name="detail-pane"></a>Detailbereich  
 Die Darstellung des Monitors ist abhängig davon, ob eine Datenbank ausgewählt ist. Wenn Sie den Monitor öffnen, zeigt der Detailbereich den Link **Gespiegelte Datenbank registrieren** an. Klicken Sie darauf, um eine Datenbank zu registrieren. Registrierte Datenbanken werden unter dem Knoten **Datenbankspiegelungs-Monitor** in der Navigationsstruktur aufgelistet. Der Datenbankspiegelungs-Monitor versucht immer, eine Verbindung mit jeder Serverinstanz herzustellen, für die gespeicherte Anmeldeinformationen vorliegen.  
  
 Wenn Sie eine Datenbank auswählen, wird der Status auf der Seite im Registerformat **Status** im Detailbereich angezeigt. Der Inhalt dieser Seite stammt sowohl von der Prinzipal- als auch von der Spiegelserverinstanz. Die Seite wird mit den Statusinformationen, die über jeweils separate Verbindungen zur Prinzipalserver- und Spiegelserverinstanz gesammelt werden, asynchron gefüllt. Der Status wird automatisch in Intervallen von 30 Sekunden aktualisiert.  
  
> [!NOTE]  
>  Sie können die Aktualisierungsrate des Monitors nicht ändern, aber die Statustabelle vom Dialogfeld **Datenbankspiegelungsverlauf** aus aktualisieren.  
  
 Ein Systemadministrator kann die aktuelle Konfiguration der Warnungen für die Datenbank anzeigen, indem er die Seite im Registerformat **Warnungen** auswählt. Von dort aus kann der Administrator das Dialogfeld **Schwellenwerte für Warnung festlegen** öffnen, um einen oder mehrere Schwellenwerte für Warnungen zu aktivieren und zu konfigurieren.  
  
 Im Banner über den Registerkarten wird im Detailbereich der Zeitpunkt angezeigt, zu dem die Statusinformationen zuletzt vom Monitor aktualisiert wurden: **Letzte Aktualisierung:** _\<date>\<time>_ . Normalerweise ruft der Datenbankspiegelungs-Monitor Statusinformationen von der Prinzipal- und der Spiegelserverinstanz zu unterschiedlichen Zeiten ab. Die ältere dieser beiden Aktualisierungszeiten wird angezeigt.  
  
## <a name="action-menu"></a>Menü Aktion  
 Das Menü **Aktion** enthält immer die folgenden Befehle:  
  
|Get-Help|BESCHREIBUNG|  
|-------------|-----------------|  
|**Gespiegelte Datenbank registrieren...**|Öffnet das Dialogfeld **Gespiegelte Datenbank registrieren** . Verwenden Sie dieses Dialogfeld, um eine oder mehrere gespiegelte Datenbanken auf einer bestimmten Serverinstanz zu registrieren, indem Sie die Datenbank bzw. Datenbanken dem Datenbankspiegelungs-Monitor hinzufügen. Wenn eine Datenbank hinzugefügt wurde, werden Informationen zu der Datenbank, zu ihren Partnern und zum Herstellen von Verbindungen mit den Partnern vom Datenbankspiegelungs-Monitor lokal zwischengespeichert.|  
|**Serverinstanzverbindungen verwalten...**|Wenn Sie diesen Befehl auswählen, wird das Dialogfeld **Serververbindungen verwalten** geöffnet. In diesem Dialogfeld können Sie eine Serverinstanz auswählen, für die Sie Anmeldeinformationen angeben möchten, die der Monitor beim Herstellen einer Verbindung mit einem bestimmten Partner verwenden soll.<br /><br /> Suchen Sie den betreffenden Eintrag im Raster **Serverinstanzen** , und klicken Sie in dieser Zeile auf **Bearbeiten** , um die Anmeldeinformationen für einen Partner zu bearbeiten. Das Dialogfeld **Verbindung mit Server herstellen** wird mit festgelegtem Serverinstanznamen und mit auf den aktuellen zwischengespeicherten Wert initialisierten Steuerelementen für die Anmeldeinformationen angezeigt. Ändern Sie die Authentifizierungsinformationen nach Bedarf, und klicken Sie auf **Verbinden**. Wenn die Anmeldeinformationen über ausreichende Privilegien verfügen, wird die **Verbindung herstellen über** -Spalte mit den neuen Anmeldeinformationen aktualisiert.|  
  
 Wenn Sie eine Datenbank auswählen, enthält das Menü **Aktion** auch die folgenden Befehle.  
  
|Get-Help|BESCHREIBUNG|  
|-------------|-----------------|  
|**Registrierung für diese Datenbank aufheben**|Entfernt die ausgewählte Datenbank aus dem Datenbankspiegelungs-Monitor.|  
|**Schwellenwerte für Warnung festlegen...**|Öffnet das Dialogfeld **Schwellenwerte für Warnung festlegen** . In diesem Dialogfeld kann ein Systemadministrator Warnungen für die Datenbank auf jedem der Partner aktivieren oder deaktivieren und den Schwellenwert jeder Warnung ändern. Es ist empfehlenswert, einen Schwellenwert für eine bestimmte Warnung auf beiden Partnern festzulegen, um sicherzustellen, dass die Warnung weiterhin angezeigt wird, wenn ein Failover der Datenbank erfolgt. Der geeignete Schwellenwert für jeden Partner ist abhängig von der Leistungsfähigkeit des Systems des betreffenden Partners.<br /><br /> Ein Ereignis wird nur dann in das Ereignisprotokoll für eine Leistung geschrieben, sofern der Wert seinen Schwellenwert erreicht oder überschreitet, wenn die Statustabelle aktualisiert wird. Wenn ein Spitzenwert den Schwellenwert vorübergehend zwischen zwei Statusupdates erreicht, wird dieser Spitzenwert nicht erkannt.|  
  
 **So überwachen Sie die Datenbankspiegelung mithilfe von SQL Server Management Studio**  
  
-   [Starten des Datenbankspiegelungs-Monitors &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Überwachen der Datenbankspiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)   
 [Starten des Assistenten zum Konfigurieren der Sicherheit für die Datenbankspiegelung &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-the-configuring-database-mirroring-security-wizard.md)  
  
  
