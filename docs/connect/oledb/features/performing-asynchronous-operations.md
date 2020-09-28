---
title: Ausführen asynchroner Vorgänge | Microsoft-Dokumentation
description: Der OLE DB-Treiber für SQL Server unterstützt asynchrone Datenbankvorgänge. Auf diese Weise können Methoden zurückgegeben werden, ohne den aufrufenden Thread zu blockieren.
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- initialization [OLE DB Driver for SQL Server]
- database connections [OLE DB Driver for SQL Server]
- data access [OLE DB Driver for SQL Server], asynchronous operations
- connections [OLE DB Driver for SQL Server]
- asynchronous operations [OLE DB Driver for SQL Server]
- rowsets [SQL Server], initializing
- MSOLEDBSQL, asynchronous operations
- OLE DB Driver for SQL Server, asynchronous operations
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 9b9005314e248f5aff1f377944395143d67363fd
ms.sourcegitcommit: c95f3ef5734dec753de09e07752a5d15884125e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2020
ms.locfileid: "88861337"
---
# <a name="performing-asynchronous-operations"></a>Ausführen asynchroner Vorgänge
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ermöglicht es Anwendungen, asynchrone Datenbankvorgänge auszuführen. Die asynchrone Verarbeitung ermöglicht es Methoden, Rückgaben unverzüglich zu übermitteln, ohne den aufrufenden Thread zu blockieren. Dadurch kann ein Großteil der Leistungsfähigkeit und Flexibilität des Multithreadings genutzt werden, ohne dass Entwickler Threads explizit erstellen oder die Synchronisation durchführen müssen. Anwendungen fordern die asynchrone Verarbeitung an, wenn sie eine Datenbankverbindung oder das Ergebnis einer Befehlsausführung initialisieren.  
  
## <a name="opening-and-closing-a-database-connection"></a>Öffnen und Schließen einer Datenbankverbindung  
 Wenn der OLE DB-Treiber für SQL Server verwendet wird, können Anwendungen, die für die asynchrone Initialisierung eines Datenquellenobjekts ausgelegt sind, das DBPROPVAL_ASYNCH_INITIALIZE-Bit in der DBPROP_INIT_ASYNCH-Eigenschaft vor dem Aufrufen von **IDBInitialize::Initialize** festlegen. Wenn diese Eigenschaft festgelegt ist, antwortet der Anbieter unverzüglich auf den Aufruf von **Initialize** mit S_OK, wenn der Vorgang sofort ausgeführt wird, oder mit DB_S_ASYNCHRONOUS, wenn die Initialisierung asynchron fortgesetzt wird. Anwendungen können die **IDBAsynchStatus**-Schnittstelle oder die [ISSAsynchStatus](../../oledb/ole-db-interfaces/issasynchstatus-ole-db.md)-Schnittstelle des Datenquellenobjekts abfragen und dann **IDBAsynchStatus::GetStatus** oder [ISSAsynchStatus::WaitForAsynchCompletion](../../oledb/ole-db-interfaces/issasynchstatus-waitforasynchcompletion-ole-db.md) aufrufen, um den Status der Initialisierung zu ermitteln.  
  
 Außerdem wurde dem DBPROPSET_SQLSERVERROWSET-Eigenschaftensatz die SSPROP_ISSAsynchStatus-Eigenschaft hinzugefügt. Anbieter, die die **ISSAsynchStatus**-Schnittstelle unterstützen, müssen diese Eigenschaft mit dem Wert VARIANT_TRUE implementieren.  
  
 **IDBAsynchStatus::Abort** oder [ISSAsynchStatus::Abort](../../oledb/ole-db-interfaces/issasynchstatus-abort-ole-db.md) kann aufgerufen werden, um den asynchronen Aufruf von **Initialize** abzubrechen. Der Consumer muss die asynchrone Datenquellinitialisierung explizit anfordern. Andernfalls erfolgt die Rückgabe durch **IDBInitialize::Initialize** erst, wenn das Datenquellenobjekt vollständig initialisiert wurde.  
  
> [!NOTE]  
>  Datenquellenobjekte, die für das Verbindungspooling verwendet werden, können die **ISSAsynchStatus**-Schnittstelle im OLE DB-Treiber für SQL Server aufrufen. Die **ISSAsynchStatus**-Schnittstelle wird nicht für Datenquellenobjekte aus dem Pool verfügbar gemacht.  
>   
>  Wenn eine Anwendung die Verwendung der Cursor-Engine explizit erzwingt, unterstützen **IOpenRowset::OpenRowset** und **IMultipleResults::GetResult** keine asynchrone Verarbeitung.  
>   
>  Zudem kann die DLL des Remoteproxys/-stubs (in MDAC 2.8) die **ISSAsynchStatus**-Schnittstelle im OLE DB-Treiber für SQL Server nicht aufrufen. Die **ISSAsynchStatus**-Schnittstelle wird durch Remoting nicht verfügbar gemacht.  
>   
>  Dienstkomponenten unterstützen **ISSAsynchStatus** nicht.  
  
## <a name="execution-and-rowset-initialization"></a>Ausführung und Rowsetinitialisierung  
 Anwendungen, die dafür ausgelegt sind, das Ergebnis einer Befehlsausführung asynchron zu öffnen, können das DBPROPVAL_ASYNCH_INITIALIZE-Bit in der DBPROP_ROWSET_ASYNCH-Eigenschaft festlegen. Wenn dieses Bit vor dem Aufrufen von **IDBInitialize::Initialize**, **ICommand::Execute**, **IOpenRowset::OpenRowset** oder **IMultipleResults::GetResult** festgelegt wird, muss das Argument *riid* auf IID_IDBAsynchStatus, IID_ISSAsynchStatus oder IID_Iunknown festgelegt werden.  
  
 Die Methode gibt unverzüglich S_OK zurück, wenn die Rowsetinitialisierung sofort ausgeführt wird, oder DB_S_ASYNCHRONOUS, wenn die Rowsetinitialisierung asynchron fortgesetzt wird, wobei *ppRowset* im Rowset auf die angeforderte Schnittstelle festgelegt ist. Für den OLE DB-Treiber für SQL Server kann diese Schnittstelle nur auf **IDBAsynchStatus** oder **ISSAsynchStatus** festgelegt werden. Bis das Rowset vollständig initialisiert wurde, verhält sich diese Schnittstelle so, als befände sie sich im Zustand „Angehalten“. Der Aufruf von **QueryInterface** für andere Schnittstellen als **IID_IDBAsynchStatus** oder **IID_ISSAsynchStatus** führt möglicherweise zur Rückgabe von E_NOINTERFACE. Sofern der Consumer die asynchrone Verarbeitung nicht explizit anfordert, wird das Rowset synchron initialisiert. Alle angeforderten Schnittstellen sind verfügbar, wenn **IDBAsynchStaus::GetStatus** oder **ISSAsynchStatus::WaitForAsynchCompletion** die Rückgabe übermittelt, dass der asynchrone Vorgang abgeschlossen ist. Das bedeutet nicht notwendigerweise, dass das Rowset vollständig aufgefüllt ist, jedoch ist es komplett und voll funktionstüchtig.  
  
 Wenn der ausgeführte Befehl kein Rowset zurückgibt, so gibt er doch unverzüglich ein Objekt zurück, das **IDBAsynchStatus** unterstützt.  
  
 Wenn Sie mehrere Ergebnisse von der asynchronen Befehlsausführung benötigen, sollten Sie Folgendes tun:  
  
-   Legen Sie das DBPROPVAL_ASYNCH_INITIALIZE-Bit der DBPROP_ROWSET_ASYNCH-Eigenschaft vor dem Ausführen des Befehls fest.  
  
-   Rufen Sie **ICommand::Execute** auf, und fordern Sie **IMultipleResults** an.  
  
 Die **IDBAsynchStatus**-Schnittstelle und die **ISSAsynchStatus**-Schnittstelle können dann angefordert werden, indem die Schnittstelle für mehrere Ergebnisse mit **QueryInterface** abgefragt wird.  
  
 Wenn die Ausführung des Befehls abgeschlossen ist, kann **IMultipleResults** wie gewohnt verwendet werden. Es gibt jedoch eine Ausnahme für den synchronen Fall: Möglicherweise wird DB_S_ASYNCHRONOUS zurückgegeben; in diesem Fall kann mit **IDBAsynchStatus** oder **ISSAsynchStatus** ermittelt werden, wann der Vorgang abgeschlossen ist.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel ruft die Anwendung eine nicht blockierende Methode auf, führt andere Verarbeitungsvorgänge aus und kehrt dann zur Verarbeitung der Ergebnisse zurück. **ISSAsynchStatus::WaitForAsynchCompletion** wartet auf das interne Ereignisobjekt, bis der asynchrone Vorgang ausgeführt wurde oder die durch *dwMilisecTimeOut* angegebene Zeit verstrichen ist.  
  
```  
// Set the DBPROPVAL_ASYNCH_INITIALIZE bit in the   
// DBPROP_ROWSET_ASYNCH property before calling Execute().  
  
DBPROPSET CmdPropset[1];  
DBPROP CmdProperties[1];  
  
CmdPropset[0].rgProperties = CmdProperties;  
CmdPropset[0].cProperties = 1;  
CmdPropset[0].guidPropertySet = DBPROPSET_ROWSET;  
  
// Set asynch mode for command.  
CmdProperties[0].dwPropertyID = DBPROP_ROWSET_ASYNCH;  
CmdProperties[0].vValue.vt = VT_I4;  
CmdProperties[0].vValue.lVal = DBPROPVAL_ASYNCH_INITIALIZE;  
CmdProperties[0].dwOptions = DBPROPOPTIONS_REQUIRED;  
  
hr = pICommandProps->SetProperties(1, CmdPropset);  
  
hr = pICommand->Execute(  
   pUnkOuter,  
   IID_ISSAsynchStatus,  
   pParams,  
   pcRowsAffected,  
   (IUnknown**)&pISSAsynchStatus);  
  
if (hr == DB_S_ASYNCHRONOUS)  
{  
   // Do some work here...  
  
   hr = pISSAsynchStatus->WaitForAsynchCompletion(dwMilisecTimeOut);  
   if ( hr == S_OK)  
   {  
      hr = pISSAsynchStatus->QueryInterface(IID_IRowset, (void**)&pRowset);  
      pISSAsynchStatus->Release();  
   }  
}  
```  
  
 **ISSAsynchStatus::WaitForAsynchCompletion** wartet auf das interne Ereignisobjekt, bis der asynchrone Vorgang ausgeführt oder der *dwMilisecTimeOut*-Wert übergeben wurde.  
  
 Im folgenden Beispiel wird die asynchrone Verarbeitung mit mehreren Resultsets veranschaulicht:  
  
```  
DBPROP CmdProperties[1];  
  
// Set asynch mode for command.  
CmdProperties[0].dwPropertyID = DBPROP_ROWSET_ASYNCH;  
CmdProperties[0].vValue.vt = VT_I4;  
CmdProperties[0].vValue.lVal = DBPROPVAL_ASYNCH_INITIALIZE;  
  
hr = pICommand->Execute(  
   pUnkOuter,  
   IID_IMultipleResults,  
   pParams,  
   pcRowsAffected,  
   (IUnknown**)&pIMultipleResults);  
  
// Use GetResults for ISSAsynchStatus.  
hr = pIMultipleResults->GetResult(IID_ISSAsynchStatus, (void **) &pISSAsynchStatus);  
  
if (hr == DB_S_ASYNCHRONOUS)  
{  
   // Do some work here...  
  
   hr = pISSAsynchStatus->WaitForAsynchCompletion(dwMilisecTimeOut);  
   if (hr == S_OK)  
   {  
      hr = pISSAsynchStatus->QueryInterface(IID_IRowset, (void**)&pRowset);  
      pISSAsynchStatus->Release();  
   }  
}  
```  
  
 Der Client kann den Status eines asynchron ausgeführten Vorgangs überprüfen, um das Blockieren zu verhindern, wie im folgenden Codebeispiel gezeigt:  
  
```  
// Set the DBPROPVAL_ASYNCH_INITIALIZE bit in the   
// DBPROP_ROWSET_ASYNCH property before calling Execute().  
hr = pICommand->Execute(  
   pUnkOuter,  
   IID_ISSAsynchStatus,  
   pParams,  
   pcRowsAffected,  
   (IUnknown**)&pISSAsynchStatus);   
  
if (hr == DB_S_ASYNCHRONOUS)  
{  
   do{  
      // Do some work...  
      hr = pISSAsynchStatus->GetStatus(DB_NULL_HCHAPTER, DBASYNCHOP_OPEN, NULL, NULL, &ulAsynchPhase, NULL);  
   }while (DBASYNCHPHASE_COMPLETE != ulAsynchPhase)  
   if SUCCEEDED(hr)  
   {  
      hr = pISSAsynchStatus->QueryInterface(IID_IRowset, (void**)&pRowset);  
   }  
   pIDBAsynchStatus->Release();  
}  
```  
  
 Im folgenden Beispiel wird veranschaulicht, wie Sie die gerade ausgeführte asynchrone Operation abbrechen können:  
  
```  
// Set the DBPROPVAL_ASYNCH_INITIALIZE bit in the   
// DBPROP_ROWSET_ASYNCH property before calling Execute().  
hr = pICommand->Execute(  
   pUnkOuter,  
   IID_ISSAsynchStatus,  
   pParams,  
   pcRowsAffected,  
   (IUnknown**)&pISSAsynchStatus);  
  
if (hr == DB_S_ASYNCHRONOUS)  
{  
   // Do some work...  
   hr = pISSAsynchStatus->Abort(DB_NULL_HCHAPTER, DBASYNCHOP_OPEN);  
}  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [OLE DB-Treiber für SQL Server-Features](../../oledb/features/oledb-driver-for-sql-server-features.md)   
 [Eigenschaften und Verhaltensweisen von Rowsets](../../oledb/ole-db-rowsets/rowset-properties-and-behaviors.md)   
 [ISSAsynchStatus &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/issasynchstatus-ole-db.md)  
  
  
