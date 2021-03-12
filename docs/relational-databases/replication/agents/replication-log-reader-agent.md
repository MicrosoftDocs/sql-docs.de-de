---
title: Replikationsprotokolllese-Agents | Microsoft-Dokumentation
description: Der Replikationsprotokolllese-Agent überwacht SQL Server-Datenbanken, die für die Transaktionsreplikation konfiguriert sind, und kopiert Transaktionen in die Verteilungsdatenbank.
ms.custom: ''
ms.date: 10/29/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- Log Reader Agent, executables
- Log Reader Agent, parameter reference
- agents [SQL Server replication], Log Reader Agent
- command prompt [SQL Server replication]
ms.assetid: 5487b645-d99b-454c-8bd2-aff470709a0e
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 58e9ed82d9518d423001342fa288e2c7aba1f5bf
ms.sourcegitcommit: 98acedd435aecfda1b3c4c23d3f0c3c1a12682a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2021
ms.locfileid: "102532342"
---
# <a name="replication-log-reader-agent"></a>Replikationsprotokolllese-Agent
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  Der Replikationsprotokolllese-Agent ist eine ausführbare Datei, die das Transaktionsprotokoll jeder für die Transaktionsreplikation konfigurierten Datenbank überwacht und die für die Replikation markierten Transaktionen aus dem Transaktionsprotokoll in die Verteilungsdatenbank kopiert.  
  
> [!NOTE]  
>  Parameter können in beliebiger Reihenfolge angegeben werden. Wenn keine optionalen Parameter angegeben werden, werden vordefinierte Werte auf Grundlage des Standardagentprofils verwendet.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
logread [-?]   
-Publisher server_name[\instance_name]   
-PublisherDB publisher_database   
[-Continuous]  
[-DefinitionFile def_path_and_file_name]  
[-Distributor server_name[\instance_name]]  
[-DistributorLogin distributor_login]  
[-DistributorPassword distributor_password]  
[-DistributorSecurityMode [0|1]]  
[-EncryptionLevel [0|1|2]]  
[-ExtendedEventConfigFile configuration_path_and_file_name]  
[-HistoryVerboseLevel [0|1|2]]  
[-KeepAliveMessageInterval keep_alive_message_interval_seconds]  
[-LoginTimeOut login_time_out_seconds]  
[-LogScanThreshold scan_threshold]  
[-MaxCmdsInTran number_of_commands]  
[-MessageInterval message_interval]
[-MultiSubnetFailover [0|1]]
[-Output output_path_and_file_name]  
[-OutputVerboseLevel [0|1|2|3|4]]  
[-PacketSize packet_size]  
[-PollingInterval polling_interval]  
[-ProfileName profile_name]   
[-PublisherFailoverPartner server_name[\instance_name] ]  
[-PublisherSecurityMode [0|1]]  
[-PublisherLogin publisher_login]  
[-PublisherPassword publisher_password]   
[-QueryTimeOut query_time_out_seconds]  
[-ReadBatchSize number_of_transactions]   
[-ReadBatchThreshold read_batch_threshold]  
[-RecoverFromDataErrors]  
```  
  
## <a name="arguments"></a>Argumente  
 **-?**  
 Zeigt Informationen zur Nutzung an.  
  
 **-Publisher** _server_name_[ **\\** _instance_name_]  
 Der Name des Verlegers. Geben Sie den *server_name* für die Standardinstanz von [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] auf diesem Server an. Geben Sie _server_name_ **\\** _instance_name_ für eine benannte Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] auf diesem Server an.  
  
 **-PublisherDB** _publisher_database_  
 Der Name der Verlegerdatenbank.  
  
 **-Continuous**  
 Gibt an, ob der Agent fortlaufend versucht, replizierte Transaktionen abzurufen. Wenn dieses Argument angegeben ist, ruft der Agent replizierte Transaktionen in festgelegten Abrufintervallen aus der Quelle ab, selbst wenn keine ausstehenden Transaktionen vorhanden sind.  
  
 **-DefinitionFile** _def_path_and_file_name_  
 Der Pfad der Agentdefinitionsdatei. Eine Agentdefinitionsdatei enthält Befehlszeilenargumente für den Agent. Der Inhalt der Datei wird als ausführbare Datei analysiert. Verwenden Sie doppelte Anführungszeichen ("), um Argumentwerte anzugeben, die beliebige Zeichen enthalten.  
  
 **-Distributor** _server_name_[ **\\** _instance_name_]  
 Der Name des Verteilers. Geben Sie *server_name* für die Standardinstanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] auf diesem Server an. Geben Sie _server_name_ **\\** _instance_name_ für eine benannte Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] auf diesem Server an.  
  
 **-DistributorLogin** _distributor_login_  
 Der Anmeldename des Verteilers.  
  
 **-DistributorPassword** _distributor_password_  
 Das Verteilerkennwort.  
  
 **-DistributorSecurityMode** [**0**\| **1**]  
 Gibt den Sicherheitsmodus des Verteilers an. Der Wert **0** steht für den [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Authentifizierungsmodus (Standard), der Wert **1** für den [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows-Authentifizierungsmodus.  
  
 **-EncryptionLevel** [**0** \| **1** \| **2**]  
 Dies ist die Verschlüsselungsebene der Transport Layer Security (TLS, früher als Secure Sockets Layer, SSL, bezeichnet), die vom Protokolllese-Agent beim Herstellen von Verbindungen verwendet wird.  
  
|Wert von EncryptionLevel|BESCHREIBUNG|  
|---------------------------|-----------------|  
|**0**|Gibt an, dass TLS nicht verwendet wird.|  
|**1**|Gibt an, dass TLS verwendet wird, der Agent jedoch nicht überprüft, ob das TLS/SSL-Serverzertifikat von einem vertrauenswürdigen Aussteller signiert wurde.|  
|**2**|Gibt an, dass TLS verwendet und das Zertifikat überprüft wird.|  

 > [!NOTE]  
 >  Ein gültiges TLS/SSL-Zertifikat wird mit dem vollqualifizierten Domänennamen der SQL Server-Instanz definiert. Damit der Agent die Verbindung erfolgreich herstellen kann, wenn „-EncryptionLevel“ auf 2 festgelegt ist, sollten Sie einen Alias auf der lokalen SQL Server-Instanz erstellen. Der Parameter „Alias Name“ sollte den Servernamen enthalten, und für den Parameter „Server“ sollte der vollqualifizierte Name der SQL Server-Instanz festgelegt werden.
 
 Weitere Informationen finden Sie unter [Anzeigen und Ändern von Replikationssicherheitseinstellungen](../../../relational-databases/replication/security/view-and-modify-replication-security-settings.md).  
  
 **-ExtendedEventConfigFile** _configuration_path_and_file_name_  
 Gibt den Pfad und den Dateinamen für die erweiterte Ereignis-XML-Konfigurationsdatei an. Die erweiterten Ereignis-Konfigurationsdatei ermöglicht das Konfigurieren von Sitzungen und das Aktivieren der Nachverfolgung für Ereignisse.  
  
 **-HistoryVerboseLevel** [**0**\| **1**\| **2**]  
 Gibt den Umfang des Verlaufs an, der während eines Vorgangs des Protokolllese-Agents protokolliert wird. Sie können die negativen Auswirkungen der Verlaufsprotokollierung auf die Leistung minimieren, indem Sie den Wert **1** auswählen.  
  
|Wert von <legacyBold>HistoryVerboseLevel</legacyBold>|BESCHREIBUNG|  
|-------------------------------|-----------------|  
|**0**||  
|**1**|Standard. Aktualisieren Sie immer eine vorherige Verlaufsmeldung mit dem gleichen Status (Start, Status, Erfolg usw.). Wenn kein vorheriger Datensatz mit dem gleichen Status vorhanden ist, fügen Sie einen neuen Datensatz ein.|  
|**2**|Fügen Sie neue Verlaufsdatensätze ein, es sei denn, der Datensatz bezieht sich z. B. auf Leerlaufmeldungen oder Meldungen zu Aufträgen mit langer Ausführungszeit. In diesen Fällen aktualisieren Sie die vorherigen Datensätze.|  
  
 **-KeepAliveMessageInterval** _keep_alive_message_interval_seconds_  
 Die Anzahl von Sekunden, bevor vom Verlaufsthread geprüft wird, ob bestehende Verbindungen auf eine Antwort vom Server warten. Dieser Wert kann verringert werden, damit der Protokolllese-Agent vom Überprüfungs-Agent nicht als fehlerverdächtig markiert wird, wenn ein lang andauernder Batch ausgeführt wird. Der Standardwert ist 300 Sekunden.  
  
 **-LoginTimeOut** _login_time_out_seconds_  
 Die Anzahl von Sekunden, nach denen ein Timeout bei der Anmeldung eintritt. Der Standardwert ist 15 Sekunden.  
  
 **-LogScanThreshold** _scan_threshold_  
 Nur interne Verwendung.  
  
 **-MaxCmdsInTran** _number_of_commands_  
 Gibt die maximale Anzahl von Anweisungen an, die in einer Transaktion zusammengefasst werden, wenn der Protokolllese-Agent Befehle in die Verteilungsdatenbank schreibt. Mithilfe dieses Parameters können der Protokolllese-Agent und der Verteilungs-Agent beim Anwenden auf dem Abonnenten umfangreiche Transaktionen (die aus zahlreichen Befehlen bestehen) auf dem Verleger in mehrere kleinere Transaktionen aufteilen. Durch die Angabe dieses Parameters kommt es auf dem Verteiler möglicherweise zu weniger Konflikten, und die Latenzzeit zwischen Verleger und Abonnent kann reduziert werden. Da die ursprüngliche Transaktion in kleineren Einheiten angewendet wird, kann der Abonnent vor Ende der ursprünglichen Transaktion auf Zeilen einer umfangreichen logischen Verleger-Transaktion zugreifen; dies widerspricht der strikten Unteilbarkeit von Transaktionen. **0** ist der Standardwert, durch den die Transaktionsgrenzen des Verlegers beibehalten werden.  
  
> [!NOTE]
>  Für Veröffentlichungen, die nicht über [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] durchgeführt wurden, wird dieser Parameter ignoriert. Weitere Informationen finden Sie im Abschnitt "Konfigurieren des Transaktionssatz-Auftrags" unter [Performance Tuning for Oracle Publishers](../../../relational-databases/replication/non-sql/performance-tuning-for-oracle-publishers.md).  
  
 **-MessageInterval** _message_interval_  
 Das für die Verlaufsprotokollierung verwendete Zeitintervall. Wenn nach der Protokollierung des letzten Verlaufsereignisses der Wert von **MessageInterval** erreicht wird, wird erneut ein Verlaufsereignis protokolliert.  
  
 Wenn an der Quelle keine replizierte Transaktion vorhanden ist, sendet der Agent eine entsprechende Meldung an den Verteiler. Mit dieser Option wird angegeben, wie lange der Agent wartet, bevor eine weitere Meldung gesendet wird, dass keine Transaktion vorhanden ist. Agents melden immer, dass keine Transaktion vorhanden ist, wenn sie feststellen, dass an der Quelle keine Transaktionen verfügbar sind, nachdem zuvor replizierte Transaktionen verarbeitet wurden. Der Standardwert ist 60 Sekunden.  
 
 **-MultiSubnetFailover** [**0**\|**1**] gibt an, ob die MultiSubnetFailover-Eigenschaft aktiviert ist. Wenn Ihre Anwendung eine Verbindung mit einer Always On-Verfügbarkeitsgruppe in unterschiedlichen Subnetzen herstellt, ermöglicht die Festlegung von „MultiSubnetFailover“ auf 1 (TRUE) eine schnellere Erkennung des (derzeit) aktiven Servers und eine schnellere Verbindung mit diesem.   
   **Gilt für**: [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (ab [!INCLUDE [sssql19-md](../../../includes/sssql19-md.md)]).  
  
 **-Output** _output_path_and_file_name_  
 Der Pfad der Agentausgabedatei. Wenn kein Dateiname angegeben ist, wird die Ausgabe an die Konsole gesendet. Wenn eine Datei mit dem angegebenen Namen vorhanden ist, wird die Ausgabe an diese Datei angefügt.  
  
 **-OutputVerboseLevel** [**0**\| **1**\| **2** \| **3** \| **4**]  
 Gibt an, ob die Ausgabe ausführlich sein soll.  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**0**|Nur Fehlermeldungen werden gedruckt.|  
|**1**|Alle Agent-Statusberichtsmeldungen werden gedruckt.|  
|**2** (Standardwert)|Alle Fehlermeldungen und Agent-Statusberichtsmeldungen werden gedruckt.|  
|**3**|Die ersten 100 Bytes jedes replizierten Befehls werden gedruckt.|  
|**4**|Alle replizierten Befehle werden gedruckt.|  
  
 Die Werte 2-4 sind beim Debuggen hilfreich.  
  
 **-PacketSize** _packet_size_  
 Die Paketgröße in Bytes. Der Standardwert ist 4096 (Bytes).  
  
 **-PollingInterval** _polling_interval_  
 Gibt an, wie häufig replizierte Transaktionen aus dem Protokoll abgefragt werden (in Sekunden). Die Standardeinstellung ist 5 Sekunden.  
  
 **-ProfileName** _profile_name_  
 Gibt ein Agentprofil an, das für Agentparameter verwendet werden soll. Wenn **ProfileName** den Wert NULL aufweist, wird das Agentprofil deaktiviert. Wenn **ProfileName** nicht angegeben ist, wird das Standardprofil für den Agenttyp verwendet. Weitere Informationen finden Sie unter [Replication Agent Profiles](../../../relational-databases/replication/agents/replication-agent-profiles.md).  
  
 **-PublisherFailoverPartner** _server_name_[ **\\** _instance_name_]  
 Gibt die Failoverpartnerinstanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] an, die an einer Datenbank-Spiegelungssitzung mit der Veröffentlichungsdatenbank teilnimmt. Weitere Informationen finden Sie unter [Datenbankspiegelung und Replikation &#40;SQL Server&#41;](../../../database-engine/database-mirroring/database-mirroring-and-replication-sql-server.md).  
  
 **-PublisherSecurityMode** [**0**\| **1**]  
 Gibt den Sicherheitsmodus des Verlegers an. Der Wert **0** steht für die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Authentifizierung (Standard), der Wert **1** für den Windows-Authentifizierungsmodus.  
  
 **-PublisherLogin** _publisher_login_  
 Der Anmeldename des Verlegers.  
  
 **-PublisherPassword** _publisher_password_  
 Das Kennwort des Verlegers.  
  
 **-QueryTimeOut** _query_time_out_seconds_  
 Die Anzahl von Sekunden, nach denen ein Timeout bei der Abfrage eintritt. Die Standardeinstellung ist 1800 Sekunden.  
  
 **-ReadBatchSize** _number_of_transactions_  
 Die maximale Anzahl von Transaktionen, die pro Verarbeitungszyklus aus dem Transaktionsprotokoll der Veröffentlichungsdatenbank ausgelesen werden. Der Standardwert lautet 500, der Maximalwert 10.000. Der Agent setzt das Lesen von Transaktionen in Batches fort, bis alle Transaktionen aus dem Protokoll gelesen wurden. Der Parameter wird von Oracle-Verlegern nicht unterstützt.  
  
 **-ReadBatchThreshold** _number_of_commands_  
 Die Anzahl von Replikationsbefehlen, die aus dem Transaktionsprotokoll gelesen werden sollen, bevor sie vom Verteilungs-Agent an den Abonnenten ausgegeben werden. Die Standardeinstellung ist 0. Wenn dieser Parameter nicht angegeben wird, liest der Protokolllese-Agent bis zum Ende des Protokolls oder bis zu der Nummer, die in **-ReadBatchSize** (Anzahl von Transaktionen) angegeben wurde.  
  
 **-RecoverFromDataErrors**  
 Gibt an, dass der Protokolllese-Agent weiter ausgeführt wird, wenn in Spaltendaten, die von einem Nicht-SQL Server-Verleger veröffentlicht wurden, Fehler auftreten. Standardmäßig bewirken solche Fehler, dass der Protokolllese-Agent fehlschlägt. Bei Verwendung von **-RecoverFromDataErrors** werden fehlerhafte Spaltendaten entweder als NULL oder als geeigneter Nicht-NULL-Wert repliziert, und in der [MSlogreader_history](../../../relational-databases/system-tables/mslogreader-history-transact-sql.md) -Tabelle werden Warnmeldungen protokolliert. Der Parameter wird nur von Oracle-Verlegern unterstützt.  
  
## <a name="remarks"></a>Bemerkungen  
  
> [!IMPORTANT]  
>  Wenn Sie den [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Agent so installiert haben, dass er unter einem lokalen Systemkonto und nicht unter einem Domänenbenutzerkonto (Standard) ausgeführt wird, kann der Dienst nur auf den lokalen Computer zugreifen. Wenn der Protokolllese-Agent, der unter dem [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Agent ausgeführt wird, so konfiguriert ist, dass beim Anmelden bei [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]der Windows-Authentifizierungsmodus verwendet wird, schlägt der Protokolllese-Agent fehl. Die Standardeinstellung ist die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Authentifizierung. Weitere Informationen zum Ändern von Sicherheitskonten finden Sie unter [View and Modify Replication Security Settings](../../../relational-databases/replication/security/view-and-modify-replication-security-settings.md).  
  
 Führen Sie zum Starten des Protokolllese-Agents von der Eingabeaufforderung **logread.exe** aus. Informationen hierzu finden Sie unter [Ausführbare Konzepte für die Programmierung von Replikations-Agents](../../../relational-databases/replication/concepts/replication-agent-executables-concepts.md).  
  
## <a name="change-history"></a>Änderungsverlauf  
  
|Aktualisierter Inhalt|  
|---------------------|  
|Der **-ExtendedEventConfigFile** -Parameter wurde hinzugefügt.|  
|Der Parameter **-MultiSubnetFailover** wurde hinzugefügt.|
  
## <a name="see-also"></a>Weitere Informationen  
 [Replikations-Agent-Verwaltung](../../../relational-databases/replication/agents/replication-agent-administration.md)  
  
  
