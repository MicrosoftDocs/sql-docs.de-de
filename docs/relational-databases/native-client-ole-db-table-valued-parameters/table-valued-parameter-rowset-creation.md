---
description: Erstellung eines Tabellenwert Parameter-Rowsets in SQL Server Native Client
title: Erstellung eines Tabellenwert Parameter-Rowsets (Native Client OLE DB-Anbieter)
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- table-valued parameters, rowset creation
ms.assetid: ffe213ca-cc0e-465e-b31c-a8272324c4fe
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0bf030073ce8b8a22fe605ef54ea521253aa3337
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88482573"
---
# <a name="table-valued-parameter-rowset-creation-in-sql-server-native-client"></a>Erstellung eines Tabellenwert Parameter-Rowsets in SQL Server Native Client
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Consumer können zwar ein beliebiges Rowsetobjekt für Tabellenwertparameter bereitstellen, typische Rowsetobjekte werden jedoch mit Back-End-Datenspeichern implementiert und bieten somit nur eine eingeschränkte Leistung. Der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB-Anbieter ermöglicht es somit Consumern, ein spezielles Rowsetobjekt auf speicherinternen Daten zu erstellen. Dieses spezielle, in-Memory-Rowsetobjekt ist ein neues COM-Objekt, das als Tabellenwert Parameter-Rowset bezeichnet wird. Es bietet ähnliche Funktionen wie Parametersätze.  
  
 Tabellenwertparameter-Rowsetobjekte werden explizit vom Consumer für Eingabeparameter durch mehrere Schnittstellen auf Sitzungsebene erstellt. Es steht eine Instanz des Tabellenwertparameter-Rowsetobjekts pro Tabellenwertparameter zur Verfügung. Der Consumer kann die Tabellenwertparameter-Rowsetobjekte entweder durch Bereitstellen der Metadateninformationen, die bereits bekannt sind (statisches Szenario), oder durch Ermitteln über Anbieterschnittstellen (dynamisches Szenario) erstellen. In den folgenden Abschnitten werden diese beiden Szenarien beschrieben.  
  
## <a name="static-scenario"></a>Statisches Szenario  
 Wenn die Typinformationen bekannt sind, verwendet der Consumer ITableDefinitionWithConstraints::CreateTableWithConstraints, um einen Tabellenwertparameter für Rowsetobjekte zu instanziieren, die einem Tabellenwertparameter entsprechen.  
  
 Das *guid*-Feld (*pTableID*-Parameter) enthält die besondere GUID (CLSID_ROWSET_TVP). Das Element *pwszName* enthält den Namen des Tabellenwertparameter-Typs, den der Consumer instanziieren möchte. Das Feld *eKin* wird auf DBKIND_GUID_NAME festgelegt. Der Name ist erforderlich, wenn es sich um eine Ad-hoc-SQL-Anweisung handelt; bei einem Prozeduraufruf ist die Angabe des Namens optional.  
  
 Bei der Aggregation übergibt der Consumer den *pUnkOuter*-Parameter mit dem kontrollierenden IUnknown-Element.  
  
 Die Eigenschaften des Tabellenwert Parameter-Rowsetobjekts sind schreibgeschützt, sodass der Consumer keine Eigenschaften in *rgPropertySets*festlegen soll.  
  
 Für das Element *rgPropertySets* jeder DBCOLUMNDESC-Struktur kann der Consumer zusätzliche Eigenschaften für jede Spalte angeben. Diese Eigenschaften gehören zum DBPROPSET_SQLSERVERCOLUMN-Eigenschaftensatz. Sie ermöglichen es Ihnen, berechnete und standardmäßige Einstellungen für jede Spalte anzugeben. Sie unterstützen auch vorhandene Spalteneigenschaften, z. B. NULL-Zulässigkeit und Identität.  
  
 Um entsprechende Informationen aus einem Tabellenwertparameter-Rowsetobjekt abzurufen, verwendet der Consumer IRowsetInfo::GetProperties.  
  
 Zum Abrufen von Informationen über den NULL-, eindeutigen, berechneten und Update Status der einzelnen Spalten verwendet der Consumer IColumnsRowset:: GetColumnsRowset oder IColumnsInfo:: GetColumnInfo. Diese Methoden stellen ausführliche Informationen über jede Tabellenwertparameter-Rowsetspalte bereit.  
  
 Der Consumer gibt den Typ jeder Spalte des Tabellenwertparameters an. Dies ähnelt der Angabe von Spalten, wenn in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] eine Tabelle erstellt wird. Der Consumer erhält ein Tabellenwert Parameter-Rowsetobjekt vom [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB-Anbieter über den *ppRowset* -Ausgabeparameter.  
  
## <a name="dynamic-scenario"></a>Dynamisches Szenario  
 Wenn der Consumer keine Typinformationen hat, sollte er IOpenRowset:: OPENROWSET verwenden, um Tabellenwert Parameter-Rowsetobjekte zu instanziieren. Der Consumer muss dem Anbieter somit nur den Typnamen zur Verfügung stellen.  
  
 In diesem Szenario erhält der Anbieter im Namen des Consumers Typinformationen zu einem Tabellenwertparameter-Rowsetobjekt vom Server.  
  
 Die Parameter *pTableID* und *pUnkOuter* sollten wie im statischen Szenario festgelegt werden. Der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB-Anbieter ruft dann die Typinformationen (Spalten Informationen und Einschränkungen) vom Server ab und gibt ein Tabellenwert Parameter-Rowsetobjekt über den *ppRowset* -Parameter zurück. Für diesen Vorgang ist eine Kommunikation mit dem Server notwendig, sodass die Leistung nicht so gut ist wie beim statischen Szenario. Das dynamische Szenario funktioniert nur mit parametrisierten Prozeduraufrufen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Tabellenwertparameter &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-table-valued-parameters/table-valued-parameters-ole-db.md)   
 [Verwenden von Tabellenwertparametern &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-how-to/use-table-valued-parameters-ole-db.md)  
  
  
