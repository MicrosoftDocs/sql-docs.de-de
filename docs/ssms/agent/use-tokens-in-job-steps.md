---
description: Verwenden von Token in Auftragsschritten
title: Verwenden von Token in Auftragsschritten
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- job steps [SQL Server Agent]
- security [SQL Server Agent], enabling alert job step tokens
- SQL Server Agent jobs, job steps
- tokens [SQL Server]
- escape macros [SQL Server Agent]
ms.assetid: 105bbb66-0ade-4b46-b8e4-f849e5fc4d43
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 7463f708819d772ef8d9e94a8f8b58ca4a7e4f7a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344683"
---
# <a name="use-tokens-in-job-steps"></a>Verwenden von Token in Auftragsschritten
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance) werden derzeit die meisten, aber nicht alle, SQL Server-Agent-Features unterstützt. Details dazu finden Sie unter [T-SQL-Unterschiede zwischen Azure SQL Managed Instance und SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent ermöglicht die Verwendung von Token in [!INCLUDE[tsql](../../includes/tsql-md.md)] -Auftragsschrittskripts. Durch die Verwendung von Token verfügen Sie beim Schreiben von Auftragsschritten über dieselbe Flexibilität, die die Verwendung von Variablen beim Schreiben von Softwareprogrammen bietet. Die von Ihnen in ein Auftragsschrittskript eingefügten Token werden vom [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent zur Ausführungszeit ersetzt, bevor der Auftragsschritt vom [!INCLUDE[tsql](../../includes/tsql-md.md)] -Subsystem ausgeführt wird.  
  
  
## <a name="understanding-using-tokens"></a>Grundlegendes zum Verwenden von Token  
  
> [!IMPORTANT]  
> Jeder Windows-Benutzer mit Schreibberechtigungen für das Windows-Ereignisprotokoll kann auf Auftragsschritte zugreifen, die durch [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent-Warnungen oder WMI-Warnungen aktiviert werden. Zur Vermeidung dieses Sicherheitsrisikos sind [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent-Tokens, die in von Warnungen aktivierten Aufträgen verwendet werden können, standardmäßig deaktiviert. Dabei handelt es sich um folgende Token: **A-DBN**, **A-SVR**, **A-ERR**, **A-SEV**, **A-MSG** und **WMI(** _Eigenschaft_ **)** . Beachten Sie, dass in dieser Version die Verwendung von Token auf alle Warnungen ausgeweitet ist.  
>   
> Wenn Sie diese Token verwenden müssen, stellen Sie zuvor sicher, dass ausschließlich Mitglieder von vertrauenswürdigen Windows-Sicherheitsgruppen, wie der Administratorengruppe, über Schreibberechtigungen für das Ereignisprotokoll des Computers verfügen, auf dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ausgeführt wird. Klicken Sie dann zum Aktivieren dieser Token im Objekt-Explorer mit der rechten Maustaste auf **SQL Server-Agent** , wählen Sie **Eigenschaften**, und wählen Sie anschließend auf der Seite **Warnungssystem** die Option **Token für alle Auftragsantworten auf Warnungen ersetzen** aus.  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent-Token können einfach und effizient ersetzt werden: Der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent ersetzt das Token durch ein genaues Zeichenfolgenliteral. Die Groß- und Kleinschreibung ist bei Token grundsätzlich zu beachten. Dies muss in den Auftragsschritten berücksichtigt werden, damit die verwendeten Token richtig angegeben werden oder die Ersatzzeichenfolge in den richtigen Datentyp konvertiert wird.  
  
Mit der folgenden Anweisung können Sie z. B. den Namen der Datenbank in einem Auftragsschritt drucken:  
  
`PRINT N'Current database name is $(ESCAPE_SQUOTE(A-DBN))' ;`  
  
In diesem Beispiel wird das Makro **ESCAPE_SQUOTE** mit dem Token **A-DBN** eingefügt. Zur Laufzeit wird das Token **A-DBN** durch den entsprechenden Datenbanknamen ersetzt. Das Escapemakro umgeht alle einfachen Anführungszeichen, die möglicherweise unbeabsichtigt in der Zeichenfolge, durch die das Token ersetzt werden soll, übergeben werden. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent ersetzt in der endgültigen Zeichenfolge jedes einfache Anführungszeichen durch zwei einfache Anführungszeichen.  
  
Wenn beispielsweise als Ersatz für das Token die Zeichenfolge `AdventureWorks2012'SELECT @@VERSION --`übergeben wird, wird im [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent-Auftragsschritt der folgende Befehl ausgeführt:  
  
`PRINT N'Current database name is AdventureWorks2012''SELECT @@VERSION --' ;`  
  
In diesem Fall wird die eingefügte Anweisung `SELECT @@VERSION`nicht ausgeführt. Stattdessen wird durch das überzählige einfache Anführungszeichen der Server veranlasst, die eingefügte Anweisung als Zeichenfolge zu analysieren. Wenn die Zeichenfolge, durch die das Token ersetzt werden soll, keine einfachen Anführungszeichen enthält, werden keine Zeichen umgangen, und der Auftragsschritt mit dem Token wird wie beabsichtigt ausgeführt.  
  
Verwenden Sie zum Debuggen der Tokenverwendung in Auftragsschritten Druckanweisungen, wie z. B. `PRINT N'$(ESCAPE_SQUOTE(SQLDIR))'` , und speichern Sie die Auftragsschrittausgabe in einer Datei oder Tabelle. Auf der Seite **Erweitert** des Dialogfelds **Auftragsschritt-Eigenschaften** können Sie eine Datei oder Tabelle für die Auftragsschrittausgabe angeben.  
  
## <a name="sql-server-agent-tokens-and-macros"></a>SQL Server-Agent-Token und -Makros  
In der folgenden Tabelle sind die vom [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent unterstützten Token und Makros aufgelistet und beschrieben.  
  
### <a name="sql-server-agent-tokens"></a>SQL Server-Agent-Token  
  
|Token|BESCHREIBUNG|  
|---------|---------------|  
|**(A-DBN)**|Datenbankname. Wenn der Auftrag durch eine Warnung ausgeführt wird, ersetzt der Wert des Datenbanknamens dieses Token im Auftragsschritt automatisch.|  
|**(A-SVR)**|Servername. Wenn der Auftrag durch eine Warnung ausgeführt wird, ersetzt der Wert des Servernamens dieses Token im Auftragsschritt automatisch.|  
|**(A-ERR)**|Fehlernummer. Wenn der Auftrag durch eine Warnung ausgeführt wird, ersetzt der Wert der Fehlernummer dieses Token im Auftragsschritt automatisch.|  
|**(A-SEV)**|Fehlerschweregrad. Wenn der Auftrag durch eine Warnung ausgeführt wird, ersetzt der Wert des Fehlerschweregrads dieses Token im Auftragsschritt automatisch.|  
|**(A-MSG)**|Meldungstext. Wenn der Auftrag durch eine Warnung ausgeführt wird, ersetzt der Wert des Meldungstexts dieses Token im Auftragsschritt automatisch.|  
|**(JOBNAME)**|Der Name des Auftrags. Dieses Token ist nur in SQL Server 2016 und höher verfügbar.|  
|**(STEPNAME)**|Der Name des Schritts. Dieses Token ist nur in SQL Server 2016 und höher verfügbar.|  
|**(DATE)**|Das aktuelle Datum (im Format YYYYMMDD).|  
|**(INST)**|Der Instanzname. Für eine Standardinstanz erhält dieses Token den Standardinstanznamen: MSSQLSERVER.|  
|**(JOBID)**|Auftrags-ID.|  
|**(MACH)**|Name des Computers|  
|**(MSSA)**|Name des Master-SQLServerAgent-Diensts.|  
|**(OSCMD)**|Das Präfix des Programms, das zur Ausführung der **CmdExec** -Auftragsschritte verwendet wird.|  
|**(SQLDIR)**|Verzeichnis, in dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installiert ist. Der Standardwert lautet "C:\Programme\Microsoft SQL Server\MSSQL".|  
|**(SQLLOGDIR)**|Ersetzungstoken für den Pfad des SQL Server-Fehlerprotokollordners, beispielsweise $(ESCAPE_SQUOTE(SQLLOGDIR)). Dieses Token ist nur in SQL Server 2014 und höher verfügbar.|  
|**(STEPCT)**|Die Angabe, wie oft dieser Schritt ausgeführt wurde (ohne Abbrüche). Mit diesem Token kann der Schrittbefehl den Abbruch einer aus mehreren Schritten bestehenden Schleife erzwingen.|  
|**(STEPID)**|Schritt-ID.|  
|**(SRVR)**|Name des Computers, auf dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]ausgeführt wird. Wenn es sich bei der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz um eine benannte Instanz handelt, ist auch der Name der Instanz angegeben.|  
|**(TIME)**|Die aktuelle Zeit (im Format HHMMSS).|  
|**(STRTTM)**|Uhrzeit (im Format HHMMSS), zu der die Ausführung des Auftrags begonnen hat.|  
|**(STRTDT)**|Datum (im Format YYYYMMDD), an dem die Ausführung des Auftrags begonnen hat.|  
|**(WMI(** _Eigenschaft_ **))**|Bei Aufträgen, die als Antwort auf WMI-Warnungen ausgeführt werden, ist dies der Wert der durch *property* angegebenen Eigenschaft. In `$(WMI(DatabaseName))` ist beispielsweise der Wert der **DatabaseName** -Eigenschaft für das WMI-Ereignis angegeben, das die Ausführung der Warnung verursacht hat.|  
  
### <a name="sql-server-agent-escape-macros"></a>SQL Server-Agent-Escapemakros  
  
|Escapemakros|BESCHREIBUNG|  
|-----------------|---------------|  
|**$(ESCAPE_SQUOTE(** _token\_name_ **))**|Umgeht einfache Anführungszeichen (') in der Token-Ersetzungszeichenfolge. Ein einfaches Anführungszeichen wird durch zwei einfache Anführungszeichen ersetzt.|  
|**$(ESCAPE_DQUOTE(** _token\_name_ **))**|Umgeht doppelte Anführungszeichen (") in der Token-Ersetzungszeichenfolge. Ein doppeltes Anführungszeichen wird durch zwei doppelte Anführungszeichen ersetzt.|  
|**$(ESCAPE_RBRACKET(** _token\_name_ **))**|Umgeht rechte eckige Klammern (]) in der Token-Ersetzungszeichenfolge. Ersetzt jede rechte eckige Klammer durch zwei rechte eckige Klammern.|  
|**$(ESCAPE_NONE(** _token\_name_ **))**|Ersetzt Token, ohne irgendein Zeichen in der Zeichenfolge zu umgehen. Dieses Makro wird zur Unterstützung der Abwärtskompatibilität in Umgebungen bereitgestellt, in denen Token-Ersetzungszeichenfolgen nur von vertrauenswürdigen Benutzern erwartet werden. Weitere Informationen finden Sie weiter unten im Abschnitt zum Aktualisieren von Auftragsschritten für die Verwendung von Makros.|  
  
## <a name="updating-job-steps-to-use-macros"></a>Aktualisieren von Auftragsschritten für die Verwendung von Makros  
Die folgende Tabelle beschreibt, wie die symbolische Ersetzung von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent behandelt wird. Um die Tokenersetzung durch Warnungen ein- oder auszuschalten, klicken Sie im Objekt-Explorer mit der rechten Maustaste auf **SQL Server-Agent** , wählen Sie **Eigenschaften**, und aktivieren bzw. deaktivieren Sie anschließend auf der Seite **Warnungssystem** das Kontrollkästchen **Token für alle Auftragsantworten auf Warnungen ersetzen** .  
  
|Tokensyntax|Tokenersetzung durch Warnungen aktiviert|Tokenersetzung durch Warnungen deaktiviert|  
|----------------|------------------------------|-------------------------------|  
|ESCAPE-Makro verwendet|Alle Token in Auftragsschritten werden erfolgreich ersetzt.|Durch Warnungen aktivierte Token werden nicht ersetzt. Dabei handelt es sich um folgende Token: **A-DBN**, **A-SVR**, **A-ERR**, **A-SEV**, **A-MSG** und **WMI(** _Eigenschaft_ **)** . Andere, statische Token werden erfolgreich ersetzt.|  
|Kein ESCAPE-Makro verwendet|Alle Auftragsschritte mit Token werden nicht ausgeführt.|Alle Auftragsschritte mit Token werden nicht ausgeführt.|  
  
## <a name="token-syntax-update-examples"></a>Beispiele für Updates der Tokensyntax  
  
### <a name="a-using-tokens-in-non-nested-strings"></a>A. Verwenden von Token in nicht geschachtelten Zeichenfolgen  
Im folgenden Beispiel wird gezeigt, wie Sie ein einfaches, nicht geschachteltes Skript mit dem entsprechenden Escapemakro aktualisieren. Vor dem Ausführen des Updateskripts verwendet das folgende Auftragsschrittskript ein Auftragsschritttoken, um den entsprechenden Datenbanknamen zu drucken:  
  
`PRINT N'Current database name is $(A-DBN)' ;`  
  
Nach dem Ausführen des Updateskripts wird vor dem `ESCAPE_NONE` -Token ein `A-DBN` -Makro eingefügt. Da die Druckzeichenfolge durch einfache Anführungszeichen begrenzt wird, müssen Sie den Auftragsschritt aktualisieren, indem Sie das `ESCAPE_SQUOTE` -Makro folgendermaßen einfügen:  
  
`PRINT N'Current database name is $(ESCAPE_SQUOTE(A-DBN))' ;`  
  
### <a name="b-using-tokens-in-nested-strings"></a>B. Verwenden von Token in geschachtelten Zeichenfolgen  
In Auftragsschritten, in denen Token in geschachtelten Zeichenfolgen oder Anweisungen verwendet werden, sollten die geschachtelten Anweisungen als Einzelanweisungen neu geschrieben werden, bevor die entsprechenden Escapemakros eingefügt werden.  
  
Betrachten Sie z. B. den folgenden Auftragsschritt, der das Token `A-MSG` verwendet und nicht mit einem Escapemakro aktualisiert wurde:  
  
`PRINT N'Print ''$(A-MSG)''' ;`  
  
Nach dem Ausführen des Updateskripts wird mit dem Token ein `ESCAPE_NONE` -Makro eingefügt. In diesem Fall müssten Sie das Skript jedoch ohne Verwenden von Schachtelungen folgendermaßen neu schreiben und das `ESCAPE_SQUOTE` -Makro einfügen, damit die in der Token-Ersetzungszeichenfolge möglicherweise übergebenen Begrenzungszeichen richtig umgangen werden:  
  
```sql
DECLARE @msgString nvarchar(max);
SET @msgString = '$(ESCAPE_SQUOTE(A-MSG))';
SET @msgString = QUOTENAME(@msgString,'''');
PRINT N'Print ' + @msgString;
```
  
Beachten Sie auch, dass in diesem Beispiel mit der Funktion QUOTENAME das Anführungszeichen festgelegt wird.  
  
### <a name="c-using-tokens-with-the-escape_none-macro"></a>C. Verwenden von Token mit dem ESCAPE_NONE-Makro  
Das folgende Beispiel ist Teil eines Skripts, mit dem die Auftrags-ID ( `job_id` ) aus der `sysjobs` -Tabelle abgerufen wird und mithilfe des `JOBID` -Tokens die zuvor im Skript als binärer Datentyp deklarierte `@JobID` -Variable aufgefüllt wird. Beachten Sie, dass mit dem `ESCAPE_NONE` -Token das `JOBID` -Makro verwendet wird, da für binäre Datentypen keine Begrenzungszeichen erforderlich sind. In diesem Fall muss der Auftragsschritt nach Ausführen des Updateskripts nicht aktualisiert werden.  
  
```sql
SELECT * FROM msdb.dbo.sysjobs  
WHERE @JobID = CONVERT(uniqueidentifier, $(ESCAPE_NONE(JOBID)));
```
  
## <a name="see-also"></a>Weitere Informationen  
[Implementieren von Aufträgen](../../ssms/agent/implement-jobs.md)  
[Verwalten von Auftragsschritten](../../ssms/agent/manage-job-steps.md)  
