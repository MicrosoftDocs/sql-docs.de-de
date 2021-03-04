---
title: Konfigurieren der Active Directory-Authentifizierung mit SQL Server für Linux-basierten Containern mithilfe von adutil
description: Hier erhalten Sie eine detaillierte Anleitung zum Konfigurieren der Active Directory-Authentifizierung mit SQL Server für Linux-basierten Containern mithilfe von adutil.
author: amvin87
ms.author: amitkh
ms.reviewer: vanto
ms.date: 12/10/2020
ms.topic: tutorial
ms.prod: sql
ms.technology: linux
moniker: '>= sql-server-linux-2017 || >= sql-server-2017 || =sqlallproducts-allversions'
ms.openlocfilehash: c82362cf9a3d73d1bd9a7c714957e222ad15af18
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837631"
---
# <a name="tutorial-configure-active-directory-authentication-with-sql-server-on-linux--containers"></a>Tutorial: Konfigurieren der Active Directory-Authentifizierung mit SQL Server für Linux-Container

> [!NOTE]
> **adutil** ist aktuell als **öffentliche Vorschauversion** verfügbar.

In diesem Tutorial wird erläutert, wie Sie SQL Server für Linux-Container konfigurieren, um die Active Directory-Authentifizierung (AD) zu unterstützen, die auch als integrierte Authentifizierung bezeichnet wird. Eine Übersicht dazu finden Sie unter [Active Directory-Authentifizierung für SQL Server für Linux](sql-server-linux-active-directory-auth-overview.md).

Dieses Tutorial umfasst die folgenden Aufgaben:

> [!div class="checklist"]
> - Installieren von adutil-preview
> - Einbinden des Linux-Hosts in die AD-Domäne
> - Erstellen eines AD-Benutzers für SQL Server und Festlegen des Dienstprinzipalnamens (Service Principal Name, SPN) mithilfe des adutil-Tools
> - Erstellen der Schlüsseltabellendatei für den SQL Server-Dienst
> - Erstellen der Dateien mmsql.conf und krb5.conf zur Verwendung durch den SQL Server-Container
> - Einbinden der Konfigurationsdateien und Bereitstellen der SQL Server-Container
> - Erstellen von AD-basierten SQL Server-Anmeldeinformationen mit Transact-SQL
> - Herstellen einer Verbindung zu SQL Server mit der AD-Authentifizierung

## <a name="prerequisites"></a>Voraussetzungen

Folgende Voraussetzungen bestehen für das Konfigurieren der AD-Authentifizierung:

- Richten Sie einen AD-Domänencontroller (Windows) in Ihrem Netzwerk ein.
- Installieren Sie das adutil-preview-Tool auf einem Linux-Hostcomputer, der in eine Domäne eingebunden ist. Befolgen Sie entsprechend Ihrer ausgeführten Linux-Distribution die Anleitung im Abschnitt [Installieren von adutil-preview](#install-adutil-preview) unten, um das adutil-preview-Tool zu installieren.

## <a name="container-deployment-and-preparation"></a>Containerbereitstellung und -vorbereitung

Zum Einrichten Ihres Containers müssen Sie im Voraus den Port kennen, der vom Container auf dem Host verwendet wird. Der Standardport 1433 ist möglicherweise auf Ihrem Containerhost anders zugeordnet. Für dieses Tutorial wird Port 5433 auf dem Host Port 1433 des Containers zugeordnet. Weitere Informationen finden Sie unter [Schnellstart: Ausführen von SQL Server-Containerimages mit Docker](quickstart-install-connect-docker.md).

Beim Registrieren des Dienstprinzipalnamens (Service Principal Name, SPN) können Sie den Hostnamen des Computers oder den Namen des Containers verwenden. Dieser Name wird jedoch auch angezeigt, wenn extern eine Verbindung zum Container hergestellt wird, weshalb Sie einen entsprechend geeigneten Namen festlegen sollten.

Stellen Sie sicher, dass in Active Directory ein Eintrag für den Weiterleitungshost (A) für die Linux-Host-IP-Adresse hinzugefügt wurde. Achten Sie dabei auf eine Entsprechung mit dem Namen des SQL Server-Containers. In diesem Tutorial lautet die IP-Adresse des `myubuntu`-Hostcomputers `10.0.0.10` und der Name des SQL Server-Containers `sql1`. Fügen Sie den Eintrag für den Weiterleitungshost in Active Directory wie unten gezeigt hinzu. Der Eintrag stellt sicher, dass Benutzer beim Herstellen einer Verbindung mit sql1.contoso.com an den richtigen Host weitergeleitet werden.

:::image type="content" source="media/sql-server-linux-containers-ad-auth-adutil-tutorial/host-a-record.png" alt-text="Hinzufügen des Hosteintrags":::

Für dieses Tutorial wird eine Umgebung in Azure mit drei VMs verwendet. Eine VM fungiert als Windows-Domänencontroller (DC) mit dem Domänennamen `contoso.com`. Der Domänencontroller trägt den Namen `adVM.contoso.com`. Der zweite Computer ist ein Windows-Computer namens `winbox`, auf dem Windows 10 Desktop ausgeführt wird, der als Clientfeld verwendet wird und auf dem SQL Server Management Studio (SSMS) installiert ist. Der dritte Computer ist ein Ubuntu 18.04 LTS-Computer namens `myubuntu`, der SQL Server-Container hostet. Alle Computer wurde in die `contoso.com`-Domäne eingebunden. Weitere Informationen finden Sie unter [Verknüpfen eines Hosts für SQL Server für Linux mit einer Active Directory-Domäne](sql-server-linux-active-directory-join-domain.md).

> [!NOTE]
> Wie später in diesem Artikel noch genauer erläutert wird, ist das Einbinden des Hostcontainercomputers in die Domäne nicht obligatorisch.

## <a name="install-adutil-preview"></a>Installieren von adutil-preview

Verwenden Sie in Abhängigkeit von der Linux-Distribution auf dem Linux-Hostcomputer die folgenden Befehle zum Installieren von adutil-preview.

> [!NOTE]
> Bei dieser Vorschauversion ist im Zusammenhang mit bestimmten Linux-Distributionen zu beachten, dass die Installation beeinträchtigt wird, wenn die adutil-Installation ohne den Parameter `ACCEPT_EULA` durchgeführt wird. Daher wird im Folgenden empfohlen, das adutil-preview-Tool mit `ACCEPT_EULA=Y` zu installieren. Sie können vor der Installation den [Endbenutzer-Lizenzvertrag](https://go.microsoft.com/fwlink/?linkid=2151376) der Vorschauversion lesen. Wir arbeiten aktiv an der Behebung dieses Problems. Es sollte bis zum Release der allgemein verfügbaren Version behoben sein.

### <a name="rhel"></a>RHEL

1. Laden Sie die Konfigurationsdatei für das Microsoft Red Hat-Repository herunter.

    ```bash
    sudo curl -o /etc/yum.repos.d/msprod.repo https://packages.microsoft.com/config/rhel/8/prod.repo
    ```

1. Wenn Sie eine frühere Version von adutil installiert hatten, entfernen Sie alle älteren adutil-Pakete.

    ```bash
    sudo yum remove adutil
    ```

1. Führen Sie die folgenden Befehle aus, um adutil-preview zu installieren. `ACCEPT_EULA=Y` akzeptiert den Endbenutzer-Lizenzvertrag der Vorschauversion für adutil. Den Endbenutzer-Lizenzvertrag finden Sie unter dem Pfad `/usr/share/adutil/`.

    ```bash
    sudo ACCEPT_EULA=Y yum install -y adutil-preview
    ```

### <a name="ubuntu"></a>Ubuntu

1. Importieren Sie die GPG-Schlüssel des öffentlichen Repository, und registrieren Sie dann das Microsoft Ubuntu-Repository.

    ```bash
    curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
    sudo curl https://packages.microsoft.com/config/ubuntu/18.04/prod.list | sudo tee /etc/apt/sources.list.d/msprod.list
    ```

1. Wenn Sie eine frühere Version von adutil installiert hatten, entfernen Sie alle älteren adutil-Pakete mithilfe der folgenden Befehle.

    ```bash
    sudo apt-get remove adutil
    ```

1. Führen Sie den folgenden Befehl aus, um adutil-preview zu installieren. `ACCEPT_EULA=Y` akzeptiert den Endbenutzer-Lizenzvertrag der Vorschauversion für adutil. Den Endbenutzer-Lizenzvertrag finden Sie unter dem Pfad `/usr/share/adutil/`.

    ```bash
    sudo apt-get update
    sudo ACCEPT_EULA=Y apt-get install -y adutil-preview
    ```

### <a name="sles"></a>SLES

1. Fügen Sie das Microsoft SQL Server-Repository zu Zypper hinzu.

    ```bash
    sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/prod.repo
    ```

1. Wenn Sie eine frühere Version von adutil installiert hatten, entfernen Sie alle älteren adutil-Pakete.

    ```bash
    sudo zypper remove adutil
    ```

1. Führen Sie den folgenden Befehl aus, um adutil-preview zu installieren. `ACCEPT_EULA=Y` akzeptiert den Endbenutzer-Lizenzvertrag der Vorschauversion für adutil. Den Endbenutzer-Lizenzvertrag finden Sie unter dem Pfad `/usr/share/adutil/`.

    ```bash
    sudo ACCEPT_EULA=Y zypper install -y adutil-preview
    ```

## <a name="creating-the-ad-user-spns-and-sql-server-service-keytab"></a>Erstellen des AD-Benutzers, der SPNs und der Schlüsseltabelle des SQL Server-Diensts

Wenn der SQL Server für Linux-Containerhost nicht Teil der Domäne sein soll und wenn Sie die Schritte zum Einbinden des Computers in die Domäne nicht befolgt haben, befolgen Sie auf einem anderen Linux-Computer, der bereits Teil der AD-Domäne ist, die folgenden Schritte:

 1. Erstellen eines AD-Benutzers für SQL Server und Festlegen des SPN mithilfe des adutil-Tools

 2. Erstellen und Konfigurieren der Schlüsseltabellendatei für den SQL Server-Dienst

Kopieren Sie die mssql.keytab-Datei, die erstellt wurde, auf den Hostcomputer, auf dem der SQL Server-Container ausgeführt wird, und konfigurieren Sie den Container so, dass er die kopierte mssql.keytab-Datei verwendet. Optional können Sie Ihren Linux-Host, der den SQL Server-Container ausführt, auch in die AD-Domäne einbinden und die folgenden Schritte auf demselben Computer ausführen.

### <a name="create-an-ad-user-for-sql-server-and-set-the-serviceprincipalname-using-the-adutil-tool"></a>Erstellen eines AD-Benutzers für SQL Server und Festlegen des SPN mithilfe des adutil-Tools

Wenn die AD-Authentifizierung für SQL Server für Linux-Container aktiviert werden soll, müssen dazu die unten aufgeführten Schritte 1–3 auf einem Linux-Computer ausgeführt werden, der Teil der AD-Domäne ist.

1. Erwerben oder erneuern Sie das Kerberos-TGT (Ticket-Granting Ticket) mithilfe des `kinit`-Befehls. Verwenden Sie ein privilegiertes Konto für den `kinit`-Befehl. Das Konto muss über die Berechtigung verfügen, eine Verbindung mit der Domäne herzustellen. Außerdem sollte es in der Lage sein, Konten und SPNs in der Domäne zu erstellen.

    > [!IMPORTANT]
    > Vor dem Ausführen dieses Befehls sollte der Host wie im vorherigen Schritt gezeigt bereits Teil der Domäne sein.

    ```bash
    kinit privilegeduser@DOMAIN.COM
    ```

    Beispiel: Für die oben beschriebene Umgebung ist `amvin@CONTOSO.COM` das privilegierte Konto.

    ```bash
    kinit amvin@CONTOSO.COM
    ```

2. Erstellen Sie mit dem adutil-Tool den neuen Benutzer, der von SQL Server als privilegiertes AD-Konto verwendet wird.

   ```bash
   adutil user create --name sqluser --distname CN=sqluser,CN=Users,DC=CONTOSO,DC=COM --password 'P@ssw0rd'
   ```

    > [!NOTE]
    > Kennwörter können mit einer der drei folgenden Methoden angegeben werden:
    >
    > - Kennwortflag: --password \<password\>
    > - Umgebungsvariablen: `ADUTIL_ACCOUNT_PWD`
    > - Interaktive Eingabe
    >
    > Die Rangfolge der Methoden zur Kennworteingabe entspricht der Reihenfolge der oben aufgeführten Optionen. Es empfiehlt sich, das Kennwort mithilfe von Umgebungsvariablen oder interaktiven Eingaben bereitzustellen, da diese im Vergleich zum Kennwortflag sicherer sind.

    Sie können den Namen des Kontos wie oben gezeigt mithilfe des Distinguished Name (`-distname`) angeben, oder Sie können auch den Namen der Organisationseinheit (OE) verwenden. Falls Sie beide angeben, hat der Name der Organisationseinheit (`--ou`) Vorrang vor dem Distinguished Name. Sie können den folgenden Befehl ausführen, um weitere Informationen zu erhalten:

    ```bash
    adutil user create --help
    ```

3. Registrieren Sie SPNs für den oben erstellten Benutzer. Sie können den Namen des Hostcomputers anstelle des Containernamens nutzen, je nachdem, wie die Verbindung extern angezeigt werden soll. In diesem Tutorial wird Port 5433 anstelle von Port 1433 verwendet. Hierbei handelt es sich um die Portzuordnung für den Container. Ihre Portnummer kann anders lauten.

    ```bash
    adutil spn addauto -n sqluser -s MSSQLSvc -H sql1.contoso.com -p 5433
    ```

    > [!NOTE]
    >
    > - `addauto` erstellt die SPNs automatisch, sofern das kinit-Konto über die erforderlichen Berechtigungen verfügt.
    > - `-n`: Dies ist der Name des Kontos, dem die SPNs zugewiesen werden.
    > - `-s`: Dies ist der Dienstname, der zum Erstellen von SPNs verwendet werden soll. In diesem Fall ist es der Name für den SQL Server-Dienst, weshalb der Dienstname MSSQLSvc lautet.
    > - `-H`: Dies ist der Hostname, der zum Erstellen von SPNs verwendet werden soll. Wenn kein Name angegeben ist, wird der FQDN des lokalen Hosts verwendet. Geben Sie auch für den Containernamen den FQDN an. In diesem Fall ist `sql1` der Containername, und der FQDN lautet `sql1.contoso.com`.
    > - `-p`: Dies ist der Port, der zum Erstellen von SPNs verwendet werden soll. Wenn dieser nicht angegeben wird, werden SPNs ohne einen Port generiert. SQL-Verbindungen funktionieren in diesem Fall nur, wenn die SQL Server-Instanz am Standardport 1433 lauscht.

### <a name="create-the-sql-server-service-keytab-file"></a>Erstellen der Schlüsseltabellendatei für den SQL Server-Dienst

Erstellen Sie die Schlüsseltabellendatei, die Einträge für jeden der vier zuvor erstellten SPNs und einen Eintrag für den Benutzer enthält. Die Schlüsseltabellendatei wird mit dem Container verknüpft. So ist die Erstellung an jedem Speicherort des Hosts möglich. Sie können diesen Pfad sicher ändern, solange die daraus resultierende Schlüsseltabelle ordnungsgemäß verknüpft wird, wenn Docker/Podman zum Bereitstellen des Containers verwendet werden.

Zum Erstellen der Schlüsseltabelle für alle SPNs können Sie die `createauto`-Option verwenden:

```bash
adutil keytab createauto -k /container/sql1/secrets/mssql.keytab -p 5433 -H sql1.contoso.com --password 'P@ssw0rd' -s MSSQLSvc
```

> [!NOTE]
>
> - `-k`: Dies ist der Pfad, in dem die `mssql.keytab`-Datei erstellt werden soll. Im obigen Beispiel sollte das Verzeichnis /container/sql1/secrets bereits auf dem Host vorhanden sein.
> - `-p`: Dies ist der Port, der zum Erstellen von SPNs verwendet werden soll. Wenn dieser nicht angegeben wird, werden SPNs ohne einen Port generiert.
> - `-H`: Dies ist der Hostname, der zum Erstellen von SPNs verwendet werden soll. Wenn kein Name angegeben ist, wird der FQDN des lokalen Hosts verwendet. Geben Sie auch für den Containernamen den FQDN an. In diesem Fall ist `sql1` der Containername, und der FQDN lautet `sql1.contoso.com`.
> - `-s`: Dies ist der Dienstname, der zum Erstellen von SPNs verwendet werden soll. In diesem Fall ist es der Name für den SQL Server-Dienst, weshalb der Dienstname MSSQLSvc lautet.
> - `--password`: Dies ist das Kennwort des privilegierten AD-Benutzerkontos, das zuvor erstellt wurde.
> - `-e` oder `--enctype`: Dies sind Verschlüsselungstypen für den Schlüsseltabelleneintrag. Verwenden Sie eine durch Trennzeichen getrennte Liste von Werten. Wenn kein Typ angegeben wird, wird eine interaktive Eingabeaufforderung angezeigt.

Bei der Auswahl der Verschlüsselungstypen können Sie mehr als einen auswählen. In diesem Beispiel werden `aes256-cts-hmac-sha1-96` und `arcfour-hmac` ausgewählt. Stellen Sie sicher, dass Sie den Verschlüsselungstyp auswählen, der vom Host und der Domäne unterstützt wird.

Wenn Sie den Verschlüsselungstyp nicht interaktiv auswählen möchten, können Sie mit dem Argument „-e“ im obigen Befehl den gewünschten Verschlüsselungstyp angeben. Führen Sie den folgenden Befehl aus, um zusätzliche Hilfe zu den adutil-Befehlen zu erhalten.

```bash
adutil keytab createauto --help
```

> [!NOTE]
> Bei `arcfour-hmac` handelt es sich um eine schwache Verschlüsselung und keinen empfohlenen Verschlüsselungstyp, der in der Produktionsumgebung verwendet werden soll.

Der Befehl zum Erstellen der Schlüsseltabelle für den Benutzer lautet folgendermaßen:

```bash
adutil keytab create -k /container/sql1/secrets/mssql.keytab -p sqluser --password 'P@ssw0rd!'
```

> [!NOTE]
>
> - `-k`: Dies ist der Pfad, in dem die `mssql.keytab`-Datei erstellt werden soll. Im obigen Beispiel sollte das Verzeichnis /container/sql1/secrets bereits auf dem Host vorhanden sein.
> - `-p`: Dies ist der Prinzipal, der der Schlüsseltabelle hinzugefügt wird.

Durch die adutil-Schlüsseltabelle „create/autocreate“ werden die vorherigen Dateien nicht überschrieben, sondern nur an die bereits vorhandene Datei angefügt.

Sorgen Sie dafür, dass für die erstellte Schlüsseltabelle die erforderlichen Berechtigungen festgelegt sind, wenn der Container bereitgestellt wird.

```bash
chmod 440 /container/sql1/secrets/mssql.keytab
```

> [!NOTE]
> Nun können Sie die mssql.keytab-Datei vom aktuellen Linux-Host auf den Linux-Host kopieren, auf dem der SQL Server-Container bereitgestellt werden soll. Anschließend können Sie die restlichen Schritte für den Linux-Host befolgen, der den SQL Server-Container ausführt. Wenn die oben stehenden Schritte für denselben Linux-Host ausgeführt wurden, auf dem auch die SQL-Container bereitgestellt werden sollen, befolgen Sie die nächsten Schritte ebenfalls auf diesem Host.

## <a name="create-the-config-files-to-be-used-by-the-sql-server-container"></a>Erstellen der Konfigurationsdateien zur Verwendung durch den SQL Server-Container

1. Erstellen Sie eine `mssql.conf`-Datei mit den Einstellungen für AD. Diese Datei kann an einer beliebigen Stelle auf dem Host erstellt werden und muss während des docker run-Befehls ordnungsgemäß eingebunden werden. In diesem Beispiel wurde diese Datei `mssql.conf` unter `/container/sql1 ` gespeichert. Dabei handelt es sich um das Containerverzeichnis. Der Inhalt von `mssql.conf` wird unten angezeigt:

    ```output
    [network]
    privilegedadaccount = sqluser
    kerberoskeytabfile = /var/opt/mssql/secrets/mssql.keytab
    ```

    > [!NOTE]
    >
    > - `privilagedadaccount`: Privilegierter AD-Benutzer zur Verwendung für die AD-Authentifizierung.
    > - `kerberoskeytabfile`: Hierbei handelt es sich um den Pfad im Container, unter dem sich die mssql.keytab-Datei befindet.

1. Erstellen Sie eine krb5.conf-Datei. Unten sehen Sie ein Beispiel. Groß- und Kleinschreibung muss für diese Dateien beachtet werden.

    ```output
    [libdefaults]
    default_realm = DOMAIN.COM

    [realms]
     CONTOSO.COM = {
         kdc = adVM.contoso.com
         admin_server = adVM.contoso.com
         default_domain = CONTOSO.COM
     }

    [domain_realm]
     .contoso.com = CONTOSO.COM
     contoso.com = CONTOSO.COM


1. Copy all files, `mssql.conf`, `krb5.conf`, `mssql.keytab` to a location that will be mounted to the SQL Server container. In this example, these files are placed on the host at the following locations: `mssql.conf` and `krb5.conf` at `/container/sql1/`. `mssql.keytab` is placed at the location `/container/sql1/secrets/`.

1. Make sure there's enough permission on these folders for the user running the docker/podman command. When the container starts, the user needs access to the folder path created. In this example, we provided the below permissions given to the folder path:

    ```bash
    sudo chmod 755 /container/sql1/
    ```

## <a name="mount-the-config-files-and-deploy-the-sql-server-container"></a>Einbinden der Konfigurationsdateien und Bereitstellen der SQL Server-Container

Führen Sie Ihren SQL Server-Container aus, und verknüpfen Sie die richtigen AD-Konfigurationsdateien, die zuvor erstellt wurden, wie unten gezeigt:

```bash
sudo docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=\<YourStrong@Passw0rd\>" \
-p 5433:1433 --name sql1 \
-v /container/sql1:/var/opt/mssql \
-v /container/sql1/krb5.conf:/etc/krb5.conf \
-d mcr.microsoft.com/mssql/server:2019-latest
```

> [!NOTE]
> Wenn Container mit dem LSM-Framework (Linux Security Module) ausgeführt werden, z. B. SELinux-fähige Hosts, müssen Sie die Volumes mithilfe der `Z`-Option verknüpfen. So wird Docker angewiesen, den Inhalt mit einer Bezeichnung als privat und nicht freigegebenen zu kennzeichnen. Weitere Informationen finden Sie unter [Konfigurieren der selinux-Bezeichnung](https://docs.docker.com/storage/bind-mounts/#configure-the-selinux-label).

Im vorliegenden Beispiel sind die folgenden Befehle enthalten:

```bash
sudo docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=P@ssw0rd" -p 5433:1433 --name sql1 \
-v /container/sql1:/var/opt/mssql/ \
-v /container/sql1/krb5.conf:/etc/krb5.conf \
--dns-search contoso.com \
--dns 10.0.0.4 \
--add-host adVM.contoso.com:10.0.0.4 \
--add-host contoso.com:10.0.0.4 \
--add-host contoso:10.0.0.4 \
-d mcr.microsoft.com/mssql/server:2019-latest
```

> [!NOTE]
>
> - Die Dateien `mssql.conf` und `krb5.conf` befinden sich unter dem Hostdateipfad `/container/sql1`.
> - Die `mssql.keytab`-Datei, die erstellt wurde, befindet sich unter dem Hostdateipfad `/container/sql1/secrets`.
> - Da sich der Hostcomputer in Azure befindet, müssen die AD-Details in derselben Reihenfolge an den docker run-Befehl angehängt werden. In unserem Beispiel befindet sich der Domänencontroller `adVM` in der Domäne `contoso.com`. Die IP-Adresse lautet `10.0.0.4`. Der Domänencontroller führt DNS und KDC aus.

## <a name="create-ad-based-sql-server-logins-in-transact-sql"></a>Erstellen von AD-basierten SQL Server-Anmeldeinformationen in Transact-SQL

Stellen Sie eine Verbindung mit dem SQL-Container her, und führen Sie die folgenden Befehle aus, um die Anmeldung zu erstellen und daraufhin sicherzustellen, dass diese in der Liste aufgeführt wird. Sie können diesen Befehl auf einem Clientcomputer (Windows oder Linux) ausführen, auf dem SSMS, Azure Data Studio (ADS) oder ein anderes Befehlszeilenschnittstellentool (CLI) ausgeführt wird.

```sql
create login [contoso\amvin] From Windows
SELECT name FROM sys.server_principals;
```

## <a name="connect-to-sql-server-using-ad-authentication"></a>Herstellen einer Verbindung mit SQL Server über die AD-Authentifizierung

Melden Sie sich in SQL Server mit Windows-Anmeldeinformationen mithilfe des SQL Server-Namens und der Portnummer an, um eine Verbindung mithilfe von [SSMS](../ssms/download-sql-server-management-studio-ssms.md) oder [ADS](../azure-data-studio/download-azure-data-studio.md) herzustellen. Der Name kann der Containername oder der Hostname sein. Im vorliegenden Beispiel ist der Servername `sql1.contoso.com, 5433`.

Sie können auch ein Tool wie [sqlcmd](../tools/sqlcmd-utility.md) verwenden, um eine Verbindung mit der SQL Server-Instanz in Ihrem Container herzustellen.

```bash
sqlcmd -E -S 'sql1.contoso.com, 5433'
```

## <a name="next-steps"></a>Nächste Schritte

- [Schnellstart: Ausführen von SQL Server-Containerimages mit Docker](quickstart-install-connect-docker.md)
- [Verknüpfen eines Hosts für SQL Server für Linux mit einer Active Directory-Domäne](sql-server-linux-active-directory-auth-overview.md)
