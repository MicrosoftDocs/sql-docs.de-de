---
title: 'Schnellstart: Herstellen einer Verbindung mit PostgreSQL und Abfragen von Daten mit PostgreSQL'
description: In diesem Schnellstart verwenden Sie Azure Data Studio zum Herstellen einer Verbindung mit PostgreSQL, und dann verwenden Sie SQL-Anweisungen, um eine Datenbank zu erstellen und abzufragen.
ms.custom: seodec18
ms.date: 09/18/2019
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.reviewer: alayu, maghan, sstein
ms.topic: quickstart
author: sunilagarwal
ms.author: sunila
ms.openlocfilehash: dad9f6348b3df9d7589be739af5ba7c748d37b6e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040082"
---
# <a name="quickstart-use-azure-data-studio-to-connect-and-query-postgresql"></a>Schnellstart: Verwenden von Azure Data Studio zum Verbinden mit und Abfragen von PostgreSQL

In diesem Schnellstart erfahren Sie, wie Sie mithilfe von Azure Data Studio eine Verbindung mit PostgreSQL herstellen und anschließend SQL-Anweisungen verwenden, um die Datenbank *tutorialdb* zu erstellen und abzufragen.

## <a name="prerequisites"></a>Voraussetzungen

Für diesen Schnellstart benötigen Sie Azure Data Studio, die PostgreSQL-Erweiterung für Azure Data Studio und Zugriff auf eine PostgreSQL-Serverinstanz.

- [Installieren Sie Azure Data Studio](./download-azure-data-studio.md).
- [Installieren Sie die PostgreSQL-Erweiterung für Azure Data Studio](./extensions/postgres-extension.md).
- [Installieren Sie PostgreSQL](https://www.postgresql.org/download/). (Alternativ können Sie mithilfe von [az postgres up](/azure/postgresql/quickstart-create-server-up-azure-cli) eine Postgres-Datenbank in der Cloud erstellen.) 

## <a name="connect-to-postgresql"></a>Herstellen einer Verbindung mit PostgreSQL

1. Starten Sie **Azure Data Studio**.

2. Wenn Sie Azure Data Studio zum ersten Mal starten, wird das Dialogfeld **Verbindung** geöffnet. Wenn das Dialogfeld **Verbindung** nicht geöffnet wird, klicken Sie auf der Seite **SERVER** auf das Symbol **Neue Verbindung**:

   ![Symbol „Neue Verbindung“](media/quickstart-postgresql/new-connection-icon.png)

3. Wechseln Sie in dem angezeigten Formular zu **Verbindungstyp**, und wählen Sie in der Dropdownliste **PostgreSQL** aus.


4. Füllen Sie die übrigen Felder mit dem Servernamen, Benutzernamen und Kennwort für Ihre PostgreSQL-Serverinstanz aus. 

   ![Bildschirm „Neue Verbindung“](media/quickstart-postgresql/new-connection-screen.png)  

   | Einstellung       | Beispielwert | BESCHREIBUNG |
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Servername** | localhost | Der vollqualifizierte Servername |
   | **Benutzername** | postgres | Der Benutzername, mit dem Sie sich anmelden möchten. |
   | **Kennwort (SQL-Anmeldung)** | *password* | Das Kennwort für das Konto, mit dem Sie sich anmelden. |
   | **Kennwort** | *Überprüfung* | Aktivieren Sie dieses Kontrollkästchen, wenn Sie nicht immer dann, wenn Sie eine Verbindung herstellen, Ihr Kennwort eingeben möchten. |
   | **Datenbankname** | \<Default\> | Füllen Sie dies aus, wenn die Verbindung eine Datenbank angeben soll. |
   | **Servergruppe** | \<Default\> | Mit dieser Option können Sie diese Verbindung einer bestimmten Servergruppe zuweisen, die Sie erstellen. | 
   | **Name (optional)** | *Leer lassen* | Mit dieser Option können Sie einen Anzeigenamen für den Server angeben. | 

5. Wählen Sie **Verbinden**. 

Nachdem die Verbindung erfolgreich hergestellt wurde, wird Ihr Server in der Randleiste **SERVER** geöffnet.


## <a name="create-a-database"></a>Erstellen einer Datenbank

Mit den folgenden Schritten wird eine Datenbank mit dem Namen **tutorialdb** erstellt:

1. Klicken Sie in Ihrer PostgreSQL-Serverinstanz mit der rechten Maustaste auf die Randleiste **SERVER**, und wählen Sie **Neue Abfrage** aus.

2. Fügen Sie diese SQL-Anweisung in den Abfrage-Editor ein, der geöffnet wird.

   ```sql
   CREATE DATABASE tutorialdb;
   ```

3. Wählen Sie in der Symbolleiste **Ausführen** aus, um die Abfrage auszuführen. Benachrichtigungen werden im Bereich **MELDUNGEN** angezeigt, um den Status der Abfrage anzuzeigen.

>[!TIP]
> Sie können die Anweisung mit **F5** ausführen, anstatt **Ausführen** zu verwenden.

Klicken Sie nach Abschluss der Abfrage mit der rechten Maustaste auf **Datenbanken**, und wählen Sie **Aktualisieren** aus, um **tutorialdb** in der Liste unter dem Knoten **Datenbanken** anzuzeigen.


## <a name="create-a-table"></a>Erstellen einer Tabelle

 Mit den folgenden Schritten wird eine neue Tabelle in **tutorialdb** erstellt:

1. Ändern Sie den Verbindungskontext mithilfe der Dropdownliste im Abfrage-Editor in **tutorialdb**. 

   ![Kontext ändern](media/quickstart-postgresql/change-context.png)

2. Fügen Sie die folgende SQL-Anweisung in den Abfrage-Editor ein, und klicken Sie auf **Ausführen**. 

   > [!NOTE]
   > Sie können dies entweder anfügen oder die vorhandene Abfrage im Editor überschreiben. Beim Klicken auf **Ausführen** wird nur die hervorgehobene Abfrage ausgeführt. Wenn nichts hervorgehoben ist, werden beim Klicken auf **Ausführen** alle Abfragen im Editor ausgeführt.

   ```sql
   -- Drop the table if it already exists
   DROP TABLE IF EXISTS customers;
   -- Create a new table called 'customers'
   CREATE TABLE customers(
       customer_id SERIAL PRIMARY KEY,
       name VARCHAR (50) NOT NULL,
       location VARCHAR (50) NOT NULL,
       email VARCHAR (50) NOT NULL
   );
   ```

## <a name="insert-rows"></a>Einfügen von Zeilen

Fügen Sie den folgenden Codeausschnitt in das Abfragefenster ein, und klicken Sie auf **Ausführen**:

   ```sql
   -- Insert rows into table 'customers'
   INSERT INTO customers
       (customer_id, name, location, email)
    VALUES
      ( 1, 'Orlando', 'Australia', ''),
      ( 2, 'Keith', 'India', 'keith0@adventure-works.com'),
      ( 3, 'Donna', 'Germany', 'donna0@adventure-works.com'),
      ( 4, 'Janet', 'United States','janet1@adventure-works.com');
   ```

## <a name="query-the-data"></a>Abfragen der Daten

1. Fügen Sie den folgenden Codeausschnitt in den Abfrage-Editor ein, und klicken Sie auf **Ausführen**:
   
   ```sql
   -- Select rows from table 'customers'
   SELECT * FROM customers; 
   ```

2. Die Abfrageergebnisse werden angezeigt:

   ![Anzeigen der Ergebnisse](media/quickstart-postgresql/view-results.png)

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die [in Azure Data Studio für Postgres verfügbaren Szenarios](./extensions/postgres-extension.md).