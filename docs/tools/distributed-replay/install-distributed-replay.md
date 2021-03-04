---
title: Installieren von Distributed Replay
titleSuffix: SQL Server Distributed Replay
description: 'In diesem Artikel werden die verschiedenen Möglichkeiten zum Installieren des Distributed Replays beschrieben: mit dem Installations-Assistenten, dem Eingabeaufforderungsfenster oder einer Konfigurationsdatei.'
ms.prod: sql
ms.technology: tools-other
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 1dfe3a196af914c8cbba177256b41b9da454025b
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101838085"
---
# <a name="install-distributed-replay"></a>Installieren von Distributed Replay

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Sie können Distributed Replay auf eine von drei Weisen installieren:  
  
- [Installieren von Distributed Replay über den Installations-Assistenten](#bkmk_wizard)  
  
- [Installieren von Distributed Replay von der Eingabeaufforderung](#bkmk_command_prompt)  
  
- [Installieren von Distributed Replay mithilfe einer Konfigurationsdatei](#bkmk_configuration_file)  
  
##  <a name="install-distributed-replay-from-the-installation-wizard"></a><a name="bkmk_wizard"></a> Installieren von Distributed Replay über den Installations-Assistenten  

Installieren Sie die [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay-Features mit dem [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] -Installations-Assistenten. Beachten Sie Folgendes, bevor Sie sich für ein Verzeichnis für die Installation der Funktionen entscheiden:  
  
- Sie können das Verwaltungstool auf demselben Computer wie den Distributed Replay-Controller oder auf anderen Computern installieren.  
  
- Es kann in jeder Distributed Replay-Umgebung jeweils nur einen Controller geben.  
  
- Sie können den Clientdienst auf bis zu 16 (physischen oder virtuellen) Computern installieren.  
  
- Nur eine Instanz des Clientdiensts kann auf dem Distributed Replay Controller-Computer installiert werden. Wenn die Distributed Replay-Umgebung mehr als einen Client umfasst, wird davon abgeraten, den Clientdienst auf demselben Computer wie den Controller zu installieren. Dadurch könnte sich die Gesamtgeschwindigkeit der verteilten Wiedergabe verringern.  
  
- Für Leistungstestszenarien sollten das Verwaltungstool, der Distributed Replay Controller-Dienst oder Client-Dienst auf der Zielinstanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]nicht installiert werden. Das Installieren aller dieser Features auf dem Zielserver sollte auf Funktionstests für die Anwendungskompatibilität beschränkt werden.  
  
- Nach der Installation muss der Controllerdienst, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay Controller, ausgeführt werden, bevor Sie den Distributed Replay Client-Dienst auf den Clients starten.  
  
> [!NOTE]  
>  Um die Distributed Replay-Funktionen zu entfernen oder zu ändern, verwenden Sie das Fenster **Programme und Funktionen** in der **Systemsteuerung**. Wählen Sie im **Programm deinstallieren oder ändern**[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] aus, und klicken Sie anschließend auf **Entfernen** , um den [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] -Installations-Assistenten zu öffnen. Kontrollieren Sie auf der Seite **Funktionen auswählen** die Distributed Replay-Funktionen, die Sie entfernen möchten.  
  
 **Voraussetzungen:**  
  
- Stellen Sie sicher, dass die Computer, die Sie verwenden möchten, die im Thema [Distributed Replay Requirements](../../tools/distributed-replay/distributed-replay-requirements.md)beschriebenen Anforderungen erfüllen.  
  
- Erstellen Sie zuvor die Domänenbenutzerkonten, unter denen die Controller- und Clientdienste ausgeführt werden. Diese Konten sollten keine Mitglieder der Windows-Gruppe "Administratoren" sein. Weitere Informationen finden Sie im Abschnitt "Benutzer- und Dienstkonten" im Thema [Distributed Replay Security](../../tools/distributed-replay/distributed-replay-security.md) .  
  
    > [!NOTE]  
    >  Sie können lokale Benutzerkonten verwenden, wenn Sie das Verwaltungstool, den Controllerdienst und den Clientdienst auf dem gleichen Computer ausführen.  
  
 **Installationsorte:**  
  
 Angenommen, Sie verwenden die Standardorte und eine Standardinstallation, dann befindet sich das Basisverzeichnis in "C:\Programme\Microsoft SQL Server". Innerhalb des Verzeichnisses werden die Binärdateien und Assemblys in folgenden Unterverzeichnissen installiert:  
  
- Auf einem 32-Bit-System:  
  
     [!INCLUDE[ssInstallPath](../../includes/ssinstallpath-md.md)]Tools  
  
     \- ODER -  
  
     \<Share Feature Directory>\Tools\\(alternatives freigegebenes Featureverzeichnis, das vom Benutzer angegeben wird)  
  
- Auf einem 64-Bit-System:  
  
     C:\Programme\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (x86)\130\Tools  
  
     \- ODER -  
  
     \<Share Feature Directory (x86)>\Tools\\(alternatives freigegebenes x86-Featureverzeichnis, das vom Benutzer angegeben wird)  
  
#### <a name="to-install-distributed-replay-features"></a>So installieren Sie Distributed Replay-Funktionen  
  
1.  Um die Installation einer der Distributed Replay-Funktionen zu starten, öffnen Sie den [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] -Installations-Assistenten.  
  
2.  Auf der Seite **Setupunterstützungsregeln** werden Probleme erkannt, die auftreten können, wenn die SQL Server-Setupunterstützungsdateien installiert werden. Sie müssen vor dem Fortfahren mit Setup alle Setupunterstützungsfehler korrigieren.  
  
3.  Wählen Sie auf der Seite **Product Key** ein Optionsfeld aus, um anzugeben, ob Sie eine kostenlose Edition von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]installieren oder ob Sie über eine Produktionsversion des Produkts mit einem PID-Schlüssel verfügen. Weitere Informationen finden Sie unter [Editionen und Komponenten von SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md).  
  
4.  Lesen Sie auf der Seite **Lizenzbedingungen** den Lizenzvertrag, und aktivieren Sie dann das Kontrollkästchen, um den Lizenzbestimmungen zuzustimmen. Falls Sie zur Verbesserung von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]beitragen möchten, können Sie auch die Option zur Funktionsverwendung aktivieren und Berichte an [!INCLUDE[msCoName](../../includes/msconame-md.md)]senden.  
  
5.  Klicken Sie auf der Seite **Setup-Unterstützungsdateien** auf **Installieren** , um die Setup-Unterstützungsdateien für [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]zu installieren oder zu aktualisieren.  
  
6.  Wählen Sie auf der Seite **Setuprolle** die **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Funktionsinstallation** aus, und klicken Sie dann auf **Weiter** , um mit der Seite für die **Funktionsauswahl** fortzufahren.  
  
7.  Konfigurieren Sie auf der Seite **Funktionsauswahl** , welche Funktionen Sie installieren möchten.  
  
    - Wählen Sie zum Installieren des Verwaltungstools **Verwaltungstools - Einfach** aus.  
  
    - Wählen Sie **Distributed Replay Controller** aus, um den Controllerdienst zu installieren.  
  
    - Wählen Sie **Distributed Replay Client** aus, um den Clientdienst zu installieren.  
  
     **Wichtig**: Wenn Sie den Distributed Replay-Controller konfigurieren, können Sie mindestens ein Benutzerkonto angeben, das zum Ausführen der Distributed Replay-Clientdienste verwendet wird. Die folgenden Kontotypen werden unterstützt:  
  
    - Domänenbenutzerkonto  
  
    - Vom Benutzer erstelltes lokales Benutzerkonto  
  
    - Administrator  
  
    - Virtuelles Konto und verwaltetes Dienstkonto (Managed Service Account, MSA)  
  
    - Netzwerkdienste, lokale Dienste und System  
  
     Gruppenkonten (lokales oder Domänenbenutzerkonto) und andere integrierte Konten (wie "Jeder") werden nicht akzeptiert.  
  
8.  Klicken Sie optional auf die Schaltfläche mit den Auslassungspunkten (…), um den Pfad des freigegebenen Funktionsverzeichnisses zu ändern.  
  
    1.  Auf 32-Bit-Computern ist der Standardinstallationspfad **C:\Programme\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\**  
  
    2.  Auf 64-Bit-Computern ist der Standardinstallationspfad **C:\Programme (x86)\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\** .  
  
9. Wenn Sie fertig sind, klicken Sie auf **Weiter**.  
  
10. Auf der Seite **Installationsregeln** überprüft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Setup Ihre Computerkonfiguration. Sobald der Überprüfungsprozess abgeschlossen wurde, klicken Sie auf **Weiter**.  
  
11. Auf der Seite **Erforderlicher Speicherplatz** wird der für die angegebenen Funktionen erforderliche Speicherplatz berechnet. Der erforderliche Speicherplatz wird dann mit dem verfügbaren Speicherplatz auf dem Computer verglichen.  
  
12. Geben Sie auf der Seite **Fehlerberichterstellung** die Informationen an, die Sie an [!INCLUDE[msCoName](../../includes/msconame-md.md)] senden möchten, um zur Verbesserung von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]beizutragen. Die Option für Fehlerberichte ist standardmäßig aktiviert.  
  
13. Auf der Seite **Konfigurationsregeln für die Installation** führt die Systemkonfigurationsprüfung einen weiteren Regelsatz aus, um die Konfiguration des Computers anhand der angegebenen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Funktionen zu überprüfen.  
  
14. Klicken Sie auf der Seite **Das Programm kann jetzt installiert werden** auf **Installieren**.  
  
    > [!IMPORTANT]  
    >  Nachdem Sie Distributed Replay installiert haben, müssen Sie auf dem Controller und den Clientcomputern Firewallregeln erstellen und jedem Clientcomputer Berechtigungen für den Zielserver gewähren. Weitere Informationen finden Sie unter [Ausführen der Schritte nach der Installation](../../tools/distributed-replay/complete-the-post-installation-steps.md).  
  
### <a name="net-framework-security"></a>.NET Framework-Sicherheit  
 Sie müssen über Administratorberechtigungen verfügen, um eine der Distributed Replay-Funktionen zu installieren. Nur bei einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anmeldung mit sysadmin-Berechtigungen können der sysadmin-Serverrolle des Testservers die Clientdienstkonten hinzugefügt werden. Weitere Informationen zu Sicherheitsüberlegungen für Distributed Replay finden Sie unter [Distributed Replay Security](../../tools/distributed-replay/distributed-replay-security.md).  
  
##  <a name="install-distributed-replay-from-the-command-prompt"></a><a name="bkmk_command_prompt"></a> Installieren von Distributed Replay von der Eingabeaufforderung  
 Wenn Sie eine neue Distributed Replay-Instanz mithilfe der Eingabeaufforderung installieren, können Sie angeben, welche Funktionen installiert und wie diese konfiguriert werden sollen. Die Installation an der Eingabeaufforderung unterstützt das Installieren, Reparieren, Aktualisieren und Deinstallieren der Distributed Replay Utility-Komponenten. Beim Installieren über die Eingabeaufforderung unterstützt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mithilfe des /Q-Parameters den vollständigen stillen Modus.  
  
> [!NOTE]  
>  Bei lokalen Installationen müssen Sie das Setup als Administrator ausführen. Wenn Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] von einer Remotefreigabe installieren, müssen Sie ein Domänenkonto verwenden, das Lese- und Ausführungsberechtigungen auf der Remotefreigabe hat.  
  
### <a name="installation-parameters"></a>Installationsparameter  
 Die Liste der Funktionen höchster Ebene umfasst [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]und Tools. Mit der Funktion Tools werden [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Verwaltungstools, die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Onlinedokumentation, [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]und weitere freigegebene Komponenten installiert. Geben Sie für die Installation der Distributed Replay-Komponenten die folgenden Parameter an:  
  
|Komponente|Parameter|  
|---------------|---------------|  
|Distributed Replay Controller|**DREPLAY_CTLR**|  
|Distributed Replay Client|**DREPLAY_CLT**|  
|Verwaltungstool|**Tools**|  
  
> [!IMPORTANT]  
>  Nachdem Sie Distributed Replay installiert haben, müssen Sie auf dem Controller und den Clientcomputern Firewallregeln erstellen und jedem Clientcomputer Berechtigungen für den Zielserver gewähren. Weitere Informationen finden Sie unter [Ausführen der Schritte nach der Installation](../../tools/distributed-replay/complete-the-post-installation-steps.md).  
  
 Entwickeln Sie mit den in der folgenden Tabelle aufgeführten Parametern Befehlszeilenskripts für die Installation.  
  
|Parameter|BESCHREIBUNG|Unterstützte Werte|  
|---------------|-----------------|----------------------|  
|/CTLRSVCACCOUNT<br /><br /> **Optional**|Dienstkonto für den Distributed Replay Controller-Dienst.|Überprüft Konto und Kennwort.|  
|/CTLRSVCPASSWORD<br /><br /> **Optional**|Kennwort für das Distributed Replay Controller-Dienstkonto.|Überprüft Konto und Kennwort.|  
|/CTLRSTARTUPTYPE<br /><br /> **Optional**|Starttyp für den Distributed Replay Controller-Dienst.|Automatic<br /><br /> Disabled<br /><br /> Manuell|  
|/CTLRUSERS<br /><br /> **Optional**|Geben Sie an, welche Benutzer über Berechtigungen für den Distributed Replay Controller-Dienst verfügen.|Satz von Benutzerkontozeichenfolgen, mit „ “ (Leerzeichen) als Trennzeichen.<br /><br /> **Wichtig**: Wenn Sie den Distributed Replay-Controllerdienst konfigurieren, können Sie mindestens ein Benutzerkonto angeben, das zum Ausführen der Distributed Replay-Clientdienste verwendet wird. Die folgenden Kontotypen werden unterstützt:<br /><br /> Domänenbenutzerkonto<br /><br /> Vom Benutzer erstelltes lokales Benutzerkonto<br /><br /> Administrator<br /><br /> Administrator<br /><br /> Virtuelles Konto und verwaltetes Dienstkonto (Managed Service Account, MSA)<br /><br /> Netzwerkdienste, lokale Dienste und System<br /><br /> <br /><br /> Hinweis: Gruppenkonten (lokales oder Domänenbenutzerkonto) und andere integrierte Konten (wie "Jeder") werden nicht akzeptiert.|  
|/CLTSVCACCOUNT<br /><br /> **Optional**|Dienstkonto für den Distributed Replay Client-Dienst.|Überprüft Konto und Kennwort.|  
|/CLTSVCPASSWORD<br /><br /> **Optional**|Kennwort für das Distributed Replay Client-Dienstkonto.|Überprüft Konto und Kennwort.|  
|/CLTSTARTUPTYPE<br /><br /> **Optional**|Starttyp für den Distributed Replay Client-Dienst.|Automatic<br /><br /> Disabled<br /><br /> Manuell|  
|/CLTCTLRNAME<br /><br /> **Optional**|Der Computername, mit dem der Client für den Distributed Replay Controller-Dienst kommuniziert.||  
|/CLTWORKINGDIR<br /><br /> **Optional**|Das Arbeitsverzeichnis für den Distributed Replay Client-Dienst.|Gültiger Pfad|  
|/CLTRESULTDIR<br /><br /> **Optional**|Das Ergebnisverzeichnis für den Distributed Replay Client-Dienst.|Gültiger Pfad|  
  
### <a name="sample-syntax"></a>Beispielsyntax:  
 **So installieren Sie die Distributed Replay Controller-Komponente**  
  
```  
setup /q /ACTION=Install /FEATURES=DREPLAY_CTLR /IAcceptSQLServerLicenseTerms /CTLRUSERS="domain\user1" "domain\user2" /CTLRSVCACCOUNT="domain\svcuser" /CTLRSVCPASSWORD="password" /CTLRSTARTUPTYPE=Automatic  
```  
  
 **So installieren Sie die Distributed Replay Client-Komponente**  
  
```  
setup /q /ACTION=Install /FEATURES=DREPLAY_CLT /IAcceptSQLServerLicenseTerms /CLTSVCACCOUNT="domain\svcuser" /CLTSVCPASSWORD="password" /CLTSTARTUPTYPE=Automatic /CLTCTLRNAME=ControllerMachineName /CLTWORKINGDIR="C:\WorkingDir" /CLTRESULTDIR="C:\ResultDir  
```  
  
##  <a name="install-distributed-replay-using-a-configuration-file"></a><a name="bkmk_configuration_file"></a> Installieren von Distributed Replay mithilfe einer Konfigurationsdatei  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Setup bietet die Möglichkeit, eine Konfigurationsdatei auf der Grundlage von Benutzereingaben und Systemstandards zu generieren. Wenn Sie angeben, dass die Verwaltungstools installiert werden sollen, können Sie mithilfe der Konfigurationsdatei die drei Distributed Replay-Komponenten bereitstellen (Verwaltungstool, Distributed Replay Controller und Distributed Replay Client). Unterstützt werden das Installieren, Reparieren und Deinstallieren der Distributed Replay-Komponenten.  
  
 Setup unterstützt die Verwendung der Konfigurationsdatei nur in der Befehlszeile. Die Verarbeitungsreihenfolge der Parameter während der Verwendung der Konfigurationsdatei wird im Folgenden erläutert:  
  
- Die Konfigurationsdatei überschreibt die Standards in einem Paket  
  
- Befehlszeilenwerte überschreiben die Werte in der Konfigurationsdatei  
  
 Weitere Informationen zum Verwenden einer Konfigurationsdatei finden Sie unter [Installieren von SQL Server 2016 mithilfe einer Konfigurationsdatei](../../database-engine/install-windows/install-sql-server-using-a-configuration-file.md).  
  
> [!IMPORTANT]  
>  Nachdem Sie Distributed Replay installiert haben, müssen Sie auf dem Controller und den Clientcomputern Firewallregeln erstellen und jedem Clientcomputer Berechtigungen für den Zielserver gewähren. Weitere Informationen finden Sie unter [Ausführen der Schritte nach der Installation](../../tools/distributed-replay/complete-the-post-installation-steps.md).  
  
#### <a name="to-generate-a-configuration-file"></a>So generieren Sie eine Konfigurationsdatei  
  
1.  Befolgen Sie die Anweisungen des Setup-Assistenten bis zur Seite **Installationsbereit** . Der Pfad zur Konfigurationsdatei wird auf der Seite **Installationsbereit** im Abschnitt mit dem Konfigurationsdateipfad angegeben.  
  
2.  Brechen Sie das Setupprogramm ab, ohne dabei die Installation abzuschließen, um die INI-Datei zu generieren.  
  
#### <a name="to-install-distributed-replay-using-the-configuration-file"></a>So installieren Sie Distributed Replay mithilfe der Konfigurationsdatei  
  
- Führen Sie die Installation an der Eingabeaufforderung aus, und geben Sie die Datei ConfigurationFile.ini mit dem ConfigurationFile-Parameter an.  
  
 **Beispielsyntax**  
  
 Im Folgenden finden Sie ein Beispiel zum Angeben der Konfigurationsdatei an der Eingabeaufforderung:  
  
```
Setup.exe /CTLRSVCPASSWORD="ctlrsvcpswd" /CLTSVCPASSWORD="cltsvcpswd" / ConfigurationFile=ConfigurationFile.INI\  
```

> [!NOTE]
> Sie müssen beide Kennwörter in der Befehlszeile angeben, da Sie diese nicht in der Konfigurationsdatei konfigurieren können.  

## <a name="see-also"></a>Weitere Informationen

- [Von den Editionen von SQL Server 2016 unterstützte Funktionen](~/sql-server/editions-and-supported-features-for-sql-server-2016.md)
- [SQL Server Distributed Replay](../../tools/distributed-replay/sql-server-distributed-replay.md)
- [Distributed Replay: Anforderungen](../../tools/distributed-replay/distributed-replay-requirements.md)
- [Befehlszeilenoptionen für das Verwaltungstool &#40;Distributed Replay Utility&#41;](../../tools/distributed-replay/administration-tool-command-line-options-distributed-replay-utility.md)
- [Konfigurieren von Distributed Replay](../../tools/distributed-replay/configure-distributed-replay.md)