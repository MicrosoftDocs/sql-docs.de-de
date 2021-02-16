---
description: CREATE FUNCTION (Transact-SQL)
title: CREATE FUNCTION (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/16/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- FUNCTION
- CREATE FUNCTION
- CREATE_FUNCTION_TSQL
- FUNCTION_TSQL
- TVF
- MSTVF
dev_langs:
- TSQL
helpviewer_keywords:
- invoking functions
- extended stored procedures [SQL Server], functions
- table-valued parameters
- user-defined functions [SQL Server], creating
- CLR functions [SQL Server]
- CREATE FUNCTION statement
- nondeterministic functions
- user-defined data types
- functions [SQL Server], creating
- inline table-valued functions [SQL Server]
- multi-statement table-valued functions [SQL Server]
- table-valued functions [SQL Server], CREATE FUNCTION
- parameters [SQL Server], functions
- nesting user-defined functions
- deterministic functions
- scalar-valued functions
- scalar UDF
- MSTVF
- TVF
- functions [SQL Server], invoking
ms.assetid: 864b393f-225f-4895-8c8d-4db59ea60032
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 8b2c611d9f4ee1aaf169137864edba15b9da0c15
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344035"
---
# <a name="create-function-transact-sql"></a>CREATE FUNCTION (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Erstellt eine benutzerdefinierte Funktion in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. Eine benutzerdefinierte Funktion, die eine [!INCLUDE[tsql](../../includes/tsql-md.md)]- oder CLR-Routine (Common Language Runtime) darstellt, die Parameter annehmen, eine Aktion ausführen (z. B. eine komplexe Berechnung) und das Ergebnis dieser Aktion als Wert zurückgeben kann. Der Rückgabewert kann ein Skalarwert (Einzelwert) oder eine Tabelle sein. Verwenden Sie diese Anweisung zum Erstellen einer wiederverwendbaren Routine, die auf folgende Weise verwendet werden kann:

- In [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen, wie z. B. SELECT

- In Anwendungen, die die Funktion aufrufen

- Bei der Definition einer anderen benutzerdefinierten Funktion

- Zum Parametrisieren einer Sicht oder zur Verbesserung der Funktionalität einer indizierten Sicht

- Zum Definieren einer Spalte in einer Tabelle

- Zum Definieren einer CHECK-Einschränkung für eine Spalte

- Zum Ersetzen einer gespeicherten Prozedur

- Zum Verwenden einer Inlinefunktion als Filterprädikat für eine Sicherheitsrichtlinie

> [!NOTE]
>
> - Die Integration der .NET Framework-CLR in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wird in diesem Artikel erläutert. Die CLR-Integration gilt nicht für [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].
> - Informationen zu [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] finden Sie unter [CREATE FUNCTION (Azure Synapse Analytics)](../../t-sql/statements/create-function-sql-data-warehouse.md).

![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Syntax

```syntaxsql
-- Transact-SQL Scalar Function Syntax
CREATE [ OR ALTER ] FUNCTION [ schema_name. ] function_name
( [ { @parameter_name [ AS ][ type_schema_name. ] parameter_data_type
 [ = default ] [ READONLY ] }
    [ ,...n ]
  ]
)
RETURNS return_data_type
    [ WITH <function_option> [ ,...n ] ]
    [ AS ]
    BEGIN
        function_body
        RETURN scalar_expression
    END
[ ; ]
```

```syntaxsql
-- Transact-SQL Inline Table-Valued Function Syntax
CREATE [ OR ALTER ] FUNCTION [ schema_name. ] function_name
( [ { @parameter_name [ AS ] [ type_schema_name. ] parameter_data_type
    [ = default ] [ READONLY ] }
    [ ,...n ]
  ]
)
RETURNS TABLE
    [ WITH <function_option> [ ,...n ] ]
    [ AS ]
    RETURN [ ( ] select_stmt [ ) ]
[ ; ]
```

```syntaxsql
-- Transact-SQL Multi-Statement Table-Valued Function Syntax
CREATE [ OR ALTER ] FUNCTION [ schema_name. ] function_name
( [ { @parameter_name [ AS ] [ type_schema_name. ] parameter_data_type
    [ = default ] [READONLY] }
    [ ,...n ]
  ]
)
RETURNS @return_variable TABLE <table_type_definition>
    [ WITH <function_option> [ ,...n ] ]
    [ AS ]
    BEGIN
        function_body
        RETURN
    END
[ ; ]

```

```syntaxsql
-- Transact-SQL Function Clauses
<function_option>::=
{
    [ ENCRYPTION ]
  | [ SCHEMABINDING ]
  | [ RETURNS NULL ON NULL INPUT | CALLED ON NULL INPUT ]
  | [ EXECUTE_AS_Clause ]
  | [ INLINE = { ON | OFF }]
}

<table_type_definition>:: =
( { <column_definition> <column_constraint>
  | <computed_column_definition> }
    [ <table_constraint> ] [ ,...n ]
)
<column_definition>::=
{
    { column_name data_type }
    [ [ DEFAULT constant_expression ]
      [ COLLATE collation_name ] | [ ROWGUIDCOL ]
    ]
    | [ IDENTITY [ (seed , increment ) ] ]
    [ <column_constraint> [ ...n ] ]
}

<column_constraint>::=
{
    [ NULL | NOT NULL ]
    { PRIMARY KEY | UNIQUE }
      [ CLUSTERED | NONCLUSTERED ]
      [ WITH FILLFACTOR = fillfactor
        | WITH ( < index_option > [ , ...n ] )
      [ ON { filegroup | "default" } ]
  | [ CHECK ( logical_expression ) ] [ ,...n ]
}

<computed_column_definition>::=
column_name AS computed_column_expression

<table_constraint>::=
{
    { PRIMARY KEY | UNIQUE }
      [ CLUSTERED | NONCLUSTERED ]
      ( column_name [ ASC | DESC ] [ ,...n ]
        [ WITH FILLFACTOR = fillfactor
        | WITH ( <index_option> [ , ...n ] )
  | [ CHECK ( logical_expression ) ] [ ,...n ]
}

<index_option>::=
{
    PAD_INDEX = { ON | OFF }
  | FILLFACTOR = fillfactor
  | IGNORE_DUP_KEY = { ON | OFF }
  | STATISTICS_NORECOMPUTE = { ON | OFF }
  | ALLOW_ROW_LOCKS = { ON | OFF }
  | ALLOW_PAGE_LOCKS ={ ON | OFF }
}
```

```syntaxsql
-- CLR Scalar Function Syntax
CREATE [ OR ALTER ] FUNCTION [ schema_name. ] function_name
( { @parameter_name [AS] [ type_schema_name. ] parameter_data_type
    [ = default ] }
    [ ,...n ]
)
RETURNS { return_data_type }
    [ WITH <clr_function_option> [ ,...n ] ]
    [ AS ] EXTERNAL NAME <method_specifier>
[ ; ]
```

```syntaxsql
-- CLR Table-Valued Function Syntax
CREATE [ OR ALTER ] FUNCTION [ schema_name. ] function_name
( { @parameter_name [AS] [ type_schema_name. ] parameter_data_type
    [ = default ] }
    [ ,...n ]
)
RETURNS TABLE <clr_table_type_definition>
    [ WITH <clr_function_option> [ ,...n ] ]
    [ ORDER ( <order_clause> ) ]
    [ AS ] EXTERNAL NAME <method_specifier>
[ ; ]
```

```syntaxsql
-- CLR Function Clauses
<order_clause> ::=
{
   <column_name_in_clr_table_type_definition>
   [ ASC | DESC ]
} [ ,...n]

<method_specifier>::=
    assembly_name.class_name.method_name

<clr_function_option>::=
}
    [ RETURNS NULL ON NULL INPUT | CALLED ON NULL INPUT ]
  | [ EXECUTE_AS_Clause ]
}

<clr_table_type_definition>::=
( { column_name data_type } [ ,...n ] )
```

```syntaxsql
-- In-Memory OLTP: Syntax for natively compiled, scalar user-defined function
CREATE [ OR ALTER ] FUNCTION [ schema_name. ] function_name
 ( [ { @parameter_name [ AS ][ type_schema_name. ] parameter_data_type
    [ NULL | NOT NULL ] [ = default ] [ READONLY ] }
    [ ,...n ]
  ]
)
RETURNS return_data_type
     WITH <function_option> [ ,...n ]
    [ AS ]
    BEGIN ATOMIC WITH (set_option [ ,... n ])
        function_body
        RETURN scalar_expression
    END

<function_option>::=
{
  |  NATIVE_COMPILATION
  |  SCHEMABINDING
  | [ EXECUTE_AS_Clause ]
  | [ RETURNS NULL ON NULL INPUT | CALLED ON NULL INPUT ]
}

```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente

*OR ALTER*
**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 und höher) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Ändert die Funktion bedingt, sofern diese bereits vorhanden ist.

> [!NOTE]
> Eine optionale [OR ALTER]-Syntax für die CLR-ist ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 CU1 verfügbar.

*schema_name* entspricht dem Namen des Schemas, zu dem die benutzerdefinierte Funktion gehört.

*function_name* entspricht dem Namen der benutzerdefinierten Funktion. Funktionsnamen müssen den Regeln für [Bezeichner](../../relational-databases/databases/database-identifiers.md) entsprechen und innerhalb der Datenbank und für jedes Schema eindeutig sein.

> [!NOTE]
> Auf den Funktionsnamen müssen Klammern folgen, selbst wenn kein Parameter angegeben ist.

@*parameter_name* ist ein Parameter in der benutzerdefinierten Funktion. Ein oder mehrere Parameter können deklariert werden.

Eine Funktion kann maximal 2.100 Parameter haben. Der Benutzer muss beim Ausführen einer Funktion den Wert jedes deklarierten Parameters angeben (sofern kein Standardwert für den betreffenden Parameter definiert ist).

Geben Sie einen Parameternamen an, der mit dem Zeichen (@) beginnt. Der Parametername muss den Regeln für Bezeichner entsprechen. Parameter gelten lokal in der jeweiligen Funktion. Dieselben Parameternamen können in anderen Funktionen verwendet werden. Parameter können nur den Platz von Konstanten einnehmen. Sie können nicht anstelle von Tabellennamen, Spaltennamen oder Namen anderer Datenbankobjekte verwendet werden.

> [!NOTE]
> ANSI_WARNINGS wird bei der Übergabe von Parametern in einer gespeicherten Prozedur oder in einer benutzerdefinierten Funktion oder beim Deklarieren und Festlegen von Variablen in einer Batchanweisung nicht berücksichtigt. Wird beispielsweise eine Variable als **char(3)** definiert und dann auf einen Wert festgelegt, der länger als drei Zeichen ist, werden die Daten auf die definierte Größe abgeschnitten, und die Anweisung `INSERT` oder `UPDATE` wird erfolgreich ausgeführt.

[ *type_schema_name*. ] *parameter_data_type* entspricht dem Parameterdatentyp und optional dem Schema, zu dem der Datentyp gehört. Für [!INCLUDE[tsql](../../includes/tsql-md.md)]-Funktionen sind abgesehen vom **timestamp**-Datentyp alle Datentypen zulässig, einschließlich CLR-benutzerdefinierter Typen und benutzerdefinierte Tabellentypen. Für CLR-Funktionen sind abgesehen von den Datentypen **text**, **ntext**, **image**, und **timestamp** sowie benutzerdefinierten Tabellentypen alle Datentypen zulässig, einschließlich benutzerdefinierter CLR-Typen. Die nicht skalaren Typen **cursor** und **table** können weder in [!INCLUDE[tsql](../../includes/tsql-md.md)]- noch in CLR-Funktionen als Parameterdatentyp angegeben werden.

Wenn *type_schema_name* nicht angegeben ist, sucht die [!INCLUDE[ssDE](../../includes/ssde-md.md)] nach *scalar_parameter_data_type* in der folgenden Reihenfolge:

- Das Schema, das die Namen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Systemdatentypen enthält
- Das Standardschema des aktuellen Benutzers in der aktuellen Datenbank
- Das **dbo** -Schema in der aktuellen Datenbank

[ =*default* ] ist ein Standardwert für den Parameter. Wenn ein *default*-Wert definiert ist, kann die Funktion ausgeführt werden, ohne dass ein Wert für diesen Parameter angegeben werden muss.

> [!NOTE]
> Standardparameterwerte können mit Ausnahme der Datentypen **varchar(max)** und **varbinary(max)** für CLR-Funktionen angegeben werden.

Wenn ein Parameter der Funktion über einen Standardwert verfügt, muss beim Aufrufen der Funktion das DEFAULT-Schlüsselwort angegeben werden, um den Standardwert abzurufen. In diesem Punkt gibt es einen Unterschied zum Verwenden von Parametern in einer gespeicherten Prozedur. Fehlt im Aufruf einer gespeicherten Prozedur ein Parameter, der einen Standardwert hat, wird automatisch dieser Standardwert verwendet. Beim Aufrufen einer Skalarfunktion mit der EXECUTE-Anweisung ist das DEFAULT-Schlüsselwort jedoch nicht erforderlich.

READONLY gibt an, dass der Parameter nicht aktualisiert oder innerhalb der Definition der Funktion geändert werden kann. READONLY ist bei benutzerdefinierten Tabellentypparametern (TVPs) erforderlich und kann nicht für andere Parametertypen verwendet werden.

*return_data_type* ist der Rückgabewert einer benutzerdefinierten Skalarfunktion. Für [!INCLUDE[tsql](../../includes/tsql-md.md)]-Funktionen sind abgesehen vom **timestamp**-Datentyp alle Datentypen zulässig, einschließlich benutzerdefinierter CLR-Typen. Für CLR-Funktionen sind abgesehen von den Datentypen **text**, **ntext**, **image** und **timestamp** alle Datentypen zulässig, einschließlich benutzerdefinierter CLR-Typen. Die nicht skalaren Typen **cursor** und **table** können weder in [!INCLUDE[tsql](../../includes/tsql-md.md)]- noch in CLR-Funktionen als Rückgabedatentyp angegeben werden.

*function_body* gibt an, dass eine Reihe von [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen, die zusammen keine Nebeneffekte erzeugen, z. B. das Ändern einer Tabelle, den Wert der Funktion definiert. *function_body* wird nur in Skalarfunktionen sowie in Tabellenwertfunktionen mit mehreren Anweisungen verwendet.

In Skalarfunktionen entspricht *function_body* einer Reihe von [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen, die zusammen einen Skalarwert ergeben.

In Tabellenwertfunktionen mit mehreren Anweisungen entspricht *function_body* einer Reihe von [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen, die eine TABLE-Rückgabevariable auffüllen.

*scalar_expression* gibt den Skalarwert an, der von der Skalarfunktion zurückgegeben wird.

TABLE gibt an, dass der Rückgabewert der Tabellenwertfunktion eine Tabelle ist. Nur Konstanten und @*local_variables* können an Tabellenwertfunktionen übergeben werden.

In Inline-Tabellenwertfunktionen wird der TABLE-Rückgabewert durch eine einzige SELECT-Anweisung definiert. Inlinefunktionen haben keine zugeordneten Rückgabevariablen.

<a name="mstvf"></a> In Tabellenwertfunktionen mit mehreren Anweisungen ist \@*return_variable* eine TABLE-Variable, die zum Speichern und Sammeln der Zeilen verwendet wird, die als Wert der Funktion zurückgegeben werden sollen. \@*return_variable* kann nur für [!INCLUDE[tsql](../../includes/tsql-md.md)]-Funktionen, nicht für CLR-Funktionen angegeben werden.

*select_stmt* ist die einzelne SELECT-Anweisung, die den Rückgabewert einer Inline-Tabellenwertfunktion definiert.

ORDER (\<order_clause>) gibt die Reihenfolge an, in der Ergebnisse von der Tabellenwertfunktion zurückgegeben werden. Weitere Informationen finden Sie im Abschnitt [Verwenden der Sortierreihenfolge in CLR-Tabellenwertfunktionen](#using-sort-order-in-clr-table-valued-functions) weiter unten in diesem Thema.

EXTERNAL NAME \<method_specifier> *assembly_name*.*class_name*.*method_name*
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] SP1 und höher).

Gibt die Assembly und die Methode an, auf die der erstellte Funktionsname verweisen soll.

- *assembly_name* muss einem Wert in der Spalte `name` von `SELECT * FROM sys.assemblies;` entsprechen.

  Dies ist der Name, der für die `CREATE ASSEMBLY`-Anweisung verwendet wurde.

- *class_name* muss einem Wert in der Spalte `assembly_name` von `SELECT * FROM sys.assembly_modules;` entsprechen.

  Häufig enthält der Wert einen Punkt. In solchen Fällen muss der Wert aufgrund der Transact-SQL-Syntax von eckigen Klammern [] oder doppelten Anführungszeichen "" umschlossen werden.

- *method_name* muss einem Wert in der Spalte `method_name` von `SELECT * FROM sys.assembly_modules;` entsprechen.

  Die Methode muss statisch sein.

In einem typischen Beispiel für MyFood.DLL, bei dem sich alle Typen im MyFood-Namespace befinden, könnte der Wert `EXTERNAL NAME` folgender sein: `MyFood.[MyFood.MyClass].MyStaticMethod`.

> [!NOTE]
>
> - Standardmäßig kann [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] keinen CLR-Code ausführen. Sie können Datenbankobjekte, die auf CLR-Module (Common Language Runtime) verweisen, erstellen, ändern und löschen. Bevor Sie diese Verweise in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ausführen können, müssen Sie jedoch die Option [clr enabled](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md) aktivieren. Verwenden Sie dazu [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).
> - Diese Option ist in einer enthaltenen Datenbank nicht verfügbar.

*\<*table_type_definition*>* ( { \<column_definition> \<column_constraint>| \<computed_column_definition> } [ \<table_constraint> ] [ ,...*n* ] ) definiert den Tabellendatentyp für eine [!INCLUDE[tsql](../../includes/tsql-md.md)]-Funktion. Die Tabellendeklaration schließt Spaltendefinitionen und Spalten- oder Tabelleneinschränkungen ein. Die Tabelle wird immer in der primären Dateigruppe abgelegt.

*\< clr_table_type_definition >* ( { *column_name**data_type* } [ ,...*n* ] ) **Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] SP1 und höher) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] ([Vorschauversion in einigen Regionen](/azure/azure-sql/database/features-comparison?WT.mc_id=TSQL_GetItTag)).

Definiert die Tabellendatentypen für eine CLR-Funktion. Die Tabellendeklaration schließt nur Spaltennamen und Datentypen ein. Die Tabelle wird immer in der primären Dateigruppe abgelegt.

NULL|NOT NULL wird nur für nativ kompilierte, benutzerdefinierte Skalarfunktionen unterstützt. Weitere Informationen dazu finden Sie unter [Benutzerdefinierte Skalarfunktionen für In-Memory-OLTP](../../relational-databases/in-memory-oltp/scalar-user-defined-functions-for-in-memory-oltp.md).

NATIVE_COMPILATION gibt an, ob eine benutzerdefinierte Funktion nativ kompiliert wird. Dieses Argument ist für nativ kompilierte, benutzerdefinierte Skalarfunktionen erforderlich.

BEGIN ATOMIC WITH wird nur für nativ kompilierte, benutzerdefinierte Skalarfunktionen unterstützt und ist erforderlich. Weitere Informationen finden Sie unter [ATOMIC-Blöcke](../../relational-databases/in-memory-oltp/atomic-blocks-in-native-procedures.md).

Das Argument SCHEMABINDING ist für nativ kompilierte, benutzerdefinierte Skalarfunktionen erforderlich.

EXECUTE AS ist für nativ kompilierte, benutzerdefinierte Skalarfunktionen erforderlich.

**\<function_option>::= und \<clr_function_option>::=**

Gibt an, dass die Funktion mindestens über eine der folgenden Optionen verfügen wird.

ENCRYPTION **Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] SP1 und höher).

Gibt an, dass [!INCLUDE[ssDE](../../includes/ssde-md.md)] den ursprünglichen Text der CREATE FUNCTION-Anweisung in ein verborgenes Format konvertiert. Die Ausgabe der Verbergung ist nicht direkt in den Katalogsichten sichtbar. Benutzer, die keinen Zugriff auf Systemtabellen oder Datenbankdateien haben, können den verborgenen Text nicht abrufen. Der Text ist jedoch für berechtigte Benutzer verfügbar, die entweder auf die Systemtabellen über den [DAC-Port](../../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md) oder direkt auf die Datenbankdateien zugreifen. Des Weiteren können Benutzer, die einen Debugger an den Serverprozess anfügen können, die Originalprozedur zur Laufzeit vom Arbeitsspeicher abrufen. Weitere Informationen zu Berechtigungen zum Zugreifen auf Systemmetadaten finden Sie unter [Konfigurieren der Sichtbarkeit von Metadaten](../../relational-databases/security/metadata-visibility-configuration.md).

Mit dieser Option verhindern Sie, dass die Funktion als Teil der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Replikation veröffentlicht wird. Diese Option kann nicht für CLR-Funktionen angegeben werden.

SCHEMABINDING gibt an, dass die Funktion an die Datenbankobjekte gebunden ist, auf die sie verweist. Wenn SCHEMABINDING angegeben ist, können an Basisobjekten keine Änderungen vorgenommen werden, die die Funktionsdefinition betreffen können. Zunächst muss die Funktionsdefinition selbst geändert oder gelöscht werden, um Abhängigkeiten in dem zu ändernden Objekt zu entfernen.

Die Bindung der Funktion an die Objekte, auf die sie verweist, wird nur bei einer der folgenden Aktionen entfernt:

- Die Funktion wird gelöscht.
- Die Funktion wird mithilfe der `ALTER`-Anweisung geändert, wobei die Option `SCHEMABINDING` nicht angegeben ist.

Eine Funktion kann nur dann schemagebunden sein, wenn die folgenden Bedingungen erfüllt sind:

- Die Funktion ist eine [!INCLUDE[tsql](../../includes/tsql-md.md)]-Funktion.
- Die benutzerdefinierten Funktionen und Sichten, auf die die Funktion verweist, sind ebenfalls schemagebunden.
- In der Funktion sind die Verweise auf Objekte mithilfe von zweiteiligen Namen angegeben.
- Die Funktion und die Objekte, auf die sie verweist, gehören zu derselben Datenbank.
- Der Benutzer, der die `CREATE FUNCTION`-Anweisung ausgeführt hat, besitzt `REFERENCES`-Berechtigungen für die Datenbankobjekte, auf die die Funktion verweist.

RETURNS NULL ON NULL INPUT | **CALLED ON NULL INPUT** gibt das **OnNULLCall**-Attribut einer Skalarfunktion an. Wenn das Attribut nicht angegeben ist, wird standardmäßig CALLED ON NULL INPUT verwendet. Dies bedeutet, dass der Hauptteil der Funktion ausgeführt wird, selbst wenn NULL als ein Argument übergeben wird.

Wenn RETURNS NULL ON NULL INPUT in einer CLR-Funktion angegeben wird, bedeutet dies, dass [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] NULL zurückgeben kann, wenn eines der ihm zugestellten Argumente NULL ist, ohne den eigentlichen Hauptteil der Funktion aufzurufen. Wenn die Methode einer in \<method_specifier> angegebenen CLR-Funktion bereits über ein benutzerdefiniertes Attribut verfügt, das RETURNS NULL ON NULL INPUT anzeigt, die CREATE FUNCTION-Anweisung jedoch CALLED ON NULL INPUT anzeigt, hat die CREATE FUNCTION-Anweisung Vorrang. Das **OnNULLCall**-Attribut kann nicht für CLR-Tabellenwertfunktionen angegeben werden.

Die EXECUTE AS-Klausel gibt den Sicherheitskontext an, in dem die benutzerdefinierte Funktion ausgeführt wird. Deshalb können Sie steuern, welches Benutzerkonto [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwendet, um Berechtigungen für eines der Datenbankobjekte, auf die in der Funktion verwiesen wird, zu überprüfen.

> [!NOTE]
> `EXECUTE AS` kann nicht für Inline-Tabellenwertfunktionen angegeben werden.

Weitere Informationen finden Sie unter [EXECUTE AS-Klausel &#40;Transact-SQL&#41;](../../t-sql/statements/execute-as-clause-transact-sql.md).

INLINE = { ON | OFF } **Gilt für:** SQL Server 2019 und höher

Gibt an, ob für diese benutzerdefinierte Skalarfunktion ein Inlining durchgeführt werden sollte. Diese Klausel gilt nur für benutzerdefinierte Skalarfunktionen. Die `INLINE`-Klausel ist nicht verbindlich. Wenn die `INLINE`-Klausel nicht festgelegt ist, wird sie automatisch auf ON/OFF festgelegt, je nachdem, ob ein Inlining für die benutzerdefinierte Funktion durchgeführt werden kann. Wenn `INLINE=ON` festgelegt ist, aber für die benutzerdefinierte Funktion kein Inlining durchgeführt werden kann, wird ein Fehler ausgegeben. Weitere Informationen finden Sie unter [Scalar UDF Inlining (Inlining benutzerdefinierter Skalarfunktionen)](../../relational-databases/user-defined-functions/scalar-udf-inlining.md).

**\< column_definition >::=**

Definiert den Tabellendatentyp. Die Tabellendeklaration schließt Spaltendefinitionen und Einschränkungen ein. Für CLR-Funktionen können nur *column_name* und *data_type* angegeben werden.

*column_name* ist der Name einer Spalte in der Tabelle. Spaltennamen müssen den Regeln für Bezeichner entsprechen und in der Tabelle eindeutig sein. *column_name* kann zwischen 1 und 128 Zeichen aufweisen.

*data_type* gibt den Datentyp der Spalte an. Für [!INCLUDE[tsql](../../includes/tsql-md.md)]-Funktionen sind abgesehen von **timestamp** alle Datentypen zulässig, einschließlich benutzerdefinierter CLR-Typen. Für CLR-Funktionen sind abgesehen von **text**, **ntext**, **image**, **char**, **varchar**, **varchar(max)** und **timestamp** alle Datentypen zulässig, einschließlich benutzerdefinierter CLR-Typen. Der nicht skalare Typ **cursor** kann weder in [!INCLUDE[tsql](../../includes/tsql-md.md)]- noch in CLR-Funktionen als Spaltendatentyp angegeben werden.

DEFAULT *constant_expression* gibt den Wert an, der für die Spalte bereitgestellt wird, wenn während eines Einfügevorgangs kein expliziter Wert angegeben wurde. *constant_expression* ist ein konstanter Wert, ein NULL-Wert oder ein Systemfunktionswert. DEFAULT-Definitionen können auf alle Spalten angewendet werden, mit Ausnahme von Spalten mit der IDENTITY-Eigenschaft. DEFAULT kann nicht für CLR-Tabellenwertfunktionen angegeben werden.

COLLATE *collation_name* Gibt die Sortierung für die Spalte an. Wenn keine Sortierung angegeben ist, wird der Spalte die Standardsortierung der Datenbank zugewiesen. Als Sortierungsname kann entweder der Name einer Windows-Sortierreihenfolge oder ein SQL-Sortierungsname verwendet werden. Eine Liste und weitere Informationen zu Sortierungen finden Sie unter [Name der Windows-Sortierung &#40;Transact-SQL&#41;](../../t-sql/statements/windows-collation-name-transact-sql.md) und [SQL Server-Sortierungsname &#40;Transact-SQL&#41;](../../t-sql/statements/sql-server-collation-name-transact-sql.md).

Mit der COLLATE-Klausel können Sie nur die Sortierungen von Spalten der Datentypen **char**, **varchar**, **nchar** und **nvarchar** ändern.

> [!NOTE]
> `COLLATE` kann nicht für CLR-Tabellenwertfunktionen angegeben werden.

ROWGUIDCOL gibt an, dass die neue Spalte eine Spalte mit für alle Zeilen global eindeutigen Bezeichnern ist. Nur eine **uniqueidentifier**-Spalte pro Tabelle kann als ROWGUIDCOL-Spalte gekennzeichnet werden. Die ROWGUIDCOL-Eigenschaft kann nur einer **uniqueidentifier**-Spalte zugewiesen werden.

Die ROWGUIDCOL-Eigenschaft erzwingt keine Eindeutigkeit der in der Spalte gespeicherten Werte. Sie erzeugt auch nicht automatisch Werte für neue Zeilen, die in die Tabelle eingefügt werden. Um eindeutige Werte für jede Spalte zu erzeugen, verwenden Sie die NEWID-Funktion in INSERT-Anweisungen. Ein Standardwert kann angegeben werden, NEWID wird jedoch nicht als Standardwert unterstützt.

IDENTITY Gibt an, dass es sich bei der neuen Spalte um eine Identitätsspalte handelt. Wenn der Tabelle eine neue Zeile hinzugefügt wird, stellt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] einen eindeutigen, inkrementellen Wert für die Spalte bereit. Identitätsspalten werden normalerweise zusammen mit PRIMARY KEY-Einschränkungen verwendet, um als eindeutiger Zeilenbezeichner für die Tabelle zu dienen. Die IDENTITY-Eigenschaft kann folgenden Spalten zugewiesen werden: **tinyint**, **smallint**, **int**, **bigint**, **decimal(p,0)** oder **numeric(p,0)** . Es kann nur eine Identitätsspalte pro Tabelle erstellt werden. Gebundene Standardwerte und DEFAULT-Einschränkungen können nicht mit einer Identitätsspalte verwendet werden. Sie müssen entweder *seed* und den *increment* oder keinen von beiden angeben. Wurden Ausgangswert und inkrementeller Wert nicht angegeben, ist der Standardwert (1,1).

IDENTITY kann nicht für CLR-Tabellenwertfunktionen angegeben werden.

*seed* ist der ganzzahlige Wert, der der ersten Zeile in der Tabelle zugewiesen werden soll.

*increment* ist der ganzzahlige Wert, der zum *seed*-Wert für nachfolgende Zeilen in der Tabelle hinzugefügt werden soll.

 **\< column_constraint >::= und \< table_constraint>::=**

Definiert die Einschränkung für eine bestimmte Spalte oder Tabelle. Für CLR-Funktionen ist der einzige zulässige Einschränkungstyp NULL. Benannte Einschränkungen sind nicht zulässig.

NULL | NOT NULL bestimmt, ob NULL-Werte in der Spalte zulässig sind. NULL ist genau genommen keine Einschränkung, kann jedoch wie NOT NULL verwendet werden. NOT NULL kann nicht für CLR-Tabellenwertfunktionen angegeben werden.

PRIMARY KEY ist eine Einschränkung, die Entitätsintegrität für eine angegebene Spalte durch einen eindeutigen Index erzwingt. In benutzerdefinierten Tabellenwertfunktionen kann die PRIMARY KEY-Einschränkung nur auf einer Spalte pro Tabelle erstellt werden. PRIMARY KEY kann nicht für CLR-Tabellenwertfunktionen angegeben werden.

UNIQUE Eine Einschränkung, die Entitätsintegrität für eine angegebene Spalte (oder Spalten) durch einen eindeutigen Index bereitstellt. Eine Tabelle kann mehrere UNIQUE-Einschränkungen haben. UNIQUE kann nicht für CLR-Tabellenwertfunktionen angegeben werden.

CLUSTERED | NONCLUSTERED Gibt an, dass ein gruppierter oder nicht gruppierter Index für die PRIMARY KEY- oder UNIQUE-Einschränkung erstellt wird. PRIMARY KEY-Einschränkungen verwenden CLUSTERED, und UNIQUE-Einschränkungen verwenden NONCLUSTERED.

CLUSTERED kann nur für eine einzelne Einschränkung angegeben werden. Wenn neben CLUSTERED für eine UNIQUE-Einschränkung auch eine PRIMARY KEY-Einschränkung angegeben wird, verwendet die Einschränkung PRIMARY KEY den Wert NONCLUSTERED.

CLUSTERED und NONCLUSTERED können nicht für CLR-Tabellenwertfunktionen angegeben werden.

CHECK Eine Einschränkung, die Domänenintegrität erzwingt, indem die möglichen Eingabewerte für eine oder mehrere Spalten beschränkt wird. CHECK-Einschränkungen können nicht für CLR-Tabellenwertfunktionen angegeben werden.

*logical_expression* Ein logischer Ausdruck, der TRUE oder FALSE zurückgibt.

**\<computed_column_definition>::=**

Gibt eine berechnete Spalte an. Weitere Informationen zu berechneten Spalten finden Sie unter [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md).

*column_name* ist der Name der berechneten Spalte.

*computed_column_expression* ist ein Ausdruck, der den Wert einer berechneten Spalte definiert.

**\<index_option>::=**

Gibt die Indexoptionen für den PRIMARY KEY- oder UNIQUE-Index an. Weitere Informationen zu Indexoptionen finden Sie unter [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).

PAD_INDEX = { ON | **OFF** } legt die Indexauffüllung fest. Der Standardwert ist OFF.

FILLFACTOR = *fillfactor* gibt einen Prozentsatz an, der anzeigt, wie weit die [!INCLUDE[ssDE](../../includes/ssde-md.md)] die Blattebene jeder Indexseite während der Indexerstellung oder -änderung füllen soll. *fillfactor* muss ein ganzzahliger Wert zwischen 1 und 100 sein. Die Standardeinstellung ist 0.

IGNORE_DUP_KEY = { ON | **OFF** } Diese Option gibt die Fehlermeldung an, wenn ein Einfügevorgang versucht, doppelte Schlüsselwerte in einen eindeutigen Index einzufügen. Die IGNORE_DUP_KEY-Option gilt nur für Einfügevorgänge nach dem Erstellen oder Neuerstellen des Index. Der Standardwert ist OFF.

STATISTICS_NORECOMPUTE = { ON | **OFF**} gibt an, ob Verteilungsstatistiken neu berechnet werden. Der Standardwert ist OFF.

ALLOW_ROW_LOCKS = { **ON** | OFF } gibt an, ob Zeilensperren zulässig sind. Der Standardwert ist ON.

ALLOW_PAGE_LOCKS = { **ON** | OFF } gibt an, ob Seitensperren zulässig sind. Der Standardwert ist ON.

## <a name="best-practices"></a>Bewährte Methoden

Wenn eine benutzerdefinierte Funktion nicht mit der `SCHEMABINDING`-Klausel erstellt wurde, können sich die an zugrunde liegenden Objekten vorgenommenen Änderungen auf die Definition der Funktion auswirken und bei Aufruf der Funktion zu unerwarteten Ergebnissen führen. Es wird empfohlen, eine der folgenden Methoden zu implementieren, damit die Funktion aufgrund von Änderungen an den zugrunde liegenden Objekten nicht veraltet ist:

- Geben Sie beim Erstellen der Funktion die `WITH SCHEMABINDING`-Klausel an. Hiermit wird sichergestellt, dass die Objekte, auf die in der Funktionsdefinition verwiesen wird, nicht geändert werden können, es sei denn, die Funktion wird auch geändert.
- Führen Sie die gespeicherte Prozedur [sp_refreshsqlmodule](../../relational-databases/system-stored-procedures/sp-refreshsqlmodule-transact-sql.md) aus, nachdem Sie ein Objekt geändert haben, das in der Definition der Funktion angegeben ist.

> [!IMPORTANT]
> Weitere Informationen und Überlegungen zur Leistung von Inline-Tabellenwertfunktionen und Tabellenwertfunktionen mit mehreren Anweisungen finden Sie unter [Erstellen benutzerdefinierter Funktionen &#40;Datenbank-Engine&#41;](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md).

## <a name="data-types"></a>Datentypen

Wenn Parameter in einer CLR-Funktion angegeben werden, sollte es sich dabei um [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Typen handeln, wie weiter oben für *scalar_parameter_data_type* definiert. Weitere Informationen zum Vergleichen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Systemdatentypen mit CLR-Integrationsdatentypen oder Datentypen der [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] Common Language Runtime finden Sie unter [Zuordnen von CLR-Parameterdaten](../../relational-databases/clr-integration-database-objects-types-net-framework/mapping-clr-parameter-data.md).

Damit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] beim Überladen in einer Klasse auf die richtige Methode verweist, muss die in \<method_specifier> angegebene Methode folgende Merkmale aufweisen:

- Die gleiche Anzahl von Parametern wie in [ ,...*n* ] angegeben.
- Alle Parameter nach Wert erhalten, nicht nach Verweis.
- Parametertypen verwenden, die mit den in der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Funktion angegebenen kompatibel sind.

Wenn der Rückgabedatentyp der CLR-Funktion einen Tabellentyp (RETURNS TABLE) angibt, sollte der Rückgabedatentyp der Methode in \<method_specifier> dem Typ **IEnumerator** oder **IEnumerable** entsprechen. Es wird zudem angenommen, dass die Schnittstelle vom Ersteller der Funktion implementiert wird. Im Gegensatz zu [!INCLUDE[tsql](../../includes/tsql-md.md)]-Funktionen können CLR-Funktionen keine PRIMARY KEY-, UNIQUE- oder CHECK-Einschränkungen in \<table_type_definition> einschließen. Die Datentypen der Spalten, die in \<table_type_definition> angegeben werden, müssen mit den Typen der entsprechenden Spalten des Resultsets übereinstimmen, das von der in \<method_specifier> angegebenen Methode zur Ausführungszeit zurückgegeben wird. Diese Typprüfung wird zum Zeitpunkt der Funktionserstellung nicht durchgeführt.

Weitere Informationen zum Programmieren von CLR-Funktionen finden Sie unter [CLR-benutzerdefinierte Funktionen](../../relational-databases/clr-integration-database-objects-user-defined-functions/clr-user-defined-functions.md).

## <a name="general-remarks"></a>Allgemeine Hinweise

Skalarfunktionen können dort aufgerufen werden, wo Skalarausdrücke verwendet werden. Hierzu gehören berechnete Spalten und CHECK-Einschränkungsdefinitionen. Skalarfunktionen können auch mithilfe der [EXECUTE](../../t-sql/language-elements/execute-transact-sql.md)-Anweisung ausgeführt werden. Skalarfunktionen müssen mindestens mit dem zweiteiligen Namen der Funktion ( *<schema>.<function>* ) aufgerufen werden. Weitere Informationen zu mehrteiligen Namen finden Sie unter [Transact-SQL-Syntaxkonventionen (Transact-SQL)](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md). Tabellenwertfunktionen können dort aufgerufen werden, wo Tabellenausdrücke in der `FROM`-Klausel der Anweisungen `SELECT`, `INSERT`, `UPDATE` oder `DELETE` zulässig sind. Weitere Informationen zu benutzerdefinierten Funktionen finden Sie unter [Ausführen von benutzerdefinierten Funktionen](../../relational-databases/user-defined-functions/execute-user-defined-functions.md).

## <a name="interoperability"></a>Interoperabilität

Die folgenden Anweisungen sind in einer Funktion zulässig:

- Zuweisungsanweisungen
- Anweisungen zur Ablaufsteuerung mit Ausnahme von `TRY...CATCH`-Anweisungen.
- `DECLARE`-Anweisungen zum Definieren lokaler Datenbankvariablen und lokaler Cursor.
- `SELECT`-Anweisungen, die Auswahllisten mit Ausdrücken enthalten, die lokalen Variablen Werte zuweisen.
- Cursorvorgänge, die auf lokale Cursor verweisen, die in der Funktion deklariert, geöffnet und geschlossen werden und deren Zuordnungen in der Funktion aufgehoben werden. Es sind nur `FETCH`-Anweisungen zulässig, die lokalen Variablen Werte mit der `INTO`-Klausel zuweisen. `FETCH`-Anweisungen, die Daten an den Client zurückgeben, sind nicht zulässig.
- `INSERT`-, `UPDATE`- und `DELETE`-Anweisungen, die lokale Tabellenvariablen ändern.
- `EXECUTE`-Anweisungen, die erweiterte gespeicherte Prozeduren aufrufen.

Weitere Informationen finden Sie unter [Erstellen von benutzerdefinierten Funktionen &#40;Datenbank-Engine&#41;](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md).

### <a name="computed-column-interoperability"></a>Interoperabilität bei berechneten Spalten

Funktionen verfügen über die folgenden Eigenschaften. Die Werte dieser Eigenschaften bestimmen, ob Funktionen in permanent berechneten oder indizierten berechneten Spalten verwendet werden können.

|Eigenschaft|BESCHREIBUNG|Notizen|
|--------------|-----------------|-----------|
|**IsDeterministic**|Die Funktion ist deterministisch oder nicht deterministisch.|Lokaler Datenzugriff ist in deterministischen Funktionen zulässig. Funktionen, die immer dasselbe Ergebnis zurückgeben, wenn sie mit bestimmten Eingabewerten und mit demselben Datenbankstatus aufgerufen werden, werden beispielsweise als deterministisch bezeichnet.|
|**IsPrecise**|Die Funktion ist präzise oder unpräzise.|Unpräzise Funktionen enthalten Vorgänge wie Gleitkommatransaktionen.|
|**IsSystemVerified**|Die Präzisions- und Determinismuseigenschaften der Funktion können von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] geprüft werden.||
|**SystemDataAccess**|Die Funktion greift auf Systemdaten (Systemkataloge oder virtuelle Systemtabellen) in der lokalen Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zu.||
|**UserDataAccess**|Die Funktion greift auf Benutzerdaten in der lokalen Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zu.|Schließt benutzerdefinierte Tabellen und temporäre Tabellen ein, jedoch keine Tabellenvariablen.|

Die Präzisions- und Determinismuseigenschaften von [!INCLUDE[tsql](../../includes/tsql-md.md)]-Funktionen werden automatisch von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bestimmt. Die Datenzugriffs- und Determinismuseigenschaften von CLR-Funktionen können vom Benutzer angegeben werden. Weitere Informationen finden Sie unter [Übersicht über benutzerdefinierte Attribute der CLR-Integration](../../relational-databases/clr-integration/database-objects/clr-integration-custom-attributes-for-clr-routines.md).

Verwenden Sie [OBJECTPROPERTYEX](../../t-sql/functions/objectpropertyex-transact-sql.md), um die aktuellen Werte für diese Eigenschaften anzuzeigen.

> [!IMPORTANT]
> Damit Funktionen deterministisch sind, müssen sie mit `SCHEMABINDING` erstellt werden.

Eine berechnete Spalte, die eine benutzerdefinierte Funktion aufruft, kann in einem Index verwendet werden, sofern die benutzerdefinierte Funktion über folgende Eigenschaftswerte verfügt:

- **IsDeterministic** = TRUE
- **IsSystemVerified** = TRUE (es sei denn, eine berechnete Spalte wird beibehalten)
- **UserDataAccess** = FALSE
- **SystemDataAccess** = FALSE

Weitere Informationen finden Sie unter [Indexes on Computed Columns](../../relational-databases/indexes/indexes-on-computed-columns.md).

### <a name="calling-extended-stored-procedures-from-functions"></a>Aufrufen erweiterter gespeicherter Prozeduren aus Funktionen

Eine erweiterte gespeicherte Prozedur kann, wenn sie aus einer Funktion heraus aufgerufen wird, keine Resultsets an den Client zurückgeben. Jede ODS-API, die Resultsets an den Client zurückgibt, gibt FAIL zurück. Eine erweiterte gespeicherte Prozedur kann eine Verbindung zurück zu einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] herstellen. Diese Prozedur sollte allerdings nicht versuchen, Bestandteil der gleichen Transaktion zu werden, zu der die Funktion gehört, die die erweiterte gespeicherte Prozedur aufgerufen hat.

Ähnlich wie Aufrufe aus einem Batch oder einer gespeicherten Prozedur wird auch eine erweiterte gespeicherte Prozedur im Kontext des Windows-Sicherheitskontos ausgeführt, unter dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ausgeführt wird. Der Besitzer der gespeicherten Prozedur sollte dies bedenken, wenn er anderen Benutzern für die gespeicherte Prozedur EXECUTE-Berechtigungen erteilt.

## <a name="limitations-and-restrictions"></a>Einschränkungen

Mit benutzerdefinierten Funktionen können keine Aktionen ausgeführt werden, die den Status einer Datenbank ändern.

Benutzerdefinierte Funktionen dürfen keine `OUTPUT INTO`-Klausel enthalten, deren Ziel eine Tabelle ist.

Die folgenden [!INCLUDE[ssSB](../../includes/sssb-md.md)]-Anweisungen können nicht in die Definition einer benutzerdefinierten [!INCLUDE[tsql](../../includes/tsql-md.md)]-Funktion eingeschlossen werden:

- `BEGIN DIALOG CONVERSATION`
- `END CONVERSATION`
- `GET CONVERSATION GROUP`
- `MOVE CONVERSATION`
- `RECEIVE`
- `SEND`

Benutzerdefinierte Funktionen können geschachtelt werden. Dies bedeutet, dass eine benutzerdefinierte Funktion eine andere aufrufen kann. Die Schachtelungsebene wird um eins erhöht, wenn die aufgerufene Funktion mit der Ausführung beginnt, und wird wieder um eins erniedrigt, wenn die aufgerufene Funktion die Ausführung beendet. Benutzerdefinierte Funktionen unterstützen bis zu 32 geschachtelte Ebenen. Ein Überschreiten der maximalen Schachtelungsebenen verursacht das Fehlschlagen der gesamten Funktionsaufrufskette. Alle Verweise auf verwalteten Code von einer benutzerdefinierten [!INCLUDE[tsql](../../includes/tsql-md.md)]-Funktion aus gelten hinsichtlich des Maximums von 32 Schachtelungsebenen als eine Ebene. Methoden, die aus verwaltetem Code aufgerufen werden, werden nicht mitgezählt.

### <a name="using-sort-order-in-clr-table-valued-functions"></a>Verwenden der Sortierreihenfolge in CLR-Tabellenwertfunktionen

Beachten Sie die folgenden Richtlinien, wenn Sie die `ORDER`-Klausel in CLR-Tabellenwertfunktionen verwenden:

- Sie müssen sicherstellen, dass die Ergebnisse immer in der angegebenen Reihenfolge sortiert werden. Wenn die Ergebnisse nicht in der angegebenen Reihenfolge sortiert sind, generiert [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] eine Fehlermeldung beim Ausführen der Abfrage.
- Wenn eine `ORDER`-Klausel angegeben wird, muss die Ausgabe der Tabellenwertfunktion entsprechend der Spaltensortierung (explizit oder implizit) sortiert sein. Wenn (in der DDL für die Tabellenwertfunktion oder über die Datenbanksortierung) beispielsweise Chinesisch für die Spaltensortierung angegeben ist, müssen die zurückgegebenen Ergebnisse entsprechend den Sortierregeln für Chinesisch sortiert werden.
- Die `ORDER`-Klausel wird, sofern angegeben, bei der Rückgabe von Ergebnissen immer von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] überprüft. Dies ist unabhängig davon, ob sie vom Abfrageprozessor zur weiteren Optimierung verwendet wird oder nicht. Verwenden Sie die `ORDER`-Klausel nur, wenn Sie wissen, dass sie für den Abfrageprozessor nützlich ist.
- Der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Abfrageprozessor verwendet die `ORDER`-Klausel automatisch in den folgenden Fällen:

  - Bei Einfügeabfragen, bei denen die `ORDER`-Klausel mit einem Index kompatibel ist.
  - Bei `ORDER BY`-Klauseln, die mit der `ORDER`-Klausel kompatibel sind.
  - Bei Aggregaten, bei denen `GROUP BY` mit der `ORDER`-Klausel kompatibel ist.
  - Bei `DISTINCT`-Aggregaten, bei denen verschiedene Spalten mit der `ORDER`-Klausel kompatibel sind.

Durch die `ORDER`-Klausel wird keine bestimmte Ergebnisreihenfolge bei der Ausführung einer SELECT-Abfrage sichergestellt, es sei denn, in der Abfrage selbst ist ebenfalls `ORDER BY` angegeben. Informationen zu Abfragen nach Spalten, die in der Sortierreihenfolge für Tabellenwertfunktionen enthalten sind, finden Sie unter [sys.function_order_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-function-order-columns-transact-sql.md).

## <a name="metadata"></a>Metadaten

In der folgenden Tabelle werden die Systemkatalogsichten aufgelistet, die Sie verwenden können, um Metadaten zu benutzerdefinierten Funktionen zurückzugeben.

|Systemsicht|BESCHREIBUNG|
|-----------------|-----------------|
|[sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)|Weitere Informationen finden Sie im Beispiel E weiter unten im Abschnitt „Beispiele“.|
|[sys.assembly_modules](../../relational-databases/system-catalog-views/sys-assembly-modules-transact-sql.md)|Zeigt Informationen zu benutzerdefinierten CLR-Funktionen an.|
|[sys.parameters](../../relational-databases/system-catalog-views/sys-parameters-transact-sql.md)|Zeigt Informationen zu den Parametern an, die in benutzerdefinierten Funktionen definiert sind.|
|[sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)|Zeigt die zugrunde liegenden Objekte an, auf die von einer Funktion verwiesen wird.|

## <a name="permissions"></a>Berechtigungen

Erfordert die `CREATE FUNCTION`-Berechtigung in der Datenbank und die `ALTER`-Berechtigung für das Schema, in dem die Funktion erstellt wird. Wenn die Funktion einen benutzerdefinierten Typ angibt, wird die `EXECUTE`-Berechtigung für den Typ benötigt.

## <a name="examples"></a>Beispiele

> [!NOTE]
> Weitere Informationen und Leistungsüberlegungen zu benutzerdefinierten Funktionen finden Sie unter [Erstellen von benutzerdefinierten Funktionen &#40;Datenbank-Engine&#41;](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md).

### <a name="a-using-a-scalar-valued-user-defined-function-that-calculates-the-iso-week"></a>A. Verwenden einer benutzerdefinierten Skalarwertfunktion zur Berechnung der ISO-Woche

Im folgenden Beispiel wird die benutzerdefinierte Funktion `ISOweek` erstellt. Diese Funktion nimmt ein Datumsargument an und berechnet die Nummer der ISO-Woche. Damit diese Funktion richtig rechnet, muss `SET DATEFIRST 1` aufgerufen werden, bevor die Funktion aufgerufen wird.

In diesem Beispiel wird auch das Verwenden der [EXECUTE AS](../../t-sql/statements/execute-as-clause-transact-sql.md)-Klausel zur Angabe des Sicherheitskontexts gezeigt, in dem eine gespeicherte Prozedur ausgeführt werden kann. In dem gezeigten Beispiel gibt die Option `CALLER` an, dass die Prozedur im Kontext des Benutzers, der die Prozedur aufruft, ausgeführt wird. Zusätzlich können Sie die Optionen `SELF`, `OWNER` und *user_name* angeben.

Im Folgenden wird der Funktionsaufruf aufgeführt. Beachten Sie, dass `DATEFIRST` auf `1` festgelegt ist.

```sql
CREATE FUNCTION dbo.ISOweek (@DATE datetime)
RETURNS int
WITH EXECUTE AS CALLER
AS
BEGIN
    DECLARE @ISOweek int;
    SET @ISOweek= DATEPART(wk,@DATE)+1
        -DATEPART(wk,CAST(DATEPART(yy,@DATE) as CHAR(4))+'0104');
--Special cases: Jan 1-3 may belong to the previous year
    IF (@ISOweek=0)
        SET @ISOweek=dbo.ISOweek(CAST(DATEPART(yy,@DATE)-1
            AS CHAR(4))+'12'+ CAST(24+DATEPART(DAY,@DATE) AS CHAR(2)))+1;
--Special case: Dec 29-31 may belong to the next year
    IF ((DATEPART(mm,@DATE)=12) AND
        ((DATEPART(dd,@DATE)-DATEPART(dw,@DATE))>= 28))
    SET @ISOweek=1;
    RETURN(@ISOweek);
END;
GO
SET DATEFIRST 1;
SELECT dbo.ISOweek(CONVERT(DATETIME,'12/26/2004',101)) AS 'ISO Week';
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
ISO Week
----------------
52
```

### <a name="b-creating-an-inline-table-valued-function"></a>B. Erstellen einer Inline-Tabellenwertfunktion

Das folgende Beispiel gibt eine Inline-Tabellenwertfunktion in der [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]-Datenbank zurück. Die Funktion gibt drei Spalten `ProductID`, `Name` und das Aggregat der gesamten Verkäufe des Jahres (nach Filiale sortiert) als `YTD Total` für jedes Produkt zurück, das an die Filiale verkauft wurde.

```sql
CREATE FUNCTION Sales.ufn_SalesByStore (@storeid int)
RETURNS TABLE
AS
RETURN
(
    SELECT P.ProductID, P.Name, SUM(SD.LineTotal) AS 'Total'
    FROM Production.Product AS P
    JOIN Sales.SalesOrderDetail AS SD ON SD.ProductID = P.ProductID
    JOIN Sales.SalesOrderHeader AS SH ON SH.SalesOrderID = SD.SalesOrderID
    JOIN Sales.Customer AS C ON SH.CustomerID = C.CustomerID
    WHERE C.StoreID = @storeid
    GROUP BY P.ProductID, P.Name
);
GO
```

Rufen Sie die Funktion mit dieser Abfrage auf.

```sql
SELECT * FROM Sales.ufn_SalesByStore (602);
```

### <a name="c-creating-a-multi-statement-table-valued-function"></a>C. Erstellen einer Tabellenwertfunktion mit mehreren Anweisungen

Im folgenden Beispiel wird die Tabellenwertfunktion `fn_FindReports(InEmpID)` in der AdventureWorks2012-Datenbank erstellt. Wenn der Funktion eine gültige Mitarbeiter-ID bereitgestellt wird, gibt sie eine Tabelle zurück, die allen Mitarbeitern entspricht, die dem Mitarbeiter entweder direkt oder indirekt unterstellt sind. Die Funktion verwendet eine rekursive Abfrage (Common Table Expression, CTE), um eine hierarchische Mitarbeiterliste zu erstellen. Weitere Informationen zu rekursiven CTEs finden Sie unter [WITH common_table_expression &#40;Transact-SQL&#41;](../../t-sql/queries/with-common-table-expression-transact-sql.md).

```sql
CREATE FUNCTION dbo.ufn_FindReports (@InEmpID INTEGER)
RETURNS @retFindReports TABLE
(
    EmployeeID int primary key NOT NULL,
    FirstName nvarchar(255) NOT NULL,
    LastName nvarchar(255) NOT NULL,
    JobTitle nvarchar(50) NOT NULL,
    RecursionLevel int NOT NULL
)
--Returns a result set that lists all the employees who report to the
--specific employee directly or indirectly.*/
AS
BEGIN
WITH EMP_cte(EmployeeID, OrganizationNode, FirstName, LastName, JobTitle, RecursionLevel) -- CTE name and columns
    AS (
        -- Get the initial list of Employees for Manager n
        SELECT e.BusinessEntityID, e.OrganizationNode, p.FirstName, p.LastName, e.JobTitle, 0
        FROM HumanResources.Employee e
INNER JOIN Person.Person p
ON p.BusinessEntityID = e.BusinessEntityID
        WHERE e.BusinessEntityID = @InEmpID
        UNION ALL
        -- Join recursive member to anchor
        SELECT e.BusinessEntityID, e.OrganizationNode, p.FirstName, p.LastName, e.JobTitle, RecursionLevel + 1
        FROM HumanResources.Employee e
            INNER JOIN EMP_cte
            ON e.OrganizationNode.GetAncestor(1) = EMP_cte.OrganizationNode
INNER JOIN Person.Person p
ON p.BusinessEntityID = e.BusinessEntityID
        )
-- copy the required columns to the result of the function
    INSERT @retFindReports
    SELECT EmployeeID, FirstName, LastName, JobTitle, RecursionLevel
    FROM EMP_cte
    RETURN
END;
GO
-- Example invocation
SELECT EmployeeID, FirstName, LastName, JobTitle, RecursionLevel
FROM dbo.ufn_FindReports(1);

GO
```

### <a name="d-creating-a-clr-function"></a>D: Erstellen einer CLR-Funktion

In diesem Beispiel wird die CLR-Funktion `len_s` erstellt. Bevor die Funktion erstellt wird, wird die Assembly `SurrogateStringFunction.dll` in der lokalen Datenbank registriert.

**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] SP1 und höher).

```sql
DECLARE @SamplesPath nvarchar(1024);
-- You may have to modify the value of this variable if you have
-- installed the sample in a location other than the default location.
SELECT @SamplesPath = REPLACE
    (  physical_name
     , 'Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\master.mdf'
     , 'Microsoft SQL Server\130\Samples\Engine\Programmability\CLR\'
    )
  FROM master.sys.database_files
  WHERE name = 'master';

CREATE ASSEMBLY [SurrogateStringFunction]
FROM @SamplesPath + 'StringManipulate\CS\StringManipulate\bin\debug\SurrogateStringFunction.dll'
WITH PERMISSION_SET = EXTERNAL_ACCESS;
GO

CREATE FUNCTION [dbo].[len_s] (@str nvarchar(4000))
RETURNS bigint
AS EXTERNAL NAME [SurrogateStringFunction].[Microsoft.Samples.SqlServer.SurrogateStringFunction].[LenS];
GO
```

Ein Beispiel zum Erstellen einer CLR-Tabellenwertfunktionen finden Sie unter [CLR-Tabellenwertfunktionen](../../relational-databases/clr-integration-database-objects-user-defined-functions/clr-table-valued-functions.md).

### <a name="e-displaying-the-definition-of-user-defined-functions"></a>E. Anzeigen der Definition von benutzerdefinierten Funktionen

```sql
SELECT definition, type
FROM sys.sql_modules AS m
JOIN sys.objects AS o ON m.object_id = o.object_id
    AND type IN ('FN', 'IF', 'TF');
GO
```

Die Definition von Funktionen, die mithilfe der `ENCRYPTION`-Option erstellt wurden, können nicht mit sys.sql_modules angezeigt werden. Im Gegensatz dazu werden andere Informationen zu den verschlüsselten Funktionen jedoch angezeigt.

## <a name="see-also"></a>Weitere Informationen

- [Erstellen benutzerdefinierter Funktionen &#40;Datenbank-Engine&#41;](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md)
- [ALTER FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-function-transact-sql.md)
- [DROP FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-function-transact-sql.md)
- [OBJECTPROPERTYEX &#40;Transact-SQL&#41;](../../t-sql/functions/objectpropertyex-transact-sql.md)
- [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)
- [sys.assembly_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-assembly-modules-transact-sql.md)
- [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md)
- [CLR-benutzerdefinierte Funktionen](../../relational-databases/clr-integration-database-objects-user-defined-functions/clr-user-defined-functions.md)
- [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)
- [CREATE SECURITY POLICY &#40;Transact-SQL&#41;](../../t-sql/statements/create-security-policy-transact-sql.md)