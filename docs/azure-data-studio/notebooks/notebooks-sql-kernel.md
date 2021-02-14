---
title: Erstellen eines Notebooks mit dem SQL-Kernel in Azure Data Studio
description: In diesem Tutorial erfahren Sie, wie Sie ein SQL Server-Notebook erstellen und ausführen.
ms.topic: how-to
ms.prod: azure-data-studio
ms.technology: azure-data-studio
author: markingmyname
ms.author: maghan
ms.reviewer: mikeray, alayu
ms.custom: ''
ms.date: 07/01/2020
ms.openlocfilehash: d2d04be4d571b14445abfb638f070b3666ce9984
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040280"
---
# <a name="create-and-run-a-sql-server-notebook"></a>Erstellen und Ausführen eines SQL Server-Notebooks

[!INCLUDE[SQL Server 2019](../../includes/applies-to-version/sqlserver2019.md)]

In diesem Tutorial wird veranschaulicht, wie Sie mithilfe von SQL Server ein Notebook in Azure Data Studio erstellen und ausführen.

## <a name="prerequisites"></a>Voraussetzungen

- [Installierte Azure Data Studio-Instanz](../download-azure-data-studio.md)
- Installierte SQL Server-Instanz
  - [Windows](../../database-engine/install-windows/install-sql-server.md)
  - [Linux](../../linux/sql-server-linux-setup.md)

## <a name="create-a--notebook"></a>Erstellen eines Notebooks

Führen Sie die folgenden Schritte aus, um eine Notebook-Datei in Azure Data Studio zu erstellen:

1. Stellen Sie in Azure Data Studio eine Verbindung mit Ihrer SQL Server-Instanz her.

2. Wählen Sie unter **Verbindungen** im Fenster **Server** Ihre SQL Server-Instanz aus. Klicken Sie anschließend auf **Neues Notebook**.

3. Warten Sie, bis die Felder für den **Kernel** und den Zielkontext (**Anfügen an**) ausgefüllt sind. Vergewissern Sie sich, dass der **Kernel** auf **SQL** festgelegt ist. Legen Sie im Feld **Anfügen an** Ihre SQL Server-Instanz (in diesem Beispiel *localhost*) fest.

   ![„Kernel“ und „Anfügen an“ festlegen](media/notebooks-sql-kernel/set-kernel-and-attach-to.png)

Sie können das Notebook mit dem Befehl **Speichern** oder **Speichern unter...** im Menü **Datei** speichern.

Zum Öffnen eines Notebooks können Sie den Befehl **Datei öffnen...** im Menü **Datei** verwenden. Wählen Sie auf der **Begrüßungs**-Seite den Befehl **Öffnen** aus, oder verwenden Sie den Befehl **Datei: Öffnen** aus der Befehlspalette.

## <a name="change-the-sql-connection"></a>Ändern der SQL-Verbindung

Gehen Sie zum Ändern der SQL-Verbindung für ein Notebook folgendermaßen vor:

1. Klicken Sie in der Symbolleiste des Notebooks auf das Menü **Anfügen an**, und wählen Sie anschließend die Option **Verbindung ändern** aus.

   ![Klicken Sie auf das Menü „Attach to“ (Anfügen an) in der Symbolleiste des Notebooks.](./media/notebooks-sql-kernel/select-attach-to-1.png)

2. Hier können Sie entweder einen zuletzt verwendeten Verbindungsserver auswählen oder neue Verbindungsdetails für die Verbindung eingeben.

## <a name="run-a-code-cell"></a>Ausführen einer Codezelle

Sie können Zellen erstellen, die SQL-Code enthalten, den Sie direkt ausführen können, indem Sie auf die Schaltfläche **Zelle ausführen** (der runde schwarze Pfeil) links neben der Zelle klicken. Die Ergebnisse werden im Notebook angezeigt, nachdem das Ausführen der Zelle abgeschlossen ist.

Beispiel:

1. Klicken Sie auf der Symbolleiste auf den Befehl **+ Code**, um eine neue Codezelle hinzuzufügen.

   ![Notebook-Symbolleiste](media/notebooks-guidance/notebook-toolbar.png)

2. Kopieren Sie das folgende Beispiel, fügen Sie es in die Zelle ein, und klicken Sie auf **Zelle ausführen**. Im folgenden Beispiel wird eine neue Datenbank erstellt:

   ```sql
   USE master
   GO

   -- Drop the database if it already exists
   IF  EXISTS (
           SELECT name
           FROM sys.databases
           WHERE name = N'TestNotebookDB'
      )
   DROP DATABASE TestNotebookDB
   GO

   -- Create the database
   CREATE DATABASE TestNotebookDB
   GO
   ```

   ![Run cell (Zelle ausführen)](media/notebooks-sql-kernel/run-notebook-cell.png)

## <a name="save-the-result"></a>Ergebnis speichern

Wenn Sie ein Skript ausführen, das ein Ergebnis zurückgibt, können Sie dieses Ergebnis in verschiedenen Formaten mithilfe der über dem Ergebnis angezeigten Symbolleiste speichern.

- Als CSV speichern
- Als Excel speichern
- Als JSON speichern
- Als XML speichern

So gibt der folgende Code z. B. das Ergebnis [PI](../../t-sql/functions/pi-transact-sql.md) zurück.

```sql
SELECT PI() AS PI;
GO
```

![Ergebnis speichern](media/notebooks-sql-kernel/run-notebook-cell-2.png)

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über Notebooks:

- [Verwenden von Notebooks in Azure Data Studio](./notebooks-guidance.md)
- [Erstellen und Ausführen eines Python-Notebooks](././notebooks-python-kernel.md)
- [Ausführen eines Beispielnotebooks mithilfe von Spark](../../big-data-cluster/notebooks-tutorial-spark.md)