---
title: Anzeigen und Lesen des Diagnoseprotokolls der Failoverclusterinstanz
description: Hier erfahren Sie, wie Sie das von einer SQL Server-Failoverclusterinstanz erzeugte laufende Diagnoseprotokoll anzeigen und lesen können.
ms.custom: seo-lt-2019
ms.date: 03/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: how-to
ms.assetid: 68074bd5-be9d-4487-a320-5b51ef8e2b2d
author: cawrites
ms.author: chadam
ms.openlocfilehash: 0957ffc760046bfd9e3e10d018f373d94950080a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346164"
---
# <a name="view-and-read-failover-cluster-instance-diagnostics-log"></a>Anzeigen und Lesen des Failoverclusterinstanz-Diagnoseprotokolls
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Alle kritischen Fehler und Warnungsereignisse für die Ressourcen-DLL von SQL Server werden in das Windows-Ereignisprotokoll geschrieben. Ein Ausführungsprotokoll mit für SQL Server spezifischen Diagnoseinformationen wird von der gespeicherten Systemprozedur [sp_server_diagnostics &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md) erfasst und in die Protokolldateien der SQL Server-Failoverclusterdiagnose (auch als *SQLDIAG*-Protokolle bezeichnet) geschrieben.  
  
-   **Vorbereitungen:**  [Empfehlungen](#Recommendations), [Sicherheit](#Security)  
  
-   **Anzeigen des Diagnoseprotokolls mit:**  [SQL Server Management Studio](#SSMSProcedure), [Transact-SQL](#TsqlProcedure)  
  
-   **Konfigurieren der Diagnoseprotokolleinstellungen mit:** [Transact-SQL](#TsqlConfigure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Empfehlungen  
 Die SQLDIAG-Protokolle werden standardmäßig in einem lokalen LOG-Ordner des Verzeichnisses der SQL Server-Instanz gespeichert, z. B. unter „C\Programme\Microsoft SQL Server\MSSQL13.\<InstanceName>\MSSQL\LOG“ für den besitzenden Knoten der Always On-Failoverclusterinstanz (FCI). Die Größe jeder SQLDIAG-Protokolldatei wird auf 100 MB begrenzt. Zehn dieser Protokolldateien werden auf dem Computer gespeichert, bevor sie für neue Protokolle wiederverwendet werden.  
  
 Die Protokolle verwenden das Dateiformat für erweiterte Ereignisse. Mit der Systemfunktion **sys.fn_xe_file_target_read_file** können Sie die Dateien lesen, die durch erweiterte Ereignisse erstellt wurden. Pro Zeile wird ein Ereignis im XML-Format zurückgegeben. Fragen Sie die Systemsicht ab, um die XML-Daten als Resultset zu analysieren. Weitere Informationen finden Sie unter [sys.fn_xe_file_target_read_file &#40;Transact-SQL&#41;](../../../relational-databases/system-functions/sys-fn-xe-file-target-read-file-transact-sql.md).  
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Die VIEW SERVER STATE-Berechtigung ist erforderlich, um **fn_xe_file_target_read_file** auszuführen.  
  
 Öffnen von SQL Server Management Studio als Administrator  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
 **So zeigen Sie die Diagnoseprotokolldateien an:**  
  
1.  Wählen Sie im Menü **Datei** die Option **Öffnen**, **Datei**, und wählen Sie die anzuzeigende Diagnoseprotokolldatei an.  
  
2.  Die Ereignisse werden als Zeilen im rechten Bereich angezeigt. Standardmäßig werden nur die Spalten **name** und **timestamp** angezeigt.  
  
     Dadurch wird außerdem das Menü **ExtendedEvents** aktiviert.  
  
3.  Rufen Sie zum Anzeigen weiterer Spalten das Menü **ExtendedEvents** auf, und wählen Sie **Spalten auswählen**.  
  
     Ein Dialogfeld mit den verfügbaren Spalten wird angezeigt, in dem Sie die anzuzeigenden Spalten auswählen können.  
  
4.  Sie können die Ereignisdaten mithilfe des Menüs **ExtendedEvents** und der Option **Filter** filtern und sortieren.  
  
##  <a name="view-diagnostic-log-files-with-transact-sql"></a><a name="TsqlProcedure"></a> Anzeigen von Diagnoseprotokolldateien mit Transact-SQL  
 **So zeigen Sie die Diagnoseprotokolldateien an:**  
  
 Um alle Protokollelemente in der SQLDIAG-Protokolldatei anzuzeigen, verwenden Sie die folgende Abfrage:  
  
```  
SELECT  
xml_data.value('(event/@name)[1]','varchar(max)') AS 'Name'  
,xml_data.value('(event/@package)[1]','varchar(max)') AS 'Package'  
,xml_data.value('(event/@timestamp)[1]','datetime') AS 'Time'  
,xml_data.value('(event/data[@name=''state'']/value)[1]','int') AS 'State'  
,xml_data.value('(event/data[@name=''state_desc'']/text)[1]','varchar(max)') AS 'State Description'  
,xml_data.value('(event/data[@name=''failure_condition_level'']/value)[1]','int') AS 'Failure Conditions'  
,xml_data.value('(event/data[@name=''node_name'']/value)[1]','varchar(max)') AS 'Node_Name'  
,xml_data.value('(event/data[@name=''instancename'']/value)[1]','varchar(max)') AS 'Instance Name'  
,xml_data.value('(event/data[@name=''creation time'']/value)[1]','datetime') AS 'Creation Time'  
,xml_data.value('(event/data[@name=''component'']/value)[1]','varchar(max)') AS 'Component'  
,xml_data.value('(event/data[@name=''data'']/value)[1]','varchar(max)') AS 'Data'  
,xml_data.value('(event/data[@name=''info'']/value)[1]','varchar(max)') AS 'Info'  
FROM  
 ( SELECT object_name AS 'event'  
  ,CONVERT(xml,event_data) AS 'xml_data'  
  FROM sys.fn_xe_file_target_read_file('C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Log\SQLNODE1_MSSQLSERVER_SQLDIAG_0_129936003752530000.xel',NULL,NULL,NULL)   
)   
AS XEventData  
ORDER BY Time;  
  
```  
  
> [!NOTE]  
>  Sie können die Ergebnisse nach bestimmten Komponenten oder Status filtern, indem Sie die WHERE-Klausel verwenden.  
  
##  <a name="configure-diagnostic-log-properties-with-transact-sql"></a><a name="TsqlConfigure"></a> Konfigurieren von Diagnoseprotokolleigenschaften mit Transact-SQL  
 **So konfigurieren Sie die Diagnoseprotokolleigenschaften:**  
  
> [!NOTE]  
>  Ein Beispiel für diese Prozedur finden Sie weiter unten in diesem Abschnitt unter [Beispiel (Transact-SQL)](#TsqlExample).  
  
 Mithilfe der DDL-Anweisung (Data Definition Language) **ALTER SERVER CONFIGURATION** können Sie die Protokollierung von Diagnosedaten, die von der Prozedur [sp_server_diagnostics &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md) erfasst wurden, starten bzw. beenden und SQLDIAG-Protokollkonfigurationsparameter festlegen, wie z.B. die Anzahl der Protokolldateirollover, die Protokolldateigröße und den Dateispeicherort. Einzelheiten zur Syntax finden Sie unter [Setting diagnostic log options](../../../t-sql/statements/alter-server-configuration-transact-sql.md#Diagnostic).  
  
###  <a name="examples-transact-sql"></a><a name="ConfigTsqlExample"></a> Beispiele (Transact-SQL)  
  
####  <a name="setting-diagnostic-log-options"></a><a name="TsqlExample"></a> Setting diagnostic log options  
 Die Beispiele in diesem Abschnitt veranschaulichen, wie die Werte für die Diagnoseprotokolloption festgelegt werden.  
  
##### <a name="a-starting-diagnostic-logging"></a>A. Starten der Diagnoseprotokollierung  
 Im folgenden Beispiel wird die Protokollierung von Diagnosedaten gestartet.  
  
```  
ALTER SERVER CONFIGURATION SET DIAGNOSTICS LOG ON;  
```  
  
##### <a name="b-stopping-diagnostic-logging"></a>B. Beenden der Diagnoseprotokollierung  
 Im folgenden Beispiel wird die Protokollierung von Diagnosedaten beendet.  
  
```  
ALTER SERVER CONFIGURATION SET DIAGNOSTICS LOG OFF;  
```  
  
##### <a name="c-specifying-the-location-of-the-diagnostic-logs"></a>C. Angeben des Speicherorts für die Diagnoseprotokolle  
 Im folgenden Beispiel wird der Speicherort für die Diagnoseprotokolle auf den angegebenen Dateipfad festgelegt.  
  
```  
ALTER SERVER CONFIGURATION  
SET DIAGNOSTICS LOG PATH = 'C:\logs';  
```  
  
##### <a name="d-specifying-the-maximum-size-of-each-diagnostic-log"></a>D: Angeben der maximalen Größe jedes Diagnoseprotokolls  
 Im folgenden Beispiel wird die maximale Größe jedes Diagnoseprotokolls auf 10 Megabytes festgelegt.  
  
```  
ALTER SERVER CONFIGURATION   
SET DIAGNOSTICS LOG MAX_SIZE = 10 MB;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Failover Policy for Failover Cluster Instances](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md)  
  
  
