---
description: Funktionsweise von Onlineindexvorgängen
title: Funktionsweise von Onlineindexvorgängen | Microsoft-Dokumentation
ms.custom: ''
ms.date: 02/17/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- online index operations
- source indexes [SQL Server]
- pre-existing indexes [SQL Server]
- target indexes [SQL Server]
- temporary mapping index [SQL Server]
- index temporary mappings [SQL Server]
ms.assetid: eef0c9d1-790d-46e4-a758-d0bf6742e6ae
author: MikeRayMSFT
ms.author: mikeray
ms.prod_service: database-engine, sql-database
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 17f070e71fe485b49eebdb441a75b809d701068f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351214"
---
# <a name="how-online-index-operations-work"></a>Funktionsweise von Onlineindexvorgängen
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  In diesem Thema werden die während eines Onlineindexvorgangs vorhandenen Strukturen definiert und die mit diesen Strukturen verbundenen Aktivitäten aufgezeigt.  
  
## <a name="online-index-structures"></a>Onlineindexstrukturen  
 Es werden folgende Strukturen während des Onlineindexvorgangs verwendet, um gleichzeitige Benutzeraktivitäten während eines Index-DDL-Vorgangs (Data Definition Language, Datendefinitionssprache) zu ermöglichen: Quellindizes und bereits vorhandene Indizes, Zielindizes sowie ein temporärer Zuordnungsindex zum Neuerstellen eines Heaps oder Löschen eines gruppierten Indexes im Onlinemodus.  
  
-   **Quellindizes und bereits vorhandene Indizes**  
  
     Als Quelle werden die Daten der ursprünglichen Tabelle oder des gruppierten Indexes bezeichnet. Bereits vorhandene Indizes sind sämtliche nicht gruppierten Indizes, die der Quellstruktur zugeordnet sind. Wenn der Onlineindexvorgang beispielsweise einen gruppierten Index neu erstellt, dem vier nicht gruppierte Indizes zugeordnet sind, stellen die vorhandenen gruppierten Indizes die Quelle dar, und die bereits vorhandenen Indizes sind die nicht gruppierten Indizes.  
  
     Die bereits vorhandenen Indizes stehen den gleichzeitigen Benutzern für Auswahl-, Einfüge-, Update- und Löschvorgänge zur Verfügung. Hierzu zählen auch Masseneinfügungen (wir unterstützt aber nicht empfohlen) und implizite Updates durch Trigger und Einschränkungen der referenziellen Integrität. Alle bereits vorhandenen Indizes sind für Abfragen und Suchvorgänge verfügbar. Dies bedeutet, dass sie vom Abfrageoptimierer ausgewählt und gegebenenfalls in Indexhinweisen angegeben werden können.  
  
-   **Ziel**  
  
     Das Ziel bzw. die Ziele stellen den neuen Index (Heap) oder eine Gruppe neuer Indizes dar, die erstellt oder neu erstellt wird. Einfüge-, Update- und Löschvorgänge durch den Benutzer an der Quelle werden von [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] während des Indexvorgangs auf das Ziel angewendet. Wenn der Onlineindexvorgang beispielsweise einen gruppierten Index neu erstellt, ist der neu erstellte gruppierte Index das Ziel, [!INCLUDE[ssDE](../../includes/ssde-md.md)] erstellt nicht gruppierte Indizes nicht neu, wenn ein gruppierter Index neu erstellt wird.  
  
     Der Zielindex wird während der Verarbeitung von SELECT-Anweisungen erst durchsucht, nachdem ein Commit für den Vorgang ausgeführt wurde. Intern wird der Index als schreibgeschützt markiert.  
  
-   **Temporärer Zuordnungsindex**  
  
     Onlineindexvorgänge, die einen gruppierten Index erstellen, löschen oder neu erstellen, benötigen auch einen temporären Zuordnungsindex. Dieser temporäre Index wird von gleichzeitigen Transaktionen verwendet, um zu bestimmen, welche Datensätze in den neuen Indizes gelöscht werden sollen, die erstellt werden, wenn Zeilen in der zugrunde liegenden Tabelle aktualisiert oder gelöscht werden. Dieser nicht gruppierte Index wird im gleichen Schritt erstellt wie der neue gruppierte Index (oder Heap) und erfordert keinen separaten Sortiervorgang. Gleichzeitige Transaktionen behalten zudem den temporären Zuordnungsindex in all ihren Einfüge-, Update- und Löschvorgängen bei.  
  
## <a name="online-index-activities"></a>Onlineindexaktivitäten  
 Während eines einfachen Onlineindexvorgangs, wie beispielsweise der Erstellung eines gruppierten Indexes für eine nicht gruppierte Tabelle (Heap), durchlaufen die Quelle und das Ziel drei Phasen: Vorbereitung, Erstellung und Endphase.  
  
 Die folgende Abbildung zeigt den Prozess der Onlineerstellung eines anfänglichen gruppierten Indexes. Das Quellobjekt (der Heap) weist keine anderen Indizes auf. Die Aktivitäten der Quell- und Zielstrukturen werden für die einzelnen Phasen dargestellt. Außerdem werden gleichzeitige Auswahl-, Einfüge-, Update- und Löschvorgänge von Benutzern angezeigt. Die Vorbereitungs-, Erstellungs- und Endphase werden zusammen mit dem in der jeweiligen Phase verwendeten Sperrmodus angezeigt.  
  
 ![Während eines Onlineindexvorgangs ausgeführte Aktionen](../../relational-databases/indexes/media/online-index.gif "Während eines Onlineindexvorgangs ausgeführte Aktionen")  
  
## <a name="source-structure-activities"></a>Quellstrukturaktivitäten  
 Die folgende Tabelle enthält die Aktivitäten in Bezug auf die Quellstrukturen während der einzelnen Phasen des Indexvorgangs und die entsprechende Sperrstrategie.  
  
|Phase|Quellaktivität|Quellsperren|  
|-----------|---------------------|------------------|  
|Vorbereitung<br /><br /> Kurze Phase|Vorbereitung der Systemmetadaten auf die Erstellung der neuen leeren Indexstruktur.<br /><br /> Es wird eine Momentaufnahme der Tabelle definiert. Das heißt, dass mithilfe der Zeilenversionsverwaltung eine Lesekonsistenz auf Transaktionsebene ermöglicht wird.<br /><br /> Gleichzeitige Schreibvorgänge an der Quelle durch Benutzer werden für einen kurzen Zeitraum gesperrt.<br /><br /> Es werden mit Ausnahme der Erstellung mehrerer nicht gruppierter Indizes keine gleichzeitigen DDL-Vorgänge zugelassen.|S (Shared) für Tabelle*<br /><br /> Beabsichtigte freigegebene Sperre (Intent Shared, IS)<br /><br /> INDEX_BUILD_INTERNAL_RESOURCE\*\*|  
|Entwickeln<br /><br /> Hauptphase|Die Daten werden gescannt, sortiert, zusammengeführt und in Massenladevorgängen in das Ziel eingefügt.<br /><br /> Gleichzeitige Auswahl-, Einfüge-, Update- und Löschvorgänge durch Benutzer werden sowohl auf die bereits vorhandenen Indizes als auch auf sämtliche neu erstellten Indizes angewendet.|IS<br /><br /> INDEX_BUILD_INTERNAL_RESOURCE**|  
|Endphase<br /><br /> Kurze Phase|Alle Transaktionen ohne Commit müssen abgeschlossen werden, bevor diese Phase gestartet wird. Je nach der eingerichteten Sperre werden alle neuen Lese- und Schreibtransaktionen durch Benutzer für einen kurzen Zeitraum gesperrt, bis diese Phase abgeschlossen ist.<br /><br /> Sie Systemmetadaten werden aktualisiert; d. h., die Quelle wurde durch das Ziel ersetzt.<br /><br /> Die Quelle wird gelöscht, falls dies erforderlich ist. Beispielsweise nach dem erneuten Erstellen oder dem Löschen eines gruppierten Indexes.|INDEX_BUILD_INTERNAL_RESOURCE**<br /><br /> S für die Tabelle, sofern ein nicht gruppierter Index erstellt wird.\*<br /><br /> Schemaänderungssperre (SCH-M, Schema Modification), sofern eine Quellstruktur (Index oder Tabelle) gelöscht wird.\*|  
  
 \* Der Indexvorgang wartet, bis Updatetransaktionen ohne Commit abgeschlossen sind, bevor er die S- oder SCH-M-Sperre für die Tabelle einrichtet. Wenn eine zeitintensive Abfrage durchgeführt wird, wartet der Onlineindexvorgang, bis die Abfrage abgeschlossen ist.
  
 ** Die Ressourcensperre INDEX_BUILD_INTERNAL_RESOURCE verhindert die Ausführung gleichzeitiger DDL-Vorgänge für die Quelle und bereits vorhandene Strukturen, während der Indexvorgang ausgeführt wird. Diese Sperre verhindert beispielsweise die gleichzeitige Neuerstellung zweier Indizes für dieselbe Tabelle. Obwohl diese Ressourcensperre der Sch-M-Sperre zugeordnet ist, verhindert sie keine Datenbearbeitungsanweisungen.  
  
 Die vorherige Tabelle zeigt eine einzelne freigegebene Sperre (Shared, S) an, die während der Erstellungsphase eines Onlineindexvorgangs, der einen einzelnen Index einschließt, eingerichtet wurde. Wenn gruppierte und nicht gruppierte Indizes erstellt oder neu erstellt werden, werden bei einem einzelnen Onlineindexvorgang (z. B. während der anfänglichen Erstellung eines gruppierten Indexes für eine Tabelle, die einen oder mehrere nicht gruppierte Indizes enthält) zwei kurzfristige S-Sperren gefolgt von langfristigen beabsichtigten gemeinsamen Sperren (IS-Sperren) während der Erstellungsphase eingerichtet. Eine S-Sperre wird zunächst für die Erstellung des gruppierten Indexes eingerichtet, und wenn die Erstellung des gruppierten Indexes abgeschlossen ist, wird eine zweite kurzfristige S-Sperre für die Erstellung der nicht gruppierten Indizes eingerichtet. Nachdem die nicht gruppierten Indizes erstellt wurden, wird die S-Sperre bis zur Endphase des Onlineindexvorgangs auf eine IS-Sperre herabgestuft.  

Weitere Informationen zur Verwendung und Verwaltung von Sperren finden Sie unter [Argumente](../../t-sql/statements/alter-table-index-option-transact-sql.md#arguments).

### <a name="target-structure-activities"></a>Zielstrukturaktivitäten  
 Die folgende Tabelle enthält die Aktivitäten in Bezug auf die Zielstrukturen während der einzelnen Phasen des Indexvorgangs und die entsprechende Sperrstrategie.  
  
|Phase|Zielaktivität|Zielsperren|  
|-----------|---------------------|------------------|  
|Vorbereitung|Neuer Index wird erstellt und auf schreibgeschützt festgelegt.|IS|  
|Entwickeln|Daten werden aus der Quelle eingefügt.<br /><br /> Änderungen durch den Benutzer (Einfügungen, Updates, Löschungen), die auf die Quelle angewendet werden, werden angewendet.<br /><br /> Diese Aktivität ist für den Benutzer sichtbar.|IS|  
|Endphase|Indexmetadaten werden aktualisiert.<br /><br /> Lese-/Schreibstatus wird für Index festgelegt.|E<br /><br /> oder<br /><br /> SCH-M|  
  
 Der Zugriff auf das Ziel durch SELECT-Anweisungen, die vom Benutzer ausgegeben werden, ist erst nach Abschluss des Vorgangs möglich.  
  
 Nach Abschluss der Vorbereitungs- und Endphase werden die im Prozedurcache gespeicherten Abfrage- und Updatepläne für ungültig erklärt. Folgeabfragen verwenden den neuen Index.  
  
 Die Lebensdauer eines Cursors, der für eine Tabelle deklariert wurde, die an einem Onlineindexvorgang beteiligt ist, wird durch die Onlineindexphasen begrenzt. Updatecursor werden in den einzelnen Phasen für ungültig erklärt. Schreibgeschützte Cursor werden nur nach der Endphase für ungültig erklärt.  
  
## <a name="related-content"></a>Verwandte Inhalte  
 [Ausführen von Onlineindexvorgängen](../../relational-databases/indexes/perform-index-operations-online.md)  
  
 [Richtlinien für Onlineindexvorgänge](../../relational-databases/indexes/guidelines-for-online-index-operations.md)  
  
## <a name="next-steps"></a>Nächste Schritte

[ALTER TABLE-Indexoptionen](../../t-sql/statements/alter-table-index-option-transact-sql.md#arguments)
