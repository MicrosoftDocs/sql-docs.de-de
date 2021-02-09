---
title: Azure Active Directory
description: Erfahren Sie, wie Sie die Azure Active Directory-Authentifizierung mit den Microsoft-Treibern für PHP für SQL Server verwenden.
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.custom: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- azure active directory, authentication, access token
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ae3e734dee5f8ac46de2646cca9c37a7ed7748df
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076958"
---
# <a name="connect-using-azure-active-directory-authentication"></a>Herstellen einer Verbindung mithilfe der Azure Active Directory-Authentifizierung
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

[Azure Active Directory](/azure/active-directory/active-directory-whatis) (Azure AD) ist eine zentrale Technologie zur Verwaltung von Benutzer-IDs, die als Alternative zur [SQL Server-Authentifizierung](how-to-connect-using-sql-server-authentication.md) fungiert. Azure AD ermöglicht Verbindungen mit Microsoft Azure SQL-Datenbank und Azure Synapse Analytics mit Verbundidentitäten in Azure AD unter Verwendung eines Benutzernamens und Kennworts, der integrierten Windows-Authentifizierung oder einem Azure AD-Zugriffstoken. Die PHP-Treiber für SQL Server unterstützen diese Features teilweise.

Wenn Sie Azure AD nutzen möchten, verwenden Sie die Schlüsselwörter **Authentication** oder **AccessToken** (diese schließen sich gegenseitig aus), wie in der folgenden Tabelle gezeigt. Weitere technische Details finden Sie unter [Verwenden von Azure Active Directory mit dem ODBC Driver](../odbc/using-azure-active-directory.md).

|Schlüsselwort|Werte|BESCHREIBUNG|
|-|-|-|
|**AccessToken**|Nicht festgelegt (Standardeinstellung)|Der Authentifizierungsmodus wird durch andere Schlüsselwörter bestimmt. Weitere Informationen finden Sie unter [Connection Options](connection-options.md). |
||Eine Bytezeichenfolge|Das aus einer OAuth-JSON-Antwort extrahierte Azure AD-Zugriffstoken. Die Verbindungszeichenfolge darf weder eine Benutzer-ID noch ein Kennwort noch das Schlüsselwort „Authentication“ enthalten (erfordert die ODBC-Treiberversion 17 oder höher unter Linux oder macOS). |
|**Authentifizierung**|Nicht festgelegt (Standardeinstellung)|Der Authentifizierungsmodus wird durch andere Schlüsselwörter bestimmt. Weitere Informationen finden Sie unter [Connection Options](connection-options.md). |
||`SqlPassword`|Direkte Authentifizierung bei einer SQL Server-Instanz (dies kann eine Azure-Instanz sein) unter Verwendung eines Benutzernamens und eines Kennworts. Benutzername und Kennwort müssen mit den Schlüsselwörtern **UID** und **PWD** an die Verbindungszeichenfolge übergeben werden. |
||`ActiveDirectoryPassword`|Authentifizierung mit einer Azure Active Directory-Identität unter Verwendung eines Benutzernamens und eines Kennworts. Benutzername und Kennwort müssen mit den Schlüsselwörtern **UID** und **PWD** an die Verbindungszeichenfolge übergeben werden. |
||`ActiveDirectoryMsi`|Authentifizierung über eine vom System oder vom Benutzer zugewiesene verwaltete Identität (erfordert die ODBC-Treiberversion 17.3.1.1 oder höher). Eine Übersicht und Tutorials finden Sie unter [Was sind verwaltete Identitäten für Azure-Ressourcen?](/azure/active-directory/managed-identities-azure-resources/overview).|
||`ActiveDirectoryServicePrincipal`|Authentifizierung mithilfe von Dienstprinzipalobjekten (erfordert Version 17.7 oder höher des ODBC-Treibers). Weitere Informationen und Beispiele finden Sie unter [Anwendungs- und Dienstprinzipalobjekte in Azure Active Directory](/azure/active-directory/develop/app-objects-and-service-principals).|

Das Schlüsselwort **Authentication** wirkt sich auf die Einstellungen der Verbindungssicherheit aus. Wenn dieses Schlüsselwort in der Verbindungszeichenfolge festgelegt ist, wird das Schlüsselwort **Encrypt** standardmäßig auf „true“ festgelegt. Das bedeutet, dass der Client eine Verschlüsselung anfordert. Darüber hinaus wird unabhängig von der Verschlüsselungseinstellung das Serverzertifikat überprüft, sofern nicht **TrustServerCertificate** auf „true“ festgelegt wird (die Standardeinstellung ist **false**). Dieses Feature unterscheidet sich von der alten, weniger sicheren Anmeldemethode, bei der das Serverzertifikat nur dann überprüft wird, wenn die Verschlüsselung in der Verbindungszeichenfolge explizit angefordert wird.

#### <a name="limitations"></a>Einschränkungen

Unter Windows unterstützt der zugrunde liegende ODBC-Treiber einen weiteren Wert für das Schlüsselwort **Authentication**: **ActiveDirectoryIntegrated**. Allerdings unterstützen die PHP-Treiber diesen Wert auf keiner Plattform.

## <a name="example---connect-using-sqlpassword-and-activedirectorypassword"></a>Beispiel: Herstellen einer Verbindung mithilfe von SqlPassword und ActiveDirectoryPassword

```php
<?php
// First connect to a local SQL Server instance by setting Authentication to SqlPassword
$serverName = "myserver.mydomain";

$connectionInfo = array("UID"=>$myusername, "PWD"=>$mypassword, "Authentication"=>'SqlPassword');

$conn = sqlsrv_connect($serverName, $connectionInfo);
if ($conn === false) {
    echo "Could not connect with Authentication=SqlPassword.\n";
    print_r(sqlsrv_errors());
} else {
    echo "Connected successfully with Authentication=SqlPassword.\n";
    sqlsrv_close($conn);
}

// Now connect to an Azure SQL database by setting Authentication to ActiveDirectoryPassword
$azureServer = "myazureserver.database.windows.net";
$azureDatabase = "myazuredatabase";
$azureUsername = "myuid";
$azurePassword = "mypassword";
$connectionInfo = array("Database"=>$azureDatabase,
                        "UID"=>$azureUsername,
                        "PWD"=>$azurePassword,
                        "Authentication"=>'ActiveDirectoryPassword');

$conn = sqlsrv_connect($azureServer, $connectionInfo);
if ($conn === false) {
    echo "Could not connect with Authentication=ActiveDirectoryPassword.\n";
    print_r(sqlsrv_errors());
} else {
    echo "Connected successfully with Authentication=ActiveDirectoryPassword.\n";
    sqlsrv_close($conn);
}

?>
```

## <a name="example---connect-using-the-pdo_sqlsrv-driver"></a>Beispiel: Herstellen einer Verbindung mithilfe des PDO_SQLSRV-Treibers

```php
<?php
// First connect to a local SQL Server instance by setting Authentication to SqlPassword
$serverName = "myserver.mydomain";

$connectionInfo = "Database = $databaseName; Authentication = SqlPassword;";

try {
    $conn = new PDO("sqlsrv:server = $serverName ; $connectionInfo", $myusername, $mypassword);
    echo "Connected successfully with Authentication=SqlPassword.\n";
    $conn = null;
} catch (PDOException $e) {
    echo "Could not connect with Authentication=SqlPassword.\n";
    print_r($e->getMessage());
    echo "\n";
}

// Now connect to an Azure SQL database by setting Authentication to ActiveDirectoryPassword
$azureServer = "myazureserver.database.windows.net";
$azureDatabase = "myazuredatabase";
$azureUsername = "myuid";
$azurePassword = "mypassword";
$connectionInfo = "Database = $azureDatabase; Authentication = ActiveDirectoryPassword;";

try {
    $conn = new PDO("sqlsrv:server = $azureServer ; $connectionInfo", $azureUsername, $azurePassword);
    echo "Connected successfully with Authentication=ActiveDirectoryPassword.\n";
    unset($conn);
} catch (PDOException $e) {
    echo "Could not connect with Authentication=ActiveDirectoryPassword.\n";
    print_r($e->getMessage());
    echo "\n";
}
?>
```

## <a name="example---connect-using-azure-ad-access-token"></a>Beispiel: Herstellen einer Verbindung mithilfe eines Azure AD-Zugriffstokens

### <a name="sqlsrv-driver"></a>SQLSRV-Treiber

```php
<?php
// Using an access token to connect: do not use UID or PWD connection options
// Assume $accToken is the valid byte string extracted from an OAuth JSON response
$connectionInfo = array("Database"=>$azureAdDatabase, "AccessToken"=>$accToken);
$conn = sqlsrv_connect($azureAdServer, $connectionInfo);
if ($conn === false) {
    echo "Could not connect with Azure AD Access Token.\n";
    print_r(sqlsrv_errors());
} else {
    echo "Connected successfully with Azure AD Access Token.\n";
    sqlsrv_close($conn);
}
?>
```

### <a name="pdo_sqlsrv-driver"></a>PDO_SQLSRV-Treiber

```php
<?php
try {
    // Using an access token to connect: do not pass in $uid or $pwd
    // Assume $accToken is the valid byte string extracted from an OAuth JSON response
    $connectionInfo = "Database = $azureAdDatabase; AccessToken = $accToken;";
    $conn = new PDO("sqlsrv:server = $azureAdServer; $connectionInfo");
    echo "Connected successfully with Azure AD Access Token\n";
    unset($conn);
} catch (PDOException $e) {
    echo "Could not connect with Azure AD Access Token.\n";
    print_r($e->getMessage());
    echo "\n";
}
?>
```

## <a name="example---connect-using-managed-identities-for-azure-resources"></a>Beispiel: Herstellen einer Verbindung mithilfe von verwalteten Identitäten für Azure-Ressourcen

### <a name="using-the-system-assigned-managed-identity-with-sqlsrv-driver"></a>Verwenden der vom System zugewiesenen verwalteten Identität mit dem SQLSRV-Treiber

Wenn Sie mithilfe der vom System zugewiesenen verwalteten Identität eine Verbindung herstellen, verwenden Sie nicht die Optionen UID und PWD.

```php
<?php

$azureServer = 'myazureserver.database.windows.net';
$azureDatabase = 'myazuredatabase';
$connectionInfo = array('Database'=>$azureDatabase,
                        'Authentication'=>'ActiveDirectoryMsi');
$conn = sqlsrv_connect($azureServer, $connectionInfo);

if ($conn === false) {
    echo "Could not connect with Authentication=ActiveDirectoryMsi (system-assigned).\n";
    print_r(sqlsrv_errors());
} else {
    echo "Connected successfully with Authentication=ActiveDirectoryMsi (system-assigned).\n";
    
    $tsql = "SELECT @@Version AS SQL_VERSION";
    $stmt = sqlsrv_query($conn, $tsql);
    if ($stmt === false) {
        echo "Failed to run the simple query (system-assigned).\n";
        print_r(sqlsrv_errors());
    } else {
        while ($row = sqlsrv_fetch_array($stmt, SQLSRV_FETCH_ASSOC)) {
            echo $row['SQL_VERSION'] . PHP_EOL;
        }

        sqlsrv_free_stmt($stmt);
    }
    
    sqlsrv_close($conn);
}
?>
```

### <a name="using-the-user-assigned-managed-identity-with-pdo_sqlsrv-driver"></a>Verwenden der vom Benutzer zugewiesenen verwalteten Identität mit dem PDO_SQLSRV-Treiber

Eine vom Benutzer zugewiesene verwaltete Identität wird als eigenständige Azure-Ressource erstellt. Azure erstellt eine Identität in dem Azure AD-Mandanten, der vom verwendeten Abonnement als vertrauenswürdig eingestuft wird. Nachdem die Identität erstellt wurde, kann sie einer oder mehreren Azure-Dienstinstanzen zugewiesen werden. Kopieren Sie die `Object ID` dieser Identität, und legen Sie sie als Benutzernamen in der Verbindungszeichenfolge fest. 

Geben Sie beim Herstellen einer Verbindung mithilfe der vom Benutzer zugewiesenen verwalteten Identität die Objekt-ID als Benutzernamen an, lassen Sie aber das Kennwort weg.

```php
<?php

$azureServer = 'myazureserver.database.windows.net';
$azureDatabase = 'myazuredatabase';
$azureUser = '2d68f56e-9547-4dae-aee8-f3g28ab9674x';    // Object ID of the identity
$connectionInfo = "Database = $azureDatabase; Authentication = ActiveDirectoryMsi;";

try {
    $conn = new PDO("sqlsrv:server = $azureServer; $connectionInfo", $azureUser);
    echo "Connected successfully with Authentication=ActiveDirectoryMsi (user-assigned).\n";
    
    $tsql = "SELECT @@Version AS SQL_VERSION";
    
    try {
        $stmt = $conn->query($tsql);
        $result = $stmt->fetchall(PDO::FETCH_ASSOC);
        print_r($result);

        unset($stmt);
    } catch (PDOException $e) {
        echo "Failed to run the simple query (user-assigned).\n";
    }
    unset($conn);
} catch (PDOException $e) {
    echo "Could not connect with Authentication=ActiveDirectoryMsi (user-assigned).\n";
    print_r($e->getMessage());
    echo "\n";
}
?>
```

## <a name="example---connect-using-service-principal-objects-in-azure-active-directory"></a>Beispiel: Herstellen einer Verbindung mithilfe von Dienstprinzipalobjekten in Azure Active Directory

Zum Authentifizieren mithilfe eines Dienstprinzipalobjekts benötigen Sie die entsprechende [Anwendungsclient-ID](/azure/active-directory/develop/howto-create-service-principal-portal#get-tenant-and-app-id-values-for-signing-in) und den [geheimen Clientschlüssel](/azure/active-directory/develop/howto-create-service-principal-portal#option-2-create-a-new-application-secret).


### <a name="sqlsrv-driver"></a>SQLSRV-Treiber

```php
<?php

$adServer = 'myazureserver.database.windows.net';
$adDatabase = 'myazuredatabase';
$adSPClientId = 'myAppClientId';
$adSPClientSecret = 'myClientSecret';

$conn = false;
$connectionInfo = array("Database"=>$adDatabase, 
                        "Authentication"=>"ActiveDirectoryServicePrincipal",
                        "UID"=>$adSPClientId,
                        "PWD"=>$adSPClientSecret);

$conn = sqlsrv_connect($adServer, $connectionInfo);
if ($conn === false) {
    echo "Could not connect using Azure AD Service Principal." . PHP_EOL;
    print_r(sqlsrv_errors());
}

sqlsrv_close($conn);

?>
```

### <a name="pdo_sqlsrv-driver"></a>PDO_SQLSRV-Treiber

```php
<?php

$adServer = 'myazureserver.database.windows.net';
$adDatabase = 'myazuredatabase';
$adSPClientId = 'myAppClientId';
$adSPClientSecret = 'myClientSecret';

$conn = false;
try {
    $connectionInfo = "Database = $adDatabase; Authentication = ActiveDirectoryServicePrincipal;";
    $conn = new PDO("sqlsrv:server = $adServer; $connectionInfo", $adSPClientId, $adSPClientSecret);
} catch (PDOException $e) {
    echo "Could not connect using Azure AD Service Principal.\n";
    print_r($e->getMessage());
    echo PHP_EOL;
}

unset($conn);
?>
```


## <a name="see-also"></a>Weitere Informationen
[Verwenden von Azure Active Directory mit dem ODBC-Treiber](../odbc/using-azure-active-directory.md)

[Was sind verwaltete Identitäten für Azure-Ressourcen?](/azure/active-directory/managed-identities-azure-resources/overview)