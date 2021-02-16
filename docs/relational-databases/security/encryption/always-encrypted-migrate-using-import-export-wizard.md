---
description: Migrieren von Daten zu oder aus Spalten mithilfe von Always Encrypted mit dem SQL Server-Import/Export-Assistenten
title: Migrieren von Daten zu oder aus Spalten mithilfe von Always Encrypted mit dem SQL Server-Import/Export-Assistenten | Microsoft-Dokumentation
ms.custom: ''
ms.date: 10/31/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
f1_keywords:
- SQL13.SWB.COLUMNMASTERKEY.ROTATION.F1
helpviewer_keywords:
- Always Encrypted, configure with SSMS
ms.assetid: 29816a41-f105-4414-8be1-070675d62e84
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 234dabee531f85b4e3f8a687067dee9f91c96b33
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345495"
---
# <a name="migrate-data-to-or-from-columns-using-always-encrypted-with-sql-server-import-and-export-wizard"></a>Migrieren von Daten zu oder aus Spalten mithilfe von Always Encrypted mit dem SQL Server-Import/Export-Assistenten 
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

Mit dem Tool [SQL Server-Import/Export-Assistent](../../../integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard.md) können Sie Daten aus einer Quelle in ein Ziel kopieren. In diesem Dokument wird die Verwendung des SQL Server-Import/Export-Assistenten beschrieben, wenn es sich bei einer Quelle und/oder einem Ziel um eine SQL Server-Datenbank mit Spalten handelt, die mit [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md) geschützt werden.

## <a name="migration-scenarios"></a>Migrationsszenarios
Mit dem SQL Server-Import/Export-Assistenten können Sie die folgenden Szenarios zum Migrieren von Daten in oder aus verschlüsselten Spalten implementieren.

### <a name="encrypt-plaintext-data-on-migration"></a>Verschlüsseln von Klartextdaten bei der Migration
Enthält die Datenquelle Klartextdaten und stellt das Ziel eine SQL Server-Datenbank mit verschlüsselten Spalten dar, können Sie mit dem SQL Server-Import/Export-Assistenten die Klartextdaten aus der Quelle abrufen und verschlüsseln. Anschließend werden die verschlüsselten Daten (Chiffretext) in die verschlüsselten Spalten der Zieldatenbank kopiert. Für dieses Migrationsszenario kann es sich bei der Datenquelle um einen beliebigen Datenspeicher handeln, den der SQL Server-Import/Export-Assistent unterstützt. Dies kann beispielsweise eine Datei, eine SQL Server-Datenbank oder eine Datenbank in einem anderen Datenbanksystem sein.

Damit der SQL Server-Import/Export-Assistent die Daten verschlüsseln kann, muss Always Encrypted für die Verbindung mit der Zieldatenbank aktiviert sein, und Sie benötigen Zugriff auf die Schlüssel, mit denen die Daten in den Spalten der Zieldatenbank geschützt werden. Weitere Informationen finden Sie in den Abschnitten [Aktivieren und Deaktivieren von Always Encrypted für eine Datenbankverbindung](#enable-and-disable-always-encrypted-for-a-database-connection) und [Berechtigungen zum Verschlüsseln oder Entschlüsseln von Daten während der Migration](#permissions-for-encrypting-or-decrypting-data-during-migration).

### <a name="decrypt-encrypted-data-on-migration"></a>Entschlüsseln verschlüsselter Daten bei der Migration
Zum Migrieren von Daten, die in verschlüsselten Datenbankspalten einer SQL Server-Datenbank gespeichert sind, können Sie den SQL Server-Import/Export-Assistenten so konfigurieren, dass die Daten entschlüsselt und die entschlüsselten (Klartext-)Daten in ein Ziel kopiert werden. Dabei kann es sich um einen beliebigen Datenspeicher handeln, den der SQL Server-Import/Export-Assistent unterstützt, wie z. B. eine Datei, eine SQL Server-Datenbank oder eine Datenbank in einem anderen Datenbanksystem.

Damit der SQL Server-Import/Export-Assistent die Daten entschlüsseln kann, muss Always Encrypted für die Verbindung mit der Quelldatenbank aktiviert sein, und Sie benötigen Zugriff auf die Schlüssel, mit denen die Daten in den Spalten der Quelldatenbank geschützt werden. Weitere Informationen finden Sie in den Abschnitten [Aktivieren und Deaktivieren von Always Encrypted für eine Datenbankverbindung](#enable-and-disable-always-encrypted-for-a-database-connection) und [Berechtigungen zum Verschlüsseln oder Entschlüsseln von Daten während der Migration](#permissions-for-encrypting-or-decrypting-data-during-migration).

### <a name="re-encrypt-data-on-migration"></a>Erneutes Verschlüsseln von Daten bei der Migration
Beim Kopieren von Daten aus verschlüsselten Spalten einer SQL Server-Quelldatenbank in verschlüsselte Spalten derselben oder einer anderen SQL Server-Datenbank können Sie den SQL Server-Import/Export-Assistenten so konfigurieren, dass die Daten nach dem Abrufen aus der Quelle entschlüsselt werden. Vor dem Einfügen der Daten in die verschlüsselten Spalten der Zieldatenbank werden sie erneut verschlüsselt. Verwenden Sie diese Methode, wenn sich das Schema der Zielspalten (z. B. Spaltendatentypen, Verschlüsselungstypen und Spaltenverschlüsselungsschlüssel) vom Schema der Quellspalten unterscheidet.

Damit der SQL Server-Import/Export-Assistent die Daten verschlüsseln und entschlüsseln kann, muss Always Encrypted für die Verbindung mit der Quell- und der Zieldatenbank aktiviert sein, und Sie benötigen Zugriff auf die Schlüssel, mit denen die Daten in den Spalten der Quell- und der Zieldatenbank geschützt werden. Weitere Informationen finden Sie in den Abschnitten [Aktivieren und Deaktivieren von Always Encrypted für eine Datenbankverbindung](#enable-and-disable-always-encrypted-for-a-database-connection) und [Berechtigungen zum Verschlüsseln oder Entschlüsseln von Daten während der Migration](#permissions-for-encrypting-or-decrypting-data-during-migration).

### <a name="keep-data-encrypted-during-migration"></a>Beibehalten der Datenverschlüsselung bei der Migration
Beim Kopieren von Daten aus verschlüsselten Spalten einer SQL Server-Quelldatenbank in verschlüsselte Spalten derselben oder einer anderen SQL Server-Datenbank bei gleichzeitiger Verwendung des **exakt** gleichen Schemas (einschließlich derselben Datentypen, Verschlüsselungstypen und Spaltenverschlüsselungsschlüssel) durch die Ziel- und Quellspalten können Sie den SQL Server-Import/Export-Assistenten so konfigurieren, dass Chiffretext aus den Quellspalten abgerufen und in die verschlüsselten Spalten der SQL Server-Zieldatenbank eingefügt wird. 

Für dieses Szenario können Sie einen beliebigen Datenanbieter verwenden, der das Herstellen einer Verbindung zwischen SQL Server und der SQL Server-Quelldatenbank oder -Zieldatenbank unterstützt. Wenn Sie einen Anbieter verwenden, der das Herstellen einer Verbindung zwischen Always Encrypted und der Zieldatenbank unterstützt, müssen Sie sicherstellen, dass Always Encrypted für die Datenbankverbindung deaktiviert ist. Weitere Informationen finden Sie im Abschnitt [Aktivieren und Deaktivieren von Always Encrypted für eine Datenbankverbindung](#enable-and-disable-always-encrypted-for-a-database-connection).

Stellen Sie zudem sicher, dass für den Datenbankprinzipal (Benutzer), den der SQL Server-Import/Export-Assistent zum Herstellen einer Verbindung mit der Zieldatenbank verwendet, die Option `ALLOW_ENCRYPTED_VALUE_MODIFICATIONS` auf `ON` festgelegt ist. Diese Option unterdrückt kryptografische Metadatenüberprüfungen bei Massenkopiervorgängen auf dem Server, wodurch der Assistent die verschlüsselten Daten in einem Massenvorgang in die Zieldatenbank einfügen kann, ohne die Daten zu entschlüsseln. Weitere Informationen finden Sie unter [Massenladen von verschlüsselten Daten in durch Always Encrypted geschützte Spalten](migrate-sensitive-data-protected-by-always-encrypted.md).

## <a name="enable-and-disable-always-encrypted-for-a-database-connection"></a>Aktivieren und Deaktivieren von Always Encrypted für eine Datenbankverbindung
Erfordert das Migrationsszenario vom SQL Server-Import/Export-Assistent die Verschlüsselung und/oder Entschlüsselung von Daten, müssen Sie die Verbindung der SQL Server-Quelldatenbank und/oder -Zieldatenbank so konfigurieren, dass ein Datenanbieter verwendet wird, der Always Encrypted unterstützt. Zudem müssen Sie Always Encrypted für die Verbindung der Quell- und/oder Zieldatenbank aktivieren.

Muss der Assistent bei dieser Verbindung keine Daten verschlüsseln oder entschlüsseln, können Sie einen beliebigen Datenanbieter verwenden.

Always Encrypted wird von den folgenden Datenanbietern im SQL Server-Import/Export-Assistenten unterstützt:

- .NET Framework-Datenanbieter für SQL Server
  - Vergewissern Sie sich, dass der Computer, auf dem der Assistent ausgeführt wird, .NET Framework 4.6.1 oder höher verwendet.
  - Wenn Sie Always Encrypted für eine Verbindung aktivieren möchten, legen Sie in den Verbindungseigenschaften die Option `Column Encryption Setting` auf `Enabled` fest. Legen Sie `Column Encryption Setting` auf `Disabled` fest, um Always Encrypted zu deaktivieren. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit SQL Server mithilfe des .NET Framework-Datenanbieters für SQL Server](../../../integration-services/import-export-data/connect-to-a-sql-server-data-source-sql-server-import-and-export-wizard.md#connect-to-sql-server-with-the-net-framework-data-provider-for-sql-server) und [Aktivieren von Always Encrypted für Anwendungsabfragen](develop-using-always-encrypted-with-net-framework-data-provider.md#enabling-always-encrypted-for-application-queries).
- .NET Framework-Datenanbieter für ODBC
  - Installieren Sie den Microsoft ODBC-Treiber 13.1 oder höher.
    - Wenn Sie Always Encrypted für eine Verbindung aktivieren möchten, legen Sie in den Verbindungseigenschaften die Option `Column Encryption` auf `Enabled` fest. Legen Sie `Column Encryption` auf `Disabled` fest, um Always Encrypted zu deaktivieren. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit SQL Server mithilfe des ODBC-Treibers für SQL Server](../../../integration-services/import-export-data/connect-to-a-sql-server-data-source-sql-server-import-and-export-wizard.md#connect-to-sql-server-with-the-odbc-driver-for-sql-server) und [Aktivieren von Always Encrypted in einer ODBC-Anwendung](../../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md#enabling-always-encrypted-in-an-odbc-application).

## <a name="permissions-for-encrypting-or-decrypting-data-during-migration"></a>Berechtigungen zum Verschlüsseln oder Entschlüsseln von Daten während der Migration

Sie müssen in der Quelldatenbank über die Berechtigungen *VIEW ANY COLUMN MASTER KEY DEFINITION* und *VIEW ANY COLUMN ENCRYPTION KEY DEFINITION* verfügen, um in einer SQL Server-Quelldatenbank oder -Zieldatenbank gespeicherte Daten verschlüsseln oder entschlüsseln zu können.

Außerdem benötigen Sie Zugriff auf die Spaltenhauptschlüssel, die für die Spalten konfiguriert wurden, in denen die zu ver- oder entschlüsselnden Daten gespeichert sind:

- **Zertifikatspeicher – Lokaler Computer**: Sie benötigen Lesezugriff auf das Zertifikat, das als Spaltenhauptschlüssel verwendet wird, oder Administratorrechte auf dem Computer.
- **Azure Key Vault**: Sie benötigen die Berechtigungen _get_, _unwrapKey_ und _verify_ für den Tresor, der den Spaltenhauptschlüssel enthält.
- **Schlüsselspeicheranbieter (CNG)**: Die erforderlichen Berechtigungen und Anmeldeinformationen, die Sie möglicherweise eingeben müssen, wenn Sie einen Schlüsselspeicher oder einen Schlüssel verwenden, je nach Speicher und KSP-Konfiguration.
- **Kryptografiedienstanbieter (CAPI)**: Die erforderlichen Berechtigungen und Anmeldeinformationen, die Sie möglicherweise eingeben müssen, wenn Sie einen Schlüsselspeicher oder einen Schlüssel verwenden, je nach Speicher und CSP-Konfiguration.
Weitere Informationen finden Sie unter [Create and Store Column Master Keys (Always Encrypted)](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)(Erstellen und Speichern von Spaltenhauptschlüsseln (Always Encrypted)).

## <a name="next-steps"></a>Nächste Schritte
- [Abfragen von Spalten mit Always Encrypted und SQL Server Management Studio](always-encrypted-query-columns-ssms.md)
- [Entwickeln von Anwendungen mit Always Encrypted](always-encrypted-client-development.md)

## <a name="see-also"></a>Weitere Informationen
- [Always Encrypted](always-encrypted-database-engine.md)
- [Exportieren und Importieren von Datenbanken mit Always Encrypted](always-encrypted-migrate-using-bacpac.md)
- [Sichern und Wiederherstellen von Datenbanken mit Always Encrypted](always-encrypted-migrate-using-backup-restore.md)
- [Massenladen von verschlüsselten Daten in Spalten mithilfe von Always Encrypted](migrate-sensitive-data-protected-by-always-encrypted.md)
