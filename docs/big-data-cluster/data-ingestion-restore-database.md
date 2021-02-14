---
title: Wiederherstellen einer Datenbank
titleSuffix: SQL Server big data clusters
description: In diesem Artikel ist beschrieben, wie eine Datenbank in der Masterinstanz eines Big Data-Clusters für SQL Server 2019 wiederhergestellt wird.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 08/21/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 3082c970c648fddf67b635ba7cbae5b19bfc0170
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049020"
---
# <a name="restore-a-database-into-the-sql-server-big-data-cluster-master-instance"></a>Wiederherstellen einer Datenbank in der Masterinstanz eines Big Data-Clusters für SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

In diesem Artikel wird beschrieben, wie eine vorhandene Datenbank in der Masterinstanz eines [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] wiederhergestellt wird. Die empfohlene Methode besteht darin, einen Ansatz mit Sichern, Kopieren und Wiederherstellen zu verwenden.

## <a name="backup-your-existing-database"></a>Sichern Ihrer vorhandenen Datenbank

Sichern Sie zunächst Ihre vorhandene SQL Server-Datenbank aus SQL Server für Windows oder Linux. Verwenden Sie Standardsicherungsverfahren mit Transact-SQL oder mit einem Tool wie SQL Server Management Studio (SSMS).

In diesem Artikel ist gezeigt, wie Sie die AdventureWorks-Datenbank wiederherstellen, Sie können aber jede beliebige Datenbanksicherung verwenden. 

> [!TIP]
> Laden Sie die [AdventureWorks-Sicherung](../samples/adventureworks-install-configure.md) herunter.

## <a name="copy-the-backup-file"></a>Kopieren der Sicherungsdatei

Kopieren Sie die Sicherungsdatei in den SQL Server-Container im Masterinstanzpod des Kubernetes-Clusters.

```bash
kubectl cp <path to .bak file> master-0:/var/tmp/<.bak filename> -c mssql-server -n <name of your big data cluster>
```

Beispiel:

```bash
kubectl cp ~/Downloads/AdventureWorks2016CTP3.bak master-0:/var/tmp/AdventureWorks2016CTP3.bak -c mssql-server -n clustertest
```

Vergewissern Sie sich anschließend, dass die Sicherungsdatei in den Podcontainer kopiert wurde.

```bash
kubectl exec -it master-0 -n <name of your big data cluster> -c mssql-server -- bin/bash
cd /var/
ls /tmp
exit
```

Beispiel:

```bash
kubectl exec -it master-0 -n clustertest -c mssql-server -- bin/bash
cd /var/
ls /tmp
exit
```

## <a name="restore-the-backup-file"></a>Wiederherstellen der Sicherungsdatei

Stellen Sie im nächsten Schritt die Datenbanksicherung in der Masterinstanz von SQL Server wieder her.  Wenn Sie eine Datenbanksicherung wiederherstellen, die unter Windows erstellt wurde, müssen Sie die Namen der Dateien ermitteln.  Stellen Sie in Azure Data Studio eine Verbindung mit der Masterinstanz her, und führen Sie dieses SQL-Skript aus:

```sql
RESTORE FILELISTONLY FROM DISK='/tmp/<db file name>.bak'
```

Beispiel:

```sql
RESTORE FILELISTONLY FROM DISK='/tmp/AdventureWorks2016CTP3.bak'
```

![Sicherungsdateiliste](media/restore-database/database-restore-file-list.png)

Stellen Sie die Datenbank nun wieder her. Das folgende Skript ist ein Beispiel. Ersetzen Sie die Namen/Pfade entsprechend den Anforderungen Ihrer Datenbanksicherung.

```sql
RESTORE DATABASE AdventureWorks2016CTP3
FROM DISK='/tmp/AdventureWorks2016CTP3.bak'
WITH MOVE 'AdventureWorks2016CTP3_Data' TO '/var/opt/mssql/data/AdventureWorks2016CTP3_Data.mdf',
        MOVE 'AdventureWorks2016CTP3_Log' TO '/var/opt/mssql/data/AdventureWorks2016CTP3_Log.ldf',
        MOVE 'AdventureWorks2016CTP3_mod' TO '/var/opt/mssql/data/AdventureWorks2016CTP3_mod'
```

## <a name="configure-data-pool-and-hdfs-access"></a>Konfigurieren von Datenpool- und HDFS-Zugriff

Nun führen Sie, damit die SQL Server-Masterinstanz auf Datenpools und HDFS zugreifen kann, die gespeicherten Prozeduren für den Datenpool und den Speicherpool aus. Führen Sie die folgenden Transact-SQL-Skripts für Ihre neu wiederhergestellte Datenbank aus:

```sql
USE AdventureWorks2016CTP3
GO
-- Create the SqlDataPool data source:
IF NOT EXISTS(SELECT * FROM sys.external_data_sources WHERE name = 'SqlDataPool')
  CREATE EXTERNAL DATA SOURCE SqlDataPool
  WITH (LOCATION = 'sqldatapool://controller-svc/default');

-- Create the SqlStoragePool data source:
IF NOT EXISTS(SELECT * FROM sys.external_data_sources WHERE name = 'SqlStoragePool')
   CREATE EXTERNAL DATA SOURCE SqlStoragePool
   WITH (LOCATION = 'sqlhdfs://controller-svc/default');
GO
```

> [!NOTE]
> Sie müssen diese Einrichtungsskripts nur für Datenbanken durchlaufen, die aus älteren Versionen von SQL Server wiederhergestellt wurden. Wenn Sie eine neue Datenbank in der SQL Server-Masterinstanz erstellen, sind die gespeicherten Prozeduren für den Datenpool und den Speicherpool bereits für Sie konfiguriert.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] finden Sie in der folgenden Übersicht:

- [Was sind [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md)
