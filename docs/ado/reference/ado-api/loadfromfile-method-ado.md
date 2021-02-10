---
description: LoadFromFile-Methode (ADO)
title: LoadFromFile-Methode (ADO) | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Stream::raw_LoadFromFile
helpviewer_keywords:
- LoadFromFile method [ADO]
ms.assetid: b18d8d38-7354-4a94-b637-6ac035faa433
author: rothja
ms.author: jroth
ms.openlocfilehash: b1b3b375158bb8d296c9010caab900db4791fcec
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041840"
---
# <a name="loadfromfile-method-ado"></a>LoadFromFile-Methode (ADO)
Lädt den Inhalt einer vorhandenen Datei in einen [Stream](./stream-object-ado.md).  
  
## <a name="syntax"></a>Syntax  
  
```  
  
Stream.LoadFromFileFileName  
```  
  
#### <a name="parameters"></a>Parameter  
 *FileName*  
 Ein **Zeichen** folgen Wert, der den Namen einer Datei enthält, die in den **Stream** geladen werden soll. *Filename* kann einen beliebigen gültigen Pfad und Namen im UNC-Format enthalten. Wenn die angegebene Datei nicht vorhanden ist, tritt ein Laufzeitfehler auf.  
  
## <a name="remarks"></a>Bemerkungen  
 Diese Methode kann verwendet werden, um den Inhalt einer lokalen Datei in ein Daten **Strom** Objekt zu laden. Dies kann verwendet werden, um den Inhalt einer lokalen Datei auf einen Server hochzuladen.  
  
 Das **Stream** -Objekt muss vor dem Aufruf von **LoadFromFile** bereits geöffnet sein. Mit dieser Methode wird die Bindung des Daten **Strom** Objekts nicht geändert. Sie wird weiterhin an das Objekt gebunden, das von der URL oder dem **Datensatz** angegeben wird, mit dem der **Stream** ursprünglich geöffnet wurde.  
  
 **LoadFromFile** überschreibt den aktuellen Inhalt des **Streamobjekts** mit aus der Datei gelesenen Daten. Alle vorhandenen Bytes im **Stream** werden durch den Inhalt der Datei überschrieben. Alle bereits vorhandenen und verbleibenden Bytes, die nach der von **LoadFromFile** erstellten [EOS](./eos-property.md) liegen, werden abgeschnitten.  
  
 Nach dem Aufrufen von **LoadFromFile** wird die aktuelle Position auf den Anfang des **Streams** festgelegt ([Position](./position-property-ado.md) ist 0).  
  
 Da dem Anfang des Streams für die Codierung 2 Bytes hinzugefügt werden können, entspricht die Größe des Streams möglicherweise nicht exakt der Größe der Datei, aus der Sie geladen wurde.  
  
## <a name="applies-to"></a>Gilt für  
 [Stream-Objekt (ADO)](./stream-object-ado.md)