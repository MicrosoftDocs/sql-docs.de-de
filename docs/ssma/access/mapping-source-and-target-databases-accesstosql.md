---
title: Zuordnung von Quell-und Ziel Datenbanken (accesstosql) | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Zieldatenbank für den Zugriff auf die Datenbankmigration zu SQL Server oder Azure SQL-Datenbank angeben, einschließlich mehrerer Datenbanken zu mehreren Datenbanken
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- database schemas
- mapping, databases
- schemas, mapping to
- schemas, SQL Azure
- schemas, SQL Server
- source database
- target database
ms.assetid: 69bee937-7b2c-49ee-8866-7518c683fad4
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 966ec1472737f0e4d67615d4e7eb65df01a52cf4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100059046"
---
# <a name="mapping-source-and-target-databases-accesstosql"></a>Zuordnung von Quell-und Ziel Datenbanken (accesstosql)
Beim Herstellen einer Verbindung mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder SQL Azure müssen Sie eine Zieldatenbank für die Migration angeben. Wenn Sie über mehrere Access-Datenbanken verfügen, können Sie diese mehreren [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Datenbanken (oder Schemas) oder mehreren Schemas unter der verbundenen Azure SQL-Datenbank zuordnen.  
  
## <a name="sql-server-or-azure-sql-database-schemas"></a>SQL Server-oder Azure SQL-Datenbankschemas  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datenbanken verwenden das Konzept von Schemas, um Objekte innerhalb einer Datenbank in logische Gruppen zu trennen. Eine Bibliotheks Datenbank könnte z. b. drei Schemas namens **Bücher**, **Audiodaten** und **Videos** verwenden, um Buch-, Audio-und Video Objekte voneinander zu trennen. Standardmäßig wird die Access-Datenbank der **Master** -Datenbank und dem **dbo** -Schema in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und der verbundenen Datenbank und dem **dbo** -Schema in SQL Azure zugeordnet.  
  
Wenn Sie die Zuordnung zwischen den einzelnen Zugriffs Datenbanken und der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Datenbank und dem Schema nicht anpassen, werden alle Schemas und Daten, die der Access-Datenbank zugeordnet sind, von SSMA der Standarddatenbank zugeordnet.  
  
## <a name="modifying-the-target-database-and-schema"></a>Ändern der Zieldatenbank und des Schemas  
Mit SSMA können Sie jede Access-Datenbank der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Azure SQL-Datenbank zuordnen. Im folgenden Verfahren wird beschrieben, wie die Zuordnung pro Datenbank angepasst wird.  
  
**So ändern Sie die Zieldatenbank und das Ziel Schema**  
  
1.  Wählen Sie im Bereich Access Metadata Explorer den Eintrag **Access-Metadata** aus.  
  
    Die Schema Zuordnung ist auch verfügbar, wenn Sie den Knoten **Datenbanken** oder einen beliebigen Datenbankknoten auswählen. Die Schema Zuordnungsliste ist für das ausgewählte Objekt angepasst.  
  
2.  Klicken Sie im rechten Bereich auf die Registerkarte **Schema Zuordnung** .  
  
    Sie sehen eine Tabelle, die Access-Datenbanknamen und die zugehörigen ssNoVersion-oder SQL Azure-Schemas enthält. Das Ziel Schema wird in einer zwei teiligen Schreibweise (Database. Schema) bezeichnet.  
  
3.  Wählen Sie die Zeile aus, die die Zuordnung enthält, die Sie anpassen möchten, und klicken Sie dann auf **ändern**.  
  
4.  Im Dialogfeld **Ziel Schema auswählen** können Sie nach verfügbaren Ziel Datenbanken und-Schemas suchen oder die Datenbank und den Schema Namen in das Textfeld in einer zwei teiligen Schreibweise (Database. Schema) eingeben und dann auf **OK** klicken.  
  
**Karten Modi**  
  
-   Zuordnung zu SQL Server  
  
Sie können Quell Datenbanken einer beliebigen Zieldatenbank zuordnen. Standardmäßig ist die Quelldatenbank der Ziel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Datenbank zugeordnet, mit der Sie eine Verbindung mit SSMA hergestellt haben. Wenn die zugeordnete Zieldatenbank nicht in vorhanden [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ist, wird die Meldung angezeigt, dass **die Datenbank und/oder das Schema in den Ziel Metadaten nicht vorhanden sind [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Es würde während der Synchronisierung erstellt werden. Möchten Sie den Vorgang fortsetzen? "** Klicken Sie auf „Ja“. Entsprechend können Sie das Schema einem nicht vorhandenen Schema unter der Zieldatenbank zuordnen, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] das während der Synchronisierung erstellt wird.  
  
-   Zuordnung zu SQL Azure  
  
Sie können die Quelldatenbank der verbundenen Ziel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Datenbank oder dem beliebigen Schema in der verbundenen Ziel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Datenbank zuordnen. Wenn Sie das Quell Schema einem nicht vorhandenen Schema unter der verbundenen Zieldatenbank zuordnen, wird die Meldung angezeigt, dass **das Schema in den Ziel Metadaten nicht vorhanden ist. Es würde während der Synchronisierung erstellt werden. Möchten Sie den Vorgang fortsetzen? "** Klicken Sie auf Ja.  
  
## <a name="reverting-to-your-initial-database-and-schema"></a>Wiederherstellen der ursprünglichen Datenbank und des ursprünglichen Schemas  
Wenn Sie die Zuordnung zwischen einer Access-Datenbank und einer- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder einer Azure SQL-Datenbank anpassen, können Sie die Zuordnung auf die Datenbank zurücksetzen, die Sie beim Herstellen einer Verbindung mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder SQL Azure angegeben haben.  
  
**So setzen Sie die Standarddatenbank und das Standardschema zurück**  
  
1.  Wählen Sie auf der Registerkarte Schema Zuordnung eine beliebige Zeile aus, und klicken Sie auf **Standard zurücksetzen** , um die Standarddatenbank und das Standardschema wiederherzustellen.  
  
## <a name="next-step"></a>Nächster Schritt  
Der nächste Schritt des Migrations Vorgangs ist das [umstellen von Datenbankobjekten](converting-access-database-objects-accesstosql.md) .  
  
## <a name="see-also"></a>Weitere Informationen  
[Migration von Access-Datenbanken zu SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
  
