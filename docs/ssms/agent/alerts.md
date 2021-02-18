---
description: Alerts
title: Alerts
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent alerts, event types
- SQL Server Agent alerts, names
- alerts [SQL Server], performance conditions
- alerts [SQL Server], about alerts
- WMI event responses [SQL Server Agent alerts]
- events [WMI]
- performance condition alerts [SQL Server Agent]
- alerts [SQL Server], event types
- SQL Server Agent alerts, performance conditions
- SQL Server Agent alerts, about alerts
- alerts [SQL Server], names
ms.assetid: 3f57d0f0-4781-46ec-82cd-b751dc5affef
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 934fa73f84989996be6797d2119f834f7e650053
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354875"
---
# <a name="alerts"></a>Alerts

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

> [!IMPORTANT]  
> In [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance) werden derzeit die meisten, aber nicht alle, SQL Server-Agent-Features unterstützt. Details dazu finden Sie unter [T-SQL-Unterschiede zwischen Azure SQL Managed Instance und SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Ereignisse werden von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] erzeugt und in das [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows-Anwendungsprotokoll geschrieben. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent liest das Anwendungsprotokoll und vergleicht die dort festgehaltenen Ereignisse mit den von Ihnen definierten Warnungen. Wenn der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent eine Übereinstimmung findet, wird eine Warnung ausgelöst, also eine automatische Antwort auf das Ereignis. Mit dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent können Sie nicht nur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Ereignisse überwachen, sondern auch den Leistungsstatus und die WMI-Ereignisse (Windows Management Instrumentation oder Windows-Verwaltungsinstrumentation).  
  
Zur Definition einer Warnung geben Sie Folgendes an:  
  
-   Der Name der Warnung.  
  
-   Das Ereignis oder den Leistungsstatus, mit dem die Warnung ausgelöst wird.  
  
-   Die Aktion, die der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent als Reaktion auf das Ereignis oder den Leistungsstatus ausführt.  
  
## <a name="naming-an-alert"></a>Benennen einer Warnung  
Jede Warnung muss einen Namen aufweisen. Warnungsnamen müssen innerhalb der jeweiligen Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] eindeutig sein und dürfen nicht länger als **128** Zeichen lang sein.  
  
## <a name="selecting-an-event-type"></a>Auswählen eines Ereignistyps  
Eine Warnung beantwortet ein Ereignis eines bestimmten Typs. Die folgenden Ereignistypen werden mithilfe von Warnungen beantwortet:  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Ereignisse  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Leistungsbedingungen  
  
-   WMI-Ereignisse  
  
Der Typ des Ereignisses bestimmt die Parameter, mit denen Sie das jeweilige Ereignis angeben.  
  
## <a name="specifying-a-sql-server-event"></a>Angeben eines SQL Server-Ereignisses  
Sie können angeben, dass eine Warnung als Antwort auf ein Ereignis oder mehrere Ereignisse eintritt. Mit den folgenden Parametern geben Sie die Ereignisse an, die eine Warnung auslösen:  
  
-   **Fehlernummer**  
  
    [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent löst eine Warnung aus, wenn ein bestimmter Fehler eintritt. Geben Sie beispielsweise an, dass die Fehlernummer 2571 als Antwort auf den unbefugten Versuch, die Datenbank-Konsolenbefehle (Database Console Commands, DBCC) aufzurufen, ausgegeben werden soll.  
  
-   **Schweregrad**  
  
    [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent löst eine Warnung aus, wenn ein Fehler des bestimmten Schweregrades eintritt. Geben Sie beispielsweise einen Schweregrad von 15 für Syntaxfehler in Transact-SQL-Anweisungen an.  
  
-   **Datenbank**  
  
    [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent löst nur dann eine Warnung aus, wenn das Ereignis in einer bestimmten Datenbank auftritt. Diese Option kann zusätzlich zur Fehlernummer und zum Schweregrad verwendet werden. Enthält eine Instanz beispielsweise eine Datenbank für die Produktion und eine zweite Datenbank für die Berichterstellung, können Sie eine Warnung definieren, die nur bei Syntaxfehlern in der Produktionsdatenbank ausgelöst werden soll.  
  
-   **Fehlermeldungstext**  
  
    [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent löst eine Warnung aus, wenn die Ereignismeldung für das angegebene Ereignis eine bestimmte Textzeichenfolge enthält. Definieren Sie beispielsweise eine Warnung als Antwort auf Meldungen, die den Namen einer bestimmten Tabelle oder Einschränkung enthält.  
  
## <a name="selecting-a-performance-condition"></a>Auswählen einer Leistungsbedingung  
Sie können Warnungen als Reaktion auf einen bestimmten Leistungsstatus angeben. In diesem Fall geben Sie den zu überwachenden Leistungsindikator, einen Schwellwert für die Warnung sowie das Verhalten des Leistungsindikators, an, bei dem die Warnung ausgelöst werden soll. Zum Festlegen eines Leistungsstatus definieren Sie die folgenden Punkte im [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent auf der Seite **Allgemein** im Dialogfeld **Neue Warnung** oder **Eigenschaften von Warnung** :  
  
-   **Object**  
  
    Das Objekt stellt den zu überwachenden Leistungsbereich dar.  
  
-   **Leistungsindikator**  
  
    Ein Leistungsindikator ist ein Attribut des zu überwachenden Leistungsbereichs.  
  
-   **Instanz**  
  
    Die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz (sofern vorhanden) bestimmt die Instanz des zu überwachenden Attributs.  
  
-   **Warnung, falls Leistungsindikator** und **Wert**  
  
    Der Schwellwert für die Warnung sowie das Verhalten, bei dem die Warnung ausgelöst wird. Der Schwellwert ist ein numerischer Wert. Es liegt eines der **folgenden Verhalten vor: unterschreitet**, **wird gleich** oder **übersteigt eine für den Wert festgelegte Zahl**. Der **Wert** ist eine Zahl, die den Leistungsindikator beschreibt. Soll beispielsweise eine Warnung für das Leistungsobjekt **SQLServer:Sperren** ausgelöst werden, wenn die **Wartezeit für Sperre** länger als 30 Minuten ist, können Sie die Option **übersteigt** verwenden und **die Zahl 30 als Wert** angeben.  
  
    Ein weiteres Beispiel wäre, wenn Sie festlegen, dass eine  Warnung für das Leistungsobjekt **SQLServer:Transactions** ausgegeben wird, wenn der freie Speicherplatz in **tempdb** unter 1000 KB fällt. Um dies festzulegen, wählen Sie den Leistungsindikator **Freier Speicherplatz in 'tempdb' (KB)** , **Unterschreitet** und einen **Wert** von **1000** aus.  
  
    > [!NOTE]  
    > Leistungsdaten werden in regelmäßigen Abständen geprüft, was zu einer geringfügigen Verzögerung (wenige Sekunden) zwischen dem Erreichen des Schwellwerts und dem Auslösen der Leistungswarnung führen kann.  
  
    > [!NOTE]  
    > Ereignisprotokollvariablen, die den Servernamen speichern, sind auf 32 Zeichen beschränkt. Daher wird möglicherweise die folgende Fehlermeldung angezeigt, wenn die Gesamtlänge des Hostnamens und des Instanznamens länger als 32 Zeichen ist:
    
   ``` 
   Warning,[466] Failed to copy server name LONGNAMESQLSERV\LONGINSTANCENAME while generating performance counter alerts.
   ```
  
  
## <a name="selecting-a-wmi-event"></a>Auswählen eines WMI-Ereignisses  
Sie können Warnungen als Reaktion auf ein bestimmtes WMI-Ereignis angeben. Zum Auswählen eines WMI-Ereignisses definieren Sie die folgenden Punkte im [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent auf der Seite **Allgemein** im Dialogfeld **Neue Warnung** oder **Eigenschaften von Warnung** :  
  
-   **Namespace**  
  
    [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent wird als WMI-Client im WMI-Namespace registriert, der zur Abfrage nach Ereignissen dient.  
  
-   **Abfrage**  
  
    [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent erkennt das angegebene Ereignis mithilfe der WQL-Anweisung (Windows Management Instrumentation Query Language, Abfragesprache der Windows-Verwaltungsinstrumentation).  
  
Die folgenden Links führen zu häufig anfallenden Aufgaben:  
  
**So erstellen Sie eine Warnung auf der Grundlage einer Meldungsnummer**  
  
-   [SQL Server Management Studio](../../ssms/agent/create-an-alert-using-an-error-number.md)  
  
-   [Transact-SQL](../../relational-databases/system-stored-procedures/sp-add-alert-transact-sql.md)  
  
**So erstellen Sie eine Warnung auf der Grundlage von Schweregraden**  
  
-   [SQL Server Management Studio](../../ssms/agent/create-an-alert-using-severity-level.md)  
  
-   [Transact-SQL](../../relational-databases/system-stored-procedures/sp-add-alert-transact-sql.md)  
  
**So erstellen Sie eine Warnung auf der Grundlage eines WMI-Ereignisses**  
  
-   [SQL Server Management Studio](../../ssms/agent/create-a-wmi-event-alert.md)  
  
-   [Transact-SQL](../../relational-databases/system-stored-procedures/sp-add-alert-transact-sql.md)  
  
**So definieren Sie die Antwort auf eine Warnung**  
  
-   [SQL Server Management Studio](../../ssms/agent/define-the-response-to-an-alert-sql-server-management-studio.md)  
  
-   [Transact-SQL](../../relational-databases/system-stored-procedures/sp-add-notification-transact-sql.md)  
  
**So erstellen Sie eine benutzerdefinierte Ereignisfehlermeldung**  
  
-   [Transact-SQL](../../relational-databases/system-stored-procedures/sp-addmessage-transact-sql.md)  
  
**So ändern Sie eine benutzerdefinierte Ereignisfehlermeldung**  
  
-   [Transact-SQL](../../relational-databases/system-stored-procedures/sp-altermessage-transact-sql.md)  
  
**So löschen Sie eine benutzerdefinierte Ereignisfehlermeldung**  
  
-   [Transact-SQL](../../relational-databases/system-stored-procedures/sp-dropmessage-transact-sql.md)  
  
**So deaktivieren oder reaktivieren Sie eine Warnung**  
  
-   [SQL Server Management Studio](../../ssms/agent/disable-or-reactivate-an-alert.md)  
  
-   [Transact-SQL](../../relational-databases/system-stored-procedures/sp-update-alert-transact-sql.md)  
  
## <a name="see-also"></a>Weitere Informationen  
[sp_update_alert (Transact-SQL)](../../relational-databases/performance-monitor/use-sql-server-objects.md)  
