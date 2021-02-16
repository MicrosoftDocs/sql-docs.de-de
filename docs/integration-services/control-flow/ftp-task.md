---
description: FTP-Task
title: FTP-Task | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.ftptask.f1
- sql13.dts.designer.ftptask.general.f1
- sql13.dts.designer.ftptask.filetransfer.f1
helpviewer_keywords:
- FTP task [Integration Services]
ms.assetid: 41c3f2c4-ee04-460a-9822-bb9ae4036c2e
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 839f9cc803dfcfe6c753851e53f0db1f2ef14daf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348964"
---
# <a name="ftp-task"></a>FTP-Task

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Mit dem FTP-Task werden Datendateien heruntergeladen und hochgeladen sowie Verzeichnisse auf Servern verwaltet. Beispielsweise kann ein Paket Datendateien von einem Remoteserver oder einem Internetstandort als Teil eines Paket-Workflows von [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] herunterladen. Der FTP-Task kann für folgende Zwecke verwendet werden:  
  
-   Kopieren von Verzeichnissen und Datendateien zwischen Verzeichnissen, vor oder nach dem Verschieben von Daten, sowie Anwenden von Transformationen auf die Daten.  
  
-   Anmelden bei einer FTP-Quelladresse und Kopieren von Dateien oder Paketen in ein Zielverzeichnis.  
  
-   Das Herunterladen von Dateien von einer FTP-Adresse und Anwenden von Transformationen auf Spaltendaten, bevor die Daten in eine Datenbank geladen werden.  
  
 Zur Laufzeit stellt der FTP-Task mithilfe eines FTP-Verbindungs-Managers eine Verbindung mit einem Server her. Der FTP-Verbindungs-Manager wird separat vom FTP-Task konfiguriert, und im FTP-Task wird dann darauf verwiesen. Der FTP-Verbindungs-Manager schließt die Servereinstellungen, die Anmeldeinformationen für den Zugriff auf den FTP-Server ein sowie Optionen wie z. B. das Timeout und die Anzahl von Verbindungsversuchen mit dem Server. Weitere Informationen finden Sie unter [FTP-Verbindungs-Manager](../../integration-services/connection-manager/ftp-connection-manager.md).  
  
> [!IMPORTANT]  
>  Der FTP-Verbindungs-Manager unterstützt nur die anonyme Authentifizierung und die Standardauthentifizierung. Er unterstützt keine Windows-Authentifizierung.  
  
 Für den Zugriff auf eine lokale Datei oder ein lokales Verzeichnis verwendet der FTP-Task einen Dateiverbindungs-Manager oder Pfadinformationen, die in einer Variablen gespeichert sind. Dagegen verwendet der FTP-Task für den Zugriff auf eine Remotedatei oder ein Remoteverzeichnis einen direkt eingegebenen Pfad auf dem Remoteserver, der im FTP-Verbindungs-Manager angegeben ist, oder in einer Variablen gespeicherte Pfadinformationen. Weitere Informationen finden Sie unter [File Connection Manager](../../integration-services/connection-manager/file-connection-manager.md) und [Integration Services-Variablen &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md).  
  
 Dies bedeutet, dass mit dem FTP-Task mehrere Dateien empfangen und mehrere Remotedateien gelöscht werden können. Mit diesem Task kann jedoch nur eine Datei gesendet und eine lokale Datei gelöscht werden, falls ein Verbindungs-Manager verwendet wird. Ein Dateiverbindungs-Manager kann nämlich nur auf eine Datei zugreifen. Für den Zugriff auf mehrere lokale Dateien muss der FTP-Task eine Variable zum Bereitstellen der Pfadinformationen verwenden. Beispielsweise stellt eine Variable, die „C:\Test\&#42;.txt“ enthält, einen Pfad bereit, der das Löschen oder Senden aller Dateien mit der Erweiterung TXT im Verzeichnis „Test“ unterstützt.  
  
 Um mehrere Dateien zu senden und auf mehrere lokale Dateien und Verzeichnisse zuzugreifen, können Sie den FTP-Task auch mehrmals ausführen, indem Sie den Task in eine Foreach-Schleife einschließen. Mit der Foreach-Schleife ist die Enumeration von Dateien in einem Verzeichnis mithilfe des Foreach-Dateienumerators möglich. Weitere Informationen finden Sie unter [Foreach-Schleifencontainer](../../integration-services/control-flow/foreach-loop-container.md).  
  
 Der FTP-Task unterstützt die Platzhalterzeichen *?* und *\** in Pfaden. Auf diese Weise kann mit dem Task auf mehrere Dateien zugegriffen werden. Platzhalterzeichen können jedoch nur in dem Teil des Pfades verwendet werden, in dem der Dateiname angegeben ist. Beispielsweise ist „C:\MyDirectory\\*.txt“ ein gültiger Pfad, „C:\\\*\MyText.txt“ hingegen nicht.  
  
 Die FTP-Vorgänge können so konfiguriert werden, dass der Task Dateisystem beendet wird, wenn der Vorgang einen Fehler erzeugt, oder dass Dateien im ASCII-Modus übertragen werden. Die Vorgänge, mit denen Dateien gesendet und empfangen werden, können so konfiguriert werden, dass Zieldateien und -verzeichnisse überschrieben werden.  
  
## <a name="predefined-ftp-operations"></a>Vordefinierte FTP-Vorgänge  
 Der FTP-Task schließt vordefinierte Vorgänge ein. In der folgenden Tabelle werden diese Vorgänge beschrieben.  
  
|Vorgang|BESCHREIBUNG|  
|---------------|-----------------|  
|Dateien senden|Sendet eine Datei vom lokalen Computer an den FTP-Server.|  
|Dateien empfangen|Speichert eine Datei vom FTP-Server auf dem lokalen Computer.|  
|Lokales Verzeichnis erstellen|Erstellt einen Ordner auf dem lokalen Computer.|  
|Remoteverzeichnis erstellen|Erstellt einen Ordner auf dem FTP-Server.|  
|Lokales Verzeichnis entfernen|Löscht einen Ordner auf dem lokalen Computer.|  
|Remoteverzeichnis entfernen|Löscht einen Ordner auf dem FTP-Server.|  
|Lokale Dateien löschen|Löscht eine Datei auf dem lokalen Computer.|  
|Remotedateien löschen|Löscht eine Datei auf dem FTP-Server.|  
  
## <a name="custom-log-entries-available-on-the-ftp-task"></a>Verfügbare benutzerdefinierte Protokolleinträge für den FTP-Task  
 In der folgenden Tabelle werden die benutzerdefinierten Protokolleinträge für den FTP-Task aufgelistet. Weitere Informationen finden Sie unter [Integration Services-Protokollierung &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
|Protokolleintrag|BESCHREIBUNG|  
|---------------|-----------------|  
|**FTPConnectingToServer**|Zeigt an, dass mit dem Task eine Verbindung zum FTP-Server initiiert wurde.|  
|**FTPOperation**|Berichtet den Beginn und Typ des vom Task ausgeführten FTP-Vorgangs.|  
  
## <a name="related-tasks"></a>Related Tasks  
 Sie können Eigenschaften mit dem [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer oder programmgesteuert festlegen.  
  
 Informationen zum Anzeigen dieser Eigenschaften im [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer finden Sie unter [Festlegen der Eigenschaften eines Tasks oder Containers](./add-or-delete-a-task-or-a-container-in-a-control-flow.md).  
  
 Weitere Informationen zum programmgesteuerten Festlegen dieser Eigenschaften finden Sie unter <xref:Microsoft.SqlServer.Dts.Tasks.FtpTask.FtpTask>.  
  
## <a name="ftp-task-editor-general-page"></a>Editor für den FTP-Task (Seite Allgemein)
  Mithilfe der Seite **Allgemein** des Dialogfelds **Editor für den FTP-Task** können Sie den FTP-Verbindungs-Manager angeben, der die Verbindung mit dem FTP-Server herstellt, mit dem der Task kommuniziert. Sie können den FTP-Task außerdem benennen und eine Beschreibung hinzufügen.  
  
### <a name="options"></a>Tastatur  
 **FtpConnection**  
 Wählen Sie einen vorhandenen FTP-Verbindungs-Manager aus, oder klicken Sie auf \<**New connection...**>, um einen Verbindungs-Manager zu erstellen.  
  
> [!IMPORTANT]  
>  Der FTP-Verbindungs-Manager unterstützt nur die anonyme Authentifizierung und die Standardauthentifizierung. Er unterstützt keine Windows-Authentifizierung.  
  
 **Verwandte Themen:** [FTP-Verbindungs-Manager](../../integration-services/connection-manager/ftp-connection-manager.md), [FTP-Verbindungs-Manager-Editor](../connection-manager/ftp-connection-manager.md)  
  
 **StopOnFailure**  
 Geben Sie an, ob der FTP-Task beendet wird, wenn ein FTP-Vorgang fehlschlägt.  
  
 **Name**  
 Geben Sie einen eindeutigen Namen für den FTP-Task an. Dieser Name wird im Tasksymbol als Bezeichnung verwendet.  
  
> [!NOTE]  
>  Tasknamen müssen innerhalb eines Pakets eindeutig sein.  
  
 **Beschreibung**  
 Geben Sie eine Beschreibung des FTP-Tasks ein.  
  
## <a name="ftp-task-editor-file-transfer-page"></a>Editor für den FTP-Task (Seite Dateiübertragung)
  Mithilfe der Seite **Dateiübertragung** des Dialogfelds **Editor für den FTP-Task** können Sie den FTP-Vorgang konfigurieren, der durch den Task ausgeführt wird.  
  
### <a name="options"></a>Tastatur  
 **IsRemotePathVariable**  
 Geben Sie an, ob der Remotepfad in einer Variablen gespeichert ist. Diese Eigenschaft besitzt die in der folgenden Tabelle aufgeführten Optionen.  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Wahr**|Der Zielpfad ist in einer Variablen gespeichert. Wenn Sie diesen Wert auswählen, wird die dynamische Option **RemoteVariable** angezeigt.|  
|**False**|Der Zielpfad wird in einem Dateiverbindungs-Manager angegeben. Wenn Sie diesen Wert auswählen, wird die dynamische Option **RemotePath** angezeigt.|  
  
 **OverwriteFileAtDestination**  
 Geben Sie an, ob eine Datei am Ziel überschrieben werden kann.  
  
 **IsLocalPathVariable**  
 Geben Sie an, ob der lokale Pfad in einer Variablen gespeichert ist. Diese Eigenschaft besitzt die in der folgenden Tabelle aufgeführten Optionen.  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Wahr**|Der Zielpfad ist in einer Variablen gespeichert. Wenn Sie diesen Wert auswählen, wird die dynamische Option **LocalVariable** angezeigt.|  
|**False**|Der Zielpfad wird in einem Dateiverbindungs-Manager angegeben. Wenn Sie diesen Wert auswählen, wird die dynamische Option **LocalPath** angezeigt.|  
  
 **Vorgang**  
 Wählen Sie den auszuführenden FTP-Vorgang aus. Diese Eigenschaft besitzt die in der folgenden Tabelle aufgeführten Optionen.  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Dateien senden**|Senden Sie Dateien. Wenn Sie diesen Wert auswählen, werden die dynamischen Optionen **LocalVariable**, **LocalPathRemoteVariable** und **RemotePath** angezeigt.|  
|**Dateien empfangen**|Empfangen Sie Dateien. Wenn Sie diesen Wert auswählen, werden die dynamischen Optionen **LocalVariable**, **LocalPathRemoteVariable** und **RemotePath** angezeigt.|  
|**Lokales Verzeichnis erstellen**|Erstellen Sie ein lokales Verzeichnis. Wenn Sie diesen Wert auswählen, werden die dynamischen Optionen **LocalVariable** und **LocalPath** angezeigt.|  
|**Remoteverzeichnis erstellen**|Erstellen Sie ein Remoteverzeichnis. Wenn Sie diesen Wert auswählen, werden die dynamischen Optionen **RemoteVariable** und **RemotePath** angezeigt.|  
|**Lokales Verzeichnis entfernen**|Entfernen Sie ein lokales Verzeichnis. Wenn Sie diesen Wert auswählen, werden die dynamischen Optionen **LocalVariable** und **LocalPath** angezeigt.|  
|**Remoteverzeichnis entfernen**|Entfernen Sie ein Remoteverzeichnis. Wenn Sie diesen Wert auswählen, werden die dynamischen Optionen **RemoteVariable** und **RemotePath** angezeigt.|  
|**Lokale Dateien löschen**|Löschen Sie lokale Dateien. Wenn Sie diesen Wert auswählen, werden die dynamischen Optionen **LocalVariable** und **LocalPath** angezeigt.|  
|**Remotedateien löschen**|Löschen Sie Remotedateien. Wenn Sie diesen Wert auswählen, werden die dynamischen Optionen **RemoteVariable** und **RemotePath** angezeigt.|  
  
 **IsTransferASCII**  
 Geben Sie an, ob die auf und von einem Remote-FTP-Server übertragenen Dateien im ASCII-Modus übertragen werden sollen.  
  
### <a name="isremotepathvariable-dynamic-options"></a>IsRemotePathVariable (dynamische Optionen)  
  
#### <a name="isremotepathvariable--true"></a>IsRemotePathVariable = True  
 **RemoteVariable**  
 Wählen Sie eine vorhandene benutzerdefinierte Variable aus, oder klicken Sie auf \<**New variable...**>, um eine benutzerdefinierte Variable zu erstellen.  
  
 **Verwandte Themen:** [Integration Services-Variablen &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), Hinzufügen von Variablen  
  
#### <a name="isremotepathvariable--false"></a>IsRemotePathVariable = False  
 **RemotePath**  
 Wählen Sie einen vorhandenen FTP-Verbindungs-Manager aus, oder klicken Sie auf \<**New connection...**>, um einen Verbindungs-Manager zu erstellen.  
  
 **Verwandte Themen:** [FTP-Verbindungs-Manager](../../integration-services/connection-manager/ftp-connection-manager.md), [FTP-Verbindungs-Manager-Editor](../connection-manager/ftp-connection-manager.md)  
  
### <a name="islocalpathvariable-dynamic-options"></a>IsLocalPathVariable (dynamische Optionen)  
  
#### <a name="islocalpathvariable--true"></a>IsLocalPathVariable = True  
 **LocalVariable**  
 Wählen Sie eine vorhandene benutzerdefinierte Variable aus, oder klicken Sie auf \<**New variable...**>, um eine Variable zu erstellen.  
  
 **Verwandte Themen:** [Integration Services-Variablen &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), Hinzufügen von Variablen  
  
#### <a name="islocalpathvariable--false"></a>IsLocalPathVariable = False  
 **LocalPath**  
 Wählen Sie einen vorhandenen Dateiverbindungs-Manager aus, oder klicken Sie auf \<**New connection...**>, um einen Verbindungs-Manager zu erstellen.  
  
 **Verwandte Themen:** [Verbindungs-Manager für Flatfiles](../../integration-services/connection-manager/flat-file-connection-manager.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Integration Services-Tasks](../../integration-services/control-flow/integration-services-tasks.md)   
 [Ablaufsteuerung](../../integration-services/control-flow/control-flow.md)  
  
