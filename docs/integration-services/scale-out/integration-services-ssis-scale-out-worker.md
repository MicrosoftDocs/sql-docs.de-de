---
title: SSIS Scale Out-Worker (SQL Server Integration Services) | Microsoft-Dokumentation
description: Erfahren Sie mehr über die Scale Out-Master-Komponente von SQL Server Integration Services (SSIS) Scale Out.
ms.custom: performance
ms.date: 01/19/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: haoqian
ms.author: haoqian
ms.openlocfilehash: 35131a210e54557d63cb3e1986bddfd9d75e35ab
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338374"
---
# <a name="integration-services-ssis-scale-out-worker"></a>Integration Services (SSIS) Scale Out-Worker

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



Der Scale Out-Worker führt den Scale Out-Workerdienst aus, um Ausführungsaufgaben aus dem Scale Out-Master zu entfernen. Daraufhin führt der Worker die Pakete lokal mit `ISServerExec.exe` aus.

## <a name="configure-the-scale-out-worker-service"></a>Konfigurieren des Scale Out-Workerdiensts
Sie können den Scale Out-Workerdienst mithilfe der `\<drive\>:\Program Files\Microsoft SQL Server\140\DTS\Binn\WorkerSettings.config`-Datei konfigurieren. Der Dienst muss nach dem Aktualisieren der Konfigurationsdatei neu gestartet werden.

|Konfiguration  |BESCHREIBUNG  |Standardwert|
|---------|---------|---------|
|DisplayName|Der Anzeigename des Scale Out-Workers. **Wird NICHT in [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] 2017 verwendet.**|Computername|
|BESCHREIBUNG|Die Beschreibung des Scale Out-Workers. **Wird NICHT in [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] 2017 verwendet.**|Leer|
|MasterEndpoint|Der Endpunkt zum Herstellen einer Verbindung mit dem Scale Out-Master|Der Endpunkt, der während der Installation des Scale Out-Workers festgelegt wurde|
|MasterHttpsCertThumbprint|Der Fingerabdruck des Client-TLS/SSL-Zertifikats, mit dem der Scale Out-Master authentifiziert wird|Der Fingerabdruck des Clientzertifikats, das bei der Installation des Scale Out-Workers angegeben wurde|
|WorkerHttpsCertThumbprint|Der Fingerabdruck des Zertifikats, das für den Scale Out-Master verwendet wird, um den Scale Out-Worker zu authentifizieren|Der Fingerabdruck eines Zertifikats, das bei der Installation des Scale Out-Workers automatisch erstellt und installiert wurde|
|StoreLocation|Der Speicherort des Workerzertifikats|LocalMachine|
|StoreName|Der Name des Speichers, in dem sich das Workerzertifikat befindet|My|
|AgentHeartbeatInterval|Das Zeitintervall für den Takt für Scale Out-Worker|00:01:00|
|TaskHeartbeatInterval|Das Zeitintervall für den Status des Berichtstasks für Scale Out-Worker|00:00:10|
|HeartbeatErrorTolerance|Nach diesem Zeitraum ab dem letzten erfolgreichen Tasktakt wird der Task beendet, wenn eine Fehlerantwort des Takts empfangen wird.|00:10:00|
|TaskRequestMaxCPU|Die Obergrenze bezüglich CPU für Scale Out-Worker, um Tasks anzufordern.|70,0|
|TaskRequestMinMemory|Die Mindestmenge von Arbeitsspeicher in MB für Scale Out-Worker, um Tasks anzufordern.|100.0|
|MaxTaskCount|Die maximale Anzahl von Tasks, die der Scale Out-Worker aufnehmen kann|10|
|LeaseInterval|Das Leaseintervall einer Taskaufbewahrung durch den Scale Out-Worker|00:01:00|
|TasksRootFolder|Der Ordner für die Taskprotokolle. Wenn der Wert leer ist, wird der Ordnerpfad `\<drive\>:\Users\[account]\AppData\Local\SSIS\Cluster\Tasks` verwendet. [Konto] ist das Konto, unter dem der Dienst für Scale Out-Worker ausgeführt wird. Standardmäßig ist dies das Konto SSISScaleOutWorker140.|Leer|
|TaskLogLevel|Die Taskprotokollebene für den Scale Out-Worker. (Ausführlich 0x01, Informationen 0x02, Warnung 0x04, Fehler 0x08, Status 0x10, schwerwiegender Fehler 0x20, Überwachung 0x40)|126 (Informationen, Warnung, Fehler, Status, schwerwiegender Fehler, Überwachung)|
|TaskLogSegment|Die Zeitspanne einer Taskprotokolldatei|00:00:00|
|TaskLogEnabled|Gibt an, ob das Taskprotokoll aktiviert ist.|true|
|ExecutionLogCacheFolder|Der Ordner, in dem das Paketausführungsprotokoll zwischengespeichert wird. Wenn der Wert leer ist, wird der Ordnerpfad `\<drive\>:\Users\[account]\AppData\Local\SSIS\Cluster\Agent\ELogCache` verwendet. [Konto] ist das Konto, unter dem der Dienst für Scale Out-Worker ausgeführt wird. Standardmäßig ist dies das Konto SSISScaleOutWorker140.|Leer|
|ExecutionLogMaxBufferLogCount|Die maximale Anzahl von Ausführungsprotokollen, die in einem Ausführungsprotokollpuffer im Arbeitsspeicher zwischengespeichert werden|10000|
|ExecutionLogMaxInMemoryBufferCount|Die maximale Anzahl von Ausführungsprotokollpuffern im Arbeitsspeicher für Ausführungsprotokolle|10|
|ExecutionLogRetryCount|Die Anzahl von Wiederholungsversuchen, wenn bei der Ausführungsprotokollierung ein Fehler auftritt|3|
|ExecutionLogRetryTimeout|Die Anzahl von Wiederholungsversuchen, wenn bei der Ausführungsprotokollierung ein Fehler auftritt. i\ Wenn ExecutionLogRetryCount erreicht wird, wird ExecutionLogRetryTimeout ignoriert. |7.00:00:00 (7 Tage)|
|AgentId|Worker-Agent-ID des Scale Out-Workers|Wird automatisch generiert|
||||    

## <a name="view-the-scale-out-worker-log"></a>Anzeigen des Protokolls des Scale Out-Workers
Die Protokolldatei des Scale Out-Workerdiensts befindet sich im Ordner `\<drive\>:\Users\\[account]\AppData\Local\SSIS\ScaleOut\Agent`.

Der Speicherort jeder einzelnen Aufgabe wird in der `WorkerSettings.config`-Datei im Ordner `TasksRootFolder` konfiguriert. Wenn kein Wert angegeben ist, befindet sich das Protokoll im Ordner `\<drive\>:\Users\\[account]\AppData\Local\SSIS\ScaleOut\Tasks`. 

Bei dem Parameter *[Konto]* handelt es sich um das Konto, unter dem der Scale Out-Workerdienst ausgeführt wird. Standardmäßig lautet das Konto `SSISScaleOutWorker140`.

## <a name="next-steps"></a>Nächste Schritte
[Scale Out-Master von Integration Services (SSIS)](integration-services-ssis-scale-out-master.md)
