---
description: Überwachen der Ausführung von Paketen und anderen Vorgängen
title: Überwachen der Ausführung von Paketen und anderen Vorgängen | Microsoft-Dokumentation
ms.custom: supportability
ms.date: 06/04/2018
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.ssis.ssms.isoperations.executions.f1
- sql13.ssis.ssms.isoperations.general.f1
ms.assetid: cbbcd79f-ab9b-46ec-84cb-4821c1d16b99
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 2716b85dde5aaa7a7e2df15e87004c838b8137ee
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353026"
---
# <a name="monitor-running-packages-and-other-operations"></a>Überwachen der Ausführung von Paketen und anderen Vorgängen

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Sie können [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Paketausführungen, Projektüberprüfungen und andere Vorgänge mit einem oder mehreren der folgenden Tools überwachen. Bestimmte Tools, z. B. Datenabzweigungen, sind nur für Projekte verfügbar, die auf dem [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Server bereitgestellt werden.  
  
-   Protokolle  
  
     Weitere Informationen finden Sie unter [Integration Services-Protokollierung &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
-   Berichte  
  
     Weitere Informationen finden Sie unter [Berichte für den Integration Services-Server](#reports).  
  
-   Sichten  
  
     Weitere Informationen finden Sie unter [Sichten &#40;Integration Services-Katalog&#41;](../../integration-services/system-views/views-integration-services-catalog.md).  
  
-   Leistungsindikatoren  
  
     Weitere Informationen finden Sie unter [Performance Counters](../../integration-services/performance/performance-counters.md).  
  
-   Datenabzweigungen  

> [!NOTE]
> In diesem Artikel wird beschrieben, wie Sie ausgeführte SSIS-Pakete allgemein und lokal überwachen. Sie können SSIS-Pakete auch in Azure SQL-Datenbank ausführen und überwachen. Weitere Informationen finden Sie unter [Migration von SQL Server Integration Services-Workloads in die Cloud per Lift und Shift](../lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md).
>
> Sie können SSIS-Pakete zwar auch unter Linux ausführen, dort werden jedoch keine Überwachungstools bereitgestellt. Weitere Informationen finden Sie unter [Extrahieren, Transformieren und Laden von Daten unter Linux mit SSIS](../../linux/sql-server-linux-migrate-ssis.md).

## <a name="operation-types"></a>Vorgangstypen  
 Mehrere verschiedene Typen von Vorgängen werden im **SSISDB** -Katalog auf dem [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Server überwacht. Jeder Vorgang kann mehrere ihm zugeordnete Meldungen aufweisen. Jede Meldung kann einem von mehreren Typklassen zugeordnet werden. Eine Meldung kann z. B. vom Typ Information, Warnung oder Fehler sein. Die vollständige Liste von Meldungstypen finden Sie in der Dokumentation zur Transact-SQL-Sicht [catalog.operation_messages &#40;SSISDB-Datenbank&#41;](../../integration-services/system-views/catalog-operation-messages-ssisdb-database.md). Eine vollständige Liste der Vorgangstypen finden Sie unter [catalog.operations &#40;SSISDB-Datenbank&#41;](../../integration-services/system-views/catalog-operations-ssisdb-database.md).  
  
 Neun verschiedene Statustypen werden verwendet, um den Status eines Vorgangs anzugeben. Eine vollständige Liste der Statustypen finden Sie in der Sicht [catalog.operations &#40;SSISDB-Datenbank&#41;](../../integration-services/system-views/catalog-operations-ssisdb-database.md).  

## <a name="active-operations-dialog-box"></a><a name="active_ops"></a> Dialogfeld „Aktive Vorgänge“
  Verwenden Sie das Dialogfeld **Aktive Vorgänge** , um den Status der derzeit ausgeführten [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Vorgänge auf dem [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Server anzuzeigen, z. B. Bereitstellung, Überprüfung und Paketausführung. Diese Daten werden im SSISDB-Katalog gespeichert.  
  
 Weitere Informationen zu verwandten [!INCLUDE[tsql](../../includes/tsql-md.md)]-Ansichten finden Sie unter [catalog.operations &#40;SSISDB-Datenbank&#41;](../../integration-services/system-views/catalog-operations-ssisdb-database.md), [catalog.validations &#40;SSISDB-Datenbank&#41;](../../integration-services/system-views/catalog-validations-ssisdb-database.md) und [catalog.executions &#40;SSISDB-Datenbank&#41;](../../integration-services/system-views/catalog-executions-ssisdb-database.md).  
  
###  <a name="open-the-active-operations-dialog-box"></a><a name="open_dialog"></a> Dialogfeld "Aktive Vorgänge" öffnen  
  
1.  Öffnen Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
  
2.  Stellen Sie eine Verbindung mit einer Microsoft SQL Server-Datenbank-Engine her.  
  
3.  Erweitern Sie im Objekt-Explorer den Knoten **Integration Services** , klicken Sie mit der rechten Maustaste auf den Knoten **SSISDB**, und klicken Sie dann auf **Aktive Vorgänge**.  
  
### <a name="configure-the-options"></a>Konfigurieren der Optionen  
  
 **Typ**  
 Gibt den Vorgangstyp an. Im Folgenden sind die Werte aufgeführt, die im Feld **Typ** zulässig sind, und die entsprechenden Werte in der operations_type-Spalte der Transact-SQL-Sicht **catalog.operations** .  
  
|Beschreibung des Felds „Typ“|operations_type-Wert|  
|-|-|  
|Initialisierung der Integration Services|1|  
|Vorgangsbereinigung (SQL Agent-Auftrag)|2|  
|Projektversions-Cleanup (SQL Agent-Auftrag)|3|  
|Projekt bereitstellen|101|  
|Projekt wiederherstellen|106|  
|Paketausführung erstellen und starten|200|  
|Vorgang beenden (Beenden einer Überprüfung oder Ausführung)|202|  
|Projekt überprüfen|300|  
|Paket überprüfen|301|  
|Katalog konfigurieren|1000|  
  
 **Beenden**  
 Klicken Sie, um einen gerade ausgeführten Vorgang zu beenden.  

## <a name="viewing-and-stopping-packages-running-on-the-integration-services-server"></a>Anzeigen und Beenden von auf dem Integration Services-Server ausgeführten Paketen
  In der **SSISDB** -Datenbank wird der Ausführungsverlauf in internen Tabellen gespeichert, die für Benutzer nicht sichtbar sind. Es werden jedoch Informationen verfügbar gemacht, die für öffentliche Sichten benötigt werden, die Sie abfragen können. Außerdem werden gespeicherte Prozeduren bereitgestellt, die Sie aufrufen können, um allgemeine Aufgaben im Zusammenhang mit Paketen auszuführen.  
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Objekte auf dem Server werden i. d. R. in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]verwaltet. Sie können jedoch auch die Datenbanksichten abfragen und gespeicherte Prozeduren direkt aufrufen oder benutzerdefinierten Code schreiben, mit dem die verwaltete API aufgerufen wird. [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] und die verwaltete API führen zur Ausführung vieler Aufgaben eine Abfrage der Sichten durch und rufen gespeicherte Prozeduren auf. Sie können beispielsweise die Liste der [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Pakete anzeigen, die derzeit auf dem Server ausgeführt werden, und bei Bedarf einzelne Pakete anhalten.  
  
### <a name="viewing-the-list-of-running-packages"></a>Anzeigen der Liste ausgeführter Pakete  
 Sie können die Liste der momentan auf dem Server ausgeführten Pakete im Dialogfeld **Aktive Vorgänge** anzeigen. Weitere Informationen finden Sie unter [Active Operations Dialog Box](#active_ops).  
  
 Informationen zu weiteren Methoden, mit denen Sie die Liste der ausgeführten Pakete anzeigen können, finden Sie in den folgenden Themen.  
  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] -Zugriff  
 Fragen Sie die Sicht [catalog.executions &#40;SSISDB-Datenbank&#41;](../../integration-services/system-views/catalog-executions-ssisdb-database.md) nach Paketen ab, die einen Status von 2 aufweisen, um die Liste der Pakete anzuzeigen, die auf dem Server ausgeführt werden.  
  
 Programmgesteuerter Zugriff auf die verwaltete API  
 Weitere Details finden Sie in den Informationen zum <xref:Microsoft.SqlServer.Management.IntegrationServices>-Namespace und den zugehörigen Klassen.  
  
### <a name="stopping-a-running-package"></a>Beenden eines ausgeführten Pakets  
 Sie können die Beendigung eines ausgeführten Pakets im Dialogfeld **Aktive Vorgänge** anfordern. Weitere Informationen finden Sie unter [Active Operations Dialog Box](#active_ops).  
  
 Informationen zu weiteren Methoden, mit denen Sie Pakete beenden können, die derzeit ausgeführt werden, finden Sie in den folgenden Themen.  
  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] -Zugriff  
 Rufen Sie die gespeicherte Prozedur [catalog.stop_operation &#40;SSISDB-Datenbank&#41;](../../integration-services/system-stored-procedures/catalog-stop-operation-ssisdb-database.md) auf, um ein Paket zu beenden, das auf dem Server ausgeführt wird.  
  
 Programmgesteuerter Zugriff auf die verwaltete API  
 Weitere Details finden Sie in den Informationen zum <xref:Microsoft.SqlServer.Management.IntegrationServices>-Namespace und den zugehörigen Klassen.  
  
### <a name="viewing-the-history-of-packages-that-have-run"></a>Anzeigen des Verlaufs ausgeführter Pakete  
 Verwenden Sie den Bericht [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]Alle Ausführungen **, um den Verlauf der Pakete anzuzeigen, die in** ausgeführt wurden. Weitere Informationen zum Bericht **Alle Ausführungen** und zu anderen Standardberichten finden Sie unter [Berichte für den Integration Services-Server](#reports).  
  
 Informationen zu weiteren Methoden, mit denen Sie den Verlauf der ausgeführten Pakete anzeigen können, finden Sie in den folgenden Themen.  
  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] -Zugriff  
 Um Informationen zu Paketen anzuzeigen, die ausgeführt wurden, fragen Sie die Sicht [catalog.executions &#40;SSISDB-Datenbank&#41;](../../integration-services/system-views/catalog-executions-ssisdb-database.md) ab.  
  
 Programmgesteuerter Zugriff auf die verwaltete API  
 Weitere Details finden Sie in den Informationen zum <xref:Microsoft.SqlServer.Management.IntegrationServices>-Namespace und den zugehörigen Klassen.  

## <a name="reports-for-the-integration-services-server"></a><a name="reports"></a> Berichte für den Integration Services-Server
  In der aktuellen Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]sind Standardberichte in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] verfügbar, die zum Überwachen von [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Projekten hilfreich sind, die auf dem [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Server bereitgestellt wurden. Mit diesen Berichten können Sie den Paketstatus und -verlauf anzeigen und ggf. die Ursache von Paketausführungsfehlern identifizieren.  
  
 Im oberen Bereich jeder Berichtsseite werden die folgenden Symbole bereitgestellt: Zurück-Symbol (um zur vorherigen Seite zurückzukehren), Aktualisierungssymbol (um die auf der Seite angezeigten Informationen zu aktualisieren) und Druckersymbol (um die aktuelle Seite zu drucken).  
  
 Weitere Informationen zum Bereitstellen von Paketen auf dem [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Server, finden Sie unter [Bereitstellen von SQL Server Integration Services-Projekten und Paketen (SSIS)](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md).  
  
### <a name="integration-services-dashboard"></a>Integration Services-Dashboard  
 Der Bericht **Integration Services-Dashboard** bietet eine Übersicht über alle Paketausführungen für die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz. Im Dashboard können Sie die Informationen zu jedem auf dem Server ausgeführten Paket erweitern, um spezifische Details über möglicherweise aufgetretene Paketausführungsfehler zu suchen.  
  
 Der Bericht zeigt die folgenden Abschnitte von Informationen an.  
  
|`Section`|BESCHREIBUNG|  
|-------------|-----------------|  
|**Ausführungsinformationen**|Zeigt die Anzahl von Ausführungen an, die in den vergangenen 24 Stunden unterschiedliche Status (fehlerhaft, ausgeführt, erfolgreich, andere) aufgewiesen haben.|  
|**Paketinformationen**|Zeigt die Gesamtanzahl der Pakete, die in den letzten 24 Stunden ausgeführt wurden.|  
|**Verbindungsinformationen**|Zeigt die Verbindungen, die in den letzten 24 Stunden in fehlerhaften Ausführungen verwendet wurden.|  
|**Detaillierte Paketinformationen**|Zeigt die Details der abgeschlossenen Ausführungen an, die in den letzten 24 Stunden durchgeführt wurden. Dieser Abschnitt zeigt z. B. die Anzahl der fehlerhaften Ausführungen im Vergleich zur Gesamtzahl der Ausführungen, die Dauer der Ausführungen (in Sekunden) und die durchschnittliche Dauer der Ausführungen in den vergangenen drei Monaten an.<br /><br /> Sie können weitere Informationen für ein Paket anzeigen, indem Sie auf **Übersicht**, **Alle Meldungen** und **Ausführungsleistung** klicken.<br /><br /> Der Bericht **Ausführungsleistung** zeigt die Dauer der letzten Ausführungsinstanz sowie die Start- und Endzeiten und die Umgebung an, die angewendet wurde.<br /><br /> Das Diagramm und die zugeordnete Tabelle im Bericht **Ausführungsleistung** zeigen die Dauer der letzten 10 erfolgreichen Ausführungen des Pakets an. In der Tabelle wird auch die durchschnittliche Ausführungsdauer in einem Zeitraum von drei Monaten angezeigt. Unterschiedliche Umgebungen und Literalwerte wurden möglicherweise zur Laufzeit für diese 10 erfolgreichen Ausführungen des Pakets angewendet.<br /><br /> Schließlich wird im Bericht **Ausführungsleistung** die aktive Zeit und die Gesamtzeit für die Datenflusskomponenten des Pakets angezeigt. Die aktive Zeit bezieht sich auf die gesamte Zeit, die für die Ausführung der Komponente in allen Phasen benötigt wurde, und die Gesamtzeit bezieht sich auf die insgesamt verstrichene Zeit für eine Komponente. Der Bericht zeigt nur diese Informationen für Paketkomponenten an, wenn der Protokolliergrad der letzten Paketausführung auf "Leistung" oder "Ausführlich" festgelegt wird.<br /><br /> Der Bericht **Übersicht** zeigt den Status von Pakettasks an. Der Bericht **Meldungen** zeigt die Ereignismeldungen und die Fehlermeldungen für das Paket und die Tasks an, beispielsweise das Melden der Start- und Endzeiten sowie die Anzahl der geschriebenen Zeilen.<br /><br /> Sie können auch im Bericht **Übersicht** auf **Meldungen anzeigen** klicken, um zum Bericht **Meldungen** zu navigieren. Sie können auch im Bericht **Meldungen** auf **Übersicht anzeigen** klicken, um zum Bericht **Übersicht** zu navigieren.|  
  
 Sie können die auf einer beliebigen Seite angezeigte Tabelle filtern, indem Sie auf **Filtern** klicken und dann im Dialogfeld **Filtereinstellungen** die gewünschten Kriterien auswählen. Abhängig von den angezeigten Daten sind unterschiedliche Filterkriterien verfügbar. Sie können die Sortierreihenfolge des Berichts ändern, indem Sie im Dialogfeld **Filtereinstellungen** auf das Sortiersymbol klicken.  
  
### <a name="all-executions-report"></a>Bericht "Alle Ausführungen"  
 Der Bericht **Alle Ausführungen** zeigt eine Zusammenfassung aller [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Ausführungen, die auf dem Server ausgeführt wurden. Es kann mehrere Ausführungen des Beispielpakets geben. Im Gegensatz zum Bericht **Integration Services-Dashboard** können Sie den Bericht **Alle Ausführungen** so konfigurieren, dass Ausführungen angezeigt werden, die während eines Datumsbereichs gestartet wurden. Die Datumsangaben können mehrere Tage, Monate oder Jahre umfassen.  
  
 Der Bericht zeigt die folgenden Abschnitte von Informationen an.  
  
|`Section`|BESCHREIBUNG|  
|-------------|-----------------|  
|Filtern|Zeigt den aktuellen Filter an, der für den Bericht verwendet wird, z. B. der Startzeitraum.|  
|Ausführungsinformationen|Zeigt die Startzeit, die Endzeit und die Dauer für jede Paketausführung an. Sie können eine Liste der Parameterwerte anzeigen, die bei einer Paketausführung verwendet wurden, z. B. Werte, die mit dem Task "Paket ausführen" an ein untergeordnetes Paket übergeben wurden. Um die Parameterliste anzuzeigen, klicken Sie auf "Übersicht".|  
  
 Weitere Informationen über das Verwenden des Tasks "Paket ausführen" zum Verfügbarmachen von Werten für untergeordnete Pakete finden Sie unter [Execute Package Task](../../integration-services/control-flow/execute-package-task.md).  
  
 Weitere Informationen zu Parametern finden Sie unter [Integration Services-Paket- und -Projektparameter (SSIS)](../../integration-services/integration-services-ssis-package-and-project-parameters.md).  
  
### <a name="all-connections"></a>Alle Verbindungen  
 Der Bericht **Alle Verbindungen** enthält die folgenden Informationen für Verbindungen, die nicht hergestellt werden konnten, und für Ausführungen, die in der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz aufgetreten sind.  
  
 Der Bericht zeigt die folgenden Abschnitte von Informationen an.  
  
|`Section`|BESCHREIBUNG|  
|-------------|-----------------|  
|Filtern|Zeigt den aktuellen Filter an, der für den Bericht verwendet wird, z. B. Verbindungen mit einer angegebenen Zeichenfolge und dem Bereich **Uhrzeit des letzten Fehlers** .<br /><br /> Sie legen den Bereich **Uhrzeit des letzten Fehlers** fest, um lediglich Verbindungsfehler anzuzeigen, die während eines Datumsbereichs aufgetreten sind. Der Bereich kann mehrere Tage, Monate oder Jahre umfassen.|  
|Details|Zeigt die Verbindungszeichenfolge, die Anzahl der Ausführungen, bei denen eine Verbindung nicht hergestellt werden konnte, und das Datum an, wann die Verbindung zuletzt nicht hergestellt werden konnte.|  
  
### <a name="all-operations-report"></a>Bericht "Alle Vorgänge"  
 Der Bericht **Alle Vorgänge** zeigt eine Zusammenfassung aller [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Vorgänge an, die auf dem Server ausgeführt wurden, einschließlich Paketbereitstellung, Validierung und Ausführung sowie anderer administrativer Vorgänge. Wie in dem Integration Services-Dashboard können Sie einen Filter auf die Tabelle anwenden, um die angezeigten Informationen einzugrenzen.  
  
### <a name="all-validations-report"></a>Bericht "Alle Überprüfungen"  
 Der Bericht **Alle Überprüfungen** zeigt eine Zusammenfassung aller [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Überprüfungen, die auf dem Server ausgeführt wurden. Die Zusammenfassung enthält Informationen für jede Überprüfungen, z. B. Status, Startzeit und Endzeit. Jeder Zusammenfassungseintrag enthält einen Link zu den Meldungen, die während der Überprüfungen generiert wurden. Wie in dem Integration Services-Dashboard können Sie einen Filter auf die Tabelle anwenden, um die angezeigten Informationen einzugrenzen.  
  
### <a name="custom-reports"></a>Benutzerdefinierte Berichte  
 Sie können dem **SSISDB** -Katalogknoten unter dem Knoten **Integration Services-Kataloge** in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]einen benutzerdefinierten Bericht (RDL-Datei) hinzufügen. Stellen Sie vor dem Hinzufügen des Berichts sicher, dass Sie eine Konvention für dreiteilige Namen verwenden, um die Objekte, auf die Sie verweisen, z. B. eine Quelltabelle, vollständig zu qualifizieren. Andernfalls meldet [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] einen Fehler. Die Benennungskonvention ist \<database>.\<owner>.\<object>. Ein Beispiel wäre SSISDB.internal.executions.  
  
> [!NOTE]  
>  Wenn Sie dem **SSISDB** -Knoten unter dem Knoten **Datenbanken** benutzerdefinierte Berichte hinzufügen, ist das SSISDB-Präfix nicht erforderlich.  
  
 Anweisungen zum Erstellen und Hinzufügen eines benutzerdefinierten Berichts finden Sie unter [Add a Custom Report to Management Studio](../../ssms/object/add-a-custom-report-to-management-studio.md).  

## <a name="view-reports-for-the-integration-services-server"></a>Anzeigen von Berichten für den Integration Services-Server
  In der aktuellen Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]sind Standardberichte in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] verfügbar, die zum Überwachen von [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Projekten hilfreich sind, die auf dem [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Server bereitgestellt wurden.  Weitere Informationen zu den Berichten finden Sie unter [Berichte für den Integration Services-Server](#reports).  
  
### <a name="to-view-reports-for-the-integration-services-server"></a>So zeigen Sie Berichte für den Integration Services-Server an  
  
1.  Erweitern Sie in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]den Knoten **Integration Services-Kataloge** im Objekt-Explorer.  
  
2.  Klicken Sie mit der rechten Maustaste auf **SSISDB**, klicken Sie auf **Berichte** und dann auf **Standardberichte**.  
  
3.  Klicken Sie auf eine oder mehrere der folgenden Optionen, um einen Bericht anzuzeigen.  
  
    -   **Integration Services-Dashboard**  
  
    -   **Alle Ausführungen**  
  
    -   **Alle Überprüfungen**  
  
    -   **Alle Vorgänge**  
  
    -   **Alle Verbindungen**  

## <a name="see-also"></a>Weitere Informationen  
 [Ausführung von Projekten und Paketen](../packages/deploy-integration-services-ssis-projects-and-packages.md)   
 [Behandlung von Problemen in Berichten für die Paketausführung](../troubleshooting/troubleshooting-reports-for-package-execution.md)  
