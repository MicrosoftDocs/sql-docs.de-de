---
description: Row-Eigenschaft (ADO)
title: Row-Eigenschaft (ADO) | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ADORecordConstruction::PutRow
- ADORecordConstruction::GetRow
- ADORecordConstruction::get_Row
- ADORecordConstruction::Row
- ADORecordConstruction::put_Row
helpviewer_keywords:
- Row property [ADO]
ms.assetid: 21019d89-2dd1-4a26-ac6f-384b81d66949
author: rothja
ms.author: jroth
ms.openlocfilehash: cf90fe1219103713797dfd04b1475d7fc77c80a0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170346"
---
# <a name="row-property-ado"></a>Row-Eigenschaft (ADO)
Ruft ein OLE DB **Row** -Objekt aus oder für ein [adorecordconstruction-Schnittstellen](./adorecordconstruction-interface.md) Objekt ab oder legt es fest. Wenn Sie **put_Row** verwenden, um ein **Zeilen** Objekt festzulegen, wird eine Zeile in ein ADO- **Daten** Satz Objekt umgewandelt.  
  
## <a name="readwritesyntax"></a>Lese-/Schreibzugriff. Syntax  
  
```  
HRESULT get_Row([out, retval] IUnknown** ppRow);  
HRESULT put_Row([in] IUnknown* pRow);  
```  
  
## <a name="parameters"></a>Parameter  
 *ppRow*  
 Zeiger auf ein OLE DB **Row** -Objekt.  
  
 *Prow*  
 Ein OLE DB **Row** -Objekt.  
  
## <a name="return-values"></a>Rückgabewerte  
 Diese Eigenschaften Methode gibt die HRESULT-Standardwerte zurück, einschließlich S_OK und E_FAIL.  
  
## <a name="applies-to"></a>Gilt für  
 [ADORecordConstruction-Schnittstelle](./adorecordconstruction-interface.md)