---
title: Überprüfen von in Azure bereitgestellten SSIS-Paketen | Microsoft-Dokumentation
description: Erfahren Sie, wie der Assistent für die Bereitstellung von SSIS-Paketen Pakete auf bekannte Probleme überprüft, die die ordnungsgemäße Ausführung der Pakete in Azure verhindern können.
ms.date: 11/27/2017
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: swinarko
ms.author: sawinark
ms.reviewer: maghan
ms.openlocfilehash: 3da89cbe8d0028a2b362e6358d1b30b3075e030c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345687"
---
# <a name="validate-sql-server-integration-services-ssis-packages-deployed-to-azure"></a>Überprüfen von in Azure bereitgestellten SSIS-Paketen

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



Wenn Sie auf einem Azure-Server ein SSIS-Projekt (SQL Server Integration Services) in der SSIS-Katalog (SSISDB) bereitstellen, fügt der Assistent für die Paketbereitstellung hinter der Seite **Überprüfen** einen zusätzlichen Überprüfungsschritt hinzu. In diesem Überprüfungsschritt werden die im Projekt enthaltenen Pakete auf bekannte Probleme hin überprüft, die möglicherweise eine erwartungsgemäße Ausführung in der Azure SSIS Integration Runtime verhindern. Anschließend zeigt der Assistent alle zutreffenden Warnungen auf der Seite **Überprüfen** an.

> [!IMPORTANT]
> Die in diesem Artikel beschriebene Überprüfung tritt ein, wenn Sie ein Projekt mit SQL Server Data Tools (SSDT), Version 17.4 oder höher, bereitstellen. Informationen zum Abrufen der neuesten Version von SSDT finden Sie unter [Herunterladen von SQL Server Data Tools (SSDT)](../../ssdt/download-sql-server-data-tools-ssdt.md).

Weitere Informationen zum Assistenten für die Paketbereitstellung finden sie unter [Bereitstellen von SQL Server Integration Services-Projekten und Paketen (SSIS)](../packages/deploy-integration-services-ssis-projects-and-packages.md).

## <a name="validate-connection-managers"></a>Überprüfen von Verbindungs-Managern

Der Assistent überprüft bestimmte Verbindungs-Manager auf die folgenden Probleme, die zu einem Verbindungsfehler führen können:
- **Windows-Authentifizierung**. Wenn eine Verbindung Windows-Authentifizierung verwendet, löst die Überprüfung eine Warnung aus. Für Windows-Authentifizierung sind zusätzliche Konfigurationsschritte erforderlich. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit Datenquellen und Dateifreigaben mit der Windows-Authentifizierung](/azure/data-factory/ssis-azure-connect-with-windows-auth).
- **Dateipfad**. Wenn eine Verbindungszeichenfolge einen hartcodierten lokalen Dateipfad wie `C:\\...` enthält, löst die Überprüfung eine Warnung aus. Bei Paketen, die einen absoluten Pfad enthalten, tritt möglicherweise ein Fehler auf.
- **UNC-Pfad**. Wenn eine Verbindungszeichenfolge einen UNC-Pfad enthält, löst die Überprüfung eine Warnung aus. Bei Paketen, die einen UNC-Pfad enthalten, tritt möglicherweise ein Fehler auf, normalerweise, weil für den Zugriff auf einen UNC-Pfad Windows-Authentifizierung erforderlich ist.
- **HostName**. Wenn eine Servereigenschaft den Hostnamen anstelle der IP-Adresse enthält, löst die Überprüfung eine Warnung aus. Bei Paketen, die einen Hostnamen enthalten, tritt möglicherweise ein Fehler auf, normalerweise, weil für das virtuelle Azure-Netzwerk die richtige DNS-Konfiguration erforderlich ist, damit die DNS-Namensauflösung unterstützt werden kann.
- **Anbieter oder Treiber**. Wenn ein Anbieter oder Treiber nicht unterstützt wird, löst die Überprüfung eine Warnung aus. Aktuell wird nur eine kleine Anzahl integrierter Anbieter und Treiber unterstützt.

Der Assistent führt die folgenden Überprüfungen für die in der Liste aufgeführten Verbindungs-Manager aus.

| Verbindungs-Manager | Windows-Authentifizierung | Dateipfad | UNC-Pfad | Hostname | Anbieter oder Treiber |
|--------------------|----------|-----------|-----|-----------|-------------------|
| ADO                | âœ“        |           |     | âœ“         | âœ“                 |
| AdoNet             | âœ“        |           |     | âœ“         | âœ“                 |
| Cache              |          | âœ“         | âœ“   |           |                   |
| Excel              |          | âœ“         | âœ“   |           |                   |
| Datei               |          | âœ“         | âœ“   |           |                   |
| FlatFile           |          | âœ“         | âœ“   |           |                   |
| FTP                |          |           |     | âœ“         |                   |
| MSOLAP100          |          |           |     | âœ“         | âœ“                 |
| MultiFile          |          | âœ“         | âœ“   |           |                   |
| MultiFlatFile      |          | âœ“         | âœ“   |           |                   |
| OData              | âœ“        |           |     | âœ“         |                   |
| ODBC               | âœ“        |           |     | âœ“         | âœ“                 |
| OLEDB              | âœ“        |           |     | âœ“         | âœ“                 |
| SMOServer          | âœ“        |           |     | âœ“         |                   |
| SMTP               | âœ“        |           |     | âœ“         |                   |
| SQLMOBILE          |          | âœ“         | âœ“   |           |                   |
| WMI                | âœ“        |           |     |           |                   |
|||||||

## <a name="validate-sources-and-destinations"></a>Überprüfen von Quellen und Zielen
Die folgenden Quellen und Ziele von Drittanbietern werden nicht unterstützt:

-   Oracle-Quelle und Ziel von Attunity
-   Teradata-Quelle und Ziel von Attunity
-   SAP BW-Quelle und -Ziel

## <a name="validate-tasks-and-components"></a>Überprüfen von Aufgaben und Komponenten

### <a name="execute-process-task"></a>Prozess ausführen (Task)

Die Überprüfung löst eine Warnung aus, wenn ein Befehl mit absolutem Pfad auf eine lokale Datei oder mit einem UNC-Pfad auf eine Datei verweist. Diese Pfade können bei der Ausführung in Azure zu Fehlern führen.

### <a name="script-task-and-script-component"></a>Skriptaufgabe und Skriptkomponente

Die Überprüfung löst eine Warnung aus, wenn ein Paket eine Skriptaufgabe oder eine Skriptkomponente enthält, die auf nicht unterstützte Assemblys verweist oder diese aufruft. Diese Verweise oder Aufrufe können zu Ausführungsfehlern führen.

### <a name="other-components"></a>Andere Komponenten

Das ORC-Format wird für das HDFS-Ziel und das Azure Data Lake Store-Ziel nicht unterstützt.

## <a name="next-steps"></a>Nächste Schritte
Informationen zum Planen der Paketausführung in Azure finden Sie unter [Planen der Ausführung eines SSIS-Pakets in Azure](ssis-azure-schedule-packages.md).