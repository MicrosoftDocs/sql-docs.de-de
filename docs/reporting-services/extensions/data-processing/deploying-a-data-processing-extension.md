---
title: Bereitstellen einer Datenverarbeitungserweiterung | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie dafür sorgen können, dass die Datenverarbeitungserweiterung der Reporting Services vom Berichtsserver und vom Berichts-Designer erkannt werden kann.
ms.date: 03/18/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- data processing extensions [Reporting Services], deploying
- Extension element
- deploying [Reporting Services], extensions
ms.assetid: e5c0b5a9-1386-47cb-aade-96653ecfaa54
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: df78278f655d6a11bb560440f86d662c08889059
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100061155"
---
# <a name="deploying-a-data-processing-extension"></a>Bereitstellen von Datenverarbeitungserweiterungen
  Wenn Sie die [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]-Datenverarbeitungserweiterung geschrieben und in eine [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)]-Bibliothek kompiliert haben, müssen Sie sie für den Berichtsserver und den Berichts-Designer erkennbar machen. Dazu müssen Sie lediglich die Erweiterung in die entsprechenden Verzeichnisse kopieren und Einträge zu den zugehörigen [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]-Konfigurationsdateien hinzufügen.  
  
## <a name="configuration-file-extension-element"></a>Extension-Element für die Konfigurationsdatei  
 Datenverarbeitungserweiterungen, die Sie für den Berichtsserver oder den Berichts-Designer bereitstellen, müssen in den Konfigurationsdateien als **Extension**-Elemente hinzugefügt werden. Bei diesen Dateien handelt es sich um RSReportServer.config für den Berichtsserver und RSReportDesigner.config für den Berichts-Designer.  
  
 In der folgenden Tabelle werden die Attribute für das **Extension**-Element für Datenverarbeitungserweiterungen beschrieben.  
  
|Attribut|BESCHREIBUNG|  
|---------------|-----------------|  
|**Name**|Ein eindeutiger Name für die Erweiterung, z. B. "SQL" für die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Datenverarbeitungserweiterung oder "OLEDB" für die OLE DB-Datenverarbeitungserweiterung. Die maximale Länge für das **Name** -Attribut beträgt 255 Zeichen. Der Name muss für sämtliche Einträge im **Extension** -Element einer Konfigurationsdatei eindeutig sein.|  
|**Typ**|Eine durch Trennzeichen getrennte Liste, die den vollqualifizierten Namespace und den Namen der Assembly enthält|  
|**Visible**|Der Wert **FALSE** gibt an, dass die Datenverarbeitungserweiterung auf Benutzeroberflächen nicht sichtbar sein soll. Wenn das Attribut nicht enthalten ist, ist **true** der Standardwert.|  
  
 Weitere Informationen über die Dateien „RSReportServer.config“ und „RSReportDesigner.config“ finden Sie unter [Reporting Services-Konfigurationsdateien](../../../reporting-services/report-server/reporting-services-configuration-files.md).  
  
## <a name="in-this-section"></a>In diesem Abschnitt  
  
|Thema|BESCHREIBUNG|  
|-----------|-----------------|  
|[Vorgehensweise: Bereitstellen einer Datenverarbeitungserweiterung für einen Berichtsserver](../../../reporting-services/extensions/data-processing/deploying-a-data-processing-extension-to-a-report-server.md)|Beschreibt, wie Sie eine Datenverarbeitungserweiterung auf einem Berichtsserver bereitstellen|  
|[Vorgehensweise: Bereitstellen einer Datenverarbeitungserweiterung für den Berichts-Designer](../../../reporting-services/extensions/data-processing/deploying-a-data-processing-extension-to-report-designer.md)|Beschreibt, wie Sie eine Datenverarbeitungserweiterung für den Berichts-Designer bereitstellen|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Erweiterungen für Reporting Services](../../../reporting-services/extensions/reporting-services-extensions.md)   
 [Implementieren von Datenverarbeitungserweiterungen](../../../reporting-services/extensions/data-processing/implementing-a-data-processing-extension.md)   
 [Reporting Services Extension Library (Reporting Services-Erweiterungsbibliothek)](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
