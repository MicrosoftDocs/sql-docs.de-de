---
description: ParentRow-Eigenschaft (ADO)
title: Para Row-Eigenschaft (ADO) | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- ADORecordConstruction::put_ParentRow
- ADORecordConstruction::ParentRow
- ADORecordConstruction::putParentRow
helpviewer_keywords:
- ParentRow property [ADO]
ms.assetid: 5ea8029b-eda4-490b-ae84-2ad036fb582f
author: rothja
ms.author: jroth
ms.openlocfilehash: 03ef3135e72cac39f91dc1393458c36d63458db6
ms.sourcegitcommit: 18a98ea6a30d448aa6195e10ea2413be7e837e94
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2020
ms.locfileid: "88990091"
---
# <a name="parentrow-property-ado"></a>ParentRow-Eigenschaft (ADO)
Legt den Container eines OLE DB **Row** -Objekts für ein **adorecordconstruction** -Objekt fest, damit das übergeordnete Element der Zeile in ein ADO- **Daten Satz** Objekt umgewandelt wird.  
  
 Nur Schreibzugriff.  
  
## <a name="syntax"></a>Syntax  
  
```  
HRESULT put_ParentRow([in] IUnknown* pParent);  
```  
  
## <a name="parameters"></a>Parameter  
 *pparent*  
 Ein Container einer Zeile.  
  
## <a name="return-values"></a>Rückgabewerte  
 Diese Eigenschaften Methode gibt die HRESULT-Standardwerte zurück, einschließlich S_OK und E_FAIL.  
  
## <a name="applies-to"></a>Gilt für  
 [ADORecordConstruction-Schnittstelle](./adorecordconstruction-interface.md)