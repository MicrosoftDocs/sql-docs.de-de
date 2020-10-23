---
description: Azure SQL DW-Uploadtask
title: Azure SQL DW-Uploadtask | Microsoft-Dokumentation
ms.custom: ''
ms.date: 12/16/2016
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- SQL13.DTS.DESIGNER.AFPDWUPTASK.F1
- sql14.dts.designer.afpdwuptask.f1
ms.assetid: eef82c89-228a-4dc7-9bd0-ea00f57692f5
author: Lingxi-Li
ms.author: lingxl
ms.openlocfilehash: c0864f868cc046fcd1f0763fff7e5a97e2fe8607
ms.sourcegitcommit: a5398f107599102af7c8cda815d8e5e9a367ce7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2020
ms.locfileid: "92006204"
---
# <a name="azure-sql-dw-upload-task"></a>Azure SQL DW-Uploadtask

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



Der **Azure SQL DW-Uploadtask** ermöglicht es einem SSIS-Paket, Tabellendaten aus dem Dateisystem oder Azure Blob Storage in Azure Synapse Analytics (DW) zu kopieren.
Der Task nutzt PolyBase zur Leistungssteigerung gemäß der Beschreibung im Artikel [Azure Synapse Analytics Loading Patterns and Strategies](/archive/blogs/sqlcat/azure-sql-data-warehouse-loading-patterns-and-strategies)(Azure Synapse Analytics: Muster und Strategien zum Laden).
Das gegenwärtig unterstützte Quelldatenformat ist Text mit Trennzeichen in UTF8-Codierung.
Beim Kopieren aus einem Dateisystem werden Daten zunächst in Azure Blob Storage für das Staging und dann in Azure SQL Data Warehouse hochgeladen. Darum ist ein Azure Blob Storage-Konto erforderlich.

Der **Azure SQL DW-Uploadtask** ist eine Komponente des [SQL Server Integration Services-Feature Packs (SSIS) für Azure](../../integration-services/azure-feature-pack-for-integration-services-ssis.md).

Um einen **Azure SQL DW-Uploadtask**hinzuzufügen, ziehen Sie ihn mittels Drag &amp; Drop aus der SSIS-Toolbox auf den Designercanvas, und doppelklicken Sie, oder klicken Sie mit der rechten Maustaste darauf, und klicken Sie anschließend auf **Bearbeiten** , um das Dialogfeld des Task-Editors anzuzeigen.

Konfigurieren Sie auf der Seite **Allgemein** die folgenden Eigenschaften.

**SourceType** gibt den Typ des Quelldatenspeichers an. Wählen Sie einen der folgenden Typen aus:

* **FileSystem:** Die Quelldaten befinden sich im lokalen Dateisystem.
* **BlobStorage:** Die Quelldaten befinden sich in Azure Blob Storage.

Im Folgenden sind die Eigenschaften für jeden Quelltyp aufgeführt.

### <a name="filesystem"></a>FileSystem

Feld|BESCHREIBUNG
-----|-----------
LocalDirectory|Gibt das lokale Verzeichnis mit den Datendateien an, die hochgeladen werden sollen.
Rekursiv|Gibt an, ob Unterverzeichnisse rekursiv durchsucht werden sollen.
FileName|Geben Sie einen Namensfilter zum Auswählen von Dateien mit einem bestimmten Namensmuster an. Beispiel: „MeinArbeitsblatt\*.xsl\* “ schließt „MeinArbeitsblatt001.xsl“ und „MeinArbeitsblattABC.xslx“ ein.
RowDelimiter|Gibt ein oder mehrere Zeichen an, die das Ende jeder Zeile markieren.
ColumnDelimiter|Gibt ein oder mehrere Zeichen an, die das Ende jeder Spalte markieren. Beispiel: &#124; (senkrechter Strich), \t (Tabulator), ' (einfaches Anführungszeichen), " (doppeltes Anführungszeichen) und 0x5c (umgekehrter Schrägstrich).
IsFirstRowHeader|Gibt an, ob die erste Zeile in jeder Datendatei Spaltennamen statt tatsächlicher Daten enthält.
AzureStorageConnection|Gibt einen Azure Storage-Verbindungs-Manager an.
BlobContainer|Gibt den Namen des Blobcontainers an, zu dem lokale Daten hochgeladen und über PolyBase an Azure DW weitergeleitet werden. Falls noch nicht vorhanden ist, wird ein neuer Container erstellt.
BlobDirectory|Gibt das Blobverzeichnis an (virtuelle Hierarchiestruktur), zu dem lokale Daten hochgeladen und über PolyBase an Azure DW weitergeleitet werden.
RetainFiles|Gibt an, ob die zu Azure Storage hochgeladenen Dateien beibehalten werden sollen.
CompressionType|Gibt an, welches Komprimierungsformat beim Hochladen von Dateien in Azure Storage verwendet werden soll. Die lokale Quelle ist nicht betroffen.
CompressionLevel|Gibt an, welcher Komprimierungsgrad für das Komprimierungsformat verwendet werden soll.
AzureDwConnection|Gibt einen ADO.NET-Verbindungs-Manager für Azure SQL Data Warehouse an.
TableName|Gibt den Namen der Zieltabelle an. Wählen Sie entweder einen vorhandenen Tabellennamen, oder erstellen Sie einen neuen durch Auswahl von **\<New Table ...>** .
TableDistribution|Gibt die Verteilungsmethode für die neue Tabelle an. Gilt, wenn für **TableName**ein neuer Tabellenname angegeben wird.
HashColumnName|Gibt an, welche Spalte für die Verteilung der Hashtabelle verwendet werden soll. Gilt, wenn **HASH** für **TableDistribution**angegeben wird.

### <a name="blobstorage"></a>BlobStorage

Feld|BESCHREIBUNG
-----|-----------
AzureStorageConnection|Gibt einen Azure Storage-Verbindungs-Manager an.
BlobContainer|Gibt den Namen des Blobcontainers an, in dem sich die Quelldaten befinden.
BlobDirectory|Gibt das Blobverzeichnis (virtuelle Hierarchiestruktur) an, in dem sich die Quelldaten befinden.
RowDelimiter|Gibt ein oder mehrere Zeichen an, die das Ende jeder Zeile markieren.
ColumnDelimiter|Gibt ein oder mehrere Zeichen an, die das Ende jeder Spalte markieren. Beispiel: &#124; (senkrechter Strich), \t (Tabulator), ' (einfaches Anführungszeichen), " (doppeltes Anführungszeichen) und 0x5c (umgekehrter Schrägstrich).
CompressionType|Gibt das für die Quelldaten verwendete Komprimierungsformat an.
AzureDwConnection|Gibt einen ADO.NET-Verbindungs-Manager für Azure SQL Data Warehouse an.
TableName|Gibt den Namen der Zieltabelle an. Wählen Sie entweder einen vorhandenen Tabellennamen, oder erstellen Sie einen neuen durch Auswahl von **\<New Table ...>** .
TableDistribution|Gibt die Verteilungsmethode für die neue Tabelle an. Gilt, wenn für **TableName**ein neuer Tabellenname angegeben wird.
HashColumnName|Gibt an, welche Spalte für die Verteilung der Hashtabelle verwendet werden soll. Gilt, wenn **HASH** für **TableDistribution**angegeben wird.

Je nachdem, ob Sie zu einer neuen oder einer vorhandenen Tabelle kopieren, sehen Sie eine andere Seite **Zuordnungen**.
Konfigurieren Sie im ersten Fall, welche Quellspalten zugeordnet werden sollen, sowie ihre entsprechenden Namen in der zu erstellenden Zieltabelle.
Konfigurieren Sie im letzten Fall die Zuordnungsbeziehungen zwischen Quell- und Zielspalten.

Konfigurieren Sie auf der Seite **Spalten** die Datentypeigenschaften für jede Quellspalte.

Die Seite **T-SQL** zeigt das zum Laden von Daten aus Azure Blob Storage in Azure SQL Data Warehouse verwendete T-SQL an.
Das T-SQL wird automatisch von Konfigurationen auf den anderen Seiten generiert und als Teil der Taskausführung ausgeführt.
Um das generierte T-SQL wahlweise nach Ihren speziellen Bedürfnissen manuell zu bearbeiten, klicken Sie auf die Schaltfläche **Bearbeiten** .
Sie können später durch Klicken auf die Schaltfläche **Zurücksetzen** das automatisch generierte wiederherstellen.