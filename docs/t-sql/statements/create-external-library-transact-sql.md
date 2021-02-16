---
description: CREATE EXTERNAL LIBRARY (Transact-SQL) – SQL Server
title: CREATE EXTERNAL LIBRARY (Transact-SQL) – SQL Server | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/28/2021
ms.prod: sql
ms.reviewer: ''
ms.technology: machine-learning
ms.topic: reference
f1_keywords:
- CREATE EXTERNAL LIBRARY
- CREATE_EXTERNAL_LIBRARY_TSQL
- EXTERNAL LIBRARY
- EXTERNAL_LIBRARY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE EXTERNAL LIBRARY
author: dphansen
ms.author: davidph
manager: cgronlund
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 94de0210201668f5ce7096c93220c7776aa5e9d0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345254"
---
# <a name="create-external-library-transact-sql"></a>CREATE EXTERNAL LIBRARY (Transact-SQL)  
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
Lädt R-, Python- oder Java-Paketdateien vom angegebenen Bytedatenstrom oder Dateipfad in eine Datenbank hoch. Diese Anweisung dient als generischer Mechanismus, mithilfe dessen der Datenbankadministrator Artefakte hochladen kann, die von Runtimes neuer externer Sprachen und Betriebssystemplattformen benötigt werden, die von [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] unterstützt werden. 

> [!NOTE]
> In SQL Server 2017 werden die R-Sprache und die Windows-Plattform unterstützt. Ab SQL Server 2019 werden R, Python und externe Programmiersprachen auf Windows- und Linux-Plattformen unterstützt.
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
Lädt R- oder Python-Paketdateien vom angegebenen Bytedatenstrom oder Dateipfad in eine Datenbank hoch. Diese Anweisung dient als generischer Mechanismus für den Datenbankadministrator zum Hochladen von benötigten Artefakten. 

> [!NOTE]
> In Azure SQL Managed Instance können Sie **sqlmlutils** verwenden, um eine Bibliothek zu installieren. Weitere Informationen finden Sie unter [Installieren von Python-Paketen mit sqlmlutils](../../machine-learning/package-management/install-additional-python-packages-on-sql-server.md?context=%252fazure%252fazure-sql%252fmanaged-instance%252fcontext%252fml-context&view=azuresqldb-mi-current&preserve-view=true) und [Installieren von neuen R-Paketen mit sqlmlutils](../../machine-learning/package-management/install-additional-r-packages-on-sql-server.md?context=%252fazure%252fazure-sql%252fmanaged-instance%252fcontext%252fml-context&view=azuresqldb-mi-current&preserve-view=true).
::: moniker-end

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
## <a name="syntax-for-sql-server-2019"></a>Syntax für SQL Server 2019

```syntaxsql
CREATE EXTERNAL LIBRARY library_name  
[ AUTHORIZATION owner_name ]  
FROM <file_spec> [ ,...2 ]  
WITH ( LANGUAGE = <language> )  
[ ; ]  

<file_spec> ::=  
{  
    (CONTENT = { <client_library_specifier> | <library_bits> }  
    [, PLATFORM = <platform> ])  
}  

<client_library_specifier> :: = 
{
    '[file_path\]manifest_file_name'  
} 

<library_bits> :: =  
{ 
      varbinary_literal 
    | varbinary_expression 
}

<platform> :: = 
{
      WINDOWS
    | LINUX
}

<language> :: = 
{
      'R'
    | 'Python'
    | <external_language>
}

```
::: moniker-end
::: moniker range=">=sql-server-2017 <=sql-server-2017"
## <a name="syntax-for-sql-server-2017"></a>Syntax für SQL Server 2017

```syntaxsql
CREATE EXTERNAL LIBRARY library_name  
[ AUTHORIZATION owner_name ]  
FROM <file_spec> [ ,...2 ]  
WITH ( LANGUAGE = 'R' )  
[ ; ]  

<file_spec> ::=  
{  
    (CONTENT = { <client_library_specifier> | <library_bits> })  
}  

<client_library_specifier> :: = 
{
    '[file_path\]manifest_file_name'
} 

<library_bits> :: =  
{ 
      varbinary_literal 
    | varbinary_expression 
}
```
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
## <a name="syntax-for-azure-sql-managed-instance"></a>Syntax für Azure SQL Managed Instance

```syntaxsql
CREATE EXTERNAL LIBRARY library_name  
[ AUTHORIZATION owner_name ]  
FROM <file_spec> [ ,...2 ]  
WITH ( LANGUAGE = <language> )  
[ ; ]  

<file_spec> ::=  
{  
    (CONTENT = <library_bits>)  
}  

<library_bits> :: =  
{ 
      varbinary_literal 
    | varbinary_expression 
}

<language> :: = 
{
      'R'
    | 'Python'
}
```
::: moniker-end

### <a name="arguments"></a>Argumente

**library_name**

Bibliotheken, die in die Instanz hochgeladen werden, können entweder öffentlich oder privat sein. Wenn die Bibliothek von einem Mitglied von `dbo` erstellt wird, ist sie öffentlich und kann mit allen Benutzern geteilt werden. Andernfalls ist die Bibliothek für diesen Benutzer privat.

Bibliotheksnamen müssen innerhalb des Kontexts eines bestimmten Benutzers oder Besitzers eindeutig sein. Beispielsweise können zwei Benutzer, **RUser1** und **RUser2**, die R-Bibliothek `ggplot2` individuell und separat hochladen. Wenn jedoch **RUser1** eine neuere Version von `ggplot2` hochladen möchte, muss die zweite Instanz anders benannt werden oder die vorhandene Bibliothek ersetzen.

Bibliotheksnamen können nicht beliebig zugewiesen werden. Der Bibliotheksname sollte mit dem Namen übereinstimmen, der benötigt wird, um die Bibliothek im externen Skript zu laden.

**owner_name**

Gibt den Namen eines Benutzers oder einer Rolle an, der oder die die externe Bibliothek besitzt. Wird kein Wert angegeben, wird der aktuelle Benutzer zum Besitzer.

Bibliotheken, die Datenbankbesitzern gehören, gelten als global für die Datenbank und Runtime. Sprich, Datenbankbesitzer können Bibliotheken erstellen, die eine gemeinsame Menge von Bibliotheken oder Paketen enthalten, die von vielen Benutzern geteilt werden. Wenn eine externe Bibliothek von einem anderen Benutzer als dem `dbo`-Benutzer erstellt wurde, ist die externe Bibliothek nur für diesen Benutzer privat.

Wenn der Benutzer **RUser1** ein externes Skript ausführt, kann der Wert von `libPath` mehrere Pfade enthalten. Der erste Pfad ist immer der Pfad zur gemeinsamen Bibliothek, die vom Datenbankbesitzer erstellt wurde. Der zweite Teil von `libPath` gibt den Pfad an, der Pakete enthält, die von **RUser1** individuell hochgeladen wurden.

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
**file_spec**

Gibt den Inhalt des Pakets für eine bestimmte Plattform an. Nur ein Dateiartefakt pro Plattform wird unterstützt.

Die Datei kann in Form eines lokalen Pfads oder eines Netzwerkpfads angegeben werden.

Beim Versuch, auf die in **<client_library_specifier>** angegebene Datei zuzugreifen, nimmt SQL Server den Sicherheitskontext des aktuellen Windows-Anmeldenamens an. Falls **<client_library_specifier>** einen Netzwerkspeicherort (UNC-Pfad) angibt, wird der Identitätswechsel des aktuellen Anmeldenamens aufgrund von Delegierungsbeschränkungen nicht auf den neuen Netzwerkspeicherort übertragen. In diesem Fall erfolgt der Zugriff mithilfe des Sicherheitskontexts des SQL Server-Dienstkontos. Weitere Informationen finden Sie unter [Anmeldeinformationen (Datenbank-Engine)](../../relational-databases/security/authentication-access/credentials-database-engine.md).

::: moniker-end
::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Optional kann eine Betriebssystemplattform für die Datei angegeben werden. Für jede Betriebssystemplattform für eine bestimmte Sprache oder Runtime ist nur jeweils ein Darteiartefakt oder Inhalt erlaubt.
::: moniker-end

**library_bits**

Gibt ähnlich wie bei Assemblys den Inhalt des Pakets als Hexadezimalliteral an. 

Diese Option ist hilfreich, wenn Sie eine Bibliothek erstellen oder eine bestehende Bibliothek ändern müssen (und über die erforderlichen Berechtigungen dafür verfügen), das Dateisystem auf dem Server jedoch eingeschränkt ist und die Bibliotheksdateien nicht an einen Speicherort kopieren kann, auf den der Server Zugriff hat.

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
**PLATFORM**

Gibt die Plattform für den Inhalt der Bibliothek an. Der Standardwert ist die Hostplattform, auf der SQL Server ausgeführt wird. Aus diesem Grund muss der Benutzer den Wert nicht angeben. Dies ist in Fällen erforderlich, in denen mehrere Plattformen unterstützt werden oder in denen der Benutzer eine andere Plattform angeben muss.
Für SQL Server 2019 werden die Plattformen Windows und Linux unterstützt.
::: moniker-end

::: moniker range="=sql-server-2017"
**LANGUAGE = 'R'**

gibt die Sprache des Pakets an. R wird in SQL Server 2017 unterstützt.
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
**language**

gibt die Sprache des Pakets an. Der Wert kann `R` oder `Python` in Azure SQL Managed Instance sein.
::: moniker-end

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
**language**

gibt die Sprache des Pakets an. Der Wert kann `R`, `Python` oder der Name einer externen Programmiersprache sein (siehe [CREATE EXTERNAL LANGUAGE](create-external-language-transact-sql.md)).
::: moniker-end

## <a name="remarks"></a>Bemerkungen

::: moniker range=">=sql-server-2017 <=sql-server-2017"
Bei der R-Sprache müssen bei Verwendung einer Datei Pakete in Form von gezippten Archivdateien mit der Dateiendung .ZIP für Windows vorbereitet werden. 
Für SQL Server 2017 wird nur die Windows-Plattform unterstützt.
::: moniker-end

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Bei der R-Sprache müssen bei Verwendung einer Datei Pakete in Form von gezippten Archivdateien mit der Erweiterung „.ZIP“ vorbereitet werden.  

Für Python muss das Paket in einer WHL- oder ZIP-Datei als ZIP-Archivdatei vorbereitet werden. Wenn das Paket bereits eine ZIP-Datei ist, muss es in eine neue ZIP-Datei eingefügt werden. Der direkte Upload einer WHL- oder ZIP-Datei wird derzeit nicht unterstützt.
::: moniker-end

Die `CREATE EXTERNAL LIBRARY`-Anweisung lädt die Bibliothekbits in die Datenbank hoch. Die Bibliothek wird installiert, wenn ein Benutzer mithilfe von [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) ein externes Skript ausführt und das Paket oder die Bibliothek aufruft.

Bibliotheken, die in die Instanz hochgeladen werden, können entweder öffentlich oder privat sein. Wenn die Bibliothek von einem Mitglied von `dbo` erstellt wird, ist sie öffentlich und kann mit allen Benutzern geteilt werden. Andernfalls ist die Bibliothek für diesen Benutzer privat.

In einer SQL-Instanz sind einige Pakete vorinstalliert. Diese werden als *Systempakete* bezeichnet. Systempakete können vom Benutzer weder hinzugefügt noch aktualisiert oder entfernt werden.

## <a name="permissions"></a>Berechtigungen

Erfordert die `CREATE EXTERNAL LIBRARY`-Berechtigung. Standardmäßig verfügt jeder Benutzer mit **dbo**, der Mitglied der Rolle **db_owner** ist, über die Berechtigung zum Erstellen einer externen Bibliothek. Allen anderen Benutzern müssen Sie die Berechtigung mithilfe einer [GRANT](./grant-database-permissions-transact-sql.md)-Anweisung explizit gewähren, in der Sie CREATE EXTERNAL LIBRARY als Berechtigung angeben.

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
In SQL Server 2019 benötigt der Benutzer zusätzlich zur Berechtigung CREATE EXTERNAL LIBRARY auch die REFERENCES-Berechtigung für eine externe Sprache, um externe Bibliotheken für diese externe Sprache zu erstellen.

```sql
GRANT REFERENCES ON EXTERNAL LANGUAGE::Java to user
GRANT CREATE EXTERNAL LIBRARY to user
```

::: moniker-end

Zum Bearbeiten einer Bibliothek ist die separate Berechtigung `ALTER ANY EXTERNAL LIBRARY` erforderlich.

Zum Erstellen einer externen Bibliothek mithilfe eines Dateipfads muss der Benutzer über eine authentifizierte Anmeldung verfügen oder Mitglied der festen sysadmin-Serverrolle sein.

## <a name="examples"></a>Beispiele

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
### <a name="add-an-external-library-to-a-database"></a>Hinzufügen einer externen Bibliothek zu einer Datenbank  

Im folgenden Beispiel wird eine externe Bibliothek namens `customPackage` zu einer Datenbank hinzugefügt.

```sql
CREATE EXTERNAL LIBRARY customPackage
FROM (CONTENT = 'C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\customPackage.zip') WITH (LANGUAGE = 'R');
```

Nachdem die Bibliothek erfolgreich in die Instanz hochgeladen wurde, führt ein Benutzer den Vorgang `sp_execute_external_script` aus, um die Bibliothek zu installieren.

```sql
EXEC sp_execute_external_script 
@language =N'R', 
@script=N'library(customPackage)'
```
::: moniker-end

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Das Beispiel funktioniert auch für Python in SQL Server 2019, wenn Sie `'R'` durch `'Python'` ersetzen.
::: moniker-end

### <a name="installing-packages-with-dependencies"></a>Installieren von Paketen mit Abhängigkeiten

Wenn das Paket, das Sie installieren möchten, Abhängigkeiten aufweist, ist es sehr wichtig, dass Sie sowohl Abhängigkeiten der ersten als auch der zweiten Ebene analysieren und sicherstellen, dass alle erforderlichen Pakete verfügbar sind, _bevor_ Sie versuchen, das Zielpaket zu installieren.

Nehmen wir beispielsweise an, dass Sie ein neues Paket, `packageA`, installieren möchten:

+ `packageA` ist abhängig von `packageB`.
+ `packageB` ist abhängig von `packageC`.

Damit die Installation von `packageA` erfolgreich ist, müssen Sie Bibliotheken für `packageB` und `packageC` zur selben Zeit erstellen, zu der Sie `packageA` zu SQL Server hinzufügen. Achten Sie darauf, dass Sie auch die erforderlichen Paketversionen überprüfen.

In der Praxis sind Paketabhängigkeiten für häufig verwendete Pakete in der Regel viel komplizierter als in diesem einfachen Beispiel. Beispielsweise könnte **ggplot2** mehr als 30 Pakete erfordern, und diese Pakete könnten wiederum zusätzliche Pakete erfordern, die auf dem Server nicht verfügbar sind. Jedes fehlende Paket und jede falsche Paketversion kann zu einem Fehler bei der Installation führen.

Da es schwierig sein kann, alle Abhängigkeiten nur durch Betrachten des Paketmanifests zu bestimmen, empfiehlt es sich, ein Paket wie [miniCRAN](https://cran.r-project.org/web/packages/miniCRAN/index.html) zu verwenden, um alle Pakete zu identifizieren, die für eine erfolgreiche Installation erforderlich sein könnten.

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"

+ Laden Sie das Zielpaket und dessen Abhängigkeiten hoch. Alle Dateien müssen sich in einem Ordner befinden, auf den der Server Zugriff hat.

    ```sql
    CREATE EXTERNAL LIBRARY packageA 
    FROM (CONTENT = 'C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\packageA.zip') 
    WITH (LANGUAGE = 'R'); 
    GO

    CREATE EXTERNAL LIBRARY packageB FROM (CONTENT = 'C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\packageB.zip') 
    WITH (LANGUAGE = 'R');
    GO

    CREATE EXTERNAL LIBRARY packageC FROM (CONTENT = 'C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\packageC.zip') 
    WITH (LANGUAGE = 'R');
    GO
    ```

+ Installieren Sie zunächst die erforderlichen Pakete.

    Wenn ein erforderliches Paket bereits in die Instanz hochgeladen wurde, müssen Sie dieses nicht erneut hinzufügen. Achten Sie nur darauf, zu überprüfen, ob es sich bei der Version des vorhandenen Pakets um die richtige handelt. 
    
    Die erforderlichen Pakete `packageC` und `packageB` werden beim ersten Ausführen von `sp_execute_external_script` zur Installation des Pakets `packageA` in der richtigen Reihenfolge installiert.

    Wenn ein erforderliches Paket jedoch nicht verfügbar ist, tritt ein Fehler bei der Installation des Zielpakets `packageA` auf.

    ```sql
    EXEC sp_execute_external_script 
    @language =N'R', 
    @script=N'
    # load the desired package packageA
    library(packageA)
    '
    ```
::: moniker-end

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Das Beispiel funktioniert auch für Python in SQL Server 2019, wenn Sie `'R'` durch `'Python'` ersetzen.
::: moniker-end

### <a name="create-a-library-from-a-byte-stream"></a>Erstellen einer Bibliothek aus einem Bytedatenstrom

Wenn Sie nicht die Möglichkeit haben, die Paketdateien in einem Speicherort auf dem Server zu speichern, können Sie die Paketinhalte in einer Variable übergeben. Im folgenden Beispiel wird eine Bibliothek erstellt, indem die Bits als Hexadezimalliteral übergeben werden.

```SQL
CREATE EXTERNAL LIBRARY customLibrary FROM (CONTENT = 0xABC123...) WITH (LANGUAGE = 'R');
```

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Das Beispiel funktioniert auch für Python in SQL Server 2019, wenn Sie **'R'** durch **'Python'** ersetzen.
::: moniker-end

> [!NOTE]
> Dieses Codebeispiel zeigt nur die Syntax; der Binärwert in `CONTENT =` wurde zur besseren Lesbarkeit gekürzt und erstellt keine funktionierende Bibliothek. Der tatsächliche Inhalt der binären Variable wäre wesentlich länger.

### <a name="change-an-existing-package-library"></a>Löschen einer vorhandenen Paketbibliothek

Die `ALTER EXTERNAL LIBRARY`-DDL-Anweisung kann verwendet werden, um neuen Bibliotheksinhalt hinzuzufügen oder bestehenden Bibliotheksinhalt zu ändern. Zum Bearbeiten einer bestehenden Bibliothek ist die Berechtigung `ALTER ANY EXTERNAL LIBRARY` erforderlich.

Weitere Informationen finden Sie unter [ALTER EXTERNAL LIBRARY](alter-external-library-transact-sql.md).

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
### <a name="add-a-java-jar-file-to-a-database"></a>Hinzufügen einer JAR-Datei zu einer Datenbank  

Im folgenden Beispiel wird eine externe JAR-Datei namens `customJar` zu einer Datenbank hinzugefügt.

```sql
CREATE EXTERNAL LIBRARY customJar
FROM (CONTENT = 'C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\customJar.jar') 
WITH (LANGUAGE = 'Java');
```

Nachdem die Bibliothek erfolgreich in die Instanz hochgeladen wurde, führt ein Benutzer den Vorgang `sp_execute_external_script` aus, um die Bibliothek zu installieren.

```sql
EXEC sp_execute_external_script
    @language = N'Java'
    , @script = N'customJar.MyCLass.myMethod'
    , @input_data_1 = N'SELECT * FROM dbo.MyTable'
WITH RESULT SETS ((column1 int))
```

### <a name="add-an-external-package-for-both-windows-and-linux"></a>Hinzufügen eines externen Pakets für Windows und Linux

Sie können `<file_spec>` maximal zweimal angeben, einmal für Windows und einmal für Linux.

```sql
CREATE EXTERNAL LIBRARY lazyeval 
FROM (CONTENT = 'C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\packageA.zip', PLATFORM = WINDOWS),
(CONTENT = 'C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\packageA.tar.gz', PLATFORM = LINUX)
WITH (LANGUAGE = 'R')
```

Wenn Sie `sp_execute_external_script` zum Installieren des Pakets verwenden, wird der Bibliotheksinhalt je nach Plattform verwendet, auf der die SQL Server-Instanz ausgeführt wird.

```sql
EXECUTE sp_execute_external_script 
    @LANGUAGE = N'R',
    @SCRIPT = N'
library(packageA)
```
::: moniker-end

## <a name="see-also"></a>Weitere Informationen

[ALTER EXTERNAL LIBRARY (Transact-SQL)](alter-external-library-transact-sql.md)  
[DROP EXTERNAL LIBRARY (Transact-SQL)](drop-external-library-transact-sql.md)  
[sys.external_library_files](../../relational-databases/system-catalog-views/sys-external-library-files-transact-sql.md)  
[sys.external_libraries](../../relational-databases/system-catalog-views/sys-external-libraries-transact-sql.md)
