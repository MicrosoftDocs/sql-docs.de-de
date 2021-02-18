---
title: Fenster "Räumliche Ergebnisse"
description: Hier erfahren Sie, wie Sie das Fenster „Räumliche Ergebnisse“ verwenden, das visuelle Zuordnungstools zum Anzeigen räumlicher Daten bereitstellt. Ihre Abfrageergebnisse müssen eine räumliche Spalte mit Geometrie- oder Geografiedaten enthalten, um Ergebnisse für räumliche Daten anzeigen zu können.
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: c2d5a477-6496-4d01-adee-7322ebdfadf3
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b08fe5a2b40f07a6ffef93862512e5132dfbb6ae
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344153"
---
# <a name="spatial-results-window"></a>Fenster "Räumliche Ergebnisse"
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Im Fenster **Räumliche Ergebnisse** werden visuelle Zuordnungstools zum Anzeigen räumlicher Daten bereitgestellt. Zum Anzeigen von Ergebnissen für räumliche Daten müssen die Abfrageergebnisse eine räumliche Spalte mit Geometrie- oder Geografiedaten enthalten.  
  
> [!NOTE]  
>  Das Fenster **Räumliche Ergebnisse** ist nur verfügbar, wenn die Ergebnisse im Fenster **Ergebnisse** in ein Raster zurückgegeben werden. Wenn Sie angeben, dass die Ergebnisse als Text zurückgegeben werden, ist dieses Fenster nicht verfügbar.  
  
## <a name="options"></a>Tastatur  
 **Räumliche Spalte auswählen**  
 Geben Sie die räumliche Spalte an, die Sie in den Abfrageergebnissen in den räumlichen Spalten anzeigen möchten. Es kann nur jeweils eine Spalte ausgewählt werden.  
  
 **Bezeichnungsspalte auswählen**  
 Geben Sie die nicht räumliche Spalte in den Spalten an, die in den Abfrageergebnissen zurückgegeben wurden, um die räumlichen Daten zu beschriften. Es kann nur jeweils eine Spalte ausgewählt werden.  
  
 Diese Option ist nicht verfügbar, wenn in einer Abfrage nur Instanzen zurückgegeben werden.  
  
 **Projektion auswählen**  
 Geografiedaten in einer von vier Projektionen anzeigen: Equirektangular, Mercator, Robinson oder Bonne.  
  
 Diese Option ist für Geometriedaten nicht verfügbar.  
  
 **Zoom**  
 Stellen Sie die Zuordnungsanzeige auf einer exponentiellen Skala ein.  
  
 **Gitternetzlinien anzeigen**  
 Sie können Koordinatengitternetzlinien aktivieren oder deaktivieren.  
  
 Bei polygonen Formen wird die Bezeichnung nur angezeigt, wenn die Form groß genug ist, um den Bezeichnungstext aufzunehmen. Um Bezeichnungen für kleine Formen anzuzeigen, passen Sie den Zoom an.  
  
> [!NOTE]  
>  Punktinstanzen können nicht bezeichnet werden.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Anzeigen räumlicher Daten im Objekt-Explorer](./view-spatial-data-in-object-explorer.md)   
 [Räumliche Daten &#40;SQL Server&#41;](../../relational-databases/spatial/spatial-data-sql-server.md)   
 [Abfrage-Editor der Datenbank-Engine &#40;SQL Server Management Studio&#41;](../f1-help/database-engine-query-editor-sql-server-management-studio.md)   
 [Abfrage- und Text-Editoren &#40;SQL Server Management Studio&#41;](../f1-help/database-engine-query-editor-sql-server-management-studio.md)  
  
