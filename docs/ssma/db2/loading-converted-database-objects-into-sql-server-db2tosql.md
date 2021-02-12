---
description: Laden von konvertierten Datenbankobjekten in SQL Server (DB2ToSQL)
title: Laden von konvertierten Datenbankobjekten in SQL Server (DB2ToSQL) | Microsoft-Dokumentation
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: f4ea1ced-9f9f-4a9d-88ab-81dbab64adc3
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 209b49e3719400b92a5425bdd680d8bed548e285
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072474"
---
# <a name="loading-converted-database-objects-into-sql-server-db2tosql"></a>Laden von konvertierten Datenbankobjekten in SQL Server (DB2ToSQL)
Nachdem Sie DB2-Schemas in konvertiert haben [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , können Sie die resultierenden Datenbankobjekte in Laden [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Sie können entweder das SSMA-Objekt erstellen, oder Sie können Skripts für die Objekte erstellen und die Skripts selbst ausführen. Außerdem können Sie mit SSMA Ziel Metadaten mit dem tatsächlichen Inhalt der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Datenbank aktualisieren.  
  
## <a name="choosing-between-synchronization-and-scripts"></a>Zwischen Synchronisierung und Skripts auswählen  
Wenn Sie die konvertierten Datenbankobjekte ohne Änderungen in laden möchten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , können Sie die Datenbankobjekte von SSMA direkt erstellen oder neu erstellen. Diese Methode ist schnell und einfach, lässt jedoch nicht zu, dass der Code, der [!INCLUDE[tsql](../../includes/tsql-md.md)] die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Objekte definiert, außer gespeicherten Prozeduren angepasst wird.  
  
Wenn Sie das ändern möchten, [!INCLUDE[tsql](../../includes/tsql-md.md)] das zum Erstellen von-Objekten verwendet wird, oder wenn Sie mehr Kontrolle über die Objekt Erstellung haben möchten, verwenden Sie SSMA zum Erstellen von-Skripts. Anschließend können Sie diese Skripts ändern, jedes Objekt einzeln erstellen und sogar den-Agent verwenden, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] um die Erstellung dieser Objekte zu planen.  
  
## <a name="using-ssma-to-synchronize-objects-with-sql-server"></a>Verwenden von SSMA zum Synchronisieren von Objekten mit SQL Server  
Wenn Sie SSMA zum Erstellen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Datenbankobjekten verwenden möchten, wählen Sie die Objekte im [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] metadatenexplorer aus, und synchronisieren Sie dann die Objekte mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , wie im folgenden Verfahren gezeigt. Wenn die Objekte bereits in vorhanden [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sind und die SSMA-Metadaten neuer als das Objekt in sind [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , ändert SSMA standardmäßig die Objekt Definitionen in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Sie können das Standardverhalten ändern, indem Sie die **Projekteinstellungen** bearbeiten.  
  
> [!NOTE]  
> Sie können vorhandene [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Datenbankobjekte auswählen, die nicht aus DB2-Datenbanken konvertiert wurden. Diese Objekte werden jedoch nicht von SSMA neu erstellt oder geändert.  
  
**So synchronisieren Sie Objekte mit SQL Server**  
  
1.  Erweitern Sie im [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] metadatenexplorer den obersten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Knoten, und erweitern Sie dann **Datenbanken**.  
  
2.  Wählen Sie die zu verarbeitenden Objekte aus:  
  
    -   Aktivieren Sie das Kontrollkästchen neben dem Datenbanknamen, um eine komplette Datenbank zu synchronisieren.  
  
    -   Aktivieren bzw. deaktivieren Sie das Kontrollkästchen neben dem Objekt oder Ordner, um einzelne Objekte oder Kategorien von Objekten zu synchronisieren oder auszulassen.  
  
3.  Nachdem Sie die zu verarbeitenden Objekte im [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] metadatenexplorer ausgewählt haben, klicken Sie mit der rechten Maustaste auf **Datenbanken**, und klicken Sie dann auf **mit Datenbank synchronisieren**.  
  
    Sie können einzelne Objekte oder Objektkategorien auch synchronisieren, indem Sie mit der rechten Maustaste auf das Objekt oder seinen übergeordneten Ordner klicken und dann auf  **mit Datenbank synchronisieren** klicken.  
  
    Danach zeigt SSMA das Dialogfeld **mit Datenbank synchronisieren an** , in dem Sie zwei Gruppen von Elementen sehen können. Auf der linken Seite zeigt SSMA ausgewählte Datenbankobjekte an, die in einer Struktur dargestellt werden. Auf der rechten Seite sehen Sie eine Baumstruktur, die die gleichen Objekte in SSMA-Metadaten darstellt. Sie können die Struktur erweitern, indem Sie auf die Schaltfläche mit der rechten oder linken Schaltfläche klicken. Die Richtung der Synchronisierung wird in der Aktionsspalte angezeigt, die zwischen den beiden Strukturen platziert ist.  
  
    Ein Aktions Vorzeichen kann drei Zustände aufweisen:  
  
    -   Der nach-links-Pfeil bedeutet, dass der Inhalt der Metadaten in der Datenbank gespeichert wird (Standardeinstellung).  
  
    -   Ein Pfeil nach rechts bedeutet, dass der Daten Bank Inhalt die SSMA-Metadaten überschreibt.  
  
    -   Ein Kreuzzeichen bedeutet, dass keine Aktion ausgeführt wird.  
  
Klicken Sie auf das Aktions Zeichen, um den Status zu ändern. Die eigentliche Synchronisierung wird durchgeführt, wenn Sie im Dialogfeld **mit Datenbank synchronisieren** auf die Schaltfläche **OK** klicken.  
  
## <a name="scripting-objects"></a>Skripterstellung für Objekte  
Zum Speichern der [!INCLUDE[tsql](../../includes/tsql-md.md)] Definitionen der konvertierten Datenbankobjekte oder zum Ändern der Objekt Definitionen und zum Ausführen von Skripts können Sie die konvertierten Datenbankobjekt Definitionen in [!INCLUDE[tsql](../../includes/tsql-md.md)] Skripts speichern.  
  
**So speichern Sie Objekte als Skripts**  
  
1.  Nachdem Sie die zu speichernden Objekte in einem Skript ausgewählt haben, klicken Sie mit der rechten Maustaste auf **Datenbanken**, und klicken Sie dann auf **als Skript speichern**.  
  
    Sie können auch Skripts für einzelne Objekte oder Kategorien von Objekten erstellen, indem Sie mit der rechten Maustaste auf das Objekt oder seinen übergeordneten Ordner klicken und dann auf **als Skript speichern** klicken.  
  
2.  Suchen Sie im Dialogfeld **Speichern** unter den Ordner, in dem Sie das Skript speichern möchten, geben Sie im Feld **Dateiname** einen Dateinamen ein, und klicken Sie dann auf [!INCLUDE[clickOK](../../includes/clickok-md.md)] . SSMA fügt die Dateinamenerweiterung ". SQL" an.  
  
### <a name="modifying-scripts"></a>Ändern von Skripts  
Nachdem Sie die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Objekt Definitionen als ein oder mehrere Skripts gespeichert haben, können Sie verwenden, [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] um die Skripts anzuzeigen und zu ändern.  
  
**So ändern Sie ein Skript**  
  
1.  Zeigen Sie im Menü [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] **Datei** auf **Öffnen**, und klicken Sie dann auf **Datei**.  
  
2.  Wählen Sie im Dialogfeld **Öffnen** die Skriptdatei aus, und klicken Sie dann auf OK.
  
3.  Bearbeiten Sie die Skriptdatei mit dem Abfrage-Editor.  
  
    Weitere Informationen zum Abfrage-Editor finden Sie in der-Online Dokumentation unter "praktische Befehle und Features im Editor" [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
4.  Um das Skript zu speichern, klicken Sie im Menü Datei auf **Speichern**.  
  
### <a name="running-scripts"></a>Ausführen von Skripts  
Sie können in ein Skript oder einzelne Anweisungen ausführen [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .  
  
**So führen Sie ein Skript aus**  
  
1.  Zeigen Sie im Menü [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] **Datei** auf **Öffnen**, und klicken Sie dann auf **Datei**.  
  
2.  Wählen Sie im Dialogfeld **Öffnen** die Skriptdatei aus, und klicken Sie dann auf [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
3.  Drücken Sie die Taste **F5** , um das komplette Skript auszuführen.  
  
4.  Um einen Satz von-Anweisungen auszuführen, wählen Sie die Anweisungen im Abfrage-Editor-Fenster aus, und drücken Sie dann die Taste **F5** .  
  
Weitere Informationen zur Verwendung des Abfrage-Editors zum Ausführen von Skripts finden Sie unter " [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] [!INCLUDE[tsql](../../includes/tsql-md.md)] Abfrage" in der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Online Dokumentation.  
  
Sie können Skripts auch über die Befehlszeile ausführen, indem Sie das Hilfsprogramm **sqlcmd** und den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent verwenden. Weitere Informationen zu **sqlcmd** finden Sie unter "sqlcmd Utility" in der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Online Dokumentation. Weitere Informationen zum- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent finden Sie unter "Automatisieren administrativer Aufgaben ( [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent)" in der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Online Dokumentation.  
  
## <a name="securing-objects-in-sql-server"></a>Sichern von Objekten in SQL Server  
Nachdem Sie die konvertierten Datenbankobjekte in geladen haben [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , können Sie Berechtigungen für diese Objekte erteilen und verweigern. Es empfiehlt sich, dies zu tun, bevor Sie Daten zu migrieren [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Informationen zum Sichern von Objekten in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] finden Sie unter "Sicherheitsüberlegungen für Datenbanken und Datenbankanwendungen" in der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Online Dokumentation.  
  
## <a name="next-step"></a>Nächster Schritt  
Der nächste Schritt des Migrations Vorgangs besteht darin, [DB2-Daten in SQL Server zu migrieren](./migrating-db2-data-into-sql-server-db2tosql.md).  
  
## <a name="see-also"></a>Weitere Informationen  
[Migrieren von DB2-Daten in SQL Server &#40;DB2ToSQL-&#41;](../../ssma/db2/migrating-db2-data-into-sql-server-db2tosql.md)  
