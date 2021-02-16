---
description: Rotieren von Always Encrypted-Schlüsseln mithilfe von PowerShell
title: Rotation von Always Encrypted-Schlüsseln mithilfe von PowerShell | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/26/2019
ms.prod: sql
ms.prod_service: security, sql-database"
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
ms.assetid: 5117b4fd-c8d3-48d5-87c9-756800769f31
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 5d12d2e93c7580e3a70f7f63af085af335bbc5d7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340154"
---
# <a name="rotate-always-encrypted-keys-using-powershell"></a>Rotieren von Always Encrypted-Schlüsseln mithilfe von PowerShell
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

Dieser Artikel enthält die Schritte zur Rotation von Always Encrypted-Schlüsseln unter Verwendung des SqlServer PowerShell-Moduls. Informationen darüber, wie Sie die Arbeit mit dem SQL Server-PowerShell-Modul beginnen, finden Sie unter [Konfigurieren von Always Encrypted-Schlüsseln mithilfe von PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md).

Die Rotation von Always Encrypted-Schlüsseln bezieht sich auf das Ersetzen eines vorhandenen Schlüssels mit einem neuen Schlüssel. Sie müssen einen Schlüssel möglicherweise rotieren, wenn er kompromittiert wurde, oder um die Richtlinien und Kompatibilitätsbestimmungen Ihrer Organisation einzuhalten, die die regelmäßige Rotation kryptografischer Schlüssel vorschreiben. 

Always Encrypted verwendet zwei Schlüsseltypen, es gibt also zwei allgemeine Schlüsselrotationsworkflows: das Drehen von Spaltenhauptschlüssel und das Drehen von Spaltenverschlüsselungsschlüssel.

* **Rotieren des Spaltenverschlüsselungsschlüssels** – umfasst das Entschlüsseln von Daten, die mit dem vorhandenen Schlüssel verschlüsselt sind und das erneute Verschlüsseln der Daten mit einem neuen Spaltenverschlüsselungsschlüssel. Da ein Spaltenverschlüsselungsschlüssel sowohl Zugriff auf den Schlüssel als auch die Datenbank benötigt, kann die Rotation von Spaltenverschlüsselungsschlüsseln nur ohne Rollentrennung ausgeführt werden.
* **Rotation des Spaltenhauptschlüssels** – umfasst das Entschlüsseln von Spaltenverschlüsselungsschlüsseln, die mit dem aktuellen Spaltenhauptschlüssel verschlüsselt sind, das erneute Verschlüsseln mit dem neuen Spaltenhauptschlüssel sowie das Aktualisieren der Metadaten für beide Schlüsseltypen. Die Rotation des Spaltenhauptschlüssels kann mit oder ohne Rollentrennung abgeschlossen werden (bei Verwendung des SqlServer PowerShell-Moduls).

## <a name="column-master-key-rotation-without-role-separation"></a>Rotation eines Spaltenhauptschlüssels ohne Rollentrennung

Die in diesem Abschnitt beschriebene Methode zum Rotieren eines Spaltenhauptschlüssels unterstützt Rollentrennung zwischen einem Sicherheitsadministrator und einem Datenbankadministrator (DBA) nicht. Einige der nachfolgenden Schritte kombinieren Operationen an physischen Schlüsseln mit Operationen an Schlüsselmetadaten. Dieser Workflow ist also für Organisationen empfohlen, die das DevOps-Modell verwenden, oder für Fälle, in denen die Datenbank in der Cloud gehostet wird und das primäre Ziel ist, Cloudadministratoren (nicht jedoch lokalen DBAs) den Zugriff auf sensible Daten zu verweigern. Die Methode wird nicht empfohlen, wenn DBAs zu potenziellen Angreifer gehören, oder wenn DBAs nicht über Zugriff auf sensible Daten verfügen sollen.  


| Aufgabe | Artikel | Greift auf Klartextschlüssel/Schlüsselspeicher zu| Greift auf Datenbank zu
|:---|:---|:---|:---
|Schritt 1: Erstellen Sie einen neuen Spaltenhauptschlüssel in einem Schlüsselspeicher.<br><br>**Hinweis:** Das PowerShell-Modul „SqlServer“ unterstützt diesen Schritt nicht. Sie müssen Tools verwenden, die für Ihren Schlüsselspeicher spezifisch sind, um diese Aufgabe mithilfe der Befehlszeile durchzuführen. | [Erstellen und Speichern von Spaltenhauptschlüsseln für Always Encrypted](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)| Ja | Nein
|Schritt 2: Starten Sie eine PowerShell-Umgebung, und importieren Sie das SqlServer-Modul. | [Importieren des SqlServer-Moduls](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#importsqlservermodule) | Nein | Nein
|Schritt 3: Stellen Sie eine Verbindung mit Ihrem Server und Ihrer Datenbank her. | [Herstellen einer Verbindung mit einer Datenbank](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#connectingtodatabase) | Nein | Ja
|Schritt 4. Erstellen Sie ein SqlColumnMasterKeySettings-Objekt, dass Informationen über den Speicherort Ihres neuen Spaltenhauptschlüssels enthält. SqlColumnMasterKeySettings ist ein Objekt, das im Arbeitsspeicher (in PowerShell) vorhanden ist. Verwenden Sie das Cmdlet, das für Ihren Schlüsselspeicher spezifisch ist, um es zu erstellen. |[New-SqlAzureKeyVaultColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlazurekeyvaultcolumnmasterkeysettings)<br><br>[New-SqlCertificateStoreColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcertificatestorecolumnmasterkeysettings)<br><br>[New-SqlCngColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcngcolumnmasterkeysettings)<br><br>[New-SqlCspColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcspcolumnmasterkeysettings)<br> | Nein | Nein
|Schritt 5: Erstellen Sie die Metadaten über den neuen Spaltenhauptschlüssel in Ihrer Datenbank. | [New-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnmasterkey)<br><br>**Hinweis:** Im Hintergrund gibt das Cmdlet die Anweisung [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md) heraus, um Schlüsselmetadaten zu erstellen. | Nein | Ja
|Schritt 6: Authentifizieren Sie sich bei Azure, wenn Ihr aktueller Spaltenhauptschlüssel oder Ihr neuer Spaltenhauptschlüssel in Azure Key Vault gespeichert ist. | [Add-SqlAzureAuthenticationContext](/powershell/sqlserver/sqlserver/vlatest/add-sqlazureauthenticationcontext) | Ja | Nein
|Schritt 7. Starten Sie die Rotation, indem Sie jeden der Spaltenverschlüsselungsschlüssel verschlüsseln, der aktuell mit dem alten Spaltenhauptschlüssel geschützt ist, indem Sie den neuen Spaltenhauptschlüssel verwenden. Nach diesem Schritt ist jeder betroffene Spaltenverschlüsselungsschlüssel (der dem alten Spaltenhauptschlüssel zugeordnet ist, der rotiert wird) jeweils mit dem alten und dem neuen Spaltenhauptschlüssel verschlüsselt und besitzt zwei verschlüsselte Werte in den Datenbankmetadaten.| [Invoke-SqlColumnMasterKeyRotation](/powershell/sqlserver/sqlserver/vlatest/invoke-sqlcolumnmasterkeyrotation) | Ja | Ja
|Schritt 8: Stimmen Sie mit den Administratoren aller Anwendungen ab, die verschlüsselte Spalten in der Datenbank abfragen (die mit dem alten Spaltenhauptschlüssel geschützt sind), sodass sie sicherstellen können, dass die Anwendungen über Zugriff auf den neuen Spaltenhauptschlüssel verfügen.| [Create and Store Column Master Keys (Always Encrypted) (Erstellen und Speichern von Spaltenhauptschlüsseln (Always Encrypted))](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md) | Ja | Nein
|Schritt 9 Schließen Sie die Rotation ab.<br><br>**Hinweis:** Stellen Sie vor der Ausführung dieses Schritts sicher, dass alle Anwendungen, die verschlüsselte Spalten abfragen, die mit dem alten Spaltenhauptschlüssel geschützt sind, so konfiguriert sind, dass sie den neuen Spaltenhauptschlüssel verwenden. Wenn Sie diesen Schritt vorzeitig ausführen, können einige der Anwendungen möglicherweise die Daten nicht verschlüsseln. Schließen Sie die Rotation ab, indem Sie die verschlüsselten Werte aus der Datenbank entfernen, die mit dem alten Spaltenhauptschlüssel generiert wurden. Dadurch wird die Zuordnung zwischen dem alten Spaltenhauptschlüssel und den von ihm geschützten Spaltenverschlüsselungsschlüsseln entfernt. |[Complete-SqlColumnMasterKeyRotation](/powershell/sqlserver/sqlserver/vlatest/complete-sqlcolumnmasterkeyrotation)| Nein | Ja
|Schritt 10. Entfernen Sie die Metadaten aus dem alten Spaltenhauptschlüssel. |[Remove-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/remove-sqlcolumnmasterkey)| Nein | Ja

> [!NOTE]
> Es wird dringend empfohlen, dass Sie den alten Spaltenhauptschlüssel nach der Rotation nicht dauerhaft löschen. Stattdessen sollten Sie den alten Spaltenhauptschlüssel im aktuellen Schlüsselspeicher oder an einem anderen sicheren Ort archivieren. Wenn Sie die Datenbank aus einer Sicherungsdatei auf einen Zeitpunkt *vor* der Konfiguration des neuen Spaltenhauptschlüssels wiederherstellen, benötigen Sie den alten Schlüssel für den Datenzugriff.

### <a name="rotating-a-column-master-key-without-role-separation-windows-certificate-example"></a>Rotieren eines Spaltenhauptschlüssels ohne Rollentrennung (Windows-Zertifikat-Beispiel)

Das folgende Skript stellt ein End-to-End-Beispiel dar, das einen vorhandenen Spaltenhauptschlüssel (CMK1) mit einem neuen Spaltenhauptschlüssel (CMK2) ersetzt.

```powershell
# Create a new column master key in Windows Certificate Store.
$cert = New-SelfSignedCertificate -Subject "AlwaysEncryptedCert" -CertStoreLocation Cert:CurrentUser\My -KeyExportPolicy Exportable -Type DocumentEncryptionCert -KeyUsage KeyEncipherment -KeySpec KeyExchange -KeyLength 2048

# Import the SqlServer module
Import-Module "SqlServer"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Create a SqlColumnMasterKeySettings object for your new column master key. 
$newCmkSettings = New-SqlCertificateStoreColumnMasterKeySettings -CertificateStoreLocation "CurrentUser" -Thumbprint $cert.Thumbprint

# Create metadata for your new column master key in the database.
$newCmkName = "CMK2"
New-SqlColumnMasterKey -Name $newCmkName -InputObject $database -ColumnMasterKeySettings $newCmkSettings

# Initiate the rotation from the current column master key to the new column master key.
$oldCmkName = "CMK1"
Invoke-SqlColumnMasterKeyRotation -SourceColumnMasterKeyName $oldCmkName -TargetColumnMasterKeyName $newCmkName -InputObject $database

# Complete the rotation of the old column master key.
Complete-SqlColumnMasterKeyRotation -SourceColumnMasterKeyName $oldCmkName  -InputObject $database

# Remove the old column master key metadata.
Remove-SqlColumnMasterKey -Name $oldCmkName -InputObject $database
```

## <a name="column-master-key-rotation-with-role-separation"></a>Rotation eines Spaltenhauptschlüssels mit Rollentrennung

Der in diesem Abschnitt beschriebene Workflow der Rotation eines Spaltenhauptschlüssels stellt die Trennung zwischen einem Sicherheitsadministrator und einem DBA sicher.

> [!IMPORTANT]
> Stellen Sie vor der Ausführung der Schritte, für die in der nachfolgenden Tabelle *Greift auf Klartextschlüssel/Schlüsselspeicher zu*=**Ja** (Schritte, die auf Nur-Text-Schlüssel oder Schlüsselspeicher zugreifen) gilt, sicher, dass die PowerShell-Umgebung auf einem sicheren Computer ausgeführt wird, der nicht der Computer ist, auf dem Ihre Datenbank gehostet wird. Weitere Informationen finden Sie im Abschnitt [Security Considerations for Key Management](overview-of-key-management-for-always-encrypted.md#security-considerations-for-key-management)(Überlegungen zur Sicherheit für die Schlüsselverwaltung).

### <a name="part-1-dba"></a>Teil 1: DBA

Ein DBA ruft Metadaten über den Spaltenhauptschlüssel ab, der rotiert werden soll, und über die betroffenen Spaltenverschlüsselungsschlüssel, die dem aktuellen Spaltenhauptschlüssel zugeordnet sind. Der DBA gibt alle Informationen für den Sicherheitsadministrator frei.


| Aufgabe | Artikel | Greift auf Klartextschlüssel/Schlüsselspeicher zu| Greift auf Datenbank zu
|:---|:---|:---|:---
|Schritt 1: Starten Sie eine PowerShell-Umgebung, und importieren Sie das SqlServer-Modul. | [Importieren des SqlServer-Moduls](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#importsqlservermodule) | Nein | Keine
|Schritt 2: Stellen Sie eine Verbindung mit Ihrem Server und Ihrer Datenbank her. | [Herstellen einer Verbindung mit einer Datenbank](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#connectingtodatabase) | Nein | Ja
|Schritt 3: Rufen Sie die Metadaten über den alten Spaltenhauptschlüssel ab.| [Get-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/get-sqlcolumnmasterkey) | Nein | Ja
|Schritt 4. Rufen Sie die Metadaten über Spaltenhauptschlüssel ab, die vom alten Spaltenhauptschlüssel verschlüsselt sind, einschließlich ihrer verschlüsselter Werte. | [Get-SqlColumnEncryptionKey](/powershell/sqlserver/sqlserver/vlatest/get-sqlcolumnencryptionkey) | Nein | Ja
|Schritt 5: Geben Sie den Speicherort des Spaltenhauptschlüssels (den Anbieternamen und einen Schlüsselpfad des Spaltenhauptschlüssels) sowie die verschlüsselten Werte der Spaltenverschlüsselungsschlüssel frei, die mit dem alten Spaltenhauptschlüssel geschützt sind.| Weitere Informationen finden Sie in den folgenden Beispielen. | Nein | Nein

### <a name="part-2-security-administrator"></a>Teil 2: Sicherheitsadministrator

Der Sicherheitsadministrator generiert einen neuen Spaltenhauptschlüssel, verschlüsselt den betroffenen Spaltenverschlüsselungsschlüssel erneut mit einem neuen Spaltenhauptschlüssel und gibt die Informationen über den neuen Spaltenhauptschlüssel sowie einen Satz neuer verschlüsselter Werte für die betroffenen Spaltenverschlüsselungsschlüssel für den DBA frei.

| Aufgabe | Artikel | Greift auf Klartextschlüssel/Schlüsselspeicher zu| Greift auf Datenbank zu
|:---|:---|:---|:---
|Schritt 1: Rufen Sie von Ihrem DBA den Speicherort des alten Spaltenhauptschlüssels sowie die verschlüsselten Werte der zugeordneten Spaltenverschlüsselungsschlüssel ab, die mit dem alten Spaltenhauptschlüssel geschützt sind.|–<br>Weitere Informationen finden Sie in den folgenden Beispielen.|Nein| Nein
|Schritt 2: Erstellen Sie einen neuen Spaltenhauptschlüssel in einem Schlüsselspeicher.<br><br>**Hinweis:** Das Modul „SqlServer“ unterstützt diesen Schritt nicht. Sie müssen die Tools Verwenden, die für den Typ Ihres Schlüsselspeichers spezifisch sind, um diese Aufgabe mithilfe einer Befehlszeile durchzuführen.|[Erstellen und Speichern von Spaltenhauptschlüsseln für Always Encrypted](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)| Ja | Nein
|Schritt 3: Starten Sie eine PowerShell-Umgebung, und importieren Sie das SqlServer-Modul. | [Importieren des SqlServer-Moduls](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#importsqlservermodule) | Nein | Nein
|Schritt 4. Erstellen Sie ein SqlColumnMasterKeySettings-Objekt, dass Informationen über den Speicherort Ihres **alten** Spaltenhauptschlüssels enthält. SqlColumnMasterKeySettings ist ein Objekt, das im Arbeitsspeicher (in PowerShell) vorhanden ist. |New-SqlColumnMasterKeySettings| Nein | Nein
|Schritt 5: Erstellen Sie ein SqlColumnMasterKeySettings-Objekt, dass Informationen über den Speicherort Ihres **neuen** Spaltenhauptschlüssels enthält. SqlColumnMasterKeySettings ist ein Objekt, das im Arbeitsspeicher (in PowerShell) vorhanden ist. Verwenden Sie das Cmdlet, das für Ihren Schlüsselspeicher spezifisch ist, um es zu erstellen. | [New-SqlAzureKeyVaultColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlazurekeyvaultcolumnmasterkeysettings)<br><br>[New-SqlCertificateStoreColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcertificatestorecolumnmasterkeysettings)<br><br>[New-SqlCngColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcngcolumnmasterkeysettings)<br><br>[New-SqlCspColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcspcolumnmasterkeysettings)| Nein | Nein
|Schritt 6: Authentifizieren Sie sich bei Azure, wenn Ihr alter (aktueller) Spaltenhauptschlüssel oder Ihr neuer Spaltenhauptschlüssel in Azure Key Vault gespeichert ist. | [Add-SqlAzureAuthenticationContext](/powershell/sqlserver/sqlserver/vlatest/add-sqlazureauthenticationcontext) | Ja | Nein
|Schritt 7. Verschlüsseln Sie jeden Wert des Spaltenverschlüsselungsschlüssels erneut, der aktuell mit dem alten Spaltenhauptschlüssel geschützt ist, indem Sie den neuen Spaltenhauptschlüssel verwenden. | [New-SqlColumnEncryptionKeyEncryptedValue](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnencryptionkeyencryptedvalue)<br><br>**Hinweis:** Übergeben Sie beim Aufrufen dieses Cmdlets die SqlColumnMasterKeySettings-Objekte jeweils für den alten und den neuen Spaltenhauptschlüssel zusammen mit einem Wert des Spaltenverschlüsselungsschlüssels, der erneut verschlüsselt werden soll.|Ja|Nein
|Schritt 8: Geben Sie den Speicherort des neuen Spaltenhauptschlüssels (den Anbieternamen und einen Schlüsselpfad des Spaltenhauptschlüssels) sowie den Satz der neu verschlüsselten Werte der Spaltenverschlüsselungsschlüssel für Ihren Datenbankadministrator frei .| Weitere Informationen finden Sie in den folgenden Beispielen. | Nein | Nein

> [!NOTE]
> Es wird dringend empfohlen, dass Sie den alten Spaltenhauptschlüssel nach der Rotation nicht dauerhaft löschen. Stattdessen sollten Sie den alten Spaltenhauptschlüssel im aktuellen Schlüsselspeicher oder an einem anderen sicheren Ort archivieren. Wenn Sie die Datenbank aus einer Sicherungsdatei auf einen Zeitpunkt *vor* der Konfiguration des neuen Spaltenhauptschlüssels wiederherstellen, benötigen Sie den alten Schlüssel für den Datenzugriff.

### <a name="part-3-dba"></a>Teil 3: DBA

Der DBA erstellt Metadaten für den neuen Spaltenhauptschlüssel und aktualisiert die Metadaten der betroffenen Spaltenverschlüsselungsschlüssel, um den neuen Satz verschlüsselter Werte hinzuzufügen. In diesem Schritt koordiniert der DBA mit den Administratoren der Anwendungen, die Verschlüsselungsspalten abfragen. Diese stellen sicher, dass die Anwendung über Zugriff auf den neuen Spaltenhauptschlüssel verfügt. Sobald alle Anwendungen so eingerichtet sind, dass sie den neuen Spaltenhauptschlüssel verwenden können, entfernt der DBA den alten Satz der verschlüsselten Werte sowie die alten Spaltenhauptschlüssel-Metadaten.

| Aufgabe | Artikel | Greift auf Klartextschlüssel/Schlüsselspeicher zu| Greift auf Datenbank zu
|:---|:---|:---|:---
|Schritt 1: Rufen Sie von Ihrem Sicherheitsadministrator den Speicherort des neuen Spaltenhauptschlüssels sowie den neuen Satz von verschlüsselten Werten der entsprechenden Spaltenverschlüsselungsschlüssel ab, die mit dem alten Spaltenhauptschlüssel geschützt sind.| Weitere Informationen finden Sie in den folgenden Beispielen. | Nein | Nein
|Schritt 2: Starten Sie eine PowerShell-Umgebung, und importieren Sie das SqlServer-Modul. | [Importieren des SqlServer-Moduls](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#importsqlservermodule) | Nein | Nein
|Schritt 3: Stellen Sie eine Verbindung mit Ihrem Server und Ihrer Datenbank her. | [Herstellen einer Verbindung mit einer Datenbank](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#connectingtodatabase) | Nein | Ja
|Schritt 4. Erstellen Sie ein SqlColumnMasterKeySettings-Objekt, dass Informationen über den Speicherort Ihres neuen Spaltenhauptschlüssels enthält. SqlColumnMasterKeySettings ist ein Objekt, das im Arbeitsspeicher (in PowerShell) vorhanden ist. |New-SqlColumnMasterKeySettings| Nein| Nein
|Schritt 5: Erstellen Sie die Metadaten über den neuen Spaltenhauptschlüssel in Ihrer Datenbank.|[New-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnmasterkey)<br><br>**Hinweis:** Im Hintergrund ruft das Cmdlet die Anweisung [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md) auf, um Schlüsselmetadaten zu erstellen. | Nein | Ja
|Schritt 6: Rufen Sie die Metadaten über Spaltenhauptschlüssel ab, die vom alten Spaltenhauptschlüssel verschlüsselt sind.| [Get-SqlColumnEncryptionKey](/powershell/sqlserver/sqlserver/vlatest/get-sqlcolumnencryptionkey)| Nein | Ja
|Schritt 7. Fügen Sie einen neuen verschlüsselten Wert (der mit dem neuen Spaltenhauptschlüssel erstellt wurde) zu den Metadaten für jeden betroffenen Spaltenverschlüsselungsschlüssel hinzu.|[Add-SqlColumnEncryptionKeyValue](/powershell/sqlserver/sqlserver/vlatest/add-sqlcolumnencryptionkeyvalue)|Nein|Ja
|Schritt 8: Stimmen Sie mit den Administratoren aller Anwendungen ab, die verschlüsselte Spalten in der Datenbank abfragen (die mit dem alten Spaltenhauptschlüssel geschützt sind), sodass sie sicherstellen können, dass die Anwendungen über Zugriff auf den neuen Spaltenhauptschlüssel verfügen.|[Erstellen und Speichern von Spaltenhauptschlüsseln (Always Encrypted)](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)| Nein|Nein
|Schritt 9 Schließen Sie die Rotation ab, indem Sie die verschlüsselten Werte, die mit dem alten Spaltenhauptschlüssel generiert wurden aus der Datenbank entfernen.<br><br>**Hinweis:** Stellen Sie vor der Ausführung dieses Schritts sicher, dass alle Anwendungen, die verschlüsselte Spalten abfragen, die mit dem alten Spaltenhauptschlüssel geschützt sind, so konfiguriert sind, dass sie den neuen Spaltenhauptschlüssel verwenden. Wenn Sie diesen Schritt vorzeitig ausführen, können einige der Anwendungen möglicherweise die Daten nicht verschlüsseln.<br><br>Durch diesen Schritt wird eine Zuordnung zwischen dem alten Spaltenhauptschlüssel und den von ihm geschützten Spaltenverschlüsselungsschlüsseln entfernt. | [Complete-SqlColumnMasterKeyRotation](/powershell/sqlserver/sqlserver/vlatest/complete-sqlcolumnmasterkeyrotation)<br><br>Alternativ können Sie den Wert [Remove-SqlColumnEncryptionKeyValue](/powershell/sqlserver/sqlserver/vlatest/remove-sqlcolumnencryptionkeyvalue)verwenden. | Nein|Ja
|Schritt 10. Entfernen Sie die Metadaten des alten Spaltenhauptschlüssel aus der Datenbank.| [Remove-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/remove-sqlcolumnmasterkey)| Nein|Ja

### <a name="rotating-a-column-master-key-with-role-separation-windows-certificate-example"></a>Rotieren eines Spaltenhauptschlüssels mit Rollentrennung (Windows-Zertifikat-Beispiel)

Das folgende Skript ist ein End-to-End-Beispiel zum Generieren eines neuen Spaltenhauptschlüssels, der im Windows-Zertifikatspeicher ein Zertifikat darstellt, und zum Rotieren eines vorhandenen (aktuellen) Spaltenhauptschlüssels, um diesen mit dem neuen Spaltenhauptschlüssel zu ersetzen. Das Skript nimmt an, dass die Zieldatenbank den Spaltenhauptschlüssel namens CMK1 (der rotiert werden soll) enthält, der einige Spaltenverschlüsselungsschlüssel verschlüsselt.

Teil 1: DBA

```powershell
# Import the SqlServer module.
Import-Module "SqlServer"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Retrieve the data about the old column master key, which needs to be rotated.
$oldCmkName = "CMK1"
$oldCmk = Get-SqlColumnMasterKey -Name $oldCmkName -InputObject $database


# Share the location of the old column master key with a Security Administrator, via a CSV file on a share drive.
$oldCmkDataFile = "Z:\oldcmkdata.txt"
"KeyStoreProviderName, KeyPath" > $oldCmkDataFile
$oldCmk.KeyStoreProviderName +", " + $oldCmk.KeyPath >> $oldCmkDataFile


# Find column encryption keys associated with the old column master key and provide the encrypted values of column encryption keys to the Security Administrator, via a CSV file on a share drive.
$ceks = Get-SqlColumnEncryptionKey -InputObject $database
$oldCekValuesFile = "Z:\oldcekvalues.txt"
"CEKName, CEKEncryptedValue" > $oldCekValuesFile 
for($i=0; $i -lt $ceks.Length; $i++){
    if($ceks[$i].ColumnEncryptionKeyValues.Length -eq 2) {
        # This column encryption has 2 encrypted values - let's check, if it is associated with the old column master key.
        if($ceks[$i].ColumnEncryptionKeyValues[0].ColumnMasterKeyName -eq $oldCmkName -or $ceks[$i].ColumnEncryptionKeyValues[1].ColumnMasterKeyName -eq $oldCmkName) {
            Write-Host $ceks[$i].Name "already has 2 encrypted values and therefore" $oldCmkName + "cannot be rotated"
            exit 1
        }
    }
    if($ceks[$i].ColumnEncryptionKeyValues[0].ColumnMasterKeyName -eq $oldCmkName) {# This column encryption key has 1 encrypted value that was produced using the old column master key
        # Save the name and the encrypted value of the column encryption key in the file.
        $encryptedValue =  "0x" + -join ($ceks[$i].ColumnEncryptionKeyValues[0].EncryptedValue |  foreach {$_.ToString("X2") } )
        $ceks[$i].Name + "," + $encryptedValue >> $oldCekValuesFile
    }
} 
```


Teil 2: Sicherheitsadministrator

```powershell
# Obtain the location of the old column master key and the encrypted values of the corresponding column encryption keys, from your DBA, via a CSV file on a share drive.
$oldCmkDataFile = "Z:\oldcmkdata.txt"
$oldCmkData = Import-Csv $oldCmkDataFile
$oldCekValuesFile = "Z:\oldcekvalues.txt"
$oldCekValues = @(Import-Csv $oldCekValuesFile)

# Create a new column master key in Windows Certificate Store.
$storeLocation = "CurrentUser"
$certPath = "Cert:\" + $storeLocation + "\My"
$cert = New-SelfSignedCertificate -Subject "AlwaysEncryptedCert" -CertStoreLocation $certPath -KeyExportPolicy Exportable -Type DocumentEncryptionCert -KeyUsage DataEncipherment -KeySpec KeyExchange

# Import the SqlServer module.
Import-Module "SqlServer"

# Create a SqlColumnMasterKeySettings object for your old column master key. 
$oldCmkSettings = New-SqlColumnMasterKeySettings -KeyStoreProviderName $oldCmkData.KeyStoreProviderName -KeyPath $oldCmkData.KeyPath

# Create a SqlColumnMasterKeySettings object for your new column master key. 
$newCmkSettings = New-SqlCertificateStoreColumnMasterKeySettings -CertificateStoreLocation $storeLocation -Thumbprint $cert.Thumbprint


# Prepare a CSV file, you will use to share the encrypted values of column encryption keys, produced using the new column master key.
$newCekValuesFile = "Z:\newcekvalues.txt"
"CEKName, CEKEncryptedValue" > $newCekValuesFile

# Re-encrypt each value with the new column master key and save the new encrypted value in the file.
for($i=0; $i -lt $oldCekValues.Count; $i++){
    # Re-encrypt each value with the new CMK
    $newValue = New-SqlColumnEncryptionKeyEncryptedValue -TargetColumnMasterKeySettings $newCmkSettings -ColumnMasterKeySettings $oldCmkSettings -EncryptedValue $oldCekValues[$i].CEKEncryptedValue
    $oldCekValues[$i].CEKName + ", " + $newValue >> $newCekValuesFile
}

# Share the new column master key data with your DBA, via a CSV file.
$newCmkDataFile = $home + "\newcmkdata.txt"
"KeyStoreProviderName, KeyPath" > $newCmkDataFile
$newCmkSettings.KeyStoreProviderName +", " + $newCmkSettings.KeyPath >> $newCmkDataFile
```


Teil 3: DBA

```powershell
# Obtain the location of the new column master key and the new encrypted values of the corresponding column encryption keys, from your Security Administrator, via a CSV file on a share drive.
$newCmkDataFile = "Z:\newcmkdata.txt"
$newCmkData = Import-Csv $newCmkDataFile
$newCekValuesFile = "Z:\newcekvalues.txt"
$newCekValues = @(Import-Csv $newCekValuesFile)

# Import the SqlServer module.
Import-Module "SqlServer"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Create a SqlColumnMasterKeySettings object for your new column master key. 
$newCmkSettings = New-SqlColumnMasterKeySettings -KeyStoreProviderName $newCmkData.KeyStoreProviderName -KeyPath $newCmkData.KeyPath
# Create metadata for the new column master key in the database.
$newCmkName = "CMK2"
New-SqlColumnMasterKey -Name $newCmkName -InputObject $database -ColumnMasterKeySettings $newCmkSettings


# Get all CEK objects
$oldCmkName = "CMK1"
$ceks = Get-SqlColumnEncryptionKey -InputObject $database
for($i=0; $i -lt $ceks.Length; $i++){
    if($ceks[$i].ColumnEncryptionKeyValues.Length -eq 2) {# This column encryption key has 2 encrypted values. Let's check, if it is associated with the old CMK.
        if($ceks[$i].ColumnEncryptionKeyValues[0].ColumnMasterKeyName -eq $oldCmkName -or $ceks[$i].ColumnEncryptionKeyValues[1].ColumnMasterKeyName -eq $oldCmkName) {
            Write-Host $ceks[$i].Name "already has 2 encrypted values and therefore" $oldCmkName + "cannot be rotated"
            exit 1
        }
    }
    if($ceks[$i].ColumnEncryptionKeyValues[0].ColumnMasterKeyName -eq $oldCmkName) {
        # Find the corresponding new encrypted value, received from the Security Administrator.
        $newValueRow = ($newCekValues| Where-Object {$_.CEKName -eq $ceks[$i].Name }[0])
        # Update the column encryption key metadata object by adding the new encrypted value
        Add-SqlColumnEncryptionKeyValue -ColumnMasterKeyName $newCmkName -Name $ceks[$i].Name -EncryptedValue $newValueRow.CEKEncryptedValue -InputObject $database 
    }
}

# Complete the rotation of the current column master key.
Complete-SqlColumnMasterKeyRotation -SourceColumnMasterKeyName $oldCmkName  -InputObject $database

# Remove the old column master key.
Remove-SqlColumnMasterKey -Name $oldCmkName -InputObject $database
```

## <a name="rotating-a-column-encryption-key"></a>Rotieren eines Spaltenverschlüsselungsschlüssels

Die Rotation eines Spaltenverschlüsselungsschlüssels umfasst das Entschlüsseln der Daten in allen Spalten, die mit dem zu rotierenden Schlüssel verschlüsselt wurden, und die Neuverschlüsselung der Daten mithilfe des neuen Spaltenverschlüsselungsschlüssels. Dieser Workflow für die Rotation benötigt Zugriff auf die Schlüssel und die Datenbank und kann deshalb nicht mit Rollentrennung ausgeführt werden. Die Rotation eines Spaltenverschlüsselungsschlüssels kann viel Zeit in Anspruch nehmen, wenn die Tabellen mit den Spalten, die mit dem zu rotierenden Schlüssel verschlüsselt wurden, groß ist. Daher muss Ihre Organisation eine Rotation der Spaltenverschlüsselungsschlüssel sorgfältig planen.

Sie können einen Spaltenverschlüsselungsschlüssel offline oder online rotieren. Die erste Methode ist wahrscheinlich schneller, aber Ihre Anwendungen können nicht in die betroffenen Tabellen schreiben. Der zweite Ansatz dauert wahrscheinlich länger, aber Sie können den Zeitraum einschränken, in dem die betroffenen Tabellen für Anwendungen nicht verfügbar sind. Weitere Details finden Sie unter [Konfigurieren der Spaltenverschlüsselung mithilfe von Always Encrypted mit PowerShell](configure-column-encryption-using-powershell.md) und [Set-SqlColumnEncryption](/powershell/module/sqlserver/set-sqlcolumnencryption/).

| Aufgabe | Artikel | Greift auf Klartextschlüssel/Schlüsselspeicher zu| Greift auf Datenbank zu
|:---|:---|:---|:---
|Schritt 1: Starten Sie eine PowerShell-Umgebung, und importieren Sie das SqlServer-Modul. | [Importieren des SqlServer-Moduls](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#importsqlservermodule) | Nein | Nein
|Schritt 2: Stellen Sie eine Verbindung mit Ihrem Server und Ihrer Datenbank her. | [Herstellen einer Verbindung mit einer Datenbank](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#connectingtodatabase) | Nein | Ja
|Schritt 3: Authentifizieren Sie sich bei Azure, wenn Ihr Spaltenhauptschlüssel (der den Spaltenverschlüsselungsschlüssel schützt, der rotiert werden soll) in Azure Key Vault gespeichert ist. | [Add-SqlAzureAuthenticationContext](/powershell/sqlserver/sqlserver/vlatest/add-sqlazureauthenticationcontext) | Ja | Nein
|Schritt 4. Generieren Sie einen neuen Spaltenverschlüsselungsschlüssel, verschlüsseln Sie ihn mit dem Spaltenhauptschlüssel, und erstellen Sie Spaltenverschlüsselungsschlüssel-Metadaten in der Datenbank.  | [New-SqlColumnEncryptionKey](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnencryptionkey)<br><br>**Hinweis:** Verwenden Sie eine Variation des Cmdlets, in der intern ein Spaltenverschlüsselungsschlüssel generiert und verschlüsselt wird.<br>Im Hintergrund gibt das Cmdlet die Anweisung [CREATE COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-column-encryption-key-transact-sql.md) heraus, um die Schlüsselmetadaten zu erstellen. | Ja | Ja
|Schritt 5: Suchen Sie alle Spalten, die mit dem alten Spaltenverschlüsselungsschlüssel verschlüsselt sind. | [Programmierungshandbuch für SQL Server Management Objects (SMO)](../../../relational-databases/server-management-objects-smo/sql-server-management-objects-smo-programming-guide.md) | Nein | Ja
|Schritt 6: Erstellen Sie ein *SqlColumnEncryptionSettings* -Objekt für jede betroffene Spalte.  SqlColumnMasterKeySettings ist ein Objekt, das im Arbeitsspeicher (in PowerShell) vorhanden ist. Es gibt das Zielverschlüsselungsschema für eine Spalte an. In diesem Fall sollte das Objekt angeben, dass die betroffene Spalte mit dem neuen Spaltenverschlüsselungsschlüssel verschlüsselt werden soll. | [New-SqlColumnEncryptionSettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnencryptionsettings) | Nein | Nein
|Schritt 7. Verschlüsseln Sie die Spalten, die in Schritt 5 angegeben sind, erneut mit dem neuen Verschlüsselungsschlüssel. | [Set-SqlColumnEncryption](/powershell/sqlserver/sqlserver/vlatest/set-sqlcolumnencryption)<br><br>**Hinweis:** Dieser Schritt kann lange dauern. Je nach gewähltem Ansatz (online oder offline) können Ihre Anwendungen während des gesamten Vorgangs oder nur teilweise nicht auf die Tabellen zugreifen. | Ja | Ja
|Schritt 8: Entfernen Sie die Metadaten für den alten Spaltenverschlüsselungsschlüssel. | [Remove-SqlColumnEncryptionKey](/powershell/sqlserver/sqlserver/vlatest/remove-sqlcolumnencryptionkey) | Nein | Ja

### <a name="example---rotating-a-column-encryption-key"></a>Beispiel – Rotieren eines Spaltenverschlüsselungsschlüssels

Das folgende Skript zeigt, wie ein Spaltenverschlüsselungsschlüssel rotiert wird.  Das Skript nimmt an, dass die Zieldatenbank einige Spalten enthält, die mit einem Spaltenverschlüsselungsschlüssel namens CEK1 (der rotiert werden soll) verschlüsselt sind, der mithilfe eines Spaltenhauptschlüssels namens CMK1 geschützt ist (der Spaltenhauptschlüssel ist nicht in Azure Key Vault gespeichert). 


```powershell
# Import the SqlServer module.
Import-Module "SqlServer"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Generate a new column encryption key, encrypt it with the column master key and create column encryption key metadata in the database. 
$cmkName = "CMK1"
$newCekName = "CEK2"
New-SqlColumnEncryptionKey -Name $newCekName -InputObject $database -ColumnMasterKey $cmkName 


# Find all columns encrypted with the old column encryption key, and create a SqlColumnEncryptionSetting object for each column.
$ces = @()
$oldCekName = "CEK1"
$tables = $database.Tables
for($i=0; $i -lt $tables.Count; $i++){
    $columns = $tables[$i].Columns
    for($j=0; $j -lt $columns.Count; $j++) {
        if($columns[$j].isEncrypted -and $columns[$j].ColumnEncryptionKeyName -eq $oldCekName) {
            $threeColPartName = $tables[$i].Schema + "." + $tables[$i].Name + "." + $columns[$j].Name 
            $ces += New-SqlColumnEncryptionSettings -ColumnName $threeColPartName -EncryptionType $columns[$j].EncryptionType -EncryptionKey $newCekName
        }
     }
}

# Re-encrypt all columns, currently encrypted with the old column encryption key, using the new column encryption key.
Set-SqlColumnEncryption -ColumnEncryptionSettings $ces -InputObject $database -UseOnlineApproach -MaxDowntimeInSeconds 120 -LogFileDirectory .

# Remove the old column encryption key metadata.
Remove-SqlColumnEncryptionKey -Name $oldCekName -InputObject $database
```

## <a name="next-steps"></a>Nächste Schritte
- [Abfragen von Spalten mit Always Encrypted und SQL Server Management Studio](always-encrypted-query-columns-ssms.md)
- [Entwickeln von Anwendungen mit Always Encrypted](always-encrypted-client-development.md)
  
## <a name="see-also"></a>Weitere Informationen
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Übersicht über die Schlüsselverwaltung für Always Encrypted](overview-of-key-management-for-always-encrypted.md) 
- [Konfigurieren von Always Encrypted mithilfe von PowerShell](configure-always-encrypted-using-powershell.md)
- [Rotieren von Always Encrypted-Schlüsseln mithilfe von SQL Server Management Studio](rotate-always-encrypted-keys-using-ssms.md)
- [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md)
- [DROP COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/drop-column-master-key-transact-sql.md)
- [CREATE COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-column-encryption-key-transact-sql.md)
- [ALTER COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/alter-column-encryption-key-transact-sql.md)
- [DROP COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/drop-column-encryption-key-transact-sql.md) 
- [sys.column_master_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-master-keys-transact-sql.md)
- [sys.column_encryption_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-encryption-keys-transact-sql.md)