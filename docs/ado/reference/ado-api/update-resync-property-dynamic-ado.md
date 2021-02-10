---
description: Update Resync – dynamische Eigenschaft (ADO)
title: Aktualisieren von Resync-Property-Dynamic (ADO) | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Update Resync property [ADO]
ms.assetid: 8a3bb608-66d7-4128-a3ef-84cb0556de0d
author: rothja
ms.author: jroth
ms.openlocfilehash: 8e52c07a4f776c740079ae1460a2890d7aac02ec
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056295"
---
# <a name="update-resync-property-dynamic-ado"></a>Update Resync – dynamische Eigenschaft (ADO)
Gibt an, ob auf die [UpdateBatch](./updatebatch-method.md) -Methode ein impliziter Vorgang zum [erneuten Synchronisieren](./resync-method.md) der Methode folgt, und wenn ja, der Gültigkeitsbereich dieses Vorgangs.  
  
## <a name="settings-and-return-values"></a>Einstellungen und Rückgabewerte  
 Legt einen oder mehrere der [ADCPROP_UPDATERESYNC_ENUM](./adcprop-updateresync-enum.md) Werte fest oder gibt ihn zurück.  
  
## <a name="remarks"></a>Bemerkungen  
 Die Werte ADCPROP_UPDATERESYNC_ENUM können kombiniert werden, mit Ausnahme von adresyncall, das bereits die Kombination der restlichen Werte darstellt.  
  
 Die Konstante **adresyncconflicts** speichert die Werte für die erneute Synchronisierung als zugrunde liegende Werte, überschreibt jedoch ausstehende Änderungen nicht.  
  
 **Update Resync** ist eine dynamische Eigenschaft, die an die Auflistung der [Recordset](./recordset-object-ado.md) -Objekt [Eigenschaften](./properties-collection-ado.md) angehängt wird, wenn die [Cursor Location](./cursorlocation-property-ado.md) -Eigenschaft auf **adUseClient** festgelegt ist.  
  
## <a name="applies-to"></a>Gilt für  
 [Recordset-Objekt (ADO)](./recordset-object-ado.md)