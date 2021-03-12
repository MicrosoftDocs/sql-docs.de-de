---
title: Replikationsverteilungs-Agent | Microsoft-Dokumentation
description: Verschieben Sie eine Momentaufnahme und die in den Tabellen der Verteilungsdatenbank gespeicherten Transaktionen mithilfe des Replikationsverteilungs-Agents in die Zieltabellen der Abonnenten.
ms.custom: ''
ms.date: 10/29/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- Distribution Agent, executables
- agents [SQL Server replication], Distribution Agent
- Distribution Agent, parameter reference
- command prompt [SQL Server replication]
ms.assetid: 7b4fd480-9eaf-40dd-9a07-77301e44e2ac
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016
ms.openlocfilehash: 729fcb2bdd1feccfb80d7b591fe96eeb38c23933
ms.sourcegitcommit: 98acedd435aecfda1b3c4c23d3f0c3c1a12682a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2021
ms.locfileid: "102532352"
---
# <a name="replication-distribution-agent"></a>Replikationsverteilungs-Agent
[!INCLUDE[sql-asdb](../../../includes/applies-to-version/sql-asdb.md)]
  Der Replikationsverteilungs-Agent ist eine ausführbare Datei, die die Momentaufnahme (bei der Momentaufnahme- und Transaktionsreplikation) und die in den Tabellen der Verteilungsdatenbank gespeicherten Transaktionen (bei der Transaktionsreplikation) in die Zieltabellen auf den Abonnenten verschiebt.  
  
> [!NOTE]  
>  Parameter können in beliebiger Reihenfolge angegeben werden. Wenn keine optionalen Parameter angegeben werden, werden Werte von vordefinierten Registrierungseinstellungen auf dem lokalen Computer verwendet.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
distrib [-?]  
-Publisher server_name[\instance_name]  
-PublisherDB publisher_database  
-Subscriber server_name[\instance_name]  
-SubscriberDB subscriber_database   
[-AltSnapshotFolder alt_snapshot_folder_path]   
[-BcpBatchSize bcp_batch_size]  
[-CommitBatchSize commit_batch_size]  
[-CommitBatchThreshold commit_batch_threshold]  
[-Continuous]  
[-DefinitionFile def_path_and_file_name]  
[-Distributor distributor]  
[-DistributorLogin distributor_login]  
[-DistributorPassword distributor_password]  
[-DistributorSecurityMode [0|1]]  
[-EncryptionLevel [0|1|2]]  
[-ErrorFile error_path_and_file_name]  
[-ExtendedEventConfigFile configuration_path_and_file_name]  
[-FileTransferType [0|1]]  
[-FtpAddress ftp_address]  
[-FtpPassword ftp_password]   
[-FtpPort ftp_port]  
[-FtpUserName ftp_user_name]  
[-HistoryVerboseLevel [0|1|2|3]]  
[-Hostname host_name]  
[-KeepAliveMessageInterval keep_alive_message_interval_seconds]  
[-LoginTimeOut login_time_out_seconds]  
[-MaxBcpThreads]  
[-MaxDeliveredTransactions number_of_transactions]  
[-MessageInterval message_interval]  
[-MultiSubnetFailover [0|1]]
[-OledbStreamThreshold oledb_stream_threshold]  
[-Output output_path_and_file_name]  
[-OutputVerboseLevel [0|1|2]]  
[-PacketSize packet_size]  
[-PollingInterval polling_interval]  
[-ProfileName profile_name]  
[-Publication publication]  
[-QueryTimeOut query_time_out_seconds]  
[-QuotedIdentifier quoted_identifier]  
[-SkipErrors native_error_id [:...n]]  
[-SubscriberDatabasePath subscriber_path]  
[-SubscriberLogin subscriber_login]  
[-SubscriberPassword subscriber_password]  
[-SubscriberSecurityMode [0|1]]  
[-SubscriberType [0|1|3]]  
[-SubscriptionStreams [1|2|...64]]  
[-SubscriptionTableName subscription_table]  
[-SubscriptionType [0|1|2]]  
[-TransactionsPerHistory [0|1|...10000]]  
[-UseDTS]  
[-UseInprocLoader]  
[-UseOledbStreaming]  
```  
  
## <a name="arguments"></a>Argumente  
 **-?**  
 Druckt alle verfügbaren Parameter.  
  
 **-Publisher** _server_name_[ **\\** _instance_name_]  
 Der Name des Verlegers. Geben Sie den *server_name* für die Standardinstanz von [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] auf diesem Server an. Geben Sie _server_name_ **\\** _instance_name_ für eine benannte Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] auf diesem Server an.  
  
 **-PublisherDB** _publisher_database_  
 Der Name der Verlegerdatenbank.  
  
 **-Subscriber** _server_name_[ **\\** _instance_name_]  
 Der Name des Abonnenten. Geben Sie *server_name* für die Standardinstanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] auf diesem Server an. Geben Sie _server_name_ **\\** _instance_name_ für eine benannte Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] auf diesem Server an.  
  
 **-SubscriberDB** _subscriber_database_  
 Der Name der Abonnentendatenbank.  
  
 **-AltSnapshotFolder** _alt_snapshot_folder_path_  
 Der Pfad zu dem Ordner, der die Anfangsmomentaufnahme für ein Abonnement enthält.  
  
 **-BcpBatchSize** _bcp_batch_size_  
 Die Anzahl von Zeilen, die in einem Massenkopiervorgang gesendet werden sollen. Bei Ausführung eines **bcp in** -Vorgangs entspricht die Batchgröße der Anzahl von Zeilen, die als eine Transaktion an den Server gesendet werden sollen, und ebenso der Anzahl von Zeilen, die gesendet werden müssen, bevor der Verteilungs-Agent eine **bcp** -Statusmeldung protokolliert. Bei Ausführung eines **bcp out** -Vorgangs wird eine feste Batchgröße von **1000** verwendet.  
  
 **-CommitBatchSize** _commit_batch_size_  
 Die Anzahl von Transaktionen, die an den Abonnenten ausgegeben werden sollen, bevor eine COMMIT-Anweisung ausgegeben wird. Der Standardwert ist 100, der maximale Wert beträgt 10.000.
  
 **-CommitBatchThreshold** _commit_batch_threshold_  
 Die Anzahl von Replikationsbefehlen, die an den Abonnenten ausgegeben werden sollen, bevor eine COMMIT-Anweisung ausgegeben wird. Der Standardwert ist 1.000, der maximale Wert beträgt 10.000. 
  
 **-Continuous**  
 Gibt an, ob der Agent fortlaufend versucht, replizierte Transaktionen abzurufen. Wenn dieses Argument angegeben ist, ruft der Agent replizierte Transaktionen in festgelegten Abrufintervallen aus der Quelle ab, selbst wenn keine ausstehenden Transaktionen vorhanden sind.  
  
 **-DefinitionFile** _def_path_and_file_name_  
 Der Pfad der Agentdefinitionsdatei. Eine Agentdefinitionsdatei enthält Eingabeaufforderungsargumente für den Agent. Der Inhalt der Datei wird als ausführbare Datei analysiert. Verwenden Sie doppelte Anführungszeichen ("), um Argumentwerte anzugeben, die beliebige Zeichen enthalten.  
  
 **-Distributor** _distributor_  
 Der Name des Verteilers. Bei der Verteilung durch den Verteiler (Push) wird der Name standardmäßig auf dem Namen des lokalen Verteilers festgelegt.  
  
 **-DistributorLogin** _distributor_login_  
 Der Anmeldename des Verteilers.  
  
 **-DistributorPassword** _distributor_password_  
 Das Verteilerkennwort.  
  
 **-DistributorSecurityMode** [**0**\| **1**]  
 Gibt den Sicherheitsmodus des Verteilers an. Der Wert 0 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] steht für den -Authentifizierungsmodus, der Wert 1 für den Windows-Authentifizierungsmodus (Standard).  
  
 **-EncryptionLevel** [**0** \| **1** \| **2**]  
 Die Ebene der Transport Layer Security (TLS), früher als Secure Sockets Layer (SSL) bezeichnet, die vom Verteilungs-Agent beim Herstellen von Verbindungen verwendete Verschlüsselung.  
  
|Wert von EncryptionLevel|BESCHREIBUNG|  
|---------------------------|-----------------|  
|**0**|Gibt an, dass TLS nicht verwendet wird.|  
|**1**|Gibt an, dass TLS verwendet wird, der Agent jedoch nicht überprüft, ob das TLS/SSL-Serverzertifikat von einem vertrauenswürdigen Aussteller signiert wurde.|  
|**2**|Gibt an, dass TLS verwendet und das Zertifikat überprüft wird.|  
 
 > [!NOTE]  
 >  Ein gültiges TLS/SSL-Zertifikat wird mit dem vollqualifizierten Domänennamen der SQL Server-Instanz definiert. Damit der Agent die Verbindung erfolgreich herstellen kann, wenn „-EncryptionLevel“ auf 2 festgelegt ist, sollten Sie einen Alias auf der lokalen SQL Server-Instanz erstellen. Der Parameter „Alias Name“ sollte den Servernamen enthalten, und für den Parameter „Server“ sollte der vollqualifizierte Name der SQL Server-Instanz festgelegt werden.

 Weitere Informationen finden Sie unter [Anzeigen und Ändern von Replikationssicherheitseinstellungen](../../../relational-databases/replication/security/view-and-modify-replication-security-settings.md).  
  
 **-ErrorFile** _error_path_and_file_name_  
 Der Pfad und Dateiname der Fehlerdatei, die der Verteilungs-Agent generiert. Diese Datei wird an dem jeweiligen Punkt erstellt, an dem während der Anwendung der Replikationstransaktionen auf dem Abonnenten ein Fehler aufgetreten ist. Fehler, die auf dem Verleger oder Verteiler auftreten, werden nicht in dieser Datei protokolliert. Diese Datei enthält die Replikationstransaktionen, bei denen Fehler aufgetreten sind, sowie die zugeordneten Fehlermeldungen. Wenn dieser Parameter nicht angegeben ist, wird die Fehlerdatei im aktuellen Verzeichnis des Verteilungs-Agents generiert. Der Name der Fehlerdatei entspricht dem Namen des Verteilungs-Agents mit der Erweiterung ERR. Wenn eine Datei mit dem angegebenen Namen vorhanden ist, werden Fehlermeldungen an diese Datei angefügt. Dieser Parameter kann maximal 256 Unicode-Zeichen umfassen.  
  
 **-ExtendedEventConfigFile** _configuration_path_and_file_name_  
 Gibt den Pfad und den Dateinamen für die erweiterte Ereignis-XML-Konfigurationsdatei an. Die erweiterten Ereignis-Konfigurationsdatei ermöglicht das Konfigurieren von Sitzungen und das Aktivieren der Nachverfolgung für Ereignisse.  
  
 **-FileTransferType** [ **0**| **1**]  
 Gibt den Dateiübertragungstyp an. Der Wert **0** steht für UNC (Universal Naming Convention), der Wert **1** für FTP (File Transfer Protocol).  
  
 **-FtpAddress** _ftp_address_  
 Die Netzwerkadresse des FTP-Diensts für den Verteiler. Wenn die Netzwerkadresse nicht angegeben wird, wird **DistributorAddress** verwendet. Wenn **DistributorAddress** nicht angegeben wird, wird **Distributor** verwendet.  
  
 **-FtpPassword** _ftp_password_  
 Das Benutzerkennwort, mit dem eine Verbindung zum FTP-Dienst hergestellt wird.  
  
 **-FtpPort** _ftp_port_  
 Die Anschlussnummer des FTP-Diensts für den Verteiler. Wenn keine Portnummer angegeben wird, wird die Standardportnummer für den FTP-Dienst (21) verwendet.  
  
 **-FtpUserName** _ftp_user_name_  
 Der Benutzername, mit dem eine Verbindung zum FTP-Dienst hergestellt wird. Wenn kein Benutzername angegeben wird, wird **anonymous** verwendet.  
  
 **-HistoryVerboseLevel** [**0** \| **1** \| **2** \| **3**]  
 Gibt den Umfang des Verlaufs an, der während eines Verteilungsvorgangs protokolliert wird. Sie können die negativen Auswirkungen der Verlaufsprotokollierung auf die Leistung minimieren, indem Sie den Wert **1** auswählen.  
  
|Wert von <legacyBold>HistoryVerboseLevel</legacyBold>|BESCHREIBUNG|  
|-------------------------------|-----------------|  
|**0**|Statusmeldungen werden entweder an der Konsole ausgegeben oder in eine Ausgabedatei geschrieben. Verlaufsdatensätze werden nicht in der Verteilungsdatenbank protokolliert.|  
|**1**|Standard. Aktualisieren Sie immer eine vorherige Verlaufsmeldung mit dem gleichen Status (Start, Status, Erfolg usw.). Wenn kein vorheriger Datensatz mit dem gleichen Status vorhanden ist, fügen Sie einen neuen Datensatz ein.|  
|**2**|Fügen Sie neue Verlaufsdatensätze ein, es sei denn, der Datensatz bezieht sich z. B. auf Leerlaufmeldungen oder Meldungen zu Aufträgen mit langer Ausführungszeit. In diesen Fällen aktualisieren Sie die vorherigen Datensätze.|  
|**3**|Fügen Sie immer neue Datensätze ein, es sei denn, ein Datensatz bezieht sich auf Leerlaufmeldungen.|  
  
 **-Hostname** _host_name_  
 Der Hostname, der beim Herstellen der Verbindung mit dem Verleger verwendet wird. Dieser Parameter kann maximal 128 Unicode-Zeichen umfassen.  
  
 **-KeepAliveMessageInterval** _keep_alive_message_interval_seconds_  
 Die Anzahl von Sekunden, bevor vom Verlaufsthread geprüft wird, ob bestehende Verbindungen auf eine Antwort vom Server warten. Dieser Wert kann verringert werden, damit der Verteilungs-Agent vom Überprüfungs-Agent nicht als fehlerverdächtig markiert wird, wenn ein lang andauernder Batch ausgeführt wird. Die Standardeinstellung ist **300** Sekunden.  
  
 **-LoginTimeOut** _login_time_out_seconds_  
 Die Anzahl von Sekunden, nach denen ein Timeout bei der Anmeldung eintritt. Der Standardwert ist **15** Sekunden.  
  
 **-MaxBcpThreads** _number_of_threads_  
 Gibt die Anzahl von Massenkopiervorgängen an, die parallel ausgeführt werden können. Die maximale Anzahl von Threads und gleichzeitig vorhandenen ODBC-Verbindungen entspricht entweder **MaxBcpThreads** oder der Anzahl von Massenkopieranforderungen, die in der Verteilungsdatenbank in der Synchronisierungstransaktion enthalten sind. Dabei gilt der jeweils kleinere Wert. Der Wert von **MaxBcpThreads** muss größer als **0** sein. Es ist keine hartcodierte Obergrenze vorhanden. Der Standardwert entspricht **2** multipliziert mit der Anzahl von Prozessoren. Der Maximalwert beträgt **8**. Wenn Sie eine Momentaufnahme anwenden, der auf dem Verleger mit der Option für gleichzeitige Momentaufnahmen generiert wurde, wird unabhängig von der für **MaxBcpThreads** angegebenen Anzahl nur ein Thread verwendet.  
  
 **-MaxDeliveredTransactions** _number_of_transactions_  
 Die maximale Anzahl von Push- oder Pulltransaktionen, die in einer Synchronisierung auf Abonnenten angewendet werden. Der Wert **0** gibt an, dass das Maximum einer unendlichen Anzahl von Transaktionen entspricht. Andere Werte können von Abonnenten verwendet werden, um die Dauer einer Synchronisierung zu verkürzen, die von einem Verleger abgerufen wird.  
  
> [!NOTE]  
>  Wenn sowohl -MaxDeliveredTransactions als auch -Continuous angegeben wird, übermittelt der Verteilungs-Agent die angegebene Anzahl der Transaktionen und wird dann (obwohl -Continuous angegeben wurde) beendet. Nachdem der Auftrag abgeschlossen wurde, müssen Sie den Verteilungs-Agent neu starten.  
  
 **-MessageInterval** _message_interval_  
 Das für die Verlaufsprotokollierung verwendete Zeitintervall. Ein Verlaufsereignis wird protokolliert, wenn einer der folgenden Parameter erreicht wird:  
  
-   Der Wert von **TransactionsPerHistory** wird erreicht, nachdem das letzte Verlaufsereignis protokolliert wurde.  
  
-   Der Wert von **MessageInterval** wird erreicht, nachdem das letzte Verlaufsereignis protokolliert wurde.  
  
 Wenn an der Quelle keine replizierte Transaktion vorhanden ist, sendet der Agent eine entsprechende Meldung an den Verteiler. Mit dieser Option wird angegeben, wie lange der Agent wartet, bevor eine weitere Meldung gesendet wird, dass keine Transaktion vorhanden ist. Agents melden immer, dass keine Transaktion vorhanden ist, wenn sie feststellen, dass an der Quelle keine Transaktionen verfügbar sind, nachdem zuvor replizierte Transaktionen verarbeitet wurden. Der Standardwert ist 60 Sekunden.  

**-MultiSubnetFailover** gibt an, ob die MultiSubnetFailover-Eigenschaft aktiviert ist. Wenn Ihre Anwendung eine Verbindung mit einer Always On-Verfügbarkeitsgruppe in unterschiedlichen Subnetzen herstellt, ermöglicht die Festlegung von „MultiSubnetFailover“ auf TRUE eine schnellere Erkennung des (derzeit) aktiven Servers und eine schnellere Verbindung mit diesem.   
  **Gilt für**: [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (ab [!INCLUDE [sssql19-md](../../../includes/sssql19-md.md)]).  

  
 **-OledbStreamThreshold** _oledb_stream_threshold_  
 Gibt die Mindestgröße in Bytes für BLOB-Daten (Binary Large Object) an, ab der die Daten als Datenstrom gebunden werden. Zur Verwendung dieses Parameters müssen Sie **-UseOledbStreaming** angeben. Die Werte können zwischen 400 und1048576 Bytes liegen. Der Standardwert beträgt 16384 Bytes.  
  
 **-Output** _output_path_and_file_name_  
 Der Pfad der Agentausgabedatei. Wenn kein Dateiname angegeben ist, wird die Ausgabe an die Konsole gesendet. Wenn eine Datei mit dem angegebenen Namen vorhanden ist, wird die Ausgabe an diese Datei angefügt.  
  
 **-OutputVerboseLevel** [ **0**| **1**| **2**]  
 Gibt an, ob die Ausgabe ausführlich sein soll. Wenn die Meldungsstufe **0** beträgt, werden nur Fehlermeldungen gedruckt. Wenn die Meldungsstufe **1** beträgt, werden alle Statusberichtsmeldungen gedruckt. Wenn die Meldungsstufe **2** (Standard) beträgt, werden alle Fehlermeldungen und Statusberichtsmeldungen gedruckt, was beim Debuggen nützlich ist.  
  
 **-PacketSize** _packet_size_  
 Die Paketgröße in Bytes. Der Standardwert ist 4096 (Bytes).  
  
 **-PollingInterval** _polling_interval_  
 Gibt an, wie häufig replizierte Transaktionen aus der Verteilungsdatenbank abgefragt werden (in Sekunden). Die Standardeinstellung ist 5 Sekunden.  
  
 **-ProfileName** _profile_name_  
 Gibt ein Agentprofil an, das für Agentparameter verwendet werden soll. Wenn **ProfileName** den Wert NULL aufweist, wird das Agentprofil deaktiviert. Wenn **ProfileName** nicht angegeben ist, wird das Standardprofil für den Agenttyp verwendet. Weitere Informationen finden Sie unter [Replication Agent Profiles](../../../relational-databases/replication/agents/replication-agent-profiles.md).  
  
 **-Publication** _publication_  
 Der Name der Veröffentlichung. Dieser Parameter ist nur gültig, wenn die Veröffentlichung so festgelegt ist, dass sie immer eine Momentaufnahme für neue oder neu initialisierte Abonnements zur Verfügung hat.  
  
 **-QueryTimeOut** _query_time_out_seconds_  
 Die Anzahl von Sekunden, nach denen ein Timeout bei der Abfrage eintritt. Die Standardeinstellung ist 1800 Sekunden.  
  
 **-QuotedIdentifier** _quoted_identifier_  
 Gibt den zu verwendenden Bezeichner in Anführungszeichenzeichen an. Das erste Zeichen des Werts gibt den Wert an, den der Verteilungs-Agent verwendet. Wenn **QuotedIdentifier** ohne Wert verwendet wird, verwendet der Verteilungs-Agent ein Leerzeichen. Wenn **QuotedIdentifier** nicht verwendet wird, verwendet der Verteilungs-Agent einen vom Abonnenten unterstützten Bezeichner in Anführungszeichen.  
  
 **-SkipErrors** _native_error_id_ [ **:** _...n_]  
 Eine durch Doppelpunkte getrennte Liste, die die Fehlernummern angibt, die von diesem Agent übersprungen werden sollen.  
  
 **-SubscriberDatabasePath** _subscriber_database_path_  
 Der Pfad der Jet-Datenbank (MDB-Datei), wenn **SubscriberType** auf **2** festgelegt ist. (Dadurch wird eine Verbindung mit einer Jet-Datenbank ohne ODBC-DSN (Data Source Name) möglich.)  
  
 **-SubscriberLogin** _subscriber_login_  
 Der Anmeldename des Abonnenten. Wenn **SubscriberSecurityMode** auf **0** festgelegt ist (für die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Authentifizierung), muss dieser Parameter angegeben werden.  
  
 **-SubscriberPassword** _subscriber_password_  
 Das Kennwort des Abonnenten. Wenn **SubscriberSecurityMode** auf **0** festgelegt ist (für die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Authentifizierung), muss dieser Parameter angegeben werden.  
  
 **-SubscriberSecurityMode** [ **0**| **1**]  
 Gibt den Sicherheitsmodus des Abonnenten an. Der Wert **0** steht für die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Authentifizierung, der Wert **1** für den Windows-Authentifizierungsmodus (Standard).  
  
 **-SubscriberType** [ **0**| **1**| **3**]  
 Gibt den Typ der vom Verteilungs-Agent verwendeten Abonnentenverbindung an.  
  
|Wert von SubscriberType|BESCHREIBUNG|  
|--------------------------|-----------------|  
|**0**|[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]|  
|**1**|ODBC-Datenquelle (ODBC data source)|  
|**3**|OLE DB-Datenquelle|  
  
 **-SubscriptionStreams** [**0**\|**1**\|**2**\|...**64**]  
 Die Anzahl zulässiger Verbindungen pro Verteilungs-Agent, um Änderungsbatches parallel auf einen Abonnenten anzuwenden, während viele Transaktionsmerkmale beibehalten werden, die bei Verwendung eines einzigen Threads vorhanden sind. Für einen [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Verleger wird ein Wertebereich von 1 bis 64 unterstützt. Dieser Parameter wird nur unterstützt, wenn auf dem Verleger und dem Verteiler [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] oder höhere Versionen ausgeführt werden. Dieser Parameter wird für Nicht-Abonnenten von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] oder Peer-zu-Peer-Abonnements nicht unterstützt, oder er muss auf 0 festgelegt werden.  
  
> [!NOTE]  
>  Wenn eine der Verbindungen oder ein Commit hierfür nicht ausgeführt werden kann, wird der aktuelle Batch von allen Verbindungen verworfen, und der Agent versucht mithilfe eines einzigen Datenstroms, die fehlgeschlagenen Batches zu wiederholen. Vor dem Abschluss dieser Wiederholungsphase kann es auf dem Abonnenten vorübergehend zur Transaktionsinkonsistenzen kommen. Nach dem erfolgreichen Ausführen (Commit) der fehlgeschlagenen Batches wird der Abonnent wieder in einen Zustand der Transaktionskonsistenz versetzt.  
  
> [!IMPORTANT]  
>  Wenn Sie für **-SubscriptionStreams** einen Wert von mindestens 2 angeben, weicht die Reihenfolge, in der Transaktionen auf dem Abonnenten empfangen werden, möglicherweise von der Reihenfolge ab, in der sie auf dem Verleger erstellt wurden. Falls dieses Verhalten bei der Synchronisierung zu Einschränkungsverletzungen führt, verwenden Sie die NOT FOR REPLICATION-Option, um die Einschränkungserzwingung während der Synchronisierung zu deaktivieren. Weitere Informationen finden Sie unter [Kontrollieren des Verhaltens von Triggern und Einschränkungen während der Synchronisierung &#40;Replikationsprogrammierung mit Transact-SQL&#41;](../../../relational-databases/replication/control-behavior-of-triggers-and-constraints-in-synchronization.md).  
  
> [!NOTE]  
>  subscriptionstreams [!INCLUDE[tsql](../../../includes/tsql-md.md)]kann nicht für Artikel verwendet werden, die für die Übermittlung von  konfiguriert sind. Um subscriptionstreams zu verwenden, konfigurieren Sie Artikel stattdessen für die Übermittlung von Aufrufen gespeicherter Prozeduren.  
  
 **-SubscriptionTableName** _subscription_table_  
 Der Name der Abonnementtabelle, die auf dem angegebenen Abonnenten generiert oder verwendet wird. Wenn nicht anders angegeben, wird die [MSreplication_subscriptions &#40;Transact-SQL&#41;](../../../relational-databases/system-tables/msreplication-subscriptions-transact-sql.md)-Tabelle verwendet. Verwenden Sie diese Option für Datenbank-Managementsysteme (DBMS), die keine langen Dateinamen unterstützen.  
  
 **-SubscriptionType** [ **0**| **1**| **2**]  
 Gibt den Abonnementtyp für die Verteilung an. Der Wert **0** steht für ein Pushabonnement, der Wert **1** für ein Pullabonnement und der Wert **2** für ein anonymes Abonnement.  
  
 **-TransactionsPerHistory** [ **0**| **1**|... **10000**]  
 Gibt das Transaktionsintervall für die Verlaufsprotokollierung an. Wenn die Anzahl von Transaktionen mit ausgeführtem Commit seit der letzten Instanz der Verlaufsprotokollierung den Wert dieser Option übersteigt, wird eine Verlaufsmeldung protokolliert. Der Standardwert ist 100. Der Wert **0** gibt unendliche **TransactionsPerHistory** an. See the preceding **–MessageInterval** parameter.  
  
 **-UseDTS**  
 Muss als Parameter für eine Veröffentlichung angegeben werden, die eine Datentransformation ermöglicht.  
  
 **-UseInprocLoader**  
 Verbessert die Leistung der Anfangsmomentaufnahme, indem der Verteilungs-Agent veranlasst wird, beim Anwenden von Momentaufnahmedateien auf dem Abonnenten den BULK INSERT-Befehl zu verwenden. Dieser Parameter ist veraltet, da er mit dem XML-Datentyp nicht kompatibel ist. Wenn Sie keine XML-Daten replizieren, können Sie diesen Parameter verwenden. Dieser Parameter kann mit Momentaufnahmen im Zeichenmodus oder mit Nicht-Abonnenten von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] nicht verwendet werden. Wenn Sie den Parameter verwenden, muss das [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Dienstkonto auf dem Abonnenten über Leseberechtigungen für das Verzeichnis verfügen, in dem sich die BCP-Datendateien der Momentaufnahmen befinden. Wenn der Parameter nicht verwendet wird, liest der Agent (bei Nicht-Abonnenten von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]) oder der vom Agent geladene ODBC-Treiber (bei [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Abonnenten) aus den Dateien, sodass der Sicherheitskontext des [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Dienstkontos nicht verwendet wird.  
  
 **-UseOledbStreaming**  
 Wenn dieser Parameter angegeben wird, wird die Bindung der BLOB-Daten (Binary Large Object) als Datenstrom aktiviert. Verwenden Sie **-OledbStreamThreshold** , um die Größe in Bytes anzugeben, ab der ein Datenstrom verwendet wird. **UseOledbStreaming** ist standardmäßig aktiviert. **UseOledbStreaming** schreibt in den Ordner **C:\Programme\Microsoft SQL Server\\<Version\>\COM**.  
  
## <a name="remarks"></a>Bemerkungen  
  
> [!IMPORTANT]  
>  Wenn Sie den [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Agent so installiert haben, dass er unter einem lokalen Systemkonto und nicht unter einem Domänenbenutzerkonto (Standard) ausgeführt wird, kann der Dienst nur auf den lokalen Computer zugreifen. Wenn der Verteilungs-Agent, der unter dem [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Agent ausgeführt wird, so konfiguriert ist, dass beim Anmelden bei einer Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]der Windows-Authentifizierungsmodus verwendet wird, schlägt der Verteilungs-Agent fehl. Die Standardeinstellung ist die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Authentifizierung. Weitere Informationen zum Ändern von Sicherheitskonten finden Sie unter [View and Modify Replication Security Settings](../../../relational-databases/replication/security/view-and-modify-replication-security-settings.md).  
  
 Führen Sie zum Starten des Verteilungs-Agents von der Eingabeaufforderung **distrib.exe** aus. Informationen hierzu finden Sie unter [Ausführbare Konzepte für die Programmierung von Replikations-Agents](../../../relational-databases/replication/concepts/replication-agent-executables-concepts.md).  
  
## <a name="change-history"></a>Änderungsverlauf  
  
|Aktualisierter Inhalt|  
|---------------------|  
|Der **-ExtendedEventConfigFile** -Parameter wurde hinzugefügt.|  
|Der Parameter **-MultiSubnetFailover** wurde hinzugefügt.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Replikations-Agent-Verwaltung](../../../relational-databases/replication/agents/replication-agent-administration.md)  
  
  
