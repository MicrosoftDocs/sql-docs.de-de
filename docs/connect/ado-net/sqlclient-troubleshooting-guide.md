---
title: Leitfaden zur Problembehandlung bei SqlClient
description: Seite mit Lösungen für gängige Probleme.
ms.date: 03/03/2021
dev_langs:
- csharp
- vb
ms.assetid: 6f5ff56a-a57e-49d7-8ae9-bbed697e42e3
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-jizho2
ms.openlocfilehash: 725d1522f98e721f581a7d4b7614181fd2632962
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464803"
---
# <a name="sqlclient-troubleshooting-guide"></a>Leitfaden zur Problembehandlung bei SqlClient

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

## <a name="exceptions-when-connecting-to-sql-server"></a>Ausnahmen beim Herstellen einer Verbindung mit SQL Server

Beim Herstellen einer Verbindung kann es aus verschiedenen Gründen zu einem Fehler kommen. Nachfolgend sind Tipps zur Problembehandlung aufgeführt, mit denen eine Vielzahl dieser Probleme analysiert und behandelt werden können.

### <a name="unable-to-load-native-sni-server-name-indication-library"></a>Native SNI-Bibliothek (Server Name Indication, Servernamensanzeige) kann nicht geladen werden

#### <a name="issues-in-net-framework-applications"></a>Probleme in .NET Framework-Anwendungen

StackTrace beobachtet:

```log
TypeInitializationException: The type initializer for 'Microsoft.Data.SqlClient.SNILoadHandle' threw an exception.
DllNotFoundException: Unable to load DLL 'Microsoft.Data.SqlClient.SNI.x64.dll': The specified module could not be found. (Exception from HRESULT: 0x8007007E)
```

```log
TypeInitializationException: The type initializer for 'Microsoft.Data.SqlClient.SNILoadHandle' threw an exception.
DllNotFoundException: Unable to load DLL 'Microsoft.Data.SqlClient.SNI.x86.dll': The specified module could not be found. (Exception from HRESULT: 0x8007007E)
```

SNI ist die native C++-Bibliothek, von der SqlClient bei verschiedenen Netzwerkvorgängen unter Windows abhängt. In .NET Framework-Anwendungen, die mit dem MSBuild-Projekt-SDK erstellt wurden, werden native DLLs nicht mit Befehlen zum Wiederherstellen verwaltet. Daher wird zum NuGet-Paket „Microsoft.Data.SqlClient.SNI“ eine TARGETS-Datei hinzugefügt, die die erforderlichen Kopiervorgänge definiert.

Auf diese enthaltene TARGETS-Datei wird automatisch verwiesen, wenn für die Microsoft.Data.SqlClient-Bibliothek eine direkte Abhängigkeit festgelegt wird. In Szenarien, in denen ein transitiver (indirekter) Verweis erstellt wird, sollte manuell auf diese TARGETS-Datei verwiesen werden, um sicherzustellen, dass Kopiervorgänge bei Bedarf ausgeführt werden können.

**Empfohlene Lösung**: Stellen Sie sicher, dass in der CSPROJ-Datei der Anwendung auf die TARGETS-Datei verwiesen wird, damit Kopiervorgänge ausgeführt werden können.

Diese Ziele umfassen ausschließlich die bekannten und häufig verwendeten Ziele von Microsoft. Wenn in einem externen Tool oder einer externen Anwendung benutzerdefinierte Ziele zum Kopieren von Binärdateien definiert sind, müssen die Verantwortlichen dieser Tools neue Ziele definieren, um sicherzustellen, dass native SNI-DLLs gemeinsam mit den Microsoft.Data.SqlClient.dll-Binärdateien kopiert werden und beim Ausführen der Clientanwendungen verfügbar sind.

#### <a name="issues-in-net-core-applications"></a>Probleme in .NET Core-Anwendungen

StackTrace beobachtet:

```log
System.TypeInitializationException: The type initializer for 'Microsoft.Data.SqlClient.TdsParser' threw an exception.
---> System.TypeInitializationException: The type initializer for 'Microsoft.Data.SqlClient.SNILoadHandle' threw an exception.
---> System.DllNotFoundException: Unable to load shared library 'Microsoft.Data.SqlClient.SNI.dll' or one of its dependencies.
```

> [!NOTE]
> Dieser Fehler kann nur bei Windows-Anwendungen auftreten. Wenn der Fehler in einer Unix-Umgebung auftritt, müssen Sie sicherstellen, dass in Ihrer Anwendung nicht Windows, sondern eine Unix-Laufzeit als Ziel festgelegt ist.

SNI ist die native C++-Bibliothek, von der SqlClient bei verschiedenen Netzwerkvorgängen unter Windows abhängt. Das Laden/Entladen dieser Bibliothek in .NET Core wird nicht von Microsoft.Data.SqlClient verwaltet.

**Empfohlene Lösung**: Stellen Sie sicher, dass für das Dateisystem, in dem im .NET Core-Prozess native Laufzeitbibliotheken geladen werden, Berechtigungen zum Ausführen zugewiesen wurden. Wenn sich das Problem dadurch nicht beheben lässt, können Sie das Problem im [dotnet/runtime](https://github.com/dotnet/runtime)-Repository beschreiben bzw. melden, um weitere Unterstützung zu erhalten.

#### <a name="native-sni-pdb-not-found-errors"></a>Fehler bei der nativen SNI (PDB nicht gefunden)

StackTrace beobachtet:

```log
An assembly specified in the application dependencies manifest (sql2csv.deps.json) was not found:
  package: 'Microsoft.Data.SqlClient.SNI.runtime', version: '2.0.0'
  path: 'runtimes/win-x64/native/Microsoft.Data.SqlClient.SNI.pdb'
```

**Empfohlene Lösung**: Stellen Sie sicher, dass die Clientanwendung auf Version [v2.1.0](https://www.nuget.org/packages/Microsoft.Data.SqlClient/2.1.0) oder höher des Microsoft.Data.SqlClient-Pakets verweist. Bei Verwendung von EF Core fügen Sie dieser Paketversion von Microsoft.Data.SqlClient direkt einen Verweis hinzu, um die Abhängigkeit außer Kraft zu setzen.

### <a name="hostname-resolution-errors"></a>Fehler beim Auflösen des Hostnamens

StackTrace beobachtet:

```log
Microsoft.Data.SqlClient.SqlException (0x80131904): A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible.
Verify that the instance name is correct and that SQL Server is configured to allow remote connections.
(provider: TCP Provider, error: 0 - No such host is known.)
```

```log
Microsoft.Data.SqlClient.SqlException (0x80131904): A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible.
Verify that the instance name is correct and that SQL Server is configured to allow remote connections.
(provider: TCP Provider, error: 35 - An internal exception was caught)
```

```log
Microsoft.Data.SqlClient.SqlException (0x80131904): A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible.
Verify that the instance name is correct and that SQL Server is configured to allow remote connections.
(provider: TCP Provider, error: 35 - An internal exception was caught)
 ---> System.Net.Internals.SocketExceptionFactory+ExtendedSocketException (00000005, 0xFFFDFFFF): Name does not resolve
```

#### <a name="possible-reasons"></a>Mögliche Ursachen

- TCP/Named Pipes-Protokoll ist nicht für SQL Server aktiviert

  **Empfohlene Lösung**: Aktivieren Sie über die SQL Server-Konfigurations-Manager-Konsole das TCP/Named Pipes-Protokoll für die SQL Server-Instanz.

- Unbekannter Hostname

  **Empfohlene Lösung**: Stellen Sie sicher, dass der Hostname auf dem Client, über den die Verbindung initiiert wird, in die IP-Adresse des Servers aufgelöst wird.


### <a name="login-phase-errors"></a>Fehler in der Anmeldephase

StackTraces beobachtet:

```log
Microsoft.Data.SqlClient.SqlException (0x80131904): A connection was successfully established with the server, but then an error occurred during the pre-login handshake.
(provider: SSL Provider, error: 31 - Encryption(ssl/tls) handshake failed)
System.IO.EndOfStreamException: End of stream reached
```

```log
A connection was successfully established with the server, but then an error occurred during the login process.
(provider: SSL Provider, error: 0 - The target principal name is incorrect.)
```

```log
Microsoft.Data.SqlClient.SqlException (0x80131904): Connection Timeout Expired. The timeout period elapsed during the post-login phase. The connection could have timed out while waiting for server to complete the login process and respond; Or it could have timed out while attempting to create multiple active connections.
The duration spent while attempting to connect to this server was - [Pre-Login] initialization=837; handshake=394; [Login] initialization=3; authentication=15; [Post-Login] complete=1027;
---> System.ComponentModel.Win32Exception (258): Unknown error 258
at Microsoft.Data.SqlClient.SqlInternalConnection.OnError(SqlException exception, Boolean breakConnection, Action1 wrapCloseInAction)
```

#### <a name="possible-reasons-and-solutions"></a>Mögliche Ursachen und Lösungen

- SQL Server bietet keine Unterstützung für TLS 1.2

  Dieser Fehler tritt üblicherweise in Clientumgebungen wie Docker-Imagecontainern, auf Unix-Clients oder auf Windows-Clients auf, auf denen TLS 1.2 die unterstützte Mindestversion des TLS-Protokolls ist.

  **Empfohlene Lösung**: Installieren Sie die aktuellen Updates für unterstützte Versionen von SQL Server <sup>1</sup>, und stellen Sie sicher, dass TLS 1.2 auf dem Server aktiviert ist.

  _<sup>1</sup> Unter [Lebenszyklus der SqlClient-Treiberunterstützung](sqlclient-driver-support-lifecycle.md) sind die unterstützten SQL Server-Versionen mit verschiedenen Microsoft.Data.SqlClient-Versionen aufgeführt._

  **Unsichere Lösung**: Konfigurieren Sie die TLS/SSL-Einstellungen für das Docker-Image bzw. die Clientumgebung so, dass die Verbindung mit TLS 1.0 hergestellt wird.

  ```docker
  MinProtocol = TLSv1
  CipherString = DEFAULT@SECLEVEL=1
  ```

  > [!NOTE]
  > Beim Herstellen einer Verbindung mit Microsoft.Data.SqlClient v2.0+ aus einer Windows/Linux-Umgebung mit TLS 1.0 oder TLS 1.1 wird eine Sicherheitswarnung ausgelöst, wenn die SQL Server-Zielinstanz und der Client beim Herstellen der Verbindung nicht TLS 1.2 oder höher aushandeln können: `Security Warning: The negotiated <TLS1.0 | TLS1.1> is an insecure protocol and is supported for backward compatibility only. The recommended protocol version is TLS 1.2 and later.`

- Erzwungene Verschlüsselung bei SQL Server

  Wenn der Zielserver eine Azure SQL-Instanz oder eine lokale SQL Server-Instanz mit aktivierter Eigenschaft „Verschlüsselung erzwingen“ ist, wird eine verschlüsselte Verbindung hergestellt, für die der Client eine Vertrauensstellung mit dem Server einrichten muss.

  **Empfohlene Lösung**: Zum Behandeln dieses Problems sind zwei Vorgehensweisen verfügbar:

    1. Installieren Sie das TLS/SSL-Zertifikat der SQL Server-Zielinstanz in der Clientumgebung. Wenn eine Verschlüsselung erforderlich ist, wird das Zertifikat überprüft.
    2. Legen Sie in der Verbindungszeichenfolge die Eigenschaft „Trust Server Certificate = true“ fest.

  **Unsichere Lösung**: Deaktivieren Sie die Einstellung „Verschlüsselung erzwingen“ für SQL Server.

- TLS/SSL-Zertifikate, die nicht mit SHA-256 oder höher signiert sind.

  **Empfohlene Lösung**: Generieren Sie ein neues TLS/SSL-Zertifikat für den Server, dessen Hash mit dem Hashalgorithmus SHA-256 oder höher signiert ist.

- Stark eingeschränkte Verschlüsselungssammlungen unter Linux mit .NET 5 und höher

  .NET 5 brachte eine wichtige Änderung für Linux-Clients, da bei dieser Version standardmäßig stark eingeschränkte Verschlüsselungssammlungen verwendet werden. Unter Umständen müssen Sie die Liste der Standardverschlüsselungsssammlungen erweitern, damit Legacyclients akzeptiert (oder Legacyserver kontaktiert) werden. Geben Sie dazu einen `CipherSuitePolicy`-Wert an, oder ändern Sie die _OpenSSL_-Konfigurationsdatei.
  
  Weitere Informationen zu den empfohlenen Aktionen finden Sie unter [TLS-Standardverschlüsselungssammlungen für .NET unter Linux](/dotnet/core/compatibility/cryptography/5.0/default-cipher-suites-for-tls-on-linux).

### <a name="connection-pool-exhaustion-errors"></a>Fehler aufgrund eines erschöpften Verbindungspools

StackTrace beobachtet:

```log
System.InvalidOperationException: Timeout expired. The timeout period elapsed prior to obtaining a connection from the pool.
This may have occurred because all pooled connections were in use and max pool size was reached.
```

#### <a name="possible-reasons-and-solutions"></a>Mögliche Ursachen und Lösungen

Die von der Clientanwendung gestarteten Verbindungen überschreiten die maximale Anzahl von aktiven Verbindungen des Verbindungspools.

**Empfohlene Lösung**: Legen Sie für die Verbindungseigenschaft „Maximale Poolgröße“ einen höheren Wert fest, und schließen Sie nicht verwendete Verbindungen zeitnah.

## <a name="contact-support"></a>Kontaktieren des Supports

Wenn sich Ihre Konnektivitätsprobleme mit den hier beschriebenen Vorgehensweisen nicht beheben lassen, sind im [dotnet/sqlclient](https://github.com/dotnet/SqlClient)-Repository weitere Probleme und Lösungen beschrieben, die Ihnen möglicherweise weiterhelfen. Anderenfalls können Sie dort ein neues Problem erstellen.
