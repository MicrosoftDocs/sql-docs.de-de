---
description: CancelUpdate-Methode (RDS)
title: CancelUpdate-Methode (RDS) | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- CancelUpdate method [RDS]
ms.assetid: 76d8a6e9-bc6c-4ea0-8e7a-2bae5ed06650
author: rothja
ms.author: jroth
ms.openlocfilehash: a321c84beb8277f05b0779a540a72b6a82b87933
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049540"
---
# <a name="cancelupdate-method-rds"></a>CancelUpdate-Methode (RDS)
Bricht alle Änderungen ab, die an der aktuellen oder neuen Zeile eines [Recordset](../ado-api/recordset-object-ado.md) -Objekts vorgenommen wurden.  
  
> [!IMPORTANT]
>  Ab Windows 8 und Windows Server 2012 sind RDS-Server Komponenten nicht mehr im Windows-Betriebssystem enthalten (weitere Details finden Sie unter Windows 8 und [Windows Server 2012 Compatibility Cookbook](https://www.microsoft.com/download/details.aspx?id=27416) ). RDS-Client Komponenten werden in einer zukünftigen Version von Windows entfernt. Nutzen Sie diese Funktionen bei Neuentwicklungen nicht mehr, und planen Sie die Änderung von Anwendungen, die diese Funktion zurzeit verwenden. Anwendungen, die RDS verwenden, sollten zu [WCF Data Service](/dotnet/framework/wcf/)migriert werden.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
DataControl.CancelUpdate  
```  
  
#### <a name="parameters"></a>Parameter  
 *DataControl*  
 Eine Objekt Variable, die einen [RDS darstellt. DataControl](./datacontrol-object-rds.md) -Objekt.  
  
## <a name="remarks"></a>Bemerkungen  
 Der Cursor Dienst für OLE DB behält sowohl eine Kopie der ursprünglichen Werte als auch einen Cache von Änderungen bei. Wenn Sie **CancelUpdate** aufrufen, wird der Cache der Änderungen auf "Empty" zurückgesetzt, und alle gebundenen Steuerelemente werden mit den ursprünglichen Daten aktualisiert.  
  
## <a name="applies-to"></a>Gilt für  
 [DataControl-Objekt (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [CancelUpdate-Methode (Beispiel) (VBScript)](./cancelupdate-method-example-vbscript.md)   
 [Adressbuch-Befehls Schaltflächen](../../guide/remote-data-service/address-book-command-buttons.md)   
 [Cancel-Methode (ADO)](../ado-api/cancel-method-ado.md)   
 [Cancel-Methode (RDS)](./cancel-method-rds.md)   
 [CancelBatch-Methode (ADO)](../ado-api/cancelbatch-method-ado.md)   
 [CancelUpdate-Methode (ADO)](../ado-api/cancelupdate-method-ado.md)   
 [Refresh-Methode (RDS)](./refresh-method-rds.md)   
 [SubmitChanges-Methode (RDS)](./submitchanges-method-rds.md)