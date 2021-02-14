---
title: SSMS-Skriptobjekte
description: Erstellen von T-SQL-Skripts (Transact-SQL) für verschiedene Objekte in SQL Server Management Studio (SSMS).
ms.prod: sql
ms.technology: ssms
ms.prod_service: sql-tools
ms.topic: tutorial
keywords: SQL Server, SSMS, SQL Server Management Studio, Skripts, Skripterstellung
helpviewer_keywords:
- projects [SQL Server Management Studio], tutorials
- source controls [SQL Server Management Studio], tutorials
- Help [SQL Server], SQL Server Management Studio
- tutorials [SQL Server Management Studio]
- Transact-SQL tutorials
- solutions [SQL Server Management Studio], tutorials
- SQL Server Management Studio [SQL Server], tutorials
- scripts [SQL Server], SQL Server Management Studio
author: markingmyname
ms.author: maghan
ms.reviewer: sstein
ms.date: 03/13/2018
ms.openlocfilehash: 92f2d7c06116ef9c97fbf03c7849dd8ffef9dbf6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100075871"
---
# <a name="script-objects-in-sql-server-management-studio"></a>Erstellen von Skripts für Objekte in SQL Server Management Studio

In diesem Tutorial erfahren Sie, wie Sie T-SQL-Skripts (Transact-SQL) für verschiedene Objekte in SQL Server Management Studio (SSMS) erstellen können. Dabei wird die Skripterstellung für die folgenden Objekte beschrieben:

> [!div class="checklist"]
> * Abfragen beim Ausführen von Aktionen auf der grafischen Benutzeroberfläche
> * Datenbanken (mithilfe der beiden Methoden „Script Database As“ (Skript für Datenbank erstellen als) und „Skript generieren“)
> * Tabellen
> * Gespeicherte Prozeduren
> * Erweiterte Ereignisse

Zum Erstellen eines Skripts für ein beliebiges Objekt im **Objekt-Explorer** klicken Sie mit der rechten Maustaste auf das Objekt, und wählen Sie die Option **Skript für Objekt erstellen als** aus. Dieses Tutorial zeigt Ihnen die Vorgehensweise.

## <a name="prerequisites"></a>Voraussetzungen

Zur Durchführung dieses Tutorials benötigen Sie SQL Server Management Studio, Zugriff auf einen Server, auf dem SQL-Server ausgeführt wird, und eine AdventureWorks-Datenbank.

* Installieren Sie [SQL Server Management Studio](../download-sql-server-management-studio-ssms.md).
* Installieren Sie die [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads).
* Laden Sie die [AdventureWorks 2016-Beispieldatenbank](https://github.com/Microsoft/sql-server-samples/releases) herunter.

Anweisungen zum Wiederherstellen von Datenbanken in SSMS finden Sie hier: [Restore a Database (Wiederherstellen einer Datenbank)](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md). 

## <a name="script-queries-from-the-gui"></a>Erstellen von Skripts für Abfragen über die grafische Benutzeroberfläche

Sie können den zugeordneten T-SQL-Code für eine Aufgabe generieren, wenn Sie für ihre Ausführung die Benutzeroberfläche in SSMS verwenden. In den folgenden Beispielen wird dies anhand der Sicherung einer Datenbank und der Verkleinerung eines Transaktionsprotokolls demonstriert. Die gleichen Schritte lassen sich auf jede Aktion anwenden, die über die grafische Benutzeroberfläche ausgeführt wird.

### <a name="script-t-sql-when-you-back-up-a-database"></a>Erstellen von T-SQL-Skripts beim Sichern einer Datenbank

1. Stellen Sie eine Verbindung mit einem Server her, der SQL Server ausführt.

2. Erweitern Sie den Knoten **Datenbanken** .

3. Klicken Sie mit der rechten Maustaste auf die Datenbank **AdventureWorks2016** > **Aufgaben** > **Sichern**:

    ![So sichern Sie eine Datenbank](media/scripting-ssms/backupdb.png)

4. Konfigurieren Sie die Sicherung auf die gewünschte Weise. In diesem Tutorial werden die Standardeinstellungen beibehalten. Alle Änderungen, die Sie in diesem Fenster vornehmen, werden jedoch auch im Skript übernommen. 

5. Wählen Sie **Skript** > **Skript für Aktion im Fenster „Neue Abfrage“ schreiben**:

    ![Erstellen von Skripts für Datenbanksicherungen – Skriptaktion](media/scripting-ssms/scriptdbbackup.PNG)
6. Sehen Sie sich den T-SQL-Code im Abfragefenster an.

    ![Erstellen von Skripts für Datenbanksicherungen – T-SQL-Code überprüfen](media/scripting-ssms/dbbackupscript.PNG)
7. Wählen Sie **Ausführen** aus, um die Abfrage auszuführen und ein Backup der Datenbank mithilfe von T-SQL zu erstellen. 

### <a name="script-t-sql-when-you-shrink-the-transaction-log"></a>T-SQL-Skripts beim Verkleinern des Transaktionsprotokolls

1. Klicken Sie mit der rechten Maustaste auf die Datenbank **AdventureWorks2016** > **Aufgaben** > **Verkleinern** > **Dateien**:

     ![Verkleinern von Dateien](media/scripting-ssms/shrinkfiles.png)

2. Wählen Sie im Dropdown-Listenfeld **Dateityp** **Protokoll** aus:

    ![Verkleinern des Transaktionsprotokolls](media/scripting-ssms/shrinktlog.png)

3. Wählen Sie **Skript** und **Skriptaktion in Zwischenablage** aus:

    ![Skript in Zwischenablage schreiben](media/scripting-ssms/scriptactiontoclipboard.png)

4. Öffnen Sie ein **Neues Abfragefenster**, und fügen Sie ein. (Klicken Sie mit der rechten Maustaste auf das Fenster. Wählen Sie dann **Einfügen** aus.)

    ![Einfügen des Skripts](media/scripting-ssms/paste.png)

5. Klicken Sie auf **Ausführen**, um die Abfrage auszuführen und das Transaktionsprotokoll zu schließen.

## <a name="script-databases"></a>Erstellen von Skripts für Datenbanken

Im folgenden Abschnitt erfahren Sie, wie Sie mit den Optionen **Script As** (Skript erstellen als) und **Skripts generieren** Skripts für eine Datenbank erstellen. Mithilfe der Option **Script Database As** (Skript für Datenbank erstellen als) werden die Datenbank und die zugehörigen Konfigurationsoptionen neu erstellt. Mithilfe der Option **Skripts generieren** können Sie Skripts sowohl für das Schema als auch für die Daten erstellen. In diesem Abschnitt erstellen Sie zwei neue Datenbanken. Sie verwenden die Option **Script As** (Skript erstellen als), um *AdventureWorks2016a* zu erstellen. Sie verwenden die Option **Skripts generieren**, um *AdventureWorks2016b* zu erstellen.

### <a name="script-a-database-by-using-the-script-option"></a>Erstellen Sie mithilfe der Option „Skript“ ein Skript für eine Datenbank.

1. Stellen Sie eine Verbindung mit einem Server her, der SQL Server ausführt.

2. Erweitern Sie den Knoten **Datenbanken** .

3. Klicken Sie mit der rechten Maustaste auf die Datenbank **AdventureWorks2016**-Datenbank, wählen Sie dann  > **Script Database As** > **Create To** > **Neues Abfrage-Editor-Fenster** (Skript für Datenbank erstellen als > Erstellen in) aus:

    ![Skripterstellung für Datenbanken](media/scripting-ssms/scriptdb.png)

4. Sehen Sie sich die Datenbankerstellungsabfrage im Fenster an:

    ![Scripted-out database](media/scripting-ssms/scriptedoutdb.png) (Datenbank-Ausgabeskript) Mit dieser Option werden nur die Datenbankkonfigurationsoptionen ausgegeben.

5. Wählen Sie auf der Tastatur STRG+F aus, um das Dialogfeld **Suchen** zu öffnen. Wählen Sie den Pfeil nach unten aus, um die Option **Ersetzen** zu öffnen. Geben Sie oben in der **Suchzeile** „AdventureWorks2016“ ein, und geben Sie unten unter **Ersetzen** „AdventureWorks2016a“ ein.

6. Wählen Sie **Alle ersetzen** aus, um alle Instanzen von *AdventureWorks2016* durch *AdventureWorks2016a* zu ersetzen. 

    ![Suchen und Ersetzen](media/scripting-ssms/findandreplace.png)

7. Wählen Sie **Ausführen** aus, um die Abfrage auszuführen und Ihre neue AdventureWorks2016a-Datenbank zu erstellen. 

### <a name="script-a-database-by-using-the-generate-scripts-option"></a>Erstellen Sie mithilfe der Option „Skript generieren“ ein Skript für eine Datenbank.

1. Stellen Sie eine Verbindung mit einem Server her, der SQL Server ausführt.

2. Erweitern Sie den Knoten **Datenbanken** .

3. Klicken Sie mit der rechten Maustaste auf **AdventureWorks2016** > **Aufgaben** > **Skripts generieren**:

    ![Generieren von Skripts für Datenbanken](media/scripting-ssms/generatescriptsfordb.png)

4. Die Seite **Einführung** wird geöffnet. Wählen Sie **Weiter** aus, um die Seite **Objekte auswählen** zu öffnen. Sie können die gesamte Datenbank oder nur bestimmte Objekte in der Datenbank auswählen. Wählen Sie **Skripterstellung für gesamte Datenbank und alle Datenbankobjekte** aus.

    ![Generieren von Skripts für Objekte](media/scripting-ssms/scriptobjects.png)

5. Wählen Sie **Weiter** aus, um die Seite **Skripterstellungsoptionen festlegen** zu öffnen. Hier können Sie den Speicherort des Skripts und einige zusätzliche erweiterte Optionen konfigurieren. 

    a. Wählen Sie **In Fenster 'Neue Abfrage' speichern** aus.

    b. Wählen Sie **Erweitert** aus, und vergewissern Sie sich, dass die folgenden Optionen festgelegt sind:

      * **Skripterstellung für Statistiken** ist auf *Skripterstellung für Statistiken* festgelegt.
      * **Datentypen, für die ein Skript erstellt wird** ist auf *Nur Schema* festgelegt.
      * **Skripterstellung für Indizes** ist auf *True* festgelegt.

   ![Erstellen von Skripts für Objekte](media/scripting-ssms/advancedscripts.png)

   > [!NOTE]
   > Sie können für die Datenbank Skripts für die Daten erstellen, indem Sie für die Option **Datentypen, für die ein Skript erstellt wird***Schema und Daten* auswählen. Allerdings ist dies bei großen Datenbanken nicht ideal. Möglicherweise ist mehr Arbeitsspeicher erforderlich, als SSMS zuordnen kann. Diese Einschränkung ist für kleine Datenbanken aber unproblematisch. Wenn Sie Daten für eine größere Datenbank verschieben möchten, verwenden Sie den [Import- und Export-Assistenten](../../integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard.md).

6. Wählen Sie **OK** und anschließend **Weiter** aus.

7. Wählen Sie in der **Zusammenfassung** **Weiter** aus. Wählen Sie dann erneut **Weiter** aus, um das Skript in einem Fenster **Neue Abfrage** zu erstellen.

8. Öffnen Sie über die Tastatur das Dialogfeld **Suche** (STRG+F). Wählen Sie den Pfeil nach unten aus, um die Option **Ersetzen** zu öffnen. Geben Sie in der oberen **Suchen**-Zeile *AdventureWorks2016* ein. Geben Sie in der unteren **Ersetzen**-Zeile *AdventureWorks2016b* ein.

9. Wählen Sie **Alle ersetzen** aus, um alle Instanzen von *AdventureWorks2016* durch *AdventureWorks2016b* zu ersetzen.

    ![AdventureWorks 2016](media/scripting-ssms/adventureworks2016b.png)

10. Wählen Sie **Ausführen** aus, um die Abfrage auszuführen und Ihre neue AdventureWorks2016b-Datenbank zu erstellen.

## <a name="script-tables"></a>Erstellen von Skripts für Tabellen

In diesem Abschnitt wird beschrieben, wie Sie Skripts für Datenbanktabellen erstellen. Mithilfe dieser Option können Sie entweder die Tabelle erstellen oder die Tabelle löschen und neu erstellen. Sie können diese Option auch verwenden, um ein Skript für den T-SQL-Code zu erstellen, der im Zusammenhang mit der Bearbeitung der Tabelle steht. Ein Beispiel sind Einfügungen in die Tabelle oder Aktualisierungen der Tabelle. In diesem Abschnitt löschen Sie eine Tabelle und erstellen diese dann neu. 

1. Stellen Sie eine Verbindung mit einem Server her, der SQL Server ausführt.

2. Erweitern Sie den Knoten **Datenbanken**.

3. Klappen Sie den Datenbankknoten **AdventureWorks2016** auf. 

4. Erweitern Sie den Knoten **Tabellen**.

5. Klicken Sie mit der rechten Maustaste auf **dbo.ErrorLog** > **Skript für Tabelle als** > **DROP And CREATE To** > **Neues Abfrage-Editor-Fenster** (LÖSCHEN und NEU ERSTELLEN in):

    ![Skripttabelle](media/scripting-ssms/scripttable.png)

6. Wählen Sie **Ausführen** aus, um die Abfrage auszuführen. Diese Aktion löscht die *Errorlog*-Tabelle und erstellt sie neu. 

    >[!NOTE]
    > Die *Errorlog*-Tabelle ist in der AdventureWorks2016-Datenbank standardmäßig leer. Daher verlieren Sie durch das Löschen der Tabelle keine Daten. Dieselbe Vorgehensweise führt jedoch bei einer Tabelle mit Daten zu Datenverlust.

## <a name="script-stored-procedures"></a>Erstellen von Skripts für gespeicherte Prozeduren

In diesem Abschnitt erfahren Sie, wie Sie eine gespeicherte Prozedur löschen und an anderer Stelle erstellen.  

1. Stellen Sie eine Verbindung mit einem Server her, der SQL Server ausführt.

2. Erweitern Sie den Knoten **Datenbanken**.

3. Erweitern Sie den Knoten **Programmierbarkeit**. 

4. Erweitern Sie den Knoten **Gespeicherte Prozedur**.

5. Klicken Sie mit der rechten Maustaste auf die gespeicherte Prozedur **dbo.uspGetBillOfMaterials** > **Skript für gespeicherte Prozeduren als** > **DROP and CREATE To** > **Neues Abfrage-Editor-Fenster** (LÖSCHEN und NEU ERSTELLEN in):

    ![Erstellen von Skripts für gespeicherte Prozeduren](media/scripting-ssms/script-stored-procedure.png)

## <a name="script-extended-events"></a>Erstellen von Skripts für erweiterte Ereignisse

In diesem Abschnitt wird beschrieben, wie Sie ein Skript für [erweiterte Ereignisse](../../relational-databases/extended-events/extended-events.md) erstellen.

1. Stellen Sie eine Verbindung mit einem Server her, der SQL Server ausführt.

2. Erweitern Sie den Knoten **Verwaltung**.

3. Erweitern Sie den Knoten **Erweiterte Ereignisse**.

4. Erweitern Sie den Knoten **Sitzungen**.

5. Klicken Sie mit der rechten Maustaste zunächst auf die erweiterte Sitzung, die Sie interessiert, und dann auf **Script Session As** > **CREATE To** > **Neues Abfrage-Editor-Fenster** (Skript für Sitzung als > CREATE in):

    ![Erweitertes neues Abfrage-Editorfenster-Sitzung](media/scripting-ssms/scriptxevents.png)

6. Ändern Sie unter **Neues Abfrage-Editor-Fenster** den neuen Namen der Sitzung von *system_health* in *system_health2*. Wählen Sie **Ausführen** aus, um die Abfrage auszuführen.

7. Klicken Sie mit der rechten Maustaste im **Objekt-Explorer** auf **Sitzungen**. Wählen Sie **Aktualisieren** aus, um Ihre neue erweiterte Ereignissitzung anzuzeigen. Das grüne Symbol neben der Sitzung zeigt an, dass die Sitzung ausgeführt wird. Das rote Symbol zeigt an, dass die Sitzung beendet wurde.

    ![Neue erweiterte Ereignissitzung](media/scripting-ssms/newxevent.png)

    >[!NOTE]
    > Sie können die Sitzung starten, indem Sie mit der rechten Maustaste darauf klicken und **Start** (Starten) auswählen. Es handelt sich allerdings hier um eine Kopie der bereits laufenden **system_health**-Sitzung, daher können Sie diesen Schritt überspringen. Sie können die Kopie der erweiterten Ereignissitzung löschen, indem Sie mit der rechten Maustaste darauf klicken, und **Löschen** auswählen.

## <a name="next-steps"></a>Nächste Schritte

Am besten machen Sie sich mit SSMS vertraut, indem Sie einige praktische Aufgaben durchführen. Dieses *Tutorial* und Artikel zu *Vorgehensweisen* unterstützen Sie bei der Verwendung der verschiedenen Features, die in SSMS verfügbar sind. In diesen Artikeln erfahren Sie, wie Sie die Komponenten von SSMS verwalten und wie Sie die Funktionen finden, die Sie regelmäßig verwenden.

* [Herstellen einer Verbindung mit einer Instanz und Abfragen einer Instanz](../quickstarts/ssms-connect-query-sql-server.md)
* [Verwenden von Vorlagen in SSMS](../template/templates-ssms.md)
* [SSMS-Konfiguration](ssms-configuration.md)
* [Zusätzliche Tipps und Tricks für die Verwendung von SSMS](ssms-tricks.md)