---
title: Datenquellenobjekte (OLE DB-Treiber) | Microsoft-Dokumentation
description: Erfahren Sie, wie ein Consumer des OLE DB-Treibers für SQL Server eine Instanz eines Datenquellenobjekts für einen Anbieter erstellt.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data access [OLE DB Driver for SQL Server], data source objects
- OLE DB Driver for SQL Server, data source objects
- MSOLEDBSQL, data source objects
- OLE DB Driver for SQL Server, data source objects
- OLE DB data source objects [OLE DB Driver for SQL Server]
- data source objects [OLE DB]
- CLSID
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 9abdf54d533eab604ff564c9f32b99b2eba566d9
ms.sourcegitcommit: c95f3ef5734dec753de09e07752a5d15884125e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2020
ms.locfileid: "88862407"
---
# <a name="data-source-objects-ole-db"></a>Datenquellenobjekte (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Der OLE DB-Treiber für SQL Server verwendet den Begriff „Datenquelle“ für die Gruppe der OLE DB-Schnittstellen, die zum Herstellen einer Verknüpfung mit dem Datenspeicher verwendet werden, z.B. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Die erste Aufgabe eines Consumers des OLE DB-Treibers für SQL Server besteht darin, eine Instanz des Datenquellenobjekts des Anbieters zu erstellen.  
  
 Jeder OLE DB-Anbieter deklariert einen Klassenbezeichner (CLSID) für sich. Die CLSID für den OLE DB-Treiber für SQL Server ist die C/C++-GUID CLSID_MSOLEDBSQL (das Symbol MSOLEDBSQL_CLSID wird in die korrekte ProgID in der msoledbsql.h-Datei aufgelöst, auf die Sie verweisen). Mit der CLSID verwendet der Consumer die OLE-Funktion **CoCreateInstance** zum Erstellen einer Instanz des Datenquellenobjekts.  
  
 Der OLE DB-Treiber für SQL Server ist ein In-Process-Server. Instanzen von OLE DB-Treiber für SQL Server-Objekten werden mithilfe des Makros CLSCTX_INPROC_SERVER erstellt, um den ausführbaren Kontext anzugeben.  
  
 Das Datenquellenobjekt des OLE DB-Treibers für SQL Server stellt die OLE DB-Initialisierungsschnittstellen bereit, die Consumern ermöglichen, Verbindungen zu vorhandenen [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Datenbanken herzustellen.  
  
 Jede über den OLE DB-Treiber für SQL Server hergestellte Verbindung legt diese Optionen automatisch fest:  
  
-   SET ANSI_WARNINGS ON  
  
-   SET ANSI_NULLS ON  
  
-   SET ANSI_PADDING ON  
  
-   SET ANSI_NULL_DFLT_ON ON  
  
-   SET QUOTED_IDENTIFIER ON  
  
-   SET CONCAT_OF_NULL_YIELDS_NULL ON  
  
 Dieses Beispiel verwendet das Klassenbezeichnermakro zum Erstellen eines OLE DB-Treiber für SQL Server-Datenquellenobjekts und zum Abrufen einer Referenz auf die **IDBInitialize**-Schnittstelle.  
  
```  
IDBInitialize*   pIDBInitialize;  
HRESULT          hr;  
  
hr = CoCreateInstance(CLSID_MSOLEDBSQL, NULL, CLSCTX_INPROC_SERVER,  
    IID_IDBInitialize, (void**) &pIDBInitialize);  
  
if (SUCCEEDED(hr))  
{  
    //  Perform necessary processing with the interface.  
    pIDBInitialize->Uninitialize();  
    pIDBInitialize->Release();  
}  
else  
{  
    // Display error from CoCreateInstance.  
}  
```  
  
 Mit der erfolgreichen Erstellung einer Instanz eines OLE DB-Treiber für SQL Server-Datenquellenobjekts kann die Consumeranwendung fortfahren, indem die Datenquelle initialisiert und Sitzungen erstellt werden. OLE DB-Sitzungen präsentieren die Schnittstellen, die Datenzugriff und -bearbeitung ermöglichen.  
  
 Der OLE DB-Treiber für SQL Server stellt die erste Verbindung zu einer angegebenen Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] als Teil einer erfolgreichen Datenquelleninitialisierung her. Die Verbindung wird beibehalten, solange eine Referenz auf einer beliebigen Datenquellen-Initialisierungsschnittstelle beibehalten wird oder bis die Methode **IDBInitialize::Uninitialize** aufgerufen wird.  
  
## <a name="in-this-section"></a>In diesem Abschnitt  
  
-   [Datenquelleneigenschaften &#40;OLE DB&#41;](../../oledb/ole-db-data-source-objects/data-source-properties-ole-db.md)  
  
-   [Eigenschaften von Datenquelleninformationen](../../oledb/ole-db-data-source-objects/data-source-information-properties.md)  
  
-   [Initialisierungs- und Autorisierungseigenschaften](../../oledb/ole-db-data-source-objects/initialization-and-authorization-properties.md)  
  
-   [Sitzungen](../../oledb/ole-db-data-source-objects/sessions.md)  
  
-   [Sitzungseigenschaften: OLE DB-Treiber für SQL Server](../../oledb/ole-db-data-source-objects/session-properties-oledb-driver-for-sql-server.md)  
  
-   [Persistente Datenquellenobjekte](../../oledb/ole-db-data-source-objects/persisted-data-source-objects.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [OLE DB-Treiber für SQL Server-Programmierung](../../oledb/ole-db/oledb-driver-for-sql-server-programming.md)  
  
  
