---
title: Konstruktorfunktionen (XQuery) | Microsoft-Dokumentation
description: Erfahren Sie mehr über die Konstruktorfunktionen in XQuery, die es Ihnen ermöglichen, Instanzen der integrierten XSD-Typen oder benutzerdefinierten atomaren Typen zu erstellen.
ms.custom: ''
ms.date: 03/09/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- constructor functions [XQuery]
ms.assetid: 98562d0e-d0e0-4f62-b001-90acbac67277
author: rothja
ms.author: jroth
ms.openlocfilehash: 0cc1fe3f4f36191a04973c759474f55a8356abda
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349337"
---
# <a name="constructor-functions-xquery"></a>Konstruktorfunktionen (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Die Konstruktorfunktionen erstellen Instanzen beliebiger integrierter oder benutzerdefinierter atomarer XSD-Typen aus den angegebenen Eingaben.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
TYP($atomicvalue as xdt:anyAtomicType?  
  
) as TYP?  
  
```  
  
## <a name="arguments"></a>Argumente  
 *$strval*  
 Zu konvertierende Zeichenfolge.  
  
 *TYP*  
 Ein beliebiger integrierter XSD-Typ.  
  
## <a name="remarks"></a>Bemerkungen  
 Konstruktoren werden von atomaren XSD-Basistypen sowie abgeleiteten Typen unterstützt. Allerdings werden die Untertypen von **xs: Duration**, einschließlich **xdt: yearMonthDuration und xdt: dayTimeDuration** und **xs: QName**, **xs: NMTOKEN** und **xs: Notation** , nicht unterstützt. Die benutzerdefinierten atomaren Typen, die in den damit verbundenen Schemaauflistungen verfügbar sind, stehen auch hier zur Verfügung, unter der Voraussetzung, dass sie direkt oder indirekt aus den folgenden Typen abgeleitet sind.  
  
#### <a name="supported-base-types"></a>Unterstützte Basistypen  
 Es folgen die unterstützten Basistypen:  
  
-   xs:string  
  
-   xs:boolean  
  
-   xs:decimal  
  
-   xs:float  
  
-   xs:double  
  
-   xs:duration  
  
-   xs:dateTime  
  
-   xs:time  
  
-   xs:date  
  
-   xs:gYearMonth  
  
-   xs:gYear  
  
-   xs:gMonthDay  
  
-   xs:gDay  
  
-   xs:gMonth  
  
-   xs:hexBinary  
  
-   xs:base64Binary  
  
-   xs:anyURI  
  
#### <a name="supported-derived-types"></a>Unterstützte abgeleitete Typen  
 Es folgen die unterstützten abgeleiteten Typen:  
  
-   xs:normalizedString  
  
-   xs:token  
  
-   xs:language  
  
-   xs:Name  
  
-   xs:NCName  
  
-   xs:ID  
  
-   xs:IDREF  
  
-   xs:ENTITY  
  
-   xs:integer  
  
-   xs:nonPositiveInteger  
  
-   xs:negativeInteger  
  
-   xs:long  
  
-   xs:int  
  
-   xs:short  
  
-   xs:byte  
  
-   xs:nonNegativeInteger  
  
-   xs:unsignedLong  
  
-   xs:unsignedInt  
  
-   xs:unsignedShort  
  
-   xs:unsignedByte  
  
-   xs:positiveInteger  
  
 SQL Server unterstützt außerdem die Reduktion konstanter Ausdrücke beim Aufrufen von Konstruktorfunktionen unter folgenden Bedingungen:  
  
-   Wenn das Argument ein Zeichenfolgenliteral ist, wird der Ausdruck zur Kompilierzeit ausgewertet. Wenn der Wert die Typeinschränkungen nicht erfüllt, wird ein statischer Fehler ausgelöst.  
  
-   Wenn das Argument ein Literal eines anderen Typs ist, wird der Ausdruck zur Kompilierzeit ausgewertet. Wenn der Wert die Typeinschränkungen nicht erfüllt, wird die leere Sequenz zurückgegeben.  
  
## <a name="examples"></a>Beispiele  
 Dieses Thema stellt XQuery-Beispiele für XML-Instanzen bereit, die in verschiedenen Spalten vom Typ **XML** in der AdventureWorks-Datenbank gespeichert sind.  
  
### <a name="a-using-the-datetime-xquery-function-to-retrieve-older-product-descriptions"></a>A. Abrufen älterer Produktbeschreibungen mit der dateTime()-Funktion von XQuery  
 In diesem Beispiel wird ein XML-Beispiel Dokument zuerst einer Variablen vom Typ **XML** zugewiesen. Dieses Dokument enthält drei Beispiel <`ProductDescription`> Elemente, die jeweils ein <> untergeordnetes `DateCreated` Element enthalten.  
  
 Die Variable wird dann abgefragt, um nur diejenigen Produktbeschreibungen abzurufen, die vor einem bestimmten Datum erstellt worden sind. Für Vergleichszwecke verwendet die Abfrage die **xs: DateTime ()** -Konstruktorfunktion, um die Datumsangaben zu geben.  
  
```  
declare @x xml  
set @x = '<root>  
<ProductDescription ProductID="1" >  
  <DateCreated DateValue="2000-01-01T00:00:00Z" />  
  <Summary>Some Summary description</Summary>  
</ProductDescription>  
<ProductDescription  ProductID="2" >  
  <DateCreated DateValue="2001-01-01T00:00:00Z" />  
  <Summary>Some Summary description</Summary>  
</ProductDescription>  
<ProductDescription ProductID="3" >  
  <DateCreated DateValue="2002-01-01T00:00:00Z" />  
  <Summary>Some Summary description</Summary>  
</ProductDescription>  
</root>'  
  
select @x.query('  
     for $PD in  /root/ProductDescription  
     where xs:dateTime(data( ($PD/DateCreated/@DateValue)[1] )) < xs:dateTime("2001-01-01T00:00:00Z")  
     return  
        element Product  
       {   
        ( attribute ProductID { data($PD/@ProductID ) },  
        attribute DateCreated { data( ($PD/DateCreated/@DateValue)[1] ) } )  
        }  
 ')  
```  
  
 Beachten Sie hinsichtlich der vorherigen Abfrage Folgendes:  
  
-   Der für... Die WHERE-Schleifen Struktur wird verwendet, um das Element abzurufen, das \<ProductDescription> die in der WHERE-Klausel angegebene Bedingung erfüllt.  
  
-   Die **DateTime ()** -Konstruktorfunktion wird verwendet, um **DateTime** -Typwerte zu erstellen, damit Sie entsprechend verglichen werden können.  
  
-   Anschließend konstruiert die Abfrage die resultierende XML-Ausgabe: Nachdem dabei eine Reihe von Attributen konstruiert wird, werden in der XML-Konstruktion Kommas und Klammern verwendet.  
  
 Dies ist das Ergebnis:  
  
```  
<Product   
   ProductID="1"   
   DateCreated="2000-01-01T00:00:00Z"/>  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [XML-Konstruktion &#40;XQuery-&#41;](../xquery/xml-construction-xquery.md)   
 [XQuery-Funktionen für den xml-Datentyp](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
