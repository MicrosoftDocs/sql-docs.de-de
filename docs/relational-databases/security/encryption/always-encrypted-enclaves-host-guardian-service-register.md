---
title: Registrieren des Computers beim Host-Überwachungsdienst
description: Registrieren Sie den SQL Server-Computer beim Host-Überwachungsdienst für Always Encrypted mit Secure Enclaves.
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
author: rpsqrd
ms.author: ryanpu
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 64971ecd3ed3a821ca823ad3cc5d0d05d08669f4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031392"
---
# <a name="register-computer-with-host-guardian-service"></a>Registrieren des Computers beim Host-Überwachungsdienst

[!INCLUDE [sqlserver2019-windows-only](../../../includes/applies-to-version/sqlserver2019-windows-only.md)]

In diesem Artikel wird beschrieben, wie Sie [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Computer für den Nachweis beim Host-Überwachungsdienst (Host Guardian Service, HGS) registrieren.

> [!NOTE]
> Der Registrierungsprozess einer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Instanz beim HGS erfordert eine Zusammenarbeit zwischen dem HGS-Administrator und dem [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Computeradministrator. Weitere Informationen finden Sie unter [Bereitstellen des Host-Überwachungsdiensts für SQL Server](always-encrypted-enclaves-host-guardian-service-plan.md#roles-and-responsibilities-when-configuring-attestation-with-hgs).

Bevor Sie beginnen, vergewissern Sie sich, dass Sie mindestens einen HGS-Computer bereitgestellt und den HGS-Nachweisdienst eingerichtet haben.
Weitere Informationen finden Sie unter [Bereitstellen des Host-Überwachungsdiensts für [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]](./always-encrypted-enclaves-host-guardian-service-deploy.md).

## <a name="step-1-install-the-attestation-client-components"></a>Schritt 1: Installieren der Nachweis-Clientkomponenten

> [!NOTE]
> Dieser Schritt sollte vom [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Computeradministrator ausgeführt werden.

Damit ein SQL-Client überprüfen kann, ob er mit einem vertrauenswürdigen [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Computer kommuniziert, muss sich der [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Computer erfolgreich beim Host-Überwachungsdienst beglaubigen.
Der Nachweisprozess wird von einer optionalen Windows-Komponente namens HGS-Client verwaltet.
Mithilfe der folgenden Schritte können Sie diese Komponente installieren und mit dem Beglaubigen beginnen.

1. Stellen Sie sicher, dass der [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Computer die [Voraussetzungen erfüllt, die im HGS-Planungsdokument beschrieben sind](./always-encrypted-enclaves-host-guardian-service-plan.md#prerequisites).

2. Führen Sie den folgenden Befehl in einer PowerShell-Konsole mit erhöhten Rechten aus, um das Host Guardian Hyper-V-Unterstützungsfeature zu installieren, das den HGS-Client und die Nachweiskomponenten enthält.

    ```powershell
    Enable-WindowsOptionalFeature -Online -FeatureName HostGuardian -All
    ```

3. Führen Sie einen Neustart aus, um die Installation abzuschließen.

## <a name="step-2-verify-virtualization-based-security-is-running"></a>Schritt 2: Überprüfen, ob die virtualisierungsbasierte Sicherheit ausgeführt wird

> [!NOTE]
> Dieser Schritt sollte vom [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Computeradministrator ausgeführt werden.

Wenn Sie die Hyper-V-Unterstützung des Host-Überwachungsdiensts installieren, wird die virtualisierungsbasierte Sicherheit (VSB) automatisch konfiguriert und aktiviert.
Die Enklaven für [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] Always Encrypted werden von der VBS-Umgebung geschützt und innerhalb der VBS-Umgebung ausgeführt.
VBS wird möglicherweise nicht gestartet, wenn auf dem Computer kein IOMMU-Gerät installiert und aktiviert ist.
Um zu überprüfen, ob VBS ausgeführt wird, öffnen Sie das Tool Systeminformationen, indem Sie `msinfo32.exe` ausführen und die `Virtualization-based security`-Elemente gegen Ende der Systemzusammenfassung suchen.

![Screenshot der Systeminformationen mit Status der virtualisierungsbasierten Sicherheit und Konfiguration](./media/always-encrypted-enclaves/msinfo32-vbs-status.png)

Das erste zu prüfende Element ist `Virtualization-based security`, das die folgenden drei Werte aufweisen kann:

- `Running` bedeutet, dass VBS ordnungsgemäß konfiguriert ist und erfolgreich gestartet werden konnte. Wenn der Computer diesen Status anzeigt, können Sie mit Schritt 3 fortfahren.
- `Enabled but not running` bedeutet, dass VBS für die Ausführung konfiguriert ist, die Hardware jedoch nicht den Mindestanforderungen an die Sicherheit genügt, die für die Ausführung von VBS erforderlich sind. Möglicherweise müssen Sie die Konfiguration der Hardware in BIOS oder UEFI ändern, um optionale Prozessorfeatures wie ein IOMMU zu aktivieren. Wenn die Hardware die erforderlichen Features tatsächlich nicht unterstützt, müssen Sie möglicherweise die VBS-Sicherheitsanforderungen herabsetzen. Setzen Sie das Lesen dieses Abschnitts fort, um mehr zu erfahren.
- `Not enabled` bedeutet, dass VBS nicht für die Ausführung konfiguriert ist. Das Hyper-V-Unterstützungsfeature des Host-Überwachungsdiensts aktiviert VBS automatisch, daher wird empfohlen, Schritt 1 zu wiederholen, wenn dieser Status angezeigt wird.

Wenn VSB nicht auf dem Computer ausgeführt wird, überprüfen Sie die `Virtualization-based security`-Eigenschaften. Vergleichen Sie die Werte im Element `Required Security Properties` mit den Werten im Element `Available Security Properties`.
Die erforderlichen Eigenschaften müssen sich mit den verfügbaren Sicherheitseigenschaften für VSB decken oder eine Teilmenge von ihnen darstellen, damit VBS ausgeführt werden kann.

Im Kontext der Beglaubigung von [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Enklaven kommt den Sicherheitseigenschaften folgende Bedeutung zu:

- `Base virtualization support` ist immer erforderlich, da es die Mindestanforderungen an die Hardwarefeatures darstellt, die zum Ausführen eines Hypervisors erforderlich sind.
- `Secure Boot` wird für [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] Always Encrypted empfohlen, ist aber nicht erforderlich. Secure Boot schützt vor Rootkits, da es einen von Microsoft signierten Bootloader vorschreibt, der unmittelbar nach der UEFI-Initialisierung ausgeführt werden muss. Wenn Sie TPM-Nachweis (Trusted Platform Module) verwenden, wird die Aktivierung von Secure Boot ermittelt und durchgesetzt, unabhängig davon, ob für VBS erforderliches Secure Boot konfiguriert ist oder nicht.
- `DMA Protection` wird für [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] Always Encrypted empfohlen, ist aber nicht erforderlich. Der DMA-Schutz verwendet IOMMU, um VBS und Enklavenspeicher vor Angriffen über direkten Speicherzugriff zu schützen. In einer Produktionsumgebung sollten Sie stets Computer mit DMA-Schutz verwenden. In einer Dev/Test-Umgebung ist es akzeptabel, DMA-Schutz nicht vorzuschreiben. Wenn die [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Instanz virtualisiert ist, steht höchstwahrscheinlich kein DMA-Schutz zur Verfügung, und die Anforderung muss entfernt werden, damit VBS ausgeführt werden kann. Überprüfen Sie das [Vertrauensmodell](./always-encrypted-enclaves-host-guardian-service-plan.md#trust-model) auf Informationen zu den verringerten Sicherheitsgarantien bei der Ausführung auf einem virtuellen Computer.

Bevor Sie die erforderlichen Sicherheitsfeatures für VBS herabsetzen, wenden Sie sich an Ihren OEM oder Clouddienstanbieter, um zu überprüfen, ob es eine Möglichkeit gibt, die fehlenden Plattformanforderungen in UEFI oder BIOS zu aktivieren (z. B. durch Aktivieren von Secure Boot, Intel VT-d oder AMD IOV).

Führen Sie den folgenden Befehl in einer PowerShell-Konsole mit erhöhten Rechten aus, um die erforderlichen Plattformsicherheitsfeatures für VBS zu ändern:

```powershell
# Value 0 = No security features required (use this for Azure VMs)
# Value 1 = Only Secure Boot is required
# Value 2 = Only DMA protection is required (default configuration)
# Value 3 = Both Secure Boot and DMA protection are required
Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard -Name RequirePlatformSecurityFeatures -Value 0
```

Führen Sie nach dem Ändern der Registrierung einen Neustart des [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Computers durch, und überprüfen Sie erneut, ob VBS ausgeführt wird.

Wenn der Computer von Ihrem Unternehmen verwaltet wird, setzen Gruppenrichtlinien oder Microsoft Endpoint Manager nach dem Neustart möglicherweise alle Änderungen außer Kraft, die Sie an diesen Registrierungsschlüsseln vorgenommen haben.
Wenden Sie sich an Ihren IT-Helpdesk, um zu erfahren, ob sie Richtlinien zur Verwaltung Ihrer VBS-Konfiguration bereitstellen.

## <a name="step-3-configure-the-attestation-url"></a>Schritt 3: Konfigurieren der Nachweis-URL

> [!NOTE]
> Dieser Schritt sollte vom [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Computeradministrator ausgeführt werden.

Als Nächstes konfigurieren Sie den [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Computer mit der URL für den HGS-Nachweisdienst, die Sie vom HGS-Administrator erhalten haben.

Aktualisieren Sie in einer PowerShell-Konsole mit erhöhten Rechten den folgenden Befehl, und führen Sie ihn aus, um die Nachweis-URL zu konfigurieren.

- Ersetzen Sie `hgs.bastion.local` durch den Namen des HGS-Clusters.
- Sie können `Get-HgsServer` auf einem beliebigen HGS-Computer ausführen, um den Clusternamen abzurufen.
- Die Nachweis-URL sollte immer auf `/Attestation` enden.
- SQL Server nutzt die wichtigsten Schutzfeatures von HGS nicht, geben Sie daher eine beliebige Pseudo-URL wie `http://localhost` für `-KeyProtectionServerUrl` an.

```powershell
Set-HgsClientConfiguration -AttestationServerUrl "https://hgs.bastion.local/Attestation" -KeyProtectionServerUrl "http://localhost"
```

Wenn Sie diesen Computer zuvor noch nicht bei HGS registriert hatten, meldet der Befehl einen Nachweisfehler. Dieses Ergebnis ist normal.

Das `AttestationMode`-Feld in der Ausgabe des Cmdlets gibt an, welcher Nachweismodus von HGS verwendet wird.

Fahren Sie mit [Schritt 4A](#step-4a-register-a-computer-in-tpm-mode) fort, um den Computer im TPM-Modus zu registrieren, oder mit [Schritt 4B](#step-4b-register-a-computer-in-host-key-mode), um den Computer im Hostschlüsselmodus zu registrieren.

## <a name="step-4a-register-a-computer-in-tpm-mode"></a>Schritt 4A: Registrieren eines Computers im TPM-Modus

> [!NOTE]
> Dieser Schritt wird gemeinsam vom [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Computeradministrator und dem HGS-Administrator durchgeführt. Ausführlichere Informationen finden Sie in den Hinweisen unten.

### <a name="prepare"></a>Vorbereiten

> [!NOTE]
> Diese Aktion sollte vom [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Computeradministrator ausgeführt werden.

In diesem Schritt erfassen Sie Informationen zum TPM-Status des Computers und registrieren ihn bei HGS.

Wenn der HGS-Nachweisdienst für die Verwendung des Hostschlüsselmodus konfiguriert ist, fahren Sie stattdessen mit [Schritt 4B](#step-4b-register-a-computer-in-host-key-mode) fort.

Bevor Sie mit dem Erfassen von TPM-Messungen beginnen, vergewissern Sie sich, dass Sie in einer als funktionierend bekannten Konfiguration des [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Computers arbeiten.
Auf dem Computer muss sämtliche erforderliche Hardware installiert sein, außerdem müssen die neuesten Firmware- und Softwareupdates installiert sein.
HGS misst die Computer für den Nachweis an dieser Baseline. Es ist daher wichtig, beim Erfassen der TPM-Messungen den sichersten möglichen, beabsichtigten Status zugrunde zu legen.

Für den TPM-Nachweis werden drei Datendateien gesammelt, von denen einige wiederverwendet werden können, wenn Sie über Computer mit identischer Konfiguration verfügen.

| Nachweisartefakt | Gegenstand der Messung | Eindeutigkeit |
| -------------------- | ---------------- | ---------- |
| Plattformbezeichner  | Der öffentliche Endorsement Key im TPM des Computers und das Endorsement Key-Zertifikat vom TPM-Hersteller. | 1 pro Computer |
| TPM-Baseline | Die Plattform-Steuerungsregister (Platform Control Registers, PCRs) im TPM, die die während des Bootvorgangs geladene Firmware- und Betriebssystemkonfiguration messen. Beispiele: Secure Boot-Status und Angabe, ob Crashdumps verschlüsselt werden. | Eine Baseline pro eindeutiger Computerkonfiguration (identische Hard- und Software kann sich auf die gleiche Baseline stützen) |
| Codeintegritätsrichtlinie | Die [Windows Defender-Anwendungssteuerungs](/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control)-Richtlinie, der Sie den Schutz der Computer anvertrauen | Eine pro eindeutiger CI-Richtlinie, die auf den Computern bereitgestellt ist. |

Von jedem Nachweisartefakt können in HGS mehrere Instanzen konfiguriert werden, um eine in Hard- und Software gemischte Flotte zu unterstützen.
HGS schreibt für beglaubigte Computer lediglich vor, dass einer Richtlinie aus jeder Richtlinienkategorie genügt wird.
Wenn Sie beispielsweise drei TPM-Baselines bei HGS registriert haben, ist es ausreichend, wenn die Computermessungen mit einer dieser Baselines übereinstimmen, um der Richtlinienanforderung zu genügen.

### <a name="configure-a-code-integrity-policy"></a>Konfigurieren einer Codeintegritätsrichtlinie

> [!NOTE]
> Die unten stehenden Schritte sollten vom [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Computeradministrator ausgeführt werden.

Für HGS ist es erforderlich, dass jeder Computer, der im TPM-Modus beglaubigt wird, eine Windows Defender Application Control-Richtlinie (WDAD) aufweist.
WDAC-Codeintegritätsrichtlinien beschränken, welche Software auf einem Computer ausgeführt werden kann, indem sie jeden Prozess, der versucht, Code auszuführen, anhand einer Liste vertrauenswürdiger Herausgeber und Dateihashes überprüft.
Für den [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Anwendungsfall werden die Enklaven durch virtualisierungsbasierte Sicherheit geschützt und können nicht vom Hostbetriebssystem aus geändert werden. Daher wirkt sich die Strenge der WDAC-Richtlinie nicht auf die Sicherheit verschlüsselter Abfragen aus.
Daher wird empfohlen, eine Richtlinie für den Überwachungsmodus auf den [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Computern bereitzustellen, um die Nachweisanforderung zu erfüllen, ohne dem System zusätzliche Einschränkungen aufzuerlegen.

Wenn Sie bereits eine benutzerdefinierte WDAC-Codeintegritätsrichtlinie auf den Computern verwenden, um die Konfiguration des Betriebssystems zu festigen, können Sie mit [Erfassen von TPM-Nachweisinformationen](#collect-tpm-attestation-information) fortfahren.

1. Vordefinierte Beispielrichtlinien stehen in den Betriebssystemen Windows Server 2019, Windows 10, Version 1809, und in späteren Betriebssystemen zur Verfügung. Mit der `AllowAll`-Richtlinie kann jede Software ohne Einschränkungen auf dem Computer ausgeführt werden. Konvertieren Sie die Richtlinie in ein von Betriebssystem und HGS verstandenes Binärformat, um sie zu verwenden. Führen Sie in einer PowerShell-Konsole mit erhöhten Rechten die folgenden Befehle aus, um die `AllowAll`-Richtlinie zu kompilieren:

    ```powershell
    # We are changing the policy to disable enforcement and user mode code protection before compiling
    $temppolicy = "$HOME\Desktop\allowall_edited.xml"
    Copy-Item -Path "$env:SystemRoot\schemas\CodeIntegrity\ExamplePolicies\AllowAll.xml" -Destination $temppolicy
    Set-RuleOption -FilePath $temppolicy -Option 0 -Delete
    Set-RuleOption -FilePath $temppolicy -Option 3

    ConvertFrom-CIPolicy -XmlFilePath $temppolicy -BinaryFilePath "$HOME\Desktop\allowall_cipolicy.bin"
    ```

2. Befolgen Sie die Anweisungen im [Windows Defender Application Control-Bereitstellungshandbuch](/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control-deployment-guide), um die `allowall_cipolicy.bin`-Datei mithilfe von [Gruppenrichtlinien](/windows/security/threat-protection/windows-defender-application-control/deploy-windows-defender-application-control-policies-using-group-policy) auf den [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Computern bereitzustellen. Verwenden Sie für Arbeitsgruppencomputer denselben Prozess mit dem Editor für lokale Gruppenrichtlinien (`gpedit.msc`).

3. Führen Sie `gpupdate /force` auf den [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Computern aus, um die neue Codeintegritätsrichtlinie zu konfigurieren, und starten Sie die Computer dann neu, um die Richtlinie anzuwenden.

### <a name="collect-tpm-attestation-information"></a>Erfassen von TPM-Nachweisinformationen

> [!NOTE]
> Die unten stehenden Schritte sollten vom [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Computeradministrator ausgeführt werden.

Wiederholen Sie die folgenden Schritte für jeden [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Computer, der mit HGS beglaubigt werden soll:

1. Wenn sich der Computer in einem als funktionierend bekannten Zustand befindet, führen Sie die folgenden Befehle in PowerShell aus, um die TPM-Nachweisinformationen zu erfassen:

    ```powershell
    # Collects the TPM EKpub and EKcert
    $name = $env:computername
    $path = "$HOME\Desktop"
    (Get-PlatformIdentifier -Name $name).Save("$path\$name-EK.xml")

    # Collects the TPM baseline (current PCR values)
    Get-HgsAttestationBaselinePolicy -Path "$path\$name.tcglog" -SkipValidation

    # Collects the applied CI policy, if one exists
    Copy-Item -Path "$env:SystemRoot\System32\CodeIntegrity\SIPolicy.p7b" -Destination "$path\$name-CIpolicy.bin"
    ```

2. Geben Sie die drei Nachweisdateien für den HGS-Administrator frei. 

### <a name="register-the-sql-server-computer-with-hgs"></a>Registrieren des SQL Server-Computers beim HGS

> [!NOTE]
> Die unten stehenden Schritte sollten vom HGS-Administrator ausgeführt werden.

Wiederholen Sie die folgenden Schritte für jeden [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Computer, der mit HGS beglaubigt werden soll:

1. Kopieren Sie die Nachweisdateien, die Sie vom [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Computeradministrator erhalten haben, auf den HGS-Server. 
2. Führen Sie auf dem HGS-Server die folgenden Befehle in einer PowerShell-Konsole mit erhöhten Rechten aus, um den [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Computer zu registrieren:

    ```powershell
    # TIP: REMEMBER TO CHANGE THE FILENAMES
    # Registers the unique TPM with HGS (required for every computer)
    Add-HgsAttestationTpmHost -Path "C:\temp\SQL01-EK.xml"

    # Registers the TPM baseline (required ONCE for each unique hardware and software configuration)
    Add-HgsAttestationTpmPolicy -Name "MyHWSoftwareConfig" -Path "C:\temp\SQL01.tcglog"

    # Registers the CI policy (required ONCE for each unique CI policy)
    Add-HgsAttestationCiPolicy -Name "AllowAll" -Path "C:\temp\SQL01-CIpolicy.bin"
    ```

    > [!TIP]
    > Wenn beim Registrieren des eindeutigen TPM-Bezeichners ein Fehler auftritt, vergewissern Sie sich, dass Sie auf dem von Ihnen verwendeten HGS-Computer [die TPM-Zwischen- und Stammzertifikate importiert](./always-encrypted-enclaves-host-guardian-service-deploy.md#switch-to-tpm-attestation) haben

Über den Plattformbezeichner, die TPM-Baseline und die Codeintegritätsrichtlinie hinaus gibt es integrierte Richtlinien, die von HGS konfiguriert und durchgesetzt werden, und die Sie möglicherweise ändern müssen.
Diese integrierten Richtlinien werden ausgehend von der TPM Baseline gemessen, die Sie auf dem Server erfasst haben, und stellen eine Vielzahl von Sicherheitseinstellungen dar, die zum Schutz des Computers aktiviert sein sollten.
Wenn Sie über Computer verfügen, auf denen kein IOMMU zum Schutz vor DMA-Angriffen vorhanden ist (beispielsweise eine VM), müssen Sie die IOMMU-Richtlinie deaktivieren.

Um die IOMMU-Anforderung zu deaktivieren, führen Sie den folgenden Befehl auf dem HGS-Server aus:

```powershell
Disable-HgsAttestationPolicy Hgs_IommuEnabled
```

> [!NOTE]
> Wenn Sie die IOMMU-Richtlinie deaktivieren, sind IOMMUs für keinen der Computer, die mithilfe von HGS beglaubigt werden, erforderlich.
> Es ist nicht möglich, die IOMMU-Richtlinie nur für einen einzelnen Computer zu deaktivieren.

Sie können die Liste der registrierten TPM-Hosts und -Richtlinien mit den folgenden PowerShell-Befehlen überprüfen:

```powershell
Get-HgsAttestationTpmHost
Get-HgsAttestationTpmPolicy
```

## <a name="step-4b-register-a-computer-in-host-key-mode"></a>Schritt 4B: Registrieren eines Computers im Hostschlüsselmodus

> [!NOTE]
> Dieser Schritt wird gemeinsam vom [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Computeradministrator und dem HGS-Administrator durchgeführt. Ausführlichere Informationen finden Sie in den Hinweisen unten.

Dieser Schritt führt Sie durch den Prozess zum Generieren eines eindeutigen Schlüssels für den Host und zu seiner Registrierung bei HGS.
Wenn der HGS-Nachweisdienst für die Verwendung des TPM-Modus konfiguriert ist, befolgen Sie stattdessen die Anweisungen in [Schritt 4A](#step-4a-register-a-computer-in-tpm-mode).

### <a name="generate-a-key-for-a-ssnoversion-md-computer"></a>Generieren eines Schlüssels für einen [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Computer

> [!NOTE]
> Dieser Teil sollte vom [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Computeradministrator ausgeführt werden.

Der Hostschlüsselnachweis funktioniert durch die Generierung eines asymmetrischen Schlüsselpaars auf dem [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Computer und die anschließende Übergabe der öffentlichen Hälfte dieses Schlüssels an den HGS.

Wiederholen Sie die folgenden Schritte für jeden [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Computer, der mit HGS beglaubigt werden soll:

1. Um das Schlüsselpaar zu generieren, führen Sie den folgenden Befehl in einer PowerShell-Konsole mit erhöhten Rechten aus:

    ```powershell
    Set-HgsClientHostKey
    Get-HgsClientHostKey -Path "$HOME\Desktop\$env:computername-key.cer"
    ```

    Wenn Sie bereits einen Hostschlüssel erstellt haben und ein neues Schlüsselpaar generieren möchten, verwenden Sie stattdessen den folgenden Befehl:

    ```powershell
    Remove-HgsClientHostKey
    Set-HgsClientHostKey
    Get-HgsClientHostKey -Path "$HOME\Desktop\$env:computername-key.cer"
    ```

2. Geben Sie die Zertifikatdatei für den HGS-Administrator frei.

### <a name="register-the-sql-server-computer-with-hgs"></a>Registrieren des SQL Server-Computers beim HGS

> [!NOTE]
> Die unten stehenden Schritte sollten vom HGS-Administrator ausgeführt werden.

Wiederholen Sie die folgenden Schritte für jeden [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Computer, der mit HGS beglaubigt werden soll:

1. Kopieren Sie die Zertifikatdatei, die Sie vom [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Computeradministrator erhalten haben, auf einen HGS-Server.
2. Führen Sie den folgenden Befehl in einer PowerShell-Konsole mit erhöhten Rechten aus, um den [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Computer zu registrieren:

    ```powershell
    Add-HgsAttestationHostKey -Name "YourComputerName" -Path "C:\temp\yourcomputername.cer"
   ```

## <a name="step-5-confirm-the-host-can-attest-successfully"></a>Schritt 5: Bestätigen, dass der Host erfolgreich beglaubigen kann

> [!NOTE]
> Dieser Schritt sollte vom [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Computeradministrator ausgeführt werden.

Nachdem Sie den [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Computer bei HGS registriert haben ([Schritt 4A](#step-4a-register-a-computer-in-tpm-mode) für den TPM-Modus, [Schritt 4B](#step-4b-register-a-computer-in-host-key-mode) für den Hostschlüsselmodus), sollten Sie überprüfen, ob er erfolgreich beglaubigen kann.

Sie können die Konfiguration des HGS-Nachweisclients überprüfen und jederzeit mithilfe von [Get-HgsClientConfiguration](/powershell/module/hgsclient/get-hgsclientconfiguration?view=win10-ps&preserve-view=true) einen Beglaubigungsversuch durchführen.

Die Ausgabe des Befehls sieht etwa wie folgt aus:

```
PS C:\> Get-HgsClientConfiguration


IsHostGuarded                  : True
Mode                           : HostGuardianService
KeyProtectionServerUrl         : http://localhost
AttestationServerUrl           : http://hgs.bastion.local/Attestation
AttestationOperationMode       : HostKey
AttestationStatus              : Passed
AttestationSubstatus           : NoInformation
FallbackKeyProtectionServerUrl :
FallbackAttestationServerUrl   :
IsFallbackInUse                : False
```

Die zwei wichtigsten Felder in der Ausgabe sind `AttestationStatus`, das Sie darüber informiert, ob der Computer den Nachweis erbracht hat, und `AttestationSubStatus`, das erläutert, welchen Richtlinien der Computer nicht genügt hat, falls der Computer den Nachweis nicht erbringen konnte.

Die häufigsten Werte, die für `AttestationStatus` angezeigt werden können, werden unten erläutert:

| `AttestationStatus` | Erklärung |
| ----------------- | ----------- |
| Abgelaufen | Der Host wurde zuvor erfolgreich beglaubigt, das ausgestellte Integritätszertifikat ist jedoch abgelaufen. Stellen Sie sicher, dass die Systemzeit von Host und HGS synchron sind. |
| `InsecureHostConfiguration` | Der Computer hat mindestens einer der auf dem HGS-Server konfigurierten Nachweisrichtlinien nicht genügt. Weitere Informationen finden Sie unter `AttestationSubStatus`. |
| NotConfigured | Auf dem Computer wurde keine Nachweis-URL konfiguriert. [Konfigurieren der Nachweis-URL](#step-3-configure-the-attestation-url) |
| Erfolgreich | Der Computer hat den Nachweis erbracht und wird für die Ausführung von [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Enklaven als vertrauenswürdig angesehen. |
| `TransientError` | Der Nachweisversuch ist aufgrund eines vorübergehenden Fehlers fehlgeschlagen. Dieser Fehler bedeutet normalerweise, dass ein Fehler beim Verbindungsaufbau mit dem HGS über das Netzwerk aufgetreten ist. Überprüfen Sie die Netzwerkverbindung, und stellen Sie sicher, dass der Computer den Namen des HGS-Dienstanbieters auflösen und an ihn routen kann. |
| `TpmError` | Das TPM-Gerät des Computers hat während des Nachweisversuchs einen Fehler gemeldet. Überprüfen Sie die TPM-Protokolle, um weitere Informationen zu erhalten. Das Problem kann möglicherweise durch Löschen des TPMs gelöst werden, achten Sie aber darauf, BitLocker und andere Dienste, die sich auf das TPM stützen, zu löschen, bevor Sie das TPM löschen. |
| `UnauthorizedHost` | Der Hostschlüssel ist HGS nicht bekannt. Befolgen Sie die Anweisungen in [Schritt 4B](#step-4b-register-a-computer-in-host-key-mode), um den Computer bei HGS zu registrieren. |

Wenn der `AttestationStatus``InsecureHostConfiguration` anzeigt, wird das Feld `AttestationSubStatus` mit dem Namen einer oder mehrerer Richtlinien aufgefüllt, bei denen ein Fehler aufgetreten ist.
In der Tabelle unten werden die gängigsten Werte und die Vorgehensweise zum Beheben der Fehler erläutert.

| AttestationSubStatus | Bedeutung und Vorgehensweise |
| -------------------- | ---------------------------- |
| CodeIntegrityPolicy | Die Codeintegritätsrichtlinie auf dem Computer ist nicht bei HGS registriert, *oder* der Computer verwendet zurzeit keine Codeintegritätsrichtlinie. Anleitungen finden Sie unter [Konfigurieren einer Codeintegritätsrichtlinie](#configure-a-code-integrity-policy). |
| DumpsEnabled | Der Computer ist so konfiguriert, dass Crashdumps zugelassen werden, aber die Hgs_DumpsEnabled-Richtlinie lässt keine Dumps zu. Deaktivieren Sie Absturzabbilder auf diesem Computer, oder deaktivieren Sie die Hgs_DumpsEnabled-Richtlinie, um fortzufahren. |
| FullBoot | Der Computer wird nach dem Standbymodus oder Ruhezustand fortgesetzt, was zu geänderten TPM-Messungen führt. Führen Sie einen Neustart des Computers durch, um saubere TPM-Messungen zu generieren. |
| HibernationEnabled | Der Computer ist dafür konfiguriert, den Ruhezustand mit unverschlüsselten Ruhezustandsdateien zuzulassen. Deaktivieren Sie den Ruhezustand auf dem Computer, um dieses Problem zu lösen. |
| HypervisorEnforcedCodeIntegrityPolicy | Der Computer ist nicht für die Verwendung einer Codeintegritätsrichtlinie konfiguriert. Überprüfen Sie „Gruppenrichtlinie“ oder „Richtlinien der lokalen Gruppe“ > „Computerkonfiguration“ > „Administrative Vorlagen“ > „System“ > „Device Guard“ > „Virtualisierungsbasierte Sicherheit aktivieren“ > „Virtualisierungsbasierter Schutz der Codeintegrität“. Dieses Richtlinienelement sollte den Status „aktiviert ohne UEFI-Sperre“ aufweisen. |
| Iommu | Auf diesem Computer ist kein IOMMU-Gerät aktiviert. Wenn es sich um einen physischen Computer handelt, aktivieren Sie IOMMU im UEFI-Konfigurationsmenü. Wenn es sich um einen virtuellen Computer handelt und kein IOMMU verfügbar ist, führen Sie `Disable-HgsAttestationPolicy Hgs_IommuEnabled` auf dem HGS-Server aus. |
| SecureBoot | Secure Boot ist auf diesem Computer nicht aktiviert. Aktivieren Sie Secure Boot im UEFI-Konfigurationsmenü, um diesen Fehler zu beheben. |
| VirtualSecureMode | Auf diesem Computer wird keine virtualisierungsbasierte Sicherheit ausgeführt. Befolgen Sie die Anweisungen in [Schritt 2: Überprüfen Sie, ob VSB auf dem Computer ausgeführt wird](#step-2-verify-virtualization-based-security-is-running). |

## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren der Secure Enclave-Instanz in SQL Server](always-encrypted-enclaves-configure-enclave-type.md)