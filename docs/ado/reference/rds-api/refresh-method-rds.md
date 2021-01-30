---
description: Refresh-Methode (RDS)
title: Refresh-Methode (RDS) | Microsoft-Dokumentation
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.topic: reference
apitype: COM
f1_keywords:
- Refresh
- RDS.DataControl::Refresh
- DataControl::Refresh
helpviewer_keywords:
- Refresh method [RDS]
ms.assetid: c90a8050-0ff4-4c83-9925-261f2f2ccfe9
author: rothja
ms.author: jroth
ms.openlocfilehash: f35574b0e9623560ddfafe123340e302a9fc5220
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168793"
---
# <a name="refresh-method-rds"></a>Refresh-Methode (RDS)
Fragt die in der [Connect](./connect-property-rds.md) -Eigenschaft angegebene Datenquelle ab und aktualisiert die Abfrageergebnisse.  
  
> [!IMPORTANT]
>  Ab Windows 8 und Windows Server 2012 sind RDS-Server Komponenten nicht mehr im Windows-Betriebssystem enthalten (weitere Details finden Sie unter Windows 8 und [Windows Server 2012 Compatibility Cookbook](https://www.microsoft.com/download/details.aspx?id=27416) ). RDS-Client Komponenten werden in einer zukünftigen Version von Windows entfernt. Nutzen Sie diese Funktionen bei Neuentwicklungen nicht mehr, und planen Sie die Änderung von Anwendungen, die diese Funktion zurzeit verwenden. Anwendungen, die RDS verwenden, sollten zu [WCF Data Service](/dotnet/framework/wcf/)migriert werden.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
DataControl.Refresh  
```  
  
#### <a name="parameters"></a>Parameter  
 *DataControl*  
 Eine Objekt Variable, die einen [RDS darstellt. DataControl](./datacontrol-object-rds.md) -Objekt.  
  
## <a name="remarks"></a>Bemerkungen  
 Sie müssen die Eigenschaften " [Connect](./connect-property-rds.md)", " [Server](./server-property-rds.md)" und " [SQL](./sql-property.md) " festlegen, bevor Sie die **Refresh** -Methode verwenden. Alle Daten gebundenen Steuerelemente auf dem Formular, das einem **RDS zugeordnet ist. Das DataControl** -Objekt reflektiert den neuen Satz von Datensätzen. Alle bereits vorhandenen [Recordset](../ado-api/recordset-object-ado.md) -Objekte werden freigegeben, und alle nicht gespeicherten Änderungen werden verworfen. Die **Aktualisierungs Methode erstellt** automatisch den ersten Datensatz zum aktuellen Datensatz.  
  
 Es empfiehlt sich, die **Aktualisierungs** Methode in regelmäßigen Abständen aufzurufen, wenn Sie mit Daten arbeiten. Wenn Sie Daten abrufen und Sie dann eine Weile auf einem Client Computer belassen, ist Sie wahrscheinlich veraltet. Es ist möglich, dass alle Änderungen, die Sie vornehmen, fehlschlagen, weil ein anderer Benutzer den Datensatz geändert und Änderungen übermittelt hat.  
  
## <a name="applies-to"></a>Gilt für  
 [DataControl-Objekt (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Beispiel für Aktualisierungs Methode (VB)](../ado-api/refresh-method-example-vb.md)   
 [Aktualisierungs Methode (Beispiel) (VBScript)](./refresh-method-example-vbscript.md)   
 [Adressbuch-Befehls Schaltflächen](../../guide/remote-data-service/address-book-command-buttons.md)   
 [CancelUpdate-Methode (RDS)](./cancelupdate-method-rds.md)   
 [Refresh-Methode (ADO)](../ado-api/refresh-method-ado.md)   
 [SubmitChanges-Methode (RDS)](./submitchanges-method-rds.md)