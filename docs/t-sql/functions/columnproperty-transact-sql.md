---
description: COLUMNPROPERTY (Transact-SQL)
title: COLUMNPROPERTY (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- COLUMNPROPERTY
- COLUMNPROPERTY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- column properties [SQL Server]
- parameters [SQL Server], properties
- COLUMNPROPERTY function
ms.assetid: 2408c264-6eca-4120-bb71-df043c7c2792
author: cawrites
ms.author: chadam
ms.openlocfilehash: 914de62168c17f39f926eddade21d67085b52f39
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100270664"
---
# <a name="columnproperty-transact-sql"></a>COLUMNPROPERTY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Diese Funktion gibt Informationen zu Spalten oder Parametern zurück.
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
COLUMNPROPERTY ( id , column , property )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
*id*  
Ein [Ausdruck](../../t-sql/language-elements/expressions-transact-sql.md), der den Bezeichner (ID) der Tabelle oder Prozedur enthält.
  
*column*  
Ein Ausdruck, der den Namen der Spalte oder des Parameters enthält.
  
*property*  
Das *property*-Argument gibt für das *id*-Argument den Informationstyp an, den die `COLUMNPROPERTY`-Funktion zurückgibt. Für das *property*-Argument sind die folgenden Werte möglich:
  
|Wert|BESCHREIBUNG|Zurückgegebener Wert|  
|---|---|---|
|**AllowsNull**|Lässt NULL-Werte zu.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL = Ungültige Eingabe|  
|**ColumnId**|Der Wert der Spalten-ID, der **sys.columns.column_id** entspricht.|Column ID<br /><br /> **Hinweis:** Wenn mehrere Spalten abgefragt werden, können Lücken in der Abfolge von Werten für Spalten-IDs auftreten.|  
|**FullTextTypeColumn**|Der TYPE COLUMN-Wert in der Tabelle, die die Dokumenttypinformationen der *Spalte* enthält.|Die ID der Volltexttypspalte TYPE COLUMN für den Ausdruck des Spaltennamens, der als zweiter Parameter dieser Funktion übergeben wurde.|  
|**GeneratedAlwaysType**|Wird durch den Spaltenwert systemgeneriert. Entspricht **sys.columns.generated_always_type**|**Gilt für**:  [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher.<br /><br /> 0 = Nicht immer generiert<br /><br /> 1 = Immer am Zeilenanfang generiert<br /><br /> 2 = Immer am Zeilenende generiert|  
|**IsColumnSet**|Spalte ist ein Spaltensatz. Weitere Informationen finden Sie unter [Verwenden von Spaltensätzen](../../relational-databases/tables/use-column-sets.md).|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL = Ungültige Eingabe|  
|**IsComputed**|Die Spalte ist eine berechnete Spalte.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL = Ungültige Eingabe|  
|**IsCursorType**|Der Prozedurparameter ist vom Typ CURSOR.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL = Ungültige Eingabe|  
|**IsDeterministic**|Die Spalte ist deterministisch. Diese Eigenschaft gilt nur für berechnete Spalten und Sichtspalten.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL = Ungültige Eingabe Keine berechnete Spalte oder Sichtspalte.|  
|**IsFulltextIndexed**|Die Spalte wird für die Volltextindizierung registriert.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL = Ungültige Eingabe|  
|**IsHidden**|Wird durch den Spaltenwert systemgeneriert. Entspricht **sys.columns.is_hidden**|**Gilt für**:  [!INCLUDE[ssCurrentLong](../../includes/sscurrent-md.md)] und höher.<br /><br /> 0 = Nicht ausgeblendet<br /><br /> 1 = Ausgeblendet|  
|**IsIdentity**|Die Spalte verwendet die IDENTITY-Eigenschaft.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL = Ungültige Eingabe|  
|**IsIdNotForRepl**|In der Spalte wird die IDENTITY_INSERT-Einstellung überprüft.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL = Ungültige Eingabe|  
|**IsIndexable**|Diese Spalte kann indiziert werden.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL = Ungültige Eingabe|  
|**isoutparam**|Der Prozedurparameter ist ein Ausgabeparameter.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL = Ungültige Eingabe|  
|**IsPrecise**|Diese Spalte ist eine genaue Spalte. Diese Eigenschaft wird nur auf deterministische Spalten angewendet.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL = Ungültige Eingabe Keine deterministische Spalte|  
|**IsRowGuidCol**|Die Spalte weist den Datentyp **uniqueidentifier** auf und wird über die ROWGUIDCOL-Eigenschaft definiert.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL = Ungültige Eingabe|  
|**IsSparse**|Spalte ist eine Sparsespalte. Weitere Informationen finden Sie unter [Verwenden von Spalten mit geringer Dichte](../../relational-databases/tables/use-sparse-columns.md).|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL = Ungültige Eingabe|  
|**IsSystemVerified**|Die Eigenschaften für Determinismus und Genauigkeit der Spalte können von [!INCLUDE[ssDE](../../includes/ssde-md.md)] überprüft werden. Diese Eigenschaft gilt nur für berechnete Spalten und Spalten von Sichten.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL = Ungültige Eingabe|  
|**IsXmlIndexable**|Die XML-Spalte kann in einem XML-Index verwendet werden.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL = Ungültige Eingabe|  
|**Genauigkeit**|Länge des Datentyps der Spalte oder des Parameters.|Die Länge des angegebenen Spaltendatentyps.<br /><br /> -1 = **xml** oder Typen für hohe Werte<br /><br /> NULL = Ungültige Eingabe|  
|**Skalierung**|Dezimalstellen des Datentyps der Spalte oder des Parameters.|Wert der Dezimalstellen<br /><br /> NULL = Ungültige Eingabe|  
|**StatisticalSemantics**|Spalte ist für die semantische Indizierung aktiviert.|1: TRUE<br /><br /> 0: FALSE|  
|**SystemDataAccess**|Die Spalte wird von einer Funktion abgeleitet, die auf Daten in den Systemkatalogen oder virtuellen Tabellen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zugreift. Diese Eigenschaft gilt nur für berechnete Spalten und Spalten von Sichten.|1 = TRUE (gibt schreibgeschützten Zugriff an)<br /><br /> 0: FALSE<br /><br /> NULL = Ungültige Eingabe|  
|**UserDataAccess**|Die Spalte wird von einer Funktion abgeleitet, die auf Daten in Benutzertabellen zugreift, einschließlich Sichten und temporäre Tabellen, die in der lokalen Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gespeichert sind. Diese Eigenschaft gilt nur für berechnete Spalten und Spalten von Sichten.|1 = TRUE (gibt schreibgeschützten Zugriff an)<br /><br /> 0: FALSE<br /><br /> NULL = Ungültige Eingabe|  
|**UsesAnsiTrim**|ANSI_PADDING wurde zum Zeitpunkt der Erstellung der Tabelle auf ON festgelegt. Diese Eigenschaft gilt nur für Spalten oder Parameter vom Typ **char** oder **varchar**.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL = Ungültige Eingabe|  
  
## <a name="return-types"></a>Rückgabetypen
 **int**  
  
## <a name="exceptions"></a>Ausnahmen  
Gibt NULL bei einem Fehler zurück oder wenn ein Aufrufer nicht über Berechtigungen zum Anzeigen des Objekts verfügt.
  
Ein Benutzer kann nur die Metadaten sicherungsfähiger Elemente anzeigen, bei denen der Benutzer entweder der Besitzer ist oder für die dem Benutzer eine Berechtigung erteilt wurde. Dies bedeutet, dass Metadaten ausgebende integrierte Funktionen, z.B. `COLUMNPROPERTY`, möglicherweise NULL zurückgeben, wenn dem Benutzer für das Objekt nicht die korrekte Berechtigung erteilt wurde. Weitere Informationen finden Sie unter [Konfigurieren der Sichtbarkeit von Metadaten](../../relational-databases/security/metadata-visibility-configuration.md).
  
## <a name="remarks"></a>Bemerkungen  
Beim Überprüfen der deterministischen Eigenschaft einer Spalte prüfen Sie zuerst, ob die Spalte eine berechnete Spalte ist. Das Argument **IsDeterministic** gibt für nicht berechnete Spalten NULL zurück. Berechnete Spalten können als Indexspalten angegeben werden.
  
## <a name="examples"></a>Beispiele  
Dieses Beispiel gibt die Länge der Spalte `LastName` zurück.
  
```sql
USE AdventureWorks2012;  
GO  
SELECT COLUMNPROPERTY( OBJECT_ID('Person.Person'),'LastName','PRECISION')AS 'Column Length';  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Column Length
-------------
50
```  
  
## <a name="see-also"></a>Weitere Informationen
[Metadata Functions &#40;Transact-SQL&#41; (Metadatenfunktionen (Transact-SQL))](../../t-sql/functions/metadata-functions-transact-sql.md)  
[TYPEPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/typeproperty-transact-sql.md)
  
  
