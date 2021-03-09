---
description: sys.database_mirroring_endpoints (Transact-SQL)
title: sys.database_mirroring_endpoints (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.database_mirroring_endpoints_TSQL
- database_mirroring_endpoints
- sys.database_mirroring_endpoints
- database_mirroring_endpoints_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- database mirroring [SQL Server], endpoint
- HADR [SQL Server], endpoint
- database mirroring [SQL Server], catalog views
- sys.database_mirroring_endpoints catalog view
ms.assetid: f2285199-97ad-473c-a52d-270044dd862b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: a402c209d94c74a7709462a6f4db64ed73fbb318
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464621"
---
# <a name="sysdatabase_mirroring_endpoints-transact-sql"></a>sys.database_mirroring_endpoints (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Enthält eine Zeile für den Datenbankspiegelungs-Endpunkt einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Der Datenbankspiegelungs-Endpunkt unterstützt beide Sitzungen zwischen Datenbank-Spiegelungs Partnern und Zeugen und Sitzungen zwischen dem primären Replikat einer Always on Verfügbarkeits Gruppe und den zugehörigen sekundären Replikaten  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**\<inherited columns>**|-|Erbt Spalten von **sys. Endpoints** (Weitere Informationen finden Sie unter [sys. Endpoints &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-endpoints-transact-sql.md)).|  
|**role**|**tinyint**|Spiegelungsrolle. Folgende Werte sind möglich:<br /><br /> **0** = keine<br /><br /> **1** = Partner<br /><br /> **2** = Zeuge<br /><br /> **3** = alle<br /><br /> Hinweis: dieser Wert ist nur für die Daten Bank Spiegelung relevant.|  
|**role_desc**|**nvarchar(60)**|Beschreibung der Spiegelungsrolle. Folgende Werte sind möglich:<br /><br /> **NONE**<br /><br /> **Partners**<br /><br /> **Denke**<br /><br /> **ALL**<br /><br /> Hinweis: dieser Wert ist nur für die Daten Bank Spiegelung relevant.|  
|**is_encryption_enabled**|**bit**|**1** bedeutet, dass die Verschlüsselung aktiviert ist.<br /><br /> **0** bedeutet, dass die Verschlüsselung deaktiviert ist.|  
|**connection_auth**|**tinyint**|Der Typ von Verbindungsauthentifizierung, der für Verbindungen mit diesem Endpunkt erforderlich ist. Folgende Werte sind möglich:<br /><br /> **1** -NTLM<br /><br /> **2** : Kerberos<br /><br /> **3** -aushandeln<br /><br /> **4** : Zertifikat<br /><br /> **5** -NTLM, Zertifikat<br /><br /> **6** -Kerberos, Zertifikat<br /><br /> **7** -aushandeln, Zertifikat<br /><br /> **8** : Zertifikat, NTLM<br /><br /> **9** : Zertifikat, Kerberos<br /><br /> **10** -Zertifikat, Aushandlung|  
|**connection_auth_desc**|**Nvarchar (60)**|Beschreibung des Authentifizierungstyps, der für Verbindungen mit diesem Endpunkt erforderlich ist. Folgende Werte sind möglich:<br /><br /> NTLM<br /><br /> KERBEROS<br /><br /> NEGOTIATE<br /><br /> CERTIFICATE<br /><br /> NTLM, CERTIFICATE<br /><br /> KERBEROS, CERTIFICATE<br /><br /> NEGOTIATE, CERTIFICATE<br /><br /> CERTIFICATE, NTLM<br /><br /> CERTIFICATE, KERBEROS<br /><br /> CERTIFICATE, NEGOTIATE|  
|**certificate_id**|**int**|Gegebenenfalls ID des für die Authentifizierung verwendeten Zertifikats.<br /><br /> 0 = Windows-Authentifizierung wird verwendet.|  
|**encryption_algorithm**|**tinyint**|Verschlüsselungsalgorithmus. Folgende Werte sind möglich:<br /><br /> **0** -keine<br /><br /> **1** -RC4<br /><br /> **2** -AES<br /><br /> **3** -None, RC4<br /><br /> **4** -None, AES<br /><br /> **5** -RC4, AES<br /><br /> **6** -AES, RC4<br /><br /> **7** -None, RC4, AES<br /><br /> **8** -None, AES, RC4|  
|**encryption_algorithm_desc**|**nvarchar(60)**|Beschreibung des Verschlüsselungsalgorithmus. Folgende Werte sind möglich:<br /><br /> Keine<br /><br /> RC4<br /><br /> AES<br /><br /> NONE, RC4<br /><br /> NONE, AES<br /><br /> RC4, AES<br /><br /> AES, RC4<br /><br /> NONE, RC4, AES<br /><br /> NONE, AES, RC4|  
  
## <a name="remarks"></a>Bemerkungen  
  
> [!NOTE]  
>  Der RC4-Algorithmus wird nur aus Gründen der Abwärtskompatibilität unterstützt. Neues Material kann nur mit RC4 oder RC4_128 verschlüsselt werden, wenn die Datenbank den Kompatibilitätsgrad 90 oder 100 besitzt. (Nicht empfohlen.) Verwenden Sie stattdessen einen neueren Algorithmus, z. B. einen der AES-Algorithmen. In [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher können mithilfe von RC4 oder RC4_128 verschlüsselte Materialien in jedem Kompatibilitäts Grad entschlüsselt werden.  
  
## <a name="permissions"></a>Berechtigungen  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Weitere Informationen finden Sie unter [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Geben Sie die Endpunkt-URL beim Hinzufügen oder Ändern eines Verfügbarkeits Replikat &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)   
 [sys.availability_replicas &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)   
 [sys.database_mirroring &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-database-mirroring-transact-sql.md)   
 [sys.database_mirroring_witnesses &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/database-mirroring-witness-catalog-views-sys-database-mirroring-witnesses.md)   
 [Der Datenbankspiegelungs-Endpunkt &#40;SQL Server&#41;](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [FAQ: Abfragen des SQL Server-Systemkatalogs](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.yml)  
  
  
