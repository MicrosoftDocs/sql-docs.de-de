---
title: Hinzufügen eines SSIS Scale Out-Workers mit dem Scale Out-Manager | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie ein SSIS Scale Out-Worker mithilfe des Scale Out-Managers in eine vorhandene Scale Out-Umgebung hinzugefügt wird.
ms.custom: performance
ms.date: 12/19/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: haoqian
ms.author: haoqian
ms.openlocfilehash: 961ea14fc7663206a4d1ad86c6ca611be0f863d9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354893"
---
# <a name="add-a-scale-out-worker-with-scale-out-manager"></a>Hinzufügen eines SSIS Scale Out-Workers mit dem Scale Out-Manager

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



Der Scale Out-Manager für Integration Services vereinfacht das Hinzufügen des Scale Out-Workers zu Ihrer bereits bestehenden Scale Out-Umgebung. 

Führen Sie die folgenden Schritte aus, um Ihrer Scale Out-Topologie einen Scale Out-Worker hinzufügen:

## <a name="1-install-scale-out-worker"></a>1. Installieren des SSIS Scale Out-Workers
Wählen Sie im Assistenten zum Installieren von SQL Server auf der Seite **Funktionsauswahl** „Integration Services“ und „Scale Out-Worker“ aus. 
![Komponentenauswahl Worker](media/feature-select-worker.PNG)

Auf der Seite **Integration Services Scale Out-Konfiguration – Workerknoten** können Sie auf **Weiter** klicken, um die Konfiguration an dieser Stelle zu überspringen, und den **Scale Out-Manager** verwenden, um die Konfiguration nach der Installation durchzuführen.

Schließen Sie den Installations-Assistenten ab.

## <a name="2-open-the-firewall-on-the-scale-out-master-computer"></a>2. Öffnen Sie die Firewall auf dem Scale Out-Mastercomputer.
Öffnen Sie in der Windows-Firewall auf dem Scale Out-Mastercomputer den Port, der während der Installation des Scale Out-Masters angegeben wurde (standardmäßig 8391), sowie den Port für SQL Server (standardmäßig 1433).

## <a name="3-add-a-scale-out-worker-with-scale-out-manager"></a>3. Hinzufügen eines SSIS Scale Out-Workers mit dem Scale Out-Manager
Führen Sie SQL Server Management Studio als Administrator aus, und stellen Sie eine Verbindung mit der SQL Server-Instanz des Scale Out-Masters her.

Klicken Sie in Objekt-Explorer mit der rechten Maustaste auf **SSISDB**, und wählen Sie **Manage Scale Out** (Scale Out verwalten) aus. 

![Aufskalieren verwalten](media/manage-scale-out.PNG)

Wechseln Sie im Dialogfeld **Scale Out-Manager** zu **Worker-Manager**. Klicken Sie auf die **+** , und befolgen Sie die Anweisungen im Dialogfeld **Connect Worker** (Worker verbinden). 

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie unter [Scale Out-Manager](integration-services-ssis-scale-out-manager.md).
