---
title: Entfernen einer Instanz von SQL Server aus dem SQL Server-Hilfsprogramm | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie eine Instanz von SQL Server aus dem SQL Server-Hilfsprogramm entfernen. Sie können hierfür ein PowerShell-Skript ausführen oder SQL Server Management Studio verwenden.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
f1_keywords:
- sql13.swb.utility.remove.f1
ms.assetid: ae1d126a-46d2-47bf-b339-17c743df6491
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 3b316b0fdf68debf590d3946b24c6ae9c6c79ae9
ms.sourcegitcommit: 04cf7905fa32e0a9a44575a6f9641d9a2e5ac0f8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/07/2020
ms.locfileid: "91810688"
---
# <a name="remove-an-instance-of-sql-server-from-the-sql-server-utility"></a>Entfernen einer Instanz von SQL Server aus dem SQL Server-Hilfsprogramm
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Führen Sie die folgenden Schritte aus, um eine verwaltete Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aus dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Hilfsprogramm zu entfernen. Mit diesem Verfahren wird die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aus der UCP-Listenansicht entfernt und die Datensammlung des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Hilfsprogramms beendet. Die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wird nicht deinstalliert.  
  
> [!IMPORTANT]  
>  Bevor Sie eine Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mithilfe dieses Verfahrens aus dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Hilfsprogramm entfernen können, stellen Sie sicher, dass [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und SQL Server-Agent-Dienste auf der zu entfernenden Instanz ausgeführt werden.  
  
1.  Klicken Sie im Hilfsprogramm-Explorer in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]auf **Verwaltete Instanzen**. Achten Sie im Inhaltsbereich des Hilfsprogramm-Explorers auf die Listenansicht verwalteter [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanzen.  
  
2.  Wählen Sie in der Spalte **Name der SQL Server-Instanz** der Listenansicht die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz aus, die aus dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Hilfsprogramm entfernt werden soll. Klicken Sie mit der rechten Maustaste auf die zu entfernende Instanz, und wählen Sie **Verwaltete Instanz entfernen** aus.  
  
3.  Geben Sie Anmeldeinformationen mit Administratorrechten für die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz an: Klicken Sie auf **Verbinden**, überprüfen Sie die Informationen im Dialogfeld **Verbindung mit Server herstellen**, und klicken Sie dann auf **Verbinden**. Sie sehen die Anmeldeinformationen im Dialogfeld **Verwaltete Instanz entfernen** .  
  
4.  Um das Entfernen zu bestätigen, klicken Sie auf **OK**. Um den Vorgang zu beenden, klicken Sie auf **Abbrechen**.  

## <a name="manually-remove-a-managed-instance-of-sql-server-from-a-sql-server-utility"></a>Manuelles Entfernen einer verwalteten Instanz von SQL Server aus einem SQL Server-Hilfsprogramm  
 Mit diesem Verfahren wird die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aus der UCP-Listenansicht entfernt und die Datensammlung des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Hilfsprogramms beendet. Die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wird nicht deinstalliert.  
  
 So verwenden Sie PowerShell, um eine verwaltete Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aus dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Hilfsprogramm zu entfernen. Dieses Skript führt die folgenden Vorgänge aus:  
  
-   Ruft den UCP nach dem Serverinstanznamen ab.  
  
-   Entfernt die verwaltete Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aus dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Hilfsprogramm.  
  
```  
# Get Ucp connection  
$UcpServerInstanceName = "ComputerName\InstanceName";  
$UtilityInstance = new-object -Type Microsoft.SqlServer.Management.Smo.Server $UcpServerInstanceName;  
$UcpConnection = new-object -Type Microsoft.SqlServer.Management.Sdk.Sfc.SqlStoreConnection $UtilityInstance.ConnectionContext.SqlConnectionObject;  
$Utility = [Microsoft.SqlServer.Management.Utility.Utility]::Connect($UcpConnection);  
  
# Now remove the ManagedInstance from the SQL Server Utility  
$ServerInstanceName = "ComputerName\InstanceName";  
$Instance = new-object -Type Microsoft.SqlServer.Management.Smo.Server $ServerInstanceName;  
$InstanceConnection = new-object -Type Microsoft.SqlServer.Management.Sdk.Sfc.SqlStoreConnection $Instance.ConnectionContext.SqlConnectionObject;  
$ManagedInstance = $Utility.ManagedInstances[$ServerInstanceName];  
$ManagedInstance.Remove($InstanceConnection);  
```  
  
 Es ist wichtig, den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanznamen genau so anzugeben, wie er in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]gespeichert ist. Bei einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], bei der Groß-/Kleinschreibung beachtet wird, müssen Sie den Instanznamen mit genau der Groß-/Kleinschreibung angeben, die von @@SERVERNAME zurückgegeben wird. Um den Instanznamen für die verwaltete Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]abzurufen, führen Sie die folgende Abfrage für die verwaltete Instanz aus:  
  
```  
select @@SERVERNAME AS instance_name  
```  
  
 An diesem Punkt wird die verwaltete Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vollständig aus dem UCP entfernt. Sie wird nicht mehr in der Listenansicht angezeigt, wenn Sie das nächste Mal Daten für das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Hilfsprogramm aktualisieren. Das Ergebnis ist genauso, als würde ein Benutzer den Vorgang zum Entfernen verwalteter Instanzen erfolgreich in der SSMS-Benutzeroberfläche abschließen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Verwenden des Hilfsprogramm-Explorers zum Verwalten des SQL Server-Hilfsprogramms](../../relational-databases/manage/use-utility-explorer-to-manage-the-sql-server-utility.md)   
 [Problembehandlung beim SQL Server-Hilfsprogramm](/previous-versions/sql/sql-server-2016/ee210592(v=sql.130))  
  
