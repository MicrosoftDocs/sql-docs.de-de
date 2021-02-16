---
title: 'Tutorial: Verwenden der Active Directory-Authentifizierung für SQL Server für Linux'
titleSuffix: SQL Server
description: In diesem Tutorial werden die Konfigurationsschritte für die Active Directory-Authentifizierung für SQL Server unter Linux beschrieben.
author: Dylan-MSFT
ms.author: dygray
ms.reviewer: vanto
ms.date: 12/18/2019
ms.topic: tutorial
ms.prod: sql
ms.custom: seodec18
ms.technology: linux
helpviewer_keywords:
- Linux, AAD authentication
ms.openlocfilehash: c6487f5c22a61cf1a4979af18d5cc3fdae5b0eb2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100273151"
---
# <a name="tutorial-use-active-directory-authentication-with-sql-server-on-linux"></a>Tutorial: Verwenden der Active Directory-Authentifizierung für SQL Server für Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

In diesem Tutorial wird erläutert, wie Sie [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] unter Linux konfigurieren, um die Active Directory-Authentifizierung (AD) zu unterstützen, die auch als integrierte Authentifizierung bezeichnet wird. Eine Übersicht dazu finden Sie unter [Active Directory-Authentifizierung für SQL Server für Linux](sql-server-linux-active-directory-auth-overview.md).

Dieses Tutorial umfasst die folgenden Aufgaben:

> [!div class="checklist"]
> * Verknüpfen des [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Hosts mit der AD-Domäne
> * Erstellen eines AD-Benutzers für [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] und Festlegen eines SPN
> * Konfigurieren der KEYTAB-Datei für den [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Dienst
> * Sichern der KEYTAB-Datei
> * Konfigurieren von SQL Server für die Verwendung der KEYTAB-Datei für die Kerberos-Authentifizierung
> * Erstellen von AD-basierten Anmeldeinformationen in Transact-SQL
> * Herstellen einer Verbindung mit [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] über die AD-Authentifizierung

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie die AD-Authentifizierung konfigurieren können, müssen Sie Folgendes tun:

* Richten Sie einen AD-Domänencontroller (Windows) auf Ihrem Netzwerk ein.  
* Installieren von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]
  * [Red Hat Enterprise Linux (RHEL)](quickstart-install-connect-red-hat.md)
  * [SUSE Linux Enterprise Server (SLES)](quickstart-install-connect-suse.md)
  * [Ubuntu](quickstart-install-connect-ubuntu.md)

## <a name="join-ssnoversion-host-to-ad-domain"></a><a id="join"></a> Verknüpfen des [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Hosts mit der AD-Domäne

Verknüpfen Sie Ihren Linux-Host für SQL Server mit einem Active Directory-Domänencontroller. Informationen dazu finden Sie unter [Verknüpfen von eines Linux-Hosts für SQL Server mit einer Active Directory-Domäne](sql-server-linux-active-directory-join-domain.md).

## <a name="create-ad-user-or-msa-for-ssnoversion-and-set-spn"></a><a id="createuser"></a> Erstellen eines AD-Benutzers (oder -MSA) für [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] und Festlegen eines SPN

> [!NOTE]
> Verwenden Sie für die folgenden Schritte Ihren [vollqualifizierten Domänennamen](https://en.wikipedia.org/wiki/Fully_qualified_domain_name). Wenn Sie in **Azure** arbeiten, müssen Sie **[einen erstellen](/azure/virtual-machines/linux/portal-create-fqdn)** ,bevor Sie fortfahren können.

1. Führen Sie auf Ihrem Domänencontroller den PowerShell-Befehl [New-ADUser](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee617253(v=technet.10)) aus, um einen neuen AD-Benutzer mit einem Kennwort zu erstellen, das niemals abläuft. Im folgenden Beispiel hat das Konto den Namen `mssql`. Sie können aber jeden beliebigen Namen auswählen. Sie werden aufgefordert, ein neues Kennwort für das Konto einzugeben.

   ```PowerShell
   Import-Module ActiveDirectory

   New-ADUser mssql -AccountPassword (Read-Host -AsSecureString "Enter Password") -PasswordNeverExpires $true -Enabled $true
   ```

   > [!NOTE]
   > Es hat sich als Sicherheitsmethode bewährt, ein dediziertes AD-Konto für SQL Server zu verwenden, damit die Anmeldeinformationen für SQL Server nicht für andere Dienste freigegeben werden, die dasselbe Konto verwenden. Sie können jedoch als Alternative auch ein vorhandenes AD-Konto wiederverwenden, wenn Sie das Kennwort für das Konto kennen. Sie benötigen dieses, um im nächsten Schritt eine KEYTAB-Datei zu erstellen. Außerdem sollte das Konto 128-Bit- und 256-Bit-Kerberos-AES-Verschlüsselung (Attribut **msDS-SupportedEncryptionTypes**) für das Benutzerkonto unterstützen können. Wenn Sie überprüfen möchten, ob das Konto für die AES-Verschlüsselung aktiviert ist, suchen Sie das Konto im Hilfsprogramm **Active Directory-Benutzer und -Computer**, und klicken Sie auf **Eigenschaften**. Suchen Sie unter **Eigenschaften** nach der Registerkarte **Konten**, und überprüfen Sie, ob die beiden folgenden Kontrollkästchen ausgewählt sind. 
   >
   > 1. **Dieses Konto unterstützt Kerberos-AES-128-Bit-Verschlüsselung**
   >
   > 2. **Dieses Konto unterstützt Kerberos-AES-256-Bit-Verschlüsselung**

2. Legen Sie den Dienstprinzipalnamen (Service Principle Name, SPN) für dieses Konto mithilfe des Tools **setspn.exe** fest. Der SPN muss genau wie im folgenden Beispiel angegeben formatiert werden. Sie können den vollqualifizierten Domänennamen des Hostcomputers für [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ermitteln, indem Sie `hostname --all-fqdns` auf de Host [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ausführen. In der Regel wird der TCP-Port 1433 verwendet, es sei denn, Sie haben [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] so konfiguriert, dass eine andere Portnummer verwendet wird.

   ```PowerShell
   setspn -A MSSQLSvc/<fully qualified domain name of host machine>:<tcp port> mssql
   setspn -A MSSQLSvc/<netbios name of the host machine>:<tcp port> mssql
   ```

   > [!NOTE]
   > Wenn Sie die Fehlermeldung `Insufficient access rights` erhalten, prüfen Sie zusammen mit Ihrem Domänenadministrator, ob Sie über die nötigen Berechtigungen verfügen, um einen SPN für dieses Konto festzulegen. Für das Konto, das zum Registrieren des SPN verwendet wird, benötigen Sie die Schreibberechtigungen für **servicePrincipalName**. Weitere Informationen finden Sie unter [Registrieren eines Dienstprinzipalnamens für Kerberos-Verbindungen](../database-engine/configure-windows/register-a-service-principal-name-for-kerberos-connections.md).
   >
   > Wenn Sie den TCP-Port später ändern, müssen Sie den Befehl **setspn** noch einmal mit der neuen Portnummer ausführen. Außerdem müssen Sie den neuen SPN zur KEYTAB-Datei des SQL Server-Diensts hinzufügen, indem Sie die im nächsten Abschnitt beschriebenen Schritte ausführen.

Weitere Informationen finden Sie unter [Registrieren eines Dienstprinzipalnamens für Kerberos-Verbindungen](../database-engine/configure-windows/register-a-service-principal-name-for-kerberos-connections.md).

## <a name="configure-ssnoversion-service-keytab"></a><a id="configurekeytab"></a> Konfigurieren der KEYTAB-Datei für den [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Dienst

Zum Konfigurieren der AD-Authentifizierung für SQL Server unter Linux benötigen Sie ein AD-Konto (MSA oder AD-Benutzerkonto) sowie den im vorherigen Abschnitt erstellten SPN.

> [!IMPORTANT]
> Wird das Kennwort für das AD-Konto oder für das Konto, dem die SPN zugewiesen sind, geändert, müssen Sie die KEYTAB-Datei mit dem neuen Kennwort und der Schlüsselversionsnummer (Key Version Number, KVNO) aktualisieren. Einige Dienste rotieren die Kennwörter möglicherweise auch automatisch. Überprüfen Sie sämtliche Richtlinien für die Kennwortrotation, die für die betreffenden Konten gelten, und passen Sie geplante Wartungsaktivitäten an diese an, um unerwartete Ausfallzeiten zu vermeiden.

### <a name="spn-keytab-entries"></a><a id="spn"></a> KEYTAB-Einträge für den SPN

1. Überprüfen Sie die KVNO des AD-Kontos, das Sie im vorherigen Schritt erstellt haben. Normalerweise lautet diese 2, aber es kann auch ein anderer Integerwert sein, wenn Sie das Kennwort des Kontos mehrmals geändert haben. Führen Sie auf dem Hostcomputer für SQL Server die folgenden Befehle aus:

    - In den folgenden Beispielen wird davon ausgegangen, dass sich `user` in der Domäne `@CONTOSO.COM` befindet. Ändern Sie den Benutzer- und Domänennamen in Ihren Benutzer- und Domänennamen.

   ```bash
   kinit user@CONTOSO.COM
   kvno user@CONTOSO.COM
   kvno MSSQLSvc/<fully qualified domain name of host machine>:<tcp port>@CONTOSO.COM
   ```

   > [!NOTE]
   > Es kann einige Minuten dauern, bis die SPNs für Ihre Domäne übernommen werden, insbesondere, wenn die Domäne groß ist. Wenn Sie die Fehlermeldung `kvno: Server not found in Kerberos database while getting credentials for MSSQLSvc/<fully qualified domain name of host machine>:<tcp port>@CONTOSO.COM` erhalten, warten Sie einige Minuten, und versuchen Sie es noch mal.</br></br> Die obigen Befehle funktionieren nur, wenn der Server, wie im vorherigen Abschnitt beschrieben, einer AD-Domäne hinzugefügt wurde.

1. Fügen Sie über [**ktpass**](/windows-server/administration/windows-commands/ktpass) für jeden SPN KEYTAB-Einträge hinzu, indem Sie in der Eingabeaufforderung eines Windows-Computers die folgenden Befehle eingeben:

    - `<DomainName>\<UserName>`: Ein MSA oder AD-Benutzerkonto.
    - `@CONTOSO.COM`: Verwenden Sie Ihren Domänennamen.
    - `/kvno <#>`: Ersetzen Sie `<#>` durch die KVNO, die Sie in einem vorherigen Schritt erhalten haben.
    - `<StrongPassword>`: Verwenden Sie ein sicheres Kennwort.

   ```bash
   ktpass /princ MSSQLSvc/<fully qualified domain name of host machine>:<tcp port>@CONTOSO.COM /ptype KRB5_NT_PRINCIPAL /crypto aes256-sha1 /mapuser <DomainName>\<UserName> /out mssql.keytab -setpass -setupn /kvno <#> /pass <StrongPassword>

   ktpass /princ MSSQLSvc/<fully qualified domain name of host machine>:<tcp port>@CONTOSO.COM /ptype KRB5_NT_PRINCIPAL /crypto rc4-hmac-nt /mapuser <DomainName>\<UserName> /in mssql.keytab /out mssql.keytab -setpass -setupn /kvno <#> /pass <StrongPassword>

   ktpass /princ MSSQLSvc/<netbios name of the host machine>:<tcp port>@CONTOSO.COM /ptype KRB5_NT_PRINCIPAL /crypto aes256-sha1 /mapuser <DomainName>\<UserName> /in mssql.keytab /out mssql.keytab -setpass -setupn /kvno <#> /pass <StrongPassword>

   ktpass /princ MSSQLSvc/<netbios name of the host machine>:<tcp port>@CONTOSO.COM /ptype KRB5_NT_PRINCIPAL /crypto rc4-hmac-nt /mapuser <DomainName>\<UserName> /in mssql.keytab /out mssql.keytab -setpass -setupn /kvno <#> /pass <StrongPassword>

   ktpass /princ <UserName>@CONTOSO.COM /ptype KRB5_NT_PRINCIPAL /crypto aes256-sha1 /mapuser <DomainName>\<UserName> /in mssql.keytab /out mssql.keytab -setpass -setupn /kvno <#> /pass <StrongPassword>

   ktpass /princ <UserName>@CONTOSO.COM /ptype KRB5_NT_PRINCIPAL /crypto rc4-hmac-nt /mapuser <DomainName>\<UserName> /in mssql.keytab /out mssql.keytab -setpass -setupn /kvno <#> /pass <StrongPassword>
   ```

   > [!NOTE]
   > Mit den obigen Befehlen werden für die AD-Authentifizierung sowohl AES- als auch RC4-Verschlüsselungsmethoden zugelassen. RC4 ist eine ältere Verschlüsselungsmethode. Ist ein höherer Sicherheitsgrad erforderlich, können Sie die KEYTAB-Einträge auch nur mithilfe der AES-Verschlüsselung erstellen.
   > Die letzten zwei `UserName`-Einträge müssen kleingeschrieben werden, da sonst die Authentifizierung der Berechtigungen fehlschlagen kann.

1. Nach dem Ausführen des obigen Befehls sollte eine KEYTAB-Datei namens „mssql.keytab“ vorhanden sein. Kopieren Sie die Datei auf den SQL Server-Computer in den Ordner `/var/opt/mssql/secrets`.

1. Schützen Sie die KEYTAB-Datei.

    Jeder Benutzer mit Zugriff auf diese KEYTAB-Datei kann die Identität von SQL Server in der Domäne annehmen. Stellen Sie daher sicher, dass Sie den Zugriff auf die Datei so einschränken, dass nur das MSSQL-Konto über Lesezugriff verfügt:

    ```bash
    sudo chown mssql:mssql /var/opt/mssql/secrets/mssql.keytab
    sudo chmod 400 /var/opt/mssql/secrets/mssql.keytab
    ```

1. Legen Sie mithilfe des Tools **mssql-conf** die folgende Konfigurationsoption fest, um das Konto anzugeben, das für den Zugriff auf die KEYTAB-Datei verwendet werden soll.

   ```bash
   sudo mssql-conf set network.privilegedadaccount <username>
   ```

   > [!NOTE]
   > Geben Sie nur den Benutzernamen an und nicht etwa „Domänenname\Benutzername“ oder username@domain. SQL Server fügt den Domänennamen nach Bedarf intern hinzu, wenn dieser Benutzername verwendet wird.

1. Gehen Sie wie im Folgenden beschrieben vor, um die SQL Server-Instanz so zu konfigurieren, dass die KEYTAB-Datei für die Kerberos-Authentifizierung verwendet wird.

    ```bash
    sudo mssql-conf set network.kerberoskeytabfile /var/opt/mssql/secrets/mssql.keytab
    sudo systemctl restart mssql-server
    ```

    > [!TIP]
    > Optional können Sie die UDP-Verbindungen mit dem Domänencontroller deaktivieren, um die Leistung zu verbessern. In vielen Fällen schlagen UDP-Verbindungen beim Herstellen einer Verbindung mit einem Domänencontroller immer wieder fehl. Sie können deshalb in den Konfigurationsoptionen in der Datei **/etc/krb5.conf** festlegen, dass UDP-Aufrufe übersprungen werden sollen. Bearbeiten Sie die Datei **/etc/krb5.conf**, und legen Sie die folgenden Optionen fest:
    > ```bash
    > /etc/krb5.conf
    > [libdefaults]
    > udp_preference_limit=0
    > ```

Nun können Sie AD-basierte Anmeldeinformationen in SQL Server verwenden.

## <a name="create-ad-based-logins-in-transact-sql"></a><a id="createsqllogins"></a> Erstellen von AD-basierten Anmeldeinformationen in Transact-SQL

1. Stellen Sie eine Verbindung mit SQL Server her, und erstellen Sie neue AD-basierte Anmeldeinformationen:

   ```sql
   CREATE LOGIN [CONTOSO\user] FROM WINDOWS;
   ```

1. Überprüfen Sie danach, ob diese Anmeldeinformationen in der Systemkatalogsicht [sys.server_principals](../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md) aufgeführt werden:

   ```sql
   SELECT name FROM sys.server_principals;
   ```

## <a name="connect-to-sql-server-using-ad-authentication"></a><a id="connect"></a> Herstellen einer Verbindung mit SQL Server über die AD-Authentifizierung

Melden Sie sich über Ihre Domänenanmeldeinformationen bei einem Clientcomputer an. Nun können Sie eine Verbindung mit SQL Server herstellen, ohne Ihr Kennwort über die AD-Authentifizierung erneut eingeben zu müssen. Wenn Sie Anmeldeinformationen für eine AD-Gruppe erstellen, kann jeder AD-Benutzer, der Mitglied dieser Gruppe ist, auf die gleiche Weise eine Verbindung herstellen.

Der genaue Verbindungszeichenfolgenparameter, den die Clients benötigen, um die AD-Authentifizierung verwenden zu können, ist davon abhängig, welchen Treiber Sie verwenden. Beispiele dazu finden Sie in den folgenden Abschnitten.

### <a name="sqlcmd-on-a-domain-joined-linux-client"></a>sqlcmd auf einem in die Domäne eingebundenen Linux-Client

Melden Sie sich mithilfe von **ssh** und Ihren Domänenanmeldeinformationen bei einem in die Domäne eingebundenen Linux-Client an:

```bash
ssh -l user@contoso.com client.contoso.com
```

Stellen Sie sicher, dass das Paket [mssql-tools](sql-server-linux-setup-tools.md) installiert wurde, und stellen Sie dann mithilfe von **sqlcmd** eine Verbindung her, ohne die Anmeldeinformationen anzugeben:

```bash
sqlcmd -S mssql-host.contoso.com
```
Im Gegensatz zu SQL Windows können Sie die Kerberos-Authentifizierung für lokale Verbindungen in SQL Linux verwenden. Dafür ist jedoch die Bereitstellung des FQDN des Linux-Hosts für SQL Server erforderlich. Die AD-Authentifizierung funktioniert nicht beim Herstellen einer Verbindung mit „.“, „localhost“, „127.0.0.1“ usw.

### <a name="ssms-on-a-domain-joined-windows-client"></a>SSMS auf einem in die Domäne eingebundenen Windows-Client

Melden Sie sich mithilfe Ihrer Domänenanmeldeinformationen bei einem in die Domäne eingebundenen Windows-Client an. Vergewissern Sie sich, dass SQL Server Management Studio installiert ist, und stellen Sie dann eine Verbindung mit Ihrer SQL Server-Instanz her (z. B. `mssql-host.contoso.com`), indem Sie im Dialogfeld **Mit Server verbinden****Windows-Authentifizierung** angeben.

### <a name="ad-authentication-using-other-client-drivers"></a>AD-Authentifizierung mit anderen Clienttreibern

In der folgenden Tabelle werden die Empfehlungen für andere Clienttreiber beschrieben:

| Clienttreiber | Empfehlung |
|---|---|
| **JDBC** | Verwenden Sie die integrierte Kerberos-Authentifizierung, um eine Verbindung mit der SQL Server-Instanz herzustellen. |
| **ODBC** | Verwenden Sie die integrierte Authentifizierung. |
| **ADO.NET** | Verwenden Sie die Syntax der Verbindungszeichenfolge. |

## <a name="additional-configuration-options"></a><a id="additionalconfig"></a> Zusätzliche Konfigurationsoptionen

Wenn Sie Hilfsprogramme von Drittanbietern wie [PBIS](https://www.beyondtrust.com/), [VAS](https://www.oneidentity.com/products/authentication-services/) oder [Centrify](https://www.centrify.com/) verwenden, um den Linux-Host mit der AD-Domäne zu verknüpfen, und Sie erzwingen möchten, dass die SQL Server-Instanz die OpenLDAP-Bibliothek direkt verwendet, können Sie die Option **disablesssd** wie folgt mithilfe des Tools **mssql-conf** konfigurieren:

```bash
sudo mssql-conf set network.disablesssd true
systemctl restart mssql-server
```

> [!NOTE]
> Einige Hilfsprogramme wie **realmd** richten die SSSD ein, wohingegen andere Tools wie PBIS, VAS und Centrify dies nicht tun. Wenn das Hilfsprogramm, das verwendet wird, um die AD-Domäne zu verknüpfen, nicht SSSD einrichtet, empfiehlt es sich, die Option **disablesssd** auf `true` festzulegen. Dies ist zwar nicht zwingend notwendig, weil die SQL Server-Instanz versucht, SSSD zu verwenden, bevor sie auf den OpenLDAP-Mechanismus zurückgreift, aber Sie würden dadurch die Leistung verbessern, weil die Instanz dann direkt OpenLDAP-Aufrufe durchführt und den SSSD-Mechanismus umgeht.

Wenn Ihr Domänencontroller LDAPS unterstützt, können Sie erzwingen, dass alle Verbindungen der SQL Server-Instanz mit den Domänencontrollern über LDAPS erfolgen. Führen Sie den Bash-Befehl `ldapsearch -H ldaps://contoso.com:3269` aus, um zu überprüfen, ob der Client den Domänencontroller über LDAPS kontaktieren kann. Wenn Sie festlegen möchten, dass SQL Server nur LDAPS verwendet, führen Sie Folgendes aus:

```bash
sudo mssql-conf set network.forcesecureldap true
systemctl restart mssql-server
```

Dann wird LDAPS vor SSSD verwendet, wenn der Domänenbeitritt auf dem Host über das SSSD-Paket erfolgt ist und die Option **disablesssd** nicht auf TRUE festgelegt wurde. Wenn sowohl **disablesssd** als auch **forcesecureldap** auf TRUE festgelegt sind, wird das LDAPS-Protokoll gegenüber Aufrufen der OpenLDAP-Bibliothek bevorzugt, die von der SQL Server-Instanz ausgeführt werden.

### <a name="post-sql-server-2017-cu14"></a>Vorgehensweise ab Version SQL Server 2017 CU14

Ab SQL Server 2017 CU14, können Sie auch die Option **enablekdcfromkrb5** verwenden, um zu erzwingen, dass die SQL Server-Instanz anstelle eines DNS-Reverse-Lookups für den KDC-Server die krb5-Bibliothek für die KDC-Suche verwendet. Voraussetzung dafür ist, dass die SQL Server-Instanz mithilfe eines Drittanbieters mit der Domäne verknüpft und die Option **disablesssd** auf TRUE festgelegt wurde, sodass OpenLDAP-Aufrufe für die allgemeine AD-Suche verwendet werden.

Dies kann in Szenarios nützlich sein, in denen Sie die Domänencontroller, mit denen die SQL Server-Instanz kommuniziert, manuell konfigurieren möchten. Sie können den OpenLDAP-Bibliotheksmechanismus mithilfe der KDC-Liste in der Datei **krb5.conf** verwenden.

Legen Sie zunächst die Optionen **disablesssd** und **enablekdcfromkrb5conf** auf TRUE fest, und starten Sie dann die SQL Server-Instanz neu:

```bash
sudo mssql-conf set network.disablesssd true
sudo mssql-conf set network.enablekdcfromkrb5conf true
systemctl restart mssql-server
```

Konfigurieren Sie anschließend wie folgt die KDC-Liste in der Datei **/etc/krb5.conf**:

```/etc/krb5.conf
[realms]
CONTOSO.COM = {
  kdc = dcWithGC1.contoso.com
  kdc = dcWithGC2.contoso.com
}
```

> [!NOTE]
> Es wird zwar nicht empfohlen, aber es ist trotzdem möglich, Hilfsprogramme wie **realmd** zu verwenden, die SSSD einrichten, während der Linux-Host der Domäne beitritt, wenn Sie die Option **disablesssd** auf TRUE festlegen, sodass die SQL Server-Instanz OpenLDAP-Aufrufe anstelle von SSSD für Aufrufe im Zusammenhang mit AD verwendet.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben wir erläutert, wie Sie die Active Directory-Authentifizierung mit SQL Server für Linux einrichten können. Sie haben Folgendes gelernt:
> [!div class="checklist"]
> * Verknüpfen des [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Hosts mit der AD-Domäne
> * Erstellen eines AD-Benutzers für [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] und Festlegen eines SPN
> * Konfigurieren der KEYTAB-Datei für den [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Dienst
> * Erstellen von AD-basierten Anmeldeinformationen in Transact-SQL
> * Herstellen einer Verbindung mit [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] über die AD-Authentifizierung

Lernen Sie als Nächstes weitere Sicherheitsszenarios für SQL Server für Linux kennen.

> [!div class="nextstepaction"]
> [Verschlüsseln von Verbindungen mit SQL Server für Linux](sql-server-linux-encrypted-connections.md)