---
title: Mapping von Oracle-Schemas zu SQL Server Schemas (oracleto SQL) | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie SSMA für Oracle-Zuordnungen zwischen Oracle-Schemas und SQL Server anpassen oder die Standardeinstellung übernehmen.
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 0edeaa08-9c5d-4e3a-bc15-b9a1f0c8a9dc
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 16c4de0bdf08be3bfdbc8644ae61d300284a0c25
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080771"
---
# <a name="mapping-oracle-schemas-to-sql-server-schemas-oracletosql"></a>Zuordnen von Oracle-Schemas zu SQL Server-Schemas (OracleToSQL)
In Oracle verfügt jede Datenbank über ein oder mehrere Schemas. Standardmäßig migriert SSMA alle Objekte in einem Oracle-Schema zu einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Datenbank mit dem Namen für das Schema. Sie können jedoch die Zuordnung zwischen Oracle-Schemas und- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Datenbanken anpassen.  
  
## <a name="oracle-and-sql-server-schemas"></a>Oracle-und SQL Server Schemas  
Eine Oracle-Datenbank enthält Schemas. Eine Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] enthält mehrere Datenbanken, von denen jede über mehrere Schemas verfügen kann.  
  
Das Oracle-Konzept eines Schemas wird dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Konzept einer Datenbank und einem ihrer Schemas zugeordnet. Beispielsweise kann Oracle ein Schema mit dem Namen **HR** aufweisen. Eine Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] kann eine Datenbank mit dem Namen **HR** aufweisen, und innerhalb dieser Datenbank sind Schemas. Ein Schema ist das **dbo** -Schema (oder das Datenbankbesitzer Schema). Standardmäßig wird die Oracle-Schema- **Personalabteilung** der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Datenbank und dem Schema **HR. dbo** zugeordnet. SSMA bezieht sich auf die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Kombination von Datenbank und Schema als Schema.  
  
Sie können die Zuordnung zwischen Oracle und [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Schemas ändern.  
  
## <a name="modifying-the-target-database-and-schema"></a>Ändern der Zieldatenbank und des Schemas  
In SSMA können Sie einem beliebigen verfügbaren Schema ein Oracle-Schema zuordnen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
**So ändern Sie die Datenbank und das Schema**  
  
1.  Wählen Sie im Oracle-metadatenexplorer **Schemas** aus.  
  
    Die Registerkarte **Schema Zuordnung** ist auch verfügbar, wenn Sie eine einzelne Datenbank, den Ordner **Schemas** oder einzelne Schemas auswählen. Die Liste auf der Registerkarte **Schema Zuordnung** ist für das ausgewählte Objekt angepasst.  
  
2.  Klicken Sie im rechten Bereich auf die Registerkarte **Schema Zuordnung** .  
  
    Es wird eine Liste aller Oracle-Schemas gefolgt von einem Zielwert angezeigt. Dieses Ziel ist in einer zwei teiligen Schreibweise (*Database. Schema*) angegeben, in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] der die Objekte und Daten migriert werden.  
  
3.  Wählen Sie die Zeile aus, die die Zuordnung enthält, die Sie ändern möchten, und klicken Sie dann auf **ändern**.  
  
    Im Dialogfeld **Ziel Schema auswählen** können Sie nach verfügbaren Ziel Datenbanken und-Schemas suchen oder die Datenbank und den Schema Namen in das Textfeld in einer zwei teiligen Schreibweise (Database. Schema) eingeben und dann auf **OK** klicken.  
  
4.  Das Ziel ändert sich auf der Registerkarte **Schema Zuordnung** .  
  
**Karten Modi**  
  
-   Zuordnung zu SQL Server  
  
Sie können Quell Datenbanken einer beliebigen Zieldatenbank zuordnen. Standardmäßig ist die Quelldatenbank der Ziel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Datenbank zugeordnet, mit der Sie eine Verbindung mit SSMA hergestellt haben. Wenn die zugeordnete Zieldatenbank nicht in vorhanden ist [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , wird eine Meldung angezeigt, dass **die Datenbank und/oder das Schema in den Ziel Metadaten nicht vorhanden sind [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Es würde während der Synchronisierung erstellt werden. Möchten Sie den Vorgang fortsetzen? "** Klicken Sie auf „Ja“. Entsprechend können Sie das Schema einem nicht vorhandenen Schema unter der Zieldatenbank zuordnen, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] das während der Synchronisierung erstellt wird.  
  
## <a name="reverting-to-the-default-database-and-schema"></a>Wiederherstellen der Standarddatenbank und des Standard Schemas  
Wenn Sie die Zuordnung zwischen einem Oracle-Schema und einem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Schema anpassen, können Sie die Zuordnung auf die Standardwerte zurücksetzen.  
  
**So setzen Sie die Standarddatenbank und das Standardschema zurück**  
  
1.  Wählen Sie auf der Registerkarte Schema Zuordnung eine beliebige Zeile aus, und klicken Sie auf **Standard zurücksetzen** , um die Standarddatenbank und das Standardschema wiederherzustellen.  
  
## <a name="next-steps"></a>Nächste Schritte  
Wenn Sie die Konvertierung von Oracle-Objekten in Objekte analysieren möchten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , können Sie [einen Konvertierungs Bericht erstellen](assessing-oracle-schemas-for-conversion-oracletosql.md). Andernfalls können Sie [die Definitionen der Oracle-Datenbankobjekte](converting-oracle-schemas-oracletosql.md) in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Objekt Definitionen konvertieren.  
  
## <a name="see-also"></a>Weitere Informationen  
[Herstellen einer Verbindung mit SQL Server &#40;oracleto SQL-&#41;](../../ssma/oracle/connecting-to-sql-server-oracletosql.md)  
[Migrieren von Oracle-Datenbanken zu SQL Server &#40;oracleto SQL-&#41;](../../ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql.md)  
  
