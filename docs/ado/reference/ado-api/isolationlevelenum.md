---
description: IsolationLevelEnum
title: Isolationlevelumum | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- IsolationLevelEnum
helpviewer_keywords:
- IsolationLevelEnum enumeration [ADO]
ms.assetid: 8e17a7bc-b8a3-4ae2-b6c9-ce088ad31fdf
author: rothja
ms.author: jroth
ms.openlocfilehash: 7f01e97bea7d7e066d39898c7d76eb33ddbf710d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041910"
---
# <a name="isolationlevelenum"></a>IsolationLevelEnum
Gibt die Ebene der Transaktions Isolation für ein [Verbindungs](./connection-object-ado.md) Objekt an.  
  
|Konstante|Wert|BESCHREIBUNG|  
|--------------|-----------|-----------------|  
|**adxactunfest gelegt**|-1|Gibt an, dass der Anbieter eine andere Isolationsstufe als angegeben verwendet, jedoch nicht bestimmt werden kann.|  
|**adxactchaos**|16|Gibt an, dass ausstehende Änderungen von höher isolierten Transaktionen nicht überschrieben werden können.|  
|**adxactbrowse**|256|Gibt an, dass Sie von einer Transaktion in anderen Transaktionen Änderungen anzeigen können, für die kein Commit ausgeführt wurde.|  
|**adXactReadUncommitted**|256|Identisch mit **adxactbrowse**.|  
|**adxactcurrsorstability**|4096|Gibt an, dass Sie in einer Transaktion Änderungen in anderen Transaktionen erst anzeigen können, nachdem ein Commit ausgeführt wurde.|  
|**adXactReadCommitted**|4096|Identisch mit **adxactcurrsorstability**.|  
|**adXactRepeatableRead**|65536|Gibt an, dass Sie von einer Transaktion aus keine Änderungen sehen können, die in anderen Transaktionen vorgenommen wurden, aber dass durch die Anforderung neue **Recordset** -Objekte abgerufen werden können.|  
|**adxactisolated**|1048576|Gibt an, dass Transaktionen isoliert von anderen Transaktionen durchgeführt werden.|  
|**adxactserialisierbar**|1048576|Identisch mit **adxactisolated**.|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC-Entsprechung  
 Paket: **com. ms. wfc. Data**  
  
|Konstante|  
|--------------|  
|Adoumums. IsolationLevel. nicht angegeben|  
|Adoumums. IsolationLevel. Chaos|  
|Adoumums. IsolationLevel. Browse|  
|Adoumums. IsolationLevel. Read-Commit|  
|Adoumums. IsolationLevel. Cursor stability|  
|Adoumums. IsolationLevel. Read Commit|  
|Adoteums. IsolationLevel. RepeatableRead|  
|Adoumums. IsolationLevel. isoliert|  
|Adoerums. IsolationLevel. serialisierbar|  
  
## <a name="applies-to"></a>Gilt für  
 [IsolationLevel-Eigenschaft](./isolationlevel-property.md)