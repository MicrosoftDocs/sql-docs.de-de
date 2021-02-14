---
title: Herstellen einer Verbindung mit der Masterinstanz und HDFS-Big Data-Clustern
description: Informieren Sie sich, wie Sie eine Verbindung mit der SQL Server-Masterinstanz und dem HDFS/Spark-Gateway für einen SQL Server-Big Data Cluster herstellen.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: b7f05551da025f513cd69ddf592002126c6f05c3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039053"
---
# <a name="connect-to-a-sql-server-big-data-cluster-with-azure-data-studio"></a>Herstellen einer Verbindung mit einem Big-Data-Cluster für SQL Server mit Azure Data Studio

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

In diesem Artikel wird beschrieben, wie Sie über Azure Data Studio eine Verbindung mit einem [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] herstellen.

## <a name="prerequisites"></a>Voraussetzungen

- Ein bereitgestellter [Big-Data-Cluster für SQL Server 2019](deployment-guidance.md).
- [Big Data-Tools für SQL Server 2019](deploy-big-data-tools.md):
   - **Azure Data Studio**
   - **Erweiterung von SQL Server 2019**
   - **kubectl**
   - **azdata**

## <a name="connect-to-the-cluster"></a><a id="master"></a> Herstellen einer Verbindung mit dem Cluster

Stellen Sie eine neue Verbindung mit der SQL Server-Masterinstanz im Cluster her, um eine Verbindung mit einem Big-Data-Cluster mit Azure Data Studio herzustellen. Gehen Sie dabei folgendermaßen vor:

1. Suchen Sie den Endpunkt der SQL Server-Masterinstanz:

   ```
   azdata bdc endpoint list -e sql-server-master
   ```

   > [!TIP]
   > Weitere Informationen zum Abrufen von Endpunkten finden Sie unter [Abrufen von Endpunkten](deployment-guidance.md#endpoints).

1. Drücken Sie **F1** > **Neue Verbindung** in Azure Data Studio.

1. Wählen Sie unter **Verbindungstyp** die Option **Microsoft SQL Server** aus.

1. Geben Sie den für die SQL Server-Masterinstanz gefundenen Endpunktnamen in das Textfeld bei **Servername** ein (Beispiel: **\<IP_Address\>,31433**). 

1. Wählen Sie einen Authentifizierungstyp aus. Bei der SQL Server-Masterinstanz, die in einem Big Data Cluster ausgeführt wird, werden nur die **Windows-Authentifizierung** und **SQL-Anmeldung** unterstützt. 

1. Geben Sie Ihren **Benutzernamen** und Ihr **Kennwort** ein, wenn Sie die SQL-Anmeldung verwenden.

   > [!TIP]
   > Der Benutzername **SA** ist während der Bereitstellung eines Big Data-Clusters standardmäßig deaktiviert. Während der Bereitstellung wird ein neuer sysadmin-Benutzer bereitgestellt, dessen Name und Kennwort den Umgebungsvariablen **AZDATA_USERNAME** und **AZDATA_PASSWORD** entsprechen, die entweder vor oder während der Bereitstellung festgelegt wurden.

1. Ändern Sie den **Zieldatenbanknamen** in einen ihrer relationalen Datenbanken.

   ![Herstellen einer Verbindung mit der Masterinstanz](./media/connect-to-big-data-cluster/connect-to-cluster.png)

1. Klicken Sie auf **Verbinden**, und das **Serverdashboard** sollte angezeigt werden.

Mit der Azure Data Studio-Version vom Februar 2019 können Sie mit dem Herstellen einer Verbindung mit der SQL Server-Masterinstanz auch mit dem HDFS/Spark-Gateway interagieren. Dies bedeutet, dass Sie keine separate Verbindung für HDFS und Spark verwenden müssen, die im nächsten Abschnitt beschrieben wird.

- Der Objekt-Explorer enthält jetzt einen neuen **Data Services**-Knoten für Big-Data-Cluster-Aufgaben (z. B. das Erstellen neuer Notebooks oder das Übermitteln von Spark-Aufträgen), die mit einem Rechtsklick ausgeführt werden können. 
- Der **Data Services**-Knoten enthält auch einen **HDFS**-Ordner, mit dem Sie den Inhalt des HDFS untersuchen und allgemeine Aufgaben im Zusammenhang mit dem HDFS ausführen können (z. B. das Erstellen einer externen Tabelle oder das Öffnen eines Notebooks zum Analysieren des HDFS-Inhalts).
- Das **Serverdashboard** für die Verbindung enthält auch Registerkarten für **SQL Server-Big Data Cluster** und **SQL Server 2019**, wenn die Erweiterung installiert ist.

   ![Data Services-Knoten für Azure Data Studio](./media/connect-to-big-data-cluster/connect-data-services-node.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] finden Sie unter [Was sind [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md).
