---
title: Debuggen von Übermittlungserweiterungscode | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Debuggingtools für das Microsoft .NET Framework verwenden können, um Ihren Übermittlungserweiterungscode zu analysieren und Fehler darin zu finden.
ms.date: 03/16/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- delivery extensions [Reporting Services], debugging
- debugging delivery extensions [Reporting Services]
- troubleshooting [Reporting Services], delivery extensions
ms.assetid: a7d959da-5005-4a50-aca7-2cef36aa9947
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 830ff0bf030ad81f02f915ff50078baef9d57600
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100060965"
---
# <a name="debugging-delivery-extension-code"></a>Debuggen von Übermittlungserweiterungscode
  Das [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] stellt mehrere hilfreiche Tools zum Debuggen zur Verfügung, die Sie bei der Analyse des Codes für Übermittlungserweiterungen und bei der Fehlersuche darin unterstützen. Welches Tool dafür am besten geeignet ist, hängt von Ihrer Zielsetzung ab. In diesem Beispiel wird [!INCLUDE[vsOrcas](../../../includes/vsorcas-md.md)] verwendet.  
  
#### <a name="to-debug-your-delivery-extension-code"></a>So debuggen Sie Code für Übermittlungserweiterungen  
  
1.  Starten Sie [!INCLUDE[vsOrcas](../../../includes/vsorcas-md.md)], und öffnen Sie das Projekt mit der Übermittlungserweiterung.  
  
2.  Erstellen Sie das Projekt, und stellen Sie die Assembly der Übermittlungserweiterung und die dazugehörige PDB-Datei im Berichtsserver und im Berichts-Manager bereit. Weitere Informationen zur Bereitstellung finden Sie unter [Bereitstellen von Übermittlungserweiterungen](../../../reporting-services/extensions/delivery-extension/deploying-a-delivery-extension.md).  
  
3.  Wenn Sie eine Abonnementbenutzeroberfläche zur Erweiterung des Berichts-Managers geschrieben haben, öffnen Sie den Internet Explorer, und navigieren Sie zum Berichts-Manager, während Sie den Code der Übermittlungserweiterung in [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] geöffnet lassen. Wenn Sie keine Abonnementbenutzeroberfläche auf dem Berichts-Manager eingesetzt haben, öffnen Sie einfach mithilfe der SOAP-API die Clientanwendung, von der Sie die Übermittlungserweiterung aufrufen.  
  
4.  Wechseln Sie zu [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] und zum Projekt der Übermittlungserweiterung, und legen Sie einige Breakpoints im Code fest.  
  
5.  Während das Projekt mit der Übermittlungserweiterung noch immer im aktiven Fenster geöffnet ist, klicken Sie im Menü **Debuggen** auf **An den Prozess anhängen**.  
  
     Das Dialogfeld **An den Prozess anhängen** wird geöffnet.  
  
6.  Wählen Sie aus der Liste der Prozesse „aspnet_wp.exe“ aus (oder „w3wp.exe“, wenn Ihre Anwendung unter IIS 6.0 läuft), und klicken Sie auf **Anfügen**.  
  
7.  Definieren Sie mithilfe der Übermittlungserweiterung ein neues Abonnement. Höchstwahrscheinlich verwenden Sie den Berichts-Manager oder die SOAP-API. Dadurch sollte der Debugger aufgerufen und Code den Breakpoints gemäß ausgeführt werden.  
  
8.  Gehen Sie den Code schrittweise mit der Taste **F11** durch. Weitere Informationen zum Debuggen mit [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] finden Sie in der Dokumentation zu [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)].  
  
## <a name="see-also"></a>Weitere Informationen  
 [Implementieren von Übermittlungserweiterungen](../../../reporting-services/extensions/delivery-extension/implementing-a-delivery-extension.md)   
 [Reporting Services Extension Library (Reporting Services-Erweiterungsbibliothek)](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
