---
title: Zugreifen auf den WMI-Anbieter mit WQL und Skripterstellung
description: Erfahren Sie, wie Sie mit dem WMI-Anbieter auf SQL Server Dienste und Netzwerkeinstellungen zugreifen, indem Sie einen WQL-Editor oder ein Abfrage Tool oder eine Skriptsprache verwenden.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: wmi
ms.topic: reference
helpviewer_keywords:
- queries [WMI]
- scripts [WMI]
- query language [WMI]
- WMI Query Language [WMI]
- WQL [WMI]
- WMI Provider for Configuration Management, WQL
- WMI Provider for Configuration Management, scripts
ms.assetid: c1e64905-3c2b-4974-88f4-abf17cf7e289
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 52bc3bc300b1f6412ba2a4cdd3ce55ba5ca93aee
ms.sourcegitcommit: 783b35f6478006d654491cb52f6edf108acf2482
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91891250"
---
# <a name="using-wql-and-scripting-languages-with-the-wmi-provider"></a>Verwenden von WQL und Skriptsprachen mit dem WMI-Anbieter
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Verwaltungsanwendungen greifen auf [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Dienste und -Netzwerkeinstellungen mithilfe des WMI-Anbieters (Windows Management Instrumentation) für Konfigurationsverwaltungsobjekte auf zwei verschiedene Arten zu:  
  
-   Mithilfe eines WQL-Editors oder eines Abfragetools, wie beispielsweise WBEMTest.exe, zum Abfragen des Objektsatzes mit der WMI-Sprache (WQL)  
  
-   Mithilfe einer Skriptsprache, z. B. VBScript  
  
 Alternativ können [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Dienste und -Netzwerkeinstellungen programmgesteuert mithilfe von verwalteten WMI-Objekten in SMO verwaltet werden. Weitere Informationen zum Programmieren von WMI-verwalteten Objekten finden [Sie unter Verwalten von Diensten und Netzwerkeinstellungen mit dem WMI-Anbieter](../../relational-databases/server-management-objects-smo/tasks/managing-services-and-network-settings-by-using-wmi-provider.md).  
  
 Der Zugriff auf den WMI-Anbieter für die Konfigurationsverwaltung erfolgt über den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Konfigurations-Manager und über [!INCLUDE[msCoName](../../includes/msconame-md.md)] Management Console. Weitere Informationen zum Zugreifen auf den WMI-Anbieter über eine Benutzeroberfläche finden [Sie unter Verwalten von Diensten &#40;SQL Server-Konfigurations-Manager&#41;](../../database-engine/configure-windows/scm-services-connect-to-another-computer.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Zugreifen auf den WMI-Anbieter für die Konfigurations Verwaltung mit WQL](../../relational-databases/wmi-provider-configuration/access-wmi-provider-for-configuration-management-using-wql.md)   
 [Zugreifen auf den WMI-Anbieter mit VBScript](../../relational-databases/wmi-provider-configuration/access-wmi-provider-for-configuration-management-using-vbscript.md)  
  
