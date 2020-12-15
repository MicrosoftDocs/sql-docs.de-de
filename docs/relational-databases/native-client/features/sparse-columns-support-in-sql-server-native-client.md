---
description: Unterstützung für Spalten mit geringer Dichte in SQL Server Native Client
title: Unterstützung für Spalten unter
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- sparse columns, ODBC
- sparse columns, SQL Server Native Client
- sparse columns, OLE DB
ms.assetid: aee5ed81-7e23-42e4-92d3-2da7844d9bc3
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 54419481ea32fb3b1a5cfc896f16bf4db974c0fb
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97462051"
---
# <a name="sparse-columns-support-in-sql-server-native-client"></a>Unterstützung für Spalten mit geringer Dichte in SQL Server Native Client
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client unterstützt Sparsespalten. Weitere Informationen zu Sparsespalten in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] finden Sie unter [Verwenden von Spalten mit geringer Dichte](../../../relational-databases/tables/use-sparse-columns.md) und [Verwenden von Spaltensätzen](../../../relational-databases/tables/use-column-sets.md).  
  
 Weitere Informationen zur Unterstützung für sparsespalten in Native Client finden Sie unter Unterstützung für [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [sparsespalten &#40;ODBC-&#41;](../../../relational-databases/native-client/odbc/sparse-columns-support-odbc.md) und [&#41;OLE DB &#40;unter ](../../../relational-databases/native-client/ole-db/sparse-columns-support-ole-db.md)Stützung für sparsespalten  
  
 Informationen zu Beispielanwendungen, die diese Funktion veranschaulichen, finden Sie unter [Programmierbeispiele für SQL Server-Daten](https://msftdpprodsamples.codeplex.com/).  
  
## <a name="user-scenarios-for-sparse-columns-and-sql-server-native-client"></a>Benutzerszenarien für Spalten mit geringer Dichte und SQL Server Native Client  
 In der folgenden Tabelle sind die gängigen Benutzerszenarien für [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client-Benutzer mit Sparsespalten zusammengefasst:  
  
|Szenario|Verhalten|  
|--------------|--------------|  
|**select \* from table** oder IOpenRowset::OpenRowset.|Gibt alle Spalten, die keine Elemente der Sparsespalte **column_set** sind, sowie eine XML-Spalte zurück, die die Werte aller Spalten ungleich NULL enthält, die Elemente der Sparsespalte **column_set** sind.|  
|Verweisen auf eine Spalte über den Namen|Auf die Spalte kann unabhängig von ihrem Status als Sparsespalte oder ihrer **column_set**-Zugehörigkeit verwiesen werden.|  
|Zugreifen auf **column_set**-Elementspalten über eine berechnete XML-Spalte.|Auf Spalten, die Elemente der Sparsespalte **column_set** sind, kann zugegriffen werden, indem Sie den **column_set** über den Namen auswählen und Werte durch Aktualisieren des XML-Codes in der Spalte **column_set** einfügen und aktualisieren.<br /><br /> Der Wert muss dem Schema für **column_set**-Spalten entsprechen.|  
|Abrufen von Metadaten für alle Spalten in einer Tabelle über SQLColumns mit einem Spalten Suchmuster von NULL oder '% ' (ODBC); oder über das DBSCHEMA_COLUMNS Schemarowset ohne Spalten Einschränkung (OLE DB).|Gibt eine Zeile für alle Spalten zurück, die nicht Elemente eines **column_set** sind. Wenn die Tabelle eine Sparsespalte **column_set** aufweist, wird eine Zeile dafür zurückgegeben.<br /><br /> Beachten Sie, dass hier keine Metadaten für Spalten zurückgegeben werden, die Elemente eines **column_set** sind.|  
|Abrufen von Metadaten für alle Spalten, unabhängig von geringer Dichte oder Zugehörigkeit in einem **column_set**. Hier könnte eine sehr große Anzahl an Zeilen zurückgegeben werden.|Legen Sie das Deskriptorfeld SQL_SOPT_SS_NAME_SCOPE auf SQL_SS_NAME_SCOPE_EXTENDED und [SQLColumns](../../../relational-databases/native-client-odbc-api/sqlcolumns.md) (ODBC) aufzurufen.<br /><br /> Nennen Sie IDBSchemaRowset:: GetRowset für das DBSCHEMA_COLUMNS_EXTENDED Schemarowset (OLE DB).<br /><br /> Dieses Szenario ist in einer Anwendung, die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client einer früheren Version als [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] verwendet, nicht möglich. Eine solche Anwendung könnte jedoch Systemsichten direkt abfragen.|  
|Abrufen von Metadaten nur für Spalten, die Elemente eines **column_set** sind. Hier könnte eine sehr große Anzahl an Zeilen zurückgegeben werden.|Legen Sie das Deskriptorfeld SQL_SOPT_SS_NAME_SCOPE auf SQL_SS_NAME_SCOPE_SPARSE_COLUMN_SET und SQLColumns (ODBC) aufzurufen.<br /><br /> Nennen Sie IDBSchemaRowset:: GetRowset für das DBSCHEMA_SPARSE_COLUMN_SET Schemarowset (OLE DB).<br /><br /> Dieses Szenario ist in einer Anwendung, die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client einer früheren Version als [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] verwendet, nicht möglich. Eine solche Anwendung könnte jedoch Systemsichten abfragen.|  
|Bestimmen, ob eine Spalte eine geringe Dichte aufweist|Weitere Informationen finden Sie in der SS_IS_SPARSE-Spalte des SQLColumns-Resultsets (ODBC).<br /><br /> Betrachten Sie die SS_IS_SPARSE-Spalte des DBSCHEMA_COLUMNS-Schemarowsets (OLE DB).<br /><br /> Dieses Szenario ist in einer Anwendung, die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client einer früheren Version als [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] verwendet, nicht möglich. Eine solche Anwendung könnte jedoch Systemsichten abfragen.|  
|Bestimmen, ob eine Spalte ein **column_set** ist.|Weitere Informationen finden Sie in der SS_IS_COLUMN_SET-Spalte des Resultsets SQLColumns. Oder sehen Sie sich das [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-spezifische SQL_CA_SS_IS_COLUMN_SET-Spaltenattribut (ODBC) an.<br /><br /> Betrachten Sie die SS_IS_COLUMN_SET-Spalte des DBSCHEMA_COLUMNS-Schemarowsets. Betrachten Sie alternativ *dwFlags*, das von IColumnsInfo::GetColumnInfo zurückgegeben wird, oder DBCOLUMNFLAGS in dem von IColumnsRowset::GetColumnsRowset zurückgegebenen Rowset. Für **column_set** Spalten werden DBCOLUMNFLAGS_SS_ISCOLUMNSET festgelegt (OLE DB).<br /><br /> Dieses Szenario ist in einer Anwendung, die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client einer früheren Version als [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] verwendet, nicht möglich. Eine solche Anwendung könnte jedoch Systemsichten abfragen.|  
|Importieren und Exportieren von Sparsespalten über BCP für eine Tabelle ohne **column_set**.|Keine Änderung im Verhalten im Vergleich zu vorherigen Versionen von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client.|  
|Importieren und Exportieren von Sparsespalten über BCP für eine Tabelle mit einem **column_set**.|Der **column_set** wird auf die gleiche Weise wie XML importiert und exportiert, d. h. wie **varbinary(max)** bei Bindung als Binärtyp oder **nvarchar(max)** bei Bindung als **char**- oder **wchar**-Typ.<br /><br /> Spalten, die Elemente der Sparsespalte **column_set** sind, werden nicht als unterschiedliche Spalten exportiert. Sie werden nur im Wert des **column_set** exportiert.|  
|**queryout**-Verhalten für BCP.|Keine Änderung in der Behandlung explizit benannter Spalten im Vergleich zu vorherigen Versionen von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client.<br /><br /> In Szenarien, die das Importieren und Exportieren zwischen Tabellen mit unterschiedlichen Schemas umfassen, ist möglicherweise eine besondere Behandlung erforderlich.<br /><br /> Weitere Informationen über BCP finden Sie unter „Massenkopierunterstützung (BCP) für Spalten mit geringer Dichte” weiter unten in diesem Thema.|  
  
## <a name="down-level-client-behavior"></a>Downlevelclient-Verhalten  
 Downlevelclients geben Metadaten nur für Spalten zurück, die nicht Elemente der Sparsespalte **column_set** für SQLColumns und DBSCHMA_COLUMNS sind. Die zusätzlichen OLE DB Schemarowsets, die in [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] Native Client eingeführt werden, sind nicht verfügbar, und die Änderungen an SQLColumns in ODBC über SQL_SOPT_SS_NAME_SCOPE.  
  
 Downlevelclients können auf Spalten, die Elemente der Sparsespalte **column_set** sind, über den Namen zugreifen. Auf die Spalte **column_set** kann als XML-Spalte für [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]-Clients zugegriffen werden.  
  
## <a name="bulk-copy-bcp-support-for-sparse-columns"></a>Massenkopierunterstützung (BCP) für Spalten mit geringer Dichte  
 Es gibt keine Änderungen an der BCP-API in ODBC oder OLE DB für die sparsespalten oder **column_set** Features.  
  
 Wenn eine Tabelle einen **column_set** aufweist, werden Sparsespalten nicht wie unterschiedliche Spalten behandelt. Die Werte aller Sparsespalten sind im Wert des **column_set** enthalten, der auf dieselbe Art und Weise wie eine XML-Spalte exportiert wird, d. h. als **varbinary(max)** , wenn er als Binärtyp gebunden ist, oder als **nvarchar(max)** , wenn er als **char**-Typ oder **wchar**-Typ gebunden ist. Beim Importieren muss der **column_set**-Wert dem Schema des **column_set** entsprechen.  
  
 Bei **queryout**-Vorgängen gibt es keine Änderungen in der Behandlung von Spalten, auf die explizit verwiesen wird. **column_set**-Spalten weisen das gleiche Verhalten auf wie XML-Spalten, und die Sparseeigenschaft hat keine Auswirkungen auf die Behandlung von benannten Sparsespalten.  
  
 Wenn jedoch **queryout** für das Exportieren verwendet wird, und Sie über den Namen auf Sparsespalten verweisen, die Elemente des Sparsespaltensatzes sind, können Sie keinen direkten Import in eine Spalte mit gleicher Struktur durchführen. Dies liegt daran, dass bcp mit einem **Select &#42;** -Vorgang für den Import konsistente Metadaten verwendet und **column_set** Member-Spalten mit diesen Metadaten nicht übereinstimmen kann. Damit **column_set**-Elementspalten einzeln importiert werden können, müssen Sie eine Ansicht für die Tabelle definieren, die auf die gewünschten **column_set**-Spalten verweist. Darüber hinaus müssen Sie den Importvorgang über die Ansicht ausführen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Programmierung für SQL Server Native Client](../../../relational-databases/native-client/sql-server-native-client-programming.md)  
  
  
