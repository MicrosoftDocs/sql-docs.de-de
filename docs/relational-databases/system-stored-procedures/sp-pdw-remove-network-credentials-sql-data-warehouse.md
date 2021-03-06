---
description: sp_pdw_remove_network_credentials (Azure-Synapse-Analyse)
title: sp_pdw_remove_network_credentials
titleSuffix: Azure Synapse Analytics
ms.date: 03/14/2017
ms.prod_service: sql-data-warehouse, pdw
ms.reviewer: ''
ms.service: sql-data-warehouse
ms.subservice: design
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: c12696a2-5939-402b-9866-8a837ca4c0a3
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.custom: seo-dt-2019
ms.openlocfilehash: 07de301f6407caf8ab2775ed1d65dfdd290e1185
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196050"
---
# <a name="sp_pdw_remove_network_credentials-azure-synapse-analytics"></a>sp_pdw_remove_network_credentials (Azure-Synapse-Analyse)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Dadurch werden die in gespeicherten Netzwerk Anmelde Informationen [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] für den Zugriff auf eine Netzwerkdatei Freigabe entfernt. Verwenden Sie diese gespeicherte Prozedur beispielsweise, um die Berechtigung zum [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] Ausführen von Sicherungs-und Wiederherstellungs Vorgängen auf einem Server zu entfernen, der sich in Ihrem eigenen Netzwerk befindet.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql  
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
sp_pdw_remove_network_credentials 'target_server_name'  
```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="arguments"></a>Argumente  
 "*target_server_name*"  
 Gibt den Hostnamen oder die IP-Adresse des Zielservers an. Anmelde Informationen für den Zugriff auf diesen Server werden aus entfernt [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] . Dadurch werden Berechtigungen auf dem eigentlichen Zielserver, der von Ihrem eigenen Team verwaltet wird, nicht geändert oder entfernt.  
  
 *target_server_name* ist als nvarchar (337) definiert.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 „0“ (erfolgreich) oder „1“ (fehlerhaft)  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die **Alter Server State** -Berechtigung.  
  
## <a name="error-handling"></a>Fehlerbehandlung  
 Ein Fehler tritt auf, wenn das Entfernen von Anmelde Informationen auf dem Steuer Knoten und allen Computeknoten nicht erfolgreich ist.  
  
## <a name="general-remarks"></a>Allgemeine Hinweise  
 Diese gespeicherte Prozedur entfernt Netzwerk Anmelde Informationen aus dem Network Service-Konto für [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] . Das Network Service-Konto führt jede Instanz von SMP [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf dem Steuer Knoten und den Computeknoten aus. Wenn beispielsweise ein Sicherungs Vorgang ausgeführt wird, verwenden der Steuerungs Knoten und alle Computeknoten die Anmelde Informationen des Network Service-Kontos für den Zugriff auf den Zielserver.  
  
## <a name="metadata"></a>Metadaten  
 Verwenden Sie [sys.dm_pdw_network_credentials &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-network-credentials-transact-sql.md), um alle Anmelde Informationen aufzulisten und zu überprüfen, ob die Anmelde Informationen entfernt wurden.  
  
 Um Anmelde Informationen hinzuzufügen, verwenden Sie [sp_pdw_add_network_credentials &#40;Azure Synapse Analytics&#41;](../../relational-databases/system-stored-procedures/sp-pdw-add-network-credentials-sql-data-warehouse.md).  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Beispiele: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="a-remove-credentials-for-performing-a-database-backup"></a>A. Entfernen von Anmelde Informationen zum Ausführen einer Datenbanksicherung  
 Im folgenden Beispiel werden die Anmelde Informationen für Benutzername und Kennwort für den Zugriff auf den Zielserver mit der IP-Adresse 10.192.147.63 entfernt.  
  
```sql  
EXEC sp_pdw_remove_network_credentials '10.192.147.63';  
```  
  
  

