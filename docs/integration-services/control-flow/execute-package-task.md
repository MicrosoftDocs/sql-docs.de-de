---
description: Paket ausführen (Task)
title: Paket ausführen (Task) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.executepackagetask.f1
- sql13.dts.designer.executepackagetask.package.f1
- sql13.dts.designer.executepackagetask.parameter.F1
- sql13.dts.designer.executepackagetask.general.f1
helpviewer_keywords:
- Execution Package task [Integration Services]
- child packages
- parent packages [Integration Services]
ms.assetid: 042d4ec0-0668-401c-bb3a-a25fe2602eac
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8251f2ff30fafff584b4f56fe93a9b7423ea412b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352082"
---
# <a name="execute-package-task"></a>Paket ausführen (Task)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Der Task Paket ausführen erweitert die Unternehmensfunktionen von [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , indem Paketen das Ausführen anderer Pakete als Teil eines Workflows ermöglicht wird.  
  
 Der Task Paket ausführen kann für folgende Zwecke verwendet werden:  
  
-   Unterteilen eines komplexen Paketworkflows. Mit diesem Task können Sie Workflow in mehrere Pakete unterteilen, die einfacher zu lesen, zu testen und zu warten sind. Wenn Sie z. B. Daten in ein Sternschema laden, können Sie ein separates Paket erstellen, um jede Dimension und die Faktentabelle aufzufüllen.  
  
-   Wiederverwenden von Paketteilen. Andere Pakete können Teile eines Paketworkflows wiederverwenden. Sie können z. B. ein Modul zum Extrahieren von Daten erstellen, das von verschiedenen Paketen aus aufgerufen werden kann. Jedes Paket, das das Modul zum Extrahieren aufruft, kann verschiedene Datenbereinigungs-, Filter- oder Aggregationsvorgänge ausführen.  
  
-   Gruppieren von Arbeitseinheiten. Arbeitseinheiten können in separaten Paketen gekapselt und als Transaktionskomponenten mit dem Workflow eines übergeordneten Pakets verknüpft werden. Beispielsweise führt das übergeordnete Paket die zusätzlichen Pakete aus und führt basierend auf dem Erfolg oder dem Fehlschlagen der zusätzlichen Pakete, einen Commit oder ein Rollback der Transaktion aus.  
  
-   Steuern der Paketsicherheit. Paketersteller benötigen Zugriff auf nur einen Teil einer Multipaketlösung. Das Aufteilen eines Pakets in mehrere Pakete stellt mehr Sicherheit bereit, weil Sie einem Ersteller Zugriff nur auf die relevanten Pakete erteilen können.  
  
 Ein Paket, das andere Pakete ausführt, wird im Allgemeinen als übergeordnetes Paket bezeichnet, und die Pakete, die von einem übergeordneten Workflow ausgeführt werden, werden als untergeordnete Pakete bezeichnet.  
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] enthält Tasks, die Workflowvorgänge ausführen, z. B. das Ausführen von ausführbaren Dateien und Batchdateien. Weitere Informationen finden Sie unter [Execute Process Task](../../integration-services/control-flow/execute-process-task.md).  
  
## <a name="running-packages"></a>Ausführen von Paketen  
 Der Task "Paket ausführen" kann untergeordnete Pakete ausführen, die im gleichen Projekt enthalten sind, das auch das übergeordnete Paket enthält. Sie wählen ein untergeordnetes Paket vom Projekt aus, indem Sie die **ReferenceType** -Eigenschaft auf **Projektverweis** und dann die **PackageNameFromProjectReference** -Eigenschaft entsprechend festlegen.  
  
> [!NOTE]  
>  Die Option **ReferenceType** ist schreibgeschützt und auf **Externer Verweis** festgelegt, wenn das Projekt, das das Paket enthält, nicht in das Projektbereitstellungsmodell konvertiert wurde. [Stellen Sie SQL Server Integration Services-Projekte und Pakete (SSIS) bereit](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md).  
  
 Mit dem Task „Paket ausführen“ können Pakete ausgeführt werden, die in der msdb-Datenbank von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gespeichert sind, sowie im Dateisystem gespeicherte Pakete. Der Task stellt mithilfe eines OLE DB-Verbindungs-Managers eine Verbindung mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder mit einem Dateiverbindungs-Manager her, um auf das Dateisystem zuzugreifen. Weitere Informationen finden Sie unter [OLE DB Connection Manager](../../integration-services/connection-manager/ole-db-connection-manager.md) und [Flat File Connection Manager](../../integration-services/connection-manager/flat-file-connection-manager.md).  
  
 Mit dem Task Paket ausführen kann auch ein Datenbankwartungsplan ausgeführt werden. Auf diese Weise können [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Pakete und Datenbankwartungspläne in derselben [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Lösung verwaltet werden. Ein Datenbankwartungsplan ist mit einem [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Paket zu vergleichen. Ein Plan kann jedoch nur Datenbankwartungstasks enthalten und wird immer in der msdb-Datenbank gespeichert.  
  
 Wenn Sie ein im Dateisystem gespeichertes Paket auswählen, müssen Sie den Namen und den Speicherort des Pakets angeben. Das Paket kann überall im Dateisystem gespeichert sein. Es muss sich nicht in demselben Ordner wie das übergeordnete Paket befinden.  
  
 Das untergeordnete Paket kann im Prozess des übergeordneten Pakets oder als eigener Prozess ausgeführt werden. Das Ausführen des untergeordneten Pakets als eigener Prozess erfordert mehr Arbeitsspeicher, bietet allerdings mehr Flexibilität. Beispielsweise kann bei einem Fehler des untergeordneten Prozesses der übergeordnete Prozess weiterhin ausgeführt werden.  
  
 Alternativ kann es vorkommen, dass in bestimmten Situationen das übergeordnete und das untergeordnete Paket gemeinsam einen Fehler erzeugen sollen, oder Sie möchten den zusätzlichen Verarbeitungsaufwand eines anderen Prozesses übernehmen. Wenn z. B. bei einem untergeordneten Prozess ein Fehler auftritt und die nachfolgende Verarbeitung im übergeordneten Prozess des Pakets vom Erfolg des untergeordneten Prozesses abhängt, sollte das untergeordnete Paket im Prozess des übergeordneten Pakets ausgeführt werden.  
  
 Die ExecuteOutOfProcess-Eigenschaft des Tasks „Paket ausführen“ ist standardmäßig auf **FALSE** festgelegt, und das untergeordnete Paket wird im selben Prozess wie das übergeordnete Paket ausgeführt. Wenn Sie diese Eigenschaft auf **True** festlegen, wird das untergeordnete Paket in einem separaten Prozess ausgeführt. Dadurch kann sich der Start des untergeordneten Pakets verlangsamen. Wenn Sie die Eigenschaft auf **TRUE** festlegen, ist außerdem das Debuggen des Pakets in einer Installation, die nur die Tools enthält, nicht möglich. Sie müssen [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]installieren. Weitere Informationen finden Sie unter [Installieren von Integration Services](../../integration-services/install-windows/install-integration-services.md).  
  
## <a name="extending-transactions"></a>Erweitern von Transaktionen  
 Die vom übergeordneten Paket verwendete Transaktion kann auf das untergeordnete Paket erweitert werden. Deshalb kann für die von beiden Paketen ausgeführte Arbeit ein Commit oder Rollback ausgeführt werden. Beispielsweise kann für Datenbankeinfügungen, die vom übergeordneten Paket ausgeführt werden, ein Commit oder Rollback ausgeführt werden, und zwar in Abhängigkeit von den vom untergeordneten Paket ausgeführten Datenbankeinfügungen und umgekehrt. Weitere Informationen finden Sie unter [Inherited Transactions](../integration-services-transactions.md).  
  
## <a name="propagating-logging-details"></a>Weitergeben von Protokollierungsdetails  
 Für das untergeordnete Paket, das der Task Paket ausführen ausführt, ist möglicherweise die Verwendung der Protokollierung konfiguriert. Das untergeordnete Paket leitet jedoch die Protokolldetails stets an das übergeordnete Paket weiter. Falls für den Task Paket ausführen die Verwendung der Protokollierung konfiguriert ist, protokolliert der Task die Protokolldetails des untergeordneten Pakets. Weitere Informationen finden Sie unter [Integration Services-Protokollierung &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
## <a name="passing-values-to-child-packages"></a>Übergeben von Werten an untergeordnete Pakete  
 Häufig verwendet ein untergeordnetes Paket Werte, die von einem anderen Paket übergeben werden, das das untergeordnete Paket aufruft. Normalerweise handelt es sich dabei um das übergeordnete Paket. Das Verwenden von Werten eines übergeordneten Pakets ist in folgenden Szenarien hilfreich:  
  
-   Teile eines größeren Workflows sind verschiedenen Paketen zugewiesen. Beispielsweise könnte ein Paket jede Nacht Daten herunterladen, die Daten zusammenfassen, Zusammenfassungsdatenwerte Variablen zuweisen und dann die Werte an ein anderes Paket zum weiteren Verarbeiten der Daten übergeben.  
  
-   Das übergeordnete Paket koordiniert Tasks in einem untergeordneten Paket dynamisch. Beispielsweise könnte das übergeordnete Paket die Anzahl von Tagen im aktuellen Monat bestimmen und diesen Wert einer Variablen zuweisen. Das untergeordnete Paket führt dann einen Task entsprechend oft aus.  
  
-   Ein untergeordnetes Paket benötigt Zugriff auf Daten, die vom übergeordneten Paket dynamisch abgeleitet werden. Beispielsweise könnte das übergeordnete Paket Daten aus einer Tabelle extrahieren und das Rowset in eine Variable laden, und das untergeordnete Paket könnte zusätzliche Vorgänge für die Daten ausführen.  
  
 Sie mithilfe der folgenden Methoden Werte an ein untergeordnetes Paket übergeben:  
  
-   **Paketkonfigurationen**  
  
     [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] bietet einen Konfigurationstyp, die Variablenkonfiguration für übergeordnete Pakete, mit dem Werte von übergeordneten an untergeordnete Pakete übergeben werden können. Diese Konfiguration basiert auf dem untergeordneten Paket und verwendet eine Variable im untergeordneten Paket. Die Konfiguration wird einer Variablen im untergeordneten Paket zugeordnet oder der Eigenschaft eines Objekts im untergeordneten Paket. Die Variable kann auch in den Skripts verwendet werden, die vom Skripttask oder der Skriptkomponente verwendet werden.  
  
-   **Parameter**  
  
     Sie können den Task "Paket ausführen" konfigurieren, um Variablen für übergeordnete Pakete oder Parameter untergeordneten Paketparametern zuzuordnen. Das Projekt muss das Projektbereitstellungsmodell verwenden, und das untergeordnete Paket muss im gleichen Projekt enthalten sein wie das übergeordnete Paket. Weitere Informationen finden Sie unter [Execute Package Task Editor]().  
  
    > [!NOTE]  
    >  Wenn der untergeordnete Paketparameter nicht vertraulich ist und einem übergeordneten Parameter zugeordnet wird, der vertraulich ist, wird das untergeordnete Paket nicht ausgeführt.  
    >   
    >  Die folgenden Zuordnungsbedingungen unterstützt.  
    >   
    >  Vertraulich, ein untergeordneten Paketparameter wird einem vertraulichen übergeordneten Parameter zugeordnet  
    >   
    >  Vertraulich, ein untergeordneten Paketparameter wird einem nicht vertraulichen übergeordneten Parameter zugeordnet  
    >   
    >  Nicht vertraulich, ein untergeordneten Paketparameter wird einem nicht vertraulichen übergeordneten Parameter zugeordnet  
  
 Die Variable für das untergeordnete Paket kann im Bereich des Tasks "Paket ausführen" oder in einem übergeordneten Container wie dem Paket definiert werden. Falls mehrere gleichnamige Variablen vorhanden sind, wird die im Bereich des Tasks Paket ausführen definierte Variable verwendet oder die Variable, die im Bereich dem Task am nächsten liegt.  
  
 Weitere Informationen finden Sie unter [Verwenden der Werte von Variablen und Parametern in einem untergeordneten Paket](../../integration-services/packages/legacy-package-deployment-ssis.md#child).  
  
### <a name="accessing-parent-package-variables"></a>Zugriff auf Variablen für übergeordnete Pakete  
 Untergeordnete Pakete greifen über den Skripttask auf Variablen für übergeordnete Pakete zu. Wenn Sie im **Skripttask-Editor** auf der Seite **Skript** den Namen der Variablen für das übergeordnete Paket eingeben, lassen Sie **Benutzer:** im Variablennamen aus. Andernfalls wird die Variable beim Ausführen des übergeordneten Pakets vom untergeordneten Paket nicht gesucht.  
  
## <a name="configuring-the-execute-package-task"></a>Konfigurieren des Tasks Paket ausführen  
 Sie können Eigenschaften mit dem [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer oder programmgesteuert festlegen.  
  
 Klicken Sie auf das folgende Thema, um weitere Informationen zu den Eigenschaften zu erhalten, die Sie im [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer festlegen können:  
  
-   [Seite Ausdrücke](../../integration-services/expressions/expressions-page.md)  
  
 Klicken Sie auf das folgende Thema, um weitere Informationen zum Festlegen dieser Eigenschaften im [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer zu erhalten:  
  
-   [Festlegen der Eigenschaften eines Tasks oder Containers](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
## <a name="configuring-the-execute-package-task-programmatically"></a>Programmgesteuertes Konfigurieren des Tasks Paket ausführen  
 Klicken Sie auf das folgende Thema, um weitere Informationen zum programmgesteuerten Festlegen dieser Eigenschaften anzuzeigen:  
  
-   [N:Microsoft.SqlServer.Dts.Tasks.ExecutePackageTask](https://technet.microsoft.com/library/microsoft.sqlserver.dts.tasks.executepackagetask\(v=sql.110\).aspx)  
  
## <a name="execute-package-task-editor"></a>Editor für den Task 'Paket ausführen'
  Mit dem Editor für den Task "Paket ausführen" können Sie Task "Paket ausführen" konfigurieren. Der Task Paket ausführen erweitert die Unternehmensfunktionen von [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , indem Paketen das Ausführen anderer Pakete als Teil eines Workflows ermöglicht wird.  
  
 **Was möchten Sie tun?**  
  
-   [Öffnen des Editors für den Task "Paket ausführen"](#open)  
  
-   [Festlegen der Optionen auf der Seite „Allgemein“](#general)  
  
-   [Festlegen der Optionen auf der Seite "Paket"](#package)  
  
-   [Festlegen der Optionen auf der Seite "Parameterbindungen"](#parameter)  
  
###  <a name="open-the-execute-package-task-editor"></a><a name="open"></a> Öffnen des Editors für den Task "Paket ausführen"  
  
1.  Öffnen Sie in [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] ein [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] -Projekt, das einen Task „Paket ausführen“ enthält.  
  
2.  Klicken Sie im SSIS-Designer mit der rechten Maustaste auf den Task, und klicken Sie dann auf **Bearbeiten**.  
  
###  <a name="set-the-options-on-the-general-page"></a><a name="general"></a> Festlegen der Optionen auf der Seite „Allgemein“  
 **Name**  
 Geben Sie einen eindeutigen Namen für den Task "Paket ausführen" an. Dieser Name wird im Tasksymbol als Bezeichnung verwendet.  
  
> [!NOTE]  
>  Tasknamen müssen innerhalb eines Pakets eindeutig sein.  
  
 **Beschreibung**  
 Geben Sie eine Beschreibung des Tasks "Paket ausführen" ein.  
  
###  <a name="set-the-options-on-the-package-page"></a><a name="package"></a> Festlegen der Optionen auf der Seite "Paket"  
 **ReferenceType**  
 Wählen Sie **Projektverweis** für untergeordnete Pakete im Projekt aus. Wählen Sie **Externer Verweis** für untergeordnete Pakete aus, die sich außerhalb des Pakets befinden.  
  
> [!NOTE]  
>  Die Option **ReferenceType** ist schreibgeschützt und auf **Externer Verweis** festgelegt, wenn das Projekt, das das Paket enthält, nicht in das Projektbereitstellungsmodell konvertiert wurde. [Stellen Sie SQL Server Integration Services-Projekte und Pakete (SSIS) bereit](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md).  
  
 **Kennwort**  
 Wenn das untergeordnete Paket kennwortgeschützt ist, stellen Sie das Kennwort für das untergeordnete Paket bereit, oder klicken Sie auf die Schaltfläche mit den drei Auslassungspunkten (...), und erstellen Sie ein neues Kennwort für das untergeordnete Paket.  
  
 **ExecuteOutOfProcess**  
 Geben Sie an, ob das untergeordnete Paket im Prozess des übergeordneten Pakets oder in einem separaten Prozess ausgeführt wird. Die ExecuteOutOfProcess-Eigenschaft des Tasks „Paket ausführen“ ist standardmäßig auf **FALSE** festgelegt, und das untergeordnete Paket wird im selben Prozess wie das übergeordnete Paket ausgeführt. Wenn Sie diese Eigenschaft auf **TRUE** festlegen, wird das untergeordnete Paket in einem separaten Prozess ausgeführt. Dadurch kann sich der Start des untergeordneten Pakets verlangsamen. Wenn die Eigenschaft auf **TRUE** festgelegt wurde, ist außerdem das Debuggen des Pakets in einer Installation, die nur die Tools enthält, nicht möglich. Das [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Produkt muss von Ihnen installiert werden. Weitere Informationen finden Sie unter [Installieren von Integration Services](../../integration-services/install-windows/install-integration-services.md).  
  
#### <a name="referencetype-dynamic-options"></a>Dynamische Optionen für ReferenceType  
  
##### <a name="referencetype--external-reference"></a>ReferenceType = Externer Verweis  
 **Location**  
 Wählen Sie den Speicherort des untergeordneten Pakets aus. Diese Eigenschaft besitzt die in der folgenden Tabelle aufgeführten Optionen.  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**SQL Server**|Legt den Speicherort als Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]fest.|  
|**Dateisystem**|Legen Sie als Speicherort das Dateisystem fest.|  
  
 **Connection**  
 Wählen Sie den Typ des Speicherorts für das untergeordnete Paket aus.  
  
 **PackageNameReadOnly**  
 Zeigt den Paketnamen an.  
  
##### <a name="referencetype--project-reference"></a>ReferenceType = Projektverweis  
 **PackageNameFromProjectReference**  
 Wählen Sie ein im Projekt enthaltenes Paket als untergeordnetes Paket aus.  
  
#### <a name="location-dynamic-options"></a>Dynamische Optionen für Location  
  
##### <a name="location--sql-server"></a>Location = SQL Server  
 **Connection**  
 Wählen Sie einen OLE DB-Verbindungs-Manager aus der Liste aus, oder klicken Sie auf \<**New connection...**>, um einen neuen Verbindungs-Manager zu erstellen.  
  
 **Verwandte Themen:** [OLE DB-Verbindungs-Manager](../../integration-services/connection-manager/ole-db-connection-manager.md)  
  
 **PackageName**  
 Geben Sie den Namen des untergeordneten Pakets an, oder klicken Sie auf die Schaltfläche mit den drei Auslassungspunkten (...), um nach dem Paket zu suchen.  
  
##### <a name="location--file-system"></a>Location = File system  
 **Connection**  
 Wählen Sie einen Dateiverbindungs-Manager aus der Liste aus, oder klicken Sie auf \<**New connection...**>, um einen neuen Verbindungs-Manager zu erstellen.  
  
 **Verwandte Themen:** [Dateiverbindungs-Manager](../../integration-services/connection-manager/file-connection-manager.md)  
  
 **PackageNameReadOnly**  
 Zeigt den Paketnamen an.  
  
###  <a name="set-the-options-on-the-parameter-bindings-page"></a><a name="parameter"></a> Festlegen der Optionen auf der Seite "Parameterbindungen"  
 Sie können Werte aus dem übergeordneten Paket oder dem Projekt an das untergeordnete Paket übergeben. Das Projekt muss das Projektbereitstellungsmodell verwenden, und das untergeordnete Paket muss im gleichen Projekt enthalten sein wie das übergeordnete Paket.  
  
 Weitere Informationen zum Konvertieren eines Projekts in das Projektbereitstellungsmodell finden Sie unter [Deploy Integration Services (SSIS) Projects and Packages](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md) (Bereitstellen von SQL Server Integration Services-Projekten und -Paketen [SSIS]).  
  
 **Untergeordneter Paketparameter**  
 Geben Sie einen Namen für den untergeordneten Paketparameter ein, oder wählen Sie diesen aus.  
  
 **Bindungsparameter oder Variable**  
 Wählen Sie den Parameter oder die Variable mit dem Wert aus, den Sie an das untergeordnete Paket übergeben möchten.  
  
 **Add (Hinzufügen)**  
 Klicken Sie, um einen Parameter oder eine Variable einem untergeordneten Paketparameter zuzuordnen.  
  
 **Remove**  
 Klicken Sie, um eine Zuordnung zwischen einem Parameter oder einer Variable und einem untergeordneten Paketparameter zu entfernen.  
  
