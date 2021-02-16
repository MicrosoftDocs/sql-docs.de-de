---
description: Ereignisprotokollierung durch den Integration Services-Dienst
title: Ereignisprotokollierung durch den Integration Services-Dienst | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- service [Integration Services], events
- events [Integration Services], service
- Integration Services service, events
ms.assetid: d4122dcf-f16f-47a0-93a2-ffa3d0d4f9cf
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 45612ea0bfdbda2b27e6cac8b4deb1e302eb9298
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346552"
---
# <a name="events-logged-by-the-integration-services-service"></a>Ereignisprotokollierung durch den Integration Services-Dienst

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Der [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Dienst protokolliert verschiedene Meldungen in das Windows-Anwendungsereignisprotokoll. Der Dienst Paket protokolliert diese Meldungen, wenn er startet, wenn er anhält und wenn bestimmte Probleme auftreten.  
  
 Dieses Thema enthält Informationen über die allgemeinen Ereignismeldungen, die von einem Dienst im Anwendungsereignisprotokoll protokolliert werden. Der [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Dienst protokolliert alle in diesem Thema beschriebenen Meldungen mit der Ereignisquelle SQLISService.  
  
 Allgemeine Informationen zum [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Dienst finden Sie unter [Integration Services-Dienst &#40;SSIS-Dienst&#41;](../../integration-services/service/integration-services-service-ssis-service.md).  
  
## <a name="service-status-messages"></a>Meldungen zur Servicequalität
 Wenn Sie [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] zur Installation auswählen, wird der Dienst [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] installiert und gestartet, und der Starttyp wird auf Automatisch gesetzt.  
  
|Ereignis-ID|Symbolischer Name|Text|Notizen|  
|--------------|-------------------|----------|-----------|  
|256|DTS_MSG_SERVER_STARTING|Der [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssIS](../../includes/ssis-md.md)]-Dienst wird gestartet.|Der Dienst wird gerade gestartet.|  
|257|DTS_MSG_SERVER_STARTED|Der [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssIS](../../includes/ssis-md.md)]-Dienst wurde gestartet.|Der Dienst wurde gestartet.|  
|260|DTS_MSG_SERVER_START_FAILED|Fehler beim Starten des [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssIS](../../includes/ssis-md.md)]-Diensts.%nFehler: %1|Der Dienst konnte nicht gestartet werden. Dass der Dienst nicht starten konnte, könnte die Folge einer beschädigten Installation oder eines unpassenden Dienstkontos sein.|  
|258|DTS_MSG_SERVER_STOPPING|Der [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssIS](../../includes/ssis-md.md)]-Dienst wird beendet.%n%n Alle ausgeführten Pakete werden beim beenden angehalten. %1|Der Dienst wird angehalten, und wenn der Dienst so konfiguriert ist, hält er alle ausgeführten Pakete an. In der Konfigurationsdatei können Sie den Wert True oder False festlegen. Dieser Wert bestimmt, ob der Dienst die Ausführung von Paketen beendet, wenn der Dienst selbst beendet wird. Die Meldung für dieses Ereignis enthält den Wert dieser Einstellung.|  
|259|DTS_MSG_SERVER_STOPPED|Der [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssIS](../../includes/ssis-md.md)]-Dienst wurde beendet.%nServerversion %1|Der Dienst wurde beendet.|  
  
## <a name="settings-file-messages"></a>Meldungen zur Einstellungsdatei  
 Einstellungen für den [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Dienst werden in einer XML-Datei gespeichert, die Sie ändern können. Weitere Informationen finden Sie unter [Integration Services-Dienst &#40;SSIS-Dienst&#41;](../../integration-services/service/integration-services-service-ssis-service.md).  
  
|Ereignis-ID|Symbolischer Name|Text|Notizen|  
|--------------|-------------------|----------|-----------|  
|274|DTS_MSG_SERVER_MISSING_CONFIG_REG|[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssIS](../../includes/ssis-md.md)]-Dienst: %nRegistrierungseinstellung mit Angabe der Konfigurationsdatei ist nicht vorhanden. %nEs wird versucht, die Standardkonfigurationsdatei zu laden.|Der Registrierungseintrag, der den Pfad der Konfigurationsdatei enthält, ist nicht vorhanden oder leer.|  
|272|DTS_MSG_SERVER_MISSING_CONFIG|Die Konfigurationsdatei für den [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssIS](../../includes/ssis-md.md)]-Dienst ist nicht vorhanden.%nWird mit Standardeinstellungen geladen.|Die Konfigurationsdatei ist am angegebenen Speicherort nicht vorhanden.|  
|273|DTS_MSG_SERVER_BAD_CONFIG|Falsche Konfigurationsdatei für den [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssIS](../../includes/ssis-md.md)]-Dienst.%nFehler beim Lesen der Konfigurationsdatei: %1%n%nServer wird mit Standardeinstellungen geladen.|Die Konfigurationsdatei konnte nicht gelesen werden oder ist nicht gültig. Dieser Fehler könnte die Folge eines XML-Syntaxfehlers in der Datei sein.|  
  
## <a name="other-messages"></a>Andere Meldungen  
  
|Ereignis-ID|Symbolischer Name|Text|Notizen|  
|--------------|-------------------|----------|-----------|  
|336|DTS_MSG_SERVER_STOPPING_PACKAGE|[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssIS](../../includes/ssis-md.md)]-Dienst; Ausgeführtes Paket wird beendet.%nPaketinstanz-ID: %1%nPaket-ID: %2%nPaketname: %3%nPaketbeschreibung: %4%nPaket|Der Dienst versucht, ein ausgeführtes Paket zu beenden. Sie können ausgeführte Pakete in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]überwachen und anhalten. Weitere Informationen zum Verwalten von Paketen in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] finden Sie unter [Paketverwaltung &#40;SSIS-Dienst&#41;](../../integration-services/service/package-management-ssis-service.md).|  

## <a name="view-events"></a>Anzeigen von Ereignissen
  Es gibt zwei Tools, in denen Sie Ereignisse für den [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Dienst anzeigen können:  
  
-   Das Dialogfeld **Protokolldatei-Viewer** in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Das Dialogfeld **Protokolldatei-Viewer** enthält Optionen zum Exportieren, Filtern und Durchsuchen des Protokolls. Weitere Informationen zu den Optionen im Dialogfeld **Protokolldatei-Viewer** finden Sie unter [Protokolldatei-Viewer (F1-Hilfe)](../../relational-databases/logs/log-file-viewer-f1-help.md).  
  
-   Die Windows-Ereignisanzeige.  
  
 Eine Beschreibung der vom [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Dienst protokollierten Ereignisse finden Sie unter [Ereignisprotokollierung durch den Integration Services-Dienst](../../integration-services/service/events-logged-by-the-integration-services-service.md).  
  
### <a name="to-view-service-events-for-integration-services-in-sql-server-management-studio"></a>So zeigen Sie Dienstereignisse für Integration Services in SQL Server Management Studio an  
  
1.  Öffnen Sie [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
2.  Klicken Sie im Menü **Datei** auf **Objekt-Explorer verbinden**.  
  
3.  Wählen Sie im Dialogfeld **Verbindung mit Server herstellen** den [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Servertyp aus, wählen Sie den Server aus, mit dem die Verbindung hergestellt werden soll, oder suchen Sie ihn, und klicken Sie anschließend auf **Verbinden**.  
  
4.  Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , und klicken Sie anschließend auf **Protokolle anzeigen**.  
  
5.  Wählen Sie [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] SQL Server Integration Services **aus, um die**-Ereignisse anzuzeigen. Die Option **NT-Ereignisse** ist automatisch ausgewählt und wird durch die Option **SQL Server Integration Services** deaktiviert.  
  
### <a name="to-view-service-events-for-integration-services-in-windows-event-viewer"></a>So zeigen Sie Dienstereignisse für Integration Services in der Windows-Ereignisanzeige an  
  
1.  Wenn Sie die klassische Ansicht verwenden, klicken Sie in der **Systemsteuerung** auf **Verwaltung**. Wenn Sie die Kategorieansicht verwenden, klicken Sie auf **Leistung und Wartung** und dann auf **Verwaltung**.  
  
2.  Klicken Sie auf **Ereignisanzeige**.  
  
3.  Klicken Sie im Dialogfeld **Ereignisanzeige** auf **Anwendung**.  
  
4.  Suchen Sie im **Anwendungs** -Snap-In einen Eintrag in der Spalte **Quelle** , die den Wert **SQLISService** enthält. Klicken Sie mit der rechten Maustaste auf den Eintrag und anschließend auf **Eigenschaften**.  
  
5.  Klicken Sie wahlweise auf den Nach-Oben- oder Nach-Unten-Pfeil, um das vorherige oder nächste Ereignis anzuzeigen.  
  
6.  Klicken Sie wahlweise auf das Symbol "In Zwischenablage speichern", um die Ereignisinformationen zu kopieren.  
  
7.  Zeigen Sie die Ereignisdaten in Byte oder Wörtern an.  
  
8.  Klicken Sie auf **OK**.  
  
9. Klicken Sie im Menü **Datei** auf **Beenden** , um das Dialogfeld **Ereignisanzeige** zu schließen.  
 
## <a name="related-tasks"></a>Related Tasks  
 Informationen zum Anzeigen von Protokolleinträgen finden Sie unter [Durch ein Integration Services-Paket protokollierte Ereignisse](../../integration-services/performance/events-logged-by-an-integration-services-package.md)  
