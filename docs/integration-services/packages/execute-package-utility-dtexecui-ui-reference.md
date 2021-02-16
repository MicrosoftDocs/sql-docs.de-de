---
description: Paketausführungsprogramm (dtexecui)
title: Paketausführungsprogramm (dtexecui) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.dtexecui.setvalues.f1
- sql13.dts.dtexecui.reporting.f1
- sql13.dts.dtexecui.datasources.f1
- sql13.dts.dtexecui.commandfiles.f1
- sql13.dts.dtexecui.logging.f1
- sql13.dts.dtexecui.general.f1
- sql13.dts.dtexecui.verification.f1
- sql13.dts.dtexecui.executionoptions.f1
- sql13.dts.dtexecui.commandline.f1
- sql13.dts.dtexecui.configuration.f1
helpviewer_keywords:
- DTExecUI utility
ms.assetid: 3d71df39-126b-4c8e-bd77-128bbd5b0887
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 14392191ab3d11165f75f1637c2fb3330aa886ad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346909"
---
# <a name="execute-package-utility-dtexecui"></a>Paketausführungsprogramm (dtexecui)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Verwenden Sie das **Paketausführungshilfsprogramm** , um [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Pakete auszuführen. Das Hilfsprogramm führt Pakete aus, die an einem von drei Speicherorten gespeichert wurden: in der [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbank, im [!INCLUDE[ssIS](../../includes/ssis-md.md)]-Paketspeicher und im Dateisystem. Diese Benutzeroberfläche, die über [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder durch Eingeben von **dtexecui** an der Eingabeaufforderung geöffnet werden kann, stellt eine Alternative zum Ausführen von Paketen mithilfe des Eingabeaufforderungstools **DTExec** dar.  
  
 Pakete werden im selben Prozess ausgeführt wie das **dtexecui.exe** -Hilfsprogramm. Da es sich bei diesem Hilfsprogramm um ein 32-Bit-Tool handelt, werden Pakete, die mithilfe von **dtexecui.exe** in einer 64-Bit-Umgebung ausgeführt werden, in Windows unter Win32 (WOW) ausgeführt. Beim Entwickeln und Testen von Befehlen mithilfe des Paketausführungshilfsprogramms (dtexecui.exe) auf einem 64-Bit-Computer sollten Sie die Befehle mithilfe der 64-Bit-Version von **dtexec.exe** im 64-Bit-Modus testen, bevor Sie die Befehle auf einem Produktionsserver bereitstellen oder deren Ausführung planen.  
  
 Das **Paketausführungshilfsprogramm** ist eine grafische Benutzeroberfläche für das Eingabeaufforderungs-Hilfsprogramm **DTExec** . Die Benutzeroberfläche erleichtert das Konfigurieren von Optionen und stellt automatisch die Befehlszeile zusammen, die an das Eingabeaufforderungs-Hilfsprogramm **DTExec** übergeben wird, wenn Sie das Paket unter den angegebenen Optionen ausführen.  
  
 Das **Paketausführungshilfsprogramm** eignet sich auch zum Erstellen von Befehlszeilen, die Sie verwenden, wenn **DTExec** direkt ausgeführt wird.  
  
### <a name="to-open-execute-package-utility-in-sql-server-management-studio"></a>So öffnen Sie das Paketausführungsprogramm in SQL Server Management Studio  
  
1.  Klicken Sie in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]im Menü **Ansicht** auf **Objekt-Explorer**.  
  
2.  Klicken Sie im Objekt-Explorer auf **Verbinden**, und klicken Sie anschließend auf **Integration Services**.  
  
3.  Geben Sie im Dialogfeld **Verbindung mit Server herstellen** in der Liste **Servername** den Servernamen ein, und klicken Sie anschließend auf **Verbinden**.  
  
4.  Erweitern Sie den Ordner **Gespeicherte Pakete** und dessen Unterordner, klicken Sie mit der rechten Maustaste auf das Paket, das Sie ausführen möchten, und klicken Sie anschließend auf **Paket ausführen**.  
  
### <a name="to-open-the-execute-package-utility-at-the-command-prompt"></a>So öffnen Sie das Paketausführungsprogramm an der Eingabeaufforderung  
  
-   Öffnen Sie ein Eingabeaufforderungsfenster, und führen Sie **dtexecui** aus.  
  
 In den folgenden Abschnitten werden Seiten des Dialogfelds **Paketausführungshilfsprogramm** beschrieben.  
  
## <a name="general-page"></a>Seite Allgemein  
 Auf der Seite **Allgemein** des Dialogfelds **Paketausführungshilfsprogramm** können Sie einen Namen und einen Speicherort für das Paket angeben.  
  
 Das Paketausführungshilfsprogramm (dtexecui.exe) führt Pakete immer auf dem lokalen Computer aus, auch wenn das Paket auf einem Remoteserver gespeichert ist. Falls das Remotepaket Konfigurationsdateien verwendet, die ebenfalls auf dem Remoteserver gespeichert sind, kann das Paketausführungshilfsprogramm die Konfigurationen u. U. nicht finden und das Paket nicht ausführen. Zum Vermeiden dieses Problems müssen für Verweise auf Konfigurationen UNC-Freigabenamen (Universal Naming Convention) verwendet werden, z.B. \\\myserver\myfile.  
  
### <a name="static-options"></a>Statische Optionen  
 **Paketquelle**  
 Geben Sie mithilfe der folgenden Optionen den Speicherort des Pakets an:  
  
|Wert|BESCHREIBUNG|  
|-|-|  
|**SQL Server**|Wählen Sie diese Option, wenn das Paket auf [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gespeichert ist. Geben Sie eine Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und einen Benutzernamen und das Kennwort für die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Authentifizierung an. Für jeden Benutzernamen und jedes Kennwort werden an der Eingabeaufforderungen die Optionen **/USER** _Benutzername_ und **/PASSWORD** _Kennwort_ hinzugefügt.|  
|**Dateisystem**|Wählen Sie diese Option, wenn das Paket im Dateisystem gespeichert ist.|  
|**SSIS-Paketspeicher**|Wählen Sie diese Option aus, wenn das Paket im [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Paketspeicher gespeichert ist.|  
  
 Mit jeder dieser Auswahlmöglichkeiten sind die folgenden Optionen verbunden.  
  
 **Ausführen**  
 Klicken Sie hier, um das Paket auszuführen.  
  
 **Schließen**  
 Klicken Sie hier, um das Dialogfeld **Paketausführungshilfsprogramm** zu schließen.  
  
### <a name="dynamic-options"></a>Dynamische Optionen  
  
#### <a name="package-source--sql-server"></a>Package Source = SQL Server  
 **Server**  
 Geben Sie den Namen des Servers ein, auf dem das Paket gespeichert ist, oder wählen Sie einen Server aus der Liste aus.  
  
 **Am Server anmelden**  
 Gibt an, ob das Paket zum Herstellen der Verbindung mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die Windows-Authentifizierung oder die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Authentifizierung verwenden soll. Im Sinne einer größeren Sicherheit wird die Windows-Authentifizierung empfohlen. Bei der Windows-Authentifizierung müssen Sie keinen Benutzernamen und das zugehörige Kennwort angeben.  
  
 **Windows-Authentifizierung verwenden**  
 Wählen Sie diese Option aus, um die Windows-Authentifizierung zu verwenden und sich mithilfe eines [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows-Benutzerkontos anzumelden.  
  
 **SQL Server-Authentifizierung verwenden**  
 Wählen Sie diese Option aus, um die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Authentifizierung zu verwenden. Wenn ein Benutzer eine Verbindung mit einem angegebenen Benutzernamen und einem Kennwort von einer nicht vertrauenswürdigen Verbindung herstellt, führt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die Authentifizierung aus, indem überprüft wird, ob ein [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anmeldekonto eingerichtet wurde und ob das angegebene Kennwort mit dem zuvor aufgezeichneten übereinstimmt. Wenn [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] das Anmeldekonto nicht finden kann, schlägt die Authentifizierung fehl und der Benutzer erhält eine Fehlermeldung.  
  
> [!IMPORTANT]  
>  Verwenden Sie nach Möglichkeit die Windows-Authentifizierung.  
  
 **Pakete**  
 Geben Sie den Namen des Pakets ein, oder klicken Sie auf die Schaltfläche mit den Auslassungspunkten **(...)**, um das Paket mithilfe des Dialogfelds **SSIS-Paket auswählen** zu suchen.  
  
#### <a name="package-source--file-system"></a>Package Source = File System  
 **Pakete**  
 Geben Sie den Namen des Pakets ein, oder klicken Sie auf die Schaltfläche mit den Auslassungspunkten **(...)**, um das Paket mithilfe des Dialogfelds „Öffnen“ zu suchen. In dem Dialogfeld werden standardmäßig nur Dateien mit der Erweiterung .dtsx aufgelistet.  
  
#### <a name="package-source--ssis-package-store"></a>Package Source = SSIS Package Store  
 **Server**  
 Geben Sie den Namen des Computers ein, auf dem das Paket gespeichert ist, oder wählen Sie einen Computer aus der Liste aus.  
  
 **Am Server anmelden**  
 Gibt an, ob das Paket die Microsoft Windows-Authentifizierung verwenden soll, um eine Verbindung mit der Paketquelle herzustellen. Im Sinne einer größeren Sicherheit wird die Windows-Authentifizierung empfohlen. Bei der Windows-Authentifizierung müssen Sie keinen Benutzernamen und das zugehörige Kennwort angeben.  
  
 **Windows-Authentifizierung verwenden**  
 Wählen Sie diese Option aus, um die Windows-Authentifizierung zu verwenden und sich mithilfe eines Microsoft Windows-Benutzerkontos anzumelden.  
  
 **SQL Server-Authentifizierung verwenden**  
 Diese Option ist nicht verfügbar, wenn Sie ein in **SSIS-Paketspeicher** gespeichertes Paket ausführen.  
  
 **Pakete**  
 Geben Sie den Namen des Pakets ein, oder klicken Sie auf die Schaltfläche mit den Auslassungspunkten **(...)**, um das Paket mithilfe des Dialogfelds **SSIS-Paket auswählen** zu suchen.  
  
## <a name="configurations-page"></a>Konfigurationsseite  
 Auf der Seite **Konfigurationen** des Dialogfelds **Paketausführungsprogramm** wählen Sie die Konfigurationsdateien aus, die zur Laufzeit geladen werden sollen, und geben Sie die Reihenfolge an, in der sie geladen werden sollen.  
  
### <a name="options"></a>Tastatur  
 **Konfigurationsdateien**  
 Listet die vom Paket verwendeten Konfigurationen auf. Durch jede der Konfigurationsdateien wird der Eingabeaufforderung eine Option **/CONFIGFILE filename** hinzugefügt.  
  
 **Pfeiltasten**  
 Wählen Sie eine Konfigurationsdatei in der Liste aus, und ändern Sie die Reihenfolge für den Ladevorgang mithilfe der Pfeiltasten auf der rechten Seite. Die Konfigurationen werden beginnend mit der obersten in der Listenreihenfolge geladen.  
  
> [!NOTE]  
>  Wenn eine Eigenschaft durch mehrere Konfigurationen geändert wird, wird die zuletzt geladene Konfiguration verwendet.  
  
 **Add (Hinzufügen)**  
 Klicken Sie auf diese Option, um Konfigurationen mithilfe des Dialogfelds **Öffnen** hinzuzufügen. In dem Dialogfeld werden standardmäßig nur Dateien mit der Erweiterung .dtsconfig aufgelistet.  
  
 **Remove**  
 Wählen Sie in der Liste eine Konfigurationsdatei aus, und klicken Sie anschließend auf **Entfernen**.  
  
 **Ausführen**  
 Klicken Sie hier, um das Paket auszuführen.  
  
 **Schließen**  
 Klicken Sie hier, um das Dialogfeld **Paketausführungshilfsprogramm** zu schließen.  
  
## <a name="command-files-page"></a>Seite "Befehlsdateien"  
 Wählen Sie auf der Seite **Befehlsdateien** des Dialogfelds **Paketausführungsprogramm** die Befehlsdateien aus, die zur Laufzeit geladen werden sollen.  
  
### <a name="options"></a>Tastatur  
 **Befehlsdateien**  
 Führt die vom Paket verwendeten Befehlsdateien auf. Ein Paket kann mehrere Dateien verwenden, um Befehlszeilenoptionen festzulegen.  
  
 **Pfeiltasten**  
 Wählen Sie eine Befehlsdatei in der Liste aus, und ändern Sie die Reihenfolge für den Ladevorgang mithilfe der Pfeiltasten auf der rechten Seite. Die Befehlsdateien werden der Reihe nach beginnend mit der obersten in der Liste geladen.  
  
 **Add (Hinzufügen)**  
 Klicken Sie auf diese Option, um eine Befehlsdatei mithilfe des Dialogfelds **Öffnen** hinzuzufügen.  
  
 **Remove**  
 Wählen Sie im Textfeld eine Befehlsdatei aus, und entfernen Sie sie anschließend mithilfe der Schaltfläche **Entfernen** .  
  
 **Ausführen**  
 Klicken Sie hier, um das Paket auszuführen.  
  
 **Schließen**  
 Klicken Sie hier, um das Dialogfeld **Paketausführungshilfsprogramm** zu schließen.  
  
## <a name="connection-managers-page"></a>Seite "Verbindungs-Manager"  
 Auf der Seite **Verbindungs-Manager** des Dialogfelds **Paketausführungshilfsprogramm** können Sie die Verbindungszeichenfolgen der vom Paket verwendeten Verbindungs-Manager bearbeiten.  
  
### <a name="options"></a>Tastatur  
 **Verbindungs-Manager**  
 Aktivieren Sie dieses Kontrollkästchen, um die **Verbindungszeichenfolge** -Spalte bearbeiten zu können.  
  
 **Beschreibung**  
 Zeigt eine Beschreibung für jeden Verbindungs-Manager an. Die Beschreibungen können nicht bearbeitet werden.  
  
 **Verbindungszeichenfolge**  
 Bearbeiten Sie die Verbindungszeichenfolge für einen Verbindungs-Manager. Dieses Feld kann nur bearbeitet werden, wenn das Kontrollkästchen **Verbindungs-Manager** aktiviert ist.  
  
 **Ausführen**  
 Klicken Sie hier, um das Paket auszuführen.  
  
 **Schließen**  
 Klicken Sie hier, um das Dialogfeld **Paketausführungshilfsprogramm** zu schließen.  
  
## <a name="execution-options-page"></a>Seite "Ausführungsoptionen"  
 Auf der Seite **Ausführungsoptionen** des Dialogfelds **Paketausführungshilfsprogramm** können Sie Laufzeitoptionen für das Paket angeben.  
  
### <a name="options"></a>Tastatur  
 **Paket bei Überprüfungswarnungen fehlschlagen lassen**  
 Gibt an, ob ein Paket fehlschlägt, wenn eine Überprüfungswarnung auftritt.  
  
 **Paket ohne Ausführung überprüfen**  
 Gibt an, ob das Paket nur überprüft wird.  
  
 **Maximale Anzahl von gleichzeitig ausführbaren Dateien**  
 Gibt an, ob Sie die maximale Anzahl der ausführbaren Dateien angeben möchten, die gleichzeitig im Paket ausgeführt werden können. Nachdem Sie dieses Kontrollkästchen aktiviert haben, verwenden Sie das Drehfeld, um die maximale Anzahl von ausführbaren Dateien anzugeben.  
  
 **Paketprüfpunkte aktivieren**  
 Gibt an, ob Paketprüfpunkte aktiviert sind.  
  
 **Prüfpunktdatei**  
 Führt die vom Paket verwendete Prüfpunktdatei auf, wenn die Paketprüfpunkte aktiviert sind.  
  
 **Durchsuchen**  
 Klicken Sie auf die Schaltfläche zum Durchsuchen **(…)**, um die Prüfpunktdatei mithilfe des Dialogfelds **Öffnen** zu suchen, wenn Paketprüfpunkte aktiviert sind. Wenn bereits eine Prüfpunktdatei angegeben ist, wird diese durch die ausgewählte Datei ersetzt.  
  
 **Neustartoptionen überschreiben**  
 Gibt an, ob die Neustartoptionen überschrieben werden, wenn Sie die Paketprüfpunkte aktivieren.  
  
 **Neustartoption**  
 Wählen Sie aus, wie die Prüfpunkte verwendet werden sollen, wenn die Neustartoptionen überschrieben werden.  
  
 **Ausführen**  
 Klicken Sie hier, um das Paket auszuführen.  
  
 **Schließen**  
 Klicken Sie hier, um das Dialogfeld **Paketausführungshilfsprogramm** zu schließen.  
  
## <a name="reporting-page"></a>Berichtsseite  
 Auf der Seite **Berichterstellung** des Dialogfelds **Paketausführungsprogramm** können Sie angeben, welche Ereignisse und Informationen zu dem Paket beim Ausführen des Pakets an der Konsole protokolliert werden sollen.  
  
### <a name="options"></a>Tastatur  
 **Konsolenereignisse**  
 Geben Sie die Ereignisse und Meldungstypen an, die gemeldet werden sollen.  
  
 **None**  
 Wählen Sie diese Option aus, um keinerlei Informationen zu melden.  
  
 **Fehler**  
 Wählen Sie diese Option aus, um Fehlermeldungen zu melden.  
  
 **Warnungen**  
 Wählen Sie diese Option aus, um Warnungen zu melden.  
  
 **Benutzerdefinierte Ereignisse**  
 Wählen Sie diese Option aus, um Meldungen zu benutzerdefinierten Ereignissen zu melden.  
  
 **Pipelineereignisse**  
 Wählen Sie diese Option aus, um Meldungen zu Datenflussereignissen zu melden.  
  
 **Information**  
 Wählen Sie diese Option aus, um Informationsmeldungen zu melden.  
  
 **Ausführlich**  
 Wählen Sie diese Option aus, um die ausführliche Berichterstellung zu verwenden.  
  
 **Konsolenprotokollierung**  
 Geben Sie die Informationen ein, die in das Protokoll geschrieben werden sollen, wenn das ausgewählte Ereignis eintritt.  
  
 **Name**  
 Wählen Sie diese Option aus, wenn der Name der Person gemeldet werden soll, die das Paket erstellt hat.  
  
 **Computer**  
 Wählen Sie diese Option aus, um den Namen des Computers zu melden, auf dem das Paket ausgeführt wird.  
  
 **Operator**  
 Wählen Sie diese Option aus, wenn der Name der Person gemeldet werden soll, die das Paket gestartet hat.  
  
 **Quellname**  
 Wählen Sie diese Option aus, um den Paketnamen zu melden.  
  
 **Quell-GUID**  
 Wählen Sie diese Option aus, um die GUID des Pakets zu melden.  
  
 **Ausführungs-GUID**  
 Wählen Sie diese Option aus, um die GUID der Paketausführungsinstanz zu melden.  
  
 **Meldung**  
 Wählen Sie diese Option aus, um Meldungen zu melden.  
  
 **Startzeit und Beendigungszeit**  
 Wählen Sie diese Option aus, um zu melden, wann das Paket gestartet und beendet wurde.  
  
 **Ausführen**  
 Klicken Sie hier, um das Paket auszuführen.  
  
 **Schließen**  
 Klicken Sie hier, um das Dialogfeld **Paketausführungshilfsprogramm** zu schließen.  
  
## <a name="logging-page"></a>Protokollierungsseite  
 Auf der Seite **Protokollierung** des Dialogfelds **Paketausführungsprogramm** können Sie festlegen, welche Protokollanbieter dem Paket zur Laufzeit zur Verfügung stehen sollen. Stellen Sie den Typ des Paketprotokollanbieters und die Verbindungszeichenfolge für die Verbindung mit dem Protokoll bereit. Für jeden Protokollanbietereintrag wird der Eingabeaufforderung eine Instanz der Option **/LOGGER**_classid_ hinzugefügt.  
  
### <a name="options"></a>Tastatur  
 **Protokollanbieter**  
 Wählen Sie einen Protokollanbieter aus der Liste aus.  
  
 **Konfigurationszeichenfolge**  
 Wählen Sie den Namen des Verbindungs-Managers aus dem Paket aus, das auf den Speicherort des Protokolls zeigt, oder geben Sie die Verbindungszeichenfolge für die Verbindung mit dem Protokollanbieter ein.  
  
 **Remove**  
 Wählen Sie einen Protokollanbieter aus, und klicken Sie auf ihn, um ihn zu entfernen.  
  
 **Ausführen**  
 Klicken Sie hier, um das Paket auszuführen.  
  
 **Schließen**  
 Klicken Sie hier, um das Dialogfeld **Paketausführungshilfsprogramm** zu schließen.  
  
## <a name="set-values-page"></a>Seite "Werte festlegen"  
 Auf der Seite **Werte festlegen** des Dialogfelds **Paketausführungsprogramm** können Sie die Eigenschaftenwerte von Paketen, ausführbaren Dateien, Verbindungen, Variablen und Protokollanbietern festlegen, indem Sie die Pfade der Eigenschaften und die Eigenschaftenwerte eingeben. Für jeden Pfadeintrag wird der Eingabeaufforderung eine Instanz der Option **/SET**_propertypath;value_ hinzugefügt.  
  
### <a name="options"></a>Tastatur  
 **Eigenschaftspfad**  
 Geben Sie den Pfad der Eigenschaft ein. In der Pfadsyntax wird der umgekehrte Schrägstrich (\\) verwendet, um zu kennzeichnen, dass es sich beim nächsten Element um einen Container handelt, durch einen Punkt (.) wird gekennzeichnet, dass das folgende Element eine Eigenschaft ist, und Klammern kennzeichnen ein Sammelelement. Das Element kann anhand seines Index oder Namens identifiziert werden. Der Eigenschaftspfad einer Paketvariablen lautet beispielsweise \Package.Variables[MyVariable].Value.  
  
 **Wert**  
 Geben Sie den Wert der Eigenschaft ein.  
  
 **Remove**  
 Wählen Sie einen Eigenschaftspfad aus, und klicken Sie darauf, um ihn zu entfernen.  
  
 **Ausführen**  
 Klicken Sie hier, um das Paket auszuführen.  
  
 **Schließen**  
 Klicken Sie hier, um das Dialogfeld **Paketausführungshilfsprogramm** zu schließen.  
  
## <a name="verification-page"></a>Überprüfungsseite  
 Auf der Seite **Überprüfung** des Dialogfelds **Paket ausführen** können Sie Kriterien zum Überprüfen des Pakets festlegen.  
  
### <a name="options"></a>Tastatur  
 **Nur signierte Pakete ausführen**  
 Wählen Sie diese Option aus, um nur signierte Pakete auszuführen.  
  
 **Paketbuild überprüfen**  
 Wählen Sie diese Option aus, um das Paketbuild zu überprüfen.  
  
 Build  
 Geben Sie die dem Build zugeordnete fortlaufende Buildnummer an.  
  
 **Paket-ID überprüfen**  
 Wählen Sie diese Option aus, um die Paket-ID zu überprüfen.  
  
 Paket-ID  
 Geben Sie die Paket-ID an.  
  
 **Versions-ID überprüfen**  
 Wählen Sie diese Option aus, um die Versions-ID zu überprüfen.  
  
 Versions-ID  
 Geben Sie die Versions-ID an.  
  
 **Ausführen**  
 Klicken Sie hier, um das Paket auszuführen.  
  
 **Schließen**  
 Klicken Sie hier, um das Dialogfeld **Paketausführungshilfsprogramm** zu schließen.  
  
## <a name="command-line-page"></a>Befehlszeilenseite  
 Auf der Seite **Befehlszeile** des Dialogfelds **Paketausführungsprogramm** können Sie die Befehlszeile bearbeiten, die von Optionen generiert wurde, die in den verschiedenen Dialogfeldern erstellt wurden.  
  
### <a name="options"></a>Tastatur  
 **Die ursprünglichen Optionen wiederherstellen**  
 Klicken Sie hier, um den ursprünglichen Status der Befehlszeile wiederherzustellen. Verwenden Sie diese Option, wenn Sie mithilfe der Option **Befehlszeile manuell bearbeiten** Änderungen vorgenommen haben und nun die ursprünglichen Optionen der Befehlszeile wiederherstellen möchten.  
  
 **Befehlszeile manuell bearbeiten**  
 Klicken Sie hier, um die Befehlszeile im Textfeld **Befehlszeile** zu bearbeiten.  
  
 **Befehlszeile**  
 Zeigt die aktuelle Befehlszeile an. Die Befehlszeile kann nur bearbeitet werden, wenn Sie die Option zum manuellen Bearbeiten der Befehlszeile aktiviert haben.  
  
 **Ausführen**  
 Klicken Sie hier, um das Paket auszuführen.  
  
 **Schließen**  
 Klicken Sie hier, um das Dialogfeld **Paketausführungshilfsprogramm** zu schließen.  
  
## <a name="see-also"></a>Siehe auch  
 [dtexec (Hilfsprogramm)](../../integration-services/packages/dtexec-utility.md)  
  
  
