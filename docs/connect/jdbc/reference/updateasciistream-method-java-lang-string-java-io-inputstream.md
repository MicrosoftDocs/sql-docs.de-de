---
description: updateAsciiStream-Methode (java.lang.String, java.io.InputStream)
title: updateAsciiStream-Methode (java.lang.String, java.io.InputStream)
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 747b0308-1ce6-4eba-bdfc-af29c21c18cf
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5f42bda7c4938d1dc0a4ed3fad9bf446c63bad2d
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88478512"
---
# <a name="updateasciistream-method-javalangstring-javaioinputstream"></a>updateAsciiStream-Methode (java.lang.String, java.io.InputStream)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Aktualisiert die angegebene Spalte mit einem ASCII-Datenstromwert.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
public void updateAsciiStream(java.lang.String columnLabel,  
                              java.io.InputStream x)  
```  
  
#### <a name="parameters"></a>Parameter  
 *columnLabel*  
  
 Eine **Zeichenfolge**, die die Spaltenbezeichnung enthält.  
  
 *x*  
  
 Ein InputStream-Objekt  
  
## <a name="exceptions"></a>Ausnahmen  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Bemerkungen  
 Diese updateAsciiStream-Methode wird von der updateAsciiStream-Methode in der java.sql.ResultSet-Schnittstelle angegeben.  
  
 Von dieser Methode werden ASCII-Zeichen (Bytes) von einem InputStream-Objekt an konvertierbare Zeichenspalten übergeben, bei denen es sich um den ASCII-Bereich „[0x00 – 0x7F]“ von Unicode sowie um die Codepages 874, 932, 936, 949, 950 und 1250 bis 1258 handelt. Von dieser Methode wird eine Konvertierung zur Zielsortierseite vorgenommen. Beim Versuch, eine nicht konvertierbare Zielspalte zu aktualisieren, wird eine Ausnahme ausgelöst. Für Binärspalten werden Rohbytes übergeben.  
  
 Die Verwendung dieser Methode für die Datentypen **image**, **text** und **ntext**[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] kann sich negativ auf die Leistung auswirken.  
  
## <a name="see-also"></a>Weitere Informationen  
 [updateAsciiStream-Methode &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/updateasciistream-method-sqlserverresultset.md)   
 [SQLServerResultSet-Elemente](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet-Klasse](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
