---
title: Abrufen von großen Datenmengen (OLE DB-Treiber) | Microsoft-Dokumentation
description: Erfahren Sie in diesem Beispiel, wie ein Consumer des OLE DB-Treibers für SQL Server einen großen Datenwert aus einer einzelnen Spalte abrufen kann.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- BLOBs, OLE objects
- DBPROP_ACCESSORDER property
- OLE DB Driver for SQL Server, BLOBs
- large data, OLE objects
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 547f5afc56d0a2cffef0c73c716721fc2dfa9183
ms.sourcegitcommit: c95f3ef5734dec753de09e07752a5d15884125e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2020
ms.locfileid: "88862306"
---
# <a name="getting-large-data"></a>Abrufen großer Datenmengen
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Im Allgemeinen sollten Consumer Code, der ein Speicherobjekt des OLE DB-Treibers für SQL Server erzeugt, von anderem Code isolieren, der Daten verarbeitet, die nicht durch einen **ISequentialStream**-Schnittstellenzeiger referenziert sind.  
  
 In diesem Artikel wird die mit folgenden Funktionen verfügbare Funktionalität behandelt:  
  
-   IRowset:GetData  
  
-   IRow::GetColumns  
  
-   ICommand::Execute  
  
 Wenn die Eigenschaft DBPROP_ACCESSORDER (in der Rowset-Eigenschaftengruppe) entweder auf DBPROPVAL_AO_SEQUENTIAL oder auf DBPROPVAL_AO_SEQUENTIALSTORAGEOBJECTS festgelegt ist, sollte der Consumer in einem Aufruf der **GetNextRows**-Methode nur eine einzige Datenzeile abrufen. Das liegt daran, dass Blob-Daten nicht gepuffert werden. Ist der Wert von DBPROP_ACCESSORDER auf DBPROPVAL_AO_RANDOM festgelegt, kann der Consumer mehrere Datenzeilen mit **GetNextRows** abrufen.  
  
 Der OLE DB-Treiber für SQL Server ruft erst dann große Datenmengen von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ab, wenn er vom Consumer dazu aufgefordert wird. Der Consumer sollte alle kleinen Datenmengen in einem Accessor zusammenfassen und dann einen oder mehrere Accessoren zum Abrufen großer Datenwerte verwenden.  
  
## <a name="example"></a>Beispiel  
 In diesem Beispiel wird ein großer Datenwert aus einer einzelnen Spalte abgerufen:  
  
```  
HRESULT GetUnboundData  
    (  
    IRowset* pIRowset,  
    HROW hRow,  
    ULONG nCol,   
    BYTE* pUnboundData  
    )  
    {  
    UINT                cbRow = sizeof(IUnknown*) + sizeof(ULONG);  
    BYTE*               pRow = new BYTE[cbRow];  
  
    DBOBJECT            dbobject;  
  
    IAccessor*          pIAccessor = NULL;  
    HACCESSOR           haccessor;  
  
    DBBINDING           dbbinding;  
    ULONG               ulbindstatus;  
  
    ULONG               dwStatus;  
    ISequentialStream*  pISequentialStream;  
    ULONG               cbRead;  
  
    HRESULT             hr;  
  
    // Set up the DBOBJECT structure.  
    dbobject.dwFlags = STGM_READ;  
    dbobject.iid = IID_ISequentialStream;  
  
    // Create the DBBINDING, requesting a storage-object pointer from  
    // The OLE DB Driver for SQL Server.  
    dbbinding.iOrdinal = nCol;  
    dbbinding.obValue = 0;  
    dbbinding.obStatus = sizeof(IUnknown*);  
    dbbinding.obLength = 0;  
    dbbinding.pTypeInfo = NULL;  
    dbbinding.pObject = &dbobject;  
    dbbinding.pBindExt = NULL;  
    dbbinding.dwPart = DBPART_VALUE | DBPART_STATUS;  
    dbbinding.dwMemOwner = DBMEMOWNER_CLIENTOWNED;  
    dbbinding.eParamIO = DBPARAMIO_NOTPARAM;  
    dbbinding.cbMaxLen = 0;  
    dbbinding.dwFlags = 0;  
    dbbinding.wType = DBTYPE_IUNKNOWN;  
    dbbinding.bPrecision = 0;  
    dbbinding.bScale = 0;  
  
    if (FAILED(hr = pIRowset->  
        QueryInterface(IID_IAccessor, (void**) &pIAccessor)))  
        {  
        // Process QueryInterface failure.  
        return (hr);  
        }  
  
    // Create the accessor.  
    if (FAILED(hr = pIAccessor->CreateAccessor(DBACCESSOR_ROWDATA, 1,  
        &dbbinding, 0, &haccessor, &ulbindstatus)))  
        {  
        // Process error from CreateAccessor.  
        pIAccessor->Release();  
        return (hr);  
        }  
  
    // Read and process BLOCK_SIZE bytes at a time.  
    if (SUCCEEDED(hr = pIRowset->GetData(hRow, haccessor, pRow)))  
        {  
        dwStatus = *((ULONG*) (pRow + dbbinding.obStatus));  
  
        if (dwStatus == DBSTATUS_S_ISNULL)  
            {  
            // Process NULL data  
            }  
        else if (dwStatus == DBSTATUS_S_OK)  
            {  
            pISequentialStream = *((ISequentialStream**)   
                (pRow + dbbinding.obValue));  
  
            do  
                {  
                if (SUCCEEDED(hr =  
                    pISequentialStream->Read(pUnboundData,  
                    BLOCK_SIZE, &cbRead)))  
                    {  
                    pUnboundData += cbRead;  
                    }  
                }  
            while (SUCCEEDED(hr) && cbRead >= BLOCK_SIZE);  
  
            pISequentialStream->Release();  
            }  
        }  
    else  
        {  
        // Process error from GetData.  
        }  
  
    pIAccessor->ReleaseAccessor(haccessor, NULL);  
    pIAccessor->Release();  
    delete [] pRow;  
  
    return (hr);  
    }  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [BLOBs und OLE-Objekte](../../oledb/ole-db-blobs/blobs-and-ole-objects.md)   
 [Verwenden von Datentypen mit umfangreichen Werten](../../oledb/features/using-large-value-types.md)  
  
  
