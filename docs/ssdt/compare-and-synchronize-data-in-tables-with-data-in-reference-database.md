---
title: Vergleichen und Synchronisieren von Daten in Tabellen mit Daten aus einer Referenzdatenbank
description: Hier erfahren Sie, wie Sie Daten aus zwei verschiedenen Datenbanken vergleichen. Es wird gezeigt, wie Sie die Daten synchronisieren und wie Sie das für den Synchronisierungsprozess verwendete Skript anzeigen.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 96d743b0-b69a-45bb-ae0e-62103dca76e2
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/27/2020
ms.openlocfilehash: 40d09ff78845707729a94359c30d07c73df1bb77
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100079621"
---
# <a name="compare-and-synchronize-data-in-one-or-more-tables-with-data-in-a-reference-database"></a>Vergleichen und Synchronisieren von Daten in einer oder mehreren Tabellen anhand von Daten aus einer Verweisdatenbank

Sie können die Daten in einer *Quelldatenbank* und einer *Zieldatenbank* vergleichen und angeben, welche Tabellen verglichen werden sollen. Die Daten können überprüft werden, um zu bestimmen, welche Änderungen synchronisiert werden sollen. Sie können dann entweder das Ziel aktualisieren, um die Datenbanken zu synchronisieren, oder das Updateskript in den Transact\-SQL-Editor oder in eine Datei exportieren.  
  
So können Sie beispielsweise Datenbanken synchronisieren, um einen Stagingserver mit einer Kopie der Produktionsdaten zu aktualisieren. Auch können Sie einzelne oder mehrere Tabellen synchronisieren, um sie mit Referenzdaten aus einer anderen Datenbank zu versehen. Darüber hinaus können Sie Daten vor und nach dem Ausführen von Tests vergleichen, um einen zusätzlichen Überprüfungsschritt einzubauen.  
  
Sie können Daten in zwei Datenbanken vergleichen, aber keine Datenbankprojekt- oder DACPAC-Datei für einen Vergleich angeben, da diese keine Daten enthalten.  
  
Dieser Abschnitt enthält die folgenden Informationsbereiche:  
  
-   [Vorgehensweise: Vergleichen und Synchronisieren von Daten aus zwei Datenbanken](../ssdt/how-to-compare-and-synchronize-the-data-of-two-databases.md)  
  
-   [Vorgehensweise: Anzeigen von Datenunterschieden](../ssdt/how-to-view-data-differences.md)  
  
## <a name="requirements"></a>Requirements (Anforderungen)  
Bei einem Vergleich der Daten in einer Tabelle oder Sicht muss die Tabelle oder Sicht in der Quelldatenbank einige Attribute mit einer Tabelle oder Sicht in der Zieldatenbank gemeinsam haben. Tabellen und Sichten, die die folgenden Kriterien nicht erfüllen, werden nicht verglichen und auch nicht auf der zweiten Seite des Assistenten **Neuer Datenvergleich** angezeigt:  
  
-   Tabellen müssen übereinstimmende Spaltennamen mit kompatiblen Datentypen enthalten.  
  
    Bei den Namen von Tabellen, Sichten und Besitzern wird die Groß-/Kleinschreibung berücksichtigt.  
  
-   Tabellen müssen den gleichen Primärschlüssel, den gleichen eindeutigen Index oder die gleiche eindeutige Einschränkung besitzen.  
  
-   Sichten müssen den gleichen eindeutigen gruppierten Index besitzen.  
  
-   Eine Tabelle kann nur dann mit einer Sicht verglichen werden, wenn beide den gleichen Namen besitzen.  
  
Jedes Objekt besitzt einen Schlüssel oder Index, der bestimmt, welchen anderen Objekten es entspricht. Tabellen und Sichten können mehrere Primärschlüssel, eindeutige Indizes oder eindeutige Einschränkungen besitzen. Daher empfiehlt es sich unter Umständen anzugeben, welcher Schlüssel oder Index bzw. welche Einschränkung verwendet werden soll.  
  
## <a name="common-tasks"></a>Allgemeine Aufgaben  
In diesem Abschnitt werden allgemeine Aufgaben für dieses Szenario beschrieben.  
  
**Festlegen von Optionen zum Steuern des Datenvergleichs:** Beim Vergleich von Daten können Sie problemlos Identitätsspalten ignorieren sowie Trigger und Fremdschlüssel deaktivieren. Außerdem können Sie Primärschlüssel, Indizes und eindeutige Einschränkungen aus dem Updateskript entfernen.  
  
**Vergleichen Sie Daten in Tabellen, und aktualisieren Sie gegebenenfalls das Ziel, sodass es der Quelle entspricht:** Nachdem Sie eine Quell- und eine Zieldatenbank angegeben und den Vergleich ausgeführt haben, zeigen Sie die Ergebnisse im Fenster **Datenvergleich** an. Sie können nicht nur Details zu den Unterschieden anzeigen, sondern auch das Updateskript, mit dessen Hilfe Sie die Daten synchronisieren können. Nach Ermittlung der Unterschiede zwischen den beiden Datenbanken können Sie eine Aktion für die einzelnen Unterschiede angeben. So können Sie das Ziel aktualisieren oder das Updateskript in den Transact\-SQL-Editor oder in eine Datei exportieren. Das Exportieren des Skripts kann hilfreich sein, um es vor dem Anwenden der Änderungen zu überprüfen oder von einer anderen Person überprüfen zu lassen.  
  
## <a name="understanding-comparison-results"></a><a name="UnderstandingDataCompareResults"></a>Grundlegendes zu Vergleichsergebnissen  
In der folgenden Tabelle werden die fünf Spalten im Fenster **Datenvergleich** beschrieben.  
  
|Column|Notizen|  
|----------|---------|  
|Object|Enthält den Namen der Tabelle oder Sicht sowie ein Kontrollkästchen, mit dem angegeben wird, ob das Ziel beim Schreiben von Updates oder beim Exportieren des Updateskripts synchronisiert werden soll. Bei Tabellen oder Sichten ohne Daten ist das Kontrollkästchen nicht verfügbar.|  
|Unterschiedliche Datensätze|Enthält die Anzahl der Datensätze im Ziel, die zwar den gleichen Schlüssel wie die Quelle besitzen, nicht aber die gleichen Daten. Die Anzahl der Datensätze, die markiert sind, um beim Schreiben von Updates oder beim Exportieren des Updateskripts aktualisiert zu werden, ist in Klammern angegeben.|  
|Nur in der Quelle|Enthält die Anzahl der Datensätze in der Quelle, die nicht im Ziel enthalten sind. Die Anzahl der Datensätze, die markiert sind, um beim Schreiben von Updates oder beim Exportieren des Updateskripts hinzugefügt zu werden, ist in Klammern angegeben.|  
|Nur im Ziel|Enthält die Anzahl der Datensätze im Ziel, die nicht in der Quelle enthalten sind. Die Anzahl der Datensätze, die markiert sind, um beim Schreiben von Updates oder beim Exportieren des Updateskripts gelöscht zu werden, ist in Klammern angegeben.|  
|Identische Datensätze|Enthält die Anzahl der Datensätze im Ziel, die den gleichen Schlüssel sowie die gleichen Daten wie die Quelle besitzen. Diese Datensätze werden beim Schreiben von Updates oder beim Exportieren des Updateskripts nicht aktualisiert.|  
  
### <a name="table-and-view-details"></a>Details zu Tabellen und Sichten  
Nach dem Klicken auf eine Tabelle oder Sicht im Fenster **Datenvergleich** werden im Detailbereich alle in der Tabelle oder Sicht enthaltenen Zeilen angezeigt. Jede Registerkarte im Detailbereich zeigt eine andere Kategorie (Unterschiedliche Datensätze, Nur in der Quelle, Nur im Ziel oder Identische Datensätze). Für jede Zeile kann das entsprechende Kontrollkästchen aktiviert oder deaktiviert werden, um anzugeben, ob die jeweilige Änderung in das Updateskript eingefügt werden soll.  
  
## <a name="see-also"></a>Weitere Informationen  
[SQL Server Data Tools](../ssdt/sql-server-data-tools.md)  
[Vorgehensweise: Vergleichen verschiedener Datenbankdefinitionen mithilfe des Schemavergleichs](../ssdt/how-to-use-schema-compare-to-compare-different-database-definitions.md)  
  
