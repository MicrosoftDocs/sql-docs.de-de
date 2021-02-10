---
description: Zuordnen von Sybase ASE-Schemas zu SQL Server-Schemas (SybaseToSQL)
title: Zuordnung von Sybase-ASE-Schemas zu SQL Server Schemata (sybaseto SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Schema Mapping
ms.assetid: 2c927003-c49d-4fe1-8e3e-5b2899166268
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: a0084551b2e1ab6de4c3731f33525ccd3a8a48d5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017522"
---
# <a name="mapping-sybase-ase-schemas-to-sql-server-schemas-sybasetosql"></a>Zuordnen von Sybase ASE-Schemas zu SQL Server-Schemas (SybaseToSQL)
In Sybase Adaptive Server Enterprise (ASE) verfügt jede Datenbank über ein oder mehrere Schemas. Standardmäßig migriert SSMA alle Objekte innerhalb einer Datenbank und eines Schemas in die gleiche Datenbank und das gleiche Schema in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder SQL Azure. Sie können jedoch die Zuordnung zwischen ASE und [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder Azure SQL-Datenbank anpassen.  
  
## <a name="ase-and-sql-server-or-sql-azure-schemas"></a>ASE-und SQL Server-oder SQL Azure Schemas  
ASE und [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder SQL Azure geben Datenbanken und Ihre Schemas mit zwei teiligen Schreibweise als " *Database. Schema*" an. Beispielsweise kann in einer ASE- **Demo** Datenbank ein **dbo** -Schema vorhanden sein. Diese Datenbank und das Schema paar werden als **Demo. dbo** angegeben. Wenn [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder SQL Azure dieselbe Datenbank und dasselbe Schema hat, wird das Paar auch als **Demo. dbo** angegeben.  
  
## <a name="modifying-the-target-database-and-schema"></a>Ändern der Zieldatenbank und des Schemas  
In SSMA können Sie einem beliebigen verfügbaren oder SQL Azure Schema ein ASE-Schema zuordnen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
**So ändern Sie die Datenbank und das Schema**  
  
1.  Wählen Sie im Sybase-metadatenexplorer **Datenbanken** aus.  
  
    Die Registerkarte **Schema Zuordnung** ist auch verfügbar, wenn Sie eine einzelne Datenbank, den Ordner **Schemas** oder einzelne Schemas auswählen. Die Liste auf der Registerkarte **Schema Zuordnung** ist für das ausgewählte Objekt angepasst.  
  
2.  Klicken Sie im rechten Bereich auf die Registerkarte **Schema Zuordnung** .  
  
    Es wird eine Liste aller ASE-Datenbanken mit Ihren Schemas angezeigt, gefolgt von einem Zielwert. Dieses Ziel wird in einer zwei teiligen Schreibweise (*Database. Schema*) in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder SQL Azure, in der die Objekte und Daten migriert werden.  
  
3.  Wählen Sie die Zeile aus, die die Zuordnung enthält, die Sie ändern möchten, und klicken Sie dann auf **ändern**.  
  
4.  Im Dialogfeld **Ziel Schema auswählen** können Sie nach verfügbaren Ziel Datenbanken und-Schemas suchen oder die Datenbank und den Schema Namen in das Textfeld in einer zwei teiligen Schreibweise (Database. Schema) eingeben und dann auf **OK** klicken.  
  
5.  Das Ziel ändert sich auf der Registerkarte **Schema Zuordnung** .  
  
**Karten Modi**  
  
-   Zuordnung zu SQL Server  
  
Sie können Quell Datenbanken einer beliebigen Zieldatenbank zuordnen. Standardmäßig ist die Quelldatenbank der Ziel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Datenbank zugeordnet, mit der Sie eine Verbindung mit SSMA hergestellt haben. Wenn die zugeordnete Zieldatenbank nicht in vorhanden ist [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , wird eine Meldung angezeigt, dass **die Datenbank und/oder das Schema in den Ziel Metadaten nicht vorhanden sind [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Es würde während der Synchronisierung erstellt werden. Möchten Sie den Vorgang fortsetzen? "** Klicken Sie auf „Ja“. Entsprechend können Sie das Schema einem nicht vorhandenen Schema unter der Zieldatenbank zuordnen, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] das während der Synchronisierung erstellt wird.  
  
-   Zuordnung zu SQL Azure  
  
Sie können die Quelldatenbank der verbundenen Azure SQL-Zieldatenbank oder einem beliebigen Schema in der verbundenen Azure SQL-Zieldatenbank zuordnen. Wenn Sie das Quell Schema einem nicht vorhandenen Schema unter der verbundenen Zieldatenbank zuordnen, wird die Meldung angezeigt, dass **das Schema in den Ziel Metadaten nicht vorhanden ist. Es würde während der Synchronisierung erstellt werden. Möchten Sie den Vorgang fortsetzen? "** Klicken Sie auf Ja.  
  
## <a name="reverting-to-the-default-database-and-schema"></a>Wiederherstellen der Standarddatenbank und des Standard Schemas  
Wenn Sie die Zuordnung zwischen einem ASE-Schema und einem- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder-SQL Azure Schema anpassen, können Sie die Zuordnung auf die Standardwerte zurücksetzen.  
  
**So setzen Sie die Standarddatenbank und das Standardschema zurück**  
  
1.  Wählen Sie auf der Registerkarte Schema Zuordnung eine beliebige Zeile aus, und klicken Sie auf **Standard zurücksetzen** , um die Standarddatenbank und das Standardschema wiederherzustellen.  
  
## <a name="next-steps"></a>Nächste Schritte  
Wenn Sie die Konvertierung von Sybase ASE-Objekten in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder SQL Azure-Objekte analysieren möchten, können Sie [einen Konvertierungs Bericht erstellen](assessing-sybase-ase-database-objects-for-conversion-sybasetosql.md). Andernfalls können Sie [die Objekt Definitionen der ASE-Datenbank](converting-sybase-ase-database-objects-sybasetosql.md) in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder SQL Azure Objekt Definitionen konvertieren.  
  
## <a name="see-also"></a>Weitere Informationen  
[Migrieren von Sybase ASE-Datenbanken zu SQL Server-Azure SQL-Datenbank &#40;sybaseto SQL-&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)  
  
