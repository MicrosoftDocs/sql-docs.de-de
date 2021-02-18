---
title: ssbdiagnose-Hilfsprogramm (Service Broker)
description: Das Hilfsprogramm „ssbdiagnose“ meldet Probleme bei Service Broker-Konversationen oder der Konfiguration von Service Broker-Diensten.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- Service Broker, runtime reports
- Service Broker, command prompt utilities
- troubleshooting [Service Broker], conversations
- troubleshooting [Service Broker], configurations
- command prompt utilities [Service Broker]
- Service Broker, troubleshooting
- Service Broker, configuration reports
- Service Broker, tools
- troubleshooting [Service Broker], runtime
- conversations [Service Broker], troubleshooting
- troubleshooting [Service Broker], ssbdiagnose utility
- tools [Service Broker], ssbdiagnose
- Service Broker, ssbdiagnose utility
- ssbdiagnose
ms.assetid: 0c1636e8-a3db-438e-be4c-1ea40d1f4877
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: a5001821d5a396f65ee739341424a2eaae3dad78
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100335897"
---
# <a name="ssbdiagnose-utility-service-broker"></a>ssbdiagnose-Hilfsprogramm (Service Broker)

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

Das Hilfsprogramm **ssbdiagnose** meldet Probleme in [!INCLUDE[ssSB](../../includes/sssb-md.md)] -Konversationen oder der Konfiguration von [!INCLUDE[ssSB](../../includes/sssb-md.md)] -Diensten. Konfigurationsüberprüfungen können entweder für zwei Dienste oder für einen einzelnen Dienst ausgeführt werden. Probleme werden entweder im Eingabeaufforderungsfenster als für den Benutzer lesbarer Text oder als formatierte XML, die in eine Datei oder ein anderes Programm umgeleitet werden kann, gemeldet.

## <a name="syntax"></a>Syntax  
  
```  
  
ssbdiagnose   
[ [ -XML ]  
    [ -LEVEL { ERROR | WARNING | INFO } ]  
  [-IGNORE error_id ] [ ...n]  
    [ <baseconnectionoptions> ]  
  { <configurationreport> | <runtimereport> }  
]  
| -?  
  
<configurationreport> ::=  
    CONFIGURATION  
  { [ FROM SERVICE service_name  
      [ <fromconnectionoptions> ]  
      [ MIRROR <mirrorconnectionoptions> ]  
    ]  
    [ TO SERVICE service_name[, broker_id ]  
      [ <toconnectionoptions> ]  
      [ MIRROR <mirrorconnectionoptions> ]  
    ]  
  }  
    ON CONTRACT contract_name  
  [ ENCRYPTION { ON | OFF | ANONYMOUS } ]  
  
<runtime_report> ::=  
    RUNTIME  
    [-SHOWEVENTS ]  
        [ -NEW  
         [ -ID { conversation_handle  
                | conversation_group_id  
                 | conversation_id  
                  }  
        ] [ ...n]  
        ]  
    [ -TIMEOUT timeout_interval ]  
    [ <runtimeconnectionoptions> ]  
  
<baseconnectionoptions> ::=  
  <connectionoptions>  
  
<fromconnectionoptions> ::=  
  <connectionoptions>  
  
<toconnectionoptions> ::=  
  <connectionoptions>  
  
<mirrorconnectionoptions> ::=  
  <connectionoptions>  
  
<runtimeconnectionoptions> ::=  
  [ CONNECT TO <connectionoptions> ] [ ...n]  
  
<connectionoptions> ::=  
    [ -E | { -U login_id [ -P password ] } ]  
  [ -S server_name[\instance_name] ]  
  [ -d database_name ]  
  [ -l login_timeout ]  
  
```  
  
## <a name="command-line-options"></a>Befehlszeilenoptionen  
 **-XML**  
 Gibt an, dass die Ausgabe von **ssbdiagnose** als formatierte XML generiert wird. Dieses kann in eine Datei oder in eine andere Anwendung umgeleitet werden. Wenn **-XML** nicht angegeben ist, wird die Ausgabe von **ssbdiagnose** als für den Benutzer lesbarer Text formatiert.  
  
 **-LEVEL** { **ERROR** | **WARNING** | **INFO**}  
 Gibt die Ebene der Meldungen an, die gemeldet werden sollen.  
  
 **ERROR**: Nur Fehler werden gemeldet.  
  
 **WARNING**: Fehler und Warnungen werden gemeldet.  
  
 **INFO**: Fehler, Warnungen und Informationsmeldungen werden gemeldet.  
  
 Die Standardeinstellung ist **WARNING**.  
  
 **-IGNORE** _error_id_  
 Gibt an, dass Fehler oder Meldungen mit der angegebenen *Fehler-ID* in Berichten nicht eingeschlossen werden. Sie können **-IGNORE** mehrmals angeben, um mehrere Meldungs-IDs zu unterdrücken.  
  
 **\<baseconnectionoptions>**  
 Gibt die grundlegenden Verbindungsinformationen an, die von **ssbdiagnose** verwendet werden, wenn eine bestimmte Klausel keine Verbindungsoptionen enthält. Die in einer bestimmten Klausel angegebenen Verbindungsinformationen überschreiben die Informationen von **baseconnectionoption** . Dieser Vorgang wird für jeden Parameter separat ausgeführt. Beispiel: Wenn sowohl **-S** als auch **-d** in **baseconnectionoptions** angegeben sind und nur **-d** in **toconnectionoptions** angegeben ist, verwendet **ssbdiagnose** „-S“ aus **baseconnectionoptions** und „-d“ aus **toconnectionoptions**.  
  
 **CONFIGURATION**  
 Fordert einen Bericht über Konfigurationsfehler für ein Paar von [!INCLUDE[ssSB](../../includes/sssb-md.md)] -Diensten oder für einen einzelnen Dienst an.  
  
 **FROM SERVICE** _service_name_  
 Gibt den Dienst an, der Konversationen initiiert.  
  
 **\<fromconnectionoptions>**  
 Gibt die Informationen an, die erforderlich sind, um eine Verbindung mit der Datenbank herzustellen, die den Initiatordienst enthält. Wenn **fromconnectionoptions** nicht angegeben ist, verwendet **ssbdiagnose** die Verbindungsinformationen aus **baseconnectionoptions** , um eine Verbindung mit der Initiatordatenbank herzustellen. Wenn **fromconnectionoptions** angegeben ist, muss darin die Datenbank angegeben sein, die den Initiatordienst enthält. Wenn **fromconnectionoptions** nicht angegeben wird, muss **baseconnectionoptions** die Initiatordatenbank angeben.  
  
 **TO SERVICE** _service_name_[, *broker_id* ]  
 Gibt den Dienst an, der das Ziel für die Konversationen darstellt.  
  
 *Dienstname*: Gibt den Namen des Zieldiensts an.  
  
 *Broker-ID*: Gibt die [!INCLUDE[ssSB](../../includes/sssb-md.md)] -ID für die Zieldatenbank an. Die *Broker-ID* ist eine GUID. Sie können die folgende Abfrage in der Zieldatenbank ausführen, um diese zu finden:  
  
```  
SELECT service_broker_guid  
FROM sys.databases  
WHERE database_id = DB_ID();  
```  
  
 **\<toconnectionoptions>**  
 Gibt die Informationen an, die erforderlich sind, um eine Verbindung mit der Datenbank herzustellen, die den Zieldienst enthält. Wenn **toconnectionoptions** nicht angegeben ist, verwendet **ssbdiagnose** die Verbindungsinformationen aus **baseconnectionoptions** , um eine Verbindung mit der Zieldatenbank herzustellen.  
  
 **MIRROR**  
 Gibt an, dass der dazugehörige [!INCLUDE[ssSB](../../includes/sssb-md.md)] -Dienst in einer gespiegelten Datenbank gehostet wird. **ssbdiagnose** überprüft, dass die Route zum Dienst eine gespiegelte Route ist, bei der MIRROR_ADDRESS für CREATE ROUTE angegeben wurde.  
  
 **\<mirrorconnectionoptions>**  
 Gibt die Informationen an, die erforderlich sind, um eine Verbindung mit der Spiegeldatenbank herzustellen. Wenn **mirrorconnectionoptions** nicht angegeben ist, verwendet **ssbdiagnose** die Verbindungsinformationen aus **baseconnectionoptions** , um eine Verbindung mit der Spiegeldatenbank herzustellen.  
  
 **ON CONTRACT** _contract_name_  
 Fordert an, dass **ssbdiagnose** nur Konfigurationen überprüft, die den angegebenen Vertrag verwenden. Wenn ON CONTRACT nicht angegeben ist, berichtet **ssbdiagnose** über den Vertrag mit dem Namen DEFAULT.  
  
 **ENCRYPTION** { **ON** | **OFF** | **ANONYMOUS** }  
 Fordert an, dass überprüft wird, ob der Dialog für die angegebene Ebene der Verschlüsselung ordnungsgemäß konfiguriert ist:  
  
 **ON**: Standardeinstellung. Vollständige Dialogsicherheit wird konfiguriert. Auf beiden Seiten des Dialogs wurden Zertifikate bereitgestellt, eine Remotedienstbindung ist vorhanden, und in der GRANT SEND-Anweisung für den Zieldienst wurde der Initiatorbenutzer angegeben.  
  
 **OFF**: Es wird keine Dialogsicherheit konfiguriert. Es wurden keine Zertifikate bereitgestellt, keine Remotedienstbindung erstellt, und in der GRANT SEND-Anweisung für den Initiatordienst wurde die **public** -Rolle angegeben.  
  
 **ANONYMOUS**: Anonyme Dialogsicherheit wird konfiguriert. Ein Zertifikat wurde bereitgestellt, die Remotedienstbindung wurde in der ANONYMOUS-Klausel angegeben, und in der GRANT SEND-Anweisung für den Zieldienst wurde die **public** -Rolle angegeben.  
  
 **RUNTIME**  
 Fordert einen Bericht über Probleme an, die Laufzeitfehler in einer [!INCLUDE[ssSB](../../includes/sssb-md.md)] -Konversation verursachen. Wenn weder **-NEW** noch **-ID** angegeben ist, überwacht **ssbdiagnose** alle Konversationen in allen in den Verbindungsoptionen angegebenen Datenbanken. Wenn **-NEW** oder **-ID** angegeben ist, erstellt **ssbdiagnose** eine Liste der in den Parametern angegebenen IDs.  
  
 Solange **ssbdiagnose** ausgeführt wird, werden alle [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] -Ereignisse aufgezeichnet, die Laufzeitfehler angeben. Es werden die Ereignisse, die für die angegebenen IDs auftreten, sowie Ereignisse auf Systemebene aufgezeichnet. Wenn Laufzeitfehler auftreten, führt **ssbdiagnose** einen Konfigurationsbericht über die zugeordnete Konfiguration aus.  
  
 Standardmäßig werden keine Laufzeitfehler, sondern nur die Ergebnisse der Konfigurationsanalyse in den Ausgabebericht aufgenommen. Verwenden Sie **-SHOWEVENTS** , um die Laufzeitfehler in den Bericht aufzunehmen.  
  
 **-SHOWEVENTS**  
 Gibt an, dass **ssbdiagnose** in einem RUNTIME-Bericht [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] -Ereignisse melden soll. Nur Ereignisse, die als Fehlerbedingungen erachtet werden, werden gemeldet. Standardmäßig überwacht **ssbdiagnose** Fehlerereignisse nur, meldet sie jedoch in der Ausgabe nicht.  
  
 **-NEW**  
 Fordert die Laufzeitüberwachung der ersten Konversation an, die beginnt, nachdem **ssbdiagnose** gestartet wurde.  
  
 **-ID**  
 Fordert die Laufzeitüberwachung der angegebenen Konversationselemente an. Sie können **-ID** mehrmals angeben.  
  
 Wenn Sie ein Konversationshandle angeben, werden nur Ereignisse für den zugeordneten Konversationsendpunkt gemeldet. Wenn Sie eine Konversations-ID angeben, werden alle Ereignisse für diese Konversation sowie für deren Endpunkte für den Initiator und das Ziel gemeldet. Wenn Sie eine Konversationsgruppen-ID angeben, werden alle Ereignisse für alle Konversationen und Endpunkte in der Konversationsgruppe gemeldet.  
  
 *conversation_handle*  
 Ein eindeutiger Bezeichner, der einen Konversationsendpunkt in einer Anwendung identifiziert. Konversationshandles sind für einen Endpunkt einer Konversation eindeutig, d. h., die Endpunkte für den Initiator und das Ziel weisen unterschiedliche Konversationshandles auf.  
  
 Konversationshandles werden durch den *\@dialog_handle*-Parameter der **BEGIN DIALOG**-Anweisung und die Spalte **conversation_handle** im Resultset einer **RECEIVE**-Anweisung an Anwendungen zurückgegeben.  
  
 Konversationshandles werden in der Spalte **conversation_handle** der Katalogsichten **sys.transmission_queue** und **sys.conversation_endpoints** gemeldet.  
  
 *conversation_group_id*  
 Der eindeutige Bezeichner, der eine Konversationsgruppe identifiziert.  
  
 Konversationsgruppen-IDs werden durch den *\@conversation_group_id*-Parameter der **GET CONVERSATION GROUP**-Anweisung und die Spalte **conversation_group_id** im Resultset einer **RECEIVE**-Anweisung an Anwendungen zurückgegeben.  
  
 Konversationsgruppen-IDs werden in der Spalte **conversation_group_id** der Katalogsichten **sys.conversation_groups** und **sys.conversation_endpoints** gemeldet.  
  
 *conversation_id*  
 Der eindeutige Bezeichner, der eine Konversation identifiziert. Konversations-IDs sind für die Endpunkte für den Initiator und das Ziel einer Konversation identisch.  
  
 Konversations-IDs werden in der Spalte **conversation_id** der Katalogsicht und **sys.conversation_endpoints** gemeldet.  
  
 **-TIMEOUT** _timeout_interval_  
 Gibt die Anzahl der Sekunden für die Ausführung eines **RUNTIME** -Berichts an. Wenn **-TIMEOUT** nicht angegeben ist, wird der Laufzeitbericht ohne zeitliche Begrenzung ausgeführt. **-TIMEOUT** wird nur für **RUNTIME** -Berichte und nicht für **CONFIGURATION** -Berichte verwendet. Mit STRG+C können Sie **ssbdiagnose** beenden, wenn **-TIMEOUT** nicht angegeben wurde, oder Sie können einen Laufzeitbericht vor Ablauf des Timeoutintervalls beenden. **-** Das *Timeoutintervall* muss eine Zahl zwischen 1 und 2.147.483.647 sein.  
  
 **\<runtimeconnectionoptions>**  
 Gibt die Verbindungsinformationen für die Datenbanken an, in denen die den überwachten Konversationselementen zugeordneten Dienste enthalten sind. Wenn alle Dienste in der gleichen Datenbank enthalten sind, müssen Sie nur eine **CONNECT TO** -Klausel angeben. Wenn sich die Dienste in unterschiedlichen Datenbanken befinden, müssen Sie für jede dieser Datenbanken eine **CONNECT TO** -Klausel angeben. Wenn **runtimeconnectionoptions** nicht angegeben ist, verwendet **ssbdiagnose** die Verbindungsinformationen aus **baseconnectionoptions**.  
  
 **-E**  
 Öffnen Sie mithilfe der Windows-Authentifizierung eine Verbindung mit einer Instanz von [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Verwenden Sie dazu das aktuelle Windows-Konto als Anmelde-ID. Die Anmeldung muss Mitglied der festen Serverrolle **sysadmin** sein.  
  
 Die Option "-E" ignoriert die Benutzer- und Kennworteinstellungen der Umgebungsvariablen SQLCMDUSER und SQLCMDPASSWORD.  
  
 Wenn weder **-E** noch **-U** angegeben ist, verwendet **ssbdiagnose** den Wert aus der Umgebungsvariablen SQLCMDUSER. Wenn SQLCMDUSER auch nicht festgelegt ist, verwendet **ssbdiagnose** die Windows-Authentifizierung.  
  
 Wird die Option **-E** zusammen mit der Option **-U** oder der Option **-P** verwendet, wird eine Fehlermeldung generiert.  
  
 **-U** _login_id_  
 Öffnen Sie mit der angegebenen Anmelde-ID eine Verbindung mit der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Authentifizierung. Die Anmeldung muss Mitglied der festen Serverrolle **sysadmin** sein.  
  
 Wenn weder **-E** noch **-U** angegeben ist, verwendet **ssbdiagnose** den Wert aus der Umgebungsvariablen SQLCMDUSER. Wenn SQLCMDUSER auch nicht festgelegt ist, versucht **ssbdiagnose** , eine Verbindung unter Verwendung des Windows-Authentifizierungsmodus herzustellen. Dabei wird das Windows-Konto des Benutzers verwendet, der **ssbdiagnose** ausführt.  
  
 Wird die Option **-U** zusammen mit der Option **-E** verwendet, wird eine Fehlermeldung generiert. Werden nach der Option **-U** mehrere Argumente angegeben, wird eine Fehlermeldung generiert und das Programm beendet.  
  
 **-P** _password_  
 Gibt das Kennwort für die Anmelde-ID **-U** an. Bei Kennwörtern wird nach Groß- und Kleinschreibung unterschieden. Wenn die Option **-U** verwendet wird, nicht aber die Option **-P** , verwendet **ssbdiagnose** den Wert aus der Umgebungsvariablen SQLCMDPASSWORD. Wenn SQLCMDPASSWORD auch nicht festgelegt ist, fordert **ssbdiagnose** den Benutzer zur Eingabe eines Kennworts auf.  
  
> [!IMPORTANT]  
>  Wenn Sie einen SET SQLCMDPASSWORD-Befehl eingeben, kann das Kennwort von jeder Person gelesen werden, die auf den Bildschirm schauen kann.  
  
 Wenn die Option **-P** ohne Kennwort angegeben ist, verwendet **ssbdiagnose** das Standardkennwort (NULL).  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)] Weitere Informationen finden Sie unter [Sichere Kennwörter](../../relational-databases/security/strong-passwords.md).  
  
 Die Aufforderung zur Eingabe des Kennworts wird folgendermaßen an der Konsole ausgegeben: `Password:`  
  
 Die Benutzereingabe bleibt verborgen, d. h. es erfolgt keine Anzeige, und der Cursor bleibt an der Anfangsposition.  
  
 Wird die Option **-P** zusammen mit der Option **-E** verwendet, wird eine Fehlermeldung generiert.  
  
 Wird nach der Option **-P** mehr als ein Argument angegeben, wird eine Fehlermeldung generiert.  
  
 **-S** _server_name_[\\*instance_name*]  
 Gibt die Instanz von [!INCLUDE[ssDE](../../includes/ssde-md.md)] an, die die zu analysierenden [!INCLUDE[ssSB](../../includes/sssb-md.md)] -Dienste enthält.  
  
 Geben Sie *Servername* an, um eine Verbindung mit der Standardinstanz von [!INCLUDE[ssDE](../../includes/ssde-md.md)] auf diesem Server herzustellen. Geben Sie _server\_name_ **\\** _instance\_name_ an, um eine Verbindung mit der benannten Instanz von [!INCLUDE[ssDE](../../includes/ssde-md.md)] auf diesem Server herzustellen. Wenn **-S** nicht angegeben ist, verwendet **ssbdiagnose** den Wert der Umgebungsvariablen SQLCMDSERVER. Wenn SQLCMDSERVER auch nicht festgelegt ist, stellt **ssbdiagnose** eine Verbindung mit der Standardinstanz von [!INCLUDE[ssDE](../../includes/ssde-md.md)] auf dem lokalen Computer her.  
  
 **-d** _database_name_  
 Gibt die Datenbank an, die die zu analysierenden [!INCLUDE[ssSB](../../includes/sssb-md.md)] -Dienste enthält. Wenn die Datenbank nicht vorhanden ist, wird eine Fehlermeldung generiert. Wenn **-d** nicht angegeben ist, wird standardmäßig die Datenbank verwendet, die in der Standarddatenbank-Eigenschaft Ihrer Anmeldung angegeben ist.  
  
 **-l** _login_timeout_  
 Gibt die Anzahl von Sekunden an, die verstreichen, ehe für den Versuch einer Verbindung mit einem Server ein Timeout eintritt. Wenn **-l** nicht angegeben ist, verwendet **ssbdiagnose** den für die Umgebungsvariable SQLCMDLOGINTIMEOUT festgelegten Wert. Wenn SQLCMDLOGINTIMEOUT auch nicht festgelegt ist, beträgt der Standardwert für das Timeout dreißig Sekunden. Der Timeoutwert für den Anmeldungszeitraum muss eine Zahl zwischen 0 und 65534 sein. Wenn der angegebene Wert kein numerischer Wert ist oder außerhalb dieses Bereichs liegt, generiert **ssbdiagnose** eine Fehlermeldung. Mit dem Wert 0 wird eine unbegrenzte Wartezeit festgelegt.  
  
 **-?**  
 Zeigt die Hilfe zur Befehlszeile an.  
  
## <a name="remarks"></a>Bemerkungen  
 Verwenden Sie **ssbdiagnose** , um folgende Aufgaben auszuführen:  
  
-   Bestätigen, dass in einer neu konfigurierten [!INCLUDE[ssSB](../../includes/sssb-md.md)] -Anwendung keine Konfigurationsfehler vorhanden sind.  
  
-   Bestätigen, dass keine Konfigurationsfehler vorhanden sind, nachdem Sie die Konfiguration einer vorhandenen [!INCLUDE[ssSB](../../includes/sssb-md.md)] -Anwendung geändert haben.  
  
-   Bestätigen, dass keine Konfigurationsfehler vorhanden sind, nachdem eine [!INCLUDE[ssSB](../../includes/sssb-md.md)] -Datenbank getrennt und dann an eine neue Instanz von [!INCLUDE[ssDE](../../includes/ssde-md.md)]angefügt wurde.  
  
-   Überprüfen, ob Konfigurationsfehler vorhanden sind, wenn Nachrichten nicht erfolgreich zwischen Diensten übertragen werden.  
  
-   Abrufen eines Berichts über Fehler, die in einer Gruppe von [!INCLUDE[ssSB](../../includes/sssb-md.md)] -Konversationselementen auftreten.  
  
## <a name="configuration-reporting"></a>Konfigurationsberichte  
 Führen Sie einen **ssbdiagnose** -Konfigurationsbericht aus, für den die gleichen Optionen wie für die Konversation verwendet werden, um die von einer Konversation verwendete Konfiguration ordnungsgemäß zu analysieren. Wenn Sie für **ssbdiagnose** weniger Optionen angeben, als für die Konversation verwendet werden, meldet **ssbdiagnose** möglicherweise keine Bedingungen, die für die Konversation erforderlich sind. Wenn Sie für **ssbdiagnose** mehr Optionen angeben, werden möglicherweise Elemente gemeldet, die für die Konversation nicht erforderlich sind. So kann z. B. eine Konversation zwischen zwei Diensten in der gleichen Datenbank mit ENCPRYPTION OFF ausgeführt werden. Wenn Sie **ssbdiagnose** ausführen, um die Konfiguration zwischen den beiden Diensten zu überprüfen, dabei jedoch die Standardeinstellung ENCRYPTION ON verwenden, meldet **ssbdiagnose** , dass in der Datenbank ein Hauptschlüssel fehlt. Ein Hauptschlüssel ist für die Konversation nicht erforderlich.  
  
 Der **ssbdiagnose** -Konfigurationsbericht analysiert bei jeder Ausführung nur einen einzelnen [!INCLUDE[ssSB](../../includes/sssb-md.md)] -Dienst oder ein einzelnes Paar von Diensten. Wenn mehrere Paare von [!INCLUDE[ssSB](../../includes/sssb-md.md)] -Diensten im Bericht berücksichtigt werden sollen, erstellen Sie eine CMD-Befehlsdatei, die **ssbdiagnose** mehrmals aufruft.  
  
## <a name="runtime-reporting"></a>Laufzeitberichte  
 Wenn -RUNTIME angegeben ist, durchsucht **ssbdiagnose** alle in **runtimeconnectionoptions** und **baseconnectionoptions** angegebenen Datenbanken, um eine Liste der [!INCLUDE[ssSB](../../includes/sssb-md.md)] -IDs zu erstellen. Wie die vollständige Liste von IDs aussieht, hängt davon ab, was für - NEW und - ID angegeben wird:  
  
-   Wenn weder **-NEW** noch **-ID** angegeben ist, enthält die Liste alle Konversationen für alle in den Verbindungsoptionen angegebenen Datenbanken.  
  
-   Wenn **-NEW** angegeben ist, schließt **ssbdiagnose** die Elemente für die erste Konversation ein, die gestartet wird, nachdem **ssbdiagnose** gestartet wurde. Hierzu zählen die Konversations-ID und die Konversationshandles für die Konversationsendpunkte für den Initiator und das Ziel.  
  
-   Wenn **-ID** mit einem Konversationshandle angegeben ist, wird nur dieses Handle in die Liste aufgenommen.  
  
-   Wenn **-ID** mit einer Konversations-ID angegeben ist, werden die Konversations-ID und die Handles für beide Konversationsendpunkte der Liste hinzugefügt.  
  
-   Wenn **-ID** mit einer Konversationsgruppen-ID angegeben ist, werden alle in der Gruppe enthaltenen Konversations-IDs und Konversationshandles der Liste hinzugefügt.  
  
 Die Liste enthält keine Elemente aus Datenbanken, die in den Verbindungsoptionen nicht angegeben wurden. Beispiel: Angenommen, Sie verwenden **-ID** , um eine Konversations-ID anzugeben, geben jedoch nur für die Initiatordatenbank eine **runtimeconnectionoptions** -Klausel an, nicht für die Zieldatenbank. **ssbdiagnose** schließt das Zielkonversationshandle in diesem Fall nicht in die Liste der IDs ein, sondern nur die Konversations-ID und das Initiatorkonversationshandle.  
  
 **ssbdiagnose** überwacht die [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] -Ereignisse in den von **runtimeconnectionoptions** und **baseconnectionoptions** abgedeckten Datenbanken. Das Hilfsprogramm sucht [!INCLUDE[ssSB](../../includes/sssb-md.md)] -Ereignisse, die angeben, dass von einer oder mehreren der [!INCLUDE[ssSB](../../includes/sssb-md.md)] -IDs in der Laufzeitliste ein Fehler gefunden wurde. **ssbdiagnose** sucht auch nach [!INCLUDE[ssSB](../../includes/sssb-md.md)] -Fehlerereignissen auf Systemebene, die nicht explizit mit einer bestimmten Konversationsgruppe verknüpft sind.  
  
 Wenn **ssbdiagnose** Konversationsfehler feststellt, versucht das Hilfsprogramm, die Ursache der Ereignisse zurückzugeben, indem zusätzlich ein Konfigurationsbericht ausgeführt wird. **ssbdiagnose** ermittelt anhand der Metadaten in den Datenbanken die von der Konversation verwendeten Instanzen, [!INCLUDE[ssSB](../../includes/sssb-md.md)] -IDs, Datenbanken, Dienste und Verträge. Anschließend wird ein Konfigurationsbericht mit allen verfügbaren Informationen ausgeführt.  
  
 Standardmäßig meldet **ssbdiagnose** keine Fehlerereignisse. Es werden nur die während der Konfigurationsüberprüfung gefundenen eigentlichen Probleme gemeldet. Dadurch wird die Menge gemeldeter Informationen minimiert, und Sie können sich besser auf die eigentlichen Konfigurationsprobleme konzentrieren. Sie können **-SHOWEVENTS** angeben, um die von **ssbdiagnose** gefundenen Fehlerereignisse anzuzeigen.  
  
## <a name="issues-reported-by-ssbdiagnose"></a>Von "ssbdiagnose" gemeldete Probleme  
 **ssbdiagnose** meldet drei Klassen von Problemen. In der XML-Ausgabedatei wird jede Klasse von Problemen als gesonderter Typ des Issue-Elements gemeldet. Folgende drei Typen von Problemen werden von **ssbdiagnose** gemeldet:  
  
 **Diagnosis**  
 Meldet ein Konfigurationsproblem. Hierunter fallen Probleme, die entweder bei der Ausführung eines **CONFIGURATION** -Berichts oder in der Konfigurationsphase eines **RUNTIME** -Berichts gefunden werden. **ssbdiagnose** meldet jedes Konfigurationsproblem nur einmal.  
  
 **Event**  
 Meldet ein [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] -Ereignis, das auf ein Problem bei einer überwachten Konversation während eines **RUNTIME** -Berichts hindeutet. **ssbdiagnose** meldet jedes generierte Ereignis. Ereignisse können mehrmals gemeldet werden, wenn das Problem in mehreren Konversationen auftritt.  
  
 **Problem**  
 Meldet ein Problem, das **ssbdiagnose** daran hindert, eine Konfigurationsanalyse abzuschließen oder Konversationen zu überwachen.  
  
## <a name="sqlcmd-environment-variables"></a>Umgebungsvariablen von "sqlcmd"  
 Das Hilfsprogramm **ssbdiagnose** unterstützt die Umgebungsvariablen SQLCMDSERVER, SQLCMDUSER, SQLCMDPASSWORD und SQLCMDLOGINTIMOUT, die auch von dem Hilfsprogramm **sqlcmd** verwendet werden. Sie können zum Festlegen der Umgebungsvariablen entweder den SET-Befehl an der Eingabeaufforderung oder den **setvar** -Befehl in [!INCLUDE[tsql](../../includes/tsql-md.md)] -Skripts verwenden, die Sie mit **sqlcmd** ausführen. Weitere Informationen zum Verwenden von **setvar** in **sqlcmd** finden Sie unter [Verwenden von sqlcmd mit Skriptvariablen](../../ssms/scripting/sqlcmd-use-with-scripting-variables.md).  
  
## <a name="permissions"></a>Berechtigungen  
 In jeder **connectionoptions** -Klausel muss der mit **-E** oder **-U** angegebene Anmeldename Mitglied der festen Serverrolle **sysadmin** in der in **-S** angegebenen Instanz sein.  
  
## <a name="examples"></a>Beispiele  
 Dieser Abschnitt enthält Beispiele für die Verwendung von **ssbdiagnose** an einer Eingabeaufforderung.  
  
### <a name="a-checking-the-configuration-of-two-services-in-the-same-database"></a>A. Überprüfen der Konfiguration von zwei Diensten in der gleichen Datenbank  
 Im folgenden Beispiel wird gezeigt, wie ein Konfigurationsbericht angefordert wird, wenn folgende Bedingungen erfüllt sind:  
  
-   Der Initiator- und der Zieldienst befinden sich in der gleichen Datenbank.  
  
-   Die Datenbank befindet sich in der Standardinstanz von [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
-   Die Instanz befindet sich auf dem Computer, auf dem **ssbdiagnose** ausgeführt wird.  
  
 Das Hilfsprogramm **ssbdiagnose** meldet die Konfiguration, die den DEFAULT-Vertrag verwendet, weil ON CONTRACT nicht angegeben ist.  
  
```  
ssbdiagnose -E -d MyDatabase CONFIGURATION FROM SERVICE /test/initiator TO SERVICE /test/target  
```  
  
### <a name="b-checking-the-configuration-of-two-services-on-separate-computers-that-use-one-login"></a>B. Überprüfen der Konfiguration von zwei Diensten auf unterschiedlichen Computern, die einen Anmeldenamen verwenden  
 Das folgende Beispiel zeigt, wie ein Konfigurationsbericht angefordert wird, wenn sich der Initiator- und der Zieldienst auf unterschiedlichen Computern befinden, der Zugriff auf diese jedoch mit dem gleichen Anmeldenamen für die Windows-Authentifizierung erfolgt.  
  
```  
ssbdiagnose -E CONFIGURATION FROM SERVICE /text/initiator -S InitiatorComputer -d InitiatorDatabase TO SERVICE /test/target -S TargetComputer -d TargetDatabase ON CONTRACT TestContract  
```  
  
### <a name="c-checking-the-configuration-of-two-services-on-separate-computers-that-use-separate-logins"></a>C. Überprüfen der Konfiguration von zwei Diensten auf unterschiedlichen Computern, die unterschiedliche Anmeldenamen verwenden  
 Das folgende Beispiel zeigt, wie ein Konfigurationsbericht angefordert wird, wenn sich der Initiator- und der Zieldienst auf unterschiedlichen Computern befinden und für jede Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ein separater Anmeldename für die [!INCLUDE[ssDE](../../includes/ssde-md.md)]-Authentifizierung erforderlich ist.  
  
```  
ssbdiagnose CONFIGURATION FROM SERVICE /text/initiator   
-S InitiatorComputer -U InitiatorLogin -p !wEx23Dvb   
-d InitiatorDatabase TO SERVICE /test/target -S TargetComputer   
-U TargetLogin -p ER!49jiy -d TargetDatabase ON CONTRACT TestContract  
```  
  
### <a name="d-checking-mirrored-service-configurations-on-separate-computers-with-anonymous-encryption"></a>D: Überprüfen der Konfigurationen gespiegelter Dienste auf separaten Computern mit anonymer Verschlüsselung  
 Das folgende Beispiel zeigt, wie ein Konfigurationsbericht angefordert wird, wenn sich der Initiator- und der Zieldienst auf unterschiedlichen Computern befinden und der Initiator auf eine benannte Instanz gespiegelt ist. Mit dem Bericht wird auch überprüft, ob die Dienste für die anonyme Verschlüsselung konfiguriert sind.  
  
```  
ssbdiagnose -E CONFIGURATION FROM SERVICE /text/initiator   
-S InitiatorComputer -d InitiatorDatabase MIRROR   
-S MirrorComputer/MirrorInstance TO SERVICE /test/target   
-S TargetComputer -d TargetDatabase ON CONTRACT TestContract ENCRYPTION ANONYMOUS  
```  
  
### <a name="e-checking-the-configuration-of-two-contracts"></a>E. Überprüfen der Konfiguration von zwei Verträgen  
 Im folgenden Beispiel wird gezeigt, wie eine Befehlsdatei erstellt wird, mit der Konfigurationsberichte angefordert werden, wenn folgende Bedingungen erfüllt sind:  
  
-   Der Initiator- und der Zieldienst befinden sich in der gleichen Datenbank.  
  
-   Die Datenbank befindet sich in der Standardinstanz von [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
-   Die Instanz befindet sich auf dem Computer, auf dem **ssbdiagnose** ausgeführt wird.  
  
 Bei jeder Ausführung von **ssbdiagnose** wird die Konfiguration für einen anderen Vertrag zwischen den gleichen Diensten gemeldet.  
  
```  
ssbdiagnose -E -d MyDatabase CONFIGURATION FROM SERVICE   
/test/initiator TO SERVICE /test/target ON CONTRACT PayRaiseContract  
ssbdiagnose -E -d MyDatabase CONFIGURATION FROM SERVICE /test/initiator   
TO SERVICE /test/target ON CONTRACT PromotionContract  
```  
  
### <a name="f-monitor-the-status-of-a-specific-conversation-on-the-local-computer-with-a-time-out"></a>F. Überwachen des Status einer bestimmten Konversation auf dem lokalen Computer mit einem Timeout  
 Das folgende Beispiel zeigt, wie eine bestimmte Konversation überwacht wird, wenn sich der Initiator- und der Zieldienst in der gleichen Datenbank in der Standardinstanz auf dem gleichen Computer befinden, auf dem auch **ssbdiagnose** ausgeführt wird. Das Timeoutintervall ist auf 20 Sekunden festgelegt.  
  
```  
ssbdiagnose -E -d TestDatabase RUNTIME -ID D68D77A9-B1CF-41BF-A5CE-279ABCAB140D -TIMEOUT 20  
```  
  
### <a name="g-monitor-the-status-of-a-conversation-that-spans-two-computers"></a>G. Überwachen des Status einer Konversation, die zwei Computer umfasst  
 Das folgende Beispiel zeigt, wie eine bestimmte Konversation überwacht wird, wenn sich der Initiator- und der Zieldienst auf unterschiedlichen Computern befinden.  
  
```  
ssbdiagnose RUNTIME -ID D68D77A9-B1CF-41BF-A5CE-279ABCAB140D   
-TIMEOUT 10 CONNECT TO -E -S InitiatorComputer/InitiatorInstance   
-d InitiatorDatabase CONNECT TO -E -S TargetComputer/TargetInstance   
-d TargetDatabase  
```  
  
### <a name="h-monitor-the-status-of-a-conversation-in-two-databases-in-the-same-instance"></a>H. Überwachen des Status einer Konversation in zwei Datenbanken in der gleichen Instanz  
 Das folgende Beispiel zeigt, wie eine bestimmte Konversation überwacht wird, wenn sich der Initiator- und der Zieldienst in unterschiedlichen Datenbanken in der gleichen Instanz von [!INCLUDE[ssDE](../../includes/ssde-md.md)]befinden. In diesem Beispiel wird **baseconnectionoptions** verwendet, um die Instanz und die Anmeldeinformationen anzugeben, und es werden zwei CONNECT TO-Klauseln zum Angeben der Datenbanken verwendet. -SHOWEVENTS wird angegeben, damit alle Laufzeitereignisse in der Berichtsausgabe enthalten sind.  
  
```  
ssbdiagnose -E -S TestComputer/DevTestInstance RUNTIME -SHOWEVENTS   
-ID 5094d4a7-e38c-4c37-da37-1d58b1cb8455 -TIMEOUT 10 CONNECT TO   
-d InitiatorDatabase CONNECT TO -d TargetDatabase  
```  
  
### <a name="i-monitor-the-status-of-two-conversations-between-two-databases"></a>I. Überwachen des Status von zwei Konversationen zwischen zwei Datenbanken  
 Das folgende Beispiel zeigt, wie zwei Konversationen überwacht wenn, bei denen sich der Initiator- und der Zieldienst in unterschiedlichen Datenbanken in der gleichen Instanz von [!INCLUDE[ssDE](../../includes/ssde-md.md)]befinden. In diesem Beispiel wird **baseconnectionoptions** verwendet, um die Instanz und die Anmeldeinformationen anzugeben, und es werden zwei CONNECT TO-Klauseln zum Angeben der Datenbanken verwendet.  
  
```  
ssbdiagnose -E -S TestComputer/DevTestInstance RUNTIME   
-ID 5094d4a7-e38c-4c37-da37-1d58b1cb8455   
-ID 9b293be9-226b-4e22-e169-1d2c2c15be86 -TIMEOUT 10 CONNECT TO   
-d InitiatorDatabase CONNECT TO -d TargetDatabase  
```  
  
### <a name="j-monitor-the-status-of-all-conversations-between-two-databases"></a>J. Überwachen des Status aller Konversationen zwischen zwei Datenbanken  
 Das folgende Beispiel zeigt, wie alle Konversationen zwischen zwei Datenbanken in der gleichen Instanz von [!INCLUDE[ssDE](../../includes/ssde-md.md)]überwacht werden In diesem Beispiel wird **baseconnectionoptions** verwendet, um die Instanz und die Anmeldeinformationen anzugeben, und es werden zwei CONNECT TO-Klauseln zum Angeben der Datenbanken verwendet.  
  
```  
ssbdiagnose -E -S TestComputer/DevTestInstance RUNTIME   
-TIMEOUT 10 CONNECT TO -d InitiatorDatabase CONNECT TO   
-d TargetDatabase  
```  
  
### <a name="k-ignore-specific-errors"></a>K. Ignorieren bestimmter Fehler  
 Das folgende Beispiel zeigt, wie bekannte Fehler (303 und 304) in der aktuellen Konfiguration der Aktivierung in einem Testsystem ignoriert werden.  
  
```  
ssbdiagnose -IGNORE 303 -IGNORE 304 -E -d TestDatabase   
CONFIGURATION FROM SERVICE /test/initiator TO SERVICE /test/target   
ON CONTRACT TextContract  
```  
  
### <a name="l-redirecting-ssbdiagnose-xml-output"></a>L. Umleiten der XML-Ausgabe von ssbdiagnose  
 Das folgende Beispiel zeigt, wie die Anforderung erstellt wird, dass die Ausgabe von **ssbdiagnose** als XML-Datei generiert werden soll, die in eine Datei umgeleitet wird. Die Datei „TestDiag.xml“ kann dann von einer Anwendung geöffnet werden, um **ssbdiagnose** -XML-Dateien zu analysieren oder zu melden. Die Anzeige ist auch in einem allgemeinen XML-Editor wie XML Editor möglich.  
  
```  
ssbdiagnose -XML -E -d MyDatabase CONFIGURATION FROM SERVICE   
/test/initiator TO SERVICE /test/target > c:\MyDiagnostics\TestDiag.xml  
```  
  
### <a name="m-using-an-environment-variable"></a>M. Verwenden einer Umgebungsvariablen  
 Im folgenden Beispiel wird zunächst die SQLCMDSERVER-Umgebungsvariable für den Servernamen festgelegt, und anschließend wird **ssbdiagnose** ohne Angabe von **-S** ausgeführt.  
  
```  
SET SQLCMDSERVER=MyComputer  
ssbdiagnose -XML -E -d MyDatabase CONFIGURATION FROM SERVICE   
/test/initiator TO SERVICE /test/target  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQL Server Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md)   
 [BEGIN DIALOG CONVERSATION &#40;Transact-SQL&#41;](../../t-sql/statements/begin-dialog-conversation-transact-sql.md)   
 [CREATE BROKER PRIORITY &#40;Transact-SQL&#41;](../../t-sql/statements/create-broker-priority-transact-sql.md)   
 [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)   
 [CREATE CONTRACT &#40;Transact-SQL&#41;](../../t-sql/statements/create-contract-transact-sql.md)   
 [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/create-endpoint-transact-sql.md)   
 [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-master-key-transact-sql.md)   
 [CREATE MESSAGE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/create-message-type-transact-sql.md)   
 [CREATE QUEUE &#40;Transact-SQL&#41;](../../t-sql/statements/create-queue-transact-sql.md)   
 [CREATE REMOTE SERVICE BINDING &#40;Transact-SQL&#41;](../../t-sql/statements/create-remote-service-binding-transact-sql.md)   
 [CREATE ROUTE &#40;Transact-SQL&#41;](../../t-sql/statements/create-route-transact-sql.md)   
 [CREATE SERVICE &#40;Transact-SQL&#41;](../../t-sql/statements/create-service-transact-sql.md)   
 [RECEIVE &#40;Transact-SQL&#41;](../../t-sql/statements/receive-transact-sql.md)   
 [sys.transmission_queue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-transmission-queue-transact-sql.md)   
 [sys.conversation_endpoints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-conversation-endpoints-transact-sql.md)   
 [sys.conversation_groups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-conversation-groups-transact-sql.md)  
  
