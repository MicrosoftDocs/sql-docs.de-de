---
description: ALTER EXTERNAL LIBRARY (Transact-SQL)
title: ALTER EXTERNAL LIBRARY (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/26/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: machine-learning
ms.topic: reference
f1_keywords:
- ALTER EXTERNAL LIBRARY
- ALTER_EXTERNAL_LIBRARY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER EXTERNAL LIBRARY
author: dphansen
ms.author: davidph
manager: cgronlund
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 7a939a09d107d08ee436d3b1f98c2f74e882ebf6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337965"
---
# <a name="alter-external-library-transact-sql"></a>ALTER EXTERNAL LIBRARY (Transact-SQL)  
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

Ändert den Inhalt einer vorhandenen externen Paketbibliothek.

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
> [!NOTE]
> In SQL Server 2017 werden die R-Sprache und die Windows-Plattform unterstützt. Ab SQL Server 2019 werden R, Python und externe Programmiersprachen auf Windows- und Linux-Plattformen unterstützt.
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
> [!NOTE]
> In Azure SQL Managed Instance können Sie eine Bibliothek ändern, indem Sie diese entfernen und dann mithilfe von **sqlmlutils** die geänderte Version installieren. Weitere Informationen zu **sqlmlutils** finden Sie unter [Installieren von Python-Paketen mit sqlmlutils](../../machine-learning/package-management/install-additional-python-packages-on-sql-server.md?context=%252fazure%252fazure-sql%252fmanaged-instance%252fcontext%252fml-context&view=azuresqldb-mi-current&preserve-view=true) und [Installieren von neuen R-Paketen mit sqlmlutils](../../machine-learning/package-management/install-additional-r-packages-on-sql-server.md?context=%252fazure%252fazure-sql%252fmanaged-instance%252fcontext%252fml-context&view=azuresqldb-mi-current&preserve-view=true).
::: moniker-end

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
## <a name="syntax-for-sql-server-2019"></a>Syntax für SQL Server 2019

```syntaxsql
ALTER EXTERNAL LIBRARY library_name
[ AUTHORIZATION owner_name ]
SET <file_spec>
WITH ( LANGUAGE = <language> )
[ ; ]

<file_spec> ::=
{
    (CONTENT = { <client_library_specifier> | <library_bits> | NONE}
    [, PLATFORM = <platform> )
}

<client_library_specifier> :: =
{
      '[\\computer_name\]share_name\[path\]manifest_file_name'
    | '[local_path\]manifest_file_name'
    | '<relative_path_in_external_data_source>'
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
::: moniker range="=sql-server-2017"
## <a name="syntax-for-sql-server-2017"></a>Syntax für SQL Server 2017

```syntaxsql
ALTER EXTERNAL LIBRARY library_name
[ AUTHORIZATION owner_name ]
SET <file_spec>
WITH ( LANGUAGE = 'R' )
[ ; ]

<file_spec> ::=
{
    (CONTENT = { <client_library_specifier> | <library_bits> | NONE}
    [, PLATFORM = WINDOWS )
}

<client_library_specifier> :: =
{
      '[\\computer_name\]share_name\[path\]manifest_file_name'
    | '[local_path\]manifest_file_name'
    | '<relative_path_in_external_data_source>'
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

Gibt den Namen einer vorhandenen Paketbibliothek an. Bibliotheken umfassen nur den Benutzer. Bibliotheksnamen müssen innerhalb des Kontexts eines bestimmten Benutzers oder Besitzers eindeutig sein.

Der Bibliotheksname kann nicht nach dem Zufallsprinzip zugewiesen werden. D.h., Sie müssen den Namen verwenden, den die aufrufende Runtime beim Laden des Pakets erwartet.

**owner_name**

Gibt den Namen eines Benutzers oder einer Rolle an, der oder die die externe Bibliothek besitzt.

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
**file_spec**

Gibt den Inhalt des Pakets für eine bestimmte Plattform an. Nur ein Dateiartefakt pro Plattform wird unterstützt.

Die Datei kann in Form eines lokalen Pfads oder eines Netzwerkpfads angegeben werden. Wenn die Datenquellenoption angegeben ist, kann der Dateiname ein relativer Pfad zu dem Container sein, auf den in `EXTERNAL DATA SOURCE` verwiesen wird.

Optional kann eine Betriebssystemplattform für die Datei angegeben werden. Für jede Betriebssystemplattform für eine bestimmte Sprache oder Runtime ist nur jeweils ein Darteiartefakt oder Inhalt erlaubt.
::: moniker-end

**library_bits**

Gibt ähnlich wie bei Assemblys den Inhalt des Pakets als Hexadezimalliteral an.

Diese Option ist nützlich, wenn Sie die erforderliche Berechtigung zum Ändern einer Bibliothek haben. Jedoch ist der Zugriff auf Daten auf dem Server beschränkt, und Sie können die Inhalte nicht in einem Pfad speichern, auf den der Server zugreifen kann.

Stattdessen können Sie den Paketinhalt als Variable im Binärformat übergeben.

::: moniker range=">=sql-server-2017 <=sql-server-2017"
**platform = WINDOWS**

Gibt die Plattform für den Inhalt der Bibliothek an. Dieser Wert ist erforderlich, wenn eine vorhandene Bibliothek geändert wird, um eine andere Plattform hinzuzufügen.
Für SQL Server 2017 ist Windows die einzige unterstützte Plattform.
::: moniker-end

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
**platform**

Gibt die Plattform für den Inhalt der Bibliothek an. Dieser Wert ist erforderlich, wenn eine vorhandene Bibliothek geändert wird, um eine andere Plattform hinzuzufügen. 
Für SQL Server 2019 werden die Plattformen Windows und Linux unterstützt.
::: moniker-end

::: moniker range=">=sql-server-2017 <=sql-server-2017"
**LANGUAGE = 'R'**

gibt die Sprache des Pakets an. R wird in SQL Server 2017 unterstützt.
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
**language**

gibt die Sprache des Pakets an. Der Wert kann **R** oder **Python** in Azure SQL Managed Instance sein.
::: moniker-end

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
**language**

gibt die Sprache des Pakets an. Der Wert kann **R**, **Python** oder der Name einer externen Programmiersprache (siehe [CREATE EXTERNAL LANGUAGE](create-external-language-transact-sql.md)) sein.
::: moniker-end

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Bemerkungen

::: moniker range=">=sql-server-2017 <=sql-server-2017"
Bei der R-Sprache müssen Pakete in Form von gezippten Archivdateien mit der Dateiendung .zip für Windows vorbereitet werden. Für SQL Server 2017 wird nur die Windows-Plattform unterstützt.  
::: moniker-end

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Bei der R-Sprache müssen bei Verwendung einer Datei Pakete in Form von gezippten Archivdateien mit der Erweiterung „.ZIP“ vorbereitet werden. 

Für Python muss das Paket in einer WHL- oder ZIP-Datei als ZIP-Archivdatei vorbereitet werden. Wenn das Paket bereits eine ZIP-Datei ist, muss es in eine neue ZIP-Datei eingefügt werden. Der direkte Upload einer WHL- oder ZIP-Datei wird derzeit nicht unterstützt.
::: moniker-end

Die `ALTER EXTERNAL LIBRARY`-Anweisung lädt nur die Bibliothekbits in die Datenbank hoch. Die geänderte Bibliothek wird installiert, wenn ein Benutzer Code in [sp_execute_external_script (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) ausführt, der die Bibliothek aufruft.

In einer SQL-Instanz sind einige Pakete vorinstalliert. Diese werden als *Systempakete* bezeichnet. Systempakete können vom Benutzer weder hinzugefügt noch aktualisiert oder entfernt werden.

## <a name="permissions"></a>Berechtigungen

Standardmäßig verfügen der **dbo**-Benutzer sowie alle Mitglieder der Rolle **db_owner** über die Berechtigung zum Ausführen von ALTER EXTERNAL LIBRARY. Darüber hinaus kann der Benutzer, der die externe Bibliothek erstellt hat, diese ändern.

## <a name="examples"></a>Beispiele

Die folgenden Beispiele ändern eine externe Bibliothek namens `customPackage`.

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
### <a name="replace-the-contents-of-a-library-using-a-file"></a>Ersetzen des Inhalts einer Bibliothek mithilfe einer Datei

Im folgenden Beispiel wird eine externe Bibliothek mit dem Namen `customPackage` mithilfe einer ZIP-Datei geändert, die die aktualisierten Bits enthält.

```sql
ALTER EXTERNAL LIBRARY customPackage 
SET 
  (CONTENT = 'C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\customPackage.zip')
WITH (LANGUAGE = 'R');
```

Führen Sie die gespeicherte Prozedur `sp_execute_external_script` aus, um die aktualisierte Bibliothek zu installieren.

```sql
EXEC sp_execute_external_script 
@language =N'R', 
@script=N'library(customPackage)'
;
```
::: moniker-end

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Das Beispiel funktioniert auch für Python, wenn Sie `'R'` durch `'Python'` ersetzen.
::: moniker-end

### <a name="alter-an-existing-library-using-a-byte-stream"></a>Ändern einer vorhandenen Bibliothek mithilfe eines Bytedatenstroms

Im folgenden Beispiel wird die vorhandene Bibliothek geändert, indem die neuen Bits als ein Hexadezimalliteral übergeben werden.

```SQL
ALTER EXTERNAL LIBRARY customLibrary 
SET (CONTENT = 0xABC123...) WITH (LANGUAGE = 'R');
```

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15||=azuresqldb-mi-current"
Das Beispiel funktioniert auch für Python, wenn Sie `'R'` durch `'Python'` ersetzen.
::: moniker-end

> [!NOTE]
> Dieses Codebeispiel zeigt nur die Syntax; der Binärwert in `CONTENT =` wurde zur besseren Lesbarkeit gekürzt und erstellt keine funktionierende Bibliothek. Der tatsächliche Inhalt der binären Variable wäre wesentlich länger.

## <a name="see-also"></a>Weitere Informationen

[CREATE EXTERNAL LIBRARY (Transact-SQL)](create-external-library-transact-sql.md)  
[DROP EXTERNAL LIBRARY (Transact-SQL)](drop-external-library-transact-sql.md)  
[sys.external_library_files](../../relational-databases/system-catalog-views/sys-external-library-files-transact-sql.md)  
[sys.external_libraries](../../relational-databases/system-catalog-views/sys-external-libraries-transact-sql.md)