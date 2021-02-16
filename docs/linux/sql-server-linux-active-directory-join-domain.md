---
title: Verknüpfen von SQL Server für Linux mit Active Directory
titleSuffix: SQL Server
description: Diese Artikel enthält Anleitungen zum Hinzufügen eines Linux-Hostcomputers für SQL Server zu einer AD-Domäne. Sie können ein integriertes SSSD-Paket oder AD-Drittanbieter verwenden.
author: tejasaks
ms.author: tejasaks
ms.reviewer: vanto
ms.date: 11/30/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 33ce3edfd7afee30e09db78b908bdaebd9a4e322
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345658"
---
# <a name="join-sql-server-on-a-linux-host-to-an-active-directory-domain"></a>Verknüpfen eines Hosts für SQL Server für Linux mit einer Active Directory-Domäne

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Dieser Artikel enthält allgemeine Anleitungen zum Verknüpfen eines Hostcomputers für SQL Server für Linux mit einer Active Directory-Domäne (AD). Es gibt zwei Möglichkeiten: Sie können entweder ein integriertes SSSD-Paket oder ein Drittanbietertool für Active Directory verwenden. Beispielsweise können Sie die folgenden Drittanbieterprodukte für den Domänenbeitritt verwenden: [PowerBroker Identity Services (PBIS)](https://www.beyondtrust.com/), [One Identity](https://www.oneidentity.com/products/authentication-services/) und [Centrify](https://www.centrify.com/). Außerdem wird in diesem Artikel ausführlich beschrieben, wie Sie Ihre Active Directory-Konfiguration überprüfen können. Wir werden jedoch keine Anweisungen dazu bereitstellen, wie Sie mithilfe von Drittanbietertools einen Computer mit einer Domäne verknüpfen können.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie die Active Directory-Authentifizierung konfigurieren können, müssen Sie einen Active Directory-Domänencontroller (Windows) für Ihr Netzwerk einrichten. Verknüpfen Sie dann Ihren Host für SQL Server für Linux mit einer Active Directory-Domäne.

> [!IMPORTANT]
> Die in diesem Artikel beispielhaft beschriebenen Schritte dienen lediglich zur Orientierung und beziehen sich auf die Betriebssysteme Ubuntu 16.04, Red Hat Enterprise Linux (RHEL) 7.x und SUSE Enterprise Linux (SLES) 12. Die tatsächlichen Schritte können sich je nach Konfiguration Ihrer gesamten Umgebung und Version des Betriebssystems geringfügig unterscheiden. So verwendet Ubuntu 18.04 als Tool für die Netzwerkverwaltung und -konfiguration beispielsweise Netplan, während Red Hat Enterprise Linux (RHEL) 8.x unter anderem nmcli verwendet. Es wird empfohlen, dass Sie sich mit den System- und Domänenadministratoren für Ihre Umgebung in Verbindung setzen, um genaue Informationen zu Tools, Konfiguration und Anpassung sowie zur Behandlung von möglicherweise auftretenden Problemen zu erhalten.

### <a name="reverse-dns-rdns"></a>Reverse DNS (RDNS)

Wenn Sie einen Computer, auf dem Windows Server ausgeführt wird, als Domänencontroller einrichten, verfügen Sie möglicherweise nicht standardmäßig über eine RDNS-Zone. Stellen Sie sicher, dass für den Domänencontroller und die IP-Adresse des Linux-Computers, auf dem SQL Server ausgeführt wird, eine gültige RDNS-Zone vorhanden ist.

Sorgen Sie außerdem dafür, dass ein auf Ihre Domänencontroller verweisender PTR-Eintrag vorhanden ist.

## <a name="check-the-connection-to-a-domain-controller"></a>Überprüfen der Verbindung mit einem Domänencontroller

Überprüfen Sie, ob Sie den Domänencontroller mithilfe des kurzen und des vollqualifizierten Domänennamens (Fully Qualified Domain Name, FQDN) sowie mithilfe des Hostnamens des Domänencontrollers kontaktieren können. Die IP-Adresse des Domänencontrollers sollte auch in den FQDN des Domänencontrollers aufgelöst werden:

```bash
ping contoso
ping contoso.com
ping dc1.contoso.com
nslookup <IP address of dc1.contoso.com>
```

> [!TIP]
> In diesem Tutorial werden die Beispielnamen **contoso.com** (für die Domäne) und **CONTOSO.COM** (für den Bereich) verwendet. Außerdem wird **DC1.CONTOSO.COM** als vollqualifizierter Domänenname des Domänencontrollers verwendet. Diese Namen müssen Sie durch eigene ersetzen.

Wenn eine dieser Namensprüfungen fehlschlägt, aktualisieren Sie Ihre Domänensuchliste. In den folgenden Abschnitten finden Sie Anweisungen für Ubuntu, Red Hat Enterprise Linux (RHEL) und SuSE Linux Enterprise Server (SLES).

### <a name="ubuntu-1604"></a>Ubuntu 16.04

1. Bearbeiten Sie die Datei **/etc/network/interfaces** so, dass Ihre Active Directory-Domäne in die Domänensuchliste aufgenommen wird:

   ```/etc/network/interfaces
   # The primary network interface
   auto eth0
   iface eth0 inet dhcp
   dns-nameservers **<AD domain controller IP address>**
   dns-search **<AD domain name>**
   ```

   > [!NOTE]
   > Die Netzwerkschnittstelle `eth0` kann je nach Computer abweichen. Führen Sie **ifconfig** aus, um herauszufinden, welche Schnittstelle für Ihren Computer verwendet wird. Kopieren Sie dann die Schnittstelle, die eine IP-Adresse aufweist sowie Bytes übertragen und empfangen hat.

1. Nachdem Sie diese Datei bearbeitet haben, sollten Sie den Netzwerkdienst neu starten:

   ```bash
   sudo ifdown eth0 && sudo ifup eth0
   ```

1. Überprüfen Sie als Nächstes, ob die Datei **/etc/resolv.conf** eine Zeile wie die Folgende enthält:

   ```/etc/resolv.conf
   search contoso.com com  
   nameserver **<AD domain controller IP address>**
   ```

### <a name="ubuntu-1804"></a>Ubuntu 18.04

1. Bearbeiten Sie die Datei [sudo vi /etc/netplan/******.yaml], sodass Ihre Active Directory-Domäne in die Domänensuchliste aufgenommen wird:

   ```/etc/netplan/******.yaml
   network:
     ethernets:
       eth0:
               dhcp4: true

               dhcp6: true
               nameservers:
                       addresses: [ **<AD domain controller IP address>**]
                       search: [**<AD domain name>**]
     version: 2
   ```

   > [!NOTE]
   > Die Netzwerkschnittstelle `eth0` kann je nach Computer abweichen. Führen Sie **ifconfig** aus, um herauszufinden, welche Schnittstelle für Ihren Computer verwendet wird. Kopieren Sie dann die Schnittstelle, die eine IP-Adresse aufweist sowie Bytes übertragen und empfangen hat.

1. Nachdem Sie diese Datei bearbeitet haben, sollten Sie den Netzwerkdienst neu starten:

   ```bash
   sudo netplan apply
   ```

1. Überprüfen Sie als Nächstes, ob die Datei **/etc/resolv.conf** eine Zeile wie die Folgende enthält:

   ```/etc/resolv.conf
   search contoso.com com  
   nameserver **<AD domain controller IP address>**
   ```

### <a name="rhel-7x"></a>RHEL 7.x

1. Bearbeiten Sie die Datei **/etc/sysconfig/network-scripts/ifcfg-eth0** so, dass Ihre Active Directory-Domäne in die Domänensuchliste aufgenommen wird. Stattdessen können Sie auch eine andere Konfigurationsdatei entsprechend bearbeiten:

   ```/etc/sysconfig/network-scripts/ifcfg-eth0
   PEERDNS=no
   DNS1=**<AD domain controller IP address>**
   DOMAIN="contoso.com com"
   ```

1. Nachdem Sie diese Datei bearbeitet haben, sollten Sie den Netzwerkdienst neu starten:

   ```bash
   sudo systemctl restart network
   ```

1. Überprüfen Sie als Nächstes, ob die Datei **/etc/resolv.conf** eine Zeile wie die Folgende enthält:

   ```/etc/resolv.conf
   search contoso.com com  
   nameserver **<AD domain controller IP address>**
   ```

1. Wenn Sie weiterhin nicht den Domänencontroller pingen können, suchen Sie den vollqualifizierten Domänennamen und die IP-Adresse des Domänencontrollers. Der Domänenname kann z. B. **DC1.CONTOSO.COM** lauten. Fügen Sie den folgenden Eintrag zur Datei **/etc/hosts** hinzu:

   ```/etc/hosts
   **<IP address>** DC1.CONTOSO.COM CONTOSO.COM CONTOSO
   ```

### <a name="sles-12"></a>SLES 12

1. Bearbeiten Sie die Datei **/etc/sysconfig/network/config** so, dass die IP-Adresse des Active Directory-Domänencontrollers für DNS-Abfragen verwendet und Ihre Active Directory-Domäne in die Domänensuchliste aufgenommen wird:

   ```/etc/sysconfig/network/config
   NETCONFIG_DNS_STATIC_SEARCHLIST=""
   NETCONFIG_DNS_STATIC_SERVERS="**<AD domain controller IP address>**"
   ```

1. Nachdem Sie diese Datei bearbeitet haben, sollten Sie den Netzwerkdienst neu starten:

   ```bash
   sudo systemctl restart network
   ```

1. Überprüfen Sie als Nächstes, ob die Datei **/etc/resolv.conf** eine Zeile wie die Folgende enthält:

   ```/etc/resolv.conf
   search contoso.com com
   nameserver **<AD domain controller IP address>**
   ```

## <a name="join-to-the-ad-domain"></a>Beitreten zur AD-Domäne

Nachdem Sie die Basiskonfiguration und die Konnektivität mit dem Domänencontroller überprüft haben, haben Sie zwei Möglichkeiten, um einen Hostcomputer für SQL Server für Linux mit dem Active Directory-Domänencontroller zu verknüpfen:

- [Option 1: Verwenden eines SSSD-Pakets](#option1)
- [Option 2: Verwenden von OpenLDAP-Drittanbietertools](#option2)

### <a name="option-1-use-sssd-package-to-join-ad-domain"></a><a id="option1"></a> Option 1: Verwenden des SSSD-Pakets für den Beitritt zur AD-Domäne

Bei dieser Methode wird der SQL Server-Host mithilfe von **realmd** und **SSSD**-Paketen mit einer AD-Domäne verknüpft.

> [!NOTE]
> Diese Methode wird bevorzugt, um einen Linux-Host mit einem AD-Domänencontroller zu verknüpfen.

Führen Sie die folgenden Schritte aus, um einen SQL Server-Host mit einer Active Directory-Domäne zu verknüpfen:

1. Verwenden Sie [realmd](https://www.freedesktop.org/software/realmd/docs/guide-active-directory-join), um den Hostcomputer mit Ihrer AD-Domäne zu verknüpfen. Zuvor müssen Sie mithilfe des Paket-Managers für Ihre Linux-Distribution sowohl das **realmd**- als auch das Kerberos-Clientpaket auf dem SQL Server-Hostcomputer installieren:

   **RHEL:**

   ```base
   sudo yum install realmd krb5-workstation
   ```
   
   **SLES 12:**
   
   Beachten Sie, dass diese Schritte speziell für SLES 12 gelten, die einzige offiziell unterstützte SUSE-Version für Linux.

   ```bash
   sudo zypper addrepo https://download.opensuse.org/repositories/network/SLE_12/network.repo
   sudo zypper refresh
   sudo zypper install realmd krb5-client sssd-ad
   ```

   **Ubuntu 16.04:**

   ```bash
   sudo apt-get install realmd krb5-user software-properties-common python-software-properties packagekit
   ```

   **Ubuntu 18.04:**

   ```bash
   sudo apt-get install realmd krb5-user software-properties-common python3-software-properties packagekit
   sudo apt-get install adcli libpam-sss libnss-sss sssd sssd-tools
   ```

1. Wenn Sie bei der Installation des Pakets für den Kerberos-Client aufgefordert werden, einen Bereichsnamen einzugeben, geben Sie Ihren Domänennamen in Großbuchstaben ein.

1. Nachdem Sie sich vergewissert haben, dass Ihr DNS ordnungsgemäß konfiguriert wurde, führen Sie den folgenden Befehl aus, um der Domäne beizutreten. Sie müssen sich mit einem AD-Konto authentifizieren, das über ausreichende Berechtigungen in AD verfügt, um einen neuen Computer mit der Domäne verknüpfen zu können. Mithilfe des folgenden Befehls wird ein neues Computerkonto in AD und die KEYTAB-Datei für den Host (**/etc/krb5.keytab**) erstellt, die Domäne in der Datei **/etc/sssd/sssd.conf** wird konfiguriert, und die Datei **/etc/krb5.conf** wird aktualisiert.

   Legen Sie den Hostnamen des Computers aufgrund eines Problems mit **realmd** zunächst auf den FQDN anstatt auf den Computernamen fest. Andernfalls erstellt **realmd** möglicherweise nicht alle erforderlichen SPNs für den Computer, und DNS-Einträge werden nicht automatisch aktualisiert, auch wenn der Domänencontroller dynamische DNS-Updates unterstützt.
   
   ```bash
   sudo hostname <old hostname>.contoso.com
   ```
   
   Nachdem Sie den obigen Befehl ausgeführt haben, sollte die Datei „/etc/hostname“ <old hostname>.contoso.com enthalten.

   ```bash
   sudo realm join contoso.com -U 'user@CONTOSO.COM' -v
   ```

   Dann sollte die Meldung `Successfully enrolled machine in realm` angezeigt werden.

   In der folgenden Tabelle sind einige Fehlermeldungen,die Sie erhalten könnten, einschließlich möglicher Lösungen aufgeführt:

   | Fehlermeldung | Empfehlung |
   |---|---|
   | `Necessary packages are not installed` | Installieren Sie diese Pakete mithilfe des Paket-Managers für Ihre Linux-Distribution, bevor Sie den Befehl „realm join“ erneut ausführen. |
   | `Insufficient permissions to join the domain` | Vergewissern Sie sich bei einem Domänenadministrator, ob Sie über ausreichende Berechtigungen verfügen, um Linux-Computer mit einer Domäne verknüpfen zu können. |
   | `KDC reply did not match expectations` | Sie haben möglicherweise nicht den richtigen Bereichsnamen für den Benutzer angegeben. Bei Bereichsnamen wird die Groß-/Kleinschreibung beachtet. Normalerweise werden sie in Großbuchstaben geschrieben und können über den Befehl „realm discover contoso.com“ ermittelt werden. |

   SQL Server verwendet SSSD und NSS, um Benutzerkonten und Gruppen Sicherheits-IDs zuzuordnen. SSSD muss konfiguriert sein und ausgeführt werden, damit SQL Server erfolgreich Anmeldeinformationen für AD erstellen kann. **realmd** tut dies in der Regel zwar automatisch beim Domänenbeitritt, aber in einigen Fällen müssen Sie sich selbst darum kümmern.

   Weitere Informationen finden Sie unter [Manuelles Konfigurieren von SSSD](https://access.redhat.com/articles/3023951) und [Konfigurieren von NSS für SSSD](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/system-level_authentication_guide/configuring_services#Configuration_Options-NSS_Configuration_Options).

1. Überprüfen Sie, ob Sie jetzt über die Domäne Informationen zum Benutzer erfassen und als dieser Benutzer ein Kerberos-Ticket abrufen können. Im folgenden Beispiel werden dafür die Befehle **id**, [kinit](https://web.mit.edu/kerberos/krb5-1.12/doc/user/user_commands/kinit.html) und [klist](https://web.mit.edu/kerberos/krb5-1.12/doc/user/user_commands/klist.html) verwendet.

   ```bash
   id user@contoso.com

   uid=1348601103(user@contoso.com) gid=1348600513(domain group@contoso.com) groups=1348600513(domain group@contoso.com)

   kinit user@CONTOSO.COM

   Password for user@CONTOSO.COM:

   klist
   Ticket cache: FILE:/tmp/krb5cc_1000
   Default principal: user@CONTOSO.COM
   ```

   > [!NOTE]
   > - Wenn der Befehl **id user\@contoso.com**`No such user` zurückgibt, vergewissern Sie sich, ob der SSSD-Dienst erfolgreich gestartet wurde, indem Sie den Befehl `sudo systemctl status sssd` ausführen. Wenn der Dienst ausgeführt, aber der Fehler weiterhin angezeigt wird, können Sie versuchen, die ausführliche Protokollierung für SSSD zu aktivieren. Weitere Informationen finden Sie in der Red Hat-Dokumentation zur [Behandlung von Problemen mit SSSD](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/7/html/System-Level_Authentication_Guide/trouble.html#SSSD-Troubleshooting).
   >
   > - Wenn der Befehl **kinit user\@CONTOSO.COM**`KDC reply did not match expectations while getting initial credentials` zurückgibt, vergewissern Sie sich, dass Sie den Bereichsnamen in Großbuchstaben angegeben haben.

Weitere Informationen finden Sie in der Red Hat-Dokumentation zum [Ermitteln und Verknüpfen von Identitätsdomänen](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/7/html/Windows_Integration_Guide/realmd-domain.html).

### <a name="option-2-use-third-party-openldap-provider-utilities"></a><a id="option2"></a> Option 2: Verwenden von OpenLDAP-Drittanbietertools

Sie können auch Hilfsprogramme von Drittanbietern wie [PBIS](https://www.beyondtrust.com/), [VAS](https://www.oneidentity.com/products/authentication-services/) oder [Centrify](https://www.centrify.com/) verwenden. In diesem Artikel können aber keine Anweisungen zu den einzelnen Hilfsprogrammen bereitgestellt werden. Sie müssen zunächst mithilfe eines dieser Hilfsprogramme den Linux-Host für SQL Server mit der Domäne verknüpfen, bevor Sie fortfahren können.  

SQL Server verwendet keinen Integratorcode oder Bibliotheken von Drittanbietern für AD-bezogene Abfragen. SQL Server fragt AD bei diesem Setup immer mithilfe von direkten Aufrufen der OpenLDAP-Bibliothek ab. Die Drittanbieterintegratoren werden nur verwendet, um den Linux-Host mit der AD-Domäne zu verknüpfen. SQL Server kommuniziert nicht direkt mit diesen Hilfsprogrammen.

> [!IMPORTANT]
> Lesen Sie sich die Empfehlungen für die Verwendung der **mssql-conf-Konfiguration** `network.disablesssd` im Abschnitt **Zusätzliche Konfigurationsoptionen** des Artikels [Verwenden der Active Directory-Authentifizierung mit SQL Server für Linux](sql-server-linux-active-directory-authentication.md#additionalconfig) durch.

Vergewissern Sie sich, dass die Datei **/etc/krb5.conf** ordnungsgemäß konfiguriert ist. Bei den meisten Drittanbietern für Active Directory erfolgt diese Konfiguration automatisch. Überprüfen Sie jedoch die Datei **/etc/krb5.conf** auf die folgenden Werte, um zukünftige Probleme zu vermeiden:

```/etc/krb5.conf
[libdefaults]
default_realm = CONTOSO.COM

[realms]
CONTOSO.COM = {
}

[domain_realm]
contoso.com = CONTOSO.COM
.contoso.com = CONTOSO.COM
```

## <a name="check-that-the-reverse-dns-is-properly-configured"></a>Überprüfen, ob das Reverse-DNS ordnungsgemäß konfiguriert ist

Der folgende Befehl sollte den vollqualifizierten Domänennamen des Hosts zurückgeben, der SQL Server ausführt, z. B. **SqlHost.contoso.com**.

```bash
host **<IP address of SQL Server host>**
```

Die Ausgabe dieses Befehls sollte in etwa wie folgt aussehen: `**<reversed IP address>**.in-addr.arpa domain name pointer SqlHost.contoso.com`. Wenn dieser Befehl nicht den vollqualifizierten Domänennamen des Hosts zurückgibt oder wenn dieser falsch ist, fügen Sie dem DNS-Server einen Reverse-DNS-Eintrag für Ihren Host für SQL Server für Linux hinzu.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel werden die Voraussetzungen für die Konfiguration eines Hostcomputers für SQL Server für Linux über die Active Directory-Authentifizierung behandelt. Führen Sie die unter [Verwenden der Active Directory-Authentifizierung mithilfe von SQL Server für Linux](sql-server-linux-active-directory-authentication.md) beschriebenen Schritte aus, um die Konfiguration der Unterstützung von Active Directory-Konten für SQL Server für Linux abzuschließen.
