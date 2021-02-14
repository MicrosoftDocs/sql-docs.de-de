---
description: Bewerten von MySQL-Datenbanken für die Konvertierung (MySqlToSql)
title: Bewerten der MySQL-Datenbanken für die Konvertierung (mysqltoisql) | Microsoft-Dokumentation
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Assessment reports
ms.assetid: 2a56a003-3b0f-453a-963c-00c9e40933ec
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 61e8cd99640b45cfdfd74de60beb2fcbd9393308
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100069195"
---
# <a name="assessing-mysql-databases-for-conversion-mysqltosql"></a>Bewerten von MySQL-Datenbanken für die Konvertierung (MySqlToSql)
Bevor Sie Objekte laden und Daten zu [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder SQL Azure migrieren, sollten Sie bestimmen, wie komplex die Migration ist und wie viel Zeit die Migration dauern wird. SSMA kann einen Bewertungsbericht erstellen, in dem der Prozentsatz der Objekte angezeigt wird, die erfolgreich konvertiert werden. Mit SSMA können Sie auch die spezifischen Probleme anzeigen, die Konvertierungs Fehler verursachen.  
  
## <a name="creating-assessment-reports"></a>Erstellen von Bewertungsberichten  
Beim Erstellen dieses Bewertungsberichts konvertiert SSMA die ausgewählten MySQL-Datenbankobjekte in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder SQL Azure Syntax und zeigt dann die Ergebnisse an.  
  
**So erstellen Sie einen Bewertungsbericht**  
  
1.  Wählen Sie im MySQL-metadatenexplorer die zu überwachenden Schemas aus.  
  
2.  Deaktivieren Sie die Kontrollkästchen neben den Kontrollkästchen, um einzelne Objekte auszulassen.  
  
3.  Klicken Sie mit der rechten Maustaste auf **Schemas**, und wählen Sie dann **Bericht erstellen** aus.  
  
    Klicken Sie mit der rechten Maustaste auf ein Objekt, um einzelne Objekte zu analysieren Klicken Sie dann auf **Bericht erstellen**.  
  
    SSMA zeigt den Fortschritt in der Statusleiste am unteren Rand des Fensters an. Wenn der Ausgabebereich sichtbar ist, werden im Ausgabebereich auch Meldungen angezeigt.  
  
    Wenn die Bewertung fertiggestellt ist, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wird das Fenster Migration Assistant für MySQL, Bewertungsbericht angezeigt.  
  
## <a name="using-assessment-reports"></a>Verwenden von Bewertungsberichten  
Das Fenster "Bewertungsbericht" enthält drei Bereiche:  
  
-   Der linke Bereich enthält die Hierarchie der Objekte, die im Bewertungsbericht enthalten sind. Sie können die Hierarchie durchsuchen und Objekte und Kategorien von Objekten auswählen, um Konvertierungsstatistiken und Code anzuzeigen.  
  
-   Der Inhalt des rechten Bereichs hängt von dem Element ab, das im linken Bereich ausgewählt ist.  
  
    Wenn eine Gruppe von Objekten ausgewählt ist, z. b. Schema, enthält der Rechte Bereich einen Bereich für die Konvertierungs Statistik und Objekte im Bereich Kategorien. Im Bereich Konvertierungs Statistik werden die Konvertierungsstatistiken für die ausgewählten Objekte angezeigt. Der Bereich Objekte nach Kategorien zeigt die Konvertierungsstatistiken für das Objekt oder die Kategorien von Objekten an.  
  
    Wenn eine Funktion, eine Prozedur, eine Tabelle oder eine Sicht ausgewählt ist, enthält der Rechte Bereich Statistiken, Quellcode und Zielcode.  
  
    -   Im oberen Bereich werden die Gesamt Statistiken für das Objekt angezeigt. Möglicherweise müssen Sie die **Statistik** erweitern, um diese Informationen anzuzeigen.  
  
    -   Der Quellbereich zeigt den Quellcode des-Objekts, das im linken Bereich ausgewählt ist. Die markierten Bereiche zeigen problematischen Quellcode.  
  
    -   Der Zielbereich zeigt den konvertierten Code. Roter Text zeigt problematische Code und Fehlermeldungen an.  
  
-   Der untere Bereich zeigt Konvertierungs Meldungen nach Nachrichtennummer gruppiert an. Sie können auf **Fehler**, **Warnungen** oder **Informationen** klicken, um Kategorien von Nachrichten anzuzeigen und dann eine Gruppe von Nachrichten zu erweitern. Klicken Sie auf eine einzelne Nachricht, um das Objekt im linken Bereich auszuwählen, und zeigen Sie die Details im rechten Bereich an.  
  
## <a name="analyzing-conversion-problems-by-using-the-assessment-report"></a>Analysieren von Konvertierungs Problemen mithilfe des Bewertungsberichts  
Im Bereich Konvertierungs Statistik werden die Konvertierungsstatistiken angezeigt. Wenn der Prozentsatz für eine Kategorie weniger als 100 Prozent beträgt, sollten Sie bestimmen, warum die Konvertierung nicht erfolgreich war.  
  
**So zeigen Sie Konvertierungsprobleme an**  
  
1.  Erstellen Sie den Bewertungsbericht mithilfe der Anweisungen im vorherigen Verfahren.  
  
2.  Erweitern Sie im linken Bereich Schemas oder Ordner mit einem roten Fehler Symbol. Erweitern Sie Elemente, bis Sie ein einzelnes Element auswählen, für das die Konvertierung fehlgeschlagen ist.  
  
3.  Klicken Sie oben im Bereich Quelle auf **nächstes Problem**.  
  
    Der problematische Code wird hervorgehoben, ebenso wie der zugehörige Code im Ziel Navigationsbereich.  
  
4.  Überprüfen Sie alle Fehlermeldungen, und legen Sie dann fest, was Sie mit dem Objekt tun möchten, das das Konvertierungs Problem verursacht hat.  
  
-   Aktualisieren Sie die MySQL-Syntax in SSMA. Die Syntax kann nur für Prozeduren und Funktionen aktualisiert werden. Um die Syntax zu aktualisieren, wählen Sie das Objekt im Bereich MySQL-metadatenexplorer aus, klicken Sie auf die Registerkarte **SQL** , und ändern Sie dann den SQL-Code. Wenn Sie vom Element Weg navigieren, werden Sie aufgefordert, die aktualisierte Syntax zu speichern. Sie können die gemeldeten Fehler für das Objekt auf der Registerkarte **Bericht** anzeigen.  
  
-   In MySQL können Sie das MySQL-Objekt ändern, um problematischen Code zu entfernen oder zu überarbeiten. Um den aktualisierten Code in SSMA zu laden, müssen Sie die Metadaten aktualisieren. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit MySQL &#40;mysqlto SQL&#41;](../../ssma/mysql/connecting-to-mysql-mysqltosql.md).  
  
-   Sie können das Objekt von der Migration ausschließen. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Deaktivieren Sie in oder SQL Azure Metadaten-Explorer und MySQL-metadatenexplorer das Kontrollkästchen neben dem Element, bevor Sie die Objekte in Laden [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder SQL Azure und Daten aus MySQL migrieren.  
  
## <a name="next-step"></a>Nächster Schritt  
[MySQL-Datenbanken &#40;mysqlto SQL-&#41;werden umgerechnet ](../../ssma/mysql/converting-mysql-databases-mysqltosql.md)  
  
## <a name="see-also"></a>Weitere Informationen  
[Migrieren von MySQL-Datenbanken zu SQL Server Azure SQL-Datenbank &#40;mysqlto SQL&#41;](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)  
  
