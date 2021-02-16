---
description: ALTER SERVER CONFIGURATION (Transact-SQL)
title: ALTER SERVER CONFIGURATION (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 05/22/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER SERVER CONFIGURATION
- ALTER_SERVER_CONFIGURATION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- SERVER CONFIGURATION, ALTER
- process affinity, setting
- ALTER SERVER CONFIGURATION statement
- setting process affinity
ms.assetid: f3059e42-5f6f-4a64-903c-86dca212a4b4
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e1e9186d4b0de1540f4d8e7a1ca54c15a36817ed
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340622"
---
# <a name="alter-server-configuration-transact-sql"></a>ALTER SERVER CONFIGURATION (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Ändert globale Konfigurationseinstellungen für den aktuellen Server in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  

```syntaxsql
ALTER SERVER CONFIGURATION  
SET <optionspec>   
[;]  
  
<optionspec> ::=  
{  
     <process_affinity>  
   | <diagnostic_log>  
   | <failover_cluster_property>  
   | <hadr_cluster_context>  
   | <buffer_pool_extension>  
   | <soft_numa>  
   | <memory_optimized>
}  
  
<process_affinity> ::=   
   PROCESS AFFINITY   
   {  
     CPU = { AUTO | <CPU_range_spec> }   
   | NUMANODE = <NUMA_node_range_spec>   
   }  
   <CPU_range_spec> ::=   
      { CPU_ID | CPU_ID  TO CPU_ID } [ ,...n ]   
  
   <NUMA_node_range_spec> ::=   
      { NUMA_node_ID | NUMA_node_ID TO NUMA_node_ID } [ ,...n ]  
  
<diagnostic_log> ::=   
   DIAGNOSTICS LOG   
   {   
     ON    
   | OFF    
   | PATH = { 'os_file_path' | DEFAULT }    
   | MAX_SIZE = { 'log_max_size' MB | DEFAULT }    
   | MAX_FILES = { 'max_file_count' | DEFAULT }    
   }  
  
<failover_cluster_property> ::=   
   FAILOVER CLUSTER PROPERTY <resource_property>  
   <resource_property> ::=  
      {  
        VerboseLogging = { 'logging_detail' | DEFAULT }    
      | SqlDumperDumpFlags = { 'dump_file_type' | DEFAULT }  
      | SqlDumperDumpPath = { 'os_file_path'| DEFAULT }  
      | SqlDumperDumpTimeOut = { 'dump_time-out' | DEFAULT }  
      | FailureConditionLevel = { 'failure_condition_level' | DEFAULT }  
      | HealthCheckTimeout = { 'health_check_time-out' | DEFAULT }  
      }  
  
<hadr_cluster_context> ::=  
   HADR CLUSTER CONTEXT = { 'remote_windows_cluster' | LOCAL }  
  
<buffer_pool_extension>::=  
    BUFFER POOL EXTENSION   
    { ON ( FILENAME = 'os_file_path_and_name' , SIZE = <size_spec> )   
    | OFF }  
  
    <size_spec> ::=  
        { size [ KB | MB | GB ] }  
  
<soft_numa> ::=  
    SOFTNUMA  
    { ON | OFF }  

<memory-optimized> ::=   
   MEMORY_OPTIMIZED   
   {   
     ON 
   | OFF
   | [ TEMPDB_METADATA = { ON [(RESOURCE_POOL='resource_pool_name')] | OFF }
   | [ HYBRID_BUFFER_POOL = { ON | OFF }
   }  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
**\<process_affinity> ::=**  
  
PROCESS AFFINITY  
Ermöglicht das Zuordnen von Hardwarethreads zu CPUs.  
  
CPU = { AUTO | \<CPU_range_spec> }  
Verteilt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Arbeitsthreads auf alle im angegebenen Bereich enthaltenen CPUs. CPUs außerhalb des angegebenen Bereichs werden keine Threads zugewiesen.  
  
AUTO  
Gibt an, dass einer CPU kein Thread zugewiesen ist. Das Betriebssystem kann Threads auf der Grundlage der Serverarbeitsauslastung beliebig zwischen CPUs verschieben. Dies ist die Standard- und empfohlene Einstellung.  
  
\<CPU_range_spec> ::=  
Gibt die CPU oder den Bereich von CPUs an, der bzw. denen Threads zugewiesen werden sollen.  
  
{ CPU_ID | CPU_ID TO CPU_ID } [ ,...n ]  
Eine Liste mit mindestens einer CPU. CPU-IDs beginnen bei 0 und sind **integer**-Werte.  
  
NUMANODE = \<NUMA_node_range_spec>  
Weist allen CPUs, die um angegebenen NUMA-Knoten oder Bereich von Konten gehören, Threads zu.  
  
\<NUMA_node_range_spec> ::=  
Gibt den NUMA-Knoten oder den Bereich von NUMA-Knoten an.  
  
{ NUMA_node_ID | NUMA_node_ID  TO NUMA_node_ID } [ ,...n ]  
Eine Liste mit mindestens einem NUMA-Knoten. NUMA-Knoten-IDs beginnen bei 0 und sind **integer**-Werte.  
  
**\<diagnostic_log> ::=**  
  
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]).  

  
DIAGNOSTICS LOG  
Startet oder beendet die Protokollierung von Diagnosedaten, die die sp_server_diagnostics-Prozedur erfasst. Dieses Argument legt außerdem SQLDIAG-Protokollkonfigurationsparameter wie die Anzahl der Protokolldateirollover, die Protokolldateigröße und den Dateispeicherort fest. Weitere Informationen finden Sie unter [Anzeigen und Lesen des Failoverclusterinstanz-Diagnoseprotokolls](../../sql-server/failover-clusters/windows/view-and-read-failover-cluster-instance-diagnostics-log.md).  
  
EIN  
Startet die [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)]-Protokollierung von Diagnosedaten an dem in der PATH-Dateioption angegebenen Speicherort. Dies ist das Standardargument.  
  
OFF  
Beendet die Protokollierung von Diagnosedaten.  
  
PATH = { 'os_file_path' | DEFAULT }  
Pfad, der den Speicherort für die Diagnoseprotokolle angibt. Der Standardspeicherort ist \<\MSSQL\Log> im Installationsordner der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Failoverclusterinstanz.  
  
MAX_SIZE = { 'log_max_size' MB | DEFAULT }  
Die maximale Größe (in MB) für jedes Diagnoseprotokoll. Die Standardeinstellung ist 100 MB.  
  
MAX_FILES = { 'max_file_count' | DEFAULT }  
Maximale Anzahl von Diagnoseprotokolldateien, die auf dem Computer gespeichert werden können, bevor sie für neue Diagnoseprotokolle wiederverwendet werden.  
  
**\<failover_cluster_property> ::=**  
  
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]).    
  
FAILOVER CLUSTER PROPERTY  
Ändert die privaten Failoverclustereigenschaften der SQL Server-Ressource.  
  
VERBOSE LOGGING = { 'logging_detail' | DEFAULT }  
Legt den Protokolliergrad für SQL Server-Failoverclustering fest. Diese Option kann aktiviert werden, um in den Fehlerprotokollen für die Problembehandlung weitere Details bereitzustellen.  
  
-   0: Die Protokollierung ist deaktiviert (Standard)  
  
-   1 – Nur Fehler  
  
-   2: Fehler und Warnungen  
  
In Ressourcenfailoverszenarios kann die Ressourcen-DLL in SQL Server ein Speicherabbild abrufen, bevor ein Failover durchgeführt wird. Dies gilt sowohl für FCI- als auch für Verfügbarkeitsgruppentechnologien. Wenn die Ressourcen-DDL in SQL Server feststellt, dass es bei einer SQL Server-Ressource zu einem Fehler kam, verwendet die Ressourcen-DLL in SQL Server das Hilfsprogramm Sqldumper.exe, um ein Speicherabbild des SQL Server-Prozesses abzurufen. Sie müssen die folgenden drei Eigenschaften als Voraussetzung festlegen, damit das Hilfsprogramm Sqldumper.exe das Speicherabbild bei einem Ressourcenfailover erfolgreich erstellen kann: SqlDumperDumpTimeOut, SqlDumperDumpPath, SqlDumperDumpFlags.

SQLDUMPEREDUMPFLAGS  
Bestimmt den Typ der Dumpdateien, die vom SQL Server-Hilfsprogramm SQLDumper generiert werden. Die Standardeinstellung ist 0. Dezimalwerte anstelle von Hexadezimalwerten werden für diese Einstellung verwendet. Verwenden Sie für ein Miniabbild den Wert 288. Für ein Miniabbild mit indirektem Arbeitsspeicher verwenden Sie den Wert 296. Für ein gefiltertes Abbild verwenden Sie den Wert 33024. Weitere Informationen finden Sie im [Knowledge Base-Artikel zum SQL Server Dumper-Hilfsprogramm](/troubleshoot/sql/tools/use-sqldumper-generate-dump-file).  
  
SQLDUMPERDUMPPATH = { 'os_file_path' | DEFAULT }  
Der Speicherort, an dem das Hilfsprogramm SQLDumper die Dumpdateien speichert. Weitere Informationen finden Sie im [Knowledge Base-Artikel zum SQL Server Dumper-Hilfsprogramm](/troubleshoot/sql/tools/use-sqldumper-generate-dump-file).  
  
SQLDUMPERDUMPTIMEOUT = { 'dump_time-out' | DEFAULT }  
Der Timeoutwert in Millisekunden für die Generierung eines Dumps durch das Hilfsprogramm SQLDumper, falls ein SQL Server-Fehler auftritt. Der Standardwert ist 0; das bedeutet, dass das Fertigstellen des Dumps nicht zeitlich begrenzt ist. Weitere Informationen finden Sie im [Knowledge Base-Artikel zum SQL Server Dumper-Hilfsprogramm](/troubleshoot/sql/tools/use-sqldumper-generate-dump-file).  
  
 FAILURECONDITIONLEVEL = { 'failure_condition_level' | DEFAULT }  
 Die Bedingungen, unter denen für die SQL Server-Failoverclusterinstanz ein Failover oder Neustart durchgeführt werden soll. Der Standardwert ist 3; das bedeutet, dass für die SQL Server-Ressource bei kritischen Serverfehlern ein Failover oder Neustart durchgeführt wird. Weitere Informationen zum Konfigurieren dieser Eigenschaft finden Sie unter [Configure FailureConditionLevel Property Settings (Konfigurieren von FailureConditionLevel-Eigenschafteneinstellungen)](../../sql-server/failover-clusters/windows/configure-failureconditionlevel-property-settings.md).  
  
HEALTHCHECKTIMEOUT = { 'health_check_time-out' | DEFAULT }  
Der Timeoutwert, der festlegt, wie lange die Ressourcen-DLL der SQL Server-Datenbank-Engine auf Informationen über den Serverzustand warten soll, bevor eine Instanz von SQL Server als nicht reagierend eingestuft wird. Der Timeoutwert wird in Millisekunden angegeben. Der Standardwert beträgt 60.000 Millisekunden (60 Sekunden).  
  
**\<hadr_cluster_context> ::=**  
  
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]).   
  
HADR CLUSTER CONTEXT **=** { **'** _remote\_windows\_cluster_ **'** | LOCAL }  
Wechselt mit dem HADR-Clusterkontext der Serverinstanz zum angegebenen WSFC (Windows Server Failover Cluster). Der *HADR-Clusterkontext* bestimmt, welcher WSFC die Metadaten für die von der Serverinstanz gehosteten Verfügbarkeitsreplikate verwaltet. Verwenden Sie die SET HADR CLUSTER CONTEXT-Option nur während einer clusterübergreifenden Migration von [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] zu einer Instanz von [!INCLUDE[ssSQL11SP1](../../includes/sssql11sp1-md.md)] oder höher auf einem neuen WSFC.  
  
Sie können den HADR-Clusterkontext nur vom lokalen WSFC zu einem Remote-WSFC wechseln. Dann können Sie sich entschließen, vom Remote-WSFC zum lokalen WSFC wieder zurückzuwechseln. Der HADR-Clusterkontext kann nur zu einem Remotecluster wechseln, wenn die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] keine Verfügbarkeitsreplikate hostet.  
  
Ein Remote-HADR-Clusterkontext kann jederzeit zurück zum lokalen Cluster wechseln. Der Kontext kann jedoch nicht erneut gewechselt werden, solange die Serverinstanz Verfügbarkeitsreplikate hostet. 
  
Um den Zielcluster zu identifizieren, geben Sie einen der folgenden Werte an:  
  
*Windows-Cluster*  
Der Netzwerkname eines WSFC. Sie können entweder den Kurznamen oder den vollständigen Domänennamen angeben. ALTER SERVER CONFIGURATION sucht die Ziel-IP-Adresse eines Kurznamens mithilfe einer DNS-Auflösung. Unter manchen Umständen könnte ein Kurzname für Verwirrung sorgen, und DNS gibt möglicherweise die falsche IP-Adresse zurück. Es wird empfohlen, dass Sie den vollständigen Domänennamen angeben.  
  
> [!NOTE] 
> Eine clusterübergreifende Migration unter Verwendung dieser Einstellung wird nicht mehr unterstützt. Verwenden Sie zum Ausführen einer clusterübergreifenden Migration eine verteilte Verfügbarkeitsgruppe oder eine andere Methode, wie etwa Protokollversand. 
  
LOCAL  
Der lokale WSFC.  
  
Weitere Informationen finden Sie unter [Ändern des HADR-Clusterkontexts der Serverinstanz &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/change-the-hadr-cluster-context-of-server-instance-sql-server.md).  
  
**\<buffer_pool_extension>::=**  
  
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]).    
  
EIN  
Aktiviert die Pufferpoolerweiterungsoption. Diese Option erweitert die Größe des Pufferpools, indem sie permanenten (nicht flüchtigen) Speicher verwendet. Permanenter Speicher wie Solid State Drives (SSD) behält nicht modifizierte Datenseiten im Pool bei. Weitere Informationen zu diesem Feature finden Sie im Artikel zur [Pufferpoolerweiterung](../../database-engine/configure-windows/buffer-pool-extension.md). Die Pufferpoolerweiterung ist nicht in jeder Edition von SQL Server verfügbar. Weitere Informationen finden Sie unter [Von den SQL Server 2016-Editionen unterstützte Features](../../sql-server/editions-and-components-of-sql-server-2016.md).  
  
FILENAME = 'os_file_path_and_name'  
Definiert den Verzeichnispfad und den Namen der Cachedatei der Pufferpoolerweiterung. Die Dateierweiterung muss als .BPE angegeben werden. Deaktivieren Sie BUFFER POOL EXTENSION, bevor Sie FILENAME ändern.  
  
SIZE = *size* [ **KB** | MB | GB ]  
Definiert die Größe des Caches. Die Standardgröße wird in KB angegeben. Die Mindestgröße ist die Größe des max. Serverarbeitsspeichers. Die maximale Anzahl ist die 32fache Größe von Max. Serverarbeitsspeicher. Weitere Informationen zum maximalen Serverarbeitsspeicher finden Sie unter [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).  
  
Deaktivieren Sie BUFFER POOL EXTENSION, bevor Sie die Größe der Datei ändern. Um eine Größe anzugeben, die kleiner als die aktuelle Größe ist, muss die Instanz von SQL Server neu gestartet werden, um Arbeitsspeicher freizugeben. Andernfalls muss die angegebene Größe größer oder gleich der aktuellen Größe sein.  
  
OFF  
Deaktiviert die Pufferpoolerweiterungsoption. Deaktivieren Sie die Pufferpoolerweiterungsoption, bevor Sie alle zugeordneten Parameter, wie die Größe oder den Namen der Datei, ändern. Wenn diese Option deaktiviert wird, werden alle zugehörigen Konfigurationsdaten aus der Registrierung entfernt.  
  
> [!WARNING]  
>  Das Deaktivieren der Pufferpoolerweiterung könnte eine negative Auswirkung auf die Serverleistung haben, da die Größe des Pufferpools erheblich reduziert wird.  
  
**\<soft_numa>**  

**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]).  
  
EIN  
Aktiviert die automatische Partitionierung, um große NUMA-Hardwareknoten in kleine NUMA-Knoten aufzuteilen. Das Ändern des ausgeführten Werts erfordert einen Neustart der Datenbank-Engine.  
  
OFF  
Deaktiviert die automatische Softwarepartitionierung, um große NUMA-Hardwareknoten in kleine NUMA-Knoten aufzuteilen. Das Ändern des ausgeführten Werts erfordert einen Neustart der Datenbank-Engine.  

> [!WARNING]
> Es gibt bekannte Probleme beim Verhalten der ALTER SERVER CONFIGURATION-Anweisung mit der SOFT NUMA-Option und dem SQL Server-Agent.  Es wird empfohlen, dass Sie folgendermaßen vorgehen:  
> 1) Beenden Sie die Instanz des SQL Server-Agents.  
> 2) Führen Sie die Option ALTER SERVER CONFIGURATION SOFT NUMA aus.  
> 3) Starten Sie die SQL Server-Instanz neu.  
> 4) Starten Sie die Instanz des SQL Server-Agents wieder.  
  
**Weitere Informationen** Wenn Sie die ALTER SERVER CONFIGURATION mit dem Befehl SET SOFT NUMA ausführen, bevor der SQL Server-Dienst neu startet, führt der SQL Server-Agent beim Beenden den Befehl T-SQL RECONFIGURE aus, der die SOFT NUMA-Einstellungen auf die Einstellungen vor ALTER SERVER CONFIGURATION zurücksetzt. 

**\<memory_optimized> ::=**

**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]).

EIN <br>
Aktiviert alle Features auf Instanzebene, die Teil der [In-Memory Database](../../relational-databases/in-memory-database.md)-Featurefamilie sind. Hierzu gehören derzeit [speicheroptimierte tempdb-Metadaten](../../relational-databases/databases/tempdb-database.md#memory-optimized-tempdb-metadata) und der [hybride Pufferpool](../../database-engine/configure-windows/hybrid-buffer-pool.md). Diese Option erfordert einen Neustart, um wirksam zu werden.

OFF <br>
Deaktiviert alle Features auf Instanzebene, die Teil der In-Memory Database-Featurefamilie sind. Diese Option erfordert einen Neustart, um wirksam zu werden.

TEMPDB_METADATA = ON | OFF <br>
Aktiviert oder deaktiviert nur speicheroptimierte tempdb-Metadaten. Diese Option erfordert einen Neustart, um wirksam zu werden. 

RESOURCE_POOL='resource_pool_name' <br>
In Kombination mit TEMPDB_METADATA = ON gibt diese Option den benutzerdefinierten Ressourcenpool an, der für tempdb verwendet werden soll. Ist dieser nicht angegeben, verwendet tempdb den Standardpool. Der Pool muss bereits vorhanden sein. Ist der Pool beim Neustart des Diensts nicht verfügbar, verwendet tempdb den Standardpool.


HYBRID_BUFFER_POOL = ON | OFF <br>
Aktiviert oder deaktiviert den hybriden Pufferpool auf Instanzebene. Diese Option erfordert einen Neustart, um wirksam zu werden.


## <a name="general-remarks"></a>Allgemeine Hinweise  
Diese Anweisung erfordert keinen Neustart von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], wenn dies nicht explizit angegeben wird. Im Fall einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Failoverclusterinstanz ist kein Neustart der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Clusterressource erforderlich.  
  
## <a name="limitations-and-restrictions"></a>Einschränkungen  
Diese Anweisung unterstützt keine DDL-Trigger.  
  
## <a name="permissions"></a>Berechtigungen  
Erfordert:
- `ALTER SETTINGS`-Berechtigungen für die Prozessaffinitätsoption.
- `ALTER SETTINGS`- und `VIEW SERVER STATE`-Berechtigungen für das Diagnoseprotokoll und Optionen für die Failoverclustereigenschaften.
- `CONTROL SERVER`-Berechtigung für die HADR-Clusterkontextoption.  
- `ALTER SERVER STATE`-Berechtigung für die Pufferpoolerweiterungsoption.  
  
Die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssDE](../../includes/ssde-md.md)]-Ressourcen-DLL wird unter dem lokalen Systemkonto ausgeführt. Deshalb muss das lokale Systemkonto über Lese- und Schreibzugriff auf den in der Diagnoseprotokolloption angegebenen Pfad verfügen.  
  
## <a name="examples"></a>Beispiele  
  
|Category|Funktionssyntaxelemente|  
|--------------|------------------------------|  
|[Festlegen der Prozessaffinität](#Affinity)|CPU • NUMANODE • AUTO|  
|[Festlegen der Optionen des Diagnoseprotokolls](#Diagnostic)|ON • OFF • PATH • MAX_SIZE|  
|[Festlegen der Failoverclustereigenschaften](#Failover)|HealthCheckTimeout|  
|[Ändern des Clusterkontexts eines Verfügbarkeitsreplikats](#ChangeClusterContextExample)|**'** *windows_cluster* **'**|  
|[Festlegen der Pufferpoolerweiterung](#BufferPoolExtension)|BUFFER POOL EXTENSION| 
|[Festlegen von In-Memory Database-Optionen](#MemoryOptimized)|MEMORY_OPTIMIZED|

  
###  <a name="setting-process-affinity"></a><a name="Affinity"></a> Festlegen der Prozessaffinität  
Die Beispiele in diesem Abschnitt veranschaulichen, wie die Prozessaffinität für CPUs und NUMA-Knoten festgelegt wird. In den Beispielen wird davon ausgegangen, dass der Server 256 CPUs umfasst, die in vier Gruppen von jeweils 16 NUMA-Knoten unterteilt sind. Den NUMA-Knoten oder CPUs sind keine Threads zugewiesen.  
  
-   Gruppe 0: NUMA-Knoten 0 bis 3, CPUs 0 bis 63  
-   Gruppe 1: NUMA-Knoten 4 bis 7, CPUs 64 bis 127  
-   Gruppe 2: NUMA-Knoten 8 bis 12, CPUs 128 bis 191  
-   Gruppe 3: NUMA-Knoten 13 bis 16, CPUs 192 bis 255  
  
#### <a name="a-setting-affinity-to-all-cpus-in-groups-0-and-2"></a>A. Festlegen der Affinität für alle CPUs in den Gruppen 0 und 2  
Im folgenden Beispiel wird die Affinität für alle CPUs in den Gruppen 0 und 2 festgelegt.  
  
```sql  
ALTER SERVER CONFIGURATION   
SET PROCESS AFFINITY CPU=0 TO 63, 128 TO 191;  
```  
  
#### <a name="b-setting-affinity-to-all-cpus-in-numa-nodes-0-and-7"></a>B. Festlegen der Affinität für alle CPUs in den NUMA-Knoten 0 und 7  
Im folgenden Beispiel wird veranschaulicht, wie die CPU-Affinität lediglich auf die Knoten `0` und `7` festgelegt wird.  
  
```sql  
ALTER SERVER CONFIGURATION   
SET PROCESS AFFINITY NUMANODE=0, 7;  
```  
  
#### <a name="c-setting-affinity-to-cpus-60-through-200"></a>C. Festlegen der Affinität für die CPUs 60 bis 200  
Im folgenden Beispiel wird die Affinität für die CPUs 60 bis 200 festgelegt.  
  
```sql  
ALTER SERVER CONFIGURATION   
SET PROCESS AFFINITY CPU=60 TO 200;  
```  
  
#### <a name="d-setting-affinity-to-cpu-0-on-a-system-that-has-two-cpus"></a>D: Festlegen der Affinität auf einem System, das über zwei CPUs verfügt, für CPU 0  
Im folgenden Beispiel wird die Affinität auf einem Computer, der über zwei CPUs verfügt, auf `CPU=0` festgelegt. Vor Ausführung der folgenden Anweisung ist die interne Affinitätsbitmaske 00.  
  
```sql  
ALTER SERVER CONFIGURATION SET PROCESS AFFINITY CPU=0;  
```  
  
#### <a name="e-setting-affinity-to-auto"></a>E. Festlegen der Affinität auf AUTO  
Im folgenden Beispiel wird die Affinität auf `AUTO` festgelegt.  
  
```sql  
ALTER SERVER CONFIGURATION  
SET PROCESS AFFINITY CPU=AUTO;  
```  
  
###  <a name="setting-diagnostic-log-options"></a><a name="Diagnostic"></a> Setting diagnostic log options  
  
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]).    
  
Die Beispiele in diesem Abschnitt veranschaulichen, wie die Werte für die Diagnoseprotokolloption festgelegt werden.  
  
#### <a name="a-starting-diagnostic-logging"></a>A. Starten der Diagnoseprotokollierung  
Im folgenden Beispiel wird die Protokollierung von Diagnosedaten gestartet.  
  
```sql  
ALTER SERVER CONFIGURATION SET DIAGNOSTICS LOG ON;  
```  
  
#### <a name="b-stopping-diagnostic-logging"></a>B. Beenden der Diagnoseprotokollierung  
Im folgenden Beispiel wird die Protokollierung von Diagnosedaten beendet.  
  
```sql  
ALTER SERVER CONFIGURATION SET DIAGNOSTICS LOG OFF;  
```  
  
#### <a name="c-specifying-the-location-of-the-diagnostic-logs"></a>C. Angeben des Speicherorts für die Diagnoseprotokolle  
Im folgenden Beispiel wird der Speicherort für die Diagnoseprotokolle auf den angegebenen Dateipfad festgelegt.  
  
```sql  
ALTER SERVER CONFIGURATION  
SET DIAGNOSTICS LOG PATH = 'C:\logs';  
```  
  
#### <a name="d-specifying-the-maximum-size-of-each-diagnostic-log"></a>D: Angeben der maximalen Größe jedes Diagnoseprotokolls  
Im folgenden Beispiel wird die maximale Größe jedes Diagnoseprotokolls auf 10 Megabytes festgelegt.  
  
```sql  
ALTER SERVER CONFIGURATION   
SET DIAGNOSTICS LOG MAX_SIZE = 10 MB;  
```  
  
###  <a name="setting-failover-cluster-properties"></a><a name="Failover"></a> Festlegen der Failoverclustereigenschaften  
  
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]).   
  
Im folgenden Beispiel wird veranschaulicht, wie die Werte für die Ressourceneigenschaften des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Failoverclusters festgelegt werden.  
  
#### <a name="a-specifying-the-value-for-the-healthchecktimeout-property"></a>A. Angeben des Werts für die HealthCheckTimeout-Eigenschaft  
Im folgenden Beispiel wird die Option `HealthCheckTimeout` auf 15.000 Millisekunden (15 Sekunden) festgelegt.  
  
```sql  
ALTER SERVER CONFIGURATION   
SET FAILOVER CLUSTER PROPERTY HealthCheckTimeout = 15000;  
```  
  
###  <a name="b-changing-the-cluster-context-of-an-availability-replica"></a><a name="ChangeClusterContextExample"></a> B. Ändern des Clusterkontexts eines Verfügbarkeitsreplikats  
Im folgenden Beispiel wird der HADR-Clusterkontext der Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] geändert. Im Beispiel wird der vollständige Clusterobjektname, `clus01`, angegeben, um den Ziel-WSFC-Cluster `clus01.xyz.com` anzugeben.  
  
```sql  
ALTER SERVER CONFIGURATION SET HADR CLUSTER CONTEXT = 'clus01.xyz.com';  
```  
  
### <a name="setting-buffer-pool-extension-options"></a>Festlegen der Pufferpoolerweiterungsoptionen  
  
####  <a name="a-setting-the-buffer-pool-extension-option"></a><a name="BufferPoolExtension"></a> A. Festlegen der Pufferpoolerweiterungsoption  
  
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]).    
  
Im folgenden Beispiel wird die Pufferpoolerweiterungsoption aktiviert, und es werden Name und Größe der Datei angegeben.  
  
```sql  
ALTER SERVER CONFIGURATION   
SET BUFFER POOL EXTENSION ON  
    (FILENAME = 'F:\SSDCACHE\Example.BPE', SIZE = 50 GB);  
```  
  
#### <a name="b-modifying-buffer-pool-extension-parameters"></a>B. Ändern von Pufferpoolerweiterungsparametern  
Im folgenden Beispiel wird die Größe einer Pufferpoolerweiterungsdatei geändert. Die Pufferpoolerweiterungsoption muss deaktiviert werden, bevor einer der Parameter geändert wird.  
  
```sql  
ALTER SERVER CONFIGURATION   
SET BUFFER POOL EXTENSION OFF;  
GO  
EXEC sp_configure 'max server memory (MB)', 12000;  
GO  
RECONFIGURE;  
GO  
ALTER SERVER CONFIGURATION  
SET BUFFER POOL EXTENSION ON  
    (FILENAME = 'F:\SSDCACHE\Example.BPE', SIZE = 60 GB);  
GO   
```  

### <a name="setting-in-memory-database-options"></a><a name="MemoryOptimized"></a> Festlegen von In-Memory Database-Optionen

**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]).

#### <a name="a-enable-all-in-memory-database-features-with-default-options"></a>A. Aktivieren aller In-Memory Database-Features mit Standardoptionen

```sql
ALTER SERVER CONFIGURATION SET MEMORY_OPTIMIZED ON;
GO
```

#### <a name="b-enable-memory-optimized-tempdb-metadata-using-the-default-resource-pool"></a>B. Aktivieren speicheroptimierter tempdb-Metadaten unter Verwendung des Standardressourcenpools

```sql
ALTER SERVER CONFIGURATION SET MEMORY_OPTIMIZED TEMPDB_METADATA = ON;
GO
```

#### <a name="c-enable-memory-optimized-tempdb-metadata-with-a-user-defined-resource-pool"></a>C. Aktivieren speicheroptimierter tempdb-Metadaten mit einem benutzerdefinierten Ressourcenpool

```sql
ALTER SERVER CONFIGURATION SET MEMORY_OPTIMIZED TEMPDB_METADATA = ON (RESOURCE_POOL = 'pool_name');
GO
```

#### <a name="d-enable-hybrid-buffer-pool"></a>D: Aktivieren des hybriden Pufferpools

```sql
ALTER SERVER CONFIGURATION SET MEMORY_OPTIMIZED HYBRID_BUFFER_POOL = ON;
GO
```

## <a name="see-also"></a>Weitere Informationen  
[Soft-NUMA &#40;SQL Server&#41;](../../database-engine/configure-windows/soft-numa-sql-server.md)   
[Ändern des HADR-Clusterkontexts der Serverinstanz &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/change-the-hadr-cluster-context-of-server-instance-sql-server.md)   
[sys.dm_os_schedulers &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-schedulers-transact-sql.md)   
[sys.dm_os_memory_nodes &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-nodes-transact-sql.md)   
[sys.dm_os_buffer_pool_extension_configuration &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-buffer-pool-extension-configuration-transact-sql.md)   
[Pufferpoolerweiterung](../../database-engine/configure-windows/buffer-pool-extension.md)  
