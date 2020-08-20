---
description: Datenquelle auswählen (SQL Server-Import/Export-Assistent)
title: Auswählen einer Datenquelle (SQL Server-Import/Export-Assistent) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/28/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.impexpwizard.chooseadatasource.f1
ms.assetid: ebf28a62-dfc1-4b39-9db5-df1919e5fccb
author: chugugrace
ms.author: chugu
ms.openlocfilehash: fa0d5c4164af7f6cc6e0c62987a7fab3d5ed3405
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88484143"
---
# <a name="choose-a-data-source-sql-server-import-and-export-wizard"></a>Datenquelle auswählen (SQL Server-Import/Export-Assistent)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Nach der Begrüßungsseite zeigt der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Import/Export-Assistent die Seite **Datenquelle auswählen**an. Auf dieser Seite geben Sie Informationen zur Quelle für Ihre Daten an und darüber, wie Sie eine Verbindung mit dieser herstellen.
  
Weitere Informationen zu Datenquellen, die Sie verwenden können, finden Sie unter [Welche Datenquellen und -ziele kann ich verwenden?](../../integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard.md#wizardSources)

> [!NOTE]
> Der Import- und Export-Assistent von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nutzt SQL Server Integration Services (SSIS). Daher gelten dieselben Einschränkungen, die für SSIS gelten, auch für den Assistenten.  Z. B. die Spalten „ErrorCode“ und „ErrorColumn“, die standardmäßig hinzugefügt werden, wie in [Fehlerbehandlung in Daten](../../integration-services/data-flow/error-handling-in-data.md) beschrieben.

## <a name="screen-shot-of-the-choose-a-data-source-page"></a>Screenshot der Seite „Datenquelle auswählen“ 
Die folgende Abbildung zeigt den oberen Teil der Seite **Datenquelle auswählen** im Assistenten, der sich nicht ändert. Auf den restlichen Seiten finden Sie verschiedene Optionen, die von der von Ihnen ausgewählten Datenquelle abhängig sind.

![Quelle auswählen](../../integration-services/import-export-data/media/choose-source.png)

## <a name="choose-a-data-source"></a>Datenquelle auswählen
 **Datenquelle**  
Geben Sie die Datenquelle an, indem Sie einen Datenanbieter auswählen, der eine Verbindung mit der Quelle herstellen kann.

-   **In der Regel können Sie anhand des jeweiligen Namens erkennen, welchen Datenanbieter Sie benötigen**, da der Name des Anbieters für gewöhnlich den Namen der Datenquelle enthält, z.B. *Flatfile*-Quelle, Microsoft *Excel*, Microsoft *Access*, .NET Framework-Datenanbieter für *SQL Server* und .NET Framework-Datenanbieter für *Oracle*.

-   **Wenn Sie über einen ODBC-Treiber für Ihre Datenquelle verfügen**, wählen Sie den .NET Framework-Datenanbieter für ODBC aus. Geben Sie dann die für den Treiber spezifischen Informationen ein. ODBC-Treiber werden in der Dropdownliste der Datenquellen nicht aufgeführt. Der .NET Framework-Datenanbieter für ODBC fungiert als Wrapper für den ODBC-Treiber. Weitere Informationen finden Sie unter [Connect to an ODBC Data Source (SQL Server Import and Export Wizard)](../../integration-services/import-export-data/connect-to-an-odbc-data-source-sql-server-import-and-export-wizard.md) (Herstellen einer Verbindung mit einer ODBC-Datenquelle [SQL Server-Import/Export-Assistent]).

-   **Möglicherweise sind für die Datenquelle mehrere Anbieter verfügbar.** In der Regel können Sie alle Anbieter auswählen, die für Ihre Quelle verwendet werden können. Sie können z.B. den .NET Framework-Datenanbieter für SQL Server oder den SQL Server-ODBC-Treiber verwenden, um eine Verbindung mit Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] herzustellen. (Andere Anbieter werden zwar noch in der Liste aufgeführt, aber nicht mehr unterstützt.) 

## <a name="my-data-source-isnt-in-the-list"></a>Die Datenquelle befindet sich nicht in der Liste
-   **Sie müssen möglicherweise den Datenanbieter** von Microsoft oder einem Drittanbieter herunterladen. Die Liste der verfügbaren Anbieter in der Liste **Datenquelle** beinhaltet nur die auf Ihrem Computer installierten Anbieter. Weitere Informationen zu Datenquellen, die Sie verwenden können, finden Sie unter [Welche Datenquellen und -ziele kann ich verwenden?](import-and-export-data-with-the-sql-server-import-and-export-wizard.md#wizardSources)

-   **Ist ein ODBC-Treiber für die Datenquelle installiert?** ODBC-Treiber werden in der Dropdownliste der Datenquellen nicht aufgeführt. Wenn Sie beispielsweise über einen ODBC-Treiber für Ihre Datenquelle verfügen, wählen Sie den .NET Framework-Datenanbieter für ODBC aus. Geben Sie dann die für den Treiber spezifischen Informationen ein. Der .NET Framework-Datenanbieter für ODBC fungiert als Wrapper für den ODBC-Treiber. Weitere Informationen finden Sie unter [Connect to an ODBC Data Source (SQL Server Import and Export Wizard)](../../integration-services/import-export-data/connect-to-an-odbc-data-source-sql-server-import-and-export-wizard.md) (Herstellen einer Verbindung mit einer ODBC-Datenquelle [SQL Server-Import/Export-Assistent]).

-   **64-Bit- und 32-Bit-Anbieter.** Wenn Sie den 64-Bit-Assistenten ausführen, werden Ihnen keine Datenquellen angezeigt, für die nur ein 32-Bit-Anbieter installiert ist (und umgekehrt).

> [!NOTE]
> Sie müssen SQL Server installieren, um die 64-Bit-Version des SQL Server-Import/Export-Assistenten verwenden zu können. SQL Server Data Tools (SSDT) und SQL Server Management Studio (SSMS) sind 32-Bit-Anwendungen und installieren daher auch nur 32-Bit-Dateien, einschließlich der 32-Bit-Version des Assistenten.

## <a name="after-you-choose-a-data-source"></a>Nach dem Auswählen der Datenquelle
Nachdem Sie eine Datenquelle ausgewählt haben, stehen Ihnen auf der Seite **Datenquelle auswählen** verschiedene Optionen zur Verfügung, die vom Datenanbieter abhängig sind, den Sie auswählen.

Sehen Sie sich eine der folgenden Seiten an, um eine Verbindung mit einer häufig verwendeten Datenquelle herzustellen.
-   [Connect to SQL Server (Herstellen einer Verbindung mit SQL Server)](../../integration-services/import-export-data/connect-to-a-sql-server-data-source-sql-server-import-and-export-wizard.md)
-   [Herstellen einer Verbindung mit Oracle](../../integration-services/import-export-data/connect-to-an-oracle-data-source-sql-server-import-and-export-wizard.md)
-   [Connect to flat files (text files)](../../integration-services/import-export-data/connect-to-a-flat-file-data-source-sql-server-import-and-export-wizard.md) (Herstellen einer Verbindung mit Flatfiles [Textdateien])
-   [Connect to Excel](../../integration-services/import-export-data/connect-to-an-excel-data-source-sql-server-import-and-export-wizard.md) (Herstellen einer Verbindung mit Excel)
-   [Herstellen einer Verbindung mit Access](../../integration-services/import-export-data/connect-to-an-access-data-source-sql-server-import-and-export-wizard.md)
-   [Herstellen einer Verbindung mit ODBC](../../integration-services/import-export-data/connect-to-an-odbc-data-source-sql-server-import-and-export-wizard.md)
-   [Herstellen einer Verbindung mit Azure Blob Storage](../../integration-services/import-export-data/connect-to-azure-blob-storage-sql-server-import-and-export-wizard.md)
-   [Herstellen einer Verbindung mit PostgreSQL](../../integration-services/import-export-data/connect-to-a-postgresql-data-source-sql-server-import-and-export-wizard.md)
-   [Herstellen einer Verbindung mit MySQL](../../integration-services/import-export-data/connect-to-a-mysql-data-source-sql-server-import-and-export-wizard.md)

Weitere Informationen zum Herstellen einer Verbindung mit einer Datenquelle, die hier nicht aufgeführt ist, finden Sie unter [The Connection Strings Reference (Verweis auf Verbindungszeichenfolgen)](https://www.connectionstrings.com/). Auf dieser Website eines Drittanbieters sind Beispielverbindungszeichenfolgen aufgelistet, und es werden Angaben zu Datenanbietern und den erforderlichen Verbindungsinformationen bereitgestellt.

## <a name="whats-next"></a>Nächste Schritte
 Nachdem Sie Informationen zur Quelle Ihrer Daten bereitgestellt haben und darüber, wie Sie eine Verbindung mit ihnen herstellen, wird als nächste Seite **Ziel auswählen**angezeigt. Auf dieser Seite geben Sie Informationen zum Ziel für Ihre Daten an und dazu, wie Sie eine Verbindung damit herstellen. Weitere Informationen finden Sie unter [Ziel auswählen](../../integration-services/import-export-data/choose-a-destination-sql-server-import-and-export-wizard.md).

## <a name="see-also"></a>Weitere Informationen
[Erste Schritte mit diesem einfachen Beispiel des Import/Export-Assistenten](../../integration-services/import-export-data/get-started-with-this-simple-example-of-the-import-and-export-wizard.md)

[!INCLUDE[get-help-options](../../includes/paragraph-content/get-help-options.md)]

[!INCLUDE[contribute-to-content](../../includes/paragraph-content/contribute-to-content.md)]
