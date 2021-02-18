---
title: Verwenden von sqlcmd mit Skriptvariablen
description: Hier erfahren Sie, wie Sie Skriptvariablen verwenden, um ein Skript zu erstellen, das in mehreren Szenarios verwendet werden kann.
ms.custom: seo-lt-2019
ms.date: 08/09/2016
ms.prod: sql
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- scripts [SQL Server], sqlcmd utility
- variables [SQL Server], scripts
- scripting variables [SQL Server]
- sqlcmd utility, scripts
- setvar command
ms.assetid: 793495ca-cfc9-498d-8276-c44a5d09a92c
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 799bc2eca98002343d8ff755782d8f3b71d2af77
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342642"
---
# <a name="sqlcmd---use-with-scripting-variables"></a>Verwenden von „sqlcmd“ mit Skriptvariablen
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Variablen, die in Skripts verwendet werden, werden als Skriptvariablen bezeichnet. Durch Skriptvariablen wird ein Skript aktiviert, das in verschiedenen Szenarien verwendet wird. Wenn Sie beispielsweise ein einzelnes Skript auf mehreren Servern ausführen möchten, anstatt das Skript für jeden Server zu ändern, können Sie eine Skriptvariable für den Servernamen verwenden. Durch das Ändern des Servernamens für die Skriptvariable kann das gleiche Skript auf verschiedenen Servern ausgeführt werden.  
  
 Skriptvariablen können explizit mithilfe des Befehls **setvar** oder implizit mithilfe der Option **sqlcmd -v** definiert werden.  
  
 Dieses Thema enthält auch Beispiele zum Definieren von Umgebungsvariablen an der Eingabeaufforderung von „Cmd.exe“ mithilfe von **SET**.  
  
## <a name="setting-scripting-variables-by-using-the-setvar-command"></a>Festlegen von Skriptvariablen mithilfe des setvar-Befehls  
 Der Befehl **setvar** wird zum Definieren von Skriptvariablen verwendet. Mithilfe des Befehls **setvar** definierte Variablen werden intern gespeichert. Skriptvariablen dürfen nicht mit Umgebungsvariablen verwechselt werden, die mithilfe von **SET** an der Eingabeaufforderung definiert werden. Wenn ein Skript auf eine Variable verweist, die keine Umgebungsvariable ist oder nicht mithilfe von **setvar** definiert wurde, wird eine Fehlermeldung zurückgegeben und die Ausführung des Skripts unterbrochen. Weitere Informationen finden Sie unter der Option **-b** im [Hilfsprogramm sqlcmd](../../tools/sqlcmd-utility.md).  
  
## <a name="variable-precedence-low-to-high"></a>Rangfolge der Variablen (vom niedrigsten bis zum höchsten Rang)  
 Wenn mehrere Variablentypen denselben Namen aufweisen, wird die Variable mit der höchsten Rangfolge verwendet.  
  
1.  Umgebungsvariablen auf Systemebene  
  
2.  Umgebungsvariablen auf Benutzerebene  
  
3.  Die vor dem Starten von **SET X=Y** an der Eingabeaufforderung festgelegte Befehlsshell ( **SET X=Y**)  
  
4.  **sqlcmd -v** X=Y  
  
5.  **:Setvar** X Y  
  
> [!NOTE]  
>  Öffnen Sie die **Systemsteuerung**, klicken Sie auf **System** und anschließend auf die Registerkarte **Erweitert** , um die Umgebungsvariablen anzuzeigen.  
  
## <a name="implicitly-setting-scripting-variables"></a>Implizites Festlegen von Skriptvariablen  
 Wenn Sie **sqlcmd** mit einer Option starten, die eine verknüpfte **sqlcmd** -Variable aufweist, wird die **sqlcmd** -Variable implizit auf den Wert festgelegt, der mithilfe der Option angegeben wurde. Im folgenden Beispiel beginnt `sqlcmd` mit der Option `-l` . Dadurch wird implizit die SQLLOGINTIMEOUT-Variable festgelegt.  
  
```
c:\> sqlcmd -l 60
```
 
Sie können auch die Option **-v** verwenden, um eine Skriptvariable festzulegen, die in einem Skript vorhanden ist. Im folgenden Skript (der Dateiname lautet `testscript.sql`) wird `ColumnName` als Skriptvariable verwendet.  
 
```
USE AdventureWorks2012;

SELECT x.$(ColumnName)
FROM Person.Person x
WHERE x.BusinessEntityID < 5;
```

Sie können daraufhin den Namen der Spalte angeben, die mithilfe der Option `-v` zurückgegeben werden soll:  
 
```
sqlcmd -v ColumnName ="FirstName" -i c:\testscript.sql
```

Ändern Sie den Wert der `ColumnName` -Skriptvariablen, um eine andere Spalte mithilfe desselben Skripts zurückzugeben.  
  
```
sqlcmd -v ColumnName ="LastName" -i c:\testscript.sql
```

## <a name="guidelines-for-scripting-variable-names-and-values"></a>Richtlinien für Namen und Werte von Skriptvariablen  
 Die folgenden Richtlinien sollten bei der Benennung von Skriptvariablen berücksichtigt werden:  
  
-   Variablennamen dürfen keine Leerzeichen oder Anführungszeichen enthalten.  
  
-   Variablennamen dürfen nicht die gleiche Form wie Variablenausdrücke (beispielsweise *$(var)* ) aufweisen.  
  
-   Bei Skriptvariablen wird nicht zwischen Groß- und Kleinschreibung unterschieden.  
  
    > [!NOTE]  
    >  Wenn einer **sqlcmd** -Umgebungsvariablen kein Wert zugewiesen wird, wird die Variable entfernt. Bei Verwendung von **:setvar VarName** ohne Wert wird die Variable gelöscht.  
  
 Die folgenden Richtlinien sollten beim Angeben von Werten für Skriptvariablen berücksichtigt werden:  
  
-   Variablenwerte, die mit **setvar** oder mit der Option **-v** definiert werden, müssen in Anführungszeichen eingeschlossen werden, sofern im Zeichenfolgenwert Leerzeichen enthalten sind.  
  
-   Wenn Anführungszeichen Bestandteil des Variablenwerts sind, müssen sie mit Escapezeichen versehen werden. Beispiel: :`setvar MyVar "spac""e"`.  
  
## <a name="guidelines-for-cmdexe-set-variable-values-and-names"></a>Richtlinien für Cmd.exe SET-Variablennamen und -werte  
 Mithilfe von SET definierte Variablen sind Teil der Cmd.exe-Umgebung, und es kann mit **sqlcmd** auf sie verwiesen werden. Berücksichtigen Sie die folgenden Richtlinien:  
  
-   Variablennamen dürfen keine Leerzeichen oder Anführungszeichen enthalten.  
  
-   Variablenwerte dürfen Leerzeichen oder Anführungszeichen enthalten.  
  
## <a name="sqlcmd-scripting-variables"></a>sqlcmd-Skriptvariablen  
 Mithilfe von **sqlcmd** definierte Variablen werden als Skriptvariablen bezeichnet. In der folgenden Tabelle sind die **sqlcmd** -Skriptvariablen aufgelistet.  
  
|        Variable         | Zugehörige Option | R/W |         Standard         |
| ----------------------- | -------------- | --- | ----------------------- |
| SQLCMDUSER\*             | -U             | R   | ""                      |
| SQLCMDPASSWORD\*         | -P             | --  | ""                      |
| SQLCMDSERVER\*           | -S             | R   | "DefaultLocalInstance"  |
| SQLCMDWORKSTATION       | -H             | R   | "ComputerName"          |
| SQLCMDDBNAME            | -d             | R   | ""                      |
| SQLCMDLOGINTIMEOUT      | -l             | R/W | "8" (Sekunden)           |
| SQLCMDSTATTIMEOUT       | -t             | R/W | "0" = unbegrenzt warten |
| SQLCMDHEADERS           | -H             | R/W | "0"                     |
| SQLCMDCOLSEP            | -S             | R/W | " "                     |
| SQLCMDCOLWIDTH          | -w             | R/W | "0"                     |
| SQLCMDPACKETSIZE        | -a             | R   | "4096"                  |
| SQLCMDERRORLEVEL        | -M             | R/W | "0"                     |
| SQLCMDMAXVARTYPEWIDTH   | -y             | R/W | "256"                   |
| SQLCMDMAXFIXEDTYPEWIDTH | -y             | R/W | "0" = unbegrenzt         |
| SQLCMDEDITOR            |                | R/W | "edit.com"              |
| SQLCMDINI               |                | R   | ""                      |

SQLCMDUSER, SQLCMDPASSWORD und SQLCMDSERVER werden festgelegt, wenn **:Connect** verwendet wird.  

Durch R wird angezeigt, dass der Wert nur einmal während der Programminitialisierung festgelegt werden kann.  
  
Durch R/W wird angezeigt, dass der Wert mithilfe des Befehls **setvar** zurückgesetzt werden kann. Für nachfolgende Befehle wird der neue Wert verwendet.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-using-the-setvar-command-in-a-script"></a>A. Verwenden des setvar-Befehls in einem Skript  
 Viele **sqlcmd** -Optionen können in einem Skript mithilfe des Befehls **setvar** gesteuert werden. Im folgenden Beispiel wird das Skript `test.sql` erstellt, in dem die Variable `SQLCMDLOGINTIMEOUT` auf `60` Sekunden festgelegt ist. Eine weitere Skriptvariable ( `server`) wird auf `testserver`festgelegt. Der folgende Code befindet sich in `test.sql`.  

```
:setvar SQLCMDLOGINTIMEOUT 60
:setvar server "testserver"
:connect $(server) -l $(SQLCMDLOGINTIMEOUT)

USE AdventureWorks2012;

SELECT FirstName, LastName
FROM Person.Person;
```

Das Skript wird anschließend mit „sqlcmd“ aufgerufen:

```
sqlcmd -i c:\test.sql
```
  
### <a name="b-using-the-setvar-command-interactively"></a>B. Interaktives Verwenden des setvar-Befehls  
 Im folgenden Beispiel wird veranschaulicht, wie eine Skriptvariable mithilfe des `setvar` -Befehls interaktiv festgelegt wird.  

```
sqlcmd
:setvar  MYDATABASE AdventureWorks2012
USE $(MYDATABASE);
GO
```

 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
Changed database context to 'AdventureWorks2012'
1>
```
  
### <a name="c-using-command-prompt-environment-variables-within-sqlcmd"></a>C. Verwenden von Eingabeaufforderung-Umgebungsvariablen innerhalb von "sqlcmd"  
 `are` Im folgenden Beispiel werden vier Umgebungsvariablen festgelegt und dann von `sqlcmd`aufgerufen.  

```
C:\>SET tablename=Person.Person
C:\>SET col1=FirstName
C:\>SET col2=LastName
C:\>SET title=Ms.
C:\>sqlcmd -d AdventureWorks2012
1> SELECT TOP 5 $(col1) + ' ' + $(col2) AS Name
2> FROM $(tablename)
3> WHERE Title ='$(title)'
4> GO
```
  
### <a name="d-using-user-level-environment-variables-within-sqlcmd"></a>D: Verwenden von Umgebungsvariablen auf Benutzerebene in "sqlcmd"  
 Im folgenden Beispiel wird die `%Temp%` -Umgebungsvariable auf Benutzerebene an der Eingabeaufforderung festgelegt und an die `sqlcmd` -Eingabedatei übergeben. Zum Abrufen der Umgebungsvariable auf Benutzerebene doppelklicken Sie unter **Systemsteuerung** auf **System**. Klicken Sie auf die Registerkarte **Erweitert** , und klicken Sie dann auf **Umgebungsvariablen**.  
  
 In der Eingabedatei `c:\testscript.txt`ist der folgende Code enthalten:

```
:OUT $(MyTempDirectory)
USE AdventureWorks2012;

SELECT FirstName
FROM AdventureWorks2012.Person.Person
WHERE BusinessEntityID` `< 5;
```

Der folgende Code wird an der Eingabeaufforderung eingegeben:

```
C:\ >SET MyTempDirectory=%Temp%\output.txt
C:\ >sqlcmd -i C:\testscript.txt
```

 Das folgende Ergebnis wird an die Ausgabedatei „C:\Dokumente und Einstellungen\\<Benutzer\>\Lokale Einstellungen\Temp\output.txt“ gesendet.  

```
Changed database context to 'AdventureWorks2012'.
FirstName
--------------------------------------------------
Gustavo
Catherine
Kim
Humberto

(4 rows affected)
```

### <a name="e-using-a-startup-script"></a>E. Verwenden eines Startskripts  
 Beim Starten von **sqlcmd** wird ein **sqlcmd** -Startskript ausgeführt. Im folgenden Beispiel wird die Umgebungsvariable `SQLCMDINI`festgelegt. Dies ist der Inhalt von `init.sql.`  

```
SET NOCOUNT ON
GO

DECLARE @nt_username nvarchar(128)
SET @nt_username = (SELECT rtrim(convert(nvarchar(128), nt_username))
FROM sys.dm_exec_sessions WHERE spid = @@SPID)
SELECT  @nt_username + ' is connected to ' +
rtrim(CONVERT(nvarchar(20), SERVERPROPERTY('servername'))) +
' (' +`  
rtrim(CONVERT(nvarchar(20), SERVERPROPERTY('productversion'))) +
')'
:setvar SQLCMDMAXFIXEDTYPEWIDTH 100
SET NOCOUNT OFF
GO

:setvar SQLCMDMAXFIXEDTYPEWIDTH
```

 Damit wird die Datei `init.sql` beim Starten von `sqlcmd` aufgerufen.  
  
```
c:\> SET sqlcmdini=c:\init.sql
>1 Sqlcmd
```

 Dies ist die Ausgabe.  

```
>1 < user > is connected to < server > (9.00.2047.00)
```

  
> [!NOTE]  
>  Mit der Option **-X** wird die Startskriptfunktion deaktiviert.  
  
### <a name="f-variable-expansion"></a>F. Variablenerweiterung  
 Im folgenden Beispiel wird die Verwendung von Daten in Form einer **sqlcmd** -Variablen veranschaulicht.  

```
USE AdventureWorks2012;
CREATE TABLE AdventureWorks2012.dbo.VariableTest
(
Col1 nvarchar(50)
);
GO
```

 Fügen Sie eine Zeile in `Col1` von `dbo.VariableTest` ein, in der der Wert `$(tablename)`enthalten ist.  

```
INSERT INTO AdventureWorks2012.dbo.VariableTest(Col1)
VALUES('$(tablename)');
GO
```
  
 Wenn keine Variable auf einen Wert gleich `sqlcmd` festgelegt ist, wird die Zeile an der `$(tablename)`-Eingabeaufforderung mit folgenden Anweisungen zurückgegeben:  
  
```
C:\> sqlcmd
>1 SELECT Col1 FROM dbo.VariableTest WHERE Col1 = '$(tablename)';
>2 GO
>3 SELECT Col1 FROM dbo.VariableTest WHERE Col1 = N'$(tablename)';
>4 GO
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  

```
>1 Col1
>2 ------------------
>3 $(tablename)
>4
>5 (1 rows affected)
```

 Es wird angenommen, dass die Variable `MyVar` auf `$(tablename)`festgelegt ist.  

```
>6 :setvar MyVar $(tablename)
```

 Die Zeile wird mit diesen Anweisungen zurückgegeben. Außerdem wird die Meldung "Die 'tablename'-Skriptvariable ist nicht definiert." zurückgegeben.  

```
>6 SELECT Col1 FROM dbo.VariableTest WHERE Col1 = '$(tablename)';
>7 GO

>1 SELECT Col1 FROM dbo.VariableTest WHERE Col1 = N'$(tablename)';
>2 GO
```

 Die Zeile wird mit diesen Anweisungen zurückgegeben.  

```
>1 SELECT Col1 FROM dbo.VariableTest WHERE Col1 = '$(MyVar)';
>2 GO
```

```
>1 SELECT Col1 FROM dbo.VariableTest WHERE Col1 = N'$(MyVar)';
>2 GO
```
  
## <a name="see-also"></a>Weitere Informationen  
 [Verwenden des Hilfsprogramms „sqlcmd“](./sqlcmd-use-the-utility.md)   
 [sqlcmd (Hilfsprogramm)](../../tools/sqlcmd-utility.md)   
 [Referenz zum Eingabeaufforderungs-Hilfsprogramm &#40;Datenbank-Engine&#41;](../../tools/command-prompt-utility-reference-database-engine.md)  
  
