---
title: Konfigurieren des Enclave-Typs für die Always Encrypted-Serverkonfigurationsoption | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Secure Enclaves für Always Encrypted aktivieren oder deaktivieren. Außerdem erfahren Sie, wie Sie überprüfen, ob eine Enclave ordnungsgemäß initialisiert wurde.
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.prod_service: security
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: a35a08152dcb1c551c493ac88b0073f07e1280dd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100059485"
---
# <a name="configure-the-enclave-type-for-always-encrypted-server-configuration-option"></a>Konfigurieren des Enclave-Typs für die Always Encrypted-Serverkonfigurationsoption

[!INCLUDE [sqlserver2019-windows-only](../../includes/applies-to-version/sqlserver2019-windows-only.md)]

In diesem Artikel wird beschrieben, wie Sie eine Secure Enclave für Always Encrypted mit Secure Enclaves aktivieren oder deaktivieren. Weitere Informationen finden Sie unter [Always Encrypted mit Secure Enclaves](../../relational-databases/security/encryption/always-encrypted-enclaves.md) und [Konfigurieren des Enclave-Typs für die Always Encrypted-Serverkonfigurationsoption](../../relational-databases/security/encryption/always-encrypted-enclaves-configure-enclave-type.md).

Die Serverkonfigurationsoption **column encryption enclave type** steuert den Typ einer für Always Encrypted verwendeten Secure Enclave. Die Option kann auf einen der folgenden Werte festgelegt werden:  
  
|Wert|BESCHREIBUNG|  
|-------------------|-----------------| 
|0|**Keine Secure Enclave**. Die [!INCLUDE[ssDE](../../includes/ssde-md.md)] kann die Secure Enclave für Always Encrypted nicht initialisieren. Aus diesem Grund ist die Funktionalität von Always Encrypted mit Secure Enclaves nicht verfügbar.|  
|1|**Virtualisierungsbasierte Sicherheit (VBS)** . Die [!INCLUDE[ssDE](../../includes/ssde-md.md)] versucht, eine Enclave mit virtualisierungsbasierter Sicherheit (VBS) zu initialisieren.

> [!IMPORTANT]
> Änderungen an **column encryption enclave type** treten erst in Kraft, nachdem Sie die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz neu gestartet haben.
   
Sie können den konfigurierten Wert für den Enclave-Typ und den aktiven Wert für den Enclave-Typ über die Sicht [sys.configurations (Transact-SQL)](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md) überprüfen. 

Überprüfen Sie die Sicht [sys.dm_column_encryption_enclave (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-column-encryption-enclave.md), um zu bestätigen, dass eine Enclave des aktiven Typs (größer 0) nach dem letzten Neustart von [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)] ordnungsgemäß initialisiert wurde:
 - Wenn die Sicht genau eine Zeile enthält, wurde die Enclave ordnungsgemäß initialisiert. 
 - Wenn die Sicht keine Zeilen enthält, überprüfen Sie das SQL Server-Fehlerprotokoll auf Enclave-Initialisierungsfehler. Informationen hierzu finden Sie unter [Anzeigen des SQL Server-Fehlerprotokolls (SQL Server Management Studio)](../../relational-databases/performance/view-the-sql-server-error-log-sql-server-management-studio.md).

Eine detaillierte Anleitung zum Konfigurieren einer VSB-Enclave finden Sie unter [Aktivieren von Always Encrypted mit Secure Enclaves in SQL Server](../../relational-databases/security/tutorial-getting-started-with-always-encrypted-enclaves.md#step-3-enable-always-encrypted-with-secure-enclaves-in-sql-server).

## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird die Secure Enclave aktiviert und der Enclave-Typ auf VSB festgelegt:

```sql  
sp_configure 'column encryption enclave type', 1;  
GO  
RECONFIGURE;  
GO  
```  

Das folgende Beispiel deaktiviert die Secure Enclave:  

```sql  
sp_configure 'column encryption enclave type', 0;  
GO  
RECONFIGURE;  
GO  
```  

Mit der folgenden Abfrage werden der konfigurierte Enclave-Typ und der derzeit aktive Enclave-Typ abgerufen:

```sql  
USE [master];
GO
SELECT
[value]
, CASE [value] WHEN 0 THEN 'No enclave' WHEN 1 THEN 'VBS' ELSE 'Other' END AS [value_description]
, [value_in_use]
, CASE [value_in_use] WHEN 0 THEN 'No enclave' WHEN 1 THEN 'VBS' ELSE 'Other' END AS [value_in_use_description]
FROM sys.configurations
WHERE [name] = 'column encryption enclave type'; 
```  
## <a name="next-steps"></a>Nächste Schritte
 [Verwalten von Schlüsseln für Always Encrypted mit Secure Enclaves](../../relational-databases/security/encryption/always-encrypted-enclaves-manage-keys.md)

## <a name="see-also"></a>Weitere Informationen  
 [Serverkonfigurationsoptionen &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [sys.configurations (Transact-SQL)](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md)   
 [sys.dm_column_encryption_enclave (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-column-encryption-enclave.md)   
