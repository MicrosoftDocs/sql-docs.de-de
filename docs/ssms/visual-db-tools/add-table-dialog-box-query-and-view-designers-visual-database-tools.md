---
description: Tabelle hinzufügen (Dialogfeld) (Abfrage- und Sicht-Designer) (Visual Database Tools)
title: Dialogfeld „Tabelle hinzufügen“ (Abfrage- und Sichtdesigner)
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- vdt.dlgbox.query.addtable
- vdtsql.chm:65565
ms.assetid: fce7adcc-4cf5-4a52-9203-11c13d1ecf08
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.openlocfilehash: 76fcb614bfab599f6f6a9e279e8b4f9341f6209c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100270799"
---
# <a name="add-table-dialog-box-query-and-view-designers-visual-database-tools"></a>Tabelle hinzufügen (Dialogfeld) (Abfrage- und Sicht-Designer) (Visual Database Tools)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Mit diesem Dialogfeld können Sie einer Abfrage oder Sicht Tabellen, Sichten, benutzerdefinierte Funktionen oder Synonyme hinzufügen.  
  
> [!NOTE]  
> Wenn die Tabelle zur Replikation veröffentlicht ist, müssen Sie mit der Transact-SQL-Anweisung [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) oder mit SMO (SQL Server Management Objects) Schemaänderungen ausführen. Wenn die Schemaänderungen mit dem Tabellen-Designer oder dem Datenbankdiagramm-Designer ausgeführt werden, wird versucht, die Tabelle zu entfernen und erneut zu erstellen. Da veröffentlichte Objekte nicht gelöscht werden können, schlägt die Schemaänderung fehl.  
  
## <a name="options"></a>Optionen  
**Tabellen**  
Listet die Tabellen auf, die dem Bereich **Diagramm** hinzugefügt werden können. Um eine Tabelle hinzuzufügen, wählen Sie sie aus, und klicken Sie auf **Hinzufügen**. Um mehrere Tabellen gleichzeitig hinzuzufügen, wählen Sie sie aus, und klicken Sie auf **Hinzufügen**.  
  
**Ansichten**  
Listet die Sichten auf, die dem Bereich **Diagramm** hinzugefügt werden können. Um eine Sicht hinzuzufügen, wählen Sie sie aus, und klicken Sie auf **Hinzufügen**. Um mehrere Sichten gleichzeitig hinzuzufügen, wählen Sie sie aus, und klicken Sie auf **Hinzufügen**.  
  
**Funktionen**  
Listet die benutzerdefinierten Funktionen auf, die dem Bereich **Diagramm** hinzugefügt werden können. Um eine Funktion hinzuzufügen, wählen Sie sie aus, und klicken Sie auf **Hinzufügen**. Um mehrere Funktionen gleichzeitig hinzuzufügen, wählen Sie sie aus, und klicken Sie auf **Hinzufügen**.  
  
**Synonyme**  
Listet die Synonyme auf, die dem Bereich **Diagramm** hinzugefügt werden können. Um ein Synonym hinzuzufügen, wählen Sie es aus, und klicken Sie auf **Hinzufügen**. Um mehrere Synonyme gleichzeitig hinzuzufügen, wählen Sie sie aus, und klicken Sie auf **Hinzufügen**.  
  
**Aktualisieren**  
Aktualisieren Sie die Liste, sodass alle Änderungen an der Datenbank in die Liste aufgenommen werden, die seit dem letzten Abrufen der Liste vorgenommen wurden.  
  
**Add (Hinzufügen)**  
Fügen Sie die jeweils ausgewählten Elemente hinzu.  
  
## <a name="see-also"></a>Weitere Informationen  
[Themen zur Vorgehensweise: Entwerfen von Abfragen und Sichten &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/design-queries-and-views-how-to-topics-visual-database-tools.md)  
  
