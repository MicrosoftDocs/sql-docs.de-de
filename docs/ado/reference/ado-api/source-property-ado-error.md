---
description: Source-Eigenschaft (ADO Error)
title: Source-Eigenschaft (ADO-Fehler) | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Error::get_Source
- Error::Source
- Error::GetSource
helpviewer_keywords:
- Source property [ADO Error]
ms.assetid: 4044ba15-f013-4c4c-9fe1-b4410fe9a778
author: rothja
ms.author: jroth
ms.openlocfilehash: b468c7b99d69e9da026a462b0c33084a4880eea3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051411"
---
# <a name="source-property-ado-error"></a>Source-Eigenschaft (ADO Error)
Gibt den Namen des Objekts oder der Anwendung an, das ursprünglich einen Fehler generiert hat.  
  
## <a name="return-value"></a>Rückgabewert  
 Gibt einen **Zeichen** folgen Wert zurück, der den Namen eines Objekts oder einer Anwendung angibt.  
  
## <a name="remarks"></a>Bemerkungen  
 Verwenden Sie die **Source** -Eigenschaft für ein [Fehler](./error-object.md) Objekt, um den Namen des Objekts oder der Anwendung zu ermitteln, das ursprünglich einen Fehler generiert hat. Dies könnte der Klassenname des Objekts oder die programmgesteuerte ID sein. Bei Fehlern in ADO lautet der Eigenschafts Wert **ADODB.** _ObjectName_, wobei *objectName* der Name des Objekts ist, das den Fehler ausgelöst hat. Für ADOX und ADO MD lautet der Wert " **ADOX".** _ObjectName_ und **ADOMD.** _ObjectName_.  
  
 Basierend auf der Fehler Dokumentation aus den **Quellen**-, [Zahlen](./number-property-ado.md)-und [Beschreibungs](./description-property.md) Eigenschaften von **Error** -Objekten können Sie Code schreiben, der den Fehler entsprechend behandelt.  
  
 Die **Source** -Eigenschaft ist für **Fehler** Objekte schreibgeschützt.  
  
## <a name="applies-to"></a>Gilt für  
 [Error-Objekt](./error-object.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Beispiel für Beschreibung, HelpContext, HelpFile, NativeError, Number, Source und SQLSTATE Properties (VB)](./description-helpcontext-helpfile-nativeerror-number-source-example-vb.md)   
 [Beschreibung, HelpContext, HelpFile, NativeError, Number, Source und SQLSTATE Properties Example (VC + +)](./description-helpcontext-helpfile-nativeerror-number-source-example-vc.md)   
 [Description-Eigenschaft](./description-property.md)   
 [HelpContext, HelpFile-Eigenschaften](./helpcontext-helpfile-properties.md)   
 [Number-Eigenschaft (ADO)](./number-property-ado.md)   
 [Source-Eigenschaft (ADO-Datensatz)](./source-property-ado-record.md)   
 [Source-Eigenschaft (ADO-Recordset)](./source-property-ado-recordset.md)