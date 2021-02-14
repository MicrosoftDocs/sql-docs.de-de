---
title: Fehlerbehandlung (XQuery) | Microsoft-Dokumentation
description: Erfahren Sie mehr über die Fehlerbehandlung in XQuery, und sehen Sie sich Beispiele zur Behandlung dynamischer Fehler an.
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- static errors
- errors [XQuery]
- XQuery, error handling
- dynamic errors [XQuery]
ms.assetid: 7dee3c11-aea0-4d10-9126-d54db19448f2
author: rothja
ms.author: jroth
ms.openlocfilehash: 61e6b8b53e4519eb88f94f9521434b2e48aac40d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100335862"
---
# <a name="error-handling-xquery"></a>Fehlerbehandlung (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Gemäß der W3C-Spezifikation können Typfehler statisch oder dynamisch ausgelöst werden; die Spezifikation definiert statische, dynamische und Typfehler.  
  
## <a name="compilation-and-error-handling"></a>Kompilierung und Fehlerbehandlung  
 Von syntaktisch fehlerhaften XQuery-Ausdrücken und XML DML-Anweisungen werden Kompilierungsfehler zurückgegeben. In der Kompilierungsphase wird die Richtigkeit des statischen Typs von XQuery-Ausdrücken und von DML-Anweisungen überprüft, und es werden XML-Schemas für Typrückschlüsse für typisiertes XML verwendet. Es werden statische Typfehler ausgelöst, wenn ein Ausdruck aufgrund eines Verstoßes gegen die Typsicherheit zur Laufzeit fehlschlagen könnte. Beispiele für statische Fehler sind das Hinzufügen einer Zeichenfolge zu einer ganzen Zahl und das Abfragen eines nicht vorhandenen Knotens für typisierte Daten.  
  
 In Abweichung vom W3C-Standard werden XQuery-Laufzeitfehler in leere Sequenzen umgewandelt. Diese Sequenzen können je nach Aufrufkontext als leere XML-Daten oder als NULL-Wert an das Abfrageergebnis übergeben werden.  
  
 Die explizite Umwandlung in den richtigen Typ ermöglicht Benutzern das Umgehen von statischen Fehlern, obwohl dabei Laufzeitumwandlungsfehler in leere Sequenzen transformiert werden.  
  
## <a name="static-errors"></a>Statische Fehler  
 Statische Fehler werden mithilfe des [!INCLUDE[tsql](../includes/tsql-md.md)]-Fehlermechanismus zurückgegeben. In [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] werden XQuery-Typfehler statisch zurückgegeben. Weitere Informationen finden Sie unter [XQuery und statische Typisierung](../xquery/xquery-and-static-typing.md).  
  
## <a name="dynamic-errors"></a>Dynamische Fehler  
 In XQuery werden die meisten dynamischen Fehler einer leeren Sequenz ("()") zugeordnet. Dabei gelten die folgenden beiden Ausnahmen: Überlaufbedingungen in XQuery-Aggregatfunktionen und XML-DML-Überprüfungsfehler. Beachten Sie, dass die meisten dynamischen Fehler einer leeren Sequenz zugeordnet werden. Anderenfalls kann eine Abfrageausführung, die XML-Indizes nutzt, unerwartete Fehler auslösen. Um eine effiziente Ausführung zu ermöglichen, ohne dass unerwartete Fehler generiert werden, ordnet deshalb [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] dynamische Fehler () zu.  
  
 Wenn ein dynamischer Fehler in einem Prädikat auftritt, bedeutet das Nichtauslösen des Fehlers, dass die Semantik nicht geändert wird, weil () False zugeordnet wird. In einigen Fällen kann die Rückgabe von () statt eines dynamischen Fehlers jedoch zu unerwarteten Ergebnissen führen. Die folgenden Beispiele veranschaulichen dies.  
  
### <a name="example-using-the-avg-function-with-a-string"></a>Beispiel: Verwenden der avg()-Funktion mit einer Zeichenfolge  
 Im folgenden Beispiel wird die [AVG-Funktion](../xquery/aggregate-functions-avg.md) aufgerufen, um den Durchschnitt der drei Werte zu berechnen. Einer dieser Werte ist eine Zeichenfolge. Da die XML-Instanz in diesem Fall nicht typisiert ist, sind alle darin enthaltenen Daten nicht typisierte atomare Typen. Die **AVG ()** -Funktion wandelt diese Werte zuerst in **xs: Double** um, bevor der Durchschnitt berechnet wird. Allerdings kann der Wert, `"Hello"` , nicht in **xs: Double** umgewandelt werden und führt zu einem dynamischen Fehler. In diesem Fall verursacht die Umwandlung von `"Hello"` in **xs: Double** eine leere Sequenz, anstatt einen dynamischen Fehler zurückzugeben. Die **AVG ()** -Funktion ignoriert diesen Wert, berechnet den Durchschnitt der beiden anderen Werte und gibt 150 zurück.  
  
```  
DECLARE @x xml  
SET @x=N'<root xmlns:myNS="test">  
 <a>100</a>  
 <b>200</b>  
 <c>Hello</c>  
</root>'  
SELECT @x.query('avg(//*)')  
```  
  
### <a name="example-using-the-not-function"></a>Beispiel: Verwenden der not-Funktion  
 Wenn Sie die [not-Funktion](../xquery/functions-on-boolean-values-not-function.md) in einem Prädikat verwenden, z. b `/SomeNode[not(Expression)]` ., und der Ausdruck einen dynamischen Fehler verursacht, wird anstelle eines Fehlers eine leere Sequenz zurückgegeben. Wenn Sie **nicht ()** auf die leere Sequenz anwenden, wird anstelle eines Fehlers true zurückgegeben.  
  
### <a name="example-casting-a-string"></a>Beispiel: Umwandeln einer Zeichenfolge  
 Im folgenden Beispiel wird die Literalzeichenfolge "NaN" zuerst in xs:string und dann in xs:double umgewandelt. Das Ergebnis ist ein leeres Rowset. Die Zeichenfolge "NaN" kann nicht erfolgreich in xs:double umgewandelt werden; dies wird jedoch erst zur Laufzeit bestimmt, weil die Zeichenfolge zuerst in xs:string umgewandelt wird.  
  
```  
DECLARE @x XML  
SET @x = ''  
SELECT @x.query(' xs:double(xs:string("NaN")) ')  
GO  
```  
  
 In diesem Beispiel tritt jedoch ein statischer Typfehler auf.  
  
```  
DECLARE @x XML  
SET @x = ''  
SELECT @x.query(' xs:double("NaN") ')  
GO  
```  
  
#### <a name="implementation-limitations"></a>Implementierungseinschränkungen  
 Die **FN: Error ()** -Funktion wird nicht unterstützt.  
  
## <a name="see-also"></a>Weitere Informationen  
 [XQuery-Sprachreferenz &#40;SQL Server&#41;](../xquery/xquery-language-reference-sql-server.md)   
 [XQuery-Grundlagen](../xquery/xquery-basics.md)  
  
  
