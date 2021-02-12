---
description: FilterGroupEnum
title: Filtergroupum | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- FilterGroupEnum
helpviewer_keywords:
- FilterGroupEnum enumeration [ADO]
ms.assetid: b22e725e-84bd-4286-a070-290c278c3783
author: rothja
ms.author: jroth
ms.openlocfilehash: 27b8b217287f3c623fc626de1cb46dcc2eed5394
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100024596"
---
# <a name="filtergroupenum"></a>FilterGroupEnum
Gibt die Gruppe von Datensätzen an, die von einem [Recordset](./recordset-object-ado.md)gefiltert werden sollen.  
  
|Konstante|Wert|BESCHREIBUNG|  
|--------------|-----------|-----------------|  
|**adFilterAffectedRecords**|2|Filter zum Anzeigen nur der Datensätze, die vom letzten [Delete](./delete-method-ado-recordset.md)-, [Resync](./resync-method.md)-, [Update Batch](./updatebatch-method.md)-oder [CancelBatch](./cancelbatch-method-ado.md) -Befehl betroffen sind.|  
|**adFilterConflictingRecords**|5|Filter zum Anzeigen der Datensätze, bei denen das letzte Batch Update nicht erfolgreich war.|  
|**adfilterfetchedrecords**|3|Filter zum Anzeigen der Datensätze im aktuellen Cache, d. h. die Ergebnisse des letzten Aufrufes zum Abrufen von Datensätzen aus der Datenbank.|  
|**adfilternone**|0|Entfernt den aktuellen Filter und stellt alle Datensätze für die Anzeige wieder her.|  
|**adfilterpdingrecords**|1|Filter zum Anzeigen nur von Datensätzen, die geändert wurden, aber noch nicht an den Server gesendet wurden. Gilt nur für den Batch Aktualisierungs Modus.|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC-Entsprechung  
 Paket: **com. ms. wfc. Data**  
  
|Konstante|  
|--------------|  
|AdoEnums. filtergroup. affectedrecords|  
|AdoEnums. filtergroup. conflictingrecords|  
|AdoEnums. filtergroup. fetchedrecords|  
|AdoEnums. filtergroup. None|  
|AdoEnums. filtergroup. pdingrecords|  
  
## <a name="applies-to"></a>Gilt für  
 [Filter-Eigenschaft](./filter-property.md)