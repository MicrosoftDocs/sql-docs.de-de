---
title: Verwalten von SQL Server für Linux mit PowerShell
description: Informieren Sie sich über SQL Server PowerShell, und sehen Sie sich einige Beispiele zur Verwendung von Windows mit SQL Server für Linux an.
author: VanMSFT
ms.author: vanto
ms.date: 10/02/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: a3492ce1-5d55-4505-983c-d6da8d1a94ad
ms.openlocfilehash: d10b498569bac6a166ded3eaf9bd03f23ea6b718
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345635"
---
# <a name="use-powershell-on-windows-to-manage-sql-server-on-linux"></a>Verwenden von PowerShell unter Windows zum Verwalten von SQL Server für Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

In diesem Artikel werden [SQL Server PowerShell](../powershell/sql-server-powershell.md) und einige Beispiele zur Verwendung mit SQL Server für Linux vorgestellt. PowerShell für SQL Server wird derzeit unter Windows, macOS und Linux unterstützt. In diesem Artikel werden die Schritte zur Verwendung eines Windows-Computers zum Herstellen einer Verbindung mit einer SQL Server-Remoteinstanz unter Linux beschrieben.

## <a name="install-the-newest-version-of-sql-powershell-on-windows"></a>Installieren der neuesten Version von SQL PowerShell für Windows

[SQL PowerShell](../powershell/download-sql-server-ps-module.md) für Windows wird im PowerShell-Katalog verwaltet. Wenn Sie mit SQL Server arbeiten, sollten Sie immer die neueste Version der PowerShell-Moduls „SqlServer“ verwenden.

## <a name="before-you-begin"></a>Voraussetzungen

Informieren Sie sich über die [bekannten Probleme](sql-server-linux-release-notes.md) von SQL Server für Linux.

## <a name="launch-powershell-and-import-the-sqlserver-module"></a>Starten Sie PowerShell, und importieren Sie das Modul *SqlServer*.

Starten Sie zunächst PowerShell unter Windows. Drücken Sie auf Ihrem Windows-Computer <kbd>WINDOWS</kbd>+<kbd>R</kbd>, und geben Sie **PowerShell** ein, um eine neue Windows PowerShell-Sitzung zu starten.

```
PowerShell
```

SQL Server stellt ein PowerShell-Modul namens **SqlServer** bereit. Sie können das Modul **SqlServer** verwenden, um die SQL Server-Komponenten (SQL Server-Anbieter und -Cmdlets) in eine PowerShell-Umgebung oder ein PowerShell-Skript zu importieren.

Kopieren Sie den folgenden Befehl in die PowerShell-Eingabeaufforderung, um das Modul **SqlServer** in Ihre aktuelle PowerShell-Sitzung zu importieren:

```powershell
Import-Module SqlServer
```

Geben Sie den folgenden Befehl in die PowerShell-Eingabeaufforderung ein, um zu überprüfen, ob das **SqlServer**-Modul ordnungsgemäß importiert wurde:

```powershell
Get-Module -Name SqlServer
```

PowerShell sollte Informationen ähnlich der folgenden Ausgabe anzeigen:

```
ModuleType Version    Name          ExportedCommands
---------- -------    ----          ----------------
Script     21.1.18102 SqlServer     {Add-SqlAvailabilityDatabase, Add-SqlAvailabilityGroupList...
```

## <a name="connect-to-sql-server-and-get-server-information"></a>Herstellen einer Verbindung mit SQL Server und Abrufen von Serverinformationen

Im Folgenden verwenden Sie PowerShell unter Windows, um eine Verbindung mit Ihrer SQL Server-Instanz unter Linux herzustellen und einige Servereigenschaften anzuzeigen.

Kopieren Sie die folgenden Befehle in die PowerShell-Eingabeaufforderung. Wenn Sie diese Befehle ausführen, führt PowerShell Folgendes aus:
- Ein Dialogfeld wird angezeigt, das Sie auffordert, den Hostnamen oder die IP-Adresse Ihrer Instanz einzugeben.
- Das Dialogfeld *Bei Windows PowerShell anmelden* wird angezeigt, das Sie auffordert, die Anmeldeinformationen einzugeben. Sie können Ihren *SQL-Benutzernamen* und Ihr *SQL-Kennwort* verwenden, um eine Verbindung mit Ihrer SQL Server-Instanz unter Linux herzustellen.
- Das Cmdlet **Get-SqlInstance** wird verwendet, um eine Verbindung mit dem **Server** herzustellen und einige Eigenschaften anzuzeigen.

Optional können Sie einfach die `$serverInstance`-Variable durch die IP-Adresse oder den Hostnamen Ihrer SQL Server-Instanz ersetzen.

```powershell
# Prompt for instance & credentials to login into SQL Server
$serverInstance = Read-Host "Enter the name of your instance"
$credential = Get-Credential

# Connect to the Server and get a few properties
Get-SqlInstance -ServerInstance $serverInstance -Credential $credential
# done
```

PowerShell sollte Informationen ähnlich der folgenden Ausgabe anzeigen:

```
Instance Name                   Version    ProductLevel UpdateLevel  HostPlatform HostDistribution                
-------------                   -------    ------------ -----------  ------------ ----------------                
your_server_instance            14.0.3048  RTM          CU13         Linux        Ubuntu 
```
> [!NOTE]
> Wenn für diese Werte nichts angezeigt wird, ist beim Herstellen einer Verbindung mit der SQL Server-Zielinstanz höchstwahrscheinlich ein Fehler aufgetreten. Stellen Sie sicher, dass Sie dieselben Verbindungsinformationen verwenden können, um eine Verbindung über SQL Server Management Studio herzustellen. Überprüfen Sie anschließend die [Empfehlungen zur Verbindungsproblembehandlung](sql-server-linux-troubleshooting-guide.md#connection).

## <a name="using-the-sql-server-powershell-provider"></a>Verwenden des SQL Server PowerShell-Anbieters

Die Verwendung des [SQL Server PowerShell-Anbieters](../powershell/sql-server-powershell-provider.md) ist eine weitere Option zum Herstellen einer Verbindung mit Ihrer SQL Server-Instanz.  Dieser Anbieter ermöglicht Ihnen die Navigation Ihrer SQL Server-Instanz über die Befehlszeile, als würden Sie in der Struktur im Objekt-Explorer navigieren.  Dieser Anbieter wird standardmäßig als PSDrive (PowerShell-Laufwerk) namens `SQLSERVER:\` dargestellt, das Sie verwenden können, um Verbindungen mit SQL Server-Instanzen herzustellen, auf die Ihr Domänenkonto zugreifen kann, und in diesen zu navigieren.  Informationen zum Einrichten der Active Directory-Authentifizierung für SQL Server für Linux finden Sie in den [Konfigurationsschritten](./sql-server-linux-active-directory-auth-overview.md#configuration-steps).

Für den SQL Server PowerShell-Anbieter können Sie auch die SQL-Authentifizierung verwenden. Verwenden Sie hierzu das Cmdlet `New-PSDrive`, um einen neuen PSDrive zu erstellen und die entsprechenden Anmeldeinformationen zum Herstellen einer Verbindung bereitzustellen.

Im folgenden Beispiel wird veranschaulicht, wie ein neuer PSDrive mithilfe der SQL-Authentifizierung erstellt wird.

```powershell
# NOTE: We are reusing the values saved in the $credential variable from the above example.
New-PSDrive -Name SQLonDocker -PSProvider SqlServer -Root 'SQLSERVER:\SQL\localhost,10002\Default\' -Credential $credential
```

Sie können überprüfen, ob das Laufwerk erstellt wurde, indem Sie das Cmdlet `Get-PSDrive` ausführen.

```powershell
Get-PSDrive
```

Sobald Sie das neue PSDrive erstellt haben, können Sie damit beginnen, darin zu navigieren.

```powershell
dir SQLonDocker:\Databases
```

Die Ausgabe kann wie folgt aussehen.  Möglicherweise stellen Sie fest, dass die Ausgabe der Anzeige von SSMS des Datenbankknotens ähnelt.  Es werden die Benutzerdatenbanken angezeigt, aber nicht die Systemdatenbanken.

```powershell
Name                 Status           Size     Space  Recovery Compat. Owner
                                            Available  Model     Level
----                 ------           ---- ---------- -------- ------- -----
AdventureWorks2016   Normal      209.63 MB    1.31 MB Simple       130 sa
AdventureWorksDW2012 Normal      167.00 MB   32.47 MB Simple       110 sa
AdventureWorksDW2014 Normal      188.00 MB   78.10 MB Simple       120 sa
AdventureWorksDW2016 Normal      172.00 MB   74.76 MB Simple       130 sa
AdventureWorksDW2017 Normal      208.00 MB   40.57 MB Simple       140 sa
```

Wenn Sie alle Datenbanken in Ihrer Instanz anzeigen müssen, bietet sich beispielsweise das Cmdlet [Get-SqlDatabase](/powershell/module/sqlserver/Get-SqlDatabase) an.

## <a name="examine-sql-server-error-logs"></a>Untersuchen von SQL Server-Fehlerprotokollen

In den folgenden Schritten wird PowerShell unter Windows zum Untersuchen von Fehlerprotokollen zu Ihrer SQL Server-Instanz verwendet. Außerdem wird das Cmdlet **Out-GridView** verwendet, um Informationen aus den Fehlerprotokollen in einer Rasteransicht darzustellen.

Kopieren Sie die folgenden Befehle in die PowerShell-Eingabeaufforderung. Die Ausführung dieser Befehle kann einige Minuten dauern. Diese Befehle bewirken Folgendes:
- Ein Dialogfeld wird angezeigt, das Sie auffordert, den Hostnamen oder die IP-Adresse Ihrer Instanz einzugeben.
- Das Dialogfeld *Bei Windows PowerShell anmelden* wird angezeigt, das Sie auffordert, die Anmeldeinformationen einzugeben. Sie können Ihren *SQL-Benutzernamen* und Ihr *SQL-Kennwort* verwenden, um eine Verbindung mit Ihrer SQL Server-Instanz unter Linux herzustellen.
- Das Cmdlet **Get-SqlErrorLog** wird verwendet, um eine Verbindung mit der SQL Server-Instanz unter Linux herzustellen und die Fehlerprotokolle abzurufen, die seit gestern (**Yesterday**) aufgetreten sind.
- Die Ausgabe wird an das Cmdlet **Out-GridView** übergeben.

Alternativ dazu können Sie die `$serverInstance`-Variable durch die IP-Adresse oder den Hostnamen Ihrer SQL Server-Instanz ersetzen.

```powershell
# Prompt for instance & credentials to login into SQL Server
$serverInstance = Read-Host "Enter the name of your instance"
$credential = Get-Credential

# Retrieve error logs since yesterday
Get-SqlErrorLog -ServerInstance $serverInstance -Credential $credential -Since Yesterday | Out-GridView
# done
```
## <a name="see-also"></a>Weitere Informationen
- [SQL Server-PowerShell](../powershell/sql-server-powershell.md)
- [SqlServer-Cmdlets](/powershell/module/sqlserver)