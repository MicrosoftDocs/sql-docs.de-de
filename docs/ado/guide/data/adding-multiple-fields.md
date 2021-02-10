---
description: Hinzufügen mehrerer Felder und Werte
title: Hinzufügen mehrerer Felder | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- AddNew method [ADO]
- ADO, adding data
- editing data [ADO], adding multiple fields
- editing data [ADO], AddNew method
ms.assetid: f3648ef4-9f36-4991-a868-83a617389844
author: rothja
ms.author: jroth
ms.openlocfilehash: 4b72ed3beafd211439890946da9c41f39af8eb90
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028625"
---
# <a name="adding-multiple-fields-and-values"></a>Hinzufügen mehrerer Felder und Werte
Gelegentlich ist es möglicherweise effizienter, ein Array von Feldern und ihre entsprechenden Werte an die **AddNew** -Methode zu übergeben, anstatt für jedes neue Feld einen **Wert** mehrmals festzulegen. Wenn *FieldList* ein Array ist, müssen die *Werte* auch ein Array mit derselben Anzahl von Membern sein. Andernfalls tritt ein Fehler auf. Die Reihenfolge der Feldnamen muss der Reihenfolge der Feldwerte in jedem Array entsprechen. Der folgende Code übergibt ein Array von Feldern und ein Array von Werten an die **AddNew** -Methode.

```
'BeginAddNew2
    Dim avarFldNames As Variant
    Dim avarFldValues As Variant

    avarFldNames = Array("CompanyName", "Phone")
    avarFldValues = Array("Sample Shipper 2", "(931) 555-6334")

    If objRs1.Supports(adAddNew) Then
        objRs1.AddNew avarFldNames, avarFldValues
    End If

    'Re-establish a Connection and update
    Set objRs1.ActiveConnection = GetNewConnection
    objRs1.UpdateBatch
'EndAddNew2
```
