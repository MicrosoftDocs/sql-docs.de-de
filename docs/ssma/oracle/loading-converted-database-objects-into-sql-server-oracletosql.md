---
title: Laden von konvertierten Datenbankobjekten in SQL Server (oracleto SQL) | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Datenbankobjekte, die Sie aus Oracle konvertiert haben, mithilfe von SSMA für Oracle in die SQL Server Instanz laden.
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Synchronization, Securing Objects in SQL Server
- Synchronization,Scripting Objects
ms.assetid: a8ae33b2-1883-4785-922b-ea0e31c0b37a
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 4b315b0b26f771a2e215f03e1abcfeb5f829e86b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080801"
---
# <a name="loading-converted-database-objects-into-sql-server-oracletosql"></a>Laden konvertierter Datenbankobjekte in SQL Server (OracleToSQL)
Nachdem Sie Oracle-Schemas in SQL Server konvertiert haben, können Sie die resultierenden Datenbankobjekte in SQL Server laden. Sie können entweder das SSMA-Objekt erstellen, oder Sie können Skripts für die Objekte erstellen und die Skripts selbst ausführen. Außerdem können Sie mit SSMA Ziel Metadaten mit dem tatsächlichen Inhalt SQL Server Datenbank aktualisieren.  
  
## <a name="choosing-between-synchronization-and-scripts"></a>Zwischen Synchronisierung und Skripts auswählen  
Wenn Sie die konvertierten Datenbankobjekte ohne Änderungen in SQL Server laden möchten, können Sie die Datenbankobjekte von SSMA direkt erstellen oder neu erstellen. Diese Methode ist schnell und einfach, lässt jedoch nicht zu, dass der Code, der [!INCLUDE[tsql](../../includes/tsql-md.md)] die SQL Server Objekte definiert, außer gespeicherten Prozeduren angepasst wird.  
  
Wenn Sie das ändern möchten, [!INCLUDE[tsql](../../includes/tsql-md.md)] das zum Erstellen von-Objekten verwendet wird, oder wenn Sie mehr Kontrolle über die Objekt Erstellung haben möchten, verwenden Sie SSMA zum Erstellen von-Skripts. Anschließend können Sie diese Skripts ändern, jedes Objekt einzeln erstellen und sogar SQL Server-Agent verwenden, um die Erstellung dieser Objekte zu planen.  
  
## <a name="using-ssma-to-synchronize-objects-with-sql-server"></a>Verwenden von SSMA zum Synchronisieren von Objekten mit SQL Server  
Wenn Sie SSMA zum Erstellen von SQL Server Datenbankobjekten verwenden möchten, wählen Sie die Objekte in SQL Server Metadaten-Explorer aus, und synchronisieren Sie dann die Objekte mit SQL Server, wie im folgenden Verfahren gezeigt. Wenn die Objekte bereits in SQL Server vorhanden sind und die SSMA-Metadaten neuer als das Objekt in SQL Server sind, ändert SSMA standardmäßig die Objekt Definitionen in SQL Server. Sie können das Standardverhalten ändern, indem Sie die **Projekteinstellungen** bearbeiten.  
  
> [!NOTE]  
> Sie können vorhandene SQL Server Datenbankobjekte auswählen, die nicht aus Oracle-Datenbanken konvertiert wurden. Diese Objekte werden jedoch nicht von SSMA neu erstellt oder geändert.  
  
**So synchronisieren Sie Objekte mit SQL Server**  
  
1.  Erweitern Sie in SQL Server metadatenexplorer den oberen SQL Server Knoten, und erweitern Sie dann **Datenbanken**.  
  
2.  Wählen Sie die zu verarbeitenden Objekte aus:  
  
    -   Aktivieren Sie das Kontrollkästchen neben dem Datenbanknamen, um eine komplette Datenbank zu synchronisieren.  
  
    -   Aktivieren bzw. deaktivieren Sie das Kontrollkästchen neben dem Objekt oder Ordner, um einzelne Objekte oder Kategorien von Objekten zu synchronisieren oder auszulassen.  
  
3.  Nachdem Sie die zu verarbeitenden Objekte in SQL Server Metadaten-Explorer ausgewählt haben, klicken Sie mit der rechten Maustaste auf **Datenbanken**, und klicken Sie dann auf **mit Datenbank synchronisieren**.  
  
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
  
2.  Suchen Sie im Dialogfeld **Speichern** unter den Ordner, in dem Sie das Skript speichern möchten, geben Sie im Feld **Dateiname** einen Dateinamen ein, und klicken Sie dann auf OK, um die Dateinamenerweiterung ". SQL" anzufügen.  
  
### <a name="modifying-scripts"></a>Ändern von Skripts  
Nachdem Sie die SQL Server Objekt Definitionen als ein oder mehrere Skripts gespeichert haben, können Sie verwenden, [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] um die Skripts anzuzeigen und zu ändern.  
  
**So ändern Sie ein Skript**  
  
1.  Zeigen Sie im Menü [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] **Datei** auf **Öffnen**, und klicken Sie dann auf **Datei**.  
  
2.  Wählen Sie im Dialogfeld **Öffnen** die Skriptdatei aus, und klicken Sie dann auf OK.
  
3.  Bearbeiten Sie die Skriptdatei mit dem Abfrage-Editor.  
  
    Weitere Informationen zum Abfrage-Editor finden Sie unter "praktische Befehle und Features im Editor" in SQL Server-Onlinedokumentation.  
  
4.  Um das Skript zu speichern, klicken Sie im Menü Datei auf **Speichern**.  
  
### <a name="running-scripts"></a>Ausführen von Skripts  
Sie können in ein Skript oder einzelne Anweisungen ausführen [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .  
  
**So führen Sie ein Skript aus**  
  
1.  Zeigen Sie im Menü [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] **Datei** auf **Öffnen**, und klicken Sie dann auf **Datei**.  
  
2.  Wählen Sie im Dialogfeld **Öffnen** die Skriptdatei aus, und klicken Sie dann auf OK.  
  
3.  Drücken Sie die Taste **F5** , um das komplette Skript auszuführen.  
  
4.  Um einen Satz von-Anweisungen auszuführen, wählen Sie die Anweisungen im Abfrage-Editor-Fenster aus, und drücken Sie dann die Taste **F5** .  
  
Weitere Informationen zur Verwendung des Abfrage-Editors zum Ausführen von Skripts finden Sie unter " [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] [!INCLUDE[tsql](../../includes/tsql-md.md)] Abfrage" in SQL Server-Onlinedokumentation.  
  
Sie können Skripts auch über die Befehlszeile ausführen, indem Sie das Hilfsprogramm **sqlcmd** und die SQL Server-Agent verwenden. Weitere Informationen zu **sqlcmd** finden Sie unter "sqlcmd Utility" in SQL Server-Onlinedokumentation. Weitere Informationen zu SQL Server-Agent finden Sie im Thema zum Automatisieren administrativer Aufgaben (SQL Server-Agent) unter SQL Server-Onlinedokumentation.  
  
## <a name="securing-objects-in-sql-server"></a>Sichern von Objekten in SQL Server  
Nachdem Sie die konvertierten Datenbankobjekte in SQL Server geladen haben, können Sie Berechtigungen für diese Objekte erteilen und verweigern. Es empfiehlt sich, dies zu tun, bevor Sie Daten zu SQL Server migrieren. Informationen zum Sichern von Objekten in SQL Server finden Sie unter "Sicherheitsüberlegungen für Datenbanken und Datenbankanwendungen" in SQL Server-Onlinedokumentation.  
  
## <a name="next-step"></a>Nächster Schritt  
Der nächste Schritt des Migrations Vorgangs besteht darin, [Daten in SQL Server zu migrieren](migrating-oracle-data-into-sql-server-oracletosql.md).  
  
## <a name="see-also"></a>Weitere Informationen  
[Migrieren von Oracle-Datenbanken zu SQL Server &#40;oracleto SQL-&#41;](../../ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql.md)  
  
