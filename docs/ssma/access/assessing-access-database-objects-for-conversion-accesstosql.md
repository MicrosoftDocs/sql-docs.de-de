---
description: Bewerten von Access-Datenbankobjekten für die Konvertierung (accesstosql)
title: Bewerten von Access-Datenbankobjekten für die Konvertierung (accesstosql) | Microsoft-Dokumentation
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- assessing SQL
- assessing syntax
- assessment reports
- creating assessment reports
- estimating migration effort
- reports
- SQL, assessing
- syntax, assessing
ms.assetid: 8b9e23d6-da62-437a-8c05-8ad2628b9441
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 07adac4e991afd70b70a5f087a881f1933baaae1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100076421"
---
# <a name="assessing-access-database-objects-for-conversion-accesstosql"></a>Bewerten von Access-Datenbankobjekten für die Konvertierung (accesstosql)
Bevor Sie Objekte laden und Daten zu [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder SQL Azure migrieren, sollten Sie bestimmen, wie viel der Migration erfolgreich verlaufen soll und wie lange die Konvertierung dauern kann. SSMA kann einen Bewertungsbericht erstellen, in dem der Prozentsatz der Objekte angezeigt wird, die erfolgreich in konvertiert wurden, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder SQL Azure Syntax-und Zeit Schätzwerte zum Durchführen der Migration. Mit SSMA können Sie auch die spezifischen Probleme anzeigen, die Konvertierungs Fehler verursacht haben.  
  
## <a name="creating-assessment-reports"></a>Erstellen von Bewertungsberichten  
Wenn ein Bewertungsbericht erstellt wird, konvertiert SSMA die ausgewählten Access-Datenbankobjekte in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder SQL Azure Syntax und zeigt dann die Ergebnisse an.  
  
**So erstellen Sie einen Bewertungsbericht**  
  
1.  Wählen Sie unter Access Metadata Explorer die Datenbank oder Datenbanken aus, die Sie bewerten möchten.  
  
2.  Wenn Sie einzelne Objekte weglassen möchten, deaktivieren Sie die Kontrollkästchen neben den Objekten, die nicht bewertet werden sollen.  
  
3.  Klicken Sie mit der rechten Maustaste auf **Datenbanken** und dann auf **Bericht erstellen**.  
  
    Sie können einzelne Objekte auch analysieren, indem Sie mit der rechten Maustaste auf ein Objekt klicken und dann **Bericht erstellen** auswählen.  
  
    SSMA zeigt den Fortschritt in der Statusleiste am unteren Rand des Fensters an. Wenn der Ausgabebereich sichtbar ist, werden im Ausgabebereich auch Meldungen angezeigt.  
  
Wenn die Bewertung fertiggestellt ist, wird das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Fenster Migration Assistant für Zugriff: Bewertungsbericht angezeigt.  
  
## <a name="using-assessment-reports"></a>Verwenden von Bewertungsberichten  
Das Fenster Bewertungsbericht enthält drei Bereiche: einen Explorer, einen Detailbereich und einen Meldungs Bereich.  
  
-   Im Bereich Explorer können Sie die Objekte durchsuchen, die bewertet wurden. Sie können in diesem Bereich auf Elemente klicken, um einen Drilldown zu einzelnen Tabellen, Indizes und Schlüsseln auszuführen.  
  
-   Im Detailbereich werden die Konvertierungsstatistiken für das ausgewählte Objekt angezeigt.  
  
-   Im Meldungs Bereich werden die Fehler, Warnungen und Informationsmeldungen für die Konvertierung sowie Zeit Schätzwerte für die Migration und die einzelnen Schritte zur Fehlerkorrektur angezeigt.  
  
Beheben Sie die Fehler, bevor Sie den Bewertungsbericht erneut ausführen oder Schemas konvertieren. Um Fehler zu finden, klicken Sie auf die Schaltfläche **Fehler** im Bereich Meldungen, und erweitern Sie dann jeden Fehler, um eine Liste der Objekte anzuzeigen, in denen der Fehler aufgetreten ist. Wenn Sie im Bereich Meldungen auf ein Objekt klicken, werden alle Fehler und Warnungen für dieses Objekt im Detailbereich angezeigt.  
  
## <a name="next-step"></a>Nächster Schritt  
[Konvertieren von Access-Datenbankobjekten](converting-access-database-objects-accesstosql.md)  
  
## <a name="see-also"></a>Weitere Informationen  
[Migration von Access-Datenbanken zu SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
  
