---
title: PolyBase-Fehler und mögliche Lösungen
description: PolyBase-Referenz für Fehler und vorgeschlagene Lösungen.
ms.date: 02/17/2021
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
dev_langs:
- TSQL
- XML
f1_keywords:
- PolyBase, monitoring
- PolyBase, performance monitoring
helpviewer_keywords:
- PolyBase, troubleshooting
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.reviewer: ''
monikerRange: '>= sql-server-linux-ver15 || >= sql-server-2016'
ms.openlocfilehash: 463b54aefd36e74318331c90cf2c944734f8a5cc
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101873329"
---
# <a name="polybase-errors-and-possible-solutions"></a>PolyBase-Fehler und mögliche Lösungen

[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

Dieser Artikel enthält allgemeine Fehlerszenarien und Lösungen für PolyBase.

Weitere Informationen zur Überwachung und Problembehandlung für PolyBase finden Sie unter [Überwachung und Problembehandlung für PolyBase](polybase-troubleshooting.md).

Die üblichen Speicherorte von PolyBase-Protokolldateien unter Windows und Linux finden Sie unter [Überwachung und Problembehandlung für PolyBase](polybase-troubleshooting.md#log-file-locations).


## <a name="error-messages-and-possible-solutions"></a>Fehlermeldungen und mögliche Lösungen


### <a name="service-account-change"></a>Änderung des Dienstkontos

Beispiel für Fehlermeldung:

> 107035: Fehler bei DMS-Autorisierung, da [DOMÄNE\Benutzer] nicht Mitglied der Gruppe [PdwDataMovementAccess] ist <BR>
> 107017: Ungültiger DMS-Steuerungsheader:

Dieser Fehler ist wahrscheinlich auf eine Änderung des PolyBase-Dienstkontos zurückzuführen. Deinstallieren Sie das PolyBase-Feature, und installieren Sie es erneut, um die Dienstkonten für die PolyBase-Engine und den PolyBase-Datenverschiebungsdienst zu ändern.


### <a name="data-movement-service-permissions-errors"></a>Berechtigungsfehler für Datenverschiebungsdienst

Weitere Informationen zur Behandlung und Behebung von Berechtigungsproblemen mit dem Datenverschiebungsdienst finden Sie unter [PolyBase Service Account Permissions and Common Errors Observed When They Are Missing](https://techcommunity.microsoft.com/t5/sql-server-support/polybase-service-account-permissions-and-common-errors-observed/ba-p/2112711) (PolyBase-Dienstkontoberechtigungen und häufige Fehler bei fehlenden Berechtigungen).


### <a name="windows-authentication-failure"></a>Windows-Authentifizierungsfehler

Weitere Informationen zur Behandlung und Behebung von Berechtigungsproblemen im Zusammenhang mit einem Fehler bei der Windows-Authentifizierung finden Sie unter [PolyBase Service Account Permissions and Common Errors Observed When They Are Missing](https://techcommunity.microsoft.com/t5/sql-server-support/polybase-service-account-permissions-and-common-errors-observed/ba-p/2112711) (PolyBase-Dienstkontoberechtigungen und häufige Fehler bei fehlenden Berechtigungen).


### <a name="cannot-execute-the-query-remote-query"></a>Die Abfrage „Remoteabfrage“ kann nicht ausgeführt werden 

Beispiel für Fehlermeldung:

> Meldung 7320, Ebene 16, Status 110, Zeile 14<BR>
> Die Abfrage „Remoteabfrage“ kann für den OLE DB-Anbieter SQLNCLI11 für den Verbindungsserver „(null)“ nicht ausgeführt werden. Abfrage abgebrochen: Beim Lesen einer externen Quelle wurde der maximale Ablehnungsschwellenwert (0 Zeilen) erreicht: 1 abgelehnte Zeile von insgesamt 1 verarbeiteten Zeile.
(/nation/sensors.ldjson.txt)Ordnungszahl der Spalte: 0, Erwarteter Datentyp: INT, Beanstandeter Wert: {"id":"S2740036465E2B","time":"2016-02-26T16:59:02.9300000Z","temp":23.3,"hum":0.77,"wind":17,"press":1032,"loc":[-76.90914996169623,38.8929314364726]} (Spaltenkonvertierungsfehler), Fehler: Fehler beim Konvertieren des Datentyps NVARCHAR in INT.

Beachten Sie, dass es Ableitungen dieses Fehlers geben kann. Der Name der ersten abgelehnten Datei wird in SQL Server Management Studio (SSMS) mit beanstandeten Datentypen oder Werten angezeigt.

**Mögliche Ursache:**  
Der Grund dieses Fehlers ist, dass jede Datei ein anderes Schema hat. Die PolyBase-DDL der externen Tabelle liest, wenn sie auf ein Verzeichnis zeigt, rekursiv alle Dateien in diesem Verzeichnis. Wenn eine Spalte oder ein Datentyp nicht übereinstimmt, wird diese Fehlermeldung möglicherweise in SSMS angezeigt.

**Mögliche Lösung:**  
Wenn die Daten für jede Tabelle aus einer einzelnen Datei stammen, verwenden Sie den Dateinamen im Abschnitt LOCATION, dem das Verzeichnis der externen Dateien vorangestellt ist. Wenn es pro Tabelle mehrere Dateien gibt, legen Sie jeden Satz von Dateien in verschiedenen Verzeichnissen in Azure Blob Storage ab. Verweisen Sie LOCATION auf das Verzeichnis statt auf eine bestimmte Datei. Diese Lösung wird empfohlen.

**Beispiel:**  

```sqlsyntax
Create External Table foo
(col1 int)WITH (LOCATION='/bar/foobar.txt',DATA_SOURCE…); OR
Create External Table foo
(col1 int) WITH (LOCATION = '/bar/', DATA_SOURCE…);
```


### <a name="kerberos-support"></a>Kerberos-Unterstützung

SQL Server ist für den Zugriff auf einen unterstützten Hadoop-Cluster konfiguriert. Die Kerberos-Sicherheit wird in Hadoop-Clustern nicht erzwungen.

Wenn Sie Daten in der externen Tabelle auswählen, wird der folgende Fehler zurückgegeben:

> Meldung 105019, Ebene 16, Status 1, Zeile 55<BR>
> Zugriff auf EXTERNE TABELLE aufgrund eines internen Fehlers fehlgeschlagen: 'Java-Ausnahme bei Aufruf von HdfsBridge_Connect ausgelöst: Fehler [LoginClass kann nicht instanziiert werden] beim Zugriff auf externe Datei aufgetreten.'<BR>
> Meldung 7320, Ebene 16, Status 110, Zeile 55<BR>
> Die Abfrage „Remoteabfrage“ kann für den OLE DB-Anbieter SQLNCLI11 für den Verbindungsserver „(null)“ nicht ausgeführt werden. Zugriff auf EXTERNE TABELLE aufgrund eines internen Fehlers fehlgeschlagen: 'Java-Ausnahme bei Aufruf von HdfsBridge_Connect ausgelöst: Fehler [LoginClass kann nicht instanziiert werden] beim Zugriff auf externe Datei aufgetreten.'

Die Untersuchung des DWEngine-Serverprotokolls ergibt folgenden Fehler:

> [Thread:16432] [EngineInstrumentation:EngineQueryErrorEvent] (Fehler, hoch):<BR>
> Zugriff auf EXTERNE TABELLE aufgrund eines internen Fehlers fehlgeschlagen: 'Java-Ausnahme bei Aufruf von HdfsBridge_Connect ausgelöst: Fehler [com.microsoft.polybase.client.KerberosSecureLogin] beim Zugriff auf externe Datei aufgetreten.'
Microsoft.SqlServer.DataWarehouse.Common.ErrorHandling.MppSqlException: Zugriff auf EXTERNE TABELLE aufgrund eines internen Fehlers fehlgeschlagen: 'Java-Ausnahme bei Aufruf von HdfsBridge_Connect ausgelöst: Fehler [com.microsoft.polybase.client.KerberosSecureLogin] beim Zugriff auf externe Datei aufgetreten.' ---> Microsoft.SqlServer.DataWarehouse.DataMovement.Common.ExternalAccess.HdfsAccessException: Java-Ausnahme bei Aufruf von HdfsBridge_Connect ausgelöst: Fehler [com.microsoft.polybase.client.KerberosSecureLogin] beim Zugriff auf externe Datei aufgetreten.

**Mögliche Ursache:**  
Kerberos ist im Hadoop-Cluster nicht aktiviert, aber die Kerberos-Sicherheit ist in der Datei „core-site.xml“, „yarn-site.xml“ oder „hdfs-site.xml“ aktiviert, die sich standardmäßig unter „Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn\Polybase\Hadoop\conf“ befindet. Unter Linux befinden sich die Dateien standardmäßig in /var/opt/mssql/binn/polybase/hadoop/conf/.

**Mögliche Lösung:**  
Kommentieren Sie die Kerberos-Sicherheitsinformationen in den oben genannten Dateien aus.

Weitere Informationen zur Problembehandlung bei PolyBase und Kerberos finden Sie unter [Problembehandlung: PolyBase-Kerberos-Konnektivität](polybase-troubleshoot-connectivity.md).

### <a name="internal-query-processor-error"></a>Interner Fehler des Abfrageprozessors

Beim Abfragen einer externen Tabelle wird der folgende Fehler zurückgegeben:

> Meldung 8680, Ebene 17, Status 5, Zeile 118<BR>
> Interner Fehler des Abfrageprozessors: Unerwarteter Fehler beim Abfrageprozessor bei der Verarbeitung einer Remoteabfragephase.

Das DWEngine-Serverprotokoll enthält die folgenden Meldungen:

> [Thread:5216] [ControlNodeMessenger:ErrorEvent] (Fehler, hoch): ***** DMS-System hat getrennte Knoten :<BR>
> [Thread:5216] [ControlNodeMessenger:ErrorEvent] (Fehler, hoch): ***** DMS-System hat getrennte Knoten :<BR>
> [Thread:5216] [ControlNodeMessenger:ErrorEvent] (Fehler, hoch): ***** DMS-System hat getrennte Knoten :<BR>

**Mögliche Ursache:**  
Die Ursache dieses Fehlers könnte sein, dass SQL Server nach dem Konfigurieren von PolyBase nicht neu gestartet wurde.

**Mögliche Lösung:**  
Starten Sie SQL Server neu. Prüfen Sie das DWEngine-Serverprotokoll, um sicherzustellen, dass nach dem Neustart keine DMS-Verbindungsabbrüche erfolgen.


### <a name="user-needed-for-hdfs-access"></a>Benutzer für HDFS-Zugriff erforderlich

**Szenario:**  
SQL Server ist mit einem ungesicherten Hadoop-Cluster verbunden (Kerberos ist nicht aktiviert). PolyBase ist für die Übertragung der Berechnung an den Hadoop-Cluster konfiguriert.

**Beispielabfrage:**  

```sql
select count(*) from foo WITH (FORCE EXTERNALPUSHDOWN);
```

Es wird eine Fehlermeldung ähnlich der folgenden zurückgegeben: 

> Meldung 105019, Ebene 16, Status 1, Zeile 1<BR>
> Zugriff auf EXTERNE TABELLE aufgrund eines internen Fehlers fehlgeschlagen: 'Java-Ausnahme bei Aufruf von obSubmitter_PollJobStatus ausgelöst: Fehler [java.net.ConnectException: Aufruf von big1506sql2016/172.16.1.4 an 0.0.0.0:10020 bei Verbindungsausnahme fehlgeschlagen: java.net ConnectException: Verbindung verweigert: keine weiteren Informationen. Weitere Details finden Sie unter: http://wiki.apache.org/hadoop/ConnectionRefused ] beim Zugriff auf externe Datei aufgetreten.'<BR>
> Der OLE DB-Anbieter SQLNCLI11 für Verbindungsserver „(null)“ hat die Meldung „Nicht spezifizierter Fehler“ zurückgegeben.<BR>
> Meldung 7421, Ebene 16, Status 2, Zeile 1<BR>
> Das Rowset kann nicht vom OLE DB-Anbieter 'SQLNCLI11 für den Verbindungsserver „(null)“ abgerufen werden. .<BR>

Hadoop Yarn-Protokollfehler:
> Fehler beim Einrichten des Auftrags: org.apache.hadoop.security.AccessControlException: Berechtigung verweigert: user=pdw_user, access=WRITE, inode="/user":hdfs:hdfs:drwxr-xr-x at org.apache.hadoop.hdfs.server.namenode.FSPermissionChecker.checkFsPermission(FSPermissionChecker.java:265) bei org.apache.hadoop.hdfs.server.namenode.FSPermissionChecker.check(FSPermissionChecker.java:251) bei org.apache.hadoop.hdfs.server.namenode.FSPermissionChecker.check(FSPermissionChecker.java:232) org.apache.hadoop.hdfs.server.namenode.FSPermissionChecker.checkPermission(FSPermissionChecker.java:176) bei org.apache.hadoop.hdfs.server.namenode.FSNamesystem.checkPermission(FSNamesystem.java:5525)

**Mögliche Ursache:**  
Wenn Kerberos deaktiviert ist, verwendet PolyBase pdw_user als Benutzer für den Zugriff auf HDFS und das Senden von MapReduce-Aufträgen.

**Mögliche Lösung:**  
Erstellen Sie pdw_user in Hadoop, und erteilen Sie diesem Benutzer die erforderlichen Berechtigungen für die Verzeichnisse, die während der MapReduce-Verarbeitung verwendet werden. Stellen Sie außerdem sicher, dass pdw_user der Besitzer des HDFS-Verzeichnisses „/user/pdw_user“ ist.

Im Folgenden finden Sie ein Beispiel für das Erstellen des Basisverzeichnisses und das Zuweisen von Berechtigungen zu pdw_user:

```console
sudo -u hdfs hadoop fs -mkdir /user/pdw_user
sudo -u hdfs hadoop fs -chown pdw_user /user/pdw_user
```

Stellen Sie danach sicher, dass pdw_user Lese-, Schreib- und Ausführungsberechtigungen für das Verzeichnis „/user/pdw_user“ hat. Stellen Sie sicher, dass das Verzeichnis „/tmp“ die Berechtigung 777 hat.

Weitere Informationen zur Problembehandlung bei PolyBase und Kerberos finden Sie unter [Problembehandlung: PolyBase-Kerberos-Konnektivität](polybase-troubleshoot-connectivity.md).

### <a name="java-memory-error-due-to-utf-8"></a>Java-Arbeitsspeicherfehler aufgrund von UTF-8

**Szenario:**  
SQL Server PolyBase wird mit Hadoop-Cluster oder Azure Blob Storage eingerichtet. SELECT-Abfragen schlagen mit folgendem Fehler fehl:

> Meldung 106000, Ebene 16, Status 1, Zeile 1<BR>
> Java-Heapspeicher<BR>

**Mögliche Ursache:**  
Unzulässige Eingaben können den Java-Fehler „Zu wenig Arbeitsspeicher“ verursachen. Die Datei hat möglicherweise nicht das UTF-8-Format. DMS versucht, die gesamte Datei als eine Zeile zu lesen, da das Zeilentrennzeichen nicht decodiert werden kann, was zu einem Java- Heapspeicherfehler führt.

**Mögliche Lösung:**  
Konvertieren Sie die Datei in das UTF-8-Format, da PolyBase derzeit das UTF-8-Format für Dateien mit Texttrennzeichen benötigt.

### <a name="hadoop-connectivity-configuration"></a>Konfiguration der Hadoop-Konnektivität

Das Konfigurieren von SQL Server PolyBase zum Herstellen einer Verbindung mit Azure Blob Storage gibt in SQL Server die folgende Fehlermeldung zurück:

> Meldung 105019, Ebene 16, Status 1, Zeile 74<BR>
> Zugriff auf EXTERNE TABELLE aufgrund eines internen Fehlers fehlgeschlagen: 'Java-Ausnahme bei Aufruf von HdfsBridge_Connect ausgelöst: Fehler [Kein Dateisystem für Schema: wasbs] beim Zugriff auf externe Datei aufgetreten.'<BR>

**Mögliche Ursache:**  
Die Hadoop-Konnektivität ist nicht auf den Konfigurationswert für den Zugriff auf Azure Blob Storage festgelegt.

**Mögliche Lösung:**  
Legen Sie die Hadoop-Konnektivität auf einen Wert (vorzugsweise 7) fest, der Azure Blob Storage unterstützt, und starten Sie SQL Server neu. Eine Liste der Konnektivitätswerte und unterstützten Typen finden Sie unter [Konfiguration der PolyBase-Netzwerkkonnektivität](../../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md#arguments).


### <a name="create-table-as-select-error"></a>Fehler CREATE TABLE AS SELECT

**Szenario:**  
Der Versuch, Daten in Azure Blob Storage oder das Hadoop-Dateisystem unter Verwendung von PolyBase mit der Syntax CREATE EXTERNAL TABLE AS SELECT (CETAS) aus SQL Server zu exportieren, schlägt mit der folgenden Fehlermeldung fehl:

> Meldung 156, Ebene 15, Status 1, Zeile 177<BR>
> Falsche Syntax in der Nähe des Schlüsselworts WITH.<BR>
> Meldung 319, Ebene 15, Status 1, Zeile 177<BR>
> Falsche Syntax in der Nähe des WITH-Schlüsselworts. Falls diese Anweisung ein allgemeiner Tabellenausdruck, eine XMLNAMESPACES-Klausel oder eine CHANGE TRACKING CONTEXT-Klausel ist, muss die vorherige Anweisung mit einem Semikolon abgeschlossen werden.<BR>

**Mögliche Ursache:**  
Beim Exportieren von Daten nach Hadoop oder Azure Blob Storage über PolyBase werden nur die Daten exportiert, nicht die Spaltennamen (Metadaten), wie im CREATE EXTERNAL TABLE-Befehl definiert.

**Mögliche Lösung:**  
Erstellen Sie zuerst die externe Tabelle, und verwenden Sie dann INSERT INTO SELECT für den Export an den externen Speicherort. Ein Codebeispiel finden Sie unter [PolyBase-Abfrageszenarien](polybase-queries.md#export-data).


### <a name="create-external-table-from-azure-blob-storage-fails"></a>Fehler beim Erstellen einer externen Tabelle aus Azure Blob Storage

**Szenario:**  
SQL DW ist für den Import von Daten aus Azure Blob Storage eingerichtet. Das Erstellen einer externen Tabelle schlägt mit der folgenden Meldung fehl.

> Meldung 105019, Ebene 16, Status 1, Zeile 34<BR>
> Zugriff auf EXTERNE TABELLE aufgrund eines internen Fehlers fehlgeschlagen: 'Java-Ausnahme bei Aufruf von HdfsBridge_IsDirExist ausgelöst. Java-Ausnahme message:com.microsoft.azure.storage.StorageException: Server konnte die Anforderung nicht authentifizieren. Stellen Sie sicher, dass der Wert des Headers „Authorization“ ordnungsgemäß gebildet wird, einschließlich der Signatur: Fehler [com.microsoft.azure.storage.StorageException: Server konnte die Anforderung nicht authentifizieren. Stellen Sie sicher, dass der Wert des Headers „Authorization“ ordnungsgemäß gebildet wird, einschließlich der Signatur.] beim Zugriff auf die externe Datei aufgetreten.'<BR>

**Mögliche Ursache:**  
Beim Erstellen der datenbankbezogenen Anmeldeinformationen wurde ein falscher Azure Storage-Schlüssel verwendet.

**Mögliche Lösung:**  
Löschen Sie alle zugehörigen Objekte (also Datenquelle, Dateiformat) und dann die datenbankbezogene Anmeldeinformation, und erstellen Sie sie mit dem richtigen Speicherschlüssel neu.


### <a name="kerberos-configuration-capitalization"></a>Großschreibung bei Kerberos-Konfiguration

**Szenario:**  
SQL Server ist mit einem Kerberos-fähigen Cloudera-Cluster eingerichtet. SQL Server wurde nach allen Konfigurationsänderungen neu gestartet. Die PolyBase-Engine und der PolyBase-Datenverschiebungsdienst werden nach dem Neustart ausgeführt. Die folgenden Fehlermeldungen werden zurückgegeben:

Datenquelle ohne Speicherort für Auftragsnachverfolgung konfiguriert:  
> org.apache.hadoop.fs.FileSystem: Anbieter org.apache.hadoop.fs.viewfs.ViewFileSystem konnte nicht instanziiert werden

Datenquelle mit Speicherort für Auftragsnachverfolgung konfiguriert:  
> Fehler [Kerberos-Bereich kann nicht abgerufen werden] beim Zugriff auf externe Datei aufgetreten

**Mögliche Ursache:**  
Der Wert für die Eigenschaft „hadoop.security.authentication“ in der Datei „Coresite.xml“ lautet „kerberos“.

**Mögliche Lösung:**  
Die Eigenschaft „hadoop.security.authentication“ in Coresite.xml muss als Wert KERBEROS (in Großbuchstaben) enthalten. 

Weitere Informationen zur Problembehandlung bei PolyBase und Kerberos finden Sie unter [Problembehandlung: PolyBase-Kerberos-Konnektivität](polybase-troubleshoot-connectivity.md).

### <a name="mapred-sitexml-missing-needed-values"></a>In „Mapred-site.xml“ fehlen benötigte Werte

**Szenario:**  
SQL Server oder APS ist mit unterstütztem HDP-Cluster eingerichtet. Abfragen, die PUSHDOWN nicht benötigen, funktionieren, schlagen aber mit folgender Fehlermeldung fehl, wenn der Hinweis FORCE PUSHDOWN verwendet wird:

> Meldung 7320, Ebene 16, Status 110, Zeile 35<BR>
> Die Abfrage „Remoteabfrage“ kann für den OLE DB-Anbieter SQLNCLI11 für den Verbindungsserver „(null)“ nicht ausgeführt werden. Zugriff auf EXTERNE TABELLE aufgrund eines internen Fehlers fehlgeschlagen: 'Java-Ausnahme bei Aufruf von JobSubmitter_PollJobStatus ausgelöst: Fehler [org.apache.hadoop.ipc.RemoteException(java.lang.NullPointerException): java.lang.NullPointerException<BR>
> bei org.apache.hadoop.mapreduce.v2.hs.HistoryClientService$HSClientProtocolHandler.getTaskAttemptCompletionEvents(HistoryClientService.java:277)<BR>
> bei org.apache.hadoop.mapreduce.v2.api.impl.pb.service.MRClientProtocolPBServiceImpl.getTaskAttemptCompletionEvents(MRClientProtocolPBServiceImpl.java:173)<BR>
> bei org.apache.hadoop.yarn.proto.MRClientProtocol$MRClientProtocolService$2.callBlockingMethod(MRClientProtocol.java:283)<BR>
> bei org. Apache. Hadoop. IPC. protobufrpcengine $ Server $ protobufrpcinvoker. Aufruf (protobufrpcengine. Java: 619)<BR>
> bei org.apache.hadoop.ipc.RPC$Server.call(RPC.java:962)<BR>
> bei org.apache.hadoop.ipc.Server$Handler$1.run(Server.java:2127)<BR>
> bei org.apache.hadoop.ipc.Server$Handler$1.run(Server.java:2123)<BR>
> bei java.security.AccessController.doPrivileged(Native Methode)<BR>
> bei javax.security.auth.Subject.doAs(Subject.java:415)<BR>
> bei org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1671)<BR>
> bei org.apache.hadoop.ipc.Server$Handler.run(Server.java:2121)<BR>
> ] beim Zugriff auf die externe Datei aufgetreten.'<BR>

**Mögliche Ursache:**  
In der Datei „Mapred-site.xml“ fehlen einige benötigte Werte, die auf Zwischen- und Endergebnisse prüfen.

**Mögliche Lösung:**  
Fügen Sie die folgenden Eigenschaften hinzu, und ordnen Sie die ordnungsgemäßen Werte entsprechend der Darstellung in Ambari in der Datei „mapred-site.xml“ in SQL Server zu. 

```xml
<property>
<name>yarn.app.mapreduce.am.staging-dir</name>
<value>/user</value>
</property>
<property>
<name>mapreduce.jobhistory.done-dir</name>
<value>/mr-history/done</value>
</property>
<property>
<name>mapreduce.jobhistory.intermediate-done-dir</name>
<value>/mr-history/tmp</value>
</property>
```

### <a name="configuring-access-by-hostname"></a>Konfigurieren des Zugriffs nach Hostname 

**Szenario:**  
SQL Server ist für den Zugriff auf einen unterstützten Hadoop-Cluster eingerichtet. Beim Erstellen einer externen Tabelle wird einer der folgenden Fehler zurückgegeben:

> Die Abfrage „Remoteabfrage“ kann für den OLE DB-Anbieter SQLNCLI11 für den Verbindungsserver „(null)“ nicht ausgeführt werden. 110802;Es ist ein interner DMS-Fehler aufgetreten, der zum Fehlschlagen dieses Vorgangs geführt hat. Details: Ausnahme: Microsoft.SqlServer.DataWarehouse.DataMovement.Workers.DmsSqlNativeException, Message: SqlNativeBufferReader.Run, Fehler in OdbcExecuteQuery: SqlState: 42000, NativeError: 8680, 'Fehler beim Aufruf von: SQLExecDirect(this->GetHstmt(), (SQLWCHAR *)statementText, SQL_NTS), SQL-Rückgabecode: -1 | SQL-Fehlerinformation: SrvrMsgState: 26, SrvrSeverity: 17, Fehler <1>: ErrorMsg: [Microsoft][ODBC Driver 13 for SQL Server][SQL Server]Interner Fehler des Abfrageprozessors: Im Abfrageprozessor wurde ein unerwarteter Fehler bei der Verarbeitung einer Remoteabfragephase festgestellt. | Fehler beim Aufrufen von: pReadConn->ExecuteQuery(statementText, bufferFormat) | Status: FFFF, Nummer: 24, aktive Verbindungen: 8', Verbindungszeichenfolge: Driver={pdwodbc};APP=RCSmall-DmsNativeReader:WAD1D16HD2001\mpdwsvc (3600)-ODBC-PoolId1433;Trusted_Connection=yes;AutoTranslate=no;Server=\\.\pipe\sql\query<BR>

> [Thread:30544] [AbstractReaderWorker:ErrorEvent] (Fehler, hoch): QueryId QID2433 PlanId 6c3a4551-e54c-4c06-a5ed-a8733edac691 StepId 7:<BR>
> Block konnte nicht abgerufen werden: BP-1726738607-192.168.225.121-1443123675290:blk_1159687047_86196509 file=/user/hive/warehouse/u_data/000000_0<BR>
> Microsoft.SqlServer.DataWarehouse.Common.ErrorHandling.MppSqlException: Block konnte nicht abgerufen werden: BP-1726738607-192.168.225.121-1443123675290:blk_1159687047_86196509 file=/user/hive/warehouse/u_data/000000_0<BR>
> bei Microsoft.SqlServer.DataWarehouse.DataMovement.Common.ExternalAccess.HdfsBridgeReadAccess.Read(MemoryBuffer buffer, Boolean& isDone)<BR>
> bei Microsoft.SqlServer.DataWarehouse.DataMovement.Workers.DataReader.ExternalMoveBufferReader.Read()<BR>
> bei Microsoft.SqlServer.DataWarehouse.DataMovement.Workers.ExternalMoveReaderWorker.ReadAndSendData()<BR>
> bei Microsoft.SqlServer.DataWarehouse.DataMovement.Workers.ExternalMoveReaderWorker.Execute(Object status)<BR>

**Mögliche Ursache:**  
Diese Fehlermeldung kann auftreten, wenn der Hadoop-Cluster in einer Konfiguration eingerichtet ist, in der die Datenknoten außerhalb des Clusters nur über den Hostnamen und nicht über die IP-Adresse zugänglich sind.

**Mögliche Lösung:**  
Fügen Sie Folgendes zur Datei „hdfs-site.xml“ auf Clientseite (SQL Server) hinzu. Durch diese Konfiguration wird der Namensknoten gezwungen, einen URI für die Datenknoten mit dem Hostnamen anstelle der internen IP-Adresse zurückzugeben.

```xml
<property>
<name>dfs.client.use.datanode.hostname</name>
<value>true</value>
</property>
```

### <a name="folder-organization-forces-excess-memory-overhead"></a>Ordnerorganisation erzwingt übermäßigen Overhead des Arbeitsspeichers

**Szenario:**  
SQL Server wendet eine PolyBase-Abfrage auf ein Verzeichnis mit einer großen Anzahl von Dateien (>30.000 Dateien unter dem Verzeichnispfad rekursiv) aus, und eine der folgenden Fehlermeldungen wird zurückgegeben:

> Meldung 105019, Ebene 16, Status 1, Zeile 1<BR>
> Zugriff auf EXTERNE TABELLE aufgrund eines internen Fehlers fehlgeschlagen: 'Java-Ausnahme bei Aufruf von HdfsBridge_GetFileNameByIndex ausgelöst. Java-Ausnahmemeldung: Grenzwert für GC-Overhead überschritten: Beim Zugriff auf eine externe Datei ist der Fehler [Grenzwert für GC-Overhead überschritten] aufgetreten.'<BR>

> Meldung 105019, Ebene 16, Status 1, Zeile 1<BR>
> Zugriff auf EXTERNE TABELLE aufgrund eines internen Fehlers fehlgeschlagen: 'Java-Ausnahme bei Aufruf von HdfsBridge_GetDirectoryFiles ausgelöst. Java-Ausnahmemeldung: Java-Heapspeicher: Fehler [ Java-Heapspeicher] bei Zugriff auf externe Datei aufgetreten.'

**Mögliche Ursache:**  
Bei der Verarbeitung eines Pfads zählt PolyBase alle Dateien unter diesem Pfad auf. Es gibt einen festen Arbeitsspeicheroverhead in Verbindung mit der Datenstruktur, die zur Darstellung der Dateien verwendet wird. Bei einer großen Anzahl von Dateien macht sich dieser Overhead bemerkbar und kann schließlich den gesamten der JVM zur Verfügung stehenden Arbeitsspeicher beanspruchen.

**Mögliche Lösung:**  
Ordnen Sie die Daten in mehreren Verzeichnissen neu an, sodass jedes Verzeichnis eine Teilmenge von Dateien enthält. Zerlegen Sie dann die Abfrage in mehrere, die jeweils auf einen Teil des ursprünglichen Pfads angewendet werden, und materialisieren Sie die Tabellen als SQL Server-Tabellen (ehe Sie sie verknüpfen).

Beispiel: Angenommen, die Daten Ihrer externen Tabelle befinden sich an folgendem Speicherort: Orders/file1.txt,...,file30000.txt.

Ändern Sie das Layout so, dass die Daten in einer herkömmlichen Dateipartitionsstruktur in „Orders/*JJJJ*/*MM*/*TT*/file1.txt“ angeordnet sind.
Verweisen Sie Ihre externe Tabelle auf einen niedrigeren Verzeichnispfad wie Monat(MM) oder Tag(TT), und importieren Sie die Dateien stückweise in SQL Server-Tabellen. Fügen Sie sie dann als Teil einer Tabelle hinzu.
Selbst wenn Sie anfangs die richtige Verzeichnisstruktur hatten, befolgen Sie Schritt 2, um mit so vielen Dateien arbeiten zu können, ohne dass der JVM-Arbeitsspeicher knapp wird.


### <a name="unexpected-characters-in-configuration-files"></a>Unerwartete Zeichen in Konfigurationsdateien

**Szenario:**  
Die Einrichtung von SQL Server oder APS mit einem Hadoop-Cluster, was die Änderung von „yarn-site.xml“, „hdfs-site.xml“ und anderen Konfigurationsdateien umfasst. Die folgende SQL Server-Fehlermeldung wird beobachtet:

> Meldung 105019, Ebene 16, Status 1, Zeile 1<BR>
> Microsoft.SqlServer.DataWarehouse.Common.ErrorHandling.MppSqlException: Zugriff auf EXTERNE TABELLE aufgrund eines internen Fehlers fehlgeschlagen: 'Java-Ausnahme bei Aufruf von HdfsBridge_Connect ausgelöst. Java-Ausnahme message:com.sun.org.apache.xerces.internal.impl.io.MalformedByteSequenceException: Invalid byte 1 of 1-byte UTF-8 sequence.: Fehler [com.sun.org.apache.xerces.internal.impl.io.MalformedByteSequenceException: Invalid byte 1 of 1-byte UTF-8 sequence.] beim Zugriff auf externe Datei aufgetreten.' ---><BR>

**Mögliche Ursache:**  
Dies kann passieren, wenn Sie Text von einer Website oder aus einem Chatfenster kopiert und in Konfigurationsdateien eingefügt haben. Es ist möglich, dass sich in den Konfigurationsdateien unerwünschte/ nicht druckbare Zeichen befinden.

**Mögliche Lösung:**  
Öffnen Sie die Dateien in einem anderen Text-Editor (nicht in Editor), suchen Sie nach diesen Zeichen, und entfernen Sie sie. Starten Sie die erforderlichen Dienste neu. 


## <a name="see-also"></a>Weitere Informationen

[Überwachung und Problembehandlung für PolyBase](polybase-troubleshooting.md)  
[Problembehandlung: PolyBase-Kerberos-Konnektivität](polybase-troubleshoot-connectivity.md)  

