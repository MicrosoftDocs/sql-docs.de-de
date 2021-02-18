---
title: Generieren von Skripts
description: Hier erfahren Sie, wie Sie den Assistenten zum Generieren und Veröffentlichen von Skripts verwenden, um Transact-SQL-Skripts für mehrere Objekte zu erstellen. Außerdem wird erläutert, wie Sie das Skript als Menü im Objekt-Explorer nutzen, um Skripts für einzelne oder mehrere Objekte zu generieren.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
ms.assetid: 9711c617-3c68-4e5a-aea3-befc64d51524
author: markingmyname
ms.author: maghan
ms.reviewer: mathoma
ms.custom: seo-lt-2019
ms.date: 04/07/2020
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 31907e51df24aaaee20b1fc27bbc97bf809abe03
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100271294"
---
# <a name="generate-scripts-sql-server-management-studio"></a>Erstellen von Skripts (SQL Server Management Studio)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] stellt zwei Mechanismen zum Generieren von [!INCLUDE[tsql](../../includes/tsql-md.md)] -Skripts bereit. Verwenden Sie zum Erstellen von Skripts für mehrere Objekte den **Assistenten zum Generieren und Veröffentlichen von Skripts**. Sie können ein Skript für einzelne Objekte oder mehrere Objekte auch über das Menü **Skript für** im **Objekt-Explorer** generieren.

Ein ausführliches Tutorial zur Skripterstellung für verschiedene Objekte mithilfe von SQL Server Management Studio finden Sie unter [Tutorial: Scripting in SSMS (Skripterstellung in SSMS)](../tutorials/scripting-ssms.md).

## <a name="before-you-begin"></a>Vorbereitungen

Wählen Sie den Mechanismus aus, der Ihre Anforderungen am besten erfüllt. 

###  <a name="generate-and-publish-scripts-wizard"></a><a name="GenPubScriptWiz"></a> Assistent zum Generieren und Veröffentlichen von Skripts

Verwenden Sie den **Assistenten zum Generieren und Veröffentlichen von Skripts** , um ein [!INCLUDE[tsql](../../includes/tsql-md.md)] -Skript für zahlreiche Objekte zu erstellen. Der Assistent generiert ein Skript für alle in einer Datenbank enthaltenen Objekte bzw. für eine ausgewählte Teilmenge der Objekte. Der Assistent verfügt über viele Optionen für Skripts, z. B. ob Berechtigungen, Sortierung, Einschränkungen usw. eingeschlossen werden sollen. Anweisungen zum Verwenden des Assistenten finden Sie unter [Generate and Publish Scripts Wizard](./generate-and-publish-scripts-wizard.md).
  
### <a name="object-explorer-script-as-menu"></a><a name="OEScriptAsMenu"></a> Objekt-Explorer-Menü "Skript für Objekttyp als"

Sie können das Menü **Skripterstellung als** des Objekt-Explorers verwenden, um ein Skript für ein einzelnes Objekt, für mehrere Objekte bzw. mehrere Anweisungen für einzelne Objekte zu erstellen. Sie können unter mehreren Skripttypen auswählen: z. B. zum Erstellen, Ändern oder Löschen des Objekts. Sie können das Skript entweder im Abfrage-Editor-Fenster, in einer Datei oder in der Zwischenablage speichern. Das Skript wird im Unicode-Format erstellt.

## <a name="to-generate-a-script-of-a-single-object"></a><a name="ScriptSingleObject"></a> So generieren Sie ein Skript für ein einzelnes Objekt

**So erstellen Sie ein Skript für ein einzelnes Objekt**

1. Stellen Sie im Objekt-Explorer eine Verbindung mit einer Instanz von [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] her, und erweitern Sie dann diese Instanz.

2. Erweitern Sie **Datenbanken**, und erweitern Sie dann die Datenbank, die das Objekt enthält, das geschrieben werden soll.

3. Erweitern Sie die Kategorie des Objekts. Beispiel: Erweitern Sie den Knoten **Tabellen** oder **Sichten** .

4. Klicken Sie mit der rechten Maustaste auf das Objekt, und zeigen Sie auf **Skript für \<object type> als** (z. B. **Skript für Tabelle als**).

5. Zeigen Sie auf den Skripttyp, z. B. **CREATE in** oder **ALTER in**.

6. Wählen Sie den Speicherort zum Speichern des Skripts aus, z. B. **Neues Abfrage-Editor-Fenster** oder **Zwischenablage**.

    ![Skripterstellung für Tabellen](media/generate-scripts-sql-server-management-studio/script-table.png)

Sie können den Bereich **Details** des Objekt-Explorers verwenden, um ein Skript für mehrere Objekte der gleichen Kategorie zu generieren.

1. Stellen Sie im Objekt-Explorer eine Verbindung mit einer Instanz von [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] her, und erweitern Sie dann diese Instanz.

2. Erweitern Sie **Datenbanken**, und erweitern Sie dann die Datenbank, die die Objekte enthält, die geschrieben werden sollen.

3. Erweitern Sie den Kategorieknoten der Objekttypen, für die Sie ein Skript erstellen möchten, z. B. den Knoten **Tabellen** .

4. Öffnen Sie den Bereich **Details zum Objekt-Explorer** durch Drücken von **F7** oder durch Öffnen des Menüs **Ansicht** und Auswahl der Option **Details zum Objekt-Explorer**.

    ![Menü Ansicht](media/generate-scripts-sql-server-management-studio/object-explorer-details-view-menu.png)

5. Klicken Sie mit der linken Maustaste auf eines der Objekte, für das Sie ein Skript erstellen möchten.

6. Klicken Sie bei gedrückter STRG-TASTE mit der linken Maustaste auf das zweite Objekt, für das Sie ein Skript erstellen möchten.

7. Klicken Sie mit der rechten Maustaste auf eines der ausgewählten Objekte und dann auf **Skript für \<object type> als**.

    ![Details](media/generate-scripts-sql-server-management-studio/object-explorer-details.png)