---
title: Verteilte Abfrageunterstützung für Schemarowsets | Microsoft-Dokumentation
description: Die IDBSchemaRowset-Schnittstelle des OLE DB-Treibers für SQL Server gibt Metadaten zu verknüpften Servern zurück, um verteilte SQL Server-Abfragen zu unterstützen.
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- DBPROPSET_SQLSERVERSESSION property
- schema rowsets [OLE DB]
- distributed queries [SQL Server], OLE DB Driver for SQL Server
- OLE DB, schema rowsets
- OLE DB rowsets, schema
- rowsets [OLE DB], schema
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 61105b2e974cd111cd4f5dfd10e7f528cd04519a
ms.sourcegitcommit: c95f3ef5734dec753de09e07752a5d15884125e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2020
ms.locfileid: "88861566"
---
# <a name="schema-rowsets---distributed-query-support"></a>Schemarowsets: Verteilte Abfrageunterstützung
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Zur Unterstützung verteilter Abfragen in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] gibt die **IDBSchemaRowset**-Schnittstelle des OLE DB-Treibers für SQL Server Metadaten über Verbindungsserver zurück.  
  
 Wenn die DBPROPSET_SQLSERVERSESSION-Eigenschaft SSPROP_QUOTEDCATALOGNAMES auf VARIANT_TRUE festgelegt wurde, kann für den Katalognamen ein Bezeichner in Anführungszeichen angegeben werden (beispielsweise "my.catalog"). Wenn eine Katalogeinschränkung für die Ausgabe eines Schemarowsets angegeben wird, erkennt der OLE DB-Treiber für SQL Server zweiteilige Namen, die sich aus dem Namen des Verbindungsservers und dem Katalognamen zusammensetzen. Für die Schemarowsets in der Tabelle unten wird durch die Angabe eines zweiteiligen Katalognamens in Form von _linked\_server_ **.** _catalog_ die Ausgabe auf den betreffenden Katalog des genannten Verbindungsservers beschränkt.  
  
|Schemarowset|Katalogeinschränkung|  
|-------------------|-------------------------|  
|DBSCHEMA_CATALOGS|CATALOG_NAME|  
|DBSCHEMA_COLUMNS|TABLE_CATALOG|  
|DBSCHEMA_PRIMARY_KEYS|TABLE_CATALOG|  
|DBSCHEMA_TABLES|TABLE_CATALOG|  
|DBSCHEMA_FOREIGN_KEYS|PK_TABLE_CATALOG FK_TABLE_CATALOG|  
|DBSCHEMA_INDEXES|TABLE_CATALOG|  
|DBSCHEMA_COLUMN_PRIVILEGES|TABLE_CATALOG|  
|DBSCHEMA_TABLE_PRIVILEGES|TABLE_CATALOG|  
  
> [!NOTE]  
>  Zur Beschränkung eines Schemarowsets auf alle Kataloge eines Verbindungsservers, verwenden Sie die Syntax *linked_server* (wobei der Unterstrich als Trennzeichen Teil der Namensangabe ist). Diese Syntax ist gleichbedeutend mit der Angabe von NULL für die Katalognamensbeschränkung und wird auch verwendet, wenn der Verbindungsserver eine Datenquelle angibt, die Kataloge nicht unterstützt.  
 
 Der OLE DB-Treiber für SQL Server definiert das Schemarowset LINKEDSERVERS und gibt eine Liste der OLE DB-Datenquellen zurück, die als Verbindungsserver registriert sind.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Schema Rowset Support &#40;OLE DB&#41; (Schemarowset-Unterstützung &#40;OLE DB&#41;)](../../oledb/ole-db/schema-rowset-support-ole-db.md)   
 [Schemarowsets: LINKEDSERVERS-Rowset](../../oledb/ole-db/schema-rowsets-linkedservers-rowset.md)  
  
  
