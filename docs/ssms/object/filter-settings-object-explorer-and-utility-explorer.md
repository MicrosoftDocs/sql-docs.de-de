---
description: Filtereinstellungen (Objekt-Explorer und Hilfsprogramm-Explorer)
title: Filtereinstellungen (Objekt-Explorer und Hilfsprogramm-Explorer)
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.swb.common.filtersettings.f1
- sql13.ag.job.filtersettings.f1
ms.assetid: 4aab04bc-e1ab-4d4b-ab74-b287fc805bc2
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 50a7eccca6f889a7bff57b244d8c209af7b73ec2
ms.sourcegitcommit: 22dacedeb6e8721e7cdb6279a946d4002cfb5da3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2020
ms.locfileid: "92037203"
---
# <a name="filter-settings-object-explorer-and-utility-explorer"></a>Filtereinstellungen (Objekt-Explorer und Hilfsprogramm-Explorer)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Mithilfe dieses Dialogfelds können Sie einen Filter angeben. Mit einem Filter können Sie Objekt-Explorer und Hilfsprogramm-Explorer so konfigurieren, dass nur Elemente angezeigt werden, die bestimmte Kriterien erfüllen. So können Sie mit einem Filter beispielsweise nur Aufträge mit Namen anzeigen lassen, die das Wort "Wartung" enthalten. Die Kopfzeile für das Dialogfeld **Filtereinstellungen** enthält den Namen des Servers und kann den Namen der Datenbank enthalten.  
  
## <a name="ui-element-list"></a>Liste der Benutzeroberflächenelemente  
**Eigenschaft**  
Zeigt die Eigenschaft an, nach der gefiltert werden soll.  
  
**Operator**  
Wählen Sie die Art aus, in der der Filter den Wert auf die Eigenschaft anwendet. Folgende Optionen stehen zur Verfügung:  
  
-   **Ist gleich**  
  
    Der Filter zeigt Elemente an, bei denen die Eigenschaft und der Wert genau übereinstimmen.  
  
-   **Contains**  
  
    Der Filter zeigt die Elemente an, bei denen die Eigenschaft den Wert enthält. Die Eigenschaft kann noch weiteren Text enthalten.  
  
-   **Enthält nicht**  
  
    Der Filter zeigt die Elemente an, bei denen die Eigenschaft den Wert nicht enthält.  
  
-   **Kleiner als**  
  
    Verfügbar für Datumsangaben. Dieser Filter zeigt Elemente an, deren Datum vor dem eingegebenen Wert liegt.  
  
-   **Kleiner oder gleich**  
  
    Verfügbar für Datumsangaben. Dieser Filter zeigt Elemente an, deren Datum vor dem eingegebenen Wert liegt bzw. genau dem Wert entspricht.  
  
-   **Größer als**  
  
    Verfügbar für Datumsangaben. Dieser Filter zeigt Elemente an, deren Datum nach dem eingegebenen Wert liegt.  
  
-   **Größer oder gleich**  
  
    Verfügbar für Datumsangaben. Dieser Filter zeigt Elemente an, deren Datum nach dem eingegebenen Wert liegt bzw. genau dem Wert entspricht.  
  
-   **Zwischen**  
  
    Verfügbar für Datumsangaben. Dieser Filter zeigt Elemente an, deren Datum zwischen den eingegebenen Datumsangaben liegt. Wählen Sie **Zwischen** aus, und drücken Sie TAB, um eine weitere Zeile für die Eingabe des zweiten Datums hinzuzufügen.  
  
-   **Nicht zwischen**  
  
    Verfügbar für Datumsangaben. Dieser Filter zeigt Elemente an, deren Datum vor oder nach den beiden eingegebenen Datumsangaben liegt. Wählen Sie **Nicht zwischen** aus, und fügen Sie der **Operator** -Spalte mit TAB eine neue Zeile für die Eingabe des zweiten Datums hinzu.  
  
**Wert**  
Geben Sie den Wert ein, mit dem die Eigenschaft verglichen werden soll. Bei Datumsangaben klicken Sie auf den nach unten zeigenden Pfeil, um einen Kalender für die Auswahl des Datums anzuzeigen.  
  
**Filter löschen**  
Entfernt alle aktuellen Filtereinstellungen.  
  
## <a name="see-also"></a>Weitere Informationen  
[Verwenden von SQL Server Management Studio](../sql-server-management-studio-ssms.md)  
[Übersicht über das SQL Server-Hilfsprogramm](../../relational-databases/manage/sql-server-utility-features-and-tasks.md)  
