---
description: Set the Service Startup Account for SQL Server Agent (SQL Server Configuration Manager)
title: Festlegen des Dienststartkontos
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent, service accounts
- startup accounts [SQL Server]
- service startup accounts [SQL Server Agent]
ms.assetid: 46ffe818-ebb5-43a0-840b-923f219a2472
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 30944838b0ed66bc87bc49ff26fbba0efc05a683
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344698"
---
# <a name="set-the-service-startup-account-for-sql-server-agent-sql-server-configuration-manager"></a>Set the Service Startup Account for SQL Server Agent (SQL Server Configuration Manager)

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance) werden derzeit die meisten, aber nicht alle, SQL Server-Agent-Features unterstützt. Details dazu finden Sie unter [T-SQL-Unterschiede zwischen Azure SQL Managed Instance und SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Das Dienststartkonto des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agents definiert das Windows-Konto, unter dem der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent ausgeführt wird, sowie die zugehörigen Netzwerkberechtigungen. In diesem Thema wird beschrieben, wie Sie das Dienstkonto für den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent mit dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Konfigurations-Manager in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]festlegen.  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Vorbereitungen  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>Einschränkungen  
  
-   Seit [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]muss das Dienststartkonto für den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent nicht mehr ein Mitglied der Administratorengruppe von [!INCLUDE[msCoName](../../includes/msconame_md.md)] sein. Das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent-Dienststartkonto muss jedoch ein Mitglied der festen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Serverrolle „sysadmin“ sein. Das Konto muss Mitglied der „msdb“-Datenbankrolle „TargetServersRole“ auf dem Masterserver sein, um die Multiserver-Auftragsverarbeitung verwenden zu können.  
  
-   Der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent-Knoten wird nur im Objekt-Explorer angezeigt, wenn Sie die Berechtigung besitzen, ihn zu verwenden.  
  
### <a name="security"></a><a name="Security"></a>Sicherheit  
  
#### <a name="permissions"></a><a name="Permissions"></a>Berechtigungen  
Der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent muss zur Verwendung der Anmeldeinformationen eines Kontos konfiguriert werden, das Mitglied der festen Serverrolle **sysadmin** in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]ist, um seine Funktionen ausführen zu können. Das Konto muss über die folgenden Windows-Berechtigungen verfügen:  
  
-   Anmelden als Dienst (SeServiceLogonRight)  
  
-   Ersetzen von Token auf Prozessebene (SeAssignPrimaryTokenPrivilege)  
  
-   Umgehen von durchsuchenden Prüfungen (SeChangeNotifyPrivilege)  
  
-   Anpassen des Arbeitsspeicherkontingents für einen Prozess (SeIncreaseQuotaPrivilege)  
  
Weitere Informationen zu den Windows-Berechtigungen, die für das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent-Dienstkonto erforderlich sind, finden Sie unter [Auswählen eines Kontos für den SQL Server-Agent-Dienst](../../ssms/agent/select-an-account-for-the-sql-server-agent-service.md) und [Einrichten von Windows-Dienstkonten](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Verwenden von SQL Server Management Studio  
  
#### <a name="to-set-the-service-startup-account-for-sql-server-agent"></a>So legen Sie das Dienststartkonto für den SQL Server-Agent fest  
  
1.  Klicken Sie in **Registrierte Server** auf das Pluszeichen, um **Datenbank-Engine** zu erweitern.  
  
2.  Klicken Sie auf das Pluszeichen, um den Ordner **Lokale Servergruppen** zu erweitern.  
  
3.  Klicken Sie mit der rechten Maustaste auf die Serverinstanz, auf der Sie das Dienststartkonto festlegen möchten, und wählen Sie dann **SQL Server-Konfigurations-Manager** aus.  
  
4.  Klicken Sie im Dialogfeld **Benutzerkontensteuerung** auf **Ja**.  
  
5.  Wählen Sie im [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Konfigurations-Manager im Konsolenbereich **SQL Server-Dienste** aus.  
  
6.  Klicken Sie im Detailbereich mit der rechten Maustaste auf **SQL Server-Agent** _(Servername)_ , wobei *Servername* der Name der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent-Instanz ist, deren Dienststartkonto Sie ändern möchten. Klicken Sie anschließend auf **Eigenschaften**.  
  
7.  Wählen Sie im Dialogfeld **SQL Server-Agent**_(Name_des\_Servers)_ **Eigenschaften** auf der Registerkarte **Anmelden** unter **Anmelden als** eine der folgenden Optionen aus:  
  
    -   **Integriertes Konto**: Wählen Sie diese Option aus, wenn die Aufträge nur Ressourcen vom lokalen Server benötigen. Informationen zum Auswählen eines integrierten Kontotyps finden Sie unter [Auswählen eines Kontos für den SQL Server-Agent-Dienst](./select-an-account-for-the-sql-server-agent-service.md).  
  
        > [!IMPORTANT]  
        >  Das lokale Dienstkonto [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in  wird nicht für den [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]-Agentdienst unterstützt.  
  
    -   **Dieses Konto**: Wählen Sie diese Option aus, wenn die Aufträge Ressourcen aus dem gesamten Netzwerk benötigen, einschließlich Anwendungsressourcen, wenn Sie Ereignisse an andere Windows-Anwendungsprotokolle weiterleiten möchten oder wenn Sie Operatoren per E-Mail oder Pager benachrichtigen möchten.  
  
        Bei Auswahl dieser Option:  
  
        1.  Geben Sie das Konto, das verwendet wird, um den SQL Server-Agent auszuführen, in das Feld **Kontoname** ein. Klicken Sie alternativ auf **Durchsuchen** , um das Dialogfeld **Benutzer oder Gruppe auswählen** zu öffnen, und wählen Sie das zu verwendende Konto aus.  
  
        2.  Geben Sie im Feld **Kennwort** das Kennwort für das Konto ein. Geben Sie im Feld **Kennwort bestätigen** das Kennwort erneut ein.  
  
8.  Klicken Sie auf **OK**.  
  
9. Klicken Sie im [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Konfigurations-Manager auf die Schaltfläche **Schließen** .  
