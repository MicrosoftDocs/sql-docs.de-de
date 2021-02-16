---
title: Installieren von PolyBase unter Linux
titlesuffix: SQL Server
description: Erfahren Sie, wie Sie SQL Server PolyBase unter Linux installieren. Mit PolyBase können Sie externe Abfragen für Remotedatenquellen ausführen.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: dakryze
ms.date: 8/18/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
monikerRange: '>= sql-server-linux-ver15 || >= sql-server-ver15'
ms.openlocfilehash: 85beab1eb4d256c0433678ca34a8ac2627ebfb52
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100062246"
---
# <a name="install-polybase-on-linux"></a>Installieren von PolyBase unter Linux

[!INCLUDE [sqlserver2019-linux](../../includes/applies-to-version/sqlserver2019-linux.md)]

Mit den folgenden Schritten können Sie unter Linux [PolyBase](../../relational-databases/polybase/polybase-guide.md) (`mssql-server-polybase` und `mssql-server-polybase-hadoop`) installieren. Mit PolyBase können Sie externe Abfragen für Remotedatenquellen ausführen.

>[!NOTE]
> Installieren Sie zunächst [SQL Server 2019](../../linux/sql-server-linux-setup.md#platforms), bevor Sie PolyBase installieren. Dadurch werden die Schlüssel und Repositorys konfiguriert, die Sie beim Installieren der Pakete `mssql-server-polybase` und `mssql-server-polybase-hadoop` verwenden.

>[!NOTE]
>
> - PolyBase wird unter SQL Server 2017 für Linux nicht unterstützt.
> - Eine horizontale Skalierung für PolyBase ist derzeit unter Linux nicht verfügbar.

Installieren Sie PolyBase für Ihr Betriebssystem:

- [Red Hat Enterprise Linux](#RHEL)
- [Ubuntu](#ubuntu)
- [SUSE Linux Enterprise Server](#SLES)

## <a name="install-on-rhel"></a><a name="RHEL"></a>Installieren unter RHEL

Verwenden Sie zum Installieren von `mssql-server-polybase` unter Red Hat Enterprise Linux (RHEL) den folgenden Befehl: 

```bash
sudo yum install -y mssql-server-polybase
```

Sie werden zu einem Neustart der SQL Server-Instanz aufgefordert. Verwenden Sie hierfür den folgenden Befehl:

```bash
sudo systemctl restart mssql-server
```

>[!NOTE]
>Nach der Installation müssen Sie das [PolyBase-Feature aktivieren](#enable).

Verwenden Sie die folgenden Befehle, um `mssql-server-polybase-hadoop` zu installieren. 

```bash
sudo yum install -y mssql-server-polybase-hadoop
```

Das PolyBase-Hadoop-Paket weist Abhängigkeiten von den folgenden Paketen auf:
- `mssql-server`
- `mssql-server-polybase`
- `mssql-server-extensibility`
- `mssql-zulu-jre-11`. 

Bei der Installation wird `launchpadd` neu gestartet. Verwenden Sie hierfür den folgenden Befehl:

```bash
sudo systemctl restart mssql-launchpadd
```

>[!NOTE]
>Nach der Installation müssen Sie die [Hadoop-Konnektivitätsebene festlegen](../../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md#c-set-hadoop-connectivity).

Wenn Sie eine Offlineinstallation benötigen, suchen Sie in den [Versionshinweisen](../../linux/sql-server-linux-release-notes.md) nach der Downloadmöglichkeit für das PolyBase-Paket. Führen Sie dann die im Artikel [Installieren von SQL Server](../../linux/sql-server-linux-setup.md#offline) beschriebenen Schritte für die Offlineinstallation aus.

## <a name="install-on-ubuntu"></a><a name="ubuntu"></a>Installieren unter Ubuntu

Verwenden Sie zum Installieren von `mssql-server-polybase` unter Ubuntu den folgenden Befehl: 

```bash
sudo apt-get install mssql-server-polybase
```

Sie werden zu einem Neustart der SQL Server-Instanz aufgefordert. Verwenden Sie hierfür den folgenden Befehl:

```bash
sudo systemctl restart mssql-server
```

>[!NOTE]
>Nach der Installation müssen Sie das [PolyBase-Feature aktivieren](#enable).

Wenn Sie eine Offlineinstallation benötigen, suchen Sie in den [Versionshinweisen](../../linux/sql-server-linux-release-notes.md) nach der Downloadmöglichkeit für das PolyBase-Paket. Führen Sie dann die im Artikel [Installieren von SQL Server](../../linux/sql-server-linux-setup.md#offline) beschriebenen Schritte für die Offlineinstallation aus.

Verwenden Sie die folgenden Befehle, um `mssql-server-polybase-hadoop` zu installieren. 

```bash
sudo apt-get install mssql-server-polybase-hadoop
```

Das PolyBase-Hadoop-Paket weist Abhängigkeiten von den folgenden Paketen auf:
- `mssql-server`
- `mssql-server-polybase`
- `mssql-server-extensibility`
- `mssql-zulu-jre-11`. 

Bei der Installation wird `launchpadd` neu gestartet. Verwenden Sie hierfür den folgenden Befehl:

```bash
sudo systemctl restart mssql-launchpadd
```

>[!NOTE]
>Nach der Installation müssen Sie die [Hadoop-Konnektivitätsebene festlegen](../../relational-databases/polybase/polybase-configure-hadoop.md#configure-hadoop-connectivity).

## <a name="install-on-sles"></a><a name="SLES"></a>Installieren unter SLES

Verwenden Sie zum Installieren von `mssql-server-polybase` unter SUSE Linux Enterprise Server (SLES) den folgenden Befehl: 

```bash
sudo zypper install mssql-server-polybase
```

Sie werden zu einem Neustart der SQL Server-Instanz aufgefordert. Verwenden Sie hierfür den folgenden Befehl:

```bash
sudo systemctl restart mssql-server
```

>[!NOTE]
>Nach der Installation müssen Sie das [PolyBase-Feature aktivieren](#enable).

Wenn Sie eine Offlineinstallation benötigen, suchen Sie in den [Versionshinweisen](../../linux/sql-server-linux-release-notes.md) nach der Downloadmöglichkeit für das PolyBase-Paket. Führen Sie dann die im Artikel [Installieren von SQL Server](../../linux/sql-server-linux-setup.md#offline) beschriebenen Schritte für die Offlineinstallation aus.


## <a name="enable-polybase"></a><a name="enable"></a> Aktivieren von PolyBase

Nach der Installation muss PolyBase aktiviert werden, um auf die Features zugreifen zu können. Stellen Sie eine Verbindung mit der installierten SQL Server-Instanz her, und verwenden Sie zum Aktivieren den folgenden Transact-SQL-Befehl:

```sql
exec sp_configure @configname = 'polybase enabled', @configvalue = 1;
RECONFIGURE WITH OVERRIDE;
```

## <a name="update-polybase"></a>Aktualisieren von PolyBase

Wenn Sie `mssql-server-polybase` bereits installiert haben, können Sie es mit den folgenden Befehlen auf die neueste Version aktualisieren:

### <a name="rhel"></a>RHEL

```bash
sudo yum remove -y mssql-server-polybase-hadoop
sudo yum remove -y mssql-server-polybase
sudo yum check-update
sudo yum install -y mssql-server-polybase
sudo yum install -y mssql-server-polybase-hadoop
```

Sie werden zu einem Neustart der SQL Server-Instanz aufgefordert. Verwenden Sie hierfür den folgenden Befehl:

```
sudo systemctl restart mssql-server
```

### <a name="ubuntu"></a>Ubuntu

```bash
sudo apt-get remove mssql-server-polybase-hadoop
sudo apt-get remove mssql-server-polybase
sudo apt-get update 
sudo apt-get install mssql-server-polybase
sudo apt-get remove mssql-server-polybase-hadoop
```

Sie werden zu einem Neustart der SQL Server-Instanz aufgefordert. Verwenden Sie hierfür den folgenden Befehl:

```
sudo systemctl restart mssql-server
```

### <a name="sles"></a>SLES

```bash
sudo zypper remove mssql-server-polybase
sudo zypper refresh
sudo zypper install mssql-server-polybase
```

Sie werden zu einem Neustart der SQL Server-Instanz aufgefordert. Verwenden Sie hierfür den folgenden Befehl:

```
sudo systemctl restart mssql-server
```

>[!NOTE]
>Nach der Installation müssen Sie das [PolyBase-Feature aktivieren](#enable).

## <a name="next-steps"></a>Nächste Schritte

PolyBase kann unter Linux auf die folgenden Datenquellen zugreifen. Informationen zum Erstellen einer externen Tabelle aus diesen Quellen, wenn PolyBase aktiviert ist, finden Sie unter den folgenden Links: 

- [SQL Server, SQL-Datenbank, Azure Synapse Analytics](../../relational-databases/polybase/polybase-configure-sql-server.md)
- [Hadoop](../../relational-databases/polybase/polybase-configure-hadoop.md)
- [Azure Blob Storage](../../relational-databases/polybase/polybase-configure-azure-blob-storage.md)
- [Oracle](../../relational-databases/polybase/polybase-configure-oracle.md)
- [Teradata](../../relational-databases/polybase/polybase-configure-teradata.md)
- [MongoDB (& Cosmos DB)](../../relational-databases/polybase/polybase-configure-mongodb.md)

Weitere Informationen zur Verwendung finden Sie im Transact-SQL-Referenzartikel für [CREATE EXTERNAL TABLE](../../t-sql/statements/create-external-table-transact-sql.md).
