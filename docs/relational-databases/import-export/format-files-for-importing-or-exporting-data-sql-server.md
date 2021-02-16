---
title: Formatdateien zum Importieren und Exportieren von Daten
description: Beim Massenimport von Daten in eine SQL Server-Tabelle oder beim Massenexport aus einer Tabelle können in einer Formatdatei Informationen zum Feldformat einer Datendatei relativ zu einer Tabelle gespeichert werden.
ms.date: 09/25/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- bulk exporting [SQL Server], format files
- bulk importing [SQL Server], format files
- format files [SQL Server]
ms.assetid: b7b97d68-4336-4091-aee4-1941fab568e3
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: 35650bcd31127dcf3eaad46be4c958bc8f94547e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351381"
---
# <a name="format-files-to-import-or-export-data-sql-server"></a>Formatdateien zum Importieren oder Exportieren von Daten (SQL Server)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Beim Massenimportieren bzw. -exportieren von Daten in eine bzw. aus einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Tabelle können Sie eine *Formatdatei* verwenden, um alle für den Massenimport oder -export erforderlichen Informationen zu speichern. Dazu zählen Formatinformationen für jedes Feld einer Datendatei in Bezug auf die betreffende Tabelle.

[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] unterstützt zwei Typen von Formatdateien: XML-Formatdateien und Nicht-XML-Formatdateien. Sowohl Nicht-XML-Formatdateien als auch XML-Formatdateien enthalten Beschreibungen jedes Felds in einer Datendatei. XML-Formatdateien enthalten darüber hinaus auch Beschreibungen der entsprechenden Tabellenspalten. Im Allgemeinen sind XML-Formatdateien und Nicht-XML-Formatdateien austauschbar. Es empfiehlt sich jedoch, für neue Formatdateien die XML-Syntax zu verwenden, weil sich im Vergleich zu Nicht-XML-Formatdateien mehrere Vorteile ergeben. Weitere Informationen finden Sie unter [XML-Formatdateien &#40;SQL Server&#41;](../../relational-databases/import-export/xml-format-files-sql-server.md).

## <a name="benefits-of-format-files"></a><a name="Benefits"></a> Vorteile von Formatdateien

- Formatdateien stellen ein flexibles System für das Schreiben von Datendateien bereit, die für die Kompatibilität mit anderen Datenformaten und zum Lesen von Datendateien aus anderen Softwareprogrammen nur geringfügig oder gar nicht bearbeitet werden müssen.
- Durch Verwenden einer Formatdatei ist es möglich, Daten per Massenimport zu kopieren, ohne überflüssige Daten hinzufügen oder löschen oder vorhandene Daten in der Datendatei neu anordnen zu müssen. Formatdateien sind besonders hilfreich, wenn Felder in der Datendatei und Spalten in der Tabelle nicht übereinstimmen.

## <a name="examples-of-format-files"></a><a name="ExamplesOfFFs"></a> Beispiele für Formatdateien

Die folgenden Beispiele zeigen das Layout einer Nicht-XML-Formatdatei und einer XML-Formatdatei. Diese Formatdateien entsprechen der `HumanResources.myTeam` -Tabelle in der [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] -Beispieldatenbank. Diese Tabelle enthält vier Spalten: `EmployeeID`, `Name`, `Title`und `ModifiedDate`.

> [!NOTE]
> Informationen zu dieser Tabelle und zum Erstellen der Tabelle finden Sie unter [HumanResources.myTeam-Beispieltabelle & #40;SQL Server& #41;](../../relational-databases/import-export/humanresources-myteam-sample-table-sql-server.md).

### <a name="a-using-a-non-xml-format-file"></a>A. Verwenden einer Nicht-XML-Formatdatei

Die folgende Nicht-XML-Formatdatei verwendet das systemeigene Datenformat von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] für die Tabelle `HumanResources.myTeam` . Diese Formatdatei wurde mithilfe des folgenden `bcp` -Befehls erstellt.

```cmd
bcp AdventureWorks.HumanResources.myTeam format nul -f myTeam.Fmt -n -T
The contents of this format file are as follows: 9.0
4
1       SQLSMALLINT   0       2       ""   1     EmployeeID               ""  
2       SQLNCHAR      2       100     ""   2     Name                     SQL_Latin1_General_CP1_CI_AS  
3       SQLNCHAR      2       100     ""   3     Title                    SQL_Latin1_General_CP1_CI_AS  
4       SQLNCHAR      2       100     ""   4     Background               SQL_Latin1_General_CP1_CI_AS  
```  

Weitere Informationen finden Sie unter [Nicht-XML-Formatdateien &#40;SQL Server&#41;](../../relational-databases/import-export/non-xml-format-files-sql-server.md)unterstützt wird.

### <a name="b-using-an-xml-format-file"></a>B. Verwenden einer XML-Formatdatei

Die folgende XML-Formatdatei verwendet das systemeigene Datenformat von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] für die Tabelle `HumanResources.myTeam` . Diese Formatdatei wurde mithilfe des folgenden `bcp` -Befehls erstellt.

```cmd
bcp AdventureWorks.HumanResources.myTeam format nul -f myTeam.Xml -x -n -T
```

Die Formatdatei enthält Folgendes:

```xml
<?xml version="1.0"?>
<BCPFORMAT xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
<RECORD>
  <FIELD ID="1" xsi:type="NativePrefix" LENGTH="1"/>
  <FIELD ID="2" xsi:type="NCharPrefix" PREFIX_LENGTH="2" MAX_LENGTH="100" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
  <FIELD ID="3" xsi:type="NCharPrefix" PREFIX_LENGTH="2" MAX_LENGTH="100" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
  <FIELD ID="4" xsi:type="NCharPrefix" PREFIX_LENGTH="2" MAX_LENGTH="100" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
 </RECORD>
 <ROW>
  <COLUMN SOURCE="1" NAME="EmployeeID" xsi:type="SQLSMALLINT"/>
  <COLUMN SOURCE="2" NAME="Name" xsi:type="SQLNVARCHAR"/>
  <COLUMN SOURCE="3" NAME="Title" xsi:type="SQLNVARCHAR"/>
  <COLUMN SOURCE="4" NAME="Background" xsi:type="SQLNVARCHAR"/>
</ROW>
</BCPFORMAT>
```

Weitere Informationen finden Sie unter [XML-Formatdateien &#40;SQL Server&#41;](../../relational-databases/import-export/xml-format-files-sql-server.md). 

## <a name="when-is-a-format-file-required"></a><a name="WhenFFrequired"></a> Wann ist eine Formatdatei erforderlich?

- Eine INSERT ... SELECT * FROM OPENROWSET(BULK...)-Anweisung erfordert stets eine Formatdatei.
- Für **bcp** oder BULK INSERT ist in einfachen Situationen das Verwenden einer Formatdatei optional und selten notwendig. In komplexen Massenimportsituationen ist jedoch oft eine Formatdatei erforderlich.

Formatdateien sind in folgenden Fällen erforderlich:

- Dieselbe Datendatei wird als Quelle für mehrere Tabellen mit unterschiedlichen Schemas verwendet.
- Die Datendatei hat eine andere Anzahl von Feldern, als die Zieltabelle Spalten hat. Beispiel:

  - Die Zieltabelle enthält mindestens eine Spalte, für die entweder ein Standardwert definiert oder NULL zulässig ist.
  - Den Benutzern fehlen für mindestens eine Spalte in der Tabelle die SELECT/INSERT-Berechtigungen.
  - Eine einzige Datendatei wird für mindestens zwei Tabellen mit unterschiedlichen Schemas verwendet.

- Die Spaltenreihenfolge ist bei der Datendatei und der Tabelle unterschiedlich.
- Die abschließenden Zeichen oder Präfixlängen weichen bei den Spalten der Datendatei ab.

> [!NOTE]
> Wenn keine Formatdatei vorhanden und für einen **bcp** -Befehl ein Datenformatschalter angegeben ist ( **-n**, **-c**, **-w** oder **-N**) oder für einen BULK INSERT-Vorgang die Option DATAFILETYPE angegeben ist, wird das angegebene Datenformat als Standardmethode zum Interpretieren der Felder der Datendatei verwendet.

## <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben

- [Erstellen einer Formatdatei &#40;SQL Server&#41;](../../relational-databases/import-export/create-a-format-file-sql-server.md)
- [Massenimport von Daten mithilfe einer Formatdatei &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md)
- [Überspringen einer Tabellenspalte mithilfe einer Formatdatei &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)
- [Auslassen eines Datenfelds mithilfe einer Formatdatei &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)
- [Verwenden einer Formatdatei zum Zuordnen von Tabellenspalten zu Datendateifeldern &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)

## <a name="see-also"></a>Weitere Informationen

- [Nicht-XML-Formatdateien &#40;SQL Server&#41;](../../relational-databases/import-export/non-xml-format-files-sql-server.md)
- [XML-Formatdateien &#40;SQL Server&#41;](../../relational-databases/import-export/xml-format-files-sql-server.md)
- [Datenformate für Massenimport oder Massenexport &#40;SQL Server&#41;](../../relational-databases/import-export/data-formats-for-bulk-import-or-bulk-export-sql-server.md)
