---
title: Ausführen von SSIS-Paketen in Azure | Microsoft-Dokumentation
description: Übersicht über die verfügbaren Methoden zum Ausführen von SSIS-Paketen, die für Azure SQL-Datenbank bereitgestellt sind.
ms.date: 05/29/2018
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: swinarko
ms.author: sawinark
ms.reviewer: maghan
ms.openlocfilehash: 2c7209d43628296169e973012ea459296acc3549
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351968"
---
# <a name="run-sql-server-integration-services-ssis-packages-deployed-in-azure"></a>Ausführen von in Azure bereitgestellten SSIS-Paketen

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



Sie können SSIS-Pakete ausführen, die im SSISDB-Katalog eines Azure SQL-Datenbank-Servers bereitgestellt sind, indem Sie eine der in diesem Artikel beschriebenen Methoden verwenden. Sie können ein Paket direkt oder als Teil einer Azure Data Factory-Pipeline ausführen. Eine Übersicht über SSIS in Azure finden Sie unter [Bereitstellen und Ausführen von SSIS-Paketen in Azure](ssis-azure-lift-shift-ssis-packages-overview.md).

- Direktes Ausführen eines Pakets

  - [Ausführen mit SSMS](#ssms)

  - [Ausführen mit gespeicherten Prozeduren](#sproc)

  - [Ausführen mit Skript oder Code](#script)

- Ausführen eines Pakets als Teil einer Azure Data Factory-Pipeline

  - [Ausführen mit der Aktivität „SSIS-Paket ausführen“](#exec_activity)

  - [Ausführen mit der Aktivität „Gespeicherte Prozedur“](#sproc_activity)

> [!NOTE]
> Das Ausführen eines Pakets mit `dtexec.exe` wurde bei in Azure bereitgestellten Paketen nicht getestet.

## <a name="run-a-package-with-ssms"></a><a name="ssms"></a> Ausführen eines Pakets mit SSMS

In SQL Server Management Studio (SSMS) können Sie mit der rechten Maustaste auf ein Paket klicken, das in der SSIS-Katalogdatenbank SSISDB bereitgestellt wurde, und **Ausführen** auswählen, um das Dialogfeld **Paket ausführen** zu öffnen. Weitere Informationen finden Sie unter [Run an SSIS package with SQL Server Management Studio (SSMS) (Ausführen eines SSIS-Pakets mit SQL Server Management Studio (SSMS))](../ssis-quickstart-run-ssms.md).

## <a name="run-a-package-with-stored-procedures"></a><a name="sproc"></a> Ausführen eines Pakets mit gespeicherten Prozeduren

Sie können in jeder Umgebung, in der Sie eine Verbindung mit Azure SQL-Datenbank herstellen und Transact-SQL-Code ausführen können, ein Paket ausführen, indem Sie folgende gespeicherte Prozeduren aufrufen:

1. **[catalog].[create_execution]** . Weitere Informationen finden Sie unter [catalog.create_execution](../system-stored-procedures/catalog-create-execution-ssisdb-database.md).

2. **[catalog].[set_execution_parameter_value]** . Weitere Informationen finden Sie unter [catalog.set_execution_parameter_value](../system-stored-procedures/catalog-set-execution-parameter-value-ssisdb-database.md).

3. **[catalog].[start_execution]** Weitere Informationen finden Sie unter [catalog.start_execution](../system-stored-procedures/catalog-start-execution-ssisdb-database.md).

Weitere Informationen finden Sie in den folgenden Beispielen:

- [Ausführen eines SSIS-Pakets aus SSMS mit Transact-SQL](../ssis-quickstart-run-tsql-ssms.md)

- [Ausführen eines SSIS-Pakets aus Visual Studio Code mit Transact-SQL](../ssis-quickstart-run-tsql-vscode.md)

## <a name="run-a-package-with-script-or-code"></a><a name="script"></a> Ausführen eines Pakets mit Skript oder Code

Sie können in jeder Entwicklungsumgebung, in der Sie eine verwaltete API aufrufen können, ein Paket ausführen, indem Sie die Methode `Execute` des Objekts `Package` im Namespace `Microsoft.SQLServer.Management.IntegrationServices` aufrufen.

Weitere Informationen finden Sie in den folgenden Beispielen:

- [Run an SSIS package with PowerShell](../ssis-quickstart-run-powershell.md) (Ausführen eines SSIS-Pakets mit PowerShell)

- [Ausführen eines SSIS-Pakets mit C#-Code in einer .NET-App](../ssis-quickstart-run-dotnet.md)

## <a name="run-a-package-with-the-execute-ssis-package-activity"></a><a name="exec_activity"></a> Ausführen eines Pakets mit der Aktivität „SSIS-Paket ausführen“

Weitere Informationen finden Sie unter [Ausführen eines SSIS-Pakets mit der Aktivität „SSIS-Paket ausführen“ in Azure Data Factory](/azure/data-factory/how-to-invoke-ssis-package-ssis-activity).

## <a name="run-a-package-with-the-stored-procedure-activity"></a><a name="sproc_activity"></a> Ausführen eines Pakets mit der Aktivität „Gespeicherte Prozedur“

Weitere Informationen finden Sie unter [Ausführen eines SSIS-Pakets mit der Aktivität „Gespeicherte Prozedur“ in Azure Data Factory](/azure/data-factory/how-to-invoke-ssis-package-stored-procedure-activity).

## <a name="next-steps"></a>Nächste Schritte

In diesem Abschnitt erhalten Sie Informationen zu Optionen für die Planung von in Azure bereitgestellten SSIS-Paketen. Weitere Informationen finden Sie unter [Planen der Ausführung eines SSIS-Pakets in Azure](ssis-azure-schedule-packages.md).