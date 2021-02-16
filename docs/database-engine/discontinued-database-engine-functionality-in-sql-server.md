---
title: Nicht mehr unterstützte Datenbank-Engine-Funktionen
description: In diesem Artikel erfahren Sie, welche Funktionen und Features der Datenbank-Engine seit SQL Server 2019 (15.x), SQL Server 2016 (13.x) und älteren Versionen nicht mehr zur Verfügung stehen.
ms.custom: seo-lt-2019
ms.date: 07/22/2020
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: release-landing
ms.topic: conceptual
helpviewer_keywords:
- VIA protocol
- unsupported features [SQL Server]
- SQL Mail
- discontinued functionality [SQL Server]
- RESTORE WITH DBO_ONLY
- BACKUP WITH PASSWORD
- user instances enabled
- BACKUP WITH MEDIAPASSWORD
- AWE
- SQL-DMO
- '*= and =*'
- 80 compatibility levels
- COMPUTE BY
- user instance timeout
- sp_dropalias
- COMPUTE
- SSL
- WITH APPEND
- sys.database_principal_aliases
- sp_dboption
- DATABASEPROPERTY
- FASTFIRSTROW hint
- SET DISABLE_DEF_CNST_CHK
ms.assetid: d686cdf0-d11d-4dba-9ec8-de1a5f189f25
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>= sql-server-linux-2017  || >= sql-server-2016'
ms.openlocfilehash: 2b735e98bd49e06057008a714a868cec6c9f5282
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347879"
---
# <a name="discontinued-database-engine-functionality-in-sql-server"></a>Nicht mehr unterstützte Datenbank-Engine-Funktionalität in SQL Server
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  In diesem Thema werden die [!INCLUDE[ssDE](../includes/ssde-md.md)] -Funktionen beschrieben, die in [!INCLUDE[ssnoversion](../includes/ssnoversion-md.md)]nicht mehr verfügbar sind.  

## <a name="discontinued-features-in-sssql19"></a>Nicht mehr unterstützte Funktionen in [!INCLUDE[sssql19](../includes/sssql19-md.md)]  

- Die folgenden datenbankweit gültigen Konfigurationsoptionen werden nicht mehr unterstützt:

  - `DISABLE_BATCH_MODE_ADAPTIVE_JOIN`
  - `DISABLE_BATCH_MODE_MEMORY_GRANT_FEEDBACK`
  - `DISABLE_INTERLEAVED_EXECUTION_TVF`

Aktuelle Konfigurationsoptionen finden Sie unter [ALTER DATABASE SCOPED CONFIGURATION (Transact-SQL)](../t-sql/statements/alter-database-scoped-configuration-transact-sql.md).

>[!NOTE]
>In [!INCLUDE[sssql14](../includes/sssql17-md.md)] werden alle Funktionen weiterhin unterstützt.

## <a name="discontinued-features-in-sssql15-md"></a>Nicht mehr unterstützte Funktionen in [!INCLUDE[sssql15-md](../includes/sssql16-md.md)]

- [!INCLUDE[sssql15-md](../includes/sssql16-md.md)] ist eine 64-Bit-Anwendung. Die 32-Bit-Installation wird eingestellt, obwohl einige Elemente als 32-Bit-Komponenten ausgeführt werden.  

- Kompatibilitätsgrad 90 wird nicht mehr unterstützt. Weitere Informationen finden Sie unter [ALTER DATABASE-Kompatibilitätsgrad &#40;Transact-SQL&#41;](../t-sql/statements/alter-database-transact-sql-compatibility-level.md).  

- Das ActiveX-Subsystem wird nicht mehr unterstützt. Verwenden Sie stattdessen die Befehlszeile oder PowerShell-Skripts.

- Startparameter **-h** und **-g**. Weitere Informationen finden Sie unter [Startoptionen für den Datenbank-Engine-Dienst](/previous-versions/sql/2014/database-engine/configure-windows/database-engine-service-startup-options?view=sql-server-2014&preserve-view=true).

- Die SSL-Verschlüsselung (Secure Sockets Layer) wird nicht mehr unterstützt. Verwenden Sie stattdessen Transport Layer Security (TLS). Weitere Informationen finden Sie unter [Aktivieren von verschlüsselten Verbindungen zur Datenbank-Engine](../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).

## <a name="previous-versions"></a>Vorherige Versionen

- [Nicht mehr unterstützte Datenbank-Engine-Funktionalität in SQL Server 2014](/previous-versions/sql/2014/database-engine/discontinued-database-engine-functionality-in-sql-server-2016?view=sql-server-2014&preserve-view=true)

### <a name="see-also"></a>Weitere Informationen

- [Als veraltet markierte Funktionen der Datenbank-Engine in SQL Server 2019](deprecated-database-engine-features-in-sql-server-version-15.md)
- [Als veraltet markierte Funktionen der Datenbank-Engine in SQL Server 2017](deprecated-database-engine-features-in-sql-server-2017.md)
- [Als veraltet markierte Funktionen der Datenbank-Engine in SQL Server 2016](../database-engine/deprecated-database-engine-features-in-sql-server-2016.md)
- [Wichtige Änderungen an Funktionen der Datenbank-Engine in SQL Server 2019](breaking-changes-to-database-engine-features-in-sql-server-version-15.md)
- [Wichtige Änderungen an Funktionen der Datenbank-Engine in SQL Server 2017](breaking-changes-to-database-engine-features-in-sql-server-2017.md)
- [Wichtige Änderungen an Funktionen der Datenbank-Engine in SQL Server 2016](breaking-changes-to-database-engine-features-in-sql-server-2016.md)
- [Als veraltet markierte Funktionen der SQL Server-Replikation](../relational-databases/replication/deprecated-features-in-sql-server-replication.md)