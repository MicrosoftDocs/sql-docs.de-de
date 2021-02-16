---
title: Herstellen einer Verbindung mit dem SSIS-Katalog (SSISDB) in Azure | Microsoft-Dokumentation
description: Finden Sie Verbindungsinformationen, die benötigt werden, um eine Verbindung mit dem auf einem Azure SQL-Datenbank-Server gehosteten SSIS-Katalog (SSISDB) herzustellen.
ms.date: 09/23/2018
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: swinarko
ms.author: sawinark
ms.reviewer: maghan
ms.openlocfilehash: e5ce8d83ba29843cc14c6798eb4f550eab62e557
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352011"
---
# <a name="connect-to-the-ssis-catalog-ssisdb-in-azure"></a>Herstellen einer Verbindung mit dem SSIS-Katalog (SSISDB) in Azure

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



Finden Sie Verbindungsinformationen, die benötigt werden, um eine Verbindung mit dem auf einem Azure SQL-Datenbank-Server gehosteten SSIS-Katalog (SSISDB) herzustellen. Sie benötigen die folgenden Elemente, um eine Verbindung herzustellen:
- Vollqualifizierten Servernamen
- Datenbanknamen
- Anmeldeinformationen 

> [!IMPORTANT]
> Zurzeit kann die SSISDB-Katalogdatenbank in Azure SQL-Datenbank nur zusammen mit der Azure SSIS Integration Runtime in Azure Data Factory erstellt werden. Die Azure SSIS IR ist die Laufzeitumgebung, die SSIS-Pakete auf Azure ausführt. Einen exemplarischen Prozess finden Sie unter [Bereitstellen und Ausführen von SSIS-Paketen in Azure](/azure/data-factory/tutorial-create-azure-ssis-runtime-portal). 

## <a name="prerequisites"></a>Voraussetzungen
Prüfen Sie, ob Sie über die Version 17.2 oder höher von SQL Server Management Studio (SSMS) verfügen, bevor Sie beginnen. Wenn die SSISDB-Katalogdatenbank auf SQL Managed Instance gehostet wird, stellen Sie sicher, dass Sie die Version 17.6 oder höher von SSMS verwenden. Die neueste Version von SSMS können Sie unter [Herunterladen von SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md) herunterladen.

## <a name="get-the-connection-info-from-the-azure-portal"></a>Abrufen der Verbindungsinformationen vom Azure-Portal
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Wählen Sie im Azure-Portal im Menü auf der linken Seite **SQL-Datenbanken** aus, und wählen Sie dann auf der Seite **SQL-Datenbanken** die `SSISDB`-Datenbank aus. 
3. Überprüfen Sie auf der Seite **Übersicht** Ihrer `SSISDB`-Datenbank den vollqualifizierten Servernamen, wie in der folgenden Abbildung gezeigt wird. Zeigen Sie auf den Servernamen, damit die Option **Klicken Sie zum Kopieren** angezeigt wird.

    ![Serververbindungsinformationen](media/ssis-azure-connect-to-catalog-database/server-name.png) 

4. Wenn Sie die Anmeldeinformationen für den SQL-Datenbankserver vergessen haben, navigieren Sie zur Seite „SQL-Datenbankserver“. Dort können Sie den Namen des Serveradministrators einsehen und ggf. das Kennwort zurücksetzen.

## <a name="connect-with-ssms"></a>Herstellen einer Verbindung mit SSMS
1. Öffnen Sie SQL Server Management Studio.

2. **Stellen Sie eine Verbindung mit dem Server her**. Geben Sie im Dialogfeld **Mit Server verbinden** die folgenden Informationen ein:

   | Einstellung       | Vorgeschlagener Wert | BESCHREIBUNG | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Servertyp** | Datenbank-Engine | Dieser Wert ist erforderlich. |
   | **Servername** | Der vollqualifizierte Servername | Der Name muss das folgende Format aufweisen: **mysqldbserver.database.windows.net**. |
   | **Authentifizierung** | SQL Server-Authentifizierung | |
   | **Anmeldung** | Das Serveradministratorkonto | Hierbei handelt es sich um das Konto, das Sie beim Erstellen des Servers angegeben haben. |
   | **Kennwort** | Das Kennwort für das Serveradministratorkonto | Hierbei handelt es sich um das Kennwort, das Sie beim Erstellen des Servers angegeben haben. |

    ![Herstellen einer Verbindung mit dem Server mit SSMS](media/ssis-azure-connect-to-catalog-database/ssisdb-connect-1.png)

3. **Stellen Sie eine Verbindung mit der SSIS-Datenbank her**. Wählen Sie **Optionen**  aus, um das Dialogfeld **Mit Server verbinden** zu erweitern. Wählen Sie im erweiterten Dialogfeld **Mit Server verbinden** die Registerkarte **Verbindungseigenschaften** aus. Wählen Sie `SSISDB` im Feld **Mit Datenbank verbinden** aus, oder geben Sie es ein.

    > [!IMPORTANT]
    > Wenn Sie beim Herstellen einer Verbindung nicht `SSISDB` auswählen, wird der SSIS-Katalog möglicherweise nicht im Objekt-Explorer angezeigt.

    ![Auswählen der SSISDB-Datenbank für die Verbindung](media/ssis-azure-connect-to-catalog-database/ssisdb-connect-2.png)

4. Wählen Sie dann **Verbinden** aus.

5. Erweitern Sie im Objekt-Explorer **Integration Services-Kataloge** und dann **SSISDB**, um die Objekte in der SSIS-Katalogdatenbank anzuzeigen.

    ![Suchen der SSISDB-Datenbank im Objekt-Explorer in SSMS](media/ssis-azure-connect-to-catalog-database/ssisdb-connect-3.png)

## <a name="next-steps"></a>Nächste Schritte
- Stellen Sie ein Paket bereit. Weitere Informationen finden Sie unter [Deploy an SSIS project with SQL Server Management Studio (SSMS) (Bereitstellen eines SSIS-Projekts mit SQL Server Management Studio (SSMS))](../ssis-quickstart-deploy-ssms.md).
- Führen Sie ein Paket aus. Weitere Informationen finden Sie unter [Run an SSIS package with SQL Server Management Studio (SSMS) (Ausführen eines SSIS-Pakets mit SQL Server Management Studio (SSMS))](../ssis-quickstart-run-ssms.md).
- Planen Sie ein Paket. Weitere Informationen finden Sie unter [Planen der Ausführung eines SSIS-Pakets in Azure](ssis-azure-schedule-packages.md).