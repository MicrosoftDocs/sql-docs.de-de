---
description: ActualSize-Eigenschaft (ADO)
title: ActualSize-Eigenschaft (ADO) | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 03/20/2018
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Field20::ActualSize
helpviewer_keywords:
- ActualSize property [ADO]
ms.assetid: 722803d0-cef5-4d4c-b79d-3f2f58052229
author: rothja
ms.author: jroth
ms.openlocfilehash: 3fa2172e43b1c3af015b8483a8740641a0e443d5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100035860"
---
# <a name="actualsize-property-ado"></a>ActualSize-Eigenschaft (ADO)
Gibt die tatsächliche Länge des Werts eines Felds in Bytes an.  
  
## <a name="settings-and-return-values"></a>Einstellungen und Rückgabewerte  
 Gibt einen **Long** -Wert zurück.  
  
## <a name="remarks"></a>Bemerkungen  
 Verwenden Sie die **ActualSize** -Eigenschaft, um die tatsächliche Länge des Werts eines [Feld](./field-object.md) Objekts zurückzugeben. Die **ActualSize** -Eigenschaft ist für alle Felder schreibgeschützt. Wenn ADO die Länge des **Feld** Objekt Werts nicht ermitteln kann, gibt die **ActualSize** -Eigenschaft **adunknown** zurück.  
  
 Die Eigenschaften **ActualSize** und [DefinedSize](./definedsize-property.md) sind unterschiedlich, wie im folgenden Beispiel gezeigt. Ein **Feld** Objekt mit einem deklarierten Typ von **adVarChar** und eine maximale Länge von 50 Zeichen gibt einen **DefinedSize** -Eigenschafts Wert von 50 zurück, aber der Rückgabewert der **ActualSize** -Eigenschaft ist die Länge der Daten, die im Feld für den aktuellen Datensatz gespeichert sind. **Felder** mit einer **DefinedSize** größer als 255 Bytes werden als Spalten variabler Länge behandelt.  
  
## <a name="applies-to"></a>Gilt für  
 [Field-Objekt](./field-object.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Beispiel für ActualSize und DefinedSize-Eigenschaften (VB)](./actualsize-and-definedsize-properties-example-vb.md)   
 [Beispiel für ActualSize und DefinedSize-Eigenschaften (VC + +)](./actualsize-and-definedsize-properties-example-vc.md)   
 [DefinedSize-Eigenschaft](./definedsize-property.md)