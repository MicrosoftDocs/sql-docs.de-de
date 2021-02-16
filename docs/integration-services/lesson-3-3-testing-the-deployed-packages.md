---
description: 'Lektion 3-3: Testen der bereitgestellten Pakete'
title: 'Schritt 3: Testen der bereitgestellten Pakete | Microsoft-Dokumentation'
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: 9159da3f-c9ca-4015-9e85-3bf4373a1349
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 96a06d721c1ccbb4ce254705ceb676c1c338b68a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354934"
---
# <a name="lesson-3-3---testing-the-deployed-packages"></a>Lektion 3-3: Testen der bereitgestellten Pakete

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


In dieser Aufgabe testen Sie die Pakete, die Sie auf einer Instanz von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]bereitgestellt haben.  
  
In anderen [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] -Tutorials haben Sie Pakete in [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)], der Entwicklungsumgebung für [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)], mithilfe des Befehls **Debuggen starten** im Menü **Debuggen** ausgeführt. Dieses Mal führen Sie die Pakete auf andere Weise aus.  
  
[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] „Integration Services“ stellt mehrere Tools bereit, die Sie zum Ausführen von Paketen in der Test- und Produktionsumgebung verwenden können: das Eingabeaufforderungs-Hilfsprogramm **dtexec** und das Paketausführungshilfsprogramm. Das Paketausführungshilfsprogramm ist ein grafisches Tool, das auf **dtexec** aufbaut. Mit diesen beiden Tools wird das Paket sofort ausgeführt. Zusätzlich stellt [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ein Subsystem des SQL Server-Agents bereit, das speziell dazu dient, die Paketausführung als Schritt in einem SQL Server-Agent-Auftrag zu planen.  
  
Sie verwenden das Paketausführungshilfsprogramm, um die bereitgestellten Pakete auszuführen. Die Pakete werden in ihrem aktuellen Zustand verwendet. Sie brauchen die Informationen auf den Seiten des Dialogfelds also nicht zu aktualisieren. Sie führen die Pakete von der Seite Allgemein aus. Hierbei handelt es sich um die erste Seite des Paketausführungshilfsprogramms. Sie können auf die anderen Seiten klicken, um zu sehen, welche Informationen für jedes Paket angezeigt werden.  
  
> [!NOTE]  
> Sie sollten die Optionen nicht ändern, um sicherzustellen, dass die Pakete im Kontext dieses Lernprogramms erfolgreich ausgeführt werden.  
  
Bevor Sie Pakete mithilfe des Paketausführungshilfsprogramms in [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] ausführen, stellen Sie sicher, dass der Integration Services-Dienst ausgeführt wird. Der Integration Services-Dienst stellt Unterstützung für das Speichern und Ausführen von Paketen bereit. Wird der Dienst beendet, können Sie keine Verbindung mit [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] herstellen, und die auszuführenden Pakete werden nicht in [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] aufgelistet. Sie benötigen außerdem Berechtigungen zum Ausführen des Pakets auf der Instanz, auf der das Paket bereitgestellt wurde. Weitere Informationen finden Sie unter [Integration Services Roles &#40;SSIS Service&#41;](../integration-services/security/integration-services-roles-ssis-service.md) (Integration Services-Rollen [SSIS-Dienst]).  
  
Die Ordner auf der obersten Ebene innerhalb des Ordners Gespeicherte Pakete sind die benutzerdefinierten Ordner, die vom Integration Services-Dienst überwacht werden. Sie können beliebig viele (oder wenige) Ordner in der Datei MsDtsSrvr.ini.xml angeben. In diesem Lernprogramm wird davon ausgegangen, dass Sie die Standarddatei MsDtsSrvr.ini.xml verwenden und dass die Ordner auf der obersten Ebene von Gespeicherte Pakete die Namen Dateisystem und MSDB besitzen.  
  
### <a name="to-connect-to-integration-services-in-sql-server-management-studio"></a>So stellen Sie eine Verbindung mit Integration Services in SQL Server Management Studio her  
  
1.  Klicken Sie auf **Start**, zeigen Sie auf **Alle Programme**, zeigen Sie auf **Microsoft SQL Server**, und klicken Sie dann auf **SQL Server Management Studio**.  
  
2.  Wählen Sie im Dialogfeld **Verbindung mit Server herstellen** in der Liste **Servertyp** die Option **Integration Services** aus, geben Sie im Feld **Servername** einen Servernamen an, und klicken Sie auf **Verbinden**.  
  
    > [!IMPORTANT]  
    > Wenn Sie keine Verbindung mit [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]herstellen können, wird der [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] -Dienst wahrscheinlich nicht ausgeführt. Wenn Sie weitere Informationen zum Status des Diensts erhalten möchten, klicken Sie auf **Start**, zeigen Sie auf **Alle Programme**, zeigen Sie auf **Microsoft SQL Server**, zeigen Sie auf **Konfigurationstools**, und klicken Sie dann auf **SQL Server-Konfigurations-Manager**. Klicken Sie im linken Bereich auf **SQL Server-Dienste**. Suchen Sie im rechten Bereich den [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] -Dienst. Starten Sie den Dienst, wenn er nicht bereits ausgeführt wird.  
  
    [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] wird geöffnet. Standardmäßig ist das Fenster des Objekt-Explorers geöffnet und in der oberen rechten Ecke des Studios platziert. Ist der Objekt-Explorer nicht geöffnet, klicken Sie im Menü **Ansicht** auf **Objekt-Explorer** .  
  
### <a name="to-run-the-packages-using-the-execute-package-utility"></a>So führen Sie die Pakete mithilfe des Paketausführungshilfsprogramms aus  
  
1.  Erweitern Sie im Objekt-Explorer den Ordner Gespeicherte Pakete.  
  
2.  Erweitern Sie den Ordner MSDB. Da Sie die Pakete für [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]bereitgestellt haben, werden alle bereitgestellten Pakete in der msdb-Datenbank von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] gespeichert und im Ordner MSDB angezeigt. Der Ordner Dateisystem ist leer, es sei denn, Sie haben unabhängig von Deployment Tutorial Pakete im Dateisystem bereitgestellt.  
  
3.  Beginnen Sie am oberen Ende der Paketliste, klicken Sie mit der rechten Maustaste auf DataTransfer und anschließend auf **Paket ausführen**.  
  
4.  Klicken Sie im Dialogfeld **Paketausführungshilfsprogramm** auf **Ausführen**.  
  
5.  Im Dialogfeld **Paketausführungshilfsprogramm** werden der Status und die Ausführungsergebnisse des Pakets angezeigt. Das Paket ist abgeschlossen, wenn die Schaltfläche **Beenden** nicht mehr verfügbar ist. Klicken Sie daraufhin auf **Schließen**.  
  
    > [!IMPORTANT]  
    > Falls Sie während der Ausführung des Pakets auf **Beenden** klicken, wird das Paket nicht vollständig ausgeführt.  
  
6.  Klicken Sie im Dialogfeld **Paketausführungshilfsprogramm** auf **Schließen**.  
  
7.  Wiederholen Sie die Schritte 3 bis 6 für das LoadXML-Paket.  
  
8.  Klicken Sie im Menü **Datei** auf **Beenden**.  
  
### <a name="to-verify-the-results-of-the-datatransfer-package"></a>So überprüfen Sie die Ergebnisse des DataTransfer-Pakets  
  
1.  Klicken Sie auf der Symbolleiste in [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]auf **Neue Abfrage**.  
  
2.  Wählen Sie im Dialogfeld **Verbindung mit Server herstellen** in der Liste **Servertyp** den Typ **Datenbank-Engine** aus, geben Sie im Feld **Servername** entweder den Namen des Servers, auf dem Sie die Tutorialpakete installiert haben, oder (local) ein, und wählen Sie einen Authentifizierungsmodus aus. Wenn Sie die SQL Server-Authentifizierung verwenden, geben Sie einen Benutzernamen und ein Kennwort an.  
  
3.  Klicken Sie auf **Verbinden**.  
  
4.  Geben oder fügen Sie im Abfragefenster die folgende SQL-Anweisung ein:  
  
    `USE AdventureWorks`  
  
    `SELECT * FROM HighIncomeCustomers`  
  
5.  Drücken Sie **F5** oder klicken Sie auf der Symbolleiste auf das Ausführen-Symbol.  
  
    Die Abfrage gibt 31 Datenzeilen zurück. Das Rückgabeergebnis enthält alle Zeilen aus der Textdatei Customers.txt, bei denen der Wert in der YearlyIncome-Spalte größer als 100000 ist.  
  
6.  Suchen Sie den Ordner DeploymentTutorial, klicken Sie mit der rechten Maustaste auf die XML-Protokolldatei Deployment Tutorial Log und anschließend auf **Öffnen**. Sie können die Datei öffnen, indem Sie den Editor oder einen anderen Text- bzw. XML-Editor Ihrer Wahl verwenden.  
  
### <a name="to-verify-the-results-of-the-loadxmldata-package"></a>So überprüfen Sie die Ergebnisse des LoadXMLData-Pakets  
  
1.  Klicken Sie auf der Symbolleiste in [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]auf **Neue Abfrage**.  
  
2.  Wenn Sie aufgefordert werden, erneut eine Verbindung herzustellen, wählen Sie im Dialogfeld **Verbindung mit Server herstellen** in der Liste **Servertyp** die Option **Datenbank-Engine** aus, geben Sie im Feld **Servername** entweder den Namen des Servers, auf dem Sie die Tutorialpakete installiert haben, oder „(local)“ ein, und wählen Sie einen Authentifizierungsmodus aus. Wenn Sie die SQL Server-Authentifizierung verwenden, geben Sie einen Benutzernamen und ein Kennwort an.  
  
3.  Klicken Sie auf **Verbinden**.  
  
4.  Geben oder fügen Sie im Abfragefenster die folgende SQL-Anweisung ein:  
  
    `USE AdventureWorks`  
  
    `SELECT * FROM OrderDatesByCountryRegion`  
  
5.  Drücken Sie **F5** oder klicken Sie auf der Symbolleiste auf das Ausführen-Symbol.  
  
    Die Abfrage gibt 21 Datenzeilen zurück. Das Ergebnis besteht aus den Zeilen der XML-Datendatei orders.xml. Die einzelnen Zeilen sind Zusammenfassungen nach Land bzw. Region. In der Zeile werden der Name eines Landes bzw. einer Region, die Anzahl der Aufträge für jedes Land bzw. für jede Region und die Datumswerte der neuesten und ältesten Aufträge aufgelistet.  
  
## <a name="see-also"></a>Siehe auch  
[dtexec (Hilfsprogramm)](../integration-services/packages/dtexec-utility.md)  
  
  
  
