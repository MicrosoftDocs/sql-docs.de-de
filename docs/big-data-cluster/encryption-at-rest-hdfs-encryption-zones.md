---
title: Leitfaden zur Verwendung von SQL Server HDFS-Verschlüsselungszonen für Big Data-Cluster
titleSuffix: SQL Server big data clusters
description: In diesem Artikel wird gezeigt, wie Sie die SQL Server HDFS-Verschlüsselungszonenfunktion von BDC verwenden.
author: DaniBunny
ms.author: dacoelho
ms.reviewer: mihaelab
ms.date: 10/19/2020
ms.topic: tutorial
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 4c56d065de396a282c97c0723f118e5911617544
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489294"
---
# <a name="sql-server-big-data-clusters-hdfs-encryption-zones-usage-guide"></a>Leitfaden zur Verwendung von SQL Server HDFS-Verschlüsselungszonen für Big Data-Cluster

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

In diesem Leitfaden wird veranschaulicht, wie Sie Verschlüsselungsfunktionen für ruhende Daten von SQL Server Big Data-Clustern verwenden, um HDFS-Ordner mithilfe von Verschlüsselungszonen zu verschlüsseln. Darüber hinaus werden Aufgaben der HDFS-Schlüsselverwaltung beschrieben.

Beachten Sie, dass unter __```/securelake```__ eine einsatzbereite Standardverschlüsselungszone eingebunden ist. Sie wurde mit einem vom System generierten 256-Bit-Schlüssel namens __securelakekey__ erstellt. Mit diesem Schlüssel können Sie zusätzliche Verschlüsselungszonen erstellen.

## <a name="prerequisites"></a><a id="prereqs"></a> Voraussetzungen

- [SQL Server-Big Data-Cluster ab CU8](release-notes-big-data-cluster.md) mit [Active Directory](active-directory-prerequisites.md)-Integration.
- Benutzer mit Administratorrechten.
- Im AD-Modus in den Cluster konfigurierte und protokollierte [!INCLUDE[azdata](../includes/azure-data-cli-azdata.md)].

## <a name="create-an-encryption-zone-using-the-provided-system-managed-key"></a>Erstellen einer Verschlüsselungszone mithilfe des bereitgestellten, systemseitig verwalteten Schlüssels

1. Erstellen eines HDFS-Ordners

   ```console
   azdata bdc hdfs mkdir -p /user/zone/folder
   ```

1. Geben Sie den Befehl zum Erstellen der Verschlüsselungszone aus, um den Ordner mit dem Schlüssel __securelakekey__ zu verschlüsseln.

   ```console
   azdata bdc hdfs encryption-zone create --path /user/zone/folder --keyname securelakekey
   ```

## <a name="create-a-custom-new-key-and-encryption-zone"></a>Erstellen eines benutzerdefinierten neuen Schlüssels und einer Verschlüsselungszone

1. Verwenden Sie folgendes Muster, um einen 256-Bit-Schlüssel zu erstellen.

   ```console
   azdata bdc hdfs key create -n mydatalakekey
   ```

1. Erstellen und verschlüsseln Sie einen neuen HDFS-Pfad mithilfe des Benutzerschlüssels.

   ```console
   azdata bdc hdfs encryption-zone create --path /user/mydatalake --keyname mydatalakekey
   ```

## <a name="hdfs-key-rotation-and-encryption-zone-re-encryption"></a>HDFS-Schlüsselrotation und Neuverschlüsselung von Verschlüsselungszonen

1. Damit wird eine neue Version von __securelakey__ mit neuem Schlüsselmaterial erstellt.

   ```console
   azdata hdfs bdc key roll -n securelakekey
   ```

1. Verschlüsseln Sie die mit dem oben genannten Schlüssel verknüpfte Verschlüsselungszone neu.

   ```console
   azdata bdc hdfs encryption-zone reencrypt --path /securelake --action start
   ```

## <a name="hdfs-key-and-encryption-zone-monitoring"></a>Überwachung von HDFS-Schlüssel und Verschlüsselungszone

1. So überwachen Sie den Status der Neuverschlüsselung einer Verschlüsselungszone 

   ```console
   azdata bdc hdfs encryption-zone status
   ```

1. So rufen Sie die Verschlüsselungsinformationen zu einer Datei in einer Verschlüsselungszone ab

   ```console
   azdata bdc hdfs encryption-zone get-file-encryption-info --path /securelake/data.csv
   ```

1. So listen Sie alle Verschlüsselungszonen auf

   ```console
   azdata bdc hdfs encryption-zone list
   ```

1. So listen Sie alle verfügbaren Schlüssel für HDFS auf

   ```console
   azdata bdc hdfs key list
   ```

1. So erstellen Sie einen benutzerdefinierten Schlüssel für die HDFS-Verschlüsselung. Mögliche Größen: 128, 192 256. Der Standardwert lautet 256.

   ```console
   azdata hdfs key create --name key1 --size 256
   ```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Verwendung von azdata mit Big Data-Cluster finden Sie unter [Was ist [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md)

Verwenden von azdata mit [Azure Arc-fähigen Datendiensten](/azure/azure-arc/data/)
