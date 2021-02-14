---
description: sp_pdw_add_network_credentials (Azure-Synapse-Analyse)
title: sp_pdw_add_network_credentials
titleSuffix: Azure Synapse Analytics
ms.date: 03/14/2017
ms.service: sql-data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 0729eeff-ac7e-43f0-80fa-ff5346a75985
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.custom: seo-dt-2019
ms.openlocfilehash: 68a4abf4e532719d67032c2eeb64a0027c1c88a5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100271797"
---
# <a name="sp_pdw_add_network_credentials-azure-synapse-analytics"></a>sp_pdw_add_network_credentials (Azure-Synapse-Analyse)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Dadurch werden Netzwerk Anmelde Informationen in gespeichert [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] und einem Server zugeordnet. Verwenden Sie diese gespeicherte Prozedur z. b., um [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] geeignete Lese-/Schreibberechtigungen zum Durchführen von Daten Bank Sicherungs-und Wiederherstellungs Vorgängen auf einem Zielserver zu erhalten, oder um eine Sicherung eines für TDE verwendeten Zertifikats zu erstellen.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql  
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
sp_pdw_add_network_credentials 'target_server_name',  'user_name', 'password'  
```  
[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="arguments"></a>Argumente  
 "*target_server_name*"  
 Gibt den Hostnamen oder die IP-Adresse des Zielservers an. [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] greift auf diesen Server mit den Anmelde Informationen für Benutzername und Kennwort zu, die an diese gespeicherte Prozedur übermittelt wurden  
  
 Verwenden Sie zum Herstellen einer Verbindung über das InfiniBand-Netzwerk die InfiniBand-IP-Adresse des Zielservers.  
  
 *target_server_name* ist als nvarchar (337) definiert.  
  
 '*user_name*'  
 Gibt den user_name an, der über Berechtigungen für den Zugriff auf den Zielserver verfügt. Wenn für den Zielserver bereits Anmelde Informationen vorhanden sind, werden Sie auf die neuen Anmelde Informationen aktualisiert.  
  
 *user_name* ist als nvarchar (513) definiert.  
  
 '*Password* ꞌ  
 Gibt das Kennwort für *user_name* an.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 „0“ (erfolgreich) oder „1“ (fehlerhaft)  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die **Alter Server State** -Berechtigung.  
  
## <a name="error-handling"></a>Fehlerbehandlung  
 Ein Fehler tritt auf, wenn das Hinzufügen von Anmelde Informationen auf dem Steuer Knoten und allen Computeknoten nicht erfolgreich ist.  
  
## <a name="general-remarks"></a>Allgemeine Hinweise  
 Diese gespeicherte Prozedur fügt dem NetworkService-Konto für Netzwerk Anmelde Informationen hinzu [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] . Das Network Service-Konto führt jede Instanz von SMP [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf dem Steuer Knoten und den Computeknoten aus. Wenn beispielsweise ein Sicherungs Vorgang ausgeführt wird, verwenden der Steuer Knoten und alle Computeknoten die Anmelde Informationen des Network Service-Kontos, um Lese-und Schreibberechtigungen für den Zielserver zu erhalten.  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Beispiele: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="a-add-credentials-for-performing-a-database-backup"></a>A. Hinzufügen von Anmelde Informationen zum Ausführen einer Datenbanksicherung  
 Im folgenden Beispiel werden die Anmelde Informationen für den Benutzernamen und das Kennwort für den Domänen Benutzer "tsattle\david" einem Zielserver mit der IP-Adresse 10.172.63.255 zugeordnet. Der Benutzer "einattle\david" verfügt über Lese-/Schreibberechtigungen für den Zielserver. [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] speichert diese Anmelde Informationen und verwendet diese zum Lesen und Schreiben auf den und vom Zielserver, sofern dies für Sicherungs-und Wiederherstellungs Vorgänge erforderlich ist.  
  
```sql  
EXEC sp_pdw_add_network_credentials '10.172.63.255', 'seattle\david', '********';  
```  
  
 Der Backup-Befehl erfordert, dass der Servername als IP-Adresse eingegeben wird.  
  
> [!NOTE]  
>  Um die Datenbanksicherung über InfiniBand auszuführen, achten Sie darauf, die InfiniBand-IP-Adresse des Sicherungs Servers zu verwenden.  
  
## <a name="see-also"></a>Weitere Informationen  
 [sp_pdw_remove_network_credentials &#40;Azure-Synapse-Analyse&#41;](../../relational-databases/system-stored-procedures/sp-pdw-remove-network-credentials-sql-data-warehouse.md)  
  
  

