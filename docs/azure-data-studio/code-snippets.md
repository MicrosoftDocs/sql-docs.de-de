---
title: Erstellen von wiederverwendbaren Codeausschnitten
description: Erfahren Sie, wie Sie Azure Data Studio-SQL-Codeausschnitte erstellen und verwenden, die das Erstellen von Datenbanken und Datenbankobjekten vereinfachen.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: how-to
author: markingmyname
ms.author: maghan
ms.reviewer: alayu, sstein
ms.custom: seodec18
ms.date: 09/24/2018
ms.openlocfilehash: e2ea3a950a548cae6ad23f40055acc09b2e9534f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048690"
---
# <a name="create-and-use-code-snippets-to-quickly-create-transact-sql-t-sql-scripts-in-azure-data-studio"></a>Erstellen und Verwenden von Codeausschnitten zum schnellen Erstellen von Transact-SQL-Skripts in Azure Data Studio

Codeausschnitte in Azure Data Studio sind Vorlagen, die das Erstellen von Datenbanken und Datenbankobjekten vereinfachen. 

Azure Data Studio stellt mehrere T-SQL-Codeausschnitte (Transact-SQL) bereit, mit denen Sie schnell Skripts mit richtiger Syntax generieren können. 

Es können auch benutzerdefinierte Codeausschnitte erstellt werden.

## <a name="using-built-in-t-sql-code-snippets"></a>Verwenden von integrierten T-SQL-Codeausschnitten

1. Um auf die verfügbaren Codeausschnitte zuzugreifen, geben Sie *sql* im Abfrage-Editor ein, um die Liste zu öffnen:

   ![Ausschnitte](media/code-snippets/sql-snippets.png)

2. Wählen Sie den Codeausschnitt aus, den Sie verwenden möchten. Auf Basis dieses Ausschnitts wird dann das T-SQL-Skript generiert. Wählen Sie beispielsweise *sqlCreateTable* aus:

   ![Codeausschnitt zum Erstellen einer Tabelle](media/code-snippets/create-table.png)

3. Aktualisieren Sie die markierten Felder mit ihren speziellen Werten. Ersetzen Sie z. B. *TableName* und *Schema* durch die Werte für Ihre Datenbank:

   ![Tabelle aus dem Codeausschnitt](media/code-snippets/table-from-snippet.png)

   Wenn das Feld, das Sie ändern möchten, nicht mehr markiert ist (dies passiert, wenn Sie den Cursor im Editor bewegen), klicken Sie mit der rechten Maustaste auf das Wort, das Sie ändern möchten, und wählen Sie **Alle Vorkommen ändern** aus:

   ![Alle ändern](media/code-snippets/change-all.png)

4. Aktualisieren Sie die T-SQL-Befehle oder fügen Sie die zusätzlichen T-SQL-Befehle hinzu, die Sie für den ausgewählten Codeausschnitt benötigen. Aktualisieren Sie beispielsweise *Column1*, *Column2*, und fügen Sie weitere Spalten hinzu.

## <a name="creating-sql-code-snippets"></a>Erstellen von SQL-Codeausschnitten

Sie können eigene Codeausschnitte definieren. So öffnen Sie die Datei mit SQL-Codeausschnitten zum Bearbeiten:

1. Öffnen Sie die *Befehlspalette* (**UMSCHALT+STRG+P**), geben Sie *snip* (Ausschnitt) ein, und wählen Sie **Einstellungen: Benutzercodeausschnitte öffnen** aus.

   ![Benutzerdefinierte Codeausschnitte](media/code-snippets/user-snippets.png)

2. Wählen Sie **SQL** aus:

   > [!NOTE]
   > Azure Data Studio erbt die Codeausschnittfunktionalität von Visual Studio Code, weshalb in diesem Artikel insbesondere die Verwendung von SQL-Ausschnitten erläutert wird. Ausführlichere Informationen finden Sie unter [Snippets in Visual Studio Code: Create your own snippets](https://code.visualstudio.com/docs/editor/userdefinedsnippets) (Codeausschnitte in Visual Studio Code: Erstellen eigener Codeausschnitte) in der Visual Studio Code-Dokumentation. 

   ![SQL auswählen](media/code-snippets/select-sql.png)

3. Fügen Sie den folgenden Code in *sql.json* ein:

    ```sql
    {
     "Select top 5": {
    "prefix": "sqlSelectTop5",
    "body": "SELECT TOP 5 * FROM ${1:TableName}",
    "description": "User-defined snippet example 1"
    },
    "Create Table snippet":{
    "prefix": "sqlCreateTable2",
    "body": [
    "-- Create a new table called '${1:TableName}' in schema '${2:SchemaName}'",
    "-- Drop the table if it already exists",
    "IF OBJECT_ID('$2.$1', 'U') IS NOT NULL",
    "DROP TABLE $2.$1",
    "GO",
    "-- Create the table in the specified schema",
    "CREATE TABLE $2.$1",
    "(",
    "$1Id INT NOT NULL PRIMARY KEY, -- primary key column",
    "Column1 [NVARCHAR](50) NOT NULL,",
    "Column2 [NVARCHAR](50) NOT NULL",
    "-- specify more columns here",
    ");",
    "GO"
    ],
       "description": "User-defined snippet example 2"
       }
       }
    ```

4. Speichern Sie die Datei „sql.json“.

5. Öffnen Sie ein neues Fenster mit dem Abfrage-Editor, indem Sie auf **STRG+N** klicken.

6. Geben Sie **sql** ein. Sie sehen nun die beiden Benutzercodeausschnitte, die Sie soeben hinzugefügt haben: *sqlCreateTable2* und *sqlSelectTop5*.

Wählen Sie einen der neuen Codeausschnitte aus, und führen Sie ihn zum Testen aus.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum SQL-Editor finden Sie unter [Tutorial: Verwenden des Code-Editors](tutorial-sql-editor.md).
