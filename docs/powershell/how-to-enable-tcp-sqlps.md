---
title: Aktivieren des TCP-Protokolls mit SQLPS
description: Erfahren Sie, wie Sie TCP-Protokolle mithilfe von SQLPS aktivieren.
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: matteot
ms.custom: ''
ms.date: 08/06/2020
ms.openlocfilehash: 6aad2599996294679d5bd20d3a0e1a71a099b00b
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837957"
---
# <a name="how-to-enable-the-tcp-protocol"></a>Aktivieren des TCP-Protokolls

## <a name="how-to-enable-the-tcp-protocol-when-connected-to-the-console-with-sqlps"></a>Aktivieren des TCP-Protokolls, wenn eine Verbindung mit der Konsole mit SQLPS besteht.

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

1. Öffnen Sie eine Eingabeaufforderung, und geben Sie Folgendes ein:

    ```console
    C:\> SQLPS.EXE
    ```

    > [!TIP]
    > Wenn SQLPS nicht gefunden wird, müssen Sie möglicherweise eine neue Eingabeaufforderung öffnen oder sich einfach abmelden und wieder anmelden.

2. Geben Sie an der PowerShell-Eingabeaufforderung Folgendes ein:

    ```powershell
    # Instantiate a ManagedComputer object which exposes primitives to control the
    # installation of SQL Server on this machine.

    $wmi = New-Object 'Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer' localhost

    # Enable the TCP protocol on the default instance. If the instance is named, 
    # replace MSSQLSERVER with the instance name in the following line.

    $tcp = $wmi.ServerInstances['MSSQLSERVER'].ServerProtocols['Tcp']
    $tcp.IsEnabled = $true  
    $tcp.Alter()  

    # You need to restart SQL Server for the change to persist
    # -Force takes care of any dependent services, like SQL Agent.
    # Note: if the instance is named, replace MSSQLSERVER with MSSQL$ followed by
    # the name of the instance (e.g. MSSQL$MYINSTANCE)

    Restart-Service -Name MSSQLSERVER -Force
    ```

## <a name="how-to-enable-the-tcp-protocol-when-connected-to-the-console-not-using-sqlps"></a>Aktivieren des TCP-Protokolls, wenn eine Verbindung mit der Konsole ohne SQLPS besteht.

1. Öffnen Sie eine Eingabeaufforderung, und geben Sie Folgendes ein:

    ```console
    C:\> PowerShell.exe
    ```

2. Geben Sie an der PowerShell-Eingabeaufforderung Folgendes ein:

    ```powershell
    # Get access to SqlWmiManagement DLL on the machine with SQL
    # we are on, which is where SQL Server was installed.
    # Note: this is installed in the GAC by SQL Server Setup.

    [System.Reflection.Assembly]::LoadWithPartialName('Microsoft.SqlServer.SqlWmiManagement')

    # Instantiate a ManagedComputer object which exposes primitives to control the
    # installation of SQL Server on this machine.

    $wmi = New-Object 'Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer' localhost

    # Enable the TCP protocol on the default instance. If the instance is named, 
    # replace MSSQLSERVER with the instance name in the following line.

    $tcp = $wmi.ServerInstances['MSSQLSERVER'].ServerProtocols['Tcp']
    $tcp.IsEnabled = $true  
    $tcp.Alter()  

    # You need to restart SQL Server for the change to persist
    # -Force takes care of any dependent services, like SQL Agent.
    # Note: if the instance is named, replace MSSQLSERVER with MSSQL$ followed by
    # the name of the instance (e.g. MSSQL$MYINSTANCE)

    Restart-Service -Name MSSQLSERVER -Force
    ```

## <a name="next-steps"></a>Nächste Schritte

- [Installieren des SQL Server PowerShell-Moduls](download-sql-server-ps-module.md)
- [SQL Server-PowerShell](sql-server-powershell.md)
- [Aktivieren oder Deaktivieren eines Servernetzwerkprotokolls](../database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md)
- [Konfigurieren von SQL Server in einer Server Core-Installation](../database-engine/install-windows/configure-sql-server-on-a-server-core-installation.md)
