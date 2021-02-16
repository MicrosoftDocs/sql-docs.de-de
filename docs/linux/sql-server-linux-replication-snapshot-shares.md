---
title: Konfigurieren von Momentaufnahmeordnerfreigaben
titleSuffix: SQL Server on Linux
description: Hier erfahren Sie, wie Sie Momentaufnahmeordnerfreigaben für die SQL Server-Replikation unter Linux konfigurieren.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 09/24/2018
ms.topic: article
ms.prod: sql
ms.technology: linux
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: fdd43008abb9ba49ab3a5e3a2859a6366a3a6542
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346474"
---
# <a name="configure-replication-snapshot-folder-with-shares"></a>Konfigurieren der Replikation eines Momentaufnahmeordners mit Freigaben

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Der Momentaufnahmeordner ist ein von Ihnen freigegebenes Verzeichnis. Agents, die aus diesem Ordner lesen bzw. in den Ordner schreiben, benötigen ausreichende Zugriffsberechtigungen.

![Replikationsdiagramm][1]

### <a name="replication-snapshot-folder-share-explained"></a>Erläuterung der Replikation der Momentaufnahmeordner-Freigabe

Vor den Beispielen wird erläutert, wie SQL Server Samba-Freigaben in der Replikation verwendet. Im Folgenden finden Sie ein einfaches Beispiel hierfür.

1. Samba-Freigaben sind so konfiguriert, dass die von den Replikations-Agents auf dem Herausgeber auf `/local/path1` geschriebenen Dateien vom Abonnenten angezeigt werden können.
2. SQL Server ist für die Verwendung von Freigabepfaden beim Einrichten des Herausgebers auf dem Verteilungsserver so konfiguriert, dass alle Instanzen den `//share/path` beachten.
3. SQL Server sucht den lokalen Pfad in `//share/path`, um zu wissen, wo die Dateien gesucht werden sollen.
4. SQL Server liest/schreibt in lokale Pfade, die von einer Samba-Freigabe gesichert sind.


## <a name="configure-a-samba-share-for-the-snapshot-folder"></a>Konfigurieren einer Samba-Freigabe für den Momentaufnahmeordner 

Replikations-Agents benötigen ein zwischen den Replikationshosts freigegebenes Verzeichnis, um auf Momentaufnahmeordner zuzugreifen, die sich auf anderen Computern befinden. Beispielsweise befindet sich der Verteilungs-Agent bei der transaktionalen Pullreplikation auf dem Abonnenten, der Zugriff auf den Verteiler benötigt, um Artikel abzurufen. In diesem Abschnitt untersuchen wir ein Beispiel für die Konfiguration einer Samba-Freigabe auf zwei Replikationshosts.


## <a name="steps"></a>Schritte

Als Beispiel konfigurieren wir einen Momentaufnahmeordner auf Host 1 (dem Verteiler), der mithilfe von Samba für Host 2 (den Abonnenten) freigegeben werden soll. 

### <a name="install-and-start-samba-on-both-machines"></a>Installieren und Starten von Samba auf beiden Computern 

Auf Ubuntu:

```bash
sudo apt-get -y install samba
sudo service smbd restart
```

Auf RHEL:

```bash
sudo yum install samba
sudo service smb start
sudo service smb status
```

### <a name="on-host-1-distributor-set-up-the-samba-share"></a>Einrichten der Samba-Freigabe auf Host 1 (Verteiler) 

1. Einrichten von Benutzernamen und Kennwort für Samba:

  ```bash
  sudo smbpasswd -a mssql 
  ```

1. Bearbeiten Sie die Datei `/etc/samba/smb.conf`, sodass der folgende Eintrag enthalten ist, und füllen Sie die Felder *share_name* und *path* aus.
 ```bash
  <[share_name]>
  path = </local/path/on/host/1>
  writable = yes
  create mask = 770
  directory mask 
  valid users = mssql 
  ```

  **Beispiel**

  ```bash
  [mssql_data]    <- Name of the shared directory
  path = /var/opt/mssql/repldata <- location of directory we wish to share
  writable = yes  <- determines if the share is writable from other hosts
  create mask = 770  <- Linux permissions for files created 
  directory mask = 770 <- Linux permissions for directories created
  valid users = mssql   <- list of users who can login to this share
  ```

### <a name="on-host-2-subscriber--mount-the-samba-share"></a>Einbinden der Samba-Freigabe auf Host 2 (Abonnent)

Bearbeiten Sie den Befehl mit den richtigen Pfaden, und führen Sie den folgenden Befehl auf machine2 aus:

  ```bash
  sudo mount //<name_of_host_1>/<share_name> </local/path/on/host/2> -o user=mssql,uid=mssql,gid=mssql
  ```

  **Beispiel**

  ```bash
  mount //host1/mssql_data /var/opt/mssql/repldata_shared -o user=mssql,uid=mssql,gid=mssql

  user=mssql <- sets the login name for samba
  uid=mssql   <- makes the mssql user as the owner of the mounted directory
  gid=mssql   <- sets the mssql group as the owner of the mounted directory
  ```

### <a name="on-both-hosts--configure-sql-server-on-linux-instances-to-use-snapshot-share"></a>Konfigurieren von SQL Server für Linux-Instanzen auf beiden Hosts für die Verwendung der Momentaufnahmenfreigabe

Fügen Sie auf beiden Computern `mssql.conf` den folgenden Abschnitt hinzu. Ersetzen Sie „//share/path“ überall durch die Samba-Freigabe. In diesem Beispiel wäre es `//host1/mssql_data`.

  ```bash
  [uncmapping]
  //share/path = /local/path/on/hosts/
  ```

  **Beispiel**

  Auf host1:

  ```bash
  [uncmapping]
  //host1/mssql_data = /local/path/on/hosts/1
  ```

  Auf host2:
  
  ```bash
  [uncmapping]
  //host1/mssql_data = /local/path/on/hosts/2
  ```

### <a name="configuring-publisher-with-shared-paths"></a>Konfigurieren des Herausgebers mit freigegebenen Pfaden

* Verwenden Sie beim Einrichten der Replikation den Freigabenpfad (Beispiel: `//host1/mssql_data`).
* Ordnen Sie `//host1/mssql_data` einem lokalen Verzeichnis und der Zuordnung zu, die `mssql.conf` hinzugefügt wurde.

## <a name="next-steps"></a>Nächste Schritte

[Konzepte: SQL Server-Replikation unter Linux](sql-server-linux-replication.md)

[Gespeicherte Prozeduren für die Replikation](../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md).

[1]: ./media/sql-server-linux-replication-snapshot-shares/image1.png
