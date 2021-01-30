---
description: NativeError-Eigenschaft (ADO)
title: NativeError-Eigenschaft (ADO) | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Error::GetNativeError
- Error::get_NativeError
- Error::NativeError
helpviewer_keywords:
- NativeError property [ADO]
ms.assetid: b9b47e57-18a4-4186-aef5-5bd18d7b1d74
author: rothja
ms.author: jroth
ms.openlocfilehash: b25d6c3ffd9563032df8743c60a5db1cd3f78b4e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167061"
---
# <a name="nativeerror-property-ado"></a>NativeError-Eigenschaft (ADO)
Gibt den anbieterspezifischen Fehlercode für ein bestimmtes [Fehler](./error-object.md) Objekt an.  
  
## <a name="return-value"></a>Rückgabewert  
 Gibt einen **Long** -Wert zurück, der den Fehlercode angibt.  
  
## <a name="remarks"></a>Bemerkungen  
 Verwenden Sie die **NativeError** -Eigenschaft, um die datenbankspezifischen Fehlerinformationen für ein bestimmtes **Fehler** Objekt abzurufen. Wenn Sie z. b. den Microsoft ODBC-Anbieter für die OLE DB mit einer Microsoft SQL Server-Datenbank verwenden, werden systemeigene Fehlercodes, die aus SQL Server stammen, über ODBC und den ODBC-Anbieter an die ADO **NativeError** -Eigenschaft übergeben.  
  
## <a name="applies-to"></a>Gilt für  
 [Error-Objekt](./error-object.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Beispiel für Beschreibung, HelpContext, HelpFile, NativeError, Number, Source und SQLSTATE Properties (VB)](./description-helpcontext-helpfile-nativeerror-number-source-example-vb.md)   
 [Beschreibung, HelpContext, HelpFile, NativeError, Number, Source und SQLSTATE Properties Example (VC + +)](./description-helpcontext-helpfile-nativeerror-number-source-example-vc.md)