---
title: Einrichten der erweiterbaren Schlüsselverwaltung für Transparent Data Encryption (TDE) mit Azure Key Vault
description: Installieren und Konfigurieren des SQL Server-Connectors für Azure Key Vault
ms.custom: seo-lt-2019
ms.date: 11/25/2020
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Extensible Key Management
- EKM, with key vault setup
- SQL Server Connector, setup
- SQL Server Connector
- TDE, AKV, EKM
ms.assetid: c1f29c27-5168-48cb-b649-7029e4816906
author: Rupp29
ms.author: arupp
ms.openlocfilehash: 83a1b1c7e923362678a013de24b211a9da70be0e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345378"
---
# <a name="set-up-sql-server-tde-extensible-key-management-by-using-azure-key-vault"></a>Einrichten der erweiterbaren Schlüsselverwaltung für SQL Server-TDE mit Azure Key Vault

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

In diesem Artikel installieren und konfigurieren Sie den [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Connector für Azure Key Vault.  
  
## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie Azure Key Vault mit Ihrer SQL Server-Instanz verwenden, müssen Sie sicherstellen, dass die folgenden Voraussetzungen erfüllt sind:  
  
- Sie benötigen ein Azure-Abonnement.
  
- Installieren Sie [Azure PowerShell Version 5.2.0 oder höher](/powershell/azure/).  

- Erstellen Sie eine Azure Active Directory-Instanz (Azure AD).

- Machen Sie sich anhand des Artikels [Erweiterbare Schlüsselverwaltung mit Azure Key Vault (SQL Server)](../../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md) mit den Speicherungsgrundlagen der erweiterbaren Schlüsselverwaltung (EKM) mit Azure Key Vault vertraut.  

- Installieren Sie die Version von Visual C++ Redistributable für Visual Studio, die auf der von Ihnen ausgeführten Version von SQL Server basiert:
  
  SQL Server-Version  | Version von Visual C++ Redistributable für Visual Studio
  ---------|---------
  2008, 2008 R2, 2012, 2014 | [Visual C++ Redistributable-Pakete für Visual Studio 2013](https://www.microsoft.com/download/details.aspx?id=40784)
  2016 | [Visual C++ Redistributable für Visual Studio 2015](https://www.microsoft.com/download/details.aspx?id=48145)

## <a name="step-1-set-up-an-azure-ad-service-principal"></a>Schritt 1: Einrichten eines Azure AD-Dienstprinzipals

Um Ihren SQL Server-Instanzen Zugriffsberechtigungen für Ihren Azure Key Vault zu erteilen, benötigen Sie ein Dienstprinzipalkonto in Azure AD.  
  
1. Melden Sie sich beim [Azure-Portal](https://ms.portal.azure.com/) an, und führen Sie einen der folgenden Schritte aus:

    - Wählen Sie die Schaltfläche **Azure Active Directory** aus.

      ![Screenshot des Bereichs „Azure-Dienste“](../../../relational-databases/security/encryption/media/ekm/ekm-part1-login-portal.png)

    - Wählen Sie **Weitere Dienste** aus, und geben Sie dann im Feld **Alle Dienste** **Azure Active Directory** ein.

      ![Screenshot des Bereichs „Alle Azure-Dienste“](../../../relational-databases/security/encryption/media/ekm/ekm-part1-select-aad.png)  

1. Registrieren Sie wie unten beschrieben eine Anwendung mit Azure Active Directory. (Eine ausführliche schrittweise Anleitung finden Sie im Abschnitt über das Einrichten einer Identität für die Anwendung im [Azure Key Vault-Blogbeitrag](/archive/blogs/kv/azure-key-vault-step-by-step).)

    a. Wählen Sie in Azure Active Directory im Bereich **Übersicht** die Option **App-Registrierungen** aus.

    ![Screenshot des Bereichs „Azure Active Directory-Übersicht“](../../../relational-databases/security/encryption/media/ekm/ekm-part1-aad-app-register.png)

    b. Wählen Sie im Bereich **App-Registrierungen** die Option **Neue Registrierung** aus.

    ![Screenshot des Bereichs „App-Registrierungen“](../../../relational-databases/security/encryption/media/ekm/ekm-part1-aad-new-registration.png)  

    c. Geben Sie im Bereich **Anwendung registrieren** den Benutzernamen für die App ein, und wählen Sie dann **Registrieren** aus.

    ![Screenshot des Bereichs „Anwendung registrieren“](../../../relational-databases/security/encryption/media/ekm/ekm-part1-aad-register.png)

    d. Wählen Sie im linken Bereich **Zertifikate und Geheimnisse** und dann **Neuer geheimer Clientschlüssel** aus.

    ![Screenshot des Bereichs „Zertifikate und Geheimnisse“](../../../relational-databases/security/encryption/media/ekm/ekm-part1-aad-certs-secrets.png)  

    e. Geben Sie unter **Geheimen Clientschlüssel hinzufügen** eine Beschreibung und eine entsprechende Ablaufzeit ein, und wählen Sie dann **Hinzufügen** aus.

    ![Screenshot des Abschnitts „Geheimen Clientschlüssel hinzufügen“](../../../relational-databases/security/encryption/media/ekm/ekm-part1-aad-add-secret.png)  

    f. Wählen Sie im Bereich **Zertifikate und Geheimnisse** unter **"Wert"** die Schaltfläche **Kopieren** neben dem Wert des geheimen Clientschlüssels aus, der zum Erstellen eines asymmetrischen Schlüssels in SQL Server verwendet werden soll.

    ![Screenshot des Geheimniswerts](../../../relational-databases/security/encryption/media/ekm/ekm-part1-aad-new-secret.png)  

    g. Wählen Sie im linken Bereich **Übersicht** aus, und kopieren Sie dann im Feld **Anwendungs-ID (Client)** den Wert, der zum Erstellen eines asymmetrischen Schlüssels in SQL Server verwendet werden soll.

    ![Screenshot des Werts „Anwendungs-ID (Client)“ im Bereich „Übersicht“](../../../relational-databases/security/encryption/media/ekm/ekm-part1-aad-appid.png)  

## <a name="step-2-create-a-key-vault"></a>Schritt 2: Erstellen eines Schlüsseltresors

Wählen Sie die Methode aus, mit der Sie einen Schlüsseltresor erstellen möchten.

## <a name="azure-portal"></a>[Azure portal](#tab/portal)

### <a name="create-a-key-vault-by-using-the-azure-portal"></a>Erstellen eines Schlüsseltresors über das Azure-Portal

Sie können das Azure-Portal verwenden, um den Schlüsseltresor zu erstellen und ihm anschließend einen Azure AD-Prinzipal hinzuzufügen.

1. Erstellen Sie eine Ressourcengruppe.

   Alle Azure-Ressourcen, die Sie über das Azure-Portal erstellen, müssen in einer Ressourcengruppe enthalten sein, die Sie für Ihren Schlüsseltresor anlegen. Der Ressourcenname in diesem Beispiel lautet *ContosoDevRG*. Wählen Sie Ihren eigenen Ressourcengruppen- und Schlüsseltresornamen, da alle Schlüsseltresornamen global eindeutig sein müssen.

   Geben Sie im Bereich **Ressourcengruppe erstellen** unter **Projektdetails** die Werte ein, und wählen Sie dann **Überprüfen + erstellen** aus.

      ![Screenshot des Bereichs „Ressourcengruppe erstellen“](../../../relational-databases/security/encryption/media/ekm/ekm-part2-create-resource-group.png)  

1. Erstellen eines Schlüsseltresors

    Wählen Sie im Bereich **Schlüsseltresor erstellen** die Registerkarte **Grundlagen** aus, geben Sie die entsprechenden Werte ein, und wählen Sie dann **Überprüfen + erstellen** aus.

    ![Screenshot des Bereichs „Schlüsseltresor erstellen“](../../../relational-databases/security/encryption/media/ekm/ekm-part2-create-key-vault.png)  

1. Wählen Sie im Bereich **Zugriffsrichtlinien** die Option **Zugriffsrichtlinie hinzufügen** aus.

    ![Screenshot des Links „Zugriffsrichtlinie hinzufügen“ im Bereich „Zugriffsrichtlinien“](../../../relational-databases/security/encryption/media/ekm/ekm-part2-add-access-policy.png)  

1. Führen Sie im Bereich **Zugriffsrichtlinie hinzufügen** die folgenden Schritte aus:
  
    a. Wählen Sie in der Dropdownliste **Anhand einer Vorlage konfigurieren (optional)** die Option **Schlüsselverwaltung** aus.

    b. Wählen Sie im linken Bereich die Registerkarte **Schlüsselberechtigungen** aus, und vergewissern Sie sich dann, dass die Kontrollkästchen **Abrufen**, **Auflisten**, **Schlüssel entpacken** und **Schlüssel packen** aktiviert sind.

    c. Wählen Sie **Hinzufügen**.

    ![Screenshot des Bereichs „Zugriffsrichtlinie hinzufügen“](../../../relational-databases/security/encryption/media/ekm/ekm-part2-access-policy-permission.png)

1. Wählen Sie im linken Bereich die Registerkarte **Prinzipal auswählen** aus, und gehen Sie dann wie folgt vor:

    a. Geben Sie im Bereich **Prinzipal** unter **Auswählen** die ersten Zeichen des Namens Ihrer Azure AD-Anwendung ein, und wählen Sie dann in der Ergebnisliste die Anwendung aus, die Sie hinzufügen möchten.

    ![Screenshot des Anwendungssuchfelds im Bereich „Prinzipal“](../../../relational-databases/security/encryption/media/ekm/ekm-part2-select-principal.png)  

    b. Wählen Sie die Schaltfläche **Auswählen** aus, um den Prinzipal zu Ihrem Schlüsseltresor hinzuzufügen.

    ![Screenshot der Schaltfläche „Auswählen“ im Bereich „Prinzipal“](../../../relational-databases/security/encryption/media/ekm/ekm-part2-save-principal.png)

    c. Wählen Sie unten links **Hinzufügen** aus, um die Änderungen zu speichern.

    ![Screenshot der Schaltfläche „Hinzufügen“ im Bereich „Zugriffsrichtlinie hinzufügen“](../../../relational-databases/security/encryption/media/ekm/ekm-part2-select-principal-new.png)

1. Wählen Sie im Bereich **Schlüsseltresor** die Option **Schlüssel** aus, und geben Sie einen Namen für den Schlüsseltresor ein. Verwenden Sie den Schlüsseltyp **RSA** und die RSA-Schlüsselgröße **2048**. Legen Sie die Werte für Aktivierungs- und Ablaufdatum entsprechend fest, und legen Sie **Aktiviert?** auf **Ja** fest.

   ![Screenshot des Bereichs „Schlüssel erstellen“](../../../relational-databases/security/encryption/media/ekm/ekm-part2-add-key-vault-key.png)  

1. Wählen Sie im Bereich **Zugriffsrichtlinien** die Option **Speichern** aus.
  
   ![Screenshot der Schaltfläche „Speichern“ im Bereich „Zugriffsrichtlinie hinzufügen“](../../../relational-databases/security/encryption/media/ekm/ekm-part2-save-access-policy.png)  

## <a name="powershell"></a>[PowerShell](#tab/powershell)

### <a name="create-a-key-vault-and-key-by-using-powershell"></a>Erstellen eines Schlüsseltresors und eines Schlüssels mithilfe von PowerShell

Der hier erstellte Schlüsseltresor und der hier erstellte Schlüssel werden von der SQL Server-Datenbank-Engine zum Schutz des Verschlüsselungsschlüssels verwendet.  
  
> [!IMPORTANT]
> Das Abonnement, in dem der Schlüsseltresor erstellt wird, muss sich in derselben Azure AD-Standardinstanz befinden, in der der Azure AD-Dienstprinzipal erstellt wurde. Wenn Sie eine von der Standardinstanz abweichende Azure AD-Instanz zum Erstellen eines Dienstprinzipals für den SQL Server-Connector verwenden möchten, müssen Sie die Azure AD-Standardinstanz in Ihrem Azure-Konto ändern, bevor Sie den Schlüsseltresor erstellen. Informationen zum Ändern der Azure AD-Standardinstanz in die von Ihnen gewünschte Instanz finden Sie im Abschnitt „Häufig gestellte Fragen“ unter [SQL Server-Connector – Verwaltung und Problembehandlung](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md#AppendixB).  
  
1. Installieren Sie mit dem folgenden Befehl [Azure PowerShell Version 5.2.0 oder höher](/powershell/azure/), und melden Sie sich bei dieser an:  
  
    ```powershell  
    Connect-AzAccount  
    ```  
  
    Die Anweisung gibt Folgendes zurück:  
  
    ```console  
    Environment           : AzureCloud  
    Account               : <account_name>  
    TenantId              : <tenant_id>  
    SubscriptionId        : <subscription_id>  
    CurrentStorageAccount :  
    ```  
  
    > [!NOTE]  
    > Wenn Sie über mehrere Abonnements verfügen und ein bestimmtes Abonnement zur Verwendung für den Tresor angeben möchten, verwenden Sie `Get-AzSubscription`, um die Abonnements anzuzeigen, und `Select-AzSubscription`, um das richtige Abonnement auszuwählen. Andernfalls sucht PowerShell standardmäßig ein Abonnement aus.  
  
1. Erstellen Sie eine Ressourcengruppe.

    Alle Azure-Ressourcen, die Sie mithilfe von PowerShell erstellen, müssen in Ressourcengruppen enthalten sein, die Sie für Ihren Schlüsseltresor anlegen. Der Ressourcenname in diesem Beispiel lautet *ContosoDevRG*. Wählen Sie Ihren eigenen Ressourcengruppen- und Schlüsseltresornamen, da alle Schlüsseltresornamen global eindeutig sein müssen.  
  
    ```powershell  
    New-AzResourceGroup -Name ContosoDevRG -Location 'East Asia'  
    ```  
  
    Die Anweisung gibt Folgendes zurück:  
  
    ```console
    ResourceGroupName: ContosoDevRG  
    Location         : eastasia  
    ProvisioningState: Succeeded  
    Tags             :
    ResourceId       : /subscriptions/<subscription_id>/  
                        resourceGroups/ContosoDevRG  
    ```  
  
    > [!NOTE]
    > Verwenden Sie für den Parameter `-Location` den Befehl `Get-AzureLocation`, um einen anderen Speicherort als den in diesem Beispiel verwendeten Speicherort anzugeben. Wenn Sie weitere Informationen hierzu benötigen, geben Sie **Get-Help Get-AzureLocation** ein.  
  
1. Erstellen eines Schlüsseltresors

    Das `New-AzKeyVault` -Cmdlet benötigt einen Ressourcengruppennamen, einen Schlüsseltresornamen und einen geografischen Standort. Führen Sie z. B. für einen Schlüsseltresor mit dem Namen `ContosoEKMKeyVault` Folgendes aus:  
  
    ```powershell  
    New-AzKeyVault -VaultName 'ContosoEKMKeyVault' `  
       -ResourceGroupName 'ContosoDevRG' -Location 'East Asia'  
    ```  
  
    Notieren Sie den Namen Ihres Schlüsseltresors.  
  
    Die Anweisung gibt Folgendes zurück:

    ```console
    Vault Name                       : ContosoEKMKeyVault  
    Resource Group Name              : ContosoDevRG  
    Location                         : East Asia  
    ResourceId                       : /subscriptions/<subscription_id>/  
                                        resourceGroups/ContosoDevRG/providers/  
                                        Microsoft/KeyVault/vaults/ContosoEKMKeyVault  
    Vault URI: https://ContosoEKMKeyVault.vault.azure.net  
    Tenant ID                        : <tenant_id>  
    SKU                              : Standard  
    Enabled For Deployment?          : False  
    Enabled For Template Deployment? : False  
    Enabled For Disk Encryption?     : False  
    Access Policies                  :  
             Tenant ID              : <tenant_id>  
             Object ID              : <object_id>  
             Application ID         :
             Display Name           : <display_name>  
             Permissions to Keys    : get, create, delete, list, update, import,
                                      backup, restore  
             Permissions to Secrets : all  
    Tags                             :  
    ```  
  
1. Erteilen Sie dem Azure AD-Dienstprinzipal Berechtigungen für den Zugriff auf den Azure-Schlüsseltresor.  
  
    Sie können andere Benutzer und andere Anwendungen autorisieren, Ihren Schlüsseltresor zu verwenden.  In unserem Beispiel verwenden wir den Dienstprinzipal, den Sie in [Schritt 1: Einrichten eines Azure AD-Dienstprinzipals](#step-1-set-up-an-azure-ad-service-principal) erstellt haben, um die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Instanz zu autorisieren.  
  
    > [!IMPORTANT]
    > Der Azure AD-Dienstprinzipal muss mindestens über die Berechtigungen *Abrufen*, *Auflisten*,*Schlüssel packen* und *Schlüssel entpacken* für den Schlüsseltresor verfügen.  
  
    Wie im folgenden Befehl gezeigt, verwenden Sie die **App-ID (Client)** aus [Schritt 1: Einrichten eines Azure AD-Dienstprinzipals](#step-1-set-up-an-azure-ad-service-principal) für den `ServicePrincipalName`-Parameter. `Set-AzKeyVaultAccessPolicy` wird lautlos ausgeführt und gibt bei erfolgreicher Ausführung keinen Wert zurück.  
  
    ```powershell  
    Set-AzKeyVaultAccessPolicy -VaultName 'ContosoEKMKeyVault' `  
      -ServicePrincipalName 9A57CBC5-4C4C-40E2-B517-EA677 `  
      -PermissionsToKeys get, list, wrapKey, unwrapKey  
    ```  
  
    Rufen Sie das `Get-AzKeyVault` -Cmdlet auf, um die Berechtigungen zu bestätigen. In der Ausgabe der Anweisung sollten Sie unter `Access Policies` den Namen Ihrer Azure AD-Anwendung als weiteren Mandanten finden, der Zugriff auf diesen Schlüsseltresor hat.  
  
1. Erstellen Sie einen asymmetrischen Schlüssel im Schlüsseltresor. Dies ist auf zwei Arten möglich: Importieren eines vorhandenen Schlüssels oder Erstellen eines neuen Schlüssels.  

     > [!NOTE]
     > SQL Server unterstützt nur RSA- und RSA-HSM-Schlüssel mit jeweils 2048 oder 3072 Bit.

### <a name="best-practices"></a>Bewährte Methoden

Es werden die folgenden bewährten Methoden empfohlen, um die schnelle Wiederherstellung von Schlüsseln sicherzustellen und den Zugriff auf Ihre Daten außerhalb von Azure zu ermöglichen:

- Erstellen Sie Ihren Verschlüsselungsschlüssel lokal auf einem lokalen HSM-Gerät (Hardware Security Module). Stellen Sie sicher, dass Sie einen asymmetrischen RSA-Schlüssel mit 2048 oder 3072 Bit verwenden, damit er von SQL Server unterstützt wird.
- Importieren Sie den Verschlüsselungsschlüssel in Ihren Azure Key Vault. Dieser Prozess wird in den folgenden Abschnitten beschrieben.
- Bevor Sie den Schlüssel zum ersten Mal in Ihrem Azure Key Vault verwenden, erstellen Sie eine Schlüsselsicherung Ihres Azure Key Vaults. Weitere Informationen finden Sie im Zusammenhang mit dem Befehl [Backup-AzureKeyVaultKey]().
- Stellen Sie sicher, dass Sie jedes Mal eine neue Schlüsselsicherung Ihres Azure Key Vaults erstellen, wenn Sie Änderungen am Schlüssel vornehmen (z. B. wenn Sie ACLs, Tags oder Schlüsselattribute hinzufügen).

  > [!NOTE]
  > Die Sicherung eines Schlüssels ist ein Azure Key Vault-Schlüsselvorgang, der eine Datei zurückgibt, die an einer beliebigen Stelle gespeichert werden kann.

### <a name="types-of-keys"></a>Schlüsseltypen

In einem Azure-Schlüsseltresor können vier Typen von Schlüsseln generiert werden, die mit SQL Server kompatibel sind. asymmetrische RSA-Schlüssel und RSA-HSM-Schlüssel mit jeweils 2048 oder 3072 Bit
  
- **Softwaregeschützt**: In Software verarbeitet und in Ruhezeiten verschlüsselt. Vorgänge für softwaregeschützte Schlüssel erfolgen auf virtuellen Azure-Computern. Dieser Typ wird für Schlüssel empfohlen, die nicht in einer Produktionsbereitstellung verwendet werden.  

- **HSM-geschützt**: Dieser Typ wird für zusätzliche Sicherheit von einem Hardwaresicherheitsmodul (HSM) erstellt und geschützt. Die Kosten betragen ungefähr 1 USD pro Schlüsselversion.  
  
    > [!IMPORTANT]
    > Verwenden Sie für den SQL Server-Connector nur die Zeichen a-z, A-Z, 0-9 und Bindestriche (-). Das Zeichenlimit beträgt 26 Zeichen.
    > In Verbindung mit dem [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Connector funktionieren verschiedene Schlüsselversionen unter dem gleichen Schlüsselnamen nicht in einem Azure Key Vault. Informationen zur Rotation eines Azure Key Vault-Schlüssels, der von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] verwendet wird, finden Sie in den Schritten zum Schlüsselrollover in Abschnitt A. „Wartungsanweisungen für [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Connector“ des Artikels [SQL Server-Connector – Verwaltung und Problembehandlung](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md).  

### <a name="import-an-existing-key"></a>Importieren eines vorhandenen Schlüssels
  
Wenn Sie bereits über einen softwaregeschützten 2048-Bit-RSA-Schlüssel verfügen, können Sie diesen Schlüssel in den Azure Key Vault hochladen. Wenn beispielsweise eine PFX-Datei auf Ihrem Laufwerk `C:\\` in einer Datei mit dem Namen *softkey.pfx* gespeichert ist, die Sie in den Azure-Schlüsseltresor hochladen möchten, führen Sie den folgenden Befehl aus, um die Variable `securepfxpwd` für das Kennwort `12987553` für die PFX-Datei festzulegen:  
  
``` powershell  
$securepfxpwd = ConvertTo-SecureString -String '12987553' `  
  -AsPlainText -Force  
```  

Anschließend können Sie den folgenden Befehl ausführen, um den Schlüssel aus der PFX-Datei, die ihn durch die Hardware schützt (empfohlen), in den Schlüsseltresordienst zu importieren:  
  
``` powershell  
    Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' `  
      -Name 'ContosoFirstKey' -KeyFilePath 'c:\softkey.pfx' `  
      -KeyFilePassword $securepfxpwd $securepfxpwd  -Destination 'HSM'  
```  

> [!CAUTION]
> Das Importieren des asymmetrischen Schlüssels wird für Produktionsszenarios dringend empfohlen, da der Administrator dann den Schlüssel in einem Schlüsselhinterlegungssystem hinterlegen kann. Wenn der asymmetrische Schlüssel im Tresor erstellt wird, kann er nicht hinterlegt werden, da der private Schlüssel nie den Tresor verlassen kann. Schlüssel zum Schützen kritischer Daten sollten hinterlegt werden. Der Verlust eines asymmetrischen Schlüssels führt zu permanenten Datenverlust.  

### <a name="create-a-new-key"></a>Erstellen eines neuen Schlüssels

Alternativ können Sie einen neuen Verschlüsselungsschlüssel direkt in Azure Key Vault erstellen und mittels Software oder HSM schützen.  In diesem Beispiel erstellen wir mit dem Cmdlet `Add-AzureKeyVaultKey` einen softwaregeschützten Schlüssel:  

``` powershell  
Add-AzureKeyVaultKey -VaultName 'ContosoEKMKeyVault' `  
  -Name 'ContosoRSAKey0' -Destination 'Software'  
```  
  
Die Anweisung gibt Folgendes zurück:  
  
```console
Attributes : Microsoft.Azure.Commands.KeyVault.Models.KeyAttributes  
Key        :  {"kid":"https:contosoekmkeyvault.azure.net/keys/  
                ContosoRSAKey0/<guid>","dty":"RSA:,"key_ops": ...  
VaultName  : contosodevkeyvault  
Name       : contosoRSAKey0  
Version    : <guid>  
Id         : https://contosoekmkeyvault.vault.azure.net:443/  
              keys/ContosoRSAKey0/<guid>  
```  

> [!IMPORTANT]
> Der Schlüsseltresor unterstützt mehrere Versionen eines Schlüssels unter dem gleichen Namen, für vom [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Connector verwendete Schlüssel sollte aber weder Versionierung noch Rollover ausgeführt werden. Wenn der Administrator den für die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Verschlüsselung verwendeten Schlüssel wiederverwenden möchte, sollte ein neuer Schlüssel mit einem anderen Namen im Schlüsseltresor erstellt und zum Verschlüsseln des Verschlüsselungsschlüssels verwendet werden.  

---

## <a name="step-3-install-the-ssnoversion-connector"></a>Schritt 3: Installieren des [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Connectors

Laden Sie den SQL Server-Connector aus dem [Microsoft Download Center](https://go.microsoft.com/fwlink/p/?LinkId=521700)herunter. Der Download sollte vom Administrator des [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Computers ausgeführt werden.  

> [!NOTE]
> - Die SQL Server Connector-Versionen 1.0.0.440 und früher wurden ersetzt und werden in Produktionsumgebungen nicht mehr unterstützt. Lesen Sie die Anweisungen auf der Seite [SQL Server Connector – Wartung und Problembehandlung](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md) unter [Upgrade des SQL Server-Connectors](sql-server-connector-maintenance-troubleshooting.md#upgrade-of--connector).
> - Von Version 1.0.3.0 an meldet der SQL Server-Connector relevante Fehlermeldungen zur Problembehandlung an die Windows-Ereignisprotokolle.
> - Von Version 1.0.4.0 an werden private Azure-Clouds unterstützt, darunter Azure China, Azure Deutschland und Azure Government.
> - In Version 1.0.5.0 gibt es einen Breaking Change, die den Fingerabdruckalgorithmus betrifft. Nach dem Upgrade auf Version 1.0.5.0 treten möglicherweise Fehler bei der Datenbankwiederherstellung auf. Weitere Informationen finden Sie im [KB-Artikel 447099](https://support.microsoft.com/help/4470999/db-backup-problems-to-sql-server-connector-for-azure-1-0-5-0).
> - Ab der Version 1.0.5.0 (Zeitstempel: September 2020) unterstützt der SQL Server-Connector das Filtern von Nachrichten und Wiederholungslogik für Netzwerkanforderungen.
> - **Beginnend mit der aktualisierten Version 1.0.5.0 (TimeStamp: November 2020) unterstützt der SQL Server-Connector RSA- und RSA-HSM-Schlüssel mit jeweils 2048 oder 3072 Bit.**
  
  ![Screenshot des Installationsassistenten für den SQL Server-Connector](../../../relational-databases/security/encryption/media/ekm/ekm-connector-install.png)  
  
In der Standardeinstellung ist der Connector unter C:\Programme\SQL Server-Connector für Microsoft Azure Key Vault installiert. Dieser Speicherort kann während des Setups geändert werden. Wenn Sie ihn ändern, müssen Sie die Skripts im nächsten Abschnitt anpassen.  
  
Der Connector hat keine Benutzeroberfläche, doch wenn er erfolgreich installiert wurde, befindet sich die Datei *Microsoft.AzureKeyVaultService.EKM.dll* auf dem Computer. Diese Assembly ist die DLL des EKM-Kryptografieanbieters, die mithilfe der `CREATE CRYPTOGRAPHIC PROVIDER`-Anweisung bei [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] registriert werden muss.  
  
Die SQL Server-Connectorinstallation ermöglicht außerdem den Download von Beispieldateien für die SQL Server-Verschlüsselung.  
  
Fehlercodeerläuterungen, Konfigurationseinstellungen oder Wartungsaufgaben für SQL Server-Connector finden Sie unter:  
  
- [A. Wartungsanweisungen für SQL Server-Connector](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md#AppendixA)  
- [C. Erläuterungen zu Fehlercodes für SQL Server-Connector](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md#AppendixC)  
  
## <a name="step-4-configure-ssnoversion"></a>Schritt 4: Konfigurieren von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]  

Einen Hinweis zu den minimal erforderlichen Berechtigungsstufen für jede Aktion in diesem Abschnitt finden Sie unter [B. Häufig gestellte Fragen](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md#AppendixB).  
  
1. Führen Sie *sqlcmd.exe* aus, oder öffnen Sie [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Management Studio.  
  
1. Konfigurieren Sie [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] für die EKM-Verwendung durch Ausführen des folgenden [!INCLUDE[tsql](../../../includes/tsql-md.md)]-Skripts:  
  
    ```sql  
    -- Enable advanced options.  
    USE master;  
    GO  

    EXEC sp_configure 'show advanced options', 1;  
    GO  
    RECONFIGURE;  
    GO  

    -- Enable EKM provider  
    EXEC sp_configure 'EKM provider enabled', 1;  
    GO  
    RECONFIGURE;  
    ```  
  
1. Registrieren Sie den [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Connector bei [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] als EKM-Anbieter.  
  
    Erstellen Sie einen Kryptografieanbieter mithilfe des [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Connectors, der einen EKM-Anbieter für den Azure-Schlüsseltresor darstellt.
    In diesem Beispiel lautet der Name des Anbieters `AzureKeyVault_EKM`.  
  
    ```sql  
    CREATE CRYPTOGRAPHIC PROVIDER AzureKeyVault_EKM
    FROM FILE = 'C:\Program Files\SQL Server Connector for Microsoft Azure Key Vault\Microsoft.AzureKeyVaultService.EKM.dll';  
    GO  
    ```  

    > [!NOTE]
    > Der Dateipfad darf höchstens 256 Zeichen umfassen.  
  
1. Richten Sie [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Anmeldeinformationen für eine [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Anmeldung zur Verwendung des Schlüsseltresors ein.  
  
    Für jede Anmeldung, die eine Verschlüsselung mithilfe eines Schlüssels aus dem Schlüsseltresor ausführen soll, müssen Anmeldeinformationen hinzugefügt werden. Dazu können beispielsweise gehören:  
  
    - Eine [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Administratoranmeldung, die den Schlüsseltresor verwendet, um [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Verschlüsselungsszenarios einzurichten und zu verwalten.  
  
    - Andere [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Anmeldungen, die TDE oder andere [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Verschlüsselungsfeatures aktivieren können.  
  
    Es besteht eine Eins-zu-Eins-Zuordnung zwischen Anmeldeinformationen und Anmeldungen. Da heißt, für jede Anmeldung müssen eindeutige Anmeldeinformationen vorhanden sein.  
  
    Ändern Sie dieses [!INCLUDE[tsql](../../../includes/tsql-md.md)]-Skript wie folgt:  
  
    - Bearbeiten Sie das `IDENTITY`-Argument (`ContosoEKMKeyVault`), damit es auf Ihren Azure-Schlüsseltresor verweist.
      - Wenn Sie eine *globale Azure-Cloud* verwenden, ersetzen Sie das `IDENTITY`-Argument durch den Namen Ihres Azure-Schlüsseltresors aus [Schritt 2: Erstellen eines Schlüsseltresors](#step-2-create-a-key-vault).
      - Wenn Sie eine *private Azure Cloud* (z. B. Azure Government, Azure China 21ViaNet oder Azure Deutschland) verwenden, ersetzen Sie das `IDENTITY`-Argument durch den Tresor-URI, der in Schritt 3 des Abschnitts [Erstellen eines Schlüsseltresors und eines Schlüssels mithilfe von PowerShell](#create-a-key-vault-and-key-by-using-powershell) zurückgegeben wurde. Schließen Sie „https://“ nicht in den Tresor-URI ein.
    - Ersetzen Sie den ersten Teil des `SECRET`-Arguments durch die ID des Azure Active Directory-Clients aus [Schritt 1: Einrichten eines Azure AD-Dienstprinzipals](#step-1-set-up-an-azure-ad-service-principal). In diesem Beispiel lautet die **Client-ID** `9A57CBC54C4C40E2B517EA677E0EFA00`.  
  
      > [!IMPORTANT]
      > Achten Sie darauf, die Bindestriche aus der App-ID (Client) zu entfernen.  
  
    - Vervollständigen Sie den zweiten Teil des `SECRET`-Arguments mit dem **geheimen Clientschlüssel** aus [Schritt 1: Einrichten eines Azure AD-Dienstprinzipals](#step-1-set-up-an-azure-ad-service-principal).  In diesem Beispiel lautet der geheime Clientschlüssel `08:k?[:XEZFxcwIPvVVZhTjHWXm7w1?m`. Die endgültige Zeichenfolge für das `SECRET`-Argument ist eine lange Abfolge von Buchstaben und Ziffern ohne Bindestriche.  
  
    ```sql  
    USE master;  
    CREATE CREDENTIAL sysadmin_ekm_cred
        WITH IDENTITY = 'ContosoEKMKeyVault',                            -- for public Azure
        -- WITH IDENTITY = 'ContosoEKMKeyVault.vault.usgovcloudapi.net', -- for Azure Government
        -- WITH IDENTITY = 'ContosoEKMKeyVault.vault.azure.cn',          -- for Azure China 21Vianet
        -- WITH IDENTITY = 'ContosoEKMKeyVault.vault.microsoftazure.de', -- for Azure Germany
               --<----Application (Client) ID ---><--Azure AD app (Client) ID secret-->
        SECRET = '9A57CBC54C4C40E2B517EA677E0EFA0008:k?[:XEZFxcwIPvVVZhTjHWXm7w1?m'
    FOR CRYPTOGRAPHIC PROVIDER AzureKeyVault_EKM;  
  
    -- Add the credential to the SQL Server administrator's domain login
    ALTER LOGIN [<domain>\<login>]  
    ADD CREDENTIAL sysadmin_ekm_cred;  
    ```  
  
    Ein Beispiel für die Verwendung von Variablen für das `CREATE CREDENTIAL`-Argument und die programmgesteuerte Entfernung der Bindestriche aus der Client-ID finden Sie unter [CREATE CREDENTIAL &#40;Transact-SQL&#41;](../../../t-sql/statements/create-credential-transact-sql.md).  
  
1. Öffnen Sie Ihren Azure Key Vault-Schlüssel in Ihrer SQL Server-Instanz.  

    Sie müssen den Schlüssel öffnen – unabhängig davon, ob Sie einen neuen Schlüssel erstellt oder einen asymmetrischen Schlüssel wie in [Schritt 2: Erstellen eines Schlüsseltresors](#step-2-create-a-key-vault) beschrieben importiert haben. Öffnen Sie den Schlüssel, indem Sie im folgenden [!INCLUDE[tsql](../../../includes/tsql-md.md)]-Skript Ihren Schlüsselnamen angeben.  
  
    - Ersetzen Sie `EKMSampleASYKey` durch den Namen, den der Schlüssel in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] enthalten soll.  
    - Ersetzen Sie `ContosoRSAKey0` durch den Namen Ihres Schlüssels im Azure Key Vault.  
  
    ```sql  
    CREATE ASYMMETRIC KEY EKMSampleASYKey
    FROM PROVIDER [AzureKeyVault_EKM]  
    WITH PROVIDER_KEY_NAME = 'ContosoRSAKey0',  
    CREATION_DISPOSITION = OPEN_EXISTING;  
    ```  

1. Erstellen Sie mit dem asymmetrischen Schlüssel in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], den Sie im vorherigen Schritt erstellt haben, eine neue Anmeldung.

     ```sql  
    --Create a Login that will associate the asymmetric key to this login
    CREATE LOGIN TDE_Login
    FROM ASYMMETRIC KEY EKMSampleASYKey;
    ```  

1. Erstellen Sie mithilfe des asymmetrischen Schlüssels in SQL Server eine neue Anmeldung. Löschen Sie die Zuordnung der Anmeldeinformationen aus Schritt 4: Konfigurieren Sie SQL Server so, dass die Anmeldeinformationen der neuen Anmeldung zugeordnet werden können.

     ```sql  
    --Now drop the credential mapping from the original association
    ALTER LOGIN [<domain>\<login>]
    DROP CREDENTIAL sysadmin_ekm_cred;
    ```

1. Ändern Sie die neue Anmeldung, und ordnen Sie die EKM-Anmeldeinformationen der neuen Anmeldung zu.

     ```sql  
    --Now add the credential mapping to the new Login
    ALTER LOGIN TDE_Login
    ADD CREDENTIAL sysadmin_ekm_cred;
    ```  

1. Erstellen Sie eine Testdatenbank, die mit dem Schlüssel des Azure Key Vaults verschlüsselt wird.

     ```sql
    --Create a test database that will be encrypted with the Azure key vault key
    CREATE DATABASE TestTDE
    ```  

1. Erstellen Sie mithilfe von ASYMMETRIC KEY (EKMSampleASYKey) einen Verschlüsselungsschlüssel für die Datenbank.

    ```sql  
    --Create an ENCRYPTION KEY using the ASYMMETRIC KEY (EKMSampleASYKey)
    CREATE DATABASE ENCRYPTION KEY
    WITH ALGORITHM = AES_256
    ENCRYPTION BY SERVER ASYMMETRIC KEY EKMSampleASYKey;
    ```
  
1. Verschlüsseln Sie die Testdatenbank. Aktivieren Sie TDE, indem Sie ENCRYPTION ON festlegen.

     ```sql  
    --Enable TDE by setting ENCRYPTION ON
    ALTER DATABASE TestTDE
    SET ENCRYPTION ON;  
     ```  

1. Bereinigen Sie die Testobjekte. Löschen Sie alle Objekte, die in diesem Testskript erstellt wurden.

    ```sql  
    -- CLEAN UP
    USE Master
    ALTER DATABASE [TestTDE] SET SINGLE_USER WITH ROLLBACK IMMEDIATE
    DROP DATABASE [TestTDE]

    ALTER LOGIN [TDE_Login] DROP CREDENTIAL [sysadmin_ekm_cred]
    DROP LOGIN [TDE_Login]

    DROP CREDENTIAL [sysadmin_ekm_cred]  

    USE MASTER
    DROP ASYMMETRIC KEY [EKMSampleASYKey]
    DROP CRYPTOGRAPHIC PROVIDER [AzureKeyVault_EKM]
    ```  

Beispielskripts finden Sie im Blog zum Thema [SQL Server Transparent Data Encryption und erweiterbare Schlüsselverwaltung mit Azure Key Vault](https://techcommunity.microsoft.com/t5/sql-server/intro-sql-server-transparent-data-encryption-and-extensible-key/ba-p/1427549).

## <a name="next-steps"></a>Nächste Schritte  
  
Da Sie nun die Grundkonfiguration abgeschlossen haben, folgt als Nächstes das Thema [Verwenden von SQL Server-Connector mit SQL-Verschlüsselungsfunktionen](../../../relational-databases/security/encryption/use-sql-server-connector-with-sql-encryption-features.md).
  
## <a name="see-also"></a>Weitere Informationen  

- [Erweiterbare Schlüsselverwaltung mit Azure Key Vault](../../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)
- [SQL Server-Connector – Verwaltung und Problembehandlung](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md)
