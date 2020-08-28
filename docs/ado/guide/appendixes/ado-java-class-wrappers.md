---
description: ADO-Java-Klassen-Wrapper
title: ADO-Java-Klassen Wrapper | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- class wrappers [ADO]
ms.assetid: 1fc09dc1-9e32-412e-9f43-b8eb8bb483ca
author: rothja
ms.author: jroth
ms.openlocfilehash: 1694c3782aa57993cee39ea4f449e7b280fccdf6
ms.sourcegitcommit: 18a98ea6a30d448aa6195e10ea2413be7e837e94
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2020
ms.locfileid: "88991190"
---
# <a name="ado-java-class-wrappers"></a>ADO-Java-Klassen-Wrapper
Dieser Code deklariert eine Instanz des ADO- [Recordset](../../reference/ado-api/recordset-object-ado.md) -Klassen-Wrapper und initialisiert Sie, alle in derselben Codezeile. Außerdem werden Variablen für jedes der Argumente in der [Open](../../reference/ado-api/open-method-ado-recordset.md) -Methode deklariert, insbesondere für [LockType](../../reference/ado-api/locktype-property-ado.md) und Cursor Type (da Java Enumerationstypen nicht unterstützt). [CursorType](../../reference/ado-api/cursortype-property-ado.md) Das **Recordset** -Objekt wird geöffnet und geschlossen. Durch Festlegen von "RS1 auf NULL" wird lediglich die Freigabe dieser Variablen geplant, wenn Java die systematische und vorübergehende Freigabe nicht verwendeter Objekte ausführt.  
  
```java
public static void main( String args[])  
{  
   msado15._Recordset   Rs1 = new msado15.Recordset();  
   Variant Source     = new Variant( "SELECT * FROM Authors" );  
   Variant Connect    = new Variant( "DSN=AdoDemo;UID=admin;PWD=;" );  
   int     LockType   = msado15.CursorTypeEnum.adOpenForwardOnly;  
   int     CursorType = msado15.LockTypeEnum.adLockReadOnly;  
   int     Options    = -1;  
  
   Rs1.Open( Source, Connect, LockType,  CursorType, Options );  
   Rs1.Close();  
   Rs1 = null;  
  
   System.out.println( "Success!\n" );  
}  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Verwenden des Microsoft SDK für Java](./using-the-microsoft-sdk-for-java.md)