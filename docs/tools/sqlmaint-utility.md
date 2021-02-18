---
title: sqlmaint (Hilfsprogramm)
description: Verwenden Sie in SQL Server „sqlmaint“, um DBCC-Überprüfungen auszuführen, eine Datenbank und das zugehörige Transaktionsprotokoll zu sichern, Statistiken zu aktualisieren und Indizes neu zu erstellen.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- database maintenance plans [SQL Server]
- sqlmaint utility
- maintaining databases [SQL Server]
- backups [SQL Server], sqlmaint utility
- command prompt utilities [SQL Server], sqlmaint
- maintenance plans [SQL Server], command prompt
- backing up [SQL Server], sqlmaint utility
ms.assetid: 937a9932-4aed-464b-b97a-a5acfe6a50de
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c822bd2acbd92bcfd9b3008c78c3ccef244f3c88
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338551"
---
# <a name="sqlmaint-utility"></a>sqlmaint (Hilfsprogramm)
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]
  Das **sqlmaint** -Hilfsprogramm führt eine Reihe angegebener Wartungsvorgänge für eine oder mehrere Datenbanken aus. Verwenden Sie **sqlmaint** , um DBCC-Überprüfungen auszuführen, eine Datenbank und das zugehörige Transaktionsprotokoll zu sichern, Statistiken zu aktualisieren und Indizes neu zu erstellen. Bei allen Datenbankwartungsaktivitäten wird ein Bericht generiert, der an eine festgelegte Textdatei, HTML-Datei oder ein festgelegtes E-Mail-Konto gesendet werden kann. **sqlmaint** führt Datenbankwartungspläne aus, die in früheren Versionen von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]erstellt wurden. Verwenden Sie das Hilfsprogramm [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] dtexec [, um Wartungspläne von](../integration-services/packages/dtexec-utility.md)über die Eingabeaufforderung auszuführen.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepNextAvoid](../includes/ssnotedepnextavoid-md.md)] Verwenden Sie stattdessen die Wartungsplanfunktion von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Weitere Informationen zu Wartungsplänen finden Sie unter [Wartungspläne](../relational-databases/maintenance-plans/maintenance-plans.md).  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sqlmaint   
[-?] |  
[  
     [-S server_name[\instance_name]]  
     [-U login_ID [-P password]]  
     {  
          [-D database_name | -PlanName name | -PlanID guid ]  
          [-Rpt text_file]  
          [-To operator_name]  
          [-HtmlRpt html_file [-DelHtmlRpt <time_period>] ]  
          [-RmUnusedSpace threshold_percentfree_percent]  
          [-CkDB | -CkDBNoIdx]  
          [-CkAl | -CkAlNoIdx]  
          [-CkCat]  
          [-UpdOptiStats sample_percent]  
          [-RebldIdx free_space]  
          [-SupportComputedColumn]  
          [-WriteHistory]  
          [  
               {-BkUpDB [backup_path] | -BkUpLog [backup_path] }  
               {-BkUpMedia  
                    {DISK [  
                           [-DelBkUps <time_period>]   
                           [-CrBkSubDir ]   
                           [-UseDefDir ]   
                          ]  
                     | TAPE   
                    }  
               }  
               [-BkUpOnlyIfClean]  
               [-VrfyBackup]  
          ]  
     }  
]  
<time_period> ::=  
number[minutes | hours | days | weeks | months]  
```  
  
## <a name="arguments"></a>Argumente  
 Die Parameter und ihre Werte müssen jeweils durch ein Leerzeichen getrennt werden. So muss beispielsweise zwischen **-S** und *Servername* ein Leerzeichen stehen.  
  
 **-?**  
 Gibt an, dass das Syntaxdiagramm für **sqlmaint** zurückgegeben werden soll. Dieser Parameter darf nur alleine verwendet werden.  
  
 **-S** _server_name_[ **\\** _instance\_name_]  
 Gibt die Zielinstanz von [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] an. Geben Sie _server\_name_ an, um eine Verbindung mit der Standardinstanz von [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] auf diesem Server herzustellen. Geben Sie _server\_name_ **\\** _instance\_name_ an, um eine Verbindung mit der benannten Instanz von [!INCLUDE[ssDE](../includes/ssde-md.md)] auf diesem Server herzustellen. Wenn kein Server angegeben wird, stellt **sqlmaint** eine Verbindung mit der Standardinstanz von [!INCLUDE[ssDE](../includes/ssde-md.md)] auf dem lokalen Computer her.  
  
 **-U** _login_ID_  
 Gibt die Anmelde-ID an, der beim Verbinden zum Server verwendet werden soll. Wenn dieses Argument nicht angegeben wird, versucht **sqlmaint** , die [!INCLUDE[msCoName](../includes/msconame-md.md)] Windows-Authentifizierung zu verwenden. Wenn die *Anmelde-ID* Sonderzeichen enthält, muss das Argument in doppelte Anführungszeichen (") eingeschlossen werden. Andernfalls sind die doppelten Anführungszeichen optional.  
  
> [!IMPORTANT]  
>  Verwenden Sie nach Möglichkeit die Windows-Authentifizierung.  
  
 **-P** _password_  
 Gibt das Kennwort für die Anmelde-ID an. Nur gültig, wenn der Parameter **-U** ebenfalls angegeben wird. Wenn das *Kennwort* Sonderzeichen enthält, muss das Argument in doppelte Anführungszeichen eingeschlossen werden. Andernfalls sind die doppelten Anführungszeichen optional.  
  
> [!IMPORTANT]  
>  Das Kennwort wird nicht maskiert. Verwenden Sie nach Möglichkeit die Windows-Authentifizierung.  
  
 **-D** _database_name_  
 Gibt den Namen der Datenbank an, in der der Wartungsvorgang durchgeführt werden soll. Wenn der *Datenbankname* Sonderzeichen enthält, muss das Argument in doppelte Anführungszeichen eingeschlossen werden. Andernfalls sind die doppelten Anführungszeichen optional.  
  
 **-PlanName** _name_  
 Gibt den Namen eines Datenbank-Wartungsplans an, der mithilfe des Datenbank-Wartungsplanungs-Assistenten definiert wurde. Von den Informationen, die dieser Plan enthält, verwendet **sqlmaint** nur die Liste der Datenbanken im Plan. Alle Wartungsaktivitäten, die Sie in den anderen **sqlmaint** -Parametern angeben, werden auf die in dieser Liste aufgeführten Datenbanken angewendet.  
  
 **-PlanID** _guid_  
 Gibt einen global eindeutigen Bezeichner (Globally Unique Identifier, GUID) eines Datenbank-Wartungsplans an, der mithilfe des Datenbank-Wartungsplanungs-Assistenten definiert wurde. Von den Informationen, die dieser Plan enthält, verwendet **sqlmaint** nur die Liste der Datenbanken im Plan. Alle Wartungsaktivitäten, die Sie in den anderen **sqlmaint** -Parametern angeben, werden auf die in dieser Liste aufgeführten Datenbanken angewendet. Der GUID muss mit einem der plan_id-Werte in msdb.dbo.sysdbmaintplans übereinstimmen.  
  
 **-Rpt** _text_file_  
 Gibt den vollständigen Pfad und Namen der Datei an, in der der Bericht generiert werden soll. Der Bericht wird auch auf dem Bildschirm generiert. Der Bericht verwaltet Versionsinformationen, indem er das Datum zum Dateinamen hinzufügt. Das Datum wird folgendermaßen generiert: am Ende des Dateinamens, aber vor dem Punkt im Format _ *yyyyMMddhhmm*. *yyyy* = Jahr, *MM* = Monat, *dd* = Tag, *hh* = Stunde, *mm* = Minute.  
  
 Wenn Sie das Hilfsprogramm am 1. Dezember 1996 um 10:23 Uhr ausführen und dies der *Textdatei* -Wert ist:  
  
```  
c:\Program Files\Microsoft SQL Server\Mssql\Backup\AdventureWorks2012_maint.rpt  
```  
  
 Die generierte Datei hat dann folgenden Namen:  
  
```  
c:\Program Files\Microsoft SQL Server\Mssql\Backup\AdventureWorks2012_maint_199612011023.rpt  
```  
  
 Für *extdatei* ist der vollständige UNC-Dateiname (Universal Naming Convention) erforderlich, wenn **sqlmaint** auf einen Remoteserver zugreift.  
  
 **-To** _operator_name_  
 Gibt den Operator an, an den der generierte Bericht über SQL Mail gesendet wird.  
  
 **-HtmlRpt** _html_file_  
 Gibt den vollständigen Pfad und Namen der Datei an, in der der HTML-Bericht generiert werden soll. **sqlmaint** generiert den Dateinamen, indem eine Zeichenfolge im Format _ *yyyyMMddhhmm* an den Dateinamen angefügt wird, ebenso wie beim **-Rpt** -Parameter.  
  
 Für *HTML-Datei* ist der vollständige UNC-Dateiname erforderlich, wenn **sqlmaint** auf einen Remoteserver zugreift.  
  
 **-DelHtmlRpt** \<*time_period*>  
 Gibt an, dass alle HTML-Berichte im Berichtsverzeichnis gelöscht werden sollen, wenn das Zeitintervall nach dem Erstellen der Berichtsdatei den Wert für \<*time_period*> überschreitet. **-DelHtmlRpt** sucht nach Dateien, deren Namen dem Muster entsprechen, das aus dem *HTML-Datei*-Parameter generiert wurde. Wenn für *html_file* der Wert „C:\Programme\Microsoft SQL Server\Mssql\Backup\AdventureWorks2012_maint.htm“ angegeben wird, bewirkt **-DelHtmlRpt**, dass **sqlmaint** alle Dateien löscht, deren Namen dem Muster „C:\Programme\Microsoft SQL Server\Mssql\Backup\AdventureWorks2012_maint\*.htm“ entsprechen und die älter als der angegebene Wert für \<*time_period*> sind.  
  
 **-RmUnusedSpace** _threshold_percent free_percent_  
 Gibt an, dass nicht verwendeter Speicherplatz aus der mit **-D** angegebenen Datenbank entfernt wird. Diese Option ist nur für Datenbanken nützlich, die für das automatische Wachstum definiert wurden. *Schwellenwert_Prozent* gibt die Größe in Megabytes an, die die Datenbank erreichen muss, bevor **sqlmaint** versucht, nicht verwendeten Datenspeicherplatz zu entfernen. Wenn die Datenbank kleiner als *Schwellenwert_Prozent* ist, wird keine Aktion ausgeführt. *Prozent_frei* gibt an, wie viel nicht verwendeter Speicherplatz in der Datenbank verbleiben muss. Die Angabe erfolgt als Prozentsatz der endgültigen Größe der Datenbank. Wenn eine 200 MB große Datenbank z.B. 100 MB an Daten enthält, bewirkt die Angabe des Werts 10 für *Prozent_frei* , dass die endgültige Größe der Datenbank 110 MB beträgt. Beachten Sie, dass eine Datenbank nicht erweitert wird, wenn sie kleiner als der Wert ist, der sich aus *Prozent_frei* zuzüglich der Menge der Daten in der Datenbank ergibt. Wenn eine 108 MB große Datenbank z.B. 100 MB an Daten enthält, bewirkt die Angabe des Werts 10 für *Prozent_frei* nicht, dass die Datenbank auf 110 MB erweitert wird; die Datenbank bleibt 108 MB groß.  
  
 **-CkDB** |  **-CkDBNoIdx**  
 Gibt an, dass eine DBCC CHECKDB-Anweisung oder eine DBCC CHECKDB-Anweisung mit der Option NOINDEX in der Datenbank ausgeführt werden soll, die mit **-D** angegeben wurde. Weitere Informationen finden Sie unter DBCC CHECKDB.  
  
 Wenn die Datenbank zum Zeitpunkt der Ausführung von *sqlmaint* verwendet wird, wird eine Warnung in die Datei geschrieben, die mit **Textdatei** angegeben wurde.  
  
 **-CkAl** |  **-CkAlNoIdx**  
 Gibt an, dass eine DBCC CHECKALLOC-Anweisung mit der Option NOINDEX in der Datenbank ausgeführt werden soll, die mit **-D** angegeben wurde. Weitere Informationen finden Sie unter [DBCC CHECKALLOC &#40;Transact-SQL&#41;](../t-sql/database-console-commands/dbcc-checkalloc-transact-sql.md).  
  
 **-CkCat**  
 Gibt an, dass eine DBCC CHECKCATALOG-Anweisung (Transact-SQL) in der Datenbank ausgeführt werden soll, die mit **-D** angegeben wurde. Weitere Informationen finden Sie unter [DBCC CHECKCATALOG &#40;Transact-SQL&#41;](../t-sql/database-console-commands/dbcc-checkcatalog-transact-sql.md).  
  
 **-UpdOptiStats** _sample_percent_  
 Gibt an, dass für jede Tabelle der Datenbank die folgende Anweisung ausgeführt werden soll:  
  
```  
UPDATE STATISTICS table WITH SAMPLE sample_percent PERCENT;  
```  
  
 Falls die Tabellen berechnete Spalten enthalten, müssen Sie auch das **-SupportedComputedColumn** -Argument angeben, wenn Sie **-UpdOptiStats** verwenden.  
  
 Weitere Informationen finden Sie unter [UPDATE STATISTICS &#40;Transact-SQL&#41;](../t-sql/statements/update-statistics-transact-sql.md)erstellt wurden.  
  
 **-RebldIdx** _free_space_  
 Gibt an, dass die Indizes der Tabellen in der Zieldatenbank neu erstellt werden sollen, wobei der *Speicher_frei* -Prozentwert als Umkehrwert des Füllfaktors verwendet wird. Wenn der *Speicher_frei* -Prozentsatz z.B. 30 beträgt, dann ist der verwendete Füllfaktor 70. Wenn ein *Speicher_frei* -Prozentwert von 100 angegeben wird, werden die Indizes mit dem ursprünglichen Füllfaktorwert neu erstellt.  
  
 Falls die Indizes für berechnete Spalten erstellt wurden, müssen Sie zudem das **-SupportComputedColumn-** Argument angeben, wenn Sie **-RebldIdx** verwenden.  
  
 **-SupportComputedColumn-**  
 Muss angegeben werden, um DBCC-Wartungsbefehle mit **sqlmaint** für berechnete Spalten auszuführen.  
  
 **-WriteHistory**  
 Gibt an, dass für jede von **sqlmaint** durchgeführte Wartungsaktion ein Eintrag in msdb.dbo.sysdbmaintplan_history vorgenommen wird. Wenn **-PlanName** oder **-PlanID** angegeben ist, wird für die Einträge in sysdbmaintplan_history die ID des angegebenen Plans verwendet. Wenn **-D** angegeben ist, werden für die Einträge in sysdbmaintplan_history Nullen für die Plan-ID verwendet.  
  
 **-BkUpDB** [ *Sicherungspfad*] |  **-BkUpLog** [ *Sicherungspfad* ]  
 Gibt eine Sicherungsaktion an. **-BkUpDb** sichert die gesamte Datenbank. **-BkUpLog** sichert nur das Transaktionsprotokoll.  
  
 *Sicherungspfad* gibt das Verzeichnis für die Sicherung an. *Sicherungspfad* wird nicht benötigt, wenn auch **-UseDefDir** angegeben wird, und wird von **-UseDefDir** überschrieben, wenn beide angegeben werden. Die Sicherung kann in einem Verzeichnis oder einer Bandmediumadresse, z.B. \\\\.\TAPE0, platziert werden. Der Dateiname für eine Datenbanksicherung wird automatisch folgendermaßen generiert:  
  
```  
dbname_db_yyyyMMddhhmm.BAK  
  
```  
  
 Hierbei gilt:  
  
-   *dbname* ist der Name der Datenbank, die gesichert werden soll.  
  
-   *yyyyMMddhhmm* ist die Uhrzeit des Sicherungsvorgangs ( *yyyy* = Jahr, *MM* = Monat, *dd* = Tag, *hh* = Stunde und *mm* = Minute).  
  
 Der Dateiname für eine Transaktionssicherung wird automatisch in einem ähnlichen Format generiert:  
  
```  
dbname_log_yyyymmddhhmm.BAK  
  
```  
  
 Wenn Sie den **-BkUpDB** -Parameter verwenden, müssen Sie mithilfe des **-BkUpMedia** -Parameters auch das Medium angeben.  
  
 **-BkUpMedia**  
 Gibt den Medientyp der Sicherung an, entweder DISK oder TAPE.  
  
 **DISK**  
 Gibt an, dass das Sicherungsmedium ein Datenträger ist.  
  
 **-DelBkUps**< *Zeitraum* >  
 Gibt bei Datenträgersicherungen an, dass alle Sicherungsdateien im Sicherungsverzeichnis gelöscht werden sollen, wenn das Zeitintervall nach dem Erstellen der Sicherungsdatei den Wert für \<*time_period*> überschreitet.  
  
 **-CrBkSubDir**  
 Gibt bei Datenträgersicherungen an, dass ein Unterverzeichnis im Verzeichnis [*Sicherungspfad*] oder im Standardsicherungsverzeichnis erstellt werden soll, wenn **-UseDefDir** ebenfalls angegeben ist. Der Name des Unterverzeichnisses wird anhand des Datenbanknamens generiert, der mit **-D** angegeben wurde. **-CrBkSubDir** bietet ein einfaches Verfahren, um alle Sicherungen für verschiedene Datenbanken in unterschiedlichen Unterverzeichnissen abzulegen, ohne den *Sicherungspfad* -Parameter ändern zu müssen.  
  
 **-UseDefDir**  
 Gibt für Datenträgersicherungen an, dass die Sicherungsdatei im Standardsicherungsverzeichnis erstellt werden soll. **UseDefDir** überschreibt *Sicherungspfad* , wenn beide Parameter angegeben werden. Bei einer Standardeinrichtung von [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] befindet sich das Standardsicherungsverzeichnis unter dem Pfad „C:\Program Files\Microsoft SQL Server\MSSQL10_50.MSSQLSERVER\MSSQL\Backup“.  
  
 **TAPE**  
 Gibt an, dass das Sicherungsmedium ein Band ist.  
  
 **-BkUpOnlyIfClean**  
 Gibt an, dass eine Sicherung nur dann erfolgt, wenn bei den mit **-Ck** angegebenen Überprüfungen keine Probleme bei den Daten gefunden wurden. Wartungsaktionen werden in derselben Reihenfolge ausgeführt, in der sie an der Eingabeaufforderung angezeigt werden. Geben Sie die Parameter **-CkDB**, **-CkDBNoIdx**, **-CkAl**, **-CkAlNoIdx**, **-CkTxtAl** oder **-CkCat** vor den Parametern **-BkUpDB**/ **-BkUpLog** an, wenn Sie auch **-BkUpOnlyIfClean** angeben möchten. Andernfalls erfolgt die Sicherung unabhängig davon, ob bei der Überprüfung Probleme gemeldet werden oder nicht.  
  
 **-VrfyBackup**  
 Gibt an, dass für die Sicherung RESTORE VERIFYONLY ausgeführt wird, sobald die Sicherung abgeschlossen ist.  
  
 *number*[**minutes**\| **hours**\| **day**\| **weeks**\| **months**]  
 Gibt das Zeitintervall an, das verwendet wurde, um zu bestimmen, ob ein Bericht oder eine Sicherungsdatei alt genug ist, um gelöscht zu werden. *number* ist eine ganze Zahl, gefolgt von einer Zeiteinheit (ohne Leerzeichen). Gültige Beispiele:  
  
-   **12weeks**  
  
-   **3months**  
  
-   **15days**  
  
 Wird nur *number* angegeben, wird **weeks** als standardmäßiges Datumsteil verwendet.  
  
## <a name="remarks"></a>Bemerkungen  
 Das Hilfsprogramm **sqlmaint** führt Wartungsvorgänge für eine oder mehrere Datenbanken aus. Wenn **-D** angegeben wird, werden die Vorgänge, die mit den verbleibenden Schaltern angegeben werden, nur für die angegebene Datenbank ausgeführt. Wenn **-PlanName** oder **-PlanID** angegeben wird, ruft **sqlmaint** aus dem angegebenen Wartungsplan nur die Liste der Datenbanken im Plan ab. Alle in den verbleibenden **sqlmaint** -Parametern angegebenen Vorgänge werden für jede Datenbank in der Liste ausgeführt, die aus dem Plan abgerufen wurde. Das Hilfsprogramm **sqlmaint** wendet keine der im Plan selbst definierten Wartungsaktivitäten an.  
  
 Das Hilfsprogramm **sqlmaint** gibt bei erfolgreicher Ausführung 0 und bei Auftreten eines Fehlers 1 zurück. Ein Fehler wird in folgenden Fällen gemeldet:  
  
-   Eine Wartungsaktion ist fehlgeschlagen.  
  
-   Die Überprüfungen **-CkDB**, **-CkDBNoIdx**, **-CkAl**, **-CkAlNoIdx**, **-CkTxtAl** oder **-CkCat** finden Probleme mit den Daten.  
  
-   Ein allgemeiner Fehler ist aufgetreten.  
  
## <a name="permissions"></a>Berechtigungen  
 Das Hilfsprogramm **sqlmaint** kann von jedem Windows-Benutzer ausgeführt werden, der über die Berechtigung **Lesen und Ausführen** für die Datei `sqlmaint.exe`verfügt, die standardmäßig im Ordner `x:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER1\MSSQL\Binn` gespeichert ist. Darüber hinaus muss der mit [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -login_ID **angegebene** -Anmeldename über die [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Berechtigungen verfügen, die zum Ausführen der jeweiligen Aktion erforderlich sind. Wird bei der Verbindung zu [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] die Windows-Authentifizierung verwendet, muss der dem authentifizierten Windows-Benutzer zugeordnete [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Anmeldename über die [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Berechtigungen zum Ausführen der jeweiligen Aktion verfügen.  
  
 Beispielsweise erfordert die Verwendung von **-BkUpDB** die Berechtigung zum Ausführen der BACKUP-Anweisung. Die Verwendung des **-UpdOptiStats** -Arguments erfordert zudem die Berechtigung zum Ausführen der UPDATE STATISTICS-Anweisung. Weitere Informationen finden Sie in den Abschnitten zu Berechtigungen in den entsprechenden Themen der Onlinedokumentation.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-performing-dbcc-checks-on-a-database"></a>A. Ausführen von DBCC-Überprüfungen für eine Datenbank  
  
```  
sqlmaint -S MyServer -D AdventureWorks2012 -CkDB -CkAl -CkCat -Rpt C:\MyReports\AdvWks_chk.rpt  
```  
  
### <a name="b-updating-statistics-using-a-15-sample-in-all-databases-in-a-plan-also-shrink-any-of-the-database-that-have-reached-110-mb-to-having-only-10-free-space"></a>B. Aktualisieren von Statistiken in allen Datenbanken in einem Plan mithilfe einer Stichprobe von 15 %. Weiterhin: Verkleinern aller Datenbanken, die eine Größe von 110 MB erreicht haben, sodass sie anschließend nur noch 10 % freien Speicherplatz aufweisen  
  
```  
sqlmaint -S MyServer -PlanName MyUserDBPlan -UpdOptiStats 15 -RmUnusedSpace 110 10  
```  
  
### <a name="c-backing-up-all-the-databases-in-a-plan-to-their-individual-subdirectories-in-the-default-xprogram-filesmicrosoft-sql-servermssql13mssqlservermssqlbackup-directory-also-delete-any-backups-older-than-2-weeks"></a>C. Sichern aller Datenbanken in einem Plan in ihre jeweiligen Unterverzeichnissen im Standardverzeichnis „x:\Programme\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Backup“. Weiterhin: Löschen aller Sicherungen, die älter als 2 Wochen sind  
  
```  
sqlmaint -S MyServer -PlanName MyUserDBPlan -BkUpDB -BkUpMedia DISK -UseDefDir -CrBkSubDir -DelBkUps 2weeks  
```  
  
### <a name="d-backing-up-a-database-to-the-default-xprogram-filesmicrosoft-sql-servermssql13mssqlservermssqlbackup-directory"></a>D: Sichern einer Datenbank in das Standardverzeichnis „x:\Programme\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Backup“.  
  
```  
sqlmaint -S MyServer -BkUpDB -BkUpMedia DISK -UseDefDir  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [BACKUP &#40;Transact-SQL&#41;](../t-sql/statements/backup-transact-sql.md)   
 [UPDATE STATISTICS &#40;Transact-SQL&#41;](../t-sql/statements/update-statistics-transact-sql.md)  
  
  
