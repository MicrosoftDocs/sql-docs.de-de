---
description: Verknüpfen (Dialogfeld) (Visual Database Tools)
title: Dialogfeld „Join“
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- vdt.ppg.joinline
- vdtsql.chm:69638
ms.assetid: 0d9516bb-4ad3-4fcf-bb77-93474dea698f
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: 385e2a9cafa9323772068ed91872e8add00b3257
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88423134"
---
# <a name="join-dialog-box-visual-database-tools"></a>Verknüpfen (Dialogfeld) (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
In diesem Dialogfeld können Sie Optionen zum Verknüpfen von Tabellen angeben. Um das Dialogfeld aufzurufen, wählen Sie im Bereich **Entwurf** eine Joinlinie aus. Klicken Sie anschließend im Fenster **Eigenschaften** auf **Joinbedingung und -typ**, und klicken Sie auf die Auslassungspunkte **(...)**, die rechts neben der Eigenschaft angezeigt werden.  
  
In der Standardeinstellung sind verknüpfte Tabellen über eine innere Verknüpfung miteinander verbunden. Mithilfe dieser Verknüpfung wird ein Resultset auf der Grundlage von Zeilen erstellt, deren Inhalte mit den Informationen in den verknüpften Spalten übereinstimmen. Wenn Sie die Optionen im Dialogfeld **Verbinden** festlegen, können Sie einen Join angeben, die auf einem anderen Operator basiert. Außerdem können Sie einen äußeren Join festlegen.  
  
Weitere Informationen zum Verbinden von Tabellen finden Sie unter [Erstellen von Abfragen mit Joins &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/query-with-joins-visual-database-tools.md).  
  
## <a name="options"></a>Optionen  
  
|**Begriff**|**Definition**|  
|------------|------------------|  
|**Table**|Die Namen der verknüpften Tabellen bzw. Tabellenwertobjekte. Die Tabellennamen können an dieser Stelle nicht geändert werden. Sie werden lediglich zu Informationszwecken angezeigt.|  
|**Spalte**|Die Namen der Spalten, mit denen die Tabellen verknüpft werden. Mit dem Operator in der Operatorliste wird die Beziehung zwischen den Daten in den Spalten angegeben. Die Spaltennamen können an dieser Stelle nicht geändert werden. Sie werden lediglich zu Informationszwecken angezeigt.|  
|**Operator**|Geben Sie den Operator an, der die Beziehung zwischen den verknüpften Spalten bestimmt. Wenn nicht der Gleichheitsoperator (=) festgelegt werden soll, wählen Sie den gewünschten Operator aus der Liste aus. Wenn Sie die Eigenschaftenseite schließen, wird der von Ihnen ausgewählte Operator im Rautensymbol der Joinlinie wie folgt angezeigt:<br /><br />![Visual Database Tools (Symbol)](../../ssms/visual-db-tools/media/dv3wbii.gif "Visual Database Tools (Symbol)")|  
|**Alle Zeilen von <table1>**|Geben Sie an, dass alle Zeilen der linken Tabelle auch dann in der Ausgabe aufgeführt werden, wenn die rechte Tabelle keine entsprechenden Zeilen enthält. Für Spalten ohne übereinstimmende Daten in der rechten Tabelle werden keine Werte angezeigt. Das Auswählen dieser Option ist gleichbedeutend mit der Angabe von LEFT OUTER JOIN in der SQL-Anweisung.|  
|**Alle Zeilen von <table2>**|Geben Sie an, dass alle Zeilen der rechten Tabelle auch dann in der Ausgabe aufgeführt werden, wenn die linke Tabelle keine entsprechenden Zeilen enthält. Für Spalten ohne übereinstimmende Daten in der linken Tabelle werden keine Werte angezeigt. Das Auswählen dieser Option ist gleichbedeutend mit der Angabe von RIGHT OUTER JOIN in der SQL-Anweisung.|  
  
Wenn Sie sowohl **Alle Zeilen von <table1>** als auch **Alle Zeilen von <table2>** auswählen, entspricht dies der Angabe von FULL OUTER JOIN in der SQL-Anweisung.  
  
Wenn eine Option zum Erstellen eines äußeren Joins angegeben wird, ändert sich das Rautensymbol in der Joinlinie, um anzuzeigen, dass es sich beim Join um einen linken äußeren, rechten äußeren oder vollständigen äußeren Join handelt.  
  
> [!NOTE]  
> Die Wörter "links" und "rechts" müssen nicht unbedingt der Position der Tabellen im Diagrammbereich entsprechen. "Links" bezieht sich auf die Tabelle, die in der SQL-Anweisung links vom Schlüsselwort JOIN genannt wird, und "rechts" bezieht sich dementsprechend auf die Tabelle zur Rechten des Schlüsselworts JOIN. Ein Verschieben der Tabellen im Bereich **Diagramm** hat keinen Einfluss darauf, welche Tabelle als links oder rechts behandelt wird.  
  
## <a name="see-also"></a>Weitere Informationen  
[Erstellen von Abfragen mit Joins &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/query-with-joins-visual-database-tools.md)  
[Themen zur Vorgehensweise: Entwerfen von Abfragen und Sichten &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/design-queries-and-views-how-to-topics-visual-database-tools.md)  
  
