---
title: Überwachen des Protokollversands (Transact-SQL) | Microsoft-Dokumentation
description: Erfahren Sie, welche Tabellen einen Verlauf mit Überwachungsinformationen und gespeicherten Prozeduren für die Überwachung des Protokollversands in SQL Server speichern.
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: log-shipping
ms.topic: conceptual
helpviewer_keywords:
- log shipping [SQL Server], status
- history tables [SQL Server]
- historical information [SQL Server], log shipping
- log shipping [SQL Server], monitoring
- alerts [SQL Server], log shipping
- status information [SQL Server], log shipping
- monitoring log shipping [SQL Server]
ms.assetid: acf3cd99-55f7-4287-8414-0892f830f423
author: cawrites
ms.author: chadam
ms.openlocfilehash: 389a5810611b4721fb78b3fd97f484e5ba7b1974
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346979"
---
# <a name="monitor-log-shipping-transact-sql"></a>Überwachen des Protokollversands (Transact-SQL)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Nach der Konfiguration des Protokollversands können Sie Informationen zum Status aller Protokollversandserver überwachen. Der Verlauf und der Status von Protokollversandvorgängen werden immer lokal durch die Protokollversandaufträge gespeichert. Der Verlauf und der Status des Sicherungsvorgangs werden auf dem primären Server gespeichert, und der Verlauf und der Status von Kopier- und Wiederherstellungsvorgängen werden auf dem sekundären Server gespeichert. Falls Sie einen Remoteüberwachungsserver implementiert haben, werden diese Informationen auch auf dem Überwachungsserver gespeichert.  
  
 Sie können Warnungen konfigurieren, die ausgelöst werden, falls Protokollversandvorgänge nicht wie geplant ausgeführt werden können. Fehler werden durch einen Warnungsauftrag ausgelöst, der den Status der Sicherungs- und Wiederherstellungsvorgänge überwacht. Sie können Warnungen definieren, mit denen ein Operator benachrichtigt wird, wenn diese Fehler ausgelöst werden. Falls ein Überwachungsserver konfiguriert ist, wird ein Warnungsauftrag auf dem Überwachungsserver ausgeführt, der Fehler für alle Vorgänge in der Protokollversandkonfiguration auslöst. Falls kein Überwachungsserver angegeben ist, wird ein Warnungsauftrag in der primären Serverinstanz ausgeführt, die den Sicherungsvorgang überwacht. Falls kein Überwachungsserver angegeben ist, wird außerdem ein Warnungsauftrag in jeder sekundären Serverinstanz ausgeführt, um die lokalen Kopier- und Wiederherstellungsvorgänge zu überwachen.  
  
> [!IMPORTANT]  
>  Zum Überwachen einer Protokollversandkonfiguration müssen Sie den Überwachungsserver hinzufügen, wenn Sie den Protokollversand aktivieren. Wenn Sie später einen Überwachungsserver hinzufügen, müssen Sie die Protokollversandkonfiguration entfernen und sie anschließend durch eine Konfiguration ersetzen, die einen Überwachungsserver enthält. Weitere Informationen finden Sie unter [Konfigurieren des Protokollversands &#40;SQL Server&#41;](../../database-engine/log-shipping/configure-log-shipping-sql-server.md)eingeführt. Nach dem Konfigurieren des Überwachungsservers kann dieser zudem nicht ohne vorhergehendes Entfernen des Protokollversands geändert werden.  
  
## <a name="history-tables-containing-monitoring-information"></a>Verlaufstabellen mit Überwachungsinformationen  
 Die Überwachungsverlaufstabellen enthalten Metadaten, die auf dem Überwachungsserver gespeichert sind. Eine Kopie der Informationen für einen bestimmten primären oder sekundären Server wird außerdem lokal gespeichert.  
  
 Sie können diese Tabellen abfragen, um den Status einer Protokollversandsitzung zu überwachen. Um z. B. den Status des Protokollversands abzurufen, überprüfen Sie den Status und den Verlauf des Sicherungsauftrags, des Kopierauftrags und des Wiederherstellungsauftrags. Sie können spezifische Details zum Protokollversandverlauf und zu Fehlern anzeigen, indem Sie die folgenden Überwachungstabellen abfragen.  
  
|Tabelle|BESCHREIBUNG|  
|-----------|-----------------|  
|[log_shipping_monitor_alert](../../relational-databases/system-tables/log-shipping-monitor-alert-transact-sql.md)|Speichert die Warnungsauftrags-ID.|  
|[log_shipping_monitor_error_detail](../../relational-databases/system-tables/log-shipping-monitor-error-detail-transact-sql.md)|Speichert Fehlerdetails für Protokollversandaufträge. Sie können diese Tabelle abfragen, um die Fehler für eine Agentsitzung anzuzeigen. Optional können Sie die Fehler nach dem Datum und der Uhrzeit der Protokollierung sortieren. Jeder Fehler wird als Abfolge von Ausnahmen protokolliert, und mehrere Fehler (Sequenzen) können pro Agentsitzung protokolliert werden.|  
|[log_shipping_monitor_history_detail](../../relational-databases/system-tables/log-shipping-monitor-history-detail-transact-sql.md)|Enthält Verlaufsdetails für Protokollversand-Agents. Sie können diese Tabelle abfragen, um die Verlaufsdetails für eine Agentsitzung anzuzeigen.|  
|[log_shipping_monitor_primary](../../relational-databases/system-tables/log-shipping-monitor-primary-transact-sql.md)|Speichert einen Überwachungsdatensatz für die primäre Datenbank in jeder Protokollversandkonfiguration, einschließlich Informationen zur letzten Sicherungsdatei und zur letzten wiederhergestellten Datei, die für die Überwachung hilfreich sind.|  
|[log_shipping_monitor_secondary](../../relational-databases/system-tables/log-shipping-monitor-secondary-transact-sql.md)|Speichert einen Überwachungsdatensatz für jede sekundäre Datenbank, einschließlich Informationen zur letzten Sicherungsdatei und zur letzten wiederhergestellten Datei, die für die Überwachung hilfreich sind.|  
  
## <a name="stored-procedures-for-monitoring-log-shipping"></a>Gespeicherte Prozeduren für die Überwachung des Protokollversands  
 Überwachungs- und Verlaufsinformationen werden in Tabellen in der **msdb**-Datenbank gespeichert, auf die mithilfe gespeicherter Prozeduren für den Protokollversand zugegriffen werden kann. Führen Sie diese gespeicherten Prozeduren auf den in der folgenden Tabelle angegebenen Servern aus.  
  
|Gespeicherte Prozedur|BESCHREIBUNG|Ausführen dieser Prozedur auf|  
|----------------------|-----------------|---------------------------|  
|[sp_help_log_shipping_monitor_primary](../../relational-databases/system-stored-procedures/sp-help-log-shipping-monitor-primary-transact-sql.md)|Gibt Überwachungsdatensätze für die angegebene primäre Datenbank aus der **log_shipping_monitor_primary** -Tabelle zurück.|Überwachungsserver oder primärer Server|  
|[sp_help_log_shipping_monitor_secondary](../../relational-databases/system-stored-procedures/sp-help-log-shipping-monitor-secondary-transact-sql.md)|Gibt Überwachungsdatensätze für die angegebene sekundäre Datenbank aus der **log_shipping_monitor_secondary** -Tabelle zurück.|Überwachungsserver oder sekundärer Server|  
|[sp_help_log_shipping_alert_job](../../relational-databases/system-stored-procedures/sp-help-log-shipping-alert-job-transact-sql.md)|Gibt die Auftrags-ID des Warnungsauftrags zurück.|Überwachungsserver oder primärer bzw. sekundärer Server, falls kein Überwachungsserver definiert ist.|  
|[sp_help_log_shipping_primary_database](../../relational-databases/system-stored-procedures/sp-help-log-shipping-primary-database-transact-sql.md)|Ruft Einstellungen der primären Datenbank ab und zeigt die Werte aus den Tabellen **log_shipping_primary_databases** und **log_shipping_monitor_primary** an.|Primärer Server|  
|[sp_help_log_shipping_primary_secondary](../../relational-databases/system-stored-procedures/sp-help-log-shipping-primary-secondary-transact-sql.md)|Ruft die Namen sekundärer Datenbanken für eine primäre Datenbank ab.|Primärer Server|  
|[sp_help_log_shipping_secondary_database](../../relational-databases/system-stored-procedures/sp-help-log-shipping-secondary-database-transact-sql.md)|Ruft Einstellungen der sekundären Datenbank aus den Tabellen **log_shipping_secondary**, **log_shipping_secondary_databases** und **log_shipping_monitor_secondary** ab.|Sekundärer Server|  
|[sp_help_log_shipping_secondary_primary &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-log-shipping-secondary-primary-transact-sql.md)|Diese gespeicherte Prozedur ruft die Einstellungen einer angegebenen primären Datenbank auf dem sekundären Server ab.|Sekundärer Server|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Anzeigen des Protokollversandberichts &#40;SQL Server Management Studio&#41;](../../database-engine/log-shipping/view-the-log-shipping-report-sql-server-management-studio.md)   
 [Gespeicherte Prozeduren und Tabellen für den Protokollversand](../../database-engine/log-shipping/log-shipping-tables-and-stored-procedures.md)  
  
  
