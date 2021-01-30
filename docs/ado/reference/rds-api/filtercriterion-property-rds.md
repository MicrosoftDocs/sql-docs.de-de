---
description: FilterCriterion-Eigenschaft (RDS)
title: Filterkriterium-Eigenschaft (RDS) | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- FilterCriterion property [RDS]
ms.assetid: 24eb03ba-ccfd-4353-b6af-03586b2da6fd
author: rothja
ms.author: jroth
ms.openlocfilehash: 2bb91a09167b10a7b5b1f969e2d0775f6ced31a4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168936"
---
# <a name="filtercriterion-property-rds"></a>FilterCriterion-Eigenschaft (RDS)
Gibt den Auswertungs Operator an, der im Filter Wert verwendet werden soll.  
  
> [!IMPORTANT]
>  Ab Windows 8 und Windows Server 2012 sind RDS-Server Komponenten nicht mehr im Windows-Betriebssystem enthalten (weitere Details finden Sie unter Windows 8 und [Windows Server 2012 Compatibility Cookbook](https://www.microsoft.com/download/details.aspx?id=27416) ). RDS-Client Komponenten werden in einer zukünftigen Version von Windows entfernt. Nutzen Sie diese Funktionen bei Neuentwicklungen nicht mehr, und planen Sie die Änderung von Anwendungen, die diese Funktion zurzeit verwenden. Anwendungen, die RDS verwenden, sollten zu [WCF Data Service](/dotnet/framework/wcf/)migriert werden.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
DataControl.FilterCriterion = String  
```  
  
#### <a name="parameters"></a>Parameter  
 *DataControl*  
 Eine Objekt Variable, die einen [RDS darstellt. DataControl](./datacontrol-object-rds.md) -Objekt.  
  
 *String*  
 Ein **Zeichen** folgen Wert, der den Auswertungs Operator des [FilterValue](./filtervalue-property-rds.md) zu den Datensätzen angibt. Kann eine der folgenden sein: <, \<=, > , >=, = oder <>.  
  
## <a name="remarks"></a>Bemerkungen  
 Die Eigenschaften [SortColumn](./sortcolumn-property-rds.md), [SortDirection](./sortdirection-property-rds.md), [FilterValue](./filtervalue-property-rds.md), **Filterkriterium** und [FilterColumn](./filtercolumn-property-rds.md) stellen Sortier-und Filterfunktionen für den Client seitigen Cache bereit. Die Sortierfunktion ordnet Datensätze nach Werten aus einer Spalte an. Die Filterfunktion zeigt eine Teilmenge der Datensätze basierend auf den Suchkriterien an, während das vollständige [Recordset](../ado-api/recordset-object-ado.md) im Cache beibehalten wird. Die [Reset](./reset-method-rds.md) -Methode führt die Kriterien aus und ersetzt das aktuelle **Recordset** durch ein Aktualisier bares **Recordset**.  
  
 Der Operator "! =" ist für **Filterkriterium** ungültig. Verwenden Sie stattdessen "<>".  
  
 Wenn die Filter-und Sortierungs Eigenschaften festgelegt sind und Sie die **Reset** -Methode aufzurufen, wird das Rowset zuerst gefiltert und dann sortiert. Bei aufsteigenden Sortierungen sind die NULL-Werte ganz oben. bei absteigenden Sortierungen befinden sich NULL-Werte unten (aufsteigend ist Standardverhalten).  
  
## <a name="applies-to"></a>Gilt für  
 [DataControl-Objekt (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Filter Column, Filterkriterium, FilterValue, SortColumn und SortDirection Properties und Reset Method example (VBScript)](./filter-column-criterion-value-sortcolumn-sortdirection-example-vbscript.md)   
 [FilterColumn-Eigenschaft (RDS)](./filtercolumn-property-rds.md)   
 [FilterValue-Eigenschaft (RDS)](./filtervalue-property-rds.md)   
 [SortColumn-Eigenschaft (RDS)](./sortcolumn-property-rds.md)   
 [SortDirection-Eigenschaft (RDS)](./sortdirection-property-rds.md)