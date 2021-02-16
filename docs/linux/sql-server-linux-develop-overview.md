---
title: Entwickeln von Apps für SQL Server für Linux
description: Sie können Apps erstellen, die SQL Server für Linux mit einer Vielzahl von Programmiersprachen und beliebten Webframeworks verwenden und eine Verbindung herstellen.
author: VanMSFT
ms.author: vanto
ms.date: 11/17/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 758cb738-b018-465b-9ab0-59a24b892e66
ms.openlocfilehash: 01655b948afa721365b7ea7adebd1e22f8f6f851
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100273081"
---
# <a name="how-to-get-started-developing-applications-for-sql-server-on-linux"></a>Erste Schritte bei der Entwicklung von Apps für SQL Server für Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Sie können Apps erstellen, die SQL Server für Linux mit einer Vielzahl von Programmiersprachen, wie C#, Java, Node.js, PHP, Python, Ruby und C++, verwenden und sich damit verbinden. Sie können auch beliebte Webframeworks und ORM-Frameworks (Object Relational Mapping, objektrelationale Abbildung) nutzen.

> [!VIDEO https://channel9.msdn.com/events/Connect/2017/T153/player]

> [!TIP]
> Mithilfe dieser Entwicklungsoptionen können Sie SQL Server auch auf anderen Plattformen verwenden. Apps können SQL Server lokal oder in der Cloud, unter Linux, unter Windows oder in Docker unter macOS ausführen. Sie können auch Azure SQL-Datenbank und Azure Synapse Analytics verwenden.

## <a name="try-the-tutorials"></a>Ausprobieren der Tutorials

Am besten steigen Sie in das Erstellen von Anwendungen mit SQL Server ein, indem Sie es einfach ausprobieren.

- Navigieren Sie zu den [Tutorials für die ersten Schritte](https://aka.ms/sqldev).
- Wählen Sie Ihre Sprache und Entwicklungsplattform aus.
- Testen Sie die Codebeispiele.

> [!TIP]
> Weitere Informationen zur Entwicklung für SQL Server in Docker finden Sie in den Tutorials von **macOS**.

## <a name="create-new-applications"></a>Erstellen neuer Apps

Wenn Sie eine neue App erstellen, sehen Sie sich eine Liste der [Konnektivitätsbibliotheken](sql-server-linux-develop-connectivity-libraries.md) an, um eine Zusammenfassung der Connectors und beliebten Frameworks zu erhalten, die für verschiedene Programmiersprachen verfügbar sind.

## <a name="use-existing-applications"></a>Verwenden vorhandener Apps

Wenn Sie bereits eine Datenbank-App haben, können Sie die Verbindungszeichenfolge einfach so ändern, dass SQL Server für Linux verwendet wird. Lesen Sie sich den Abschnitt [Bekannte Probleme](sql-server-linux-release-notes.md) in SQL Server für Linux durch.

## <a name="use-existing-sql-tools-on-windows-with-sql-server-on-linux"></a>Verwenden vorhandener SQL-Tools unter Windows mit SQL Server für Linux

Tools, die derzeit unter Windows ausgeführt werden, wie SSMS, SSDT und PowerShell, funktionieren auch mit SQL Server für Linux. Obwohl sie nicht nativ unter Linux ausgeführt werden, können Sie weiterhin SQL Server-Remoteinstanzen unter Linux verwalten. 

Weitere Informationen finden Sie in den nachfolgenden Themen:

- [SQL Server Management Studio (SSMS)](sql-server-linux-manage-ssms.md)
- [SQL Server Data Tools (SSDT)](sql-server-linux-develop-use-ssdt.md)
- [SQL PowerShell](sql-server-linux-manage-powershell.md)

> [!Note]
> Vergewissern Sie sich, dass Sie die neuesten Versionen dieser Tools verwenden.

## <a name="use-new-sql-tools-for-linux"></a>Verwenden neuer SQL-Tools für Linux

Sie können die neue [mssql-Erweiterung](https://aka.ms/mssql-marketplace) für [Visual Studio Code](https://code.visualstudio.com) unter Linux, macOS und Windows verwenden. Eine detaillierte Anleitung finden Sie im folgenden Tutorial:

- [Verwenden von Visual Studio Code](../tools/visual-studio-code/sql-server-develop-use-vscode.md)

Sie können auch neue Befehlszeilentools verwenden, die für Linux nativ sind. Hierzu gehören die folgenden Tools:

- [sqlcmd](../tools/sqlcmd-utility.md)
- [bcp](sql-server-linux-migrate-bcp.md)
- [mssql-conf](sql-server-linux-configure-mssql-conf.md)

## <a name="next-steps"></a>Nächste Schritte

Installieren Sie zunächst SQL Server für Linux mithilfe einer der nachfolgenden Schnellstarts:

- [Installation unter Red Hat Enterprise Linux](quickstart-install-connect-red-hat.md)
- [Installation unter SUSE Linux Enterprise Server](quickstart-install-connect-suse.md)
- [Installation unter Ubuntu](quickstart-install-connect-ubuntu.md)
- [Ausführung in Docker](quickstart-install-connect-ubuntu.md)