---
title: Verwenden der Azure Active Directory-Authentifizierung mit SqlClient
description: Erfahren Sie, wie unterstützte Azure Active Directory-Authentifizierungsmodi verwendet werden, um mithilfe von SqlClient eine Verbindung mit Azure SQL-Datenquellen herzustellen.
ms.date: 11/20/2020
dev_langs:
- csharp
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: karinazhou
ms.author: v-jizho2
ms.reviewer: v-daenge
ms.openlocfilehash: c57c2d10854ed902a6230eafc3a912cd0508c989
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101835997"
---
# <a name="using-azure-active-directory-authentication-with-sqlclient"></a>Verwenden der Azure Active Directory-Authentifizierung mit SqlClient

[!INCLUDE [appliesto-netfx-netcore-netst-md](../../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE [Driver_ADONET_Download](../../../includes/driver_adonet_download.md)]

In diesem Artikel wird beschrieben, wie eine Verbindung mit Azure SQL-Datenquellen hergestellt werden kann, indem die Azure Active Directory-Authentifizierung (Azure AD) von einer .NET-Anwendung mit SqlClient verwendet wird.

Die Azure AD-Authentifizierung nutzt Identitäten in Azure AD für den Zugriff auf Azure SQL-Datenquellen wie Azure SQL Database, Azure SQL Managed Instance und Azure Synapse Analytics. Der Namespace **Microsoft.Data.SqlClient** ermöglicht es Clientanwendungen, Azure AD-Anmeldeinformationen in verschiedenen Authentifizierungsmodi anzugeben, wenn sie eine Verbindung mit Azure SQL-Datenbank herstellen.

Wenn Sie die Verbindungseigenschaft `Authentication` in der Verbindungszeichenfolge festlegen, kann der Client entsprechend dem angegebenen Wert einen bevorzugten Azure AD-Authentifizierungsmodus wählen:

- Die früheste Version von **Microsoft.Data.SqlClient** unterstützt für .NET Framework, .NET Core und .NET Standard `Active Directory Password`. Außerdem werden für .NET Framework unterstützt die Authentifizierungstypen `Active Directory Integrated` und `Active Directory Interactive`. 

- Ab **Microsoft.Data.SqlClient** 2.0.0 wird die Unterstützung der Authentifizierungstypen `Active Directory Integrated` und `Active Directory Interactive` auf .NET Framework, .NET Core und .NET Standard ausgedehnt. 

  SqlClient 2.0.0 bietet nun auch den neuen Authentifizierungsmodus `Active Directory Service Principal`. Dieser nutzt zur Authentifizierung die Client-ID und das Geheimnis einer Dienstprinzipalidentität. 

- **Microsoft.Data.SqlClient** 2.1.0 bietet weitere Authentifizierungsmodi, einschließlich `Active Directory Device Code Flow` und `Active Directory Managed Identity` (auch `Active Directory MSI` genannt). Diese neuen Modi ermöglichen der Anwendung das Abrufen eines Zugriffstokens, um eine Verbindung mit dem Server herzustellen. 

Informationen zur Azure AD-Authentifizierung, die über das hinausgehen, was in den folgenden Abschnitten beschrieben wird, finden Sie unter [Herstellen einer Verbindung mit SQL-Datenbank unter Verwendung der Azure Active Directory-Authentifizierung](/azure/azure-sql/database/authentication-aad-overview).

## <a name="setting-azure-active-directory-authentication"></a>Festlegen der Azure Active Directory-Authentifizierung

Wenn die Anwendung eine Verbindung mit Azure SQL-Datenquellen unter Verwendung der Azure AD-Authentifizierung herstellt, muss sie einen gültigen Authentifizierungsmodus bieten. In der folgenden Tabelle sind die unterstützten Authentifizierungsmodi aufgeführt. Die Anwendung gibt einen Modus mithilfe der Verbindungseigenschaft `Authentication` in der Verbindungszeichenfolge an.

| Wert | BESCHREIBUNG  | Framework    | Version von Microsoft.Data.SqlClient |
|:--|:--|:--|:--:|
| Active Directory Password | Authentifizierung mit einer Azure AD-Identität unter Verwendung eines Benutzernamens und eines Kennworts | Ab .NET Framework 4.6, NET Core 2.1, .NET Standard 2.0  | Ab 1.0|
| Active Directory Integrated |Authentifizierung mit einer Azure AD-Identität unter Verwendung der integrierten Authentifizierung | Ab .NET Framework 4.6, NET Core 2.1, .NET Standard 2.0 | Ab 2.0.0<sup>1</sup> |
| Active Directory Interactive | Authentifizierung mit einer Azure AD-Identität unter Verwendung der interaktiven Authentifizierung | Ab .NET Framework 4.6, NET Core 2.1, .NET Standard 2.0 | Ab 2.0.0<sup>1</sup> |
| Active Directory Service Principal | Authentifizieren mit einer Azure AD-Identität unter Verwendung der Client-ID und des Geheimnisses einer Dienstprinzipalidentität | Ab .NET Framework 4.6, NET Core 2.1, .NET Standard 2.0 | Ab 2.0.0 |
| Active Directory Device Code Flow | Authentifizierung mit einer Azure AD-Identität unter Verwendung des Modus „Gerätecodeflow“ | Ab .NET Framework 4.6, NET Core 2.1, .NET Standard 2.0 | Ab 2.1.0 |
| Active Directory Managed Identity, <br>Active Directory MSI | Authentifizieren mit einer Azure AD-Identität unter Verwendung einer vom System oder Benutzer zugewiesenen verwalteten Identität | Ab .NET Framework 4.6, NET Core 2.1, .NET Standard 2.0 | Ab 2.1.0 |

<sup>1</sup> Vor **Microsoft.Data.SqlClient** 2.0.0 wurden die Authentifizierungsmodi `Active Directory Integrated` und `Active Directory Interactive` nur ab .NET Framework 4.6 unterstützt.

## <a name="using-active-directory-password-authentication"></a>Verwenden der Authentifizierung „Active Directory Password“

Der Authentifizierungsmodus `Active Directory Password` unterstützt die Authentifizierung bei Azure-Datenquellen mit Azure AD für native Azure AD-Benutzer bzw. Azure AD-Verbundbenutzer. Bei Wahl dieses Modus müssen Benutzeranmeldeinformationen in der Verbindungszeichenfolge angegeben werden. Das folgende Beispiel zeigt, wie die Authentifizierung `Active Directory Password` verwendet wird.

```c#
// Use your own server, database, user ID, and password.
string ConnectionString = @"Server=demo.database.windows.net; Authentication=Active Directory Password; Database=testdb; User Id=user@domain.com; Password=***";

using (SqlConnection conn = new SqlConnection(ConnectionString)) {
    conn.Open();
}
```


## <a name="using-active-directory-integrated-authentication"></a>Verwenden der Authentifizierung „Active Directory Integrated“

Für den Authentifizierungsmodus `Active Directory Integrated` müssen Sie einen Verbund zwischen der lokalen Active Directory-Instanz und Azure AD in der Cloud herstellen. Einen Verbund können Sie beispielsweise mithilfe von Active Directory-Verbunddienste (AD FS) einrichten.

Wenn Sie bei einem in die Domäne eingebundenen Computer angemeldet sind, können Sie in diesem Modus auf Azure SQL-Datenquellen zugreifen, ohne zur Eingabe von Anmeldeinformationen aufgefordert zu werden. In der Verbindungszeichenfolge für .NET Framework-Anwendungen können Sie nicht den Benutzernamen und das Kennwort angeben. Für .NET Core- und .NET Standard-Anwendungen ist der Benutzername in der Verbindungszeichenfolge optional. Die `Credential`-Eigenschaft von SqlConnection kann in diesem Modus nicht festgelegt werden. 

Der folgende Codeausschnitt ist ein Beispiel der Verwendung der `Active Directory Integrated`-Authentifizierung.

```c#
// Use your own server and database.
string ConnectionString1 = @"Server=demo.database.windows.net; Authentication=Active Directory Integrated; Database=testdb";

using (SqlConnection conn = new SqlConnection(ConnectionString1)) {
    conn.Open();
}

// User ID is optional for .NET Core and .NET Standard.
string ConnectionString2 = @"Server=demo.database.windows.net; Authentication=Active Directory Integrated; Database=testdb; User Id=user@domain.com";

using (SqlConnection conn = new SqlConnection(ConnectionString2)) {
    conn.Open();
}
```


## <a name="using-active-directory-interactive-authentication"></a>Verwenden der Authentifizierung „Active Directory Interactive“

Der Authentifizierungsmodus `Active Directory Interactive` unterstützt zum Herstellen einer Verbindung mit Azure SQL-Datenquellen eine mehrstufige Authentifizierungstechnologie. Wenn Sie diesen Authentifizierungsmodus in der Verbindungszeichenfolge angeben, wird ein Bildschirm zur Authentifizierung bei Azure angezeigt, auf dem der Benutzer zur Eingabe gültiger Anmeldeinformationen aufgefordert wird. Sie können das Kennwort nicht in der Verbindungszeichenfolge eingeben. 

Die `Credential`-Eigenschaft von SqlConnection kann in diesem Modus nicht festgelegt werden. Ab **Microsoft.Daten.SqlClient** 2.0.0 ist der Benutzername in der Verbindungszeichenfolge zulässig, sofern Sie sich im interaktiven Modus befinden. 

Das folgende Beispiel zeigt, wie die Authentifizierung `Active Directory Interactive` verwendet wird.

```c#
// Use your own server, database, and user ID.
// User ID is optional.
string ConnectionString1 = @"Server=demo.database.windows.net; Authentication=Active Directory Interactive; Database=testdb; User Id=user@domain.com";

using (SqlConnection conn = new SqlConnection(ConnectionString1)) {
    conn.Open();
}

// User ID is not provided.
string ConnectionString2 = @"Server=demo.database.windows.net; Authentication=Active Directory Interactive; Database=testdb";

using (SqlConnection conn = new SqlConnection(ConnectionString2)) {
    conn.Open();
}
```


## <a name="using-active-directory-service-principal-authentication"></a>Verwenden der Authentifizierung „Active Directory Service Principal“

Im Authentifizierungsmodus `Active Directory Service Principal` kann die Clientanwendung eine Verbindung mit Azure-SQL-Datenquellen herstellen, und zwar durch Angabe der Client-ID und des Geheimnisses einer Dienstprinzipalidentität. Die Dienstprinzipalauthentifizierung umfasst Folgendes:
1. Einrichten einer App-Registrierung mit einem Geheimnis
1. Erteilen von Berechtigungen für die App in der Azure SQL-Datenbankinstanz
1. Herstellen der Verbindung mit den richtigen Anmeldeinformationen 

Das folgende Beispiel zeigt, wie die Authentifizierung `Active Directory Service Principal` verwendet wird.

```c#
// Use your own server, database, app ID, and secret.
string ConnectionString = @"Server=demo.database.windows.net; Authentication=Active Directory Service Principal; Database=testdb; User Id=AppId; Password=secret";

using (SqlConnection conn = new SqlConnection(ConnectionString)) {
    conn.Open();
}
```


## <a name="using-active-directory-device-code-flow-authentication"></a>Verwenden der Authentifizierung „Active Directory Device Code Flow“

Mithilfe der [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) für .NET (MSAL.NET) ermöglicht die `Active Directory Device Code Flow`-Authentifizierung, dass die Clientanwendung mit Azure SQL-Datenquellen über Geräte und Betriebssysteme, die keinen interaktiven Webbrowser haben, verbunden wird. Die interaktive Authentifizierung erfolgt auf einem anderen Gerät. Weitere Informationen zur Gerätecodeflow-Authentifizierung finden Sie unter [OAuth 2.0-Gerätecodeflow](/azure/active-directory/develop/v2-oauth2-device-code). 

Bei Wahl dieses Modus können Sie nicht die `Credential`-Eigenschaft von `SqlConnection` festlegen. Außerdem dürfen Benutzername und Kennwort nicht in der Verbindungszeichenfolge angegeben werden. 

Der folgende Codeausschnitt ist ein Beispiel der Verwendung der `Active Directory Device Code Flow`-Authentifizierung.

```c#
// Use your own server and database.
string ConnectionString = @"Server=demo.database.windows.net; Authentication=Active Directory Device Code Flow; Database=testdb";

using (SqlConnection conn = new SqlConnection(ConnectionString)) {
    conn.Open();
}
```


## <a name="using-active-directory-managed-identity-authentication"></a>Verwenden der Authentifizierung „Active Directory Managed Identity“

*Verwaltete Identitäten* für Azure-Ressourcen ist der neue Name des Diensts, der zuvor als „Verwaltete Dienstidentität“ (Managed Service Identity, MSI) bezeichnet wurde. Wenn eine Clientanwendung mithilfe einer Azure-Ressource auf einen Azure-Dienst mit Unterstützung der Azure AD-Authentifizierung zugreift, können Sie verwaltete Identitäten zur Authentifizierung verwenden, indem Sie eine Identität für die Azure-Ressource in Azure AD bereitstellen. Anschließend können Sie mithilfe dieser Identität Zugriffstoken abrufen. Dank dieser Authentifizierungsmethode müssen keine Anmeldeinformationen und Geheimnisse mehr verwaltet werden.

Es gibt zwei Arten von verwalteten Identitäten:

- Eine _systemseitig zugewiesene verwaltete Identität_ wird für eine Dienstinstanz in Azure AD erstellt, die mit dem Lebenszyklus dieser Dienstinstanz verknüpft ist. 
- Eine _benutzerseitig zugewiesene verwaltete Identität_ wird als eigenständige Azure-Ressource erstellt und kann einer oder mehreren Instanzen eines Azure-Diensts zugewiesen werden. 

Weitere Informationen zu verwalteten Identitäten finden Sie unter [Was sind verwaltete Identitäten für Azure-Ressourcen?](/azure/active-directory/managed-identities-azure-resources/overview).

Ab **Microsoft.Daten.SqlClient** 2.1.0 unterstützt der Treiber die Authentifizierung bei Azure SQL-Datenbank, Azure Synapse Analytics und Azure SQL Managed Instance durch den Bezug von Zugriffstoken über eine verwaltete Identität. Für diese Authentifizierung müssen Sie in der Verbindungszeichenfolge entweder `Active Directory Managed Identity` oder `Active Directory MSI` angeben, wobei kein Kennwort erforderlich ist. 

Die `Credential`-Eigenschaft von `SqlConnection` kann in diesem Modus auch nicht festgelegt werden. Für eine benutzerseitig zugewiesene verwaltete Identität muss ein Benutzername angegeben werden. 

Im folgenden Beispiel wird gezeigt, wie die `Active Directory Managed Identity`-Authentifizierung mit einer systemseitig zugewiesenen verwalteten Identität verwendet wird.

```c#
// For system-assigned managed identity
// Use your own server and database.
string ConnectionString1 = @"Server=demo.database.windows.net; Authentication=Active Directory Managed Identity; Database=testdb";

using (SqlConnection conn = new SqlConnection(ConnectionString1)) {
    conn.Open();
}

string ConnectionString2 = @"Server=demo.database.windows.net; Authentication=Active Directory MSI; Database=testdb";

using (SqlConnection conn = new SqlConnection(ConnectionString2)) {
    conn.Open();
}
```

Im folgenden Beispiel wird gezeigt, wie die `Active Directory Managed Identity`-Authentifizierung mit einer benutzerseitig zugewiesenen verwalteten Identität verwendet wird.

```c#
// For user-assigned managed identity
// Use your own server, database, and user ID.
string ConnectionString1 = @"Server=demo.database.windows.net; Authentication=Active Directory Managed Identity; User Id=ObjectIdOfManagedIdentity; Database=testdb";

using (SqlConnection conn = new SqlConnection(ConnectionString1)) {
    conn.Open();
}

string ConnectionString2 = @"Server=demo.database.windows.net; Authentication=Active Directory MSI; User Id=ObjectIdOfManagedIdentity; Database=testdb";

using (SqlConnection conn = new SqlConnection(ConnectionString2)) {
    conn.Open();
}
```


## <a name="customizing-active-directory-authentication"></a>Anpassen der Active Directory-Authentifizierung

Neben der Verwendung der in den Treiber integrierten Active Directory-Authentifizierung wird ab **Microsoft.Data.SqlClient** 2.1.0 Anwendungen die Möglichkeit geboten, die Active Directory-Authentifizierung anzupassen. Die Anpassung basiert auf der `ActiveDirectoryAuthenticationProvider`-Klasse, die der abstrakten [`SqlAuthenticationProvider`](/dotnet/api/system.data.sqlclient.sqlauthenticationprovider)-Klasse abgeleitet ist. 

Während der Active Directory-Authentifizierung kann die Clientanwendung ihre eigene `ActiveDirectoryAuthencationProvider`-Klasse wie folgt definieren:

- Durch Verwenden einer angepassten Rückrufmethode
- Durch Übergeben einer Anwendungsclient-ID an die MSAL-Bibliothek über den SqlClient-Treiber zum Abrufen von Zugriffstoken

Das folgende Beispiel zeigt, wie bei Verwendung der `Active Directory Device Code Flow`-Authentifizierung ein benutzerdefinierter Rückruf erfolgt. 

[!code-csharp [AADAuthenticationCustomDeviceFlowCallback#1](~/../sqlclient/doc/samples/AADAuthenticationCustomDeviceFlowCallback.cs#1)]

Mit einer angepassten `ActiveDirectoryAuthenticationProvider`-Klasse kann eine benutzerdefinierte Clientanwendungs-ID an SqlClient übergeben werden, sofern ein unterstützter Active Directory-Authentifizierungsmodus eingesetzt wird. Die folgenden Active Directory-Authentifizierungsmodi werden unterstützt: `Active Directory Password`, `Active Directory Integrated`, `Active Directory Interactive`, `Active Directory Service Principal` und `Active Directory Device Code Flow`.

Die Anwendungsclient-ID lässt sich auch über `SqlAuthenticationProviderConfigurationSection` oder `SqlClientAuthenticationProviderConfigurationSection` konfigurieren. Die Konfigurationseigenschaft `applicationClientId` gilt für .NET Framework ab  4.6 und .Net Core ab 2.1.

Der folgende Codeausschnitt ist ein Beispiel für die Verwendung einer benutzerdefinierten `ActiveDirectoryAuthenticationProvider`-Klasse mit einer benutzerdefinierten Clientanwendungs-ID, wenn die `Active Directory Interactive`-Authentifizierung verwendet wird.

[!code-csharp [ApplicationClientIdAzureAuthenticationProvider#1](~/../sqlclient/doc/samples/ApplicationClientIdAzureAuthenticationProvider.cs#1)]

Das folgende Beispiel zeigt, wie die Client-ID einer Anwendung in einem Konfigurationsabschnitt festgelegt wird.

```xml
<configuration>
  <configSections>
    <section name="SqlClientAuthenticationProviders"
             type="Microsoft.Data.SqlClient.SqlClientAuthenticationProviderConfigurationSection, Microsoft.Data.SqlClient" />
  </configSections>
  <SqlClientAuthenticationProviders applicationClientId ="<GUID>" />
</configuration>

<!--or-->

<configuration>
  <configSections>
    <section name="SqlAuthenticationProviders"
             type="Microsoft.Data.SqlClient.SqlAuthenticationProviderConfigurationSection, Microsoft.Data.SqlClient" />
  </configSections>
  <SqlAuthenticationProviders applicationClientId ="<GUID>" />
</configuration>
```

## <a name="support-for-a-custom-sql-authentication-provider"></a>Unterstützung eines benutzerdefinierten SQL-Authentifizierungsanbieters

Angesichts der größeren Flexibilität kann die Clientanwendung auch ihren eigenen Anbieter für die Active Directory-Authentifizierung anstelle der `ActiveDirectoryAuthenticationProvider`-Klasse verwenden. Der benutzerdefinierte Authentifizierungsanbieter muss eine Unterklasse von `SqlAuthenticationProvider` mit überschriebenen Methoden sein. 

Das folgende Beispiel zeigt, wie zur `Active Directory Device Code Flow`-Authentifizierung ein neuer Authentifizierungsanbieter verwendet wird.

[!code-csharp [CustomDeviceCodeFlowAzureAuthenticationProvider#1](~/../sqlclient/doc/samples/CustomDeviceCodeFlowAzureAuthenticationProvider.cs#1)]

Zusätzlich zur verbesserten `Active Directory Interactive`-Authentifizierung werden ab **Microsoft.Data.SqlClient** 2.1.0 die folgenden APIs für Clientanwendungen bereitgestellt, um die interaktive und Gerätecodeflow-Authentifizierung anzupassen.

```c#
public class ActiveDirectoryAuthenticationProvider
{
    // For .NET Framework targeted applications only
    // Sets a reference to the current System.Windows.Forms.IWin32Window that triggers the browser to be shown. 
    // Used to center the browser pop-up onto this window.
    public void SetIWin32WindowFunc(Func<IWin32Window> iWin32WindowFunc);

    // For .NET Standard targeted applications only
    // Sets a reference to the ViewController (if using Xamarin.iOS), Activity (if using Xamarin.Android) IWin32Window, or IntPtr (if using .NET Framework). 
    // Used for invoking the browser for Active Directory Interactive authentication.
    public void SetParentActivityOrWindowFunc(Func<object> parentActivityOrWindowFunc);

    // For .NET Framework, .NET Core, and .NET Standard targeted applications
    // Sets a callback method that's invoked with a custom web UI instance that will let the user sign in with Azure AD, present consent if needed, and get back the authorization code. 
    // Applicable when working with Active Directory Interactive authentication.
    public void SetAcquireAuthorizationCodeAsyncCallback(Func<Uri, Uri, CancellationToken, Task<Uri>> acquireAuthorizationCodeAsyncCallback);

    // For .NET Framework, .NET Core, and .NET Standard targeted applications
    // Clears cached user tokens from the token provider.
    public static void ClearUserTokenCache();
}
```


## <a name="see-also"></a>Weitere Informationen
- [Anwendungs- und Dienstprinzipalobjekte in Azure Active Directory](/azure/active-directory/develop/app-objects-and-service-principals)
- [Authentifizierungsflows](/azure/active-directory/develop/msal-authentication-flows)
