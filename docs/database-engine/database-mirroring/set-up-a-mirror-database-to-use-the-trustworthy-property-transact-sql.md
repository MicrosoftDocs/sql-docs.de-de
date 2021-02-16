---
title: Aktivieren der vertrauenswürdigen Eigenschaft für eine gespiegelte Datenbank
description: Erfahren Sie, wie Sie mithilfe von Transact-SQL in SQL Server die Datenbankeigenschaft TRUSTWORTHY in einer neu gespiegelten Datenbank aktivieren.
ms.custom: seo-lt-2019
ms.date: 03/09/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- TRUSTWORTHY database option
- mirror database [SQL Server]
- database mirroring [SQL Server], security
ms.assetid: 6993b076-78ef-453e-b0ea-e341b8e5ee3e
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: e8431286a4b19d8c2c3390bfd604cd6556d984d0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344486"
---
# <a name="set-up-a-mirror-database-to-use-the-trustworthy-property-transact-sql"></a>Einrichten der TRUSTWORTHY-Eigenschaft für eine Spiegeldatenbank (Transact-SQL)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Beim Sichern einer Datenbank wird die TRUSTWORTHY-Datenbankeigenschaft auf OFF festgelegt. Deshalb ist TRUSTWORTHY bei einer neuen Spiegeldatenbank immer auf OFF festgelegt. Muss die Datenbank nach einem Failover vertrauenswürdig sein, sind zusätzliche Installationsschritte nach dem Beginn der Spiegelung erforderlich.  
  
> [!NOTE]  
>  Informationen zu dieser Datenbankeigenschaft finden Sie unter [TRUSTWORTHY-Datenbankeigenschaft](../../relational-databases/security/trustworthy-database-property.md).  
  
## <a name="procedure"></a>Verfahren  
  
#### <a name="to-setup-a-mirror-database-to-use-the-trustworthy-property"></a>So richten Sie die TRUSTWORTHY-Eigenschaft für eine Spiegeldatenbank ein  
  
1.  Stellen Sie auf der Prinzipalserverinstanz sicher, dass die TRUSTWORTHY-Eigenschaft für die Prinzipaldatenbank aktiviert ist.  
  
    ```  
    SELECT name, database_id, is_trustworthy_on FROM sys.databases   
    ```  
  
     Weitere Informationen finden Sie unter [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).  
  
2.  Stellen Sie nach dem Beginn der Spiegelung sicher, dass die Datenbank derzeit die Prinzipaldatenbank ist, die Sitzung derzeit im synchronen Betriebsmodus ausgeführt wird und die Sitzung bereits synchronisiert ist.  
  
    ```  
    SELECT database_id, mirroring_role, mirroring_safety_level_desc, mirroring_state_desc FROM sys.database_mirroring  
    ```  
  
     Weitere Informationen finden Sie unter [sys.database_mirroring &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-mirroring-transact-sql.md).  
  
3.  Nach dem Synchronisieren der Spiegelungssitzung führen Sie manuell ein Failover zur Spiegeldatenbank aus.  
  
     Dies ist entweder mithilfe von SQL Server Management Studio oder Transact-SQL möglich:  
  
    -   [Manueller Failover für eine Datenbank-Spiegelungssitzung &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/manually-fail-over-a-database-mirroring-session-sql-server-management-studio.md)  
  
    -   [Ausführen des manuellen Failovers einer Datenbank-Spiegelungssitzung &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/manually-fail-over-a-database-mirroring-session-transact-sql.md)  
  
4.  Aktivieren Sie die TRUSTWORTHY-Datenbankeigenschaft mithilfe des folgenden ALTER DATABASE-Befehls:  
  
    ```  
    ALTER DATABASE <database_name> SET TRUSTWORTHY ON  
    ```  
  
     Weitere Informationen finden Sie unter [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  
  
5.  Führen Sie optional erneut ein Failover aus, um zur ursprünglichen Prinzipaldatenbank zurückzukehren.  
  
6.  Wechseln Sie optional in den asynchronen Modus für hohe Leistung, indem Sie SAFETY auf OFF festlegen und sicherstellen, dass WITNESS ebenfalls auf OFF festgelegt ist.  
  
     In Transact-SQL:  
  
    -   [Ändern der Transaktionssicherheit in einer Datenbank-Spiegelungssitzung &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/change-transaction-safety-in-a-database-mirroring-session-transact-sql.md)  
  
    -   [Entfernen des Zeugen aus einer Datenbank-Spiegelungssitzung &#40;SQL Server&#41;](../../database-engine/database-mirroring/remove-the-witness-from-a-database-mirroring-session-sql-server.md)  
  
     In SQL Server Management Studio:  
  
    -   [Einrichten einer Datenbank-Spiegelungssitzung mithilfe der Windows-Authentifizierung &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [TRUSTWORTHY-Datenbankeigenschaft](../../relational-databases/security/trustworthy-database-property.md)   
 [Einrichten einer verschlüsselten Spiegeldatenbank](../../database-engine/database-mirroring/set-up-an-encrypted-mirror-database.md)  
  
  
