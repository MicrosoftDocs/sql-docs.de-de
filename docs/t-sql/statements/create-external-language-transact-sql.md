---
description: CREATE EXTERNAL LANGUAGE (Transact-SQL) – SQL Server
title: CREATE EXTERNAL LANGUAGE (Transact-SQL) – SQL Server | Microsoft-Dokumentation
ms.custom: ''
ms.date: 04/03/2020
ms.prod: sql
ms.technology: language-extensions
ms.topic: language-reference
author: dphansen
ms.author: davidph
manager: cgronlun
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: a299c47b9d0d2fccde5c44c26d9f78e8bc1cca2c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100070673"
---
# <a name="create-external-language-transact-sql"></a>CREATE EXTERNAL LANGUAGE (Transact-SQL)
[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

Registriert externe Spracherweiterungen aus dem angegebenen Dateipfad oder Bytedatenstrom in der Datenbank. Diese Anweisung dient als generischer Mechanismus, mit dem der Datenbankadministrator neue externe Spracherweiterungen auf jeder von SQL Server unterstützten Betriebssystemplattform registrieren kann. Weitere Informationen finden Sie unter [Spracherweiterungen](../../language-extensions/language-extensions-overview.md).

> [!NOTE]
> **R** und **Python** sind reservierte Namen, und es kann keine externe Sprache mit diesen Namen erstellt werden. Weitere Informationen zur Verwendung von **R** und **Python** finden Sie unter [SQL Server-Machine Learning-Services](../../machine-learning/index.yml).

## <a name="syntax"></a>Syntax

```syntaxsql
CREATE EXTERNAL LANGUAGE language_name  
[ AUTHORIZATION owner_name ]  
FROM <file_spec> [ ,...2 ]  
[ ; ]  

<file_spec> ::=  
{
    ( CONTENT = { <external_lang_specifier> | <content_bits> },
    FILE_NAME = <external_lang_file_name>
    [ , PLATFORM = <platform> ]
    [ , PARAMETERS = <external_lang_parameters> ]
    [ , ENVIRONMENT_VARIABLES = <external_lang_env_variables> ] )
}

<external_lang_specifier> :: =  
{
    '[file_path\]os_file_name'  
}

<content_bits> :: =  
{
    varbinary_literal
    | varbinary_expression
}

<external_lang_file_name> :: =  
'extension_file_name'


<platform> :: =
{
    WINDOWS
  | LINUX
}

<external_lang_parameters> :: =  
'extension_specific_parameters'
```

### <a name="arguments"></a>Argumente

**language_name**

Sprachen sind datenbankweite Objekte. Sprachnamen müssen innerhalb der Datenbank eindeutig sein.

**owner_name**

Gibt den Namen eines Benutzers oder einer Rolle an, der oder die die externe Sprache besitzt. Wird kein Wert angegeben, wird der aktuelle Benutzer zum Besitzer. Je nach Berechtigungen muss anderen Benutzern möglicherweise die explizite Berechtigung zum Ausführen von Skripts unter Verwendung einer bestimmten Sprache erteilt werden.

**file_spec**

Gibt den Inhalt der Spracherweiterung an. Es ist nur eine Dateiangabe für eine bestimmte Sprache pro Plattform zulässig.

**external_lang_specifier**

Der vollständige Dateipfad zur ZIP- oder TAR.GZ-Datei mit dem Erweiterungscode. Dieser Inhalt kann entweder ein Pfad zu einer ZIP-Datei (unter Windows) oder einer TAR.GZ-Datei (unter Linux) sein.

**content_bits**

Gibt ähnlich wie bei Assemblys den Inhalt der Sprache als Hexadezimalliteral an.

Diese Option ist hilfreich, wenn Sie eine Sprache erstellen oder eine bestehende Sprache ändern müssen (und über die erforderlichen Berechtigungen dafür verfügen), das Dateisystem auf dem Server jedoch eingeschränkt ist und Sie die Bibliotheksdateien nicht an einen Speicherort kopieren können, auf den der Server Zugriff hat.

**external_lang_file_name**

Der Name der DLL- oder SO-Datei der Erweiterung. Dieser ist erforderlich, um die richtige Datei in solchen Fällen zu identifizieren, in denen mehrere DLL- oder SO-Dateien in der ZIP- oder TAR.GZ-Datei für <external_lang_specifier> vorhanden sind.

**external_lang_parameters**

Dies bietet eine Möglichkeit, einen Satz von Parametern für die Runtime der externen Sprache bereitzustellen. Parameterwerte werden der Runtime der externen Sprache nach dem Starten des externen Prozesses bereitgestellt. Auf Umgebungsvariablen kann die Spracherweiterung jedoch vor dem Starten des externen Prozesses zugreifen.

**external_lang_env_variables**

Dies bietet eine Möglichkeit, einen Satz von Umgebungsvariablen für die Runtime der externen Sprache vor dem Starten des externen Prozesses bereitzustellen. Ein Beispiel für eine Umgebungsvariable ist das Basisverzeichnis der Runtime selbst. Beispiel: JRE_HOME.

**platform**

Dieser Parameter ist für Szenarien mit hybridem Betriebssystem erforderlich. In einer hybriden Architektur muss die Sprache einmal pro Plattform registriert werden. Plattform und Sprachname sind der eindeutige Schlüssel für jede externe Sprache. Wenn keine Plattform angegeben ist, wird vom aktuellen Betriebssystem davon ausgegangen.

## <a name="permissions"></a>Berechtigungen

Erfordert die `CREATE EXTERNAL LANGUAGE`-Berechtigung. Standardmäßig verfügt jeder Benutzer mit **dbo**, der Mitglied der Rolle **db_owner** ist, über die Berechtigung zum Erstellen einer externen Sprache. Allen anderen Benutzern müssen Sie die Berechtigung mithilfe einer [GRANT](./grant-database-permissions-transact-sql.md)-Anweisung explizit gewähren, in der Sie CREATE EXTERNAL LANGUAGE als Berechtigung angeben.

Zum Bearbeiten einer Bibliothek ist die separate Berechtigung `ALTER ANY EXTERNAL LANGUAGE` erforderlich.

### <a name="execute-external-script-permission"></a>EXECUTE EXTERNAL SCRIPT-Berechtigung

Sie können EXECUTE EXTERNAL SCRIPT-Berechtigungen verwenden, damit die Ausführung externer Skripts für bestimmte Sprachen gewährt werden kann. Dies unterscheidet sich von der Datenbankberechtigung EXECUTE ANY EXTERNAL SCRIPT, mit der keine Ausführungsberechtigung für eine bestimmte Sprache gewährt werden kann.

Das bedeutet, dass Benutzern ohne **dbo** die Berechtigung zum Ausführen einer bestimmten Sprache erteilt werden muss:

```sql
GRANT EXECUTE EXTERNAL SCRIPT ON EXTERNAL LANGUAGE ::language_name 
TO database_principal_name;
```

### <a name="reference-permissions-to-external-libraries"></a>Verweisberechtigungen auf externe Bibliotheken

Ähnlich wie bei Assemblys sind Verweisberechtigungen für externe Sprachen erforderlich, damit eine Verknüpfung zwischen externen Bibliotheken und externen Sprachen vorhanden ist. Wenn beispielsweise eine externe Sprache gelöscht werden soll, muss der Benutzer zunächst sicherstellen, dass alle externen Bibliotheken gelöscht werden, die auf diese Sprache verweisen. Sie können die externe Sprache als ein Objekt einer höheren Ebene als externe Bibliotheken in einer Hierarchie anzeigen.

## <a name="examples"></a>Beispiele

### <a name="a-create-an-external-language-in-a-database"></a>A. Erstellen einer externen Sprache in einer Datenbank  

Im folgenden Beispiel wird eine externe Sprache mit dem Namen Java einer Datenbank auf SQL Server unter Windows hinzugefügt.

```sql
CREATE EXTERNAL LANGUAGE Java 
FROM (CONTENT = N'<path-to-zip>', FILE_NAME = 'javaextension.dll');
GO
```

### <a name="b-create-an-external-language-for-both-windows-and-linux"></a>B. Erstellen einer externen Sprache für Windows und Linux

Sie können `<file_spec>` maximal zweimal angeben, einmal für Windows und einmal für Linux.

```sql
CREATE EXTERNAL LANGUAGE Java
FROM
(CONTENT = N'<path-to-zip>', FILE_NAME = 'javaextension.dll', PLATFORM = WINDOWS),
(CONTENT = N'<path-to-tar.gz>', FILE_NAME = 'javaextension.so', PLATFORM = LINUX);
GO
```
### <a name="c-grant-permissions-to-execute-external-script"></a>C. Gewähren der Berechtigungen zum Ausführen von externen Skripts

Im folgenden Beispiel wird dem **mylogin**-Prinzipal der Zugriff zum Ausführen externer Skripts mithilfe der externen Sprache **Java** gewährt.

```sql
GRANT EXECUTE EXTERNAL SCRIPT ON EXTERNAL LANGUAGE ::Java 
TO mylogin;
```


## <a name="see-also"></a>Weitere Informationen

[ALTER EXTERNAL LANGUAGE (Transact-SQL)](alter-external-language-transact-sql.md)  
[DROP EXTERNAL LANGUAGE (Transact-SQL)](drop-external-language-transact-sql.md)  
[sys.external_languages](../../relational-databases/system-catalog-views/sys-external-languages-transact-sql.md)  
[sys.external_language_files](../../relational-databases/system-catalog-views/sys-external-language-files-transact-sql.md)