---
description: Integration Services-Protokollierung (SSIS)
title: Integration Services-Protokollierung (SSIS) | Microsoft-Dokumentation
ms.custom: supportability
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.configuredtslogs.containers.f1
- sql13.dts.designer.configuredtslogs.loggingdetails.f1
- sql13.dts.designer.configuredtslogs.providersandlogs.f1
- SQL13.SSIS.SSMS.ISMANAGECUSTOMIZEDLOGGINGLEVEL.F1
helpviewer_keywords:
- containers [Integration Services], logs
- Windows Event log provider [Integration Services]
- XML File log provider [Integration Services]
- SQL Server Profiler, log provider
- SQL Server Integration Services packages, logs
- SSIS packages, logs
- tasks [Integration Services], logs
- logs [Integration Services], log providers
- Integration Services packages, logs
- log providers [Integration Services]
- packages [Integration Services], logs
- Text File log provider
- SQL Server log provider
ms.assetid: 65e17889-371f-4951-9a7e-9932b2d0dcde
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8ac0f913188b23b85548f87a4f458d1c2b754d15
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347452"
---
# <a name="integration-services-ssis-logging"></a>Integration Services-Protokollierung (SSIS)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] umfasst Protokollanbieter, mit denen Sie die Protokollierung in Paketen, Containern und Tasks implementieren können. Mit der Protokollierung können Sie Laufzeitinformationen zu einem Paket aufzeichnen, damit Sie ein Paket bei jeder Ausführung überwachen und Probleme behandeln können. Beispielsweise können in einem Protokoll der Name des Operators, der das Paket ausgeführt hat, und der Zeitpunkt, zu dem die Paketausführung begann und endete, aufgezeichnet werden.  
  
 Sie können den Protokollierungsbereich konfigurieren, der während einer Paketausführung auf dem [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Server auftritt. Weitere Informationen finden Sie unter [Aktivieren der Protokollierung für die Paketausführung auf dem SSIS-Server](#server_logging).  
  
 Sie können die Protokollierung auch einschließen, wenn Sie ein Paket mit dem Eingabeaufforderungs-Hilfsprogramm **dtecex** ausführen. Weitere Informationen zu den Befehlszeilenargumenten, die die Protokollierung unterstützen, finden Sie unter [dtexec Utility](../../integration-services/packages/dtexec-utility.md).  
  
## <a name="configure-logging-in-sql-server-data-tools"></a>Konfigurieren der Protokollierung in SQL Server Data Tools  
 Protokolle werden Paketen zugeordnet und auf Paketebene konfiguriert. Jeder Task oder Container in einem Paket kann Informationen in einem Paketprotokoll protokollieren. Die Tasks und Container in einem Paket können für die Protokollierung auch dann aktiviert werden, wenn das Paket selbst nicht aktiviert ist. Sie können z. B. die Protokollierung für den Task "SQL ausführen" aktivieren, ohne die Protokollierung für das übergeordnete Paket zu aktivieren. Ein Paket, ein Container oder ein Task kann in mehrere Protokolle schreiben. Sie haben die Möglichkeit, die Protokollierung nur für das Paket bzw. für einen beliebigen im Paket enthaltenen Task oder Container zu aktivieren.  
  
 Wenn Sie das Protokoll einem Paket hinzufügen, wählen Sie den Protokollanbieter und den Speicherort des Protokolls aus. Der Protokollanbieter gibt das Format für die Protokolldaten an, z. B. eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datenbank oder eine Textdatei.  
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] enthält die folgenden Protokollanbieter:  
  
-   Den Textdatei-Protokollanbieter, der Protokolleinträge in ASCII-Textdateien durch Trennzeichen getrennt im CSV-Format schreibt. Die standardmäßige Dateinamenerweiterung für diesen Anbieter ist LOG.  
  
-   Den [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] -Protokollanbieter, der Ablaufverfolgungen schreibt, die Sie mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Profiler anzeigen können. Die standardmäßige Dateinamenerweiterung für diesen Anbieter ist TRC.  
  
    > [!NOTE]  
    >  Der [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] -Protokollanbieter kann nicht in einem Paket verwendet werden, das im 64-Bit-Modus ausgeführt wird.  
  
-   Den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Protokollanbieter, der Protokolleinträge in die **sysssislog** -Tabelle in einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datenbank schreibt.  
  
-   Den Windows-Ereignisprotokollanbieter, der Einträge in das Anwendungsprotokoll im Windows-Ereignisprotokoll auf dem lokalen Computer schreibt.  
  
-   Den XML-Dateiprotokollanbieter, der Protokolldateien in eine XML-Datei schreibt. Die standardmäßige Dateinamenerweiterung für diesen Anbieter ist XML.  
  
 Wenn Sie einem Paket einen Protokollanbieter hinzufügen oder die Protokollierung programmgesteuert konfigurieren, können Sie eine ProgID oder ClassID zum Identifizieren des Protokollanbieters verwenden, statt die vom [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer im Dialogfeld **SSIS-Protokolle konfigurieren** angezeigten Namen zu verwenden.  
  
 In der folgenden Tabelle sind die ProgID und die ClassID für die Protokollanbieter von [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] aufgeführt, sowie der Speicherort der Protokolle, in die Protokollanbieter schreiben.  
  
|Protokollanbieter|ProgID|ClassID|Location|  
|------------------|------------|-------------|--------------|  
|Textdatei|DTS.LogProviderTextFile|{0A039101-ACC1-4E06-943F-279948323883}|Der vom Protokollanbieter verwendete Dateiverbindungs-Manager gibt den Pfad der Textdatei an.|  
|SQL Server Profiler|DTS.LogProviderSQLProfiler|{E93F6300-AE0C-4916-A7BF-A8D0CE12C77A}|Der vom Protokollanbieter verwendete Dateiverbindungs-Manager gibt den Pfad der von [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]verwendeten Textdatei an.|  
|SQL Server|DTS.LogProviderSQLServer|{94150B25-6AEB-4C0D-996D-D37D1C4FDEDA}|Der vom Protokollanbieter verwendete OLE DB-Verbindungs-Manager gibt die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datenbank an, die die sysssislog-Tabelle mit den Protokolleinträgen enthält.|  
|Windows-Ereignisprotokoll|DTS.LogProviderEventLog|{071CC8EB-C343-4CFF-8D58-564B92FCA3CF}|Das Anwendungsprotokoll der Windows-Ereignisanzeige enthält die [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Protokollinformationen.|  
|XML-Datei|DTS.LogProviderXMLFile|{440945A4-2A22-4F19-B577-EAF5FDDC5F7A}|Der vom Protokollanbieter verwendete Dateiverbindungs-Manager gibt den Pfad der XML-Datei an.|  
  
 Außerdem können Sie benutzerdefinierte Protokollanbieter erstellen. Weitere Informationen finden Sie unter [Creating a Custom Log Provider](../../integration-services/extending-packages-custom-objects/log-provider/creating-a-custom-log-provider.md).  
  
 Die Protokollanbieter in einem Paket sind Elemente der Protokollanbieterauflistung des Pakets. Wenn Sie mit dem [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer ein Paket erstellen und die Protokollierung implementieren, können Sie eine Liste der Sammelelemente im Ordner **Protokollanbieter** auf der Registerkarte **Paket-Explorer** des [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designers anzeigen.  
  
 Zum Konfigurieren eines Protokollanbieters stellen Sie einen Namen und eine Beschreibung für den Protokollanbieter bereit und geben den Verbindungs-Manager an, den der Protokollanbieter verwendet. Der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Protokollanbieter verwendet einen OLE DB-Verbindungs-Manager. Für die Textdatei-, [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]- und XML-Datei-Protokollanbieter werden Dateiverbindungs-Manager verwendet. Für den Windows-Ereignisprotokollanbieter wird kein Verbindungs-Manager verwendet, da direkt in das Windows-Ereignisprotokoll geschrieben wird. Weitere Informationen finden Sie unter [OLE DB Connection Manager](../../integration-services/connection-manager/ole-db-connection-manager.md) und [File Connection Manager](../../integration-services/connection-manager/file-connection-manager.md).  
  
### <a name="logging-customization"></a>Protokollierungsanpassung  
 Zum Anpassen der Protokollierung eines Ereignisses oder einer benutzerdefinierten Meldung wird in [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] ein Schema häufig protokollierter Informationen bereitgestellt, die in Protokolleinträgen aufgenommen werden. Das [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Protokollschema definiert die Informationen, die Sie protokollieren können. Für jeden Protokolleintrag können Sie Elemente aus dem Protokollschema auswählen.  
  
 Ein Paket und seine Container und Tasks protokollieren nicht dieselben Informationen, und Tasks innerhalb desselben Pakets oder Containers können verschiedene Informationen protokollieren. Ein Paket kann z. B. Operatorinformationen protokollieren, wenn das Paket gestartet wird, ein Task kann die Fehlerquelle des Tasks protokollieren, und ein weiterer Task kann Informationen protokollieren, wenn Fehler auftreten. Wenn ein Paket und seine Container mehrere Protokolle verwenden, werden dieselben Informationen in alle Protokolle geschrieben.  
  
 Sie können einen Protokolliergrad auswählen, der Ihren Anforderungen entspricht, indem Sie die zu protokollierenden Ereignisse und die für jedes Ereignis zu protokollierenden Informationen festlegen. Eventuell enthalten Ihrer Meinung nach einige Ereignisse nützlichere Informationen als andere. Vielleicht möchten Sie z. B. nur die Computer- und Operatornamen für das **PreExecute** -Ereignis protokollieren, für das **Error** -Ereignis jedoch alle verfügbaren Informationen.  
  
 Damit die Protokolldateien keine großen Speichermengen belegen oder um eine zu umfangreiche Protokollierung zu vermeiden, was zu einer Leistungsverschlechterung führen könnte, können Sie die Protokollierung einschränken, indem Sie bestimmte zu protokollierende Ereignisse und Informationen auswählen. So können Sie z. B. ein Protokoll konfigurieren, das nur das Datum und den Computernamen für einen Fehler erfasst.  
  
 In [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer legen Sie die Protokollierungsoptionen im Dialogfeld **SSIS-Protokolle konfigurieren** fest.  
  
#### <a name="log-schema"></a>Protokollschema  
 In der folgenden Tabelle werden die Elemente im Protokollschema beschrieben.  
  
|Element|BESCHREIBUNG|  
|-------------|-----------------|  
|Computer|Der Name des Computers, auf dem das Protokollereignis auftrat.|  
|Operator|Die Identität des Benutzers, der das Paket gestartet hat.|  
|SourceName|Der Name des Containers oder des Tasks, für den das Protokollereignis auftrat.|  
|SourceID|Der eindeutige Bezeichner des Pakets; der For-Schleifen-, Foreach-Schleifen- oder Sequenz-Container; oder der Task, für den das Protokollereignis auftrat.|  
|ExecutionID|Die GUID der Paketausführungsinstanz.<br /><br /> Hinweis: Beim Ausführen eines einzelnen Pakets werden möglicherweise Protokolleinträge mit anderen Werten für das ExecutionID-Element erstellt. Wenn Sie ein Paket beispielsweise in [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]ausführen, werden in der Überprüfungsphase möglicherweise Protokolleinträge mit einem ExecutionID-Element erstellt, das [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]entspricht. In der Ausführungsphase werden jedoch unter Umständen Protokolleinträge mit einem ExecutionID-Element erstellt, das dtshost.exe entspricht. Um ein weiteres Beispiel zu geben, können Sie ein Paket ausführen, das Tasks vom Typ "Paket ausführen" enthält, wobei von jedem dieser Tasks ein untergeordnetes Paket ausgeführt wird. Diese untergeordneten Pakete erstellen möglicherweise Protokolleinträge mit einem anderen ExecutionID-Element als in den vom übergeordneten Paket erstellten Protokolleinträgen.|  
|MessageText|Eine dem Protokolleintrag zugeordnete Meldung.|  
|DataBytes|Ein für den Protokolleintrag spezifisches Bytearray. Die Bedeutung dieses Felds variiert je nach Protokolleintrag.|  
  
 Die folgende Tabelle beschreibt drei zusätzliche Elemente des Protokollschemas, die auf der Registerkarte **Details** des Dialogfensters **SSIS-Protokolle konfigurieren** nicht verfügbar sind.  
  
|Element|BESCHREIBUNG|  
|-------------|-----------------|  
|StartTime|Der Zeitpunkt, zu dem der Container oder der Task die Ausführung beginnt.|  
|EndTime|Der Zeitpunkt, zu dem der Container oder der Task die Ausführung beendet.|  
|DataCode|Ein optionaler ganzzahliger Wert, der typischerweise einen Wert aus der <xref:Microsoft.SqlServer.Dts.Runtime.DTSExecResult> -Enumeration enthält, der das Ergebnis des aktuell ausgeführten Containers oder Tasks angibt:<br /><br /> 0 – Erfolg<br /><br /> 1 – Fehlschlag<br /><br /> 2 – Abgeschlossen<br /><br /> 3 – Abgebrochen|  
  
#### <a name="log-entries"></a>Protokolleinträge  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] unterstützt Protokolleinträge für vordefinierte Ereignisse und stellt für viele [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Objekte benutzerdefinierte Protokolleinträge bereit. Im Dialogfeld **SSIS-Protokolle konfigurieren** des [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designers werden diese Ereignisse und benutzerdefinierten Protokolleinträge aufgelistet.  
  
 In der folgenden Tabelle werden die vordefinierten Ereignisse beschrieben, die beim Auftreten von Laufzeitereignissen zum Schreiben von Protokolleinträgen aktiviert werden können. Diese Protokolleinträge gelten für ausführbare Dateien, das Paket sowie die im Paket enthaltenen Tasks und Container. Der Name des Protokolleintrags entspricht dem Namen des ausgelösten Laufzeitereignisses, mit dem das Schreiben des Protokolleintrags verursacht wurde.  
  
|Events|BESCHREIBUNG|  
|------------|-----------------|  
|**OnError**|Schreibt einen Protokolleintrag, wenn ein Fehler auftritt.|  
|**OnExecStatusChanged**|Schreibt einen Protokolleintrag, wenn ein Task (nicht ein Container) während des Debuggens angehalten oder wieder aufgenommen wurde.|  
|**OnInformation**|Schreibt einen Protokolleintrag während der Prüfung und Ausführung einer ausführbaren Datei, um Informationen zu melden.|  
|**OnPostExecute**|Schreibt einen Protokolleintrag unmittelbar nach Beendigung der Ausführung der ausführbaren Datei.|  
|**OnPostValidate**|Schreibt einen Protokolleintrag, wenn die Überprüfung der ausführbaren Datei beendet ist.|  
|**OnPreExecute**|Schreibt einen Protokolleintrag unmittelbar vor Ausführung der ausführbaren Datei.|  
|**OnPreValidate**|Schreibt einen Protokolleintrag, wenn die Überprüfung der ausführbaren Datei beginnt.|  
|**OnProgress**|Schreibt einen Protokolleintrag, wenn die ausführbare Datei einen messbaren Fortschritt gemacht hat.|  
|**OnQueryCancel**|Schreibt einen Protokolleintrag für jeden Punkt in der Taskverarbeitung, an dem ein Abbruch der Ausführung sinnvoll ist.|  
|**OnTaskFailed**|Schreibt einen Protokolleintrag, wenn ein Task fehlschlägt.|  
|**OnVariableValueChanged**|Schreibt einen Protokolleintrag, wenn sich der Wert einer Variablen ändert.|  
|**OnWarning**|Schreibt einen Protokolleintrag, wenn eine Warnung auftritt.|  
|**PipelineComponentTime**|Schreibt für jede Datenflusskomponente einen Protokolleintrag für jede Phase der Überprüfung und der Ausführung. Der Protokolleintrag gibt die Verarbeitungszeit für jede Phase an.|  
|**Diagnostic**<br /><br /> **DiagnosticEx**|Schreibt einen Protokolleintrag, der Diagnoseinformationen enthält.<br /><br /> Sie können zum Beispiel vor und nach jedem Aufruf eines externen Datenanbieters eine Meldung protokollieren. Weitere Informationen finden Sie unter [Behandeln von Problemen mit Paketausführungstools](../../integration-services/troubleshooting/troubleshooting-tools-for-package-execution.md).<br /><br /> Protokollieren Sie das **DiagnosticEx** -Ereignis, wenn Sie die Spaltennamen für Spalten im Datenfluss finden möchten, die Fehler enthalten. Dieses Ereignis schreibt eine Herkunftszuordnung für den Datenfluss in das Protokoll. Sie können den Spaltennamen dann in dieser Herkunftszuordnung nachschlagen, und zwar anhand des von einer Fehlerausgabe erfassten Spaltenbezeichners. Weitere Informationen finden Sie unter [Fehlerbehandlung in Daten](../../integration-services/data-flow/error-handling-in-data.md).<br /><br /> Beachten Sie, dass Leerraum in der XML-Ausgabe beim **DiagnosticEx** -Ereignis nicht beibehalten wird, um die Größe des Protokolls zu verringern. Kopieren Sie das Protokoll in einen XML-Editor wie z.B. Visual Studio, der XML-Formatierung und Syntaxhervorhebung unterstützt, um die Lesbarkeit zu verbessern.<br /><br /> Hinweis: Wenn Sie das **DiagnosticEx** -Ereignis mit dem SQL Server-Protokollanbieter protokollieren, wird die Ausgabe möglicherweise abgeschnitten. Das **message** -Feld des SQL Server-Protokollanbieters ist vom Typ „nvarchar(2048)“. Verwenden Sie beim Protokollieren des **DiagnosticEx** -Ereignisses einen anderen Protokollanbieter, um eine abgeschnittene Ausgabe zu vermeiden.|  
  
 Das Paket und viele Tasks verfügen über benutzerdefinierte Protokolleinträge, die für die Protokollierung aktiviert werden können. Der Task „Mail senden“ stellt z.B. den benutzerdefinierten Protokolleintrag **SendMailTaskBegin** bereit, mit dem im Zeitraum zwischen dem Beginn der Ausführung des Tasks „Mail senden“ und dem Senden einer E-Mail-Nachricht Informationen protokolliert werden. Weitere Informationen finden Sie unter [Custom Messages for Logging](#custom_messages).  
  
### <a name="differentiating-package-copies"></a>Unterscheiden von Paketkopien  
 Protokolldaten schließen den Namen und den GUID des Pakets ein, zu dem die Protokolleinträge gehören. Beim Erstellen eines neuen Pakets durch Kopieren eines vorhandenen Pakets werden Name und GUID des vorhandenen Pakets ebenfalls kopiert. Folglich können GUID und Name zweier Pakete identisch sein, sodass die Pakete in den Protokolldaten schwer voneinander zu unterscheiden sind.  
  
 Sie sollten den Namen und GUID der neuen Pakete ändern, um diese Mehrdeutigkeit zu umgehen. Sie können in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]mithilfe des Eigenschaftenfensters die GUID in der **ID** -Eigenschaft neu generieren und den Wert der **Name** -Eigenschaft aktualisieren. Sie haben auch die Möglichkeit, den GUID und Namen programmgesteuert oder mit dem Hilfsprogramm **dtutil** zu ändern. Weitere Informationen finden Sie unter [Festlegen von Paketeigenschaften](../../integration-services/set-package-properties.md) und [dtutil (Hilfsprogramm)](../../integration-services/dtutil-utility.md).  
  
### <a name="parent-logging-options"></a>Übergeordnete Protokollierungsoptionen  
 Häufig entsprechen die Protokollierungsoptionen von Tasks und For-Schleifen-, Foreach-Schleifen- und Sequenz-Containern denen des Paketcontainers oder eines übergeordneten Containers. In diesem Fall können Sie diese so konfigurieren, dass sie die Protokollierungsoptionen von ihrem übergeordneten Container erben. In einem For-Schleifen-Container, der den Task "SQL ausführen" enthält, kann z. B. der Task "SQL ausführen" die Protokollierungsoptionen verwenden, die für den For-Schleifen-Container festgelegt werden. Legen Sie zum Verwenden der übergeordneten Protokollierungsoptionen die LoggingMode-Eigenschaft des Containers auf **UseParentSetting** fest. Sie können diese Eigenschaft im Fenster **Eigenschaften** von [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] oder im Dialogfeld **SSIS-Protokolle konfigurieren** in [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer festlegen.  
  
### <a name="logging-templates"></a>Protokollierungsvorlagen  
 Im Dialogfeld **SSIS-Protokolle konfigurieren** können Sie auch häufig verwendete Protokollierungskonfigurationen als Vorlagen erstellen und speichern und die Vorlagen dann in mehreren Paketen verwenden. So ist es einfach, eine konsequente Protokollierungsstrategie für mehrere Pakete anzuwenden und Protokolleinstellungen für Pakete zu ändern, indem die Vorlagen aktualisiert und dann angewendet werden. Die Vorlagen werden als XML-Dateien gespeichert.  
  
 **So konfigurieren Sie die Protokollierung im Dialogfeld "SSIS-Protokolle konfigurieren"**  
  
1.  Aktivieren Sie das Paket und seine Tasks für die Protokollierung. Die Protokollierung kann auf Paket-, Container- und Taskebene stattfinden. Sie können verschiedene Protokolle für Pakete, Container und Tasks angeben.  
  
2.  Wählen Sie einen Protokollanbieter aus, und fügen Sie ein Protokoll für das Paket hinzu. Protokolle können nur auf Paketebene erstellt werden, und ein Task oder Container muss eines der für das Paket erstellten Protokolle verwenden. Jedes Protokoll ist einem der folgenden Protokollanbieter zugeordnet: Textdatei, [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], Windows-Ereignisprotokoll oder XML-Datei. Weitere Informationen finden Sie unter [Aktivieren der Paketprotokollierung in SQL Server Data Tools](#ssdt).  
  
3.  Wählen Sie die Ereignisse und die Protokollschemainformationen zu jedem Ereignis aus, das Sie im Protokoll erfassen möchten. Weitere Informationen finden Sie unter [Konfigurieren der Protokollierung mithilfe einer gespeicherten Konfigurationsdatei](#saved_config).  
  
### <a name="configuration-of-log-provider"></a>Konfiguration des Protokollanbieters  
 Sie können Eigenschaften mit dem [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer oder programmgesteuert festlegen.  
  
 Ein Protokollanbieter wird als ein Schritt beim Implementieren der Protokollierung in einem Paket erstellt und konfiguriert.  
  
 Nachdem Sie einen Protokollanbieter erstellt haben, können Sie dessen Eigenschaften im Eigenschaftenfenster von [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]anzeigen und ändern.  
  
 Weitere Informationen zum programmgesteuerten Festlegen dieser Eigenschaften finden Sie in der Dokumentation zur <xref:Microsoft.SqlServer.Dts.Runtime.LogProvider> -Klasse.  
  
### <a name="logging-for-data-flow-tasks"></a>Protokollierung von Datenflusstasks  
 Der Datenflusstask stellt eine Reihe von benutzerdefinierten Protokolleinträge bereit, die zum Überwachen und Anpassen der Leistung verwendet werden können. Sie können beispielsweise Komponenten überwachen, die möglicherweise Arbeitsspeicherverluste verursachen, oder nachverfolgen, wie lange das Ausführen einer bestimmten Komponente dauert. Eine Liste dieser benutzerdefinierten Protokolleinträge sowie Beispiele für Protokollausgaben finden Sie unter [Data Flow Task](../../integration-services/control-flow/data-flow-task.md).  
  
#### <a name="capture-the-names-of-columns-in-which-errors-occur"></a>Erfassen der Namen fehlerhafter Spalten  
 Die Fehlerausgabe im Datenfluss ist standardmäßig so konfiguriert, dass sie nur den numerischen Bezeichner der Spalte bereitstellt, in der der Fehler aufgetreten ist. Weitere Informationen finden Sie unter [Fehlerbehandlung in Daten](../../integration-services/data-flow/error-handling-in-data.md).  
  
 Sie finden die Spaltennamen, indem Sie die Protokollierung aktivieren und das **DiagnosticEx** -Ereignis auswählen. Dieses Ereignis schreibt eine Herkunftszuordnung für den Datenfluss in das Protokoll. Anschließend können Sie den Namen der Spalte anhand ihres Bezeichners in dieser Herkunftszuordnung nachschlagen. Beachten Sie, dass Leerraum in der XML-Ausgabe beim **DiagnosticEx** -Ereignis nicht beibehalten wird, um die Größe des Protokolls zu verringern. Kopieren Sie das Protokoll in einen XML-Editor wie z.B. Visual Studio, der XML-Formatierung und Syntaxhervorhebung unterstützt, um die Lesbarkeit zu verbessern.  
  
#### <a name="use-the-pipelinecomponenttime-event"></a>Verwenden des PipelineComponentTime-Ereignisses  
 Das PipelineComponentTime-Ereignis stellt möglicherweise den nützlichsten benutzerdefinierten Protokolleintrag dar. Dieser Protokolleintrag meldet den Zeitaufwand in Millisekunden, den jede Komponente im Datenfluss für die fünf Hauptverarbeitungsschritte jeweils aufbringt. In der folgenden Tabelle werden diese Verarbeitungsschritte beschrieben. [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Entwickler kennen diese Schritte als Hauptmethoden einer <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent>.  
  
|Schritt|BESCHREIBUNG|  
|----------|-----------------|  
|Überprüfen|Die Komponente überprüft Eigenschaftswerte und Konfigurationseinstellungen auf ihre Gültigkeit.|  
|PreExecute|Die Komponente führt eine einmalige Verarbeitung aus, bevor sie mit der Verarbeitung von Datenzeilen beginnt.|  
|PostExecute|Die Komponente führt eine einmalige Verarbeitung aus, nachdem alle Datenzeilen verarbeitet wurden.|  
|ProcessInput|Die Transformations- oder Zielkomponente verarbeitet die eingehenden Datenzeilen, die von einer Upstreamquelle oder einer Transformation an sie übergeben wurden.|  
|PrimeOutput|Die Quell- oder Transformationskomponente füllt die Puffer von Daten auf, die an eine Downstreamtransformation oder eine Zielkomponente übergeben werden sollen.|  
  
 Wenn Sie das PipelineComponentTime-Ereignis aktivieren, protokolliert [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] eine Meldung für jeden der von den einzelnen Komponenten ausgeführten Verarbeitungsschritt. Die folgenden Protokolleinträge zeigen eine Teilmenge der Meldungen, die vom [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] CalculatedColumns-Paketbeispiel protokolliert werden:  
  
 `The component "Calculate LineItemTotalCost" (3522) spent 356 milliseconds in ProcessInput.`  
  
 `The component "Sum Quantity and LineItemTotalCost" (3619) spent 79 milliseconds in ProcessInput.`  
  
 `The component "Calculate Average Cost" (3662) spent 16 milliseconds in ProcessInput.`  
  
 `The component "Sort by ProductID" (3717) spent 125 milliseconds in ProcessInput.`  
  
 `The component "Load Data" (3773) spent 0 milliseconds in ProcessInput.`  
  
 `The component "Extract Data" (3869) spent 688 milliseconds in PrimeOutput filling buffers on output "OLE DB Source Output" (3879).`  
  
 `The component "Sum Quantity and LineItemTotalCost" (3619) spent 141 milliseconds in PrimeOutput filling buffers on output "Aggregate Output 1" (3621).`  
  
 `The component "Sort by ProductID" (3717) spent 16 milliseconds in PrimeOutput filling buffers on output "Sort Output" (3719).`  
  
 Diese Protokolleinträge zeigen, dass der Datenflusstask den größten Zeitaufwand für die folgenden, hier in absteigender Reihenfolge gezeigten Schritte benötigt hat:  
  
-   Die OLE DB-Quelle mit dem Namen "Extract Data" hat 688 Millisekunden für das Laden von Daten benötigt.  
  
-   Die Transformation für abgeleitete Spalten mit dem Namen "Calculate LineItemTotalCost" hat 356 Millisekunden für das Durchführen von Berechnungen für eingehende Zeilen benötigt.  
  
-   Die Transformation für das Aggregieren mit dem Namen „Sum Quantity and LineItemTotalCost“ hat insgesamt 220 Millisekunden (141 für den PrimeOutput- und 79 für den ProcessInput-Schritt) benötigt, um Berechnungen durchzuführen und die Daten an die nächste Transformation zu übergeben.  

## <a name="enable-package-logging-in-sql-server-data-tools"></a><a name="ssdt"></a> Aktivieren der Paketprotokollierung in SQL Server Data Tools
  In diesem Verfahren wird beschrieben, wie einem Paket Protokolle hinzugefügt werden, die Protokollierung auf Paketebene konfiguriert wird und die Protokollierungskonfiguration in eine XML-Datei gespeichert wird. Sie können Protokolle nur auf der Paketebene hinzufügen, das Paket muss jedoch keine Protokollierung durchführen, um die Protokollierung in den Containern zu ermöglichen, die im Paket enthalten sind.  
  
> [!IMPORTANT]  
>  Wenn Sie das [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Projekt auf dem [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Server bereitstellen, überschreibt der Protokolliergrad, den Sie für die Paketausführung festgelegt haben, die Paketprotokollierung, die Sie mit [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]konfigurieren.  
  
 Die Container im Paket verwenden standardmäßig dieselbe Protokollierungskonfiguration wie der übergeordnete Container. Weitere Informationen zum Festlegen der Protokollierungsoptionen für einzelne Container finden Sie unter [Konfigurieren der Protokollierung mithilfe einer gespeicherten Konfigurationsdatei](#saved_config).  
  
### <a name="to-enable-logging-in-a-package"></a>So aktivieren Sie die Protokollierung in einem Paket  
  
1.  Öffnen Sie in [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] das [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Projekt mit dem gewünschten Paket.  
  
2.  Klicken Sie im Menü **SSIS** auf **Protokollierung**.  
  
3.  Wählen Sie einen Protokollanbieter in der Liste **Anbietertyp** aus, und klicken Sie dann auf **Hinzufügen**.  
  
4.  Wählen Sie in der Spalte **Konfiguration** einen Verbindungs-Manager aus, oder klicken Sie auf **\<New connection>** , um einen neuen Verbindungs-Manager des entsprechenden Typs für den Protokollanbieter zu erstellen. Verwenden Sie je nach ausgewähltem Anbieter einen der folgenden Verbindungs-Manager.  
  
    -   Verwenden Sie für Textdateien einen Dateiverbindungs-Manager. Weitere Informationen finden Sie unter [File Connection Manager](../../integration-services/connection-manager/file-connection-manager.md)  
  
    -   Verwenden Sie für [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]einen Dateiverbindungs-Manager.  
  
    -   Verwenden Sie für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]einen OLE DB-Verbindungs-Manager. Weitere Informationen finden Sie unter [OLE DB Connection Manager](../../integration-services/connection-manager/ole-db-connection-manager.md).  
  
    -   Führen Sie für das Windows-Ereignisprotokoll keine Aktion durch. [!INCLUDE[ssIS](../../includes/ssis-md.md)] erstellt das Protokoll automatisch.  
  
    -   Verwenden Sie für XML-Dateien einen Dateiverbindungs-Manager.  
  
5.  Wiederholen Sie die Schritte 3 und 4 für jedes im Paket zu verwendende Protokoll.  
  
    > [!NOTE]  
    >  Ein Paket kann mehrere Protokolle pro Typ verwenden.  
  
6.  Aktivieren Sie wahlweise das Kontrollkästchen für das Paket, wählen Sie die für die Paketprotokollierung zu verwendenden Protokolle aus, und klicken Sie dann auf die Registerkarte **Details** .  
  
7.  Wählen Sie auf der Registerkarte **Details** die Option **Ereignisse** , um alle Protokolleinträge zu protokollieren, oder löschen Sie **Ereignisse** , um einzelne Ereignisse auszuwählen.  
  
8.  Klicken Sie wahlweise auf **Erweitert** , um die zu protokollierenden Informationen anzugeben.  
  
    > [!NOTE]  
    >  Standardmäßig werden alle Informationen protokolliert.  
  
9. Klicken Sie auf der Registerkarte **Details** auf **Speichern**. Das Dialogfeld **Speichern unter** wird angezeigt. Suchen Sie den Ordner, in dem Sie die Protokollierungskonfiguration speichern möchten, geben Sie einen Dateinamen für die neue Protokollkonfiguration ein, und klicken Sie dann auf **Speichern**.  
  
10. Klicken Sie auf **OK**.  
  
11. Klicken Sie im Menü **Datei** auf **Ausgewählte Elemente speichern** , um das aktualisierte Paket zu speichern.  

## <a name="configure-ssis-logs-dialog-box"></a><a name="configure_logs"></a> SSIS-Protokolle konfigurieren (Dialogfeld)
  Verwenden Sie das Dialogfeld **SSIS-Protokolle konfigurieren** , um Protokollierungsoptionen für ein Paket zu definieren.  
  
 **Was möchten Sie tun?**  
  
1.  [Öffnen Sie das Dialogfeld "SSIS-Protokolle konfigurieren".](#open_dialog)  
  
2.  [Konfigurieren Sie die Optionen im Bereich "Container".](#container)  
  
3.  [Konfigurieren Sie die Optionen auf der Registerkarte "Anbieter und Protokolle".](#provider)  
  
4.  [Konfigurieren Sie die Optionen auf der Registerkarte "Details".](#detail)  
  
###  <a name="open-the-configure-ssis-logs-dialog-box"></a><a name="open_dialog"></a> Öffnen Sie das Dialogfeld "SSIS-Protokolle konfigurieren".  
 **So öffnen Sie das Dialogfeld "SSIS-Protokolle konfigurieren"**  
  
-   Klicken Sie in [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer auf **Protokollierung** im **SSIS** -Menü.  
  
###  <a name="configure-the-options-in-the-containers-pane"></a><a name="container"></a> Konfigurieren Sie die Optionen im Bereich "Container".  
 Mithilfe des **Container** -Bereichs des Dialogfelds **SSIS-Protokolle konfigurieren** können Sie das Paket und seine Container für das Protokollieren aktivieren.  
  
#### <a name="options"></a>Tastatur  
 **Container**  
 Aktivieren Sie die Kontrollkästchen in der hierarchischen Sicht, um das Paket und seine Container für die Protokollierung zu aktivieren:  
  
-   Falls diese Option deaktiviert ist, ist der Container nicht für die Protokollierung aktiviert. Wählen Sie diese Option aus, um die Protokollierung zu aktivieren.  
  
-   Falls die Option abgeblendet ist, verwendet der Container die Protokollierungsoptionen des ihm übergeordneten Containers. Diese Option steht für das Paket nicht zur Verfügung.  
  
-   Wenn diese Option aktiviert ist, definiert der Container seine eigenen Protokollierungsoptionen.  
  
 Falls ein Container abgeblendet ist und Sie Protokollierungsoptionen für den Container festlegen möchten, klicken Sie zweimal auf das entsprechende Kontrollkästchen. Durch das erste Klicken wird das Kontrollkästchen deaktiviert, durch das zweite Klicken wird es ausgewählt, und Sie können somit die zu verwendenden Protokollanbieter und die zu protokollierenden Informationen auswählen.  
  
###  <a name="configure-the-options-on-the-providers-and-logs-tab"></a><a name="provider"></a> Konfigurieren Sie die Optionen auf der Registerkarte "Anbieter und Protokolle".  
 Verwenden Sie die Registerkarte **Anbieter und Protokolle** des Dialogfelds **SSIS-Protokolle konfigurieren** , um Protokolle für das Aufzeichnen von Laufzeitereignissen zu erstellen und zu konfigurieren.  
  
#### <a name="options"></a>Tastatur  
 **Anbietertyp**  
 Wählen Sie einen Protokollanbietertyp aus der Liste aus.  
  
 **Add (Hinzufügen)**  
 Fügen Sie der Auflistung der Protokollanbieter des Pakets ein Protokoll des angegebenen Typs hinzu.  
  
 **Name**  
 Aktivieren oder deaktivieren Sie Protokolle für Container und Tasks, die Sie im Bereich **Container** des Dialogfelds **SSIS-Protokolle konfigurieren** auswählen, indem Sie die Kontrollkästchen aktivieren bzw. deaktivieren. Das Feld Name kann bearbeitet werden. Verwenden Sie den Standardnamen für den Anbieter, oder geben Sie einen eindeutigen, beschreibenden Namen ein.  
  
 **Beschreibung**  
 Das Feld Beschreibung kann bearbeitet werden. Klicken Sie in das Feld und ändern Sie die Standardbeschreibung des Protokolls.  
  
 **Konfiguration**  
 Wählen Sie einen vorhandenen Verbindungs-Manager aus der Liste aus, oder klicken Sie auf \<**New connection...**>, um einen neuen Verbindungs-Manager zu erstellen. Abhängig vom Typ des Protokollanbieters können Sie einen OLE DB-Verbindungs-Manager oder einen Dateiverbindungs-Manager konfigurieren. Der [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows-Ereignisprotokollanbieter erfordert keine Verbindung.  
  
 Verwandte Themen: [OLE DB Connection Manager](../../integration-services/connection-manager/ole-db-connection-manager.md) , [File Connection Manager](../../integration-services/connection-manager/file-connection-manager.md)  
  
 **Löschen**  
 Wählen Sie einen Protokollanbieter aus, und klicken Sie auf **Löschen**.  
  
###  <a name="configure-the-options-on-the-details-tab"></a><a name="detail"></a> Konfigurieren Sie die Optionen auf der Registerkarte "Details".  
 Auf der Registerkarte **Details** des Dialogfelds **SSIS-Protokolle konfigurieren** können Sie die Ereignisse für das Protokollieren sowie die zu protokollierenden Informationsdetails aktivieren. Die Informationen, die Sie auswählen, gelten für alle Protokollanbieter im Paket. Sie können z. B. nicht einige Informationen in die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz und andere Informationen in eine Textdatei schreiben.  
  
#### <a name="options"></a>Tastatur  
 **Ereignisse**  
 Aktivieren oder Deaktivieren von Ereignissen für die Protokollierung.  
  
 **Beschreibung**  
 Anzeigen einer Beschreibung des Ereignisses.  
  
 **Erweitert**  
 Auswählen oder Löschen zu protokollierender Ereignisse und Auswählen oder Löschen von Informationen, die für jedes Ereignis protokolliert werden sollen. Klicken Sie auf **Standard** , um alle Protokollierungsdetails mit Ausnahme der Liste der Ereignisse auszublenden. Die folgenden Informationen sind für die Protokollierung verfügbar:  
  
|value|BESCHREIBUNG|  
|-----------|-----------------|  
|**Computer**|Der Name des Computers, auf dem das protokollierte Ereignis aufgetreten ist.|  
|**Operator**|Der Benutzername der Person, die das Paket gestartet hat.|  
|**SourceName**|Der Name des Pakets, Containers oder Tasks, in dem das protokollierte Ereignis aufgetreten ist.|  
|**SourceID**|GUID (Global Unique Identifier) des Pakets, Containers oder Tasks, in dem das protokollierte Ereignis aufgetreten ist.|  
|**ExecutionID**|Der global eindeutige Bezeichner der Paketausführungsinstanz.|  
|**MessageText**|Eine dem Protokolleintrag zugeordnete Meldung.|  
|**DataBytes**|Für die zukünftige Verwendung reserviert.|  
  
 **Grundlegend**  
 Wählen Sie zu protokollierende Ereignisse aus, oder löschen Sie diese. Diese Option blendet alle Protokollierungsdetails mit Ausnahme der Liste der Ereignisse aus. Wenn Sie ein Ereignis auswählen, werden standardmäßig alle Protokollierungsdetails für das Ereignis ausgewählt. Klicken Sie auf **Erweitert** , um alle Protokollierungsdetails anzuzeigen.  
  
 **Load**  
 Geben Sie eine vorhandene XML-Datei an, die als Vorlage zum Festlegen der Protokollierungsoptionen verwendet werden soll.  
  
 **Speichern**  
 Speichern Sie Konfigurationsdetails als Vorlage in einer XML-Datei.  

## <a name="configure-logging-by-using-a-saved-configuration-file"></a><a name="saved_config"></a> Konfigurieren der Protokollierung mithilfe einer gespeicherten Konfigurationsdatei
  In diesem Verfahren wird beschrieben, wie die Protokollierung für neue Container in einem Paket konfiguriert wird, indem eine zuvor gespeicherte Protokollierungskonfigurationsdatei geladen wird.  
  
 Alle Container in einem Paket verwenden standardmäßig dieselbe Protokollierungskonfiguration wie der übergeordnete Container. Die Tasks in einer Foreach-Schleife verwenden z. B. dieselbe Protokollierungskonfiguration wie die Foreach-Schleife.  
  
### <a name="to-configure-logging-for-a-container"></a>So konfigurieren Sie die Protokollierung für einen Container  
  
1.  Öffnen Sie in [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] das [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Projekt mit dem gewünschten Paket.  
  
2.  Klicken Sie im Menü **SSIS** auf **Protokollierung**.  
  
3.  Erweitern Sie die Paketbaumansicht, und wählen Sie den zu konfigurierenden Container aus.  
  
4.  Wählen Sie auf der Registerkarte **Anbieter und Protokolle** die Protokolle aus, die Sie für den Container verwenden möchten.  
  
    > [!NOTE]  
    >  Sie können Protokolle nur auf Paketebene erstellen. Weitere Informationen finden Sie unter [Aktivieren der Paketprotokollierung in SQL Server Data Tools](#ssdt).  
  
5.  Klicken Sie auf die Registerkarte **Details** und dann auf **Laden**.  
  
6.  Suchen Sie die zu verwendende Protokollierungskonfigurationsdatei, und klicken Sie auf **Öffnen**.  
  
7.  Wählen Sie optional einen anderen zu protokollierenden Protokolleintrag aus, indem Sie das entsprechende Kontrollkästchen in der Spalte **Ereignisse** aktivieren. Klicken Sie auf **Erweitert** , um den für diesen Eintrag zu protokollierenden Informationstyp auszuwählen.  
  
    > [!NOTE]  
    >  Der neue Container enthält u. U. weitere Protokolleinträge, die für den Container nicht verfügbar sind, der ursprünglich zum Erstellen der Protokollierungskonfiguration verwendet wurde. Diese zusätzlichen Protokolleinträge müssen manuell ausgewählt werden, falls diese protokolliert werden sollen.  
  
8.  Um die aktualisierte Version der Protokollierungskonfiguration zu speichern, klicken Sie auf **Speichern**.  
  
9. Klicken Sie im Menü **Datei** auf **Ausgewählte Elemente speichern** , um das aktualisierte Paket zu speichern.  

## <a name="enable-logging-for-package-execution-on-the-ssis-server"></a><a name="server_logging"></a> Aktivieren Sie die Protokollierung for Package Execution on the SSIS Server
  In diesem Thema wird beschrieben, wie Sie den Protokolliergrad für ein Paket festlegen oder ändern, wenn Sie ein auf dem [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Server bereitgestelltes Paket ausführen. Der Protokolliergrad, den Sie beim Ausführen des Pakets festlegen, überschreibt die Paketprotokollierung, die Sie zur Entwurfszeit in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]konfigurieren. Weitere Informationen finden Sie unter [Aktivieren der Paketprotokollierung in SQL Server Data Tools](#ssdt) .  
  
 Sie können nun in den **Servereigenschaften** von SQL Server in der Eigenschaft **Serverprotokolliergrad** einen Standardwert für den serverweiten Protokolliergrad festlegen. Sie können sich zwischen einem der in diesem Thema beschriebenen integrierten Protokolliergrade oder einem vorhandenen benutzerdefinierten Protokolliergrad entscheiden. Der ausgewählte Protokolliergrad wird standardmäßig auf alle im SSIS-Katalog bereitgestellten Pakete angewendet. Er gilt standardmäßig auch für SQL Agent-Auftragsschritte, die ein SSIS-Paket ausführen.  
  
 Sie können den Protokolliergrad für ein einzelnes Paket auch mithilfe einer der folgenden Methoden angeben. In diesem Thema wird die erste Methode behandelt.  
  
-   Konfigurieren einer Instanz einer Paketausführung mithilfe des Dialogfelds "Paket ausführen"  
  
-   Das Festlegen von Parametern für eine Instanz mithilfe von [catalog.set_execution_parameter_value &#40;SSISDB-Datenbank&#41;](../../integration-services/system-stored-procedures/catalog-set-execution-parameter-value-ssisdb-database.md)  
  
-   Das Konfigurieren eines Auftrags des SQL Server-Agents für eine Paketausführung mit dem Dialogfeld "Neuer Auftragsschritt".  
  
### <a name="set-the-logging-level-for-a-package-by-using-the-execute-package-dialog-box"></a>Festlegen des Protokolliergrads für ein Paket mithilfe des Dialogfelds „Paket ausführen“  
  
1.  Navigieren Sie in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]zum Paket im Objekt-Explorer.  
  
2.  Klicken Sie mit der rechten Maustaste auf das Paket, und wählen Sie **Ausführen** aus.  
  
3.  Wählen Sie im Dialogfeld **Paket ausführen** die Registerkarte **Erweitert** aus.  
  
4.  Wählen Sie unter **Protokolliergrad** den Protokolliergrad aus. Dieses Thema enthält eine Beschreibung der verfügbaren Werte.  
  
5.  Nehmen Sie ggf. weitere Einstellungen für die Paketkonfiguration vor, und klicken Sie anschließend auf **OK** , um das Paket auszuführen.  
  
### <a name="select-a-logging-level"></a>Auswählen eines Protokolliergrads  
 Die folgenden integrierten Protokolliergrade sind verfügbar. Sie können auch einen vorhandenen benutzerdefinierten Protokolliergrad auswählen. Dieses Thema enthält eine Beschreibung benutzerdefinierter Protokolliergrade.  
  
|Protokolliergrad|BESCHREIBUNG|  
|-------------------|-----------------|  
|Keine|Die Protokollierung ist deaktiviert. Nur der Status der Ausführung von Paketen wird protokolliert.|  
|Basic|Alle Ereignisse werden protokolliert, außer benutzerdefinierten und Diagnose-Ereignissen. Dies ist der Standardwert.|  
|RuntimeLineage|Sammelt die Daten, die zum Nachverfolgen von Informationen über die Datenherkunft im Datenfluss benötigt werden. Sie können diese Herkunftsinformationen analysieren, um die Herkunftsbeziehung zwischen Tasks zu bestimmen. Unabhängige Softwareentwickler (ISVs) und Entwickler können mit diesen Informationen benutzerdefinierte Herkunftszuordnungstools erstellen.|  
|Leistung|Nur Leistungsstatistiken sowie OnError- und OnWarning-Ereignisse werden protokolliert.<br /><br /> In dem Bericht **Ausführungsleistung** wird die aktive Zeit und die Gesamtzeit für Datenflusskomponenten des Pakets angezeigt. Diese Informationen sind verfügbar, wenn der Protokolliergrad der letzten Paketausführung auf **Leistung** oder **Ausführlich** festgelegt wurde. Weitere Informationen finden Sie unter [Berichte für den Integration Services-Server](../../integration-services/performance/monitor-running-packages-and-other-operations.md#reports).<br /><br /> In der [catalog.execution_component_phases](../../integration-services/system-views/catalog-execution-component-phases.md) -Sicht werden die Start- und Beendigungszeiten der Datenflusskomponenten für jede Ausführungsphase angezeigt. In dieser Sicht werden die Informationen für diese Komponenten nur angezeigt, wenn der Protokolliergrad der Paketausführung auf **Leistung** oder **Ausführlich** festgelegt ist.|  
|Ausführlich|Alle Ereignisse werden protokolliert, einschließlich benutzerdefinierter Ereignisse und Diagnose-Ereignissen.<br /><br /> Zu benutzerdefinierten Ereignissen zählen auch die von [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Tasks protokollierten Ereignisse. Weitere Informationen zu benutzerdefinierten Ereignissen finden Sie unter [Benutzerdefinierte Meldungen für die Protokollierung](#custom_messages).<br /><br /> Ein Beispiel für ein Diagnoseereignis ist das **DiagnosticEx** -Ereignis. Immer wenn ein „Paket ausführen“-Task ein untergeordnetes Paket ausführt, erfasst dieses Ereignis die an untergeordnete Pakete übergebenen Parameterwerte.<br /><br /> Das **DiagnosticEx** -Ereignis hilft Ihnen auch dabei, die Namen der Spalten abzurufen, in denen Fehler auf Zeilenebene auftreten. Dieses Ereignis schreibt eine Herkunftszuordnung für den Datenfluss in das Protokoll. Sie können den Spaltennamen dann in dieser Herkunftszuordnung nachschlagen, und zwar anhand des von einer Fehlerausgabe erfassten Spaltenbezeichners.  Weitere Informationen finden Sie unter [Fehlerbehandlung in Daten](../../integration-services/data-flow/error-handling-in-data.md).<br /><br /> Der Wert der Meldungsspalte für **DiagnosticEx** ist XML-Text. Fragen Sie zum Anzeigen des Meldungstexts für eine Paketausführung die Sicht [catalog.operation_messages &#40;SSISDB-Datenbank&#41;](../../integration-services/system-views/catalog-operation-messages-ssisdb-database.md) ab. Beachten Sie, dass Leerraum in der XML-Ausgabe beim **DiagnosticEx** -Ereignis nicht beibehalten wird, um die Größe des Protokolls zu verringern. Kopieren Sie das Protokoll in einen XML-Editor wie z.B. Visual Studio, der XML-Formatierung und Syntaxhervorhebung unterstützt, um die Lesbarkeit zu verbessern.<br /><br /> In der [catalog.execution_data_statistics](../../integration-services/system-views/catalog-execution-data-statistics.md) -Sicht wird eine Zeile angezeigt, sobald eine Datenflusskomponente Daten zur Paketausführung an eine Downstreamkomponente sendet. Der Protokolliergrad muss auf **Ausführlich** festgelegt werden, um diese Informationen in der Sicht zu erfassen.|  
  
### <a name="create-and-manage-customized-logging-levels-by-using-the-customized-logging-level-management-dialog-box"></a>Erstellen und Verwalten benutzerdefinierter Protokolliergrade mithilfe des Dialogfelds „Customized Logging Level Management“ (Verwaltung benutzerdefinierter Protokolliergrade)  
 Sie können benutzerdefinierte Protokolliergrade erstellen, die nur die von Ihnen gewünschten Statistiken und Ereignisse sammeln. Optional können Sie auch den Kontext von Ereignissen erfassen, der Variablenwerte, Verbindungszeichenfolgen und Komponenteneigenschaften umfasst. Wenn Sie ein Paket ausführen, können Sie überall dort einen benutzerdefinierten Protokolliergrad auswählen, wo Sie auch einen integrierten Protokolliergrad auswählen können.  
  
> [!TIP]  
>  Die **IncludeInDebugDump** -Eigenschaft der Variablen muss auf **TRUE** festgelegt werden, damit die Werte der Paketvariablen erfasst werden.  
  
1.  Klicken Sie in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]mit der rechten Maustaste auf die SSISDB-Datenbank, und wählen Sie **Customized Logging Level** (Angepasster Protokolliergrad) aus, um das Dialogfeld **Verwaltung des angepassten Protokolliergrads** zu öffnen. So erstellen und verwalten Sie angepasste Protokolliergrade. Die Liste **Angepasste Protokolliergrade** enthält alle vorhandenen angepassten Protokolliergrade.  
  
2.  Klicken Sie zum **Erstellen** eines neuen angepassten Protokolliergrads auf **Erstellen**, und stellen Sie anschließend einen Namen und eine Beschreibung bereit. Wählen Sie auf den Registerkarten **Statistiken** und **Ereignisse** die Statistiken und Ereignisse aus, die Sie sammeln möchten. Wählen Sie optional auf der Registerkarte **Ereignisse** für einzelne Ereignisse **Kontext einschließen** aus. Klicken Sie anschließend auf **Speichern**.  
  
3.  Wählen Sie zum **Aktualisieren** eines vorhandenen angepassten Protokolliergrads diesen in der Liste aus, konfigurieren Sie ihn erneut, und klicken Sie anschließend auf **Speichern**.  
  
4.  Wählen Sie zum **Löschen** eines vorhandenen angepassten Protokolliergrads diesen in der Liste aus, und klicken Sie anschließend auf **Löschen**.  
  
 **Berechtigungen für benutzerdefinierte Protokolliergrade.**  
  
-   Alle Benutzer der SSISDB-Datenbank können benutzerdefinierte Protokolliergrade sehen und beim Ausführen von Paketen einen benutzerdefinierten Protokolliergrad auswählen.  
  
-   Nur Benutzer in der ssis_admin- oder der sysadmin-Rolle können benutzerdefinierte Protokolliergrade erstellen, aktualisieren oder löschen.  

## <a name="custom-messages-for-logging"></a><a name="custom_messages"></a> Custom Messages for Logging
SQL Server Integration Services stellt einen umfangreichen Satz an benutzerdefinierten Ereignissen zum Schreiben von Protokolleinträgen für Pakete und für mehrere Tasks bereit. Sie können diese Einträge verwenden, um detaillierte Informationen zum Fortschritt sowie über die Ergebnisse und Probleme der Ausführung zu speichern, indem Sie vordefinierte Ereignisse bzw. benutzerdefinierte Meldungen für die spätere Analyse erfassen. Sie können beispielsweise Beginn und Ende eines Masseneinfügungsvorgangs erfassen, um Leistungsprobleme beim Ausführen des Pakets zu identifizieren.  
  
 Die benutzerdefinierten Protokolleinträge unterscheiden sich von den für Pakete und alle Container und Tasks verfügbaren Standardprotokollierungsereignissen. Die benutzerdefinierten Protokolleinträge dienen zum Erfassen nützlicher Informationen zu einem bestimmten Task eines Pakets. Beispielsweise zeichnet einer der benutzerdefinierten Protokolleinträge für den Task SQL ausführen die von dem Task ausgeführte SQL-Anweisung im Protokoll auf.  
  
 In allen Protokolleinträgen sind jeweils das Datum und die Uhrzeit enthalten, einschließlich der beim Beginnen und Beenden eines Pakets automatisch geschriebenen Protokolleinträge. Bei vielen Protokollereignissen werden mehrere Einträge in das Protokoll geschrieben. In der Regel tritt dies dann auf, wenn ein Ereignis verschiedene Phasen aufweist. Beispielsweise schreibt das **ExecuteSQLExecutingQuery** -Protokollereignis drei Einträge: einen Eintrag, nachdem der Task eine Verbindung mit der Datenbank erhalten hat; einen weiteren, nachdem der Task begonnen hat, die SQL-Anweisung vorzubereiten; und noch einen, nachdem die Ausführung der SQL-Anweisung abgeschlossen wurde.  
  
 Die folgenden [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Objekte verfügen über benutzerdefinierte Protokolleinträge:  
  
 [Paket](#Package)  
  
 [Masseneinfügungstask](#BulkInsert)  
  
 [Datenflusstask](#DataFlow)  
  
 [DTS 2000 ausführen (Task)](#ExecuteDTS200)  
  
 [Prozess ausführen (Task)](#ExecuteProcess)  
  
 [SQL ausführen (Task)](#ExecuteSQL)  
  
 [Task „Dateisystem“](#FileSystem)  
  
 [FTP-Task](#FTP)  
  
 [Nachrichtenwarteschlange (Task)](#MessageQueue)  
  
 [Skripttask](#Script)  
  
 [Mail senden (Task)](#SendMail)  
  
 [Datenbanken übertragen (Task)](#TransferDatabase)  
  
 [Fehlermeldungen übertragen (Task)](#TransferErrorMessages)  
  
 [Aufträge übertragen (Task)](#TransferJobs)  
  
 [Task „Anmeldungen übertragen“](#TransferLogins)  
  
 [In master gespeicherte Prozeduren übertragen (Task)](#TransferMasterStoredProcedures)  
  
 [SQL Server-Objekte kopieren (Task)](#TransferSQLServerObjects)  
  
 [Webdienste (Task)](#WebServices)  
  
 [WMI-Datenleser (Task)](#WMIDataReader)  
  
 [WMI-Ereignisüberwachung (Task)](#WMIEventWatcher)  
  
 [XML-Task](#XML)  
  
### <a name="log-entries"></a>Protokolleinträge  
  
####  <a name="package"></a><a name="Package"></a> Paket  
 In der folgenden Tabelle werden die benutzerdefinierten Protokolleinträge für Pakete aufgelistet.  
  
|Protokolleintrag|BESCHREIBUNG|  
|---------------|-----------------|  
|**PackageStart**|Zeigt den Beginn der Paketausführung an. Dieser Protokolleintrag wird automatisch in das Protokoll geschrieben. Dieser Eintrag kann nicht ausgeschlossen werden.|  
|**PackageEnd**|Zeigt den Abschluss der Paketausführung an. Dieser Protokolleintrag wird automatisch in das Protokoll geschrieben. Dieser Eintrag kann nicht ausgeschlossen werden.|  
|**Diagnostic**|Enthält Informationen zur Systemkonfiguration, die sich auf die Paketausführung auswirken, wie z. B. die Anzahl ausführbarer Dateien, die gleichzeitig ausgeführt werden können.<br /><br /> Der Protokolleintrag **Diagnostic** enthält auch vorherige und nachfolgende Einträge für Aufrufe von externen Datenanbietern.|  
  
####  <a name="bulk-insert-task"></a><a name="BulkInsert"></a> Masseneinfügungstask  
 In der folgenden Tabelle werden die benutzerdefinierten Protokolleinträge für den Masseneinfügungstask aufgelistet.  
  
|Protokolleintrag|BESCHREIBUNG|  
|---------------|-----------------|  
|**DTSBulkInsertTaskBegin**|Zeigt den Beginn der Masseneinfügung an.|  
|**DTSBulkInsertTaskEnd**|Zeigt die Fertigstellung der Masseneinfügung an.|  
|**DTSBulkInsertTaskInfos**|Enthält beschreibende Informationen zum Task.|  
  
####  <a name="data-flow-task"></a><a name="DataFlow"></a> Data Flow Task  
 In der folgenden Tabelle werden die benutzerdefinierten Protokolleinträge für den Datenflusstask aufgelistet.  
  
|Protokolleintrag|BESCHREIBUNG|  
|---------------|-----------------|  
|**BufferSizeTuning**|Zeigt an, dass der Datenflusstask die Größe des Puffers geändert hat. Der Protokolleintrag beschreibt die Gründe für die Größenänderung und listet die temporäre neue Puffergröße auf.|  
|**OnPipelinePostEndOfRowset**|Gibt an, dass eine Komponente das Signal für das Ende des Rowsets erhalten hat. Dieses Signal wird durch den letzten Aufruf der **ProcessInput** -Methode festgelegt. Für jede Komponente im Datenfluss, die eine Eingabe verarbeitet, wird ein Eintrag geschrieben. Der Eintrag schließt den Namen der Komponente ein.|  
|**OnPipelinePostPrimeOutput**|Zeigt an, dass die Komponente ihren letzten Aufruf der **PrimeOutput** -Methode abgeschlossen hat. Je nach Datenfluss werden möglicherweise mehrere Protokolleinträge geschrieben. Wenn es sich bei der Komponente um eine Quelle handelt, bedeutet das, dass die von der Komponente durchgeführte Zeilenverarbeitung fertig gestellt wurde.|  
|**OnPipelinePreEndOfRowset**|Zeigt an, dass eine Komponente das Signal für das Ende des Rowsets erhalten soll. Dieses Signal wird durch den letzten Aufruf der **ProcessInput** -Methode festgelegt. Für jede Komponente im Datenfluss, die eine Eingabe verarbeitet, wird ein Eintrag geschrieben. Der Eintrag schließt den Namen der Komponente ein.|  
|**OnPipelinePrePrimeOutput**|Zeigt an, dass die Komponente einen Aufruf aus der **PrimeOutput** -Methode erhalten soll. Je nach Datenfluss werden möglicherweise mehrere Protokolleinträge geschrieben.|  
|**OnPipelineRowsSent**|Berichtet die Anzahl von Zeilen, die einer Komponenteneingabe durch einen Aufruf der **ProcessInput** -Methode bereitgestellt wurden. Der Protokolleintrag enthält den Komponentennamen.|  
|**PipelineBufferLeak**|Stellt Informationen zu Komponenten bereit, die Puffer aufrechterhalten haben, nachdem der Puffer-Manager beendet wurde. Das bedeutet, dass Pufferressourcen nicht freigegeben wurden, was zu Speicherverlusten führen kann. Der Protokolleintrag stellt den Namen der Komponente und die ID des Puffers bereit.|  
|**PipelineExecutionPlan**|Berichtet den Ausführungsplan des Datenflusses. Es werden Informationen darüber bereitgestellt, wie Puffer an Komponenten gesendet werden. Diese Informationen in Verbindung mit dem PipelineExecutionTrees-Eintrag beschreiben, was in dem Task geschieht.|  
|**PipelineExecutionTrees**|Berichtet die Ausführungsstrukturen des Layouts im Datenfluss. Der Planer der Datenfluss-Engine verwendet die Strukturen zum Erstellen des Datenflussplans.|  
|**PipelineInitialization**|Bietet Initialisierungsinformationen zu dem Task. Zu diesen Informationen gehören die Verzeichnisse für die temporäre Speicherung von BLOB-Daten, die Standardpuffergröße und die Zeilenanzahl in einem Puffer. Je nach der Konfiguration des Datenflusstasks werden möglicherweise mehrere Protokolleinträge geschrieben.|  
  
####  <a name="execute-dts-2000-task"></a><a name="ExecuteDTS200"></a> DTS 2000 ausführen (Task)  
 In der folgenden Tabelle werden die benutzerdefinierten Protokolleinträge für den Task DTS 2000 ausführen aufgelistet.  
  
|Protokolleintrag|BESCHREIBUNG|  
|---------------|-----------------|  
|**ExecuteDTS80PackageTaskBegin**|Zeigt an, dass die Ausführung eines DTS 2000-Pakets über den Task gestartet wurde.|  
|**ExecuteDTS80PackageTaskEnd**|Zeigt an, dass die Ausführung über den Task beendet wurde.<br /><br /> Hinweis: Das DTS 2000-Paket kann nach Beendigung des Tasks mit der Ausführung fortfahren.|  
|**ExecuteDTS80PackageTaskTaskInfo**|Enthält beschreibende Informationen zum Task.|  
|**ExecuteDTS80PackageTaskTaskResult**|Berichtet das Ausführungsergebnis des durch den Task ausgeführten DTS 2000-Pakets.|  
  
####  <a name="execute-process-task"></a><a name="ExecuteProcess"></a> Prozess ausführen (Task)  
 In der folgenden Tabelle werden die benutzerdefinierten Protokolleinträge für den Task Prozess ausführen aufgelistet.  
  
|Protokolleintrag|BESCHREIBUNG|  
|---------------|-----------------|  
|**ExecuteProcessExecutingProcess**|Enthält Informationen zum Ausführprozess der zur Ausführung konfigurierten ausführbaren Datei.<br /><br /> Es werden zwei Protokolleinträge geschrieben. Der eine Protokolleintrag enthält Informationen über den Namen und Speicherort der vom Task ausgeführten ausführbaren Datei, im anderen Eintrag wird das Beenden der ausführbaren Datei erfasst.|  
|**ExecuteProcessVariableRouting**|Enthält Informationen darüber, welche Variablen an die Eingabe und an die Ausgaben der ausführbaren Datei geleitet werden. Es werden Protokolleinträge für stdin (für die Eingabe), für stdout (für die Ausgabe) und für stderr (für die Fehlerausgabe) geschrieben.|  
  
####  <a name="execute-sql-task"></a><a name="ExecuteSQL"></a> SQL ausführen (Task)  
 In der folgenden Tabelle wird der benutzerdefinierte Protokolleintrag für den Task <legacyBold>SQL ausführen</legacyBold> beschrieben.  
  
|Protokolleintrag|BESCHREIBUNG|  
|---------------|-----------------|  
|**ExecuteSQLExecutingQuery**|Enthält Informationen zu den Ausführungsphasen der SQL-Anweisung. Protokolleinträge werden geschrieben, wenn der Task eine Verbindung mit der Datenbank erhält, wenn der Task beginnt, die SQL-Anweisung vorzubereiten, und nachdem die Ausführung der SQL-Anweisung abgeschlossen wurde. Der Protokolleintrag für die Vorbereitungsphase schließt die vom Task verwendete SQL-Anweisung ein.|  
  
####  <a name="file-system-task"></a><a name="FileSystem"></a> Task Dateisystem  
 In der folgenden Tabelle wird der benutzerdefinierte Protokolleintrag für den Task "Dateisystem" beschrieben.  
  
|Protokolleintrag|BESCHREIBUNG|  
|---------------|-----------------|  
|**FileSystemOperation**|Berichtet den vom Task durchgeführten Vorgang. Der Protokolleintrag wird geschrieben, wenn der Dateisystemvorgang begonnen wird, und schließt Informationen über die Quelle und das Ziel ein.|  
  
####  <a name="ftp-task"></a><a name="FTP"></a> FTP-Task  
 In der folgenden Tabelle werden die benutzerdefinierten Protokolleinträge für den FTP-Task aufgelistet.  
  
|Protokolleintrag|BESCHREIBUNG|  
|---------------|-----------------|  
|**FTPConnectingToServer**|Zeigt an, dass mit dem Task eine Verbindung zum FTP-Server initiiert wurde.|  
|**FTPOperation**|Berichtet den Beginn und Typ des vom Task ausgeführten FTP-Vorgangs.|  
  
####  <a name="message-queue-task"></a><a name="MessageQueue"></a> Nachrichtenwarteschlange (Task)  
 In der folgenden Tabelle werden die benutzerdefinierten Protokolleinträge für den Task Nachrichtenwarteschlange aufgelistet.  
  
|Protokolleintrag|BESCHREIBUNG|  
|---------------|-----------------|  
|**MSMQAfterOpen**|Zeigt an, dass das Öffnen der Warteschlange beendet wurde.|  
|**MSMQBeforeOpen**|Zeigt an, dass das Öffnen der Warteschlange begonnen wurde.|  
|**MSMQBeginReceive**|Zeigt an, dass das Empfangen einer Meldung begonnen wurde.|  
|**MSMQBeginSend**|Zeigt an, dass das Senden einer Meldung begonnen wurde.|  
|**MSMQEndReceive**|Zeigt an, dass das Empfangen einer Meldung beendet wurde.|  
|**MSMQEndSend**|Zeigt an, dass das Senden einer Meldung beendet wurde.|  
|**MSMQTaskInfo**|Enthält beschreibende Informationen zum Task.|  
|**MSMQTaskTimeOut**|Zeigt an, dass beim Task ein Timeout eingetreten ist.|  
  
####  <a name="script-task"></a><a name="Script"></a> Skripttask  
 In der folgenden Tabelle wird der benutzerdefinierte Protokolleintrag für den Skripttask beschrieben.  
  
|Protokolleintrag|BESCHREIBUNG|  
|---------------|-----------------|  
|**ScriptTaskLogEntry**|Gibt die Ergebnisse des Implementierens der Protokollierung innerhalb des Skripts an. Für jeden Aufruf der **Log** -Methode des **Dts** -Objekts wird jeweils ein Protokolleintrag geschrieben. Der Eintrag wird beim Ausführen des Codes geschrieben. Weitere Informationen finden Sie unter [Logging in the Script Task](../../integration-services/extending-packages-scripting/task/logging-in-the-script-task.md).|  
  
####  <a name="send-mail-task"></a><a name="SendMail"></a> Mail senden (Task)  
 In der folgenden Tabelle werden die benutzerdefinierten Protokolleinträge für den Task 'Mail senden' aufgelistet.  
  
|Protokolleintrag|BESCHREIBUNG|  
|---------------|-----------------|  
|**SendMailTaskBegin**|Zeigt an, dass das Senden einer E-Mail-Nachricht begonnen wurde.|  
|**SendMailTaskEnd**|Zeigt an, dass das Senden einer E-Mail-Nachricht beendet wurde.|  
|**SendMailTaskInfo**|Enthält beschreibende Informationen zum Task.|  
  
####  <a name="transfer-database-task"></a><a name="TransferDatabase"></a> Datenbanken übertragen (Task)  
 In der folgenden Tabelle werden die benutzerdefinierten Protokolleinträge für den Task Datenbanken übertragen aufgelistet.  
  
|Protokolleintrag|BESCHREIBUNG|  
|---------------|-----------------|  
|**SourceDB**|Gibt die vom Task kopierte Datenbank an.|  
|**SourceSQLServer**|Gibt den Computer an, von dem die Datenbank kopiert wurde.|  
  
####  <a name="transfer-error-messages-task"></a><a name="TransferErrorMessages"></a> Fehlermeldungen übertragen (Task)  
 In der folgenden Tabelle werden die benutzerdefinierten Protokolleinträge für den Task Fehlermeldungen übertragen aufgelistet.  
  
|Protokolleintrag|BESCHREIBUNG|  
|---------------|-----------------|  
|**TransferErrorMessagesTaskFinishedTransferringObjects**|Zeigt an, dass das Übertragen von Fehlermeldungen beendet wurde.|  
|**TransferErrorMessagesTaskStartTransferringObjects**|Zeigt an, dass das Übertragen von Fehlermeldungen gestartet wurde.|  
  
####  <a name="transfer-jobs-task"></a><a name="TransferJobs"></a> Aufträge übertragen (Task)  
 In der folgenden Tabelle werden die benutzerdefinierten Protokolleinträge für den Task Aufträge übertragen aufgelistet.  
  
|Protokolleintrag|BESCHREIBUNG|  
|---------------|-----------------|  
|**TransferJobsTaskFinishedTransferringObjects**|Zeigt an, dass das Übertragen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent-Aufträgen beendet wurde.|  
|**TransferJobsTaskStartTransferringObjects**|Zeigt an, dass das Übertragen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent-Aufträgen gestartet wurde.|  
  
####  <a name="transfer-logins-task"></a><a name="TransferLogins"></a> Task "Anmeldungen übertragen"  
 In der folgenden Tabelle werden die benutzerdefinierten Protokolleinträge für den Task Anmeldungen übertragen aufgelistet.  
  
|Protokolleintrag|BESCHREIBUNG|  
|---------------|-----------------|  
|**TransferLoginsTaskFinishedTransferringObjects**|Zeigt an, dass das Übertragen von Anmeldungen beendet wurde.|  
|**TransferLoginsTaskStartTransferringObjects**|Zeigt an, dass das Übertragen von Anmeldungen gestartet wurde.|  
  
####  <a name="transfer-master-stored-procedures-task"></a><a name="TransferMasterStoredProcedures"></a> In master gespeicherte Prozeduren übertragen (Task)  
 In der folgenden Tabelle werden die benutzerdefinierten Protokolleinträge für den Task In master gespeicherte Prozeduren übertragen aufgelistet.  
  
|Protokolleintrag|BESCHREIBUNG|  
|---------------|-----------------|  
|**TransferStoredProceduresTaskFinishedTransferringObjects**|Zeigt an, dass das Übertragen von benutzerdefinierten gespeicherten Prozeduren, die in der **master** -Datenbank gespeichert sind, beendet wurde.|  
|**TransferStoredProceduresTaskStartTransferringObjects**|Zeigt an, dass das Übertragen von benutzerdefinierten gespeicherten Prozeduren, die in der **master** -Datenbank gespeichert sind, gestartet wurde.|  
  
####  <a name="transfer-sql-server-objects-task"></a><a name="TransferSQLServerObjects"></a> SQL Server-Objekte kopieren (Task)  
 In der folgenden Tabelle werden die benutzerdefinierten Protokolleinträge für den Task [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Objekte kopieren aufgelistet.  
  
|Protokolleintrag|BESCHREIBUNG|  
|---------------|-----------------|  
|**TransferSqlServerObjectsTaskFinishedTransferringObjects**|Zeigt an, dass das Übertragen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datenbankobjekten beendet wurde.|  
|**TransferSqlServerObjectsTaskStartTransferringObjects**|Zeigt an, dass das Übertragen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datenbankobjekten gestartet wurde.|  
  
####  <a name="web-services-task"></a><a name="WebServices"></a> Webdienste (Task)  
 In der folgenden Tabelle werden die benutzerdefinierten Protokolleinträge aufgelistet, die für den Task Webdienste aktiviert werden können.  
  
|Protokolleintrag|BESCHREIBUNG|  
|---------------|-----------------|  
|**WSTaskBegin**|Der Zugriff auf einen Webdienst wurde begonnen.|  
|**WSTaskEnd**|Eine Webdienstmethode wurde beendet.|  
|**WSTaskInfo**|Beschreibende Informationen zum Task.|  
  
####  <a name="wmi-data-reader-task"></a><a name="WMIDataReader"></a> WMI-Datenleser (Task)  
 In der folgenden Tabelle werden die benutzerdefinierten Protokolleinträge für den Task WMI-Datenleser aufgelistet.  
  
|Protokolleintrag|BESCHREIBUNG|  
|---------------|-----------------|  
|**WMIDataReaderGettingWMIData**|Zeigt an, dass das Lesen der WMI-Daten begonnen wurde.|  
|**WMIDataReaderOperation**|Berichtet die vom Task ausgeführte WQL-Abfrage.|  
  
####  <a name="wmi-event-watcher-task"></a><a name="WMIEventWatcher"></a> WMI-Ereignisüberwachung (Task)  
 In der folgenden Tabelle werden die benutzerdefinierten Protokolleinträge für den Task WMI-Ereignisüberwachung aufgelistet.  
  
|Protokolleintrag|BESCHREIBUNG|  
|---------------|-----------------|  
|**WMIEventWatcherEventOccurred**|Zeigt an, dass das vom Task überwachte Ereignis aufgetreten ist.|  
|**WMIEventWatcherTimedout**|Zeigt an, dass beim Task ein Timeout eingetreten ist.|  
|**WMIEventWatcherWatchingForWMIEvents**|Zeigt an, dass die Ausführung der WQL-Abfrage begonnen wurde. Der Eintrag schließt die Abfrage ein.|  
  
####  <a name="xml-task"></a><a name="XML"></a> XML-Task  
 In der folgenden Tabelle werden die benutzerdefinierten Protokolleinträge für den XML-Task beschrieben.  
  
|Protokolleintrag|BESCHREIBUNG|  
|---------------|-----------------|  
|**XMLOperation**|Stellt Informationen über den vom Task durchgeführten Vorgang bereit.|  

## <a name="related-tasks"></a>Related Tasks  
 Die folgende Liste enthält Links zu Themen, die die Ausführung von Tasks in Verbindung mit der Protokollierungsfunktion beschreiben.  
  
-   [Durch ein Integration Services-Paket protokollierte Ereignisse](../../integration-services/performance/events-logged-by-an-integration-services-package.md)  
  
## <a name="related-content"></a>Verwandte Inhalte  
 [DTLoggedExec-Tool für vollständige und Detailprotokollierung (CodePlex-Projekt)](https://go.microsoft.com/fwlink/?LinkId=150579)  
