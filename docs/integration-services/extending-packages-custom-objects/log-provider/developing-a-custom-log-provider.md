---
description: Entwickeln eines benutzerdefinierten Protokollanbieters
title: Entwickeln eines benutzerdefinierten Protokollanbieters | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- SSIS packages, log providers
- custom log providers [Integration Services]
- SQL Server Integration Services packages, log providers
- log providers [Integration Services]
- packages [Integration Services], logs
- Integration Services packages, log providers
ms.assetid: 3f715b95-7074-4f5c-8ae2-246998052e78
author: chugugrace
ms.author: chugu
ms.openlocfilehash: ed384b5c9c83a1233167ff3c1caeaf3b37fb8070
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354344"
---
# <a name="developing-a-custom-log-provider"></a>Entwickeln eines benutzerdefinierten Protokollanbieters

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] bietet umfassende Protokollierungsfunktionen, mit denen Sie bei der Paketausführung auftretende Ereignisse erfassen können. [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] beinhaltet eine Palette von Protokollanbietern, über die Protokolle erstellt und in Formaten wie XML, in Textform, in Datenbanken oder im Windows-Ereignisprotokoll gespeichert werden können. Sollten die Protokollanbieter und die angebotenen Ausgabeformate Ihre Anforderungen nicht vollständig erfüllen, können Sie benutzerdefinierte Protokollanbieter erstellen.  
  
 Zum Erstellen eines benutzerdefinierten Protokollanbieters müssen Sie eine Klasse erstellen, die von der <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase>-Basisklasse erbt, das <xref:Microsoft.SqlServer.Dts.Runtime.DtsLogProviderAttribute>-Attribut auf die neue Klasse anwenden und die Hauptmethoden und -eigenschaften der Basisklasse, einschließlich der <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.ConfigString%2A>-Eigenschaft und der <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Log%2A>-Methode, überschreibt.  
  
## <a name="in-this-section"></a>In diesem Abschnitt  
 In diesem Abschnitt wird beschrieben, wie Sie einen benutzerdefinierten Protokollanbieter erstellen, konfigurieren und den entsprechenden Code schreiben.  
  
 [Erstellen eines benutzerdefinierten Protokollanbieters](../../../integration-services/extending-packages-custom-objects/log-provider/creating-a-custom-log-provider.md)  
 Beschreibt die Erstellung der Klassen für ein benutzerdefiniertes Protokollanbieterprojekt.  
  
 [Codieren eines benutzerdefinierten Protokollanbieters](../../../integration-services/extending-packages-custom-objects/log-provider/coding-a-custom-log-provider.md)  
 Beschreibt die Implementierung eines benutzerdefinierten Protokollanbieters durch Überschreiben der Methoden und Eigenschaften der Basisklasse.  
  
 [Entwickeln einer Benutzeroberfläche für einen benutzerdefinierten Protokollanbieter](../../../integration-services/extending-packages-custom-objects/log-provider/developing-a-user-interface-for-a-custom-log-provider.md)  
 Benutzerdefinierte Benutzeroberflächen für benutzerdefinierte Protokollanbieter werden in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] nicht unterstützt.  
  
## <a name="related-topics"></a>Verwandte Themen  
  
### <a name="information-common-to-all-custom-objects"></a>Informationen, die für alle benutzerdefinierten Objekte gelten  
 Informationen zu allen Arten benutzerdefinierter Objekte, die Sie in [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] erstellen können, finden Sie in den folgenden Themen:  
  
 [Entwickeln benutzerdefinierter Objekte für Integration Services](../../../integration-services/extending-packages-custom-objects/developing-custom-objects-for-integration-services.md)  
 Beschreibt die grundlegenden Schritte bei der Implementierung aller Typen von benutzerdefinierten Objekten in [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)].  
  
 [Beibehalten von benutzerdefinierten Objekten](../../../integration-services/extending-packages-custom-objects/persisting-custom-objects.md)  
 Beschreibt die benutzerdefinierte Persistenz und erklärt, wann diese notwendig ist.  
  
 [Erstellen, Bereitstellen und Debuggen von benutzerdefinierten Objekten](../../../integration-services/extending-packages-custom-objects/building-deploying-and-debugging-custom-objects.md)  
 Beschreibt die Techniken für das Erstellen, Signieren, Bereitstellen und Debuggen von benutzerdefinierten Objekten.  
  
### <a name="information-about-other-custom-objects"></a>Informationen zu anderen benutzerdefinierten Objekten  
 Informationen zu den anderen Typen benutzerdefinierter Objekte, die Sie in [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] erstellen können, finden Sie in den folgenden Themen:  
  
 [Entwickeln eines benutzerdefinierten Tasks](../../../integration-services/extending-packages-custom-objects/task/developing-a-custom-task.md)  
 Erläutert die Programmierung benutzerdefinierter Tasks.  
  
 [Entwickeln eines benutzerdefinierten Verbindungs-Managers](../../../integration-services/extending-packages-custom-objects/connection-manager/developing-a-custom-connection-manager.md)  
 Erläutert die Programmierung benutzerdefinierter Verbindungs-Manager.  
  
 [Entwickeln eines benutzerdefinierten ForEach-Enumerators](../../../integration-services/extending-packages-custom-objects/foreach-enumerator/developing-a-custom-foreach-enumerator.md)  
 Erläutert die Programmierung benutzerdefinierter Enumeratoren.  
  
 [Entwickeln einer benutzerdefinierten Datenflusskomponente](../../../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md)  
 Erläutert die Programmierung benutzerdefinierter Datenflussquellen, Transformationen und Ziele.  
  
