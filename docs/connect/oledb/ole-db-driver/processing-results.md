---
title: Verarbeiten von Ergebnissen (OLE DB-Treiber)
description: Erfahren Sie, wie ein Consumer des OLE DB-Treibers für SQL Server Daten in einem Rowset, das von einem Befehl oder Anbieter generiert wurde, abruft und darauf zugreift.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- OLE DB Driver for SQL Server, results processing
- OLE DB, processing results
- rowsets [SQL Server], results processing
- results [OLE DB Driver for SQL Server]
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3cea6c2ef9cfdab250966cbd809be44e6506e4dc
ms.sourcegitcommit: c95f3ef5734dec753de09e07752a5d15884125e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2020
ms.locfileid: "88861531"
---
# <a name="processing-results"></a>Ergebnisverarbeitung
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Wenn ein Rowsetobjekt durch die Ausführung eines Befehls oder vom Anbieter direkt erzeugt wird, muss der Consumer die Daten im Rowset abrufen und darauf zugreifen können.  
  
 Rowsets sind die zentralen Objekte, die es dem OLE DB-Treiber für SQL Server ermöglichen, Daten in tabellarischer Form verfügbar zu machen. Grundsätzlich ist ein Rowset ein Satz von Zeilen, in dem jede Zeile Spaltendaten aufweist. Ein Rowsetobjekt macht Schnittstellen verfügbar, z.B. **IRowset** (enthält Methoden zum sequenziellen Abrufen von Zeilen aus dem Rowset), **IAccessor** (lässt die Definition einer Gruppe von Spaltenbindungen zu, die beschreibt, wie die Tabellendaten an die Programmvariablen des Consumers gebunden werden sollen), **IColumnsInfo** (stellt Information zu den Spalten im Rowset bereit) und **IRowsetInfo** (stellt Information zum Rowset bereit).  
  
 Ein Consumer kann die **IRowset::GetData**-Methode aufrufen, um eine Datenzeile aus dem Rowset abzurufen und in einen Puffer einzufügen. Bevor **GetData** aufgerufen wird, beschreibt der Consumer den Puffer mit mehreren DBBINDING-Strukturen. Jede Bindung beschreibt, wie eine Spalte des Rowsets in einem Puffer des Consumer gespeichert werden soll, und enthält folgende Angaben:  
  
-   Ordnungszahl der Spalte (oder des Parameters), auf den sich die Bindung bezieht  
  
-   Informationen darüber, was gebunden wird (z. B. Datenwert, Länge der Daten und Bindungsstatus)  
  
-   Informationen zur Größe des Offsets im Puffer zu jedem dieser Teile  
  
-   Länge und den Typ der Datenwerte, die im Puffer des Consumers vorhanden sind  
  
 Beim Abruf der Daten bestimmt der Anbieter anhand der Informationen in jeder Bindung, wo und wie die Daten aus dem Puffer des Consumers abzurufen sind. Beim Einfügen der Daten in den Puffer des Consumers bestimmt der Anbieter anhand der Informationen in jeder Bindung, wo und wie die Daten in diesen Puffer einzufügen sind.  
  
 Nachdem die DBBINDING-Strukturen angegeben wurden, wird ein Accessor (**IAccessor::CreateAccessor**) erstellt. Ein Accessor ist eine Sammlung von Bindungen. Er dient zum Abrufen oder Einfügen von Daten in den Puffer des Consumers.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Creating an OLE DB Driver for SQL Server Application (Erstellen eines OLE DB-Treibers für eine SQL Server-Anwendung)](../../oledb/ole-db-driver/creating-a-oledb-driver-for-sql-server-application.md)   
 [Vorgehensweisen für OLE DB](../../oledb/ole-db-how-to/ole-db-how-to-topics.md)  
  
  
