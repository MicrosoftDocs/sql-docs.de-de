---
description: Konfigurieren der Spaltenverschlüsselung unter Verwendung von Always Encrypted mit einem DAC-Paket
title: Konfigurieren der Spaltenverschlüsselung unter Verwendung von Always Encrypted mit einem DAC-Paket | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/26/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Always Encrypted, configure with SSMS
ms.assetid: 29816a41-f105-4414-8be1-070675d62e84
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: cae5c4c74f5fc4f6df9b62b1ad985b469aa528ce
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345419"
---
# <a name="configure-column-encryption-using-always-encrypted-with-a-dac-package"></a>Konfigurieren der Spaltenverschlüsselung unter Verwendung von Always Encrypted mit einem DAC-Paket 
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

Ein [DAC-Paket (data-tier application, Datenschichtanwendung)](../../data-tier-applications/data-tier-applications.md), auch bekannt als DACPAC, ist eine portable Einheit der SQL Server-Datenbankbereitstellung, die alle SQL Server-Objekte einschließlich Tabellen und Spalten in den Tabellen definiert. Wenn Sie eine DACPAC-Datei in einer Datenbank veröffentlichen (wenn Sie eine Datenbank mithilfe einer DACPAC-Datei aktualisieren), wird das Schema der Zieldatenbank aktualisiert, sodass es dem Schema in der DACPAC-Datei entspricht. Sie können eine DACPAC-Datei mithilfe des [Assistenten zum Aktualisieren von Datenebenenanwendungen](../../data-tier-applications/upgrade-a-data-tier-application.md#UsingDACUpgradeWizard) in SQL Server Management Studio, [PowerShell](../../data-tier-applications/upgrade-a-data-tier-application.md#UpgradeDACPowerShell) oder [sqlpackage](../../../tools/sqlpackage/sqlpackage-publish.md) veröffentlichen.

In diesem Artikel werden spezielle Überlegungen zum Aktualisieren einer Datenbank behandelt, wenn die DACPAC-Datei oder/und die Zieldatenbank mit [Always Encrypted](always-encrypted-database-engine.md) geschützte Spalten enthält. Wenn das Verschlüsselungsschema für eine Spalte in der DACPAC-Datei vom Verschlüsselungsschema für eine vorhandene Spalte in der Zieldatenbank abweicht, führt die Veröffentlichung der DACPAC-Datei dazu, dass die in der Spalte gespeicherte Daten verschlüsselt, entschlüsselt oder erneut verschlüsselt werden. Ausführlichere Informationen finden Sie in der folgenden Tabelle.

| Bedingung|Aktion|
|:---|:---|
|Die Spalte wird in der DACPAC-Datei verschlüsselt, nicht in der Datenbank.| Die Daten in der Spalte werden verschlüsselt.|
|Die Spalte wird in der DACPAC-Datei verschlüsselt, nicht in der Datenbank.| Die Daten in der Spalte werden entschlüsselt (die Verschlüsselung wird für die Spalte entfernt).|
| Die Spalte wird sowohl in der DACPAC-Datei als auch in der Datenbank verschlüsselt. Allerdings verwendet die Spalte in der DACPAC-Datei einen anderen Verschlüsselungstyp und/oder einen Spaltenverschlüsselungsschlüssel als die entsprechende Spalte in der Datenbank.|Die Daten in der Spalte werden entschlüsselt und anschließend neu verschlüsselt, damit sie der Verschlüsselungskonfiguration in DACPAC-Datei entsprechen.|

Das Bereitstellen eines DAC-Pakets kann auch dazu führen, dass Metadatenobjekte für Spaltenhauptschlüssel oder Spaltenverschlüsselungsschlüssel für Always Encrypted erstellt oder entfernt werden.

## <a name="performance-considerations"></a>Leistungserwägungen
Ein Tool, mit dem Sie eine DACPAC-Datei bereitstellen, muss die Daten aus der Datenbank verschieben, um kryptografische Vorgänge auszuführen. Das Tool erstellt eine neue Tabelle (oder Tabellen) mit der gewünschten Verschlüsselungskonfiguration in der Datenbank, lädt alle Daten aus den ursprünglichen Tabellen, führt die angeforderten kryptografischen Vorgänge aus, lädt die Daten in die neue(n) Tabelle(n) hoch und vertauscht dann die ursprüngliche(n) Tabelle(n) mit der/den neuen Tabelle(n). Das Ausführen kryptografischer Vorgänge kann einige Zeit in Anspruch nehmen. Während dieser Zeit steht die Datenbank nicht zum Schreiben von Transaktionen zur Verfügung. 

::: moniker range=">=sql-server-ver15"

> [!NOTE]
> Wenn Sie [!INCLUDE [sssql19-md](../../../includes/sssql19-md.md)] verwenden und Ihre SQL Server-Instanz mit Secure Enclave konfiguriert ist, können Sie kryptografische Vorgänge direkt ausführen, ohne Daten aus der Datenbank zu verschieben. Informationen hierzu finden Sie unter [Konfigurieren einer direkten Spaltenverschlüsselung mithilfe von Always Encrypted mit Secure Enclaves](always-encrypted-enclaves-configure-encryption.md). Beachten Sie, dass die direkte Verschlüsselung für DACPAC-Bereitstellungen nicht verfügbar ist.

::: moniker-end

## <a name="permissions-for-publishing-a-dac-package-if-always-encrypted-is-set-up"></a>Berechtigungen zum Veröffentlichen eines DAC-Pakets, wenn Always Encrypted eingerichtet ist

Je nach den Unterschieden zwischen den Schemas der DACPAC-Datei und der Zieldatenbank benötigen Sie eventuell einige oder alle der unten aufgeführten Berechtigungen, um ein DAC-Paket zu veröffentlichen, wenn Always Encrypted in der DACPAC-Datei oder/und in der Zieldatenbank eingerichtet ist.

*ALTER ANY COLUMN MASTER KEY*, *ALTER ANY COLUMN ENCRYPTION KEY*, *VIEW ANY COLUMN MASTER KEY DEFINITION*, *VIEW ANY COLUMN ENCRYPTION KEY DEFINITION*

Wenn der Upgradevorgang eine Datenverschlüsselung auslöst, müssen Sie auch auf die für die betroffenen Spalten konfigurierten Spaltenhauptschlüssel zugreifen können:

- **Zertifikatspeicher – Lokaler Computer**: Sie benötigen Lesezugriff auf das Zertifikat, das als Spaltenhauptschlüssel verwendet wird, oder Administratorrechte auf dem Computer.
- **Azure Key Vault**: Sie benötigen die Berechtigungen *create*, *get*, *unwrapKey*, *wrapKey*, *sign* und *verify* für den Tresor, der den Spaltenhauptschlüssel enthält.
- **Schlüsselspeicheranbieter (CNG)** : Bei der Verwendung eines Schlüsselspeichers oder Schlüssels werden Sie möglicherweise aufgefordert, die erforderlichen Berechtigungen und Anmeldeinformationen anzugeben, welche von der Konfiguration des Speichers und des KSP abhängen.
- **Kryptografiedienstanbieter (Kryptografie-API)** : Bei der Verwendung eines Schlüsselspeichers oder Schlüssels werden Sie möglicherweise aufgefordert, die erforderlichen Berechtigungen und Anmeldeinformationen anzugeben, welche von der Konfiguration des Speichers und des CSP abhängen.

Weitere Informationen finden Sie unter [Create and Store Column Master Keys (Always Encrypted)](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)(Erstellen und Speichern von Spaltenhauptschlüsseln (Always Encrypted)). 

 
## <a name="next-steps"></a>Nächste Schritte
- [Entwickeln von Anwendungen mit Always Encrypted](always-encrypted-client-development.md)
- [Abfragen von Spalten mit Always Encrypted und SQL Server Management Studio](always-encrypted-query-columns-ssms.md)

## <a name="see-also"></a>Weitere Informationen  
 - [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
 - [Übersicht über die Schlüsselverwaltung für Always Encrypted](overview-of-key-management-for-always-encrypted.md) 
 - [Konfigurieren von Always Encrypted mithilfe von SQL Server Management Studio](configure-always-encrypted-using-sql-server-management-studio.md)
 - [Konfigurieren der Spaltenverschlüsselung mit dem Always Encrypted-Assistenten](always-encrypted-wizard.md)
 - [Konfigurieren der Spaltenverschlüsselung mithilfe von Always Encrypted mit PowerShell](configure-column-encryption-using-powershell.md)
 
