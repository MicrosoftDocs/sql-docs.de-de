---
title: Verschlüsseln von Verbindungen mit SQL Server für Linux
description: SQL Server für Linux verwendet TLS zum Verschlüsseln der Daten, die über ein Netzwerk zwischen einer Clientanwendung und einer Instanz von SQL Server übertragen werden.
ms.date: 06/29/2020
author: vin-yu
ms.author: vinsonyu
ms.reviewer: vanto
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
helpviewer_keywords:
- Linux, encrypted connections
ms.openlocfilehash: 1040fec65e6ffce580a33b6ef581c4f22a3903ef
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186408"
---
# <a name="encrypting-connections-to-sql-server-on-linux"></a>Verschlüsseln von Verbindungen mit SQL Server für Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] für Linux kann Transport Layer Security (TLS) zum Verschlüsseln der Daten verwendet werden, die über ein Netzwerk zwischen einer Clientanwendung und einer Instanz von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] übertragen werden. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] unterstützt unter Windows und Linux die gleichen TLS-Protokolle: TLS 1.2, 1.1 und 1.0. Die Schritte zum Konfigurieren von TLS sind jedoch spezifisch für das Betriebssystem, auf dem [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ausgeführt wird.  

## <a name="requirements-for-certificates"></a>Anforderungen an Zertifikate 
Bevor Sie beginnen, müssen Sie sicherstellen, dass Ihre Zertifikate den folgenden Anforderungen entsprechen:
- Die aktuelle Systemzeit muss nach der Eigenschaft „Gültig ab“ und vor der Eigenschaft „Gültig bis“ des Zertifikats liegen.
- Das Zertifikat muss für die Serverauthentifizierung vorgesehen sein. Dazu muss für die Eigenschaft „Erweiterte Schlüsselverwendung“ des Zertifikats „Serverauthentifizierung (1.3.6.1.5.5.7.3.1)“ angegeben sein.
- Das Zertifikat muss mit der KeySpec-Option von AT_KEYEXCHANGE erstellt werden. Normalerweise enthält die Schlüsselverwendungseigenschaft (KEY_USAGE) des Zertifikats auch die Schlüsselverschlüsselung (CERT_KEY_ENCIPHERMENT_KEY_USAGE).
- Mit der Eigenschaft „Antragsteller“ des Zertifikats muss angegeben werden, dass der allgemeine Name (Common Name, CN) mit dem Hostnamen oder dem vollqualifizierten Domänennamen (Fully Qualified Domain Name, FQDN) des Servercomputers übereinstimmt. Hinweis: Platzhalterzertifikate werden unterstützt.

## <a name="configuring-the-openssl-libraries-for-use-optional"></a>Konfigurieren der OpenSSL-Bibliotheken zur Verwendung (optional)
Sie können symbolische Verknüpfungen im `/opt/mssql/lib/`-Verzeichnis erstellen, die darauf verweisen, welche `libcrypto.so`- und `libssl.so`-Bibliotheken für die Verschlüsselung verwendet werden sollen. Dies ist hilfreich, wenn Sie erzwingen möchten, dass SQL Server eine bestimmte andere OpenSSL-Version als die vom System bereitgestellte Standardversion verwendet. Wenn diese symbolischen Verknüpfungen nicht vorhanden sind, lädt SQL Server die standardmäßig auf dem System konfigurierten OpenSSL-Bibliotheken.

Diese symbolischen Verknüpfungen sollten mit `libcrypto.so` und `libssl.so` benannt und in das `/opt/mssql/lib/`-Verzeichnis eingefügt werden.

## <a name="overview"></a>Übersicht
TLS wird zum Verschlüsseln von Verbindungen einer Clientanwendung mit [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] verwendet. Bei ordnungsgemäßer Konfiguration bietet TLS sowohl Datenschutz als auch Datenintegrität für die Kommunikation zwischen dem Client und dem Server.  TLS-Verbindungen können entweder vom Client oder vom Server initiiert werden. 

## <a name="client-initiated-encryption"></a>Vom Client initiierte Verschlüsselung 
- **Generieren des Zertifikats** („/CN“ sollte mit dem vollqualifizierten Domänennamen des SQL Server-Hosts identisch sein)

> [!NOTE]
> In diesem Beispiel verwenden wir ein selbstsigniertes Zertifikat, das nicht für Produktionsszenarien verwendet werden sollte. Sie sollten Zertifizierungsstellenzertifikate verwenden.<br>
> Stellen Sie sicher, dass die Ordner, in denen Sie Ihre Zertifikate und privaten Schlüssel speichern, für die MSSQL-Benutzer/-Gruppe zugänglich sind und die Berechtigung auf „700 (drwx-----)“ festgelegt ist. Sie können Ordner manuell erstellen, die Berechtigung auf „700 (drwx-----)“ festlegen und die MSSQL-Benutzer/-Gruppe als Besitzer festlegen. Alternativ können Sie die Berechtigung auf „755 (drwxr-xr-x)“ und einen anderen Benutzer als Besitzer festlegen, sodass die Ordner dennoch für die MSSQL-Benutzergruppe zugänglich sind. Beispielsweise können Sie einen Ordner namens „sslcert“ unter dem Pfad „/var/opt/mssql/“ erstellen und dann das Zertifikat und den privaten Schlüssel mit den Berechtigungen für die Dateien festgelegt auf „600“ speichern (siehe unten). 

```bash
openssl req -x509 -nodes -newkey rsa:2048 -subj '/CN=mssql.contoso.com' -keyout mssql.key -out mssql.pem -days 365 
sudo chown mssql:mssql mssql.pem mssql.key 
sudo chmod 600 mssql.pem mssql.key 
# in this case we are saving the certificate to the certs folder under /etc/ssl/ which has the following permission 755(drwxr-xr-x)
sudo mv mssql.pem /etc/ssl/certs/ drwxr-xr-x 
# in this case we are saving the private key to the private folder under /etc/ssl/ with permissions set to 755(drwxr-xr-x)
sudo mv mssql.key /etc/ssl/private/ 
```

- **Konfigurieren von SQL Server**

```bash
systemctl stop mssql-server 
sudo cat /var/opt/mssql/mssql.conf 
sudo /opt/mssql/bin/mssql-conf set network.tlscert /etc/ssl/certs/mssql.pem 
sudo /opt/mssql/bin/mssql-conf set network.tlskey /etc/ssl/private/mssql.key 
sudo /opt/mssql/bin/mssql-conf set network.tlsprotocols 1.2 
sudo /opt/mssql/bin/mssql-conf set network.forceencryption 0 
systemctl start mssql-server 
```

- **Registrieren des Zertifikats auf dem Clientcomputer (Windows, Linux oder macOS)**

    -   Wenn Sie ein von der Zertifizierungsstelle signiertes Zertifikat verwenden, müssen Sie das Zertifizierungsstellenzertifikat (Certificate Authority, CA) anstelle des Benutzerzertifikats auf den Clientcomputer kopieren. 
    -   Wenn Sie das selbstsignierte Zertifikat verwenden, kopieren Sie einfach die PEM-Datei in die folgenden Ordner bzw. in die Distribution, und führen Sie die Befehle aus, um sie zu aktivieren. 
        - **Ubuntu**: Kopieren Sie das Zertifikat in `/usr/share/ca-certificates/`, ändern Sie die Erweiterung in „.crt“, und verwenden Sie `dpkg-reconfigure ca-certificates`, um dieses als Zertifizierungsstellenzertifikat für das System zu verwenden. 
        - **RHEL**: Kopieren Sie das Zertifikat in `/etc/pki/ca-trust/source/anchors/`, und verwenden Sie `update-ca-trust`, um dieses als Zertifizierungsstellenzertifikat für das System zu verwenden.
        - **SUSE**: Kopieren Sie das Zertifikat in `/usr/share/pki/trust/anchors/`, und verwenden Sie `update-ca-certificates`, um dieses als Zertifizierungsstellenzertifikat für das System zu verwenden.
        - **Windows**:  Importieren Sie die PEM-Datei als Zertifikat unter „Aktueller Benutzer -> Vertrauenswürdige Stammzertifizierungsstellen -> Zertifikate“.
        - **macOS**: 
           - Kopieren Sie das Zertifikat in `/usr/local/etc/openssl/certs`.
           - Führen Sie den folgenden Befehl aus, um den Hashwert zu erhalten: `/usr/local/Cellar/openssl/1.0.2l/openssl x509 -hash -in mssql.pem -noout`.
           - Benennen Sie das Zertifikat in den Wert um. Beispiel: `mv mssql.pem dc2dd900.0`. Stellen Sie sicher, dass dc2dd900.0 sich in `/usr/local/etc/openssl/certs` befindet.
    
-   **Exemplarische Verbindungszeichenfolgen** 

    - **[!INCLUDE[ssmanstudiofull-md](../includes/ssmanstudiofull-md.md)]**   
  ![SSMS-Verbindungsdialogfeld](media/sql-server-linux-encrypted-connections/ssms-encrypt-connection.png "SSMS-Verbindungsdialogfeld")  
  
    - **SQLCMD** 

        `sqlcmd  -S <sqlhostname> -N -U sa -P '<YourPassword>'`

    - **ADO.NET** 

        `"Encrypt=True; TrustServerCertificate=False;"`

    - **ODBC** 

        `"Encrypt=Yes; TrustServerCertificate=no;"`

    - **JDBC** 

        `"encrypt=true; trustServerCertificate=false;"`

## <a name="server-initiated-encryption"></a>Vom Server initiierte Verschlüsselung 

- **Generieren des Zertifikats** („/CN“ sollte mit dem vollqualifizierten Domänennamen des SQL Server-Hosts identisch sein)

```bash
openssl req -x509 -nodes -newkey rsa:2048 -subj '/CN=mssql.contoso.com' -keyout mssql.key -out mssql.pem -days 365 
sudo chown mssql:mssql mssql.pem mssql.key 
sudo chmod 600 mssql.pem mssql.key   
sudo mv mssql.pem /etc/ssl/certs/ 
sudo mv mssql.key /etc/ssl/private/ 
```

- **Konfigurieren von SQL Server**

```bash
systemctl stop mssql-server 
sudo cat /var/opt/mssql/mssql.conf 
sudo /opt/mssql/bin/mssql-conf set network.tlscert /etc/ssl/certs/mssql.pem 
sudo /opt/mssql/bin/mssql-conf set network.tlskey /etc/ssl/private/mssql.key 
sudo /opt/mssql/bin/mssql-conf set network.tlsprotocols 1.2 
sudo /opt/mssql/bin/mssql-conf set network.forceencryption 1
systemctl start mssql-server 
```

-   **Exemplarische Verbindungszeichenfolgen** 

    - **SQLCMD**

        `sqlcmd  -S <sqlhostname> -U sa -P '<YourPassword>'`

    - **ADO.NET** 

        `"Encrypt=False; TrustServerCertificate=False;"`

    - **ODBC** 

        `"Encrypt=no; TrustServerCertificate=no;"`

    - **JDBC** 

        `"encrypt=false; trustServerCertificate=false;"`

> [!NOTE]
> Legen Sie für **TrustServerCertificate** „True“ fest, wenn der Client keine Verbindung mit der Zertifizierungsstelle herstellen kann, um die Authentizität des Zertifikats zu überprüfen.

## <a name="common-connection-errors"></a>Häufige Verbindungsfehler  

|Fehlermeldung |Fix |
|--- |--- |
|Die Zertifikatkette wurde von einer nicht vertrauenswürdigen Zertifizierungsstelle ausgestellt.  |Dieser Fehler tritt auf, wenn Clients die Signatur des Zertifikats, das von SQL Server während des TLS-Handshakes angezeigt wird, nicht überprüfen können. Stellen Sie sicher, dass der Client entweder direkt dem [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Zertifikat vertraut, oder der Zertifizierungsstelle, die das SQL Server-Zertifikat signiert hat. |
|Der Zielprinzipalname ist falsch.  |Stellen Sie sicher, dass das Feld „Allgemeiner Name“ auf dem SQL Server-Zertifikat mit dem in der Verbindungszeichenfolge des Clients angegebenen Servernamen übereinstimmt. |  
|An existing connection was forcibly closed by the remote host. |Dieser Fehler kann auftreten, wenn der Client die für SQL Server erforderliche TLS-Protokollversion nicht unterstützt. Wenn [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] beispielsweise für die Verwendung von TLS 1.2 konfiguriert ist, müssen Sie sicherstellen, dass Ihre Clients auch das TLS 1.2-Protokoll unterstützen. |
| | |   
