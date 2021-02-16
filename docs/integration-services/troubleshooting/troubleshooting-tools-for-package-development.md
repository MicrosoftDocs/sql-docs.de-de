---
description: Tools zur Problembehandlung für die Paketentwicklung
title: Tools zur Problembehandlung für die Paketentwicklung | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Integration Services packages, troubleshooting
- SSIS packages, troubleshooting
- Integration Services, troubleshooting
- errors [Integration Services], troubleshooting
- packages [Integration Services], troubleshooting
ms.assetid: 41dd248c-dab3-4318-b8ba-789a42d5c00c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8555afca3e9b291dcbeb38269bac5603afaf37bc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352586"
---
# <a name="troubleshooting-tools-for-package-development"></a>Tools zur Problembehandlung für die Paketentwicklung

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] enthält Funktionen und Tools, mit denen Sie die Problembehandlung von Paketen vornehmen können, während Sie diese in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]entwickeln.  
  
## <a name="troubleshooting-design-time-validation-issues"></a>Behandlung von Problemen bei der Überprüfung zur Entwurfszeit  
 Wenn in der aktuellen Version von [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]ein Paket geöffnet wird, werden vom System alle Verbindungen überprüft, bevor alle Datenflusskomponenten überprüft werden, sowie alle langsamen oder nicht verfügbaren Verbindungen auf den Offlinemodus festgelegt. So kann die Verzögerung beim Überprüfen des Paketdatenflusses reduziert werden.  
  
 Wenn ein Paket geöffnet wurde, können Sie eine Verbindung auch deaktivieren, indem Sie mit der rechten Maustaste im Bereich **Verbindungs-Manager** auf den Verbindungs-Manager und anschließend auf **Offline arbeiten** klicken. So können Vorgänge im SSIS-Designer beschleunigt werden.  
  
 Auf den Offlinemodus festgelegte Verbindungen bleiben offline, bis Sie eine der folgenden Aktionen ausführen:  
  
-   Testen Sie die Verbindung, indem Sie mit der rechten Maustaste im Bereich **Verbindungs-Manager** des SSIS-Designers auf den Verbindungs-Manager und anschließend auf **Verbindung testen** klicken.  
  
     Eine Verbindung ist beispielsweise anfänglich auf den Offlinemodus festgelegt, wenn das Paket geöffnet wird. Sie ändern die Verbindungszeichenfolge, um das Problem zu beheben, und klicken auf **Verbindung testen** , um die Verbindung zu testen.  
  
-   Öffnen Sie das Projekt erneut, oder öffnen Sie das Projekt mit dem Paket erneut. Die Überprüfung wird erneut für alle Verbindungen im Paket ausgeführt.  
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] umfasst die folgenden, zusätzlichen Funktionen, um Ihnen das Vermeiden von Überprüfungsfehlern zu erleichtern:  
  
-   **Festlegen des gesamten Pakets und aller Verbindungen auf den Offlinemodus, wenn keine Datenquellen verfügbar sind**. Sie können **Offline arbeiten** im Menü **SSIS** aktivieren. Im Gegensatz zur **DelayValidation** -Eigenschaft ist die Option **Offline arbeiten** vor dem Öffnen eines Pakets verfügbar. Sie können die Option **Offline arbeiten** auch aktivieren, um die Vorgänge im Designer zu beschleunigen, und sie lediglich zum Überprüfen des Pakets deaktivieren.  
  
-   **Konfiguration der DelayValidation-Eigenschaft für Paketelemente, die bis zur Laufzeit nicht gültig sind**. Zum Verhindern von Überprüfungsfehlern können Sie für Paketelemente, deren Konfigurationen zur Entwurfszeit ungültig sind, **DelayValidation** auf **True** festlegen. Ein Beispiel hierfür wäre ein Datenflusstask, der eine Zieltabelle verwendet, die erst zur Laufzeit durch einen Task 'SQL ausführen' erstellt wird. Die **DelayValidation** -Eigenschaft kann auf Paketebene oder auf der Ebene der einzelnen, in den Paketen enthaltenen Tasks und Container aktiviert werden. In der Regel sollte bei der Bereitstellung des Pakets diese Eigenschaft für die betreffenden Paketelemente auf dem Wert **True** festgelegt bleiben, da andernfalls die gleichen Überprüfungsfehler zur Laufzeit auftreten.  
  
     Die **DelayValidation** -Eigenschaft kann für einen Datenflusstask, jedoch nicht für einzelne Datenflusskomponenten festgelegt werden. Sie erreichen ein ähnliches Ergebnis, wenn Sie die <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.ValidateExternalMetadata%2A> -Eigenschaft einzelner Datenflusskomponenten auf **FALSE**. Wenn jedoch der Wert dieser Eigenschaft auf **false** festgelegt ist, erkennt die Komponente keine Änderungen der Metadaten externer Datenquellen.  
  
 Wenn vom Paket verwendete Datenbankobjekte zum Zeitpunkt der Überprüfung gesperrt sind, reagiert der Überprüfungsvorgang möglicherweise nicht mehr. Unter diesen Umständen reagiert der [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer ebenfalls nicht mehr. Sie können die Überprüfung fortsetzen, indem Sie die zugehörigen Sitzung in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]schließen. Sie können dieses Problem auch mit den in diesem Abschnitt beschriebenen Einstellungen umgehen.  
  
## <a name="troubleshooting-control-flow"></a>Ablaufsteuerung (Problembehandlung)  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] enthält die folgenden Funktionen und Tools, mit denen Sie während der Paketentwicklung Probleme bei der Ablaufsteuerung in Paketen behandeln können:  
  
-   **Festlegen von Breakpoints für Tasks, Container sowie für das Paket**. Sie können dabei mithilfe der vom [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer bereitgestellten grafischen Tools Breakpoints festlegen. Breakpoints können auf Paketebene oder auf der Ebene der einzelnen, in den Paketen enthaltenen Tasks und Container aktiviert werden. Bestimmte Tasks und Container stellen zusätzliche Unterbrechungsbedingungen zum Festlegen von Breakpoints bereit. Beispielsweise können Sie eine Unterbrechungsbedingung für den For-Schleifencontainer aktivieren, welche die Ausführung zu Beginn jeder Iteration der Schleife anhält.  
  
-   **Verwenden der Debugfenster**. Beim Ausführen von Paketen mit Breakpoints ermöglichen die Debugfenster in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] Zugriff auf Variablenwerte und Statusmeldungen.  
  
-   **Überprüfen der Informationen auf der Registerkarte Status**. [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer werden zusätzliche Informationen zur Ablaufsteuerung beim Ausführen von Paketen in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. Auf der Registerkarte Status werden Tasks und Container in der Ausführungsreihenfolge aufgeführt. Diese Registerkarte enthält außerdem die Start- und Beendigungszeiten, Warnungen und Fehlermeldungen für jeden Task, Container sowie das Paket selbst.  
  
 Weitere Informationen zu diesen Funktionen finden Sie unter [Debugging Control Flow](../../integration-services/troubleshooting/debugging-control-flow.md).  
  
## <a name="troubleshooting-data-flow"></a>Datenfluss (Problembehandlung)  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] enthält die folgenden Funktionen und Tools, mit denen Sie während der Paketentwicklung Probleme mit Datenflüssen in Paketen behandeln können.  
  
-   **Durchführen von Tests mit einer Untermenge der Daten**. Wenn Sie für die Problembehandlung des Datenflusses eines Pakets nur eine Stichprobe des Datasets verwenden wollen, können Sie eine Transformation für Prozentwert-Stichproben oder eine Transformation für Zeilenstichproben einschließen, um zur Laufzeit eine Inline-Datenstichprobe zu erstellen. Weitere Informationen finden Sie unter [Percentage Sampling Transformation](../../integration-services/data-flow/transformations/percentage-sampling-transformation.md) und [Row Sampling Transformation](../../integration-services/data-flow/transformations/row-sampling-transformation.md).  
  
-   **Verwenden von Daten-Viewern zum Überwachen des Datenflusses**. Die Daten-Viewer zeigen Datenwerte an, während die Daten zwischen Quellen, Transformationen und Zielen verschoben werden. Ein Daten-Viewer kann Daten in einem Raster anzeigen. Sie können die Daten von einem Daten-Viewer in die Zwischenablage kopieren und anschließend in eine Datei wie z. B. eine Excel-Kalkulationstabelle einfügen. Weitere Informationen finden Sie unter [Debugging Data Flow](../../integration-services/troubleshooting/debugging-data-flow.md) entwickeln.  
  
-   **Konfigurieren von Fehlerausgaben für Datenflusskomponenten, die Fehlerausgaben unterstützen**. Viele Datenflussquellen, Transformationen und Ziele unterstützen Fehlerausgaben. Sie können die Fehlerausgabe einer Datenflusskomponente so konfigurieren, dass die Fehlerdaten an ein bestimmtes Ziel geleitet werden. Sie können die fehlerhaften oder abgeschnittenen Daten z. B. in einer separaten Textdatei aufzeichnen. Sie können darüber hinaus Fehlerausgaben Daten-Viewer anfügen, in denen Sie nur die fehlerhaften Daten überprüfen. Während der Entwurfszeit zeichnen die Fehlerausgaben fehlerhafte Datenwerte auf, um Sie beim Entwerfen von Paketen mit realistischen Daten zu unterstützen. Im Gegensatz zu anderen Tools und Funktionen zur Problembehandlung, die nur zur Entwurfszeit nützlich sind, bleibt die Nützlichkeit von Fehlerausgaben in der Produktionsumgebung erhalten. Weitere Informationen finden Sie unter [Fehlerbehandlung in Daten](../../integration-services/data-flow/error-handling-in-data.md).  
  
-   **Aufzeichnen der Anzahl der verarbeiteten Zeilen**. Wenn Sie ein Paket im [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer ausführen, wird die Anzahl der Zeilen, die einen Pfad durchlaufen haben, im Datenfluss-Designer angezeigt. Dieser Wert wird regelmäßig aktualisiert, während die Daten den Pfad durchlaufen. Sie können dem Datenfluss auch eine Transformation für Zeilenanzahl hinzufügen, um die endgültige Zeilenanzahl in einer Variablen aufzuzeichnen. Weitere Informationen finden Sie unter [Row Count Transformation](../../integration-services/data-flow/transformations/row-count-transformation.md).  
  
-   **Überprüfen der Informationen auf der Registerkarte Status**. [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer werden zusätzliche Informationen zu Datenflüssen beim Ausführen von Paketen in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. Auf der Registerkarte Status werden die Datenflusskomponenten in der Ausführungsreihenfolge aufgelistet sowie der Fortschritt der einzelnen Paketphasen in Prozent und die in das Ziel geschriebenen Zeilen.  
  
 Weitere Informationen zu diesen Funktionen finden Sie unter [Debugging Data Flow](../../integration-services/troubleshooting/debugging-data-flow.md).  
  
## <a name="troubleshooting-scripts"></a>Behandlung von Problemen mit Skripts  
 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] Tools for Applications (VSTA) ist die Entwicklungsumgebung, in der Sie die vom Skripttask und der Skriptkomponente verwendeten Skripts schreiben. VSTA bietet die folgenden Funktionen und Tools, mit denen Sie während der Paketentwicklung Probleme mit Skripts behandeln können:  
  
-   **Festlegen von Breakpoints für Skripts in Skripttasks.** VSTA stellt ausschließlich Unterstützung für das Debuggen von Skripts in Skripttasks bereit. Die in Skripttasks festgelegten Breakpoints werden mit den Breakpoints in Paketen sowie mit den Breakpoints der in Paketen enthaltenen Tasks und Containern integriert. Auf diese Weise wird das nahtlose Debuggen aller Paketelemente sichergestellt.  
  
    > [!NOTE]  
    >  Wenn Sie ein Paket debuggen, das mehrere Skripttasks umfasst, erreicht der Debugger nur in einem Skripttask Breakpoints und ignoriert die Breakpoints in den anderen Skripttasks. Ist ein Skripttask Bestandteil einer Foreach-Schleife oder eines For-Schleifencontainers, dann ignoriert der Debugger nach der ersten Schleifeniteration Breakpoints im Skripttask.  
  
 Weitere Informationen finden Sie unter [Debugging Script](../../integration-services/troubleshooting/debugging-script.md). Vorschläge zum Debuggen der Skriptkomponente finden Sie unter [Codieren und Debuggen der Skriptkomponente](../../integration-services/extending-packages-scripting/data-flow-script-component/coding-and-debugging-the-script-component.md).  
  
## <a name="troubleshooting-errors-without-a-description"></a>Behandlung von Fehlern ohne Beschreibung  
 Wenn Sie während der Paketentwicklung auf eine [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Fehlernummer ohne dazugehörige Beschreibung stoßen, finden Sie die entsprechende Beschreibung unter [Fehler- und Meldungsreferenz von Integration Services](../../integration-services/integration-services-error-and-message-reference.md). Die Liste enthält zurzeit keine Informationen zur Problembehandlung.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Behandlung von Problemen mit Paketausführungstools](../../integration-services/troubleshooting/troubleshooting-tools-for-package-execution.md)   
 [Funktionen für die Datenflussleistung](../../integration-services/data-flow/data-flow-performance-features.md)  
  
  
