---
description: OLE DB Table-Valued Parametertyp Unterstützung (Native Client OLE DB Provider)
title: Unterstützung für Tabellenwert Parameter-Typen (Native Client OLE DB-Anbieter)
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- table-valued parameters (OLE DB), API support (OLE DB)
ms.assetid: 147036a0-260e-4f81-8b3b-89209e023a32
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d8b0817146d1be3ce15349844ce37a1cdae853a6
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97469271"
---
# <a name="ole-db-table-valued-parameter-type-support-native-client-ole-db-provider"></a>OLE DB Table-Valued Parametertyp Unterstützung (Native Client OLE DB Provider)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  In diesem Thema wird die OLE DB-Typunterstützung für Tabellenwertparameter beschrieben.  
  
## <a name="table-valued-parameter-rowset-object"></a>Tabellenwertparameter-Rowsetobjekt  
 Sie können ein spezielles Rowsetobjekt für Tabellenwertparameter erstellen. Sie können das Rowsetobjekt für Tabellenwertparameter mithilfe von ITableDefinitionWithConstraints::CreateTableWithConstraints oder IOpenRowset::OpenRowset erstellen. Legen Sie hierzu das *eKind*-Element des *pTableID*-Parameters auf DBKIND_GUID_NAME fest, und geben Sie für das *guid*-Element CLSID_ROWSET_INMEMORY an. Der Servertypname für den Tabellenwertparameter muss bei Verwendung von IOpenRowset:: OPENROWSET im *pwszName*-Element von *pTableID* angegeben werden. Das Tabellenwertparameter-Rowsetobjekt verhält sich wie ein reguläres Objekt des OLE DB-Anbieters von SQL Server Native Client.  
  
```  
const GUID CLSID_ROWSET_TVP =   
{0xc7ef28d5, 0x7bee, 0x443f, {0x86, 0xda, 0xe3, 0x98, 0x4f, 0xcd, 0x4d, 0xf9}};  
  
CoType RowsetTVP  
{  
[mandatory] interface IAccessor;  
[mandatory] interface IColumnsInfo;  
[mandatory] interface IConvertType;  
[mandatory] interface IRowset;  
[mandatory] interface IRowsetInfo;  
[optional]  interface IColumnsRowset;  
[optional]  interface IRowsetChange;  
[optional]  interface ISupportErrorInfo;  
};  
```  
  
## <a name="dbtype_table"></a>DBTYPE_TABLE  
 Der neue Typ DBTYPE_TABLE stellt einen Tabellentyp dar. Dieser Typ gibt Tabellenwertparameter in verschiedenen OLE DB-Schnittstellen an, wo ein DBTYPE erforderlich ist.  
  
```  
#define DBTYPE_TABLE (143)  
```  
  
 DBTYPE_TABLE hat das gleiche Format wie DBTYPE_IUNKNOWN. Es ist ein Zeiger auf ein Objekt im Datenpuffer. Um die Bindungen vollständig anzugeben, füllt der Consumer den DBOBJECT-Puffer, wobei *iid* auf eine der Schnittstellen des Rowsetobjekts (IID_IRowset) festgelegt wird. Wenn in den Bindungen die Angabe von DBOBJECT fehlt, wird IID_IRowset angenommen.  
  
 Konvertierungen zu und von DBTYPE_TABLE für andere Typen werden nicht unterstützt. Außer bei einer Konvertierung von DBTYPE_TABLE zu DBTYPE_TABLE gibt IConvertType::CanConvert bei einer nicht unterstützten Konvertierung für jede Anforderung S_FALSE zurück. Hierbei wird die Angabe von DBCONVERTFLAGS_PARAMETER für das Command-Objekt angenommen.  
  
## <a name="methods"></a>Methoden  
 Weitere Informationen zu OLE DB-Methoden, die Tabellenwertparameter unterstützen, finden Sie unter [OLE DB-Unterstützung von Tabellenwertparametern &#40;Methoden&#41;](../../relational-databases/native-client-ole-db-table-valued-parameters/ole-db-table-valued-parameter-type-support-methods.md).  
  
## <a name="properties"></a>Eigenschaften  
 Weitere Informationen zu OLE DB-Eigenschaften, die Tabellenwertparameter unterstützen, finden Sie unter [OLE DB-Unterstützung von Tabellenwertparametern &#40;Eigenschaften&#41;](../../relational-databases/native-client-ole-db-table-valued-parameters/ole-db-table-valued-parameter-type-support-properties.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Tabellenwertparameter &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-table-valued-parameters/table-valued-parameters-ole-db.md)   
 [Verwenden von Tabellenwertparametern &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-how-to/use-table-valued-parameters-ole-db.md)  
  
  
