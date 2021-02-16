---
description: CDC-Flusskomponenten
title: CDC-Flusskomponenten | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 5ae69ddf-27c3-467c-9af1-c89ec383f661
author: chugugrace
ms.author: chugu
ms.openlocfilehash: aa24cbdbafa67af73e3b1ee97592b42f6ac86d59
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347513"
---
# <a name="cdc-flow-components"></a>CDC-Flusskomponenten

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Die Change Data Capture-Komponenten von Attunity für Microsoft [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] unterstützen SSIS-Entwickler bei der Verwendung von CDC und reduzieren die Komplexität von CDC-Paketen.  
  
 Die SSIS-CDC-Komponenten können mit der CDC-Funktion von [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] verwendet werden, wenn die Quelltabellen entweder derselben [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] -Datenbank oder einer Oracle-Datenbank entsprechen (bei Verwendung des Oracle CDC Service für [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]). Partitionierte Tabellen werden unterstützt.  
  
 Zu den Komponenten zählen Steuerungs- und Datenflusskomponenten, die das Lesen und Verarbeiten von Änderungsdaten in SSIS-Paketen vereinfachen. Die Komponenten können der Komponentenbibliothek in Microsoft [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]hinzugefügt werden, werden aber separat installiert.  
  
 Folgende Change Data Capture-Komponenten von Attunity sind verfügbar:  
  
 **CDC-Ablaufsteuerungskomponente:**  
  
 [CDC Control Task](../../integration-services/control-flow/cdc-control-task.md)  
  
 **CDC-Datenflusskomponenten:**  
  
 [CDC-Quelle](../../integration-services/data-flow/cdc-source.md)  
  
 [CDC-Splitter](../../integration-services/data-flow/cdc-splitter.md)  
  
## <a name="installation"></a>Installation  
 In diesem Abschnitt werden die Installationsverfahren für die CDC-Komponenten für Microsoft [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)]beschrieben.  
  
 Die CDC-Komponenten für SSIS werden mit MicrosoftÂ® Change Data Capture Designer und Service für Oracle von Attunity für Microsoft SQL ServerÂ® in einem Paket bereitgestellt. Dieser Download ist Teil des SQL Server Feature Packs. Laden Sie die Komponenten des Feature Packs von der [SQL Server 2016 Feature Pack](https://www.microsoft.com/download/details.aspx?id=56833)-Webseite herunter.  
  
### <a name="version-support"></a>Versionsunterstützung

#### <a name="sql-server-version-support"></a>SQL Server-Versionsunterstützung

Die CDC-Komponenten für SSIS werden von allen unterstützten Versionen von Microsoft SQL Server unterstützt. Die derzeit unterstützten Versionen von SQL Server sind SQL Server 2012 bis SQL Server 2017.

#### <a name="operating-system-version-support"></a>Unterstützung der Betriebssystemversion
  
Die CDC-Komponenten für SSIS werden auf den angegebenen Betriebssystemen und Plattformen unterstützt:  
  
-   Windows 8 und 8.1
-   Windows 10  
-   Windows Server 2012 und 2012 R2
-   Windows Server 2016
  
### <a name="running-the-installation-program"></a>Ausführen des Installationsprogramms  
 Stellen Sie vor dem Ausführen des Installations-Assistenten sicher, dass [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)][!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] geschlossen ist. Folgen Sie anschließend den Anweisungen im Installations-Assistenten.  
  
### <a name="restart-ssis-service"></a>Neustarten des SSIS-Diensts 
Nachdem Sie die CDC-Komponenten installiert haben, müssen Sie den SSIS-Dienst neu starten, um sicherzustellen, dass die Komponenten beim Entwickeln von Paketen in SQL [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] ordnungsgemäß funktionieren.  
  
Nach der Installation der Komponenten wird eine Meldung angezeigt. Klicken Sie auf **Ja** , wenn eine Aufforderung angezeigt wird.  
  
### <a name="uninstalling-the-microsoft-cdc-components"></a>Deinstallieren der Microsoft-CDC-Komponenten  
 Die CDC-Quelle, der CDC-Splitter und der CDC-Steuerungstask werden mithilfe des Deinstallations-Assistenten deinstalliert. Wenn Sie [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)][!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] zur Paketentwicklung verwenden, muss [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] vor dem Ausführen des Deinstallations-Assistenten geschlossen werden.  
  
## <a name="benefits"></a>Vorteile  
 Die CDC-Komponenten für [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)][!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] ermöglichen SSIS-Entwicklern das mühelose Erstellen von SSIS-Paketen, die Änderungsdaten verarbeiten. Diese Komponenten erleichtern SSIS-Entwicklern die Verwendung von CDC und reduzieren die Komplexität von CDC-Paketen.  
  
 Die SSIS-CDC-Komponenten werden verwendet, um die Änderungsdaten in einer Form bereitzustellen, in der sie einfach zur Replikation, zum Laden eines Data Warehouse, Aktualisieren langsam veränderlicher Dimensionen für OLAP, Überwachen von Änderungen oder für weitere mögliche Zwecke verarbeitet werden können. Die Art der weiteren Verarbeitung wird vom SSIS-Entwickler bestimmt.  
  
 Die SSIS-CDC-Komponenten sind zur Verwendung mit der CDC-Funktion von [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mit Änderungstabellen konzipiert, die sich in derselben [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] -Datenbank befinden.  
  
## <a name="getting-started-with-the-change-data-capture-components"></a>Erste Schritte mit den Change Data Capture-Komponenten  
 Ein typisches CDC-Paket verarbeitet Änderungen an einer Gruppe von Tabellen. Die folgende Abbildung zeigt den grundlegenden Ablaufsteuerungsteil dieses CDC-Pakettyps. Das Paket wird als Trickle-Feed-Verarbeitungspaket bezeichnet.  
  
 ![Paketablaufsteuerung für Trickle-Feed-Verarbeitung](../../integration-services/data-flow/media/tricklefeedprocessing.gif "Paketablaufsteuerung für Trickle-Feed-Verarbeitung")  
  
 Diese [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)][!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Ablaufsteuerung enthält zwei CDC-Steuerungstasks und den Datenflusstask. Der erste Task mit dem Namen **Get CDC Processing Range** legt den LSN-Bereich für die Änderungen fest, die im Datenflusstask mit dem Namen **Process Changes** verarbeitet werden. Dieser Bereich wird auf Grundlage dessen festgelegt, was während der letzten Paketausführung verarbeitet und in einem permanenten Speicher gespeichert wurde.  
  
 Weitere Informationen zur Verwendung der CDC-Steuerungstasks finden Sie unter [CDC-Steuerungstask](../../integration-services/control-flow/cdc-control-task.md) und [CDC-Steuerungstask-Editor](../control-flow/cdc-control-task.md).  
  
 Die folgende Abbildung zeigt den Datenfluss für die **Änderungsverarbeitung** und veranschaulicht, wie Änderungen verarbeitet werden.  
  
 ![Datenfluss für Änderungsverarbeitung](../../integration-services/data-flow/media/processchangesdataflow.gif "Datenfluss für Änderungsverarbeitung")  
  
 Die folgenden Schritte werden in der Abbildung dargestellt:  
  
-   **Änderungen für Tabelle X** ist eine CDC-Quelle, die Änderungen an Tabelle X in dem von der übergeordneten Ablaufsteuerung bestimmten CDC-Verarbeitungsbereich liest.  
  
-   **CDC-Splitter X** wird verwendet, um die Änderungen in Einfügungen, Löschungen und Updates zu teilen. In diesem Szenario wird davon ausgegangen, dass die CDC-Quelle zum Erzeugen von Nettoänderungen konfiguriert ist, sodass andere Änderungstypen parallel verarbeitet werden können.  
  
-   Die spezifischen Änderungen werden dann downstream weiter verarbeitet. In dieser Abbildung werden die Änderungen in Tabellen mit mehreren ODBC-Zielen eingefügt, in der Realität kann die Verarbeitung jedoch anders aussehen.  
  
 Weitere Informationen zur CDC-Quelle finden Sie unter:  
  
 [CDC-Quelle](../../integration-services/data-flow/cdc-source.md)  
  
 [Quellen-Editor für CDC &#40;Seite „Verbindungs-Manager“&#41;](./cdc-source.md)  
  
 [Quellen-Editor für CDC &#40;Seite „Spalten“&#41;](./cdc-source.md)  
  
 [Quellen-Editor für CDC &#40;Seite „Fehlerausgabe“&#41;](./cdc-source.md)  
  
 Weitere Informationen zum CDC-Splitter finden Sie unter:  
  
 [CDC-Splitter](../../integration-services/data-flow/cdc-splitter.md)  
  
 Eines der grundlegenden Probleme, das beim Erstellen von CDC-Paketen berücksichtigt werden muss, ist die Art und Weise, wie die Änderungsverarbeitung mit dem anfänglichen Ladevorgang (oder der anfänglichen Verarbeitung) der Daten interagiert.  
  
 Die CDC-Komponenten unterstützen drei unterschiedliche Szenarien für den anfänglichen Ladevorgang und die anfängliche Änderungsverarbeitung:  
  
-   Für den anfänglichen Ladevorgang wird eine Datenbankmomentaufnahme verwendet. In diesem Fall beginnt die Änderungsverarbeitung bei der LSN des Momentaufnahmeereignisses.  
  
-   Anfängliches Laden aus einer inaktiven Datenbank. In diesem Fall werden während des anfänglichen Ladevorgangs keine Änderungen vorgenommen. Daher wird die aktuelle LSN an einem Punkt während des Ladevorgangs abgefragt, und die Verarbeitung beginnt bei dieser LSN.  
  
-   Anfängliches Laden aus einer aktiven Datenbank. In diesem Fall werden während des anfänglichen Ladevorgangs Änderungen an der Datenbank vorgenommen, und es gibt keine spezifische LSN, bei der die Änderungsverarbeitung gestartet werden kann. Der Entwickler des anfänglich geladenen Pakets kann die aktuelle LSN der Quelldatenbank vor und nach dem anfänglichen Laden abfragen. In diesem Fall ist beim Verarbeiten von Änderungen, die parallel zum anfänglichen Ladevorgang vorgenommen werden, Vorsicht geboten, da einige der verarbeiteten Änderungen bereits im anfänglichen Ladevorgang vorhanden waren (eine Insert-Änderung kann z. B. mit einem Fehler aufgrund doppelter Schlüssel fehlschlagen, da die eingefügte Zeile beim anfänglichen Ladeprozess gelesen wurde).  
  
 Die folgende Abbildung zeigt ein SSIS-Paket, das die ersten zwei Szenarien unterstützt:  
  
 ![SSIS-Paketbehandlung für die ersten beiden Szenarios](../../integration-services/data-flow/media/scenarioonetwo.gif "SSIS-Paketbehandlung für die ersten beiden Szenarios")  
  
 Die folgende Abbildung zeigt ein SSIS-Paket, das das dritte Szenario unterstützt:  
  
 ![SSIS-Paketbehandlung, Szenario 3](../../integration-services/data-flow/media/scenario3.gif "SSIS-Paketbehandlung, Szenario 3")  
  
 Nach dem anfänglich geladenen Paket wird ein Trickle-Feed-Updatepaket nach einem Zeitplan wiederholt ausgeführt, um Änderungen zu verarbeiten, sobald sie verfügbar werden.  
  
 Das Übergeben des Status der CDC-Verarbeitung vom anfänglich geladenen Paket an das Trickle-Feed-Paket und zwischen anderen Tasks innerhalb jedes Pakets erfolgt mithilfe einer speziellen SSIS-Paketzeichenfolgenvariable. Der Wert dieser Variable wird als CDC-Status bezeichnet und gibt den aktuellen Status der CDC-Verarbeitung für die Tabellengruppen wieder, die vom anfänglich geladenen Paket und vom Trickle-Feed-Paket behandelt werden.  
  
 Der Wert der CDC-Statusvariablen muss im dauerhaften Speicher beibehalten werden. Er sollte vor dem Starten der CDC-Verarbeitung gelesen werden und nach Abschluss der Verarbeitung mit dem aktuellen Zustand gespeichert werden. Das Laden und Speichern des CDC-Status kann vom SSIS-Entwickler ausgeführt werden, die CDC-Steuerungskomponente bietet jedoch die Möglichkeit, diesen Task zu automatisieren, indem der CDC-Statuswert in einer Datenbanktabelle verwaltet wird.  
  
## <a name="security-considerations"></a>Sicherheitsüberlegungen  
 In diesem Abschnitt werden einige Sicherheitsüberlegungen im Zusammenhang mit der Verwendung der CDC-Komponenten in SSIS erläutert.  
  
### <a name="access-authorization-to-change-data"></a>Autorisierung des Zugriffs auf Änderungsdaten  
 Trickle-Feed-Updatepakete erfordern Zugriff auf die CDC-Funktionen von [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] . Dieser Zugriff wird standardmäßig Mitgliedern der festen Datenbankrolle **db_owner** gewährt. Da **db_owner** eine Rolle mit umfassenden Berechtigungen ist, wird empfohlen, jeder Aufzeichnungsinstanz bei ihrer Definition in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] eine Gatingsicherheitsrolle zuzuordnen, die es dem SSIS-CDC-Paket ermöglicht, die Änderungen von einem Benutzer mit sehr viel geringeren Berechtigungen verarbeiten zu lassen.  
  
### <a name="access-to-cdc-database-current-lsn"></a>Zugriff auf die aktuelle LSN der CDC-Datenbank  
 Die CDC-Steuerungstaskvorgänge zum Markieren der Start-LSN für die Änderungsverarbeitung müssen in der Lage sein, die aktuelle LSN der CDC-Datenbank zu finden. Die Komponenten finden die LSN mit der **sp_replincrementlsn**-Prozedur der master-Datenbank. Die Ausführungsberechtigung für diese Prozedur muss dem Anmeldenamen zugewiesen werden, der zum Herstellen einer Verbindung mit der [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] CDC-Datenbank verwendet wird.  
  
### <a name="access-to-cdc-states-table"></a>Zugriff auf die CDC-Statustabelle  
 Die CDC-Statustabelle dient zum automatischen Beibehalten von CDC-Status, bei denen es erforderlich ist, dass sie von dem zum Herstellen einer Verbindung mit der [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] CDC-Datenbank verwendeten Anmeldenamen aktualisiert werden können. Da diese Tabelle vom SSIS-Entwickler erstellt wird, kann der [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] -Systemadministrator als ein Benutzer festgelegt werden, der zum Erstellen von [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] -Datenbanken sowie zum Ausführen von administrativen Tasks und Wartungstasks autorisiert ist. Ein [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] -Systemadministrator, der mit CDC-fähigen Datenbanken arbeitet, muss zudem über entsprechende Kenntnisse in der [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] CDC-Technologie und -Implementierung verfügen.  
  
## <a name="grouping-tables-for-cdc-processing"></a>Gruppieren von Tabellen für die CDC-Verarbeitung  
 Die Größe von Datenbankprojekten variiert zwischen einigen Tabellen und Tausenden von Tabellen. Beim Entwerfen der anfänglich geladenen Pakete und CDC-Pakete ist es von Vorteil, Tabellen in viel kleineren Gruppen zu gruppieren, um die Verwaltung zu vereinfachen und die Effizienz zu erhöhen. In diesem Abschnitt werden verschiedene Überlegungen im Zusammenhang mit der Sortierung von Tabellen in kleinen Gruppen erläutert. Die Tabellen in jedem Paket werden dabei anfänglich geladen und dann als Gruppe aktualisiert.  
  
 Die von CDC-Komponenten unterstützten CDC-Muster setzen voraus, dass diese Gruppierung bereits festgelegt ist. Jede Gruppe definiert einen separaten CDC-Kontext, der getrennt von anderen Gruppen verwaltet wird. Für jede Gruppe werden ein anfänglich geladenes Paket und ein Trickle-Feed-Updatepaket erstellt. Trickle-Feed-Updates werden auf Grundlage der für die Änderungsverarbeitung geltenden Einschränkungen (z. B. CPU- und EA-Verbrauch sowie Auswirkungen auf andere Systeme) und der gewünschten Latenz zur regelmäßigen Ausführung geplant.  
  
 Tabellen werden nach folgenden Gesichtspunkten gruppiert:  
  
1.  Entsprechend der Zieldatenbank. Alle Tabellen, die in unterschiedliche Zieldatenbanken geschrieben werden oder eine andere Verarbeitung durchlaufen, sollten verschiedenen CDC-Gruppen zugewiesen werden.  
  
2.  Tabellen, die mit referenziellen Integritätseinschränkungen zusammenhängen, sollten der gleichen Gruppe zugewiesen werden, um Probleme mit der referenziellen Integrität in der Zieldatenbank zu vermeiden.  
  
3.  Tabellen, bei denen eine höhere Latenz akzeptabel ist, können gruppiert werden, damit sie weniger häufig verarbeitet werden können und die Gesamtsystembelastung reduziert wird.  
  
4.  Tabellen mit einer höheren Änderungsrate sollten kleineren Gruppen zugewiesen werden, und Tabellen mit einer niedrigen Änderungsrate können in größeren Gruppen gruppiert werden.  
  
 Die folgenden zwei Pakete werden für jede CDC-Gruppe erstellt:  
  
-   Ein anfänglich geladenes Paket, das sämtliche Daten aus den Quelltabellen liest und auf die Zieltabellen anwendet.  
  
-   Ein Trickle-Feed-Updatepaket, das Änderungen an den Quelltabellen liest und die Änderungen auf die Zieltabellen anwendet. Dieses Paket sollte regelmäßig anhand eines Zeitplans ausgeführt werden.  
  
## <a name="cdc-state"></a>CDC-Status  
 Jeder CDC-Gruppe ist ein Status zugeordnet, der durch eine Zeichenfolge mit einem bestimmten Format dargestellt wird. Weitere Informationen finden Sie unter [CDC-Steuerungstask](../../integration-services/control-flow/cdc-control-task.md). In der folgenden Tabelle sind die möglichen CDC-Statuswerte aufgeführt.  
  
|Zustand|BESCHREIBUNG|  
|-----------|-----------------|  
|0-(INITIAL)|Der Status, bevor alle Pakete in der aktuellen CDC-Gruppe ausgeführt werden. Dieser Status liegt auch vor, wenn der CDC-Status leer ist.<br /><br /> Weitere Informationen zu CDC-Steuerungstaskvorgängen finden Sie unter [CDC-Steuerungstask](../../integration-services/control-flow/cdc-control-task.md).|  
|1-ILSTART (Initial-Load-Started)|Der Status beim Start des anfänglich geladenen Pakets. Dieser Schritt erfolgt nach dem Aufruf des CDC-Steuerungstasks durch den **MarkInitialLoadStart** -Vorgang.<br /><br /> Weitere Informationen zu CDC-Steuerungstaskvorgängen finden Sie unter [CDC-Steuerungstask](../../integration-services/control-flow/cdc-control-task.md).|  
|2- ILEND (Initial-Load-Ended)|Der Status bei erfolgreicher Beendigung des anfänglich geladenen Pakets. Dieser Schritt erfolgt nach dem Aufruf des CDC-Steuerungstasks durch den MarkInitialLoadEnd-Vorgang.<br /><br /> Weitere Informationen zu CDC-Steuerungstaskvorgängen finden Sie unter [CDC-Steuerungstask](../../integration-services/control-flow/cdc-control-task.md).|  
|3-ILUPDATE (Initial Load Update)|Der Status nach der ersten Ausführung des Updatepakets nach dem anfänglichen Ladevorgang, während der anfängliche Verarbeitungsbereich noch verarbeitet wird. Dieser Schritt erfolgt nach dem Aufruf des CDC-Steuerungstasks durch den **GetProcessingRange** -Vorgang.<br /><br /> Wenn die **_$reprocessing** -Spalte verwendet wird, wird sie auf 1 festgelegt, um anzugeben, dass das Paket möglicherweise Zeilen erneut verarbeitet, die bereits im Ziel vorhanden sind.<br /><br /> Weitere Informationen zu CDC-Steuerungstaskvorgängen finden Sie unter [CDC-Steuerungstask](../../integration-services/control-flow/cdc-control-task.md).|  
|4-TFEND (Trickle-Feed-Update-Ended)|Der für reguläre CDC-Ausführungen erwartete Status. Er gibt an, dass die vorherige Ausführung erfolgreich abgeschlossen wurde und eine neue Ausführung mit einem neuen Verarbeitungsbereich gestartet werden kann.|  
|5-TFSTART (Trickle-Feed-Update-Started)|Der Status bei nachfolgenden Ausführungen des Updatepakets nach dem Aufruf des CDC-Steuerungstasks durch den **GetProcessingRange** -Vorgang.<br /><br /> Er gibt an, dass eine reguläre CDC-Ausführung gestartet, aber noch nicht einwandfrei beendet wurde (**MarkProcessedRange**).<br /><br /> Weitere Informationen zu CDC-Steuerungstaskvorgängen finden Sie unter [CDC-Steuerungstask](../../integration-services/control-flow/cdc-control-task.md).|  
|6-TFREDO (Reprocessing-Trickle-Feed-Updates)|Der Status bei einem **GetProcessingRange** -Vorgang, der nach TFSTART stattfindet. Er gibt an, dass die vorherige Ausführung nicht erfolgreich abgeschlossen wurde.<br /><br /> Wenn die __$reprocessing-Spalte verwendet wird, wird sie auf 1 festgelegt, um anzugeben, dass das Paket möglicherweise Zeilen erneut verarbeitet, die bereits im Ziel vorhanden sind.|  
|7-FEHLER|Die CDC-Gruppe befindet sich in einem Fehlerstatus.|  
  
 Hier ist das Statusdiagramm für die CDC-Komponenten. Ein Fehlerstatus liegt vor, wenn ein nicht erwarteter Status erreicht wird. Die erwarteten Status werden im folgenden Diagramm dargestellt. Das Diagramm zeigt jedoch nicht den Fehlerstatus an.  
  
 Wenn Sie am Ende eines anfänglich geladenen Pakets z. B. versuchen, den Status auf ILEND festzulegen, und der Status TFSTART lautet, befindet sich die CDC-Gruppe in einem Fehlerstatus, und das Trickle-Feed-Updatepaket wird nicht ausgeführt (das anfänglich geladene Paket wird ausgeführt).  
  
 ![Zustandsdiagramm](../../integration-services/data-flow/media/statediagram.gif "Zustandsdiagramm")  
  
 Sobald das anfänglich geladene Paket erfolgreich ausgeführt wurde, wird das Trickle-Feed-Updatepaket nach einem zuvor festgelegten Zeitplan wiederholt ausgeführt, um Änderungen an den Quelltabellen zu verarbeiten. Jede Ausführung des Trickle-Feed-Updatepakets ist eine CDC-Ausführung.  
  
## <a name="in-this-section"></a>In diesem Abschnitt  
  
-   [CDC-Quelle](../../integration-services/data-flow/cdc-source.md)  
  
-   [CDC-Splitter](../../integration-services/data-flow/cdc-splitter.md)  
  
## <a name="related-tasks"></a>Related Tasks  
  
-   [Weiterleiten des CDC-Datenstroms gemäß Änderungstyp](../../integration-services/data-flow/direct-the-cdc-stream-according-to-the-type-of-change.md)  
  
-   [Definieren einer Statusvariablen](../../integration-services/data-flow/define-a-state-variable.md)  
  
## <a name="related-content"></a>Verwandte Inhalte  
  
-   Blogeintrag zu [CDC in SSIS für SQL Server 2012](https://www.mattmasson.com/2011/12/cdc-in-ssis-for-sql-server-2012-2/)auf mattmasson.com.  
  
-   Blogeintrag zum Einrichten des CDC-Diensts [CDC für Oracle in SQL Server 2012](https://sqlblog.iridule.net/index.php/2013/04/15/sql-server-2012-cdc-for-oracle-a-review-of-one-implementation/)auf blogs.msdn.com.  
  
-   Technischer Artikel [Installieren von Microsoft SQL Server 2012 Change Data Capture für Oracle von Attunity](https://go.microsoft.com/fwlink/?LinkId=252958)auf social.technet.microsoft.com.  
  
-   Technischer Artikel [Troubleshoot Configuration Issues in Microsoft Change Data Capture for Oracle by Attunity](https://go.microsoft.com/fwlink/?LinkId=252960)(Behandeln von Konfigurationsproblemen in Microsoft Change Data Capture für Oracle von Attunity) auf social.technet.microsoft.com.  
  
-   Technischer Artikel [Behandeln von Fehlern bei CDC-Instanzen in Microsoft Change Data Capture für Oracle von Attunity](https://go.microsoft.com/fwlink/?LinkId=252961)auf social.technet.microsoft.com.  
  
-   Video [CDC für Oracle-Datenbanken mit SQL Server Integration Services 2012 (SQL Server Video)](/previous-versions/dn912438(v=msdn.10))auf technet.microsoft.com.  
  
## <a name="see-also"></a>Weitere Informationen  
 [CDC Control Task](../../integration-services/control-flow/cdc-control-task.md)  
  
