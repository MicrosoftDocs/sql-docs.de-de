---
title: Konfigurieren von Advanced Data Security
titleSuffix: Azure Arc
description: Konfigurieren der erweiterten Datensicherheit für Azure Arc-fähige SQL Server-Instanzen
author: anosov1960
ms.author: sashan
ms.reviewer: mikeray
ms.date: 09/10/2020
ms.topic: conceptual
ms.prod: sql
ms.openlocfilehash: 2d7cdc1a04cfa8f3a1697eed68913f92174f972b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063225"
---
# <a name="configure-advanced-data-security-for-azure-arc-enabled-sql-server-instance"></a>Konfigurieren der erweiterten Datensicherheit für Azure Arc-fähige SQL Server-Instanzen

Sie können die erweiterte Datensicherheit für Ihre lokalen SQL Server-Instanzen aktivieren, indem Sie die folgenden Schritte ausführen.

## <a name="prerequisites"></a>Voraussetzungen

* Für Ihre SQL Server-Instanz wurde ein Onboarding in der Azure Arc-fähigen SQL Server-Instanz durchgeführt. Befolgen Sie diese Anleitung, um [ein Onboarding für Ihre SQL Server-Instanz für eine Arc-fähige SQL Server-Instanz durchzuführen](connect.md).

* Ihrem Benutzerkonto wurde eine der [Azure Security Center-Rollen (Rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC))](/azure/security-center/security-center-permissions) zugewiesen.

## <a name="create-a-log-analytics-workspace"></a>Erstellen eines Log Analytics-Arbeitsbereichs

1. Suchen Sie nach dem Ressourcentyp __Log Analytics-Arbeitsbereich__, und fügen Sie einen neuen über das Erstellungsblatt hinzu.

   ![Erstellen eines neuen Arbeitsbereichs](media/configure-advanced-data-security/create-new-log-analytics-workspace.png)

   > [!NOTE]
   > Sie können einen Log Analytics-Arbeitsbereich in einer beliebigen Region verwenden. Wenn Sie also bereits einen besitzen, können Sie diesen verwenden. Es wird jedoch empfohlen, dass Sie ihn in derselben Region erstellen, in dem Ihre Ressource __Computer – Azure Arc__ erstellt wurde.

1. Rufen Sie die Übersichtsseite der Log Analytics-Arbeitsbereichsressource auf, und klicken Sie auf „Windows, Linux und weitere Quellen“. Kopieren Sie die Arbeitsbereich-ID und den Primärschlüssel für später.

   ![Log Analytics-Arbeitsbereichsblatt](media/configure-advanced-data-security/log-analytics-workspace-blade.png)

## <a name="install-microsoft-monitoring-agent-mma"></a>Installieren des Microsoft Monitoring Agent (MMA)

Der nächste Schritt ist nur erforderlich, wenn Sie den MMA-Agent noch nicht auf dem Remotecomputer konfiguriert haben.

1. Wählen Sie die Ressource __Computer – Azure Arc__ für den virtuellen oder physischen Server aus, auf dem die SQL Server-Instanz installiert ist, und fügen Sie die Erweiterung __Microsoft Monitoring Agent – Azure Arc__ mithilfe des Features **Erweiterungen** hinzu. Wenn Sie zum Konfigurieren des Log Analytics-Arbeitsbereichs aufgefordert werden, verwenden Sie die Arbeitsbereich-ID und den Primärschlüssel, die Sie im vorherigen Schritt gespeichert haben.

   ![Installieren von MMA](media/configure-advanced-data-security/install-mma-extension.png)

1. Klicken Sie nach erfolgreicher Validierung auf **Erstellen**, um den Bereitstellungsworkflow der MMA Arc-Erweiterung zu starten. Wenn die Bereitstellung abgeschlossen wird, wird der Status in **Erfolgreich** geändert.

1. Weitere Informationen finden Sie unter [Verwalten von Erweiterungen mit Azure Arc](/azure/azure-arc/servers/manage-vm-extensions).

## <a name="enable-azure-defender"></a>Aktivieren von Azure Defender

Als Nächstes müssen Sie Azure Defender für die SQL Server-Instanz aktivieren.

1. Öffnen Sie das Security Center, und öffnen Sie die Seite **Preise und Einstellungen** über die Seitenleiste.

1. Wählen Sie den Arbeitsbereich aus, den Sie im vorherigen Schritt für die MMA-Erweiterung konfiguriert haben.

1. Klicken Sie auf **Azure Defender ein**. Stellen Sie sicher, dass die Option **SQL Server-Instanzen auf Computern** aktiviert ist.

   ![Upgraden eines Arbeitsbereichs](media/configure-advanced-data-security/enable-azure-defender.png)

 > [!NOTE]
   > Der erste Scan zum Generieren der Sicherheitsrisikobewertung erfolgt innerhalb von 24 Stunden nach Aktivierung der erweiterten Datensicherheit. Anschließend werden wöchentlich an jedem Sonntag automatische Scans durchgeführt.

## <a name="explore"></a>Erkunden

Untersuchen Sie Sicherheitsanomalien und Bedrohungen in Azure Security Center.

1. Öffnen Sie Ihre Ressource „SQL Server – Azure Arc“, und klicken Sie im linken Menü auf **Sicherheit**, um die Empfehlungen und Warnungen für diese Instanz anzuzeigen.

   ![Auswählen des Sicherheitsheaders](media/configure-advanced-data-security/security-heading-sql-server-arc.png)

1. Klicken Sie auf eine beliebige Empfehlung, um die Details zum Sicherheitsrisiko in __Security Center__ anzuzeigen.

   ![Bericht zu Sicherheitsrisiken](media/configure-advanced-data-security/vulnerabilities-report.png)

1. Klicken Sie auf eine beliebige Sicherheitswarnung, um ausführliche Informationen anzuzeigen und den Angriff in [Azure Sentinel](/azure/sentinel/overview) zu untersuchen. Im folgenden Diagramm wird ein Beispiel der Brute-Force-Warnung veranschaulicht.

   ![Brute-Force-Warnung](media/configure-advanced-data-security/brute-force-alert.png)

1. Klicken Sie auf **Aktion ausführen**, um die Ursache der Warnung zu beseitigen.

   ![Bedrohungsabwehr](media/configure-advanced-data-security/brute-force-alert-mitigation.png)

> [!NOTE]
> Der allgemeine __Security Center__-Link ganz oben auf der Seite nutzt nicht die URL für das Vorschauportal, daher werden Ihre __SQL Server – Azure Arc__-Ressourcen dort nicht angezeigt. Die folgenden Links für einzelne Empfehlungen oder Warnungen werden empfohlen.

## <a name="next-steps"></a>Nächste Schritte

Sie können Sicherheitswarnungen und Angriffe mit [Azure Sentinel](/azure/sentinel/overview) weiter untersuchen. Befolgen Sie diese [Anweisungen zum Onboarding von Azure Sentinel](/azure/sentinel/connect-data-sources).