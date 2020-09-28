---
description: SQLXML-Schnittstelle
title: SQLXML-Schnittstelle | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 7c67be98-efb5-446c-a0e3-ee67c43cb170
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: bac370362700a4a5f1b500ebd1b01eb8063d5f03
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88396266"
---
# <a name="sqlxml-interface"></a>SQLXML-Schnittstelle

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

JDBC Driver bietet Unterstützung für die JDBC 4.0-API, in der die java.sql.SQLXML-Schnittstelle eingeführt wird. Die SQLXML-Schnittstelle definiert Methoden für die Interaktion mit und die Bearbeitung von XML-Daten. Der Datentyp **SQLXML** ist dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datentypen **xml** zugeordnet.  
  
Die SQLXML-Schnittstelle stellt Methoden für den Zugriff auf den XML-Wert als **String**, **Reader**, **Writer** oder als **Stream** bereit. Der Zugriff auf den XML-Wert ist auch über eine **Quelle** möglich, und er kann als **Ergebnis** festgelegt werden. Diese werden mit XML-Parser-APIs wie DOM (Document Object Model), SAX (Simple API for XML) und StAX (Streaming API for XML) sowie mit XSLT-Transformationen und XPath verwendet.  
  
## <a name="remarks"></a>Bemerkungen  

In der folgenden Tabelle werden die in der SQLXML-Schnittstelle definierten Methoden beschrieben:  
  
|Methodensyntax|Methodenbeschreibung|  
|-------------------|------------------------|  
|[void free()](https://go.microsoft.com/fwlink/?LinkId=131685)|Mit dieser Methode werden das SQLXML-Objekt und die von diesem verwendeten Ressourcen freigegeben.|  
|[InputStream getBinaryStream()](https://go.microsoft.com/fwlink/?LinkId=131754)|Gibt einen Eingabedatenstrom zum Lesen von Daten aus dem SQLXML zurück.|  
|[Reader getCharacterStream()](https://go.microsoft.com/fwlink/?LinkId=131755)|Gibt die **XML**-Daten als java.io.Reader-Objekt oder als Zeichendatenstrom zurück.|  
|[T extends Source T getSource(Class\<T> sourceClass)](https://go.microsoft.com/fwlink/?LinkId=131756)|Gibt eine **Quelle** zum Lesen des **XML**-Werts zurück, der von diesem **SQLXML**-Objekt angegeben wird<br /><br /> **Hinweis:** Die Methode „getSource“ unterstützt die folgenden Quellen: javax.xml.transform.dom.DOMSource, javax.xml.transform.sax.SAXSource, javax.xml.transform.stax.StAXSource und java.io.InputStream.|  
|[String getString()](https://go.microsoft.com/fwlink/?LinkId=131757)|Gibt eine Zeichenfolgendarstellung des **XML**-Werts zurück, der von diesem SQLXML-Objekt angegeben wird.|  
|[OutputStream setBinaryStream()](https://go.microsoft.com/fwlink/?LinkId=131758)|Ruft einen Datenstrom ab, der zum Schreiben des **XML**-Werts verwendet werden kann, der von diesem SQLXML-Objekt angegeben wird.|  
|[Writer setCharacterStream()](https://go.microsoft.com/fwlink/?LinkId=131759)|Gibt einen Datenstrom zurück, der zum Schreiben des **XML**-Werts verwendet werden kann, der von diesem SQLXML-Objekt angegeben wird.|  
|[T extends Result T setResult(Class\<T> resultClass)](https://go.microsoft.com/fwlink/?LinkId=131760)|Gibt ein **Ergebnis** zum Festlegen des **XML**-Werts zurück, der von diesem **SQLXML**-Objekt angegeben wird<br /><br /> **Hinweis:** Die Methode „setResult“ unterstützt die folgenden Quellen: javax.xml.transform.dom.DOMResult, javax.xml.transform.sax.SAXResult, javax.xml.transform.stax.StaxResult und java.io.OutputStream.|  
|[void setString(String value)](https://go.microsoft.com/fwlink/?LinkId=131762)|Legt den von diesem SQLXML-Objekt angegebenen XML-Wert auf die angegebene **String**-Darstellung fest.|  
  
Die Anwendungen können XML-Werte nur einmal aus einem SQLXML-Objekt lesen bzw. in dieses schreiben.  
  
Nach dem Aufrufen der Methode „free()“ wird ein SQLXML-Objekt ungültig und kann weder gelesen noch geschrieben werden. Wenn die Anwendung versucht, für das SQLXML-Objekt eine andere Methode als „free()“ aufzurufen, wird eine Ausnahme ausgelöst.  
  
Nach dem Aufrufen einer der folgenden Abrufmethoden kann das SQLXML-Objekt nicht mehr gelesen oder geschrieben werden: getSource, getCharacterStream, getBinaryStream und getString.  
  
Nach dem Aufrufen einer der folgenden Festlegemethoden kann das SQLXML-Objekt nicht mehr gelesen oder geschrieben werden: setResult, setCharacterStream, setBinaryStream und setString.  
  
## <a name="see-also"></a>Weitere Informationen:  

[Unterstützen von XML-Daten](../../connect/jdbc/supporting-xml-data.md)  
