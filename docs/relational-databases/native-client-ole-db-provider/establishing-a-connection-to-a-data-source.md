---
description: Herstellen einer Verbindung mit der Datenquelle (Native Client OLE DB-Anbieter)
title: Herstellen einer Verbindung mit der Datenquelle (Native Client OLE DB-Anbieter) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- data sources [SQL Server Native Client]
- connections [SQL Server Native Client]
- SQL Server Native Client OLE DB provider, data source connections
- CoCreateInstance method
- OLE DB data sources [SQL Server Native Client]
ms.assetid: 7ebd1394-cc8d-4bcf-92f3-c374a26e7ba0
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 384d6b0b9db152d1418b009c50345151786875e3
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88490724"
---
# <a name="establishing-a-connection-to-a-sql-server-native-client-data-source"></a>Herstellen einer Verbindung mit einer SQL Server Native Client Datenquelle
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Der Consumer muss zunächst eine Instanz eines Datenquellen Objekts erstellen, indem er die **CoCreateInstance** -Methode aufrufen, um auf den Native Client OLE DB-Anbieter zuzugreifen. Ein eindeutiger Klassenbezeichner (CLSID) identifiziert jeden OLE DB-Anbieter. Für den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB-Anbieter ist der Klassen Bezeichner CLSID_SQLNCLI10. Sie können auch das Symbol SQLNCLI_CLSID verwenden, das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in den Native Client OLE DB-Anbieter aufgelöst wird, der in der sqlncli. h verwendet wird, auf die Sie verweisen.  
  
 Das Datenquellenobjekt macht die **IDBProperties**-Schnittstelle verfügbar, die der Consumer verwendet, um grundlegende Authentifizierungsinformationen wie Servername, Datenbankname, Benutzer-ID und Kennwort bereitzustellen. Die **IDBProperties::SetProperties**-Methode wird aufgerufen, um diese Eigenschaften festzulegen.  
  
 Wenn mehrere Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf dem Computer ausgeführt werden, wird der Servername als ServerName\InstanceName angegeben.  
  
 Das Datenquellenobjekt macht auch die **IDBInitialize**-Schnittstelle verfügbar. Nachdem die Eigenschaften festgelegt wurden, wird die Verbindung zur Datenquelle durch Aufrufen der **IDBInitialize::Initialize**-Methode hergestellt. Beispiel:  
  
```  
CoCreateInstance(CLSID_SQLNCLI10,   
                 NULL,   
                 CLSCTX_INPROC_SERVER,  
                 IID_IDBInitialize,   
                 (void **) &pIDBInitialize)  
```  
  
 Dieser **cokreateinstance-Code** erstellt ein einzelnes-Objekt der-Klasse, die mit CLSID_SQLNCLI10 verknüpft ist (CSLID, das den Daten und dem Code zugeordnet ist, die zum Erstellen des-Objekts verwendet werden). IID_IDBInitialize ist ein Verweis auf den Bezeichner der Schnittstelle (**IDBInitialize**), die zur Kommunikation mit dem Objekt verwendet werden soll.  
  
 Die folgende Funktion ist eine Beispielfunktion, die eine Verbindung zur Datenquelle initiiert und herstellt.  
  
```  
void InitializeAndEstablishConnection() {  
   // Initialize the COM library.  
   CoInitialize(NULL);  
  
   // Obtain access to the SQL Server Native Client OLE DB provider.  
   hr = CoCreateInstance(CLSID_SQLNCLI10,   
                         NULL,   
                         CLSCTX_INPROC_SERVER,  
                         IID_IDBInitialize,   
                         (void **) &pIDBInitialize);  
   // Initialize property values needed to establish connection.  
   for (i = 0 ; i < 4 ; i++)   
      VariantInit(&InitProperties[i].vValue);  
  
   // Server name.  
   // See DBPROP structure for more information on InitProperties  
   InitProperties[0].dwPropertyID  = DBPROP_INIT_DATASOURCE;  
   InitProperties[0].vValue.vt    = VT_BSTR;  
   InitProperties[0].vValue.bstrVal=   
                     SysAllocString(L"Server");  
   InitProperties[0].dwOptions    = DBPROPOPTIONS_REQUIRED;  
   InitProperties[0].colid       = DB_NULLID;  
  
   // Database.  
   InitProperties[1].dwPropertyID  = DBPROP_INIT_CATALOG;  
   InitProperties[1].vValue.vt    = VT_BSTR;  
   InitProperties[1].vValue.bstrVal= SysAllocString(L"database");  
   InitProperties[1].dwOptions    = DBPROPOPTIONS_REQUIRED;  
   InitProperties[1].colid       = DB_NULLID;  
  
   // Username (login).  
   InitProperties[2].dwPropertyID  = DBPROP_AUTH_INTEGRATED;  
   InitProperties[2].vValue.vt    = VT_BSTR;  
   InitProperties[2].vValue.bstrVal= SysAllocString(L"SSPI");  
   InitProperties[2].dwOptions    = DBPROPOPTIONS_REQUIRED;  
   InitProperties[2].colid       = DB_NULLID;  
   InitProperties[3].dwOptions    = DBPROPOPTIONS_REQUIRED;  
   InitProperties[3].colid       = DB_NULLID;  
  
   // Construct the DBPROPSET structure(rgInitPropSet). The   
   // DBPROPSET structure is used to pass an array of DBPROP   
   // structures (InitProperties) to the SetProperties method.  
   rgInitPropSet[0].guidPropertySet = DBPROPSET_DBINIT;  
   rgInitPropSet[0].cProperties   = 4;  
   rgInitPropSet[0].rgProperties   = InitProperties;  
  
   // Set initialization properties.  
   hr = pIDBInitialize->QueryInterface(IID_IDBProperties,   
                           (void **)&pIDBProperties);  
   hr = pIDBProperties->SetProperties(1, rgInitPropSet);   
   pIDBProperties->Release();  
  
   // Now establish the connection to the data source.  
   pIDBInitialize->Initialize();  
}  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Erstellen einer SQL Server Native Client OLE DB-Anbieteranwendung](../../relational-databases/native-client-ole-db-provider/creating-a-sql-server-native-client-ole-db-provider-application.md)  
  
  
