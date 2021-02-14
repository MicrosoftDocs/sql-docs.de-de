---
description: Kategorisierte Webdienstvorgänge (Master Data Services)
title: Kategorisierte Webdienstvorgänge
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: reference
ms.assetid: e3f346b5-7e26-481d-9821-1846e2e91289
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 65656aa902b5500d1dcc57dde914b3a0b2265008
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350128"
---
# <a name="categorized-web-service-operations-master-data-services"></a>Kategorisierte Webdienstvorgänge (Master Data Services)

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Der [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] -Webdienst enthält einen vollständigen Satz an Vorgängen, mit dem Sie Code zum Steuern aller Funktionen schreiben können, die von [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] über die zugehörige Benutzeroberfläche ausgeführt werden. Die Webdienstvorgänge werden von der <xref:Microsoft.MasterDataServices.IService>-Schnittstelle definiert und als Methoden in der <xref:Microsoft.MasterDataServices.ServiceClient>-Klasse implementiert. Dieses Thema gruppiert die Webdienstvorgänge in konzeptionelle Kategorien, sodass Sie einen besseren Einblick in die Verwendung der Webdienst-API erhalten.  
  
## <a name="model-operations"></a>Modellvorgänge  
 Anhand dieser Vorgänge werden Modelle erstellt, aktualisiert und gelöscht und Aktionen für sämtliche Inhalte eines Modells wie Entitäten, Hierarchien und Versionen ausgeführt. Weitere Informationen finden Sie unter [Modelle &#40;Master Data Services&#41;](../../master-data-services/models-master-data-services.md).  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.MetadataClone%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.MetadataCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.MetadataDelete%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.MetadataGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.MetadataUpdate%2A>
  
## <a name="entity-operations"></a>Entitätsvorgänge  
 Anhand dieser Vorgänge werden die Elemente einer einzelnen Entität erstellt, aktualisiert und gelöscht. Weitere Informationen finden Sie unter [Entitäten (Master Data Services)](../../master-data-services/entities-master-data-services.md) und [Elemente (Master Data Services)](../../master-data-services/members-master-data-services.md).  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMemberKeyLookup%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMembersCopy%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMembersCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMembersDelete%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMembersGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMembersMerge%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMembersUpdate%2A>
  
## <a name="member-operations"></a>Elementvorgänge  
 Anhand dieser Vorgänge werden Elemente abgerufen, aktualisiert und gelöscht. Der Elementsatz, für den Vorgänge ausgeführt werden, kann Elemente von mehreren Entitäten enthalten. Weitere Informationen finden Sie unter [Elemente &#40;Master Data Services&#41;](../../master-data-services/members-master-data-services.md).  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.ModelMembersBulkDelete%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.ModelMembersBulkMerge%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.ModelMembersBulkUpdate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.ModelMembersGet%2A>
  
## <a name="attribute-and-hierarchy-operations"></a>Attribut- und Hierarchievorgänge  
 Anhand dieser Vorgänge werden Attribut- und Hierarchieinformationen abgerufen. Attribute und Hierarchien können auch mithilfe der Modellvorgänge geändert werden, z. B. <xref:Microsoft.MasterDataServices.ServiceClient.MetadataUpdate%2A>. Weitere Informationen finden Sie unter [Attribute &#40;Master Data Services&#41;](../../master-data-services/attributes-master-data-services.md) und [Hierarchien &#40;Master Data Services&#41;](../../master-data-services/hierarchies-master-data-services.md).  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMemberAttributesGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.HierarchyMembersGet%2A>
  
## <a name="business-rule-operations"></a>Geschäftsregelvorgänge  
 Anhand dieser Vorgänge werden Geschäftsregeln erstellt, aktualisiert, gelöscht und veröffentlicht. Weitere Informationen finden Sie unter [Geschäftsregeln &#40;Master Data Services&#41;](../../master-data-services/business-rules-master-data-services.md).  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.BusinessRulesClone%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.BusinessRulesCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.BusinessRulesDelete%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.BusinessRulesGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.BusinessRulesPaletteGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.BusinessRulesPublish%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.BusinessRulesUpdate%2A>
  
## <a name="annotation-operations"></a>Anmerkungsvorgänge  
 Anhand dieser Vorgänge werden Anmerkungen erstellt, aktualisiert und gelöscht. Weitere Informationen finden Sie unter [Anmerkungen &#40;Master Data Services&#41;](../../master-data-services/annotations-master-data-services.md).  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.AnnotationsDelete%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.AnnotationsUpdate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMemberAnnotationsCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMemberAnnotationsGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.TransactionAnnotationsCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.TransactionAnnotationsGet%2A>
  
## <a name="transaction-operations"></a>Transaktionsvorgänge  
 Anhand dieser Vorgänge werden Transaktionen abgerufen und umgekehrt. Weitere Informationen finden Sie unter [Transaktionen &#40;Master Data Services&#41;](../../master-data-services/transactions-master-data-services.md).  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.TransactionsGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.TransactionsReverse%2A>
  
## <a name="version-and-validation-operations"></a>Versions- und Validierungsvorgänge  
 Anhand dieser Vorgänge werden Versionen kopiert und validiert. Weitere Informationen finden Sie unter [Versionen &#40;Master Data Services&#41;](../../master-data-services/versions-master-data-services.md) und [Überprüfung &#40;Master Data Services&#41;](../../master-data-services/validation-master-data-services.md).  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.VersionCopy%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.ValidationGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.ValidationProcess%2A>
  
## <a name="data-quality-operations"></a>Datenqualitätsvorgänge  
 Anhand dieser Vorgänge werden Datenqualitätsaufgaben ausgeführt und zugehörige Ergebnisse überprüft.  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.DataQualityCleansingOperationCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.DataQualityMatchingOperationCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.DataQualityInstalledState%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.DataQualityKnowledgeBasesGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.DataQualityOperationStart%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.DataQualityOperationResultsGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.DataQualityOperationStatus%2A>
  
## <a name="data-import-operations"></a>Datenimportvorgänge  
 Anhand dieser Vorgänge werden Daten in eine [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] -Datenbank importiert. Weitere Informationen finden Sie unter [Übersicht: Importieren von Daten aus Tabellen &#40;Master Data Services&#41;](../../master-data-services/overview-importing-data-from-tables-master-data-services.md).  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityStagingClear%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityStagingGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityStagingLoad%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityStagingProcess%2A>
  
 Die folgenden Vorgänge werden verwendet, um Daten mithilfe des in [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] integrierten Stagingprozesses zu importieren. Diese Vorgänge sind nur zur Unterstützung von vorhandenen Datenbanken zu verwenden. Greifen Sie für neue Entwicklungen auf die zuvor aufgelisteten Vorgänge zurück.  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.StagingClear%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.StagingGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.StagingNameCheck%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.StagingProcess%2A>
  
## <a name="data-export-operations"></a>Datenexportvorgänge  
 Anhand dieser Vorgänge werden Daten durch die Verwendung von Abonnementsichten exportiert. Weitere Informationen finden Sie unter [Übersicht: Exportieren von Daten &#40;Master Data Services&#41;](../../master-data-services/overview-exporting-data-master-data-services.md).  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.ExportViewCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.ExportViewDelete%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.ExportViewListGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.ExportViewUpdate%2A>
  
## <a name="security-operations"></a>Sicherheitsvorgänge  
 Diese Vorgänge werden verwendet, um die Sicherheitseinstellungen zu ändern, die den Zugriff auf die [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] -Datenbank steuern. Weitere Informationen finden Sie unter [Sicherheit &#40;Master Data Services&#41;](../../master-data-services/security-master-data-services.md).  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrincipalsClone%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrincipalsCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrincipalsDelete%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrincipalsGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrincipalsUpdate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrivilegesClone%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrivilegesCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrivilegesDelete%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrivilegesGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrivilegesUpdate%2A>
  
## <a name="system-operations"></a>Systemvorgänge  
 Diese Vorgänge werden verwendet, um System- und Benutzereinstellungen abzurufen und zu aktualisieren.  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.ServiceCheck%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.ServiceVersionGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SystemDomainListGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SystemPropertiesGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SystemSettingsGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SystemSettingsUpdate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.UserPreferencesDelete%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.UserPreferencesGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.UserPreferencesUpdate%2A>
  
  
