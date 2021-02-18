---
title: Leitfaden zur Verwendung von SQL Server Transparent Data Encryption(transparente Datenverschlüsselung, TDE) für Big Data-Cluster
titleSuffix: SQL Server Big Data Clusters
description: In diesem Artikel wird gezeigt, wie Sie die SQL Server TDE-Verschlüsselungsfunktion für ruhende Daten von BDC verwenden.
author: DaniBunny
ms.author: dacoelho
ms.reviewer: mihaelab
ms.date: 10/19/2020
ms.topic: tutorial
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 7d249f9b70479603ad66e6b68cec784b1476324a
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489314"
---
# <a name="sql-server-big-data-clusters-transparent-data-encryption-tde-at-rest-usage-guide"></a>Leitfaden zur Verwendung von SQL Server Transparent Data Encryption(transparente Datenverschlüsselung, TDE) für Big Data-Cluster

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

In diesem Leitfaden wird veranschaulicht, wie Sie Verschlüsselungsfunktionen für ruhende Daten von SQL Server Big Data-Clustern verwenden, um Datenbanken zu verschlüsseln.

Die Erfahrung ist im allgemeinen identisch mit der für SQL Server für Linux, und die [TDE-Standarddokumentation](../relational-databases/security/encryption/transparent-data-encryption.md) ist gültig, sofern nicht anders erwähnt. Um den Status der Verschlüsselung der Masterdatenbank zu überwachen, befolgen Sie die standardmäßigen DMV-Abfragemuster, zusätzlich zu [`sys.dm_database_encryption_keys`](../relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql.md) und [`sys.certificates`](../relational-databases/system-catalog-views/sys-certificates-transact-sql.md).

__Nicht unterstützte Funktionen:__
* Datenpoolverschlüsselung

## <a name="prerequisites"></a><a id="prereqs"></a> Voraussetzungen

- [SQL Server Big Data-Cluster CU8+](release-notes-big-data-cluster.md)
- [Big-Data-Tools](deploy-big-data-tools.md)
   - **Azure Data Studio**
- SQL Server-Benutzer mit Administratorrechten.

## <a name="query-the-installed-certificates"></a>Abfragen der installierten Zertifikate

1. Stellen Sie in Azure Data Studio eine Verbindung mit der SQL Server-Masterinstanz Ihres Big Data-Clusters her. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit der SQL Server-Masterinstanz](connect-to-big-data-cluster.md#master).

1. Doppelklicken Sie im Fenster **Server** auf die Verbindung, um das Serverdashboard der SQL Server-Masterinstanz anzuzeigen. Wählen Sie **Neue Abfrage** aus.

   ![Abfrage der SQL Server-Masterinstanz](./media/tutorial-data-pool-ingest-sql/sql-server-master-instance-query.png)

1. Führen Sie den folgenden Transact-SQL-Befehl aus, um den Kontext in der Masterinstanz in die **Master** datenbank zu ändern.

   ```sql
   USE master
   GO
   ```

1. Fragen Sie die installierten, systemseitig verwalteten Zertifikate ab. 

   ```sql
    SELECT TOP 1 name FROM sys.certificates WHERE name LIKE 'TDECertificate%' ORDER BY name DESC
   ```

    Verwenden Sie bei Bedarf andere Abfragekriterien.

    Der Zertifikatname wird als „TDECertificate{timestamp}“ aufgeführt. Wenn Sie das Präfix „TDECertificate“, gefolgt von einem Zeitstempel, sehen, handelt es sich dabei um das von der systemseitig verwalteten Funktion bereitgestellte Zertifikat.

## <a name="encrypt-a-database-using-the-system-provided-certificate"></a>Verschlüsseln einer Datenbank mithilfe des systemseitig bereitgestellten Zertifikats

In den folgenden Beispielen wird eine Datenbank mit dem Namen __userdb__ als Ziel für die Verschlüsselung verwendet und ein systemseitig bereitgestelltes Zertifikat mit dem Namen __TDECertificate2020_09_15_22_46_27__ als Ausgabe des vorherigen Abschnitts.

1. Verwenden Sie das folgende Muster, um unter Verwendung des bereitgestellten Systemzertifikats einen Verschlüsselungsschlüssel für die Datenbank zu generieren.

   ```sql
    USE userdb; 
    GO
    CREATE DATABASE ENCRYPTION KEY WITH ALGORITHM = AES_256 ENCRYPTION BY SERVER CERTIFICATE TDECertificate2020_09_15_22_46_27;
    GO
   ```

1. Verschlüsseln Sie die Datenbank __userdb__ mit dem folgenden Befehl.

   ```sql
    ALTER DATABASE userdb SET ENCRYPTION ON;
    GO
   ```

## <a name="next-steps"></a>Nächste Schritte

Informationen zur Verschlüsselung ruhender Daten für HDFS:
> [!div class="nextstepaction"]
> [HDFS-Verschlüsselungszonen](encryption-at-rest-hdfs-encryption-zones.md)
