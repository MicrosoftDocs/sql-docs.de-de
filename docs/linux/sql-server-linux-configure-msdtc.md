---
title: Konfigurieren von MS DTC unter Linux
description: In diesem Artikel erfahren Sie, wie der Microsoft Distributed Transaction Coordinator (MS DTC) unter Linux konfiguriert wird.
author: VanMSFT
ms.author: vanto
ms.date: 08/12/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: a2013fb1e32d14bed4b92aa0cc2394ad3a5c9174
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349126"
---
# <a name="how-to-configure-the-microsoft-distributed-transaction-coordinator-msdtc-on-linux"></a>Konfigurieren von Microsoft Distributed Transaction Coordinator (MS DTC) unter Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

In diesem Artikel wird beschrieben, wie Microsoft Distributed Transaction Coordinator (MS DTC) unter Linux konfiguriert wird.

> [!NOTE]
> MS DTC für Linux wird in SQL Server 2017 ab dem kumulativen Update 16 unterstützt.

## <a name="overview"></a>Übersicht

Verteilte Transaktionen sind für SQL Server für Linux aktiviert, indem MS DTC und die Funktion der RPC-Endpunktzuordnung in SQL Server eingeführt werden. Standardmäßig lauscht ein RPC-Endpunktzuordnungsprozess an Port 135 für eingehende RPC-Anforderungen und bietet Informationen zu registrierten Komponenten für Remoteanforderungen. Remoteanforderungen können die von der Endpunktzuordnung zurückgegebenen Informationen zum Kommunizieren mit registrierten RPC-Komponenten wie MS DTC-Diensten verwenden. Ein Prozess erfordert unter Linux Superuser-Berechtigungen für die Bindung an bekannte Ports (Portnummern unter 1024). Systemadministratoren müssen iptables zum Erstellen einer Netzwerkadressenübersetzung für das Routen von Datenverkehr an Port 135 zum RPC-Endpunktzuordnungsprozess von SQL Server verwenden, um zu vermeiden, dass SQL Server mit Stammberechtigungen für den RPC-Endpunktzuordnungsprozess gestartet wird.

MS DTC verwendet zwei Konfigurationsparameter für das mssql-conf-Hilfsprogramm:

| mssql-conf-Einstellung | BESCHREIBUNG |
|---|---|
| **network.rpcport** | Der TCP-Port, an den der RPC-Endpunktzuordnungsprozess gebunden wird. |
| **distributedtransaction.servertcpport** | Der Port, an dem der MS DTC-Server lauscht. Wenn dieser nicht festgelegt ist, verwendet der MS DTC-Dienst bei Neustarts des Diensts einen zufälligen kurzlebigen Port. Firewallausnahmen müssen neu konfiguriert werden, um sicherzustellen, dass die Kommunikation mit dem MS DTC-Dienst fortgesetzt werden kann. |

Weitere Informationen zu diesen Einstellungen und weiteren MS DTC-Einstellungen finden Sie unter [Konfigurieren von SQL Server für Linux mit dem mssql-conf-Tool](sql-server-linux-configure-mssql-conf.md).

## <a name="supported-transaction-standards"></a>Unterstützte Transaktionsstandards

Die folgenden MS DTC-Konfigurationen werden unterstützt:

| Transaktionsstandard | Datenquellen | ODBC-Treiber | JDBC-Treiber|
|---|---|---|---|
| OLE-TX-Transaktionen | SQL Server unter Linux | Ja | Nein|
| Verteilte XA-Transaktionen | SQL Server sowie andere ODBC- und JDBC-Datenquellen mit Unterstützung für XA | Ja (Version 17.3 oder höher erforderlich) | Ja |
| Verteilte Transaktionen auf Verbindungsserver | SQL Server | Ja | Nein

Weitere Informationen finden Sie unter [Grundlegendes zu XA-Transaktionen](../connect/jdbc/understanding-xa-transactions.md#configuration-instructions).

## <a name="msdtc-configuration-steps"></a>MS DTC-Konfigurationsschritte

Es gibt drei Schritte, um die MS DTC-Kommunikation und -Funktionalität zu konfigurieren. Wenn die erforderlichen Konfigurationsschritte nicht ausgeführt werden, aktiviert SQL Server die MS DTC-Funktionalität nicht.

- Konfigurieren von **network.rpcport** und **distributedtransaction.servertcpport** mithilfe von mssql-conf
- Konfigurieren der Firewall, um die Kommunikation über **distributedtransaction.servertcpport** und Port 135 zuzulassen
- Konfigurieren des Linux-Serverroutings, sodass die RPC-Kommunikation an Port 135 an **network.rpcport** von SQL Server umgeleitet wird

Die folgenden Abschnitte enthalten detaillierte Anweisungen für die einzelnen Schritte.

## <a name="configure-rpc-and-msdtc-ports"></a>Konfigurieren von RPC- und MS DTC-Ports

Konfigurieren Sie zunächst **network.rpcport** und **distributedtransaction.servertcpport** mithilfe von mssql-conf. Dieser Schritt gilt für SQL Server und für alle unterstützten Distributionen einheitlich.

1. Verwenden Sie mssql-conf, um den Wert von **network.rpcport** festzulegen. Im folgenden Beispiel wird dieser auf „13500“ festgelegt.

   ```bash
   sudo /opt/mssql/bin/mssql-conf set network.rpcport 13500
   ```

2. Legen Sie den Wert für **distributedtransaction.servertcpport** fest. Im folgenden Beispiel wird dieser auf 51999 festgelegt.

   ```bash
   sudo /opt/mssql/bin/mssql-conf set distributedtransaction.servertcpport 51999
   ```

3. Starten Sie SQL Server neu.

   ```bash
   sudo systemctl restart mssql-server
   ```

## <a name="configure-the-firewall"></a>Konfigurieren der Firewall

Der zweite Schritt besteht darin, die Firewall zu konfigurieren, um die Kommunikation an **servertcpport** und Port 135 zuzulassen.  Dadurch kann der RPC-Endpunktzuordnungsprozess und der MS DTC-Prozess aktiviert werden, um extern mit anderen Transaktions-Managern und Koordinatoren zu kommunizieren. Die eigentlichen Schritte hierfür sind abhängig von Ihrer Linux-Distribution und Ihrer Firewall. 

Im folgenden Beispiel wird das Erstellen dieser Regeln unter **Ubuntu** gezeigt.

```bash
sudo ufw allow from any to any port 51999 proto tcp
sudo ufw allow from any to any port 135 proto tcp
```

Im folgenden Beispiel wird gezeigt, wie dies unter **Red Hat Enterprise Linux (RHEL)** durchgeführt werden kann:

```bash
sudo firewall-cmd --zone=public --add-port=51999/tcp --permanent
sudo firewall-cmd --zone=public --add-port=135/tcp --permanent
sudo firewall-cmd --reload
```

Es ist wichtig, dass die Firewall vor dem Konfigurieren des Portroutings im nächsten Abschnitt konfiguriert wird. Das Aktualisieren der Firewall kann in manchen Fällen die Portroutingregeln löschen.

## <a name="configure-port-routing"></a>Konfigurieren des Portroutings

Konfigurieren Sie die Linux-Serverroutingtabelle, sodass die RPC-Kommunikation an Port 135 an **network.rpcport** von SQL Server umgeleitet wird. Der Konfigurationsmechanismus für die Portweiterleitung kann sich bei unterschiedlichen Distributionen unterscheiden. Die folgenden Abschnitte enthalten Anleitungen für Ubuntu, SUS Enterprise Linux (SLES) und Red Hat Enterprise Linux (RHEL).

### <a name="port-routing-in-ubuntu-and-sles"></a>Portrouting in Ubuntu und SLES

Ubuntu und SLES (SUSE Linux Enterprise Server) verwenden nicht den **firewalld**-Dienst, sodass **iptable**-Regeln einen effizienten Mechanismus zum Erreichen von Portrouting bieten. Die **iptable**-Regeln bleiben bei Neustarts möglicherweise nicht erhalten, sodass die folgenden Befehle auch Anweisungen zum Wiederherstellen der Regeln nach einem Neustart enthalten.

1. Erstellen Sie Routingregeln für Port 135. Im folgenden Beispiel wird Port 135 an den RPC-Port 13500 weitergeleitet, der im vorherigen Abschnitt definiert wurde. Ersetzen Sie `<ipaddress>` durch die IP-Adresse Ihres Servers.

   ```bash
   sudo iptables -t nat -A PREROUTING -d <ip> -p tcp --dport 135 -m addrtype --dst-type LOCAL  \
      -j DNAT --to-destination <ip>:13500 -m comment --comment RpcEndPointMapper
   sudo iptables -t nat -A OUTPUT -d <ip> -p tcp --dport 135 -m addrtype --dst-type LOCAL \
      -j DNAT --to-destination <ip>:13500 -m comment --comment RpcEndPointMapper
   ```

   Der `--comment RpcEndPointMapper`-Parameter in den vorherigen Befehlen unterstützt Sie bei der Verwaltung dieser Regeln in späteren Befehlen.

2. Lassen Sie die mit dem folgenden Befehl erstellten Routingregeln anzeigen:

   ```bash
   sudo iptables -S -t nat | grep "RpcEndPointMapper"
   ```

3. Speichern Sie die Routingregeln in einer Datei auf Ihrem Computer.

   ```bash
   sudo iptables-save > /etc/iptables.conf
   ```

4. Fügen Sie den folgenden Befehl zu `/etc/rc.local` (für Ubuntu) oder zu `/etc/init.d/after.local` (für SLES) hinzu, um die Regeln nach einem Neustart erneut zu laden:

   ```bash
   iptables-restore < /etc/iptables.conf
   ```

   > [!NOTE]
   > Sie müssen über Superuser-Berechtigungen (Sudo) verfügen, um die Dateien **rc.local** oder **after.local** zu bearbeiten.

Die Befehle **iptables-save** und **iptables-restore** stellen zusammen mit der Startkonfiguration `rc.local`/`after.local` einen grundlegenden Mechanismus zum Speichern und Wiederherstellen von iptables-Entitäten bereit. Abhängig von Ihrer Linux-Distribution sind möglicherweise erweiterte oder automatisierte Optionen verfügbar. Eine Ubuntu-Alternative ist beispielsweise das **iptables-persistent**-Paket, um Einträge persistent zu machen.

> [!IMPORTANT]
> In den vorherigen Schritte wird von einer festen IP-Adresse ausgegangen. Wenn sich die IP-Adresse für Ihre SQL Server-Instanz (aufgrund von manuellem Eingriff oder DHCP) ändert, müssen Sie die Routingregeln entfernen und neu erstellen, wenn diese mit iptables erstellt wurden. Wenn Sie vorhandene Routingregeln neu erstellen oder löschen müssen, können Sie den folgenden Befehl zum Löschen alter `RpcEndPointMapper`-Regeln verwenden:
> 
> ```bash
> sudo iptables -S -t nat | grep "RpcEndPointMapper" | sed 's/^-A //' | while read rule; do iptables -t nat -D $rule; done
> ```

### <a name="port-routing-in-rhel"></a>Portrouting in RHEL

Bei Distributionen wie Red Hat Enterprise Linux, die den **firewalld**-Dienst verwenden, kann derselbe Dienst sowohl für das Öffnen des Ports auf dem Server als auch für die interne Portweiterleitung verwendet werden. Im Fall von Red Hat Enterprise Linux sollten Sie beispielsweise anstatt iptables den **firewalld**-Dienst verwenden (über das **firewall-cmd**-Konfigurationshilfsprogramm mit `-add-forward-port` oder ähnlichen Optionen), um persistente Portweiterleitungsregeln zu erstellen und zu verwalten.

```bash
sudo firewall-cmd --permanent --add-forward-port=port=135:proto=tcp:toport=13500
sudo firewall-cmd --reload
```

## <a name="verify"></a>Überprüfung

An diesem Punkt sollte SQL Server an verteilten Transaktionen teilnehmen können. Führen Sie den Befehl **netstat** aus, um zu überprüfen, ob SQL Server lauscht (Wenn Sie RHEL verwenden, müssen Sie möglicherweise zuerst das Paket **net-tools** installieren):

```bash
sudo netstat -tulpn | grep sqlservr
```

Die Ausgabe sollte in etwa wie folgt aussehen:

```bash
tcp 0 0 0.0.0.0:1433 0.0.0.0:* LISTEN 13911/sqlservr
tcp 0 0 127.0.0.1:1434 0.0.0.0:* LISTEN 13911/sqlservr
tcp 0 0 0.0.0.0:13500 0.0.0.0:* LISTEN 13911/sqlservr
tcp 0 0 0.0.0.0:51999 0.0.0.0:* LISTEN 13911/sqlservr
tcp6 0 0 :::1433 :::* LISTEN 13911/sqlservr
tcp6 0 0 ::1:1434 :::* LISTEN 13911/sqlservr
tcp6 0 0 :::13500 :::* LISTEN 13911/sqlservr
tcp6 0 0 :::51999 :::* LISTEN 13911/sqlservr
```

Nach einem Neustart beginnt SQL Server jedoch bis zur ersten verteilten Transaktion nicht damit, an **servertcpport** zu lauschen. In diesem Fall würde SQL Server bis zur ersten verteilten Transaktion nicht an Port 51999 lauschen.

## <a name="configure-authentication-on-rpc-communication-for-msdtc"></a>Konfigurieren der Authentifizierung für die RPC-Kommunikation für MS DTC

MS DTC für SQL Server für Linux verwendet standardmäßig keine Authentifizierung für die RPC-Kommunikation. Wenn der Hostcomputer jedoch einer Active Directory-Domäne (AD) hinzugefügt wird, ist es möglich, MS DTC mithilfe der folgenden **mssql-conf**-Einstellungen für die Verwendung der authentifizierten RPC-Kommunikation zu konfigurieren:

| Einstellung | BESCHREIBUNG |
|---|---|
| **distributedtransaction.allowonlysecurerpccalls**          | Konfigurieren von sicheren RPC-Aufrufen für verteilte Transaktionen. Der Standardwert ist 0. |
| **distributedtransaction.fallbacktounsecurerpcifnecessary** | Konfigurieren von sicheren RPC-Aufrufen für verteilte Transaktionen. Der Standardwert ist 0. |
| **distributedtransaction.turnoffrpcsecurity**               | Aktivieren oder Deaktivieren der RPC-Sicherheit für verteilte Transaktionen. Der Standardwert ist 0. |

## <a name="additional-guidance"></a>Zusätzliche Anleitungen

### <a name="active-directory"></a>Active Directory

Microsoft empfiehlt die Verwendung von MS DTC mit aktiviertem RPC, wenn SQL Server in einer Active Directory-Konfiguration (AD) registriert ist. Wenn SQL Server für die Verwendung der AD-Authentifizierung konfiguriert ist, verwendet MS DTC standardmäßig die RPC-Sicherheit mit gegenseitiger Authentifizierung.

### <a name="windows-and-linux"></a>Windows und Linux

Wenn sich ein Client mit Windows-Betriebssystem bei einer verteilten Transaktion mit SQL Server für Linux eintragen muss, benötigt er die folgende Mindestversion des Windows-Betriebssystems:

| Betriebssystem | Mindestversion | Betriebssystembuild |
|---|---|---|
| [Windows Server](/windows-server/get-started/windows-server-release-info) | 1903 | 18362.30.190401-1528 |
| [Windows 10](/windows/release-information/) | 1903 | 18362.267 |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu SQL Server für Linux finden Sie unter [SQL Server für Linux](sql-server-linux-overview.md).