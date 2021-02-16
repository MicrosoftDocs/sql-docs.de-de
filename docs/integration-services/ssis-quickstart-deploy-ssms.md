---
title: Bereitstellen eines SSIS-Projekts mit SSMS
description: Bereitstellen eines SSIS-Projekts mit SQL Server Management Studio (SSMS)
ms.topic: quickstart
ms.prod: sql
ms.technology: integration-services
f1_keywords:
- sql13.swb.deploymentwizard.deploymentsettings.f1
author: chugugrace
ms.author: chugu
ms.reviewer: ''
ms.custom: ''
ms.date: 05/21/2018
ms.openlocfilehash: 3749daf45eb51cda3e37d2bd3d31105fcb23c1c2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354224"
---
# <a name="deploy-an-ssis-project-with-sql-server-management-studio-ssms"></a>Bereitstellen eines SSIS-Projekts mit SQL Server Management Studio (SSMS)

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


In diesem Schnellstart wird erläutert, wie Sie SQL Server Management Studio (SSMS) verwenden, um eine Verbindung mit der SSIS-Katalogdatenbank herzustellen, und wie Sie anschließend den Bereitstellungs-Assistent für Integration Services verwenden, um ein im SSIS-Katalog gespeichertes SSIS-Projekt bereitzustellen. 

SQL Server Management Studio ist eine integrierte Umgebung zum Verwalten jeder beliebigen SQL-Infrastruktur, von SQL Server bis hin zur SQL-Datenbank. Weitere Informationen zu SSMS finden Sie unter [SQL Server Management Studio (SSMS)](../ssms/sql-server-management-studio-ssms.md).

## <a name="prerequisites"></a>Voraussetzungen

Prüfen Sie, ob Sie über die neueste Version von SQL Server Management Studio verfügen, bevor Sie beginnen. Wie Sie SSMS herunterladen, erfahren Sie unter [Herunterladen von SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md).

Die in diesem Artikel beschriebene Überprüfung für die Bereitstellung in Azure SQL-Datenbank erfordert SQL Server Data Tools (SSDT), Version 17.4 oder höher. Informationen zum Abrufen der neuesten Version von SSDT finden Sie unter [Herunterladen von SQL Server Data Tools (SSDT)](../ssdt/download-sql-server-data-tools-ssdt.md).

Ein Azure SQL-Datenbank-Server überwacht Port 1433. Wenn Sie versuchen, eine Verbindung mit einem Azure SQL-Datenbank-Server innerhalb einer Unternehmensfirewall herzustellen, muss dieser Port in der Unternehmensfirewall geöffnet sein, damit Sie eine Verbindung herstellen können.

## <a name="supported-platforms"></a>Unterstützte Plattformen

Mithilfe der Informationen in diesem Schnellstart können Sie auf den folgenden Plattformen SSIS-Projekte bereitstellen:

-   SQL Server unter Windows

-   Azure SQL-Datenbank. Weitere Informationen zum Bereitstellen und Ausführen von Paketen in Azure finden Sie unter [Migration von SQL Server Integration Services-Workloads in die Cloud per Lift & Shift](lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md).

Dieser Schnellstart enthält keine Anleitung zum Bereitstellen von SSIS-Paketen in SQL Server unter Linux. Weitere Informationen zum Ausführen von Paketen finden Sie unter [Extrahieren, Transformieren und Laden von Daten unter Linux mit SSIS](../linux/sql-server-linux-migrate-ssis.md).

## <a name="for-azure-sql-database-get-the-connection-info"></a>Abrufen der Verbindungsinformationen für Azure SQL-Datenbank

Um das Paket in Azure SQL-Datenbank bereitzustellen, rufen Sie die Verbindungsinformationen ab, die für die Verbindungsherstellung mit der SSIS-Katalogdatenbank (SSISDB) benötigt werden. Sie benötigen den vollqualifizierten Servernamen und die Anmeldeinformationen für die folgenden Prozeduren.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Wählen Sie aus dem Menü auf der linken Seite **SQL-Datenbanken** aus, und klicken Sie auf der Seite **SQL-Datenbanken** auf die SSISDB-Datenbank. 
3. Überprüfen Sie auf der **Übersichtsseite** Ihrer Datenbank den vollqualifizierten Servernamen. Zeigen Sie auf den Servernamen, damit die Option **Klicken Sie zum Kopieren** angezeigt wird. 
4. Wenn Sie die Anmeldeinformationen für Ihren Azure SQL-Datenbank-Server vergessen, navigieren Sie zur Seite „SQL Datenbank-Server“, damit der Serveradministratorname angezeigt wird. Sie können das Kennwort falls erforderlich zurücksetzen.

## <a name="authentication-methods-for-deployment"></a>Authentifizierungsmethoden für die Bereitstellung

Wenn Sie mit dem Bereitstellungs-Assistenten eine Bereitstellung auf einem SQL-Server vornehmen, müssen Sie die Windows-Authentifizierung verwenden, nicht die SQL Server-Authentifizierung.

Wenn Sie eine Bereitstellung auf einem Azure SQL-Datenbank-Server vornehmen, müssen Sie die SQL Server-Authentifizierung oder die Azure Active Directory-Authentifizierung verwenden, nicht die Windows-Authentifizierung.

## <a name="connect-to-the-ssisdb-database"></a>Herstellen einer Verbindung mit SSISDB

Verwenden Sie SQL Server Management Studio, um eine Verbindung mit dem SSIS-Katalog einzurichten. 

1. Öffnen Sie SQL Server Management Studio.

2. Geben Sie im Dialogfeld **Mit Server verbinden** die folgenden Informationen ein:

   | Einstellung       | Vorgeschlagener Wert | Weitere Informationen | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Servertyp** | Datenbank-Engine | Dieser Wert ist erforderlich. |
   | **Servername** | Der vollqualifizierte Servername | Wenn Sie eine Verbindung mit einem Azure SQL-Datenbank-Server herstellen, ist der Name im Format `<server_name>.database.windows.net`. |
   | **Authentifizierung** | SQL Server-Authentifizierung | Mit der SQL Server-Authentifizierung können Sie eine Verbindung zu SQL Server oder Azure SQL-Datenbank herstellen. Weitere Informationen finden Sie in diesem Artikel unter [Authentifizierungsmethoden für die Bereitstellung](#authentication-methods-for-deployment). |
   | **Anmeldung** | Das Serveradministratorkonto | Dabei handelt es sich um das Konto, das Sie beim Erstellen des Servers angegeben haben. |
   | **Kennwort** | Das Kennwort für das Serveradministratorkonto | Dabei handelt es sich um das Kennwort, das Sie beim Erstellen des Servers angegeben haben. |

3. Klicken Sie auf **Verbinden**. Das Fenster „Objekt-Explorer“ wird in SSMS geöffnet. 

4. Erweitern Sie im Objekt-Explorer **Integration Services-Kataloge** und dann **SSISDB**, um die Objekte in der SSIS-Katalogdatenbank anzuzeigen.

## <a name="start-the-integration-services-deployment-wizard"></a>Starten des Bereitstellungs-Assistenten für Integration Services
1. Erweitern Sie im Objekt-Explorer den Knoten **Integration Services-Kataloge**, den Knoten **SSISDB** und dann einen Order.

2.  Wählen Sie den Knoten **Projekte** aus.

3.  Klicken Sie mit der rechten Maustaste auf den Knoten **Projekte**, und wählen Sie **Projekt bereitstellen** aus. Der Bereitstellungs-Assistent für Integration Services wird geöffnet. Sie können ein Projekt aus dem aktuellen Katalog oder dem Dateisystem bereitstellen.

## <a name="deploy-a-project-with-the-wizard"></a>Bereitstellen eines Projekts mit dem Assistenten
1. Lesen Sie auf der Seite **Einführung** des Assistenten die Einführung. Klicken Sie auf **Weiter**, um die Seite **Quelle auswählen** zu öffnen.

2. Wählen Sie auf der Seite **Quelle auswählen** das vorhandene SSIS-Projekt aus, das bereitgestellt werden soll.
    -   Um eine Projektbereitstellungsdatei bereitzustellen, die Sie über ein Projekt in einer Entwicklungsumgebung erstellt haben, wählen Sie **Projektbereitstellungsdatei** aus, und geben Sie den Pfad für die ISPAC-Datei ein.
    -   Um ein Projekt bereitzustellen, das bereits in einem SSIS-Katalog bereitgestellt wurde, wählen Sie **Integration Services-Katalog** aus, und geben Sie dann den Servernamen und den Pfad zum Projekt im Katalog ein.
    Klicken Sie auf **Weiter** , um die Seite **Ziel auswählen** zu sehen.
  
3.  Wählen Sie auf der Seite **Ziel auswählen** das Ziel für das Projekt aus.
    -   Geben Sie den vollqualifizierten Servernamen ein. Wenn es sich bei dem Zielserver um einen Azure SQL-Datenbank-Server handelt, liegt der Name im Format `<server_name>.database.windows.net` vor.
    -   Stellen Sie die Authentifizierungsinformationen bereit, und klicken Sie dann auf **Verbinden**. Weitere Informationen finden Sie in diesem Artikel unter [Authentifizierungsmethoden für die Bereitstellung](#authentication-methods-for-deployment).
    -   Klicken Sie dann auf **Durchsuchen**, um den Zielordner in SSISDB auszuwählen.
    -   Klicken Sie dann auf **Weiter**, um die Seite **Überprüfen** zu öffnen. (Die Schaltfläche **Weiter** ist nur nach der Auswahl von **Verbinden** aktiviert.)
  
4.  Überprüfen Sie auf der Seite **Überprüfen** die Einstellungen, die Sie ausgewählt haben.
    -   Sie können Ihre Auswahl ändern, indem Sie auf **Zurück** klicken oder indem Sie auf einen der Schritte im linken Bereich klicken.
    -   Klicken Sie auf **Bereitstellen** , um den Bereitstellungsprozess zu starten.

5.  Wenn Sie eine Bereitstellung auf einem Azure SQL-Datenbank-Server durchführen, wird die Seite **Überprüfen** geöffnet. Diese prüft die im Projekt enthaltenen Pakete auf bekannte Probleme, die möglicherweise eine erwartungsgemäße Ausführung in der Azure-SSIS Integration Runtime verhindern. Weitere Informationen finden Sie unter [Überprüfen von in Azure bereitgestellten SSIS-Paketen](lift-shift/ssis-azure-validate-packages.md).

6.  Nachdem der Bereitstellungsvorgang abgeschlossen ist, wird die Seite **Ergebnisse** geöffnet. Diese Seite zeigt an, ob die einzelnen Aktionen erfolgreich ausgeführt wurden oder ob Fehler aufgetreten sind.
    -   Ist die Aktion fehlerhaft, klicken Sie in der Spalte **Ergebnis** auf **Fehler**, um eine Erläuterung zum Fehler anzuzeigen.
    -   Klicken Sie auf **Bericht speichern...**, um die Ergebnisse in einer XML-Datei zu speichern.
    -   Klicken Sie auf **Schließen**, um den Assistenten zu beenden.

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über weitere Möglichkeiten, ein Paket bereitzustellen.
    - [Deploy an SSIS package with Transact-SQL (SSMS) (Bereitstellen eines SSIS-Pakets mit Transact-SQL [SSMS])](./ssis-quickstart-deploy-tsql-ssms.md)
    - [Deploy an SSIS package with Transact-SQL (VS Code) (Bereitstellen eines SSIS-Pakets mit Transact-SQL [VS Code])](ssis-quickstart-deploy-tsql-vscode.md)
    - [Deploy an SSIS package from the command prompt (Bereitstellen eines SSIS-Pakets von der Befehlszeile aus)](./ssis-quickstart-deploy-cmdline.md)
    - [Deploy an SSIS package with PowerShell (Bereitstellen eines SSIS-Pakets mit PowerShell)](ssis-quickstart-deploy-powershell.md)
    - [Deploy an SSIS package with C#](./ssis-quickstart-deploy-dotnet.md) (Bereitstellen eines SSIS-Pakets mit C#) 
- Führen Sie ein bereitgestelltes Paket aus. Für die Ausführung eines Pakets können Sie aus mehreren Tools und Sprachen auswählen. Weitere Informationen finden Sie in den folgenden Artikeln:
    - [Run an SSIS package with SSMS](./ssis-quickstart-run-ssms.md) (Ausführen eines SSIS-Pakets mit SSMS)
    - [Run an SSIS package with Transact-SQL (SSMS)](./ssis-quickstart-run-tsql-ssms.md) (Ausführen eines SSIS-Pakets mit Transact-SQL [SSMS])
    - [Run an SSIS package with Transact-SQL (VS Code)](ssis-quickstart-run-tsql-vscode.md) (Ausführen eines SSIS-Pakets mit Transact-SQL [VS Code])
    - [Run an SSIS package from the command prompt](./ssis-quickstart-run-cmdline.md) (Ausführen eines SSIS-Pakets über die Eingabeaufforderung)
    - [Run an SSIS package with PowerShell](ssis-quickstart-run-powershell.md) (Ausführen eines SSIS-Pakets mit PowerShell)
    - [Run an SSIS package with C#](./ssis-quickstart-run-dotnet.md) (Ausführen eines SSIS-Pakets mit C#)