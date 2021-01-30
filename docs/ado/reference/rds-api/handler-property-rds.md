---
description: Handler-Eigenschaft (RDS)
title: Handler-Eigenschaft (RDS) | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Handler property [ADO]
ms.assetid: fdc34362-6d47-4727-b171-8d033159408e
author: rothja
ms.author: jroth
ms.openlocfilehash: 7562c5ddc3a8360f2c8672e7526c86b7f77af04d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168910"
---
# <a name="handler-property-rds"></a>Handler-Eigenschaft (RDS)
Gibt den Namen eines serverseitigen Anpassungsprogramms (Handler) an, das die Funktionalität von [RDSServer. DataFactory](./datafactory-object-rdsserver.md)erweitert, sowie alle Parameter, die vom *Handler* verwendet werden.  
  
 **Gilt für:** [DataControl Object (RDS)](./datacontrol-object-rds.md)  
  
> [!IMPORTANT]
>  Ab Windows 8 und Windows Server 2012 sind RDS-Server Komponenten nicht mehr im Windows-Betriebssystem enthalten (weitere Details finden Sie unter Windows 8 und [Windows Server 2012 Compatibility Cookbook](https://www.microsoft.com/download/details.aspx?id=27416) ). RDS-Client Komponenten werden in einer zukünftigen Version von Windows entfernt. Nutzen Sie diese Funktionen bei Neuentwicklungen nicht mehr, und planen Sie die Änderung von Anwendungen, die diese Funktion zurzeit verwenden. Anwendungen, die RDS verwenden, sollten zu [WCF Data Service](/dotnet/framework/wcf/)migriert werden.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
DataControl.Handler = String  
```  
  
#### <a name="parameters"></a>Parameter  
 *DataControl*  
 Eine Objekt Variable, die einen [RDS darstellt. DataControl](./datacontrol-object-rds.md) -Objekt.  
  
 *String*  
 Ein **Zeichen** folgen Wert, der den Namen des Handlers und alle Parameter enthält, die alle durch Kommas getrennt sind (z `"handlerName,parm1,parm2,...,parm`  `"` . b. N).  
  
## <a name="remarks"></a>Bemerkungen  
 Diese Eigenschaft unterstützt [Anpassungen](../../guide/remote-data-service/datafactory-customization.md), eine Funktion, die das Festlegen der Eigenschaft " [Cursor Location](../ado-api/cursorlocation-property-ado.md) " auf " **adUseClient**" erfordert.  
  
 Der Name des Handlers und der zugehörigen Parameter werden ggf. durch Kommas (",") getrennt. Ein unvorhersehbares Verhalten ergibt sich, wenn ein Semikolon (";") an einer beliebigen Stelle innerhalb der *Zeichen* Folge Sie können einen eigenen Handler schreiben, sofern er die **idatafactoriyhandler** -Schnittstelle unterstützt.  
  
 Der Name des Standard Handlers ist **msdfmap. Der Handler**, und sein Standardparameter ist eine Anpassungs Datei mit dem Namen **MSDFMAP.INI**. Verwenden Sie diese Eigenschaft, um alternative Anpassungs Dateien aufzurufen, die vom Server Administrator erstellt wurden.  
  
 Die Alternative zum Festlegen der **Handlereigenschaft** ist das Angeben eines Handlers und von Parametern in der [ConnectionString](../ado-api/connectionstring-property-ado.md) -Eigenschaft. Das heißt, "**Handler =**_HandlerName, Parameter1, Parameter2,...;_".  
  
## <a name="applies-to"></a>Gilt für  
 [DataControl-Objekt (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Beispiel für Handlereigenschaft (VB)](./handler-property-example-vb.md)   
 [DataFactory-Anpassung](../../guide/remote-data-service/datafactory-customization.md)   
 [DataFactory-Objekt (RDSServer)](./datafactory-object-rdsserver.md)