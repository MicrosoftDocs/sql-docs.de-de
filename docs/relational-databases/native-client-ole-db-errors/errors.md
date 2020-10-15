---
description: SQL Server Native Client-Fehler
title: Fehler | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client OLE DB provider, errors
- OLE/COM errors
- errors [OLE DB]
- OLE DB error handling, about error handling
- OLE DB error handling
ms.assetid: bd0612f4-96ef-4919-b0f9-b5447210fe93
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f31e215a92ac3c72da4cdd62dbe1b63b43848c5c
ms.sourcegitcommit: 7eb80038c86acfef1d8e7bfd5f4e30e94aed3a75
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2020
ms.locfileid: "92081989"
---
# <a name="sql-server-native-client-errors"></a>SQL Server Native Client-Fehler
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  OLE/COM-Objekte melden Fehler durch den HRESULT-Rückgabecode von Objektelementfunktionen. Ein OLE/COM HRESULT ist eine Bitgepackte Struktur. OLE stellt Makros bereit, die Strukturmember dereferenzieren.  
  
 OLE/COM gibt die **IErrorInfo**-Schnittstelle an. Die Schnittstelle macht Methoden wie **GetDescription** verfügbar. Dies ermöglicht es Clients, Fehlerdetails aus OLE/COM-Servern zu extrahieren. OLE DB erweitert **IErrorInfo**, um die Rückgabe von mehreren Fehlerinformationspaketen bei der Ausführung einer Einzelmemberfunktion zu unterstützen.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] kann mehrere Fehler zurückgeben. Eine Anwendung kann Serverfehler einzeln abrufen, indem [IMultipleResults::GetResult](/previous-versions/windows/desktop/ms721289(v=vs.85)) in Kombination mit ISQLErrorInfo und IErrorRecords aufgerufen wird.  
  
 Der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB-Anbieter macht die OLE DB Datensatz-Enhanced **IErrorInfo**, die benutzerdefinierten **ISQLErrorInfo**und die anbieterspezifischen [ISQLServerErrorInfo](../native-client-ole-db-interfaces/isqlservererrorinfo-geterrorinfo-ole-db.md) -Fehler Objekt Schnittstellen verfügbar.  
  
 Informationen zur Ablaufverfolgung von Fehlern finden Sie unter [Data Access Tracing (Ablaufverfolgung für den Datenzugriff)](/previous-versions/sql/sql-server-2008/cc765421(v=sql.100)). Informationen zu Verbesserungen der in [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] hinzugefügten Fehlerablaufverfolgung finden Sie unter [Zugreifen auf Diagnoseinformationen im Protokoll der erweiterten Ereignisse](../../relational-databases/native-client/features/accessing-diagnostic-information-in-the-extended-events-log.md).  
  
## <a name="in-this-section"></a>In diesem Abschnitt  
  
-   [Rückgabecodes](../../relational-databases/native-client-ole-db-errors/return-codes.md)  
  
-   [Informationen in Fehlerschnittstellen](../../relational-databases/native-client-ole-db-errors/information-in-error-interfaces.md)  
  
-   [SQL Server-Fehlerdetail](../../relational-databases/native-client-ole-db-errors/sql-server-error-detail.md)  
  
-   [Abrufen von Fehlerinformationen](../../relational-databases/native-client-ole-db-errors/retrieving-error-information.md)  
  
-   [SQL Server-Meldungsergebnisse](../../relational-databases/native-client-ole-db-errors/sql-server-message-results.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQL Server Native Client &#40;OLE DB&#41;](../../relational-databases/native-client/ole-db/sql-server-native-client-ole-db.md)  
  
