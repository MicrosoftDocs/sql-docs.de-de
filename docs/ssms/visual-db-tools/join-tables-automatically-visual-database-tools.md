---
description: Automatisches Verknüpfen von Tabellen (Visual Database Tools)
title: Automatisches Verknüpfen von Tabellen
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- automatic joins
- joins [SQL Server], creating
- joins [SQL Server], automatic
ms.assetid: f152af82-bcb6-49ca-af19-48cdb7fc9ac6
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: 3efc96cafe9a7759834591473007dd3818b16e25
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88462796"
---
# <a name="join-tables-automatically-visual-database-tools"></a>Automatisches Verknüpfen von Tabellen (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
 Wenn Sie einer Abfrage zwei oder mehr Tabellen hinzufügen, versucht der [Abfrage- und Sicht-Designer](../../ssms/visual-db-tools/query-and-view-designer-tools-visual-database-tools.md) zu ermitteln, ob diese verknüpft sind. Wenn dies der Fall ist, zieht der Abfrage- und Sicht-Designer automatisch Joinlinien zwischen den Rechtecken, die die Tabellen bzw. Objekte mit Tabellenstruktur darstellen.  
  
Der Abfrage- und Sicht-Designer erkennt Tabellen unter den folgenden Umständen als verknüpft:  
  
-   Die Datenbank enthält Informationen, die angeben, dass die Tabellen verknüpft sind.  
  
-   Wenn zwei Spalten aus jeweils einer Tabelle denselben Namen und denselben Datentyp aufweisen. Die Spalte muss in mindestens einer der Tabellen ein Primärschlüssel sein. Wenn Sie z. B. die Tabellen `employee` und `jobs` hinzufügen, wobei die `job_id` -Spalte in der Tabelle `jobs` der Primärschlüssel ist und beide Tabellen eine `job_id` -Spalte mit demselben Datentyp enthalten, verknüpft der Abfrage- und Sicht-Designer die Tabellen automatisch.  
  
    > [!NOTE]  
    > Der Abfrage- und Sicht-Designer erstellt für Spalten mit demselben Namen und demselben Datentyp nur einen Join. Wenn mehrere Joins möglich sind, beendet der Abfrage- und Sicht-Designer den Vorgang nach dem Erstellen eines Joins, der auf dem ersten gefundenen Satz übereinstimmender Spalten basiert.  
  
-   Der Abfrage- und Sicht-Designer erkennt, dass eine Suchbedingung (eine WHERE-Klausel) eigentlich eine Joinbedingung ist. Sie fügen z. B. die Tabellen `employee` und `jobs`hinzu und erstellen anschließend eine Suchbedingung, die in der `job_id` -Spalte beider Tabellen nach demselben Wert sucht. Wenn Sie diesen Vorgang ausführen, erkennt der Abfrage- und Sicht-Designer, dass die Suchbedingung zu einem Join führt und erstellt anschließend eine Joinbedingung auf der Grundlage der Suchbedingung.  
  
Wenn der Abfrage- und Sicht-Designer einen Join erstellt hat, der für die Abfrage nicht geeignet ist, können Sie den Join ändern oder entfernen. Ausführliche Informationen finden Sie unter [Ändern von Joinoperatoren &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/modify-join-operators-visual-database-tools.md) und [Entfernen von Joins &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/remove-joins-visual-database-tools.md).  
  
Wenn der Abfrage- und Sicht-Designer die Tabellen in der Abfrage nicht automatisch verknüpft, können Sie selbst einen Join erstellen. Ausführliche Informationen finden Sie unter [Manuelles Verknüpfen von Tabellen &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/join-tables-manually-visual-database-tools.md).  
  
## <a name="see-also"></a>Weitere Informationen  
[Darstellungsweise von Joins im Abfrage- und Sicht-Designer &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/how-the-query-and-view-designer-represents-joins-visual-database-tools.md)  
[Themen zur Vorgehensweise: Entwerfen von Abfragen und Sichten &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/design-queries-and-views-how-to-topics-visual-database-tools.md)  
[Erstellen von Abfragen mit Joins &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/query-with-joins-visual-database-tools.md)  
  
