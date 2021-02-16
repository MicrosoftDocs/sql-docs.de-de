---
title: Konfigurieren der Replikation (SSMS)
description: Erfahren Sie, wie Sie die SQL Server-Replikation unter Linux konfigurieren. Konfigurieren Sie die Replikation entweder in SQL Server Management Studio (SSMS) oder mit gespeicherten Transact-SQL-Prozeduren.
ms.custom: seo-dt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 03/20/2018
ms.topic: article
ms.prod: sql
ms.prod_service: database-engine
ms.technology: linux
titleSuffix: SQL Server on Linux
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 911446480834b9ff8b51ce806729f2771d28d84f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346491"
---
# <a name="configure-sql-server-replication-on-linux"></a>Konfigurieren von SQL Server-Replikation unter Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Mit [!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)] wird SQL Server-Replikation für Instanzen von SQL Server für Linux eingeführt.

Ausführliche Informationen über Replikation finden Sie unter [SQL Server-Replikation](../relational-databases/replication/sql-server-replication.md).

Sie konfigurieren Replikation unter Linux entweder mit SQL Server Management Studio (SSMS) oder mit gespeicherten Transact-SQL-Prozeduren.

* Wenn Sie SMSS verwenden möchten, gehen Sie entsprechend den Anweisungen in diesem Artikel vor.

  Verwenden Sie SSMS unter Windows, um Verbindungen mit Instanzen von SQL Server herzustellen. Hintergrundinformationen und Anweisungen finden Sie unter [Verwenden von SSMS zum Verwalten von SQL Server für Linux](./sql-server-linux-manage-ssms.md).
  
* Ein Beispiel mit gespeicherten Prozeduren finden Sie im Tutorial [Konfigurieren von SQL Server-Replikation unter Linux](sql-server-linux-replication-tutorial-tsql.md).

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie Verleger, Verteiler und Abonnenten konfigurieren können, müssen Sie einige Konfigurationsschritte für die SQL Server-Instanz ausführen.

1. Ermöglichen Sie dem SQL Server-Agent die Verwendung von Replikations-Agents. Führen Sie auf einem Linux-Server die folgenden Befehle im Terminal aus.

  ```bash
  sudo /opt/mssql/bin/mssql-conf set sqlagent.enabled true
  sudo systemctl restart mssql-server
  ```

1. Konfigurieren Sie die SQL Server-Instanz für die Replikation. Um die SQL Server-Instanz für die Replikation zu konfigurieren, führen Sie `sys.sp_MSrepl_createdatatypemappings` für alle an der Replikation beteiligten Instanzen aus.

  ```sql
  USE msdb
  GO
  exec sys.sp_MSrepl_createdatatypemappings;
  GO
  ```

1. Erstellen einen Momentaufnahmeordner. Die SQL Server-Agents benötigen einen Momentaufnahmeordner, für den sie Lese-/Schreibzugriff haben. Erstellen Sie den Momentaufnahmeordner auf dem Verteiler.

  Um den Momentaufnahmeordner zu erstellen und dem `mssql`-Benutzer Zugriff zu gewähren, führen Sie den folgenden Befehl aus:

  ```bash
  sudo mkdir /var/opt/mssql/data/ReplData/
  sudo chown mssql /var/opt/mssql/data/ReplData/
  sudo chgrp mssql /var/opt/mssql/data/ReplData/
  ```

## <a name="configure-and-monitor-replication-with-sql-server-management-studio-ssms"></a>Konfigurieren und Überwachen der Replikation mit SQL Server Management Studio (SSMS)

### <a name="configure-the-distributor"></a>Konfigurieren des Verteilers
  
So konfigurieren Sie den Verteiler: 

1. Stellen Sie aus SSMS eine Verbindung mit Ihrer Instanz von SQL Server im Objekt-Explorer her.

1. Klicken Sie mit der rechten Maustaste auf **Replikation**, und klicken Sie auf **Verteilung konfigurieren...**.

1. Befolgen Sie die Anweisungen für den **Verteilungskonfigurations-Assistenten**.

### <a name="create-publication-and-articles"></a>Erstellen von Veröffentlichung und Artikeln

So erstellen Sie eine Veröffentlichung und Artikel:

1. Klicken Sie im Objekt-Explorer auf **Replikation** > **Lokale Veröffentlichungen**> **Neue Veröffentlichung...** .

1. Befolgen Sie die Anweisungen im **Assistent für neue Veröffentlichung**, um den Typ der Replikation und die Artikel zu konfigurieren, die zur Veröffentlichung gehören.

### <a name="configure-the-subscription"></a>Konfigurieren des Abonnements

Um das Abonnement im Objekt-Explorer zu konfigurieren, klicken Sie auf **Replikation** > **Lokale Abonnements**> **Neue Abonnements...** .

### <a name="monitor-replication-jobs"></a>Überwachen von Replikationsaufträgen

Verwenden Sie „Replikationsmonitor“, um Replikationsaufträge zu überwachen.

Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf **Replikation**, und klicken Sie auf **Replikationsmonitor starten**.

## <a name="next-steps"></a>Nächste Schritte

[Konzepte: SQL Server-Replikation unter Linux](sql-server-linux-replication.md)

[Gespeicherte Prozeduren für die Replikation](../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md).
