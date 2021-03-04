---
title: Herstellen von Verbindungen mit SQL Server mit einer NTLM-Authentifizierung
description: Hier erfahren Sie, wie Sie mithilfe der NTLM-Authentifizierung eine SQL-Datenbank-Verbindung mit dem JDBC-Treiber herstellen.
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.suite: sql
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: ''
author: lilgreenbird
ms.author: v-susanh
manager: kenvh
ms.openlocfilehash: a0d58875603781f4959ee8d10cb683c98bd884c1
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837469"
---
# <a name="using-ntlm-authentication-to-connect-to-sql-server"></a>Herstellen von Verbindungen mit SQL Server mit NTLM-Authentifizierung

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

In [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] können Anwendungen mithilfe der Verbindungseigenschaft **authenticationScheme** angeben, dass eine Verbindung mit einer Datenbank unter Verwendung der NTLM v2-Authentifizierung hergestellt werden soll. 

Nachfolgend sind Eigenschaften aufgeführt, die ebenfalls für die NTLM-Authentifizierung verwendet werden:

- **domain = domainName** (optional)
- **user = userName**
- **password = password**
- **integratedSecurity = true**

Mit Ausnahme von **domain** müssen diese Eigenschaften angegeben werden. Wenn die Eigenschaften bei Verwendung der authenticationScheme-Eigenschaft **NTLM** fehlen, gibt der Treiber einen Fehler aus. 

Weitere Informationen zu Verbindungseigenschaften finden Sie unter [Festlegen von Verbindungseigenschaften](../../connect/jdbc/setting-the-connection-properties.md). Weitere Informationen zum Microsoft NTLM-Authentifizierungsprotokoll finden Sie unter [Microsoft NTLM](/windows/desktop/SecAuthN/microsoft-ntlm).

## <a name="remarks"></a>Bemerkungen

Unter [Netzwerksicherheit: LAN-Manager-Authentifizierungsebene](/windows/security/threat-protection/security-policy-settings/network-security-lan-manager-authentication-level) sind die SQL Server-Einstellungen beschrieben, die das Verhalten der NTLM-Authentifizierung steuern. 

## <a name="logging"></a>Protokollierung

Zur Unterstützung der NTLM-Authentifizierung wurde eine neue Protokollierung hinzugefügt: com.microsoft.sqlserver.jdbc.internals.NTLMAuthentication. Weitere Informationen finden Sie unter [Tracing Driver Operation (Ablaufverfolgung für Treibervorgänge)](../../connect/jdbc/tracing-driver-operation.md).

## <a name="datasource"></a>DataSource

Bei Verwendung einer Datenquelle zum Herstellen von Verbindungen können die NTLM-Eigenschaften programmgesteuert über **setAuthenticationScheme**, **setDomain** und (optional) **setServerSpn** festgelegt werden.

```java
SQLServerDataSource ds = new SQLServerDataSource();
ds.setServerName("<server>");
ds.setPortNumber(1433); // change if necessary
ds.setIntegratedSecurity(true);
ds.setAuthenticationScheme("NTLM");
ds.setDomain("<domainName>");
ds.setUser("<userName>");
ds.setPassword("<password>");
ds.setDatabaseName("<database>");
ds.setServerSpn("<serverSpn");

try (Connection c = ds.getConnection(); Statement s = c.createStatement();
        ResultSet rs = s.executeQuery("select auth_scheme from sys.dm_exec_connections where session_id=@@spid")) {
    while (rs.next()) {
        System.out.println("Authentication Scheme: " + rs.getString(1));
    }
}
```

## <a name="service-principal-names"></a>Dienstprinzipalnamen

Ein Dienstprinzipalname (SPN, Service Principal Name) ist der Name, über den ein Client eine Instanz eines Diensts eindeutig identifiziert.

Sie können den SPN mithilfe der Verbindungseigenschaft **serverSpn** angeben oder ihn vom Treiber erstellen lassen (Standardeinstellung). Diese Eigenschaft kann in der Form „MSSQLSvc/fqdn:port\@REALM“ angegeben werden, wobei „fqdn“ den vollqualifizierten Domänennamen, „port“ die Portnummer und „REALM“ den Bereich des SQL Server-Computers in Großbuchstaben darstellen. Der Abschnitt für den Bereich ist bei dieser Eigenschaft optional, da der Standardbereich dem Bereich des Servers entspricht.

Nachfolgend ist ein Beispiel Ihres Dienstprinzipalnamens gezeigt: MSSQLSvc/some-server.zzz.corp.contoso.com:1433

Weitere Informationen zu Dienstprinzipalnamen (SPNs) finden Sie in folgendem Thema:

- [Unterstützung von Dienstprinzipalnamen (SPN) in Clientverbindungen](../../relational-databases/native-client/features/service-principal-name-spn-support-in-client-connections.md)

> [!NOTE]  
> Das Verbindungsattribut „serverSpn“ wird nur vom Microsoft JDBC-Treiber 4.2 und höher unterstützt.

> Vor Release 6.2 des JDBC-Treibers mussten Sie den **serverSpn** explizit festlegen. Ab Release 6.2 kann der Treiber den **serverSpn** standardmäßig erstellen. Sie können den **serverSpn** jedoch auch weiterhin explizit angeben.

## <a name="security-risks"></a>Sicherheitsrisiken

Das NTLM-Protokoll ist ein älteres Authentifizierungsprotokoll, das mit einer Reihe von Sicherheitsrisiken einhergeht. Es basiert auf einem relativ unsicheren kryptografischen Schema und ist anfällig für verschiedene Angriffe. Aus diesem Grund wird die Verwendung des deutlich sichereren Kerberos-Protokolls empfohlen. Die NTLM-Authentifizierung sollte ausschließlich in einer vertrauenswürdigen Umgebung bzw. in Szenarien verwendet werden, in denen Kerberos nicht verwendet werden kann.

[!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] unterstützt lediglich die NTLM-Version v2, die gegenüber der ursprünglichen Version v1 eine höhere Sicherheit bietet. Für eine verbesserte Sicherheit wird zudem empfohlen, den erweiterten Schutz zu aktivieren oder die SSL-Verschlüsselung zu verwenden. 

Weitere Informationen zur Aktivierung des erweiterten Schutzes finden Sie hier:

- [Herstellen einer Verbindung mit der Datenbank-Engine unter Verwendung von Erweiterter Schutz](../../database-engine/configure-windows/connect-to-the-database-engine-using-extended-protection.md)

Weitere Informationen zu Verbindungen mit SSL-Verschlüsselung finden Sie hier:

- [Herstellen von Verbindungen mit SSL-Verschlüsselung](../../connect/jdbc/connecting-with-ssl-encryption.md)

> [!NOTE]
> Bei Release 7.4 ist es nicht möglich **sowohl** den erweiterten Schutz als auch die Verschlüsselung zu aktivieren.

## <a name="see-also"></a>Weitere Informationen

[Verbinden mit SQL Server mit dem JDBC-Treiber](../../connect/jdbc/connecting-to-sql-server-with-the-jdbc-driver.md)
