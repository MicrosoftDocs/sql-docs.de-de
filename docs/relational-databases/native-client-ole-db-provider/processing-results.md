---
description: Verarbeitungsergebnisse (Native Client OLE DB-Anbieter)
title: Verarbeitungsergebnisse (Native Client OLE DB-Anbieter)
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client OLE DB provider, results processing
- OLE DB, processing results
- rowsets [SQL Server], results processing
- results [SQL Server Native Client]
ms.assetid: 20887ac4-f649-4e7f-92e6-f929e2e70952
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c2e88f5b81c29d249e58f972646b9335d02c31f6
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88490710"
---
# <a name="processing-results-native-client-ole-db-provider"></a>Verarbeitungsergebnisse (Native Client OLE DB-Anbieter)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Wenn ein Rowsetobjekt durch die Ausführung eines Befehls oder vom Anbieter direkt erzeugt wird, muss der Consumer die Daten im Rowset abrufen und darauf zugreifen können.  
  
 Rowsets sind die zentralen Objekte, die es dem OLE DB-Anbieter von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ermöglichen, Daten in tabellarischer Form verfügbar zu machen. Grundsätzlich ist ein Rowset ein Satz von Zeilen, in dem jede Zeile Spaltendaten aufweist. Ein Rowsetobjekt macht Schnittstellen verfügbar, z.B. **IRowset** (enthält Methoden zum sequenziellen Abrufen von Zeilen aus dem Rowset), **IAccessor** (lässt die Definition einer Gruppe von Spaltenbindungen zu, die beschreibt, wie die Tabellendaten an die Programmvariablen des Consumers gebunden werden sollen), **IColumnsInfo** (stellt Information zu den Spalten im Rowset bereit) und **IRowsetInfo** (stellt Information zum Rowset bereit).  
  
 Ein Consumer kann die **IRowset::GetData**-Methode aufrufen, um eine Datenzeile aus dem Rowset abzurufen und in einen Puffer einzufügen. Bevor **GetData** aufgerufen wird, beschreibt der Consumer den Puffer mit mehreren DBBINDING-Strukturen. Jede Bindung beschreibt, wie eine Spalte des Rowsets in einem Puffer des Consumer gespeichert werden soll, und enthält folgende Angaben:  
  
-   Ordnungszahl der Spalte (oder des Parameters), auf den sich die Bindung bezieht  
  
-   Informationen darüber, was gebunden wird (z. B. Datenwert, Länge der Daten und Bindungsstatus)  
  
-   Informationen zur Größe des Offsets im Puffer zu jedem dieser Teile  
  
-   Länge und den Typ der Datenwerte, die im Puffer des Consumers vorhanden sind  
  
 Beim Abruf der Daten bestimmt der Anbieter anhand der Informationen in jeder Bindung, wo und wie die Daten aus dem Puffer des Consumers abzurufen sind. Beim Einfügen der Daten in den Puffer des Consumers bestimmt der Anbieter anhand der Informationen in jeder Bindung, wo und wie die Daten in diesen Puffer einzufügen sind.  
  
 Nachdem die DBBINDING-Strukturen angegeben wurden, wird ein Accessor (**IAccessor::CreateAccessor**) erstellt. Ein Accessor ist eine Sammlung von Bindungen. Er dient zum Abrufen oder Einfügen von Daten in den Puffer des Consumers.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Erstellen einer SQL Server Native Client OLE DB-Anbieter Anwendung](../../relational-databases/native-client-ole-db-provider/creating-a-sql-server-native-client-ole-db-provider-application.md)   
 [Vorgehensweisen für OLE DB](../../relational-databases/native-client-ole-db-how-to/ole-db-how-to-topics.md)  
  
  
