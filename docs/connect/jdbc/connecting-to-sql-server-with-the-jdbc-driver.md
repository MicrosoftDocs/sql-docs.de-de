---
title: Verbinden mit SQL Server mit dem JDBC-Treiber | Microsoft-Dokumentation
description: Bei Verbindungen mit der Datenbank über den Microsoft JDBC-Treiber für SQL Server erfolgt die gesamte Interaktion mit der Datenbank über das SQLServerConnection-Objekt.
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 94bcfbe3-f00e-4774-bda8-bb7577518fec
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f88a1bbe3554b74e3dae67ddb1fad93122fc41fe
ms.sourcegitcommit: 822d4b3cfa53269535500a3db5877a82b5076728
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2020
ms.locfileid: "87988617"
---
# <a name="connecting-to-sql-server-with-the-jdbc-driver"></a>Verbinden mit SQL Server mit dem JDBC-Treiber
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Eine der grundlegenden Aufgaben, die mit [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] ausgeführt werden, ist das Herstellen einer Verbindung mit einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbank. Die gesamte Interaktion mit der Datenbank erfolgt über das [SQLServerConnection](../../connect/jdbc/reference/sqlserverconnection-class.md)-Objekt. Da der JDBC-Treiber eine extrem flache Architektur aufweist, beziehen sich nahezu alle wesentlichen Verhalten auf das SQLServerConnection-Objekt.  
  
 Wenn [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nur einen IPv6-Port überwacht, legen Sie die Systemeigenschaft „java.net.preferIPv6Addresses“ fest, um sicherzustellen, dass für die Verbindung mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] IPv6 statt IPv4 verwendet wird:  
  
```java
System.setProperty("java.net.preferIPv6Addresses", "true");  
```  
  
 Die Themen in diesem Abschnitt beschreiben, wie Sie Verbindungen mit einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbank herstellen und verwalten.  
  
## <a name="in-this-section"></a>In diesem Abschnitt  
  
|Thema|BESCHREIBUNG|  
|-----------|-----------------|  
|[Erstellen der Verbindungs-URL](../../connect/jdbc/building-the-connection-url.md)|Beschreibt das Erstellen einer Verbindungs-URL für Verbindungen mit einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbank. Darüber hinaus wird das Herstellen einer Verbindung zu benannten Instanzen einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbank beschrieben.|  
|[Festlegen von Verbindungseigenschaften](../../connect/jdbc/setting-the-connection-properties.md)|Beschreibt die verschiedenen Verbindungseigenschaften und deren Verwendung für Verbindungen mit einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbank.|  
|[Festlegen der Datenquelleneigenschaften](../../connect/jdbc/setting-the-data-source-properties.md)|Beschreibt die Verwendung von Datenquellen auf einer Java-Plattform, Enterprise Edition (Java EE).|  
|[Arbeiten mit einer Verbindung](../../connect/jdbc/working-with-a-connection.md)|Beschreibt die verschiedenen Möglichkeiten, um die Instanz einer Verbindung mit einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbank zu erstellen.|  
|[Verwenden von Verbindungspools](../../connect/jdbc/using-connection-pooling.md)|Beschreibt die Unterstützung von Verbindungspools durch den JDBC-Treiber.|  
|[Verwenden der Datenbankspiegelung &#40;JDBC&#41;](../../connect/jdbc/using-database-mirroring-jdbc.md)|Beschreibt die Unterstützung der Datenbankspiegelung durch den JDBC-Treiber.|  
|[JDBC-Treiber-Unterstützung für hohe Verfügbarkeit, Notfallwiederherstellung](../../connect/jdbc/jdbc-driver-support-for-high-availability-disaster-recovery.md)|Beschreibt das Entwickeln einer Anwendung, die eine Verbindung mit einer AlwaysOn-Verfügbarkeitsgruppe herstellt.|  
|[Verwenden der integrierten Kerberos-Authentifizierung für Verbindungen mit SQL Server](../../connect/jdbc/using-kerberos-integrated-authentication-to-connect-to-sql-server.md)|Erläutert eine Java-Implementierung für Anwendungen zum Herstellen von Verbindungen mit einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbank mit integrierter Kerberos-Authentifizierung.|  
|[Herstellen einer Verbindung mit einer Azure SQL-Datenbank](../../connect/jdbc/connecting-to-an-azure-sql-database.md)|Erläutert Verbindungsprobleme für Datenbanken in Azure SQL.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Übersicht über den JDBC-Treiber](../../connect/jdbc/overview-of-the-jdbc-driver.md)  
  
  
