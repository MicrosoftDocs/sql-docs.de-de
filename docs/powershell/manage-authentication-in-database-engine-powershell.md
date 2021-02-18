---
title: Verwalten der Authentifizierung bei SQL Server in PowerShell
description: Hier erfahren Sie, wie Sie beim Herstellen einer Verbindung mit einer Instanz der Datenbank-Engine die SQL Server-Authentifizierung anstelle der Windows-Authentifizierung (Standardeinstellung) verwenden.
titleSuffix: SQL Server on Linux
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
ms.assetid: ab9212a6-6628-4f08-a38c-d3156e05ddea
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: seo-lt-2019
ms.date: 10/14/2020
ms.openlocfilehash: ed8fce6dfaebb20ebeebe9b5197948dbf2502dad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339056"
---
# <a name="manage-authentication-to-sql-server-in-powershell"></a>Verwalten der Authentifizierung bei SQL Server in PowerShell

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Standardmäßig wird von den [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] PowerShell-Komponenten beim Herstellen einer Verbindung mit einer [!INCLUDE[ssDE](../includes/ssde-md.md)]-Instanz die Windows-Authentifizierung verwendet. Sie können die SQL Server-Authentifizierung verwenden, indem Sie entweder ein virtuelles PowerShell-Laufwerk definieren oder die Parameter **-Username** und **-Password** für **Invoke-Sqlcmd** angeben.

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

## <a name="permissions"></a>Berechtigungen

Alle Aktionen, die Sie in einer [!INCLUDE[ssDE](../includes/ssde-md.md)] -Instanz ausführen können, werden über die Berechtigungen gesteuert, die den beim Verbinden mit der Instanz verwendeten Authentifizierungsinformationen erteilt wurden. Der [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Anbieter und Cmdlets verwenden standardmäßig das Windows-Konto, unter dem sie ausgeführt werden, um eine Windows-Authentifizierungsverbindung mit [!INCLUDE[ssDE](../includes/ssde-md.md)]herzustellen.  

Zum Herstellen einer [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Authentifizierungsverbindung müssen Sie eine Anmelde-ID und ein Kennwort für die SQL Server-Authentifizierung angeben. Bei Verwendung des [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Anbieters müssen Sie die [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Anmeldeinformationen einem virtuellen Laufwerk zuordnen und anschließend mithilfe des Befehls zum Ändern des Verzeichnisses (**cd**) eine Verbindung mit diesem Laufwerk herstellen. In Windows PowerShell können Sicherheitsanmeldeinformationen nur virtuellen Laufwerken zugeordnet werden.  

## <a name="sql-server-authentication-using-a-virtual-drive"></a>SQL Server-Authentifizierung mit einem virtuellen Laufwerk

### <a name="to-create-a-virtual-drive-associated-with-a-sql-server-authentication-login"></a>So erstellen Sie ein virtuelles Laufwerk mit Zuordnung zu einer SQL Server-Authentifizierungsanmeldung

1. Erstellen Sie eine Funktion, auf die Folgendes zutrifft:

    1. Sie verfügt über Parameter für den Namen, der dem virtuellen Laufwerk zugeordnet werden soll, für die Anmelde-ID und für den Anbieterpfad, der dem virtuellen Laufwerk zugeordnet werden soll.

    2. Sie verwendet **read-host** , um den Benutzer zur Eingabe des Kennworts aufzufordern.  

    3. Sie verwendet **new-object** um ein Anmeldeinformationsobjekt zu erstellen.  

    4. Sie verwendet **new-psdrive** , um ein virtuelles Laufwerk mit den angegebenen Anmeldeinformationen zu erstellen.  

2. Rufen Sie die Funktion auf, um ein virtuelles Laufwerk mit den angegebenen Anmeldeinformationen zu erstellen.  

#### <a name="example-virtual-drive"></a>Beispiel (virtuelles Laufwerk)

In diesem Beispiel wird eine Funktion mit dem Namen **sqldrive** erstellt, mit der Sie ein virtuelles Laufwerk erstellen können, das mit dem angegebenen Anmeldenamen für die [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Authentifizierung und der angegebenen Instanz verknüpft ist.  
  
 Sie werden von der Funktion **sqldrive** zur Eingabe des Kennworts für Ihren Anmeldenamen aufgefordert. Das Kennwort wird bei der Eingabe maskiert. Wenn Sie anschließend den Befehl zum Ändern eines Verzeichnisses (**cd**) verwenden, um über den Namen des virtuellen Laufwerks eine Verbindung mit einem Pfad herzustellen, werden alle Vorgänge mithilfe der Anmeldeinformationen für die [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Authentifizierung ausgeführt, die Sie beim Erstellen des Laufwerks angegeben haben.  
  
```powershell
## Create a function that specifies the login and prompts for the password.  
  
function sqldrive  
{  
    param( [string]$name, [string]$login = "MyLogin", [string]$root = "SQLSERVER:\SQL\MyComputer\MyInstance" )  
    $pwd = read-host -AsSecureString -Prompt "Password"  
    $cred = new-object System.Management.Automation.PSCredential -argumentlist $login,$pwd  
    New-PSDrive $name -PSProvider SqlServer -Root $root -Credential $cred -Scope 1  
}  
  
## Use the sqldrive function to create a SQLAuth virtual drive.  
sqldrive SQLAuth
  
## Set-Location to the virtual drive, which invokes the supplied authentication credentials.  
sl SQLAuth:
```

## <a name="sql-server-authentication-using-invoke-sqlcmd"></a>SQL Server-Authentifizierung mit Invoke-Sqlcmd

### <a name="to-use-invoke-sqlcmd-with-sql-server-authentication"></a>So verwenden Sie Invoke-Sqlcmd mit der SQL Server-Authentifizierung

1. Verwenden Sie den Parameter **-Username**, um eine Anmelde-ID anzugeben, und den Parameter **-Password**, um das zugeordnete Kennwort anzugeben.  

#### <a name="example-invoke-sqlcmd"></a>Beispiel (Invoke-Sqlcmd)

In diesem Beispiel wird das "read-host"-Cmdlet verwendet, um den Benutzer zur Eingabe des Kennworts aufzufordern, und dann unter Verwendung der SQL Server-Authentifizierung eine Verbindung hergestellt.  

```powershell
## Prompt the user for their password.  
$pwd = read-host -AsSecureString -Prompt "Password"  
  
Invoke-Sqlcmd -Query "SELECT GETDATE() AS TimeOfQuery;" -ServerInstance "MyComputer\MyInstance" -Username "MyLogin" -Password $pwd  
```

## <a name="see-also"></a>Weitere Informationen

- [SQL Server-PowerShell](sql-server-powershell.md)
- [SQL Server PowerShell-Anbieter](sql-server-powershell-provider.md)
- [Invoke-Sqlcmd-Cmdlet](/powershell/module/sqlserver/invoke-sqlcmd)
- [Verwenden von PowerShell mit Azure Data Studio](../azure-data-studio/extensions/powershell-extension.md)
