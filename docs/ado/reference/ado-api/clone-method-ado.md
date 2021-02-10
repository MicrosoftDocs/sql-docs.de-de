---
description: Clone-Methode (ADO)
title: Clone-Methode (ADO) | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset20::Clone
- Recordset20::raw_Clone
helpviewer_keywords:
- Clone method [ADO]
ms.assetid: ad49265f-1c05-4271-9bbf-7c00010ac18c
author: rothja
ms.author: jroth
ms.openlocfilehash: 57bf5f2a4e9134ffdd497133c44741af8a84f2ac
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100027108"
---
# <a name="clone-method-ado"></a>Clone-Methode (ADO)
Erstellt ein doppeltes [Recordset](./recordset-object-ado.md) -Objekt aus einem vorhandenen **Recordset** -Objekt. Gibt optional an, dass der Klon schreibgeschützt ist.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
Set rstDuplicate = rstOriginal.Clone (LockType)  
```  
  
## <a name="return-value"></a>Rückgabewert  
 Gibt einen **Recordset** -Objekt Verweis zurück.  
  
#### <a name="parameters"></a>Parameter  
 *rstduplicate*  
 Eine Objekt Variable, die das doppelte **Recordsetobjekt** angibt, das erstellt werden soll.  
  
 *rstoriginal*  
 Eine Objekt Variable, die das **Recordset** -Objekt identifiziert, das dupliziert werden soll.  
  
 *LockType*  
 Optional. Ein [LockTypeEnum](./locktypeenum.md) -Wert, der entweder den Sperrtyp des ursprünglichen **Recordsets** oder ein Schreib geschütztes **Recordset** angibt. Gültige Werte sind " **adlockunspezifiziert** " oder " **adlockread only**".  
  
## <a name="remarks"></a>Bemerkungen  
 Verwenden Sie die **Clone** -Methode, um mehrere doppelte **Recordset** -Objekte zu erstellen, insbesondere, wenn Sie mehr als einen aktuellen Datensatz in einer bestimmten Gruppe von Datensätzen verwalten möchten. Die Verwendung der **Klon** Methode ist effizienter als das Erstellen und Öffnen eines neuen **Recordset** -Objekts, das die gleiche Definition wie das Original verwendet.  
  
 Die [Filter](./filter-property.md) -Eigenschaft des ursprünglichen **Recordsets** wird ggf. nicht auf den Klon angewendet. Legen Sie die **Filter** -Eigenschaft des neuen **Recordsets** fest, um die Ergebnisse zu filtern. Die einfachste Möglichkeit zum Kopieren eines vorhandenen **Filter** Werts besteht darin, ihn wie folgt direkt zuzuweisen.  
  
```  
rsNew.Filter = rsOriginal.Filter  
```  
  
 Der aktuelle Datensatz eines neu erstellten Klons wird auf den ersten Datensatz festgelegt.  
  
 Änderungen, die Sie an einem **Recordset** -Objekt vornehmen, sind unabhängig vom Cursortyp in allen Klonen sichtbar. Nachdem Sie jedoch die [Requery](./requery-method.md) für das ursprüngliche **Recordset** ausgeführt haben, werden die Klone nicht mehr mit der ursprünglichen synchronisiert.  
  
 Durch das Schließen des ursprünglichen **Recordsets** werden seine Kopien nicht geschlossen, und das Schließen einer Kopie schließt das Original oder eine der anderen Kopien.  
  
 Sie können nur ein **Recordset** -Objekt klonen, das Lesezeichen unterstützt. Lesezeichen Werte sind austauschbar. Das heißt, dass sich ein Lesezeichen Verweis von einem **Recordset** -Objekt auf denselben Datensatz in jedem seiner Klone bezieht.  
  
 Einige **Recordset** -Ereignisse, die ausgelöst werden, treten auch in allen **recordsetklonen** auf. Da sich der aktuelle Datensatz jedoch zwischen geklonten **Recordsets** unterscheiden kann, sind die Ereignisse für den Klon möglicherweise nicht gültig. Wenn Sie z. b. den Wert eines Felds ändern, tritt ein [WillChangeField](./willchangefield-and-fieldchangecomplete-events-ado.md) -Ereignis im geänderten **Recordset** und in allen Klonen auf. Der *Fields* -Parameter des **WillChangeField** -Ereignisses eines geklonten **Recordsets** (bei dem die Änderung nicht vorgenommen wurde) verweist auf die Felder des aktuellen Datensatzes des Klons, bei dem es sich möglicherweise um einen anderen Datensatz als den aktuellen Datensatz des ursprünglichen **Recordsets** handelt, in dem die Änderung aufgetreten ist.  
  
 Die folgende Tabelle enthält eine vollständige Liste aller **Recordset** -Ereignisse. Gibt an, ob Sie gültig sind und für beliebige recordsetklone ausgelöst werden, die mit der **Klon** Methode generiert wurden.  
  
|Ereignis|In Klonen ausgelöst?|  
|-----------|--------------------------|  
|[EndOf Recordset](./endofrecordset-event-ado.md)|Nein|  
|[FetchComplete](./fetchcomplete-event-ado.md)|Nein|  
|[FetchProgress](./fetchprogress-event-ado.md)|Nein|  
|[FieldChangeComplete](./willchangefield-and-fieldchangecomplete-events-ado.md)|Ja|  
|[Der Vorgang wurde beendet.](./willmove-and-movecomplete-events-ado.md)|Nein|  
|[RecordChangeComplete](./willchangerecord-and-recordchangecomplete-events-ado.md)|Ja|  
|[RecordsetChangeComplete](./willchangerecordset-and-recordsetchangecomplete-events-ado.md)|Nein|  
|[WillChangeField](./willchangefield-and-fieldchangecomplete-events-ado.md)|Ja|  
|[WillChangeRecord](./willchangerecord-and-recordchangecomplete-events-ado.md)|Ja|  
|[WillChangeRecordset](./willchangerecordset-and-recordsetchangecomplete-events-ado.md)|Nein|  
|[WillMove](./willmove-and-movecomplete-events-ado.md)|Nein|  
  
## <a name="applies-to"></a>Gilt für  
 [Recordset-Objekt (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Beispiel für eine Klon Methode (VB)](./clone-method-example-vb.md)   
 [Klon Methode (Beispiel) (VBScript)](./clone-method-example-vbscript.md)   
 [Clone-Methode – Beispiel (VC++)](./clone-method-example-vc.md)