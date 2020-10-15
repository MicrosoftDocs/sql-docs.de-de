---
description: Warnungseigenschaften – Neue Warnung (Seite „Allgemein“)
title: Warnungseigenschaften – Neue Warnung (Seite „Allgemein“)
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.ag.alert.general.f1
ms.assetid: f5c11610-62e3-44df-9800-a5dc35be4a09
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 2c27fa0cc8325a31cc851511375a01210e7f9d9b
ms.sourcegitcommit: 22dacedeb6e8721e7cdb6279a946d4002cfb5da3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2020
ms.locfileid: "92036652"
---
# <a name="alert-properties---new-alert-general-page"></a>Warnungseigenschaften – Neue Warnung (Seite „Allgemein“)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]


> [!IMPORTANT]  
> In [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance) werden derzeit die meisten, aber nicht alle, SQL Server-Agent-Features unterstützt. Details dazu finden Sie unter [T-SQL-Unterschiede zwischen Azure SQL Managed Instance und SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Mithilfe dieser Seite können Sie die allgemeinen Eigenschaften von [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent-Warnungen anzeigen und ändern.  

## <a name="options"></a>Tastatur  
**Name**  
Ändern Sie den Namen der Warnung.  
  
**Aktivieren**  
Aktivieren Sie die Warnung. Wenn die Warnung nicht aktiviert ist, werden die in der Warnung angegebenen Aktionen nicht ausgeführt.  
  
**Typ**  
Wählen Sie den Typ der Warnung aus:  
  
-   **SQL Server-Ereigniswarnung** reagiert auf Meldungen im [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows-Ereignisprotokoll.  
  
-   **SQL Server-Leistungsstatuswarnung** reagiert auf eine bestimmte Bedingung in einem Leistungsindikator.  
  
-   **WMI-Ereigniswarnung** reagiert auf ein WMI-Ereignis (Windows Management Instrumentation).  
  
## <a name="sql-server-event-alert-options"></a>Optionen für die SQL Server-Ereigniswarnung  
**Datenbankname**  
Geben Sie eine Datenbank für das Ereignis an, oder geben Sie **Alle Datenbanken** an, damit auf eine Meldung unabhängig von der Datenbank reagiert wird, in der das Ereignis auftritt.  
  
**Fehlernummer**  
Geben Sie an, dass dieses Ereignis auf einen Fehler reagiert, und geben Sie die Fehlernummer an.  
  
**Severity**  
Geben Sie an, dass dieses Ereignis auf alle Meldungen innerhalb eines bestimmten Schweregrads reagiert, und geben Sie den Schweregrad an.  
  
**Warnung auslösen, wenn eine Meldung Folgendes enthält**  
Filtert Ereignisse nach einer bestimmten Zeichenfolge. Wenn diese Option ausgewählt ist, reagiert die Warnung nur auf Ereignisse, die eine bestimmte Zeichenfolge enthalten.  
  
**Meldungstext**  
Geben Sie die Zeichenfolge ein, die zum Filtern von Ereignissen verwendet werden soll.  
  
## <a name="sql-server-performance-condition-alerts"></a>SQL Server-Leistungsstatuswarnungen  
**Object**  
Geben Sie das zu überwachende Leistungsobjekt an.  
  
**Leistungsindikator**  
Geben Sie den Leistungsindikator innerhalb des Leistungsobjekts an, der überwacht werden soll.  
  
**Instanz**  
Geben Sie die Instanz des Leistungsindikators an, die überwacht werden soll.  
  
**Warnung, falls Leistungsindikator**  
Geben Sie das Verhalten des Leistungsindikators an, auf das die Warnung reagiert. Die Warnung könnte beispielsweise auf eine Bedingung reagieren, bei der der Wert des Leistungsindikators **Freier Speicherplatz in 'tempdb' (KB)** unter eine bestimmte Grenze fällt, oder auf eine Bedingung, bei der der Wert für **SQL-Kompilierungen/Sekunde** einen bestimmten Wert übersteigt.  
  
**Wert**  
Geben Sie einen Wert für den Leistungsindikator an.  
  
## <a name="wmi-event-alert-options"></a>WMI-Ereigniswarnungsoptionen  
**Namespace**  
Geben Sie den Namespace an, der für die WQL-Anweisung (WMI Query Language) verwendet werden soll. Es werden nur Namespaces auf dem Computer unterstützt, auf dem der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent ausgeführt wird.  
  
**Abfrage**  
Geben Sie die WQL-Anweisung an, die das Ereignis identifiziert, auf das die Warnung reagiert.  
  
## <a name="see-also"></a>Weitere Informationen  
[Warnungen](../../ssms/agent/alerts.md)  
[Verwenden von WQL mit dem WMI-Anbieter für Serverereignisse](../../relational-databases/wmi-provider-server-events/using-wql-with-the-wmi-provider-for-server-events.md)  
[Create an Alert Using an Error Number](../../ssms/agent/create-an-alert-using-an-error-number.md)  
[Create an Alert Using Severity Level](../../ssms/agent/create-an-alert-using-severity-level.md)  
