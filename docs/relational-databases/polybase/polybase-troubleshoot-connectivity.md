---
title: 'Problembehandlung: PolyBase-Kerberos-Konnektivität | Microsoft-Dokumentation'
description: Sie können die in PolyBase integrierte interaktive Diagnose verwenden, um Authentifizierungsprobleme bei PolyBase mit einem durch Kerberos geschützten Hadoop-Cluster beheben.
author: alazad-msft
ms.author: alazad
ms.reviewer: mikeray
ms.technology: polybase
ms.devlang: ''
ms.topic: conceptual
ms.date: 10/02/2019
ms.prod: sql
ms.prod_service: polybase, sql-data-warehouse, pdw
monikerRange: '>= sql-server-2016'
ms.openlocfilehash: c1957724f028283c2e3aa6377604726f0c307add
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101838045"
---
# <a name="troubleshoot-polybase-kerberos-connectivity"></a>Problembehandlung: PolyBase-Kerberos-Konnektivität

[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Sie können interaktive, in PolyBase integrierte Diagnosefunktionen verwenden, um Probleme bei der Authentifizierung leichter zu beheben, wenn Sie PolyBase mit einem durch Kerberos gesicherten Hadoop-Cluster verwenden. 

Dieser Artikel dient als Hilfestellung beim Debuggingprozess für solche Probleme mithilfe dieser integrierten Diagnosefunktionen.

> [!TIP]
> Sie müssen nicht unbedingt wie in dieser Anleitung beschrieben vorgehen, sondern können auch den [Tester für HDFS Kerberos](https://github.com/microsoft/sql-server-samples/tree/master/samples/manage/hdfs-kerberos-tester) für HDFS Kerberos-Verbindungen für PolyBase verwenden, wenn beim Erstellen einer externen Tabelle in einem mit Kerberos gesicherten HDFS-Cluster Fehler auftreten.
> Dieses Tool hilft Ihnen dabei, Probleme zu vermeiden, die nicht mit SQL Server in Zusammenhang stehen, damit Sie sich auf die Behebung von Problemen mit dem HDFS Kerberos-Setup konzentrieren können, d. h. mit Fehlkonfigurationen von Benutzernamen bzw. Kennwörtern oder des Kerberos-Clustersetups.      
> Dieses Tool ist vollständig unabhängig von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Es ist als Jupyter Notebook-Instanz verfügbar und erfordert Azure Data Studio.

## <a name="prerequisites"></a>Voraussetzungen

1. [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] RTM CU6/[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 CU3/[!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] oder höher und PolyBase
1. Ein Hadoop-Cluster (Cloudera oder Hortonworks), das durch Kerberos gesicherte wird (Active Directory oder MIT)

## <a name="introduction"></a>Einführung
Diese bietet allgemeine Informationen, um sich mit dem Kerberos-Protokoll vertraut zu machen. Daran sind drei Akteure beteiligt:

1. der Kerberos-Client (SQL Server)
1. gesicherte Ressourcen (HDFS, MR2, YARN, Auftragsverlauf, usw.)
1. das Schlüsselverteilungscenter (in Active Directory als Domänencontroller bezeichnet)

Jede von Hadoop gesicherte Ressource wird im **Schlüsselverteilungscenter (Key Distribution Center, KDC)** mit einem eindeutigen **Dienstprinzipalnamen (Service Principal Name, SPN)** registriert, wenn Kerberos für den Hadoop-Cluster konfiguriert wird. Ziel ist es, dass der Client ein temporäres Benutzerticket, ein sogenanntes **Ticket Granting Ticket (TGT)** , erhält, um ein weiteres temporäres Ticket, ein sogenanntes **Dienstticket (Service Ticket, ST)** vom KDC mit dem gegebenen SPN anfordern zu können.  

Wenn in PolyBase die Authentifizierung mit einer durch Kerberos gesicherten Ressource angefordert wird, kommt es zu folgendem Handshake aus vier Zyklen:

1. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] stellt eine Verbindung mit dem KDC her und ruft ein TGT für den Benutzer ab. Das TGT wird mit dem privaten Schlüssel des KDC verschlüsselt.
1. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ruft die durch Hadoop gesicherte Ressource, HDFS, auf und bestimmt, für welchen SPN sie ein ST benötigt.
1. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] kehrt zum KDC zurück, übergibt das TGT und fordert ein ST an, um auf die gegebene gesicherte Ressource zugreifen zu können. Das ST wird mit dem privaten Schlüssel des gesicherten Diensts verschlüsselt.
1. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] leitet das ST an Hadoop weiter und wird authentifiziert, damit eine Sitzung für diesen Dienst erstellt werden kann.

![Polybase SQL Server](./media/polybase-sqlserver.png)

Probleme bei der Authentifizierung treten bei einem oder mehreren der oben erläuterten Schritte auf. Um das Debuggen zu beschleunigen, hat PolyBase ein integriertes Diagnosetool eingeführt, um die Stelle zu identifizieren, an der die Authentifizierung fehlgeschlagen ist.

## <a name="troubleshooting"></a>Problembehandlung

PolyBase verfügt über folgende Konfigurations-XML-Dateien, die Eigenschaften des Hadoop-Clusters enthalten:

- core-site.xml
- hdfs-site.xml
- hive-site.xml
- jaas.conf
- mapred-site.xml
- yarn-site.xml

Diese Dateien befinden sich unter:

`\[System Drive\]:{install path}\{MSSQL##.INSTANCENAME}\MSSQL\Binn\PolyBase\Hadoop\conf`

Der Standardwert für [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] lautet z. B. `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn\PolyBase\Hadoop\conf`.

Zum Aktualisieren von **core-site.xml** fügen Sie die drei untenstehenden Eigenschaften hinzu. Legen Sie die Werte passend zur Umgebung fest:

```xml
<property>
    <name>polybase.kerberos.realm</name>
    <value>CONTOSO.COM</value>
</property>
<property>
    <name>polybase.kerberos.kdchost</name>
    <value>kerberos.contoso.com</value>
</property>
<property>
    <name>hadoop.security.authentication</name>
    <value>KERBEROS</value>
</property>
```
> [!NOTE]
> Für den Wert der `polybase.kerberos.realm`-Eigenschaft muss Großschreibung verwendet werden.

Die anderen XMLs müssen später auch aktualisiert werden, wenn Weitergabevorgänge gewünscht sind. Wenn jedoch nur diese eine Datei konfiguriert ist, sollte der Zugriff auf das HDFS-Dateisystem zumindest möglich sein.

Das Tool wird unabhängig von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ausgeführt. Das bedeutet, dass es weder ausgeführt noch neu gestartet werden muss, nachdem die Konfigurations-XMLs aktualisiert wurden. Sie können das Tool ausführen, indem Sie die folgenden Befehle auf dem Host mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ausführen:

```cmd
> cd C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn\PolyBase  
> java -classpath ".\Hadoop\conf;.\Hadoop\*;.\Hadoop\HDP2_2\*" com.microsoft.polybase.client.HdfsBridge {Name Node Address} {Name Node Port} {Service Principal} {Filepath containing Service Principal's Password} {Remote HDFS file path (optional)}
```

## <a name="arguments"></a>Argumente

| Argument | BESCHREIBUNG|
| --- | --- |
| *Namenknotenadresse* | Die IP oder der FQDN des Namenknotens. Verweist auf das „LOCATION“-Argument Ihrer T-SQL „CREATE EXTERNAL DATA SOURCE“. Hinweis: In der in SQL Server 2019 enthaltenen Version des Tools muss der IP-Adresse oder dem vollqualifizierten Domänenname (Fully Qualified Domain Name, FQDN) das Präfix *hdfs:\/\/* vorangestellt werden.|
| *Namenknotenport* | Der Port des Namensknotens. Verweist auf das „LOCATION“-Argument Ihrer T-SQL „CREATE EXTERNAL DATA SOURCE“. Zum Beispiel „8020“. |
| *Dienstprinzipal* | Der Administratordienstprinzipal Ihres KDC. Entspricht dem „IDENTITY“-Argument in Ihrer T-SQL-Klausel „`CREATE DATABASE SCOPED CREDENTIAL`“.|
| *Dienstkennwort* | Statt Ihr Kennwort in der Konsole einzugeben, speichern Sie dieses in einer Datei und geben Sie hier den Dateipfad ein. Die Inhalte Ihrer Datei sollten dem „SECRET“-Argument in Ihrer T-SQL-Klausel „`CREATE DATABASE SCOPED CREDENTIAL`“ entsprechen. |
| *Remoter HDFS-Dateipfad (Hadoop Distributed File System) (optional)* | Der Pfad einer vorhandenen Datei, auf die zugegriffen werden soll. Wenn nicht angegeben, wird der Stamm „/“ verwendet. |

## <a name="example"></a>Beispiel

```cmd
java -classpath ".\Hadoop\conf;.\Hadoop\*;.\Hadoop\HDP2_2\*" com.microsoft.polybase.client.HdfsBridge 10.193.27.232 8020 admin_user C:\temp\kerberos_pass.txt
```

Die Ausgabe ist für das erweiterte Debuggen ausführlich. Allerdings gibt es nur vier Hauptprüfpunkte, die beachtet werden müssen, unabhängig davon, ob Sie MIT oder AD verwenden. Sie entsprechen den vier oben erläuterten Schritten. 

Die folgenden Auszüge stammen aus einem KDC von MIT. Am Ende dieses Artikels finden Sie vollständige Beispielausgaben sowohl von MIT als auch von AD.

## <a name="checkpoint-1"></a>Prüfpunkt 1
Es sollte einen Hexdump eines Tickets mit `Server Principal = krbtgt/MYREALM.COM@MYREALM.COM` geben. Dies gibt an, das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] erfolgreich im KDC authentifiziert wurde und ein TGT erhalten hat. Falls dies nicht der Fall ist, liegt das Problem bei [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und dem KDC, nicht bei Hadoop.

PolyBase unterstützt **keine** Vertrauensstellungen zwischen AD und MIT und muss mit dem gleichen KDC wie das Hadoop-Cluster. In derartigen Umgebungen können Sie ein Dienstkonto manuelle erstellen und dieses dann zur Authentifizierung verwenden.

```cmd
|>>> KrbAsReq creating message 
 >>> KrbKdcReq send: kdc=kerberos.contoso.com UDP:88, timeout=30000, number of retries =3, #bytes=143 
 >>> KDCCommunication: kdc=kerberos.contoso.com UDP:88, timeout=30000,Attempt =1, #bytes=143 
 >>> KrbKdcReq send: #bytes read=646 
 >>> KdcAccessibility: remove kerberos.contoso.com 
 >>> EType: sun.security.krb5.internal.crypto.Des3CbcHmacSha1KdEType 
 >>> KrbAsRep cons in KrbAsReq.getReply myuser 
 [2017-04-25 21:34:33,548] INFO 687[main] - com.microsoft.polybase.client.KerberosSecureLogin.secureLogin(KerberosSecureLogin.java:97) - Subject: 
 Principal: admin_user@CONTOSO.COM 
 Private Credential: Ticket (hex) = 
 0000: 61 82 01 48 30 82 01 44 A0 03 02 01 05 A1 0E 1B a..H0..D........ 
 0010: 0C 41 50 53 48 44 50 4D 53 2E 43 4F 4D A2 21 30 .CONTOSO.COM.!0 
 0020: 1F A0 03 02 01 02 A1 18 30 16 1B 06 6B 72 62 74 ........0...krbt 
 0030: 67 74 1B 0C 41 50 53 48 44 50 4D 53 2E 43 4F 4D gt..CONTOSO.COM 
 0040: A3 82 01 08 30 82 01 04 A0 03 02 01 10 A1 03 02 ....0........... 
 *[...Condensed...]* 
 0140: 67 6D F6 41 6C EB E0 C3 3A B2 BD B1 gm.Al...:... 
 Client Principal = admin_user@CONTOSO.COM 
 Server Principal = krbtgt/CONTOSO.COM@CONTOSO.COM 
 *[...Condensed...]* 
 [2017-04-25 21:34:34,500] INFO 1639[main] - com.microsoft.polybase.client.HdfsBridge.main(HdfsBridge.java:1579) - Successfully authenticated against KDC server. 
```

## <a name="checkpoint-2"></a>Prüfpunkt 2
PolyBase versucht auf das Hadoop Distributed File System (HDFS) zuzugreifen. Dies schlägt fehl, da die Anforderung nicht das erforderliche Dienstticket enthalten hat.

```cmd
 [2017-04-25 21:34:34,501] INFO 1640[main] - com.microsoft.polybase.client.HdfsBridge.main(HdfsBridge.java:1584) - Attempting to access external filesystem at URI: hdfs://10.193.27.232:8020 
 Found ticket for admin_user@CONTOSO.COM to go to krbtgt/CONTOSO.COM@CONTOSO.COM expiring on Wed Apr 26 21:34:33 UTC 2017 
 Entered Krb5Context.initSecContext with state=STATE_NEW 
 Found ticket for admin_user@CONTOSO.COM to go to krbtgt/CONTOSO.COM@CONTOSO.COM expiring on Wed Apr 26 21:34:33 UTC 2017 
 Service ticket not found in the subject 
```

## <a name="checkpoint-3"></a>Prüfpunkt 3
Ein zweiter Hexdump lässt darauf schließen, dass [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] das TGT erfolgreich verwendet und das zutreffende Dienstticket für den SPN des Namenknotens vom KDC erhalten hat.

```cmd
 >>> KrbKdcReq send: kdc=kerberos.contoso.com UDP:88, timeout=30000, number of retries =3, #bytes=664 
 >>> KDCCommunication: kdc=kerberos.contoso.com UDP:88, timeout=30000,Attempt =1, #bytes=664 
 >>> KrbKdcReq send: #bytes read=669 
 >>> KdcAccessibility: remove kerberos.contoso.com 
 >>> EType: sun.security.krb5.internal.crypto.Des3CbcHmacSha1KdEType 
 >>> KrbApReq: APOptions are 00100000 00000000 00000000 00000000 
 >>> EType: sun.security.krb5.internal.crypto.Des3CbcHmacSha1KdEType 
 Krb5Context setting mySeqNumber to: 1033039363 
 Created InitSecContextToken: 
 0000: 01 00 6E 82 02 4B 30 82 02 47 A0 03 02 01 05 A1 ..n..K0..G...... 
 0010: 03 02 01 0E A2 07 03 05 00 20 00 00 00 A3 82 01 ......... ...... 
 0020: 63 61 82 01 5F 30 82 01 5B A0 03 02 01 05 A1 0E ca.._0..[....... 
 0030: 1B 0C 41 50 53 48 44 50 4D 53 2E 43 4F 4D A2 26 ..CONTOSO.COM.& 
 0040: 30 24 A0 03 02 01 00 A1 1D 30 1B 1B 02 6E 6E 1B 0$.......0...nn. 
 0050: 15 73 68 61 73 74 61 2D 68 64 70 32 35 2D 30 30 .hadoop-hdp25-00 
 0060: 2E 6C 6F 63 61 6C A3 82 01 1A 30 82 01 16 A0 03 .local....0..... 
 0070: 02 01 10 A1 03 02 01 01 A2 82 01 08 04 82 01 04 ................ 
 *[...Condensed...]* 
 0240: 03 E3 68 72 C4 D2 8D C2 8A 63 52 1F AE 26 B6 88 ..hr.....cR..&.. 
 0250: C4 . 
```

## <a name="checkpoint-4"></a>Prüfpunkt 4
Zuletzt sollten die Eigenschaften des Zielpfads mit einer Bestätigungsmeldung ausgegeben werden. Die Dateieigenschaften bestätigen, dass [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] von Hadoop mithilfe des Diensttickets authentifiziert und eine Sitzung für den Zugriff auf die gesicherte Ressource gewährt wurde.

Wenn Sie hier angelangt sind bedeutet dies, dass (a) die drei Akteure ordnungsgemäß miteinander kommunizieren können, (b) „core-site.xml“ und „jaas.conf“ korrekt sind und dass (c) Ihr KDC Ihre Anmeldeinformationen erkannt hat.

```cmd
 [2017-04-25 21:34:35,096] INFO 2235[main] - com.microsoft.polybase.client.HdfsBridge.main(HdfsBridge.java:1586) - File properties for "/": FileStatus{path=hdfs://10.193.27.232:8020/; isDirectory=true; modification_time=1492028259862; access_time=0; owner=hdfs; group=hdfs; permission=rwxr-xr-x; isSymlink=false} 
 [2017-04-25 21:34:35,098] INFO 2237[main] - com.microsoft.polybase.client.HdfsBridge.main(HdfsBridge.java:1587) - Successfully accessed the external file system. 
```

## <a name="common-errors"></a>Häufige Fehler
Wenn das Tool ausgeführt wurde und die Eigenschaften den Zielpfads *nicht* ausgegeben wurden (Prüfpunkt 4), wird eine Ausnahme ausgelöst. Überprüfen Sie diese und bestimmen Sie, an welcher Stelle sie in den vier Schritten aufgetreten ist. Beachten Sie dabei die folgenden häufig auftretenden Probleme, die möglicherweise aufgetreten sein können, in dieser Reihenfolge:

| Ausnahme und Meldungen | Ursache | 
| --- | --- |
| org.apache.hadoop.security.AccessControlException<br>SIMPLE authentication is not enabled. Available:[TOKEN, KERBEROS]\(Die SIMPLE-Authentifizierung ist nicht aktiviert. Verfügbar: [TOKEN, KERBEROS]) | The core-site.xml doesn't have the hadoop.security.authentication property set to "KERBEROS" (Die Datei „core-site.xml“ verfügt nicht über die Eigenschaft „hadoop.security.authentication“ um „KERBEROS“ festzulegen).|
|javax.security.auth.login.LoginException<br>Client not found in Kerberos database (6) - CLIENT_NOT_FOUND (Der Client konnte in der Kerberos-Datenbank nicht gefunden werden – CLIENT_NOT_FOUND). |    Der angegebene Administratordienstprinzipal existiert im angegebenen Bereich in „core-site.xml“ nicht.|
| javax.security.auth.login.LoginException<br> Prüfsumme fehlgeschlagen |Der Administratordienstprinzipal existiert, aber das Kennwort ist ungültig. |
| Native config name: C:\Windows\krb5.ini<br>Loaded from native config (Nativer Konfigurationsname: C:\Windows\krb5.ini Aus nativer Konfiguration geladen) | Diese Meldung weist darauf hin, dass „krb5LoginModule“ von Java benutzerdefinierte Clientkonfigurationen auf Ihrem Computer erkannt hat. Überprüfen Sie Ihre benutzerdefinierten Clienteinstellungen, da diese möglicherweise dieses Problem verursachen. |
| javax.security.auth.login.LoginException<br>java.lang.IllegalArgumentException<br>Illegal principal name admin_user@CONTOSO.COM: org.apache.hadoop.security.authentication.util.KerberosName$NoMatchingRule: No rules applied to admin_user@CONTOSO.COM (Unzulässiger Prinzipalname admin_user@CONTOSO.COM: org.apache.hadoop.security.authentication.util.KerberosName$NoMatchingRule: Es gibt keine Regel für admin_user@CONTOSO.COM) | Fügen Sie die Eigenschaft „hadoop.security.auth_to_local“ der Datei „core-site.xml“ mit den entsprechenden Regeln für die Hadoop-Cluster hinzu. |
| java.net.ConnectException<br>Attempting to access external filesystem at URI: hdfs://10.193.27.230:8020<br>Call From IAAS16981207/10.107.0.245 to 10.193.27.230:8020 failed on connection exception (Zugriff auf das externe Dateisystem auf dem URI: hdfs://10.193.27.230:8020 Der Aufruf von IAAS16981207/10.107.0.245 an 10.193.27.230:8020 ist wegen einer Verbindungsausnahme fehlgeschlagen) |    Die Authentifizierung im KDC war erfolgreich, aber der Zugriff auf den Namenknoten von Hadoop ist fehlgeschlagen. Überprüfen Sie die IP-Adresse und den Port des Namenknoten. Überprüfen Sie, dass die Firewall in Hadoop deaktiviert ist. |
| java.io.FileNotFoundException<br>File does not exist: /test/data.csv (Die Datei existiert nicht: /test/data.csv) |    Die Authentifizierung war erfolgreich, aber der angegebene Speicherort existiert nicht. Überprüfen Sie den Pfad oder prüfen Sie zunächst mit dem Stamm „/“. |

## <a name="debugging-tips"></a>Tipps zum Debuggen

### <a name="mit-kdc"></a>KDC von MIT  
Alle im KDC registrierten SPN, einschließlich Administratoren, können angezeigt werden, indem Sie **kadmin.local** > (admin login) > **listprincs** im Host des KDC oder einem beliebigen KDC-Client ausführen. Wenn Kerberos auf dem Hadoop-Cluster richtig konfiguriert ist, sollte es für jeden der im Cluster verfügbaren Dienste eine SPN geben (z.B.: `nn`, `dn`, `rm`, `yarn`, `spnego` usw.). Die entsprechenden Schlüsseltabellendateien (Kennwortersatz) werden standardmäßig unter **/etc/security/keytabs** angezeigt. Sie werden mit dem privaten Schlüssel des KDC verschlüsselt.  

Ziehen Sie auch in Betracht, [`kinit`](https://web.mit.edu/kerberos/krb5-1.12/doc/user/user_commands/kinit.html) zu verwenden, um die Administratoranmeldeinformationen lokal im KDC zu überprüfen. Es kann z. B. so verwendet werden: `kinit identity@MYREALM.COM`. Wenn Sie nach einem Kennwort gefragt werden, zeigt dies an, dass die Identität existiert.  
Die Protokolle des KDC stehen standardmäßig unter **/var/log/krb5kdc.log** zur Verfügung. Dazu gehören auch alle Ticketanforderungen, einschließlich der Client-IP, von der die Anforderung ausgegangen ist. Es sollten zwei Anforderungen von der IP des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Computers angezeigt werden, auf dem das Tool ausgeführt wurde: die erste vom authentifizierenden Server als **AS\_REQ** und die zweite als **TGS\_REQ** für das Dienstticket vom TGT.

```bash
 [root@MY-KDC log]# tail -2 /var/log/krb5kdc.log 
 May 09 09:48:26 MY-KDC.local krb5kdc[2547](info): **AS_REQ** (3 etypes {17 16 23}) 10.107.0.245: ISSUE: authtime 1494348506, etypes {rep=16 tkt=16 ses=16}, admin_user@CONTOSO.COM for **krbtgt/CONTOSO.COM@CONTOSO.COM** 
 May 09 09:48:29 MY-KDC.local krb5kdc[2547](info): **TGS_REQ** (3 etypes {17 16 23}) 10.107.0.245: ISSUE: authtime 1494348506, etypes {rep=16 tkt=16 ses=16}, admin_user@CONTOSO.COM for **nn/hadoop-hdp25-00.local@CONTOSO.COM** 
```

### <a name="active-directory"></a>Active Directory 
In Active Directory können Sie sich die SPN anschauen, indem Sie an die folgende Stelle navigieren: Einstellungen > Active Directory-Benutzer und -Computer > *MyRealm*  > *MyOrganizationUnit* (MeinBereich > MeineOrganisationeinheit). Wenn Kerberos auf dem Hadoop-Cluster richtig konfiguriert ist, gibt es für jeden der verfügbaren Dienste eine SPN (z. B.: `nn`, `dn`, `rm`, `yarn`, `spnego` usw.).

### <a name="general-debugging-tips"></a>Allgemeine Tipps zum Debuggen
Wenn Sie die Protokolle ansehen und Kerberos-Probleme debuggen, die unabhängig vom SQL Server-PolyBase-Feature sind, ist es hilfreich, sich bereits etwas mit Java auszukennen.

Sollten Sie dennoch Probleme mit dem Zugriff auf Kerberos haben, führen Sie zum Debuggen die folgenden Schritte aus:

1. Vergewissern Sie sich, dass Sie von außerhalb der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz aus auf die Kerberos HDFS-Daten zugreifen können. Sie haben folgende Möglichkeiten: 

    - Schreiben eines eigenen Java-Programms
    - Verwenden der `HdfsBridge`-Klasse aus dem PolyBase-Installationsordner Beispiel:

      ```java
      -classpath ".\Hadoop\conf;.\Hadoop\*;.\Hadoop\HDP2_2\*" com.microsoft.polybase.client.HdfsBridge 10.193.27.232 8020 admin_user C:\temp\kerberos_pass.txt
      ```

     Im obigen Beispiel enthält `admin_user` ausschließlich den Benutzernamen – und keinen Teil der Domäne.

2. Wenn Sie außerhalb von PolyBase nicht auf die Kerberos-HDFS-Daten zugreifen können:
    - Es gibt zwei Arten der Kerberos-Authentifizierung: die Active Directory-Kerberos-Authentifizierung und die MIT-Kerberos-Authentifizierung.
    - Stellen Sie sicher, dass der Benutzer im Domänenkonto vorhanden ist, und verwenden Sie das gleiche Benutzerkonto, wenn Sie auf HDFS zugreifen.

3. Bei der Active Directory-Kerberos-Authentifizierung sollten Sie darauf achten, dass das zwischengespeicherte Ticket angezeigt wird, wenn Sie unter Windows den `klist`-Befehl ausführen.
    - Melden Sie sich bei PolyBase an, und führen Sie `klist` und `klist tgt` in der Eingabeaufforderung aus, um zu überprüfen, ob das KDC, der Benutzername und die Verschlüsselungstypen korrekt sind.

4. Wenn KDC nur AES256 unterstützt, müssen [JCE-Richtliniendateien](http://www.oracle.com/technetwork/java/javase/downloads/index.html) installiert sein.

## <a name="see-also"></a>Weitere Informationen
[Integrating PolyBase with Cloudera using Active Directory Authentication (Integration von PolyBase in Cloudera mit der Active Directory-Authentifizierung)](/archive/blogs/microsoftrservertigerteam/integrating-polybase-with-cloudera-using-active-directory-authentication)  
[Cloudera’s Guide to setting up Kerberos for CDH (Cloudera-Leitfaden: Einrichten von Kerberos für CDH)](https://www.cloudera.com/documentation/enterprise/5-6-x/topics/cm_sg_principal_keytab.html)  
[Hortonworks’ Guide to Setting up Kerberos for HDP (Hortonworks-Leitfaden: Einrichten von Kerberos für HDP)](https://docs.hortonworks.com/HDPDocuments/Ambari-2.2.0.0/bk_Ambari_Security_Guide/content/ch_configuring_amb_hdp_for_kerberos.html)  
[Problembehandlung für PolyBase](polybase-troubleshooting.md)   
[PolyBase-Fehler und mögliche Lösungen](polybase-errors-and-possible-solutions.md)   
