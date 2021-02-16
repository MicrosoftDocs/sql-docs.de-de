---
title: Exportieren und Importieren einer Datenbank unter Linux
description: In diesem Artikel wird beschrieben, wie Sie mit SQL Server Management Studio und Sqlpackage.exe eine Datenbank unter SQL Server für Linux exportieren und importieren.
author: VanMSFT
ms.author: vanto
ms.date: 10/02/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 2210cfc3-c23a-4025-a551-625890d6845f
ms.openlocfilehash: 40db493908ab0163a973fb46e885d113195b4da5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339167"
---
# <a name="export-and-import-a-database-on-linux-with-ssms-or-sqlpackageexe-on-windows"></a>Exportieren und Importieren einer Datenbank unter Linux mit SSMS oder Sqlpackage.exe unter Windows

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

In diesem Artikel wird beschrieben, wie Sie mit [SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md) und [Sqlpackage.exe](../tools/sqlpackage/sqlpackage.md) eine Datenbank unter SQL Server für Linux exportieren und importieren. SSMS und SqlPackage.exe sind Windows-Anwendungen und können daher auf einem Windows-Computer ausgeführt werden, der eine Verbindung mit einer SQL Server-Remoteinstanz unter Linux herstellen kann.

Sie sollten immer die neueste Version von SQL Server Management Studio (SSMS) installieren und verwenden, wie unter [Verwenden von SQL Server Management Studio unter Windows zum Verwalten von SQL Server für Linux](sql-server-linux-manage-ssms.md) beschrieben.

> [!NOTE]
> Wenn Sie eine Datenbank von einer SQL Server-Instanz zu einer anderen migrieren, sollten Sie [Sicherung und Wiederherstellung](sql-server-linux-migrate-restore-database.md) verwenden.

## <a name="export-a-database-with-ssms"></a>Exportieren einer Datenbank mit SSMS

1. Starten Sie SSMS, indem Sie **Microsoft SQL Server Management Studio** in das Windows-Suchfeld eingeben, und klicken Sie dann auf die Desktop-App.

    ![SQL Server Management Studio](./media/sql-server-linux-manage-ssms/ssms.png) 

2. Stellen Sie im Objekt-Explorer eine Verbindung mit der Quelldatenbank her. Die Quelldatenbank kann in Microsoft SQL Server lokal ausgeführt werden oder in der Cloud, unter Linux, Windows oder Docker und Azure SQL-Datenbank oder Azure Synapse Analytics.

3. Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf die Quelldatenbank, zeigen Sie auf **Aufgaben**, und klicken Sie auf **Datenebenenanwendung exportieren**.

4. Klicken Sie im Export-Assistenten auf **Weiter** und dann auf die Registerkarte **Einstellungen**, und konfigurieren Sie den Export, um die BACPAC-Datei entweder an einem lokalen Speicherort oder in einem Azure-Blob zu speichern.

5. Standardmäßig werden alle Objekte in der Datenbank exportiert. Klicken Sie auf die **Registerkarte „Erweitert“** , und wählen Sie die Datenbankobjekte aus, die Sie exportieren möchten.

6. Klicken Sie auf **Weiter** und anschließend auf **Fertig stellen**.

Die *.BACPAC-Datei wird erfolgreich an dem von Ihnen ausgewählten Speicherort erstellt, und Sie können Sie in eine Zieldatenbank importieren.

## <a name="import-a-database-with-ssms"></a>Importieren einer Datenbank mit SSMS

1. Starten Sie SSMS, indem Sie **Microsoft SQL Server Management Studio** in das Windows-Suchfeld eingeben, und klicken Sie dann auf die Desktop-App.

    ![SQL Server Management Studio](./media/sql-server-linux-manage-ssms/ssms.png) 

2. Stellen Sie im Objekt-Explorer eine Verbindung mit Ihrem Zielserver her. Der Zielserver kann in Microsoft SQL Server lokal ausgeführt werden oder in der Cloud, unter Linux, Windows oder Docker und Azure SQL-Datenbank oder Azure Synapse Analytics.

3. Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf den Ordner **Datenbanken**, und klicken Sie dann auf **Datenebenenanwendung importieren**.

4. Um die Datenbank auf dem Zielserver zu erstellen, geben Sie eine BACPAC-Datei auf Ihrem lokalen Datenträger an, oder wählen Sie das Azure-Speicherkonto und den Container aus, wohin Sie Ihre BACPAC-Datei hochgeladen haben.

5. Geben Sie den neuen Datenbanknamen für die neue Datenbank an. Wenn Sie eine Datenbank in Azure SQL-Datenbank importieren, legen Sie die Edition von Microsoft Azure SQL-Datenbank (Dienstebene), die maximale Datenbankgröße und das Dienstziel (Leistungsebene) fest.

6. Klicken Sie auf **Weiter** und dann auf **Fertigstellen**, um die BACPAC-Datei in eine neue Datenbank auf dem Zielserver zu importieren.

Die *.BACPAC-Datei wird importiert, um eine neue Datenbank auf dem von Ihnen angegebenen Zielserver zu erstellen.

## <a name="sqlpackage-command-line-option"></a><a id="sqlpackage"></a> SqlPackage-Befehlszeilenoption

Es ist auch möglich, das Befehlszeilentool SQL Server Data Tools (SSDT), [SqlPackage.exe](../tools/sqlpackage/sqlpackage.md), zum Exportieren und Importieren von BACPAC-Dateien zu verwenden.

Mit dem folgenden Beispielbefehl wird eine BACPAC-Datei exportiert:

```bash
SqlPackage.exe /a:Export /ssn:tcp:<your_server> /sdn:<your_database> /su:<username> /sp:<password> /tf:<path_to_bacpac>
```

Verwenden Sie den folgenden Befehl, um das Datenbankschema und die Benutzerdaten aus einer BACPAC-Datei zu importieren:

```bash
SqlPackage.exe /a:Import /tsn:tcp:<your_server> /tdn:<your_database> /tu:<username> /tp:<password> /sf:<path_to_bacpac>

```

## <a name="see-also"></a>Weitere Informationen
Weitere Informationen zum Verwenden von SSMS finden Sie unter [Was ist SQL Server Management Studio (SSMS)?](../ssms/sql-server-management-studio-ssms.md). Weitere Informationen zu SqlPackage.exe finden Sie in der [Referenzdokumentation zu SqlPackage](../tools/sqlpackage/sqlpackage.md).