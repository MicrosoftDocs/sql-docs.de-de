---
description: ADCPROP_UPDATECRITERIA_ENUM
title: ADCPROP_UPDATECRITERIA_ENUM | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- ADCPROP_UPDATECRITERIA_ENUM
helpviewer_keywords:
- ADCPROP_UPDATECRITERIA_ENUM [ADO]
ms.assetid: 33fd7b65-2ec8-4f62-91a7-630b5dab1aa2
author: rothja
ms.author: jroth
ms.openlocfilehash: 2b0e2a308d9a83c936abe8f5f39b29dac86795ec
ms.sourcegitcommit: c4d564435c008e2c92035efd2658172f20f07b2b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/24/2020
ms.locfileid: "88760254"
---
# <a name="adcprop_updatecriteria_enum"></a>ADCPROP_UPDATECRITERIA_ENUM
Gibt an, welche Felder zum Erkennen von Konflikten während einer vollständigen Aktualisierung einer Zeile der Datenquelle mit einem [Recordset](./recordset-object-ado.md) -Objekt verwendet werden können.  
  
 Verwenden Sie diese Konstanten mit **der dynamischen** %**Update Criteria**% amp; quot;%% amp; quot;%% amp; quot;, auf die im [ADO Dynamic Property Index](./ado-dynamic-property-index.md) verwiesen wird und die im [Microsoft Cursor-OLE DB Dienst](../../guide/appendixes/microsoft-cursor-service-for-ole-db-ado-service-component.md)  
  
|Konstant|Wert|BESCHREIBUNG|  
|--------------|-----------|-----------------|  
|**adcriteria**|1|Erkennt Konflikte, wenn eine Spalte der Datenquellen Zeile geändert wurde.|  
|**adkriteriakey**|0|Erkennt Konflikte, wenn die Schlüssel Spalte der Datenquellen Zeile geändert wurde, was bedeutet, dass die Zeile gelöscht wurde.|  
|**adcriteria-Stempel**|3|Erkennt Konflikte, wenn der Zeitstempel der Datenquellen Zeile geändert wurde, was bedeutet, dass auf die Zeile zugegriffen wurde, nachdem das **Recordset** abgerufen wurde.|  
|**adkriteriaupdcols**|2|Erkennt Konflikte, wenn eine der Spalten der Datenquellen Zeile, die aktualisierten Feldern des **Recordsets** entsprechen, geändert wurde.|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC-Entsprechung  
 Paket: **com. ms. wfc. Data**  
  
|Konstant|  
|--------------|  
|Adoesums. adcpropupdatecriteria. allcols|  
|Adoesums. adcpropupdatecriteria. Key|  
|Adoesums. adcpropupdatecriteria. timestamp|  
|Adoesums. adcpropupdatecriteria. updcols|