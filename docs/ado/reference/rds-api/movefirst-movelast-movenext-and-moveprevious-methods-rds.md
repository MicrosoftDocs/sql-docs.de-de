---
description: MoveFirst-, MoveLast-, MoveNext- und MovePrevious-Methode (RDS)
title: "\"Muvefirst\", \"muvelast\", \"muvenext\" und \"muveprevious\" (RDS) | Microsoft-Dokumentation"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- MoveLast method [RDS]
- MovePrevious method [RDS]
- MoveFirst method [RDS]
- MoveNext method [RDS]
ms.assetid: 45c80bb5-136f-4204-9df2-78740fa55574
author: rothja
ms.author: jroth
ms.openlocfilehash: 54395a8b0bdc88e873ad25d41ab92304e7f5efe6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049280"
---
# <a name="movefirst-movelast-movenext-and-moveprevious-methods-rds"></a>MoveFirst-, MoveLast-, MoveNext- und MovePrevious-Methode (RDS)
Wechselt zum ersten, letzten, nächsten oder vorherigen Datensatz in einem angegebenen [Recordset](../ado-api/recordset-object-ado.md) -Objekt.  
  
> [!IMPORTANT]
>  Ab Windows 8 und Windows Server 2012 sind RDS-Server Komponenten nicht mehr im Windows-Betriebssystem enthalten (weitere Details finden Sie unter Windows 8 und [Windows Server 2012 Compatibility Cookbook](https://www.microsoft.com/download/details.aspx?id=27416) ). RDS-Client Komponenten werden in einer zukünftigen Version von Windows entfernt. Nutzen Sie diese Funktionen bei Neuentwicklungen nicht mehr, und planen Sie die Änderung von Anwendungen, die diese Funktion zurzeit verwenden. Anwendungen, die RDS verwenden, sollten zu [WCF Data Service](/dotnet/framework/wcf/)migriert werden.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
DataControl.Recordset.{MoveFirst | MoveLast | MoveNext | MovePrevious}  
```  
  
#### <a name="parameters"></a>Parameter  
 *DataControl*  
 Eine Objekt Variable, die einen [RDS darstellt. DataControl](./datacontrol-object-rds.md) -Objekt.  
  
## <a name="remarks"></a>Bemerkungen  
 Die **Move** -Methoden können mit RDS verwendet werden **. DataControl** -Objekt, um durch die Datensätze in den Daten gebundenen Steuerelementen auf einer Webseite zu navigieren. Nehmen Sie beispielsweise an, dass Sie ein **Recordset** in einem Raster durch Binden an einen **RDS anzeigen. DataControl** -Objekt. Sie können dann die Schaltflächen First, Last, Next und Previous einschließen, auf die Benutzer klicken können, um zum ersten, letzten, nächsten oder vorherigen Datensatz im angezeigten **Recordset** zu wechseln. Dies erreichen Sie, indem Sie die Methoden " **muvefirst**", " **muvelast**", " **muvenext**" und " **muveprevious** " des **RDS aufrufen. DataControl** -Objekt in den OnClick-Prozeduren für die Schaltflächen First, Last, Next und Previous. Das [Adressbuch Beispiel](../../guide/remote-data-service/address-book-navigation-buttons.md) zeigt die Vorgehensweise.  
  
## <a name="applies-to"></a>Gilt für  
 [DataControl-Objekt (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Move-Methode (ADO)](../ado-api/move-method-ado.md)   
 [Muvefirst-, muvelast-, muvenext-und muveprevious-Methode (ADO)](../ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md)   
 [MoveRecord-Methode (ADO)](../ado-api/moverecord-method-ado.md)