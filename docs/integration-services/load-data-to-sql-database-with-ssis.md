---
title: Laden von Daten in SQL Server oder Azure SQL-Datenbank mit SQL Server Integration Services (SSIS) | Microsoft-Dokumentation
description: In diesem Artikel wird erläutert, wie ein SQL Server Integration Services-Paket (SSIS) erstellt wird, um Daten aus einer Vielzahl von Datenquellen in SQL Server oder Azure SQL-Datenbank zu verlagern.
documentationcenter: NA
ms.prod: sql
ms.prod_service: integration-services
ms.technology: integration-services
ms.topic: conceptual
ms.custom: loading
ms.date: 08/20/2018
ms.author: chugu
author: chugugrace
ms.openlocfilehash: 4bdd992e4cea1f2c869f3669905709f91727c874
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100074185"
---
# <a name="load-data-into-sql-server-or-azure-sql-database-with-sql-server-integration-services-ssis"></a>Laden von Daten in SQL Server oder Azure SQL-Datenbank mit SQL Server Integration Services (SSIS)

[!INCLUDE[sqlserver-ssis](../includes/ssis-appliesto-ssvrpluslinux-asdb-xxxx-xxx.md)]

Erstellen Sie ein SQL Server Integration Services-Paket (SSIS), um Daten in SQL Server oder [Azure SQL-Datenbank](/azure/sql-database/) zu verlagern. Sie können die Daten optional umstrukturieren, transformieren und bereinigen, während diese den SSIS-Datenfluss durchlaufen.

Dieser Artikel enthält Anleitungen für folgende Aktionen:

* Erstellen Sie ein neues Integration Services-Projekt in Visual Studio.
* Entwerfen Sie ein SSIS-Paket, das Daten aus der Quelle in das Ziel lädt.
* Führen Sie das SSIS-Paket aus, um die Daten zu laden.

## <a name="basic-concepts"></a>Grundlegende Konzepte

Ein Paket ist die grundlegende Bereitstellungseinheit in SSIS. Zugehörige Pakete werden in Projekten gruppiert. Sie erstellen Projekte und entwerfen Pakete in Visual Studio mit SQL Server Data Tools. Der Entwurfsprozess ist ein visueller Prozess, bei dem Sie Komponenten per Drag & Drop aus der Toolbox auf die Entwurfsoberfläche ziehen, diese verbinden und ihre Eigenschaften festlegen. Wenn Sie ein Paket fertiggestellt haben, können Sie es ausführen und zur umfassenden Verwaltung, Überwachung und Sicherheit optional in SQL Server oder SQL-Datenbank bereitstellen.

Eine ausführliche Einführung in SSIS würde den Rahmen dieses Artikels sprengen. Weitere Informationen erhalten Sie in den folgenden Artikeln:

- [SQL Server Integration Services](sql-server-integration-services.md)

- [Erstellen eines einfachen ETL-Pakets](ssis-how-to-create-an-etl-package.md)

## <a name="about-the-solution"></a>Informationen zur Projektmappe

Die Projektmappe besteht aus einem normalen Paket, das einen Datenflusstask verwendet, der eine Quelle und ein Ziel enthält. Durch diesen Ansatz werden eine Vielzahl von Datenquellen unterstützt, einschließlich SQL Server und Azure SQL-Datenbank.

In diesem Tutorial wird SQL Server als Datenquelle verwendet. SQL Server wird auf einem lokalen Computer oder auf einem virtuellen Azure-Computer ausgeführt.

Um eine Verbindung zu SQL Server und SQL-Datenbank herzustellen, können Sie den ADO.NET-Verbindungs-Manager und dazu Quelle und Ziel verwenden. Alternativ können Sie den OLE DB-Verbindungs-Manager mit Quelle und Ziel verwenden. Dieses Tutorial verwendet ADO NET, da darin die wenigsten Konfigurationsoptionen enthalten sind. OLE DB stellt möglicherweise eine geringfügig bessere Leistung als ADO .NET bereit.

Um das Verfahren abzukürzen, können Sie den SQL Server-Import/Export-Assistenten verwenden, um das einfache Paket zu erstellen. Speichern Sie das Paket anschließend, und öffnen Sie es in Visual Studio oder SSDT, um es anzuzeigen und anzupassen. Weitere Informationen finden Sie unter [Importieren und Exportieren von Daten mit dem SQL Server-Import/Export-Assistenten](import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard.md).

## <a name="prerequisites"></a>Voraussetzungen
Zum Abschließen dieses Tutorials benötigen Sie Folgendes:

1. **SQL Server Integration Services (SSIS)** . SSIS ist eine Komponente von SQL Server und erfordert eine lizenzierte Version oder die Entwickler- oder Evaluierungsversion von SQL Server. Informationen darüber, wie Sie eine Evaluierungsversion von SQL Server erhalten, finden Sie im [Evaluation Center für SQL Server](https://www.microsoft.com/evalcenter/evaluate-sql-server-2017-rtm).
2. **Visual Studio** (optional). Visual Studio Community Edition können Sie kostenlos unter [Visual Studio Community][Visual Studio Community] abrufen. Wenn Sie Visual Studio nicht installieren möchten, können Sie auch nur SQL Server Data Tools (SSDT) installieren. Mit SSDT wird auch eine Version von Visual Studio mit eingeschränkter Funktionalität installiert.
3. **SQL Server Data Tools for Visual Studio (SSDT)** . Informationen zum Installieren von SQL Server Data Tools für Visual Studio finden Sie unter [Herunterladen von SQL Server Data Tools (SSDT)][Download SQL Server Data Tools (SSDT)].
4. In diesem Tutorial stellen Sie eine Verbindung mit einer Instanz von SQL Server oder Azure SQL-Datenbank her und laden Daten in diese. Sie benötigen Berechtigungen zum Herstellen einer Verbindung, zum Erstellen einer Tabelle und zum Laden von Daten in eines der folgenden Ziele:
   - **Eine Azure SQL-Datenbank-Instanz**. Weitere Informationen finden Sie unter [Azure SQL-Datenbank](/azure/sql-database/).  
      oder
   - **Eine SQL Server-Instanz**. SQL Server wird auf einem lokalen Computer oder auf einem virtuellen Azure-Computer ausgeführt. Informationen zum Herunterladen einer kostenlosen Evaluierungs- oder Entwicklerversion von SQL Server finden Sie unter [SQL Server-Downloads](https://www.microsoft.com/sql-server/sql-server-downloads).

5. **Beispieldaten**. Dieses Tutorial verwendet als Quelldaten zum Laden in SQL Server Beispieldaten, die in der AdventureWorks-Beispieldatenbank gespeichert sind. Die AdventureWorks-Beispieldatenbank können Sie unter [AdventureWorks-Beispieldatenbanken][AdventureWorks 2014 Sample Databases] abrufen.
6. **Eine Firewallregel**, wenn Sie Daten in die Azure SQL-Datenbank-Instanz laden. Sie müssen eine Firewallregel für SQL-Datenbank mit der IP-Adresse Ihres lokalen Computers erstellen, bevor Sie Daten in SQL-Datenbank hochladen können.

## <a name="create-a-new-integration-services-project"></a>Erstellen eines neuen SQL Server Integration Services-Projekts
1. Starten Sie Visual Studio.
2. Wählen Sie im Menü **Datei** die Option **Neu | Projekt** aus.
3. Navigieren Sie zu den Projekttypen **Installiert | Vorlagen | Business Intelligence | Integration Services**.
4. Wählen Sie ein **Integration Services-Projekt** aus. Geben Sie Werte für **Name** und **Speicherort** ein, und klicken Sie dann auf **OK**.

Visual Studio wird geöffnet und erstellt ein neues Integration Services-Projekt (SSIS). Dann öffnet Visual Studio den Designer für das neue SSIS-Paket (Package.dtsx) im Projekt. Die folgenden Bildschirmbereiche werden angezeigt:

* Auf der linken Seite befindet sich die **Toolbox** der SSIS-Komponenten.
* In der Mitte befindet sich die Entwurfsoberfläche mit mehreren Registerkarten. In der Regel verwenden Sie mindestens die Registerkarten **Ablaufsteuerung** und **Datenfluss**.
* Auf der rechten Seite befinden sich die Bereiche **Projektmappen-Explorer** und **Eigenschaften**.
  
    ![Screenshot von Visual Studio mit dem Toolboxbereich, dem Entwurfsbereich, dem Projektmappen-Explorer-Bereich und dem Eigenschaftenbereich.][01]

## <a name="create-the-basic-data-flow"></a>Erstellen des grundlegenden Datenflusses
1. Ziehen Sie einen Datenflusstask aus der Toolbox in die Mitte der Entwurfsoberfläche (auf der Registerkarte **Ablaufsteuerung**).
   
    ![Screenshot von Visual Studio mit einem Datenflusstask, der auf die Registerkarte „Ablaufsteuerung“ des Entwurfsbereichs gezogen wird.][02]
2. Doppelklicken Sie auf den Datenflusstask, um zur Registerkarte „Datenfluss“ zu wechseln.
3. Ziehen Sie aus der Liste „Andere Quellen“ in der Toolbox eine ADO.NET-Quelle auf die Entwurfsoberfläche. Ändern Sie mit noch ausgewähltem Quelladapter den Namen im Bereich **Eigenschaften** in **SQL Server-Quelle**.
4. Ziehen Sie aus der Liste „Andere Ziele“ in der Toolbox ein ADO.NET-Ziel auf die Entwurfsoberfläche unter der ADO NET-Quelle. Ändern Sie mit noch ausgewähltem Zieladapter den Namen im Bereich **Eigenschaften** in **SQL-Ziel**.
   
    ![Screenshot eines Zieladapters, der an eine Stelle direkt unterhalb des Quelladapters gezogen wird.][09]

## <a name="configure-the-source-adapter"></a>Konfigurieren des Quelladapters
1. Doppelklicken Sie auf den Quelladapter, um den **ADO.NET-Quellen-Editor** zu öffnen.
   
    ![Screenshot des ADO.NET-Quellen-Editors. Die Registerkarte „Verbindungs-Manager“ ist sichtbar, und Steuerelemente zum Konfigurieren von Datenflusseigenschaften sind verfügbar.][03]
2. Klicken Sie im **ADO.NET-Quellen-Editor** auf der Registerkarte **Verbindungs-Manager** auf die Schaltfläche **Neu** neben der Liste **ADO.NET-Verbindungs-Manager**, um das Dialogfeld **ADO.NET-Verbindungs-Manager konfigurieren** zu öffnen. Nehmen Sie dann Verbindungseinstellungen für die SQL Server-Datenbank vor, von der in diesem Tutorial Daten geladen werden.
   
    ![Screenshot des Dialogfelds „ADO.NET-Verbindungs-Manager konfigurieren“. Steuerelemente für das Einrichten und Konfigurieren von Verbindungs-Managern sind verfügbar.][04]
3. Klicken Sie im Dialogfeld **ADO.NET-Verbindungs-Manager konfigurieren** auf die Schaltfläche **Neu**, um das Dialogfeld **Verbindungs-Manager** zu öffnen, und erstellen Sie eine neue Datenverbindung.
   
    ![Screenshot des Dialogfelds „Verbindungs-Manager“. Steuerelemente für das Konfigurieren einer Datenverbindung sind verfügbar.][05]
4. Führen Sie im Dialogfeld **Verbindungs-Manager** folgende Schritte durch:
   
   1. Wählen Sie als **Anbieter** den SqlClient-Datenanbieter aus.
   2. Geben Sie für **Servernamen** den SQL Server-Namen ein.
   3. Wählen Sie im Abschnitt **Beim Server anmelden** die Authentifizierungsinformationen aus, oder geben Sie sie ein.
   4. Wählen Sie im Abschnitt **Mit Datenbank verbinden** die AdventureWorks-Beispieldatenbank aus.
   5. Klicken Sie auf **Verbindung testen**.
      
       ![Screenshot eines Dialogfelds, in dem eine Schaltfläche „OK“ und ein Text angezeigt werden, der angibt, dass die Testverbindung erfolgreich war.][06]
   6. Klicken Sie im Dialogfeld, in dem die Ergebnisse des Verbindungstests gemeldet werden, auf **OK**, um zum Dialogfeld **Verbindungs-Manager** zurückzukehren.
   7. Klicken Sie im Dialogfeld **Verbindungs-Manager** auf **OK**, um zum Dialogfeld **ADO.NET-Verbindungs-Manager konfigurieren** zurückzukehren.
5. Klicken Sie im Dialogfeld **ADO.NET-Verbindungs-Manager konfigurieren** auf **OK**, um zum **ADO.NET-Quellen-Editor** zurückzukehren.
6. Wählen Sie im **ADO.NET-Quellen-Editor** in der Liste **Name der Tabelle oder Sicht** die Tabelle **Sales.SalesOrderDetail** aus.
   
    ![Screenshot des ADO.NET-Quellen-Editors. Im Namen der Tabelle oder der Sichtenliste ist die Tabelle „Sales.SalesOrderDetail“ ausgewählt.][07]
7. Klicken Sie auf **Vorschau**, um die ersten 200 Datenzeilen in der Quelltabelle im Dialogfeld **Vorschau der Abfrageergebnisse anzeigen** anzuzeigen.
   
    ![Screenshot des Dialogfelds „Vorschau der Abfrageergebnisse anzeigen“. Mehrere Zeilen mit Umsatzdaten aus der Quelltabelle sind sichtbar.][08]
8. Klicken Sie im Dialogfeld **Vorschau der Abfrageergebnisse anzeigen** auf **Schließen**, um zum **ADO.NET-Quellen-Editor** zurückzukehren.
9. Klicken Sie im **ADO.NET-Quellen-Editor** auf **OK**, um die Konfiguration der Datenquelle abzuschließen.

## <a name="connect-the-source-adapter-to-the-destination-adapter"></a>Herstellen einer Verbindung zwischen dem Quell- und Zieladapter
1. Wählen Sie den Quelladapter auf der Entwurfsoberfläche aus.
2. Wählen Sie den blauen Pfeil aus, der vom Quelladapter ausgeht, und ziehen Sie ihn zum Ziel-Editor, bis dieser fest positioniert ist.
   
    ![Screenshot der Quell- und Zieladapter. Ein blauer Pfeil zeigt vom Quelladapter zum Zieladapter.][10]
   
    Bei einem typischen SSIS-Paket verwenden Sie eine Reihe von anderen Komponenten von der SSIS-Toolbox zwischen Quelle und Ziel, um Ihre Daten beim Durchlaufen durch den SSIS-Datenfluss umzustrukturieren, zu transformieren und zu bereinigen. Um dieses Beispiel so einfach wie möglich zu halten, stellen wir eine direkte Verbindung zwischen Quelle und Ziel her.

## <a name="configure-the-destination-adapter"></a>Konfigurieren des Zieladapters
1. Doppelklicken Sie auf den Zieladapter, um den **ADO.NET-Ziel-Editor** zu öffnen.
   
    ![Screenshot des ADO.NET-Ziel-Editors. Die Registerkarte „Verbindungs-Manager“ ist sichtbar und enthält Steuerelemente zum Konfigurieren von Datenflusseigenschaften.][11]
2. Klicken Sie im **ADO.NET-Ziel-Editor** auf der Registerkarte **Verbindungs-Manager** neben der Liste **Verbindungs-Manager** auf die Schaltfläche **Neu**, um das Dialogfeld **ADO.NET-Verbindungs-Manager konfigurieren** zu öffnen. Nehmen Sie dann Verbindungseinstellungen für die Datenbank vor, in die in diesem Tutorial Daten geladen werden.
3. Klicken Sie im Dialogfeld **ADO.NET-Verbindungs-Manager konfigurieren** auf die Schaltfläche **Neu**, um das Dialogfeld **Verbindungs-Manager** zu öffnen, und erstellen Sie eine neue Datenverbindung.
4. Führen Sie im Dialogfeld **Verbindungs-Manager** folgende Schritte durch:
   1. Wählen Sie als **Anbieter** den SqlClient-Datenanbieter aus.
   2. Geben Sie unter **Servername** den Namen des Servers mit SQL Server oder Azure SQL-Datenbank ein.
   3. Wählen Sie im Abschnitt **Beim Server anmelden** die Option **SQL Server-Authentifizierung verwenden** aus, und geben Sie die Authentifizierungsinformationen ein.
   4. Wählen Sie im Abschnitt **Mit Datenbank verbinden** eine vorhandene Datenbank aus.
    a. Klicken Sie auf **Verbindung testen**.
    b. Klicken Sie im Dialogfeld, in dem die Ergebnisse des Verbindungstests gemeldet werden, auf **OK**, um zum Dialogfeld **Verbindungs-Manager** zurückzukehren.
    c. Klicken Sie im Dialogfeld **Verbindungs-Manager** auf **OK**, um zum Dialogfeld **ADO.NET-Verbindungs-Manager konfigurieren** zurückzukehren.
5. Klicken Sie im Dialogfeld **ADO.NET-Verbindungs-Manager konfigurieren** auf **OK**, um zum **ADO.NET-Ziel-Editor** zurückzukehren.
6. Klicken Sie im **ADO.NET-Ziel-Editor** neben der Liste **Tabelle oder Sicht verwenden** auf **Neu**, um das Dialogfeld **Tabelle erstellen** zu öffnen, um eine neue Zieltabelle mit einer Spaltenliste zu erstellen, die der Quelltabelle entspricht.
   
    ![Screenshot des Dialogfelds „Tabelle erstellen“. S Q L-Code zum Erstellen einer Zieltabelle ist sichtbar.][12a]
7. Führen Sie im Dialogfeld **Tabelle erstellen** die folgenden Schritte aus:
   
   1. Ändern Sie den Namen der Zieltabelle in **SalesOrderDetail**.
      
       ![Screenshot des Dialogfelds „Tabelle erstellen“. S Q L-Code ist zum Erstellen einer Tabelle namens „SalesOrderDetail“ ist sichtbar.][12b]

   2. Klicken Sie auf **OK**, um die Tabelle zu erstellen und zum **ADO.NET-Ziel-Editor** zurückzukehren.
8. Wählen Sie im **ADO.NET-Ziel-Editor** die Registerkarte **Zuordnungen** aus, um festzustellen, wie Spalten in der Quelle denen im Ziel zugeordnet werden.
   
    ![Screenshot der Registerkarte „Zuordnungen“ des ADO.NET-Ziel-Editors. Linien verbinden Spalten mit identischen Namen in den Quell- und Zieltabellen.][13]
9. Klicken Sie auf **OK**, um die Konfiguration des Ziels abzuschließen.

## <a name="run-the-package-to-load-the-data"></a>Ausführen des Pakets zum Laden der Daten
Führen Sie das Paket aus, indem Sie auf der Symbolleiste auf die Schaltfläche **Starten** klicken oder im Menü **Debuggen** eine der Optionen **Ausführen** auswählen.

In den folgenden Abschnitten wird beschrieben, was Sie sehen, wenn Sie das Paket mit der zweiten Option, die in diesem Artikel dargestellt ist, erstellt haben, also mit dem Datenfluss, der eine Quelle und ein Ziel enthält.

Wenn die Paketausführung beginnt, sehen Sie gelbe sich drehende Räder, die auf laufende Aktivität hinweisen, sowie die Anzahl der bisher verarbeiteten Zeilen.

![Screenshot der Quell- und Zieladapter. Über jedem Adapter befinden sich gelbe, sich drehende Räder, und der Text „89748 Zeilen“ steht dazwischen.][14]

Wenn die Ausführung des Pakets abgeschlossen ist, sehen Sie grüne Häkchen, die auf ihre erfolgreiche Ausführung hinweisen, sowie die Gesamtanzahl der Datenzeilen, die von der Quelle in das Ziel geladen wurden.

![Screenshot der Quell- und Zieladapter. Über jedem Adapter befinden sich grüne Häkchen, und der Text „121317 Zeilen“ steht dazwischen.][15]

Glückwunsch! Sie haben mit SQL Server Integration Services erfolgreich Daten in SQL Server oder Azure SQL-Datenbank geladen.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über das Debuggen und Behandeln von Problemen mit Ihren Paketen direkt in der Entwurfsumgebung. Beginnen Sie mit dem folgenden Artikel: [Tools zur Problembehandlung für die Paketentwicklung][Troubleshooting Tools for Package Development].

- Erfahren Sie, wie Sie Ihre SSIS-Projekte und -Pakete in Integration Services Server oder an einem anderen Speicherort bereitstellen. Beginnen Sie mit dem folgenden Artikel: [Bereitstellung von Projekten und Paketen][Deployment of Projects and Packages].

<!-- Image references -->
[01]:  ./media/load-data-to-sql-database-with-ssis/ssis-designer-01.png
[02]:  ./media/load-data-to-sql-database-with-ssis/ssis-data-flow-task-02.png
[03]:  ./media/load-data-to-sql-database-with-ssis/ado-net-source-03.png
[04]:  ./media/load-data-to-sql-database-with-ssis/ado-net-connection-manager-04.png
[05]:  ./media/load-data-to-sql-database-with-ssis/ado-net-connection-05.png
[06]:  ./media/load-data-to-sql-database-with-ssis/test-connection-06.png
[07]:  ./media/load-data-to-sql-database-with-ssis/ado-net-source-07.png
[08]:  ./media/load-data-to-sql-database-with-ssis/preview-data-08.png
[09]:  ./media/load-data-to-sql-database-with-ssis/source-destination-09.png
[10]:  ./media/load-data-to-sql-database-with-ssis/connect-source-destination-10.png
[11]:  ./media/load-data-to-sql-database-with-ssis/ado-net-destination-11.png
[12a]: ./media/load-data-to-sql-database-with-ssis/destination-query-before-12a.png
[12b]: ./media/load-data-to-sql-database-with-ssis/destination-query-after-12b.png
[13]:  ./media/load-data-to-sql-database-with-ssis/column-mapping-13.png
[14]:  ./media/load-data-to-sql-database-with-ssis/package-running-14.png
[15]:  ./media/load-data-to-sql-database-with-ssis/package-success-15.png

<!-- Article references -->

<!-- MSDN references -->
[Download SQL Server Data Tools (SSDT)]: ../ssdt/download-sql-server-data-tools-ssdt.md
[Data Flow]: ./data-flow/data-flow.md
[Troubleshooting Tools for Package Development]: ./troubleshooting/troubleshooting-tools-for-package-development.md
[Deployment of Projects and Packages]: ./packages/deploy-integration-services-ssis-projects-and-packages.md

<!--Other Web references-->
[Microsoft SQL Server 2017 Integration Services Feature Pack for Azure]: https://www.microsoft.com/download/details.aspx?id=54798
[SQL Server Evaluations]: https://www.microsoft.com/evalcenter/evaluate-sql-server-2017
[Visual Studio Community]: https://www.visualstudio.com/products/visual-studio-community-vs.aspx
[AdventureWorks 2014 Sample Databases]: https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks
