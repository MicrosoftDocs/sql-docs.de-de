---
description: Grundlegendes zur Java EE-Unterstützung
title: Grundlegendes zur Java EE-Unterstützung | Microsoft-Dokumentation
ms.custom: ''
ms.date: 02/26/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: a9448b80-b7a3-49cf-8bb4-322c73676005
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f6d9e6a008c4cf3e3e9030bf25dfe00e2f10852b
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837507"
---
# <a name="understanding-java-ee-support"></a>Grundlegendes zur Java EE-Unterstützung

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

In den folgenden Abschnitten wird dokumentiert, wie [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] die optionalen API-Features für die Java-Plattform, Enterprise Edition (Java EE) und JDBC 3.0 unterstützt. Die Quellcodebeispiele in diesem Hilfesystem stellen eine gute Referenz für erste Schritte mit diesen Funktionen dar.  
  
Stellen Sie zunächst sicher, dass die Java-Umgebung (JDK, JRE) das Paket javax.sql einschließt. Dies ist ein erforderliches Paket für alle JDBC-Anwendungen, die die optionale API verwenden. JDK 1.5 und höhere Versionen umfassen dieses Paket bereits, sodass Sie es nicht separat installieren müssen.  
  
## <a name="driver-name"></a>Treibername

Der Treiberklassenname lautet **com.microsoft.sqlserver.jdbc.SQLServerDriver**. Für die JDBC-Treiber 4.1, 4.2 und 6.0 ist der Treiber in einer der folgenden Dateien enthalten: **sqljdbc.jar**, **sqljdbc4.jar**, **sqljdbc41.jar** oder **sqljdbc42.jar**.

JDBC-Treiber 6.2: Der Treiber ist in der Datei **mssql-jdbc-6.2.2.jre7.jar** oder **mssql-jdbc-6.2.2.jre8.jar** enthalten.

JDBC-Treiber 6.4: Der Treiber ist in der Datei **mssql-jdbc-6.4.0.jre7.jar**, **mssql-jdbc-6.4.0.jre8.jar** oder **mssql-jdbc-6.4.0.jre9.jar** enthalten.

JDBC-Treiber 7.0: Der Treiber ist in der Datei **mssql-jdbc-7.0.0.jre8.jar** oder **mssql-jdbc-7.0.0.jre10.jar** enthalten.

JDBC-Treiber 7.2: Der Treiber ist in der Datei **mssql-jdbc-7.2.2.jre8.jar** oder **mssql-jdbc-7.2.2.jre11.jar** enthalten.

JDBC-Treiber 7.4: Der Treiber ist in der Datei **mssql-jdbc-7.4.1.jre8.jar**, **mssql-jdbc-7.4.1.jre11.jar** oder **mssql-jdbc-7.4.1.jre12.jar** enthalten.

JDBC Driver 8.2: Der Treiber ist in der Datei **mssql-jdbc-8.2.2.jre8.jar**, **mssql-jdbc-8.2.2.jre11.jar** oder **mssql-jdbc-8.2.2.jre13.jar** enthalten.

JDBC Driver 8.4: Der Treiber ist in der Datei **mssql-jdbc-8.4.1.jre8.jar**, **mssql-jdbc-8.4.1.jre11.jar** oder **mssql-jdbc-8.4.1.jre14.jar** enthalten.

JDBC-Treiber 9.2: Der Treiber ist in der Datei **mssql-jdbc-9.2.1.jre8.jar**, **mssql-jdbc-9.2.1.jre11.jar** oder **mssql-jdbc-9.2.1.jre15.jar** enthalten.

Der Klassenname wird jedes Mal verwendet, wenn Sie den Treiber mit der JDBC-Klasse „DriverManager“ laden und wenn Sie den Klassennamen des Treibers in einer beliebigen Treiberkonfiguration angeben. Für das Konfigurieren einer Datenquelle in einem Java EE-Anwendungsserver kann es beispielsweise erforderlich sein, den Treiberklassennamen einzugeben.  
  
## <a name="data-sources"></a>Datenquellen

Der JDBC-Treiber unterstützt Java EE-/JDBC 3.0-Datenquellen. Die JDBC-Treiberklasse [SQLServerXADataSource](../../connect/jdbc/reference/sqlserverxadatasource-class.md) wird von `com.microsoft.sqlserver.jdbc.SQLServerXADataSource` implementiert.  
  
### <a name="datasource-names"></a>Datenquellennamen

Sie können Datenbankverbindungen mithilfe von Datenquellen herstellen. Die mit dem JDBC-Treiber verfügbaren Datenquellen werden in der folgenden Tabelle beschrieben:  
  
|DataSource-Typ|Klassenname und Beschreibung|  
|---------------|--------------------------|  
|DataSource|`com.microsoft.sqlserver.jdbc.SQLServerDataSource` <br/> <br/> Die Datenquelle ohne Pooling.|  
|ConnectionPoolDataSource|`com.microsoft.sqlserver.jdbc.SQLServerConnectionPoolDataSource` <br/> <br/> Die Datenquelle zum Konfigurieren von Verbindungspools für Java EE-Anwendungsserver. Wird normalerweise verwendet, wenn die Anwendung innerhalb eines Java EE-Anwendungsservers ausgeführt wird.|  
|XADataSource|`com.microsoft.sqlserver.jdbc.SQLServerXADataSource` <br/> <br/> Die Datenquelle zum Konfigurieren von Java EE-XA-Datenquellen. Wird normalerweise verwendet, wenn die Anwendung innerhalb eines Java EE-Anwendungsservers und eines XA-Transaktionsmanagers ausgeführt wird.|  
  
### <a name="data-source-properties"></a>Datenquelleneigenschaften

Alle Datenquellen unterstützen die Möglichkeit zum Festlegen und Abrufen aller Eigenschaften, die dem Eigenschaftenset des zugrunde liegenden Treibers zugeordnet sind.  
  
Beispiele:  
  
`setServerName("localhost");`  
`setDatabaseName("AdventureWorks");`  
  
Im Folgenden wird veranschaulicht, wie eine Anwendung mit einer Datenquelle eine Verbindung herstellt:  

```java
//initialize JNDI ..  
Context ctx = new InitialContext(System.getProperties());
...
DataSource ds = (DataSource) ctx.lookup("MyDataSource");
Connection c = ds.getConnection("user", "pwd");  
```

Weitere Informationen zu den Datenquelleneigenschaften finden Sie unter [Festlegen der Datenquelleneigenschaften](../../connect/jdbc/setting-the-data-source-properties.md).  
  
## <a name="see-also"></a>Weitere Informationen

[Übersicht über den JDBC-Treiber](../../connect/jdbc/overview-of-the-jdbc-driver.md)  
