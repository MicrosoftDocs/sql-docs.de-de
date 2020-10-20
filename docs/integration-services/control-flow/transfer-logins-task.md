---
description: Task "Anmeldungen übertragen"
title: Task „Anmeldungen übertragen“ | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.transferloginstask.f1
- sql13.dts.designer.transferloginstask.general.f1
- sql13.dts.designer.transferloginstask.logins.f1
helpviewer_keywords:
- Transfer Logins task [Integration Services]
ms.assetid: 1df60fd6-c019-405d-8155-c330dbac2cc1
author: chugugrace
ms.author: chugu
ms.openlocfilehash: e7aa2c4e59ced60c31467b13a4de154887c84f52
ms.sourcegitcommit: cfa04a73b26312bf18d8f6296891679166e2754d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2020
ms.locfileid: "92195412"
---
# <a name="transfer-logins-task"></a>Task "Anmeldungen übertragen"

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Mit dem Task "Anmeldungen übertragen" wird mindestens eine Anmeldung zwischen den Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]übertragen.  
  
## <a name="transfer-logins-between-instances-of-sql-server"></a>Übertragen von Anmeldungen zwischen den Instanzen von SQL Server  
 Der Task "Anmeldungen übertragen" unterstützt eine Quelle und ein Ziel in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="events"></a>Events  
 Die Task löst ein Informationsereignis aus, das die Anzahl der übertragenen Anmeldungen meldet, sowie ein Warnungsereignis, wenn eine Anmeldung überschrieben wird.  
  
 Die Task "Anmeldungen übertragen" meldet keinen schrittweisen Fortschritt der Anmeldeübertragung; sie meldet nur 0 % und 100 % der Ausführung.  
  
## <a name="execution-value"></a>Ausführungswert  
 Der Ausführungswert, definiert in der **ExecutionValue** -Eigenschaft der Task, gibt die Anzahl der übertragenen Anmeldungen zurück. Indem der **ExecValueVariable** -Eigenschaft des Tasks „Anmeldungen übertragen“ eine benutzerdefinierte Variable zugewiesen wird, können Informationen über die Anmeldeübertragung anderen Objekten im Paket zur Verfügung gestellt werden. Weitere Informationen finden Sie unter [Integration Services-Variablen &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md) und [Verwenden von Variablen in Paketen](../integration-services-ssis-variables.md).  
  
## <a name="log-entries"></a>Protokolleinträge  
 Die Task "Anmeldungen übertragen" enthält die folgenden benutzerdefinierten Protokolleinträge:  
  
-   TransferLoginsTaskStarTransferringObjects    Dieser Protokolleintrag meldet, dass die Übertragung begonnen hat. Der Protokolleintrag enthält die Startzeit.  
  
-   TransferLoginsTaskFinishedTransferringObjects   Dieser Protokolleintrag meldet, dass die Übertragung abgeschlossen ist. Der Protokolleintrag enthält die Beendigungszeit.  
  
 Außerdem meldet ein Protokolleintrag für das **OnInformation** -Ereignis die Anzahl der übertragenen Anmeldungen, und für das **OnWarning** -Ereignis wird ein Protokolleintrag für jede Anmeldung im Ziel geschrieben, die überschrieben wird.  
  
## <a name="security-and-permissions"></a>Sicherheit und Berechtigungen  
 Um nach Anmeldungen auf dem Quellserver zu suchen und Anmeldungen auf dem Zielserver zu erstellen, muss der Benutzer Mitglied der sysadmin-Serverrolle auf beiden Servern sein.  
  
## <a name="configuration-of-the-transfer-logins-task"></a>Konfiguration des Tasks "Anmeldungen übertragen"  
 Die Task "Anmeldungen übertragen" kann so konfiguriert werden, dass alle Anmeldungen, nur die angegebenen Anmeldungen oder alle Anmeldungen, die Zugriff auf bestimmte Datenbanken haben, übertragen werden. Die Anmeldung sa kann nicht übertragen werden. Die Anmeldung „sa“ kann umbenannt werden. Die umbenannte Anmeldung „sa“ kann jedoch ebenfalls nicht übertragen werden.  
  
 Sie können auch angeben, ob der Task die mit den Anmeldungen verknüpften Sicherheits-IDs (Security Identification Numbers, SIDs) kopieren soll. Wenn die Task "Anmeldungen übertragen" zusammen mit der Task "Datenbanken übertragen" verwendet wird, müssen die SIDs in das Ziel kopiert werden. Anderenfalls werden die übertragenen Anmeldungen nicht von der Zieldatenbank erkannt.  
  
 Am Ziel werden die übertragenen Anmeldungen deaktiviert, und ihnen werden zufällige Kennwörter zugewiesen. Ein Mitglied der Rolle sysadmin am Zielserver muss die Kennwörter ändern und die Anmeldungen aktivieren, bevor die Anmeldungen verwendet werden können.  
  
 Die zu übertragenden Anmeldungen sind eventuell bereits am Ziel vorhanden. Die Task "Anmeldungen übertragen" kann zur Verarbeitung bereits vorhandener Anmeldungen auf folgende Art und Weise konfiguriert werden:  
  
-   Bereits vorhandene Anmeldungen werden überschrieben.  
  
-   Task schlägt fehl, wenn doppelte Anmeldungen vorhanden sind.  
  
-   Doppelte Anmeldungen werden übersprungen.  
  
 Zur Laufzeit stellt die Task "Anmeldungen übertragen" eine Verbindung mit den Quell- und Zielservern her. Dazu werden die SMO-Verbindungs-Manager verwendet. Die SMO-Verbindungs-Manager werden getrennt von der Task "Anmeldungen übertragen" konfiguriert. Darauf wird dann in der Task "Anmeldungen übertragen" verwiesen. Die SMO-Verbindungs-Manager geben den Server- und Authentifizierungsmodus an, der beim Zugriff auf den Server verwendet werden soll. Weitere Informationen finden Sie unter [SMO Connection Manager](../../integration-services/connection-manager/smo-connection-manager.md).  
  
 Sie können Eigenschaften mit dem [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer oder programmgesteuert festlegen.  
  
 Klicken Sie auf das folgende Thema, um weitere Informationen zu den Eigenschaften zu erhalten, die Sie im [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer festlegen können:  
  
-   [Seite Ausdrücke](../../integration-services/expressions/expressions-page.md)  
  
 Klicken Sie auf das folgende Thema, um weitere Informationen zum Festlegen dieser Eigenschaften im [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer zu erhalten:  
  
-   [Festlegen der Eigenschaften eines Tasks oder Containers](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
## <a name="programmatic-configuration-of-the-transfer-logins-task"></a>Programmgesteuerte Konfiguration des Tasks "Anmeldungen übertragen"  
 Klicken Sie auf das folgende Thema, um weitere Informationen zum programmgesteuerten Festlegen dieser Eigenschaften anzuzeigen:  
  
-   <xref:Microsoft.SqlServer.Dts.Tasks.TransferLoginsTask.TransferLoginsTask>  
  
## <a name="transfer-logins-task-editor-general-page"></a>Editor für den Task Anmeldungen übertragen (Seite Allgemein)
  Mithilfe der Seite **Allgemein** des Dialogfelds **Editor für den Task Anmeldungen übertragen** können Sie den Task Fehlermeldungen übertragen benennen und beschreiben.  
  
### <a name="options"></a>Tastatur  
 **Name**  
 Geben Sie für den Task Anmeldungen übertragen einen eindeutigen Namen ein. Dieser Name wird im Tasksymbol als Bezeichnung verwendet.  
  
> [!NOTE]  
>  Tasknamen müssen innerhalb eines Pakets eindeutig sein.  
  
 **Beschreibung**  
 Geben Sie eine Beschreibung des Tasks Anmeldungen übertragen ein.  
  
## <a name="transfer-logins-task-editor-logins-page"></a>Editor für den Task Anmeldungen übertragen (Seite Anmeldungen)
  Verwenden Sie die Seite **Anmeldungen** des Dialogfelds **Editor für den Task 'Anmeldungen übertragen'** , um die Eigenschaften für das Kopieren von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anmeldungen von einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in eine andere anzugeben.  
  
> [!IMPORTANT]  
>  Wenn der Task Anmeldungen übertragen ausgeführt wird, werden auf dem Zielserver Anmeldungen mit zufällig erzeugten Kennwörtern erstellt, und die Kennwörter werden deaktiviert. Um diese Anmeldungen zu verwenden, muss ein Mitglied der festen Serverrolle **sysadmin** die Kennwörter ändern und aktivieren. Die Anmeldung **sa** kann nicht übertragen werden.  
  
### <a name="options"></a>Tastatur  
 **SourceConnection**  
 Wählen Sie in der Liste einen SMO-Verbindungs-Manager aus, oder klicken Sie auf **\<New connection...>** , um eine neue Verbindung mit dem Quellserver zu erstellen.  
  
 **DestinationConnection**  
 Wählen Sie in der Liste einen SMO-Verbindungs-Manager aus, oder klicken Sie auf **\<New connection...>** , um eine neue Verbindung mit dem Zielserver zu erstellen.  
  
 **LoginsToTransfer**  
 Wählen Sie die vom Quell- auf den Zielserver zu kopierenden [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anmeldungen aus. Für diese Eigenschaft sind die in der folgenden Tabelle aufgeführten Optionen verfügbar:  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**AllLogins**|Alle auf dem Quellserver vorhandenen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anmeldungen werden auf den Zielserver kopiert.|  
|**SelectedLogins**|Nur die mit **LoginsList** angegebenen Anmeldungen werden auf den Zielserver kopiert.|  
|**AllLoginsFromSelectedDatabases**|Alle Anmeldungen aus den mit **DatabasesList** angegebenen Datenbanken werden auf den Zielserver kopiert.|  
  
 **LoginsList**  
 Wählen Sie die auf dem Quellserver vorhandenen Anmeldungen aus, die auf den Zielserver kopiert werden sollen. Diese Option ist nur verfügbar, wenn für **LoginsToTransfer** **SelectedLogins**ausgewählt ist.  
  
 **DatabasesList**  
 Wählen Sie die auf dem Quellserver vorhandenen Datenbanken aus, die Anmeldungen enthalten, die auf den Zielserver kopiert werden sollen. Diese Option ist nur verfügbar, wenn für **LoginsToTransfer** **AllLoginsFromSelectedDatabases**ausgewählt ist.  
  
 **IfObjectExists**  
 Wählen Sie aus, wie der Task Anmeldungen behandeln soll, die auf dem Zielserver bereits mit demselben Namen vorhanden sind.  
  
 Für diese Eigenschaft sind die in der folgenden Tabelle aufgeführten Optionen verfügbar:  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**FailTask**|Der Task schlägt fehl, wenn auf dem Zielserver bereits Anmeldungen mit demselben Namen vorhanden sind.|  
|**Overwrite**|Der Task überschreibt auf dem Zielserver Anmeldungen mit demselben Namen.|  
|**Skip**|Der Task lässt Anmeldungen aus, die auf dem Zielserver mit demselben Namen vorhanden sind.|  
  
 **CopySids**  
 Wählen Sie aus, ob die den Anmeldungen zugeordneten Sicherheits-IDs auf den Zielserver kopiert werden sollen. **CopySids** muss auf **True** festgelegt sein, wenn der Task Anmeldungen übertragen zusammen mit dem Task Datenbanken übertragen verwendet wird. Anderenfalls werden die kopierten Anmeldungen von der übertragenen Datenbank nicht erkannt.  
