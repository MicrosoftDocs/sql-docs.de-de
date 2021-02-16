---
title: Fehler- und Informationsprotokollierung des SQL Server-Connectors
description: In diesem Artikel wird beschrieben, wie Sie die Fehlerprotokollierung für den SQL Server-Connector aktivieren, indem Sie Registrierungseinträge bearbeiten.
ms.date: 10/08/2020
ms.localizationpriority: medium
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: how-to
author: rupp29
ms.author: arupp
ms.openlocfilehash: 3a674994d7028a8f4455c26d6f705b9da7b5ee08
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018683"
---
# <a name="sql-server-connector-error-and-information-logging"></a>Fehler- und Informationsprotokollierung des SQL Server-Connectors

In diesem Artikel wird erläutert, wie Sie Registrierungseinträge bearbeiten, um die Fehler- und Informationsprotokollierung für den SQL Server-Connector zu aktivieren.

## <a name="sql-server-connector-for-microsoft-azure-key-vault"></a>SQL Server-Connector für Microsoft Azure Key Vault

Der [SQL Server-Connector für Microsoft Azure Key Vault](https://www.microsoft.com/download/details.aspx?id=45344) ermöglicht es, dass bei der SQL Server-Verschlüsselung Microsoft Azure Key Vault als Anbieter für eine erweiterbare Schlüsselverwaltung (Extensible Key Management, EKM) verwendet werden kann, um Verschlüsselungsschlüssel zu schützen.

Der [Download](https://www.microsoft.com/download/details.aspx?id=45344) umfasst den SQL Server-Connector sowie Beispielskripts, um es einem Administrator für SQL Server zu ermöglichen, sich darüber zu informieren, wie der SQL Server-Connector konfiguriert wird. Außerdem wird er so in die Lage versetzt, Verschlüsselungsszenarios für SQL Server zu ermöglichen. Weitere Informationen finden Sie unter [Erweiterbare Schlüsselverwaltung mit Azure Key Vault (SQL Server)](./extensible-key-management-using-azure-key-vault-sql-server.md).

Im [Azure Key Vault-Forum](https://social.msdn.microsoft.com/Forums/AzureKeyVault) können Sie Fragen stellen, Erkenntnisse teilen und an Diskussionen zum SQL Server-Connector teilnehmen.

> [!NOTE]
> Bei einer normalen Ausführung erstellt die DLL für den SQL Server-Connector dynamisch Registrierungseinträge, um die Konnektivität zu Azure Key Vault herzustellen, damit der Schlüssel `[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\SQL Server Cryptographic Provider]` erstellt werden kann. Beim Startkonto für SQL Server muss es sich um einen lokalen Administrator handeln, oder das entsprechende Dienstkonto muss **NT SERVICE\MSSQLSERVER** sein.

## <a name="upgrade-sql-server-connector-to-the-latest-version"></a>Durchführen eines Upgrades für den SQL Server-Connector auf die aktuelle Version

Wenn Sie ein Upgrade des SQL Server-Connectors (Version: 1.0.5.0 mit einem Datum vom September 2020) auf die aktuelle Version des Verschlüsselungsanbieters der DLL durchführen möchten, befolgen Sie die folgenden Schritte.

### <a name="upgrade"></a>Aktualisieren

1. Halten Sie den SQL Server-Dienst mithilfe des SQL Server-Konfigurations-Managers an.
1. Deinstallieren Sie die alte Version über **Systemsteuerung\Programme\Programme und Features**.
    1. Anwendungsname: SQL Server-Connector für Microsoft Azure Key Vault
    1. Version: 15.0.300.96
    1. DLL-Dateidatum: 30.1.2018 15:00 Uhr
1. Installieren Sie eine neue Version des SQL Server-Connectors für Microsoft Azure Key Vault bzw. führen Sie ein entsprechendes Upgrade durch.
    1. Version: 15.0.2000.367
    1. DLL-Dateidatum: 11.9.2020 ‏‎05:17 Uhr
1. Starten Sie den SQL Server-Dienst.
1. Testen Sie, ob der Zugriff auf verschlüsselte Datenbanken möglich ist.

### <a name="rollback"></a>Rollback

1. Halten Sie den SQL Server-Dienst mithilfe des SQL Server-Konfigurations-Managers an.
1. Deinstallieren Sie die neue Version über **Systemsteuerung\Programme\Programme und Features**.
    1. Anwendungsname: SQL Server-Connector für Microsoft Azure Key Vault
    1. Version: 15.0.2000.367
    1. DLL-Dateidatum: 11.9.2020 ‏‎05:17 Uhr
1. Installieren Sie eine alte Version des SQL Server-Connectors für Microsoft Azure Key Vault.
    1. Version: 15.0.300.96
    1. DLL-Dateidatum: 30.1.2018 15:00 Uhr
1. Starten Sie den SQL Server-Dienst.
1. Testen Sie, ob der Zugriff auf verschlüsselte Datenbanken möglich ist.

> [!NOTE]
> - 1\.0.0.440-Versionen des SQL Server-Connectors sowie ältere Versionen wurden ersetzt und werden in Produktionsumgebungen nicht mehr unterstützt. Weitere Informationen zur Problembehandlung von Problemen des SQL Server-Connectors finden Sie unter [SQL Server-Connector – Verwaltung und Problembehandlung](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md).
> - Von Version 1.0.3.0 an meldet der SQL Server-Connector relevante Fehlermeldungen zur Problembehandlung an die Windows-Ereignisprotokolle.
> - Ab [1.0.4.0 (Version 13.0.811.168)](https://download.microsoft.com/download/8/0/9/809494F2-BAC9-4388-AD07-7EAF9745D77B/SQLServerConnectorforMicrosoftAzureKeyVault.msi) werden private Azure-Clouds unterstützt, darunter Azure China, Azure Deutschland und Azure Government.
> - In Version 1.0.5.0 gibt es einen Breaking Change, die den Fingerabdruckalgorithmus betrifft. Nach dem Upgrade auf Version 1.0.5.0 treten möglicherweise Fehler bei der Datenbankwiederherstellung auf. Weitere Informationen finden Sie im [KB-Artikel 447099](https://support.microsoft.com/help/4470999/db-backup-problems-to-sql-server-connector-for-azure-1-0-5-0).
> - **Ab Version 1.0.5.0 (mit einem Dateidatum vom September 2020) unterstützt der SQL Server-Connector das Filtern von Nachrichten sowie Wiederholungslogik für Netzwerkanforderungen.**
> - *Die alte Version des SQL Server-Connectors ist gleichzeitig Version: [1.0.5.0 (Version 15.0.300.96) mit einem Dateidatum vom Januar 2018](https://download.microsoft.com/download/8/0/9/809494F2-BAC9-4388-AD07-7EAF9745D77B/ENU/SQLServerConnectorforMicrosoftAzureKeyVault.msi)* . Führen Sie ein Upgrade auf die aktuelle Version des SQL Server-Connectors durch, wenn Probleme auftreten.

**Systemanforderungen:** Unterstützte SQL Server-Versionen:

- SQL Server 2019 RTM Enterprise (64-Bit)
- SQL Server 2017 RTM Enterprise (64-Bit)
- SQL Server 2016 RTM Enterprise (64-Bit)
- SQL Server 2014 RTM Enterprise (64-Bit)
- SQL Server 2012 SP2 Enterprise (64-Bit)
- SQL Server 2012 SP1 CU6 Enterprise (64-Bit)
- SQL Server 2008 R2 SP2 CU8 Enterprise (64-Bit)

Für die SQL Server-Versionen 2008 und 2012, die niedriger als die oben aufgeführten sind, muss der im folgenden KB-Artikel angegebene Patch installiert werden: [https://support2.microsoft.com/kb/2859713](https://support2.microsoft.com/kb/2859713).

Der SQL Server-Connector für Microsoft Azure Key Vault erfordert außerdem die .NET-Version 4.5.1 auf der VM für Microsoft SQL Server in Azure. Diese Bibliothek sollte installiert werden, bevor Sie den SQL Server-Connector installieren.

Überprüfen Sie, ob die für die ausgeführte SQL Server-Version geeignete Version der weitervertreibbaren Visual Studio C++-Komponente installiert ist:

- Installieren Sie für die SQL Server-Versionen 2008, 2008 R2, 2012 und 2014 die Version 2013 von Visual C++ Redistributable.

- Installieren Sie für SQL Server 2016 die Version 2015 von Visual C++ Redistributable.

## <a name="modify-windows-registry-steps"></a>Schritte zum Bearbeiten der Windows-Registrierung

Bearbeiten Sie Registrierungseinträge, um die Protokollierung von Fehlerereignissen und anderen Informationen im Anwendungsereignisprotokoll von Windows für den SQL Server-Connector zu aktivieren.

> [!CAUTION]
> Befolgen Sie die Schritte in diesem Abschnitt sorgfältig und auf eigenes Risiko. Wenn Ihnen beim Bearbeiten der Registrierung ein Fehler unterläuft, kann dies zu schwerwiegenden Problemen führen. Bevor Sie die Registrierung bearbeiten, [sichern Sie die Registrierung für eine mögliche Wiederherstellung](https://support.microsoft.com/help/322756), sollten Probleme auftreten.

1. Es gibt zwei Möglichkeiten, den Registrierungs-Editor unter Windows 10 zu öffnen:
    - Geben Sie im Suchfeld in der Taskleiste **regedit** ein. Wählen Sie dann das oberste Ergebnis für den Registrierungs-Editor (Desktop-App) aus.

    ![ekm regedit open](../../../relational-databases/security/encryption/media/ekm-registry/ekm-regedit-open.png "ekm regedit open")
    - Drücken und halten Sie die Schaltfläche „Start“, oder klicken Sie mit der rechten Maustaste darauf. Klicken Sie dann auf „Ausführen“. Geben Sie im Dialogfeld **regedit** ein, und klicken Sie auf **OK**.

   ![ekm regedit start](../../../relational-databases/security/encryption/media/ekm-registry/ekm-regedit-start.png "ekm regedit start")

1. Navigieren Sie zum Registrierungsschlüssel:

    **HKLM\SOFTWARE\Microsoft\SQL Server Cryptographic Provider\Azure Key Vault\\** .

    ![ekm regedit akv](../../../relational-databases/security/encryption/media/ekm-registry/ekm-regedit-akv.png "ekm regedit akv")  

1. Fügen Sie unter **Azure Key Vault** einen neuen Schlüssel namens `Log` hinzu:

    **HKLM\SOFTWARE\Microsoft\SQL Server Cryptographic Provider\Azure Key Vault\\Log\\**

    ![ekm regedit akv log](../../../relational-databases/security/encryption/media/ekm-registry/ekm-regedit-akv-log.png "ekm regedit akv log.png")  

1. Fügen Sie unter dem Schlüssel **Log** einen DWORD-Wert (32-Bit) namens `Level` hinzu:

    **HKLM\SOFTWARE\Microsoft\SQL Server Cryptographic Provider\Azure Key Vault\\Log\\**

    ![ekm regedit akv log dword](../../../relational-databases/security/encryption/media/ekm-registry/ekm-regedit-akv-log-dword.png "ekm regedit akv log dword")  

1. Legen Sie den Wert für DWORD auf eine geeignete Protokollebene fest (0,1,2):
   1. 0 (Info) – **Standardeinstellung**
   1. 1 (Fehler)
   1. 2 (Kein Protokoll)

   ![ekm regedit akv log level](../../../relational-databases/security/encryption/media/ekm-registry/ekm-regedit-akv-log-level.png "ekm regedit akv log level")  

Die in diesem Artikel beschriebenen Registrierungseinträge können unter diesem Schlüssel gefunden werden:

```console
\Computer
    \HKEY_LOCAL_MACHINE
       \SOFTWARE
          \Microsoft
             \SQL Server Cryptographic Provider
                \Azure Key Vault
                   \Log\
                      <Level>
```

Optional können Sie die Befehlszeile verwenden, um den Schlüssel zu generieren:

```cmd
--Create the logging parameter using (Administrator) Command Line:
REG ADD "HKLM\SOFTWARE\Microsoft\SQL Server Cryptographic Provider\Azure Key Vault\Log" /v Level /t REG_DWORD /d 1 

--Validate the new registry entry
REG QUERY "HKLM\SOFTWARE\Microsoft\SQL Server Cryptographic Provider\Azure Key Vault\Log" /v Level
```

Fehlende Nachrichten in Ereignisprotokolleinträgen für eine Anwendung können ebenfalls mithilfe eines Registrierungseintrags behoben werden. Das Ereignisprotokoll kann die folgende Nachricht enthalten: `The description for Event ID 0 from source SQL Server Connector for Microsoft Azure Key Vault cannot be found...`.  

```cmd
--Create the registry entry to enable missing messages (this works with any version)
REG ADD "HKLM\SYSTEM\ControlSet001\Services\EventLog\Application\SQL Server Connector for Microsoft Azure Key Vault" /v EventMessageFile /t REG_EXPAND_SZ /d "C:\Program Files\SQL Server Connector for Microsoft Azure Key Vault\Microsoft.AzureKeyVaultService.EKM.dll"

--Validate the new registry entry
REG QUERY "HKLM\SYSTEM\ControlSet001\Services\EventLog\Application\SQL Server Connector for Microsoft Azure Key Vault" /v EventMessageFile
```

## <a name="related-articles"></a>Verwandte Artikel

- Weitere Beispielskripts finden Sie im Blog zum Thema [SQL Server Transparent Data Encryption und erweiterbare Schlüsselverwaltung mit Azure Key Vault](https://techcommunity.microsoft.com/t5/sql-server/intro-sql-server-transparent-data-encryption-and-extensible-key/ba-p/1427549).
- [Erweiterbare Schlüsselverwaltung (Extensible Key Management, EKM)](extensible-key-management-ekm.md)  
- [Erweiterbare Schlüsselverwaltung mit Azure Key Vault (SQL Server)](extensible-key-management-using-azure-key-vault-sql-server.md)
- [SQL Server-Connector – Verwaltung und Problembehandlung](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md)