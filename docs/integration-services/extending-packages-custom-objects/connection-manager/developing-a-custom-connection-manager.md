---
description: Entwickeln eines benutzerdefinierten Verbindungs-Managers
title: Entwickeln eines benutzerdefinierten Verbindungs-Managers | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- packages [Integration Services], connections
- custom connection managers [Integration Services], about custom connection managers
- connection managers [Integration Services], custom
- Integration Services packages, connection managers
- SSIS packages, connection managers
- SQL Server Integration Services packages, connection managers
- custom connection managers [Integration Services]
ms.assetid: bda0b29e-57f5-4879-b04d-1396dc56daa8
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d92ee03836944fd3a98f89c86369d2c442ee6ea5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348925"
---
# <a name="developing-a-custom-connection-manager"></a>Entwickeln eines benutzerdefinierten Verbindungs-Managers

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] verwendet Verbindungs-Manager, um die erforderlichen Informationen für das Herstellen einer Verbindung mit einer externen Datenquelle zu kapseln. [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] beinhaltet verschiedene Verbindungs-Manager, die Verbindungen mit den gebräuchlichsten Datenquellen unterstützen, von Unternehmensdatenbanken bis hin zu Textdateien und Excel-Arbeitsblättern. Wenn die Verbindungs-Manager und von [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] unterstützten externen Datenquellen Ihre Anforderungen nicht vollständig erfüllen, können Sie einen benutzerdefinierten Verbindungs-Manager erstellen.  
  
 Zum Erstellen eines benutzerdefinierten Verbindungs-Managers müssen Sie eine Klasse erstellen, die von der <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase>-Basisklasse erbt, das <xref:Microsoft.SqlServer.Dts.Runtime.DtsConnectionAttribute>-Attribut auf die neue Klasse anwenden und die Hauptmethoden und -eigenschaften der Basisklasse, einschließlich der <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.ConnectionString%2A>-Eigenschaft und der <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.AcquireConnection%2A>-Methode, überschreiben.  
  
> [!IMPORTANT]  
>  Die meisten Tasks, Quellen und Ziele, die in [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] integriert wurden, können nur mit bestimmten Typen integrierter Verbindungs-Manager verwendet werden. Überprüfen Sie vor der Entwicklung eines benutzerdefinierten Verbindungs-Managers für die Verwendung bei integrierten Tasks und Komponenten, ob die Liste verfügbarer Verbindungs-Manager durch diese Komponenten auf einen bestimmten Typ begrenzt wird. Wenn Ihre Lösung einen benutzerdefinierten Verbindungs-Manager erfordert, müssen Sie möglicherweise auch einen benutzerdefinierten Task entwickeln oder eine benutzerdefinierte Quelle oder Ziel zur Verwendung mit dem Verbindungs-Manager.  
  
## <a name="in-this-section"></a>In diesem Abschnitt  
 In diesem Abschnitt wird beschrieben, wie Sie einen benutzerdefinierten Verbindungs-Manager und dessen optionale benutzerdefinierte Benutzeroberfläche erstellen, konfigurieren und codieren. Die in diesem Abschnitt gezeigten Codeausschnitte stammen aus dem Sql Server Custom Connection Manager-Beispiel.  
  
 [Erstellen eines benutzerdefinierten Verbindungs-Managers](../../../integration-services/extending-packages-custom-objects/connection-manager/creating-a-custom-connection-manager.md)  
 Beschreibt die Erstellung der Klassen für ein benutzerdefiniertes Verbindungs-Manager-Projekt.  
  
 [Codieren eines benutzerdefinierten Verbindungs-Managers](../../../integration-services/extending-packages-custom-objects/connection-manager/coding-a-custom-connection-manager.md)  
 Beschreibt die Implementierung eines benutzerdefinierten Verbindungs-Managers durch Überschreiben der Methoden und Eigenschaften der Basisklasse.  
  
 [Entwickeln einer Benutzeroberfläche für einen benutzerdefinierten Verbindungs-Manager](../../../integration-services/extending-packages-custom-objects/connection-manager/developing-a-user-interface-for-a-custom-connection-manager.md)  
 Beschreibt die Implementierung der Benutzeroberflächenklasse und des Formulars, das für die Konfiguration des benutzerdefinierten Verbindungs-Managers verwendet wird.  
  
## <a name="related-sections"></a>Verwandte Abschnitte  
  
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
  
 [Entwickeln eines benutzerdefinierten Protokollanbieters](../../../integration-services/extending-packages-custom-objects/log-provider/developing-a-custom-log-provider.md)  
 Erläutert die Programmierung benutzerdefinierter Protokollanbieter.  
  
 [Entwickeln eines benutzerdefinierten ForEach-Enumerators](../../../integration-services/extending-packages-custom-objects/foreach-enumerator/developing-a-custom-foreach-enumerator.md)  
 Erläutert die Programmierung benutzerdefinierter Enumeratoren.  
  
 [Entwickeln einer benutzerdefinierten Datenflusskomponente](../../../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md)  
 Erläutert die Programmierung benutzerdefinierter Datenflussquellen, Transformationen und Ziele.  
  
