---
description: Tabelle aktualisieren (Dialogfeld) (Visual Database Tools)
title: Dialogfeld „Tabelle aktualisieren“
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- vdt.dlgbox.updatetable
- vdtsql.chm:69643
ms.assetid: 174c7275-5b15-42a9-b172-5ff30de575a1
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.openlocfilehash: 93c0ed1368325ec27ff9ebb640f48ee45f7de8ea
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88369176"
---
# <a name="update-table-dialog-box-visual-database-tools"></a>Tabelle aktualisieren (Dialogfeld) (Visual Database Tools)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Mit diesem Dialogfeld können Sie die Tabelle angeben, die aktualisiert werden soll.

Dieses Dialogfeld wird angezeigt, wenn beim Ändern eines Abfragetyps in eine Updateabfrage mehr als eine Tabelle im Diagrammbereich angezeigt wird.  

Wählen Sie die zu aktualisierende Tabelle aus, und klicken Sie dann auf **OK**.

> [!NOTE]
> Wenn die Tabelle zur Replikation veröffentlicht ist, müssen Sie mit der Transact-SQL-Anweisung [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) oder mit SMO (SQL Server Management Objects) Schemaänderungen ausführen. Wenn die Schemaänderungen mit dem Tabellen-Designer oder dem Datenbankdiagramm-Designer ausgeführt werden, wird versucht, die Tabelle zu entfernen und erneut zu erstellen. Da veröffentlichte Objekte nicht gelöscht werden können, schlägt die Schemaänderung fehl.

## <a name="see-also"></a>Weitere Informationen

[Erstellen von Updateabfragen](../../ssms/visual-db-tools/create-update-queries-visual-database-tools.md)
