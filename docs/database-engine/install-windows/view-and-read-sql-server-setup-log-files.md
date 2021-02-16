---
title: Lesen und Anzeigen der Setupprotokolldateien von SQL Server | Microsoft-Dokumentation
description: In diesem Artikel werden die Protokolldateien beschrieben, die beim Setup von SQL Server erstellt werden. Die Protokolldateien werden in einem Ordner gespeichert, der mit Datum und Uhrzeit versehen wird.
ms.custom: ''
ms.date: 09/09/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- viewing logs
- displaying log files
- Setup [SQL Server], logs
- installation log files [SQL Server]
- installing SQL Server, logs
- errors [SQL Server], Setup
- logs [SQL Server], Setup
ms.assetid: 9d77af64-9084-4375-908a-d90f99535062
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 86e9ebb91c2be1c39652d939ebea3a7574ec0c6c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353805"
---
# <a name="view-and-read-sql-server-setup-log-files"></a>Lesen und Anzeigen der Setupprotokolldateien von SQL Server

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

Das SQL Server-Setup erstellt standardmäßig Protokolldateien in einem Ordner mit Datum und Zeitstempel in **\%programfiles%\Microsoft SQL Server\\*nnn*\Setup Bootstrap\Log**, wobei *nnn* Zahlen sind, die der installierten Version von SQL entsprechen. Das Namensformat für mit einem Zeitstempel versehene Protokollordner ist JJJJMMTT_hhmmss. Wenn Setup im unbeaufsichtigten Modus ausgeführt wird, werden die Protokolle in „%temp%\sqlsetup*.log“ erstellt. Alle Dateien im Protokollordner werden in der Log\*.cab-Datei im jeweiligen Protokollordner archiviert.  

   | Datei           | `Path` |
   | :------        | :----------------------------- |
   | **Summary.txt**    | \%programfiles%\Microsoft SQL Server\\*nnn*\Setup Bootstrap\Log |
   | **Summary_\<MachineName>\_Date.txt**  | \%programfiles%\Microsoft SQL Server\\*nnn*\Setup Bootstrap\Log\YYYYMMDD_hhmmss |
   | **Detail.txt** | \%programfiles%\Microsoft SQL Server\\*nnn*\Setup Bootstrap\Log\YYYYMMDD_hhmmss|
   | **Datenspeicher** | \%programfiles%\Microsoft SQL Server\\*nnn*\Setup Bootstrap\Log\YYYYMMDD_hhmmss\Datastore
   | **MSI-Protokolldateien** | \%programfiles%\Microsoft SQL Server\\*nnn*\Setup Bootstrap\Log\YYYYMMDD_hhmmss\\\<Name>.log|
   | **ConfigurationFile.ini** | \%programfiles%\Microsoft SQL Server\\*nnn*\Setup Bootstrap\Log\YYYYMMDD_hhmmss |
   | **SystemConfigurationCheck_Report.htm** | \%programfiles%\Microsoft SQL Server\\*nnn*\Setup Bootstrap\Log\YYYYMMDD_hhmmss |
   | **Für unbeaufsichtigte Installationen** | %temp%\sqlsetup*.log |


 ![Screenshot, der zeigt, wo im Ordner „Setup Bootstrap“ sich die Datei „ConfigurationFiles.ini“ befindet](media/view-and-read-sql-server-setup-log-files/setup-bootstrap-example.png)

 >[!NOTE]
 > Die Zahlen im Pfad *nnn* entsprechen der Version von SQL, die installiert wird. In der obigen Abbildung wurde SQL 2017 installiert, also ist der Ordner gleich „140“. Für SQL 2016 ist der Ordner gleich „130“, und für SQL 2014 ist der Ordner gleich „120“.
  
 SQL Server-Setup schließt drei grundlegende Phasen ab: 
  
1.  Überprüfung globaler Regeln: Die grundlegenden Systemanforderungen werden überprüft.
2.  Komponentenupdate: Es wird geprüft, ob für die Medien, die installiert werden, Updates verfügbar sind.
3.  Vom Benutzer angeforderte Aktion: Ermöglicht es dem Benutzer, Features auszuwählen und anzupassen.
  

Dieser Workflow erstellt ein einzelnes Zusammenfassungsprotokoll und entweder ein einzelnes Detailprotokoll für eine Basisinstallation von SQL Server oder zwei Detailprotokolle, wenn ein Update (z.B. ein Servicepack) zusammen mit der Basisinstallation installiert wird. 
  
Darüber hinaus gibt es Datenspeicherdateien, die eine Momentaufnahme vom Zustand aller Konfigurationsobjekte enthalten, die vom Setupprozess nachverfolgt werden, und bei der Behebung von Konfigurationsfehlern nützlich sind. XML-Dumpdateien werden für jede Ausführungsphase erstellt und werden im Unterordner für Datenspeicherprotokolle unter dem mit Zeitstempel verwalteten Protokollordner gespeichert. 

In den folgenden Abschnitten werden die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Setupprotokolldateien beschrieben.  
  
## <a name="summarytxt-file"></a>Datei „Summary.txt“ 
  
### <a name="overview"></a>Übersicht  
 In dieser Datei werden die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Komponenten, die beim Setup entdeckt wurden, die Betriebssystemumgebung, die Parameterwerte für die Befehlszeile, wenn diese angegeben wurden, und der allgemeine Status jeder MSI-/MSP-Datei, die ausgeführt wurde, angegeben.
  
 Das Protokoll ist in folgende Abschnitte unterteilt:
  
-   Eine allgemeine Zusammenfassung der Ausführung  
-   Eigenschaften und Konfiguration des Computers, auf dem das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Setup ausgeführt wurde  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Produktfeatures  
-   Die Beschreibung der Installationsversion und der Installationspaketeigenschaften
-   Laufzeiteingabeeinstellungen, die während der Installation angegeben werden  
-   Speicherort der Konfigurationsdatei  
-   Details zu den Ausführungsergebnissen  
-   Globale Regeln  
-   Für das Installationsszenario spezifische Regeln  
-   Regeln, bei denen Fehler aufgetreten sind  
-   Speicherort der Regelberichtdatei


  >[!NOTE]
  > Beachten Sie, dass beim Patchen mehrere Unterordner vorliegen können (einer für jede Instanz, die gepatcht wird, und einer für freigegebene Features), die ähnliche Dateien enthalten (d. h. %programfiles%\MicrosoftSQL Server\130\Setup Bootstrap\Log\<YYYYMMDD_HHMM>\MSSQLSERVER). 
  
### <a name="location"></a>Standort  
 Die Datei „summary.txt“ befindet sich in „% ProgramFiles%\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\*nnn*\Setup Bootstrap\Log\\“.
  
 Um Fehler in der Textdatei zu finden, die die Zusammenfassung enthält, durchsuchen Sie die Datei nach den Schlüsselwörtern "error" oder "failed".
  
## <a name="summary_machinename_yyyymmdd_hhmmsstxt-file"></a>Datei „Summary_\<MachineName>_JJJJMMTT_SSMMss.txt“
  
### <a name="overview"></a>Übersicht  
 Die summary_engine-Basisdatei ähnelt der Zusammenfassungsdatei und wird während des Hauptworkflows generiert.
  
### <a name="location"></a>Standort  
 Die Datei „Summary_\<MachineName>_JJJJMMTT_SSMMss.txt“ befindet sich unter „%ProgramFiles%"\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\*nnn*\Setup Bootstrap\Log\\<JJJJMMTT_SSMM>\\“.
  
  
## <a name="detailtxt-file"></a>Datei „Detail.txt“
  
### <a name="overview"></a>Übersicht
 Die Datei Detail.txt wird für den Hauptworkflow wie Installation oder Upgrade generiert und liefert Einzelheiten zur Ausführung. Die Protokolle in der Datei werden entsprechend dem Zeitpunkt erstellt, zu dem jede Aktion für die Installation aufgerufen wurde. In der Textdatei sind sowohl die Reihenfolge, in der die Aktionen ausgeführt wurden, als auch deren Abhängigkeiten festgehalten.  
  
### <a name="location"></a>Standort  
 Die Datei „Detail.txt“ befindet sich in „%ProgramFiles%\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\*nnn*\Setup Bootstrap\Log\\<JJJJMMTT_SSMM>\Detail.txt“.  
  
 Wenn während des Setupvorgangs ein Fehler auftritt, wird die Ausnahme oder der Fehler am Ende dieser Datei protokolliert. Um die Fehler in dieser Datei zu finden, prüfen Sie daher zunächst das Ende der Datei, und suchen Sie dann nach dem Schlüsselwort „Fehler“ oder „Ausnahme“.
    
## <a name="msi-log-files"></a>MSI-Protokolldateien
  
### <a name="overview"></a>Übersicht  
 Die MSI-Protokolldateien liefern Details zum Installationspaketprozess. Sie werden durch MSIEXEC während der Installation des angegebenen Pakets generiert.  
  
 Typen von MSI-Protokolldateien:
  
-   \<Feature>_\<Architecture>\_\<Interaction>.log   
-   \<Feature>_\<Architecture>\_\<Language>\_\<Interaction>.log   
-   \<Feature>_\<Architecture>\_\<Interaction>\_\<workflow>.log  
  
### <a name="location"></a>Standort  
 Die MSI-Protokolldateien befinden sich unter %Programme%\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\*nnn*\Setup Bootstrap\Log\\<YYYYMMDD_HHMM>\\<Name\>.log.  
  
 Am Ende der Datei befindet sich eine Zusammenfassung der Ausführung, die die Erfolgs- und Fehlerstatus sowie Eigenschaften enthält. Um den Fehler in der MSI-Datei zu finden, suchen Sie nach „value 3“, und prüfen Sie den Text davor und danach.  
  
## <a name="configurationfileini-file"></a>Datei „ConfigurationFile.ini“
  
### <a name="overview"></a>Übersicht  
 Die Konfigurationsdatei enthält die Eingabeeinstellungen, die während der Installation angegeben werden. Sie kann verwendet werden, um eine Installation neu zu starten, ohne die Einstellungen manuell eingeben zu müssen. Kennwörter für die Konten, die PID und einige Parameter werden jedoch nicht in der Konfigurationsdatei gespeichert. Diese Einstellungen können der Datei entweder hinzugefügt oder über die Befehlszeile oder die Setupbenutzeroberfläche angegeben werden. Weitere Informationen finden Sie unter [Installieren von SQL Server 2016 mithilfe einer Konfigurationsdatei](./install-sql-server-using-a-configuration-file.md).  
  
### <a name="location"></a>Standort  
 Die Datei „ConfigurationFile.ini“ befindet sich unter „%ProgramFiles%\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\*nnn*\Setup Bootstrap\Log\\<JJJJMMTT_SSMM>\\“.  
  
## <a name="systemconfigurationcheck_reporthtm-file"></a>Datei „SystemConfigurationCheck_Report.htm“
  
### <a name="overview"></a>Übersicht  
 Im Bericht zur Systemkonfigurationsprüfung sind eine kurze Beschreibung für jede ausgeführte Regel sowie der Ausführungsstatus enthalten.
  
### <a name="location"></a>Standort  
Die Datei „SystemConfigurationCheck_Report.htm“ befindet sich unter „%ProgramFiles%\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\*nnn*\Setup Bootstrap\Log\\<JJJJMMTT_SSMM>\\“.

[!INCLUDE[get-help-options](../../includes/paragraph-content/get-help-options.md)]
  
## <a name="see-also"></a>Weitere Informationen  
 [Installieren von SQL Server 2019](../../database-engine/install-windows/install-sql-server.md)