---
description: Field (ADO für Visual C++-Syntax)
title: Field (ADO für Visual C++-Syntax) | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
dev_langs:
- C++
helpviewer_keywords:
- Field collection [ADO], ADO for Visual C++ syntax
ms.assetid: 04631b08-3937-440b-ac09-cd166f239908
author: rothja
ms.author: jroth
ms.openlocfilehash: ae296d7bf6b42b5a978110b8626086528dc24d89
ms.sourcegitcommit: 18a98ea6a30d448aa6195e10ea2413be7e837e94
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2020
ms.locfileid: "88973281"
---
# <a name="field-ado-for-visual-c-syntax"></a>Field (ADO für Visual C++-Syntax)
## <a name="methods"></a>Methoden  
  
```  
AppendChunk(VARIANT Data)  
GetChunk(long Length, VARIANT *pvar)  
```  
  
## <a name="properties"></a>Eigenschaften  
  
```  
get_ActualSize(long *pl)  
get_Attributes(long *pl)  
put_Attributes(long lAttributes)  
get_DataFormat(IUnknown **ppiDF)  
put_DataFormat(IUnknown *piDF)  
get_DefinedSize(long *pl)  
put_DefinedSize(long lSize)  
get_Name(BSTR *pbstr)  
get_NumericScale(BYTE *pbNumericScale)  
put_NumericScale(BYTE bScale)  
get_OriginalValue(VARIANT *pvar)  
get_Precision(BYTE *pbPrecision)  
put_Precision(BYTE bPrecision)  
get_Type(DataTypeEnum *pDataType)  
put_Type(DataTypeEnum DataType)  
get_UnderlyingValue(VARIANT *pvar)  
get_Value(VARIANT *pvar)  
put_Value(VARIANT Val)  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Field-Objekt](../../../ado/reference/ado-api/field-object.md)
