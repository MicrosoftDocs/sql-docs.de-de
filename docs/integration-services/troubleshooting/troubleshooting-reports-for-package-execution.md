---
description: Behandlung von Problemen in Berichten für die Paketausführung
title: Behandlung von Problemen mit Berichten für die Paketausführung | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 8fc476ac-bd69-434e-9636-70776e0b3b6c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 7c59111f0626aa9506c0cab17be903da21ff5245
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342209"
---
# <a name="troubleshooting-reports-for-package-execution"></a>Behandlung von Problemen in Berichten für die Paketausführung

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  In der aktuellen Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]sind Standardberichte in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] verfügbar, die für die Überwachung und Problembehandlung der im [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Katalog bereitgestellten [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Pakete hilfreich sind. Zwei dieser Paketberichte dienen insbesondere zum Anzeigen des Status der Paketausführung und zum Ermitteln der Ursache von Ausführungsfehlern.  
  
-   **Integration Services-Dashboard** : Dieser Bericht bietet eine Übersicht über alle Paketausführungen auf der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz in den letzten 24 Stunden. Der Bericht zeigt Informationen, wie z. B. den Status, Vorgangstyp, Paketnamen usw., für jedes Paket an.  
  
     Startzeit, Endzeit und Dauer können wie folgt interpretiert werden:  
  
    -   Wenn das Paket noch ausgeführt wird, gilt Dauer = aktuelle Zeit - Startzeit.  
  
    -   Wenn das Paket abgeschlossen wurde, gilt Dauer = Endzeit - Startzeit.  
  
     Im Dashboard können Sie die Informationen zu jedem auf dem Server ausgeführten Paket erweitern, um spezifische Details über möglicherweise aufgetretene Paketausführungsfehler zu suchen. Sie können z.B. auf **Übersicht** klicken, um eine allgemeine Übersicht über die Status der in der Ausführung enthaltenen Tasks anzuzeigen, oder auf **Alle Meldungen** , um die ausführlichen Meldungen anzuzeigen, die bei der Paketausführung erfasst wurden.  
  
     Sie können die auf einer beliebigen Seite angezeigte Tabelle filtern, indem Sie auf **Filtern** klicken und dann im Dialogfeld **Filtereinstellungen** die gewünschten Kriterien auswählen. Abhängig von den angezeigten Daten sind unterschiedliche Filterkriterien verfügbar. Sie können die Sortierreihenfolge des Berichts ändern, indem Sie im Dialogfeld **Filtereinstellungen** auf das Sortiersymbol klicken.  
  
-   **Bericht „Aktivität – Alle Vorgänge“:** Dieser Bericht zeigt eine Zusammenfassung aller [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Ausführungen auf dem Server. Die Zusammenfassung enthält Informationen für jede Ausführung, z. B. Status, Startzeit und Endzeit. Jeder Zusammenfassungseintrag enthält Links zu weiteren Informationen zur Ausführung, z. B. während der Ausführung generierten Meldungen und Leistungsdaten. Wie in dem Integration Services-Dashboard können Sie einen Filter auf die Tabelle anwenden, um die angezeigten Informationen einzugrenzen.  
  
## <a name="related-content"></a>Verwandte Inhalte  
 [Berichte für den Integration Services-Server](../../integration-services/performance/monitor-running-packages-and-other-operations.md#reports)  
  
 [Behandlung von Problemen mit Paketausführungstools](../../integration-services/troubleshooting/troubleshooting-tools-for-package-execution.md)  
  
  
