---
description: ConnectionTimeout-Eigenschaft (ADO)
title: ConnectionTimeout-Eigenschaft (ADO) | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- Connection15::ConnectionTimeout
helpviewer_keywords:
- ConnectionTimeout property [ADO]
ms.assetid: 8904a403-1383-4b4b-b53d-5c01d6f5deac
author: rothja
ms.author: jroth
ms.openlocfilehash: 4391d077621377fb2a21e39ba188864a3c37ea83
ms.sourcegitcommit: 7345e4f05d6c06e1bcd73747a4a47873b3f3251f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/24/2020
ms.locfileid: "88775859"
---
# <a name="connectiontimeout-property-ado"></a>ConnectionTimeout-Eigenschaft (ADO)
Gibt an, wie lange beim Herstellen einer Verbindung gewartet werden soll, bevor der Versuch beendet und ein Fehler erzeugt wird.  
  
## <a name="settings-and-return-values"></a>Einstellungen und Rückgabewerte  
 Legt einen **Long** -Wert fest, der angibt, wie lange in Sekunden gewartet werden soll, bis die Verbindung geöffnet wird, oder gibt diesen Wert zurück. Der Standardwert ist 15.  
  
## <a name="remarks"></a>Bemerkungen  
 Verwenden Sie die **ConnectionTimeout** -Eigenschaft für ein [Verbindungs](./connection-object-ado.md) Objekt, wenn eine Verzögerung von Netzwerk Datenverkehr oder starker Server Verwendung das Abbrechen eines Verbindungsversuchs erforderlich macht. Wenn die Zeit der **ConnectionTimeout** -Eigenschafts Einstellung vor dem Öffnen der Verbindung abläuft, tritt ein Fehler auf, und der Versuch wird von ADO abgebrochen. Wenn Sie die-Eigenschaft auf 0 (null) festlegen, wartet ADO unbegrenzt, bis die Verbindung geöffnet wird. Stellen Sie sicher, dass der Anbieter, für den Sie Code schreiben, die **ConnectionTimeout** -Funktionalität unterstützt.  
  
 Die **ConnectionTimeout** -Eigenschaft ist Lese-/Schreibzugriff, wenn die Verbindung geschlossen und schreibgeschützt ist, wenn Sie geöffnet ist.  
  
## <a name="applies-to"></a>Gilt für  
 [Connection-Objekt (ADO)](./connection-object-ado.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Beispiel für ConnectionString, ConnectionTimeout und State Properties (VB)](./connectionstring-connectiontimeout-and-state-properties-example-vb.md)   
 [Beispiel für ConnectionString, ConnectionTimeout und State Properties (VC + +)](./connectionstring-connectiontimeout-and-state-properties-example-vc.md)   
 [CommandTimeout-Eigenschaft (ADO)](./commandtimeout-property-ado.md)