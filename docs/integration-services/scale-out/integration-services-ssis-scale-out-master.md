---
title: Scale Out-Master | Microsoft-Dokumentation
description: Erfahren Sie mehr über die Scale Out-Master-Komponente des SQL Server Integration Services (SSIS) Scale Out-Masterdiensts.
ms.custom: performance
ms.date: 01/19/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: haoqian
ms.author: haoqian
ms.openlocfilehash: 8f3b205acd32ba48b86ac797fd99fe4cef4fccf5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347407"
---
# <a name="integration-services-ssis-scale-out-master"></a>Scale Out-Master von Integration Services (SSIS)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



Der Scale Out-Master verwaltet das Scale Out-System über den SSISDB-Katalog und den Scale Out-Masterdienst. 

Im SSISDB-Katalog werden alle Informationen für Scale Out-Worker, Pakete und Ausführungen gespeichert. Der Katalog stellt die Schnittstelle bereit, um einen Scale Out-Worker zu aktivieren und Pakete in Scale Out auszuführen. Weitere Informationen finden Sie unter [Exemplarische Vorgehensweise: Einrichten von Integration Services Scale Out](walkthrough-set-up-integration-services-scale-out.md) und [Ausführen von Paketen in SSIS Scale Out (SQL Server Integration Services)](run-packages-in-integration-services-ssis-scale-out.md).

Der Scale Out-Masterdienst ist ein Windows-Dienst, über den die Kommunikation mit Scale Out-Workern erfolgt. Der Dienst gibt über HTTPS den Status von Paketausführungen mit Scale Out-Workern zurück und verarbeitet die Daten in SSISDB. 

## <a name="scale-out-views-and-stored-procedures-in-ssisdb"></a>Scale Out-Ansichten und gespeicherte Prozeduren in SSISDB

### <a name="views"></a>Sichten

- [[catalog].[master_properties]](../../integration-services/system-views/catalog-master-properties-ssisdb-database.md)
- [[catalog].[worker_agents]](../../integration-services/system-views/catalog-worker-agents-ssisdb-database.md)

### <a name="stored-procedures"></a>Gespeicherte Prozeduren

- Zum Verwalten von Scale Out-Workern:
    - [[catalog].[disable_worker_agent]](../../integration-services/system-stored-procedures/catalog-disable-worker-agent-ssisdb-database.md)
    - [[catalog].[enable_worker_agent]](../../integration-services/system-stored-procedures/catalog-enable-worker-agent-ssisdb-database.md)

- Zum Ausführen von Paketen in Scale Out:
    - [[catalog].[create_execution]](../../integration-services/system-stored-procedures/catalog-create-execution-ssisdb-database.md)
    - [[catalog].[add_execution_worker]](../../integration-services/system-stored-procedures/catalog-add-execution-worker-ssisdb-database.md)
    - [[catalog].[start_execution]](../../integration-services/system-stored-procedures/catalog-start-execution-ssisdb-database.md)

## <a name="configure-the-scale-out-master-service"></a>Konfigurieren des Scale Out-Masterdiensts

Konfigurieren Sie den Scale Out-Masterdienst mithilfe der `<drive>:\Program Files\Microsoft SQL Server\140\DTS\Binn\MasterSettings.config`-Datei. Der Dienst muss nach dem Aktualisieren der Konfigurationsdatei neu gestartet werden.


|Konfiguration  |BESCHREIBUNG  |Standardwert  |
|---------|---------|---------|
|PortNumber|Die Netzwerkportnummer, die zur Kommunikation mit einem Scale Out-Worker verwendet wird|8391|
|SSLCertThumbprint|Der Fingerabdruck des TLS/SSL-Zertifikats, das zum Schützen der Kommunikation mit einem Scale Out-Worker verwendet wird.|Der Fingerabdruck des bei der Installation des Scale Out-Masters angegebenen TLS/SSL-Zertifikats|
|SqlServerName|Der Name der [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)]-Instanz, die den SSISDB-Katalog enthält. Zum Beispiel: Servername\\Instanzname.|Der Name der SQL Server-Instanz, mit dem der Scale Out-Master installiert wurde|
|CleanupCompletedJobsIntervalInMs|Die Zeitspanne für das Bereinigen von abgeschlossenen Ausführungsaufträgen (in Millisekunden)|43200000|
|DealWithExpiredTasksIntervalInMs|Die Zeitspanne für das Verarbeiten von abgelaufenen Ausführungsaufträgen (in Millisekunden)|300000|
|MasterHeartbeatIntervalInMs|Das Intervall für den Takt des Scale Out-Masters (in Millisekunden) Diese Eigenschaft gibt das Intervall an, in dem der Scale Out-Master seinen Onlinestatus im SSISDB-Katalog aktualisiert.|30.000|
|SqlConnectionTimeoutInSecs|Das SQL-Verbindungstimeout in Sekunden, wenn eine Verbindung mit SSISDB hergestellt wird|15|
||||    

## <a name="view-the-scale-out-master-service-log"></a>Anzeigen des Protokolls des Scale Out-Masterdiensts

Die Protokolldatei für den Scale Out-Masterdienst befindet sich im Ordner `<drive>:\Users\[account]\AppData\Local\SSIS\ScaleOut\Master`. 

Der Parameter *[Konto]* bezieht sich auf das Konto, unter dem der Scale Out-Masterdienst ausgeführt wird. Standardmäßig lautet das Konto `SSISScaleOutMaster140`.

## <a name="next-steps"></a>Nächste Schritte

[Scale Out-Worker von Integration Services (SSIS)](integration-services-ssis-scale-out-worker.md)
