---
title: Verwenden der mssql-Erweiterung für Visual Studio Code
description: Verwenden Sie die Visual Studio Code-Erweiterung „mssql“, um Transact-SQL-Skripts für SQL Server für Linux zu bearbeiten und auszuführen.
ms.topic: conceptual
ms.prod: sql
ms.technology: tools-other
ms.assetid: 9766ee75-32d3-4045-82a6-4c7968bdbaa6
author: markingmyname
ms.author: maghan
ms.date: 10/28/2019
ms.openlocfilehash: fdb3d5e695824117def89347da883a0a7a5d7a9c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340544"
---
# <a name="use-visual-studio-code-to-create-and-run-transact-sql-scripts"></a>Verwenden von Visual Studio Code zum Erstellen und Ausführen von Transact-SQL-Skripts

[!INCLUDE [SQL Server - Linux](../../includes/applies-to-version/sql-linux.md)]

In diesem Artikel erfahren Sie, wie Sie die **mssql**-Erweiterung für Visual Studio Code verwenden, um SQL Server-Datenbanken zu entwickeln. Da Visual Studio Code plattformübergreifend ist, können Sie **mssql**-Erweiterung unter Linux, macOS und Windows verwenden.

## <a name="install-and-start-visual-studio-code"></a>Installieren und Starten von Visual Studio Code

Visual Studio Code ist ein plattformübergreifender, grafischer Code-Editor, der Erweiterungen unterstützt.

1. [Laden Sie Visual Studio Code auf Ihrem Computer herunter, und führen Sie die Installation durch.](https://code.visualstudio.com/)

2. Starten Sie Visual Studio Code.

    >[!NOTE]
    >Wenn Visual Studio Code nicht gestartet wird, wenn Sie über eine XRDP-Remotedesktopsitzung verbunden sind, finden Sie weitere Informationen unter [VS Code not working on Ubuntu when connected using XRDP (VS Code funktioniert unter Ubuntu bei bestehender XRDP-Verbindung nicht)](https://github.com/Microsoft/vscode/issues/3451).

## <a name="install-the-mssql-extension"></a>Installieren der mssql-Erweiterung

Die [mssql-Erweiterung für Visual Studio Code](https://aka.ms/mssql-marketplace) ermöglicht Ihnen das Herstellen einer Verbindung mit einer SQL Server-Instanz, das Abfragen mithilfe von Transact-SQL (T-SQL) und das Anzeigen der Ergebnisse.

1. Klicken Sie in Visual Studio Code auf **Ansicht** > **Befehlspalette**, oder drücken Sie **STRG**+**UMSCHALT**+**P** oder **F1**, um die **Befehlspalette** zu öffnen.

2. Wählen Sie in der **Befehlspalette** im Dropdownmenü **Erweiterungen: Erweiterungen installieren** aus.

3. Geben Sie *mssql* im Bereich **Erweiterungen** ein.

4. Wählen Sie die Erweiterung **SQL Server (mssql)** aus, und klicken Sie dann auf **Installieren**.

   ![Installieren der mssql-Erweiterung](./media/sql-server-develop-use-vscode/vscode-extension.png)

5. Klicken Sie nach Abschluss der Installation auf **Erneut laden**, um die Erweiterung zu aktivieren.

## <a name="create-or-open-a-sql-file"></a>Erstellen oder Öffnen einer SQL-Datei

Die mssql-Erweiterung ermöglicht mssql-Befehle und T-SQL-IntelliSense im Code-Editor, wenn der Sprachmodus auf **SQL** festgelegt ist.

1. Klicken Sie auf **Datei** > **Neue Datei**, oder drücken Sie **STRG**+**N**. Visual Studio Code öffnet standardmäßig eine neue Nur-Textdatei. 

2. Wählen Sie **Nur-Text** in der unteren Statusleiste aus, oder drücken Sie **STRG**+**K** > **M**, und wählen Sie **SQL** aus der Dropdownliste für Sprachen aus. 

   ![SQL-Sprachmodus](./media/sql-server-develop-use-vscode/vscode-language-mode.png)

   > [!NOTE]
   > Wenn Sie die Erweiterung zum ersten Mal verwenden, installiert die Erweiterung unterstützende SQL Server-Tools.

Wenn Sie eine vorhandene Datei mit der Erweiterung *.sql* öffnen, wird der Sprachmodus automatisch auf SQL festgelegt.  

## <a name="connect-to-sql-server"></a>Verbindung mit SQL Server herstellen

Führen Sue die folgenden Schritte aus, um ein Verbindungsprofil zu erstellen und eine Verbindung mit einer SQL Server-Instanz herzustellen.

1. Drücken Sie **STRG**+**UMSCHALT**+**P** oder **F1**, um die **Befehlspalette** zu öffnen. 

2. Geben Sie *sql* ein, um die mssql-Befehle anzuzeigen, oder geben Sie *sqlcon* ein, und wählen Sie dann im Dropdownmenü **MS SQL: Connect** aus.

   ![mssql-Befehle](./media/sql-server-develop-use-vscode/vscode-commands.png)

   >[!NOTE]
   >Eine SQL-Datei wie die leere SQL-Datei, die Sie erstellt haben, muss im Code-Editor über den Fokus verfügen, bevor Sie die mssql-Befehle ausführen können.

3. Wählen Sie den Befehl **MS SQL: Manage Connection Profiles** aus.

4. Klicken Sie anschließend auf **Erstellen**, um ein neues Verbindungsprofil für Ihre SQL Server-Instanz zu erstellen.

5. Befolgen Sie die Anweisungen, um die Eigenschaften für das neue Verbindungsprofil anzugeben. Nachdem Sie sämtliche Werte angegeben haben, drücken Sie die **EINGABETASTE**, um fortzufahren.

   | Verbindungseigenschaft | BESCHREIBUNG |
   |---|---|
   | **Servername oder ADO-Verbindungszeichenfolge** | Gibt den Namen der SQL Server-Instanz an. Verwenden Sie *localhost*, um eine Verbindung mit einer SQL Server-Instanz auf Ihrem lokalen Computer herzustellen. Geben Sie den Namen oder die IP-Adresse der SQL Server-Zielinstanz ein, um eine Verbindung mit einer SQL Server-Remoteinstanz herzustellen. Geben Sie die IP-Adresse des Hostcomputers des Containers an, um eine Verbindung mit einem SQL Server-Container herzustellen. Wenn Sie einen Port festlegen müssen, verwenden Sie ein Komma, um ihn vom Namen zu trennen. Geben Sie für einen Server, der an Port 1401 lauscht, beispielsweise `<servername or IP>,1401` ein.<br/><br/>Alternativ können Sie hier die ADO-Verbindungszeichenfolge für Ihre Datenbank eingeben. |
   | **Datenbankname** (optional) | Hier können Sie die Datenbank angeben, die Sie verwenden möchten. Geben Sie hier keinen Datenbanknamen an, wenn Sie eine Verbindung mit der Standarddatenbank herstellen möchten. |
   | **Authentifizierungstyp** | Wählen Sie entweder **Integriert** oder **SQL-Anmeldung** aus. |
   | **Benutzername** | Wenn Sie **SQL-Anmeldung** ausgewählt haben, geben Sie den Namen eines Benutzers mit Zugriff auf eine Datenbank auf dem Server aus. |
   | **Kennwort** | Geben Sie das Kennwort für den angegebenen Benutzer ein. |
   | **Kennwort speichern** | Drücken Sie die **EINGABETASTE**, um **Ja** auszuwählen und das Kennwort zu speichern. Wählen Sie **Nein** aus, um bei jeder Verwendung des Verbindungsprofils zur Eingabe des Kennworts aufgefordert zu werden. |
   | **Profilname** (optional) | Geben Sie einen Namen für das Verbindungsprofil ein, z. B. *localhost-Profil*. |

   Sobald Sie alle Werte eingegeben und auf **Eingeben** geklickt haben, erstellt Visual Studio Code das Verbindungsprofil und stellt eine Verbindung zur SQL Server-Instanz her.

   > [!TIP]
   > Wenn die Verbindung fehlschlägt, versuchen Sie das Problem mithilfe der Fehlermeldung im **Ausgabebereich** in Visual Studio Code zu diagnostizieren. Klicken Sie auf **Ansicht** > **Ausgabe**, um den **Ausgabebereich** zu öffnen. Überprüfen Sie auch die [Empfehlungen zur Verbindungsproblembehandlung](../../linux/sql-server-linux-troubleshooting-guide.md).

6. Überprüfen Sie Ihre Verbindung in der unteren Statusleiste.

   ![Verbindungsstatus](./media/sql-server-develop-use-vscode/vscode-connection-status.png)

Alternativ zu den vorherigen Schritten können Sie Verbindungsprofile auch in der Benutzereinstellungsdatei (*settings.json*) erstellen und bearbeiten. Klicken Sie auf **Datei** > **Einstellungen** > **Einstellungen**, um die Einstellungsdatei zu öffnen. Weitere Informationen finden Sie unter [Manage connection profiles (Verwalten von Verbindungsprofilen)](https://github.com/Microsoft/vscode-mssql/wiki/manage-connection-profiles).

## <a name="create-a-sql-database"></a>Erstellen einer SQL-Datenbank

1. Geben Sie *sql* in die neue SQL-Datei ein, die Sie zuvor gestartet haben, um eine Liste bearbeitbarer Codeausschnitte anzuzeigen.

   ![SQL-Ausschnitte](./media/sql-server-develop-use-vscode/vscode-sql-snippets.png)

2. Wählen Sie **sqlCreateDatabase** aus.

3. Ersetzen Sie „DatabaseName“ im Codeausschnitt durch `TutorialDB`:

   ```sql
   -- Create a new database called 'TutorialDB'
   -- Connect to the 'master' database to run this snippet
   USE master
   GO
   IF NOT EXISTS (
      SELECT name
      FROM sys.databases
      WHERE name = N'TutorialDB'
   )
   CREATE DATABASE [TutorialDB]
   GO
   ```

4. Drücken Sie **STRG**+**UMSCHALT**+**E**, um die Transact-SQL-Befehle auszuführen. Sehen Sie sich die Ergebnisse im Abfragefenster an.

    ![Datenbanknachrichten erstellen](./media/sql-server-develop-use-vscode/vscode-create-database-messages.png)

    > [!TIP]
    > Sie können die Tastenkombinationen für die mssql-Befehle anpassen. Weitere Informationen hierzu finden Sie unter [Customize shortcuts (Anpassen von Tastenkombinationen)](https://github.com/Microsoft/vscode-mssql/wiki/customize-shortcuts).

## <a name="create-a-table"></a>Erstellen einer Tabelle

1. Löschen Sie den Inhalt im Fenster des Code-Editors.

2. Drücken Sie **STRG**+**UMSCHALT**+**P** oder **F1**, um die **Befehlspalette** zu öffnen.

3. Geben Sie *sql* ein, um die mssql-Befehle anzuzeigen, oder geben Sie *sqluse* ein, und wählen Sie dann den Befehl **MS SQL: Use Database** aus.

4. Wählen Sie die neue Datenbank **TutorialDB** aus.

   ![Datenbank verwenden](./media/sql-server-develop-use-vscode/vscode-use-database.png)

5. Geben Sie *sql* im Code-Editor ein, um die Ausschnitte anzuzeigen. Wählen Sie **sqlCreateTable** aus, und drücken Sie dann die **EINGABETASTE**.

6. Geben Sie im Codeausschnitt `Employees` als Tabellennamen ein.

7. Drücken Sie die **TAB-TASTE**, um zum nächsten Feld zu gelangen, und geben Sie dann `dbo` als Schemanamen ein.

8. Ersetzen Sie die Spaltendefinitionen durch die folgenden Spalten:

   ```sql
   EmployeesId INT NOT NULL PRIMARY KEY,
   Name [NVARCHAR](50)  NOT NULL,
   Location [NVARCHAR](50)  NOT NULL
   ```

9. Drücken Sie **STRG**+**UMSCHALT**+**E**, um die Tabelle zu erstellen.

## <a name="insert-and-query"></a>Einfügen und Abfragen

1. Fügen Sie die folgenden Anweisungen hinzu, um vier Zeilen in die Tabelle **Employees** einzufügen.

   ```sql
   -- Insert rows into table 'Employees'
   INSERT INTO Employees
      ([EmployeesId],[Name],[Location])
   VALUES
      ( 1, N'Jared', N'Australia'),
      ( 2, N'Nikita', N'India'),
      ( 3, N'Tom', N'Germany'),
      ( 4, N'Jake', N'United States')
   GO
   -- Query the total count of employees
   SELECT COUNT(*) as EmployeeCount FROM dbo.Employees;
   -- Query all employee information
   SELECT e.EmployeesId, e.Name, e.Location
   FROM dbo.Employees as e
   GO
   ```

   Beim Schreiben unterstützt Sie T-SQL-IntelliSense bei der Vervollständigung der Anweisungen:

   ![T-SQL-IntelliSense](./media/sql-server-develop-use-vscode/vscode-intellisense.png)

   > [!TIP]
   > Die mssql-Erweiterung verfügt ebenfalls über Befehle, die Ihnen beim Erstellen von INSERT- und SELECT-Anweisungen helfen. Diese wurden im vorherigen Beispiel nicht verwendet.

2. Drücken Sie **STRG**+**UMSCHALT**+**E**, um die Befehle auszuführen. Die beiden Ergebnisse werden im Fenster **Ergebnisse** angezeigt.

   ![Ergebnisse](./media/sql-server-develop-use-vscode/vscode-result-grid.png)

## <a name="view-and-save-the-result"></a>Anzeigen und Speichern der Ergebnisse

1. Klicken Sie auf **Ansicht** > **Editorlayout** > **Layout spiegeln**, um zwischen einem vertikal oder horizontal aufgeteilten Layout zu wechseln.

2. Wählen Sie die Header der Bereiche **Ergebnisse** und **Nachrichten** aus, um die Bereiche zuzuklappen oder aufzuklappen.

   ![Header umschalten](./media/sql-server-develop-use-vscode/vscode-toggle-messages-pannel.png)

   > [!TIP]
   > Sie können das Standardverhalten der mssql-Erweiterung anpassen. Informationen dazu finden Sie unter [Customize extension options (Anpassen der Erweiterungsoptionen)](https://github.com/Microsoft/vscode-mssql/wiki/customize-options).

3. Klicken Sie im zweiten Ergebnisraster auf das Symbol „Raster maximieren“, um diese Ergebnisse zu vergrößern.

   ![Raster maximieren](./media/sql-server-develop-use-vscode/vscode-maximize-grid.png)

   > [!NOTE]
   > Das Symbol wird angezeigt, wenn Ihr T-SQL-Skript mindestens zwei Ergebnisraster erzeugt.

4. Öffnen Sie das Rasterkontextmenü, indem Sie mit der rechten Maustaste auf das Raster klicken.

   ![Kontextmenü](./media/sql-server-develop-use-vscode/vscode-grid-context-menu.png)

5. Wählen Sie **Alles auswählen** aus.

6. Öffnen Sie das Rasterkontextmenü erneut, und wählen Sie **Als JSON speichern** aus, um das Ergebnis in einer *JSON*-Datei zu speichern.

7. Geben Sie einen Dateinamen für die JSON-Datei an.

8. Überprüfen Sie, ob die JSON-Datei gespeichert wurde und in Visual Studio Code geöffnet werden kann.

   ![Als JSON speichern](./media/sql-server-develop-use-vscode/vscode-save-as-json.png)

Wenn Sie SQL-Skripts für die Verwaltung oder für ein größeres Entwicklungsprojekt später speichern und ausführen müssen, speichern Sie die Skripts mit der Erweiterung *.sql*.

## <a name="next-steps"></a>Nächste Schritte

Unter [Tutorial: Schreiben von Transact-SQL-Anweisungen](../../t-sql/tutorial-writing-transact-sql-statements.md) und [Transact-SQL-Referenz (Datenbank-Engine)](../../t-sql/language-reference.md) finden Sie weitere Informationen, wenn Sie noch nicht mit T-SQL vertraut sind.

Weitere Informationen zum Verwenden oder Mitwirken an der mssql-Erweiterung finden Sie im [Wiki des mssql-Erweiterungsprojekts](https://github.com/Microsoft/vscode-mssql/wiki).

Weitere Informationen zur Verwendung von Visual Studio Code finden Sie in der [Visual Studio Code-Dokumentation](https://code.visualstudio.com/docs).