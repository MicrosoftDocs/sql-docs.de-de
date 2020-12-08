---
title: Löschen eines Ressourcenpools | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie mithilfe von SQL Server Management Studio oder Transact-SQL einen Ressourcenpool löschen. Sie müssen die Berechtigung „CONTROL SERVER“ besitzen.
ms.custom: ''
ms.date: 03/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Resource Governor, resource pool delete
- resource pools [SQL Server], delete
ms.assetid: 3bdd348b-6582-4ffa-80ef-d49e50596ce5
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 2df6daca54fe31273d61226c210285fec305769d
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/02/2020
ms.locfileid: "96504823"
---
# <a name="delete-a-resource-pool"></a>Löschen eines Ressourcenpools
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Einen Ressourcenpool können Sie in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder mit Transact-SQL löschen.  
  
-   **Vorbereitungen:**  [Begrenzungen und Einschränkungen](#LimitationsRestrictions), [Berechtigungen](#Permissions)  
  
-   **So löschen Sie einen Ressourcenpool mit:** [SQL Server Management Studio](#DelRPSSMS), [Transact-SQL](#DelRPTSQL)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
 Ressourcenpools mit Arbeitsauslastungsgruppen können nicht gelöscht werden.  
  
###  <a name="limitations-and-restrictions"></a><a name="LimitationsRestrictions"></a> Einschränkungen  
 Standardpools oder interne Ressourcenpools der Ressourcenkontrolle können nicht gelöscht werden. Ressourcenpools mit Arbeitsauslastungsgruppen können nicht gelöscht werden. Weitere Informationen finden Sie unter [Delete a Workload Group](../../relational-databases/resource-governor/delete-a-workload-group.md).  
  
###  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Zum Löschen eines Ressourcenpools ist die CONTROL SERVER-Berechtigung erforderlich.  
  
##  <a name="delete-a-resource-pool-using-object-explorer"></a><a name="DelRPSSMS"></a> Löschen eines Ressourcenpools im Objekt-Explorer  
 **So löschen Sie einen Ressourcenpool in SQL Server Management Studio**  
  
1.  Öffnen Sie in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]den Objekt-Explorer, und erweitern Sie den Knoten **Verwaltung** rekursiv, bis **Ressourcenkontrolle** angezeigt wird.  
  
2.  Klicken Sie mit der rechten Maustaste auf den zu löschenden Ressourcenpool, und klicken Sie anschließend auf **Löschen**.  
  
3.  Im Fenster **Objekt löschen** wird der Ressourcenpool in der Liste **Zu löschendes Objekt** aufgeführt. Um den Ressourcenpool zu löschen, klicken Sie auf **OK**.  

    > [!NOTE]  
    >  Falls der zu löschende Ressourcenpool eine Arbeitsauslastungsgruppe enthält, schlägt dieser Vorgang fehl.  
  
##  <a name="delete-a-resource-pool-using-transact-sql"></a><a name="DelRPTSQL"></a> Löschen eines Ressourcenpools mit Transact-SQL  
 **So löschen Sie einen Ressourcenpool mit Transact-SQL**  
  
1.  Führen Sie die **DROP RESOURCE POOL** oder die **DROP EXTERNAL RESOURCE POOL** -Anweisung aus, und geben Sie den Namen des zu löschenden Ressourcenpools an.  
  
2.  Führen Sie die **ALTER RESOURCE GOVERNOR RECONFIGURE** -Anweisung aus.  
  
### <a name="example-transact-sql"></a>Beispiel (Transact-SQL)  
 Das folgende Beispiel löscht die Arbeitsauslastungsgruppe `poolAdhoc`.  
  
```  
DROP RESOURCE POOL poolAdhoc;  
GO  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Resource Governor](../../relational-databases/resource-governor/resource-governor.md)   
 [Ressourcenpool für die Ressourcenkontrolle](../../relational-databases/resource-governor/resource-governor-resource-pool.md)   
 [Erstellen eines Ressourcenpools](../../relational-databases/resource-governor/create-a-resource-pool.md)   
 [Ändern der Einstellungen für den Ressourcenpool](../../relational-databases/resource-governor/change-resource-pool-settings.md)   
 [Arbeitsauslastungsgruppe der Ressourcenkontrolle](../../relational-databases/resource-governor/resource-governor-workload-group.md)   
 [Klassifizierungsfunktion der Ressourcenkontrolle](../../relational-databases/resource-governor/resource-governor-classifier-function.md)   
 [DROP WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/drop-workload-group-transact-sql.md)   
 [DROP RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/drop-resource-pool-transact-sql.md)   
 [ALTER RESOURCE GOVERNOR &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-governor-transact-sql.md)   
 [DROP EXTERNAL RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/drop-external-resource-pool-transact-sql.md)   
 [CREATE EXTERNAL RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-resource-pool-transact-sql.md)   
 [ALTER EXTERNAL RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/alter-external-resource-pool-transact-sql.md)  
  
  
