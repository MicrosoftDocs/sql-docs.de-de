---
title: String-length-Funktion (XQuery) | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die XQuery-Funktion string-length () verwenden.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- string-length function
- fn:string-length function
ms.assetid: 7cd69c8b-cf2c-478c-b9a3-e0e14e1aa8aa
author: rothja
ms.author: jroth
ms.openlocfilehash: 588e3a4dc19d502da07d23971aea7b987e49a8ea
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344996"
---
# <a name="functions-on-string-values---string-length"></a>Funktionen für Zeichenfolgenwerte – string-length
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Gibt die Länge der Zeichenfolge in Zeichen zurück.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
fn:string-length() as xs:integer  
fn:string-length($arg as xs:string?) as xs:integer  
```  
  
## <a name="arguments"></a>Argumente  
 *$arg*  
 Quellzeichenfolge, deren Länge berechnet werden soll.  
  
## <a name="remarks"></a>Bemerkungen  
 Wenn der Wert *$arg* eine leere Sequenz ist, wird der **xs: Integer** -Wert 0 zurückgegeben.  
  
 Das Verhalten von Ersatzpaaren in XQuery-Funktionen hängt vom Kompatibilitätsgrad der Datenbank ab. Beim Kompatibilitätsgrad 110 oder höher wird jedes Ersatzpaar als einzelnes Zeichen gezählt. Bei niedrigeren Kompatibilitätsgraden werden Ersatzpaare als zwei Zeichen betrachtet. Weitere Informationen finden Sie unter [ALTER DATABASE-Kompatibilitäts Grad &#40;Transact-SQL-&#41;](../t-sql/statements/alter-database-transact-sql-compatibility-level.md) und [Sortierung und Unicode-Unterstützung](../relational-databases/collations/collation-and-unicode-support.md).  
  
 Wenn der Wert ein 4-Byte-Unicode-Zeichen enthält, das durch zwei Ersatzzeichen dargestellt wird, zählt [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] die Ersatzzeichen einzeln.  
  
 Die **Zeichen folgen Länge ()** ohne Parameter kann nur innerhalb eines Prädikats verwendet werden. Die folgende Abfrage gibt beispielsweise das <`ROOT`>-Element zurück:  
  
```  
DECLARE @x xml;  
SET @x='<ROOT>Hello</ROOT>';  
SELECT @x.query('/ROOT[string-length()=5]');  
```  
  
## <a name="supplementary-characters-surrogate-pairs"></a>Ergänzende Zeichen (Ersatzpaare)  
 Das Verhalten von Ersatzzeichenpaaren in XQuery-Funktionen hängt vom Kompatibilitätsgrad der Datenbank ab und in einigen Fällen vom Standardnamespace-URI für Funktionen. Weitere Informationen finden Sie im Abschnitt "XQuery-Funktionen sind Ersatz Zeichen Unterstützung" im Thema " [Breaking Changes to Datenbank-Engine Features in SQL Server 2016](../database-engine/breaking-changes-to-database-engine-features-in-sql-server-2016.md). Siehe auch [ALTER DATABASE-Kompatibilitäts Grad &#40;Transact-SQL-&#41;](../t-sql/statements/alter-database-transact-sql-compatibility-level.md) und [Sortierung und Unicode-Unterstützung](../relational-databases/collations/collation-and-unicode-support.md).  
  
## <a name="examples"></a>Beispiele  
 Dieses Thema stellt XQuery-Beispiele für XML-Instanzen bereit, die in verschiedenen Spalten vom Typ **XML** in der AdventureWorks-Datenbank gespeichert sind.  
  
### <a name="a-using-the-string-length-xquery-function-to-retrieve-products-with-long-summary-descriptions"></a>A. Verwenden der string-length()-Funktion von XQuery zum Abrufen von Produkten mit langen Zusammenfassungsbeschreibungen  
 Für Produkte, deren Zusammenfassungs Beschreibung mehr als 50 Zeichen umfasst, ruft die folgende Abfrage die Produkt-ID, die Länge der Zusammenfassungs Beschreibung und die Zusammenfassung selbst, das <`Summary`>-Element ab.  
  
```  
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' as pd)  
SELECT CatalogDescription.query('  
      <Prod ProductID= "{ /pd:ProductDescription[1]/@ProductModelID }" >  
       <LongSummary SummaryLength =   
           "{string-length(string( (/pd:ProductDescription/pd:Summary)[1] )) }" >  
           { string( (/pd:ProductDescription/pd:Summary)[1] ) }  
       </LongSummary>  
      </Prod>  
 ') as Result  
FROM Production.ProductModel  
WHERE CatalogDescription.value('string-length( string( (/pd:ProductDescription/pd:Summary)[1]))', 'decimal') > 200;  
```  
  
 Beachten Sie hinsichtlich der vorherigen Abfrage Folgendes:  
  
-   Die Bedingung in der WHERE-Klausel ruft nur die Zeilen ab, für die die im XML-Dokument gespeicherte Zusammenfassung länger als 200 Zeichen ist. Es verwendet die [value ()-Methode (XML-Datentyp)](../t-sql/xml/value-method-xml-data-type.md).  
  
-   Die SELECT-Klausel erstellt nur das von Ihnen gewünschte XML. Er verwendet die [Query ()-Methode (XML-Datentyp)](../t-sql/xml/query-method-xml-data-type.md) , um den XML-Code zu erstellen und den erforderlichen XQuery-Ausdruck anzugeben, um Daten aus dem XML-Dokument abzurufen.  
  
 Dies ist ein Teilergebnis:  
  
```  
Result  
-------------------  
<Prod ProductID="19">  
      <LongSummary SummaryLength="214">Our top-of-the-line competition   
             mountain bike. Performance-enhancing options include the  
             innovative HL Frame, super-smooth front suspension, and   
             traction for all terrain.  
      </LongSummary>  
</Prod>  
...  
```  
  
### <a name="b-using-the-string-length-xquery-function-to-retrieve-products-whose-warranty-descriptions-are-short"></a>B. Verwenden der string-length()-Funktion von XQuery zum Abrufen von Produkten, deren Garantiebeschreibungen kurz sind  
 Für Produkte, deren Garantie Beschreibungen weniger als 20 Zeichen lang sind, ruft die folgende Abfrage XML ab, das die Produkt-ID, die Länge, die Garantie Beschreibung und das <`Warranty`> Element selbst enthält.  
  
 Die Garantie ist eine der Produktfunktionen. `Warranty`Nach dem <> Element folgt ein optionales <> untergeordnetes Element `Features` .  
  
```  
WITH XMLNAMESPACES (  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS pd,  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain' AS wm)  
  
SELECT CatalogDescription.query('  
      for   $ProdDesc in /pd:ProductDescription,  
            $pf in $ProdDesc/pd:Features/wm:Warranty  
      where string-length( string(($pf/wm:Description)[1]) ) < 20  
      return   
          <Prod >  
             { $ProdDesc/@ProductModelID }  
             <ShortFeature FeatureDescLength =   
                             "{string-length( string(($pf/wm:Description)[1]) ) }" >  
                 { $pf }  
             </ShortFeature>  
          </Prod>  
     ') as Result  
FROM Production.ProductModel  
WHERE CatalogDescription.exist('/pd:ProductDescription')=1;  
```  
  
 Beachten Sie hinsichtlich der vorherigen Abfrage Folgendes:  
  
-   **PD** und **WM** sind die Namespace Präfixe, die in dieser Abfrage verwendet werden. Sie geben den gleichen Namespace an, der in dem Dokument verwendet wird, das abgefragt wird.  
  
-   Die XQuery gibt eine geschachtelte FOR-Schleife an. Die äußere for-Schleife ist erforderlich, da Sie die **ProductModelID** -Attribute des <>- `ProductDescription` Elements abrufen möchten. Die innere FOR-Schleife ist erforderlich, weil Sie nur die Produkte abrufen möchten, die Garantiefunktionsbeschreibungen besitzen, die kürzer als 20 Zeichen sind.  
  
 Dies ist das Teilergebnis:  
  
```  
Result  
-------------------------  
<Prod ProductModelID="19">  
  <ShortFeature FeatureDescLength="15">  
    <wm:Warranty   
       xmlns:wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain">  
      <wm:WarrantyPeriod>3 years</wm:WarrantyPeriod>  
      <wm:Description>parts and labor</wm:Description>  
    </wm:Warranty>  
   </ShortFeature>  
</Prod>  
...  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [XQuery-Funktionen für den xml-Datentyp](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
