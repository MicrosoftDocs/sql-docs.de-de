---
title: Herstellen einer Verbindung mit und Abfragen einer SQL Server-Instanz mithilfe des SQL Server Management Studio (SSMS)
description: Stellen Sie im SSMS eine Verbindung mit einer SQL Server-Instanz her. Erstellen Sie eine SQL Server-Datenbank, und fragen Sie sie durch Ausführen einfacher T-SQL-Abfragen im SSMS ab.
ms.prod: sql
ms.technology: ssms
ms.topic: quickstart
author: markingmyname
ms.author: maghan
ms.reviewer: sstein, mikeray
ms.custom: contperf-fy21q2
ms.date: 12/15/2020
ms.openlocfilehash: 92160da1da48fc107be98354250e4c580cb51155
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350595"
---
# <a name="quickstart-connect-and-query-a-sql-server-instance-using-sql-server-management-studio-ssms"></a>Schnellstart: Herstellen einer Verbindung mit und Abfragen einer SQL Server-Instanz mithilfe des SQL Server Management Studio (SSMS)

[!INCLUDE [sqlserver](../../includes/applies-to-version/sqlserver.md)]

Beginnen Sie mit der Nutzung des SQL Server Management Studio (SSMS) zum Herstellen einer Verbindung mit Ihrer SQL Server-Instanz und Ausführen einiger T-SQL-Befehle (Transact-SQL).

Dieser Artikel zeigt, wie die folgenden Schritte ausgeführt werden:

> [!div class="checklist"]
> - Eine Verbindung mit einer SQL Server-Instanz herstellen
> - Erstellen einer Datenbank
> - Erstellen einer Tabelle in Ihrer neuen Datenbank
> - Einfügen von Zeilen in Ihre neue Tabelle
> - Abfragen der neuen Tabelle und Aufrufen der Ergebnisse
> - Überprüfen der Verbindungseigenschaften mit der Tabelle im Abfragefenster

## <a name="prerequisites"></a>Voraussetzungen

- Installiertes [SQL Server Management Studio](../download-sql-server-management-studio-ssms.md)
- Installierte und konfigurierte [SQL Server-Instanz](https://www.microsoft.com/sql-server/sql-server-downloads)

## <a name="connect-to-a-sql-server-instance"></a>Eine Verbindung mit einer SQL Server-Instanz herstellen

1. Starten Sie SQL Server Management Studio. Beim ersten Ausführen von SSMS wird das Fenster **Connect to Server** (Verbindung mit Server herstellen) geöffnet. Wenn das Fenster nicht geöffnet wird, können Sie es manuell öffnen, indem Sie auf **Objekt-Explorer** > **Verbinden** > **Datenbank-Engine** klicken.

    :::image type="content" source="media/ssms-connect-query-sql-server/connect-object-explorer.png" alt-text="Option „Verbinden“ im Objekt-Explorer":::

2. Das Dialogfeld **Mit Server verbinden** wird angezeigt. Geben Sie Folgendes ein:

    |   Einstellung   |   Empfohlene Werte   |   BESCHREIBUNG   |
    |--------------|-----------------------|-----------------|
    | **Servertyp** | Datenbank-Engine | Wählen Sie für **Servertyp** die Option **Datenbank-Engine** (normalerweise die Standardoption) aus. |
    | **Servername** | Der vollqualifizierte Servername | Geben Sie bei **Servername** den Namen Ihrer SQL Server-Instanz ein. Wenn Sie lokal eine Verbindung herstellen, können Sie auch *localhost* als Servernamen verwenden. Wenn Sie NICHT die Standardinstanz (***MSSQLSERVER***) verwenden, müssen Sie den Servernamen und den Instanznamen angeben. </br> </br> Wenn Sie nicht genau wissen, wie Sie Ihren SQL Server-Instanznamen bestimmen sollen, erhalten Sie hier [zusätzliche Tipps und Tricks für die Verwendung von SSMS](../tutorials/ssms-tricks.md#find-sql-server-instance-name). |
    | **Authentifizierung** | Windows-Authentifizierung </br> </br> SQL Server-Authentifizierung | Standardmäßig ist die Windows-Authentifizierung festgelegt. </br> </br> Sie können zum Herstellen einer Verbindung auch die **SQL Server-Authentifizierung** verwenden. Wenn Sie die **SQL Server-Authentifizierung** auswählen, sind jedoch ein Benutzername und ein Kennwort erforderlich. </br> </br> Weitere Informationen zu Authentifizierungstypen finden Sie unter [Verbindung mit Server herstellen (Datenbank-Engine)](../f1-help/connect-to-server-database-engine.md). |
    | **Anmeldung** | Benutzer-ID des Serverkontos | Hier wird die Benutzer-ID des zum Anmelden beim Server verwendeten Serverkontos angegeben. Wenn die **SQL Server-Authentifizierung** verwendet wird, ist ein Anmeldename erforderlich. |
    | **Kennwort** | Kennwort für das Serverkonto | Hier wird das Kennwort für das zum Anmelden beim Server verwendete Serverkonto angegeben. Wenn die **SQL Server-Authentifizierung** verwendet wird, ist ein Kennwort erforderlich. |

    :::image type="content" source="media/ssms-connect-query-sql-server/connect-to-sql-server-object-explorer.png" alt-text="Feld „Servername“ für SQL Server":::

3. Nachdem Sie alle Felder ausgefüllt haben, klicken Sie auf **Verbinden**.

    Sie können auch zusätzliche Verbindungsoptionen ändern, indem Sie **Optionen** auswählen. Beispiele für Verbindungsoptionen sind die Datenbank, mit der Sie sich verbinden, der Verbindungstimeoutwert und das Netzwerkprotokoll. In diesem Artikel werden für alle Felder die Standardwerte verwendet.

4. Erweitern Sie im **Objekt-Explorer** den Eintrag mit dem Servernamen, der SQL Server-Version und dem Benutzernamen, und sehen Sie sich die darin enthaltenen Objekte an, um zu überprüfen, ob Sie erfolgreich eine Verbindung mit SQL Server hergestellt haben. Diese Objekte unterscheiden sich je nach Servertyp.

    :::image type="content" source="media/ssms-connect-query-sql-server/connect-on-prem.png" alt-text="Herstellen einer Verbindung mit einem lokalen Server":::

## <a name="troubleshoot-connectivity-issues"></a>Behandeln von Konnektivitätsproblemen

Wenn Sie sich die Problembehandlungstechniken für Fälle ansehen möchten, in denen Sie keine Verbindung mit einer Instanz der SQL Server-Datenbank-Engine auf einem einzelnen Server herstellen können, finden Sie diese unter [Beheben von Verbindungsfehlern mit der SQL Server-Datenbank-Engine](../../database-engine/configure-windows/troubleshoot-connecting-to-the-sql-server-database-engine.md).

## <a name="create-a-database"></a>Erstellen einer Datenbank

Führen Sie nun die folgenden Schritte aus, um eine Datenbank namens TutorialDB zu erstellen:

1. Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf Ihre Serverinstanz und anschließend mit der linken auf **Neue Abfrage**:

   :::image type="content" source="media/ssms-connect-query-sql-server/new-query.png" alt-text="Die Verknüpfung „Neue Abfrage“":::

2. Fügen Sie den folgenden T-SQL-Codeausschnitt in das Abfragefenster ein:

    ```sql
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

3. Klicken Sie zum Ausführen der Abfrage auf **Ausführen**, oder drücken Sie F5.

   :::image type="content" source="media/ssms-connect-query-sql-server/execute.png" alt-text="Befehl „Ausführen“":::
  
    Nachdem die Abfrage abgeschlossen ist, wird die neue Datenbank „TutorialDB“ in der Datenbankliste im Objekt-Explorer angezeigt. Wenn die Datenbank nicht angezeigt wird, klicken Sie zuerst mit der rechten Maustaste auf den **Datenbankenknoten** und anschließend mit der linken auf **Aktualisieren**.

## <a name="create-a-table-in-the-new-database"></a>Erstellen einer Tabelle in der neuen Datenbank

In diesem Abschnitt erstellen Sie nun eine Tabelle in der neuen Datenbank „TutorialDB“. Da sich der Abfrage-Editor immer noch im Kontext der *Master*-Datenbank befindet, ändern Sie den Verbindungskontext in die *TutorialDB*-Datenbank, indem Sie folgende Schritte ausführen:

1. Wählen Sie in der Dropdownliste die gewünschte Datenbank aus, so wie hier dargestellt:

   :::image type="content" source="media/ssms-connect-query-sql-server/change-db.png" alt-text="Ändern der Datenbank":::

2. Fügen Sie den folgenden T-SQL-Codeausschnitt in das Abfragefenster ein:

    ```sql
    USE [TutorialDB]
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

3. Klicken Sie zum Ausführen der Abfrage auf **Ausführen**, oder drücken Sie F5.

Nachdem die Abfrage abgeschlossen ist, wird die neue Tabelle „Customers“ in der Tabellenliste im Objekt-Explorer angezeigt. Wenn die Tabelle nicht angezeigt wird, klicken Sie im Objekt-Explorer mit der rechten Maustaste auf den Knoten **TutorialDB** > **Tabellen**, und wählen Sie dann **Aktualisieren** aus.

   :::image type="content" source="media/ssms-connect-query-sql-server/new-table.png" alt-text="Neue Tabelle":::

## <a name="insert-rows-into-the-new-table"></a>Einfügen von Zeilen in die neue Tabelle

Fügen Sie nun einige Zeilen in die von Ihnen erstellte Tabelle „Customers“ ein. Fügen Sie den folgenden T-SQL-Codeausschnitt in das Abfragefenster ein, und klicken Sie auf **Ausführen**:

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

## <a name="query-the-table-and-view-the-results"></a>Abfragen der Tabelle und Aufrufen der Ergebnisse

Die Ergebnisse einer Abfrage werden unter dem Abfragetextfenster angezeigt. Führen Sie die folgenden Schritte aus, um die Tabelle „Customers“ abzufragen und die eingefügten Zeilen anzuzeigen:

1. Fügen Sie den folgenden T-SQL-Codeausschnitt in das Abfragefenster ein, und klicken Sie auf **Ausführen**:

   ```sql
   -- Select rows from table 'Customers'
   SELECT * FROM dbo.Customers;
   ```

    Die Ergebnisse der Abfrage werden unter dem Bereich angezeigt, in dem der Text eingegeben wurde.

   :::image type="content" source="media/ssms-connect-query-sql-server/query-results.png" alt-text="Die Ergebnisliste":::

    Sie können auch die Darstellung der angezeigten Ergebnisse ändern, indem Sie eine der folgenden Optionen auswählen:

   ![Drei Optionen zum Anzeigen von Abfrageergebnissen](media/ssms-connect-query-sql-server/results.png)

   - Mit der linken Schaltfläche werden die Ergebnisse in der **Textansicht** dargestellt, wie in der Abbildung im nächsten Abschnitt zu sehen ist.
   - Die mittlere Schaltfläche zeigt die Ergebnisse in der **Rasteransicht**, also in der Standardansicht, an.
       - Dies ist die Standardeinstellung.
   - Mit der dritten Schaltfläche können Sie die Ergebnisse in einer Datei speichern, deren Erweiterung standardmäßig nicht RPT ist.

## <a name="verify-your-connection-properties-by-using-the-query-window-table"></a>Überprüfen Ihrer Verbindungseigenschaften mit der Tabelle im Abfragefenster

Informationen zu Verbindungseigenschaften finden Sie unter den Ergebnissen einer Abfrage. Nachdem Sie die Abfrage aus dem vorherigen Schritt ausgeführt haben, können Sie sich die Verbindungseigenschaften im unteren Bereich des Abfragefensters ansehen.

- Hier wird angezeigt, mit welchem Server und welcher Datenbank Sie verbunden sind. Außerdem ist der von Ihnen verwendete Benutzername zu sehen.
- Des Weiteren sind auch die Abfragedauer und die Anzahl der Zeilen angezeigt, die von der zuvor ausgeführten Abfrage zurückgegeben wurden.

   :::image type="content" source="media/ssms-connect-query-sql-server/connection-properties.png" alt-text="Verbindungseigenschaften":::

## <a name="additional-tools"></a>Weitere Tools

Sie können auch [Azure Data Studio](../../azure-data-studio/download-azure-data-studio.md) verwenden, um Verbindungen mit [SQL Server](../../azure-data-studio/quickstart-sql-server.md)-, [Azure SQL-Datenbank](../../azure-data-studio/quickstart-sql-database.md)- und [Azure Synapse Analytics](../../azure-data-studio/quickstart-sql-dw.md)-Instanzen herzustellen und diese abzufragen.

## <a name="next-steps"></a>Nächste Schritte

Am besten machen Sie sich mit SSMS vertraut, indem Sie einige praktische Aufgaben durchführen. Diese Artikel unterstützen Sie bei der Verwendung der verschiedenen Features, die in SSMS verfügbar sind.

- [SSMS-Abfrage-Editor (SQL Server Management Studio)](../f1-help/database-engine-query-editor-sql-server-management-studio.md)
- [Skripterstellung](../tutorials/scripting-ssms.md)
- [Verwenden von Vorlagen in SSMS](../template/templates-ssms.md)
- [SSMS-Konfiguration](../tutorials/ssms-configuration.md)
- [Zusätzliche Tipps und Tricks für die Verwendung von SSMS](../tutorials/ssms-tricks.md)