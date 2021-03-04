---
title: AppContext-Optionen in SqlClient
description: Hier wird beschrieben, wie Sie in SqlClient verfügbare AppContext-Optionen verwenden.
ms.date: 06/15/2020
dev_langs:
- csharp
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: johnnypham
ms.author: v-jopha
ms.reviewer: v-daenge
ms.openlocfilehash: aa6de0ff33b0d6a70ef14a42f5def3f3c6a20482
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101836005"
---
# <a name="appcontext-switches-in-sqlclient"></a>AppContext-Optionen in SqlClient

[!INCLUDE [Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Die AppContext-Klasse ermöglicht SqlClient das Bereitstellen neuer Funktionen, während weiterhin Aufrufer unterstützt werden, die vom vorherigen Verhalten abhängen. Benutzer können sich gegen Änderungen am Verhalten entscheiden, indem Sie bestimmte AppContext-Optionen festlegen.

## <a name="enabling-decimal-truncation-behavior"></a>Aktivieren des Kürzungsverhaltens für Dezimalzahlen

[!INCLUDE [appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

Ab Microsoft.Data.SqlClient 2.0 werden Dezimaldaten wie bei SQL Server standardmäßig gerundet. Sie können die AppContext-Option **"Switch.Microsoft.Data.SqlClient.TruncateScaledDecimal"** beim Anwendungsstart auf `true` festlegen, um das vorherige Kürzungsverhalten zu aktivieren:

```csharp
AppContext.SetSwitch("Switch.Microsoft.Data.SqlClient.TruncateScaledDecimal", true);
```

## <a name="enabling-managed-networking-on-windows"></a>Aktivieren verwalteter Netzwerke unter Windows

[!INCLUDE [appliesto-xxxx-netcore-netst-md](../../includes/appliesto-xxxx-netcore-netst-md.md)]

Unter Windows verwendet SqlClient standardmäßig eine native Implementierung der SNI-Netzwerkschnittstelle. Sie können die AppContext-Option **"Switch.Microsoft.Data.SqlClient.UseManagedNetworkingOnWindows"** beim Anwendungsstart auf `true` festlegen, um die Verwendung einer verwalteten SNI-Implementierung zu aktivieren:

```csharp
AppContext.SetSwitch("Switch.Microsoft.Data.SqlClient.UseManagedNetworkingOnWindows", true);
```

Diese Option schaltet das Verhalten des Treibers so um, dass in Projekten in .NET Core 2.1 und höher und in .NET Standard 2.0 und höher unter Windows eine verwaltete Netzwerkimplementierung verwendet wird. Dies beseitigt alle Abhängigkeiten von nativen Bibliotheken für die Microsoft.Data.SqlClient-Bibliothek. Die Option ist nur für Test- und Debugzwecke vorgesehen.

> [!NOTE]
> Im Vergleich zur nativen Implementierung gibt es einige bekannte Unterschiede. Beispielsweise unterstützt die verwaltete Implementierung nicht die Windows-Authentifizierung ohne Domäne.

## <a name="disabling-transparent-network-ip-resolution"></a>Deaktivieren der transparenten Netzwerk-IP-Adressauflösung

[!INCLUDE [appliesto-netfx-xxxx-xxxx-md](../../includes/appliesto-netfx-xxxx-xxxx-md.md)]

Die transparente Netzwerk-IP-Adressauflösung (Transparent Network IP Resolution, TNIR) ist eine Überarbeitung des vorhandenen MultiSubnetFailover-Features. TNIR wirkt sich auf die Verbindungssequenz des Treibers aus, wenn die erste aufgelöste IP-Adresse des Hostnamens nicht reagiert und dem Hostnamen mehrere IP-Adressen zugeordnet sind. TNIR interagiert mit MultiSubnetFailover, um die folgenden drei Verbindungssequenzen bereitzustellen:<br />
* 0: Erst wird eine IP-Adresse ausprobiert, dann alle IP-Adressen gleichzeitig.
* 1: Die IP-Adressen werden alle gleichzeitig ausprobiert.
* 2: Die IP-Adressen werden alle nacheinander ausprobiert.

|TransparentNetworkIPResolution|MultiSubnetFailover|Verhalten|
|--------|--------|--------|
|True|True|1|
|Richtig|False|0|
|False|True|1|
|False|False|2|

TransparentNetworkIPResolution ist in der Standardeinstellung aktiviert. MultiSubnetFailover ist in der Standardeinstellung deaktiviert. Sie können die AppContext-Option **"Switch.Microsoft.Data.SqlClient.DisableTNIRByDefaultInConnectionString"** beim Anwendungsstart auf `true` festlegen, um TNIR zu deaktivieren:

```csharp
AppContext.SetSwitch("Switch.Microsoft.Data.SqlClient.DisableTNIRByDefaultInConnectionString", true);
```

Weitere Informationen zum Festlegen dieser Eigenschaften finden Sie in der Dokumentation zur [SqlConnection.ConnectionString-Eigenschaft](/dotnet/api/microsoft.data.sqlclient.sqlconnection.connectionstring). 

## <a name="enable-a-minimum-timeout-during-login"></a>Aktivieren eines minimalen Timeouts bei der Anmeldung

[!INCLUDE [appliesto-netfx-xxxx-xxxx-md](../../includes/appliesto-netfx-xxxx-xxxx-md.md)]

Sie können die AppContext-Option **Switch.Microsoft.Data.SqlClient.UseOneSecFloorInTimeoutCalculationDuringLogin** beim Anwendungsstart auf `true` festlegen, um zu verhindern, dass bei einem Anmeldeversuch unbegrenzt gewartet wird:

```csharp
AppContext.SetSwitch("Switch.Microsoft.Data.SqlClient.UseOneSecFloorInTimeoutCalculationDuringLogin", false);
```

## <a name="disable-blocking-behavior-of-readasync"></a>Deaktivieren des blockierenden Verhaltens von ReadAsync

[!INCLUDE [appliesto-netfx-xxxx-xxxx-md](../../includes/appliesto-netfx-xxxx-xxxx-md.md)]

ReadAsync wird standardmäßig synchron ausgeführt und blockiert den aufrufenden Thread im .NET Framework. Sie können die AppContext-Option **Switch.Microsoft.Data.SqlClient.MakeReadAsyncBlocking** beim Anwendungsstart auf `false` festlegen, um dieses blockierende Verhalten zu deaktivieren:

```csharp
AppContext.SetSwitch("Switch.Microsoft.Data.SqlClient.MakeReadAsyncBlocking", false);
```

## <a name="see-also"></a>Weitere Informationen

[AppContext-Klasse](/dotnet/api/system.appcontext?view=netcore-3.1&preserve-view=true)