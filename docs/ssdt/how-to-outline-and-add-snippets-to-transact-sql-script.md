---
title: Gliedern und Hinzufügen von Ausschnitten zu Transact-SQL-Skripts
description: Hier erfahren Sie mehr über Codeausschnitte, die von SSDT bereitgestellt werden. Hier erfahren Sie, wie Sie Codeausschnitte in Anwendungen einfügen und wie Sie Code im Transact-SQL-Editor ausblenden und erweitern.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 543e7ce7-8639-4281-8a91-85314755e5de
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: be1adf9c385273d5cac8afbe972fd3f2f594ca34
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018140"
---
# <a name="how-to-outline-and-add-snippets-to-transact-sql-script"></a>Gewusst wie: Gliedern und Hinzufügen von Ausschnitten zu Transact-SQL-Skripts

SQL Server Data Tools enthält eine Codebibliothek mit Codeausschnitten, die in die Anwendung eingefügt werden können. Jeder Ausschnitt führt einen kompletten Skripttask aus, z. B. das Erstellen einer Funktion, einer Tabelle, eines Triggers, eines Index, einer Sicht, eines benutzerdefinierten Datentyps usw. Mit wenigen Mausklicks können Sie einen Ausschnitt in den Quellcode einfügen. Diese Ausschnitte erhöhen die Produktivität, da Sie weniger Zeit für die Eingabe benötigen.  
  
Wenn Sie nach dem geeigneten Ausschnitt suchen müssen, können Sie die Ausschnittauswahl verwenden, in der kategorisierte Listen von Ausschnitten zur Auswahl stehen. Sobald Sie dem Code den Ausschnitt hinzugefügt haben, müssen möglicherweise Teile des Codes angepasst werden, indem z. B. Variablennamen durch besser geeignete Namen ersetzt werden oder die tatsächliche Logik einer gespeicherten Prozedur eingefügt wird. Sie werden feststellen, dass zu diesem Zweck im eingefügten Codeausschnitt ein oder mehrere Ersetzungspunkte hervorgehoben werden. Wenn Sie den Mauszeiger auf den Ersetzungspunkt setzen, wird in einer QuickInfo erläutert, wie Sie den Code ändern können.  
  
Standardmäßig wird im Transact\-SQL-Editor der gesamte Text angezeigt, Sie können jedoch Teile des Codes ausblenden. Im Transact\-SQL-Editor können Sie einen Codebereich markieren und als reduzierbar festlegen, sodass dieser Bereich unter einem Pluszeichen (+) angezeigt wird. Anschließend können Sie den Bereich erweitern oder ausblenden, indem Sie auf das Pluszeichen (+) neben dem Symbol klicken. Code in einer Gliederung wird nicht gelöscht, sondern nur ausgeblendet.  
  
> [!WARNING]  
> Bei den folgenden Vorgehensweisen werden Entitäten verwendet, die in vorherigen Vorgehensweisen in den Abschnitten [Entwicklung verbundener Datenbanken](../ssdt/connected-database-development.md) und [Projektorientierte Entwicklung von Offlinedatenbanken](../ssdt/project-oriented-offline-database-development.md) erstellt wurden.  
  
### <a name="to-insert-snippets"></a>So fügen Sie Ausschnitte ein  
  
1.  Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **TradeDev**, und klicken Sie auf **Hinzufügen** und dann auf **Skript**. Klicken Sie im Dialogfeld **Neues Element hinzufügen** auf **Hinzufügen**.  
  
2.  Klicken Sie mit der rechten Maustaste auf den Transact\-SQL-Editor, und wählen Sie **Ausschnitt einfügen** aus. Die Codeausschnittauswahl wird angezeigt.  
  
3.  Doppelklicken Sie in der Codeausschnittauswahl auf **Tabelle** und dann auf **Tabelle erstellen**.  
  
4.  Beachten Sie, dass Ersetzungspunkte in Gelb hervorgehoben werden. Zeigen Sie mit der Maus auf `Sample_Table`, und in einem Infotipp wird eine Beschreibung der Ersetzung angezeigt. Doppelklicken Sie auf `Sample_Table`, und ändern Sie den Tabellennamen in `Shipper2`.  
  
5.  Wechseln Sie mithilfe der TAB-TASTE zum nächsten Ersetzungspunkt (`column_1`). Benennen Sie die Spalte in `Id` um. Führen Sie die gleichen Schritte aus, um `column_2` in `name` umzubenennen, ändern Sie den Datentyp in `nvarchar(128)`, und lassen Sie `NULL`-Werte zu.  
  
### <a name="to-outline-code"></a>So gliedern Sie Code  
  
1.  Beachten Sie das **-** -Zeichen neben der CREATE TABLE-Anweisung. Klicken Sie auf das **-** -Zeichen neben einem Abschnitt im Skript, um ihn auszublenden.  
  
2.  Klicken Sie mit der rechten Maustaste auf den Transact\-SQL-Editor, und wählen Sie **Gliedern** und dann **Gliederung entfernen** aus, um die Gliederungsinformationen ohne Auswirkungen auf den zugrunde liegenden Code im Editor zu entfernen.  
  
3.  Um den Code erneut zu gliedern, klicken Sie mit der rechten Maustaste auf den Transact\-SQL-Editor, und wählen Sie **Gliedern** und dann **Automatische Gliederung starten** aus. Sie können auch **Alle Gliederungen umschalten** auswählen, um zwischen der Ansicht mit erweiterten und ausgeblendeten Abschnitten zu wechseln.  
  
