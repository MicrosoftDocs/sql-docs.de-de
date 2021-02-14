---
title: XQueries mit Hierarchie | Microsoft-Dokumentation
description: Anzeigen von Beispielen für XQueries, die Hierarchien einschließen.
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- hierarchies [XQuery]
- XQuery, hierarchies
ms.assetid: 6953d8b7-bad8-4b64-bf7b-12fa4f10f65c
author: rothja
ms.author: jroth
ms.openlocfilehash: fdebb93459b9519eb6495054908208de610e8222
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352795"
---
# <a name="xqueries-involving-hierarchy"></a>XQuery-Abfragen unter Einbeziehung von Hierarchien
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  Die meisten Spalten vom Typ **XML** in der **AdventureWorks** -Datenbank sind halbstrukturierte Dokumente. Deshalb können die in jeder Zeile gespeicherten Dokumente unterschiedlich aussehen. Die in diesem Thema gezeigten Abfragebeispiele veranschaulichen, wie Informationen aus diesen unterschiedlichen Dokumenten extrahiert werden können.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-from-the-manufacturing-instructions-documents-retrieve-work-center-locations-together-with-the-first-manufacturing-step-at-those-locations"></a>A. Abrufen von Arbeitsplatzstandorten mit dem zugehörigen ersten Fertigungsschritt aus Dokumenten mit Produktionsanweisungen  
 Für das Produktmodell 7 konstruiert die Abfrage XML, das das <`ManuInstr`>-Element mit den Attributen **ProductModelID** und **ProductModelName** und mindestens ein <> untergeordneten `Location` Elementen enthält.  
  
 Jedes <`Location`> Element verfügt über einen eigenen Satz von Attributen und ein <> untergeordnetes `step` Element. Diese <`step`> untergeordnete Element ist der erste Fertigungsschritt am Arbeitsplatz Standort.  
  
```sql
SELECT Instructions.query('  
     declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
   \<ManuInstr  ProdModelID = "{sql:column("Production.ProductModel.ProductModelID") }"   
                ProductModelName = "{ sql:column("Production.ProductModel.Name") }" >  
            {   
              for $wc in //AWMI:root/AWMI:Location  
              return  
                <Location>  
                 {$wc/@* }  
                 <step1> { string( ($wc//AWMI:step)[1] ) } </step1>  
                </Location>  
            }  
          </ManuInstr>  
') as Result  
FROM Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 Beachten Sie hinsichtlich der vorherigen Abfrage Folgendes:  
  
-   Das **Namespace** -Schlüsselwort im [XQuery-Prolog](../xquery/modules-and-prologs-xquery-prolog.md) definiert ein Namespace Präfix. Dieses Präfix wirt später im Body-Teil der Abfrage verwendet.  
  
-   Die Tokens zum Kontextwechsel, {) und (}, bewirken das Wechseln der Abfrage von der XML-Konstruktion zur Abfrageauswertung.  
  
-   **SQL: column ()** wird verwendet, um einen relationalen Wert in den XML-Code einzufügen, der erstellt wird.  
  
-   Beim Erstellen des <`Location`> Elements ruft $WC/@ * alle Attribute für den Arbeitsplatz Speicherort ab.  
  
-   Die **String ()** -Funktion gibt den Zeichen folgen Wert aus dem <`step`>-Element zurück.  
  
 Dies ist ein Teilergebnis:  
  
```xml
<ManuInstr ProdModelID="7" ProductModelName="HL Touring Frame">  
   <Location LocationID="10" SetupHours="0.5"   
            MachineHours="3" LaborHours="2.5" LotSize="100">  
     <step1>Insert aluminum sheet MS-2341 into the T-85A   
             framing tool.</step1>  
   </Location>  
   <Location LocationID="20" SetupHours="0.15"   
            MachineHours="2" LaborHours="1.75" LotSize="1">  
      <step1>Assemble all frame components following   
             blueprint 1299.</step1>  
   </Location>  
...  
</ManuInstr>   
```  
  
### <a name="b-find-all-telephone-numbers-in-the-additionalcontactinfo-column"></a>B. Suchen aller Telefonnummern in der AdditionalContactInfo-Spalte  
 Die folgende Abfrage ruft zusätzliche Telefonnummern für einen bestimmten Kundenkontakt ab, indem die gesamte Hierarchie nach dem <>-Element durchsucht wird `telephoneNumber` . Da das <`telephoneNumber`>-Element an beliebiger Stelle in der Hierarchie angezeigt werden kann, verwendet die Abfrage den Nachfolger-und Self-Operator (//) in der Suche.  
  
```sql
SELECT AdditionalContactInfo.query('  
 declare namespace ci="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactInfo";  
 declare namespace act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes";  
for $ph in /ci:AdditionalContactInfo//act:telephoneNumber  
   return  
      $ph/act:number  
') as x  
FROM  Person.Contact  
WHERE ContactID = 1  
```  
  
 Dies ist das Ergebnis:  
  
```xml
\<act:number   
  xmlns:act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">  
  111-111-1111  
\</act:number>  
\<act:number   
  xmlns:act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">  
  112-111-1111  
\</act:number>  
```  
  
 Um nur die Telefonnummern der obersten Ebene abzurufen, insbesondere die <`telephoneNumber`> untergeordneten Elementen <`AdditionalContactInfo`>, ändert sich der for-Ausdruck in der Abfrage in.  
  
 `for $ph in /ci:AdditionalContactInfo/act:telephoneNumber`.  
  
## <a name="see-also"></a>Weitere Informationen  
 [XQuery-Grundlagen](../xquery/xquery-basics.md)   
 [XML-Konstruktion &#40;XQuery-&#41;](../xquery/xml-construction-xquery.md)   
 [XML-Daten &#40;SQL Server&#41;](../relational-databases/xml/xml-data-sql-server.md)  
  
  
