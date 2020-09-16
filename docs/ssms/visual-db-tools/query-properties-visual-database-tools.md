---
description: Abfrageeigenschaften (Visual Database Tools)
title: Abfrageeigenschaften
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- vdtsql.chm:69636
- vdt.ppg.querydesigner.query
ms.assetid: 07495669-6ed5-4004-904e-aae1230be5e4
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: ca80e60a35b9eccff0ada18854d4f8a33ddaa9dd
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88445978"
---
# <a name="query-properties-visual-database-tools"></a>Abfrageeigenschaften (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Diese Eigenschaften werden im Eigenschaftenfenster angezeigt, wenn im Abfrage- und Sicht-Designer eine Abfrage geöffnet ist. Sofern nichts anderes angegeben ist, können Sie diese Eigenschaften im Eigenschaftenfenster bearbeiten.  
  
> [!NOTE]  
> Die Eigenschaften in diesem Thema sind nicht alphabetisch, sondern nach Kategorie sortiert.  
  
## <a name="options"></a>Optionen  
**Kategorie Identität**  
Erweitern Sie das Element, um die **Name** -Eigenschaft anzuzeigen.  
  
**Name**  
Zeigt den Namen der aktuellen Abfrage an. Kann in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]nicht geändert werden.  
  
**Database Name**  
Zeigt den Namen der Datenquelle für die ausgewählte Tabelle an.  
  
**Servername**  
Zeigt den Namen des Servers für diese Datenquelle an.  
  
**Kategorie Abfrage-Designer**  
Wird erweitert, um die restlichen Eigenschaften anzuzeigen.  
  
**Zieltabelle**  
Geben Sie den Namen der Tabelle an, in welche die Daten eingefügt werden sollen. Diese Liste wird angezeigt, wenn Sie eine INSERT-Abfrage oder MAKE TABLE-Abfrage erstellen. Für eine INSERT-Abfrage wählen Sie eine Tabelle aus der Liste aus.  
  
Für eine MAKE TABLE-Abfrage geben Sie den Namen der neuen Tabelle ein. Geben Sie zum Erstellen einer Zieltabelle in einer anderen Datenquelle einen vollqualifizierten Tabellennamen an, der den Namen der Zieldatenquelle, den Besitzer (falls erforderlich) und den Namen der Tabelle enthält.  
  
> [!NOTE]  
> Der Abfrage-Designer überprüft nicht, ob der Name bereits verwendet wird oder ob Sie über eine Berechtigung zum Erstellen der Tabelle verfügen.  
  
**DISTINCT-Werte**  
Geben Sie an, dass die Abfrage alle Duplikate im Resultset herausfiltert. Diese Option ist hilfreich, wenn Sie nur einige Spalten aus der Tabelle bzw. den Tabellen verwenden und diese möglicherweise doppelte Werte enthalten, oder wenn die Verknüpfung von mehreren Tabellen eine Verdopplung von Zeilen im Resultset zur Folge hat. Das Aktivieren dieser Option ist gleichbedeutend mit dem Einfügen des Worts DISTINCT in die Anweisung im SQL-Bereich.  
  
**GROUP BY-Erweiterung**  
Geben Sie an, dass zusätzliche Optionen für Abfragen verfügbar sind, die auf Aggregatabfragen basieren. (Gilt nur für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].)  
  
**Alle Spalten ausgeben**  
Geben Sie an, dass sämtliche Spalten aller Tabellen in der aktuellen Abfrage in das Resultset aufgenommen werden. Das Aktivieren dieser Option ist gleichbedeutend mit der Angabe eines Sternchens (*) anstelle einzelner Spaltennamen nach dem Schlüsselwort SELECT in der SQL-Anweisung.  
  
**Abfrageparameterliste**  
Zeigt Abfrageparameter an. Klicken Sie auf die Eigenschaft und dann auf die Auslassungspunkte **(...)** rechts neben der Eigenschaft, um die Parameter zu bearbeiten. (Gilt nur für generische OLE DB-Datenquellen.)  
  
**SQL-Kommentar**  
Zeigt eine Beschreibung der SQL-Anweisungen an. Klicken Sie auf die Beschreibung, und klicken Sie anschließend auf die Auslassungspunkte **(...)** rechts neben der Eigenschaft, um die ganze Beschreibung anzuzeigen oder sie zu bearbeiten. Kommentare können Informationen darüber enthalten, wer die Abfrage verwendet und wann sie verwendet wird. (Gilt nur für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7.0-Datenbanken oder höher.)  
  
**Kategorie Oberste Angabe**  
Erweitern Sie das Element, um die Eigenschaften für **Oben**, **Prozent**, **Ausdruck**und **WITH TIES** anzuzeigen.  
  
**(Nach oben)**  
Geben Sie an, dass die Abfrage eine TOP-Klausel enthält, die bewirkt, dass nur die ersten *n* Zeilen oder ersten *n* Prozent der Zeilen im Resultset zurückgegeben werden. In der Standardeinstellung gibt die Abfrage die ersten 10 Zeilen im Resultset zurück.  
  
Verwenden Sie dieses Feld, um die Anzahl der zurückzugebenden Zeilen zu ändern oder einen anderen Prozentwert anzugeben. (Gilt nur für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder höher.)  
  
**Ausdruck**  
Geben Sie die Anzahl oder den Prozentsatz der Zeilen an, die bzw. der von der Abfrage zurückgeben wird. Wenn Sie für **Prozent** Ja festlegen, wird mit dieser Zahl der Prozentsatz der Zeilen festgelegt, der von der Abfrage zurückgegeben wird. Wenn Sie für **Prozent** Nein festlegen, steht diese Zahl für die Anzahl der zurückzugebenden Zeilen. (Gilt nur für Datenbanken von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7.0 oder höher.)  
  
**Prozent**  
Geben Sie an, dass die Abfrage nur die ersten *n* Prozent der Zeilen im Resultset zurückgibt. (Gilt nur für Datenbanken von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7.0 oder höher.)  
  
**WITH TIES**  
Geben Sie an, dass die Sicht eine WITH TIES-Klausel enthält. Diese Klausel ist hilfreich, wenn eine Sicht sowohl eine ORDER BY-Klausel als auch eine TOP-Klausel mit Prozentangabe enthält. Wenn diese Option festgelegt ist und der Prozentbereich in der Mitte einer Zeilenfolge mit identischen Werten in der ORDER BY-Klausel endet, wird die Sicht bis ans Ende der betreffenden Zeilenfolge erweitert. (Gilt nur für Datenbanken von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7.0 oder höher.)  
  
## <a name="see-also"></a>Weitere Informationen  
[Erstellen von Abfragen mit Parametern &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/query-with-parameters-visual-database-tools.md)  
[Themen zur Vorgehensweise: Entwerfen von Abfragen und Sichten &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/design-queries-and-views-how-to-topics-visual-database-tools.md)  
  
