---
description: catalog.configure_catalog (SSISDB-Datenbank)
title: catalog.configure_catalog (SSISDB-Datenbank) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 72690c61-f462-4c25-9fce-08a687b0bd41
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 5d52f2ea964433863f735fff505b1d80bb2e3fc8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346848"
---
# <a name="catalogconfigure_catalog-ssisdb-database"></a>catalog.configure_catalog (SSISDB-Datenbank)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Konfiguriert den [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Katalog durch das Festlegen einer Katalogeigenschaft auf einen angegebenen Wert.  
  
## <a name="syntax"></a>Syntax  
  
```sql
catalog.configure_catalog [ @property_name = ] property_name , [ @property_value = ] property_value  
```  
  
## <a name="arguments"></a>Argumente  
 [ @property_name = ] *property_name*  
 Der Name der Katalogeigenschaft. Das Argument *property_name* ist vom Typ **nvarchar(255)**. Weitere Informationen zu verfügbaren Eigenschaften siehe [catalog.catalog_properties &#40;SSISDB Database&#41;](../../integration-services/system-views/catalog-catalog-properties-ssisdb-database.md) (catalog.catalog_properties &#40;SSISDB-Datenbank&#41;).  
  
 [ @property_value = ] *property_value*  
 Der Wert der Katalogeigenschaft. Das Argument *property_value* ist vom Typ **nvarchar(255)**. Weitere Informationen zu Eigenschaftswerten siehe [catalog.catalog_properties &#40;SSISDB Database&#41;](../../integration-services/system-views/catalog-catalog-properties-ssisdb-database.md) (catalog.catalog_properties &#40;SSISDB-Datenbank&#41;).  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 „0“ (erfolgreich) oder „1“ (fehlerhaft)  
  
## <a name="result-sets"></a>Resultsets  
 Keine  
  
## <a name="remarks"></a>Bemerkungen  
 Diese gespeicherte Prozedur bestimmt, ob der *property_value* für jeden *property_name* gültig ist.  
  
 Diese gespeicherte Prozedur kann nur ausgeführt werden, wenn keine aktiven Ausführungen, z. B. ausstehende, in der Warteschlange stehende, in Ausführung befindliche oder angehaltene Ausführungen, vorhanden sind.  
  
 Während der Katalog konfiguriert wird, wird bei dem Versuch, andere im Katalog gespeicherte Prozeduren auszuführen, die Fehlermeldung „Server wird gerade konfiguriert“ angezeigt.
  
 Wenn der Katalog konfiguriert ist, wird ein Eintrag in das Vorgangsprotokoll geschrieben.  
  
## <a name="permissions"></a>Berechtigungen  
 Diese gespeicherte Prozedur erfordert eine der folgenden Berechtigungen:  
  
-   Mitgliedschaft in der Datenbankrolle **ssis_admin**  
  
-   Mitgliedschaft in der Serverrolle **sysadmin**  
  
## <a name="errors-and-warnings"></a>Fehler und Warnungen  
 In der folgenden Liste werden einige Bedingungen beschrieben, die möglicherweise einen Fehler oder eine Warnung auslösen:  
  
-   Die Eigenschaft ist nicht vorhanden.  
  
-   Der Eigenschaftswert ist ungültig.  
  
  
