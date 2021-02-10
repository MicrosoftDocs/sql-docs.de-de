---
description: RecordCreateOptionsEnum
title: Recordkreateoptionsenum | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- RecordCreateOptionsEnum
helpviewer_keywords:
- RecordCreateOptionsEnum enumeration [ADO]
ms.assetid: 6d746670-0850-4065-9cd4-168dea1d3ea9
author: rothja
ms.author: jroth
ms.openlocfilehash: 942f11dfa25a92f6914c868ffcdeb65c434e490c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051851"
---
# <a name="recordcreateoptionsenum"></a>RecordCreateOptionsEnum
Gibt an, ob ein vorhandener **Datensatz** geöffnet oder ein neuer **Datensatz** für die [Open](./open-method-ado-record.md) -Methode des [Aufzeichnungs](./record-object-ado.md) Objekts erstellt werden soll. Die Werte können mit einem and-Operator kombiniert werden.  
  
|Konstante|Wert|BESCHREIBUNG|  
|--------------|-----------|-----------------|  
|**adkreatecollection**|0x2000|Erstellt einen neuen **Datensatz** an dem durch den *Quell* Parameter angegebenen Knoten, anstatt einen vorhandenen **Datensatz** zu öffnen. Wenn die Quelle auf einen vorhandenen Knoten zeigt, tritt ein Laufzeitfehler auf, es sei denn, **adkreatecollection** wird mit **adopenifist** oder **adkreateüberschreibung** kombiniert.|  
|**adkreatenoncollection**|0|Erstellt einen neuen **Datensatz** vom Typ [adsimplerecord](./recordtypeenum.md).|  
|**adkreateüberschreibung**|0x4000000|Ändert die erstellungsflags **adkreatecollection**, **adkreatenoncollection** und **adkreatestructdoc**. Wenn oder mit diesem Wert und einem der erstellungsflagwerte verwendet wird, wird der vorhandene Datensatz überschrieben, und es wird ein neuer **Datensatz** erstellt, wenn die Quell-URL auf einen vorhandenen Knoten oder **Datensatz** verweist. Dieser Wert kann nicht in Verbindung mit **adopenises** verwendet werden.|  
|**adkreatestructdoc**|0x80000000|Erstellt einen neuen **Datensatz** vom Typ [adstructdoc](./recordtypeenum.md), anstatt einen vorhandenen **Datensatz** zu öffnen.|  
|**adfailif NotExists**|-1|Standard. Führt zu einem Laufzeitfehler, wenn die *Quelle* auf einen nicht vorhandenen Knoten zeigt.|  
|**adopenif vorhanden**|0x2000000|Ändert die erstellungsflags **adkreatecollection**, **adkreatenoncollection** und **adkreatestructdoc**. Wenn oder mit diesem Wert und einem der erstellungsflagwerte verwendet wird, muss der Anbieter den vorhandenen **Datensatz** öffnen, anstatt einen neuen Datensatz zu erstellen, wenn die Quell-URL auf ein vorhandenes Knoten-oder **Daten Satz** Objekt verweist. Dieser Wert kann nicht in Verbindung mit **adkreateüberschreibung** verwendet werden.|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC-Entsprechung  
 Diese Konstanten haben keine ADO/WFC-Entsprechungen.  
  
## <a name="applies-to"></a>Gilt für  
 [Open-Methode (ADO Record)](./open-method-ado-record.md)