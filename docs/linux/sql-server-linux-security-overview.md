---
title: Sicherheitseinschränkungen für SQL Server für Linux
description: Erfahren Sie mehr über Einschränkungen für SQL Server für Linux, z. B. dass die Verwendung von in Azure Key Vault gespeicherten Schlüsseln und die erweiterbare Schlüsselverwaltung nicht unterstützt werden.
author: VanMSFT
ms.author: vanto
ms.date: 09/12/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 64da74cc-14bf-4636-a55e-8cc1fce2aaff
ms.openlocfilehash: 68aa25c93a500b90b778be1563b0d373b07653cd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346387"
---
# <a name="security-limitations-for-sql-server-on-linux"></a>Sicherheitseinschränkungen für SQL Server für Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Für SQL Server für Linux gibt es derzeit die folgenden Einschränkungen:

* Es wird eine Standardkennwortrichtlinie bereitgestellt. MUST_CHANGE ist die einzige Option, die Sie konfigurieren können. Die Option „CHECK_POLICY“ wird nicht unterstützt.
* Die erweiterbare Schlüsselverwaltung wird nicht unterstützt. 
* Das Verwenden von Schlüsseln, die im Azure Key Vault gespeichert sind, wird nicht unterstützt.
* SQL Server generiert ein eigenes selbstsigniertes Zertifikat zum Verschlüsseln von Verbindungen. SQL Server kann so konfiguriert werden, dass ein vom Benutzer bereitgestelltes Zertifikat für TLS verwendet wird. 

Weitere Informationen zu den in SQL Server verfügbaren Sicherheitsfunktionen finden Sie unter [Sicherheitscenter für SQL Server-Datenbank-Engine und Azure SQL-Datenbank](../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md).

## <a name="next-steps"></a>Nächste Schritte

Informationen zu allgemeinen Sicherheitsaufgaben finden Sie unter [Exemplarische Vorgehensweise für die Sicherheitsfeatures von SQL Server für Linux](sql-server-linux-security-get-started.md). Ein Skript, mit dem Sie die TCP-Portnummer und die SQL Server-Verzeichnisse ändern sowie Ablaufverfolgungsflags oder die Sortierung konfigurieren können, finden Sie unter [Konfigurieren von SQL Server für Linux mit dem mssql-conf-Tool](sql-server-linux-configure-mssql-conf.md).
