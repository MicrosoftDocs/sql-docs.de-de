---
title: Methoden zur Berichtsserver-Namespaceverwaltung | Microsoft-Dokumentation
description: Der Webdienst zur Berichtsserververwaltung enthält Methoden, mit denen Sie Berichte, Ordner und Ressourcen in der Berichtsserver-Datenbank verwalten können.
ms.date: 03/06/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service
ms.topic: reference
helpviewer_keywords:
- reports [Reporting Services], managing
- management methods [Reporting Services]
- methods [Reporting Services], about methods
- methods [Reporting Services]
ms.assetid: 2aa43ce9-f51e-408a-8ce0-b40d3dd62561
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: fab863f46d1b5707359c64577e6cffde4a6e2ea2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063336"
---
# <a name="report-server-namespace-management-methods"></a>Methoden zur Berichtsserver-Namespaceverwaltung
  Der Webdienst zur Berichtsserververwaltung enthält Methoden, mit denen Sie Berichte, Ordner und Ressourcen in der Berichtsserver-Datenbank verwalten können.  
  
|Methode|Action|  
|------------|------------|  
|<xref:ReportService2010.ReportingService2010.CancelJob%2A>|Bricht die Ausführung eines Auftrags ab|  
|<xref:ReportService2010.ReportingService2010.CreateFolder%2A>|Fügt der Berichtsserver-Datenbank oder SharePoint-Bibliothek einen Ordner hinzu.|  
|<xref:ReportService2010.ReportingService2010.CreateCatalogItem%2A>|Fügt einer Berichtsserver-Datenbank oder einer SharePoint-Bibliothek ein neues Element hinzu. Diese Methode gilt für die Elementtypen **Report**, **Model**, **Dataset**, **Component**, **Resource** und **DataSource**.|  
|M:ReportService2010.ReportingService2010.CreateReportEditSession(System.String, System.String,System.Byte[],ReportService2010.Warning[]@)|Erstellt eine neue Berichtsbearbeitungssitzung.|  
|<xref:ReportService2010.ReportingService2010.DeleteItem%2A>|Entfernt ein Element aus der Berichtsserver-Datenbank oder SharePoint-Bibliothek.|  
|<xref:ReportService2010.ReportingService2010.FindItems%2A>|Gibt die Elemente in der Berichtsserver-Datenbank oder SharePoint-Bibliothek zurück, die den angegebenen Suchkriterien entsprechen.|  
|<xref:ReportService2010.ReportingService2010.FireEvent%2A>|Löst ein Ereignis auf der Basis der angegebenen Parameter aus|  
|<xref:ReportService2010.ReportingService2010.GetExtensionSettings%2A>|Gibt eine Liste der Einstellungen für eine angegebene Erweiterung zurück.|  
|<xref:ReportService2010.ReportingService2010.GetItemType%2A>|Ruft den Typ eines Elements in der Berichtsserver-Datenbank oder SharePoint-Bibliothek ab, wenn das Element vorhanden ist.|  
|<xref:ReportService2010.ReportingService2010.GetProperties%2A>|Gibt die Werte einer oder mehrerer Eigenschaften eines Elements in der Berichtsserver-Datenbank oder SharePoint-Bibliothek zurück.|  
|<xref:ReportService2010.ReportingService2010.GetItemDefinition%2A>|Ruft die Definition oder den Inhalt für ein Element ab. Diese Methode gilt für die Elementtypen **Report**, **Model**, **Dataset**, **Component**, **Resource** und **DataSource**.|  
|<xref:ReportService2010.ReportingService2010.GetItemReferences%2A>|Gibt eine Liste der einem Element zugeordneten Katalogelementverweise zurück.|  
|<xref:ReportService2010.ReportingService2010.GetReportServerConfigInfo%2A>|Gibt Informationen zur verbundenen Berichtsserverinstanz oder allen Berichtsserverinstanzen in einer Bereitstellung für horizontales Skalieren zurück.|  
|<xref:ReportService2010.ReportingService2010.GetSystemProperties%2A>|Gibt eine oder mehrere Systemeigenschaften zurück.|  
|<xref:ReportService2010.ReportingService2010.ListChildren%2A>|Ruft eine Liste der untergeordneten Elemente eines angegebenen Ordners ab|  
|<xref:ReportService2010.ReportingService2010.ListDatabaseCredentialRetrievalOptions%2A>|Gibt eine Liste unterstützter Optionen zum Abrufen von Anmeldeinformationen zurück.|  
|<xref:ReportService2010.ReportingService2010.ListEvents%2A>|Gibt eine Liste von Ereigniserweiterungen zurück, wie sie in der Berichtsserver-Konfigurationsdatei angezeigt werden.|  
|<xref:ReportService2010.ReportingService2010.ListJobs%2A>|Gibt eine Liste von Aufträgen zurück, die auf dem Berichtsserver ausgeführt werden|  
|<xref:ReportService2010.ReportingService2010.ListExtensions%2A>|Gibt eine Liste von Erweiterungen zurück, die für einen bestimmten Erweiterungstyp konfiguriert werden.|  
|<xref:ReportService2010.ReportingService2010.ListExtensionTypes%2A>|Gibt eine Liste unterstützter Erweiterungstypen zurück.|  
|<xref:ReportService2010.ReportingService2010.ListItemTypes%2A>|Gibt eine Liste unterstützter Katalogelementtypen zurück.|  
|<xref:ReportService2010.ReportingService2010.ListJobActions%2A>|Gibt eine Liste unterstützter Auftragsaktionen zurück.|  
|<xref:ReportService2010.ReportingService2010.ListJobStates%2A>|Gibt eine Liste unterstützter Auftragszustände zurück.|  
|<xref:ReportService2010.ReportingService2010.ListJobTypes%2A>|Gibt eine Liste unterstützter Auftragstypen zurück.|  
|<xref:ReportService2010.ReportingService2010.ListParents%2A>|Ruft übergeordnete Elemente für das angegebene Element ab.|  
|<xref:ReportService2010.ReportingService2010.ListSecurityScopes%2A>|Gibt eine Liste unterstützter Sicherheitsbereiche zurück.|  
|<xref:ReportService2010.ReportingService2010.Logoff%2A>|Meldet den aktuellen Benutzer ab, der Webdienstanforderungen ausführt. Diese Methode gilt nur für den einheitlichen Modus.|  
|<xref:ReportService2010.ReportingService2010.LogonUser%2A>|Meldet einen Benutzer an und authentifiziert eine Benutzeranforderung an den Berichtsserver-Webdienst. Diese Methode gilt nur für den einheitlichen Modus.|  
|<xref:ReportService2010.ReportingService2010.SetItemReferences%2A>|Legt die einem Element zugeordneten Katalogelemente fest.|  
|<xref:ReportService2010.ReportingService2010.MoveItem%2A>|Verschiebt ein Element und/oder benennt es um.|  
|<xref:ReportService2010.ReportingService2010.SetProperties%2A>|Legt mindestens eine Eigenschaft für ein Element fest.|  
|<xref:ReportService2010.ReportingService2010.SetItemDefinition%2A>|Legt die Definition oder den Inhalt für ein angegebenes Element fest. Diese Methode gilt für die Elementtypen **Report**, **Model**, **Dataset**, **Component**, **Resource** und **DataSource**.|  
|<xref:ReportService2010.ReportingService2010.SetSystemProperties%2A>|Legt mindestens eine Systemeigenschaft auf dem Berichtsserver oder in der SharePoint-Farm fest.|  
|<xref:ReportService2010.ReportingService2010.ValidateExtensionSettings%2A>|Überprüft [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]-Erweiterungseinstellungen.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Erstellen von Anwendungen mit dem Webdienst und .NET Framework](../../../reporting-services/report-server-web-service/net-framework/building-applications-using-the-web-service-and-the-net-framework.md)   
 [Berichtsserver-Webdienst](../../../reporting-services/report-server-web-service/report-server-web-service.md)   
 [Webdienstmethoden für Berichtsserver](../../../reporting-services/report-server-web-service/methods/report-server-web-service-methods.md)   
 [Technische Referenz (SSRS)](../../../reporting-services/technical-reference-ssrs.md)  
  
  
