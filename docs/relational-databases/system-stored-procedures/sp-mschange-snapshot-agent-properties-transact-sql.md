---
title: sp_MSchange_snapshot_agent_properties (T-SQL)
description: Beschreibt die sp_MSchange_snapshot_agent_properties gespeicherte Prozedur, die verwendet wird, um die Eigenschaften für die für SQL Server-Replikation verwendeten Momentaufnahmen-Agent zu ändern.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_MSchange_snapshot_agent_properties_TSQL
- sp_MSchange_snapshot_agent_properties
helpviewer_keywords:
- sp_MSchange_snapshot_agent_properties
ms.assetid: 7947a788-3fd7-469f-84db-b03ba89a153c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f07673e9dd7b613e34a5f56d6fa4c3629d3e1540
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174499"
---
# <a name="sp_mschange_snapshot_agent_properties-transact-sql"></a>sp_MSchange_snapshot_agent_properties (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Ändert die Eigenschaften eines Momentaufnahmen-Agent Auftrags, der auf einem Verteiler mit oder einer höheren Version ausgeführt wird [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] . Diese gespeicherte Prozedur wird zum Ändern von Eigenschaften verwendet, wenn der Verleger in einer Instanz von [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] ausgeführt wird. Diese gespeicherte Prozedur wird auf dem Verteiler für die Verteilungsdatenbank ausgeführt.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_MSchange_snapshot_agent_properties [ @publisher = ] 'publisher'  
        , [ @publisher_db = ] 'publisher_db'  
        , [ @publication = ] 'publication'   
        , [ @frequency_type= ] frequency_type  
        , [ @frequency_interval= ] frequency_interval  
        , [ @frequency_subday= ] frequency_subday  
        , [ @frequency_subday_interval= ] frequency_subday_interval  
        , [ @frequency_relative_interval= ] frequency_relative_interval  
        , [ @frequency_recurrence_factor= ] frequency_recurrence_factor  
        , [ @active_start_date= ] active_start_date  
        , [ @active_end_date= ] active_end_date  
        , [ @active_start_time_of_day= ] active_start_time_of_day  
        , [ @active_end_time_of_day= ] active_end_time_of_day  
        , [ @snapshot_job_name = ] 'snapshot_agent_name'  
        , [ @publisher_security_mode = ] publisher_security_mode  
        , [ @publisher_login = ] 'publisher_login'  
        , [ @publisher_password = ] 'publisher_password'   
        , [ @job_login = ] 'job_login'  
        , [ @job_password = ] 'job_password'  
        , [ @publisher_type = ] 'publisher_type'  
```  
  
## <a name="arguments"></a>Argumente  
`[ @publisher = ] 'publisher'` Der Name des Verlegers. *Publisher* ist vom **Datentyp vom Datentyp sysname** und hat keinen Standardwert.  
  
`[ @publisher_db = ] 'publisher_db'` Der Name der Veröffentlichungs Datenbank. *publisher_db* ist vom **Datentyp vom Datentyp sysname** und hat keinen Standardwert.  
  
`[ @publication = ] 'publication'` Der Name der Veröffentlichung. *Publication* ist vom **Datentyp vom Datentyp sysname** und hat keinen Standardwert.  
  
`[ @frequency_type = ] frequency_type` Die Häufigkeit, mit der die Momentaufnahmen-Agent ausgeführt wird. *frequency_type* ist vom Datentyp **int**. die folgenden Werte sind möglich:  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**1**|Einmalig|  
|**2**|On-Demand-Streaming|  
|**4**|Täglich|  
|**8**|Wöchentlich|  
|**10**|Monatlich|  
|**20**|Monatlich, relativ zum Häufigkeitsintervall|  
|**40**|Beim Starten des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agents|  
  
`[ @frequency_interval = ] frequency_interval` Der Wert, der auf die von *frequency_type* festgelegte Häufigkeit angewendet werden soll. *frequency_interval* ist vom Datentyp **int** und hat keinen Standardwert.  
  
`[ @frequency_subday = ] frequency_subday` Die Einheiten für *freq_subday_interval*. *frequency_subday* ist vom Datentyp **int**. die folgenden Werte sind möglich:  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**1**|Einmalig|  
|**2**|Sekunde|  
|**4**|Minute|  
|**8**|Stunde|  
  
`[ @frequency_subday_interval = ] frequency_subday_interval` Das Intervall für die *frequency_subday*. *frequency_subday_interval* ist vom Datentyp **int** und hat keinen Standardwert.  
  
`[ @frequency_relative_interval = ] frequency_relative_interval` Das Datum, an dem die Momentaufnahmen-Agent ausgeführt wird. *frequency_relative_interval* ist vom Datentyp **int** und hat keinen Standardwert.  
  
`[ @frequency_recurrence_factor = ] frequency_recurrence_factor` Der von *frequency_type* verwendete Wiederholungs Faktor. *frequency_recurrence_factor* ist vom Datentyp **int** und hat keinen Standardwert.  
  
`[ @active_start_date = ] active_start_date` Das Datum, an dem die Momentaufnahmen-Agent zum ersten Mal geplant ist, formatiert als YYYYMMDD. *active_start_date* ist vom Datentyp **int** und hat keinen Standardwert.  
  
`[ @active_end_date = ] active_end_date` Das Datum, an dem der Momentaufnahmen-Agent nicht mehr geplant ist, formatiert als YYYYMMDD. *active_end_date* ist vom Datentyp **int** und hat keinen Standardwert.  
  
`[ @active_start_time_of_day = ] active_start_time_of_day` Die Tageszeit, zu der die Momentaufnahmen-Agent zum ersten Mal geplant ist. dabei wird das Format HHMMSS verwendet. *active_start_time_of_day* ist vom Datentyp **int** und hat keinen Standardwert.  
  
`[ @active_end_time_of_day = ] active_end_time_of_day` Die Tageszeit, zu der die Momentaufnahmen-Agent nicht mehr geplant ist. dabei wird das Format HHMMSS verwendet. *active_end_time_of_day* ist vom Datentyp **int** und hat keinen Standardwert.  
  
`[ @snapshot_job_name = ] 'snapshot_agent_name'` Der Name eines vorhandenen Momentaufnahmen-Agent Auftrags namens, wenn ein vorhandener Auftrag verwendet wird. *snapshot_agent_name* ist vom Datentyp **nvarchar (100)** und hat keinen Standardwert.  
  
`[ @publisher_security_mode = ] publisher_security_mode` Der Sicherheitsmodus, der vom Agent beim Herstellen einer Verbindung mit dem Verleger verwendet wird. *publisher_security_mode* ist vom Datentyp **int** und hat keinen Standardwert. **0** gibt die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Authentifizierung an, und **1** gibt die Windows-Authentifizierung an. Für nicht--Verleger muss ein Wert von **0** angegeben werden [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
`[ @publisher_login = ] 'publisher_login'` Der Anmelde Name, der beim Herstellen einer Verbindung mit dem Verleger verwendet wird. *publisher_login* ist vom **Datentyp vom Datentyp sysname** und hat keinen Standardwert. *publisher_login* muss angegeben werden, wenn *publisher_security_mode* den Wert **0** hat. Wenn *publisher_login* NULL ist und Publisher *_ * * security_mode* 1 ist, wird das in *job_login* angegebene Windows-Konto verwendet, wenn **eine** Verbindung mit dem Verleger hergestellt wird.  
  
`[ @publisher_password = ] 'publisher_password'` Das Kennwort, das beim Herstellen einer Verbindung mit dem Verleger verwendet wird. *publisher_password* ist vom Datentyp **nvarchar (524)** und hat keinen Standardwert.  
  
> [!IMPORTANT]  
>  Speichern Sie keine Authentifizierungsinformationen in Skriptdateien. Es wird empfohlen, Anmeldenamen und Kennwörter zur Laufzeit bereitzustellen, um die Sicherheit zu verbessern.  
  
`[ @job_login = ] 'job_login'` Der Anmelde Name für das Windows-Konto, unter dem der Agent ausgeführt wird. *job_login* ist vom Datentyp **nvarchar (257)** und hat keinen Standardwert. Das Windows-Konto wird stets für Agent-Verbindungen mit dem Verteiler verwendet. Sie müssen diesen Parameter angeben, wenn Sie einen neuen Auftrag des Momentaufnahme-Agents erstellen. *Dies kann nicht geändert werden für eine nicht-* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *Verleger.*  
  
`[ @job_password = ] 'job_password'` Das Kennwort für das Windows-Konto, unter dem der Agent ausgeführt wird. *job_password* ist vom **Datentyp vom Datentyp sysname** und hat keinen Standardwert. Sie müssen diesen Parameter angeben, wenn Sie einen neuen Auftrag des Momentaufnahme-Agents erstellen.  
  
> [!IMPORTANT]  
>  Speichern Sie keine Authentifizierungsinformationen in Skriptdateien. Es wird empfohlen, Anmeldenamen und Kennwörter zur Laufzeit bereitzustellen, um die Sicherheit zu verbessern.  
  
`[ @publisher_type = ] 'publisher_type'` Gibt den Verlegertyp an, wenn der Verleger nicht in einer Instanz von ausgeführt wird [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *publisher_type* ist vom **Datentyp vom Datentyp sysname**. die folgenden Werte sind möglich:  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**MSSQLSERVER**|Gibt einen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Verleger an.|  
|**Orakel**|Gibt einen standardmäßigen Oracle-Verleger an.|  
|**ORACLE GATEWAY**|Gibt einen Oracle Gateway-Verleger an.|  
  
 Weitere Informationen zu den Unterschieden zwischen einem Oracle-Verleger und einem Oracle Gateway-Verleger finden Sie unter [Übersicht über die Oracle-Veröffentlichung](../../relational-databases/replication/non-sql/oracle-publishing-overview.md).  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="remarks"></a>Bemerkungen  
 **sp_MSchange_snapshot_agent_properties** wird bei der Momentaufnahme-, Transaktions-und Mergereplikation verwendet.  
  
 Wenn Sie **sp_MSchange_snapshot_agent_properties** ausführen, müssen Sie alle Parameter angeben. Führen Sie [sp_helppublication_snapshot](../../relational-databases/system-stored-procedures/sp-helppublication-snapshot-transact-sql.md) aus, um die aktuellen Eigenschaften des Momentaufnahmen-Agent Auftrags zurückzugeben.  
  
 Wenn der Verleger auf einer Instanz von oder einer höheren [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Version ausgeführt wird, sollten Sie [sp_changepublication_snapshot](../../relational-databases/system-stored-procedures/sp-changepublication-snapshot-transact-sql.md) verwenden, um die Eigenschaften eines Momentaufnahmen-Agent Auftrags zu ändern.  
  
## <a name="permissions"></a>Berechtigungen  
 Nur Mitglieder der festen Server Rolle **sysadmin** auf dem Verteiler können **sp_MSchange_snapshot_agent_properties** ausführen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md)  
  
  
