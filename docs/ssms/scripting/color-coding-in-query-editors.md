---
title: Farbcodierung im Abfrage-Editor
description: Hier erfahren Sie, wie Textkategorien farblich codiert werden, damit Sie bestimmte Textabschnitte einfacher finden können. Es wird ebenso erläutert, wie Sie ein benutzerdefiniertes Farbschema konfigurieren können.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Query Editor [SQL Server Management Studio], color coding
- color coding [Query Editor]
ms.assetid: 802882dc-c997-4e3f-8a01-994bb43169ae
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 96fe150cd54cfd37074949a278ed40570dd18e83
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354756"
---
# <a name="color-coding-in-query-editors"></a>Farbcodierung im Abfrage-Editor

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Der in die Code-Editoren eingegebene Text wird einer Kategorie zugeordnet, die jeweils durch durch Farben identifiziert werden. Mithilfe der Farben können Sie Text im Code schnell finden. Kommentare sind z. B. dunkelgrün. In der folgenden Tabelle werden die gängigsten Farben aufgelistet. Rufen Sie im Menü **Extras** die Option **Optionen** auf, in dem Sie die vollständige Liste der Farben und ihrer Kategorien anzeigen und ein benutzerdefiniertes Farbschema konfigurieren können. Weitere Informationen zum Ändern der Standardfarben finden Sie unter [Change Font Color, Size, and Style](./change-font-color-size-and-style.md).  
  
## <a name="default-code-colors"></a>Standardfarben für Code  
  
|Color|Category|  
|-----------|--------------|  
|Red|SQL-Zeichenfolge|  
|Dunkelgrün|Comment|  
|Schwarz auf silberfarbenem Hintergrund|SQLCMD-Befehl|  
|Magenta|Systemfunktion|  
|Grün|Systemtabelle, Sicht oder Tabellenwertfunktion. Außerdem die Systemschemas sys und INFORMATION_SCHEMA.|  
|Blau|Schlüsselwort|  
|Blaugrün|Zeilennummern oder Vorlagenparameter|  
|Kastanienbraun|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] )|  
|Dunkelgrau|Operatoren|  
  
## <a name="status-bar"></a>Statusleiste  
 Sie können registrierte Server oder [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Server im Objekt-Explorer konfigurieren, damit sie in der Statusleiste des [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Abfrage-Editors andere Farben aufweisen. Damit können Sie identifizieren, zu welchem Server die einzelnen Editor-Fenster gehören, wenn viele Fenster gleichzeitig geöffnet sind. Weitere Informationen zum Festlegen der Statusleistenfarben finden Sie unter [Statusleiste &#40;Abfrage-Editor der Datenbank-Engine&#41;](./status-bar-database-engine-query-editor.md).  
  
 In manchen Editor-Typen wird die Statusleiste nicht angezeigt, oder es werden keine verschiedenen Farben unterstützt.  
  
