---
title: Konfigurieren allgemeiner Eigenschaften der richtlinienbasierten Verwaltung
description: In diesem Artikel erfahren Sie, wie Sie die Eigenschaften der richtlinienbasierten Verwaltung mithilfe von SQL Server Management Studio (SSMS) oder Transact-SQL (T-SQL) konfigurieren.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.dmf.PolicyManagement.f1
helpviewer_keywords:
- Policy-Based Management, configure properties
ms.assetid: 6d1e0e37-29ea-408a-a055-384984d884be
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 453ed74f518c248caac2540540eefbba3c1718b5
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2021
ms.locfileid: "99048833"
---
# <a name="configure-the-general-properties-of-policy-based-management"></a>Konfigurieren allgemeiner Eigenschaften der richtlinienbasierten Verwaltung
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In diesem Thema wird beschrieben, wie die Eigenschaften für die richtlinienbasierte Verwaltung in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mit [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[tsql](../../includes/tsql-md.md)]konfiguriert werden.  
  
 **In diesem Thema**  
  
-   **Vorbereitungen:**  
  
     [Sicherheit](#Security)  
  
-   **Konfigurieren der richtlinienbasierten Verwaltung mit:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Erfordert die Mitgliedschaft in der festen Datenbankrolle PolicyAdministratorRole.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
  
#### <a name="to-configure-policy-based-management"></a>So konfigurieren Sie die richtlinienbasierte Verwaltung  
  
1.  Klicken Sie im **Objekt-Explorer** auf das Pluszeichen, um den Server zu erweitern, auf dem Sie die Eigenschaften der richtlinienbasierten Verwaltung konfigurieren möchten.  
  
2.  Klicken Sie auf das Pluszeichen, um den Ordner **Verwaltung** zu erweitern.  
  
3.  Klicken Sie mit der rechten Maustaste auf **Richtlinienverwaltung** , und wählen Sie dann **Eigenschaften** aus.  
  
     Die folgenden Optionen sind im Dialogfeld **Richtlinienverwaltungseigenschaften** verfügbar.  
  
     **Aktiviert**  
     Gibt an, ob die richtlinienbasierte Verwaltung aktiviert ist.  
  
     **HistoryRetentionInDays**  
     Gibt die Anzahl der Tage an, wie lange der Richtlinienauswertungsverlauf beibehalten werden soll. Wenn dieser Wert 0 ist (Standardeinstellung), wird der Verlauf nicht automatisch entfernt.  
  
     **LogOnSuccess**  
     Gibt an, ob die richtlinienbasierte Verwaltung erfolgreiche Richtlinienauswertungen protokolliert.  
  
    -   Wenn dieser Wert false ist (Standardeinstellung), werden nur fehlerhafte Richtlinienauswertungen protokolliert.  
  
    -   Wenn dieser Wert true ist, werden sowohl erfolgreiche als auch fehlerhafte Richtlinienauswertungen protokolliert.  
  
4.  Wenn Sie fertig sind, klicken Sie auf **OK**.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
  
#### <a name="to-configure-policy-based-management"></a>So konfigurieren Sie die richtlinienbasierte Verwaltung  
  
1.  Stellen Sie im **Objekt-Explorer** eine Verbindung mit einer [!INCLUDE[ssDE](../../includes/ssde-md.md)]-Instanz her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**.  
  
    ```  
    -- enables Policy-Based Management   
    USE msdb;  
    GO  
    EXEC dbo.sp_syspolicy_configure   
         @name = N'Enabled',   
         @value = 1;  
    GO  
    ```  
  
 Weitere Informationen finden Sie unter [sp_syspolicy_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syspolicy-configure-transact-sql.md).  
  
  
