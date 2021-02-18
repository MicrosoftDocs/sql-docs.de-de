---
title: Konfigurieren der Active Directory-Authentifizierung mit SQL Server für Linux mithilfe von adutil
description: Hier erhalten Sie eine detaillierte Anleitung zum Konfigurieren der Active Directory-Authentifizierung mit SQL Server für Linux mithilfe von adutil.
author: amvin87
ms.author: amitkh
ms.reviewer: vanto
ms.date: 12/10/2020
ms.topic: tutorial
ms.prod: sql
ms.technology: linux
moniker: '>= sql-server-linux-2017 || >= sql-server-2017 || =sqlallproducts-allversions'
ms.openlocfilehash: e1e14db6e55ca4af4f76220c0e3f377c38e41165
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100351871"
---
# <a name="tutorial-configure-active-directory-authentication-with-sql-server-on-linux-using-adutil"></a>Tutorial: Konfigurieren der Active Directory-Authentifizierung mit SQL Server für Linux mithilfe von adutil

> [!NOTE]
> **adutil** ist aktuell als **öffentliche Vorschauversion** verfügbar.

In diesem Tutorial wird erläutert, wie Sie die Active Directory-Authentifizierung (AD) mit SQL Server für Linux mithilfe von adutil konfigurieren. Eine weitere Methode zum Konfigurieren der AD-Authentifizierung ist die Verwendung von ktpass. Weitere Informationen finden Sie unter [Tutorial: Verwenden der Active Directory-Authentifizierung für SQL Server für Linux](sql-server-linux-active-directory-authentication.md).

Dieses Tutorial umfasst die folgenden Aufgaben:

> [!div class="checklist"]
> - Installieren von adutil-preview
> - Verknüpfen des Linux-Computers mit Ihrer AD-Domäne
> - Erstellen eines AD-Benutzers für SQL Server und Festlegen des Dienstprinzipalnamens (Service Principal Name, SPN) mithilfe des adutil-Tools
> - Erstellen der Schlüsseltabellendatei für den SQL Server-Dienst
> - Konfigurieren von SQL Server für die Verwendung der Schlüsseltabellendatei
> - Erstellen von AD-basierten SQL Server-Anmeldeinformationen mit Transact-SQL
> - Herstellen einer Verbindung zu SQL Server mit der AD-Authentifizierung

## <a name="prerequisites"></a>Voraussetzungen

Folgende Voraussetzungen bestehen für das Konfigurieren der AD-Authentifizierung:

- Richten Sie einen AD-Domänencontroller (Windows) in Ihrem Netzwerk ein.
- Installieren Sie das adutil-preview-Tool auf einem Linux-Hostcomputer. Befolgen Sie den unten stehenden Abschnitt basierend auf der Linux-Distribution, die Sie zum Installieren von adutil-preview verwenden.

## <a name="install-adutil-preview"></a>Installieren von adutil-preview

Verwenden Sie auf dem Linux-Hostcomputer die folgenden Befehle zum Installieren von adutil-preview.

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

1. Führen Sie die folgenden Befehle aus, um adutil-preview zu installieren. `ACCEPT_EULA=Y` akzeptiert den Endbenutzer-Lizenzvertrag der Vorschauversion für adutil. Den Endbenutzer-Lizenzvertrag finden Sie unter dem Pfad „/usr/share/adutil/“.

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

1. Führen Sie den folgenden Befehl aus, um adutil-preview zu installieren. `ACCEPT_EULA=Y` akzeptiert den Endbenutzer-Lizenzvertrag der Vorschauversion für adutil. Den Endbenutzer-Lizenzvertrag finden Sie unter dem Pfad „/usr/share/adutil/“.

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

1. Führen Sie den folgenden Befehl aus, um adutil-preview zu installieren. `ACCEPT_EULA=Y` akzeptiert den Endbenutzer-Lizenzvertrag der Vorschauversion für adutil. Den Endbenutzer-Lizenzvertrag finden Sie unter dem Pfad „/usr/share/adutil/“.

    ```bash
    sudo ACCEPT_EULA=Y zypper install -y adutil-preview
    ```

## <a name="domain-machine-preparation"></a>Vorbereitung des Domänencomputers

Stellen Sie sicher, dass in Active Directory ein Eintrag für den Weiterleitungshost (A) für die IP-Adresse des Linux-Hostcomputers hinzugefügt wurde. In diesem Tutorial lautet die IP-Adresse des `myubuntu`-Hostcomputers `10.0.0.10`. Fügen Sie den Eintrag für den Weiterleitungshost in Active Directory wie unten gezeigt hinzu. Der Eintrag stellt sicher, dass Benutzer beim Herstellen einer Verbindung mit myubuntu.contoso.com an den richtigen Host weitergeleitet werden.

:::image type="content" source="media/sql-server-linux-ad-auth-adutil-tutorial/host-a-record.png" alt-text="Hinzufügen des Hosteintrags":::

Für dieses Tutorial wird eine Umgebung in Azure mit drei VMs verwendet. Eine VM fungiert als Windows-Domänencontroller (DC) mit dem Domänennamen `contoso.com`. Der Domänencontroller trägt den Namen `adVM.contoso.com`. Der zweite Computer ist ein Windows-Computer namens `winbox`, auf dem Windows 10 Desktop ausgeführt wird, der als Clientfeld verwendet wird und auf dem SQL Server Management Studio (SSMS) installiert ist. Der dritte Computer ist ein Ubuntu 18.04 LTS-Computer namens `myubuntu`, der SQL Server hostet.

## <a name="join-the-linux-host-machine-to-your-ad-domain"></a>Verknüpfen des Linux-Hostcomputers mit Ihrer AD-Domäne

Verknüpfen Sie Ihren Linux-Host für SQL Server mit einem Active Directory-Domänencontroller. Informationen dazu finden Sie unter [Verknüpfen von eines Linux-Hosts für SQL Server mit einer Active Directory-Domäne](sql-server-linux-active-directory-join-domain.md).

## <a name="create-an-ad-user-for-sql-server-and-set-the-serviceprincipalname-spn-using-the-adutil-tool"></a>Erstellen eines AD-Benutzers für SQL Server und Festlegen des Dienstprinzipalnamens (Service Principal Name, SPN) mithilfe des adutil-Tools

1. Erwerben oder erneuern Sie das Kerberos-TGT (Ticket-Granting Ticket) mithilfe des `kinit`-Befehls. Verwenden Sie ein privilegiertes Konto für den `kinit`-Befehl. Das Konto muss über die Berechtigung verfügen, eine Verbindung mit der Domäne herzustellen. Außerdem sollte es in der Lage sein, Konten und SPNs in der Domäne zu erstellen.

    > [!IMPORTANT]
    > Vor dem Ausführen dieses Befehls sollte der Hostcomputer wie im vorherigen Schritt gezeigt bereits Teil der Domäne sein.

    ```bash
    kinit privilegeduser@DOMAIN.COM
    ```

    Beispiel: Für die oben beschriebene Umgebung ist `amvin@CONTOSO.COM` das privilegierte Konto.

    ```bash
    kinit amvin@CONTOSO.COM
    ```

2. Erstellen Sie mit dem adutil-Tool den neuen Benutzer, der von SQL Server als privilegiertes AD-Konto verwendet wird.

   ```bash
   adutil user create --name sqluser -distname CN=sqluser,CN=Users,DC=CONTOSO,DC=COM --password 'P@ssw0rd'
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

3. Registrieren Sie SPNs für den oben erstellten Prinzipal. Verwenden Sie den vollqualifizierten Domänennamen (FQDN) des Computers. In diesem Tutorial verwenden Sie den SQL Server-Standardport 1433. Ihre Portnummer kann anders lauten.

    ```bash
    adutil spn addauto -n sqluser -s MSSQLSvc -H myubuntu.contoso.com -p 1433
    ```

    > [!NOTE]
    >
    > - `addauto` erstellt die SPNs automatisch, sofern das kinit-Konto über die erforderlichen Berechtigungen verfügt.
    > - `-n`: Dies ist der Name des Kontos, dem die SPNs zugewiesen werden.
    > - `-s`: Dies ist der Dienstname, der zum Erstellen von SPNs verwendet werden soll. In diesem Fall ist es der Name für den SQL Server-Dienst, weshalb der Dienstname `MSSQLSvc` lautet.
    > - `-H`: Dies ist der Hostname, der zum Erstellen von SPNs verwendet werden soll. Wenn kein Name angegeben ist, wird der FQDN des lokalen Hosts verwendet. In diesem Fall ist `myubuntu` der Hostname, und der FQDN lautet `myubuntu.contoso.com`.
    > - `-p`: Dies ist der Port, der zum Erstellen von SPNs verwendet werden soll. Wenn dieser nicht angegeben wird, werden SPNs ohne einen Port generiert. SQL-Verbindungen funktionieren in diesem Fall nur, wenn die SQL Server-Instanz am Standardport 1433 lauscht.

## <a name="create-the-sql-server-service-keytab-file"></a>Erstellen der Schlüsseltabellendatei für den SQL Server-Dienst

Erstellen Sie die Schlüsseltabellendatei, die Einträge für jeden der vier zuvor erstellten SPNs und einen Eintrag für den Benutzer enthält.

```bash
adutil keytab createauto -k /var/opt/mssql/secrets/mssql.keytab -p 1433 -H myubuntu.contoso.com --password 'P@ssw0rd' -s MSSQLSvc 
```

> [!NOTE]
>
> - `-k`: Dies ist der Pfad, in dem die `mssql.keytab`-Datei erstellt werden soll. Im obigen Beispiel sollte das Verzeichnis `/var/opt/mssql/secrets/` bereits auf dem Host vorhanden sein.
> - `-p`: Dies ist der Port, der zum Erstellen von SPNs verwendet werden soll. Wenn dieser nicht angegeben wird, werden SPNs ohne einen Port generiert.
> - `-H`: Dies ist der Hostname, der zum Erstellen von SPNs verwendet werden soll. Wenn kein Name angegeben ist, wird der FQDN des lokalen Hosts verwendet. In diesem Fall ist `myubuntu` der Hostname, und der FQDN lautet `myubuntu.contoso.com`.
> - `-s`: Dies ist der Dienstname, der zum Erstellen von SPNs verwendet werden soll. In diesem Fall ist es der Name für den SQL Server-Dienst, weshalb der Dienstname `MSSQLSvc` lautet.
> - `--password`: Dies ist das Kennwort des privilegierten AD-Benutzerkontos, das zuvor erstellt wurde.
> - `-e` oder `--enctype`: Dies sind Verschlüsselungstypen für den Schlüsseltabelleneintrag. Verwenden Sie eine durch Trennzeichen getrennte Liste von Werten. Wenn kein Typ angegeben wird, wird eine interaktive Eingabeaufforderung angezeigt.

Bei der Auswahl der Verschlüsselungstypen können Sie mehr als einen auswählen. In diesem Beispiel werden `aes256-cts-hmac-sha1-96` und `arcfour-hmac` ausgewählt. Stellen Sie sicher, dass Sie den Verschlüsselungstyp auswählen, der vom Host und der Domäne unterstützt wird.

Wenn Sie den Verschlüsselungstyp nicht interaktiv auswählen möchten, können Sie mit dem Argument „-e“ im obigen Befehl den gewünschten Verschlüsselungstyp angeben. Führen Sie den folgenden Befehl aus, um zusätzliche Hilfe zu den adutil-Befehlen zu erhalten.

```bash
adutil keytab createauto --help
```

> [!NOTE]
> Bei `arcfour-hmac` handelt es sich um eine schwache Verschlüsselung und keinen empfohlenen Verschlüsselungstyp, der in der Produktionsumgebung verwendet werden soll.

Fügen Sie in der Schlüsseltabelle für den Prinzipalnamen und das zugehörige Kennwort, das von SQL Server zum Herstellen einer Verbindung mit AD verwendet wird, einen Eintrag hinzu:

```bash
adutil keytab create -k /var/opt/mssql/secrets/mssql.keytab -p sqluser --password 'P@ssw0rd!'
```

> [!NOTE]
>
> - `-k`: Dies ist der Pfad, in dem die `mssql.keytab`-Datei erstellt werden soll.
> - `-p`: Dies ist der Prinzipal, der der Schlüsseltabelle hinzugefügt wird.

Durch die adutil-Schlüsseltabelle „create/autocreate“ werden die vorherigen Dateien nicht überschrieben, sondern nur an die bereits vorhandene Datei angefügt.

Stellen Sie sicher, dass die erstellte Schlüsseltabelle dem `mssql`-Benutzer gehört und nur der `mssql`-Benutzer Lese- und Schreibzugriff auf die Datei hat. Sie können die Befehle `chown` und `chmod` wie im Folgenden gezeigt ausführen:

```bash
chown mssql. /var/opt/mssql/secrets/mssql.keytab
chmod 440 /var/opt/mssql/secrets/mssql.keytab
```

## <a name="configure-sql-server-to-use-the-keytab"></a>Konfigurieren von SQL Server für die Verwendung der Schlüsseltabelle

Führen Sie die folgenden Befehle aus, um SQL Server für die Verwendung der im vorherigen Schritt erstellten Schlüsseltabelle zu konfigurieren und das privilegierte AD-Konto als den zuvor erstellten Benutzer festzulegen. In diesem Beispiel ist `sqluser` der Benutzername.

```bash
/opt/mssql/bin/mssql-conf set network.kerberoskeytabfile /var/opt/mssql/secrets/mssql.keytab
/opt/mssql/bin/mssql-conf set network.privilegedadaccount sqluser
```

## <a name="restart-sql-server"></a>Neu starten von SQL Server

Führen Sie den folgenden Befehl aus, um den SQL Server-Dienst neu zu starten:

```bash
sudo systemctl restart mssql-server
```

## <a name="create-ad-based-sql-server-logins-in-transact-sql"></a>Erstellen von AD-basierten SQL Server-Anmeldeinformationen in Transact-SQL

Stellen Sie eine Verbindung mit der SQL Server-Instanz her, und führen Sie die folgenden Befehle aus, um die Anmeldung zu erstellen und daraufhin sicherzustellen, dass diese in der Liste aufgeführt wird.

```sql
create login [contoso\amvin] From Windows
SELECT name FROM sys.server_principals;
```

## <a name="connect-to-sql-server-using-ad-authentication"></a>Herstellen einer Verbindung mit SQL Server über die AD-Authentifizierung

Melden Sie sich mit Ihren Windows-Anmeldeinformationen bei der SQL Server-Instanz an, um mithilfe von [SSMS](../ssms/download-sql-server-management-studio-ssms.md) oder [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md) eine Verbindung herzustellen.

Sie können auch ein Tool wie [sqlcmd](../tools/sqlcmd-utility.md) verwenden, um mithilfe der Windows-Authentifizierung eine Verbindung mit der SQL Server-Instanz herzustellen.

```bash
sqlcmd -E -S 'myubuntu.contoso.com'
```

## <a name="next-steps"></a>Nächste Schritte

- [Verknüpfen eines Hosts für SQL Server für Linux mit einer Active Directory-Domäne](sql-server-linux-active-directory-auth-overview.md)
- Weitere Informationen zur Konfiguration der AD-Authentifizierung mit SQL Server für Linux-Containern finden Sie unter [Konfigurieren der Active Directory-Authentifizierung mit SQL Server für Linux-Containern](sql-server-linux-containers-ad-auth-adutil-tutorial.md).
