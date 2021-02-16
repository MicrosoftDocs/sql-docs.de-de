---
title: Fortsetzen einer Verfügbarkeitsgruppendatenbank
description: In diesem Artikel wird das Fortsetzen einer angehaltenen Verfügbarkeitsdatenbank in AlwaysOn-Verfügbarkeitsgruppen mithilfe von SQL Server Management Studio, Transact-SQL oder PowerShell in SQL Server behandelt.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.availabilitygroup.resumedatamove.f1
helpviewer_keywords:
- Availability Groups [SQL Server], resuming a database
- secondary databases [SQL Server], in availability group
- primary databases [SQL Server], in availability group
- Availability Groups [SQL Server], databases
ms.assetid: 20e9147b-e985-4caa-910e-fc4b38dbf9a1
author: cawrites
ms.author: chadam
ms.openlocfilehash: db200b1884b47ddc684646f529958095163d903a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344534"
---
# <a name="resume-an-availability-database-sql-server"></a>Fortsetzen einer Verfügbarkeitsdatenbank (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Eine angehaltene Verfügbarkeitsdatenbank können Sie in [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] mithilfe von [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]oder PowerShell in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]fortsetzen. Beim Fortsetzen einer angehaltenen Datenbank wechselt die Datenbank in den Status SYNCHRONIZING. Beim Fortsetzen der primären Datenbank werden auch ihre sekundären Datenbanken fortgesetzt, die beim Anhalten der primären Datenbank angehalten wurden. Wenn eine sekundäre Datenbank lokal von der Serverinstanz, die das sekundäre Replikat hostet, angehalten wurde, muss diese sekundäre Datenbank lokal fortgesetzt werden. Sobald sich eine bestimmte sekundäre Datenbank und die entsprechende primäre Datenbank im Status SYNCHRONIZING befinden, wird die Datensynchronisierung auf der sekundären Datenbank fortgesetzt.  
  
> [!NOTE]  
>  Das Anhalten und Fortsetzen einer sekundären AlwaysOn-Datenbank wirkt sich nicht direkt auf die Verfügbarkeit der primären Datenbank aus. Das Anhalten einer sekundären Datenbank kann jedoch sich auf Redundanz- und Failoverfunktionen für die primäre Datenbank auswirken, bis die angehaltene sekundäre Datenbank fortgesetzt wird. Dies steht im Gegensatz zur Datenbankspiegelung, bei der der Spiegelungsstatus sowohl in der Spiegeldatenbank als auch in der Prinzipaldatenbank angehalten wird, bis die Spiegelung fortgesetzt wird. Durch Anhalten einer primären AlwaysOn-Datenbank wird die Datenverschiebung auf allen entsprechenden sekundären Datenbanken angehalten, und Redundanz- und Failoverfunktionen für diese Datenbank werden deaktiviert, bis die primäre Datenbank fortgesetzt wird.  
  
  
  
## <a name="limitations-and-restrictions"></a>Einschränkungen  
 Ein RESUME-Befehl gibt einen Wert zurück, sobald es vom Replikat akzeptiert wurde, das die Zieldatenbank hostet. Das Fortsetzen der Datenbank ist jedoch dadurch asynchron.  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Voraussetzungen  
  
-   Sie müssen mit der Serverinstanz verbunden sein, auf der die fortzusetzende Datenbank gehostet wird.    
-   Die Verfügbarkeitsgruppe muss online sein.    
-   Die primäre Datenbank muss online und verfügbar sein.  
  
  
##  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Erfordert die ALTER-Berechtigung für die Datenbank.  
  
 Erfordert die ALTER AVAILABILITY GROUP-Berechtigung für die Verfügbarkeitsgruppe, die CONTROL AVAILABILITY GROUP-Berechtigung, die ALTER ANY AVAILABILITY GROUP-Berechtigung oder die CONTROL SERVER-Berechtigung.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
 **So setzen Sie eine sekundäre Datenbank fort**  
  
1.  Stellen Sie im Objekt-Explorer eine Verbindung mit der Serverinstanz mit dem Verfügbarkeitsreplikat her, auf der eine Datenbank fortgesetzt werden soll, und erweitern Sie die Serverstruktur.  
  
2.  Erweitern Sie den Knoten **Hohe Verfügbarkeit (immer aktiviert)** und den Knoten **Verfügbarkeitsgruppen** .  
  
3.  Erweitern Sie die Verfügbarkeitsgruppe.  
  
4.  Erweitern Sie den Knoten **Verfügbarkeitsdatenbanken** , klicken Sie mit der rechten Maustaste auf die Datenbank, und klicken Sie auf **Datenverschiebung fortsetzen**.  
  
5.  Klicken Sie im Dialogfeld **Datenverschiebung fortsetzen** auf **OK**.  
  
> [!NOTE]  
>  Wiederholen Sie zum Fortsetzen weiterer Datenbanken in diesem Replikatspeicherort Schritt 4 und 5 für jede Datenbank.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
 **So setzen Sie eine sekundäre Datenbank fort, die lokal angehalten wurde**  
  
1.  Stellen Sie eine Verbindung mit der Serverinstanz her, die das sekundäre Replikat hostet, dessen Datenbank Sie fortsetzen möchten.  
  
2.  Setzen Sie die sekundäre Datenbank mithilfe der folgenden [ALTER DATABASE](../../../t-sql/statements/alter-database-transact-sql-set-hadr.md)-Anweisung fort:  

     ALTER DATABASE *database_name* SET HADR RESUME;
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> PowerShell  
 **So setzen Sie eine sekundäre Datenbank fort**  
  
1.  Wechseln Sie mit **cd** in das Verzeichnis der Serverinstanz, die das Replikat hostet, dessen Datenbank Sie fortsetzen möchten. Weitere Informationen finden Sie weiter oben in diesem Thema unter [Voraussetzungen](#Prerequisites).  
  
2.  Verwenden Sie das Cmdlet **Resume-SqlAvailabilityDatabase** , um die Verfügbarkeitsgruppe fortzusetzen.  
  
     Beispielsweise wird durch den folgenden Befehl die Datensynchronisierung für die Verfügbarkeitsdatenbank `MyDb3` in der Verfügbarkeitsgruppe `MyAg`fortgesetzt.  
  
    ```  
    Resume-SqlAvailabilityDatabase `   
    -Path SQLSERVER:\Sql\Computer\Instance\AvailabilityGroups\MyAg\Databases\MyDb3  
    ```  
  
    > [!NOTE]  
    >  Um die Syntax eines Cmdlets anzuzeigen, verwenden Sie das **Get-Help** -Cmdlet in der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell-Umgebung. Weitere Informationen finden Sie unter [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Einrichten und Verwenden des SQL Server PowerShell-Anbieters**  
  
-   [SQL Server PowerShell-Anbieter](../../../powershell/sql-server-powershell-provider.md)  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
  
-   [Anhalten einer Verfügbarkeitsdatenbank &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/suspend-an-availability-database-sql-server.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Übersicht zu AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
