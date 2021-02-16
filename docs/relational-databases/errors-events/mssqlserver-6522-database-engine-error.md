---
description: MSSQLSERVER_6522
title: MSSQLSERVER_6522
ms.custom: ''
ms.date: 12/25/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, vencher, tejasaks, docast
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 6522 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 7e43d78e751ac0b98d8d4da969ac6c8c98b599f0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348737"
---
# <a name="mssqlserver_6522"></a>MSSQLSERVER_6522
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Details

|attribute|Wert|
|---|---|
|Produktname|SQL Server|
|Ereignis-ID|6522|
|Ereignisquelle|MSSQLSERVER|
|Komponente|SQLEngine|
|Symbolischer Name|SQLCLR_UDF_EXEC_FAILED|
|Meldungstext|.NET Framework-Fehler beim Ausführen der benutzerdefinierten Routine oder des benutzerdefinierten Aggregats „%.*ls“: %ls.|
||

## <a name="explanation"></a>Erklärung

Betrachten Sie folgende Szenarien:

### <a name="scenario-1"></a>Szenario 1

Sie erstellen eine Common Language Runtime (CLR)-Routine, die auf eine Microsoft .NET Framework-Assembly verweist. Die .NET Framework-Assembly ist in [922672](/troubleshoot/sql/database-design/policy-untested-net-framework-assemblies)nicht dokumentiert. Anschließend installieren Sie den .NET Framework 3.5- oder einen .NET Framework 2.0-basierten Hotfix.

### <a name="scenario-2"></a>Szenario 2

Sie erstellen eine Assembly und registrieren diese dann in einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbank. Anschließend installieren Sie eine andere Version der Assembly im globalen Assemblycache (GAC).

Wenn Sie die CLR-Routine ausführen oder die Assembly aus einem dieser Szenarien in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwenden, erhalten Sie eine Fehlermeldung, die der folgenden ähnelt:

> Server:  Meldung 6522, Ebene 16, Status 2, Zeile 1  
.NET Framework-Fehler beim Ausführen der benutzerdefinierten Routine oder des benutzerdefinierten Aggregats „getsid“:
>
> System.IO.FileLoadException: Die Datei oder Assembly „System.DirectoryServices, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a“ oder eine Abhängigkeit davon wurde nicht gefunden. Die Assembly im Hostspeicher weist eine andere Signatur als die Assembly im GAC auf. (Ausnahme von HRESULT: 0x80131050)

## <a name="possible-cause"></a>Mögliche Ursache

Wenn die CLR eine Assembly lädt, prüft sie, ob sich die gleiche Assembly im GAC befindet. Wenn sich die gleiche Assembly im GAC befindet, überprüft die CLR, ob die Modulversions-IDs (MVIDs) dieser Assemblys übereinstimmen. Wenn die MVIDs dieser Assemblys nicht übereinstimmen, erhalten Sie die Fehlermeldung, die im Abschnitt [Erklärung](#explanation) erwähnt wird.

Wird eine Assembly neu kompiliert, ändert sich die MVID der Assembly. Wenn Sie also .NET Framework aktualisieren, weisen die .NET Framework-Assemblys andere MVIDs auf, da diese Assemblys neu kompiliert werden. Auch wenn Sie Ihre eigene Assembly aktualisieren, wird sie neu kompiliert. Daher weist die Assembly ebenfalls eine andere MVID auf.

## <a name="user-action"></a>Benutzeraktion

### <a name="action-1"></a>Aktion 1

Um Szenario 1 im Abschnitt [Erklärung](#explanation) zu umgehen, müssen Sie die .NET Framework-Assemblys in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aktualisieren. Verwenden Sie hierzu die `ALTER ASSEMBLY`-Anweisung, um auf die neue Version der .NET Framework Assembly im folgenden Ordner zu verweisen:

`%Windir%\Microsoft.NET\Framework\Version`

> [!NOTE]
> **Version** gibt die .NET Framework-Version an, die Sie installiert oder aktualisiert haben.

### <a name="action-2"></a>Aktion 2

Um Szenario 2 im Abschnitt [Erklärung](#explanation) zu umgehen, verwenden Sie die `ALTER ASSEMBLY`-Anweisung, um die Assembly in der Datenbank zu aktualisieren.

Wenn das Problem auch nach dem Ausführen dieser Schritte fortbesteht, löschen Sie die Assembly aus der Datenbank, und registrieren Sie dann die neue Version der Assembly in der Datenbank.

## <a name="more-information"></a>Weitere Informationen

Es wird nicht empfohlen, .NET Framework-Assemblys zu verwenden, die nicht in der [Supportrichtlinie für ungetestete .NET Framework-Assemblys in der von der SQL Server-CLR gehosteten Umgebung](/troubleshoot/sql/database-design/policy-untested-net-framework-assemblies) dokumentiert sind. In dieser sind die Assemblys aufgelistet, die in der von der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-CLR gehosteten Umgebung getestet wurden.

### <a name="description-of-clr-routines"></a>Beschreibung von CLR-Routinen

CLR-Routinen enthalten die folgenden Objekte, die mithilfe der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Integration in die .NET Framework-CLR implementiert werden:

- Benutzerdefinierte Skalarwertfunktionen (Skalar-UDFs)
- Benutzerdefinierte Tabellenwertfunktionen (TVFs)
- Benutzerdefinierte Prozeduren (UDPs)
- Benutzerdefinierte Trigger
- Benutzerdefinierte Datentypen
- Benutzerdefinierte Aggregate

### <a name="assemblies-to-update-after-you-install-the-net-framework-35"></a>Assemblys, die nach der Installation von .NET Framework 3.5 aktualisiert werden sollen

Nachdem Sie .NET Framework 3.5 installiert haben, müssen Sie die Anweisung ALTER ASSEMBLY verwenden, um die folgenden Assemblys zu aktualisieren:

- Accessibility.dll
- AspNetMMCExt.dll
- Cscompmgd.dll
- IEExecRemote.dll
- IEHost.dll
- IIEHost.dll
- Microsoft.Build.Conversion.dll
- Microsoft.Build.Engine.dll
- Microsoft.Build.Framework.dll
- Microsoft.Build.Tasks.dll 
- Microsoft.Build.Utilities.dll 
- Microsoft.CompactFramework.Build.Tasks.dll 
- Microsoft.JScript.dll 
- Microsoft.VisualBasic.Vsa.dll 
- Microsoft.Vsa.dll 
- Microsoft.Vsa.Vb.CodeDOMProcessor.dll 
- Microsoft_VsaVb.dll 
- Sysglobl.dll 
- System.Configuration.Install.dll 
- System.Design.dll 
- System.DirectoryServices.dll 
- System.DirectoryServices.Protocols.dll 
- System.Drawing.dll 
- System.Drawing.Design.dll 
- System.EnterpriseServices.dll 
- System.Management.dll 
- System.Messaging.dll 
- System.Runtime.Serialization.Formatters.Soap.dll 
- System.ServiceProcess.dll 
- System.Web.dll 
- System.Web.Mobile.dll 
- System.Web.RegularExpressions.dll 

Diese Assemblies befinden sich im folgenden Ordner:

`%Windir%\Microsoft.NET\Framework\v2.0.50727`

### <a name="how-to-preserve-the-data-from-user-defined-data-types-after-you-drop-an-assembly"></a>Erhalten der Daten von benutzerdefinierten Datentypen nach dem Löschen einer Assembly

Wenn Sie eine Assembly löschen, die von einem benutzerdefinierten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datentyp verwendet wird, können Sie eine der folgenden Methoden verwenden, um die Daten beizubehalten.

Angenommen, das Szenario sieht folgendermaßen aus:

- Sie erstellen eine Assembly, deren Name *MyAssembly.dll* lautet.
- Die Assembly „MyAssembly „ verweist auf die `System.DirectoryServices.dll`-Assembly.
- Sie verfügen über einen benutzerdefinierten Datentyp mit dem Namen *MyDateTime*.
- Der Datentyp *MyDateTime* verwendet die Assembly *MyAssembly.dll*.
- Sie erstellen eine Tabelle mit dem Namen *MyTable*.
- Die Tabelle *MyTable* enthält die Daten des Datentyps *MyDateTime*.

#### <a name="method-1-use-the-bcpexe-utility"></a>Methode 1: Verwenden des Hilfsprogramms „Bcp. exe“

1. Verwenden Sie das Hilfsprogramm „Bcp.exe“ zusammen mit dem Schalter „-n“, um die Daten aus der Tabelle „MyTable“ in eine Datei zu kopieren. Führen Sie an der Eingabeaufforderung beispielsweise den folgenden Befehl aus:

    ```console
    bcp MyDatabase.dbo.MyTable out C:\MyFile.bcp -n -SSQLServerName -T
    ```

2. Führen Sie in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Studio die folgenden Schritte aus:
   1. Löschen Sie die MyTable-Tabelle.
   2. Löschen Sie den MyDateTime-Datentyp.
   3. Löschen Sie die `System.DirectoryServices.dll`-Assembly.
   4. Löschen Sie die MyAssembly-Assembly.
3. Führen Sie in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Studio die folgenden Schritte aus:

   1. Registrieren Sie die `System.DirectoryServices.dll`-Assembly.
   2. Registrieren Sie die MyAssembly-Assembly.
   3. Erstellen Sie den MyDateTime-Datentyp.
   4. Erstellen Sie eine neue Tabelle, die die gleiche Tabellenstruktur wie die Tabelle „MyTable“ aufweist.
4. Verwenden Sie das Hilfsprogramm „Bcp.exe“ zusammen mit dem Schalter „-n“, um die Daten aus der Datei in die Tabelle „MyTable“ zu importieren. Führen Sie an der Eingabeaufforderung beispielsweise den folgenden Befehl aus:
  
    ```console
    bcp MyDatabase.dbo.MyTable in C:\MyFile.bcp -n -SSQLServerName -T
    ```

#### <a name="method-2-use-the-insert--select-statement"></a>Methode 2: Verwenden der INSERT ... SELECT-Anweisung

Angenommen, der MyDateTime-Datentyp belegt 9 Byte im Speicher.

1. Erstellen Sie in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Studio eine neue Tabelle, die eine Spalte des Datentyps `VARBINARY(9)` enthält, indem Sie die folgende Anweisung ausführen:

    ```sql
    CREATE TABLE TempTable (c1 VARBINARY(9));
    ```

2. Führen Sie die folgende INSERT ... SELECT-Anweisung aus, um die TempTable-Tabelle aufzufüllen:

    ```sql
    INSERT INTO TempTable SELECT CAST(c1 as VARBINARY(9)) FROM MyTable;
    ```

3. Führen Sie in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Studio die folgenden Schritte aus:
   1. Löschen Sie die MyTable-Tabelle.
   2. Löschen Sie den MyDateTime-Datentyp.
   3. Löschen Sie die Assembly „System.DirectoryServices.dll“.
   4. Löschen Sie die MyAssembly-Assembly.
4. Führen Sie in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Studio die folgenden Schritte aus:
   1. Registrieren Sie die Assembly „System.DirectoryServices.dll“.
   2. Registrieren Sie die MyAssembly-Assembly.
   3. Erstellen Sie den MyDateTime-Datentyp.
   4. Erstellen Sie eine neue Tabelle, die die gleiche Tabellenstruktur wie die Tabelle „MyTable“ aufweist.
5. Führen Sie die folgende INSERT ... SELECT-Anweisung aus, um die MyTable-Tabelle aufzufüllen:

    ```sql
    INSERT INTO MyTable SELECT c1 FROM TempTable;
    ```

## <a name="references"></a>Referenzen

- Weitere Informationen zur Assemblyversion finden Sie in der [Dokumentation zu Visual Studio 2005 (veraltet)](https://www.microsoft.com/download/details.aspx?id=55984).
- Weitere Informationen zum Aktualisieren einer Assembly finden Sie unter [ALTER ASSEMBLY (Transact-SQL)](../../t-sql/statements/alter-assembly-transact-sql.md).
- Weitere Informationen zum Löschen einer Assembly finden Sie unter [DROP ASSEMBLY (Transact-SQL)](../../t-sql/statements/drop-assembly-transact-sql.md).
- Weitere Informationen zum Registrieren einer Assembly in einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbank finden Sie unter [CREATE ASSEMBLY (Transact-SQL)](../../t-sql/statements/create-assembly-transact-sql.md).
- Weitere Informationen zum Hilfsprogramm „Bcp.exe“ finden Sie unter [https://msdn2.microsoft.com/library/ms162802.aspx](../../tools/bcp-utility.md).