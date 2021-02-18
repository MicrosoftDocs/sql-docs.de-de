---
title: Option „replay“ im Verwaltungstool
titleSuffix: SQL Server Distributed Replay
description: Dieser Artikel beschreibt die Wiedergabe-Befehlszeilenoption und -syntax des Distributed Replay-Verwaltungstools von SQL Server, das die Ereigniswiedergabephase einleitet.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: d7bce6a5-d414-488d-a3cd-50c1c62019c4
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: fa147de01aaa2e70615aff9e0e4ec248aa4769c7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354485"
---
# <a name="replay-option-distributed-replay-administration-tool"></a>Option Wiedergabe (Verwaltungstool Distributed Replay)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Das Verwaltungstool [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay, **DReplay.exe**, ist ein Befehlszeilentool, das Sie für die Kommunikation mit dem Distributed Replay-Controller verwenden können. In diesem Thema werden die **replay** -Befehlszeilenoption und die entsprechende Syntax beschrieben.  
  
 Die **replay** -Option initiiert die Ereigniswiedergabephase, in der der Controller Wiedergabedaten an die angegebenen Clients weiterleitet, die verteilte Wiedergabe startet und die Clients synchronisiert. Optional kann jeder Client, der an der Wiedergabe teilnimmt, die Wiedergabeaktivität aufzeichnen und eine Ergebnisdatei der Ablaufverfolgung lokal speichern.  
  
 ![Artikellinksymbol](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") Weitere Informationen zu den Syntaxkonventionen für das Verwaltungstool finden Sie unter [Transact-SQL-Syntaxkonventionen &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  
  
## <a name="syntax"></a>Syntax  
  
```  
  
dreplay replay [-m controller] -d controller_working_dir [-o]  
    [-s target_server] -w clients [-c config_file]  
    [-f status_interval]  
```  
  
#### <a name="parameters"></a>Parameter  
 **-m** _Controller_  
 Gibt den Computernamen des Controllers an. Sie können mit "`localhost`" oder "`.`" auf den lokalen Computer verweisen.  
  
 Wenn der **-m** -Parameter nicht angegeben ist, wird der lokale Computer verwendet.  
  
 **-d** _controller_working_dir_  
 Gibt das Verzeichnis auf dem Controller an, in dem die Zwischendatei gespeichert wird. Der **-d** -Parameter ist erforderlich.  
  
 Es gelten die folgenden Anforderungen:  
  
-   Das Verzeichnis muss sich auf dem Controller befinden.  
  
-   Sie müssen den vollständigen Pfad angeben, der mit einem Laufwerkbuchstaben beginnen muss (z. B. `c:\WorkingDir`).  
  
-   Der Pfad darf nicht mit einem umgekehrten Schrägstrich ("`\`") enden.  
  
-   UNC-Pfade werden nicht unterstützt.  
  
 **-o**  
 Zeichnet die Wiedergabeaktivität der Clients auf und speichert sie in einer Ergebnisdatei der Ablaufverfolgung unter dem Pfad, der in der Clientkonfigurationsdatei `<ResultDirectory>` vom `DReplayClient.xml`-Element angegeben wird.  
  
 Wenn der **-o** -Parameter nicht angegeben wird, wird die Ergebnisdatei der Ablaufverfolgung nicht generiert. Die Konsolenausgabe gibt am Ende der Wiedergabe Zusammenfassungsinformationen zurück, es sind jedoch keine weiteren Wiedergabestatistiken verfügbar.  
  
 **-s** _target_server_  
 Gibt die Zielinstanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] an, für die die verteilte Arbeitsauslastung wiedergegeben werden soll. Sie müssen diesen Parameter im folgenden Format angeben: **server_name[\instance name]** .  
  
 Sie können den Zielserver nicht mit "`localhost`" oder "`.`" angeben.  
  
 Der **-s** -Parameter ist nicht erforderlich, wenn das `<Server>` -Element im `<ReplayOptions>` -Abschnitt der Wiedergabekonfigurationsdatei `DReplay.exe.replay.config`angegeben wird.  
  
 Wenn der **-s** -Parameter verwendet wird, wird das `<Server>` -Element im `<ReplayOptions>` -Abschnitt der Wiedergabekonfigurationsdatei ignoriert.  
  
 **-w** _clients_  
 Dieser erforderliche Parameter ist eine durch Trennzeichen getrennte Liste (ohne Leerzeichen), die die Computernamen von Clients angibt, die an der verteilten Wiedergabe teilnehmen sollten. IP-Adressen sind nicht zulässig. Beachten Sie, dass die Clients bereits beim Controller registriert sein müssen.  
  
> [!NOTE]  
>  Jeder Client wird beim Starten des Clientdiensts bei dem Controller registriert, der in der Clientkonfigurationsdatei angegeben ist.  
  
 **-c** _config_file_  
 Der vollständige Pfad der Wiedergabekonfigurationsdatei. Mit ihm wird der Speicherort angegeben, wenn die Datei an einem anderen Speicherort gespeichert wird.  
  
 Der **-c** -Parameter ist nicht erforderlich, wenn Sie die Standardwerte der Wiedergabekonfigurationsdatei `DReplay.exe.replay.config`verwenden möchten.  
  
 **-f** _status_interval_  
 Gibt die Häufigkeit (in Sekunden) für die Anzeige des Status an.  
  
 Wenn **-f** nicht angegeben wird, ist das Standardintervall 30 Sekunden.  
  
## <a name="examples"></a>Beispiele  
 In diesem Beispiel wird ein Großteil des Verhaltens der verteilten Wiedergabe von der geänderten Wiedergabekonfigurationsdatei `DReplay.exe.replay.config`abgeleitet.  
  
-   Der **-m** -Parameter gibt an, dass ein Computer mit dem Namen `controller1` als Controller fungiert. Der Computername muss angegeben werden, wenn der Controllerdienst auf einem anderen Computer ausgeführt wird.  
  
-   Der **-d** -Parameter gibt den Speicherort der Zwischendatei auf dem Controller im Verzeichnis an ( `c:\WorkingDir`).  
  
-   Der **-o** -Parameter legt fest, dass jeder angegebene Client die Wiedergabeaktivität aufzeichnet und in einer Ergebnisdatei der Ablaufverfolgung speichert. Hinweis: Mit dem `<ResultTrace>`-Element in der Konfigurationsdatei kann angegeben werden, ob Zeilenanzahl und Resultset aufgezeichnet werden.  
  
-   Der **-w** -Parameter gibt an, dass die Computer `client1` bis `client4` als Clients an der verteilten Wiedergabe teilnehmen.  
  
-   Der **-c** -Parameter wird verwendet, um auf die geänderte Konfigurationsdatei `DReplay.exe.replay.config`zu zeigen.  
  
-   Der **-s** -Parameter ist nicht erforderlich, da das `<Server>` -Element im `<ReplayOptions>` -Element der Wiedergabekonfigurationsdatei `DReplay.exe.replay.config`angegeben wird.  
  
 Die Ereigniswiedergabephase wird mit der folgenden Syntax initiiert, wenn das Verwaltungstool und der Controller nicht auf demselben Computer ausgeführt werden:  
  
```  
dreplay replay -m controller1 -d c:\WorkingDir -o -w client1,client2,client3,client4 -c c:\DReplay.exe.replay.config  
```  
  
 Um einen synchronen Sequenzierungsmodus anzugeben, wird das `<SequencingMode>` -Element der Datei `DReplay.exe.replay.config` auf den Wert `synchronization`festgelegt. Der `<ResultTrace>` -Abschnitt der Wiedergabekonfigurationsdatei wurde geändert, um anzugeben, dass die Zeilenanzahl aufgezeichnet wird. Diese Änderungen werden im folgenden XML-Beispiel gezeigt:  
  
```  
<?xml version='1.0'?>  
<Options>  
    <ReplayOptions>  
        <Server>server_name\replay_target_instance</Server>  
        <SequencingMode>synchronization</SequencingMode>  
        <ConnectTimeScale></ConnectTimeScale>  
        <ThinkTimeScale></ThinkTimeScale>  
        <HealthmonInterval>60</HealthmonInterval>  
        <QueryTimeout>3600</QueryTimeout>  
        <ThreadsPerClient></ThreadsPerClient>  
    </ReplayOptions>  
    <OutputOptions>  
        <ResultTrace>  
            <RecordRowCount>Yes</RecordRowCount>  
            <RecordResultSet>No</RecordResultSet>  
        </ResultTrace>  
    </OutputOptions>  
</Options>  
```  
  
 Um einen Belastungssequenzierungsmodus anzugeben, wird das `<SequencingMode>` -Element der Datei `DReplay.exe.replay.config` auf den Wert `stress`festgelegt. Das `<ConnectTimeScale>` -Element und das `<ThinkTimeScale>` -Element werden auf den Wert `50` festgelegt (um 50 Prozent anzugeben). Weitere Informationen zu Verbindungszeit und Reaktionszeit finden Sie unter [Konfigurieren von Distributed Replay](../../tools/distributed-replay/configure-distributed-replay.md). Diese Änderungen werden im folgenden XML-Beispiel gezeigt:  
  
```  
<?xml version='1.0'?>  
<Options>  
    <ReplayOptions>  
        <Server>server_name\replay_target_instance_name</Server>  
        <SequencingMode>stress</SequencingMode>  
        <ConnectTimeScale>50</ConnectTimeScale>  
        <ThinkTimeScale>50</ThinkTimeScale>  
        <HealthmonInterval>60</HealthmonInterval>  
        <QueryTimeout>3600</QueryTimeout>  
        <ThreadsPerClient></ThreadsPerClient>  
    </ReplayOptions>  
    <OutputOptions>  
        <ResultTrace>  
            <RecordRowCount>Yes</RecordRowCount>  
            <RecordResultSet>No</RecordResultSet>  
        </ResultTrace>  
    </OutputOptions>  
</Options>  
```  
  
## <a name="permissions"></a>Berechtigungen  
 Sie müssen das Verwaltungstool als interaktiver Benutzer mit einem lokalen Benutzerkonto oder Domänenbenutzerkonto ausführen. Um ein lokales Benutzerkonto zu verwenden, müssen das Verwaltungstool und der Controller auf demselben Computer ausgeführt werden.  
  
 Weitere Informationen finden Sie unter [Distributed Replay Security](../../tools/distributed-replay/distributed-replay-security.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Wiedergeben von Ablaufverfolgungsdaten](../../tools/distributed-replay/replay-trace-data.md)   
 [Überprüfen der Wiedergabeergebnisse](../../tools/distributed-replay/review-the-replay-results.md)   
 [SQL Server Distributed Replay](../../tools/distributed-replay/sql-server-distributed-replay.md)   
 [Konfigurieren von Distributed Replay](../../tools/distributed-replay/configure-distributed-replay.md)   
 [SQL Server Distributed Replay Forum](https://social.technet.microsoft.com/Forums/sl/sqldru/)   
 [Verwenden von Distributed Replay für den Auslastungstest von SQL Server: Teil 2](/archive/blogs/msdn/mspfe/using-distributed-replay-to-load-test-your-sql-serverpart-2)   
 [Verwenden von Distributed Replay für den Auslastungstest von SQL Server – Teil 1](/archive/blogs/batuhanyildiz/using-distributed-replay-to-load-test-your-sql-serverpart-1)  
  
