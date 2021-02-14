---
title: XQueries-Verarbeitung von relationalen Daten | Microsoft-Dokumentation
description: 'Erfahren Sie, wie Sie relationale nicht-XML-Daten mithilfe der XQuery-Erweiterungen "SQL: column ()" und "SQL: Variable ()" an XML binden.'
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- relational data [XQuery]
- XQuery, relational data
ms.assetid: 9812b71a-52ec-48a0-92f3-016a93660229
author: rothja
ms.author: jroth
ms.openlocfilehash: cadad1b4db6a0f0a6284ecb4beb6eadba9e59adc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344954"
---
# <a name="xqueries-handling-relational-data"></a>Behandlung relationaler Daten mit XQuery-Abfragen
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Sie geben XQuery für eine Spalte oder eine Variable vom Typ **XML** an, indem Sie eine der [XML-Datentyp Methoden](../t-sql/xml/xml-data-type-methods.md)verwenden. Hierzu zählen **Query ()**, **value ()**, **exist ()** oder **Modify ()**. Die XQuery wird für die XML-Instanz ausgeführt, die in der XML generierenden Abfrage angegeben ist.  
  
 XML, das durch Ausführen einer XQuery-Abfrage erzeugt wird, kann Werte enthalten, die aus anderen Transact-SQL-Variablen oder aus Rowsetspalten abgerufen werden. Um relationale Nicht-XML-Daten an das XML-Ergebnis zu binden, bietet SQL Server die folgenden Pseudofunktionen als XQuery-Erweiterungen:  
  
-   **SQL: column ()** -Funktion  
  
-   **sql:variable()** function  
  
 Sie können diese XQuery-Erweiterungen verwenden, wenn Sie eine XQuery-Methode in der **Query ()** -Methode des **XML** -Datentyps angeben. Folglich kann die **Query ()** -Methode XML-Daten liefern, die Daten aus XML-und nicht-**XML** -Datentypen kombiniert.  
  
 Sie können diese Funktionen auch verwenden, wenn Sie die **XML** -Datentyp Methoden **Modify ()**, **value ()**, **Query ()** und **exist ()** verwenden, um einen relationalen Wert in XML verfügbar zu machen.  
  
 Weitere Informationen finden Sie unter [SQL: column ()-Funktion (XQuery)](../xquery/xquery-extension-functions-sql-column.md) und [SQL: Variable ()-Funktion (XQuery)](../xquery/xquery-extension-functions-sql-variable.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [XML-Daten &#40;SQL Server&#41;](../relational-databases/xml/xml-data-sql-server.md)   
 [XQuery-Sprachreferenz &#40;SQL Server&#41;](../xquery/xquery-language-reference-sql-server.md)   
 [XML-Konstruktion &#40;XQuery-&#41;](../xquery/xml-construction-xquery.md)  
  
  
