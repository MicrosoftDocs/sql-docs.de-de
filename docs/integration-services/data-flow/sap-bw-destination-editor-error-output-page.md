---
description: Ziel-Editor für SAP BW (Seite Fehlerausgabe)
title: Ziel-Editor für SAP BW (Seite Fehlerausgabe) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.sapbwdestination.erroroutput.f1
ms.assetid: a543d811-0bd2-4890-a0d3-f5fdcd4524b8
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 6c3c080e03e34b8b7cf953a5d3544cb4d3bb39ab
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/26/2020
ms.locfileid: "88484530"
---
# <a name="sap-bw-destination-editor-error-output-page"></a>Ziel-Editor für SAP BW (Seite Fehlerausgabe)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Auf der Seite **Fehlerausgabe** im **Ziel-Editor für SAP BW** geben Sie Optionen für die Fehlerbehandlung an.  
  
 Weitere Informationen zum SAP BW-Ziel von [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector 1.1 for SAP BW finden Sie unter [SAP BW-Ziel](../../integration-services/data-flow/sap-bw-destination.md).  
  
> [!IMPORTANT]  
>  Die Dokumentation für Microsoft Connector 1.1 for SAP BW setzt Kenntnisse der SAP NetWeaver BW-Umgebung voraus. Weitere Informationen zu SAP NetWeaver BW oder Informationen zur Konfiguration von SAP NetWeaver BW-Objekten und -Prozessen finden Sie in der SAP-Dokumentation.  
  
 **So öffnen Sie die Seite "Fehlerausgabe"**  
  
1.  Öffnen Sie in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]das [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Paket, in dem das SAP BW-Ziel enthalten ist.  
  
2.  Doppelklicken Sie auf der Registerkarte **Datenfluss** auf das SAP BW-Ziel.  
  
3.  Klicken Sie im **Ziel-Editor für SAP BW** auf **Fehlerausgabe** , um die Seite **Fehlerausgabe** des Editors zu öffnen.  
  
## <a name="options"></a>Tastatur  
  
> [!NOTE]  
>  Wenn Sie nicht alle Werte kennen, die zur Konfiguration des Ziels erforderlich sind, müssen Sie ggf. Ihren SAP-Administrator um Unterstützung bitten.  
  
 **Eingabe oder Ausgabe**  
 Zeigt den Namen der Eingabe an.  
  
 **Spalte**  
 Diese Option wird nicht verwendet.  
  
 **Fehler**  
 Gibt an, welche Aktion das Ziel bei einem Fehler ausführen soll: den Fehler ignorieren, die Zeile umleiten oder die Komponente mit einem Fehler abbrechen.  
  
 **Abschneiden**  
 Diese Option wird nicht verwendet.  
  
 **Beschreibung**  
 Zeigt die Beschreibung des Vorgangs an.  
  
 **Diesen Wert für ausgewählte Zellen festlegen**  
 Gibt an, wie das Ziel bei einem Fehler oder einer Kürzung mit den ausgewählten Zellen verfahren soll: den Fehler ignorieren, die Zeile umleiten oder die Komponente mit einem Fehler abbrechen.  
  
 **Anwenden**  
 Wendet die Fehlerbehandlungsoption auf die ausgewählten Zellen an.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Ziel-Editor für SAP BW &#40;Seite „Verbindungs-Manager“&#41;](../../integration-services/data-flow/sap-bw-destination-editor-connection-manager-page.md)   
 [Ziel-Editor für SAP BW &#40;Seite „Zuordnungen“&#41;](../../integration-services/data-flow/sap-bw-destination-editor-mappings-page.md)   
 [Ziel-Editor für SAP BW &#40;Seite „Erweitert“&#41;](../../integration-services/data-flow/sap-bw-destination-editor-advanced-page.md)   
 [F1-Hilfe zum Microsoft Connector for SAP BW](../../integration-services/microsoft-connector-for-sap-bw-f1-help.md)  
  
  
