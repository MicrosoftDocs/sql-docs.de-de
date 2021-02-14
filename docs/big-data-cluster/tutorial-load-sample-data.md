---
title: Laden von Beispieldaten
titleSuffix: SQL Server big data clusters
description: Dieses Tutorial veranschaulicht das Laden von Beispieldaten in einen Big Data-Cluster für SQL Server. Die Beispieldaten beinhalten relationale Daten in der SQL Server-Masterinstanz. Sie beinhalten auch HDFS-Daten im Speicherpool. Diese Daten unterstützen andere Tutorials in diesem Abschnitt.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 08/21/2019
ms.topic: tutorial
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 9b4e7667a3f109a0b5cca2f0af60af38613c5ce8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045653"
---
# <a name="tutorial-load-sample-data-into-a-sql-server-big-data-cluster"></a>Tutorial: Laden von Beispieldaten in einen Big Data-Cluster für SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

In diesem Tutorial wird erläutert, wie Sie ein Skript zum Laden von Beispieldaten in einen [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] verwenden. In vielen anderen Tutorials der Dokumentation werden diese Beispieldaten verwendet.

> [!TIP]
> Weitere Beispiele für [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] finden Sie im GitHub-Repository [sql-server-samples](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster). Sie befinden sich im Pfad **sql-server-samples/samples/features/sql-big-data-cluster/** .

## <a name="prerequisites"></a>Voraussetzungen

- [Ein bereitgestellter Big Data-Cluster](deployment-guidance.md)
- [Big-Data-Tools](deploy-big-data-tools.md)
   - **azdata**
   - **kubectl**
   - **sqlcmd**
   - **curl**
 
## <a name="load-sample-data"></a><a id="sampledata"></a> Laden von Beispieldaten

In den folgenden Schritten wird ein Bootstrapskript zum Herunterladen einer SQL Server-Datenbanksicherung und zum Laden der Daten in ihren Big Data-Cluster verwendet. Zur einfachen Verwendung wurden diese Schritte in [Windows](#windows)- und [Linux](#linux)-Abschnitte aufgeteilt. Wenn Sie lediglich Benutzername und Kennwort als Authentifizierungsmechanismus verwenden möchten, legen Sie die Umgebungsvariablen AZDATA_USERNAME und AZDATA_PASSWORD fest, bevor Sie das Skript ausführen. Andernfalls stellt das Skript die Verbindung zur SQL Server-Masterinstanz und zum Knox-Gateway mithilfe der integrierten Authentifizierung her. Außerdem muss der jeweilige DNS-Name der Endpunkte angegeben werden, damit die integrierte Authentifizierung verwendet werden kann.

## <a name="windows"></a><a id="windows"></a> Windows

In den folgenden Schritten wird beschrieben, wie Sie einen Windows-Client verwenden, um die Beispieldaten in ihren Big Data-Cluster zu laden.

1. Öffnen Sie eine neue Windows-Eingabeaufforderung.

   > [!IMPORTANT]
   > Verwenden Sie für diese Schritte nicht Windows PowerShell. In PowerShell kann das Skript nicht ausgeführt werden, da es die PowerShell-Version von **curl** verwendet.

1. Verwenden Sie **curl**, um das Bootstrapskript für die Beispieldaten herunterzuladen.

   ```cmd
   curl -o bootstrap-sample-db.cmd "https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/bootstrap-sample-db.cmd"
   ```

1. Laden Sie das **bootstrap-sample-db.sql**-Transact-SQL-Skript herunter. Dieses Skript wird vom Bootstrapskript aufgerufen.

   ```cmd
   curl -o bootstrap-sample-db.sql "https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/bootstrap-sample-db.sql"
   ```

1. Das Bootstrapskript erfordert die folgenden Positionsparameter für Ihren Big Data-Cluster:

   | Parameter | BESCHREIBUNG |
   |---|---|
   | <CLUSTER_NAMESPACE> | Der Name, den Sie Ihrem Big Data-Cluster gegeben haben. |
   | <SQL_MASTER_ENDPOINT> | Dieser Parameter steht für den DNS-Namen oder die IP-Adresse Ihrer Masterinstanz. |
   | <KNOX_ENDPOINT> | Dieser Parameter steht für den DNS-Namen oder die IP-Adresse des HDFS/Spark-Gateways. |
   
   > [!TIP]
   > Suchen Sie mit [kubectl](cluster-troubleshooting-commands.md) die IP-Adressen für die SQL Server-Masterinstanz und Knox. Führen Sie `kubectl get svc -n <your-big-data-cluster-name>` aus, und sehen Sie sich die EXTERNAL-IP-Adressen für die Masterinstanz (**master-svc-external**) und Knox (**gateway-svc-external**) an. Der Standardname eines Clusters ist **mssql-cluster**.

1. Führen Sie das Bootstrapskript aus.

   ```cmd
   .\bootstrap-sample-db.cmd <CLUSTER_NAMESPACE> <SQL_MASTER_ENDPOINT> <KNOX_ENDPOINT>
   ```

## <a name="linux"></a><a id="linux"></a> Linux

In den folgenden Schritten wird beschrieben, wie Sie einen Linux-Client verwenden, um die Beispieldaten in ihren Big Data-Cluster zu laden.

1. Laden Sie das Bootstrapskript herunter, und weisen Sie ihm Ausführungsberechtigungen zu.

   ```bash
   curl -o bootstrap-sample-db.sh "https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/bootstrap-sample-db.sh"
   chmod +x bootstrap-sample-db.sh
   ```

1. Laden Sie das **bootstrap-sample-db.sql**-Transact-SQL-Skript herunter. Dieses Skript wird vom Bootstrapskript aufgerufen.

   ```bash
   curl -o bootstrap-sample-db.sql "https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/bootstrap-sample-db.sql"
   ```

1. Das Bootstrapskript erfordert die folgenden Positionsparameter für Ihren Big Data-Cluster:

   | Parameter | BESCHREIBUNG |
   |---|---|
   | <CLUSTER_NAMESPACE> | Der Name, den Sie Ihrem Big Data-Cluster gegeben haben. |
   | <SQL_MASTER_ENDPOINT> | Dieser Parameter steht für den DNS-Namen oder die IP-Adresse Ihrer Masterinstanz. |
   | <KNOX_ENDPOINT> | Dieser Parameter steht für den DNS-Namen oder die IP-Adresse des HDFS/Spark-Gateways. |

   > [!TIP]
   > Suchen Sie mit [kubectl](cluster-troubleshooting-commands.md) die IP-Adressen für die SQL Server-Masterinstanz und Knox. Führen Sie `kubectl get svc -n <your-big-data-cluster-name>` aus, und sehen Sie sich die EXTERNAL-IP-Adressen für die Masterinstanz (**master-svc-external**) und Knox (**gateway-svc-external**) an. Der Standardname eines Clusters ist **mssql-cluster**.

1. Führen Sie das Bootstrapskript aus.

   ```bash
   ./bootstrap-sample-db.sh <CLUSTER_NAMESPACE> <SQL_MASTER_ENDPOINT> <KNOX_ENDPOINT>
   ```

## <a name="next-steps"></a>Nächste Schritte

Nach Ausführen des Bootstrapskripts verfügt Ihr Big Data-Cluster über die Beispieldatenbanken und HDFS-Daten. In den folgenden Tutorials werden die Beispieldaten verwendet, um Big Data-Cluster-Funktionen zu veranschaulichen:

Datenvirtualisierung:

- [Tutorial: Abfragen von HDFS in einem Big Data-Cluster für SQL Server](tutorial-query-hdfs-storage-pool.md)
- [Tutorial: Abfragen von Oracle in einem Big Data-Cluster für SQL Server](tutorial-query-oracle.md)

Datenerfassung:

- [Tutorial: Erfassen von Daten in einem SQL Server-Datenpool mit Transact-SQL](tutorial-data-pool-ingest-sql.md)
- [Tutorial: Erfassen von Daten in einem SQL Server-Datenpool mithilfe von Spark-Aufträgen](tutorial-data-pool-ingest-spark.md)

Notebooks:

- [Tutorial: Ausführen eines Beispielnotebooks auf einem SQL Server 2019-Big Data-Cluster](notebooks-tutorial-spark.md)
