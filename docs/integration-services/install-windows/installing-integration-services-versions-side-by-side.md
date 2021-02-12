---
description: Paralleles Installieren von Integration Services-Versionen
title: Paralleles Installieren von Integration Services-Versionen | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- interoperability and coexistence [Integration Services]
- Integration Services, interoperability and coexistence
ms.assetid: edfbcd56-012f-462e-a542-95491394fda9
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 1be851dfaf8423ec830ad0a134a6e8610789eec5
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835378"
---
# <a name="installing-integration-services-versions-side-by-side"></a>Paralleles Installieren von Integration Services-Versionen

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Sie können [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)] Integration Services (SSIS) zusammen mit früheren Versionen von SSIS installieren. Dieses Thema beschreibt einige Einschränkungen der parallelen Installationen.  
  
## <a name="designing-and-maintaining-packages"></a>Entwerfen und Verwalten von Paketen  
 Verwenden Sie SQL Server Data Tools (SSDT) für Visual Studio 2015, um Pakete zu entwerfen und zu verwalten, die auf SQL Server 2016, SQL Server 2014 oder SQL Server 2012 ausgerichtet sind. Die neuesten SSDT können Sie unter [Download der neuesten SQL Server-Datatools](../../ssdt/download-sql-server-data-tools-ssdt.md)herunterladen.  
  
 Klicken Sie auf der Eigenschaftsseite eines Integration Services-Projekts auf der Registerkarte **Allgemein** in den **Konfigurationseigenschaften** auf die Eigenschaft **TargetServerVersion** , und wählen Sie SQL Server 2016, 2014 oder 2012 aus.  
  
|Zielversion von SQL Server|Entwicklungsumgebung für SSIS-Pakete|  
|----------------------------------|-----------------------------------------------|  
|2016|SQL Server Data Tools für Visual Studio 2015|  
|2014|SQL Server Data Tools für Visual Studio 2015<br /><br /> oder<br /><br /> SQL Server Data Tools - Business Intelligence für Visual Studio 2013|  
|2012|SQL Server Data Tools für Visual Studio 2015<br /><br /> oder<br /><br /> SQL Server Data Tools – Business Intelligence für Visual Studio 2012|  
|2008|Business Intelligence Development Studio von SQL Server 2008|  
  
 Wenn Sie ein vorhandenes Paket einem bestehenden Projekt hinzufügen, wird das Paket in das Format des Projekts konvertiert.  
  
## <a name="running-packages"></a>Ausführen von Paketen  
 Sie können die [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)] -Version des **dtexec** -Hilfsprogramms oder des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agents verwenden, um die mit früheren Versionen der Entwicklungstools erstellten Integration Services-Pakete auszuführen. Wenn diese [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)] -Tools ein Paket laden, das mit einer früheren Version der Entwicklungstools erstellt wurde, wird das Paket im Arbeitsspeicher vorübergehend in das von [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] verwendete Paketformat konvertiert. Wenn das Paket Probleme verursacht, die eine erfolgreiche Konvertierung verhindern, kann das [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)] -Tool das Paket erst ausführen, wenn diese Probleme behoben wurden. Weitere Informationen finden Sie unter [Aktualisieren von Integration Services-Paketen](../../integration-services/install-windows/upgrade-integration-services-packages.md).  
  
  
