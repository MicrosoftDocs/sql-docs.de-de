---
description: Erstellen eines Zielservers
title: Erstellen eines Zielservers
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.ag.tsxwiz.complete.f1
- sql13.ag.tsxwiz.cover.f1
- sql13.ag.tsxwiz.credentials.f1
- sql13.ag.tsxwiz.msx.f1
helpviewer_keywords:
- Target Server Wizard
- SQL Server Agent jobs, target servers
- target servers [SQL Server], creating
ms.assetid: 13aabe2d-67fe-4c67-8d49-2928dd705b7a
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 06/03/2020
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 93e9a3247cdbb06d20b531e6dcdb3a383dc14529
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/02/2021
ms.locfileid: "99251050"
---
# <a name="make-a-target-server"></a>Erstellen eines Zielservers
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance) werden derzeit die meisten, aber nicht alle, SQL Server-Agent-Features unterstützt. Details dazu finden Sie unter [T-SQL-Unterschiede zwischen Azure SQL Managed Instance und SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

In diesem Thema wird beschrieben, wie Sie einen Zielserver in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)]oder SQL Server Management Objects (SMO) erstellen.  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Vorbereitungen  
  
### <a name="security"></a><a name="Security"></a>Sicherheit  
Verteilte Aufträge mit Schritten, die einem Proxy zugeordnet sind, werden im Kontext des Proxykontos auf dem Zielserver ausgeführt. Stellen Sie sicher, dass die folgenden Bedingungen erfüllt sind, da andernfalls einem Proxy zugeordnete Auftragsschritte nicht vom Masterserver auf den Zielserver heruntergeladen werden:  
  
-   Der Registrierungsunterschlüssel des Masterservers **\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\\<&#42;instance_name&#42;>\SQL Server Agent\AllowDownloadedJobsToMatchProxyName** (REG_DWORD) wird auf 1 (true) festgelegt. Dieser Unterschlüssel ist standardmäßig auf 0 (false) festgelegt.  
  
-   Auf dem Zielserver ist ein Proxykonto vorhanden, das den gleichen Namen wie das Proxykonto des Masterservers hat, unter dem der Auftragsschritt ausgeführt wird.  
  
Falls bei Auftragsschritten, die Proxykonten verwenden, beim Herunterladen vom Masterserver auf den Zielserver ein Fehler auftritt, können Sie die **error_message** -Spalte in der **sysdownloadlist** -Tabelle der **msdb** -Datenbank auf die folgenden Fehlermeldungen überprüfen:  
  
-   "Für den Auftragsschritt ist ein Proxykonto erforderlich, das Proxyabgleichen ist auf dem Zielserver aber deaktiviert."  
  
    Um diesen Fehler zu beheben, legen Sie den Registrierungsunterschlüssel **AllowDownloadedJobsToMatchProxyName** auf 1 fest.  
  
-   "Proxy nicht gefunden."  
  
    Um diesen Fehler zu beheben, stellen Sie sicher, dass auf dem Zielserver ein Proxykonto vorhanden ist, das den gleichen Namen wie das Proxykonto des Masterservers hat, unter dem der Auftragsschritt ausgeführt wird.  
  
#### <a name="permissions"></a><a name="Permissions"></a>Berechtigungen  
Berechtigungen zur Ausführung dieser Prozedur erhalten standardmäßig Mitglieder der festen Serverrolle **sysadmin** .  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Verwenden von SQL Server Management Studio  
  
#### <a name="to-make-a-target-server"></a>So erstellen Sie einen Zielserver  
  
1.  Stellen Sie im **Objekt-Explorer** eine Verbindung mit einer Instanz von [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)] her, und erweitern Sie diese dann.  
  
2.  Klicken Sie mit der rechten Maustaste auf **SQL Server-Agent**, zeigen Sie auf **Multiserveradministration**, und klicken Sie dann auf **Als Zielserver einrichten**. Der **Zielservererstellungs-Assistent** führt Sie durch die Schritte zum Erstellen eines Zielservers.  
  
3.  Wählen Sie auf der Seite **Wählen Sie einen Masterserver aus** den Masterserver aus, von dem dieser Zielserver Aufträge erhält.  
  
    **Server auswählen**  
    Stellen Sie eine Verbindung zum Masterserver her.  
  
    **Beschreibung dieses Servers**  
    Geben Sie eine Beschreibung für diesen Zielserver ein. Die Beschreibung wird vom Zielserver auf den Masterserver hochgeladen.  
  
4.  Erstellen Sie ggf. auf der Seite **Masterserver-Anmeldeinformationen** einen neuen Anmeldenamen auf dem Zielserver.  
  
    **Bei Bedarf eine neue Anmeldung erstellen und ihr Rechte für MSX zuweisen**  
    Erstellt eine neue Anmeldung auf dem Zielserver, sofern die angegebene Anmeldung noch nicht vorhanden ist.  
  
## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a>Verwenden von Transact-SQL  
  
#### <a name="to-make-a-target-server"></a>So erstellen Sie einen Zielserver  
  
1.  Stellen Sie eine Verbindung mit dem [!INCLUDE[ssDE](../../includes/ssde_md.md)]her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**. Im folgenden Beispiel wird der aktuelle Server auf dem Masterserver "AdventureWorks1" eingetragen. Der Speicherort für den aktuellen Server ist "Building 21, Room 309, Rack 5".  
  
    ```sql
    USE msdb ;  
    GO  
  
    EXEC dbo.sp_msx_enlist N'AdventureWorks1',   
        N'Building 21, Room 309, Rack 5' ;   
    GO
    ```  
  
    Weitere Informationen finden Sie unter [sp_msx_enlist (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-msx-enlist-transact-sql.md).  
  
## <a name="see-also"></a>Weitere Informationen  
[Automatisierte Verwaltung in einem Unternehmen](../../ssms/agent/automated-administration-across-an-enterprise.md)  
