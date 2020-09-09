---
description: sp_dbmmonitorhelpalert (Transact-SQL)
title: sp_dbmmonitorhelpalert (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_dbmmonitorhelpalert_TSQL
- sp_dbmmonitorhelpalert
dev_langs:
- TSQL
helpviewer_keywords:
- sp_dbmmonitorhelpalert
- database mirroring [SQL Server], monitoring
ms.assetid: 43911660-b4e4-4934-8c02-35221160aaec
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9483733ddcaca469ea162ff9dba884ef857a9493
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89548161"
---
# <a name="sp_dbmmonitorhelpalert-transact-sql"></a>sp_dbmmonitorhelpalert (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt Informationen zu Warnungsschwellenwerten zurück, die für eine oder mehrere der Schlüsselleistungsmetriken für die Überwachung der Datenbankspiegelung festgelegt wurden.  
 
  ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_dbmmonitorhelpalert database_name   
    [ , alert_id ]   
```  
  
## <a name="arguments"></a>Argumente  
 *database_name*  
 Gibt die Datenbank an.  
  
 [ *alert_id* ]  
 Ein ganzzahliger Wert, der die zurückzugebende Warnung identifiziert. Wird das Argument nicht angegeben, werden alle Warnungen zurückgegeben, aber nicht die Beibehaltungsdauer.  
  
 Geben Sie einen der folgenden Werte an, um eine bestimmte Warnung zurückzugeben:  
  
|Wert|Leistungsmetrik|Schwellenwert für Warnung|  
|-----------|------------------------|-----------------------|  
|1|Älteste, nicht gesendete Transaktion|Gibt die Menge an Transaktionen (in Anzahl Minuten) an, die sich in der Sendewarteschlange ansammeln dürfen, bevor auf der Prinzipalserverinstanz eine Warnung generiert wird. Diese Warnung bietet die Möglichkeit, die Wahrscheinlichkeit eines Datenverlusts im Hinblick auf die Zeit zu messen. Sie ist besonders relevant für den Modus für hohe Leistung. Die Warnung ist aber auch für den Modus für hohe Sicherheit relevant, wenn die Spiegelung angehalten oder unterbrochen wird, weil die Verbindung zwischen den Partnern getrennt wurde.|  
|2|Nicht gesendetes Protokoll|Gibt an, bei welcher Menge (in KB) an nicht gesendeten Protokolldaten eine Warnung auf der Prinzipalserverinstanz generiert wird. Diese Warnung bietet die Möglichkeit, die Wahrscheinlichkeit eines Datenverlusts in KB zu messen. Sie ist besonders relevant für den Modus für hohe Leistung. Die Warnung ist aber auch für den Modus für hohe Sicherheit relevant, wenn die Spiegelung angehalten oder unterbrochen wird, weil die Verbindung zwischen den Partnern getrennt wurde.|  
|3|Nicht wiederhergestelltes Protokoll|Gibt an, bei welcher Menge (in KB) an nicht wiederhergestellten Protokolldaten eine Warnung auf der Spiegelserverinstanz generiert wird. Diese Warnung ermöglicht die Messung der Failoverzeit. Die*Failoverzeit* besteht hauptsächlich aus der Zeit, die der frühere Spiegelserver benötigt, um ein Rollforward für die Protokolldaten auszuführen, die sich noch in seiner Wiederholungswarteschlange befinden, sowie einer zusätzlichen kurzen Zeitspanne.|  
|4|Spiegelungscommitaufwand|Gibt die durchschnittliche Verzögerung (in Anzahl der Millisekunden) pro Transaktion an, die toleriert wird, bevor auf dem Prinzipalserver eine Warnung generiert wird. Hierbei handelt es sich um die Verzögerung, die entsteht, während die Prinzipalserverinstanz darauf wartet, dass die Spiegelserverinstanz den Transaktionsprotokolldatensatz in die Wiederholungswarteschlange schreibt. Dieser Wert ist nur im Modus für hohe Sicherheit relevant.|  
|5|Beibehaltungsdauer|Metadaten, die steuern, wie lange Zeilen in der Datenbankspiegelungs-Statustabelle beibehalten werden.|  
  
 Informationen zu den Ereignis-IDs, die den Warnungen entsprechen, finden Sie unter [Verwenden von Warnungs Schwellenwerten und Warnungen für Spiegelungsleistungsmetriken &#40;SQL Server&#41;](../../database-engine/database-mirroring/use-warning-thresholds-and-alerts-on-mirroring-performance-metrics-sql-server.md).  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 Keine  
  
## <a name="result-sets"></a>Resultsets  
 Gibt für jede zurückgegebene Warnung eine Zeile mit folgenden Spalten zurück:  
  
|Column|Datentyp|BESCHREIBUNG|  
|------------|---------------|-----------------|  
|**alert_id**|**int**|In der folgenden Tabelle sind die **alert_id** Werte für jede Leistungs Metrik und die Maßeinheit der Metrik aufgelistet, die im **sp_dbmmonitorresults** Resultset angezeigt wird:|  
|**threshold**|**int**|Der Schwellenwert für die Warnung. Wenn der Rückgabewert beim Aktualisieren des Spiegelungsstatus diesen Schwellenwert überschreitet, wird ein Eintrag im Windows-Ereignisprotokoll generiert. Der Wert stellt je nach Warnung KB, Minuten oder Millisekunden dar. Ist der Schwellenwert aktuell nicht festgelegt, lautet der Wert NULL.<br /><br /> **Hinweis:** Führen Sie die gespeicherte Prozedur [sp_dbmmonitorresults](../../relational-databases/system-stored-procedures/sp-dbmmonitorresults-transact-sql.md) aus, um die aktuellen Werte anzuzeigen.|  
|**enabled**|**bit**|0 = Ereignis ist deaktiviert.<br /><br /> 1 = Ereignis ist aktiviert.<br /><br /> **Hinweis:** Die Beibehaltungs Dauer ist immer aktiviert.|  
  
|Wert|Leistungsmetrik|Einheit|  
|-----------|------------------------|----------|  
|1|Älteste, nicht gesendete Transaktion|Minuten|  
|2|Nicht gesendetes Protokoll|KB|  
|3|Nicht wiederhergestelltes Protokoll|KB|  
|4|Spiegelungscommitaufwand|Millisekunden|  
|5|Beibehaltungsdauer|Stunden|  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die Mitgliedschaft in der festen Serverrolle **sysadmin** .  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird eine Zeile zurückgegeben, die angibt, ob eine Warnung für die Leistungsmetrik der ältesten, nicht gesendeten Transaktion für die [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank aktiviert ist.  
  
```  
EXEC sp_dbmmonitorhelpalert AdventureWorks2012, 1 ;  
```  
  
 Im folgenden Beispiel wird für jede Leistungsmetrik eine Zeile zurückgegeben, die angibt, ob die Metrik für die [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank aktiviert ist.  
  
```  
EXEC sp_dbmmonitorhelpalert AdventureWorks2012;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Überwachen der Datenbankspiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)   
 [sp_dbmmonitorchangealert &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangealert-transact-sql.md)   
 [sp_dbmmonitorchangemonitoring &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangemonitoring-transact-sql.md)   
 [sp_dbmmonitordropalert &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitordropalert-transact-sql.md)   
 [sp_dbmmonitorupdate &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorupdate-transact-sql.md)   
 [sp_dbmmonitorhelpmonitoring &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorhelpmonitoring-transact-sql.md)   
 [sp_dbmmonitorresults &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorresults-transact-sql.md)  
  
  
