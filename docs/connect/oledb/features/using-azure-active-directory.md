---
title: Verwenden von Azure Active Directory
description: Erfahren Sie mehr über die im Microsoft OLE DB-Treiber für SQL Server verfügbaren Azure Active Directory-Authentifizierungsmethoden, die Verbindungen mit Azure SQL-Datenbanken ermöglichen.
ms.custom: ''
ms.date: 09/30/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: conceptual
author: bazizi
ms.author: v-beaziz
ms.openlocfilehash: f75cfc74616a58975bedf2dc7b2de342c0c57066
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837365"
---
# <a name="using-azure-active-directory"></a>Verwenden von Azure Active Directory
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

## <a name="purpose"></a>Zweck


Ab Version [18.2.1](../release-notes-for-oledb-driver-for-sql-server.md#1821) ermöglicht der Microsoft OLE DB-Treiber für SQL Server, dass OLE DB-Anwendungen über einen Identitätsverbund eine Verbindung mit einer Azure SQL-Datenbankinstanz herstellen. Die neuen Authentifizierungsmethoden umfassen:
- Azure Active Directory-Anmelde-ID und -Kennwort
- Azure Active Directory-Zugriffstoken
- Integrierte Azure Active Directory-Authentifizierung
- SQL-Anmelde-ID und Kennwort


Mit Version [18.3.0](../release-notes-for-oledb-driver-for-sql-server.md#1830) wird Unterstützung für die folgenden Authentifizierungsmethoden hinzugefügt:
- Interaktive Azure Active Directory-Authentifizierung
- Azure Active Directory-Authentifizierung mit einer verwalteten Identität

Mit Version [18.5.0](../release-notes-for-oledb-driver-for-sql-server.md#1850) wird Unterstützung für die folgenden Authentifizierungsmethoden hinzugefügt:
- Azure Active Directory-Authentifizierung mit einem Dienstprinzipal

> [!NOTE]
> Die Verwendung der folgenden Authentifizierungsmodi, wenn `DataTypeCompatibility` (oder eine entsprechende Eigenschaft) auf `80` festgelegt ist, wird **nicht** unterstützt:
> - Azure Active Directory-Authentifizierung mit Anmelde-ID und Kennwort
> - Azure Active Directory-Authentifizierung mit Zugriffstoken
> - Integrierte Azure Active Directory-Authentifizierung
> - Interaktive Azure Active Directory-Authentifizierung
> - Azure Active Directory-Authentifizierung mit verwalteten Identitäten
> - Azure Active Directory-Authentifizierung mit einem Dienstprinzipal

## <a name="connection-string-keywords-and-properties"></a>Schlüsselwörter und Eigenschaften bei Verbindungszeichenfolgen
Zur Unterstützung der Azure Active Directory-Authentifizierung wurden die folgenden Schlüsselwörter für Verbindungszeichenfolgen eingeführt:

|Verbindungszeichenfolgen-Schlüsselwort|Verbindungseigenschaft|BESCHREIBUNG|
|---               |---                |---        |
|Access Token|SSPROP_AUTH_ACCESS_TOKEN|Gibt ein Zugriffstoken für die Authentifizierung gegenüber Azure Active Directory an. |
|Authentifizierung|SSPROP_AUTH_MODE|Gibt die zu verwendende Authentifizierungsmethode an.|

Weitere Informationen zu den neuen Schlüsselwörtern/Eigenschaften finden Sie auf den folgenden Seiten:
- [Verwenden von Verbindungszeichenfolgen-Schlüsselwörtern mit dem OLE DB-Treiber für SQL Server](../applications/using-connection-string-keywords-with-oledb-driver-for-sql-server.md)
- [Initialisierungs- und Autorisierungseigenschaften](../ole-db-data-source-objects/initialization-and-authorization-properties.md)

## <a name="encryption-and-certificate-validation"></a>Verschlüsselung und Zertifikatüberprüfung
In diesem Abschnitt werden die Änderungen beim Verhalten der Verschlüsselung und Zertifikatüberprüfung erläutert. Diese Änderungen treffen **nur** zu, wenn die neuen Schlüsselwörter (oder ihre entsprechenden Eigenschaften) für Authentifizierungs- oder Zugriffstoken verwendet werden.

### <a name="encryption"></a>Verschlüsselung
Für eine verbesserte Sicherheit setzt der Treiber den standardmäßigen Verschlüsselungswert außer Kraft, wenn die neuen Eigenschaften/Schlüsselwörter für die Verbindung verwendet werden. Dazu setzt er den Wert auf `yes`. Das Überschreiben geschieht zur Initialisierungszeit des Datenquellenobjekts. Wenn der Verschlüsselungswert vor der Initialisierung festgelegt wird, wird er beibehalten und nicht überschrieben.

> [!NOTE]   
> In ADO-Anwendungen und in Anwendungen, die die `IDBInitialize`-Schnittstelle über `IDataInitialize::GetDataSource` abrufen, legt die Kernkomponente, die die Schnittstelle implementiert, die Verschlüsselung explizit auf den Standardwert `no` fest. Dadurch akzeptieren die neuen Authentifizierungseigenschaften/-schlüsselwörter diese Einstellung, und der Verschlüsselungswert wird **nicht** überschrieben. Folglich wird **empfohlen**, dass diese Anwendungen `Use Encryption for Data=true` explizit festlegen, um den Standardwert zu überschreiben.

### <a name="certificate-validation"></a>Zertifikatüberprüfung
Für eine verbesserte Sicherheit akzeptieren die neuen Verbindungseigenschaften/-schlüsselwörter die `TrustServerCertificate`-Einstellung (sowie die entsprechenden Schlüsselwörter/Eigenschaften der Verbindungszeichenfolge) **unabhängig von der Verschlüsselungseinstellung des Clients**. Folglich wird das Serverzertifikat standardmäßig überprüft.

> [!NOTE]   
> Die Zertifikatüberprüfung kann zudem über das Feld `Value` des Registrierungseintrags `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\MSSQLServer\Client\SNI18.0\GeneralFlags\Flag2` gesteuert werden. Gültige Werte sind `0` und `1`. Der OLE DB-Treiber wählt aus den Registrierungseinstellungen und den Einstellungen der Verbindungseigenschaften/-schlüsselwörter die sicherste Option aus. Der Treiber überprüft also das Serverzertifikat, wenn mindestens eine der Registrierungs- oder Verbindungseinstellungen die Zertifikatüberprüfung ermöglicht.

## <a name="gui-additions"></a>GUI-Ergänzungen
Die grafische Benutzeroberfläche des Treibers wurde erweitert, um die Azure Active Directory-Authentifizierung zu ermöglichen. Weitere Informationen finden Sie unter
- [Dialogfeld „SQL Server-Anmeldung“](../help-topics/sql-server-login-dialog.md)
- [Konfiguration von Universal Data Link (UDL)](../help-topics/data-link-pages.md)

## <a name="example-connection-strings"></a>Exemplarische Verbindungszeichenfolgen
Dieser Abschnitt enthält Beispiele für neue und vorhandene Schlüsselwörter in Verbindungszeichenfolgen, die mit den Eigenschaften `IDataInitialize::GetDataSource` und `DBPROP_INIT_PROVIDERSTRING` verwendet werden können.

### <a name="sql-authentication"></a>SQL-Authentifizierung
- Verwenden von `IDataInitialize::GetDataSource`:
    - Neu:
        > Provider=MSOLEDBSQL;Data Source=[server];Initial Catalog=[database];**Authentication=SqlPassword**;User ID=[username];Password=[password];Use Encryption for Data=true
    - Veraltet:
        > Provider=MSOLEDBSQL;Data Source=[server];Initial Catalog=[database];User ID=[username];Password=[password];Use Encryption for Data=true
- Verwenden von `DBPROP_INIT_PROVIDERSTRING`:
    - Neu:
        > Server=[server];Database=[database];**Authentication=SqlPassword**;UID=[username];PWD=[password];Encrypt=yes
    - Veraltet:
        > Server=[server];Database=[database];UID=[username];PWD=[password];Encrypt=yes

### <a name="integrated-windows-authentication-using-security-support-provider-interface-sspi"></a>Integrierte Windows-Authentifizierung unter Verwendung der Security Support Provider-Schnittstelle (SSPI)

- Verwenden von `IDataInitialize::GetDataSource`:
    - Neu:
        > Provider=MSOLEDBSQL;Data Source=[server];Initial Catalog=[database];**Authentication=ActiveDirectoryIntegrated**;Use Encryption for Data=true
    - Veraltet:
        > Provider=MSOLEDBSQL;Data Source=[server];Initial Catalog=[database];**Integrated Security=SSPI**;Use Encryption for Data=true
- Verwenden von `DBPROP_INIT_PROVIDERSTRING`:
    - Neu:
        > Server=[server];Database=[database];**Authentication=ActiveDirectoryIntegrated**;Encrypt=yes
    - Veraltet:
        > Server=[server];Database=[database];**Trusted_Connection=yes**;Encrypt=yes

### <a name="azure-active-directory-username-and-password-authentication"></a>Azure Active Directory-Authentifizierung mit Benutzername und Kennwort

- Verwenden von `IDataInitialize::GetDataSource`:
    > Provider=MSOLEDBSQL;Data Source=[server];Initial Catalog=[database];**Authentication=ActiveDirectoryPassword**;User ID=[username];Password=[password];Use Encryption for Data=true
- Verwenden von `DBPROP_INIT_PROVIDERSTRING`:
    > Server=[server];Database=[database];**Authentication=ActiveDirectoryPassword**;UID=[username];PWD=[password];Encrypt=yes

### <a name="azure-active-directory-integrated-authentication"></a>Integrierte Azure Active Directory-Authentifizierung

- Verwenden von `IDataInitialize::GetDataSource`:
    > Provider=MSOLEDBSQL;Data Source=[server];Initial Catalog=[database];**Authentication=ActiveDirectoryIntegrated**;Use Encryption for Data=true
- Verwenden von `DBPROP_INIT_PROVIDERSTRING`:
    > Server=[server];Database=[database];**Authentication=ActiveDirectoryIntegrated**;Encrypt=yes

### <a name="azure-active-directory-authentication-using-an-access-token"></a>Azure Active Directory-Authentifizierung mit Zugriffstoken

- Verwenden von `IDataInitialize::GetDataSource`:
    > Provider=MSOLEDBSQL;Data Source=[server];Initial Catalog=[database];**Access Token=[access token]** ;Use Encryption for Data=true
- Verwenden von `DBPROP_INIT_PROVIDERSTRING`:
    > Die Bereitstellung von Zugriffstoken über `DBPROP_INIT_PROVIDERSTRING` wird nicht unterstützt.

### <a name="azure-active-directory-interactive-authentication"></a>Interaktive Azure Active Directory-Authentifizierung

- Verwenden von `IDataInitialize::GetDataSource`:
    > Provider=MSOLEDBSQL;Data Source=[server];Initial Catalog=[database];**Authentication=ActiveDirectoryInteractive**;User ID=[username];Use Encryption for Data=true
- Verwenden von `DBPROP_INIT_PROVIDERSTRING`:
    > Server=[server];Database=[database];**Authentication=ActiveDirectoryInteractive**;UID=[username];Encrypt=yes

### <a name="azure-active-directory-managed-identity-authentication"></a>Azure Active Directory-Authentifizierung mit einer verwalteten Identität

- Verwenden von `IDataInitialize::GetDataSource`:
    - Benutzerseitig zugewiesene verwaltete Identität:
        > Provider=MSOLEDBSQL;Data Source=[server];Initial Catalog=[database];**Authentication=ActiveDirectoryMSI**;User ID=[Object ID];Use Encryption for Data=true
    - Systemseitig zugewiesene verwaltete Identität:
        > Provider=MSOLEDBSQL;Data Source=[server];Initial Catalog=[database];**Authentication=ActiveDirectoryMSI**;Use Encryption for Data=true
- Verwenden von `DBPROP_INIT_PROVIDERSTRING`:
    - Benutzerseitig zugewiesene verwaltete Identität:
        > Server=[server];Database=[database];**Authentication=ActiveDirectoryMSI**;UID=[Object ID];Encrypt=yes
    - Systemseitig zugewiesene verwaltete Identität:
        > Server=[server];Database=[database];**Authentication=ActiveDirectoryMSI**;Encrypt=yes

### <a name="azure-active-directory-service-principal-authentication"></a>Azure Active Directory-Authentifizierung mit einem Dienstprinzipal

- Verwenden von `IDataInitialize::GetDataSource`:
    > Provider=MSOLEDBSQL;Data Source=[server];Initial Catalog=[database];**Authentication=ActiveDirectoryServicePrincipal**;User ID=[Application (client) ID];Password=[Application (client) secret];Use Encryption for Data=true
- Verwenden von `DBPROP_INIT_PROVIDERSTRING`:
    > Server=[server];Database=[database];**Authentication=ActiveDirectoryServicePrincipal**;UID=[Application (client) ID];PWD=[Application (client) secret];Encrypt=yes

## <a name="code-samples"></a>Codebeispiele

Die folgenden Beispiele zeigen den erforderlichen Code, um über Verbindungsschlüsselwörter eine Verbindung mit Azure Active Directory herzustellen. 

### <a name="access-token"></a>Access Token
```cpp
#include <string>
#include <iostream>
#include <msdasc.h>

int main()
{
    wchar_t azureServer[] = L"server";
    wchar_t azureDatabase[] = L"mydatabase";
    wchar_t accessToken[] = L"eyJ0eXAiOi...";
    IDBInitialize *pIDBInitialize = nullptr;
    IDataInitialize* pIDataInitialize = nullptr;
    HRESULT hr = S_OK;

    CoInitialize(nullptr);

    // Construct the connection string.
    std::wstring connString = L"Provider=MSOLEDBSQL;Data Source=" + std::wstring(azureServer) + L";Initial Catalog=" + 
                              std::wstring(azureDatabase) + L";Access Token=" + accessToken + L";Use Encryption for Data=true;";
    hr = CoCreateInstance(CLSID_MSDAINITIALIZE, nullptr, CLSCTX_INPROC_SERVER, 
                          IID_IDataInitialize, reinterpret_cast<LPVOID*>(&pIDataInitialize));
    if (FAILED(hr))
    {
        std::cout << "Failed to create an IDataInitialize instance." << std::endl;
        goto Cleanup;
    }
    hr = pIDataInitialize->GetDataSource(nullptr, CLSCTX_INPROC_SERVER, connString.c_str(), 
                                         IID_IDBInitialize, reinterpret_cast<IUnknown**>(&pIDBInitialize));
    if (FAILED(hr))
    {
        std::cout << "Failed to get data source object." << std::endl;
        goto Cleanup;
    }
    hr = pIDBInitialize->Initialize();
    if (FAILED(hr))
    {
        std::cout << "Failed to establish connection." << std::endl;
        goto Cleanup;
    }

Cleanup:
    if (pIDBInitialize)
    {
        pIDBInitialize->Uninitialize();
        pIDBInitialize->Release();
    }
    if (pIDataInitialize)
    {
        pIDataInitialize->Release();
    }

    CoUninitialize();
}
```
### <a name="active-directory-integrated"></a>Active Directory Integrated
```cpp
#include <string>
#include <iostream>
#include <msdasc.h>

int main()
{
    wchar_t azureServer[] = L"server";
    wchar_t azureDatabase[] = L"mydatabase";
    IDBInitialize *pIDBInitialize = nullptr;
    IDataInitialize* pIDataInitialize = nullptr;
    HRESULT hr = S_OK;

    CoInitialize(nullptr);

    // Construct the connection string.
    std::wstring connString = L"Provider=MSOLEDBSQL;Data Source=" + std::wstring(azureServer) + L";Initial Catalog=" + 
                              std::wstring(azureDatabase) + L";Authentication=ActiveDirectoryIntegrated;Use Encryption for Data=true;";

    hr = CoCreateInstance(CLSID_MSDAINITIALIZE, nullptr, CLSCTX_INPROC_SERVER, 
                          IID_IDataInitialize, reinterpret_cast<LPVOID*>(&pIDataInitialize));
    if (FAILED(hr)) 
    {
        std::cout << "Failed to create an IDataInitialize instance." << std::endl;
        goto Cleanup;
    }
    hr = pIDataInitialize->GetDataSource(nullptr, CLSCTX_INPROC_SERVER, connString.c_str(), 
                                         IID_IDBInitialize, reinterpret_cast<IUnknown**>(&pIDBInitialize));
    if (FAILED(hr))
    {
        std::cout << "Failed to get data source object." << std::endl;
        goto Cleanup;
    }
    hr = pIDBInitialize->Initialize();
    if (FAILED(hr))
    {
        std::cout << "Failed to establish connection." << std::endl;
        goto Cleanup;
    }

Cleanup:
    if (pIDBInitialize)
    {
        pIDBInitialize->Uninitialize();
        pIDBInitialize->Release();
    }
    if (pIDataInitialize)
    {
        pIDataInitialize->Release();
    }

    CoUninitialize();
}
```

## <a name="next-steps"></a>Nächste Schritte
- [Autorisieren des Zugriffs auf Azure Active Directory-Webanwendungen mit dem Flow zum Erteilen des OAuth 2.0-Codes](/azure/active-directory/azuread-dev/v1-protocols-oauth-code)

- Erfahren Sie mehr über die [Azure Active Directory-Authentifizierung](/azure/azure-sql/database/authentication-aad-overview) bei SQL Server.

- Konfigurieren Sie Treiberverbindungen mithilfe von [Verbindungszeichenfolgen-Schlüsselwörtern](../applications/using-connection-string-keywords-with-oledb-driver-for-sql-server.md), die der OLE DB-Treiber unterstützt.