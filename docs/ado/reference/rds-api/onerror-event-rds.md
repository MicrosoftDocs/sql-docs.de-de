---
description: onError-Ereignis (RDS)
title: OnError-Ereignis (RDS) | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
helpviewer_keywords:
- onError event [ADO]
ms.assetid: b01cbc62-fbd7-4068-b16c-8b0f80a05887
author: rothja
ms.author: jroth
ms.openlocfilehash: fb9ab9b7b97875ee62fe96a39ff8d34591810560
ms.sourcegitcommit: 7345e4f05d6c06e1bcd73747a4a47873b3f3251f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/24/2020
ms.locfileid: "88767949"
---
# <a name="onerror-event-rds"></a>onError-Ereignis (RDS)
Das **OnError** -Ereignis wird immer dann aufgerufen, wenn während eines Vorgangs ein Fehler auftritt.  
  
> [!IMPORTANT]
>  Ab Windows 8 und Windows Server 2012 sind RDS-Server Komponenten nicht mehr im Windows-Betriebssystem enthalten (weitere Details finden Sie unter Windows 8 und [Windows Server 2012 Compatibility Cookbook](https://www.microsoft.com/download/details.aspx?id=27416) ). RDS-Client Komponenten werden in einer zukünftigen Version von Windows entfernt. Nutzen Sie diese Funktionen bei Neuentwicklungen nicht mehr, und planen Sie die Änderung von Anwendungen, die diese Funktion zurzeit verwenden. Anwendungen, die RDS verwenden, sollten zu [WCF Data Service](https://go.microsoft.com/fwlink/?LinkId=199565)migriert werden.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
onError SCode, Description, Source, CancelDisplay  
```  
  
#### <a name="parameters"></a>Parameter  
 *SCode*  
 Eine ganze Zahl, die den Statuscode des Fehlers angibt.  
  
 *Beschreibung*  
 Eine **Zeichen** Folge, die eine Beschreibung des Fehlers angibt.  
  
 *Quelle*  
 Eine **Zeichenfolge** , die die Abfrage oder den Befehl angibt, die den Fehler verursacht hat.  
  
 *CancelDisplay*  
 Ein **boolescher** Wert, der, wenn er auf **true**festgelegt ist, verhindert, dass der Fehler in einem Dialogfeld angezeigt wird.  
  
## <a name="applies-to"></a>Gilt für  
 [DataControl-Objekt (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Beispiel für das ADO-Ereignis Modell (VC + +)](../ado-api/ado-events-model-example-vc.md)   
 [ADO-Ereignishandler – Übersicht](../../guide/data/ado-event-handler-summary.md)