---
title: Herstellen einer Verbindung mit einem Verfügbarkeitsgruppenlistener
description: Dieser Artikel enthält Informationen zum Herstellen einer Verbindung mit einem Always On-Verfügbarkeitsgruppenlistener sowie dem primären Replikat oder einem schreibgeschützten sekundären Replikat. Außerdem wird die Verwendung von TLS/SSL und Kerberos erläutert.
ms.custom: contperf-fy21q1
ms.date: 02/27/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], listeners
- read-only routing
- read-intent connections [AlwaysOn Availability Groups]
- Availability Groups [SQL Server], configuring
- Availability Groups [SQL Server], read-only routing
- Availability Groups [SQL Server], client connectivity
ms.assetid: 76fb3eca-6b08-4610-8d79-64019dd56c44
author: cawrites
ms.author: chadam
ms.openlocfilehash: 72005ed42951571d4903e42d128378a42b2bc007
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348440"
---
# <a name="connect-to-an-always-on-availability-group-listener"></a>Herstellen einer Verbindung mit einem Always On-Verfügbarkeitsgruppenlistener 
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  
Nachdem Sie [Ihren Verfügbarkeitsgruppenlistener konfiguriert haben](create-or-configure-an-availability-group-listener-sql-server.md), müssen Sie die Verbindungszeichenfolge aktualisieren, um eine Verbindung mit dem Always On-Verfügbarkeitsgruppenlistener herzustellen. Dadurch wird der Datenverkehr von der Anwendung automatisch an das vorgesehene Replikat weitergeleitet, ohne dass die Verbindungszeichenfolge nach jedem Failover manuell aktualisiert werden muss. 
  

##  <a name="connect-to-the-primary-replica"></a><a name="ConnectToPrimary"></a> Herstellen einer Verbindung mit dem primären Replikat  

Geben Sie in der Verbindungszeichenfolge den DNS-Namen des Verfügbarkeitsgruppenlisteners an, um für den Lese-/Schreibzugriff eine Verbindung mit dem primären Replikat herzustellen. 

Geben Sie beispielsweise den DNS-Namen des Listeners in das Servernamenfeld ein, um über den Listener eine Verbindung mit dem primären Replikat in SQL Server Management Studio herzustellen: 

![Verbinden mit einem Listener in SSMS](media/listeners-client-connectivity-application-failover/connect-to-listener-in-ssms.png)


Wenn bei einem Failover das primäre Replikat geändert wird, werden vorhandene Verbindungen zum Listener getrennt, und neue Verbindungen werden an das neue primäre Replikat weitergeleitet.  

Beispiel für eine einfache Verbindungszeichenfolge für den ADO.NET-Anbieter (System.Data.SqlClient): 
  
```  
Server=tcp: AGListener,1433;Database=MyDB;Integrated Security=SSPI  
```  

Sie können überprüfen, mit welchem Replikat Sie zurzeit über den Listener verbunden sind, indem Sie den folgenden Transact-SQL-Befehl (T-SQL) ausführen:

```sql
SELECT @@SERVERNAME
```

Beispiel für den Fall, dass SQLVM1 das primäre Replikat ist: 

![Überprüfen der Replikatkonnektivität](media/listeners-client-connectivity-application-failover/replica-server-name.png)


Sie können noch immer eine direkte Verbindung mit der SQL Server-Instanz herstellen, indem Sie den Instanznamen des primären oder sekundären Replikats anstelle des Verfügbarkeitsgruppenlisteners verwenden. Sie haben Sie jedoch nicht mehr den Vorteil, dass neue Verbindungen automatisch an das neue aktuelle, primäre Replikat weitergeleitet werden. Außerdem verlieren Sie den Vorteil des schreibgeschützten Routings, bei dem mit `read-intent` angegebene Verbindungen automatisch an das lesbare sekundäre Replikat weitergeleitet werden. 

##  <a name="connect-to-a-read-only-replica"></a><a name="ConnectToSecondary"></a> Herstellen einer Verbindung mit einem schreibgeschützten Replikat 

Das _schreibgeschützte Routing_ bezieht sich auf das automatische Routing eingehender Listenerverbindungen an ein lesbares sekundäres Replikat, das für das Zulassen schreibgeschützter Workloads konfiguriert ist. 

Verbindungen werden automatisch an das schreibgeschützte Replikat weitergeleitet, wenn Folgendes zutrifft: 
 
-   Mindestens ein sekundäres Replikat ist auf schreibgeschützten Zugriff festgelegt, und jedes schreibgeschützte sekundäre Replikat sowie das primäre Replikat werden [konfiguriert, um schreibgeschütztes Routing zu unterstützen](configure-read-only-routing-for-an-availability-group-sql-server.md). 

-   Die Verbindungszeichenfolge verweist auf eine Datenbank innerhalb der Verfügbarkeitsgruppe. Eine Alternative dazu ist es, für den Anmeldenamen, der für die Verbindung verwendet wird, die Datenbank als Standarddatenbank zu konfigurieren. Weitere Informationen dazu finden Sie in [diesem Blogbeitrag zur Funktionsweise des Algorithmus mit dem schreibgeschützten Routing](/archive/blogs/mattn/calculating-read_only_routing_url-for-alwayson) (in englischer Sprache).

-   Die Verbindungszeichenfolge verweist auf einen Verfügbarkeitsgruppenlistener, und die Anwendungsabsicht der eingehenden Verbindung wird auf schreibgeschützt festgelegt, z. B. mithilfe des Schlüsselworts **Application Intent=ReadOnly** in den ODBC- oder OLEBD-Verbindungszeichenfolgen, -Verbindungsattributen oder -Eigenschaften. 

Das Attribut der Anwendungsabsicht wird während der Anmeldung in der Clientsitzung gespeichert. Die Instanz von SQL Server verarbeitet dann diese Absicht und ermittelt das weitere Vorgehen entsprechend der Konfiguration der Verfügbarkeitsgruppe und dem aktuellen Lese-/Schreibstatus der Zieldatenbank im sekundären Replikat.  

Klicken Sie zunächst im Dialogfeld **Verbindung mit Server herstellen** auf **Optionen** und dann auf die Registerkarte **Zusätzliche Verbindungsparameter**, und geben Sie dann `ApplicationIntent=ReadOnly` im Textfeld an, wenn Sie beispielsweise mithilfe von SQL Server Management Studio eine Verbindung mit einem schreibgeschützten Replikat herstellen möchten:

![Schreibgeschützte Verbindung in SSMS](media/listeners-client-connectivity-application-failover/read-only-intent-in-ssms.png)
  
Beispiel für eine Verbindungszeichenfolge für den ADO.NET-Anbieter (System.Data.SqlClient) mit Festlegung einer schreibgeschützten Anwendungsabsicht: 

```  
Server=tcp:AGListener;Database=AdventureWorks;Integrated Security=SSPI;ApplicationIntent=ReadOnly  
```  
  
Weitere Informationen finden Sie unter [Konfigurieren des schreibgeschützten Zugriffs auf ein sekundäres Replikat einer Always On-Verfügbarkeitsgruppe](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md).  

## <a name="non-default-port"></a>Nicht standardmäßige Ports

Wenn Sie den Listener erstellen, legen Sie einen Port fest, der vom Listener verwendet werden soll. Wenn der Port der Standardport 1433 ist, müssen Sie beim Herstellen einer Verbindung mit dem Listener keine Portnummer angeben. Wenn es sich bei dem Port jedoch nicht um Port 1433 handelt, muss der Port beispielsweise wie folgt in der Verbindungszeichenfolge im Format `listenername,port` angegeben werden:

![Herstellen einer Verbindung mit einem nicht standardmäßigen Port](media/listeners-client-connectivity-application-failover/specify-port-in-ssms.png)

Beispiel für eine Verbindungszeichenfolge für den ADO.NET-Anbieter (System.Data.SqlClient), der einen nicht standardmäßigen Port für den Listener angibt: 

```  
Server=tcp:AGListener,1445;Database=AdventureWorks;Integrated Security=SSPI 
```  

##  <a name="bypass-listeners"></a><a name="BypassAGl"></a> Umgehungslistener  

Während Verfügbarkeitsgruppenlistener Unterstützung für Failoverumleitung und schreibgeschütztes Routing ermöglichen, müssen Clientverbindungen diese nicht verwenden. Eine Clientverbindung kann auch direkt auf die Instanz von SQL Server verweisen, statt eine Verbindung mit dem Verfügbarkeitsgruppenlistener herzustellen.  
  
Es besteht kein Unterschied zur Instanz von SQL Server, unabhängig davon, ob die Verbindung mithilfe des Verfügbarkeitsgruppenlisteners oder mithilfe eines anderen Instanzendpunkts angemeldet wird.  Die Instanz von SQL Server überprüft den Status der Zieldatenbank und ermöglicht oder verweigert die Konnektivität basierend auf der Konfiguration der Verfügbarkeitsgruppe und dem aktuellen Status der Datenbank auf der Instanz.  Wenn z.B. eine Clientanwendung direkt eine Verbindung mit einer Instanz des SQL Server-Ports herstellt und eine Verbindung mit einer in einer Verfügbarkeitsgruppe gehosteten Zieldatenbank herstellt und die Zieldatenbank sich im primären Status befindet und online geschaltet ist, ist die Konnektivität erfolgreich.  Ist die Zieldatenbank offline oder in einem Übergangsstatus, schlägt die Konnektivität zur Datenbank fehl.  
  
Alternativ können Anwendungen beim Migrieren von der Datenbankspiegelung zu [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]die Verbindungszeichenfolge für die Datenbankspiegelung angeben, sofern nur ein sekundäres Replikat vorhanden ist und Benutzerverbindungen unzulässig sind. 

##  <a name="database-mirroring-connection-strings"></a><a name="DbmConnectionString"></a> Verbindungszeichenfolgen für Datenbankspiegelung 
 Wenn eine Verfügbarkeitsgruppe nur ein sekundäres Replikat besitzt und mit ALLOW_CONNECTIONS = READ_ONLY oder ALLOW_CONNECTIONS = NONE für das sekundäre Replikat konfiguriert wird, können Clients mithilfe einer Verbindungszeichenfolge für die Datenbankspiegelung eine Verbindung mit dem primären Replikat herstellen. Dieser Ansatz kann beim Migrieren einer vorhandenen Anwendung von der Datenbankspiegelung zu einer Verfügbarkeitsgruppe nützlich sein, sofern Sie die Verfügbarkeitsgruppe auf zwei Verfügbarkeitsreplikate (ein primäres und ein sekundäres Replikat) beschränken. Wenn Sie zusätzliche sekundäre Replikate hinzufügen, müssen Sie einen Verfügbarkeitsgruppenlistener für die Verfügbarkeitsgruppe erstellen und die Anwendungen aktualisieren, damit der DNS-Name des Verfügbarkeitsgruppenlisteners verwendet wird.  
  
 Wenn Sie die Verbindungszeichenfolgen der Datenbankspiegelung verwenden, kann der Client entweder [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client oder .NET Framework-Datenanbieter für [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]verwenden. Die von einem Client bereitgestellte Verbindungszeichenfolge muss mindestens den Namen einer Serverinstanz, den *ursprünglichen Partnernamen*, angeben, um die Serverinstanz zu identifizieren, auf der das Verfügbarkeitsreplikat, zu dem Sie eine Verbindung herstellen möchten, ursprünglich gehostet wird. Optional kann die Verbindungszeichenfolge auch den Namen einer anderen Serverinstanz, den *Failoverpartnernamen*, enthalten, um die Serverinstanz zu identifizieren, auf der das sekundäre Replikat ursprünglich gehostet wird.  
  
 Weitere Informationen zu Verbindungszeichenfolgen für die Datenbankspiegelung finden Sie unter [Verbinden von Clients mit einer Datenbank-Spiegelungssitzung &#40;SQL Server&#41;](../../../database-engine/database-mirroring/connect-clients-to-a-database-mirroring-session-sql-server.md).  
  
  
##  <a name="multi-subnet-failovers"></a><a name="SupportAgMultiSubnetFailover"></a> Multisubnetzfailover  
 
 Wenn Sie Clientbibliotheken verwenden, die die Verbindungsoption „MultiSubnetFailover“ in der Verbindungszeichenfolge unterstützen, können Sie das Verfügbarkeitsgruppenfailover auf ein anderes Subnetz optimieren, indem Sie „MultiSubnetFailover“ je nach Syntax des verwendeten Anbieters auf „True“ oder „Yes“ festlegen.  
  
> [!NOTE]  
>  Diese Einstellung wird für Verbindungen mit einem Subnetz und mehreren Subnetzen mit den Namen von Verfügbarkeitslistenern und SQL Server-Failoverclusterinstanzen empfohlen.  Wenn Sie diese Option aktivieren, stehen auch für Szenarien mit einem Subnetz weitere Optimierungen zur Verfügung.  
  
 Die **MultiSubnetFailover** -Verbindungsoption funktioniert nur mit dem TCP-Netzwerkprotokoll und wird nur beim Herstellen einer Verbindung mit einem Verfügbarkeitsgruppenlistener und für beliebige virtuelle Netzwerknamen unterstützt, die eine Verbindung mit [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]herstellen.  
  
 Beispiel für eine Verbindungszeichenfolge des ADO.NET-Anbieters (System.Data.SqlClient) mit aktiviertem Multisubnetzfailover:  
  
```  
Server=tcp:AGListener,1433;Database=AdventureWorks;Integrated Security=SSPI; MultiSubnetFailover=True  
```  
  
 Die **MultiSubnetFailover** -Verbindungsoption sollte auf **True** festgelegt werden, auch wenn die Verfügbarkeitsgruppe nur ein einzelnes Subnetz umfasst.  Dies ermöglicht es Ihnen, neue Clients vorzukonfigurieren, um künftig weitere Subnetze zu unterstützen, ohne dass die Clientverbindungszeichenfolgen geändert werden müssen. Darüber hinaus wird die Failoverleistung für Failover in einem Subnetz optimiert.  Die **MultiSubnetFailover** -Verbindungsoption ist zwar nicht erforderlich, bietet jedoch den Vorteil eines schnelleren Subnetzfailovers.  Das liegt daran, dass der Clienttreiber versucht, parallel ein TCP-Socket für alle der Verfügbarkeitsgruppe zugeordneten IP-Adressen zu öffnen.  Der Clienttreiber wartet, bis die erste IP erfolgreich antwortet, und verwendet diese dann für die Verbindung.  
  
##  <a name="listeners--tlsssl-certificates"></a><a name="SSLcertificates"></a> Listener und TLS/SSL-Zertifikate  

Wenn beim Herstellen einer Verbindung mit einem Verfügbarkeitsgruppenlistener die beteiligten Instanzen von SQL Server TLS/SSL-Zertifikate zusammen mit Sitzungsverschlüsselung verwenden, muss der Clienttreiber den alternativen Antragstellernamen im TLS/SSL-Zertifikat unterstützen, um die Verschlüsselung zu erzwingen.  SQL Server-Treiberunterstützung für den alternativen Antragstellernamen des Zertifikats ist für ADO.NET (SqlClient), Microsoft JDBC und SQL Native Client (SNAC) geplant.  
  
Ein X.509-Zertifikat muss für alle beteiligten Serverknoten im Failovercluster mit einer Liste aller im alternativen Antragstellernamen des Zertifikats festgelegten Verfügbarkeitsgruppenlistenern konfiguriert werden. 

Die Zertifikatwerte haben folgendes Format: 

```  
CN = Server.FQDN  
SAN = Server.FQDN,Listener1.FQDN,Listener2.FQDN
```

Sie haben z. B. folgende Werte: 

```
Servername: Win2019   
Instance: SQL2019   
AG: AG2019   
Listener: Listener2019   
Domain: contoso.com  (which is also the FQDN)
```

Bei einem WSFC mit einer einzelnen Verfügbarkeitsgruppe sollte das Zertifikat den voll qualifizierten Domänennamen (Fully Qualified Domain Name, FQDN) des Servers und den FQDN des Listeners aufweisen: 

```
CN: Win2019.contoso.com
SAN: Win2019.contoso.com, Listener2019.contoso.com 
```

Mit dieser Konfiguration werden Ihre Verbindungen verschlüsselt, wenn eine Verbindung mit der Instanz (`WIN2019\SQL2019`) oder dem Listener (`Listener2019`) hergestellt wird. 

Abhängig von der Netzwerkkonfiguration gibt es eine kleine Teilmenge von Kunden, die dem SAN ggf. auch das NetBIOS hinzufügen müssen. In diesem Fall sollten die Zertifikatwerte wie folgt lauten: 

```
CN: Win2019.contoso.com
SAN: Win2019,Win2019.contoso.com,Listener2019,Listener2019.contoso.com
```

Der WSFC umfasst z. B. drei Verfügbarkeitsgruppenlistener: Listener1, Listener2, Listener3

Dann sollten die Zertifikatwerte wie folgt lauten: 

```
CN: Win2019.contoso.com
SAN: Win2019.contoso.com,Listener1.contoso.com,Listener2.contoso.com,Listener3.contoso.com
```
  
  
##  <a name="listeners-and-kerberos-spns"></a><a name="SPNs"></a> Listener und Kerberos (SPNs) 

Ein Domänenadministrator muss in Active Directory einen Dienstprinzipalnamen (Service Principal Name, SPN) für jeden Verfügbarkeitsgruppenlistener konfigurieren, um Kerberos für Clientverbindungen mit dem Listener zu aktivieren. Bei der Registrierung des Dienstprinzipalnamens (SPN) müssen Sie das Dienstkonto derjenigen Serverinstanz verwenden, die das Verfügbarkeitsreplikat hostet. Damit der SPN in allen Replikaten funktioniert, muss dasselbe Dienstkonto für alle Instanzen im WSFC-Cluster verwendet werden, der die Verfügbarkeitsgruppe hostet.  
  
 Konfigurieren Sie den SPN mithilfe des **setspn** Windows-Befehlszeilentools.  Beispiel für die Konfiguration eines Serverprinzipalnamens für die Verfügbarkeitsgruppe `AG1listener.Adventure-Works.com` , die auf einer Reihe von SQL Server-Instanzen gehostet wird und zur Ausführung unter dem Domänenkonto `corp\svclogin2`konfiguriert ist:  
  
```  
setspn -A MSSQLSvc/AG1listener.Adventure-Works.com:1433 corp\svclogin2  
```  
  
 Weitere Informationen zur manuellen Registrierung eines SPN für SQL Server finden Sie unter [Registrieren eines Dienstprinzipalnamens für Kerberos-Verbindungen](../../../database-engine/configure-windows/register-a-service-principal-name-for-kerberos-connections.md).  
  


## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie erfolgreich eine Verbindung mit dem Listener hergestellt haben, sollten Sie ein Offloading von [schreibgeschützten Workloads](overview-of-always-on-availability-groups-sql-server.md) und [Sicherungen](configure-backup-on-availability-replicas-sql-server.md) in Betracht ziehen, um die Leistung zu verbessern. Sie können auch verschiedene [Überwachungsstrategien für Verfügbarkeitsgruppen](monitoring-of-availability-groups-sql-server.md) überprüfen, mit denen Sie die Integrität Ihrer Verfügbarkeitsgruppe gewährleisten können. 

Weitere Informationen finden Sie im Abschnitt „Verfügbarkeitsmodi“ des Themas [Übersicht über Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).