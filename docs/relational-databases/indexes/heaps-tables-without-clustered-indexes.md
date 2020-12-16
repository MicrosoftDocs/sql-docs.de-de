---
description: Heaps (Tabellen ohne gruppierte Indizes)
title: Heaps (Tabellen ohne gruppierte Indizes) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 11/01/2016
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- heaps
- forward record
- forwarded record
- forwarding pointer
- RID
ms.assetid: df5c4dfb-d372-4d0f-859a-a2d2533ee0d7
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: be2a84e21cc55340d675041cdd353dab887531c2
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465291"
---
# <a name="heaps-tables-without-clustered-indexes"></a>Heaps (Tabellen ohne gruppierte Indizes)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Ein Heap ist eine Tabelle ohne gruppierten Index. Ein oder mehrere nicht gruppierte Indizes können für Tabellen erstellt werden, die als Heap gespeichert sind. Daten werden ohne bestimmte Reihenfolge im Heap gespeichert. Normalerweise werden Daten anfänglich in der Reihenfolge gespeichert, in der die Zeilen in die Tabelle eingefügt werden. [!INCLUDE[ssDE](../../includes/ssde-md.md)] kann die Daten jedoch im Heap verschieben, um die Zeilen effizienter zu speichern; daher kann die Reihenfolge der Daten nicht vorhergesagt werden. Um die von einem Heap zurückgegebene Zeilenreihenfolge zu garantieren, müssen Sie die `ORDER BY`-Klausel verwenden. Erstellen Sie einen gruppierten Index für die Tabelle, damit die Tabelle kein Heap ist, um eine permanente logische Reihenfolge für die Speicherung von Zeilen festzulegen.  
  
> [!NOTE]  
> In bestimmten Fällen gibt es gute Gründe, eine Tabelle als einen Heap zu belassen, statt einen gruppierten Index zu erstellen. Die effektive Verwendung von Heaps ist jedoch Benutzern mit fortgeschrittenen Kenntnissen vorbehalten. Die meisten Tabellen sollten über einen sorgfältig ausgewählten gruppierten Index verfügen, es sei denn, es gibt gute Gründe, die Tabelle als Heap beizubehalten.  
  
## <a name="when-to-use-a-heap"></a>Verwendungsbereiche für Heaps  
Wenn eine Tabelle als Heap gespeichert wird, werden einzelne Zeilen durch einen 8-Byte-Zeilenbezeichner (RID, Row Identifier) gekennzeichnet, der aus der Dateinummer, der Datenseitennummer und einem Slot auf der Seite (FileID:PageID:SlotID) besteht. Die Zeilen-ID ist eine kleine und effiziente Struktur. 

Heaps können als Stagingtabellen für große, unsortierte Einfügevorgänge verwendet werden. Da beim Einfügen von Daten keine strikte Reihenfolge erzwungen wird, ist der Einfügevorgang hier in der Regel schneller als beim Einfügen in einen gruppierten Index. Wenn die Heapdaten gelesen und im endgültigen Ziel verarbeitet werden, kann es hilfreich sein, einen schmalen, nicht gruppierten Index mit dem Suchprädikat der Leseabfrage zu erstellen. 

> [!NOTE]  
> Die Daten werden aus einem Heap in der Reihenfolge der Datenseiten abgerufen. Diese muss jedoch nicht notwendigerweise mit der Reihenfolge übereinstimmen, in der die Daten eingefügt wurden. 

In einigen Fällen verwenden Datenexperten Heaps, wenn immer über nicht gruppierte Indizes auf Daten zugegriffen wird und die RID kleiner als der Schlüssel eines gruppierten Indexes ist. 

Wenn eine Tabelle ein Heap ist und nicht über nicht gruppierte Indizes verfügt, muss die gesamte Tabelle gelesen werden (mit einem Tabellenscan), um eine Zeile zu finden. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] kann die RID nicht direkt auf dem Heap suchen. Dies kann akzeptabel sein, wenn die Tabelle klein ist.  
  
## <a name="when-not-to-use-a-heap"></a>Keine Verwendungsbereiche für Heaps  
 Verwenden Sie keinen Heap, wenn die Daten häufig in einer sortierten Reihenfolge zurückgegeben werden. Mit einem gruppierten Index für die Sortierspalte kann der Sortiervorgang vermieden werden.  
  
 Verwenden Sie keinen Heap, wenn die Daten häufig zusammen gruppiert werden. Daten müssen vor dem Gruppieren sortiert werden, und ein gruppierter Index für die Sortierspalte kann den Sortiervorgang vermeiden.  
  
 Verwenden Sie keinen Heap, wenn häufig Datenbereiche aus der Tabelle abgefragt werden. Mit einem gruppierten Index für die Bereichsspalte kann der Sortiervorgang für den gesamten Heap vermieden werden.  
  
 Verwenden Sie keinen Heap, wenn keine nicht gruppierten Indizes vorliegen und die Tabelle groß ist, es sei denn, Sie planen, den gesamten Tabelleninhalt ohne festgelegte Reihenfolge zurückzugeben. In einem Heap müssen alle Zeilen des Heaps gelesen werden, um eine Zeile zu finden.  
 
 Verwenden Sie keinen Heap, wenn die Daten regelmäßig aktualisiert werden. Wenn Sie einen Datensatz aktualisieren und dafür mehr Speicherplatz in den Datenseiten benötigen, als diese derzeit verwenden, muss der Datensatz auf eine Datenseite verschoben werden, die über ausreichend freien Speicherplatz verfügt. Dadurch wird ein **weitergeleiteter Datensatz** erstellt, der auf den neuen Speicherort der Daten verweist, und ein **weiterleitender Zeiger** muss in die Seite geschrieben werden, die die Daten zuvor enthalten hat, um den neuen physischen Ort anzugeben. Dies führt zu Fragmentierung im Heap. Beim Scannen eines Heaps muss diesen Zeigern gefolgt werden, was die Leistung beim Vorauslesen einschränkt und zu zusätzlichen E/A-Vorgängen führen kann, wodurch wiederum die Leistung des Scans reduziert wird. 
  
## <a name="managing-heaps"></a>Verwalten von Heaps  
 Erstellen Sie zum Anlegen eines Heaps eine Tabelle ohne gruppierten Index. Alternativ dazu können Sie den gruppierten Index löschen, wenn die Tabelle bereits über einen gruppierten Index verfügt, damit die Tabelle wieder ein Heap ist.  
  
 Erstellen Sie zum Löschen eines Heaps auf dem Heap einen gruppierten Index.  
  
 So erstellen Sie einen Heap neu, um nicht verwendeten Speicherplatz freizugeben:
 -  Erstellen Sie auf dem Heap einen gruppierten Index, und löschen Sie den ihn dann wieder.  
 -  Verwenden Sie den Befehl `ALTER TABLE ... REBUILD`, um den Heap neu zu erstellen.
  
> [!WARNING]  
> Das Erstellen oder Löschen von gruppierten Indizes erfordert, dass die gesamte Tabelle neue geschrieben werden muss. Wenn die Tabelle über nicht gruppierte Indizes verfügt, müssen alle nicht gruppierten Indizes immer dann neu erstellt werden, wenn der gruppierte Index geändert wird. Aus diesem Grund kann es sehr lange dauern, von einem Heap zu einem gruppierten Index zu wechseln und umgekehrt, sowie viel Festplattenspeicher zum Neuordnen der Daten in tempdb erfordern.  

## <a name="heap-structures"></a>Heapstrukturen
Ein Heap ist eine Tabelle ohne gruppierten Index. Heaps haben eine Zeile in [sys.partitions](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md), mit `index_id = 0` für jede vom Heap verwendete Partition. Standardmäßig verfügt ein Heap über eine einzelne Partition. Wenn ein Heap über mehrere Partitionen verfügt, hat jede Partition eine Heapstruktur, in der die Daten für die jeweilige Partition enthalten sind. Wenn ein Heap z. B. über vier Partitionen verfügt, gibt es vier Heapstrukturen – jeweils eine in jeder Partition.

Je nach den im Heap enthaltenen Datentypen weist jede Heapstruktur eine oder mehrere Zuordnungseinheiten auf, um die Daten für eine bestimmte Partition zu speichern und zu verwalten. Zumindest verfügt jeder Heap über eine `IN_ROW_DATA` -Zuordnungseinheit pro Partition. Der Heap hat außerdem eine `LOB_DATA` -Zuordnungseinheit pro Partition, wenn diese LOB-Spalten (Large OBject) enthält. Außerdem ist eine `ROW_OVERFLOW_DATA` -Zuordnungseinheit pro Partition vorhanden, wenn der Index Spalten variabler Länge aufweist, die die Zeilengrößenbegrenzung von 8.060 Byte übersteigen.

Die Spalte `first_iam_page` in der Systemsicht `sys.system_internals_allocation_units` verweist auf die erste IAM-Seite in der Kette der IAM-Seiten, die den Speicherplatz auf dem Heap in einer bestimmten Partition verwalten. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwendet die IAM-Seiten, um den Heap zu durchlaufen. Die Datenseiten und die Zeilen innerhalb eines Heaps weisen keine bestimmte Reihenfolge auf und sind nicht verknüpft. Die einzige logische Verbindung zwischen den Datenseiten sind die Informationen, die auf den IAM-Seiten aufgezeichnet sind.

> [!IMPORTANT]  
> Die Systemsicht `sys.system_internals_allocation_units` ist nur für die interne Verwendung von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vorgesehen. Zukünftige Kompatibilität wird nicht sichergestellt.
 
Tabellenscans oder serielle Lesevorgänge in einem Heap können durchgeführt werden, indem die IAM-Seiten gescannt werden, um auf diese Weise die Blöcke zu ermitteln, die Seiten des Heaps enthalten. Da die IAM die Blöcke in derselben Reihenfolge darstellt, in der sie in der Datendatei vorliegen, werden serielle Heapscans immer sequenziell durch jede Datei ausgeführt. Das Verwenden der IAM-Seiten zum Festlegen der Scanfolge bedeutet weiterhin, dass Zeilen aus dem Heap nicht notwendigerweise in der Reihenfolge zurückgegeben werden, in der sie eingefügt wurden.

Die folgende Abbildung zeigt, wie in [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] die IAM-Seiten zum Abrufen von Datenzeilen in einem Heap mit einer einzelnen Partition verwendet werden. 

![iam_heap](../../relational-databases/indexes/media/iam-heap.gif)
  
## <a name="related-content"></a>Verwandte Inhalte  
[CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)     
[DROP INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/drop-index-transact-sql.md)     
[Beschreibung von gruppierten und nicht gruppierten Indizes](../../relational-databases/indexes/clustered-and-nonclustered-indexes-described.md)     
  
  
