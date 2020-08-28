---
description: Charset-Eigenschaft (ADO)
title: CharSet-Eigenschaft (ADO) | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- _Stream::Charset
helpviewer_keywords:
- Charset property [ADO]
ms.assetid: e42507cb-9b46-4ce4-8191-2948eaf14ca2
author: rothja
ms.author: jroth
ms.openlocfilehash: 52daa13826bbe372b141501a0c99ac281d3118dc
ms.sourcegitcommit: 18a98ea6a30d448aa6195e10ea2413be7e837e94
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2020
ms.locfileid: "88975501"
---
# <a name="charset-property-ado"></a>Charset-Eigenschaft (ADO)
Gibt den Zeichensatz an, in den der Inhalt eines [Textstreams](./stream-object-ado.md) für den Speicher im internen Puffer des **Streamobjekts** übersetzt werden soll.  
  
## <a name="settings-and-return-values"></a>Einstellungen und Rückgabewerte  
 Legt einen **Zeichen** folgen Wert fest, der den Zeichensatz angibt, in den der Inhalt des **Streams** übersetzt wird, oder gibt ihn zurück. Der Standardwert ist **Unicode**. Zulässige Werte sind typische Zeichen folgen, die über die Schnittstelle als Namen von Internet Zeichensätzen übergeben werden (z. b. "ISO-8859-1", "Windows-1252" usw.). Eine Liste der Namen der Zeichensätze, die von einem System bekannt sind, finden Sie in den unter Schlüsseln HKEY_CLASSES_ROOT \mime\database\charset in der Windows-Registrierung.  
  
## <a name="remarks"></a>Bemerkungen  
 In einem **Textstreamobjekt** werden Textdaten im Zeichensatz gespeichert, der durch die **CharSet** -Eigenschaft angegeben wird. Der Standardwert ist Unicode. Die **CharSet** -Eigenschaft wird zum Umrechnen von Daten verwendet, die in den **Stream** fließen oder aus dem **Stream**stammen. Wenn der **Stream** beispielsweise ISO-8859-1-Daten enthält und diese Daten in ein BSTR-Objekt kopiert werden, konvertiert das **Stream** -Objekt die Daten in Unicode. Das Gegenteil trifft ebenfalls zu.  
  
 Bei einem geöffneten **Stream**muss sich die aktuelle [Position](./position-property-ado.md) am Anfang des **Streams** (0) befinden, damit **CharSet**festgelegt werden kann.  
  
 **CharSet** wird nur mit **textstreamobjekten** ([Typ](./type-property-ado-stream.md) : **adtypetext**) verwendet. Diese Eigenschaft wird ignoriert, wenn der **Typ** **adTypeBinary**ist.  
  
 Ein Codebeispiel finden Sie unter [Step 4: Auffüllen des Details-Textfelds](../../guide/data/step-4-populate-the-details-text-box.md).  
  
## <a name="applies-to"></a>Gilt für  
 [Stream-Objekt (ADO)](./stream-object-ado.md)