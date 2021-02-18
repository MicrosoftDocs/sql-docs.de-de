---
title: Inkrementelles Durchsuchen eines aktiven Dokuments
description: Hier erfahren Sie mehr über die inkrementelle Suche eines einzelnen Dokuments oder Fensters. Während der Eingabe wird beim inkrementellen Suchvorgang das nächste Vorkommen der Eingaben hervorgehoben, die Sie zu diesem Zeitpunkt eingegeben haben. Ausgeblendeter Text wird ignoriert.
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- searches [SQL Server Management Studio], incremental
- Query Editor [SQL Server Management Studio], incremental search
- incremental searches [SQL Server Management Studio]
ms.assetid: 490bb36c-dd43-4219-9e2a-ff27046b9395
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: bbf4b464266fea538ef30f8f9667d8d039f0b74c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354546"
---
# <a name="search-an-active-document-incrementally"></a>Inkrementelles Durchsuchen eines aktiven Dokuments
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Sie können ein einzelnes Dokument oder Fenster inkrementell durch Eingabe von Text durchsuchen. Beim Suchvorgang wird die erste Zeichenfolge hervorgehoben, die mit der eingegebenen Zeichenfolge für die inkrementelle Suche im Dokument bzw. Fenster übereinstimmt. Bei der inkrementellen Suche wird automatisch der gesamte Text in einem Dokument bzw. Fenster durchsucht, ausgenommen ausgeblendeter Text.  
  
 Bei der Option **Groß-/Kleinschreibung beachten** verwendet die inkrementelle Suche die Kriterien aus der vorherigen Suche. Wenn Sie z. B. mit dem Dialogfeld **In Dateien suchen** eine Suche in mehreren Dateien ausgeführt haben, die Option **Groß-/Kleinschreibung beachten** aktivieren und jetzt einen inkrementellen Suchvorgang ausführen, wird bei der Suche die Groß-/Kleinschreibung beachtet.  
  
### <a name="to-search-incrementally"></a>So führen Sie einen inkrementellen Suchvorgang aus  
  
1.  Öffnen Sie die zu durchsuchende Datei bzw. das Fenster.  
  
2.  Zeigen Sie im Menü **Bearbeiten** auf **Erweitert**, und klicken Sie dann auf **Inkrementelle Suche**.  
  
     Der Cursor wird jetzt als Fernglas mit einem Pfeil zur Anzeige der Suchrichtung dargestellt, und in der Statusleiste wird "Inkrementelle Suche" angezeigt.  
  
3.  Beginnen Sie mit der Eingabe der Textzeichenfolge.  
  
     In der Statusleiste wird der Text angezeigt, den Sie eingeben, und der Editor hebt die erste Übereinstimmung hervor. Wenn Sie mit der Eingabe fortfahren, hebt der Editor die nächste Übereinstimmung hervor. Wenn es keine Übereinstimmungen gibt, wird in der Statusleiste Folgendes angezeigt:  
  
    ```  
    Incremental Search: <text> (not found)  
    ```  
  
 Inkrementelle Suchvorgänge werden abwärts von links nach rechts von der aktuellen Position im Dokument ausgeführt. Inkrementelle Suchvorgänge können mithilfe von Tastenkombinationen ausgeführt werden.  
  
> [!NOTE]  
>  Eine vollständige Liste der Tastenkombinationen finden Sie unter [Tastenkombinationen für SQL Server Management Studio](../../ssms/sql-server-management-studio-keyboard-shortcuts.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Suchen und Ersetzen](./search-and-replace.md)   
 [Interaktives Durchsuchen von Dokumenten](./search-documents-interactively.md)   
 [Durchsuchen von Dokumenten mithilfe von Ergebnislisten](./search-documents-using-results-lists.md)   
 [Suchen von Text mit Platzhaltern](./search-text-with-wildcards.md)   
 [Suchen von Text mit regulären Ausdrücken](./search-text-with-regular-expressions.md)  
  
