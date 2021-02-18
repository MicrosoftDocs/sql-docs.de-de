---
description: Gleichzeitiges Ausführen von Anweisungen für mehrere Server
title: Gleichzeitiges Ausführen von Anweisungen für mehrere Server
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- multiserver queries
- executing queries against multiple servers
- queries [SQL Server], multiserver
ms.assetid: 197760f3-0a06-43de-8162-69c27d3fbe56
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 07/18/2016
ms.openlocfilehash: 1c597dffe7806cf4c4cdf401dd117e60e3af35dc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350511"
---
# <a name="execute-statements-against-multiple-servers-simultaneously"></a>Gleichzeitiges Ausführen von Anweisungen für mehrere Server

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

In diesem Thema wird beschrieben, wie Sie in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]Abfragen gleichzeitig für mehrere Server durchführen, indem Sie eine lokale Servergruppe oder einen zentralen Verwaltungsserver und eine oder mehrere Servergruppen sowie einen oder mehrere registrierte Server innerhalb der Gruppen erstellen und anschließend eine Abfrage für die ganze Gruppe durchführen. 

Die von der Abfrage zurückgegebenen Ergebnisse können in einem einzigen Ergebnisbereich zusammengefasst oder in gesonderten Ergebnisbereichen ausgegeben werden. Das Resultset kann zusätzliche Spalten für den Servernamen und den Anmeldenamen umfassen, der für die Abfrage auf jedem einzelnen Server verwendet wird. Zentrale Verwaltungsserver und untergeordnete registrierte Server können nur mithilfe der Windows-Authentifizierung registriert werden. Server in lokalen Servergruppen können mithilfe der Windows-Authentifizierung oder der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Authentifizierung registriert werden.  
  
> **HINWEIS!** Bevor Sie die folgenden Schritte ausführen, erstellen Sie einen zentralen Verwaltungsserver und eine Servergruppe. Weitere Informationen finden Sie unter [Erstellen eines zentralen Verwaltungsservers und einer Servergruppe &#40;SQL Server Management Studio&#41;](./create-a-central-management-server-and-server-group.md).  

  
##  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Da die durch einen zentralen Verwaltungsserver verwalteten Verbindungen im Kontext des Benutzers unter Verwendung der Windows-Authentifizierung ausgeführt werden, können die gültigen Berechtigungen auf den registrierten Servern variieren. Der Benutzer kann beispielsweise für die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz A ein Mitglied der festen Serverrolle sysadmin sein, für die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz B jedoch über eingeschränkte Berechtigungen verfügen.  
  
 ## <a name="execute-statements-against-multiple-configuration-targets-simultaneously"></a>Ausführen von Anweisungen für mehrere Konfigurationsziele gleichzeitig  

1.  Klicken Sie in SQL Server Management Studio im Menü **Ansicht** auf **Registrierte Server**.  
  
2.  Erweitern Sie einen zentralen Verwaltungsserver, klicken Sie mit der rechten Maustaste auf eine Servergruppe, zeigen Sie auf **Verbinden**, und klicken Sie anschließend auf **Neue Abfrage**.  
  
3.  Geben Sie im Abfrage-Editor eine [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisung ein, und führen Sie sie aus, z. B.:  
  
    ```  
    USE master  
    GO  
    SELECT * FROM sysdatabases;  
    GO  
    ```  
  
     Standardmäßig werden im Ergebnisbereich die Abfrageergebnisse aller Servern in der Servergruppe angezeigt.  
  
#### <a name="to-change-the-multiserver-results-options"></a>So ändern Sie die Optionen für Multiserverergebnisse  
  
1.  Klicken Sie in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]im Menü **Extras** auf **Optionen**.  
  
2.  Erweitern Sie **Abfrageergebnisse**, erweitern Sie **SQL Server**, und klicken Sie dann auf **Multiserverergebnisse**.  
  
3.  Geben Sie auf der Seite **Multiserverergebnisse** die gewünschten Optionseinstellungen an, und klicken Sie dann auf **OK**.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Verwalten mehrerer Server mithilfe von zentralen Verwaltungsservern](../../relational-databases/administer-multiple-servers-using-central-management-servers.md)  
  
