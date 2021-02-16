---
title: Aktualisieren von Data Quality Services | Microsoft-Dokumentation
description: Dieser Artikel enthält Informationen dazu, wie Sie ein Upgrade der vorhandenen Installation von SQL Server Data Quality Services (DQS) durchführen.
ms.custom: ''
ms.date: 06/29/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: f396666b-7754-4efc-9507-0fd114cc32d5
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 36d580e73939d99d8e4a08dc609a48d0390fac6e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341831"
---
# <a name="upgrade-data-quality-services"></a>Aktualisieren von Data Quality Services

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

Dieser Artikel enthält Informationen dazu, wie Sie ein Upgrade der vorhandenen Installation von [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] Data Quality Services (DQS) durchführen. Während des Upgrades des [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]-Data Quality-Servers muss auch das DQS-Datenbankschema aktualisiert werden.  
  
> [!IMPORTANT]
>  -   Sie müssen die DQS-Datenbanken sichern, bevor Sie DQS aktualisieren, um versehentliche Datenverluste während des Schemaupgrades zu verhindern. Informationen zum Sichern von DQS-Datenbanken finden Sie unter [Backing Up and Restoring DQS Databases](../../data-quality-services/backing-up-and-restoring-dqs-databases.md).  
> -   Um eine Verbindung mit dem [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]-Data Quality-Servers herzustellen und Data Quality-Aufgaben auszuführen, können Sie die aktuelle oder eine frühere Version des Data Quality-Clients oder die [DQS-Bereinigungstransformation](../../integration-services/data-flow/transformations/dqs-cleansing-transformation.md) in Integration Services verwenden.  
> -   Nachdem Data Quality Services und Master Data Services aktualisiert wurden, wird keine frühere Version des Master Data Services-Add-Ins für Excel mehr funktionieren. Sie können die [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] -Version des Master Data Services-Add-Ins für Excel [hier](../../master-data-services/master-data-services-installation-and-configuration.md)herunterladen.  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Voraussetzungen  
  
-   Sie müssen als Mitglied der Administratorgruppe auf dem [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] -Computer angemeldet sein.  
  
-   Ihr Windows-Benutzerkonto muss Mitglied der festen Serverrolle sysadmin auf der SQL Server-Instanz sein, auf der der [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] installiert ist.  
  
##  <a name="upgrading-dqs"></a><a name="Upgrade"></a> Aktualisieren von DQS  
 So aktualisieren Sie DQS  
  
1.  Sichern Sie die DQS-Datenbanken, bevor Sie den Upgradevorgang starten. Informationen zum Sichern von DQS-Datenbanken finden Sie unter [Backing Up and Restoring DQS Databases](../../data-quality-services/backing-up-and-restoring-dqs-databases.md).  
  
2.  Aktualisieren Sie die SQL Server-Instanz, auf der DQS installiert ist.  
  
    1.  Führen Sie den [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] -Setup-Assistenten aus.  
  
    2.  Klicken Sie im linken Bereich auf **Installation**.  
  
    3.  Klicken Sie im rechten Bereich auf **Upgrade von...** einer vorherigen Version von SQL Server.  
  
    4.  Schließen Sie den Setup-Assistenten ab.  
  
        > [!NOTE]  
        >  Auf diese Weise wird die SQL Server-Instanz auf [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] aktualisiert. Außerdem wird der neueste Data Quality-Client installiert, wenn zuvor ein Data Quality-Client auf dem Computer installiert war. Wenn auf anderen Computern ein Data Quality-Client installiert ist, müssen Sie auf diesen Computern die Teilschritte unter Schritt 2 ausführen, um die aktuelle Version des Data Quality-Clients zu installieren. Der Setup-Assistent installiert die aktuelle Version neben der vorhandenen Version des Data Quality-Clients. Nachdem Sie das DQS-Datenbankschema aktualisiert haben, können Sie mit der aktuellen oder einer früheren Version des Data Quality-Clients eine Verbindung mit der [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] -Version des Data Quality-Servers herstellen.  
  
3.  Aktualisieren Sie das DQS-Datenbankschema.  
  
    1.  Starten Sie eine -Eingabeaufforderung als Administrator.  
  
    2.  Wechseln Sie an der Eingabeaufforderung zu dem Verzeichnis, in dem DQSInstaller.exe enthalten ist. Bei Verwendung der Standardinstanz von SQL Server steht die Datei DQSInstaller.exe unter C:\Programme\Microsoft SQL Server\MSSQL[nn].MSSQLSERVER\MSSQL\Binn zur Verfügung:  

        >[!NOTE]
        >Ersetzen Sie [nn] im Ordnerpfad mit der [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]-Versionsnummer.
        >- Für SQL Server 2016: 13
        >- Für SQL Server 2017: 14
    
        ```console
        cd C:\Program Files\Microsoft SQL Server\MSSQL[nn].MSSQLSERVER\MSSQL\Binn  
        ```  
  
    3.  Geben Sie an der Eingabeaufforderung den folgenden Befehl ein, und drücken Sie die EINGABETASTE:  
  
        ```console
        dqsinstaller.exe -upgrade  
        ```  
  
    4.  Sie werden vom Installationsprogramm aufgefordert, die DQS-Datenbanken zu sichern, bevor Sie den Vorgang fortsetzen. Wenn Sie die DQS-Datenbanken bereits gesichert haben, geben Sie **J** oder **Ja** ein und drücken die EINGABETASTE, um das Upgrade fortzusetzen.  
  
    5.  Nach dem erfolgreichen Upgrade des DQS-Datenbankschemas wird eine Abschlussmeldung angezeigt.  
  
##  <a name="verifying-the-dqs-databases-schema-upgrade"></a><a name="Verify"></a> Überprüfen des Upgrades des DQS-Datenbankschemas  
 Um sicherzustellen, dass das DQS-Datenbankschema erfolgreich aktualisiert wurde, können Sie die aktuelle Version in den Datenbanken DQS_MAIN und DQS_PROJECTS überprüfen, indem Sie die A_DB_VERSION-Tabelle in der jeweiligen Datenbank abfragen. Gehen Sie folgendermaßen vor:  
  
1.  Starten Sie SQL Server Management Studio, und stellen Sie eine Verbindung mit der SQL Server-Instanz her, die das aktualisierte DQS-Datenbankschema enthält.  
  
2.  Führen Sie die folgende Abfrage aus:  
  
    ```sql
    SELECT * FROM DQS_MAIN.dbo.A_DB_VERSION WHERE STATUS=2;  
    SELECT * FROM DQS_PROJECTS.dbo.A_DB_VERSION WHERE STATUS=2;  
    ```  
  
3.  In der Ausgabe wird ein Eintrag für jedes Upgrade zusammen mit dem Upgradedatum angezeigt. Der höchste VERSION_ID- und ASSEMBLY_VERSION-Wert zum letzten angegebenen Datum entspricht der aktuellen Version. Der Wert 2 in der STATUS-Spalte gibt die erfolgreiche Ausführung an. Falls ein Fehler aufgetreten ist, wird dieser in der ERROR-Spalte aufgeführt. Beispiel für eine Ausgabe:  
  
    |id|UPGRADE_DATE|VERSION_ID|ASSEMBLY_VERSION|USER_NAME|STATUS|ERROR|  
    |--------|-------------------|-----------------|-----------------------|----------------|------------|-----------|  
    |1000|2013-08-11 05:26:39.567|1200|11.0.3000.0|\<DOMAIN\UserName>|2||  
    |1001|2013-09-19 15:09:37.750|1600|12.0.xxxx.0|\<DOMAIN\UserName>|2||  
  
## <a name="see-also"></a>Weitere Informationen  
 [Installieren von Data Quality Services](../../data-quality-services/install-windows/install-data-quality-services.md)   
 [Entfernen von Data Quality Server-Objekten](../../sql-server/install/remove-data-quality-server-objects.md)   
 [Aktualisieren von SQL Server](../../database-engine/install-windows/upgrade-sql-server.md)  
  
