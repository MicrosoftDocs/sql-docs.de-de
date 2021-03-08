---
title: Häufig gestellte Fragen (FAQ) zum JDBC-Treiber
description: Dieser Artikel enthält Antworten auf häufig gestellte Fragen zum Microsoft JDBC-Treiber für SQL Server.
ms.custom: ''
ms.date: 02/26/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: cbc0e397-ecf2-4494-87b2-a492609bceae
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 71c5e88db1bbc0c1b8d6698930c503a1134bac7d
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837100"
---
# <a name="frequently-asked-questions-faq-for-jdbc-driver"></a>Häufig gestellte Fragen (FAQ) zum JDBC-Treiber

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Dieser Artikel enthält Antworten auf häufig gestellte Fragen zum Microsoft JDBC-Treiber für SQL Server.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

**Wie kann ich zur Verbesserung des JDBC-Treibers beitragen?**  
Der JDBC-Treiber ist ein Open-Source-Produkt, und den Quellcode finden Sie auf [GitHub](https://github.com/microsoft/mssql-jdbc). Sie können zur Verbesserung des Treibers beitragen, indem Sie Probleme melden und an der Codebasis mitwirken.

**Welche Versionen von SQL Server und Java werden vom Treiber unterstützt?**  
Detaillierte Informationen hierzu finden Sie auf der Seite [Supportmatrix für den Microsoft JDBC-Treiber für SQL Server](../../connect/jdbc/microsoft-jdbc-driver-for-sql-server-support-matrix.md).

**Was ist der Unterschied zwischen den im [Microsoft Download Center](download-microsoft-jdbc-driver-for-sql-server.md) verfügbaren JDBC-Treiberpaketen und dem auf GitHub verfügbaren JDBC-Treiber?**  
Die im GitHub-Repository für den Microsoft JDBC-Treiber verfügbaren JDBC-Treiberdateien bilden den Kern des JDBC-Treibers und stehen unter der im Repository aufgeführten Open-Source-Lizenz. Die Treiberpakete im [Microsoft Download Center](download-microsoft-jdbc-driver-for-sql-server.md) enthalten zusätzliche Bibliotheken für die integrierte Windows-Authentifizierung und die Aktivierung von XA-Transaktionen mit dem JDBC-Treiber. Diese zusätzliche Bibliotheken stehen unter der Lizenz, die im herunterladbaren Paket enthalten ist.

**Was sollte ich wissen, bevor ich meinen Treiber aktualisiere?**  
Der Microsoft JDBC-Treiber 9.2 unterstützt die JDBC-Spezifikationen 4.2 und 4.3 (teilweise), und das Installationspaket enthält die folgenden drei JAR-Klassenbibliotheken:

| JAR                        | JDBC-Spezifikation            | JDK-Version |
| -------------------------- | ----------------------------- | ----------- |
| mssql-jdbc-9.2.1.jre15.jar | JDBC 4.3 (teilweise) und 4.2 | JDK 15.0    |
| mssql-jdbc-9.2.1.jre11.jar | JDBC 4.3 (teilweise) und 4.2 | JDK 11.0    |
| mssql-jdbc-9.2.1.jre8.jar  | JDBC 4.2                      | JDK 8.0     |
| &nbsp;                     | &nbsp;                        | &nbsp;      |

Der Microsoft JDBC-Treiber 8.4 unterstützt die JDBC-Spezifikationen 4.2 sowie 4.3 (teilweise), und das Installationspaket enthält die folgenden drei JAR-Klassenbibliotheken:

| JAR                        | JDBC-Spezifikation            | JDK-Version |
| -------------------------- | ----------------------------- | ----------- |
| mssql-jdbc-8.4.1.jre14.jar | JDBC 4.3 (teilweise) und 4.2 | JDK 14.0    |
| mssql-jdbc-8.4.1.jre11.jar | JDBC 4.3 (teilweise) und 4.2 | JDK 11.0    |
| mssql-jdbc-8.4.1.jre8.jar  | JDBC 4.2                      | JDK 8.0     |
| &nbsp;                     | &nbsp;                        | &nbsp;      |

Der Microsoft JDBC-Treiber 8.2 unterstützt die JDBC-Spezifikationen 4.2 sowie 4.3 (teilweise), und das Installationspaket enthält drei JAR-Klassenbibliotheken:

| JAR                        | JDBC-Spezifikation            | JDK-Version |
| -------------------------- | ----------------------------- | ----------- |
| mssql-jdbc-8.2.2.jre13.jar | JDBC 4.3 (teilweise) und 4.2 | JDK 13.0    |
| mssql-jdbc-8.2.2.jre11.jar | JDBC 4.3 (teilweise) und 4.2 | JDK 11.0    |
| mssql-jdbc-8.2.2.jre8.jar  | JDBC 4.2                      | JDK 8.0     |
| &nbsp;                     | &nbsp;                        | &nbsp;      |

Der Microsoft JDBC-Treiber 7.4 unterstützt die JDBC-Spezifikationen 4.2 sowie 4.3 (teilweise), und das Installationspaket enthält drei JAR-Klassenbibliotheken:

| JAR                        | JDBC-Spezifikation            | JDK-Version |
| -------------------------- | ----------------------------- | ----------- |
| mssql-jdbc-7.4.1.jre12.jar | JDBC 4.3 (teilweise) und 4.2 | JDK 12.0    |
| mssql-jdbc-7.4.1.jre11.jar | JDBC 4.3 (teilweise) und 4.2 | JDK 11.0    |
| mssql-jdbc-7.4.1.jre8.jar  | JDBC 4.2                      | JDK 8.0     |
| &nbsp;                     | &nbsp;                        | &nbsp;      |

Der Microsoft JDBC-Treiber 7.2 unterstützt die JDBC-Spezifikation 4.2 sowie 4.3 (teilweise), und das Installationspaket enthält zwei JAR-Klassenbibliotheken:

| JAR                        | JDBC-Spezifikation            | JDK-Version |
| -------------------------- | ----------------------------- | ----------- |
| mssql-jdbc-7.2.2.jre11.jar | JDBC 4.3 (teilweise) und 4.2 | JDK 11.0    |
| mssql-jdbc-7.2.2.jre8.jar  | JDBC 4.2                      | JDK 8.0     |
| &nbsp;                     | &nbsp;                        | &nbsp;      |

Der Microsoft JDBC-Treiber 7.0 unterstützt die JDBC-Spezifikation 4.2 sowie 4.3 (teilweise), und das Installationspaket enthält zwei JAR-Klassenbibliotheken:

| JAR                        | JDBC-Spezifikation            | JDK-Version |
| -------------------------- | ----------------------------- | ----------- |
| mssql-jdbc-7.0.0.jre10.jar | JDBC 4.3 (teilweise) und 4.2 | JDK 10.0    |
| mssql-jdbc-7.0.0.jre8.jar  | JDBC 4.2                      | JDK 8.0     |
| &nbsp;                     | &nbsp;                        | &nbsp;      |

Der Microsoft JDBC-Treiber 6.4 unterstützt die JDBC-Spezifikationen 4.1 und 4.2 sowie 4.3 (teilweise), und das Installationspaket enthält drei JAR-Klassenbibliotheken:

| JAR                       | JDBC-Spezifikation                 | JDK-Version |
| ------------------------- | ---------------------------------- | ----------- |
| mssql-jdbc-6.4.0.jre9.jar | JDBC 4.3 (teilweise), 4.2 und 4.1 | JDK 9.0     |
| mssql-jdbc-6.4.0.jre8.jar | JDBC 4.2 und 4.1                  | JDK 8.0     |
| mssql-jdbc-6.4.0.jre7.jar | JDBC 4.1                           | JDK 7.0     |
| &nbsp;                    | &nbsp;                             | &nbsp;      |

Der Microsoft JDBC-Treiber 6.2 unterstützt die JDBC-Spezifikationen 4.0, 4.1 und 4.2, und das Installationspaket enthält zwei JAR-Klassenbibliotheken:

| JAR                       | JDBC-Spezifikation     | JDK-Version |
| ------------------------- | ---------------------- | ----------- |
| mssql-jdbc-6.2.2.jre8.jar | JDBC 4.2, 4.1 und 4.0 | JDK 8.0     |
| mssql-jdbc-6.2.2.jre7.jar | JDBC 4.1 und 4.0       | JDK 7.0     |
| &nbsp;                    | &nbsp;                 | &nbsp;      |

Die Microsoft JDBC-Treiber 6.0 und 4.2 für SQL Server unterstützen die JDBC-Spezifikationen 4.0, 4.1 und 4.2, und das Installationspaket enthält zwei JAR-Klassenbibliotheken:

| JAR           | JDBC-Spezifikation     | JDK-Version |
| ------------- | ---------------------- | ----------- |
| sqljdbc42.jar | JDBC 4.2, 4.1 und 4.0 | JDK 8.0     |
| sqljdbc41.jar | JDBC 4.1 und 4.0       | JDK 7.0     |
| &nbsp;        | &nbsp;                 | &nbsp;      |

Der Microsoft JDBC-Treiber 4.1 für SQL Server unterstützt die JDBC-Spezifikation 4.0, und das Installationspaket enthält eine JAR-Klassenbibliothek:

| JAR           | JDBC-Spezifikation | JDK-Version     |
| ------------- | ------------------ | --------------- |
| sqljdbc41.jar | JDBC 4.0           | JDK 7.0 und 6.0 |
| &nbsp;        | &nbsp;             | &nbsp;      |

**Muss ich Änderungen am Code meiner Anwendung vornehmen, um den neuesten Treiber mit meiner bestehenden Version von SQL Server verwenden zu können?**  
In der Regel wird der Treiber abwärtskompatibel entworfen, sodass Sie Ihre vorhandenen Anwendungen bei einem Treiberupgrade nicht anpassen müssen. Wenn in einer neuen Treiberversion eine erhebliche Änderung eingeführt wird, enthält der Abschnitt [Versionshinweise zum JDBC-Treiber](../../connect/jdbc/release-notes-for-the-jdbc-driver.md) detaillierte Informationen zu dieser Änderung und zu ihren Auswirkungen auf bestehende Anwendungen. Darüber hinaus finden Sie in den Versionsanmerkungen des Treibers eine Liste bekannter Probleme und der in diesem Release behobenen Fehler.

**Wie viel kostet der Treiber?**  
Der Microsoft JDBC-Treiber für SQL steht Ihnen kostenlos zur Verfügung.

**Kann ich den Treiber weiterverteilen?**  
Die JDBC-Treiber 6.0, 6.2, 6.4 und 7.0 können weiterverteilt werden. Lesen Sie in den jeweiligen Lizenzverträgen die Klausel „Verteilbarer Code“.

**Kann ich mithilfe des Treibers von einem Linux-Computer aus auf Microsoft SQL Server zugreifen?**  
Ja. Sie können den Treiber verwenden, um von Linux, Unix und anderen nicht-Windows-Plattformen auf SQL Server zuzugreifen. Weitere Informationen finden Sie unter [Supportmatrix für den Microsoft JDBC-Treiber für SQL Server](../../connect/jdbc/microsoft-jdbc-driver-for-sql-server-support-matrix.md).

**Unterstützt der Treiber die TLS-Verschlüsselung?**  
Ab Version 1.2 unterstützt der Treiber die Transport Layer Security-Verschlüsselung (TLS), zuvor als Secure Sockets Layer (SSL) bezeichnet. Weitere Informationen finden Sie unter [Verwenden von Verschlüsselung](../../connect/jdbc/using-ssl-encryption.md).

**Welche Authentifizierungstypen werden vom Microsoft JDBC-Treiber für SQL Server unterstützt?**  
Die verfügbaren Authentifizierungsoptionen sind in der folgenden Tabelle aufgelistet. Eine reine Java Kerberos-Authentifizierung ist seit Version 4.0 des Treibers verfügbar.

| Plattform    | Authentifizierung                        |
| ----------- | ------------------------------------- |
| Nicht-Windows-System | Reine Java Kerberos                    |
| Nicht-Windows-System | SQL Server                            |
| Nicht-Windows-System | Azure Active Directory-Authentifizierung |
| Nicht-Windows-System | NTLM                                  |
| Windows     | Reine Java Kerberos                    |
| Windows     | SQL Server                            |
| Windows     | Kerberos mit NTLM als Backup             |
| Windows     | NTLM                                  |
| Windows     | Azure Active Directory-Authentifizierung |
| &nbsp;      | &nbsp;                                |

**Unterstützt der Treiber IPv6-Internetadressen (Internetprotokoll, Version 6)?**  
Ja. Der Treiber unterstützt die Verwendung von IPv6-Adressen. Verwenden Sie die Sammlung von Verbindungseigenschaften und die Verbindungszeichenfolgeneigenschaft „serverName“. Weitere Informationen finden Sie unter [Erstellen der Verbindungs-URL](../../connect/jdbc/building-the-connection-url.md).

**Was ist die adaptive Pufferung?**  
Die adaptive Pufferung wurde in Version 1.2 des Microsoft JDBC-Treibers für SQL Server 2005 eingeführt. Mit der adaptiven Pufferung können Daten mit einer großen Menge an Werten ohne den durch Servercursor verursachten Overhead abgerufen werden. Die adaptive Pufferung des Microsoft SQL Server JDBC Drivers stellt mit „responseBuffering“ eine Eigenschaft für Verbindungszeichenfolgen bereit, die auf „adaptive“ oder „full“ festgelegt werden kann. In Version 1.2 lautet der Puffermodus standardmäßig „full“, und die Anwendung muss den Modus für die adaptive Pufferung explizit festlegen. Ab Version 2.0 des JDBC-Treibers ist das Standardverhalten des Treibers „adaptive“. Daher muss Ihre Anwendung das adaptive Verhalten nicht explizit anfordern, um ein adaptives Pufferverhalten zu erzielen. Weitere Informationen finden Sie unter [Using Adaptive Buffering (Verwenden der adaptiven Pufferung)](../../connect/jdbc/using-adaptive-buffering.md) und im Blogbeitrag [What is adaptive response buffering and why should I use it? (Was ist die adaptive Pufferung und warum sollte ich sie verwenden?)](/archive/blogs/jdbcteam/).

**Unterstützt der Treiber das Verbindungspooling?**  
Der Treiber unterstützt das Verbindungspooling unter Java Platform, Enterprise Edition 5 (Java EE 5). Der Treiber implementiert die erforderlichen JDBC 3.0-Schnittstellen, damit er implementiertes Verbindungspooling von Anbietern für Anwendungsserver auf Middleware-Ebene nutzen kann. Der Treiber nutzt in diesen Umgebungen Verbindungspool. Weitere Informationen finden Sie unter [Verwenden des Verbindungspoolings](../../connect/jdbc/using-connection-pooling.md). Der Treiber bietet keine eigene Implementierung des Poolings, sondern es beruht auf Java-Anwendungsservern von Drittanbietern.

**Steht für den Treiber Support zur Verfügung?**  
Es sind zahlreiche Optionen verfügbar. Sie können Ihre Frage oder Ihr Problem in unserem [GitHub-Repository](https://github.com/microsoft/mssql-jdbc) posten, das von Microsoft überwacht wird. [Foren](https://go.microsoft.com/fwlink/?LinkID=246673) werden von Microsoft, MVPs und der Community überwacht. Sie können sich auch an den Microsoft Kundendienst wenden. Das Entwicklungsteam bittet Sie möglicherweise, das Problem außerhalb von Anwendungsservern von Drittanbietern zu reproduzieren. Wenn das Problem nicht außerhalb der Hostumgebung des Java-Containers reproduziert werden kann, müssen Sie den Drittanbieter hinzuziehen, damit das Team Ihnen weiterhin helfen kann. Außerdem bittet Sie das Team möglicherweise, Ihr Problem unter einem Betriebssystem wie Windows zu reproduzieren, damit die optimale Lösung für das Problem gefunden werden kann.

**Ist der Treiber für die Verwendung mit Anwendungsservern von Drittanbietern zertifiziert?**  
Der Treiber wurde mit allen wichtigen Anwendungsserver getestet, wie z. B. IBM WebSphere und SAP NetWeaver.

**Wie aktiviere ich die Ablaufverfolgung?**  
Der Treiber unterstützt die Verwendung der Ablaufverfolgung (oder Protokollierung). Dies hilft Probleme zu lösen, die bei der Verwendung des JDBC-Treibers in Ihrer Anwendung auftreten können. Der JDBC-Treiber verwendet die Logging-API in java.util.logging, um die clientseitige JAR-Ablaufverfolgung zu aktivieren. Weitere Informationen finden Sie unter [Tracing Driver Operation (Ablaufverfolgung für Treibervorgänge)](../../connect/jdbc/tracing-driver-operation.md). Weitere Informationen zur serverseitigen XA-Ablaufverfolgung finden Sie unter [Verfolgung des Datenzugriffs in SQL Server](/previous-versions/sql/sql-server-2012/hh880086(v=msdn.10)).

**Wo kann ich ältere Versionen des Treibers herunterladen, z.B. den JDBC-Treiber für SQL Server 2000, den 2005-Treiber oder Treiber für 1.0, 1.1 oder 1.2?**  
Diese Treiberversionen stehen nicht zum Download zur Verfügung, da sie nicht mehr unterstützt werden. Wir arbeiten laufend daran, die Unterstützung der Java-Konnektivität zu verbessern. Daher raten wir dringend zur Verwendung der neuesten Version des Microsoft JDBC-Treibers.

**Ich verwende JRE 1.4. Welcher Treiber ist mit JRE 1.4 kompatibel?**  
Kunden, die SAP-Produkte verwenden und Unterstützung für JRE 1.4 benötigen, können sich an [SAP Service Marketplace](https://service.sap.com/) wenden, um den Microsoft JDBC-Treiber 1.2 zu erhalten.

**Kann der Treiber mit durch FIPS validierten Algorithmen kommunizieren?**  
Der Microsoft JDBC-Treiber enthält keine kryptografischen Algorithmen. Wenn ein Kunde Algorithmen für Betriebssysteme, Anwendungen und JVM verwendet, die entsprechend der Federal Information Processing Standards (FIPS) zulässig sind, und der Kunde den Treiber für die Verwendung dieser Algorithmen konfiguriert, dann verwendet der Treiber nur die festgelegten Algorithmen für die Kommunikation.

## <a name="see-also"></a>Weitere Informationen

[Übersicht über den JDBC-Treiber](../../connect/jdbc/overview-of-the-jdbc-driver.md)