---
description: setNCharacterStream-Methode (java.lang.String, java.io.Reader, long)
title: setNCharacterStream-Methode für Reader-Objekt – long | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: af9a1ba8-7980-43fa-88e5-14f6cc5e897c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3020ce625f4b1cc2d9bf13094399144883755487
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178862"
---
# <a name="setncharacterstream-method-javalangstring-javaioreader-long"></a>setNCharacterStream-Methode (java.lang.String, java.io.Reader, long)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Legt den angegebenen Parameter auf das angegebene Readerobjekt fest, dessen Länge der angegebenen Zeichenanzahl entspricht.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
public final void setNCharacterStream(java.lang.String parameterName,  
                     java.io.Reader value,  
                     long length)  
```  
  
#### <a name="parameters"></a>Parameter  
 *parameterName*  
  
 Eine **Zeichenfolge** zum Angeben des Parameternamens.  
  
 *value*  
  
 Ein Reader-Objekt  
  
 *length*  
  
 Ein Wert vom Typ **long** zum Angeben der Anzahl von Zeichen im Datenstrom.  
  
## <a name="exceptions"></a>Ausnahmen  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Bemerkungen  
 Diese setNCharacterStream-Methode wird von der setNCharacterStream-Methode in der java.sql.CallableStatement-Schnittstelle angegeben.  
  
 Diese Methode sollte für die Datentypen **NCHAR**, **NVARCHAR**, **NTEXT** und **XML** verwendet werden.  
  
 Entspricht die Länge des Streams nicht der Angabe im *length*-Parameter, wird vom JDBC-Treiber beim Aktualisieren oder Einfügen der Zeile eine Ausnahme ausgelöst.  
  
 Ist die Länge des Streams nicht bekannt, kann der *length*-Parameter auf „–1“ festgelegt werden, um anzugeben, dass der Stream unabhängig von seiner Länge akzeptiert werden soll. Bei „sqljdbc4.jar“ empfiehlt sich die Verwendung der JDBC 4.0-Methode [setNCharacterStream-Methode &#40;java.lang.String, java.io.Reader&#41;](../../../connect/jdbc/reference/setncharacterstream-method-java-lang-string-java-io-reader.md), wenn von der Anwendung versucht wird, die Spalte aus einem Stream mit unbekannter Länge zu aktualisieren.  
  
## <a name="see-also"></a>Weitere Informationen  
 [setNCharacterStream-Methode &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/setncharacterstream-method-sqlservercallablestatement.md)   
 [SQLServerCallableStatement-Elemente](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)  
  
  
