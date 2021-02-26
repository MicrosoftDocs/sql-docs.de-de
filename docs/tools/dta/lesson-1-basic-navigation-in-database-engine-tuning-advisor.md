---
title: Navigation im DTA
description: Im Datenbankoptimierungsratgeber (Database Engine Tuning Advisor, DTA) werden Optimierungssitzungen und Berichte zu Optimierungsempfehlungen über eine grafische Benutzeroberfläche (graphical user interface, GUI) angezeigt.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Database Engine Tuning Advisor [SQL Server], tutorials
ms.assetid: ad49b2e0-a5e3-49d2-80fd-9f4eaa3652cb
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-dt-2019
ms.date: 03/01/2017
ms.openlocfilehash: e8c4f32eab8fa7d7791a5f4718d8d771cb1dd6cd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354477"
---
# <a name="lesson-1-basic-navigation-in-database-engine-tuning-advisor-dta"></a>Lektion 1: Grundlagen zur Navigation im Datenbankoptimierungsratgeber (DTA)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Im Datenbankoptimierungsratgeber werden Optimierungssitzungen und Berichte zu Optimierungsempfehlungen über eine grafische Benutzeroberfläche angezeigt. In dieser Lektion erfahren Sie, wie Sie das Tool starten und die Anzeige konfigurieren. Am Ende der Lektion kennen Sie verschiedene Möglichkeiten zum Starten des Tools. Sie wissen, wie Sie die Anzeige konfigurieren, sodass die Optimierungsaufgaben unterstützt werden, die Sie regelmäßig ausführen.  

## <a name="prerequisites"></a>Voraussetzungen 

Zur Durchführung dieses Tutorials benötigen Sie SQL Server Management Studio, Zugriff auf einen Server, auf dem SQL-Server ausgeführt wird, und eine AdventureWorks-Datenbank.

- Installieren Sie [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md).
- Installieren Sie die [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads).
- Laden Sie die [AdventureWorks 2017-Beispieldatenbank](../../samples/adventureworks-install-configure.md) herunter.


Anweisungen zum Wiederherstellen von Datenbanken in SSMS finden Sie hier: [Wiederherstellen einer Datenbank](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md).

  >[!NOTE]
  > Dieses Tutorial richtet sich an Benutzer, die mit der Verwendung von SQL Server Management Studio und mit grundlegenden Datenbankverwaltungsaufgaben vertraut sind. 
  

## <a name="launch-database-tuning-advisor"></a>Starten des Datenbankoptimierungsratgebers 
Zunächst öffnen Sie die grafische Benutzeroberfläche des Datenbankoptimierungsratgebers (Database Engine Tuning Advisor, DTA). Bei der ersten Verwendung muss ein Mitglied der festen Serverrolle **sysadmin** den Datenbankoptimierungsratgeber starten, um die Anwendung zu initialisieren. Nach der Initialisierung können Mitglieder der festen Datenbankrolle **db_owner** mit dem Datenbankoptimierungsratgeber eigene Datenbanken optimieren. Weitere Informationen zum Initialisieren des Datenbankoptimierungsratgebers finden Sie unter [Starten und Verwenden des Datenbankoptimierungsratgebers](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md).  
  
1. Starten Sie SQL Server Management Studio (SSMS). Bewegen Sie im **Startmenü** von Windows den Mauszeiger auf **Alle Programme**, und klicken Sie dann auf **SQL Server Management Studio**. 
2. Warten Sie, bis das Programm geöffnet ist. Klicken Sie dann auf das Menü **Extras**, und wählen Sie **Datenbankoptimierungsratgeber** aus. 

  ![Starten von DTA aus SSMS](media/dta-tutorials/launch-dta.png)

3. Der Datenbankoptimierungsratgeber wird gestartet, und das Dialogfeld **Mit Server verbinden** wird geöffnet. Überprüfen Sie die Standardeinstellungen, und klicken Sie dann auf **Verbinden**.  
  
Standardmäßig wird der Datenbankoptimierungsratgeber mit der in der folgenden Abbildung gezeigten Konfiguration geöffnet:  
  
![Datenbankoptimierungsratgeber (Standardfenster)](media/dta-tutorials/dta-default-gui.png)
  
> [!NOTE]  
> Auf der Registerkarte **Sitzungsmonitor** wird der Sitzungsname angezeigt. Er setzt sich aus dem Namen des verbundenen Benutzers sowie aus dem aktuellen Datum und der aktuellen Uhrzeit zusammen. 
  
Beim ersten Öffnen der grafischen Benutzeroberfläche des Datenbankoptimierungsratgebers werden zwei Hauptbereiche angezeigt.  
  
-   Im linken Bereich befindet sich der Sitzungsmonitor, in dem alle Optimierungssitzungen aufgelistet werden, die auf dieser [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz durchgeführt wurden. Wenn Sie den Datenbankoptimierungsratgeber öffnen, wird oben im Anzeigebereich eine neue Sitzung angezeigt. Sie können diese Sitzung im daneben liegenden Bereich benennen. Zu Beginn wird nur eine Standardsitzung aufgeführt. Dies ist die Standardsitzung, die der Datenbankoptimierungsratgeber automatisch für Sie anlegt. Wenn Sie Datenbanken optimiert haben, werden alle Optimierungssitzungen für die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz, mit der Sie verbunden sind, unter der neuen Sitzung aufgelistet. Wenn Sie mit der rechten Maustaste auf eine Optimierungssitzung klicken, können Sie diese umbenennen, schließen, löschen oder klonen. Wenn Sie mit der rechten Maustaste auf die Liste klicken, können Sie die Sitzungen nach Name, Status oder Erstellungszeit sortieren oder aber eine neue Sitzung anlegen. Im unteren Abschnitt des Bereichs werden Detailinformationen zur ausgewählten Optimierungssitzung angezeigt. Mit der Schaltfläche **Nach Kategorien** können Sie festlegen, dass die Details nach Kategorien angezeigt werden sollen. Mit der Schaltfläche **Alphabetisch** können Sie sie in einer alphabetisch sortierten Liste anzeigen. Sie können den Sitzungsmonitor auch ausblenden, indem Sie den rechten Rand des Bereichs auf die linke Seite des Fensters ziehen. Wenn Sie ihn wieder anzeigen möchten, ziehen Sie den Bereichsrand wieder nach rechts. Der Sitzungsmonitor ermöglicht das Anzeigen früherer Optimierungssitzungen, die Sie verwenden können, um neue Sitzungen mit ähnlichen Definitionen zu erstellen. Sie können den Sitzungsmonitor auch einsetzen, um Optimierungsempfehlungen auszuwerten. Weitere Informationen finden Sie unter [Anzeigen und Verwenden der Ausgabe des Datenbankoptimierungsratgebers](../../relational-databases/performance/view-and-work-with-the-output-from-the-database-engine-tuning-advisor.md). Mit der Schaltfläche **Zurück** des Browsers kehren Sie zu diesem Tutorial zurück.  
  
-   Der rechte Bereich umfasst die Registerkarten **Allgemein** und **Optimierungsoptionen** . Hier können Sie die Sitzung im Datenbankoptimierungsratgeber definieren. Auf der Registerkarte **Allgemein** können Sie den Namen für Ihre Optimierungssitzung eingeben, die Arbeitsauslastungsdatei oder Tabelle angeben, die verwendet werden soll, und die Datenbanken und Tabellen auswählen, die in dieser Sitzung optimiert werden sollen. Die Arbeitsauslastung besteht aus einer Reihe von [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisungen, die für eine oder mehrere Datenbanken ausgeführt werden, die Sie optimieren möchten. Beim Optimieren von Datenbanken werden im Datenbankoptimierungsratgeber Ablaufverfolgungsdateien, Ablaufverfolgungstabellen, [!INCLUDE[tsql](../../includes/tsql-md.md)] -Skripts oder XML-Dateien als Eingabe für die Arbeitsauslastung verwendet. Auf der Registerkarte **Optimierungsoptionen** können Sie die physischen Entwurfsstrukturen (Indizes oder indizierte Sichten) und die Partitionierungsstrategie auswählen, die vom Datenbankoptimierungsratgeber bei der Analyse berücksichtigt werden sollen. Auf dieser Registerkarte können Sie auch angeben, wie lange die Optimierung einer Arbeitsauslastung durch den Datenbankoptimierungsratgeber maximal dauern soll. Standardmäßig dauert das Optimieren einer Arbeitsauslastung durch den Datenbankoptimierungsratgeber maximal eine Stunde.  
  
> [!NOTE]
> Der Datenbankoptimierungsratgeber kann auch eine XML-Datei als Eingabe annehmen, wenn ein [!INCLUDE[tsql](../../includes/tsql-md.md)] -Skript aus dem Abfrage-Editor von [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] importiert wird. Weitere Informationen finden Sie im Abschnitt zum Datenbankoptimierungsratgeber im Abfrage-Editor von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] in [Starten und Verwenden des Datenbankoptimierungsratgebers](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md).  
  
## <a name="configure-tool-options-and-layout"></a>Festlegen von Tooloptionen und Layout 

1.  Klicken Sie im Menü **Extras** auf **Optionen**.  

   ![DTA-Optionen](media/dta-tutorials/dta-settings.png) 
  
2.  Verwenden Sie das Dialogfeld **Optionen** , um folgende Optionen anzuzeigen:  
  
    -   Erweitern Sie die Liste **Beim Start** , um festzustellen, welche Anzeigemöglichkeiten der Datenbankoptimierungsratgeber beim Start bietet. Standardmäßig ist **Neue Sitzung anzeigen** ausgewählt.  
  
    -   Klicken Sie auf **Schriftart ändern** , um die Schriftarten anzuzeigen, die Sie für die Listen von Datenbanken und Tabellen auf der Registerkarte **Allgemein** auswählen können. Die Schriftarten, die Sie in dieser Option auswählen, werden auch für die Empfehlungsraster und Berichte des Datenbankoptimierungsratgebers verwendet, die nach dem Optimieren angezeigt werden. Standardmäßig verwendet der Datenbankoptimierungsratgeber die Systemschriftarten.  
  
    -   Für **Anzahl der Elemente in der Liste zuletzt verwendeter Objekte** kann ein Wert von **1** bis **10** festgelegt werden. Damit wird die maximale Anzahl von Elementen in der Liste festgelegt, die angezeigt wird, wenn Sie im Menü **Datei** auf **Zuletzt geöffnete Sitzungen** oder **Zuletzt geöffnete Dateien** klicken. Standardmäßig ist der Wert **4** festgelegt.  
  
    -   Wenn das Kontrollkästchen **Die letzten Optimierungsoptionen speichern** aktiviert ist, verwendet der Datenbankoptimierungsratgeber standardmäßig die Optimierungsoptionen, die Sie bei der letzten Sitzung angegeben haben, auch für die nächste Optimierungssitzung. Deaktivieren Sie das Kontrollkästchen, wenn Sie die Standardeinstellungen für den Datenbankoptimierungsratgeber verwenden möchten. Diese Option ist standardmäßig ausgewählt.  
  
    -   Standardmäßig ist das Kontrollkästchen **Dauerhaftes Löschen von Sitzungen bestätigen** aktiviert, um ein versehentliches Löschen von Optimierungssitzungen zu vermeiden.  
  
    -   Standardmäßig ist **Vor dem Beenden der Sitzungsanalyse fragen** aktiviert, damit nicht eine Optimierungssitzung aus Versehen beendet wird, bevor die Analyse einer Arbeitsauslastung durch den Datenbankoptimierungsratgeber abgeschlossen ist.  
  
## <a name="next-lesson"></a>Nächste Lektion  
[Lektion 2: Verwenden des Datenbankoptimierungsratgebers](../../tools/dta/lesson-2-using-database-engine-tuning-advisor.md)  
  
  
