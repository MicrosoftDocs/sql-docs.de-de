---
title: Anfügen einer Datenbank | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie in SQL Server mithilfe von SQL Server Management Studio oder Transact-SQL eine Datenbank anfügen. Verwenden Sie diese Funktion, um eine Datenbank zu kopieren, zu verschieben oder zu aktualisieren.
ms.custom: ''
ms.date: 10/24/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
f1_keywords:
- sql13.swb.attachdatabase.f1
helpviewer_keywords:
- database attaching [SQL Server]
- attaching databases [SQL Server]
ms.assetid: b4efb0ae-cfe6-4d81-a4b4-6e4916885caa
author: stevestein
ms.author: sstein
ms.openlocfilehash: c1987fae42f804e861c1f15e55deb60e273e0d3b
ms.sourcegitcommit: cfa04a73b26312bf18d8f6296891679166e2754d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2020
ms.locfileid: "92194402"
---
# <a name="attach-a-database"></a>Anfügen einer Datenbank
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

In diesem Thema wird beschrieben, wie eine Datenbank in [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] mit [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[tsql](../../includes/tsql-md.md)]angefügt wird. Sie können diese Funktion verwenden, um eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbank zu kopieren, zu verschieben oder zu aktualisieren.  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Voraussetzungen  
  
-   Die Datenbank muss zuerst getrennt werden. Wenn Sie versuchen, eine Datenbank anzufügen, die nicht getrennt wurde, wird ein Fehler zurückgegeben. Weitere Informationen finden Sie unter [Trennen einer Datenbank](../../relational-databases/databases/detach-a-database.md).  
  
-   Wenn Sie eine Datenbank anfügen, müssen alle Datendateien (MDF- und LDF-Dateien) verfügbar sein. Wenn eine Datendatei einen anderen Pfad als beim Erstellen oder beim letzten Anfügen der Datenbank aufweist, müssen Sie den aktuellen Pfad der Datei angeben.  
  
-   Wenn Sie eine Datenbank anfügen, MDF- und LDF-Dateien sich in verschiedenen Verzeichnissen befinden und einer der Pfade „ \\\\?\GlobalRoot“ enthält, schlägt der Vorgang fehl.  
  
###  <a name="is-attach-the-best-choice"></a><a name="Recommendations"></a> Ist Anfügen am besten geeignet?  
Es wird empfohlen, Datenbanken mit der `ALTER DATABASE`-Prozedur für geplante Verschiebungen zu verschieben, anstatt Trenn- und Anfügevorgänge zu verwenden, wenn Sie Datenbankdateien innerhalb einer Instanz verschieben. Weitere Informationen finden Sie unter [Move User Databases](../../relational-databases/databases/move-user-databases.md). 
 
Es wird davon abgeraten, Trenn- und Anfügevorgänge für Sicherungen und Wiederherstellungen zu verwenden. Es gibt keine Sicherungen von Transaktionsprotokollen, und Dateien können versehentlich gelöscht werden.
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
Dateizugriffsberechtigungen werden während einer Reihe von Datenbankvorgängen festgelegt, einschließlich des Trennens oder Anfügens einer Datenbank. Informationen zu Dateiberechtigungen, die beim Trennen und Anfügen einer Datenbank festgelegt werden, finden Sie unter [Sichern von Daten- und Protokolldateien](/previous-versions/sql/sql-server-2008-r2/ms189128(v=sql.105)) in der [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] -Onlinedokumentation (noch immer lesenswert). 
  
Das Anfügen oder Wiederherstellen von Datenbanken aus unbekannten oder nicht vertrauenswürdigen Quellen wird nicht empfohlen. Solche Datenbanken können bösartigen Code enthalten, der möglicherweise unbeabsichtigten [!INCLUDE[tsql](../../includes/tsql-md.md)] -Code ausführt oder Fehler verursacht, indem er das Schema oder die physische Datenbankstruktur ändert. Bevor Sie eine Datenbank aus einer unbekannten oder nicht vertrauenswürdigen Quelle verwenden, führen Sie auf einem Nichtproduktionsserver [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) für die Datenbank aus. Überprüfen Sie außerdem den Code in der Datenbank, z.B. gespeicherte Prozeduren oder anderen benutzerdefinierten Code. Weitere Informationen zum Anfügen von Datenbanken sowie Informationen zu Änderungen, die an Metadaten vorgenommen werden, wenn Sie eine Datenbank anfügen, finden Sie unter [Anfügen und Trennen von Datenbanken (SQL Server)](../../relational-databases/databases/database-detach-and-attach-sql-server.md).  
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
Erfordert die Berechtigung `CREATE DATABASE`, `CREATE ANY DATABASE` oder `ALTER ANY DATABASE`.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  

### <a name="to-attach-a-database"></a>So fügen Sie eine Datenbank an  
  
1.  Stellen Sie im [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] -Objekt-Explorer eine Verbindung mit einer Instanz von [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]her, und klicken Sie dann, um diese Instanzansicht in SSMS zu erweitern.  
  
2.  Klicken Sie mit der rechten Maustaste auf **Datenbanken** , und klicken Sie auf **Anfügen**.  
  
3.  Wenn Sie die anzufügende Datenbank angeben möchten, klicken Sie im Dialogfeld **Datenbanken anfügen** auf **Hinzufügen**. Wählen Sie dann im Dialogfeld **Datenbankdateien suchen** den Datenträger aus, auf dem die Datenbank gespeichert ist. Erweitern Sie die Verzeichnisstruktur, um die MDF-Datei der Datenbank zu suchen und auszuwählen. Beispiel:

     `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\AdventureWorks2012_Data.mdf`  
  
    > [!IMPORTANT]  
    > Wenn Sie versuchen, eine Datenbank auszuwählen, die bereits angefügt wurde, wird ein Fehler generiert.  
  
     **Anzufügende Datenbanken**  
     Zeigt Informationen zu den ausgewählten Datenbanken an.  
  
     \<no column header>  
     Zeigt ein Symbol an, das den Status des Anfügevorgangs angibt. Die möglichen Symbole werden in der unten stehenden Beschreibung von **Status** beschrieben.  
  
     **Speicherort für MDF-Datei**  
     Zeigt den Pfad und den Dateinamen der ausgewählten MDF-Datei an.  
  
     **Database Name**  
     Zeigt den Namen der Datenbank an.  
  
     **Anfügen als**  
     Gibt wahlweise einen anderen Namen für die anzufügende Datenbank an.  
  
     **Besitzer**  
     Zeigt eine Dropdownliste mit möglichen Datenbankbesitzern an, aus der Sie wahlweise einen anderen Besitzer auswählen können.  
  
     **Status**  
     Zeigt den Status der Datenbank an (siehe folgende Tabelle).  
  
    |Symbol|Statustext|BESCHREIBUNG|  
    |----------|-----------------|-----------------|  
    |(Kein Symbol)|(Kein Text)|Das Anfügen hat noch nicht begonnen oder steht für dieses Objekt noch aus. Dies ist der Standardwert bei Öffnen des Dialogfelds.|  
    |Grünes, nach rechts zeigendes Dreieck|In Bearbeitung|Das Anfügen hat begonnen, ist aber noch nicht abgeschlossen.|  
    |Grünes Häkchen|Erfolg|Das Objekt wurde erfolgreich angefügt.|  
    |Roter Kreis mit einem weißen Kreuz darin|Fehler|Beim Anfügen ist ein Fehler aufgetreten. Der Vorgang konnte deshalb nicht erfolgreich abgeschlossen werden.|  
    |Kreis mit zwei schwarzen Quadranten (links und rechts) und zwei weißen Quadranten (oben und unten) darin|Beendet|Das Anfügen wurde nicht erfolgreich abgeschlossen, weil der Benutzer den Vorgang angehalten hat.|  
    |Kreis mit einem gekrümmten Pfeil darin, der entgegengesetzt der Uhrzeigerrichtung zeigt|Rollback wurde ausgeführt|Anfügen war erfolgreich, es wurde jedoch ein Rollback durchgeführt, weil beim Anfügen eines anderen Objekts ein Fehler aufgetreten ist.|  
  
     **Meldung**  
     Zeigt entweder eine leere Meldung oder einen "Datei nicht gefunden"-Link an.  
  
     **Add (Hinzufügen)**  
     Suchen Sie die erforderlichen Hauptdatenbankdateien. Wenn der Benutzer eine MDF-Datei auswählt, werden entsprechende Informationen automatisch in die jeweiligen Felder des Rasters **Anzufügende Datenbank** eingetragen.  
  
     **Remove**  
     Entfernt die ausgewählte Datei aus dem Raster **Anzufügende Datenbank** .  
  
     **"** *<datenbankname>* **" Datenbankdetails**  
     Zeigt die Namen der anzufügenden Dateien an. Klicken Sie zum Überprüfen oder Ändern des Pfadnamens einer Datei auf die Schaltfläche **Durchsuchen** ( **…** ).  
  
    > [!NOTE]  
    > Wenn eine Datei nicht vorhanden ist, wird in der Spalte **Meldung** "Nicht gefunden" angezeigt. Wenn keine Protokolldatei gefunden wird, liegt sie in einem anderen Verzeichnis oder wurde gelöscht. Dann müssen Sie entweder den Dateipfad im Raster **Datenbankdetails** ändern, um auf den richtigen Pfad zu verweisen, oder die Protokolldatei aus dem Raster entfernen. Wenn keine .ndf-Datei gefunden wurde, müssen Sie ihren Pfad im Raster aktualisieren, um auf den richtigen Pfad zu verweisen.  
  
     **Originaldateiname**  
     Zeigt den Namen der angefügten Datei an, die zur Datenbank gehört.  
  
     **Dateityp**  
     Gibt den Dateityp an: **Datendatei** oder **Protokolldatei**.  
  
     **Aktueller Dateipfad**  
     Zeigt den Pfad zur ausgewählten Datenbankdatei an Die Pfadangabe kann manuell bearbeitet werden.  
  
     **Meldung**  
     Zeigt entweder eine leere Meldung oder einen „**Datei nicht gefunden**“-Hyperlink an.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
  
### <a name="to-attach-a-database"></a>So fügen Sie eine Datenbank an  
  
1.  Stellen Sie eine Verbindung mit dem [!INCLUDE[ssDE](../../includes/ssde-md.md)]her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Verwenden Sie die Anweisung [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md) mit der `FOR ATTACH`-Klausel.  
  
     Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**. In diesem Beispiel werden die Dateien der Datenbank [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] angefügt, und die Datenbank wird in `MyAdventureWorks`umbenannt.  
  
    ```sql  
    CREATE DATABASE MyAdventureWorks   
        ON (FILENAME = 'C:\MySQLServer\AdventureWorks_Data.mdf'),   
        (FILENAME = 'C:\MySQLServer\AdventureWorks_Log.ldf')   
        FOR ATTACH;  
    ```  
  
    > [!NOTE]  
    > Alternativ können Sie die gespeicherte Prozedur [sp_attach_db](../../relational-databases/system-stored-procedures/sp-attach-db-transact-sql.md) oder [sp_attach_single_file_db](../../relational-databases/system-stored-procedures/sp-attach-single-file-db-transact-sql.md) verwenden. Diese Prozeduren werden jedoch in einer zukünftigen Version von Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]entfernt. Nutzen Sie diese Funktionen bei Neuentwicklungen nicht mehr, und planen Sie die Änderung von Anwendungen, die diese Funktion zurzeit verwenden. Stattdessen wird die Verwendung von `CREATE DATABASE ... FOR ATTACH` empfohlen.  
  
##  <a name="follow-up-after-upgrading-a-sql-server-database"></a><a name="FollowUp"></a>Nächster Schritt: Nach dem Aktualisieren einer SQL Server-Datenbank  
Nachdem Sie eine Datenbank mithilfe der Anfügemethode aktualisiert haben, ist die Datenbank sofort verfügbar und wird automatisch aktualisiert. Wenn die Datenbank Volltextindizes aufweist, werden diese beim Upgrade entweder importiert, zurückgesetzt oder neu erstellt, je nach der Einstellung der Servereigenschaft **Volltextupgrade-Option** . Wenn die Upgradeoption auf **Importieren** oder **Neu erstellen**festgelegt ist, sind die Volltextindizes während des Upgrades nicht verfügbar. Je nach Menge der indizierten Daten kann der Importvorgang mehrere Stunden dauern; die Neuerstellung sogar bis zu zehnmal länger. Wenn die Upgradeoption auf **Importieren**festgelegt und kein Volltextkatalog verfügbar ist, werden die zugehörigen Volltextindizes neu erstellt.  
  
War der Kompatibilitätsgrad einer Benutzerdatenbank vor dem Upgrade 100 oder höher, wird er nach dem Upgrade beibehalten. War der Kompatibilitätsgrad der aktualisierten Datenbank vor dem Upgrade 90, wird er auf 100 gesetzt, was dem niedrigsten unterstützten Kompatibilitätsgrad in [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]entspricht. Weitere Informationen finden Sie unter [ALTER DATABASE-Kompatibilitätsgrad &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).  
  
> [!NOTE]
> Wenn Sie eine Datenbank aus einer Instanz anfügen, die unter [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] oder älter ausgeführt wurde und für die Change Data Capture (CDC) aktiviert war, müssen Sie auch den unten genannten Befehl ausführen, um die Metadaten von CDC upzugraden.
  
```sql
USE <database name>
EXEC sys.sp_cdc_vupgrade  
``` 
 
## <a name="see-also"></a>Weitere Informationen  
 [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-transact-sql.md) 
 <br>[Verwalten von Metadaten beim Bereitstellen einer Datenbank auf einem anderen Server](manage-metadata-when-making-a-database-available-on-another-server.md)  
 [Trennen einer Datenbank](../../relational-databases/databases/detach-a-database.md)  
  
