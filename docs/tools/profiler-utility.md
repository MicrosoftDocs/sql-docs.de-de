---
title: Profiler-Hilfsprogramm
description: Mit dem Profiler-Hilfsprogramm wird das Tool SQL Server Profiler gestartet. Mit optionalen Argumenten können Sie steuern, wie die Anwendung gestartet wird.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- command prompt utilities [SQL Server], profiler90 utility
- profiler90 utility
- Profiler [SQL Server Profiler], starting
- SQL Server Profiler, starting
- starting SQL Server Profiler
ms.assetid: e91c30a9-0d29-4f84-bcb8-e8fb62afadda
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ab40ea190dab072d7415ded1f399cfa4a4b78568
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100335946"
---
# <a name="profiler-utility"></a>Profiler-Hilfsprogramm
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]
  Mit dem **Profiler** -Hilfsprogramm wird das [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] -Tool gestartet. Mit den optionalen Argumenten, die weiter unten in diesem Thema aufgeführt sind, können Sie steuern, wie die Anwendung gestartet wird.  
  
> [!NOTE]  
>  Das **Profiler** -Hilfsprogramm dient nicht zum Erstellen von Skripts für Ablaufverfolgungen. Weitere Informationen finden Sie unter [SQL Server Profiler](../tools/sql-server-profiler/sql-server-profiler.md).  
  
## <a name="syntax"></a>Syntax  
  
```  
  
profiler  
    [ /? ] |  
[  
{  
{ /U login_id [ /P password ] }  
| /E  
}  
{[ /S sql_server_name ] | [ /A analysis_services_server_name ] }  
[ /D database ]  
[ /T "template_name" ]  
[ /B { "trace_table_name" } ]  
{ [/F "filename" ] | [ /O "filename" ] }  
[ /L locale_ID ]  
[ /M "MM-DD-YY hh:mm:ss" ]  
[ /R ]  
[ /Z file_size ]  
]  
```  
  
## <a name="arguments"></a>Argumente  
 **/?**  
 Zeigt die Syntaxzusammenfassung der **Profiler** -Argumente an.  
  
 **/U** *login_id*  
 Die Benutzeranmelde-ID für die [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Authentifizierung. Bei Anmelde-IDs wird die Groß- und Kleinschreibung beachtet.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoteWinAuthentication](../includes/ssnotewinauthentication-md.md)].  
  
 **/P** *password*  
 Gibt ein benutzerdefiniertes Kennwort für die [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Authentifizierung an.  
  
 **/E**  
 Gibt an, dass die Verbindung mithilfe der Windows-Authentifizierung erfolgt und die Anmeldeinformationen des aktuellen Benutzers verwendet werden.  
  
 **/S** *sql_server_name*  
 Gibt eine [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Instanz an. Profiler stellt automatisch eine Verbindung mit dem angegebenen Server her und verwendet dabei die Authentifizierungsinformationen, die mit den Schaltern **/U** und **/P** oder mit dem Schalter **/E** angegeben wurden. Verwenden Sie **/S** *sql_server_name*\\*instance_name*, um eine Verbindung mit einer benannten [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Instanz herzustellen.  
  
 **/A** *analysis_services_server_name*  
 Gibt eine Analysis Services-Instanz an. Profiler stellt automatisch eine Verbindung mit dem angegebenen Server her und verwendet dabei die Authentifizierungsinformationen, die mit den Schaltern **/U** und **/P** oder mit dem Schalter **/E** angegeben wurden. Verwenden Sie **/A** *analysis_services_server_name\instance_name*, um eine Verbindung mit einer benannten [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Instanz herzustellen.  
  
 **/D** *database*  
 Gibt den Namen der Datenbank an, die zusammen mit dieser Verbindung verwendet werden soll. Mit dieser Option wird die Standarddatenbank für den angegebenen Benutzer ausgewählt, wenn keine Datenbank angegeben wurde.  
  
 **/B "** *trace_table_name* **"**  
 Gibt eine Ablaufverfolgungstabelle an, die beim Starten von Profiler geladen werden soll. Sie müssen die Datenbank, den Benutzer oder das Schema und die Tabelle angeben.  
  
 **/T"** *template_name* **"**  
 Gibt die Vorlage an, die geladen wird, um die Ablaufverfolgung zu konfigurieren. Der Vorlagenname muss in Anführungszeichen eingeschlossen werden. Die Vorlage muss sich entweder im Systemverzeichnis für Vorlagen oder im Benutzerverzeichnis für Vorlagen befinden. Wenn sich in beiden Verzeichnissen eine Vorlage mit diesem Namen befindet, wird die Vorlage aus dem Systemverzeichnis geladen. Wenn die Verzeichnisse keine Vorlage mit dem angegebenen Namen enthalten, wird die Standardvorlage geladen. Beachten Sie, dass die Dateierweiterung für die Vorlage (TDF) nicht als Teil des *Vorlagennamens* angegeben werden darf. Beispiel:  
  
```  
/T "standard"  
```  
  
 **/F"** *filename* **"**  
 Gibt den Pfad und den Dateinamen einer Ablaufverfolgungsdatei an, die beim Starten von Profiler geladen werden soll. Der Pfad und der Dateiname müssen in Anführungszeichen eingeschlossen werden. Diese Option kann nicht zusammen mit **/O** verwendet werden.  
  
 **/O "** *filename* **"**  
 Gibt den Pfad und den Dateinamen einer Datei an, in der die Ergebnisse der Ablaufverfolgung erfasst werden sollen. Der Pfad und der Dateiname müssen in Anführungszeichen eingeschlossen werden. Diese Option kann nicht zusammen mit **/F** verwendet werden.  
  
 **/L** *locale_ID*  
 Nicht verfügbar.  
  
 **/M "** *MM-DD-YY hh:mm:ss* **"**  
 Gibt das Datum und die Uhrzeit für die Beendigung der Ablaufverfolgung an. Der Beendigungszeitpunkt muss in Anführungszeichen eingeschlossen werden. Geben Sie den Beendigungszeitpunkt gemäß den Parametern in der folgenden Tabelle an:  
  
|Parameter|Definition|  
|---------------|----------------|  
|MM|Zweistellige Angabe des Monats|  
|DD|Zweistellige Angabe des Tages|  
|YY|Zweistellige Angabe des Jahres|  
|hh|Zweistellige Angabe der Stunde im 24-Stundenformat|  
|MM|Zweistellige Angabe der Minuten|  
|ss|Zweistellige Angabe der Sekunden|  
  
> [!NOTE]  
>  Das Format „MM-DD-YY hh:mm:ss“ kann nur verwendet werden, wenn die Option **Einstellungen für Land/Region zum Anzeigen von Datums- und Uhrzeitwerten** verwenden in [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)]aktiviert ist. Wenn diese Option nicht aktiviert ist, müssen Sie das Format "YYYY-MM-DD-hh:mm:ss" für Datum und Uhrzeit verwenden.  
  
 **/R**  
 Aktiviert das Rollover für Ablaufverfolgungsdateien.  
  
 **/Z** *file_size*  
 Gibt die Größe der Ablaufverfolgungsdatei in Megabytes (MB) an. Die Standardgröße ist 5 MB. Wenn das Rollover aktiviert ist, wird die Größe aller Rolloverdateien auf den in diesem Argument angegebenen Wert begrenzt.  
  
## <a name="remarks"></a>Bemerkungen  
 Verwenden Sie die Option **/S** zusammen mit der Option **/T** , um eine Ablaufverfolgung mit einer bestimmten Vorlage zu starten. Wenn Sie beispielsweise eine Ablaufverfolgung mithilfe der Standardvorlage im Verzeichnis MyServer\MyInstance starten möchten, geben Sie an der Eingabeaufforderung Folgendes ein:  
  
```  
profiler /S MyServer\MyInstance /T "Standard"  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Referenz zum Eingabeaufforderungs-Hilfsprogramm &#40;Datenbank-Engine&#41;](../tools/command-prompt-utility-reference-database-engine.md)  
  
  
