---
title: Bereitstellen von Always Encrypted-Schlüsseln mithilfe von SQL Server Management Studio | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Spaltenhauptschlüssel und Spaltenverschlüsselungsschlüssel für Always Encrypted mithilfe von SQL Server Management Studio bereitstellen.
ms.custom: ''
ms.date: 10/01/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
f1_keywords:
- SQL13.SWB.COLUMNMASTERKEY.PAGE.F1
- SQL13.SWB.COLUMNENCRYPTIONKEY.PAGE.F1
helpviewer_keywords:
- Always Encrypted, configure with SSMS
ms.assetid: 29816a41-f105-4414-8be1-070675d62e84
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d4580e3e7f4b8511e8de18f8dad1dc1e99d8d801
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345442"
---
# <a name="provision-always-encrypted-keys-using-sql-server-management-studio"></a>Bereitstellen von Always Encrypted-Schlüsseln mithilfe von SQL Server Management Studio
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

Dieser Artikel enthält die Schritte zum Bereitstellen von Spaltenhauptschlüsseln und Spaltenverschlüsselungsschlüsseln für Always Encrypted mithilfe von [SQL Server Management Studio (SSMS)](../../../ssms/download-sql-server-management-studio-ssms.md).

Einen Überblick über die Always Encrypted-Schlüsselverwaltung einschließlich bewährter Methoden und wichtiger Sicherheitsüberlegungen finden Sie unter [Übersicht der Schlüsselverwaltung für Always Encrypted](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md).

<a name="provisioncmk"></a>
## <a name="provision-column-master-keys-with-the-new-column-master-key-dialog"></a>Bereitstellen von Spaltenhauptschlüsseln mit dem Dialogfeld „Neuer Spaltenhauptschlüssel“

Mit dem Dialogfeld **Neuer Spaltenhauptschlüssel** können Sie einen Spaltenhauptschlüssel generieren oder einen vorhandenen Schlüssel aus einem Schlüsselspeicher auswählen sowie Spaltenhauptschlüssel-Metadaten zu dem erstellten oder ausgewählten Schlüssel in der Datenbank erstellen.

1.  Navigieren Sie über den **Objekt-Explorer** zu dem Ordner **Sicherheit > Always Encrypted-Schlüssel** in Ihrer Datenbank.
2.  Klicken Sie mit der rechten Maustaste auf den Ordner **Spaltenhauptschlüssel**, und wählen Sie **Neuer Spaltenhauptschlüssel...** aus. 
3.  Geben Sie im Dialogfeld **Neuer Spaltenhauptschlüssel** den Namen des Spaltenhauptschlüssel-Metadatenobjekts ein.
4.  Wählen Sie einen Schlüsselspeicher aus:
    - **Zertifikatspeicher – Aktueller Benutzer**: Gibt den Zertifikatspeicherort des aktuellen Benutzers im Windows-Zertifikatspeicher an, der Ihrem persönlichen Zertifikatspeicher entspricht. 
    - **Zertifikatspeicher – Lokaler Computer**: Gibt den Zertifikatspeicherort des lokalen Computers im Windows-Zertifikatspeicher an. 
    - **Azure Key Vault:** Sie müssen sich bei Azure anmelden (klicken Sie auf **Anmelden**). Sobald Sie sich angemeldet haben, können Sie eines Ihrer Azure-Abonnements und einen Schlüsseltresor auswählen.
    - **Schlüsselspeicheranbieter (KSP)** : Gibt einen Schlüsselspeicher an, der über einen Schlüsselspeicheranbieter (KSP) zugänglich ist, der die Cryptography Next Generation-API (CNG) implementiert. Bei dieser Art von Speicher handelt sich in der Regel um ein Hardwaresicherheitsmodul (HSM). Nachdem Sie diese Option ausgewählt haben, müssen Sie einen KSP auswählen. Standardmäßig ist der **Softwareschlüsselspeicher-Anbieter von Microsoft** aktiviert. Wenn Sie einen in einem HSM gespeicherten Spaltenhauptschlüssel verwenden möchten, wählen Sie einen KSP für Ihr Gerät aus (muss installiert und auf dem Computer konfiguriert werden, bevor Sie das Dialogfeld öffnen).
    -   **Kryptografiedienstanbieter (CSP)** – Ein Schlüsselspeicher, der über einen Kryptografiedienstanbieter (CSP) zugänglich ist, der die Kryptografie-API (CAPI) implementiert. Bei dieser Art von Speicher handelt sich in der Regel um ein Hardwaresicherheitsmodul (HSM). Nachdem Sie diese Option ausgewählt haben, müssen Sie einen CSP auswählen.  Wenn Sie einen in einem HSM gespeicherten Spaltenhauptschlüssel verwenden möchten, wählen Sie einen CSP für Ihr Gerät aus (muss installiert und auf dem Computer konfiguriert werden, bevor Sie das Dialogfeld öffnen).
    
    > [!NOTE]
    > Da es sich bei CAPI um eine veraltete API handelt, ist die Option „Kryptografiedienstanbieter“ (CAPI) standardmäßig deaktiviert. Sie können sie aktivieren, indem Sie den für den CAPI-Anbieter aktivierten DWORD-Wert unter dem Schlüssel **[HKEY_CURRENT_USER\Software\Microsoft\Microsoft SQL Server\sql13\Tools\Client\Always Encrypted]** in der Windows-Registrierung erstellen und ihn auf 1 festlegen. Sofern Ihr Schlüsselspeicher CNG unterstützt, sollten Sie CNG statt CAPI verwenden.
   
    Weitere Informationen über die oben erwähnten Schlüsselspeicher finden Sie unter [Erstellen und Speichern von Spaltenhauptschlüsseln für Always Encrypted](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

5. Wenn Sie [!INCLUDE [sssql19-md](../../../includes/sssql19-md.md)] verwenden und die SQL Server-Instanz mit Secure Enclave konfiguriert ist, können Sie das Kontrollkästchen **Allow enclave computations** (Enclave-Berechnungen zulassen) aktivieren, um den Hauptschlüssel Enclave-fähig zu machen. Weitere Informationen finden Sie unter [Always Encrypted mit Secure Enclaves](always-encrypted-enclaves.md). 

    > [!NOTE]
    > Das Kontrollkästchen **Allow enclave computations** (Enclave-Berechnungen zulassen) wird nicht angezeigt, wenn Ihre SQL Server-Instanz nicht ordnungsgemäß mit Secure Enclave konfiguriert ist.

6.  Wählen Sie einen vorhandenen Schlüssel aus Ihrem Schlüsselspeicher aus, oder klicken Sie auf die Schaltflächen **Schlüssel generieren** oder **Zertifikat generieren** , um einen Schlüssel im Schlüsselspeicher zu erstellen. 
7.  Klicken Sie auf **OK** , und der neue Schlüssel wird in der Liste angezeigt. 

Nachdem Sie das Dialogfeld geschlossen haben, erstellt SQL Server Management Studio in der Datenbank Metadaten für den Spaltenhauptschlüssel. Das Dialogfeld erreicht dies, indem es die Anweisung [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md) generiert und ausgibt.

::: moniker range=">=sql-server-ver15"

Wenn Sie einen Enclave-fähigen Spaltenhauptschlüssel konfigurieren, signiert SSMS auch die Metadaten mithilfe des Spaltenhauptschlüssels. 

::: moniker-end

### <a name="permissions-for-provisioning-a-column-master-key"></a>Berechtigungen für die Bereitstellung eines Spaltenhauptschlüssels

Sie müssen in der Datenbank über die Datenbankberechtigung *ALTER ANY COLUMN MASTER KEY* verfügen, damit das Dialogfeld einen Spaltenhauptschlüssel erstellen kann. Sie benötigen möglicherweise Berechtigungen für den Schlüsselspeicher oder/und den Schlüssel, damit Sie das Dialogfeld zum Erstellen eines neuen Spaltenhauptschlüssels oder einen vorhandenen Schlüssel in einem Schlüsselspeicher zum Erstellen verwenden zu können.
- **Zertifikatspeicher – Lokaler Computer**: Sie benötigen Lesezugriff auf das Zertifikat, das als Spaltenhauptschlüssel verwendet wird, oder Administratorrechte auf dem Computer.
- **Azure Key Vault:** Sie benötigen die Berechtigungen *get* und *list* zum Auswählen und Verwenden eines Schlüssels sowie die Berechtigung *create* zum Erstellen eines neuen Schlüssels. Außerdem benötigen Sie zum Konfigurieren eines Enclave-fähigen Spaltenhauptschlüssels auch die *sign*-Berechtigung, um eine Signatur der Schlüsselmetadaten zu generieren.
- **Schlüsselspeicheranbieter (CNG)** : Bei der Verwendung eines Schlüsselspeichers oder Schlüssels werden Sie möglicherweise aufgefordert, die erforderlichen Berechtigungen und Anmeldeinformationen anzugeben, welche von der Konfiguration des Speichers und des KSP abhängen.
- **Kryptografiedienstanbieter (Kryptografie-API)** : Bei der Verwendung eines Schlüsselspeichers oder Schlüssels werden Sie möglicherweise aufgefordert, die erforderlichen Berechtigungen und Anmeldeinformationen anzugeben, welche von der Konfiguration des Speichers und des CSP abhängen.

Weitere Informationen finden Sie unter [Erstellen und Speichern von Spaltenhauptschlüsseln für Always Encrypted](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

<a name="provisioncek"></a> 
## <a name="provision-column-encryption-keys-with-the-new-column-encryption-key-dialog"></a>Bereitstellen von Spaltenverschlüsselungsschlüsseln mit dem Dialogfeld „Neuer Spaltenverschlüsselungsschlüssel“

Mit dem Dialogfeld **Neuer Spaltenverschlüsselungsschlüssel** können Sie einen Spaltenverschlüsselungsschlüssel generieren, mit einem Spaltenhauptschlüssel verschlüsseln und Metadaten zu dem Spaltenverschlüsselungsschlüssel in der Datenbank erstellen.

1.  Navigieren Sie über den **Objekt-Explorer** zu dem Ordner **Sicherheit &gt; Always Encrypted-Schlüssel** in Ihrer Datenbank.
2.  Klicken Sie mit der rechten Maustaste auf den Ordner **Spaltenverschlüsselungsschlüssel**, und wählen Sie **Neuer Spaltenverschlüsselungsschlüssel...** aus. 
3.  Geben Sie im Dialogfeld **Neuer Spaltenverschlüsselungsschlüssel** den Namen des Spaltenverschlüsselungsschlüssel-Metadatenobjekts ein.
4.  Wählen Sie ein Metadatenobjekt aus, das Ihren Spaltenhauptschlüssel in der Datenbank darstellt.
5.  Klicken Sie auf **OK**. 

Nachdem Sie das Dialogfeld geschlossen haben, generiert SQL Server Management Studio einen neuen Spaltenverschlüsselungsschlüssel und ruft anschließend die Metadaten für den Spaltenhauptschlüssel ab, den Sie in der Datenbank ausgewählt haben. SSMS verwendet dann die Metadaten des Spaltenhauptschlüssels zur Kontaktaufnahme mit dem Schlüsselspeicher, der Ihren Spaltenhauptschlüssel enthält, sowie zur Verschlüsselung Ihres Spaltenverschlüsselungsschlüssels. Schließlich erstellt SSMS die Metadatendaten für die neue Spaltenverschlüsselung in der Datenbank, indem die Anweisung [CREATE COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-column-encryption-key-transact-sql.md) generiert und ausgegeben wird.

### <a name="permissions-for-provisioning-a-column-encryption-key"></a>Berechtigungen für die Bereitstellung eines Spaltenverschlüsselungsschlüssels

Sie müssen in der Datenbank über die Datenbankberechtigungen *ALTER ANY COLUMN ENCRYPTION KEY* und *VIEW ANY COLUMN MASTER KEY DEFINITION* verfügen, damit das Dialogfeld die Metadaten des Spaltenverschlüsselungsschlüssels erstellen und auf die Metadaten des Spaltenhauptschlüssels zugreifen kann.
Sie benötigen möglicherweise Berechtigungen für Schlüsselspeicher oder/und den Schlüssel, um auf einen Schlüsselspeicher zugreifen und den Spaltenhauptschlüssel verwenden zu können:
- **Zertifikatspeicher – Lokaler Computer**: Sie benötigen Lesezugriff auf das Zertifikat, das als Spaltenhauptschlüssel verwendet wird, oder Administratorrechte auf dem Computer.
- **Azure Key Vault**: Sie benötigen die Berechtigungen *get*, *unwrapKey*, *wrapKey*, *sign* und *verify* für den Tresor mit dem Spaltenhauptschlüssel.
- **Schlüsselspeicheranbieter (CNG)** : Bei der Verwendung eines Schlüsselspeichers oder Schlüssels werden Sie möglicherweise aufgefordert, die erforderlichen Berechtigungen und Anmeldeinformationen anzugeben, welche von der Konfiguration des Speichers und des KSP abhängen.
- **Kryptografiedienstanbieter (Kryptografie-API)** : Bei der Verwendung eines Schlüsselspeichers oder Schlüssels werden Sie möglicherweise aufgefordert, die erforderlichen Berechtigungen und Anmeldeinformationen anzugeben, welche von der Konfiguration des Speichers und des CSP abhängen.

Weitere Informationen finden Sie unter [Erstellen und Speichern von Spaltenhauptschlüsseln (Always Encrypted)](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

## <a name="provision-always-encrypted-keys-using-the-always-encrypted-wizard"></a>Bereitstellen von Always Encrypted-Schlüsseln mithilfe des Always Encrypted-Assistenten

Der [Always Encrypted-Assistent](../../../relational-databases/security/encryption/always-encrypted-wizard.md) ist ein Tool für das Verschlüsseln, Entschlüsseln und erneute Verschlüsseln ausgewählter Datenbankspalten. Obwohl bereits konfigurierte Schlüssel verwendet werden können, können Sie auch einen neuen Spaltenhauptschlüssel und eine neue Spaltenverschlüsselung generieren. 

## <a name="next-steps"></a>Nächste Schritte
- [Konfigurieren der Spaltenverschlüsselung mit dem Always Encrypted-Assistenten](always-encrypted-wizard.md)
- [Konfigurieren der Spaltenverschlüsselung unter Verwendung von Always Encrypted mit einem DAC-Paket](configure-always-encrypted-using-dacpac.md)
- [Rotieren von Always Encrypted-Schlüsseln mithilfe von SQL Server Management Studio](rotate-always-encrypted-keys-using-ssms.md)
- [Entwickeln von Anwendungen mit Always Encrypted](always-encrypted-client-development.md)
- [Migrieren von Daten zu oder aus Spalten mithilfe von Always Encrypted mit dem SQL Server-Import/Export-Assistenten](always-encrypted-migrate-using-import-export-wizard.md)

## <a name="see-also"></a>Weitere Informationen
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Übersicht über die Schlüsselverwaltung für Always Encrypted](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)
- [Erstellen und Speichern von Spaltenhauptschlüsseln für Always Encrypted](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)
- [Konfigurieren von Always Encrypted mithilfe von SQL Server Management Studio](configure-always-encrypted-using-sql-server-management-studio.md)
- [Bereitstellen von Always Encrypted-Schlüsseln mithilfe von PowerShell](configure-always-encrypted-keys-using-powershell.md)
- [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md)
- [DROP COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/drop-column-master-key-transact-sql.md)
- [CREATE COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-column-encryption-key-transact-sql.md)
- [ALTER COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/alter-column-encryption-key-transact-sql.md)
- [DROP COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/drop-column-encryption-key-transact-sql.md) 
- [sys.column_master_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-master-keys-transact-sql.md)
- [sys.column_encryption_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-encryption-keys-transact-sql.md)
