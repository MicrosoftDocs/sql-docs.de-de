---
description: Volltextindex (Dialogfeld) (Visual Database Tools)
title: Das Dialogfeld „Volltextindex“
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- vdt.dlgbox.fulltextindex
ms.assetid: ef45b585-2567-4abe-b421-9fd0994e0146
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: 566239ac238afb66650c50cd9d96c8fa81d3f70c
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88497143"
---
# <a name="full-text-index-dialog-box-visual-database-tools"></a>Volltextindex (Dialogfeld) (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Mit diesem Dialogfeld können Sie einen Volltextindex für Volltextsuchen in textbasierten Spalten von Datenbanktabellen erstellen. Da ein Volltextindex auf einem regulären Index basiert, müssen Sie zuerst einen regulären Index erstellen. Der reguläre Index muss für eine einzelne Nicht-NULL-Spalte erstellt werden. Am besten wählen Sie eine Spalte mit kleinen Werten anstatt einer Spalte mit großen Werten aus.  
  
> [!NOTE]
> Bevor Sie einen Volltextindex erstellen können, müssen Sie mithilfe eines externen Tools wie [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder Enterprise Manager einen Volltextkatalog für die Datenbank erstellen.  
> 
> [!NOTE]
> Eine Volltextindex-Funktionalität ist nicht in jeder Edition von [!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]verfügbar. Eine Liste der Funktionen, die von den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Editionen unterstützt werden, finden Sie unter [Von den SQL Server 2012-Editionen unterstützte Funktionen](https://msdn.microsoft.com/5da61ff5-12b9-48e6-b3c8-0dacca1751c4).  
  
## <a name="options"></a>Optionen  
**Ausgewählter Volltextindex**  
Listet vorhandene Volltextindizes auf. Wählen Sie einen Index aus, um seine Eigenschaften im Datenblatt rechts anzuzeigen. Wenn die Liste leer ist, wurden bisher keine Volltextbeziehungen für die Tabelle definiert.  
  
**Add (Hinzufügen)**  
Erstellen Sie einen neuen Volltextindex.  
  
**Löschen**  
Löschen Sie den in der Liste **Ausgewählter Volltextindex** ausgewählten Volltextindex.  
  
**Kategorie Allgemein**  
Wenn die Kategorie erweitert ist, werden **Spalten** und **Volltextkatalogname**angezeigt.  
  
**Spalten**  
Zeigt eine durch Trennzeichen getrennte Liste mit den Namen volltextdurchsuchbarer Spalten an. Klicken Sie auf die Schaltfläche mit den Auslassungspunkten ( **…** ) links neben dem Eigenschaftenfeld, um die Liste vollständig anzuzeigen.  
  
**Full-Text Catalog Name**  
Zeigt den Namen des Volltextkatalogs an, für den dieser Volltextindex gespeichert wird. Um den Index für einen anderen Katalog zu speichern, klicken Sie auf den Katalognamen, und wählen Sie aus der Dropdownliste den gewünschten Katalog aus.  
  
> [!NOTE]  
> Der Katalog muss vorher in einem externen Tool (z. B. [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder Enterprise Manager) erstellt werden.  
  
**Kategorie Identität**  
Wenn die Kategorie erweitert ist, wird das Feld Name für den Index angezeigt.  
  
**Name**  
Zeigt den systemspezifischen Namen für diesen Volltextindex an.  
  
**Kategorie Tabellen-Designer**  
Wenn die Kategorie erweitert ist, werden Eigenschaften angezeigt, die das Verhalten des Indexes festlegen.  
  
**Aktiv**  
Gibt an, ob Sie zurzeit mit diesem Volltextindex eine Volltextsuche ausführen können.  
  
**Einstellung für das Nachverfolgen von Änderungen**  
Beschreibt den Status der Änderungsnachverfolgung für den Index: Manuell, Automatisch oder Aus.  
  
**Die Durchforstung ist abgeschlossen**  
Zeigt an, ob die zuletzt gestartete Durchforstung abgeschlossen ist. Wenn der Wert dieser Eigenschaft No ist, wird gerade eine Durchforstung ausgeführt.  
  
**Enddatum und -uhrzeit der aktuellen oder letzten Durchforstung**  
Zeigt Abschlussdatum und -zeit der letzten Durchforstung an.  
  
**Fehler in der aktuellen oder letzten Durchforstung**  
Zeigt die Anzahl der Fehler in der aktuellen oder letzten Durchforstung an.  
  
**Indexversion**  
Zeigt die Schemaversion der Tabelle zum Zeitpunkt des Durchforstungsstarts an.  
  
**Zeilen in der aktuellen oder letzten Durchforstung**  
Zeigt die Anzahl der aktualisierten Zeilen in der aktuellen oder letzten Durchforstung an.  
  
**Startdatum und -uhrzeit der aktuellen oder letzten Durchforstung**  
Zeigt Startdatum und -zeit der aktuellen oder letzten Durchforstung an.  
  
**Timestamp für nächste Durchforstung**  
Zeigt Datum und Zeit des nächsten Durchforstungsstarts an.  
  
**Typ der aktuellen oder letzten Durchforstung**  
Zeigt den Typ der aktuellen oder letzten Durchforstung an: Vollständig, Inkrementell, Aktualisieren oder Automatische Propagierung.  
  
**Eindeutiger Indexname**  
Zeigt eine Liste der Namen aller Spalten in dieser Datenbank an, die einen eindeutigen, einspaltigen Index haben. Diese Spalten können verwendet werden, um einen Volltextindex zu erstellen.  
  
## <a name="see-also"></a>Weitere Informationen  
[Verwenden des Volltextindizierungs-Assistenten](https://msdn.microsoft.com/3e9d9605-6525-4781-9168-fdaa06db3459)  
[CREATE FULLTEXT INDEX (Transact-SQL)](https://msdn.microsoft.com/8b80390f-5f8b-4e66-9bcc-cabd653c19fd)  
  
