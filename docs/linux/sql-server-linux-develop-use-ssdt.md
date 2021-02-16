---
title: Entwickeln und Bereitstellen von SQL Server-Datenbanken für Linux | Microsoft-Dokumentation
description: SQL Server Data Tools fügt der Visual Studio-Umgebung leistungsstarke Funktionen für die Verwaltung von Datenbanklebenszyklen und die Entwicklung in SQL Server für Linux hinzu.
author: VanMSFT
ms.author: vanto
ms.date: 03/17/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 1e924704-e07c-4a8b-b243-8c1dd8cff0d3
ms.openlocfilehash: a7add65dbc62679a6a1183c7a3f6998de7a18cb6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100273091"
---
# <a name="use-visual-studio-to-create-databases-for-sql-server-on-linux"></a>Verwenden von Visual Studio zum Erstellen von Datenbanken für SQL Server für Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

SQL Server Data Tools (SSDT) wandelt Visual Studio in eine leistungsstarke Umgebung für Entwicklung und Datenbanklebenszyklus-Verwaltung (Database Lifecycle Management, DLM) für SQL Server für Linux um. Sie können Ihre Datenbank in einem Projekt mit Quellcodeverwaltung entwickeln, erstellen, testen und veröffentlichen, genauso wie Sie Ihren Anwendungscode entwickeln.

## <a name="install-visual-studio-and-sql-server-data-tools"></a>Installieren von Visual Studio und SQL Server Data Tools

1. Wenn Sie Visual Studio noch nicht auf dem Windows-Computer installiert haben, [laden Sie Visual Studio herunter, und installieren Sie es](https://visualstudio.microsoft.com/downloads/). Wenn Sie nicht über eine Visual Studio-Lizenz verfügen, können Sie die Visual Studio Community-Edition als kostenlose, voll ausgestattete IDE für Studenten, Open Source und einzelne Entwickler nutzen.

2. Wählen Sie während der Installation von Visual Studio **Benutzerdefiniert** für die Option **Wählen Sie die Installationsart aus** aus. Klicken Sie auf **Weiter**.

3. Wählen Sie in der Featureauswahlliste **Microsoft SQL Server Data Tools**, **Git für Windows** und **GitHub-Erweiterung für Visual Studio** aus.

   <img src="./media/sql-server-linux-develop-use-ssdt/ssdt-setup.png" alt="ssdt setup" style="width: 400px;"/>

4. Fahren Sie fort, und schließen Sie die Installation von Visual Studio ab. Dies kann einige Minuten dauern.

## <a name="upgrade-sql-server-data-tools-to-ssdt-170-rc-release"></a>Upgrade von SQL Server Data Tools auf SSDT 17.0 RC-Release

SQL Server für Linux wird von SSDT Version 17.0 RC oder höher unterstützt.

* [Laden Sie SSDT 17.0 RC2 herunter, und installieren Sie sie](https://go.microsoft.com/fwlink/?linkid=837939).

## <a name="create-a-new-database-project-in-source-control"></a>Erstellen eines neuen Datenbankprojekts in der Quellcodeverwaltung

1. Starten Sie Visual Studio.

2. Wählen Sie **Team Explorer** im Menü **Ansicht** aus. 

3. Klicken Sie im Abschnitt **Lokales Git-Repository** auf der Seite **Verbinden** auf **Neu**.

   <img src="./media/sql-server-linux-develop-use-ssdt/git-repository.png" alt="Screenshot of the Local Git Repository section with the New option called out." style="width: 300px;"/>

4. Klicken Sie auf **Erstellen**. Nachdem das lokale Git-Repository erstellt wurde, doppelklicken Sie auf **SSDTRepo**.

5. Klicken Sie im Abschnitt **Projektmappen** auf **Neu**. Wählen Sie im Dialogfeld **Neues Projekt** unter dem Knoten **Andere Sprachen** die Option **SQL Server** aus.

   <img src="./media/sql-server-linux-develop-use-ssdt/new-project.png" alt="Screenshot of the Solutions section with the New option and SQL Server option called out." style="width: 480px;"/>

6. Geben Sie **TutorialDB** als Name ein, und klicken Sie auf **OK**, um ein neues Datenbankprojekt zu erstellen.

## <a name="create-a-new-table-in-the-database-project"></a>Erstellen einer neuen Tabelle im Datenbankprojekt

1. Wählen Sie **Projektmappen-Explorer** im Menü **Ansicht** aus.

2. Öffnen Sie das Datenbankprojektmenü, indem Sie im Projektmappen-Explorer mit der rechten Maustaste auf **TutorialDB** klicken.

3. Wählen Sie **Tabelle** unter **Hinzufügen** aus.

   <img src="./media/sql-server-linux-develop-use-ssdt/create-table.png" alt="create table" style="width: 480px;"/>

4. Fügen Sie mit dem Tabellen-Designer die zwei Spalten „Name“ `nvarchar(50)` und „Location“ `nvarchar(50)` wie in der Abbildung dargestellt hinzu. SSDT generiert das `CREATE TABLE`-Skript, wenn Sie die Spalten im Designer hinzufügen.

   <img src="./media/sql-server-linux-develop-use-ssdt/add-columns.png" alt="Screenshot of the table designer with the Name and Location values called out." style="width: 480px;"/>

5. Speichern Sie die Datei **Table1.sql**.

## <a name="build-and-validate-the-database"></a>Erstellen und Validieren der Datenbank

1. Öffnen Sie das Datenbankprojektmenü auf **TutorialDB**, und wählen Sie **Erstellen** aus. SSDT kompiliert SQL-Quellcodedateien in Ihrem Projekt und erstellt eine DACPAC-Datei (Data-tier Application Package, Datenschichtanwendungs-Paket). Diese kann zum Veröffentlichen einer Datenbank auf Ihrer SQL Server-Instanz unter Linux verwendet werden. 

   <img src="./media/sql-server-linux-develop-use-ssdt/build.png" alt="Screenshot showing the TutorialDB with the Build option called out." style="width: 400px;"/>

2. Überprüfen Sie in Visual Studio die Builderfolgsmeldung im Fenster **Ausgabe**. 

## <a name="publish-the-database-to-sql-server-instance-on-linux"></a>Veröffentlichen der Datenbank auf der SQL Server-Instanz unter Linux

1. Öffnen Sie das Datenbankprojektmenü auf **TutorialDB**, und wählen Sie **Veröffentlichen** aus.

2. Klicken Sie auf **Bearbeiten**, um Ihre SQL Server-Instanz unter Linux auszuwählen.

   <img src="./media/sql-server-linux-develop-use-ssdt/publish-dialog.png" alt="publish dialog" style="width: 480px;"/>

3. Geben Sie im Verbindungsdialogfeld die IP-Adresse oder den Hostnamen Ihrer SQL Server-Instanz unter Linux, Benutzername und Kennwort ein.

   <img src="./media/sql-server-linux-develop-use-ssdt/connection-dialog.png" alt="connection dialog" style="width: 400px;"/>

4. Klicken Sie im Veröffentlichungsdialogfeld auf die Schaltfläche **Veröffentlichen**.

5. Beachten Sie den Veröffentlichungsstatus im Fenster **Datentoolvorgänge**.

6. Klicken Sie auf **Ergebnisse anzeigen** oder **Skript anzeigen**, um Details zum Ergebnis der Datenbankveröffentlichung auf Ihrer Instanz von SQL Server für Linux anzuzeigen.

   <img src="./media/sql-server-linux-develop-use-ssdt/publish-result.png" alt="publish result" style="width: 480px;"/>

Sie haben erfolgreich eine neue Datenbank auf der SQL Server-Instanz für Linux erstellt und die Grundlagen der Entwicklung einer Datenbank mit einem Datenbankprojekt mit Quellcodeverwaltung gelernt.

## <a name="next-steps"></a>Nächste Schritte

Wenn T-SQL für Sie neu ist, finden Sie weitere Informationen unter [Tutorial: Schreiben von Transact-SQL-Anweisungen](../t-sql/tutorial-writing-transact-sql-statements.md).

Weitere Informationen zum Entwickeln von Datenbanken mit SQL Data Tools finden Sie in den folgenden Artikeln.

* [Herunterladen und Installieren von Visual Studio](https://www.visualstudio.com/downloads/)
* [Herunterladen und Installieren von SSDT](../ssdt/download-sql-server-data-tools-ssdt.md).
* [SQL Server Data Tools](/previous-versions/sql/sql-server-data-tools/hh272686(v=vs.103))
* [Tutorial: Schreiben von Transact-SQL-Anweisungen](../t-sql/tutorial-writing-transact-sql-statements.md)
* [Transact-SQL-Referenz (Datenbank-Engine)](../t-sql/language-reference.md)