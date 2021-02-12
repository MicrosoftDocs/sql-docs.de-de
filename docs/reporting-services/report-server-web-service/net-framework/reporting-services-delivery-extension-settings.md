---
title: Einstellungen der Reporting Services-Übermittlungserweiterungen | Microsoft-Dokumentation
description: Reporting Services bietet E-Mail-Übermittlung zum Senden von Berichten an Benutzer und die Dateifreigabeübermittlung zum Senden von Berichten an eine Freigabe. Informationen zu Einstellungen für die Übermittlungserweiterung
ms.date: 03/03/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service
ms.topic: reference
helpviewer_keywords:
- XML Web service [Reporting Services], delivery extension settings
- Report Server Web service, delivery extension settings
- delivery [Reporting Services]
- network share delivery [Reporting Services]
- file share delivery [Reporting Services]
- e-mail [Reporting Services]
- mailing reports [Reporting Services]
- sharing reports
- extensions [Reporting Services], delivery
- mail [Reporting Services]
- Web service [Reporting Services], delivery extension settings
ms.assetid: 68c31a85-261c-4ec4-b8df-1f9842b46f8a
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 6249d9a0edc81dac3cfe384716d425e7ba0f83e8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063675"
---
# <a name="reporting-services-delivery-extension-settings"></a>Einstellungen der Reporting Services-Übermittlungserweiterungen
  [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] verfügt über eine Übermittlungserweiterung für E-Mails und eine für die Dateifreigabe. Über die E-Mail-Übermittlung können Sie einen Bericht per E-Mail an einzelne Benutzer oder Gruppen senden. Über die Dateifreigabeübermittlung können Sie automatisch gerenderte Berichte versenden, die auf Ihrem Netzwerk freigegeben werden sollen. Sie können jede dieser unterstützten Übermittlungserweiterungen mit Standardabonnements oder datengesteuerten Abonnements verwenden. Sie leiten die Übermittlungseinstellungen, die spezifisch für den Typ der Übermittlungserweiterung sind, bei jedem Aufruf der Methoden <xref:ReportService2010.ReportingService2010.CreateSubscription%2A>, <xref:ReportService2010.ReportingService2010.CreateDataDrivenSubscription%2A>, <xref:ReportService2010.ReportingService2010.SetSubscriptionProperties%2A> und <xref:ReportService2010.ReportingService2010.SetDataDrivenSubscriptionProperties%2A> weiter. Um programmgesteuert eine Liste der Übermittlungseinstellungen abzurufen, verwenden Sie die <xref:ReportService2010.ReportingService2010.GetExtensionSettings%2A>-Methode.  
  
> [!NOTE]  
>  Bei den Einstellungen für die Übermittlungserweiterungen wird die Groß-/Kleinschreibung beachtet.  
  
## <a name="e-mail-delivery-settings"></a>Einstellungen für die E-Mail-Übermittlung  
 In der folgenden Tabelle werden die Einstellungen der E-Mail-Übermittlung für Abonnements aufgeführt, die Berichtsserver-E-Mail verwenden.  
  
|Einstellung|value|  
|-------------|-----------|  
|**TO**|Die E-Mail-Adresse, die in der **An**-Zeile der E-Mail-Nachricht angezeigt wird. Mehrere E-Mail-Adressen werden durch Semikolon getrennt. Erforderlich.|  
|**CC**|Die E-Mail-Adresse, die in der **CC**-Zeile der E-Mail-Nachricht angezeigt wird. Mehrere E-Mail-Adressen werden durch Semikolon getrennt. Optional.|  
|**BCC**|Die E-Mail-Adresse, die in der **Bcc**-Zeile der E-Mail-Nachricht angezeigt wird. Mehrere E-Mail-Adressen werden durch Semikolon getrennt. Optional.|  
|**ReplyTo**|Die E-Mail-Adresse, die im **Antwort-an**-Header der E-Mail-Nachricht angezeigt wird. Dieser Wert darf nur eine E-Mail-Adresse enthalten. Optional.|  
|**IncludeReport**|Ein Wert, der angibt, ob der Bericht in der E-Mail-Übermittlung enthalten sein soll. Der Wert **TRUE** gibt an, dass der Bericht im Textkörper der E-Mail-Nachricht übermittelt wird.|  
|**Renderformat**|Der Name der Renderingerweiterung, die zum Generieren des gerenderten Berichts verwendet werden soll. Der Name muss einer der sichtbaren Renderingerweiterungen entsprechen, die auf dem Berichtsserver installiert sind. Dieser Wert ist erforderlich, wenn die Einstellung **IncludeReport** auf den Wert **TRUE** festgelegt ist.|  
|**Priority**|Die Priorität, mit der die E-Mail gesendet wird. Gültige Werte sind **LOW** (niedrig), **NORMAL** (normal) und **HIGH** (hoch). Der Standardwert ist **NORMAL**.|  
|**Subject**|Der Text in der Betreffzeile der E-Mail-Nachricht.|  
|**Comment**|Der im Textkörper der E-Mail-Nachricht enthaltene Text.|  
|**IncludeLink**|Ein Wert, der angibt, ob ein Link zum Bericht im Textkörper der E-Mail enthalten sein soll.|  
  
## <a name="file-share-delivery-settings"></a>Einstellungen der Dateifreigabeübermittlung  
 In der folgenden Tabelle werden die Einstellungen der Dateifreigabeübermittlung für Abonnements aufgelistet.  
  
|Einstellung|value|  
|-------------|-----------|  
|**FILENAME**|Der Name der Datei, die auf dem Datenträger gespeichert wird.|  
|**FILEEXTN**|Gibt an, ob eine Dateierweiterung für den gerenderten Bericht enthalten sein soll. Der Wert ist entweder **TRUE** oder **FALSE**.|  
|**PATH**|Der Ordnerpfad oder der UNC-Dateifreigabepfad, in dem der Bericht gespeichert werden soll.|  
|**RENDER_FORMAT**|Das Format des Berichts, der auf dem Datenträger gespeichert wird.|  
|**USERNAME**|Der Benutzername, der benötigt wird, um auf die Netzwerkressource oder den Datenträger zuzugreifen.|  
|**PASSWORD**|Das Kennwort, das benötigt wird, um auf die Netzwerkressource oder den Datenträger zuzugreifen.|  
|**WRITEMODE**|Der Schreibmodus, der zu verwenden ist, wenn Sie auf den Datenträger zugreifen. Gültige Werte sind **None** (Keine), **Overwrite** (Überschreiben) und **AutoIncrement** (automatisch inkrementieren).|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Technische Referenz (SSRS)](../../../reporting-services/technical-reference-ssrs.md)   
 [Building Applications Using the Web Service and the .NET Framework (Erstellen von Anwendungen mit dem Webdienst und .NET Framework)](../../../reporting-services/report-server-web-service/net-framework/building-applications-using-the-web-service-and-the-net-framework.md)  
  
  
