---
description: 'ISSAsynchStatus:: Abort (Native Client OLE DB-Anbieter)'
title: 'ISSAsynchStatus:: Abort (Native Client OLE DB-Anbieter) | Microsoft-Dokumentation'
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- ISSAsynchStatus::Abort (OLE DB)
apitype: COM
helpviewer_keywords:
- Abort method
ms.assetid: 2a4bd312-839a-45a8-a299-fc8609be9a2a
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 39453e5f06b2702b171d639cc6989dd1d9aaf3b4
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97469391"
---
# <a name="issasynchstatusabort-native-client-ole-db-provider"></a>ISSAsynchStatus:: Abort (Native Client OLE DB-Anbieter)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Bricht einen asynchron ausgeführten Vorgang ab.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
HRESULT Abort(  
        HCHAPTER hChapter,  
        DBASYNCHOP eOperation);  
```  
  
## <a name="arguments"></a>Argumente  
 *hChapter*[in]  
 Das Handle des Kapitels, für das der Vorgang abgebrochen werden soll. Wenn das aufgerufene Objekt kein Rowsetobjekt ist oder der Vorgang nicht für ein Kapitel gilt, muss der Aufrufer *hChapter* auf DB_NULL_HCHAPTER festlegen.  
  
 *eOperation*[in]  
 Der Vorgang, der abgebrochen werden soll. Dieser sollte den folgenden Wert haben:  
  
 DBASYNCHOP_OPEN – Die Anforderung zum Abbruch gilt für das asynchrone Öffnen oder Auffüllen eines Rowsets oder für die asynchrone Initialisierung eines Datenquellobjekts.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 S_OK  
 Die Anforderung zum Abbrechen des asynchronen Vorgangs wurde verarbeitet. Dies ist keine Garantie, dass der Vorgang selbst abgebrochen wurde. Um festzustellen, ob der Vorgang abgebrochen wurde, sollte der Consumer [ISSAsynchStatus::GetStatus](../../relational-databases/native-client-ole-db-interfaces/issasynchstatus-getstatus-ole-db.md) aufrufen und auf den Wert DB_E_CANCELED prüfen. Dieser Wert wird unter Umständen jedoch nicht gleich im nächsten Aufruf zurückgegeben.  
  
 DB_E_CANTCANCEL  
 Der asynchrone Vorgang kann nicht abgebrochen werden.  
  
 DB_E_CANCELED  
 Die Anforderung zum Abbrechen des asynchronen Vorgangs wurde während der Benachrichtigungen abgebrochen. Der Vorgang wird immer noch asynchron ausgeführt.  
  
 E_FAIL  
 Es ist ein anbieterspezifischer Fehler aufgetreten.  
  
 E_INVALIDARG  
 Der *hChapter* -Parameter ist nicht DB_NULL_HCHAPTER, oder *eOperation* ist nicht DBASYNCH_OPEN.  
  
 E_UNEXPECTED  
 **ISSAsynchStatus::Abort** wurde für ein Datenquellobjekt aufgerufen, für das **IDBInitialize::Initialize** nicht aufgerufen oder nicht abgeschlossen wurde.  
  
 **ISSAsynchStatus::Abort** wurde für ein Datenquellobjekt aufgerufen, für das **IDBInitialize::Initialize** aufgerufen wurde, das jedoch vor der Initialisierung abgebrochen wurde oder für das ein Timeout eingetreten ist. Das Datenquellobjekt wurde noch nicht initialisiert.  
  
 **ISSAsynchStatus::Abort** wurde für ein Rowset aufgerufen, für das zuvor **ITransaction::Commit** oder **ITransaction::Abort** aufgerufen wurde, und das Rowset blieb nach dem Commit oder Abbruch nicht bestehen und befindet sich in einem Zombiezustand.  
  
 **ISSAsynchStatus::Abort** wurde für ein Rowset aufgerufen, das in seiner Initialisierungsphase asynchron abgebrochen wurde. Das Rowset befindet sich in einem Zombiezustand.  
  
## <a name="remarks"></a>Bemerkungen  
 Durch Abbrechen der Initialisierung eines Rowsets oder eines Datenquellobjekts wird das Rowset bzw. das Datenquellobjekt möglicherweise in einen Zombiezustand versetzt, sodass für alle Methoden außer **IUnknown** -Methoden E_UNEXPECTED zurückgegeben wird. In diesem Fall hat der Consumer nur die Möglichkeit, das Rowset oder das Datenquellobjekt freizugeben.  
  
 Durch Aufrufen von **ISSAsynchStatus::Abort** und Übergeben eines anderen Werts für *eOperation* als DBASYNCHOP_OPEN wird S_OK zurückgegeben. Dies bedeutet nicht, dass der Vorgang abgeschlossen oder abgebrochen wurde.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Ausführen asynchroner Vorgänge](../../relational-databases/native-client/features/performing-asynchronous-operations.md)  
  
  
