---
description: Datenstrukturierung – Beispiel
title: Beispiel für Daten Strukturierung | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- data shaping [ADO], about data shaping
ms.assetid: 1bfdcad4-52e1-45bc-ad21-783657ef0a44
author: rothja
ms.author: jroth
ms.openlocfilehash: 0199dfc2165d5ae54b4b59a45ec740a9c399f114
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033280"
---
# <a name="data-shaping-example"></a>Datenstrukturierung – Beispiel
Der folgende Daten Strukturierungs Befehl veranschaulicht, wie ein hierarchisches **Recordset** aus den Tabellen **Customers** und **Orders** in der Datenbank Northwind erstellt wird.  
  
```  
SHAPE {SELECT CustomerID, ContactName FROM Customers}   
APPEND ({SELECT OrderID, OrderDate, CustomerID FROM Orders} AS chapOrders   
RELATE customerID TO customerID)   
```  
  
 Wenn dieser Befehl zum Öffnen eines **Recordset** -Objekts verwendet wird (wie in [Visual Basic Beispiel der Daten Strukturierung](./visual-basic-example-of-data-shaping.md)gezeigt), wird ein Kapitel ("**chaporders**") für jeden Datensatz erstellt, der von der Tabelle " **Customers** " zurückgegeben wird. Dieses Kapitel besteht aus einer Teilmenge des **Recordsets** , das von der **Orders** -Tabelle zurückgegeben wird. Das Kapitel " **chaporders** " enthält alle angeforderten Informationen zu den Aufträgen, die vom jeweiligen Kunden platziert wurden. In diesem Beispiel besteht das Kapitel aus drei Spalten: " **OrderID**", " **OrderDate**" und " **CustomerID**".  
  
 Die ersten beiden Einträge des resultierenden geformten **Recordsets** lauten wie folgt:  
  
|CustomerID|ContactName|OrderID|OrderDate|CustomerID|  
|----------------|-----------------|-------------|---------------|----------------|  
|ALFKI|Maria ander|10643<br /><br /> 10692<br /><br /> 10702<br /><br /> 10835<br /><br /> 10952<br /><br /> 11011|1997-08-25<br /><br /> 1997-10-03<br /><br /> 1997-10-13<br /><br /> 1998-01-15<br /><br /> 1998-03-16<br /><br /> 1998-04-09|ALFKI<br /><br /> ALFKI<br /><br /> ALFKI<br /><br /> ALFKI<br /><br /> ALFKI<br /><br /> ALFKI|  
|ANATR|Ana Trujillo|10308<br /><br /> 10625<br /><br /> 10759<br /><br /> 10926|1996-09-18<br /><br /> 1997-08-08<br /><br /> 1997-11-28<br /><br /> 1998-03-04|ANATR<br /><br /> ANATR<br /><br /> ANATR<br /><br /> ANATR|  
  
 In einem Shape-Befehl wird Append zum Erstellen eines untergeordneten **Recordsets** verwendet, das mit dem übergeordneten **Recordset** verknüpft ist (wie vom anbieterspezifischen Befehl unmittelbar nach dem zuvor erläuterten Shape-Schlüsselwort) von der Related-Klausel zurückgegeben. Das übergeordnete und das untergeordnete Element verfügen in der Regel über mindestens eine gemeinsame Spalte: der Wert der Spalte in einer Zeile des übergeordneten Elements entspricht dem Wert der Spalte in allen Zeilen des untergeordneten Elements.  
  
 Zum Generieren eines übergeordneten **Recordsets** aus einem untergeordneten **Recordset** gibt es eine zweite Möglichkeit, Form Befehle zu verwenden: Die Datensätze im untergeordneten **Recordset** werden gruppiert, in der Regel mithilfe der by-Klausel, und eine Zeile wird dem übergeordneten **Recordset** für jede resultierende Gruppe in der untergeordneten Gruppe hinzugefügt. Wenn die by-Klausel weggelassen wird, bildet das untergeordnete **Recordset** eine einzelne Gruppe, und das übergeordnete **Recordset** enthält genau eine Zeile. Dies ist nützlich für das Berechnen von "Gesamtsumme"-Aggregaten über das gesamte untergeordnete **Recordset**.  
  
 Mit dem Konstrukt Shape Command können Sie auch Programm gesteuert ein geformtes **Recordset** erstellen. Sie können dann Programm gesteuert oder über ein entsprechendes visuelles Steuerelement auf die-Komponenten des **Recordsets** zugreifen. Ein Shape-Befehl wird wie ein beliebiger anderer ADO-Befehls Text ausgegeben. Weitere Informationen finden Sie unter [Shape-Befehle im allgemeinen](./shape-commands-in-general.md).  
  
 Unabhängig davon, auf welche Weise das übergeordnete **Recordset** gebildet wird, enthält es eine Kapitel Spalte, die verwendet wird, um es mit einem untergeordneten **Recordset** zu verknüpfen. Wenn Sie möchten, kann das übergeordnete **Recordset** auch Spalten aufweisen, die Aggregate (Sum, min, Max usw.) für die untergeordneten Zeilen enthalten. Sowohl das übergeordnete als auch das untergeordnete **Recordset** können Spalten aufweisen, die einen Ausdruck in der Zeile im **Recordset** enthalten, sowie Spalten, die neu sind und anfänglich leer sind.  
  
 In diesem Abschnitt wird das folgende Thema fortgesetzt.  
  
-   [Visual Basic-Beispiel der Datenstrukturierung](./visual-basic-example-of-data-shaping.md)