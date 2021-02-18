---
title: Herstellen einer Verbindung mit und Abfragen einer SQL Server-Instanz auf einer Azure-VM mithilfe des SQL Server Management Studio (SSMS)
description: Stellen Sie mithilfe des SSMS eine Verbindung mit einer SQL Server-Instanz auf einer Azure-VM her. Erstellen Sie eine SQL Server-Instanz auf einer Azure-VM, und fragen Sie sie durch Ausführen einfacher T-SQL-Abfragen im SSMS ab.
ms.prod: sql
ms.technology: ssms
ms.topic: quickstart
author: markingmyname
ms.author: maghan
ms.reviewer: sstein, mikeray
ms.custom: contperf-fy21q2
ms.date: 12/15/2020
ms.openlocfilehash: b6442715c1482ed581302667bcdaf7985948c554
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350606"
---
# <a name="quickstart-connect-and-query-a-sql-server-instance-on-an-azure-virtual-machine-using-sql-server-management-studio-ssms"></a>Schnellstart: Herstellen einer Verbindung mit und Abfragen einer SQL Server-Instanz auf einem virtuellen Azure-Computer mithilfe des SQL Server Management Studio (SSMS)

[!INCLUDE [sqlserver](../../includes/applies-to-version/sqlserver.md)]

Beginnen Sie mit der Nutzung des SQL Server Management Studio (SSMS) zum Herstellen einer Verbindung mit Ihrer SQL Server-Instanz auf einem virtuellen Azure-Computer und Ausführen einiger T-SQL-Befehle (Transact-SQL).

> [!div class="checklist"]
> - Eine Verbindung mit einer SQL Server-Instanz herstellen
> - Erstellen einer Datenbank
> - Erstellen einer Tabelle in Ihrer neuen Datenbank
> - Einfügen von Zeilen in Ihre neue Tabelle
> - Abfragen der neuen Tabelle und Aufrufen der Ergebnisse
> - Überprüfen der Verbindungseigenschaften mit der Tabelle im Abfragefenster
## <a name="prerequisites"></a>Voraussetzungen

Zum Ausführen der Schritte in diesem Artikel benötigen Sie das SQL Server Management Studio und Zugriff auf eine Datenquelle.

- Installieren Sie [SQL Server Management Studio](../download-sql-server-management-studio-ssms.md).
- [SQL Server auf einer Azure-VM](https://azure.microsoft.com/services/virtual-machines/sql-server/?&ef_id=CjwKCAiA17P9BRB2EiwAMvwNyP3g3mY45X8dbqEtIr8HASwSLUYRBrwwciZptYt0vUU4K7TuWnXTbxoCoPQQAvD_BwE:G:s&OCID=AID2100131_SEM_CjwKCAiA17P9BRB2EiwAMvwNyP3g3mY45X8dbqEtIr8HASwSLUYRBrwwciZptYt0vUU4K7TuWnXTbxoCoPQQAvD_BwE:G:s&gclid=CjwKCAiA17P9BRB2EiwAMvwNyP3g3mY45X8dbqEtIr8HASwSLUYRBrwwciZptYt0vUU4K7TuWnXTbxoCoPQQAvD_BwE)

## <a name="connect-to-sql-virtual-machines"></a>Herstellen einer Verbindung mit virtuellen SQL-Computern

Die folgenden Schritte zeigen, wie Sie eine optionale DNS-Bezeichnung für Ihren virtuellen Azure-Computer erstellen und anschließend eine Verbindung mit SQL Server Management Studio (SSMS) herstellen.

### <a name="configure-a-dns-label-for-the-public-ip-address"></a>Konfigurieren einer DNS-Bezeichnung für die öffentliche IP-Adresse

Um über das Internet eine Verbindung mit der SQL Server-Datenbank-Engine herzustellen, ziehen Sie die Konfiguration einer DNS-Bezeichnung für Ihre öffentliche IP-Adresse in Betracht. Sie können über eine IP-Adresse beitreten. Die DNS-Bezeichnung erstellt jedoch einen A-Eintrag, der einfacher zu identifizieren ist, und abstrahiert die zugrunde liegende öffentliche IP-Adresse.

> [!NOTE]
> DNS-Bezeichnungen sind nicht erforderlich, wenn Sie nur eine Verbindung mit der SQL Server-Instanz im gleichen virtuellen Netzwerk oder nur eine lokale Verbindung herstellen möchten.

1. Erstellen Sie eine DNS-Bezeichnung, indem Sie im Portal auf die Option **Virtuelle Computer** klicken. Wählen Sie die SQL Server-VM aus, um deren Eigenschaften anzuzeigen.

2. Wählen Sie in der Übersicht für den virtuellen Computer Ihre **Öffentliche IP-Adresse**.

    :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/azure-sql-vm-ip-address-.png" alt-text="Öffentliche IP-Adresse":::

3. Erweitern Sie in den Eigenschaften für die öffentliche IP-Adresse die Option **Konfiguration**.

4. Geben Sie eine DNS-Bezeichnung ein. Hierbei handelt es sich um einen A-Eintrag, mit dem direkt eine Verbindung mit Ihrer SQL Server-VM basierend auf dem Namen statt auf der IP-Adresse hergestellt werden kann.

5. Klicken Sie auf die Schaltfläche **Speichern**.

    :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/azure-sql-vm-dns-label.png" alt-text="DNS-Bezeichnung":::

### <a name="connect"></a>Verbinden

1. Starten Sie SQL Server Management Studio. Beim ersten Ausführen von SSMS wird das Fenster **Connect to Server** (Verbindung mit Server herstellen) geöffnet. Wenn das Fenster nicht geöffnet wird, können Sie es manuell öffnen, indem Sie auf **Objekt-Explorer** > **Verbinden** > **Datenbank-Engine** klicken.

    :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/connect-object-explorer.png" alt-text="Option „Verbinden“ im Objekt-Explorer":::

2. Das Dialogfeld **Mit Server verbinden** wird angezeigt. Geben Sie Folgendes ein:

    |   Einstellung   |   Empfohlene Werte   |   BESCHREIBUNG   |
    |--------------|-----------------------|-----------------|
    | **Servertyp** | Datenbank-Engine | Wählen Sie für **Servertyp** die Option **Datenbank-Engine** (normalerweise die Standardoption) aus. |
    | **Servername** | Der vollqualifizierte Servername | Geben Sie bei **Servername** den Namen Ihrer Azure SQL-VM ein. Sie können auch die IP-Adresse der Azure SQL-VM verwenden, um eine Verbindung herzustellen. | 
    | **Authentifizierung** | SQL Server-Authentifizierung | Verwenden Sie zum Herstellen einer Verbindung für Azure SQL-VMs die **SQL Server-Authentifizierung**. Wenn Sie über ein Azure Active Directory-Umgebungssetup verfügen, können Sie auch jede der Azure Active Directory-Optionen verwenden. </br> </br> Die Methode **Windows-Authentifizierung** wird für Azure SQL-VMs nicht unterstützt. Weitere Informationen finden Sie unter [Übersicht über die Sicherheitsfunktionen von Azure SQL-Datenbank und SQL Managed Instance](/azure/sql-database/sql-database-security-overview#access-management).|
    | **Anmeldung** | Benutzer-ID des Serverkontos | Hier wird die Benutzer-ID des zum Erstellen des Servers verwendeten Serverkontos angegeben. Wenn die **SQL Server-Authentifizierung** verwendet wird, ist ein Anmeldename erforderlich. |
    | **Kennwort** | Kennwort für das Serverkonto | Hier wird das Kennwort für das zum Erstellen des Servers verwendete Serverkonto angegeben. Wenn die **SQL Server-Authentifizierung** verwendet wird, ist ein Kennwort erforderlich. |

    :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/connect-to-azure-sql-vm-object-explorer.png" alt-text="Feld „Servername“ für virtuelle SQL-Computer":::

3. Nachdem Sie alle Felder ausgefüllt haben, klicken Sie auf **Verbinden**.

    Sie können auch zusätzliche Verbindungsoptionen ändern, indem Sie **Optionen** auswählen. Beispiele für Verbindungsoptionen sind die Datenbank, mit der Sie sich verbinden, der Verbindungstimeoutwert und das Netzwerkprotokoll. In diesem Artikel werden die Standardwerte für alle Optionen verwendet.

4. Erweitern Sie im **Objekt-Explorer** den Eintrag mit dem Servernamen, der SQL Server-Version und dem Benutzernamen, und sehen Sie sich die darin enthaltenen Objekte an, um zu überprüfen, ob Ihre SQL Server-Instanz auf einer Azure-VM erfolgreich erstellt wurde. Diese Objekte unterscheiden sich je nach Servertyp.

    :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/connect-azure-sql-vm.png" alt-text="Azure SQL-VM-Verbindung":::

## <a name="troubleshoot-connectivity-issues"></a>Behandeln von Konnektivitätsproblemen

Auch wenn das Portal Optionen zum automatischen Konfigurieren der Konnektivität bereitstellt, ist es hilfreich, zu wissen, wie Sie die Konnektivität manuell konfigurieren. Grundlegende Informationen zu den Anforderungen können auch für die Problembehandlung hilfreich sein.

Die folgende Tabelle enthält eine Liste der Anforderungen für Verbindungen mit SQL Server auf einer Azure-VM.

| Anforderung | BESCHREIBUNG |
|---|---|
| [Aktivieren des SQL Server-Authentifizierungsmodus](../../database-engine/configure-windows/change-server-authentication-mode.md#use-ssms) | Die SQL Server-Authentifizierung ist für eine Remoteverbindung mit dem virtuellen Computer erforderlich, sofern Sie nicht Active Directory in einem virtuellen Netzwerk konfiguriert haben. |
| [Erstellen einer SQL-Anmeldung](../../relational-databases/security/authentication-access/create-a-login.md) | Wenn Sie die SQL-Authentifizierung verwenden, benötigen Sie eine SQL-Anmeldung mit einem Benutzernamen und einem Kennwort, die auch über die erforderlichen Berechtigungen für die Zieldatenbank verfügt. |
| Aktivieren des TCP/IP-Protokolls | SQL Server muss Verbindungen über TCP zulassen. |
| [Aktivieren von Firewallregeln für den SQL Server-Port](../../database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md) | Die Firewall auf dem virtuellen Computer muss eingehenden Datenverkehr am SQL Server-Port (Standardport: 1433) zulassen. |
| [Erstellen einer Netzwerksicherheitsgruppen-Regel für TCP-Port 1433](/azure/virtual-network/manage-network-security-group#create-a-security-rule) | Erlauben Sie eingehenden Datenverkehr an die VM über den SQL Server-Port (Standardport: 1433), wenn Sie eine Verbindung über das Internet herstellen möchten. Bei lokalen Verbindungen und Verbindungen ausschließlich über virtuelle Netzwerke ist dies nicht erforderlich. Dieser Schritt ist nur im Azure-Portal erforderlich. |

> [!TIP]
> Die Schritte in der obigen Tabelle werden für Sie automatisch ausgeführt, wenn Sie die Konnektivität im Portal konfigurieren. Befolgen Sie diese Schritte nur, um Ihre Konfiguration zu bestätigen oder die Konnektivität für SQL Server manuell einzurichten.

## <a name="create-a-database"></a>Erstellen einer Datenbank

Erstellen Sie mithilfe der nachfolgenden Schritte eine Datenbank namens „TutorialDB“:

1. Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf Ihre Serverinstanz und anschließend mit der linken auf **Neue Abfrage**:

   :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/new-query.png" alt-text="Die Verknüpfung „Neue Abfrage“":::

2. Fügen Sie den folgenden T-SQL-Codeausschnitt in das Abfragefenster ein:

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

3. Klicken Sie zum Ausführen der Abfrage auf **Ausführen**, oder drücken Sie F5.

   :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/execute.png" alt-text="Befehl „Ausführen“":::
  
    Nachdem die Abfrage abgeschlossen ist, wird die neue Datenbank „TutorialDB“ in der Datenbankliste im Objekt-Explorer angezeigt. Wenn die Datenbank nicht angezeigt wird, klicken Sie zuerst mit der rechten Maustaste auf den **Datenbankenknoten** und anschließend mit der linken auf **Aktualisieren**.

## <a name="create-a-table-in-the-new-database"></a>Erstellen einer Tabelle in der neuen Datenbank

In diesem Abschnitt erstellen Sie nun eine Tabelle in der neuen Datenbank „TutorialDB“. Da sich der Abfrage-Editor immer noch im Kontext der *Master*-Datenbank befindet, ändern Sie den Verbindungskontext in die *TutorialDB*-Datenbank, indem Sie folgende Schritte ausführen:

1. Wählen Sie in der Dropdownliste die gewünschte Datenbank aus, so wie hier dargestellt:

   :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/change-db.png" alt-text="Ändern der Datenbank":::

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

   :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/new-table.png" alt-text="Neue Tabelle":::

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

   :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/query-results.png" alt-text="Die Ergebnisliste":::

    Sie können auch die Darstellung der angezeigten Ergebnisse ändern, indem Sie eine der folgenden Optionen auswählen:

   ![Drei Optionen zum Anzeigen von Abfrageergebnissen](media/ssms-connect-query-sql-server-azure-vm/results.png)

   - Mit der linken Schaltfläche werden die Ergebnisse in der **Textansicht** dargestellt, wie in der Abbildung im nächsten Abschnitt zu sehen ist.
   - Die mittlere Schaltfläche zeigt die Ergebnisse in der **Rasteransicht**, also in der Standardansicht, an.
       - Dies ist die Standardeinstellung.
   - Mit der dritten Schaltfläche können Sie die Ergebnisse in einer Datei speichern, deren Erweiterung standardmäßig nicht RPT ist.

## <a name="verify-your-connection-properties-by-using-the-query-window-table"></a>Überprüfen Ihrer Verbindungseigenschaften mit der Tabelle im Abfragefenster

Informationen zu Verbindungseigenschaften finden Sie unter den Ergebnissen einer Abfrage. Nachdem Sie die Abfrage aus dem vorherigen Schritt ausgeführt haben, können Sie sich die Verbindungseigenschaften im unteren Bereich des Abfragefensters ansehen.

- Hier wird angezeigt, mit welchem Server und welcher Datenbank Sie verbunden sind. Außerdem ist der von Ihnen verwendete Benutzername zu sehen.
- Des Weiteren sind auch die Abfragedauer und die Anzahl der Zeilen angezeigt, die von der zuvor ausgeführten Abfrage zurückgegeben wurden.

   :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/connection-properties.png" alt-text="Verbindungseigenschaften":::

## <a name="additional-tools"></a>Weitere Tools

Sie können auch [Azure Data Studio](../../azure-data-studio/download-azure-data-studio.md) verwenden, um Verbindungen mit [SQL Server](../../azure-data-studio/quickstart-sql-server.md)-, [Azure SQL-Datenbank](../../azure-data-studio/quickstart-sql-database.md)- und [Azure Synapse Analytics](../../azure-data-studio/quickstart-sql-dw.md)-Instanzen herzustellen und diese abzufragen.

## <a name="next-steps"></a>Nächste Schritte

Am besten machen Sie sich mit SSMS vertraut, indem Sie einige praktische Aufgaben durchführen. Diese Artikel unterstützen Sie bei der Verwendung der verschiedenen Features, die in SSMS verfügbar sind.

- [SSMS-Abfrage-Editor (SQL Server Management Studio)](../f1-help/database-engine-query-editor-sql-server-management-studio.md)
- [Skripterstellung](../tutorials/scripting-ssms.md)
- [Verwenden von Vorlagen in SSMS](../template/templates-ssms.md)
- [SSMS-Konfiguration](../tutorials/ssms-configuration.md)
- [Zusätzliche Tipps und Tricks für die Verwendung von SSMS](../tutorials/ssms-tricks.md)