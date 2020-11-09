---
description: sys.cryptographic_providers (Transact-SQL)
title: sys.cryptographic_providers (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- cryptographic_providers
- sys.cryptographic_providers
- sys.cryptographic_providers_TSQL
- cryptographic_providers_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.cryptographic_providers catalog view
ms.assetid: 9da0da95-792e-48b4-9f60-47f0729c279c
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 305ea4a72855dd5ba136740dcc9b4321826384ad
ms.sourcegitcommit: 49ee3d388ddb52ed9cf78d42cff7797ad6d668f2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2020
ms.locfileid: "94384800"
---
# <a name="syscryptographic_providers-transact-sql"></a>sys.cryptographic_providers (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Gibt eine Zeile für jeden registrierten Kryptografieanbieter zurück.  
    
|Spaltenname|Datentyp|Beschreibung|  
|-----------------|---------------|-----------------|  
|**provider_id**|**int**|ID des Kryptografieanbieters.|  
|**name**|**sysname**|Der Name des Kryptografieanbieters.|  
|**guid**|**uniqueidentifier**|Eindeutige Anbieter-GUID.|  
|**version**|**nvarchar(50)**|Version des Anbieters im Format ' *aa.bb.cccc.dd* '.|  
|**dll_path**|**nvarchar(512)**|Pfad zur DLL, die die API (Anwendungsprogrammierschnittstelle ) der erweiterbaren Schlüsselverwaltung (Extensible Key Management, EKM) implementiert.|  
|**is_enabled**|**bit**|Gibt an, ob der Anbieter auf dem Server aktiviert ist.<br /><br /> 0 = nicht aktiviert (Standard)<br /><br /> 1 = aktiviert|  
  
## <a name="permissions"></a>Berechtigungen  
 Die **sys.cryptographic_providers** -Sicht ist öffentlich sichtbar.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Sicherheitskatalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [Verschlüsselungshierarchie](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [Erweiterbare Schlüsselverwaltung &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md)   
 [CREATE CRYPTOGRAPHIC PROVIDER &#40;Transact-SQL&#41;](../../t-sql/statements/create-cryptographic-provider-transact-sql.md)  
  
  
