---
title: IBCPSession::BCPReadFmt (OLE DB-Treiber) | Microsoft-Dokumentation
description: Die BCPReadFmt-Methode im OLE DB-Treiber für SQL Server liest Daten aus einer Formatdatei, die das Format der Daten in der Datendatei angibt.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- IBCPSession::BCPReadFmt (OLE DB)
apitype: COM
helpviewer_keywords:
- BCPReadFmt method
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c795ba5954db328a12772c511310d453cde5dde1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191859"
---
# <a name="ibcpsessionbcpreadfmt-ole-db"></a>'IBCPSession::BCPReadFmt' (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Liest für jede Spalte Formatinformationen aus der Formatdatei.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
HRESULT BCPReadFmt(   
      const wchar_t *pwszFormatFile);  
```  
  
## <a name="remarks"></a>Bemerkungen  
 Die **BCPReadFmt** -Methode wird verwendet, um Daten aus einer Formatdatei zu lesen, die das Format der Daten in der Datendatei angibt. Diese Methode kann die korrekte Version der Formatdatei ermitteln. Sie kann automatisch erkennen, ob die Formatdatei im XML-Format oder dem alten Textformat abgefasst ist und sich entsprechend verhält. Die Formatdateiversionen, die vom Hilfsprogramm BCP des OLE DB-Treibers für SQL Server unterstützt werden, sind Version 6.0 oder neuer.  
  
 Nachdem die **BCPReadFmt**-Methode die Formatwerte gelesen hat, nimmt sie geeignete Aufrufe der Methoden [IBCPSession::BCPColumns](../../oledb/ole-db-interfaces/ibcpsession-bcpcolumns-ole-db.md) und [IBCPSession::BCPColFmt](../../oledb/ole-db-interfaces/ibcpsession-bcpcolfmt-ole-db.md) vor. Der Benutzer muss eine Formatdatei nicht analysieren, um diese Aufrufe zu tätigen.  
  
 Rufen Sie zum Speichern einer Formatdatei die [IBCPSession::BCPWriteFmt](../../oledb/ole-db-interfaces/ibcpsession-bcpwritefmt-ole-db.md)-Methode auf. Aufrufe der **BCPReadFmt** -Methode können auf gespeicherte Formate verweisen. Alternativ dazu kann das Hilfsprogramm zum Massenkopieren (**bcp**) benutzerdefinierte Datenformate in Dateien speichern, auf die mit der **BCPReadFmt** -Methode verwiesen werden kann.  
  
 Der **BCP_OPTION_DELAYREADFMT**-Wert des Parameters *eOption* von [IBCPSession::BCPControl](../../oledb/ole-db-interfaces/ibcpsession-bcpcontrol-ole-db.md) ändert das Verhalten von „IBCPSession::BCPReadFmt“.  
  
## <a name="arguments"></a>Argumente  
 *pwszFormatFile*[in]  
 Pfad und Dateiname der Datei, die die Formatwerte für die Datendatei enthält.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 S_OK  
 Die Methode wurde erfolgreich ausgeführt.  
  
 E_FAIL  
 Ein anbieterspezifischer Fehler ist aufgetreten. Ausführliche Informationen erhalten Sie über die [ISQLServerErrorInfo](./isqlservererrorinfo-geterrorinfo-ole-db.md)-Schnittstelle.  
  
 E_OUTOFMEMORY  
 Fehler aufgrund von nicht genügend Arbeitsspeicher.  
  
 E_UNEXPECTED  
 Die Methode wurde unerwartet aufgerufen. Die [IBCPSession::BCPInit](../../oledb/ole-db-interfaces/ibcpsession-bcpinit-ole-db.md)-Methode wurde beispielsweise erst nach dem Aufruf dieser Methode aufgerufen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [IBCPSession &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/ibcpsession-ole-db.md)   
 [Durchführen von Massenkopiervorgängen](../../oledb/features/performing-bulk-copy-operations.md)  
  
