---
title: Erstellen und Bereitstellen in einer lokalen Datenbank
description: In diesem Artikel erhalten Sie Informationen zur lokalen Serverinstanz, die SQL Server 2012 bereitstellt. Außerdem erfahren Sie, wie Sie diese Instanz für das Erstellen, Testen und Debuggen von Entwicklungsprojekten verwenden.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: ebca8ff8-9a09-4207-8979-9d577af7c1d5
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 598c75f5ef502d1679bd8ed7f17cb44c0f536abc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063535"
---
# <a name="how-to-build-and-deploy-to-a-local-database"></a>Gewusst wie: Erstellen und Bereitstellen in einer lokalen Datenbank

Microsoft SQL Server 2012 stellt eine lokale bedarfsgesteuerte Serverinstanz bereit, die als „SQL Server Express Local Database Runtime“ bezeichnet wird. Diese wird beim Debuggen eines SQL Server-Datenbankprojekts aktiviert. Diese lokale Serverinstanz kann als Sandkasten zum Erstellen, Testen und Debuggen von Projekten genutzt werden. Sie ist unabhängig von installierten SQL Server-Instanzen, und außerhalb von SQL Server Data Tools (SSDT) kann nicht darauf zugegriffen werden. Eine solche Anordnung eignet sich ideal für Entwickler, die nur über eingeschränkten oder überhaupt keinen Zugriff auf Produktionsdatenbanken verfügen, die jedoch ihre Projekte lokal testen möchten, bevor sie von entsprechend berechtigten Mitarbeitern in der Produktionsumgebung bereitgestellt werden. Wenn Sie eine Datenbanklösung für SQL Azure entwickeln, können Sie zudem die Vorteile dieses lokalen Servers nutzen, um das Datenbankprojekt vor dem Bereitstellen in der Cloud lokal zu entwickeln und zu testen.  
  
> [!WARNING]  
> Eine Datenbank unter dem Knoten für lokale Datenbanken im SQL Server-Objekt-Explorer spiegelt das entsprechende Datenbankprojekt wider und steht in keiner Beziehung zu der gleichnamigen Datenbank auf einer verbundenen Serverinstanz.  
  
> [!WARNING]  
> Bei den folgenden Vorgehensweisen werden Entitäten verwendet, die in vorherigen Vorgehensweisen in den Abschnitten [Entwicklung verbundener Datenbanken](../ssdt/connected-database-development.md) und [Projektorientierte Entwicklung von Offlinedatenbanken](../ssdt/project-oriented-offline-database-development.md) erstellt wurden.  
  
### <a name="to-use-the-local-database"></a>So verwenden Sie die lokale Datenbank  
  
1.  Beachten Sie, dass im **SQL Server-Objekt-Explorer** unter dem Knoten **SQL Server** ein neuer Knoten mit dem Namen **Lokal** angezeigt wird. Hierbei handelt es sich um die lokale Datenbankinstanz.  
  
2.  Erweitern Sie die Knoten **Lokal** und **Datenbanken**. Beachten Sie die Anzeige einer Datenbank mit demselben Namen wie das Projekt "TradeDev". Erweitern Sie die Knoten unter dieser Datenbank. Im Fenster **Datentoolvorgänge** wird der Status von derzeit ausgeführten Erweiterungs-/Importvorgängen für sämtliche unter dem Knoten **Lokal** aufgeführten Datenbanken angezeigt. Beachten Sie, dass sie keine der Tabellen und Entitäten enthalten, die Sie in früheren Prozeduren erstellt haben.  
  
3.  Drücken Sie F5, um das Datenbankprojekt "TradeDev" zu debuggen.  
  
    Standardmäßig verwendet SSDT zum Debuggen von Datenbankprojekten die lokale Datenbankserverinstanz. In diesem Fall versucht SSDT zunächst, das Projekt zu erstellen. Wenn kein Fehler auftritt, werden das Projekt (und seine Entitäten) in der lokalen Datenbank bereitgestellt. Wenn Sie später dasselbe Projekt debuggen, erkennt SSDT alle seit der letzten Debugsitzung vorgenommenen Änderungen und stellt nur diese in der lokalen Datenbank bereit.  
  
4.  Erweitern Sie auf dem Datenbankserver **Lokal** wieder die Knoten unter **TradeDev**. Dieses Mal stellen Sie fest, dass die Tabellen, Sichten und Funktionen auf dem lokalen Datenbankserver bereitgestellt wurden.  
  
5.  Klicken Sie mit der rechten Maustaste auf den Knoten **TradeDev**, und wählen Sie **Neue Abfrage** aus.  
  
6.  Fügen Sie diesen Code im Skriptbereich ein, und klicken Sie auf die Schaltfläche **Abfrage ausführen**, um die Abfrage auszuführen.  
  
    ```  
    select * from dbo.GetProductsBySupplier(1)  
    ```  
  
7.  Im Bereich **Meldung** wird „(0 Zeile(n) betroffen)“ angezeigt, während im Bereich **Ergebnisse** keine Zeilen zurückgegeben werden. Das ist darauf zurückzuführen, dass die Abfrage für die lokale Datenbank und nicht für die verbundene Datenbank ausgeführt wird, die die tatsächlichen Daten enthält.  
  
    Sie können sich davon überzeugen, indem Sie unter dieser lokalen Datenbank **TradeDev** mit der rechten Maustaste auf die Tabelle **Products** klicken und **Daten anzeigen** auswählen. Die Tabelle ist leer.  
  
### <a name="to-replicate-real-data-to-the-local-database"></a>So replizieren Sie tatsächliche Daten in der lokalen Datenbank  
  
1.  Erweitern Sie im **SQL Server-Objekt-Explorer** Ihre verbundene SQL Server-Instanz, und suchen Sie die Datenbank **TradeDev**.  
  
    Klicken Sie mit der rechten Maustaste auf die Tabelle **Suppliers**, und wählen Sie **Daten anzeigen** aus.  
  
2.  Klicken Sie am oberen Rand des Daten-Editors auf die Schaltfläche **Skript** (die zweite Schaltfläche von rechts). Kopieren Sie die `INSERT`-Anweisungen aus dem Skript.  
  
3.  Erweitern Sie die Serverinstanz **Lokal**, und klicken Sie mit der rechten Maustaste auf den Knoten **TradeDev**. Wählen Sie **Neue Abfrage** aus.  
  
4.  Fügen Sie die `INSERT`-Anweisungen in diesem Abfragefenster ein, und führen Sie die Abfrage aus.  
  
5.  Führen Sie die obigen Schritte erneut aus, um Daten aus den Tabellen **Products** und **Fruits** in der verbundenen Datenbank **TradeDev** in der lokalen Datenbank **TradeDev** zu replizieren.  
  
6.  Klicken Sie mit der rechten Maustaste auf die Serverinstanz **Lokal**, und wählen Sie **Aktualisieren** aus. Untersuchen Sie die Tabellen mit **Daten anzeigen**, um sich zu vergewissern, dass die lokale Datenbank aufgefüllt wurde.  
  
7.  Klicken Sie mit der rechten Maustaste auf den Knoten **TradeDev** der Serverinstanz „Lokal“, und wählen Sie **Neue Abfrage** aus.  
  
8.  Fügen Sie diesen Code im Skriptbereich ein, und klicken Sie auf die Schaltfläche **Abfrage ausführen**, um die Abfrage auszuführen.  
  
    ```  
    select * from dbo.GetProductsBySupplier(1)  
    ```  
  
9. Im Bereich **Ergebnis** unter dem Bereich für den Transact\-SQL-Editor wurden die Zeilen „Apples“ und „Potato Chips“ aus der Tabelle `Products` zurückgegeben.  
  
