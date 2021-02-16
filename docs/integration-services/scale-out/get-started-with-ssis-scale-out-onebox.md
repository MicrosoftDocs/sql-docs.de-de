---
title: Erste Schritte mit SSIS Scale Out auf einem einzelnen Computer | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie alles, was Sie wissen müssen, um in SSIS Scale Out auf einem einzelnen Computer einzusteigen.
ms.custom: performance
ms.date: 12/13/2017
ms.prod: sql
ms.technology: integration-services
ms.topic: conceptual
author: haoqian
ms.author: haoqian
ms.reviewer: maghan
ms.openlocfilehash: 1cf9077a9b500c9e5052b48aab13d325809bb297
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347384"
---
# <a name="get-started-with-integration-services-ssis-scale-out-on-a-single-computer"></a>Erste Schritte mit Integration Services (SSIS) Scale Out auf einem einzelnen Computer

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


In diesem Artikel finden Sie eine Anleitung zum Einrichten von Integration Services Scale Out in einer Umgebung mit einem Computer mithilfe von Standardeinstellungen.

## <a name="1-install-sql-server-features"></a>1. Installieren von SQL Server-Funktionen
Wählen Sie im Installations-Assistenten auf der Seite **Funktionsauswahl** folgende Elemente aus:
-   -Datenbank-Engine-Dienste
-   Integration Services
    -   Scale Out-Master
    -   Scale Out-Worker

![Erste Hälfte der Seite „Funktionsauswahl“](media/feature-select-onebox1.PNG)

![Zweite Hälfte der Seite „Funktionsauswahl“](media/feature-select-onebox2.PNG)

Klicken Sie auf der Seite **Serverkonfiguration** auf **Weiter**, um Standarddienstkonten und -starttypen anzunehmen.

Wählen Sie auf der Seite **Database Engine Configuration** (Konfiguration der Datenbank-Engine) **gemischter Modus** aus, und klicken Sie auf **Aktuellen Benutzer hinzufügen**. 

![Engine-Konfiguration](media/engine-config.PNG)

Klicken Sie auf den Seiten **Integration Services Scale Out-Konfiguration – Masterknoten** und **Integration Services Scale Out-Konfiguration – Workerknoten** auf **Weiter**, um die Standardeinstellungen für den Port und die Zertifikate anzunehmen.

Stellen Sie den Assistenten zum Installieren von SQL Server fertig.

## <a name="2-install-sql-server-management-studio"></a>2. Installieren von SQL Server Management Studio

Laden Sie [SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md) herunter, und installieren Sie das Programm.

## <a name="3-enable-scale-out"></a>3. Aktivieren von Scale Out
Öffnen Sie SSMS, und stellen Sie eine Verbindung mit einer lokalen SQL Server-Instanz her.
Klicken Sie in Objekt-Explorer auf **Integration Services-Kataloge**, und wählen Sie **Katalog erstellen** aus.

Im Dialogfeld **Katalog erstellen** ist standardmäßig die Option **Diesen Server als SSIS Scale Out-Master aktivieren** ausgewählt.

## <a name="4-enable-a-scale-out-worker"></a>4. Aktivieren des Scale Out-Workers
Klicken Sie in SSMS auf **SSISDB**, und wählen Sie **Scale Out verwalten** aus. 

![Aufskalieren verwalten](media/manage-scale-out.PNG)

Die App „Integration Services Scale Out-Manager“ wird geöffnet. Weitere Informationen finden Sie unter [Scale Out-Manager](integration-services-ssis-scale-out-manager.md).

Wechseln Sie zu **Worker-Manager**, und wählen Sie den gewünschten Worker aus, um einen Scale Out-Worker zu aktivieren. Einige Worker sind standardmäßig deaktiviert. Klicken Sie zur Aktivierung des ausgewählten Workers auf **Worker aktivieren**.

## <a name="5-run-packages-in-scale-out"></a>5. Ausführen von Paketen in Scale Out
Jetzt können Sie SSIS-Pakete in Scale Out ausführen. Weitere Informationen finden Sie unter [Run Packages in Integration Services (SSIS) Scale Out (Ausführen von Paketen für Scale Out mit SQL Server Integration Services (SSIS))](run-packages-in-integration-services-ssis-scale-out.md).

## <a name="next-steps"></a>Nächste Schritte
-   [Hinzufügen eines Scale Out-Workers mit dem Scale Out-Manager](add-scale-out-worker.md)
