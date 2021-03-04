---
title: ISSDataClassification::GetSensitivityClassification | Microsoft-Dokumentation
description: ISSDataClassification::GetSensitivityClassification
ms.custom: ''
ms.date: 09/30/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: reference
apiname:
- ISSDataClassification::GetSensitivityClassification
apitype: COM
helpviewer_keywords:
- GetSensitivityClassification method
author: bazizi
ms.author: v-beaziz
ms.openlocfilehash: 232bcf6187967e9dab7b09414626785d7808e18f
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837307"
---
# <a name="issdataclassificationgetsensitivityclassification"></a>ISSDataClassification::GetSensitivityClassification
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW](../../../includes/applies-to-version/sql-asdb-asa.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Ruft Daten zur Vertraulichkeitsklassifizierung für das aktive Rowset ab. Weitere Informationen und ein Codebeispiel finden Sie unter [Verwenden der Datenklassifizierung](../features/using-data-classification.md).  
  
## <a name="syntax"></a>Syntax  
  
```  
HRESULT GetSensitivityClassification(
    SENSITIVITYCLASSIFICATION** ppSensitivityClassification);
```  
  
## <a name="arguments"></a>Argumente  
  *ppSensitivityClassification*[out]  
 Zeiger auf einen SENSITIVITYCLASSIFICATION-Strukturzeiger. Wenn bei der Methode ein Fehler auftritt oder keine Informationen zur Datenklassifizierung verfügbar sind, weist der Anbieter keinen Arbeitsspeicher zu. Außerdem stellt er sicher, dass das Argument „ppSensitivityClassification“ bei der Ausgabe ein NULL-Zeiger ist.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 S_OK  
 Die Methode wurde erfolgreich ausgeführt.    
  
 E_INVALIDARG  
 Das Argument *ppSensitivityClassification* war NULL.  
  
 E_OUTOFMEMORY  
 Der OLE DB-Treiber für SQL Server konnte nicht genügend Arbeitsspeicher belegen, um die Anforderung abzuschließen.  

  
## <a name="remarks"></a>Bemerkungen  
Der OLE DB-Treiber für SQL Server belegt einen Arbeitsspeicherblock für die SENSITIVITYCLASSIFICATION-Struktur und die Daten, auf die diese Struktur verweist. Wenn der Consumer keinen Zugriff mehr auf die Klassifizierungsdaten benötigt, muss er die Zuordnung dieses Arbeitsspeichers durch einen Aufruf der Methode [IMalloc::Free](/windows/win32/api/objidl/nf-objidl-imalloc-free) aufheben.  
  
 Die SENSITIVITYCLASSIFICATION-Struktur wird wie folgt definiert:
  
```cpp
typedef struct tagSensitivityClassification
{
    USHORT                     cSensitivityLabels;
    SENSITIVITYLABEL          *rgSensitivityLabels;
    USHORT                     cInformationTypes;
    INFORMATIONTYPE           *rgInformationTypes;
    USHORT                     cColumnSensitivityMetadata;
    COLUMNSENSITIVITYMETADATA *rgColumnSensitivityMetadata;
    SENSITIVITYRANKENUM        eQuerySensitivityRank;
} SENSITIVITYCLASSIFICATION;
```  

|Member|Beschreibung|  
|------------|-----------------|  
|*cSensitivityLabels*|Die Anzahl von SENSITIVITYLABEL-Strukturen in *rgSensitivityLabels*.|  
|*rgSensitivityLabels*|Ein Array aus SENSITIVITYLABEL-Strukturen.|  
|*cInformationTypes*|Die Anzahl von INFORMATIONTYPE-Strukturen in *rgInformationTypes*.|  
|*rgInformationTypes*|Ein Array aus INFORMATIONTYPE-Strukturen.|  
|*cColumnSensitivityMetadata*|Die Anzahl von COLUMNSENSITIVITYMETADATA-Strukturen in *rgColumnSensitivityMetadata*.|  
|*rgColumnSensitivityMetadata*|Ein Array aus COLUMNSENSITIVITYMETADATA-Strukturen.|  
|*eQuerySensitivityRank*|Eine relative Rangfolge der Vertraulichkeit einer Abfrage, die zum Abrufen des Rowsets ausgeführt wurde.|  

Die SENSITIVITYLABEL-Struktur wird wie folgt definiert:
```cpp
typedef struct tagSENSITIVITYLABEL
{
    LPOLESTR pwszName;
    LPOLESTR pwszId;
} SENSITIVITYLABEL;
```

|Member|Beschreibung|  
|------------|-----------------|  
|*pwszName*|Der Name für eine Vertraulichkeitsbezeichnung.|  
|*pwszId*|Der Bezeichner für eine Vertraulichkeitsbezeichnung.|  

Die INFORMATIONTYPE-Struktur wird wie folgt definiert:
```cpp
typedef struct tagINFORMATIONTYPE
{
    LPOLESTR pwszName;
    LPOLESTR pwszId;
} INFORMATIONTYPE;
```

|Member|Beschreibung|  
|------------|-----------------|  
|*pwszName*|Der Name für einen Informationstyp.|  
|*pwszId*|Der Bezeichner für einen Informationstyp.|  

Die COLUMNSENSITIVITYMETADATA-Struktur wird wie folgt definiert:
```cpp
typedef struct tagCOLUMNSENSITIVITYMETADATA
{
    SENSITIVITYPROPERTY* rgSensitivityProperties;
    USHORT cSensitivityProperties;
} COLUMNSENSITIVITYMETADATA;
```

|Member|Beschreibung|  
|------------|-----------------|  
|*cSensitivityProperties*|Die Anzahl von SENSITIVITYPROPERTY-Strukturen in *rgSensitivityProperties*.|  
|*rgSensitivityProperties*|Ein Array aus SENSITIVITYPROPERTY-Strukturen.|  

Die SENSITIVITYRANKENUM-Enumeration wird wie folgt definiert:
```cpp
typedef enum tagSENSITIVITYRANKENUM
{
    SENSITIVITYRANK_NOT_DEFINED = -1,
    SENSITIVITYRANK_NONE = 0,
    SENSITIVITYRANK_LOW = 10,
    SENSITIVITYRANK_MEDIUM = 20,
    SENSITIVITYRANK_HIGH = 30,
    SENSITIVITYRANK_CRITICAL = 40
} SENSITIVITYRANKENUM;
```

Die SENSITIVITYPROPERTY-Struktur wird wie folgt definiert:
```cpp
typedef struct tagSENSITIVITYPROPERTY
{
    SENSITIVITYLABEL* pSensitivityLabel;
    INFORMATIONTYPE* pInformationType;
    SENSITIVITYRANKENUM eSensitivityRank;
} SENSITIVITYPROPERTY;
```

|Member|Beschreibung|  
|------------|-----------------|  
|*pSensitivityLabel*|Ein Zeiger auf eine SENSITIVITYLABEL-Struktur.|  
|*pInformationType*|Ein Zeiger auf eine INFORMATIONTYPE-Struktur.|  
|*eSensitivityRank*|Eine relative Rangfolge der Vertraulichkeit einer Spalte, die Teil von spaltenbasierten Daten ist.|  

## <a name="see-also"></a>Weitere Informationen  
 [ISSDataClassification](../../oledb/ole-db-interfaces/issdataclassification-ole-db.md)  
 [Rowsets](../ole-db-rowsets/rowsets.md)  
