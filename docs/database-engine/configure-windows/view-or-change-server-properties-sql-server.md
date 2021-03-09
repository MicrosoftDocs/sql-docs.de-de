---
title: Anzeigen oder Ändern von Servereigenschaften (SQL Server)
description: Erfahren Sie, wie Sie SQL Server Management Studio, Transact-SQL oder den SQL Server-Konfigurations-Manager verwenden, um die Eigenschaften einer SQL Server-Instanz anzuzeigen oder zu ändern.
ms.prod: sql
ms.prod_service: high-availability
ms.technology: configuration
ms.topic: conceptual
f1_keywords:
- sql13.swb.connectionproperties.f1
helpviewer_keywords:
- viewing server properties
- server properties [SQL Server]
- displaying server properties
- servers [SQL Server], viewing
- Connection Properties dialog box
ms.assetid: 55f3ac04-5626-4ad2-96bd-a1f1b079659d
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: contperf-fy20q4
ms.date: 03/14/2017
ms.openlocfilehash: 80d52c19e5fde8116f7f6fed638e1120064038d3
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186054"
---
# <a name="view-or-change-server-properties-sql-server"></a>Anzeigen oder Ändern von Servereigenschaften (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In diesem Thema wird beschrieben, wie Sie die Eigenschaften einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mit [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)]oder dem SQL Server-Konfigurations-Manager anzeigen oder ändern.  

Die Schritte sind vom Tool abhängig:
+ [SQL Server Management Studio](#SSMSProcedure)  
+ [Transact-SQL](#TsqlProcedure)  
+ [SQL Server-Konfigurations-Manager](#PowerShellProcedure)  
    
## <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Einschränkungen  
  
-   Wenn Sie sp_configure verwenden, müssen Sie nach dem Festlegen einer Konfigurationsoption entweder RECONFIGURE oder RECONFIGURE WITH OVERRIDE ausführen. Die RECONFIGURE WITH OVERRIDE-Anweisung ist in der Regel für Konfigurationsoptionen reserviert, die nur mit äußerster Vorsicht verwendet werden sollten. Sie können jedoch RECONFIGURE WITH OVERRIDE für alle Konfigurationsoptionen und auch statt RECONFIGURE verwenden.  
  
    > [!NOTE]  
    >  RECONFIGURE wird in einer Transaktion ausgeführt. Schlägt einer der RECONFIGURE-Vorgänge fehl, tritt keiner der RECONFIGURE-Vorgänge in Kraft.  
  
-   Auf einigen Eigenschaftenseiten werden Informationen angezeigt, die aus der Windows-Verwaltungsinstrumentation (WMI, Windows Management Instrumentation) abgerufen wurden. Zum Anzeigen dieser Seiten muss die WMI auf dem Computer installiert sein, auf dem [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]ausgeführt wird.  
  
## <a name="server-level-roles"></a><a name="Security"></a> Rollen auf Serverebene  
  
Weitere Informationen finden Sie unter [Rollen auf Serverebene](../../relational-databases/security/authentication-access/server-level-roles.md).  
  
Die Ausführungsberechtigungen für **sp_configure** ohne Parameter oder nur mit dem ersten Parameter werden standardmäßig allen Benutzern erteilt. Zum Ausführen von **sp_configure** mit beiden Parametern zum Ändern einer Konfigurationsoption oder zum Ausführen der RECONFIGURE-Anweisung muss einem Benutzer die ALTER SETTINGS-Berechtigung auf Serverebene erteilt worden sein. Die ALTER SETTINGS-Berechtigung ist in den festen Serverrollen **sysadmin** und **serveradmin** eingeschlossen.  
  
## <a name="sql-server-management-studio"></a><a name="SSMSProcedure"></a>SQL Server Management Studio  
  
#### <a name="to-view-or-change-server-properties"></a>So zeigen Sie Servereigenschaften an oder ändern sie  
  
1.  Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf einen Server, und klicken Sie anschließend auf **Eigenschaften**.  
  
2.  Klicken Sie im Dialogfeld **Servereigenschaften** auf eine Seite, für die Sie Serverinformationen anzeigen oder ändern möchten. Einige Eigenschaften sind schreibgeschützt.  
  
##  <a name="transact-sql"></a><a name="TsqlProcedure"></a>Transact-SQL  
  
### <a name="to-view-server-properties-by-using-the-serverproperty-built-in-function"></a>So zeigen Sie Servereigenschaften mit der integrierten SERVERPROPERTY-Funktion an  
  
1.  Stellen Sie eine Verbindung mit dem [!INCLUDE[ssDE](../../includes/ssde-md.md)]her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**. In diesem Beispiel wird die integrierte [SERVERPROPERTY](../../t-sql/functions/serverproperty-transact-sql.md) -Funktion in einer `SELECT` -Anweisung verwendet, um Informationen zum aktuellen Server zurückzugeben. Dieses Szenario ist hilfreich, wenn auf einem Windows-basierten Server mehrere Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installiert sind und der Client eine weitere Verbindung mit der Instanz herstellen muss, die auch von der aktuellen Verbindung verwendet wird.  
  
    ```sql  
    SELECT CONVERT( sysname, SERVERPROPERTY('servername'));  
    GO  
    ```  
  
### <a name="to-view-server-properties-by-using-the-sysservers-catalog-view"></a>So zeigen Sie Servereigenschaften mit der sys.servers-Katalogsicht an  
  
1.  Stellen Sie eine Verbindung mit dem [!INCLUDE[ssDE](../../includes/ssde-md.md)]her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**. In diesem Beispiel wird die [sys.servers](../../relational-databases/system-catalog-views/sys-servers-transact-sql.md) -Katalogsicht abgefragt, um den Namen (`name`) und die ID (`server_id`) des aktuellen Servers sowie den Namen des OLE DB-Anbieters (`provider`) zum Herstellen einer Verbindung mit einem Verbindungsserver zurückzugeben.  
  
    ```sql  
    USE AdventureWorks2012;   
    GO  
    SELECT name, server_id, provider  
    FROM sys.servers ;   
    GO  
  
    ```  
  
### <a name="to-view-server-properties-by-using-the-sysconfigurations-catalog-view"></a>So zeigen Sie Servereigenschaften mit der sys.configurations-Katalogsicht an  
  
1.  Stellen Sie eine Verbindung mit dem [!INCLUDE[ssDE](../../includes/ssde-md.md)]her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Kopieren Sie das folgende Beispiel in das Abfragefenster, und klicken Sie dann auf **Ausführen**. In diesem Beispiel wird die [sys.configurations](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md) -Katalogsicht abgefragt, um Informationen zu jeder Serverkonfigurationsoption des aktuellen Servers zurückzugeben. Im Beispiel werden der Name (`name`), die Beschreibung (`description`) und der Wert (`value`) der Option zurückgegeben sowie ein Hinweis dazu, ob die Option eine erweiterte Option ist (`is_advanced`).  
  
    ```wmimof  
    USE AdventureWorks2012;   
    GO  
    SELECT name, description, value, is_advanced  
    FROM sys.configurations;   
    GO  
  
    ```  
  
### <a name="to-change-a-server-property-by-using-sp_configure"></a>So ändern Sie eine Servereigenschaft mit sp_configure  
  
1.  Stellen Sie eine Verbindung mit dem [!INCLUDE[ssDE](../../includes/ssde-md.md)]her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Kopieren Sie das folgende Beispiel in das Abfragefenster, und klicken Sie dann auf **Ausführen**. In diesem Beispiel wird gezeigt, wie Sie eine Servereigenschaft mithilfe von [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) ändern. Im Beispiel wird der Wert der `fill factor` -Option in `100`geändert. Der Server muss neu gestartet werden, bevor die Änderung wirksam werden kann.  
  
```sql  
Use AdventureWorks2012;  
GO  
sp_configure 'show advanced options', 1;  
GO  
RECONFIGURE;  
GO  
sp_configure 'fill factor', 100;  
GO  
RECONFIGURE;  
GO  
```  
  
 Weitere Informationen finden Sie unter [Serverkonfigurationsoptionen &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)angezeigt oder konfiguriert wird.  
  
## <a name="sql-server-configuration-manager"></a><a name="PowerShellProcedure"></a>SQL Server-Konfigurations-Manager  
 Einige Servereigenschaften können mit dem SQL Server-Konfigurations-Manager angezeigt oder geändert werden. Sie können z. B. die Version und Edition der SQL Server-Instanz anzeigen oder den Speicherort ändern, an dem Fehlerprotokolldateien gespeichert werden. Diese Eigenschaften können auch durch Abfragen der [serverbezogenen dynamischen Verwaltungssichten und Funktionen](../../relational-databases/system-dynamic-management-views/server-related-dynamic-management-views-and-functions-transact-sql.md)angezeigt werden.  
  
### <a name="to-view-or-change-server-properties"></a>So zeigen Sie Servereigenschaften an oder ändern sie  
  
1.  Zeigen Sie im Menü **Start** auf **Alle Programme**, zeigen Sie auf [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)], zeigen Sie auf **Konfigurationstools**, und klicken Sie dann auf **SQL Server-Konfigurations-Manager**.  
  
2.  Wählen Sie in **SQL Server-Konfigurations-Manager** die Option **SQL Server-Dienste** aus.  
  
3.  Klicken Sie im Detailbereich mit der rechten Maustaste auf **SQL Server (\<**_instancename_**>)** , und klicken Sie dann auf **Eigenschaften**.  
  
4.  Ändern Sie im Dialogfeld **SQL Server (\<**_instancename_**>) Eigenschaften** die Servereigenschaften auf der Registerkarte **Dienst** oder der Registerkarte **Erweitert**, und klicken Sie dann auf **OK**.  
  
## <a name="restart-after-changes"></a><a name="FollowUp"></a>Nach Änderungen neu starten

Für einige Eigenschaften müssen Sie möglicherweise den Server neu starten, damit die Änderung wirksam werden kann.  
  
## <a name="next-steps"></a>Nächste Schritte  
 [Serverkonfigurationsoptionen &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [SET-Anweisungen (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)   
 [SERVERPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/serverproperty-transact-sql.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [Konfigurieren von WMI zum Anzeigen des Serverstatus in SQL Server-Tools](../../ssms/configure-wmi-to-show-server-status-in-sql-server-tools.md)   
 [SQL Server-Konfigurations-Manager](../../relational-databases/sql-server-configuration-manager.md)   
 [Konfigurationsfunktionen (Transact-SQL)](../../t-sql/functions/configuration-functions-transact-sql.md)   
 [Serverbezogene dynamische Verwaltungssichten und -funktionen (Transact-SQL)](../../relational-databases/system-dynamic-management-views/server-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  
