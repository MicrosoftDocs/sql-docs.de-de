---
title: Datenquelleneigenschaften (OLE DB-Treiber) | Microsoft-Dokumentation
description: Erfahren Sie, wie der OLE DB-Treiber für SQL Server Datenquelleneigenschaften implementiert, einschließlich eines anbieterspezifischen Eigenschaftssatzes mit zusätzlichen Datenquelleneigenschaften.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- OLE DB Driver for SQL Server, data source properties
- properties [OLE DB]
- data source properties [OLE DB]
- OLE DB data source properties [OLE DB Driver for SQL Server]
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 281e5b04b76317a24c15fce962e88ebbe3de3b02
ms.sourcegitcommit: c95f3ef5734dec753de09e07752a5d15884125e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2020
ms.locfileid: "88862379"
---
# <a name="data-source-properties-ole-db"></a>Datenquelleneigenschaften (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Der OLE DB-Treiber für SQL Server implementiert Datenquelleneigenschaften wie folgt.  
  
|Eigenschafts-ID|BESCHREIBUNG|  
|-----------------|-----------------|  
|DBPROP_CURRENTCATALOG|R/W: Lesen/Schreiben; Standardwert: Keiner<br /><br /> Beschreibung: Der Wert von DBPROP_CURRENTCATALOG meldet die aktuelle Datenbank für eine Sitzung des OLE DB-Treibers für SQL Server. Das Festlegen des Eigenschaftswerts hat dieselbe Auswirkung wie das Festlegen der aktuellen Datenbank mithilfe der [!INCLUDE[tsql](../../../includes/tsql-md.md)]-Anweisung *USE* database.<br /><br /> Ab [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] gibt DBPROP_CURRENTCATALOG den Datenbanknamen in Kleinbuchstaben zurück, wenn Sie [sp_defaultdb](../../../relational-databases/system-stored-procedures/sp-defaultdb-transact-sql.md) aufrufen und den Datenbanknamen in Kleinbuchstaben angeben, auch wenn die Datenbank ursprünglich mit einem Namen in Groß- und Kleinbuchstaben erstellt wurde. In früheren Versionen von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] gibt DBPROP_CURRENTCATALOG den erwarteten Namen mit Groß- und Kleinbuchstaben zurück.|  
|DBPROP_MULTIPLECONNECTIONS|R/W: Lesen/Schreiben; Standardwert: VARIANT_FALSE<br /><br /> Beschreibung: Wenn während der Verbindung ein Befehl ausgeführt wird, der kein Rowset produziert, oder der ein Rowset produziert, das kein Servercursor ist, und Sie einen anderen Befehl ausführen, wird eine neue Verbindung erstellt, um den neuen Befehl auszuführen, wenn für DBPROP_MULTIPLECONNECTIONS VARIANT_TRUE festgelegt ist.<br /><br /> Der OLE DB-Treiber für SQL Server erstellt keine weitere Verbindung, wenn für DBPROP_MULTIPLECONNECTION VARIANT_FALSE festgelegt ist oder wenn in der Verbindung eine Transaktion aktiv ist. Der OLE DB-Treiber für SQL Server gibt DB_E_OBJECTOPEN zurück, wenn für DBPROP_MULTIPLECONNECTIONS VARIANT_FALSE festgelegt ist, und E_FAIL, wenn eine aktive Transaktion vorhanden ist. Transaktionen und Sperren werden von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] auf Verbindungsbasis verwaltet. Wenn eine zweite Verbindung hergestellt wird, nutzen die Befehle auf den anderen Verbindungen Sperren nicht gemeinsam. Um zu gewährleisten, dass ein Befehl einen anderen nicht blockiert, halten Sie Zeilen gesperrt, die von dem anderen Befehl angefordert werden. Dies gilt auch beim Erstellen mehrerer Sitzungen.<br /><br /> Jede Sitzung verfügt über eine separate Verbindung.|  
  
 Im anbieterspezifischen Eigenschaftensatz DBPROPSET_SQLSERVERDATASOURCE definiert der OLE DB-Treiber für SQL Server die folgenden zusätzlichen Datenquelleneigenschaften.  
  
|Eigenschafts-ID|BESCHREIBUNG|  
|-----------------|-----------------|  
|SSPROP_ENABLEFASTLOAD|R/W: Lesen/Schreiben; Standardwert: VARIANT_FALSE<br /><br /> Beschreibung: Um Massenkopiervorgänge aus dem Speicher zu aktivieren, sollte die SSPROP_ENABLEFASTLOAD-Eigenschaft auf VARIANT_TRUE festgelegt werden. Wenn diese Eigenschaft für die Datenquelle festgelegt wurde, lässt die neu erstellte Sitzung den Consumerzugriff auf die [IRowsetFastLoad](../../oledb/ole-db-interfaces/irowsetfastload-ole-db.md)-Schnittstelle zu.<br /><br /> Wenn die Eigenschaft auf VARIANT_TRUE festgelegt ist, ist die **IRowsetFastLoad**-Schnittstelle über **IOpenRowset::OpenRowset** verfügbar, indem die **IID_IRowsetFastLoad**-Schnittstelle angefordert oder **SSPROP_IRowsetFastLoad** auf VARIANT_TRUE festgelegt wird.|  
|SSPROP_ENABLEBULKCOPY|R/W: Lesen/Schreiben; Standardwert: VARIANT_FALSE<br /><br /> Beschreibung: Um Massenkopiervorgänge aus Dateien zu aktivieren, sollte die SSPROP_ENABLEBULKCOPY-Eigenschaft auf VARIANT_TRUE festgelegt werden. Wenn diese Eigenschaft für die Datenquelle festgelegt wurde, ist der Consumerzugriff auf die IBCPSession-Schnittstelle auf derselben Ebene verfügbar wie Sessions.<br /><br /> Auch SSPROP_IRowsetFastLoad muss auf VARIANT_TRUE festgelegt werden.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Datenquellenobjekte &#40;OLE DB&#41;](../../oledb/ole-db-data-source-objects/data-source-objects-ole-db.md)  
  
  
