---
title: Aktualisieren von AZDATA_PASSWORD
description: '`AZDATA_PASSWORD` manuell aktualisieren'
author: NelGson
ms.author: negust
ms.reviewer: mikeray
ms.date: 12/19/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 7fe856ae92c198d44ed0b11d1d2ce808e87fb4eb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039680"
---
# <a name="manually-update-azdata_password"></a>Manuelles Aktualisieren von `AZDATA_PASSWORD`

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Unabhängig davon, ob der Cluster mit Active Directory-Integration arbeitet oder nicht, wird `AZDATA_PASSWORD` während der Bereitstellung festgelegt. Es ermöglicht eine Standardauthentifizierung bei Clustercontroller und Masterinstanz. In diesem Dokument wird erläutert, wie `AZDATA_PASSWORD` manuell aktualisiert wird.

## <a name="change-azdata_password-for-controller"></a>Ändern von `AZDATA_PASSWORD` für den Controller

Wenn der Cluster nicht im Active Directory-Modus betrieben wird, aktualisieren Sie das Apache Knox Gateway-Kennwort wie folgt:

1. Rufen Sie die SQL Server-Anmeldeinformationen des Controllers ab, indem Sie die folgenden Befehle ausführen:

   a. Führen Sie diesen Befehl als Kubernetes-Administrator aus:

   ```bash
   kubectl get secret controller-sa-secret -n <cluster name> -o yaml | grep password
   ```

   b. Decodieren Sie das Geheimnis mit Base64:
   
   ```bash
   echo <password from kubectl command>  | base64 --decode && echo
   ```

1. Machen Sie in einem separaten Befehlsfenster den Port des Datenbankservers des Controllers verfügbar:

   ```bash
   kubectl port-forward controldb-0 1433:1433 --address 0.0.0.0 -n <cluster name>
   ```
 
1. Verwenden Sie das Kennwort des Systemadministrators, das Sie soeben erhalten haben, um sich in einem SQL-Clienttool mit dem Datenbankserver des Controllers zu verbinden.

1. Generieren Sie ein neues komplexes Kennwort für `AZDATA_USERNAME`, um das vorhandene `AZDATA_PASSWORD` zu ersetzen.

   Um das Beispiel zu vereinfachen, wird in den nächsten Schritten „newPassword“ verwendet, da das generierte Kennwort „newPassword“ ist. 

1. Rufen Sie `hexsalt` aus der Tabelle „users“ ab:

   ```sql
   SELECT hexsalt FROM [auth].[users] WHERE username = '<username>'
   ```

   `hexsalt` gibt eine zufällige hexadezimale Zeichenfolge zurück (z. B. `64FC59DF31244FFEE02F457BC0750226`).

1. Verschlüsseln Sie das neue komplexe Kennwort mithilfe von `hexsalt`:

   Zur Vereinfachung stellen wir Ihnen das vorkonfigurierte Tool `pbkdf2` zur Verschlüsselung des Kennworts zur Verfügung. Laden Sie die für die Plattform geeignete .NET Core-App für [`pbkdf2`](https://github.com/microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/security/password-hashing/pbkdf2/prebuilt-binaries)herunter.

   Die App ist eigenständig und weist keine Voraussetzungen wie z. B. .NET-Runtimes auf. Führen Sie zum Verschlüsseln des Kennworts Folgendes aus:

   ```bash
   pbkdf2 <password> <hexsalt>
   J2y4E4dhlgwHOaRr3HKiiVAKBfjuGDyYmzn88VXmrzM=
   ```

1. Aktualisieren Sie das Kennwort in der Tabelle „users“:

   ```SQL
   UPDATE [auth].[users] SET password = 'J2y4E4dhlgwHOaRr3HKiiVAKBfjuGDyYmzn88VXmrzM=' WHERE username = '<username>'
   ```

## <a name="change-azdata_password-in-the-sql-server-master-instance"></a>Ändern Sie `AZDATA_PASSWORD` in der SQL Server-Masterinstanz.

1. Verbinden Sie sich über ein beliebiges Administratorkonto mit dem SQL Server-Masterendpunkt.

1. Um das Kennwort für die Anmeldeinformationen zu ändern, die Sie während der Bereitstellung im Parameter `AZDATA_USERNAME` festgelegt haben, führen Sie den folgenden TSQL-Befehl aus:

   ```sql
   ALTER LOGIN <AZDATA_USERNAME> WITH PASSWORD = 'newPassword'
   ```
