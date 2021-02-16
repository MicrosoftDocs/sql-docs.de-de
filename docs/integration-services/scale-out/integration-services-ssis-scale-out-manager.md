---
title: SQL Server Integration Services Scale Out-Manager | Microsoft-Dokumentation
description: In diesem Artikel wird das Scale Out-Manager-Tool beschrieben, das Sie zum Verwalten von SSIS Scale Out verwenden können.
ms.custom: performance
ms.date: 12/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: haoqian
ms.author: haoqian
ms.openlocfilehash: a56792633259e2f8d224696f94c7095a0ec1ae96
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347419"
---
# <a name="integration-services-scale-out-manager"></a>Integration Services Scale Out-Manager

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



Der Scale Out-Manager ist ein Verwaltungstool, mit dem Sie Ihre gesamte SSIS Scale Out-Topologie mithilfe einer einzigen App verwalten können. Dadurch müssen Sie keine Verwaltungsaufgaben mehr erledigen und keine Transact-SQL-Befehle mehr auf mehreren Computern ausführen.

## <a name="open-scale-out-manager"></a>Öffnen des Scale Out-Managers

Es gibt zwei Möglichkeiten, den Scale Out-Manager zu öffnen.

### <a name="1-open-scale-out-manager-from-sql-server-management-studio"></a>1. Öffnen Sie Scale Out-Manager über SQL Server Management Studio.
Öffnen Sie SQL Server Management Studio (SSMS), und stellen Sie eine Verbindung mit der SQL Server-Instanz von Scale Out-Master her.

Klicken Sie in Objekt-Explorer mit der rechten Maustaste auf **SSISDB**, und wählen Sie **Scale Out verwalten** aus.

![Aufskalieren verwalten](media/manage-scale-out.PNG)

> [!NOTE]
> Es wird empfohlen, SSMS als Administrator auszuführen, da Sie für einige Verwaltungsoptionen für Scale Out wie das Hinzufügen eines Scale Out-Workers Administratorrechte benötigen.

### <a name="2-open-scale-out-manager-by-running-managementtoolexe"></a>2. Öffnen Sie den Scale Out-Manager, indem Sie „ManagementTool.exe“ ausführen

Suchen Sie `ManagementTool.exe` unter `%SystemDrive%\Program Files (x86)\Microsoft SQL Server\150\DTS\Binn\Management`. Klicken Sie mit der rechten Maustaste auf **ManagementTool.exe**, und wählen Sie **Als Administrator ausführen** aus. 

Nachdem sich der Scale Out-Manager geöffnet hat, geben Sie den Namen der SQL Server-Instanz des Scale Out-Masters ein, und stellen Sie eine Verbindung mit dieser Instanz her, um die Scale Out-Umgebung zu verwalten.

![Portal: Verbindung herstellen](media/portal-connect-new.png)

## <a name="tasks-available-in-scale-out-manager"></a>Verfügbare Aufgaben im Scale Out-Manager
Im Scale Out-Manager können Sie folgende Schritte ausführen:

### <a name="enable-scale-out"></a>Aktivieren von Scale Out
Wenn Scale Out deaktiviert ist, können Sie zum Aktivieren auf **Aktivieren** klicken, nachdem Sie eine Verbindung mit SQL Server hergestellt haben.

![Portal: Aktivieren von Scale Out](media/portal-enable-scale-out-new.PNG) 

### <a name="view-scale-out-master-status"></a>Anzeigen des Status von Scale Out-Master
Der Status von Scale Out-Master wird auf der **Dashboard**-Seite angezeigt.

![Dashboard des Portals](media/portal-dashboard-new.PNG)

### <a name="view-scale-out-worker-status"></a>Anzeigen des Status des Scale Out-Workers
Der Status des Scale Out-Workers wird auf der Seite **Worker-Manager** angezeigt. Sie können einen beliebigen Worker auswählen, um den jeweiligen Status anzuzeigen.

![Portal: Worker-Manager](media/portal-worker-manager-new.PNG)

### <a name="add-a-scale-out-worker"></a>Hinzufügen eines Scale Out-Workers
Klicken Sie im unteren Bereich der Liste „Scale Out-Worker“ auf **+** , um einen Scale Out-Worker hinzuzufügen. 

Geben Sie den Computernamen des Scale Out-Workers ein, den Sie hinzufügen möchten, und klicken Sie auf **Überprüfen**. Der Scale Out-Manager überprüft, ob der aktuelle Benutzer Zugriff auf die Zertifikatspeicher der Computer des Scale Out-Masters und des Scale Out-Workers hat.

![Verbindung herstellen: Worker](media/connect-worker-new.PNG)

Wenn die Überprüfung erfolgreich ist, versucht der Scale Out-Manager, die Konfigurationsdatei des Servers des Workers zu lesen und den Zertifikatfingerabdruck des Workers abzurufen. Weitere Informationen finden Sie unter [Scale Out-Worker](integration-services-ssis-scale-out-worker.md). Wenn der Scale Out-Manager die Konfigurationsdatei des Workerdiensts nicht lesen kann, gibt es zwei Alternativen, um das Workerzertifikat bereitzustellen. 

- Sie können entweder den Fingerabdruck des Workerzertifikats direkt eingeben

    ![Workerzertifikat (1)](media/portal-cert1-new.PNG)

- oder die Zertifikatdatei bereitstellen.

    ![Workerzertifikat (2)](media/portal-cert2-new.PNG)

Nachdem Informationen erfasst wurden, beschreibt der Scale Out-Manager die Aktionen, die ausgeführt werden sollen. In der Regel handelt es sich bei diesen Optionen um die Installation des Zertifikats, die Aktualisierung der Konfigurationsdatei des Workers und den Neustart des Workerdiensts.

![Portal: Hinzufügen bestätigen (1)](media/portal-add-confirm1-new.PNG)

Falls nicht auf die Workereinstellung zugegriffen werden kann, müssen Sie diese manuell aktualisieren und den Workerdienst neu starten.

![Portal: Hinzufügen bestätigen (2)](media/portal-add-confirm2-new.PNG)

Aktivieren Sie das Kontrollkästchen **Bestätigen**, und klicken Sie dann auf **OK**, um den Vorgang zum Hinzufügen eines Scale Out-Workers zu starten.

### <a name="delete-a-scale-out-worker"></a>Löschen eines Scale Out-Workers
Wählen Sie einen Scale Out-Worker aus, und klicken Sie im unteren Bereich der Liste „Scale Out-Worker“ auf **-** , um einen Scale Out-Worker zu löschen.

### <a name="enable-or-disable-a-scale-out-worker"></a>Aktivieren oder Deaktivieren eines Scale Out-Workers
Wählen Sie einen Scale Out-Worker aus, und klicken Sie auf die Schaltfläche **Worker aktivieren** oder **Worker deaktivieren**, um einen Scale Out-Worker zu aktivieren bzw. zu deaktivieren. Wenn der Worker online ist, ändert sich dessen Status, der im Scale Out-Manager angezeigt wird.

## <a name="edit-a-scale-out-worker-description"></a>Bearbeiten der Beschreibung eines Scale Out-Workers
Wählen Sie einen Scale Out-Worker aus, und klicken Sie auf die Schaltfläche **Bearbeiten**, um die Beschreibung eines Scale Out-Workers zu bearbeiten. Klicken Sie auf **Speichern**, nachdem Sie die Beschreibung vollständig bearbeitet haben.

![Portal: Worker speichern](media/portal-save-worker-new.PNG)

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in den folgenden Artikeln:
-   [Scale Out-Master von Integration Services (SSIS)](integration-services-ssis-scale-out-master.md)
-   [Scale Out-Worker von Integration Services (SSIS)](integration-services-ssis-scale-out-worker.md)
