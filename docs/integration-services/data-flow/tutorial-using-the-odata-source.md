---
description: 'Tutorial: Verwenden der OData-Quelle'
title: 'Tutorial: Verwenden der OData-Quelle | Microsoft-Dokumentation'
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 2c64cf8b-5edb-48df-8ffe-697096258f71
author: chugugrace
ms.author: chugu
ms.openlocfilehash: b0bb777c05b30e4b794fc889dc6322e24c4dd5ab
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339735"
---
# <a name="tutorial-using-the-odata-source"></a>Tutorial: Verwenden der OData-Quelle

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Dieses Tutorial führt Sie schrittweise durch den Prozess, bei dem die Sammlung **Employees** aus dem OData-Beispieldienst **Northwind** (https://services.odata.org/V3/Northwind/Northwind.svc/) extrahiert und anschließend in eine Flatfile geladen wird.  
  
## <a name="1-create-an-integration-services-project"></a>1. Erstellen eines Integration Services-Projekts  
  
1.  Starten Sie **SQL Server Data Tools** oder [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)].  
  
2.  Klicken Sie auf **Datei**, zeigen Sie auf **Neu**, und klicken Sie auf **Projekt**.  
  
3.  Erweitern Sie im Dialogfeld **Neues Projekt** die Knoten **Installiert**, **Vorlagen** und **Business Intelligence**, und klicken Sie auf **Integration Services**.  
  
4.  Wählen Sie als Projekttyp **Integration Services-Projekt** aus.  
  
5.  Geben Sie einen **Namen** ein, und wählen Sie einen **Speicherort** für das Projekt aus. Klicken Sie anschließend auf **OK**.  
  
## <a name="2-add-and-configure-an-odata-source"></a>2. Hinzufügen und Konfigurieren einer OData-Quelle 
  
1.  Ziehen Sie einen **Datenflusstask** aus der **SSIS-Toolbox** auf die Entwurfsoberfläche der Ablaufsteuerung des SSIS-Pakets.  
  
2.  Klicken Sie auf die Registerkarte **Datenfluss**, oder doppelklicken Sie auf den **Datenflusstask**, um die Datenfluss-Entwurfsoberfläche zu öffnen.  
  
3.  Ziehen Sie die **OData-Quelle** aus der Gruppe **Allgemein** in die **SSIS-Toolbox**.
  
4.  Doppelklicken Sie auf die Komponente **OData-Quelle**, um das Dialogfeld **Quellen-Editor für OData** zu öffnen.  
  
5.  Klicken Sie auf **Neu…**, um einen neuen OData-Verbindungs-Manager hinzuzufügen.  
  
6.  Geben Sie die OData-Dienst-URL als **Speicherort des Dienstdokuments** ein. Diese URL kann die URL zum Dienstdokument oder zu einem bestimmten Feed oder einer bestimmten Entität sein. Geben Sie im Rahmen dieses Tutorials die folgende URL zum Dienstdokument ein: [https://services.odata.org/V3/Northwind/Northwind.svc/](https://services.odata.org/V3/Northwind/Northwind.svc/).  
  
7.  Vergewissern Sie sich, dass als **Authentifizierung** der Typ **Windows-Authentifizierung** für den Zugriff auf den OData-Dienst ausgewählt ist. **Windows-Authentifizierung** ist standardmäßig ausgewählt.  
  
8.  Klicken Sie zum Testen der Verbindung auf **Verbindung testen**, und klicken Sie auf **OK**, um eine Instanz des OData-Verbindungs-Managers zu erstellen.  
  
9. Vergewissern Sie sich im Dialogfeld **Quellen-Editor für OData** , dass **Auflistung** für die Option **Auflistung für Ressourcenpfad verwenden** ausgewählt ist.  
  
10. Wählen Sie in der Dropdownliste **Auflistung** den Eintrag **Employees** aus.  
  
11. Geben Sie zusätzliche OData-Abfrageoptionen oder Filter für **Abfrageoptionen** ein. Beispiel: `$orderby=CompanyName&$top=100`. Geben Sie für die Zwecke dieses Tutorials `$top=5` ein.  
  
12. Klicken Sie auf **Vorschau** , um eine Vorschau der Daten aufzurufen.  
  
13. Klicken Sie im linken Navigationsbereich auf **Spalten** , um zur Seite **Spalten** zu wechseln.  
  
14. Wählen Sie in **Verfügbare externe Spalten** die Spalten **EmployeeID**, **FirstName** und **LastName** aus, indem Sie die Kontrollkästchen aktivieren.  
  
15. Klicken Sie auf **OK** , um das Dialogfeld **Quellen-Editor für OData** zu schließen.  
  
## <a name="3-add-and-configure-a-flat-file-destination"></a>3. Hinzufügen und Konfigurieren eines Flatfileziels
  
1.  Ziehen Sie nun ein **Flatfileziel** aus der **SSIS-Toolbox** auf die Entwurfsoberfläche Datenfluss unter die Komponente **OData-Quelle** .  
  
2.  Verbindung Sie über einen blauen Pfeil die Komponente **OData-Quelle** mit der Komponente **Flatfileziel** .  
  
3.  Doppelklicken Sie auf **Flatfileziel**. Das Dialogfeld **Ziel-Editor für Flatfiles** wird geöffnet.  
  
4.  Klicken Sie im Dialogfeld **Ziel-Editor für Flatfiles** auf **Neu** , um einen neuen Verbindungs-Manager für Flatfiles zu erstellen.  
  
5.  Wählen Sie im Dialogfeld **Flatfileformat** die Option **Mit Trennzeichen** aus. Dann wird dass Dialogfeld **Verbindungs-Manager-Editor für Flatfiles** geöffnet.  
  
6.  Geben Sie im Dialogfeld **Verbindungs-Manager-Editor für Flatfiles** für **Dateiname**`c:\Employees.txt` ein.  
  
7.  Klicken Sie im linken Navigationsbereich auf **Spalten**. Auf dieser Seite können Sie die Daten in der Vorschau anzeigen.  
  
8.  Klicken Sie auf OK, um das Dialogfeld **Verbindungs-Manager-Editor für Flatfiles** zu schließen.  
  
9. Klicken Sie im Dialogfeld **Ziel-Editor für Flatfiles** im linken Navigationsbereich auf **Zuordnungen** . Überprüfen Sie die Zuordnungen.  
  
10. Klicken Sie auf OK, um das Dialogfeld **Ziel-Editor für Flatfiles** zu schließen.  

## <a name="4-run-the-package"></a>4. Ausführen des Pakets
Führen Sie das SSIS-Paket aus. Überprüfen Sie, ob die Ausgabedatei mit ID, Vorname und Nachname für fünf Mitarbeiter aus dem OData-Feed erstellt wird.
  
  
