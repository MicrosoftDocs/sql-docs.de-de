---
title: Bereitstellen des Host-Überwachungsdiensts
description: Bereitstellen des Host-Überwachungsdiensts für Always Encrypted mit Secure Enclaves
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
author: rpsqrd
ms.author: ryanpu
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f6890f9c6a2870379fb2a54589b4657fa12467fb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100015900"
---
# <a name="deploy-the-host-guardian-service-for-ssnoversion-md"></a>Bereitstellen des Host-Überwachungsdiensts für [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]

[!INCLUDE [sqlserver2019-windows-only](../../../includes/applies-to-version/sqlserver2019-windows-only.md)]

In diesem Artikel wird beschrieben, wie Sie den Host-Überwachungsdienst (Host Guardian Service, HGS) als Nachweisdienst für [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] bereitstellen.
Bevor Sie beginnen, lesen Sie unbedingt den Artikel [Planen des Nachweises des Host-Überwachungsdiensts](./always-encrypted-enclaves-host-guardian-service-plan.md), der eine vollständige Liste der Voraussetzungen und einen Architekturleitfaden enthält.

> [!NOTE]
> Der HGS-Administrator ist verantwortlich für die Ausführung aller Schritte, die in diesem Artikel beschrieben werden. Weitere Informationen finden Sie unter [Rollen und Verantwortlichkeiten beim Konfigurieren des Nachweises mit HGS](always-encrypted-enclaves-host-guardian-service-plan.md#roles-and-responsibilities-when-configuring-attestation-with-hgs).

## <a name="step-1-set-up-the-first-hgs-computer"></a>Schritt 1: Einrichten des ersten HGS-Computers

Der Host-Überwachungsdienst (Host Guardian Service, HGS) wird als Clusterdienst auf einem oder mehreren Computern ausgeführt.
In diesem Schritt richten Sie einen neuen HGS-Cluster auf dem ersten Computer ein.
Wenn Sie bereits über einen HGS-Cluster verfügen und ihm weitere Computer hinzufügen, um Hochverfügbarkeit zu erhalten, fahren Sie mit [Schritt 2: Hinzufügen weiterer HGS-Computer zum Cluster](#step-2-add-more-hgs-computers-to-the-cluster) fort.

Bevor Sie beginnen, vergewissern Sie sich, dass auf dem verwendeten Computer Windows Server 2019 Standard oder Datacenter Edition ausgeführt wird, Sie über lokale Administratorrechte verfügen und der Computer noch nicht Mitglied einer Active Directory Domäne ist.

1. Melden Sie sich beim ersten HGS-Computer als lokaler Administrator an, und öffnen Sie eine Windows PowerShell-Konsole mit erhöhten Rechten. Führen Sie anschließend den folgenden Befehl aus, um die Host-Überwachungsdienstrolle zu installieren. Der Computer wird automatisch neu gestartet, um die Änderungen zu übernehmen.

    ```powershell
    Install-WindowsFeature -Name HostGuardianServiceRole -IncludeManagementTools -Restart
    ```

2. Führen Sie nach dem Neustart des HGS-Computers die folgenden Befehle in einer Windows PowerShell-Konsole mit erhöhten Rechten aus, um die neue Active Directory-Gesamtstruktur zu installieren:

    ```powershell
    # Select the name for your new Active Directory root domain.
    # Make sure the name does not conflict with, and is not subordinate to, any existing domains on your network.
    $HGSDomainName = 'bastion.local'

    # Specify a Directory Services Restore Mode password that can be used to recover your domain in safe mode.
    # This password is not, and will not change, your admin account password.
    # Save this password somewhere safe and include it in your disaster recovery plan.
    $DSRMPassword = Read-Host -AsSecureString -Prompt "Directory Services Restore Mode Password"

    Install-HgsServer -HgsDomainName $HgsDomainName -SafeModeAdministratorPassword $DSRMPassword -Restart
    ```

    Der HGS-Computer wird erneut neu gestartet, um die Konfiguration der Active Directory-Gesamtstruktur abzuschließen. Wenn Sie sich das nächste Mal anmelden, wird Ihr Administratorkonto als Domänenadministratorkonto verwendet. Weitere Informationen zum Verwalten und Schützen Ihrer neuen Gesamtstruktur finden Sie in der [Active Directory Domain Services Operations-Dokumentation](/windows-server/identity/ad-ds/manage/component-updates/ad-ds-operations).

3. Anschließend richten Sie den HGS-Cluster ein und installieren den Nachweisdienst, indem Sie den folgenden Befehl in einer Windows PowerShell-Konsole mit erhöhten Rechten ausführen:

    ```powershell
    # Note: the name you provide here will be shared by all HGS nodes and used to point your SQL Server computers to the HGS cluster.
    # For example, if you provide "attsvc" here, a DNS record for "attsvc.yourdomain.com" will be created for every HGS computer.
    Initialize-HgsAttestation -HgsServiceName 'hgs'
    ```

## <a name="step-2-add-more-hgs-computers-to-the-cluster"></a>Schritt 2: Hinzufügen weiterer HGS-Computer zum Cluster

Nach dem Einrichten des ersten HGS-Computers und -Clusters können Sie zusätzliche HGS-Server hinzufügen, um Hochverfügbarkeit zu gewährleisten.
Wenn Sie nur einen HGS-Server einrichten (z. B. in einer Dev/Test-Umgebung), können Sie mit Schritt 3 fortfahren.

Vergewissern Sie sich wie beim ersten HGS-Computer, dass auf dem Computer, den Sie dem Cluster hinzufügen möchten, Windows Server 2019 Standard oder Datacenter Edition ausgeführt wird, Sie über lokale Administratorrechte verfügen und der Computer noch nicht Mitglied einer Active Directory Domäne ist.

1. Melden Sie sich beim Computer als lokaler Administrator an, und öffnen Sie eine Windows PowerShell-Konsole mit erhöhten Rechten. Führen Sie anschließend den folgenden Befehl aus, um die Host-Überwachungsdienstrolle zu installieren. Der Computer wird automatisch neu gestartet, um die Änderungen zu übernehmen.

    ```powershell
    Install-WindowsFeature -Name HostGuardianServiceRole -IncludeManagementTools -Restart
    ```

2. Überprüfen Sie die DNS-Clientkonfiguration auf Ihrem Computer, um sicherzustellen, dass er die HGS-Domäne auflösen kann. Der folgende Befehl sollte eine IP-Adresse für Ihren HGS-Server zurückgeben. Wenn Sie die HGS-Domäne nicht auflösen können, müssen Sie möglicherweise die DNS-Serverinformationen auf dem Netzwerkadapter aktualisieren, damit der HGS-DNS-Server für die Namensauflösung verwendet wird.

    ```powershell
    # Change 'bastion.local' to the domain name you specified in Step 1.2
    nslookup bastion.local

    # If it fails, use sconfig.exe, option 8, to set the first HGS computer as your preferred DNS server.
    ```

3. Nachdem der Computer neu gestartet wurde, führen Sie den folgenden Befehl in einer Windows PowerShell-Konsole mit erhöhten Rechten aus, um den Computer der Active Directory Domäne hinzuzufügen, die vom ersten HGS-Server erstellt wurde. Sie benötigen Anmeldeinformationen als Domänenadministrator für die AD-Domäne, um diesen Befehl auszuführen. Wenn der Befehl abgeschlossen ist, wird der Computer neu gestartet, und der Computer wird zu einem Active Directory-Domänencontroller für die HGS-Domäne.

    ```powershell
    # Provide the fully qualified HGS domain name
    $HGSDomainName = 'bastion.local'

    # Provide a domain administrator's credential for the HGS domain
    $DomainAdminCred = Get-Credential

    # Specify a Directory Services Restore Mode password that can be used to recover your domain in safe mode.
    # This password is not, and will not change, your admin account password.
    # Save this password somewhere safe and include it in your disaster recovery plan.
    $DSRMPassword = Read-Host -AsSecureString -Prompt "Directory Services Restore Mode Password"

    Install-HgsServer -HgsDomainName $HgsDomainName -HgsDomainCredential $DomainAdminCred -SafeModeAdministratorPassword $DSRMPassword -Restart
    ```

4. Melden Sie sich nach dem Neustart des Computers mit den Anmeldeinformationen des Domänenadministrators an. Öffnen Sie eine Windows PowerShell-Konsole mit erhöhten Rechten, und führen Sie die folgenden Befehle aus, um den Nachweisdienst zu konfigurieren. Da der Nachweisdienst clusterfähig ist, wird seine Konfiguration von anderen Clustermitgliedern repliziert. Änderungen, die an den Nachweisrichtlinien auf einem beliebigen HGS-Knoten vorgenommen werden, gelten auch für alle anderen Knoten.

    ```powershell
    # Provide the IP address of an existing, initialized HGS server
    # If you are using separate networks for cluster and application traffic, choose an IP address on the cluster network.
    # You can find the IP address of your HGS server by signing in and running "ipconfig /all"
    Initialize-HgsAttestation -HgsServerIPAddress '172.16.10.20'
    ```

5. Wiederholen Sie Schritt 2 für jeden Computer, den Sie Ihrem HGS-Cluster hinzufügen möchten.

## <a name="step-3-configure-a-dns-forwarder-to-your-hgs-cluster"></a>Schritt 3: Konfigurieren einer DNS-Weiterleitung für Ihren HGS-Cluster

HGS führt einen eigenen DNS-Server aus, der die zum Auflösen des Nachweisdiensts erforderlichen Namenseinträge enthält.
Die SQL Server-Computer können diese Datensätze erst auflösen, wenn Sie den DNS-Server Ihres Netzwerks für die Weiterleitung von Anforderungen an die HGS-DNS-Server konfiguriert haben.

Der Prozess für die Konfiguration von DNS-Weiterleitungen ist herstellerspezifisch. Daher sollten Sie sich an den Netzwerkadministrator wenden, um die richtigen Anweisungen für ihr bestimmtes Netzwerk zu erhalten.

Wenn Sie die Windows Server-DNS-Serverrolle für Ihr Unternehmensnetzwerk verwenden, kann Ihr DNS-Administrator eine bedingte Weiterleitung zur HGS-Domäne erstellen, sodass nur Anforderungen für die HGS-Domäne weitergeleitet werden.
Wenn Ihre HGS-Server z. B. den Domänennamen „bastion.local“ verwenden und die IP-Adressen 172.16.10.20, 172.16.10.21 und 172.16.10.22 haben, können Sie den folgenden Befehl auf dem DNS-Unternehmensserver ausführen, um eine bedingte Weiterleitung zu konfigurieren:

```powershell
# Tip: make sure to provide every HGS server's IP address
# If you use separate NICs for cluster and application traffic, use the application traffic NIC IP addresses here
Add-DnsServerConditionalForwarderZone -Name 'bastion.local' -ReplicationScope "Forest" -MasterServers "172.16.10.20", "172.16.10.21", "172.16.10.22"
```

## <a name="step-4-configure-the-attestation-service"></a>Schritt 4: Konfigurieren des Nachweisdiensts

HGS unterstützt zwei Nachweismodi: TPM-Nachweis zur kryptografischen Überprüfung von Integrität und Identität der einzelnen [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Computer und Hostschlüssel für die einfache Überprüfung der Identität des [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Computers.
Wenn Sie noch keinen Nachweismodus ausgewählt haben, finden Sie unter den Nachweisinformationen im [Planungshandbuch](./always-encrypted-enclaves-host-guardian-service-plan.md#attestation-modes) weitere Informationen zu den Sicherheitsgarantien und Anwendungsfällen für jeden Modus.

Mit den Schritten in diesem Abschnitt werden die grundlegenden Nachweisrichtlinien für einen bestimmten Nachweismodus konfiguriert.
Hostspezifische Informationen werden in Schritt 4 registriert.
Wenn Sie den Nachweismodus in der Zukunft ändern müssen, wiederholen Sie Schritt 3 und 4 mit dem gewünschten Nachweismodus.

### <a name="switch-to-tpm-attestation"></a>Wechsel zum TPM-Nachweis

Um den TPM-Nachweis auf HGS zu konfigurieren, benötigen Sie einen Computer mit Internetzugriff und mindestens einen [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] Computer mit einem TPM 2.0-Chip Rev. 1.16.

Um HGS zum Verwenden des TPM-Modus zu konfigurieren, öffnen Sie eine PowerShell-Konsole mit erhöhten Rechten, und führen Sie den folgenden Befehl aus:

```powershell
Set-HgsServer -TrustTpm
```

Alle HGS-Computer im Cluster verwenden jetzt den TPM-Modus, wenn ein [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Computer versucht den Nachweis zu erbringen.

Bevor Sie TPM-Informationen von Ihren SQL Server-Computern bei HGS registrieren können, müssen Sie die Stammzertifikate des Endorsement Keys (EK) vom Hersteller/von den Herstellern Ihres TPMs installieren.
Jedes physische TPM wird werksseitig mit einem eindeutigen Endorsement Key konfiguriert, der von einem Endorsement Key-Zertifikat begleitet wird, das den Hersteller identifiziert.
Mit diesem Zertifikat wird sichergestellt, dass das TPM echt ist.
Wenn Sie ein neues TPM bei HGS registrieren, überprüft HGS das Endorsement Key-Zertifikat durch Vergleich der Zertifikatkette mit einer Liste vertrauenswürdiger Stammzertifikate.

Microsoft veröffentlicht eine Liste als funktionierend bekannter Stammzertifikate von TPM-Herstellern, die Sie in den HGS-Speicher für vertrauenswürdige TPM-Stammzertifikate importieren können.
Wenn Sie virtualisierte SQL Server-Computer verwenden, müssen Sie sich an Ihren Clouddienstanbieter oder den Anbieter der Virtualisierungsplattform wenden, um Informationen zum Abrufen der Zertifikatkette für den Endorsement Key Ihrer virtuellen TPMs zu erhalten.

Führen Sie die folgenden Schritte aus, um das Paket mit vertrauenswürdigen TPM-Stammzertifikaten für physische TPMs von Microsoft herunterzuladen:

1. Laden Sie auf einem Computer mit Internetzugriff das aktuelle TPM-Stammzertifikatpaket von [https://go.microsoft.com/fwlink/?linkid=2097925](https://go.microsoft.com/fwlink/?linkid=2097925) herunter.

2. Überprüfen Sie die Signatur der CAB-Datei, um sicherzustellen, dass sie authentisch ist.

    ```powershell
    # Note: replace the path below with the correct one to the file you downloaded in step 1
    Get-AuthenticodeSignature ".\TrustedTpm.cab"
    ```

    > [!WARNING]
    > Fahren Sie nicht fort, wenn die Signatur ungültig ist, und bitten Sie den Microsoft Support um Hilfe.

3. Erweitern Sie die CAB-Datei in ein neues Verzeichnis.

    ```powershell
    mkdir .\TrustedTpmCertificates
    expand.exe -F:* ".\TrustedTpm.cab" ".\TrustedTpmCertificates"
    ```

4. Im neuen Verzeichnis finden Sie Verzeichnisse für jeden TPM-Anbieter. Sie können die Verzeichnisse für Anbieter löschen, die Sie nicht verwenden.

5. Kopieren Sie das gesamte Verzeichnis „TrustedTpmCertificates“ auf Ihren HGS-Server.

6. Öffnen Sie auf dem HGS-Server eine PowerShell-Konsole mit erhöhten Rechten, und führen Sie den folgenden Befehl aus, um alle TPM-Stamm- und Zwischenzertifikate zu importieren:

    ```powershell
    # Note: replace the path below with the correct location of the TrustedTpmCertificates folder on your HGS computer
    cd "C:\scratch\TrustedTpmCertificates"
    .\setup.cmd
    ```

7. Wiederholen Sie die Schritte 5 und 6 für jeden HGS-Computer.

Wenn Sie von Ihrem OEM, Ihrem Clouddienstanbieter oder dem Hersteller Ihrer Virtualisierungsplattform Zwischen- und Stammzertifikate der Zertifizierungsstelle erhalten haben, können Sie die Zertifikate direkt in den jeweiligen Zertifikatspeicher des lokalen Computers importieren: `TrustedHgs_RootCA` oder `TrustedHgs_IntermediateCA`. Beispiel in PowerShell:

```powershell
# Imports MyCustomTpmVendor_Root.cer to the local machine's "TrustedHgs_RootCA" store
Import-Certificate -FilePath ".\MyCustomTpmVendor_Root.cer" -CertStoreLocation "Cert:\LocalMachine\TrustedHgs_RootCA"
```

### <a name="switch-to-host-key-attestation"></a>Wechsel zum Hostschlüsselnachweis

Um den Hostschlüsselnachweis zu verwenden, führen Sie den folgenden Befehl auf einem HGS-Server in einer PowerShell-Konsole mit erhöhten Rechten aus:

```powershell
Set-HgsServer -TrustHostKey
```

Alle HGS-Computer in Ihrem Cluster verwenden jetzt den Hostschlüsselmodus, wenn ein [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Computer versucht, den Nachweis zu erbringen.

## <a name="step-5-configure-the-hgs-https-binding"></a>Schritt 5: Konfigurieren der HGS-HTTPS-Bindung

In einer Standardinstallation macht HGS nur eine HTTP-Bindung (Port 80) verfügbar.
Sie können eine HTTPS-Bindung (Port 443) konfigurieren, um die gesamte Kommunikation zwischen [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Computern und HGS zu verschlüsseln.
Es empfiehlt sich, für alle Produktionsinstanzen von HGS HTTPS-Bindung zu verwenden.

1. Rufen Sie ein TLS-Zertifikat bei Ihrer Zertifizierungsstelle ab, indem Sie den vollqualifizierten HGS-Dienstnamen aus Schritt 1.3 als Antragstellernamen verwenden. Wenn Sie Ihren Dienstnamen nicht kennen, können Sie ihn ermitteln, indem Sie auf einem beliebigen HGS-Computer `Get-HgsServer` ausführen. Sie können der Liste mit alternativen Antragstellernamen alternative DNS-Namen hinzufügen, wenn Ihre [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Computer für Verbindungen mit dem HGS-Cluster einen anderen DNS-Namen verwenden (beispielsweise wenn sich HGS hinter einem Netzwerklastenausgleich mit einer anderen Adresse befinden).

2. Verwenden Sie auf dem HGS-Computer [Set-HgsServer](/powershell/module/hgsserver/set-hgsserver), um die HTTPS-Bindung zu aktivieren, und geben Sie das im vorherigen Schritt abgerufene TLS-Zertifikat an. Wenn Ihr Zertifikat bereits auf dem Computer im lokalen Zertifikatspeicher installiert ist, verwenden Sie den folgenden Befehl, um es bei HGS zu registrieren:

    ```powershell
    # Note: you'll need to know the thumbprint for your certificate to configure HGS this way
    Set-HgsServer -Http -Https -HttpsCertificateThumbprint "54A043386555EB5118DB367CFE38776F82F4A181"
    ```

    Wenn Sie Ihr Zertifikat (mit einem privaten Schlüssel) in eine kennwortgeschützte PFX-Datei exportiert haben, können Sie es bei HGS registrieren, indem Sie den folgenden Befehl ausführen:

    ```powershell
    $PFXPassword = Read-Host -AsSecureString -Prompt "PFX Password"
    Set-HgsServer -Http -Https -HttpsCertificatePath "C:\path\to\hgs_tls.pfx" -HttpsCertificatePassword $PFXPassword
    ```

3. Wiederholen Sie die Schritte 1 und 2 für jeden HGS-Computer im Cluster. TLS-Zertifikate werden nicht automatisch zwischen HGS-Knoten repliziert. Außerdem kann jeder HGS-Computer über ein eigenes, eindeutiges TLS-Zertifikat verfügen, solange der Antragsteller mit dem HGS-Dienstnamen übereinstimmt.

## <a name="step-6-determine-and-share-the-hgs-attestation-url"></a>Schritt 6: Ermitteln und Freigeben der HGS-Nachweis-URL

Als HGS-Administrator müssen Sie die Nachweis-URL von HGS sowohl für SQL Server-Computeradministratoren als auch für Anwendungsadministratoren in Ihrer Organisation freigeben. Die SQL Server-Computeradministratoren benötigen die Nachweis-URL, um zu überprüfen, ob SQL Server-Computer mit HGS Nachweise durchführen können. Anwendungsadministratoren benötigen die Nachweis-URL, um zu konfigurieren, wie ihre Apps eine Verbindung mit SQL Server herstellen.

Um die Nachweis-URL zu bestimmen, führen Sie das folgende Cmdlet aus.

```powershell
Get-HGSServer
```
Die Ausgabe des Befehls sieht etwa wie folgt aus:

```
Name                           Value                                                                         
----                           -----                                                                         
AttestationOperationMode       HostKey                                                                       
AttestationUrl                 {http://hgs.bastion.local/Attestation}                                        
KeyProtectionUrl               {}         
```

Die Nachweis-URL für Ihren HGS-Computer ist der Wert der AttestationUrl-Eigenschaft.

## <a name="next-steps"></a>Nächste Schritte

- [Registrieren von Computern bei HGS](./always-encrypted-enclaves-host-guardian-service-register.md)