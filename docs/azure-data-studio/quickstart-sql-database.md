---
title: Herstellen einer Verbindung mit einer Azure SQL-Datenbank-Instanz und deren Abfrage
description: In diesem Schnellstart verwenden Sie Azure Data Studio, um eine Verbindung mit einem Azure SQL-Datenbank-Server herzustellen. Anschließend erstellen Sie eine Datenbank und fragen diese ab.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.reviewer: alayu; maghan; sstein
ms.topic: quickstart
author: yualan
ms.author: alayu
ms.custom: seodec18; sqlfreshmay19; seo-lt-2019
ms.date: 05/14/2019
ms.openlocfilehash: c372b09d9dd3c96fb34f75e16a27a5a6aa3238b9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048250"
---
# <a name="quickstart-use-azure-data-studio-to-connect-and-query-azure-sql-database"></a>Schnellstart: Verwenden von Azure Data Studio, um eine Verbindung mit einer Azure SQL-Datenbank-Instanz herzustellen und sie abzufragen

In dieser Schnellstartanleitung stellen Sie mit Azure Data Studio eine Verbindung mit einer Azure SQL-Datenbank-Serverinstanz her. Anschließend führen Sie Transact-SQL-Anweisungen (T-SQL) aus, um die TutorialDB-Datenbank zu erstellen und abzufragen, die in anderen Azure Data Studio-Tutorials verwendet wird.

## <a name="prerequisites"></a>Voraussetzungen

Um diesen Schnellstart abzuschließen, benötigen Sie Azure Data Studio und eine Azure SQL-Datenbank-Serverinstanz.

- [Azure Data Studio installieren](./download-azure-data-studio.md)

Wenn Sie noch nicht über eine Azure SQL-Serverinstanz verfügen, führen Sie einen der folgenden Schnellstarts für Azure SQL-Datenbank aus. Merken Sie sich den voll qualifizierten Servernamen und die Anmeldeinformationen für die späteren Schritte:

- [Erstellen einer Datenbank – Portal](/azure/sql-database/sql-database-get-started-portal)
- [Erstellen einer Datenbank – CLI](/azure/sql-database/sql-database-get-started-cli)
- [Erstellen einer Datenbank – PowerShell](/azure/sql-database/sql-database-get-started-powershell)


## <a name="connect-to-your-azure-sql-database-server"></a>Herstellen einer Verbindung mit Ihrer Azure SQL-Datenbank-Serverinstanz

Verwenden Sie Azure Data Studio, um eine Verbindung mit Ihrem Azure SQL-Datenbank-Server herzustellen.

1. Wenn Sie Azure Data Studio zum ersten Mal ausführen, sollte die **Homepage** geöffnet werden. Wenn die **Homepage** nicht angezeigt wird, wählen Sie **Hilfe** > **Willkommen** aus. Wählen Sie **Neue Verbindung** aus, um den Bereich **Verbindung** zu öffnen:
   
   ![Screenshot: Dialogfeld „Willkommen – Azure Data Studio“ mit umrandeter Option „Neue Verbindung“](media/quickstart-sql-database/new-connection-icon.png)

2. In diesem Artikel wird die SQL-Anmeldung verwendet, aber auch die Windows-Authentifizierung unterstützt. Füllen Sie die folgenden Felder mit dem Servernamen, Benutzernamen und Kennwort für Ihre Azure SQL-Serverinstanz aus:

   | Einstellung       | Vorgeschlagener Wert | BESCHREIBUNG |
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Servername** | Der vollqualifizierte Servername | Etwa: **servername.database.windows.net**. |
   | **Authentifizierung** | SQL-Anmeldung| In diesem Tutorial wird die SQL-Authentifizierung verwendet. |
   | **Benutzername** | Der Benutzername des Serveradministratorkontos | Der Benutzername des Kontos, das zum Erstellen des Servers verwendet wird. |
   | **Kennwort (SQL-Anmeldung)** | Das Kennwort des Serveradministratorkontos | Das Kennwort des Kontos, das zum Erstellen des Servers verwendet wird. |
   | **Kennwort speichern** | Ja oder Nein | Wählen Sie **Ja** aus, wenn Sie nicht bei jedem Neustart Ihr Kennwort eingeben möchten. |
   | **Datenbankname** | *Leer lassen* | Sie stellen hier nur eine Verbindung mit dem Server her. |
   | **Servergruppe** | <Default> auswählen | Sie können für dieses Feld eine bestimmte Servergruppe festlegen, die Sie erstellt haben. | 

   ![Screenshot: Seite „Verbindung“ in Azure Data Studio](media/quickstart-sql-database/new-connection-screen.png)  

3. Wählen Sie **Verbinden**.

4. Wenn der Server nicht über eine Firewallregel verfügt, die Azure Data Studio ermöglicht, eine Verbindung herzustellen, wird das Formular **Neue Firewallregel erstellen** geöffnet. Füllen Sie das Formular aus, um eine neue Firewallregel zu erstellen. Weitere Informationen finden Sie unter [Firewallregeln](/azure/sql-database/sql-database-firewall-configure).

   ![Neue Firewallregel](media/quickstart-sql-database/firewall.png)  

Nachdem die Verbindung erfolgreich hergestellt wurde, wird Ihr Server in der Randleiste **SERVER** geöffnet.

## <a name="create-the-tutorial-database"></a>Erstellen der Tutorialdatenbank

In den nächsten Abschnitten wird die TutorialDB-Datenbank erstellt, die in anderen Azure Data Studio-Tutorials verwendet wird.

1. Klicken Sie in Ihrer Azure SQL-Serverinstanz mit der rechten Maustaste auf die Randleiste **SERVER**, und wählen Sie **Neue Abfrage** aus.

1. Fügen Sie diesen SQL-Code in den Abfrage-Editor ein.

   ```sql
   IF NOT EXISTS (
      SELECT name
      FROM sys.databases
      WHERE name = N'TutorialDB'
   )
   CREATE DATABASE [TutorialDB]
   GO

   ALTER DATABASE [TutorialDB] SET QUERY_STORE=ON
   GO
   ```

1. Wählen Sie in der Symbolleiste **Ausführen** aus. Benachrichtigungen werden im Bereich **MELDUNGEN** angezeigt, der den Status der Abfrage anzeigt.

## <a name="create-a-table"></a>Erstellen einer Tabelle

Der Abfrage-Editor ist mit der Datenbank **master** verbunden, aber wir möchten eine Tabelle in der Datenbank **TutorialDB** erstellen. 

1. Stellen Sie eine Verbindung mit der **TutorialDB**-Datenbank her.

   ![Kontext ändern](media/quickstart-sql-database/change-context2.png)



1. Erstellen Sie eine `Customers`-Tabelle. 

   Ersetzen Sie die vorherige Abfrage im Abfrage-Editor durch diese, und wählen Sie **Ausführen** aus.

   ```sql
   -- Create a new table called 'Customers' in schema 'dbo'
   -- Drop the table if it already exists
   IF OBJECT_ID('dbo.Customers', 'U') IS NOT NULL
   DROP TABLE dbo.Customers
   GO
   -- Create the table in the specified schema
   CREATE TABLE dbo.Customers
   (
      CustomerId        INT    NOT NULL   PRIMARY KEY, -- primary key column
      Name      [NVARCHAR](50)  NOT NULL,
      Location  [NVARCHAR](50)  NOT NULL,
      Email     [NVARCHAR](50)  NOT NULL
   );
   GO
   ```


## <a name="insert-rows-into-the-table"></a>Einfügen von Zeilen in die Tabelle

Ersetzen Sie die vorherige Abfrage durch diese, und wählen Sie **Ausführen** aus.

   ```sql
   -- Insert rows into table 'Customers'
   INSERT INTO dbo.Customers
      ([CustomerId],[Name],[Location],[Email])
   VALUES
      ( 1, N'Orlando', N'Australia', N''),
      ( 2, N'Keith', N'India', N'keith0@adventure-works.com'),
      ( 3, N'Donna', N'Germany', N'donna0@adventure-works.com'),
      ( 4, N'Janet', N'United States', N'janet1@adventure-works.com')
   GO
   ```

## <a name="view-the-result"></a>Anzeigen des Ergebnisses

Ersetzen Sie die vorherige Abfrage durch diese, und wählen Sie **Ausführen** aus.

   ```sql
   -- Select rows from table 'Customers'
   SELECT * FROM dbo.Customers;
   ```

Die Abfrageergebnisse werden angezeigt:

   ![Select-Ergebnis](media/quickstart-sql-database/select-results2.png)


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Spätere Schnellstartartikel bauen auf den hier erstellten Ressourcen auf. Wenn Sie beabsichtigen, diese Artikel durchzuarbeiten, achten Sie darauf, diese Ressourcen nicht zu löschen. Andernfalls können Sie im Azure-Portal die Ressourcen löschen, die Sie nicht mehr benötigen. Weitere Informationen finden Sie unter [Bereinigen von Ressourcen](/azure/sql-database/sql-database-get-started-portal#clean-up-resources).

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie erfolgreich eine Verbindung mit einer Azure SQL-Datenbank-Instanz hergestellt und eine Abfrage ausgeführt haben, testen Sie das [Tutorial zum Code-Editor](tutorial-sql-editor.md).