---
description: Programmgesteuerte Verwaltung von ausgeführten Paketen
title: Programmgesteuerte Verwaltung von ausgeführten Paketen | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- packages [Integration Services], managing
- running packages [Integration Services]
ms.assetid: 0e91f4ac-6f29-40d7-8c28-9b82e4802c35
author: chugugrace
ms.author: chugu
ms.openlocfilehash: e73d7c3c29ab5e22f45795b9746baedf00bcb413
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347446"
---
# <a name="managing-running-packages-programmatically"></a>Programmgesteuerte Verwaltung von ausgeführten Paketen

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Wenn Sie programmgesteuert mit [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Paketen arbeiten, möchten Sie u. U. bestimmen, welche Pakete gerade ausgeführt werden. Die <xref:Microsoft.SqlServer.Dts.Runtime.Application>-Klasse des <xref:Microsoft.SqlServer.Dts.Runtime>-Namespace stellt Methoden und Klassen bereit, um diese Anforderungen zu erfüllen.  
  
 Weitere Informationen zum Überwachen von Paketen finden Sie unter [Paketverwaltung &#40;SSIS-Dienst&#41;](../../integration-services/service/package-management-ssis-service.md).  
  
 Alle in diesem Thema erläuterten Methoden erfordern einen Verweis auf die **Microsoft.SqlServer.ManagedDTS** -Assembly. Nachdem Sie den Verweis in einem neuen Projekt hinzugefügt haben, importieren Sie den <xref:Microsoft.SqlServer.Dts.Runtime>-Namespace mit einer **using**- oder **Imports**-Anweisung.  
  
> [!IMPORTANT]  
>  Die Methoden der <xref:Microsoft.SqlServer.Dts.Runtime.Application>-Klasse zum Arbeiten mit dem SSIS-Paketspeicher unterstützen nur „.“, localhost oder den Namen des lokalen Servers. Sie können "(local)" nicht verwenden.  
  
## <a name="determining-which-packages-are-currently-running"></a>Bestimmen, welche Pakete zurzeit ausgeführt werden  
 Um zu bestimmen, welche Pakete gerade auf dem angegebenen Server ausgeführt werden, rufen Sie die <xref:Microsoft.SqlServer.Dts.Runtime.Application.GetRunningPackages%2A>-Methode auf. Diese Methode gibt eine <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackages>-Auflistung von <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackage>-Objekten zurück.  
  
> [!NOTE]  
>  Administratoren können alle Pakete sehen, die zurzeit auf dem Computer ausgeführt werden; allen anderen Benutzern werden nur die Pakete angezeigt, die sie gestartet haben.  
  
## <a name="working-with-running-packages"></a>Arbeiten mit ausgeführten Paketen  
 Nachdem Sie bestimmt haben, welche Pakete zurzeit ausgeführt werden, können Sie Informationen zu den Pakten abrufen und anfordern, dass ein Paket gestoppt wird.  
  
### <a name="getting-information-about-a-running-package"></a>Abrufen von Informationen über ein ausgeführtes Paket  
 Beim Durchlaufen der <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackages>-Auflistung können Sie mithilfe der Eigenschaften des <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackage>-Objekts ein Paket suchen oder zusätzliche Informationen zu den Paketen, die ausgeführt werden, erhalten:  
  
-   <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackage.ExecutionDuration%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackage.ExecutionStartTime%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackage.InstanceID%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackage.PackageDescription%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackage.PackageID%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackage.PackageName%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackage.UserName%2A>  
  
### <a name="stopping-a-running-package"></a>Beenden eines ausgeführten Pakets  
 Sie können die <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackage.Stop%2A>-Methode eines <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackage>-Objekts aufrufen, um anzufordern, dass das Paket beendet wird. Möglicherweise gibt es eine Zeitverzögerung zwischen der Ausgabe der Stop-Anforderung und dem tatsächlichen Beenden des Pakets.  
  
## <a name="see-also"></a>Siehe auch  
 [Paketverwaltung &#40;SSIS-Dienst&#41;](../../integration-services/service/package-management-ssis-service.md)   
 [Enumerating Available Packages Programmatically](../../integration-services/run-manage-packages-programmatically/enumerating-available-packages-programmatically.md)  
  
  
