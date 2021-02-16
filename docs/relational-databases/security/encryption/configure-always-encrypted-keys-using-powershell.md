---
title: Bereitstellen von Always Encrypted-Schlüsseln mithilfe von PowerShell | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Schlüssel mithilfe des SqlServer PowerShell-Moduls für Always Encrypted bereitstellen, um die Zugriffssteuerung für die Verschlüsselungsschlüssel und die Datenbank bereitzustellen.
ms.custom: ''
ms.date: 06/26/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
ms.assetid: 3bdf8629-738c-489f-959b-2f5afdaf7d61
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 536b6f0acb0c8f3668b24e7bc5f0ad66ba352b03
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345432"
---
# <a name="provision-always-encrypted-keys-using-powershell"></a>Bereitstellen von Always Encrypted-Schlüsseln mithilfe von PowerShell
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

    
Dieser Artikel enthält die Schritte zum Bereitstellen von Always Encrypted-Schlüsseln unter Verwendung des [SqlServer PowerShell-Moduls](../../../powershell/sql-server-powershell-provider.md). Sie können PowerShell verwenden, um Always Encrypted-Schlüssel jeweils [mit und ohne Rollentrennung](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md#KeyManagementRoles)zu verwenden, und so Kontrolle über den Zugriff auf die tatsächlichen Verschlüsselungsschlüssel im Schlüsselspeicher zu bieten, und wer über Zugang zur Datenbank verfügt. 

Eine Übersicht über die Always Encrypted-Schlüsselverwaltung, einschließlich einiger allgemeiner Empfehlungen für Best Practices, finden Sie unter [Übersicht über die Schlüsselverwaltung für Always Encrypted](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md).
Informationen darüber, wie Sie die Arbeit mit dem SQL Server-PowerShell-Modul beginnen, finden Sie unter [Konfigurieren von Always Encrypted-Schlüsseln mithilfe von PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md).


## <a name="key-provisioning-without-role-separation"></a><a name="KeyProvisionWithoutRoles"></a> Schlüsselbereitstellung ohne Rollentrennung

Die in diesem Abschnitt beschriebene Schlüsselbereitstellungsmethode unterstützt keine Rollentrennung zwischen Sicherheitsadministratoren und Datenbankadministratoren (DBAs). Einige der unten aufgeführten Schritten kombinieren Vorgänge auf physischen Schlüsseln mit Vorgängen mit Schlüsselmetadaten. Daher wird diese Bereitstellungsmethode für Organisationen empfohlen, die das DevOps-Modell verwenden, oder für Fälle, in denen die Datenbank in der Cloud gehostet wird und das primäre Ziel ist, Cloudadministratoren (nicht jedoch lokalen DBAs) den Zugriff auf sensible Daten zu verweigern. Die Methode wird nicht empfohlen, wenn DBAs zu potenziellen Angreifer gehören oder wenn DBAs nicht über Zugriff auf sensible Daten verfügen sollen.

Stellen Sie vor der Ausführung der Schritte, die Zugriff auf Nur-Text-Schlüssel oder dem Schlüsselspeicher erfordern (in der Spalte **Greift auf Klartextschlüssel/-schlüsselspeicher zu** unten in der Tabelle angegeben), sicher, dass die PowerShell-Umgebung auf einem sicheren Computer ausgeführt wird, der nicht der Computer ist, auf dem Ihre Datenbank gehostet wird. Weitere Informationen finden Sie im Abschnitt [Security Considerations for Key Management](overview-of-key-management-for-always-encrypted.md#security-considerations-for-key-management)(Überlegungen zur Sicherheit für die Schlüsselverwaltung).


Aufgabe  |Artikel  |Greift auf Klartextschlüssel/-schlüsselspeicher zu  |Greift auf Datenbank zu   
---------|---------|---------|---------
Schritt 1: Erstellen Sie einen Spaltenhauptschlüssel in einem Schlüsselspeicher.<br><br>**Hinweis:** Das PowerShell-Modul „SqlServer“ unterstützt diesen Schritt nicht. Verwenden Sie die Tools, die für Ihren gewählten Schlüsselspeicher spezifisch sind, um diese Aufgabe über eine Befehlszeile durchzuführen. |[Erstellen und Speichern von Spaltenhauptschlüsseln für Always Encrypted](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md) | Ja | Nein     
Schritt 2:  Starten Sie eine PowerShell-Umgebung, und importieren Sie das SqlServer PowerShell-Modul.  |   [Konfigurieren von Always Encrypted mithilfe von PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md)   |    Nein    | Nein         
Schritt 3:  Stellen Sie eine Verbindung mit Ihrem Server und Ihrer Datenbank her.     |     [Herstellen einer Verbindung mit einer Datenbank](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#connectingtodatabase)    |    Nein     | Ja         
Schritt 4.  Erstellen Sie ein *SqlColumnMasterKeySettings* -Objekt, dass Informationen über den Speicherort Ihres Spaltenhauptschlüssels enthält. SqlColumnMasterKeySettings ist ein Objekt, das im Arbeitsspeicher (in PowerShell) vorhanden ist. Verwenden Sie das Cmdlet, das für Ihren Schlüsselspeicher spezifisch ist.   |     [New-SqlAzureKeyVaultColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlazurekeyvaultcolumnmasterkeysettings)<br><br>[New-SqlCertificateStoreColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcertificatestorecolumnmasterkeysettings)<br><br>[New-SqlCngColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcngcolumnmasterkeysettings)<br><br>[New-SqlCspColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcspcolumnmasterkeysettings)        |   Nein      | Nein         
Schritt 5:  Erstellen Sie die Metadaten über den Spaltenhauptschlüssel in Ihrer Datenbank.      |    [New-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnmasterkey)<br><br>**Hinweis:** Im Hintergrund gibt das Cmdlet die Anweisung [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md) heraus, um Schlüsselmetadaten zu erstellen.|    Nein     |    Ja
Schritt 6:  Authentifizieren Sie sich bei Azure, wenn Ihr Spaltenhauptschlüssel in Azure Key Vault gespeichert ist. | [Add-SqlAzureAuthenticationContext](/powershell/sqlserver/sqlserver/vlatest/add-sqlazureauthenticationcontext)    |  Ja   | Nein         
Schritt 7.  Generieren Sie einen neuen Spaltenverschlüsselungsschlüssel, verschlüsseln Sie ihn mit dem Spaltenhauptschlüssel, und erstellen Sie Spaltenverschlüsselungsschlüssel-Metadaten in der Datenbank.     |    [New-SqlColumnEncryptionKey](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnencryptionkey)<br><br>**Hinweis:** Verwenden Sie eine Variation des Cmdlets, in der intern ein Spaltenverschlüsselungsschlüssel generiert und verschlüsselt wird.<br><br>**Hinweis:** Im Hintergrund gibt das Cmdlet die Anweisung [CREATE COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-column-encryption-key-transact-sql.md) aus, um Schlüsselmetadaten zu erstellen.  | Ja | Ja
  

## <a name="windows-certificate-store-without-role-separation-example"></a>Windows-Zertifikatspeicher ohne Rollentrennung (Beispiel)

Dieses Skript ist ein End-to-End-Beispiel für das Generieren eines Spaltenhauptschlüssels, der ein Zertifikat im Windows-Zertifikatspeicher darstellt, einen Spaltenverschlüsselungsschlüssel generiert und verschlüsselt und Schlüsselmetadaten in einer SQL Server-Datenbank erstellt. 


```powershell
# Create a column master key in Windows Certificate Store.
$cert = New-SelfSignedCertificate -Subject "AlwaysEncryptedCert" -CertStoreLocation Cert:CurrentUser\My -KeyExportPolicy Exportable -Type DocumentEncryptionCert -KeyUsage DataEncipherment -KeySpec KeyExchange

# Import the SqlServer module.
Import-Module "SqlServer"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Create a SqlColumnMasterKeySettings object for your column master key. 
$cmkSettings = New-SqlCertificateStoreColumnMasterKeySettings -CertificateStoreLocation "CurrentUser" -Thumbprint $cert.Thumbprint

# Create column master key metadata in the database.
$cmkName = "CMK1"
New-SqlColumnMasterKey -Name $cmkName -InputObject $database -ColumnMasterKeySettings $cmkSettings


# Generate a column encryption key, encrypt it with the column master key and create column encryption key metadata in the database. 
$cekName = "CEK1"
New-SqlColumnEncryptionKey -Name $cekName  -InputObject $database -ColumnMasterKey $cmkName
```

## <a name="azure-key-vault-without-role-separation-example"></a>Azure Key Vault ohne Rollentrennung (Beispiel)

Dieses Skript ist ein End-to-End-Beispiel für das Bereitstellen und Konfigurieren von Azure Key Vault, für das Generieren eines Spaltenhauptschlüssels im Tresor, für das Generieren und Verschlüsseln eines Spaltenhauptschlüssels und für das Erstellen von Schlüsselmetadaten in einer Azure SQL-Datenbank.


```powershell
# Create a column master key in Azure Key Vault.
Import-Module Az
Connect-AzAccount
$SubscriptionId = "<Azure SubscriptionId>"
$resourceGroup = "<resource group name>"
$azureLocation = "<datacenter location>"
$akvName = "<key vault name>"
$akvKeyName = "<key name>"
$azureCtx = Set-AzConteXt -SubscriptionId $SubscriptionId # Sets the context for the below cmdlets to the specified subscription.
New-AzResourceGroup -Name $resourceGroup -Location $azureLocation # Creates a new resource group - skip, if your desired group already exists.
New-AzKeyVault -VaultName $akvName -ResourceGroupName $resourceGroup -Location $azureLocation # Creates a new key vault - skip if your vault already exists.
Set-AzKeyVaultAccessPolicy -VaultName $akvName -ResourceGroupName $resourceGroup -PermissionsToKeys get, create, delete, list, wrapKey,unwrapKey, sign, verify -UserPrincipalName $azureCtx.Account
$akvKey = Add-AzureKeyVaultKey -VaultName $akvName -Name $akvKeyName -Destination "Software"

# Connect to your database (Azure SQL database).
Import-Module "SqlServer"
$serverName = "<Azure SQL server name>.database.windows.net"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Authentication = Active Directory Integrated"
$database = Get-SqlDatabase -ConnectionString $connStr

# Create a SqlColumnMasterKeySettings object for your column master key. 
$cmkSettings = New-SqlAzureKeyVaultColumnMasterKeySettings -KeyURL $akvKey.Key.Kid

# Create column master key metadata in the database.
$cmkName = "CMK1"
New-SqlColumnMasterKey -Name $cmkName -InputObject $database -ColumnMasterKeySettings $cmkSettings

# Authenticate to Azure
Add-SqlAzureAuthenticationContext -Interactive

# Generate a column encryption key, encrypt it with the column master key and create column encryption key metadata in the database. 
$cekName = "CEK1"
New-SqlColumnEncryptionKey -Name $cekName -InputObject $database -ColumnMasterKey $cmkName
```

## <a name="cngksp-without-role-separation-example"></a>CNG/KSP ohne Rollentrennung (Beispiel)

Das Skript unten ist ein End-to-End-Beispiel für das Generieren eines Spaltenhauptschlüssels in einem Schlüsselspeicher, der die Cryptography Next Generation API (CNG) implementiert, für das Generieren und Verschlüsseln eines Spaltenhauptschlüssels und für das Erstellen von Schlüsselmetadaten in einer SQL Server-Datenbank.

Dieses Beispiel nutzt den Schlüsselspeicher, der den Softwareschlüsselspeicher-Anbieter von Microsoft verwendet. Sie möchten vielleicht das Beispiel ändern, um einen anderen Speicher zu verwenden, z.B. Ihr Hardwaresicherheitsmodul. Dazu müssen Sie sicherstellen, dass der Schlüsselspeicheranbieter (KSP), der CNG für Ihr Gerät implementiert, ordnungsgemäß auf dem Computer installiert ist. Sie müssen `Microsoft Software Key Storage Provider` durch den KSP-Namen Ihres Geräts ersetzen.


```powershell
# Create a column master key in a key store that has a CNG provider, a.k.a key store provider (KSP).
$cngProviderName = "Microsoft Software Key Storage Provider" # If you have an HSM, you can use a KSP for your HSM instead of a Microsoft KSP
$cngAlgorithmName = "RSA"
$cngKeySize = 2048 # Recommended key size for Always Encrypted column master keys
$cngKeyName = "AlwaysEncryptedKey" # Name identifying your new key in the KSP
$cngProvider = New-Object System.Security.Cryptography.CngProvider($cngProviderName)
$cngKeyParams = New-Object System.Security.Cryptography.CngKeyCreationParameters
$cngKeyParams.provider = $cngProvider
$cngKeyParams.KeyCreationOptions = [System.Security.Cryptography.CngKeyCreationOptions]::OverwriteExistingKey
$keySizeProperty = New-Object System.Security.Cryptography.CngProperty("Length", [System.BitConverter]::GetBytes($cngKeySize), [System.Security.Cryptography.CngPropertyOptions]::None);
$cngKeyParams.Parameters.Add($keySizeProperty)
$cngAlgorithm = New-Object System.Security.Cryptography.CngAlgorithm($cngAlgorithmName)
$cngKey = [System.Security.Cryptography.CngKey]::Create($cngAlgorithm, $cngKeyName, $cngKeyParams)

# Import the SqlServer module.
Import-Module "SqlServer"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Create a SqlColumnMasterKeySettings object for your column master key. 
$cmkSettings = New-SqlCngColumnMasterKeySettings -CngProviderName $cngProviderName -KeyName $cngKeyName

# Create column master key metadata in the database.
$cmkName = "CMK1"
New-SqlColumnMasterKey -Name $cmkName -InputObject $database -ColumnMasterKeySettings $cmkSettings

# Generate a column encryption key, encrypt it with the column master key and create column encryption key metadata in the database. 
$cekName = "CEK1"
New-SqlColumnEncryptionKey -Name $cekName -InputObject $database -ColumnMasterKey $cmkName
```

## <a name="key-provisioning-with-role-separation"></a><a name="KeyProvisionWithRoles"></a> Schlüsselbereitstellung mit Rollentrennung

Dieser Abschnitt enthält die Schritte zum Konfigurieren einer Verschlüsselung, mit der Sicherheitsadministratoren nicht über Zugriff auf die Datenbank verfügen, und Datenbankadministratoren nicht über Zugriff auf den Schlüsselspeicher oder Nur-Text-Schlüssel verfügen.


### <a name="security-administrator"></a>Sicherheitsadministrator

Bevor Sie Schritte ausführen, für die Sie Zugriff auf Nur-Text-Schlüssel oder den Schlüsselspeicher benötigen (in der Spalte **Greift auf Klartextschlüssel/-schlüsselspeicher zu** in der Tabelle unten), stellen Sie sicher, dass:
1.  Die PowerShell-Umgebung wird auf einem sicheren Computer ausgeführt, der nicht der Computer ist, auf dem Ihre Datenbank gehostet wird.
2.  Datenbankadministratoren in Ihrer Organisation nicht über Zugriff auf den Computer verfügen (die den Zweck der Rollentrennung zunichte machen würden).

Weitere Informationen finden Sie im Abschnitt [Security Considerations for Key Management](overview-of-key-management-for-always-encrypted.md#security-considerations-for-key-management)(Überlegungen zur Sicherheit für die Schlüsselverwaltung).


Aufgabe  |Artikel  |Greift auf Klartextschlüssel/-schlüsselspeicher zu  |Greift auf Datenbank zu  
---------|---------|---------|---------
Schritt 1: Erstellen Sie einen Spaltenhauptschlüssel in einem Schlüsselspeicher.<br><br>**Hinweis:** Das Modul „SqlServer“ unterstützt diesen Schritt nicht. Sie müssen die Tools Verwenden, die für den Typ Ihres Schlüsselspeichers spezifisch sind, um diese Aufgabe mithilfe einer Befehlszeile durchzuführen.     | [Erstellen und Speichern von Spaltenhauptschlüsseln für Always Encrypted](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)  |    Ja    | Nein 
Schritt 2:  Starten Sie eine PowerShell-Sitzung, und importieren Sie das SqlServer-Modul.      |     [Importieren des SqlServer-Moduls](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#importsqlservermodule)     | Nein | Nein         
Schritt 3:  Erstellen Sie ein *SqlColumnMasterKeySettings* -Objekt, dass Informationen über den Speicherort Ihres Spaltenhauptschlüssels enthält. *SqlColumnMasterKeySettings* ist ein Objekt, das im Arbeitsspeicher (in PowerShell) vorhanden ist. Verwenden Sie das Cmdlet, das für Ihren Schlüsselspeicher spezifisch ist. |      [New-SqlAzureKeyVaultColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlazurekeyvaultcolumnmasterkeysettings)<br><br>[New-SqlCertificateStoreColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcertificatestorecolumnmasterkeysettings)<br><br>[New-SqlCngColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcngcolumnmasterkeysettings)<br><br>[New-SqlCspColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcspcolumnmasterkeysettings)   | Nein         | Nein         
Schritt 4.  Authentifizieren Sie sich bei Azure, wenn Ihr Spaltenhauptschlüssel in Azure Key Vault gespeichert ist. |    [Add-SqlAzureAuthenticationContext](/powershell/sqlserver/sqlserver/vlatest/add-sqlazureauthenticationcontext)    |Ja|Nein         
Schritt 5:  Generieren Sie einen Spaltenverschlüsselungsschlüssel, und verschlüsseln Sie ihn mit dem Spaltenhauptschlüssel, um einen verschlüsselten Wert des Spaltenverschlüsselungsschlüssels zu erstellen.     |   [New-SqlColumnEncryptionKeyEncryptedValue](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnencryptionkeyencryptedvalue)     |    Ja    | Nein        
Schritt 6:  Stellen Sie den Speicherort des Spaltenhauptschlüssels (den Anbieternamen und einen Schlüsselpfad des Spaltenhauptschlüssels) und einen verschlüsselten Wert des Spaltenverschlüsselungsschlüssels für den DBA bereit.  | Weitere Informationen finden Sie in den folgenden Beispielen.        |   Nein      | Nein         

### <a name="dba"></a>DBA 

Datenbankadministratoren (DBAs) verwenden die Informationen, die sie vom Sicherheitsadministrator (Schritt 6 oben) empfangen, um die Always Encrypted-Schlüsselmetadaten in der Datenbank zu erstellen und zu verwalten.

Aufgabe  |Artikel  |Greift auf Nur-Text-Schlüsse zu  |Greift auf Datenbank zu   
---------|---------|---------|---------
Schritt 1:  Rufen Sie den Speicherort des Spaltenhauptschlüssels und des verschlüsselten Werts des Spaltenverschlüsselungsschlüssels von Ihrem Sicherheitsadministrator ab. |Weitere Informationen finden Sie in den folgenden Beispielen. | Nein | Nein
Schritt 2:  Starten Sie eine PowerShell-Umgebung, und importieren Sie das SqlServer-Modul.  | [Konfigurieren von Always Encrypted mithilfe von PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md)  | Nein | Nein
Schritt 3:  Stellen Sie eine Verbindung mit Ihrem Server und Ihrer Datenbank her. | [Herstellen einer Verbindung mit einer Datenbank](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#connectingtodatabase) | Nein | Ja
Schritt 4.  Erstellen Sie ein SqlColumnMasterKeySettings-Objekt, dass Informationen über den Speicherort Ihres Spaltenhauptschlüssels enthält. SqlColumnMasterKeySettings ist ein Objekt, das im Arbeitsspeicher vorhanden ist. | New-SqlColumnMasterKeySettings | Nein | Nein
Schritt 5: Erstellen Sie die Metadaten über den Spaltenhauptschlüssel in Ihrer Datenbank. | [New-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnmasterkey)<br>**Hinweis:** Im Hintergrund gibt das Cmdlet die Anweisung [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md) heraus, um Spaltenhauptschlüssel-Metadaten zu erstellen. | Nein | Ja
Schritt 6: Erstellen Sie die Spaltenverschlüsselungsschlüssel-Metadaten in der Datenbank. | New-SqlColumnEncryptionKey<br>**Hinweis:** DBAs verwenden eine Variation des Cmdlets, in der nur Spaltenverschlüsselungsschlüssel-Metadaten erstellt werden.<br>Im Hintergrund gibt das Cmdlet die Anweisung [CREATE COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-column-encryption-key-transact-sql.md) heraus, um Spaltenverschlüsselungsschlüssel-Metadaten zu erstellen. | Nein | Ja
  
## <a name="windows-certificate-store-with-role-separation-example"></a>Windows-Zertifikatspeicher mit Rollentrennung (Beispiel)

### <a name="security-administrator"></a>Sicherheitsadministrator


```powershell
# Create a column master key in Windows Certificate Store.
$storeLocation = "CurrentUser"
$certPath = "Cert:" + $storeLocation + "\My"
$cert = New-SelfSignedCertificate -Subject "AlwaysEncryptedCert" -CertStoreLocation $certPath -KeyExportPolicy Exportable -Type DocumentEncryptionCert -KeyUsage DataEncipherment -KeySpec KeyExchange

# Import the SqlServer module
Import-Module "SqlServer"

# Create a SqlColumnMasterKeySettings object for your column master key. 
$cmkSettings = New-SqlCertificateStoreColumnMasterKeySettings -CertificateStoreLocation "CurrentUser" -Thumbprint $cert.Thumbprint

# Generate a column encryption key, encrypt it with the column master key to produce an encrypted value of the column encryption key.
$encryptedValue = New-SqlColumnEncryptionKeyEncryptedValue -TargetColumnMasterKeySettings $cmkSettings

# Share the location of the column master key and an encrypted value of the column encryption key with a DBA, via a CSV file on a share drive
$keyDataFile = "Z:\keydata.txt"
"KeyStoreProviderName, KeyPath, EncryptedValue" > $keyDataFile
$cmkSettings.KeyStoreProviderName + ", " + $cmkSettings.KeyPath + ", " + $encryptedValue >> $keyDataFile

# Read the key data back to verify
$keyData = Import-Csv $keyDataFile
$keyData.KeyStoreProviderName
$keyData.KeyPath
$keyData.EncryptedValue 
```

### <a name="dba"></a>DBA

```powershell
# Obtain the location of the column master key and the encrypted value of the column encryption key from your Security Administrator, via a CSV file on a share drive.
$keyDataFile = "Z:\keydata.txt"
$keyData = Import-Csv $keyDataFile

# Import the SqlServer module
Import-Module "SqlServer"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Create a SqlColumnMasterKeySettings object for your column master key. 
$cmkSettings = New-SqlColumnMasterKeySettings -KeyStoreProviderName $keyData.KeyStoreProviderName -KeyPath $keyData.KeyPath

# Create column master key metadata in the database.
$cmkName = "CMK1"
New-SqlColumnMasterKey -Name $cmkName -InputObject $database -ColumnMasterKeySettings $cmkSettings

# Generate a  column encryption key, encrypt it with the column master key and create column encryption key metadata in the database. 
$cekName = "CEK1"
New-SqlColumnEncryptionKey -Name $cekName -InputObject $database -ColumnMasterKey $cmkName -EncryptedValue $keyData.EncryptedValue
```  
 
## <a name="next-steps"></a>Nächste Schritte    
- [Konfigurieren der Spaltenverschlüsselung mithilfe von Always Encrypted mit PowerShell](configure-column-encryption-using-powershell.md)  
- [Rotieren von Always Encrypted-Schlüsseln mithilfe von PowerShell](../../../relational-databases/security/encryption/rotate-always-encrypted-keys-using-powershell.md)
- [Entwickeln von Anwendungen mit Always Encrypted](always-encrypted-client-development.md)

## <a name="see-also"></a>Weitere Informationen    
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)    
- [Übersicht über die Schlüsselverwaltung für Always Encrypted](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)
- [Erstellen und Speichern von Spaltenhauptschlüsseln für Always Encrypted](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)
- [Konfigurieren von Always Encrypted mithilfe von PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md)
- [Bereitstellen von Always Encrypted-Schlüsseln mithilfe von SQL Server Management Studio](configure-always-encrypted-keys-using-ssms.md)
- [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md)
- [DROP COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/drop-column-master-key-transact-sql.md)
- [CREATE COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-column-encryption-key-transact-sql.md)
- [ALTER COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/alter-column-encryption-key-transact-sql.md)
- [DROP COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/drop-column-encryption-key-transact-sql.md) 
- [sys.column_master_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-master-keys-transact-sql.md)
- [sys.column_encryption_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-encryption-keys-transact-sql.md)
