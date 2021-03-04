---
title: ISSDataClassification | Microsoft-Dokumentation
description: ISSDataClassification-Schnittstelle
ms.custom: ''
ms.date: 09/30/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: reference
apiname:
- ISSDataClassification
apitype: COM
helpviewer_keywords:
- ISSDataClassification interface
author: bazizi
ms.author: v-beaziz
ms.openlocfilehash: e799a27babfe8e7b920b7dcb1c1d8a9be8f4fb27
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837347"
---
# <a name="issdataclassification"></a>ISSDataClassification
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW](../../../includes/applies-to-version/sql-asdb-asa.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Über die **ISSDataClassification**-Schnittstelle wird die Funktionalität zum Abrufen von Vertraulichkeitsklassifizierungsdaten für das aktive Rowset bereitgestellt, die von SQL Server empfangen werden.
  

## <a name="methods"></a>Methoden

|Methode|Beschreibung|  
|------------|-----------------|  
|[ISSDataClassification::GetSensitivityClassification](../../oledb/ole-db-interfaces/issdataclassification-getsensitivityclassification-ole-db.md)|Gibt einen Zeiger auf eine SENSITIVITYCLASSIFICATION-Struktur zurück, die Vertraulichkeitsklassifizierungsinformationen enthält.|  

## <a name="see-also"></a>Weitere Informationen  
 [Schnittstellen &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/oledb-driver-for-sql-server-ole-db-interfaces.md)   
 [Rowsets](../ole-db-rowsets/rowsets.md)   
 [Verwenden der Datenklassifizierung](../features/using-data-classification.md)
