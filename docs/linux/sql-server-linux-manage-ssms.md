---
title: Verwenden von SSMS zum Verwalten von SQL Server für Linux
description: In diesem Artikel wird SQL Server Management Studio eingeführt, eine integrierte Umgebung, in der Sie auf SQL Server-Komponenten zugreifen sowie diese konfigurieren, verwalten und entwickeln können.
author: VanMSFT
ms.author: vanto
ms.date: 05/21/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: b2fcf858-21c3-462a-8d49-50c85647d092
ms.openlocfilehash: 758c4e8165efd9fd24a979bfd3a1390e88b765ce
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345638"
---
# <a name="use-sql-server-management-studio-on-windows-to-manage-sql-server-on-linux"></a>Verwenden von SQL Server Management Studio unter Windows zum Verwalten von SQL Server für Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

In diesem Artikel wird [SQL Server Management Studio (SSMS)](../ssms/sql-server-management-studio-ssms.md) vorgestellt, und Sie werden schrittweise durch einige häufige Aufgaben geführt. SSMS ist eine Windows-Anwendung und kann daher auf einem Windows-Computer ausgeführt werden, der eine Verbindung mit einer SQL Server-Remoteinstanz unter Linux herstellen kann.

> [!TIP]
> Wenn Sie nicht über einen Windows-Computer verfügen, auf dem SSMS ausgeführt werden kann, sollten Sie die Verwendung des neuen Tools [Azure Data Studio](../azure-data-studio/index.yml) in Erwägung ziehen. Es bietet ein grafisches Tool zum Verwalten von SQL Server und kann sowohl unter Linux als auch unter Windows ausgeführt werden.

[SQL Server Management Studio (SSMS)](../ssms/sql-server-management-studio-ssms.md) ist Teil einer Suite von SQL-Tools, die Microsoft kostenlos für Ihre Entwicklungs- und Verwaltungsanforderungen anbietet. SSMS ist eine integrierte Umgebung, in der Sie auf alle SQL Server-Komponenten zugreifen sowie diese konfigurieren, verwalten und entwickeln können. Sie können damit eine Verbindung mit SQL Server herstellen, unabhängig davon, ob SQL Server auf einer lokalen Plattform in Docker-Containern oder in der Cloud ausgeführt wird. Außerdem kann eine Verbindung mit Azure SQL-Datenbank und Azure Synapse Analytics hergestellt werden. SSMS kombiniert eine Vielzahl grafischer Tools mit mehreren umfassenden Skript-Editoren, um Entwicklern und Administratoren mit unterschiedlichem Kenntnisstand den Zugriff auf SQL Server zu ermöglichen.

SSMS bietet auch eine Vielzahl von Entwicklungs-und Verwaltungsfunktionen für SQL Server, einschließlich Tools für folgende Aufgaben:

- Konfigurieren, Überwachen und Verwalten von einzelnen oder mehreren SQL Server-Instanzen
- Bereitstellen, Überwachen und Aktualisieren von Datenebenenkomponenten wie Datenbanken und Data Warehouse-Datenbanken
- Sichern und Wiederherstellen von Datenbanken
- Erstellen und Ausführen von T-SQL-Abfragen und -Skripts sowie Anzeigen von Ergebnissen
- Generieren von T-SQL-Skripts für Datenbankobjekte
- Anzeigen und Bearbeiten von Daten in Datenbanken
- Visuelles Entwerfen von T-SQL-Abfragen und Datenbankobjekten wie Ansichten, Tabellen und gespeicherten Prozeduren

Weitere Informationen zu SSMS finden Sie unter [Was ist SQL Server Management Studio (SSMS)?](../ssms/sql-server-management-studio-ssms.md).

## <a name="install-the-newest-version-of-sql-server-management-studio-ssms"></a>Installieren der neuesten Version von SQL Server Management Studio (SSMS)

Wenn Sie mit SQL Server arbeiten, sollten Sie immer die neueste Version von SQL Server Management Studio (SSMS) verwenden. Die neueste Version von SSMS wird ständig aktualisiert und optimiert und funktioniert derzeit mit SQL Server für Linux. Die aktuellste Version können Sie unter [Herunterladen von SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md) herunterladen und anschließend installieren. Immer, wenn eine neue Version verfügbar ist, werden Sie in der aktuellsten Version von SSMS zum Herunterladen aufgefordert, um auf dem neuesten Stand zu bleiben.

> [!NOTE]
> Lesen Sie vor der Verwendung von SSMS zum Verwalten von Linux die [bekannten Probleme](sql-server-linux-release-notes.md) zu SSMS für Linux.

## <a name="connect-to-sql-server-on-linux"></a>Herstellen einer Verbindung mit SQL Server für Linux

Führen Sie zum Herstellen einer Verbindung die folgenden grundlegenden Schritte aus:

1. Starten Sie SSMS, indem Sie **Microsoft SQL Server Management Studio** in das Windows-Suchfeld eingeben, und klicken Sie dann auf die Desktop-App.

    ![SQL Server Management Studio](./media/sql-server-linux-manage-ssms/ssms.png)

1. Geben Sie im Fenster **Verbindung mit Server herstellen** die folgenden Informationen ein (wenn SSMS bereits ausgeführt wird, klicken Sie auf **Verbinden > Datenbank-Engine**, um das Fenster **Verbindung mit Server herstellen** zu öffnen):

   | Einstellung | BESCHREIBUNG |
   |-----|-----|
   | **Servertyp** | Der Standardwert ist die Datenbank-Engine. Ändern Sie diesen Wert nicht. |
   | **Servername** | Geben Sie den Namen des Linux-Zielcomputers für SQL Server oder dessen IP-Adresse und Port im Format `IP,port` ein. |
   | **Authentifizierung** | Verwenden Sie für SQL Server für Linux **SQL Server-Authentifizierung**. |
   | **Anmeldung** | Geben Sie den Namen eines Benutzers ein, der Zugriff auf eine Datenbank auf dem Server hat (z. B. das standardmäßig beim Setup erstellte **SA**-Konto). |
   | **Kennwort** | Geben Sie das Kennwort für den angegebenen Benutzer ein (für das **SA**-Konto haben Sie dieses während des Setups erstellt). |

    ![SQL Server Management Studio: Herstellen einer Verbindung mit dem SQL-Datenbank-Server](./media/sql-server-linux-manage-ssms/connect.png)

1. Klicken Sie auf **Verbinden**.

    > [!TIP]
    > Wenn Sie einen Verbindungsfehler erhalten, versuchen Sie zunächst das Problem aus der Fehlermeldung zu ermitteln. Überprüfen Sie anschließend die [Empfehlungen zur Verbindungsproblembehandlung](sql-server-linux-troubleshooting-guide.md#connection).
 
1. Nachdem Sie erfolgreich eine Verbindung mit Ihrem SQL Server hergestellt haben, wird der **Objekt-Explorer** geöffnet, und Sie können nun auf Ihre Datenbank zugreifen, um administrative Aufgaben auszuführen oder Daten abzufragen.

## <a name="run-transact-sql-queries"></a>Ausführen von Transact-SQL-Abfragen

Nachdem Sie eine Verbindung mit Ihrem Server hergestellt haben, können Sie eine Verbindung mit einer Datenbank herstellen und Transact-SQL-Abfragen ausführen. Transact-SQL-Abfragen können für nahezu jede beliebige Datenbankaufgabe verwendet werden.

1. Navigieren Sie im **Objekt-Explorer** zur Zieldatenbank auf dem Server. Erweitern Sie z. B. **Systemdatenbanken**, um mit der **Master**-Datenbank zu arbeiten.

1. Klicken Sie mit der rechten Maustaste auf die Datenbank, und wählen Sie **Neue Abfrage** aus.

1. Schreiben Sie im Abfragefenster eine Transact-SQL-Abfrage, um die Namen aller Datenbanken auf Ihrem Server zurückzugeben.

   ```sql
   SELECT [Name]
   FROM sys.Databases
   ```

   Wenn Sie mit dem Schreiben von Abfragen noch nicht vertraut sind, lesen Sie den Artikel [Schreiben von Transact-SQL-Anweisungen](../t-sql/tutorial-writing-transact-sql-statements.md).

1. Klicken Sie auf die Schaltfläche **Ausführen**, um die Abfrage auszuführen und die Ergebnisse anzuzeigen.

   ![Erfolg. Herstellen einer Verbindung mit dem SQL-Datenbank-Server: SQL Server Management Studio](./media/sql-server-linux-manage-ssms/execute-query.png)

Obwohl es möglich ist, fast jede beliebige Verwaltungsaufgabe mit Transact-SQL-Abfragen auszuführen, ist SSMS ein grafisches Tool, das die Verwaltung von SQL Server erleichtert. In den folgenden Abschnitten finden Sie einige Beispiele für die Verwendung der grafischen Benutzeroberfläche.

## <a name="create-and-manage-databases"></a>Erstellen und Verwalten von Datenbanken

Nachdem eine Verbindung mit der *Masterdatenbank* hergestellt wurde, können Sie Datenbanken auf dem Server erstellen und vorhandene Datenbanken ändern oder löschen. In den folgenden Schritten wird beschrieben, wie Sie über Management Studio mehrere allgemeine Datenbankverwaltungsaufgaben ausführen können. Stellen Sie sicher, dass Sie zum Ausführen dieser Aufgaben mit der Serverebenenprinzipal-Anmeldung, die Sie beim Einrichten von SQL Server für Linux erstellt haben, mit der *Masterdatenbank* verbunden sind.

### <a name="create-a-new-database"></a>Erstellen einer neuen Datenbank

1. Starten Sie SSMS, und stellen Sie eine Verbindung mit Ihrem Server in SQL Server für Linux her.

2. Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf den Ordner *Datenbanken*, und klicken Sie dann auf *Neue Datenbank...*.

3. Geben Sie im Dialogfeld *Neue Datenbank* einen Namen für die neue Datenbank ein, und klicken Sie dann auf *OK*.

Die neue Datenbank wurde erfolgreich auf Ihrem Server erstellt. Wenn Sie die Erstellung einer neuen Datenbank mit T-SQL bevorzugen, finden Sie weitere Informationen dazu unter [CREATE DATABASE (SQL Server Transact-SQL)](../t-sql/statements/create-database-transact-sql.md).

### <a name="drop-a-database"></a>Löschen einer Datenbank

1. Starten Sie SSMS, und stellen Sie eine Verbindung mit Ihrem Server in SQL Server für Linux her.

2. Erweitern Sie im Objekt-Explorer den Ordner *Datenbanken*, um eine Liste aller Datenbanken auf dem Server anzuzeigen.

3. Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf die Datenbank, die Sie löschen möchten, und klicken Sie dann auf *Löschen*.

4. Aktivieren Sie im Dialogfeld *Objekt löschen* die Option *Bestehende Verbindungen schließen*, und klicken Sie dann auf *OK*.

Die Datenbank wurde erfolgreich von Ihrem Server gelöscht. Wenn Sie die Löschung einer Datenbank mit T-SQL bevorzugen, finden Sie weitere Informationen dazu unter [DROP DATABASE (SQL Server Transact-SQL)](../t-sql/statements/drop-database-transact-sql.md).

## <a name="use-activity-monitor-to-see-information-about-sql-server-activity"></a>Verwenden des Aktivitätsmonitors zum Anzeigen von Informationen zur SQL Server-Aktivität

Das [Aktivitätsmonitor](../relational-databases/performance-monitor/activity-monitor.md)-Tool ist in SQL Server Management Studio (SSMS) integriert. Es zeigt Informationen zu SQL Server-Prozessen an, und erläutert, wie sich diese Prozesse auf die aktuelle Instanz von SQL Server auswirken.

1. Starten Sie SSMS, und stellen Sie eine Verbindung mit Ihrem Server in SQL Server für Linux her.

1. Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf den *Server*-Knoten, und klicken Sie dann auf *Aktivitätsmonitor*.

Der Aktivitätsmonitor zeigt erweiterbare und reduzierbare Bereiche mit den folgenden Informationen an:

- Übersicht
- Prozesse
- Ressourcenwartevorgänge
- Datendatei-E/A
- Letzte ressourcenintensive Abfragen
- Aktive ressourcenintensive Abfragen

Wenn ein Bereich erweitert wird, fragt der Aktivitätsmonitor die Instanz nach Informationen ab. Wenn ein Bereich reduziert wird, werden sämtliche Abfrageaktivitäten für diesen Bereich angehalten. Sie können einen oder mehrere Bereiche gleichzeitig erweitern, um unterschiedliche Aktivitätstypen für die Instanz anzuzeigen.

## <a name="see-also"></a>Weitere Informationen
- [Was ist SSMS?](../ssms/sql-server-management-studio-ssms.md)
- [Export and Import a database with SSMS (Exportieren und Importieren einer Datenbank mit SSMS)](sql-server-linux-migrate-ssms.md)
- [Tutorial: SQL Server Management Studio](../ssms/quickstarts/ssms-connect-query-sql-server.md)
- [Tutorial: Schreiben von Transact-SQL-Anweisungen](../t-sql/tutorial-writing-transact-sql-statements.md)
- [Überwachen der Serverleistung und -aktivität](../relational-databases/performance/server-performance-and-activity-monitoring.md)