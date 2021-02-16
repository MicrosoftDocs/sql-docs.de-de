---
title: 'Tutorial: Erste Schritte mit Always Encrypted mit Secure Enclaves in SQL Server'
description: In diesem Tutorial erfahren Sie, wie Sie eine grundlegende Umgebung für Always Encrypted mit Secure Enclaves in SQL Server erstellen, indem Sie Enclaves für die virtualisierungsbasierte Sicherheit (VBS, Virtualization Based Security) und den Host-Überwachungsdienst (HGS) für den Nachweis verwenden. Sie lernen zudem die direkte Verschlüsseln von Daten und das Ausgeben umfangreicher vertraulicher Abfragen für verschlüsselte Spalten mithilfe von SQL Server Management Studio (SSMS).
ms.custom: seo-lt-2019
ms.date: 01/15/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.suite: sql
ms.technology: security
ms.tgt_pltfrm: ''
ms.topic: tutorial
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: a33db9a451e8ccf01ecd2f50b832afe4565ebc8c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077831"
---
# <a name="tutorial-getting-started-with-always-encrypted-with-secure-enclaves-in-sql-server"></a>Tutorial: Erste Schritte mit Always Encrypted mit Secure Enclaves in SQL Server
[!INCLUDE [sqlserver2019-windows-only](../../includes/applies-to-version/sqlserver2019-windows-only.md)]

In diesem Tutorial werden Ihnen die erste Schritte mit [Always Encrypted mit Secure Enclaves](encryption/always-encrypted-enclaves.md) in [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)] vermittelt. Es wird Folgendes gezeigt:

> [!div class="checklist"]
> - Erstellen einer einfachen Umgebung zum Testen und Auswerten von Always Encrypted mit Secure Enclaves
> - Direktes Verschlüsseln von Daten und Ausgeben umfangreicher vertraulicher Abfragen für verschlüsselte Spalten mithilfe von SQL Server Management Studio (SSMS)

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte mit Always Encrypted mit Secure Enclaves benötigen Sie mindestens zwei Computer (dies können virtuelle Computer sein):

- Der [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)]-Computer, auf dem [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)] und SSMS gehostet werden sollen
- HGS-Computer zum Ausführen des Host-Überwachungsdiensts, der für den Enclave-Nachweis erforderlich ist

### <a name="sql-server-computer-requirements"></a>SQL Server-Computeranforderungen

- [!INCLUDE [sssql19-md](../../includes/sssql19-md.md)] oder höher.
- Sie benötigen Windows 10 Enterprise, Version 1809 oder höher, oder Windows Server 2019 Datacenter Edition. Andere Editionen von Windows 10 und Windows Server unterstützen keinen HGS-Nachweis.
- CPU-Unterstützung für Virtualisierungstechnologien:
  - Intel VT-x mit erweiterten Seitentabellen
  - AMD-V mit schneller Virtualisierungsindizierung
  - Wenn Sie [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)] in einer VM (virtueller Computer) ausführen, müssen der Hypervisor und die physische CPU Funktionen für die geschachtelte Virtualisierung bereitstellen. 
    - Für Hyper-V 2016 oder höher [aktivieren Sie die Erweiterungen für geschachtelte Virtualisierung für den VM-Prozessor](/virtualization/hyper-v-on-windows/user-guide/nested-virtualization#configure-nested-virtualization).
    - Wählen Sie in Azure eine VM-Größe aus, die die geschachtelte Virtualisierung unterstützt. Hierzu gehören alle VMs der v3-Serie, z. B. Dv3 und Ev3. Siehe [Erstellen einer schachtelungsfähigen Azure-VM](/azure/virtual-machines/windows/nested-virtualization#create-a-nesting-capable-azure-vm).
    - Aktivieren Sie bei VMware vSphere 6.7 oder höher die Unterstützung für virtualisierungsbasierte Sicherheit für den virtuellen Computer, wie in der [VMware-Dokumentation](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.vm_admin.doc/GUID-C2E78F3E-9DE2-44DB-9B0A-11440800AADD.html) beschrieben.
    - Andere Hypervisoren und öffentliche Clouds unterstützen möglicherweise Funktionen für eine geschachtelte Virtualisierung, die auch die Nutzung von Always Encrypted mit VSB-Enclaves ermöglichen. Informationen zur Kompatibilität und Konfigurationsanweisungen finden Sie in der Dokumentation zu Ihrer Virtualisierungslösung.
- [SQL Server Management Studio 18.3 (SSMS) oder höher](../../ssms/download-sql-server-management-studio-ssms.md)

Alternativ können Sie SSMS auf einem anderen Computer installieren.

> [!WARNING]
> In Produktionsumgebungen sollten Sie niemals SSMS oder andere Tools zum Verwalten von Always Encrypted-Schlüsseln oder Ausführen von Abfragen für verschlüsselte Daten auf dem SQL Server-Computer verwenden, da dies den Zweck der Verwendung von Always Encrypted mindern oder vollständig zunichte machen kann. Weitere Informationen finden Sie unter [Sicherheitsüberlegungen für die Schlüsselverwaltung](encryption/overview-of-key-management-for-always-encrypted.md#security-considerations-for-key-management).

### <a name="hgs-computer-requirements"></a>HGS-Computeranforderungen

- Windows Server 2019 Standard oder Datacenter Edition
- 2 CPUs
- 8 GB RAM
- 100 GB Speicher

> [!NOTE]
> Der HGS-Computer sollte nicht mit einer Domäne verknüpft werden, bevor Sie beginnen.

## <a name="step-1-configure-the-hgs-computer"></a>Schritt 1: Konfigurieren des HGS-Computers

In diesem Schritt konfigurieren Sie den HGS-Computer zum Ausführen des Host-Überwachungsdiensts, der den Hostschlüsselnachweis unterstützt.

1. Melden Sie sich beim HGS-Computer als Administrator (lokaler Administrator) an, öffnen Sie eine Windows PowerShell-Konsole mit erhöhten Rechten, und fügen Sie die Rolle für den Host-Überwachungsdienst hinzu, indem Sie den folgenden Befehl ausführen:

   ```powershell
   Install-WindowsFeature -Name HostGuardianServiceRole -IncludeManagementTools -Restart
   ```

2. Nach dem Neustart des HGS-Computers melden Sie sich erneut mit Ihrem Administratorkonto an, öffnen Sie eine Windows PowerShell-Konsole mit erhöhten Rechten, und führen Sie die folgenden Befehle aus, um den Host-Überwachungsdienst zu installieren und dessen Domäne zu konfigurieren. Das hier von Ihnen angegebene Kennwort gilt nur für das Kennwort des Reparaturmodus für Verzeichnisdienste für Active Directory. Das Anmeldekennwort Ihres Administratorkontos wird dadurch nicht geändert. Sie können einen Domänennamen Ihrer Wahl für „-HgsDomainName“ angeben.

   ```powershell
   $adminPassword = ConvertTo-SecureString -AsPlainText '<password>' -Force
   Install-HgsServer -HgsDomainName 'bastion.local' -SafeModeAdministratorPassword $adminPassword -Restart
   ```

3. Nach dem wiederholten Neustart des Computers melden Sie sich mit Ihrem Administratorkonto an (das nun auch ein Domänenadministrator ist), öffnen Sie eine Windows PowerShell-Konsole mit erhöhten Rechten, und konfigurieren Sie den Hostschlüsselnachweis für Ihre HGS-Instanz. 

   ```powershell
   Initialize-HgsAttestation -HgsServiceName 'hgs' -TrustHostKey  
   ```

4. Führen Sie den folgenden Befehl aus, um die IP-Adresse des HGS-Computers zu suchen. Speichern Sie diese IP-Adresse für spätere Schritte.

   ```powershell
   Get-NetIPAddress  
   ```

> [!NOTE]
> Wenn Sie auf Ihren HGS-Computer mit einem DNS-Namen verweisen möchten, können Sie eine Weiterleitung von den DNS-Servern Ihres Unternehmens an den neuen HGS-Domänencontroller einrichten.  

## <a name="step-2-configure-the-sql-server-computer-as-a-guarded-host"></a>Schritt 2: Konfigurieren des SQL Server-Computers als überwachter Host
In diesem Schritt konfigurieren Sie den SQL Server-Computer als überwachten Host, der beim Host-Überwachungsdienst mit Hostschlüsselnachweis registriert ist.

> [!WARNING]
> Der Hostschlüsselnachweis wird nur zur Verwendung in Testumgebungen empfohlen. Für Produktionsumgebungen sollten Sie den TPM-Nachweis verwenden.

1. Melden Sie sich beim SQL Server-Computer als Administrator an. Öffnen Sie eine Windows PowerShell-Konsole mit erhöhten Rechten, und rufen Sie den Namen Ihres Computers durch Zugreifen auf die Variable „computername“ ab.

   ```powershell
   $env:computername 
   ```

2. Installieren Sie die Funktion für überwachten Host, mit der auch Hyper-V installiert wird (sofern nicht bereits erfolgt).

   ```powershell
   Enable-WindowsOptionalFeature -Online -FeatureName HostGuardian -All
   ```

3. Starten Sie den SQL Server-Computer bei Aufforderung erneut, um die Installation von Hyper-V abzuschließen.

4. Wenn Ihr SQL Server-Computer ein virtueller Computer oder ein physischer Computer ist, der keinen sicheren UEFI-Start unterstützt oder nicht mit IOMMU ausgestattet ist, müssen Sie die VBS-Anforderung für die Plattformsicherheitsfeatures entfernen.
    1. Entfernen Sie die Anforderung für den sicheren Start und IOMMU, indem Sie den folgenden Befehl auf Ihrem SQL Server-Computer in einer PowerShell-Konsolenanwendung mit erhöhten Rechten ausführen:

        ```powershell
       Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard -Name RequirePlatformSecurityFeatures -Value 0
       ```

    1. Starten Sie den SQL Server-Computer neu, damit VBS mit den niedrigeren Anforderungen online geht.

        ```powershell
       Restart-Computer
       ```

5. Melden Sie sich erneut beim SQL Server-Computer als Administrator an, öffnen Sie eine Windows PowerShell-Konsole mit erhöhten Rechten, generieren Sie einen eindeutigen Hostschlüssel, und exportieren Sie den resultierenden öffentlichen Schlüssel in eine Datei.

   ```powershell
   Set-HgsClientHostKey 
   Get-HgsClientHostKey -Path $HOME\Desktop\hostkey.cer
   ```

6. Kopieren Sie manuell die Hostschlüsseldatei, die im vorherigen Schritt generiert wurde, auf den HGS-Computer. Bei den folgenden Anweisungen wird davon ausgegangen, dass der Dateiname „hostkey.cer“ lautet und Sie diese Datei auf Ihren Desktop auf dem HGS-Computer kopieren.

7. Öffnen Sie auf dem HGS-Computer eine Windows PowerShell-Konsole mit erhöhten Rechten, und registrieren Sie den Hostschlüssel Ihres SQL Server-Computers beim Host-Überwachungsdienst:

   ```powershell
   Add-HgsAttestationHostKey -Name <your SQL Server computer name> -Path $HOME\Desktop\hostkey.cer
   ```

8. Führen Sie auf dem SQL Server-Computer den folgenden Befehl in einer Windows PowerShell-Konsole mit erhöhten Rechten aus, um dem SQL Server-Computer den Ort für den Nachweis anzugeben. Vergewissern Sie sich, dass Sie die IP-Adresse oder den DNS-Namen des HSG-Computers an beiden Adressstellen angeben. 

   ```powershell
   # use http, and not https
   Set-HgsClientConfiguration -AttestationServerUrl http://<IP address or DNS name>/Attestation -KeyProtectionServerUrl http://<IP address or DNS name>/KeyProtection/  
   ```

Das Ergebnis des oben gezeigten Befehls sollte „AttestationStatus = Passed“ lauten.

Wenn Sie eine Fehlermeldung vom Typ „HostUnreachable“ erhalten, bedeutet dies, dass Ihr SQL Server-Computer nicht mit dem Host-Überwachungsdienst kommunizieren kann. Stellen Sie sicher, dass Sie den HGS Computer pingen können.

Ein Fehler vom Typ „UnauthorizedHost“ weist darauf hin, dass der öffentliche Schlüssel nicht beim HGS-Server registriert wurde. Wiederholen Sie die Schritte 5 und 6, um den Fehler zu beheben.

Wenn andere Fehler auftreten, führen Sie „Remove-HgsClientHostKey“ aus, und wiederholen Sie die Schritte 4 bis 7.

## <a name="step-3-enable-always-encrypted-with-secure-enclaves-in-sql-server"></a>Schritt 3: Aktivieren von Always Encrypted mit Secure Enclaves in SQL Server

In diesem Schritt aktivieren Sie die Funktionen von Always Encrypted mithilfe von Enclaves in Ihrer SQL Server-Instanz.

1. Verbinden Sie sich per SSMS als „sysadmin“ mit Ihrer SQL Server-Instanz, **ohne** dass Always Encrypted für die Datenverbindung aktiviert ist.
    1. Starten Sie SSMS.
    1. Geben Sie im Dialogfeld **Mit Server verbinden** Ihren Servernamen, eine Authentifizierungsmethode und Ihre Anmeldeinformationen an.
    1. Klicken Sie auf **Optionen >>** , und wählen Sie die Registerkarte **Always Encrypted** aus.
    1. Stellen Sie sicher, dass das Kontrollkästchen **Always Encrypted aktivieren (Spaltenverschlüsselung)** **nicht** ausgewählt ist.

          ![Herstellen einer Verbindung mit dem Server ohne Always Encrypted](./encryption/media/always-encrypted-enclaves/connect-without-always-encrypted-ssms.png)

    1. Wählen Sie **Verbinden**.

2. Öffnen Sie ein neues Abfragefenster, und führen Sie die folgende Anweisung, um den Typ der Secure Enclave auf virtualisierungsbasierte Sicherheit (VBS) festzulegen.

   ```sql
   EXEC sys.sp_configure 'column encryption enclave type', 1;
   RECONFIGURE;
   ```

3. Starten Sie Ihre SQL Server-Instanz neu, damit die vorherige Änderung übernommen wird. Sie können die Instanz in SSMS neu starten, indem Sie im Objekt-Explorer mit der rechten Maustaste darauf klicken und „Neustart“ auswählen. Stellen Sie nach dem Neustart der Instanz eine neue Verbindung her.

4. Vergewissern Sie sich, dass die Secure Enclave geladen ist, indem Sie die folgende Abfrage ausführen:

   ```sql
   SELECT [name], [value], [value_in_use] FROM sys.configurations
   WHERE [name] = 'column encryption enclave type';
   ```

    Die Abfrage sollte folgendes Ergebnis haben:  

    | name                           | value | value_in_use |
    | ------------------------------ | ----- | -------------- |
    | column encryption enclave type | 1     | 1              |

## <a name="step-4-create-a-sample-database"></a>Schritt 4: Erstellen einer Beispieldatenbank
In diesem Schritt erstellen Sie eine Datenbank mit einigen Beispieldaten, die Sie später verschlüsseln.

1. Führen Sie unter Verwendung der SSMS-Instanz aus dem vorherigen Schritt die folgende Anweisung in einem Abfragefenster aus, um eine neue Datenbank mit dem Namen **ContosoHR** zu erstellen.

    ```sql
    CREATE DATABASE [ContosoHR];
    ```

1. Erstellen Sie eine neue Tabelle mit dem Namen **Employees**.

    ```sql
    USE [ContosoHR];
    GO

    CREATE SCHEMA [HR];
    GO
    
    CREATE TABLE [HR].[Employees]
    (
        [EmployeeID] [int] IDENTITY(1,1) NOT NULL,
        [SSN] [char](11) NOT NULL,
        [FirstName] [nvarchar](50) NOT NULL,
        [LastName] [nvarchar](50) NOT NULL,
        [Salary] [money] NOT NULL
    ) ON [PRIMARY];
    ```

1. Fügen Sie der Tabelle **Employees** einige Mitarbeiterdatensätze hinzu.

    ```sql
    USE [ContosoHR];
    GO

    INSERT INTO [HR].[Employees]
            ([SSN]
            ,[FirstName]
            ,[LastName]
            ,[Salary])
        VALUES
            ('795-73-9838'
            , N'Catherine'
            , N'Abel'
            , $31692);

    INSERT INTO [HR].[Employees]
            ([SSN]
            ,[FirstName]
            ,[LastName]
            ,[Salary])
        VALUES
            ('990-00-6818'
            , N'Kim'
            , N'Abercrombie'
            , $55415);
    ```

## <a name="step-5-provision-enclave-enabled-keys"></a>Schritt 5: Bereitstellen Enclave-fähiger Schlüssel

In diesem Schritt erstellen Sie einen Spaltenhauptschlüssel und einen Spaltenverschlüsselungsschlüssel, die Enclave-Berechnungen zulassen.

1. Erweitern Sie Ihre Datenbank im **Objekt-Explorer** mit der SSMS-Instanz aus dem vorherigen Schritt, und navigieren Sie zu **Sicherheit** > **Always Encrypted-Schlüssel**.
1. Stellen Sie einen neuen Enclave-fähigen Spaltenhauptschlüssel bereit:
    1. Klicken Sie mit der rechten Maustaste auf **Always Encrypted-Schlüssel**, und wählen Sie **Neuer Spaltenhauptschlüssel** aus.
    2. Wählen Sie den Namen des Spaltenhauptschlüssels aus: **CMK1**.
    3. Stellen Sie sicher, dass Sie entweder **Windows-Zertifikatspeicher (Aktueller Benutzer oder lokaler Computer)** oder **Azure Key Vault** auswählen.
    4. Wählen Sie **Enclave-Berechnungen zulassen**.
    5. Wenn Sie Azure Key Vault ausgewählt haben, melden Sie sich bei Azure an, und wählen Sie Ihren Schlüsseltresor. Weitere Informationen zum Erstellen eines Schlüsseltresors für Always Encrypted finden Sie unter [Verwalten Ihrer Schlüsseltresore im Azure-Portal](/archive/blogs/kv/manage-your-key-vaults-from-new-azure-portal).
    6. Wählen Sie Ihr Zertifikat oder Ihren Azure-Schlüsselwertschlüssel aus, wenn bereits vorhanden, oder klicken Sie auf die Schaltfläche **Zertifikat generieren**, um ein neues zu erstellen.
    7. Klicken Sie auf **OK**.

        ![Enclave-Berechnungen zulassen](encryption/media/always-encrypted-enclaves/allow-enclave-computations.png)

1. Erstellen Sie einen neuen Enclave-fähigen Spaltenverschlüsselungsschlüssel:

    1. Klicken Sie mit der rechten Maustaste auf **Always Encrypted-Schlüssel**, und wählen Sie **Neuer Spaltenverschlüsselungsschlüssel**.
    2. Geben Sie einen Namen für den neuen Spaltenverschlüsselungsschlüssel ein: **CEK1**.
    3. Wählen Sie in der Dropdownliste **Spaltenhauptschlüssel** den in den vorherigen Schritten erstellten Spaltenhauptschlüssel.
    4. Klicken Sie auf **OK**.

## <a name="step-6-encrypt-some-columns-in-place"></a>Schritt 6: Direktes Verschlüsseln einiger Spalten

In diesem Schritt verschlüsseln Sie die in den Spalten **SSN** und **Salary** gespeicherten Daten in der serverseitigen Enclave und testen dann eine SELECT-Abfrage der Daten.

1. Öffnen Sie eine neue SSMS-Instanz, und verbinden Sie sich mit Ihrer SQL Server-Instanz, wenn Always Encrypted für die Datenverbindung aktiviert **ist**.
    1. Starten Sie eine neue SSMS-Instanz.
    1. Geben Sie im Dialogfeld **Mit Server verbinden** Ihren Servernamen, eine Authentifizierungsmethode und Ihre Anmeldeinformationen an.
    1. Klicken Sie auf **Optionen >>** , und wählen Sie die Registerkarte **Always Encrypted** aus.
    1. Aktivieren Sie das Kontrollkästchen **Always Encrypted aktivieren (Spaltenverschlüsselung)** und geben Sie Ihre Enclave-Nachweis-URL an (z.B. ht <span>tp://</span>hgs.bastion.local/Attestation).

          ![Herstellen einer Verbindung mit dem Server mit einem Nachweis unter Verwendung von SSMS](./encryption/media/always-encrypted-enclaves/column-encryption-setting.png)

    1. Wählen Sie **Verbinden**.
    1. Wenn Sie aufgefordert werden, die Parametrisierung für Always Encrypted zu aktivieren, klicken Sie auf **Aktivieren**.

1. Öffnen Sie mit derselben SSMS-Instanz (Always Encrypted ist aktiviert) ein neues Abfragefenster, und verschlüsseln Sie die Spalten **SSN** und **Salary**, indem Sie die folgenden Abfragen ausführen.

    ```sql
    USE [ContosoHR];
    GO

    ALTER TABLE [HR].[Employees]
    ALTER COLUMN [SSN] [char] (11) COLLATE Latin1_General_BIN2
    ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL
    WITH
    (ONLINE = ON);

    ALTER TABLE [HR].[Employees]
    ALTER COLUMN [Salary] [money]
    ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL
    WITH
    (ONLINE = ON);

    ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE;
    ```

    > [!NOTE]
    > Achten Sie auf die ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE-Anweisung, die den Abfrageplancache für die Datenbank im obigen Skript löscht. Nachdem Sie die Tabelle bearbeitet haben, müssen Sie die Pläne für alle Batches und gespeicherten Prozeduren leeren, die Zugriff auf die Tabelle haben, um die Informationen für die Parameterverschlüsselung zu aktualisieren. 

1. Um zu überprüfen, ob die Spalten **SSN** und **Salary** nun verschlüsselt sind, öffnen Sie ein neues Abfragefenster in der SSMS-Instanz, **ohne** dass Always Encrypted für die Datenbankverbindung aktiviert ist, und führen Sie die folgende Anweisung aus. Das Abfragefenster sollte verschlüsselte Werte in den Spalten **SSN** und **Salary** zurückgeben. Wenn Sie die gleiche Abfrage mit der SSMS-Instanz ausführen, bei der Always Encrypted aktiviert ist, sollten die Daten entschlüsselt angezeigt werden.

    ```sql
    SELECT * FROM [HR].[Employees];
    ```

## <a name="step-7-run-rich-queries-against-encrypted-columns"></a>Schritt 7: Ausführen umfangreicher Abfragen für verschlüsselte Spalten

Sie können nun umfangreiche Abfragen für verschlüsselte Spalten ausführen. Ein Teil der Abfrageverarbeitung wird in der serverseitigen Enclave ausgeführt. 

1. Stellen Sie in der SSMS-Instanz **mit** aktiviertem Always Encrypted sicher, dass auch die Parametrisierung für Always Encrypted aktiviert ist.
    1. Wählen Sie im Hauptmenü von SSMS **Tools** aus.
    2. Wählen Sie **Optionen...** aus.
    3. Navigieren Sie zu **Abfrageausführung** > **SQL Server** > **Erweitert**.
    4. Stellen Sie sicher, dass **Parametrisierung für Always Encrypted aktivieren** aktiviert ist.
    5. Klicken Sie auf **OK**.
2. Öffnen Sie ein neues Abfragefenster, fügen Sie die folgenden Abfrage ein, und führen Sie sie anschließend aus. Die Abfrage sollte Klartextwerte und Zeilen zurückgeben, die den angegebenen Suchkriterien entsprechen.

    ```sql
    DECLARE @SSNPattern [char](11) = '%6818';
    DECLARE @MinSalary [money] = $1000;
    SELECT * FROM [HR].[Employees]
    WHERE SSN LIKE @SSNPattern AND [Salary] >= @MinSalary;
    ```

3. Versuchen Sie dieselbe Abfrage erneut in der SSMS-Instanz, in der Always Encrypted nicht aktiviert ist, und notieren Sie den auftretenden Fehler.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie dieses Tutorial abgeschlossen haben, können Sie mit einem der folgenden Tutorials fortfahren:

- [Tutorial: Entwickeln einer .NET-Anwendung mithilfe von Always Encrypted mit Secure Enclaves](../../connect/ado-net/sql/tutorial-always-encrypted-enclaves-develop-net-apps.md)
- [Tutorial: Entwickeln einer .NET Framework-Anwendung mithilfe von Always Encrypted mit Secure Enclaves](tutorial-always-encrypted-enclaves-develop-net-framework-apps.md)
- [Tutorial: Erstellen und Verwenden von Indizes für Enclave-fähige Spalten mit zufälliger Verschlüsselung](./tutorial-creating-using-indexes-on-enclave-enabled-columns-using-randomized-encryption.md)

## <a name="see-also"></a>Weitere Informationen

- [Konfigurieren und Verwenden von Always Encrypted mit Secure Enclaves](encryption/configure-always-encrypted-enclaves.md)
- [Tutorial: Always Encrypted mit Secure Enclaves in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]](/azure/azure-sql/database/always-encrypted-enclaves-getting-started)
