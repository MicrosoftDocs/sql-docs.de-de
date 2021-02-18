---
title: osql (Hilfsprogramm)
description: Mit dem Hilfsprogramm „osql“ können Sie in SQL Server Transact-SQL-Anweisungen, Systemprozeduren und Skriptdateien an der Eingabeaufforderung eingeben. Osql verwendet ODBC für die Kommunikation mit dem Server.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- statements [SQL Server], command prompt
- QUIT command
- Transact-SQL statements, command prompt
- EXIT command
- operating systems [SQL Server], commands
- osql utility [SQL Server]
- stored procedures [SQL Server], command prompt
- scripts [SQL Server], command prompt
- RESET command
- GO command
- command prompt utilities [SQL Server], osql
- CTRL+C command
ms.assetid: cf530d9e-0609-4528-8975-ab8e08e40b9a
author: markingmyname
ms.author: maghan
ms.manageR: jroth
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/16/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017'
ms.openlocfilehash: 41c47f76dd0a4b81d1ccc337b65bf1c8c157b770
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100335986"
---
# <a name="osql-utility"></a>osql (Hilfsprogramm)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Das Hilfsprogramm **osql** ermöglicht es Ihnen, [!INCLUDE[tsql](../includes/tsql-md.md)] -Anweisungen, Systemprozeduren und Skriptdateien einzugeben. Dieses Hilfsprogramm verwendet ODBC, um Daten mit dem Server auszutauschen.  
  
> [!IMPORTANT]  
>  Diese Funktion wird in einer zukünftigen Version von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]entfernt. Verwenden Sie diese Funktion beim Entwickeln neuer Anwendungen nicht, und planen Sie das Ändern von Anwendungen, in denen sie aktuell verwendet wird. Verwenden Sie stattdessen **sqlcmd** . Weitere Informationen finden Sie unter [sqlcmd Utility](../tools/sqlcmd-utility.md).  
  
## <a name="syntax"></a>Syntax  
  
```  
  
osql  
[-?] |  
[-L] |  
[  
  {  
     {-Ulogin_id [-Ppassword]} | -E }  
     [-Sserver_name[\instance_name]] [-Hwksta_name] [-ddb_name]  
     [-ltime_out] [-ttime_out] [-hheaders]  
     [-scol_separator] [-wcolumn_width] [-apacket_size]  
     [-e] [-I] [-D data_source_name]  
     [-ccmd_end] [-q "query"] [-Q"query"]  
     [-n] [-merror_level] [-r {0 | 1}]  
     [-iinput_file] [-ooutput_file] [-p]  
     [-b] [-u] [-R] [-O]  
]  
```  
  
## <a name="arguments"></a>Argumente  
 **-?**  
 Zeigt eine Syntaxzusammenfassung der **osql** -Schalter an.  
  
 **-L**  
 Listet die lokal konfigurierten Server sowie die Namen der Server auf, die Nachrichten über das Netzwerk senden.  
  
> [!NOTE]  
>  Aufgrund der Übertragungsart in Netzwerken, ist es möglich, dass **osql** nicht zeitnah von allen Servern eine Antwort erhält. Aus diesem Grund kann die Liste der zurückgegebenen Server mit jedem Aufruf dieser Option variieren.  
  
 **-U** _login_id_  
 Die Benutzeranmelde-ID. Bei Anmelde-IDs wird die Groß- und Kleinschreibung beachtet.  
  
 **-P** _password_  
 Ein vom Benutzer angegebenes Kennwort. Falls die Option **-P** nicht verwendet wird, fordert **osql** zur Eingabe eines Kennworts auf. Falls die Option **-P** am Ende der Eingabeaufforderung ohne Angabe eines Kennworts verwendet wird, verwendet **osql** das Standardkennwort (NULL).  
  
> [!IMPORTANT]  
>  Verwenden Sie kein leeres Kennwort. Verwenden Sie ein sicheres Kennwort. Weitere Informationen finden Sie unter [Strong Passwords](../relational-databases/security/strong-passwords.md).  
  
 Bei Kennwörtern wird nach Groß- und Kleinschreibung unterschieden.  
  
 Über die OSQLPASSWORD-Umgebungsvariable können Sie ein Standardkennwort für die aktuelle Sitzung festlegen. Dadurch müssen Sie keine Kennwörter in Batchdateien hartcodieren.  
  
 Falls Sie kein Kennwort mit der Option **-P** angeben, überprüft **osql** zuerst die OSQLPASSWORD-Variable. Wurde kein Wert festgelegt, verwendet **osql** das Standardkennwort (NULL). Im folgenden Beispiel wird die OSQLPASSWORD-Variable an der Eingabeaufforderung festgelegt und greift dann auf das Hilfsprogramm **osql** zu:  
  
```  
C:\>SET OSQLPASSWORD=abracadabra  
C:\>osql   
```  
  
> [!IMPORTANT]  
>  Zum Maskieren des Kennworts sollten Sie die Option **-P** nicht in Verbindung mit der Option **-U** angeben. Drücken Sie stattdessen nach der Angabe von **osql** mit der Option **-U** und anderen Schaltern (geben Sie **-P** nicht an) die EINGABETASTE. Sie werden daraufhin von **osql** zur Eingabe eines Kennworts aufgefordert. Durch diese Methode wird sichergestellt, dass das Kennwort bei der Eingabe maskiert wird.  
  
 **-E**  
 Verwendet eine vertrauenswürdige Verbindung, statt ein Kennwort anzufordern.  
  
 **-S** _server\_name_[ **\\** _instance\_name_]  
 Gibt die [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Instanz an, mit der eine Verbindung hergestellt wird. Geben Sie *server_name* an, um eine Verbindung mit der Standardinstanz von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] auf diesem Server herzustellen. Geben Sie _server\_name_ **\\** _instance\_name_ an, um eine Verbindung mit der benannten Instanz von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] auf diesem Server herzustellen. Falls kein Server angegeben ist, stellt **osql** eine Verbindung mit der Standardinstanz von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] auf dem lokalen Computer her. Diese Option ist erforderlich, wenn **osql** von einem Remotecomputer im Netzwerk ausgeführt wird.  
  
 **-H** _wksta_name_  
 Der Name einer Arbeitsstation. Dieser Name ist in **sysprocesses.hostname** gespeichert und wird mithilfe von **sp_who** angezeigt. Wenn diese Option nicht angegeben ist, wird der Name des aktuellen Computers angenommen.  
  
 **-d** _db_name_  
 Gibt die USE *db_name* -Anweisung aus, wenn **osql** gestartet wird.  
  
 **-l** _time_out_  
 Gibt an, nach wie vielen Sekunden ein Timeout bei der **osql** -Anmeldung auftritt. Der Standardwert für das Timeout bei einer **osql** -Anmeldung ist acht Sekunden.  
  
 **-t** _time_out_  
 Gibt an, nach wie vielen Sekunden ein Befehl wegen eines Timeout abgebrochen wird. Wenn für *time_out* kein Wert angegeben wird, tritt für die Befehle kein Timeout ein.  
  
 **-h** _headers_  
 Gibt die Anzahl der Zeilen an, die zwischen den Spaltenüberschriften gedruckt werden. Standardmäßig werden die Überschriften für jedes Resultset der Abfrage einmal gedruckt. Mit -1 können Sie angeben, dass keine Überschriften gedruckt werden sollen. Falls -1 verwendet wird, darf zwischen dem Parameter und der Einstellung kein Leerzeichen stehen ( **-h-1** und nicht **-h -1**).  
  
 **-s** _col_separator_  
 Gibt das Spaltentrennzeichen an. Standardmäßig wird ein Leerzeichen verwendet. Um Zeichen verwenden zu können, die für das Betriebssystem eine besondere Bedeutung haben (z. B. | ; & < >), müssen Sie das Zeichen in doppelte Anführungszeichen (") setzen.  
  
 **-w** _column_width_  
 Ermöglicht dem Benutzer, die Bildschirmbreite für die Ausgabe festzulegen. Die Standardeinstellung ist 80 Zeichen. Wenn eine Ausgabezeile die maximale Bildschirmbreite erreicht hat, wird die Zeile umbrochen.  
  
 **-a** _packet_size_  
 Ermöglicht es Ihnen, Pakete mit einer anderen Größe anzufordern. Gültige Werte für *packet_size* sind 512 bis 65535. Der Standardwert **osql** ist der Serverstandard. Die gestiegene Paketgröße kann die Ausführung größerer Skripts verbessern, bei denen zwischen den GO-Befehlen eine erhebliche Menge an SQL-Anweisungen vorhanden ist. [!INCLUDE[msCoName](../includes/msconame-md.md)] -Tests geben an, dass 8192 in der Regel die schnellste Einstellung für Massenkopiervorgänge ist. Eine größere Paketgröße kann angefordert werden; **osql** verwendet jedoch standardmäßig den Standardwert des Servers, wenn die Anforderung nicht gewährt werden kann.  
  
 **-e**  
 Bewirkt, dass die Eingabe auf dem Bildschirm angezeigt wird.  
  
 **-I**  
 Aktiviert die QUOTED_IDENTIFIER-Verbindungsoption.  
  
 **-D** _data_source_name_  
 Stellt eine Verbindung mit einer ODBC-Datenquelle her, die so definiert ist, dass sie den ODBC-Treiber für [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]verwendet. Die **osql** -Verbindung verwendet die Optionen, die in der Datenquelle angegeben sind.  
  
> [!NOTE]  
>  Diese Option kann nicht mit Datenquellen verwendet werden, die für andere Treiber definiert sind.  
  
 **-c** _cmd_end_  
 Gibt das Befehlsabschlusszeichen an. Standardmäßig werden Befehle abgeschlossen und an [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] gesendet, indem Sie GO in eine eigene Zeile eingeben. Wenn Sie das Befehlsabschlusszeichen neu festlegen, dürfen Sie keine für [!INCLUDE[tsql](../includes/tsql-md.md)] reservierten Wörter oder Zeichen verwenden, die eine spezielle Bedeutung für das Betriebssystem haben, unabhängig davon, ob vor einem Wort bzw. Zeichen ein umgekehrter Schrägstrich steht.  
  
 **-q "** _query_ **"**  
 Führt eine Abfrage aus, wenn **osql** startet, beendet **osql** aber nicht, nachdem die Abfrage abgeschlossen ist. (Beachten Sie, dass die Abfrageanweisung keinen GO-Befehl enthalten darf.) Wenn Sie eine Abfrage aus einer Batchdatei ausgeben, können Sie Variablen (%Variable) oder Umgebungsvariablen (%Umgebungsvariable%) verwenden. Beispiel:  
  
```  
SET table=sys.objects  
osql -E -q "select name, object_id from %table%"  
```  
  
 Schließen Sie die Abfrage in doppelte Anführungszeichen und alle Elemente, die in die Abfrage eingebettet sind, in einfache Anführungszeichen ein.  
  
 **-Q"** _query_ **"**  
 Führt eine Abfrage aus und beendet **osql** sofort. Schließen Sie die Abfrage in doppelte Anführungszeichen und alle Elemente, die in die Abfrage eingebettet sind, in einfache Anführungszeichen ein.  
  
 **-n**  
 Entfernt die Nummerierung und das Symbol für die Eingabeaufforderung (>) aus den Eingabezeilen.  
  
 **-m** _error_level_  
 Passt die Anzeige der Fehlermeldungen an. Für jeden Fehler mit dem angegebenen oder einem höheren Schweregrad wird die Meldungsnummer, der Status und der Fehlergrad angezeigt. Für Fehler mit einem niedrigeren ist als dem angegebenen Schweregrad wird nichts angezeigt. Mit **-1** können Sie angeben, dass alle Header mit Meldungen zurückgegeben werden. Dies gilt auch für Informationsmeldungen. Falls Sie **-1** verwenden, darf kein Leerzeichen zwischen dem Parameter und der Einstellung stehen ( **-m-1** und nicht **-m -1**).  
  
 **-r** { **0**| **1**}  
 Leitet die Meldungsausgabe auf den Bildschirm um (**stderr**). Wenn Sie keinen Parameter oder **0** angeben, werden nur Fehlermeldungen umgeleitet, deren Schweregrad 11 oder höher ist. Wenn Sie **1** angeben, werden alle Meldungsausgaben (einschließlich der Ausgabe von "print") umgeleitet.  
  
 **-i** _input_file_  
 Identifiziert die Datei, die einen Batch mit SQL-Anweisungen oder gespeicherten Prozeduren enthält. Anstelle von **\<** -i **kann der Kleiner-als-Vergleichsoperator (** ) verwendet werden.  
  
 **-o** _output_file_  
 Identifiziert die Datei, die die Ausgabe von **osql** erhält. Anstelle von **>** -o **kann der Größer-als-Vergleichsoperator (** ) verwendet werden.  
  
 Falls *input_file* nicht im Unicode-Format vorliegt und **-u** nicht angegeben ist, wird *output_file* im OEM-Format gespeichert. Falls *input_file* im Unicode-Format vorliegt oder **-u** angegeben ist, wird *output_file* im Unicode-Format gespeichert.  
  
 **-p**  
 Druckt die Leistungsstatistik.  
  
 **-b**  
 Gibt an, dass **osql** beendet und ein DOS ERRORLEVEL-Wert zurückgegeben wird, wenn ein Fehler auftritt. Für die DOS ERRORLEVEL-Variable wird der Wert 1 zurückgegeben, wenn der Schweregrad der [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Fehlermeldung 11 oder größer ist. Andernfalls wird der Wert 0 zurückgegeben. [!INCLUDE[msCoName](../includes/msconame-md.md)] MS-DOS-Batchdateien können verwendet werden, um den Wert von DOS ERRORLEVEL zu testen und den Fehler entsprechend zu behandeln.  
  
 **-u**  
 Gibt an, dass *output_file* unabhängig vom Format von *input_file* im Unicode-Format gespeichert wird.  
  
 **-R**  
 Gibt an, dass der [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -ODBC-Treiber Clienteinstellungen verwendet, wenn Währungs-, Datums- oder Zeitdaten in Zeichendaten konvertiert werden.  
  
 **-O**  
 Gibt an, dass bestimmte **osql** -Funktionen deaktiviert werden, um eine Übereinstimmung mit dem Verhalten früherer **isql**-Versionen zu erzielen. Die folgenden Funktionen werden deaktiviert:  
  
-   EOF-Batchverarbeitung  
  
-   Automatisches Skalieren der Konsolenbreite  
  
-   Ausführliche Meldungen  
  
 Außerdem wird der Standardwert von DOS ERRORLEVEL auf -1 festgelegt.  
  
> [!NOTE]  
>  Die Optionen **-n**, **-O** und **-D** werden von **osql** nicht mehr unterstützt.  
  
## <a name="remarks"></a>Bemerkungen  
 Das Hilfsprogramm **osql** wird direkt vom Betriebssystem aus mit den hier aufgelisteten Optionen gestartet, wobei bei den Optionen zwischen Groß- und Kleinschreibung unterschieden wird. Nachdem **osql** gestartet wurde, akzeptiert das Hilfsprogramm SQL-Anweisungen und sendet diese interaktiv an [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Die Ergebnisse werden formatiert und auf dem Bildschirm angezeigt (**stdout**). Verwenden Sie QUIT oder EXIT zum Beenden von **osql**.  
  
 Falls Sie beim Starten von **osql** keinen Benutzernamen angeben, überprüft [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] die Umgebungsvariablen und verwendet diese, z.B. **osqluser=(** _user_ **)** oder **osqlserver=(** _server_ **)** . Wenn keine Umgebungsvariablen festgelegt sind, wird der Benutzername der Arbeitsstation verwendet. Wenn Sie keinen Server angeben, wird der Name der Arbeitsstation verwendet.  
  
 Falls weder die Option **-U** noch die Option **-P** verwendet wird, versucht [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , die Verbindung mithilfe des [!INCLUDE[msCoName](../includes/msconame-md.md)] Windows-Authentifizierungsmodus herzustellen. Die Authentifizierung basiert auf dem [!INCLUDE[msCoName](../includes/msconame-md.md)] -Windows-Konto des Benutzers, der **osql** ausführt.  
  
 Das Hilfsprogramm **osql** verwendet die ODBC-API. Das Hilfsprogramm verwendet die Standardeinstellungen des [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -ODBC-Treibers für die ISO-Verbindungsoptionen in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Weitere Informationen finden Sie im Thema zu den Effekten der ANSI-Optionen.  
  
> [!NOTE]  
>  Das Hilfsprogramm **osql** bietet keine Unterstützung für benutzerdefinierte CLR-Datentypen. Zum Verarbeiten dieser Datentypen müssen Sie das Hilfsprogramm **sqlcmd** verwenden. Weitere Informationen finden Sie unter [sqlcmd Utility](../tools/sqlcmd-utility.md).  
  
## <a name="osql-commands"></a>OSQL-Befehle  
 Zusätzlich zu den [!INCLUDE[tsql](../includes/tsql-md.md)] -Anweisungen in **osql**, sind auch die folgenden Befehle verfügbar.  
  
|Get-Help|BESCHREIBUNG|  
|-------------|-----------------|  
|GO|Führt alle Anweisungen aus, die nach dem letzten GO-Befehl eingegeben wurden.|  
|RESET|Löscht alle Anweisungen, die Sie eingegeben haben.|  
|QUIT oder EXIT( )|Beendet **osql**.|  
|STRG+C|Beendet eine Abfrage, ohne **osql** zu beenden.|  
  
> [!NOTE]  
>  Die Befehle !! und ED werden von **osql** nicht mehr unterstützt.  
  
 Die Befehlsabschlusszeichen GO (Standard), RESET EXIT, QUIT und STRG+C werden nur erkannt, wenn sie am Anfang einer Zeile unmittelbar hinter der **osql** -Eingabeaufforderung verwendet werden.  
  
 GO signalisiert sowohl das Ende eines Batches als auch die Ausführung aller zwischengespeicherten [!INCLUDE[tsql](../includes/tsql-md.md)] -Anweisungen. Sobald Sie am Ende einer Eingabezeile die EINGABETASTE drücken, werden die Anweisungen in dieser Zeile von **osql** zwischengespeichert. Wenn Sie nach dem Eingeben von GO die EINGABETASTE drücken, werden alle zurzeit zwischengespeicherten Anweisungen als Batch an [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]gesendet.  
  
 Die aktuelle Version des Hilfsprogramms **osql** arbeitet so, als würde sich am Ende jedes ausgeführten Skripts ein impliziter GO-Befehl befinden. Daher werden alle Anweisungen im Skript ausgeführt.  
  
 Beenden Sie einen Befehl, indem Sie eine Zeile eingeben, die mit einem Befehlsabschlusszeichen beginnt. Durch das Eingeben einer ganzen Zahl hinter einem Befehlsabschlusszeichen können Sie angeben, wie oft der Befehl ausgeführt werden soll. Wenn Sie diesen Befehl z. B. 100-mal ausführen möchten, geben Sie Folgendes ein:  
  
```  
SELECT x = 1  
GO 100  
```  
  
 Die Ergebnisse werden einmal am Ende der Ausführung ausgegeben. **osql** akzeptiert nicht mehr als 1.000 Zeichen pro Zeile. Umfangreiche Anweisungen sollten auf mehrere Zeilen verteilt werden.  
  
 Mithilfe der Befehlsrückruffunktionen von Windows können **osql** -Anweisungen zurückgerufen und geändert werden. Der vorhandene Abfragepuffer kann durch das Eingeben von RESET geleert werden.  
  
 Wenn gespeicherte Prozeduren ausgeführt werden, gibt **osql** eine Leerzeile zwischen den einzelnen Resultsets in einem Batch aus. Außerdem wird die Meldung "0 Zeilen betroffen" nicht angezeigt, wenn sie für die ausgeführte Anweisung nicht gilt.  
  
## <a name="using-osql-interactively"></a>Interaktives Verwenden von osql  
 Wenn Sie **osql** interaktiv verwenden möchten, geben Sie an der Eingabeaufforderung den Befehl **osql** (und alle darüber hinaus gewünschten Optionen) ein.  
  
 Sie können eine Datei einlesen, die eine Abfrage enthält (wie z.B. „Stores.qry“), die von **osql** ausgeführt werden soll, indem Sie in etwa folgenden Befehl eingeben:  
  
```  
osql -E -i stores.qry  
```  
  
 Sie können eine Datei mit einer Abfrage (z. B. Titles.qry) einlesen und die Ergebnisse in eine andere Datei umleiten, indem Sie einen Befehl eingeben, der in etwa wie folgt aussieht:  
  
```  
osql -E -i titles.qry -o titles.res  
```  
  
> [!IMPORTANT]  
>  Verwenden Sie, sofern möglich, die Option **-E**(vertrauenswürdige Verbindung).  
  
 Wenn Sie **osql** interaktiv verwenden, können Sie mithilfe von **:r**_file\_name_ eine Betriebssystemdatei in den Befehlspuffer einlesen. Hierdurch wird das in *file_name* angegebene SQL-Skript direkt als ein einziger Batch an den Server gesendet.  
  
> [!NOTE]  
>  Wenn Sie **osql** verwenden, behandelt [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] das Batchtrennzeichen GO als Syntaxfehler, falls es in einer SQL-Skriptdatei verwendet wird.  
  
## <a name="inserting-comments"></a>Einfügen von Kommentaren  
 Sie können Kommentare in eine Transact-SQL-Anweisung einschließen, die von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] osql **an** gesendet wird. Für Kommentare sind die beiden folgenden Formate zulässig: `--` und `/*...*/`.  
  
## <a name="using-exit-to-return-results-in-osql"></a>Verwenden von EXIT zum Zurückgeben von Ergebnissen in "osql"  
 Sie können das Ergebnis einer SELECT-Anweisung als Rückgabewert von **osql** verwenden. Wenn numerisch, wird die letzte Spalte der letzten Ergebniszeile in eine 4 Byte lange ganze Zahl (Long) konvertiert. MS-DOS übergibt das niedrige Byte an den übergeordneten Prozess oder an die Fehlerebene des Betriebssystems. Windows übergibt die gesamte 4 Byte lange ganze Zahl. Die Syntax ist:  
  
```  
EXIT ( < query > )  
```  
  
 Beispiel:  
  
```  
EXIT(SELECT @@ROWCOUNT)  
```  
  
 Sie können den EXIT-Parameter auch als Teil einer Batchdatei einschließen. Beispiel:  
  
```  
osql -E -Q "EXIT(SELECT COUNT(*) FROM '%1')"  
```  
  
 Das Hilfsprogramm **osql** übergibt alle zwischen den Klammern **( )** stehenden Zeichen exakt so an den Server, wie sie eingegeben wurden. Wenn eine gespeicherte Systemprozedur eine Menge auswählt und einen Wert zurückgibt, wird nur die Auswahl zurückgegeben. Eine EXIT **()** -Anweisung ohne Angabe zwischen den Klammern führt alle im Batch vorhergehenden Anweisungen aus und beendet das Hilfsprogramm dann ohne Rückgabewert.  
  
 Es gibt vier EXIT-Formate:  
  
-   EXIT  
  
> [!NOTE]  
>  Führt den Batch nicht aus, beendet das Hilfsprogramm sofort und gibt keinen Wert zurück.  
  
-   EXIT **()**  
  
> [!NOTE]  
>  Führt den Batch aus, beendet dann das Hilfsprogramm und gibt keinen Wert zurück.  
  
-   EXIT **(** _query_ **)**  
  
> [!NOTE]  
>  Führt den Batch einschließlich der Abfrage aus und beendet dann das Hilfsprogramm, nachdem die Ergebnisse der Abfrage zurückgegeben wurden.  
  
-   RAISERROR mit einem Status von 127  
  
> [!NOTE]  
>  Falls RAISERROR in einem **osql** -Skript verwendet wird, und der Status 127 ausgelöst wird, wird **osql** beendet und gibt die Meldungs-ID an den Client zurück. Beispiel:  
  
```  
RAISERROR(50001, 10, 127)  
```  
  
 Dieser Fehler bewirkt, dass das **osql** -Skript beendet und die Meldungs-ID 50001 an den Client zurückgegeben wird.  
  
 Die Rückgabewerte -1 bis -99 sind von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]reserviert; **osql** definiert die folgenden Werte:  
  
-   -100  
  
     Vor dem Auswählen des Rückgabewerts ist ein Fehler aufgetreten.  
  
-   -101  
  
     Beim Auswählen eines Rückgabewerts wurden keine Zeilen gefunden.  
  
-   -102  
  
     Beim Auswählen des Rückgabewerts ist ein Konvertierungsfehler aufgetreten.  
  
## <a name="displaying-money-and-smallmoney-data-types"></a>Anzeigen der Datentypen money und smallmoney  
 **osql** zeigt einen Wert des Datentyps **money** oder **smallmoney** mit zwei Dezimalstellen an, obwohl [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] den Wert intern mit vier Dezimalstellen speichert. Beachten Sie folgendes Beispiel:  
  
```  
SELECT CAST(CAST(10.3496 AS money) AS decimal(6, 4))  
GO  
```  
  
 Diese Anweisung gibt als Ergebnis `10.3496`zurück. Dieses Ergebnis zeigt an, dass der Wert mit allen unveränderten Dezimalstellen gespeichert wird.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Kommentar &#40;MDX&#41;](../mdx/comment-mdx.md)   
 [-- &#40;Kommentar&#41; &#40;MDX&#41;](../mdx/comment-mdx-operator-reference.md)   
 [CAST und CONVERT &#40;Transact-SQL&#41;](../t-sql/functions/cast-and-convert-transact-sql.md)   
 [RAISERROR &#40;Transact-SQL&#41;](../t-sql/language-elements/raiserror-transact-sql.md)  
  
  
