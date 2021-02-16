---
title: Konfigurieren von Always Encrypted mithilfe von PowerShell | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie das PowerShell-Modul „SqlServer“ importieren und verwenden, das Cmdlets bereitstellt, mit denen Sie Always Encrypted sowohl in Azure SQL-Datenbank als auch in SQL Server konfigurieren können.
ms.custom: ''
ms.date: 10/01/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
ms.assetid: 12f2bde5-e100-41fa-b474-2d2332fc7650
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c5768357f83252c4d71ac1aeff064364a63367b2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345406"
---
# <a name="configure-always-encrypted-using-powershell"></a>Konfigurieren von Always Encrypted mithilfe von PowerShell
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

Das PowerShell-Modul „SqlServer“ stellt Cmdlets bereit, mit denen Sie [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md) sowohl in [!INCLUDE[ssSDSFull](../../../includes/sssdsfull-md.md)] als auch in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] konfigurieren können.

## <a name="security-considerations-when-using-powershell-to-configure-always-encrypted"></a>Sicherheitsüberlegungen bei der Verwendung von PowerShell zum Konfigurieren von Always Encrypted

Der primäre Zweck von Always Encrypted ist, sicherzustellen, dass verschlüsselte sensible Daten sicher sind, wenn das Datenbanksystem kompromittiert wird. Daher kann das Ausführen eines PowerShell-Skripts, das Schlüssel oder sensible Daten auf dem SQL Server-Computer verarbeitet, die Vorteile der Funktion einschränken oder zunichte machen. Weitere Empfehlungen zum Thema Sicherheit finden Sie unter [Security Considerations for Key Management](overview-of-key-management-for-always-encrypted.md#security-considerations-for-key-management)(Überlegungen zur Verwaltung von Schlüsseln).

Sie können PowerShell verwenden, um Always Encrypted-Schlüssel mit oder ohne Rollentrennung zu verwalten und so Kontrolle darüber zu erhalten, wer Zugriff auf die tatsächlichen Verschlüsselungsschlüssel im Schlüsselspeicher und auf die Datenbank erhält.

 Weitere Empfehlungen finden Sie unter [Security Considerations for Key Management](overview-of-key-management-for-always-encrypted.md#security-considerations-for-key-management)(Überlegungen zur Verwaltung von Schlüsseln).

## <a name="prerequisites"></a>Voraussetzungen

Installieren Sie das [SqlServer-Modul](/powershell/sqlserver/sqlserver/vlatest/sqlserver) auf einem sicheren Computer, der NICHT der Hostcomputer Ihrer SQL Server-Instanz ist. Das Modul kann direkt aus dem PowerShell-Katalog installiert werden.  In den [Downloadanweisungen](../../../powershell/download-sql-server-ps-module.md) finden Sie weitere Informationen.


## <a name="importing-the-sqlserver-module"></a><a name="importsqlservermodule"></a> Importieren des SqlServer-Moduls 

So laden Sie das SqlServer-Modul:

1.  Verwenden Sie das Cmdlet **Set-ExecutionPolicy** , um die entsprechende Skriptausführungsrichtlinie festzulegen.
2.  Verwenden Sie das Cmdlet **Import-Module** zum Importieren des SqlServer-Moduls.

In diesem Beispiel wird das SqlServer-Modul geladen.

```
# Import the SQL Server Module.  
Import-Module "SqlServer" 
```

## <a name="connecting-to-a-database"></a><a name="connectingtodatabase"></a> Herstellen einer Verbindung mit einer Datenbank

Einige der Always Encrypted-Cmdlets arbeiten mit Daten oder Metadaten in der Datenbank und erfordern, dass Sie zuerst eine Verbindung mit der Datenbank herstellen. Es werden zwei Methoden empfohlen, um bei der Konfiguration von Always Encrypted mithilfe des SqlServer-Moduls eine Verbindung mit einer Datenbank herzustellen: 
1. Stellen Sie mithilfe des Cmdlets **Get-SqlDatabase** eine Verbindung her.
2. Stellen Sie mithilfe des SQL Server PowerShell-Anbieters eine Verbindung her.

[!INCLUDE[freshInclude](../../../includes/paragraph-content/fresh-note-steps-feedback.md)]

### <a name="using-get-sqldatabase"></a>Verwenden von „Get-SqlDatabase“
Mit dem Cmdlet **Get-SqlDatabase** können Sie eine Verbindung mit einer Datenbank in SQL Server oder in Azure SQL-Datenbank herstellen. Das Cmdlet gibt ein Datenbankobjekt zurück, das Sie mithilfe des **InputObject**-Parameters eines Cmdlets übergeben können, das die Verbindung mit der Datenbank herstellt. 

### <a name="using-sql-server-powershell"></a>SQL Server PowerShell

```
# Import the SqlServer module
Import-Module "SqlServer"  

# Connect to your database
# Set the valid server name, database name and authentication keywords in the connection string
$serverName = "<Azure SQL server name>.database.windows.net"
$databaseName = "<database name>"
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Authentication = Active Directory Integrated"
$database = Get-SqlDatabase -ConnectionString $connStr

# List column master keys for the specified database.
Get-SqlColumnMasterKey -InputObject $database
```

Alternativ könnten Sie es auch pipegetrennt übergeben:


```
$database | Get-SqlColumnMasterKey
```

### <a name="using-sql-server-powershell-provider"></a>Verwenden eines SQL Server PowerShell-Anbieters
Der [SQL Server PowerShell-Anbieter](../../../powershell/sql-server-powershell-provider.md) macht die Hierarchie von SQL Server-Objekten in Pfaden auf eine Weise verfügbar, die der Verwendung von Dateisystempfaden ähnelt. Sie können mit SQL Server PowerShell in den Pfaden navigieren, indem Sie Windows PowerShell-Aliase ähnlich den Befehlen verwenden, die Sie normalerweise zum Navigieren in den Dateisystempfaden verwenden. Nachdem Sie zur Zielinstanz und zur Datenbank navigiert sind, gelten die nachfolgenden Cmdlets für diese Datenbank, wie im folgenden Beispiel gezeigt. 

> [!NOTE]
> Diese Methode zum Herstellen einer Verbindung mit einer Datenbank funktioniert nur für SQL Server (sie wird in Azure SQL-Datenbank nicht unterstützt).

```
# Import the SqlServer module.
Import-Module "SqlServer"
# Navigate to the database in the remote instance.
cd SQLSERVER:\SQL\servercomputer\DEFAULT\Databases\yourdatabase
# List column master keys in the above database.
Get-SqlColumnMasterKey
```

 
Alternativ können Sie einen Datenbankpfad mithilfe des allgemeinen **Path** -Parameters angeben, statt zur Datenbank zu navigieren.


```
# Import the SqlServer module.
Import-Module "SqlServer" 
# List column master keys for the specified database.
Get-SqlColumnMasterKey -Path SQLSERVER:\SQL\servercomputer\DEFAULT\Databases\yourdatabase
```
 
## <a name="always-encrypted-tasks-using-powershell"></a>Always Encrypted-Tasks mithilfe von PowerShell

- [Bereitstellen von Always Encrypted-Schlüsseln mithilfe von PowerShell](configure-always-encrypted-keys-using-powershell.md)
- [Rotieren von Always Encrypted-Schlüsseln mithilfe von PowerShell](../../../relational-databases/security/encryption/rotate-always-encrypted-keys-using-powershell.md)
- [Verschlüsseln, erneutes Verschlüsseln oder Entschlüsseln von Spalten mit Always Encrypted mithilfe von PowerShell](configure-column-encryption-using-powershell.md)


##  <a name="always-encrypted-cmdlet-reference"></a><a name="aecmdletreference"></a> Referenz zu Always Encrypted-Cmdlets

Die folgenden PowerShell-Cmdlets sind für Always Encrypted verfügbar:

|CMDLET |BESCHREIBUNG
|:---|:---
|**[Add-SqlAzureAuthenticationContext](/powershell/sqlserver/sqlserver/vlatest/add-sqlazureauthenticationcontext)** |Führt die Azure-Authentifizierung aus und ruft ein Authentifizierungstoken ab.
|**[Add-SqlColumnEncryptionKeyValue](/powershell/sqlserver/sqlserver/vlatest/add-sqlcolumnencryptionkeyvalue)** |Fügt einen neuen verschlüsselten Wert für ein vorhandenes Spaltenverschlüsselungsschlüssel-Objekt in der Datenbank hinzu.
|**[Complete-SqlColumnMasterKeyRotation](/powershell/sqlserver/sqlserver/vlatest/complete-sqlcolumnmasterkeyrotation)** |Schließt die Rotation eines Spaltenhauptschlüssels ab.
|**[Get-SqlColumnEncryptionKey](/powershell/sqlserver/sqlserver/vlatest/get-sqlcolumnencryptionkey)**   |Gibt alle in der Datenbank definierten Spaltenverschlüsselungsschlüssel-Objekte zurück, oder gibt ein Spaltenverschlüsselungsschlüssel-Objekt mit dem angegebenen Namen zurück.
|**[Get-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/get-sqlcolumnmasterkey)**   |Gibt die in der Datenbank definierten Spaltenhauptschlüssel-Objekte zurück, oder gibt ein Spaltenhauptschlüssel-Objekt mit dem angegebenen Namen zurück.
|**[Invoke-SqlColumnMasterKeyRotation](/powershell/sqlserver/sqlserver/vlatest/invoke-sqlcolumnmasterkeyrotation)** |Initiiert die Rotation eines Spaltenhauptschlüssels.
|**[New-SqlAzureKeyVaultColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlazurekeyvaultcolumnmasterkeysettings)** |Erstellt ein SqlColumnMasterKeySettings-Objekt, das einen asymmetrischen Schlüssel beschreibt, der in Azure Key Vault gespeichert ist.
|**[New-SqlCngColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcngcolumnmasterkeysettings)** |Erstellt ein SqlColumnMasterKeySettings-Objekt, das einen asymmetrischen Schlüssel beschreibt, der in einem Schlüsselspeicher gespeichert ist, der die Cryptography Next Generation-API (CNG) unterstützt.
|**[New-SqlColumnEncryptionKey](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnencryptionkey)**   |Erstellt ein neues Spaltenverschlüsselungsschlüssel-Objekt in der Datenbank
|**[New-SqlColumnEncryptionKeyEncryptedValue](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnencryptionkeyencryptedvalue)**   |Erstellt den verschlüsselten Wert eines Spaltenverschlüsselungsschlüssels.
|**[New-SqlColumnEncryptionSettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnencryptionsettings)** |Erstellt ein SqlColumnEncryptionSettings-Objekt, das Informationen über die Verschlüsselung einer einzelnen Spalte kapselt, einschließlich Spaltenverschlüsselungsschlüssel und Verschlüsselungstyp.
|**[New-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnmasterkey)**   |Erstellt ein Spaltenhauptschlüssel-Objekt in der Datenbank
|**[New-SqlColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnmasterkeysettings)**|Erstellt ein SqlColumnMasterKeySettings-Objekt für einen Spaltenhauptschlüssel mit dem angegebenen Anbieter und Schlüsselpfad
|**[New-SqlCspColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcspcolumnmasterkeysettings)** |Erstellt ein SqlColumnMasterKeySettings-Objekt, das einen asymmetrischen Schlüssel beschreibt, das in einem Schlüsselspeicher mit einem Kryptografiedienstanbieter (cryptography service provider; CSP) gespeichert ist, der die Kryptografie-API (CAPI) unterstützt.
|**[Remove-SqlColumnEncryptionKey](/powershell/sqlserver/sqlserver/vlatest/remove-sqlcolumnencryptionkey)** |Entfernt das Spaltenverschlüsselungsschlüssel-Objekt aus der Datenbank.
|**[Remove-SqlColumnEncryptionKeyValue](/powershell/sqlserver/sqlserver/vlatest/remove-sqlcolumnencryptionkeyvalue)**   |Entfernt einen verschlüsselten Wert für ein vorhandenes Spaltenverschlüsselungsschlüssel-Objekt aus der Datenbank.
|**[Remove-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/remove-sqlcolumnmasterkey)** |Entfernt das Spaltenhauptschlüssel-Objekt aus der Datenbank.
|**[Set-SqlColumnEncryption](/powershell/sqlserver/sqlserver/vlatest/set-sqlcolumnencryption)** |Verschlüsselt, entschlüsselt oder verschlüsselt angegebene Spalten in der Datenbank erneut.



## <a name="see-also"></a>Weitere Informationen

- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Übersicht über die Schlüsselverwaltung für Always Encrypted](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)
- [Konfigurieren von Always Encrypted mithilfe von SQL Server Management Studio](../../../relational-databases/security/encryption/configure-always-encrypted-using-sql-server-management-studio.md)
- [Entwickeln von Anwendungen mit Always Encrypted](always-encrypted-client-development.md)