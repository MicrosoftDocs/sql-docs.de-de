---
description: FREETEXT (Transact-SQL)
title: FREETEXT (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 10/23/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- FREETEXT
- FREETEXT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- full-text search [SQL Server], meaning matches
- meaning matches [full-text search]
- FREETEXT predicate (Transact-SQL)
- words in predicate [full-text search]
- column searches [full-text search]
ms.assetid: 2f199d3c-440e-4bcf-bdb5-82bb3994005d
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 927f11ff77a50a3a7b7a04983fa197510e88232f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207747"
---
# <a name="freetext-transact-sql"></a>FREETEXT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Dies ist ein Prädikat, das in der [!INCLUDE[tsql](../../includes/tsql-md.md)]-[WHERE-Klausel](../../t-sql/queries/where-transact-sql.md) einer [!INCLUDE[tsql](../../includes/tsql-md.md)]-SELECT-Anweisung verwendet wird, um eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Volltextsuche für volltextindizierte Spalten mit zeichenbasierten Datentypen durchzuführen. Dieses Prädikat sucht nach Werten, die der Bedeutung der Suchbedingung entsprechen und nicht genau mit dem Wortlaut der Suchbedingung übereinstimmen. Bei Verwendung von FREETEXT führt die Volltextabfrage-Engine intern die folgenden Aktionen für *freetext_string* aus, weist jedem Begriff eine Gewichtung zu und sucht dann nach Übereinstimmungen:  
  
-   Trennt die Zeichenfolge in einzelne Wörter auf der Basis von Wortgrenzen (Wörtertrennung).  
  
-   Generiert Flexionen der Wörter (Wortstammerkennung).  
  
-   Legt eine Liste mit Erweiterungen oder Ersetzungen für die Begriffe auf der Basis von Übereinstimmungen im Thesaurus fest.  
  
> [!NOTE]  
>  Informationen zu den Formen der Volltextsuche, die von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unterstützt werden, finden Sie unter [Abfragen mit Volltextsuche](../../relational-databases/search/query-with-full-text-search.md).  
  
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] bis zur [aktuellen Version](/troubleshoot/sql/general/determine-version-edition-update-level)).
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
FREETEXT ( { column_name | (column_list) | * }   
          , 'freetext_string' [ , LANGUAGE language_term ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *column_name*  
 Der Name einer oder mehrerer volltextindizierten Spalten der in der FROM-Klausel angegebenen Tabelle. Die Spalten können vom Typ **char**, **varchar**, **nchar**, **nvarchar**, **text**, **ntext**, **image**, **xml**, **varbinary** oder **varbinary(max)** sein.  
  
 *column_list*  
 Gibt an, dass verschiedene, durch Trennzeichen getrennte Spalten angegeben werden können. *column_list* muss in Klammern stehen. Sofern nicht *language_term* angegeben ist, muss die Sprache aller Spalten von *column_list* identisch sein.  
  
 \*  
 Gibt an, dass alle für die Volltextsuche registrierten Spalten nach dem angegebenen *freetext_string*-Wert durchsucht werden. Wenn mehr als eine Tabelle in der FROM-Klausel vorhanden ist, muss \* durch den Tabellennamen gekennzeichnet werden. Sofern *language_term* nicht angegeben ist, muss die Sprache aller Spalten in der Tabelle identisch sein.  
  
 *freetext_string*  
 Der Text, nach dem in *column_name* gesucht werden soll. Es kann hierbei ein beliebiger Text aus Wörtern, Ausdrücken und Sätzen eingegeben werden. Übereinstimmungen werden dann generiert, wenn ein Begriff oder Formen eines Begriffs im Volltextindex gefunden werden.  
  
 Im Gegensatz zur CONTAINS- und CONTAINSTABLE-Suchbedingung, in der AND ein Schlüsselwort ist, wird das Wort „and“ in *freetext_string* als Füllwort oder als [Stoppwort](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md) eingestuft und verworfen.  
  
 Die Verwendung von WEIGHT, FORMSOF, Platzhaltern, NEAR und anderer Syntax ist nicht zulässig. Für *freetext_string* wird eine Worttrennung durchgeführt, und die Wörter werden auf den Stamm zurückgeführt und durchlaufen den Thesaurus.  
  
 *freetext_string* ist **nvarchar**. Wird ein anderer Zeichendatentyp als Eingabe verwendet, findet eine implizite Konvertierung statt. Die großen Zeichenfolgendatentypen nvarchar(max) und varchar(max) können nicht verwendet werden. Im folgenden Beispiel verursacht die `@SearchWord`-Variable, die als `varchar(30)` definiert ist, eine implizite Konvertierung im `FREETEXT`-Prädikat.  
  
```sql
USE AdventureWorks2012;  
GO  
DECLARE @SearchWord VARCHAR(30)  
SET @SearchWord ='performance'  
SELECT Description   
FROM Production.ProductDescription   
WHERE FREETEXT(Description, @SearchWord);  
  
```  
  
 Da die „Parameterermittlung“ zusammen mit der Konvertierung nicht funktionsfähig ist, sollten Sie aus Leistungsgründen **nvarchar** verwenden. Deklarieren Sie im Beispiel `@SearchWord` als `nvarchar(30)`.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @SearchWord NVARCHAR(30)  
SET @SearchWord = N'performance'  
SELECT Description   
FROM Production.ProductDescription   
WHERE FREETEXT(Description, @SearchWord);  
  
```  
  
 In Fällen, in denen ein nicht optimaler Plan generiert wird, können Sie auch den OPTIMIZE FOR-Abfragehinweis verwenden.  
  
 LANGUAGE *language_term*  
 Die Sprache, deren Ressourcen für die Wörtertrennung, die Wortstammerkennung und den Thesaurus sowie die Entfernung von Stoppwörtern in der Abfrage verwendet werden. Dieser Parameter ist optional und kann als Zeichenfolge, ganze Zahl oder Hexadezimalwert entsprechend dem Gebietsschemabezeichner (Locale Identifier – LCID) einer Sprache angegeben werden. Wird *language_term* angegeben, wird die entsprechende Sprache auf alle Elemente der Suchbedingung angewendet. Wird kein Wert angegeben, wird die Volltextsprache der Spalte verwendet.  
  
 Wenn Dokumente anderer Sprachen zusammen als BLOBs (Binary Large Objects) in einer einzelnen Spalte gespeichert werden, legt der Gebietsschemabezeichner (LCID) eines bestimmten Dokuments die zur Indizierung seines Inhalts zu verwendende Sprache fest. Beim Abfragen einer solchen Spalte kann die Angabe von LANGUAGE *language_term* die Wahrscheinlichkeit einer hohen Übereinstimmung steigern.  
  
 In Form einer Zeichenfolge entspricht *language_term* dem Wert der **alias**-Spalte in der [sys.syslanguages &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql.md)-Kompatibilitätsansicht.  Die Zeichenfolge muss in einfache Anführungszeichen gesetzt werden, z.B. '*language_term*'. In Form einer ganzen Zahl ist *language_term* der eigentliche Gebietsschemabezeichner, der die Sprache identifiziert. In Form eines Hexadezimalwerts ist *language_term* gleich 0x, gefolgt vom Hexadezimalwert des Gebietsschemabezeichners. Der Hexadezimalwert darf acht Ziffern nicht überschreiten, einschließlich führender Nullen.  
  
 Wird der Wert im Format Doppelbyte-Zeichensatz (Double-Byte Character Set, DBCS) angegeben, wird er von [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in Unicode konvertiert.  
  
 Ist die angegebene Sprache ungültig oder sind keine Ressourcen installiert, die dieser Sprache entsprechen, gibt [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] einen Fehler zurück. Geben Sie 0x0 als *language_term* an, um neutrale Sprachressourcen zu verwenden.  
  
## <a name="general-remarks"></a>Allgemeine Hinweise  
 Volltextprädikate und -funktionen gelten für eine einzelne Tabelle, die im FROM-Prädikat enthalten ist. Um eine Suche in mehreren Tabellen auszuführen, können Sie eine verknüpfte Tabelle in der FROM-Klausel verwenden, um in einem Resultset zu suchen, das aus mindestens zwei Tabellen erstellt wird.  
  
Volltextabfragen mit FREETEXT sind nicht so genau wie Volltextabfragen mit CONTAINS. Die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Volltextsuch-Engine identifiziert wichtige Wörter und Ausdrücke. Reservierten Schlüsselwörtern und Platzhalterzeichen, die normalerweise eine Bedeutung besitzen, wenn sie im \<contains_search_condition>-Parameter des CONTAINS-Prädikats angegeben werden, wird keine spezielle Bedeutung zugewiesen.
  
 Volltextprädikate sind in der [OUTPUT-Klausel](../../t-sql/queries/output-clause-transact-sql.md) nicht zulässig, wenn der Kompatibilitätsgrad der Datenbank auf 100 festgelegt ist.  
  
> [!NOTE]  
>  Die FREETEXTTABLE-Funktion eignet sich für dieselben Übereinstimmungen wie das FREETEXT-Prädikat. Auf diese Funktion kann in der [FROM-Klausel](../../t-sql/queries/from-transact-sql.md) einer SELECT-Anweisung wie auf einen regulären Tabellennamen verwiesen werden. Weitere Informationen finden Sie unter [FREETEXTTABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/freetexttable-transact-sql.md).  
  
## <a name="querying-remote-servers"></a>Abfragen von Remoteservern  
 Sie können einen vierteiligen Namen im [CONTAINS](../../t-sql/queries/contains-transact-sql.md)- oder FREETEXT-Prädikat zum Abfragen von volltextindizierten Spalten der Zieltabellen auf einem Verbindungsserver verwenden. Erstellen Sie zum Vorbereiten eines Remoteservers für den Empfang von Volltextabfragen einen Volltextindex für die Zieltabellen und -spalten auf dem Remoteserver, und fügen Sie anschließend den Remoteserver als Verbindungsserver hinzu.  
  
## <a name="comparison-of-like-to-full-text-search"></a>Vergleich zwischen LIKE und der Volltextsuche  
 Im Gegensatz zur Volltextsuche verarbeitet das [LIKE](../../t-sql/language-elements/like-transact-sql.md)[!INCLUDE[tsql](../../includes/tsql-md.md)] -Prädikat ausschließlich Zeichenmuster. Darüber hinaus können Sie mit dem LIKE-Prädikat keine formatierten Binärdaten abfragen. Eine LIKE-Abfrage in umfangreichen unstrukturierten Textdaten ist sehr viel langsamer als eine entsprechende Volltextabfrage in denselben Daten. Eine LIKE-Abfrage für Millionen von Zeilen von Textdaten kann Minuten in Anspruch nehmen; eine Volltextabfrage kann dagegen in Sekunden oder weniger für dieselben Daten ein Ergebnis liefern, je nach Anzahl der zurückgegebenen Zeilen.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-using-freetext-to-search-for-words-containing-specified-character-values"></a>A. Verwenden von FREETEXT, um nach Wörtern zu suchen, die bestimmte Zeichenwerte enthalten  
 Im folgenden Beispiel wird nach allen Dokumenten gesucht, die Wörter im Zusammenhang mit wichtigen Sicherheitskomponenten enthalten.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT Title  
FROM Production.Document  
WHERE FREETEXT (Document, 'vital safety components' );  
GO  
```  
  
### <a name="b-using-freetext-with-variables"></a>B. Verwenden von FREETEXT mit Variablen  
 Im folgenden Beispiel wird kein bestimmter Suchbegriff, sondern eine Variable verwendet.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @SearchWord NVARCHAR(30);  
SET @SearchWord = N'high-performance';  
SELECT Description   
FROM Production.ProductDescription   
WHERE FREETEXT(Description, @SearchWord);  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Erste Schritte mit der Volltextsuche](../../relational-databases/search/get-started-with-full-text-search.md)   
 [Erstellen und Verwalten von Volltextkatalogen](../../relational-databases/search/create-and-manage-full-text-catalogs.md)   
 [CREATE FULLTEXT CATALOG &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-catalog-transact-sql.md)   
 [CREATE FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-index-transact-sql.md)   
 [Erstellen und Verwalten von Volltextindizes](../../relational-databases/search/create-and-manage-full-text-indexes.md)   
 [Abfragen mit Volltextsuche](../../relational-databases/search/query-with-full-text-search.md)   
 [Erstellen von Volltextsuchabfragen &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/create-full-text-search-queries-visual-database-tools.md)   
 [CONTAINS &#40;Transact-SQL&#41;](../../t-sql/queries/contains-transact-sql.md)   
 [CONTAINSTABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/containstable-transact-sql.md)   
 [Datentypen &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [FREETEXTTABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/freetexttable-transact-sql.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)  
  
