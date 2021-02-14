---
title: XQuery-Sprachreferenz (SQL Server) | Microsoft-Dokumentation
description: Erfahren Sie mehr über die XQuery-Sprache für SQL Server und zeigen Sie eine komplette Sprachreferenz an.
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
helpviewer_keywords:
- XQuery
- XQuery, about XQuery
- xml data type [SQL Server], XQuery
- XML [SQL Server], XQuery
- queries [XML in SQL Server], XQuery
ms.assetid: 8a69344f-2990-4357-8160-cb26aac95b91
author: rothja
ms.author: jroth
ms.openlocfilehash: 6fec2edcca6a5a73c8fcdb9bb0629b7b5ed2ff24
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340459"
---
# <a name="xquery-language-reference-sql-server"></a>XQuery-Sprachreferenz (SQL Server)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[tsql](../includes/tsql-md.md)] unterstützt eine Teilmenge der XQuery-Sprache, die zum Abfragen des **XML** -Datentyps verwendet wird. Diese XQuery-Implementierung orientiert sich am Arbeitsentwurf für XQuery (Juli 2004). Diese Sprache wird zurzeit vom W3C (World Wide Web Consortium) unter Mitwirkung aller großen Datenbankhersteller und Microsoft entwickelt. Da die W3C-Spezifikationen möglicherweise überarbeitet werden, bevor eine W3C-Empfehlung ausgesprochen wird, kann sich diese Implementierung von der endgültigen Empfehlung unterscheiden. Dieses Thema beschreibt die Semantik und Syntax der Teilmenge von XQuery, die in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] unterstützt wird.  
  
 Weitere Informationen finden Sie in der [W3C XQuery 1,0-Sprachspezifikation](https://go.microsoft.com/fwlink/?LinkId=48846).  
  
 XQuery ist eine Sprache, die strukturierte oder halbstrukturierte XML-Daten abfragen kann. Mit der Unterstützung des **XML** -Datentyps, die in bereitgestellt wird [!INCLUDE[ssDE](../includes/ssde-md.md)] , können Dokumente in einer Datenbank gespeichert und dann mithilfe von XQuery abgefragt werden.  
  
 XQuery basiert auf der vorhandenen XPath-Abfragesprache. Dieser wurde Unterstützung für bessere Iteration, bessere Sortierergebnisse und eine Funktion zum Erstellen des erforderlichen XML hinzugefügt. XQuery wird auf der Grundlage des XQuery-Datenmodells ausgeführt. Dabei handelt es sich um eine Abstraktion von XML-Dokumenten und die XQuery-Ergebnisse, die typisiert oder nicht typisiert sein können. Die Typinformationen basieren auf den von der W3C XML-Schemasprache bereitgestellten Typen. Wenn keine Typisierungsinformationen verfügbar sind, behandelt XQuery die Daten so, als wären sie nicht typisiert. XPath, Version 1.0, verarbeitet XML auf ähnliche Weise.  
  
 Zum Abfragen einer XML-Instanz, die in einer Variablen oder Spalte vom Typ **XML** gespeichert ist, verwenden Sie die Methoden des XML- [Datentyps](../t-sql/xml/xml-data-type-methods.md). Beispielsweise können Sie eine Variable vom Typ **XML** deklarieren und diese mithilfe der **Query ()** -Methode des **XML** -Datentyps Abfragen.  
  
```sql
DECLARE @x xml  
SET @x = '<ROOT><a>111</a></ROOT>'  
SELECT @x.query('/ROOT/a')  
```  
  
 Im folgenden Beispiel wird die Abfrage für die Instructions-Spalte vom Typ **XML** in der ProductModel-Tabelle in der AdventureWorks-Datenbank angegeben.  
  
```sql
SELECT Instructions.query('declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";           
    /AWMI:root/AWMI:Location[@LocationID=10]  
') as Result   
FROM  Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 Die XQuery enthält die Namespace Deklaration, `declare namespace``AWMI=...` und den Abfrage Ausdruck `/AWMI:root/AWMI:Location[@LocationID=10]` .  
  
 Beachten Sie, dass XQuery für die Instructions-Spalte vom Typ **XML** angegeben wird. Die [Query ()-Methode](../t-sql/xml/query-method-xml-data-type.md) des XML-Datentyps wird zum Angeben der XQuery verwendet.  
  
 Die folgende Tabelle enthält die verwandten Themen, die das Verständnis der Implementierung von XQuery in [!INCLUDE[ssDE](../includes/ssde-md.md)] vertiefen sollen.  
  
|Thema|Beschreibung|  
|-----------|-----------------|  
|[XML-Daten &#40;SQL Server&#41;](../relational-databases/xml/xml-data-sql-server.md)|Erläutert die Unterstützung für den **XML**-Datentyp in [!INCLUDE[ssDE](../includes/ssde-md.md)] und die Methoden, die Sie für diesen Datentyp verwenden können. Der **XML** -Datentyp bildet das Eingabe-XQuery-Datenmodell, für das die XQuery-Ausdrücke ausgeführt werden.|  
|[XML-Schemaauflistungen &#40;SQL Server&#41;](../relational-databases/xml/xml-schema-collections-sql-server.md)|Beschreibt, wie die in einer Datenbank gespeicherten XML-Instanzen typisiert werden können. Dies bedeutet, dass Sie der Spalte vom Typ **XML** eine XML-Schema Auflistung zuordnen können. Alle in der Spalte gespeicherten Instanzen werden in der Auflistung anhand des Schemas überprüft und typisiert und stellen die Typinformationen für XQuery bereit.|  
|||  
  
> [!NOTE]  
>  Die Organisation dieses Abschnitts basiert auf der Arbeitsentwurfspezifikation für XQuery des World Wide Web Consortium (W3C). Einige der in diesem Abschnitt bereitgestellten Diagramme stammen aus dieser Spezifikation. In diesem Abschnitt wird die Microsoft XQuery-Implementierung mit der W3C-Spezifikation verglichen, es werden die Unterschiede zwischen XQuery und W3C beschrieben und die nicht unterstützten W3C-Funktionen genannt. Die W3C-Spezifikation ist unter verfügbar [http://www.w3.org/TR/2004/WD-xquery-20040723](https://go.microsoft.com/fwlink/?LinkId=48846) .  
  
## <a name="in-this-section"></a>In diesem Abschnitt  
  
|Thema|Beschreibung|  
|-----------|-----------------|  
|[XQuery-Grundlagen](../xquery/xquery-basics.md)|Stellt eine grundlegende Übersicht über die XQuery-Konzepte und die Auswertung von Ausdrücken (statischer und dynamischer Kontext), die Atomarmachung, effektive boolesche Werte, das XQuery-Typsystem, Sequenztypzuordnung und Fehlerbehandlung zur Verfügung.|  
|[XQuery-Ausdrücke](../xquery/xquery-expressions.md)|Beschreibt primäre XQuery-Ausdrücke, path-Ausdrücke, sequence-Ausdrücke, arithmetische Vergleiche und logische Ausdrücke, die XQuery-Erstellung, FLWOR-Ausdrücke, bedingte und quantifizierte Ausdrücke sowie verschiedene Ausdrücke für Sequenztypen.|  
|[Module und Prologs &#40;XQuery-&#41;](../xquery/modules-and-prologs-xquery.md)|Beschreibt den XQuery-Prolog.|  
|[XQuery-Funktionen für den xml-Datentyp](../xquery/xquery-functions-against-the-xml-data-type.md)|Beschreibt eine Liste der unterstützten XQuery-Funktionen.|  
|[XQuery-Operatoren für den xml-Datentyp](../xquery/xquery-operators-against-the-xml-data-type.md)|Beschreibt unterstützte XQuery-Operatoren.|  
|[Zusätzliches Beispiel für XQuery-Abfragen für den XML-Datentyp](../xquery/additional-sample-xqueries-against-the-xml-data-type.md)|Stellt zusätzliche XQuery-Beispiele bereit.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [XML-Daten &#40;SQL Server&#41;](../relational-databases/xml/xml-data-sql-server.md)   
 [XML-Schemaauflistungen &#40;SQL Server&#41;](../relational-databases/xml/xml-schema-collections-sql-server.md)   
 [Beispiele für den Massenimport und -export von XML-Dokumenten &#40;SQL Server&#41;](../relational-databases/import-export/examples-of-bulk-import-and-export-of-xml-documents-sql-server.md)  
  
  
