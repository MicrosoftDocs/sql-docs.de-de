---
description: Webkonfigurationsreferenz (Master Data Services)
title: Webkonfigurationsreferenz
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- web configuration file [Master Data Services]
ms.assetid: b8cc9a35-97ab-4fe0-ab4b-c07f13d9793a
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: f277ecb0d3f0cc3a2e4cc62d3feff954e7c22327
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348878"
---
# <a name="web-configuration-reference-master-data-services"></a>Webkonfigurationsreferenz (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] benutzt die Datei „Web.config“, die die Konfigurationseinstellungen enthält, die Internetinformationsdienste (IIS) aktivieren, um die [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] -Webanwendung und den entsprechenden Webdienst zu hosten. Diese Datei Web.config Datei befindet sich im Ordner WebApplication des [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] Installationspfads. Weitere Informationen zu Pfaden und Berechtigungen finden Sie unter [Ordner- und Dateiberechtigungen &#40;Master Data Services&#41;](../master-data-services/folder-and-file-permissions-master-data-services.md).  
  
## <a name="webconfig-elements"></a>Web.Config-Elemente  
 Die Web.config Datei enthält [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] **\<masterDataServices>** zusätzlich zu den Konfigurationselementen Standard-IIS, .NET Framework, ASP.net und Windows Communication Foundation (WCF) ein benutzerdefiniertes Element. Die folgende Tabelle beschreibt die Elemente mit den enthaltenen Web.config-Dateien.  
  
|Konfigurationselement|BESCHREIBUNG|  
|---------------------------|-----------------|  
|**masterDataServices**|Benutzerdefiniertes Element. Verbindet den [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] -Web-Service mit einer [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] -Datenbank.|  
|**connectionStrings**|ASP.NET-Element. Weitere Informationen finden Sie unter [connectionStrings-Element (ASP.NET-Einstellungsschema)](/previous-versions/dotnet/netframework-4.0/bf7sd233(v=vs.100)) in der MSDN Library.|  
|**System. Web**|ASP.NET-Element. Weitere Informationen finden Sie unter [system.web-Element (ASP.NET-Einstellungsschema)](/previous-versions/dotnet/netframework-4.0/dayb112d(v=vs.100)) in der MSDN Library.|  
|**Starts**|.NET Framework-Element. Weitere Informationen finden Sie unter [ \<startup> Element](/dotnet/framework/configure-apps/file-schema/startup/startup-element) in der MSDN Library.|  
|**Laufzeit**|.NET Framework-Element. Weitere Informationen finden Sie unter [ \<runtime> Element](/dotnet/framework/configure-apps/file-schema/runtime/runtime-element) in der MSDN Library.|  
|**system.codedom**|.NET Framework-Element. Weitere Informationen finden Sie unter [ \<system.codedom> Element](/dotnet/framework/configure-apps/file-schema/compiler/system-codedom-element) in der MSDN Library.|  
|**System. Web. Extensions**|ASP.NET-Element. Weitere Informationen finden Sie unter [system.web.extensions-Element (ASP.NET-Einstellungsschema)](/previous-versions/dotnet/netframework-4.0/bb546044(v=vs.100)) in der MSDN Library.|  
|**system.webServer**|Abschnittsgruppe, die IIS-Elemente enthält. Weitere Informationen finden Sie unter [system.webServer Section Group \[IIS 7 Settings Schema\]](/previous-versions/iis/settings-schema/ms689429(v=vs.90)) (Abschnittsgruppe „system.webServer“ [IIS 7-Einstellungsschema]) in der MSDN Library.|  
|**system.serviceModel**|WCF-Element. Weitere Informationen finden Sie unter [\<system.serviceModel>](/dotnet/framework/configure-apps/file-schema/wcf/system-servicemodel) in der MSDN Library.|  
|**system.diagnostics**|.NET Framework-Element. Weitere Informationen finden Sie unter [ \<system.diagnostics> Element](/dotnet/framework/configure-apps/file-schema/trace-debug/system-diagnostics-element) in der MSDN Library.|  
|**appSettings**|ASP.NET-Element. Weitere Informationen finden Sie unter [appSettings-Element (allgemeines Einstellungsschema)](/previous-versions/dotnet/netframework-4.0/ms228154(v=vs.100)) in der MSDN Library.|  
  
## <a name="masterdataservices-element"></a>masterDataServices-Element  
 Das- **\<masterDataServices>** Element ist ein benutzerdefiniertes Element, das verwendet wird, um einen [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] Webdienst mit einer-Datenbank zu verbinden [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] .  
  
### <a name="syntax"></a>Syntax  
  
```  
<masterDataServices>  
   <instance virtualPath="path" siteName="name" connectionName="name" serviceName="name" />  
</masterDataServices>  
```  
  
### <a name="elements-and-attributes"></a>Elemente und Attribute  
  
|Element|BESCHREIBUNG|  
|----------|-----------------|  
|**instance**|Untergeordnetes Element. Enthält Attribute, die Informationen für den Webdienst und Datenbankverbindungszeichenfolge angeben.|  
|**virtualPath**|Attribute. Gibt den virtuellen Pfad der [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] -Webanwendung und des entsprechenden Diensts an. Dies entspricht dem **path** -Attribut des- **\<application>** Elements unter dem- **\<site>** Element in der IIS-ApplicationHost.config Datei.|  
|**Sitename**|Attribute. Gibt den Namen der Website an, die die [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] -Webanwendung und den entsprechenden Dienst hostet. Dies entspricht dem **Name** -Attribut des- **\<site>** Elements unter **\<sites>** in der IIS-ApplicationHost.config Datei.|  
|**ConnectionName**|Attribute. Gibt den Namen der zu verwendeten Verbindung an. Dies entspricht dem **Name** -Attribut des- **\<add>** Elements unter dem- **\<connectionStrings>** Element in Web.config.|  
|**Service Name**|Attribute. Gibt den Namen des Web-Services an. Dies entspricht dem **Name** -Attribut des- **\<service>** Elements unter dem- **\<services>** Element in Web.config.|  
  
### <a name="example"></a>Beispiel  
 Im folgenden Beispiel wird ein Dienst mit dem Namen MDS1 auf der Contoso-Website veranschaulicht und /MDs-Pfad, der eine von MDSDB angegebene Verbindungszeichenfolge verwendet.  
  
```  
<masterDataServices>  
   <instance virtualPath="/MDS" siteName="Contoso" connectionName="MDSDB" serviceName="MDS1" />  
</masterDataServices>  
```  
  
