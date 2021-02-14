---
title: Stagingtabelle für Blattelemente
description: Verwenden Sie die Stagingtabelle für Blatt Elemente in der Master Data Services-Datenbank, um Blatt Elemente zu erstellen, zu aktualisieren, zu deaktivieren und zu löschen.
ms.custom: ''
ms.date: 04/01/2016
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- members staging table [Master Data Services]
- database [Master Data Services], members staging table
ms.assetid: a8c953da-ec20-47dc-8656-ed5f0dfed89b
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: e38a38d14551b819db0e6ff7f8558448040a7049
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338231"
---
# <a name="leaf-member-staging-table-master-data-services"></a>Stagingtabelle für Blattelemente (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Verwenden Sie die Stagingtabelle für Blattelemente (stg.name_Leaf) in der [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]-Datenbank, um Blattelemente zu erstellen, zu aktualisieren, zu deaktivieren und zu löschen. Sie können sie auch zum Aktualisieren von Attributwerten für Blattelemente verwenden.  
  
##  <a name="table-columns"></a><a name="TableColumns"></a> Tabellen Spalten  
 Die folgende Tabelle erklärt, wofür jedes der Felder in der Blattstagingtabelle verwendet wird.  
  
|Spaltenname|Beschreibung|Werte|  
|-----------------|-----------------|------------|  
|**ID**|Ein automatisch zugewiesener Bezeichner.|Geben Sie in diesem Feld keinen Wert ein. Wenn der Batch nicht verarbeitet wurde, ist dieses Feld leer.|  
|**ImportType**<br /><br /> Erforderlich|Bestimmt, wie verfahren wird, wenn bereitgestellte Daten Daten entsprechen, die bereits in der Datenbank [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] vorhanden sind.|**0**: Erstellen neuer Elemente. Ersetzen von MDS-Daten durch bereitgestellte Daten, aber nur, wenn die bereitgestellten Daten ungleich NULL sind. NULL-Werte werden ignoriert. Um den Wert eines string-Attributs in NULL zu ändern, legen Sie ihn auf **~NULL~** fest. Um den Wert eines number-Attributs in NULL zu ändern, legen Sie ihn auf **-98765432101234567890** fest. Um den Wert eines datetime-Attributs in NULL zu ändern, legen Sie ihn auf **5555-11-22T12:34:56** fest.<br /><br /> **1**: Ausschließliches Erstellen neuer Elemente. Fehler bei Updates vorhandener MDS-Daten.<br /><br /> **2**: Erstellen neuer Elemente. Ersetzen von MDS-Daten durch bereitgestellte Daten. Wenn Sie NULL-Werte importieren, überschreiben diese vorhandene MDS-Werte.<br /><br /> **3**: Deaktivieren des Elements basierend auf dem Codewert. Alle Attribute, Hierarchie- und Auflistungsmitgliedschaften sowie Transaktionen werden beibehalten, sind aber nicht mehr auf der Benutzeroberfläche verfügbar. Wenn das Element als domänenbasierter Attributwert eines anderen Elements verwendet wird, tritt ein Fehler beim Deaktivieren auf. Eine Alternative finden Sie in **ImportType5**.<br /><br /> **4**: Dauerhaftes Löschen des Elements basierend auf dem Codewert. Alle Attribute, Hierarchie- und Auflistungsmitgliedschaften sowie Transaktionen werden dauerhaft gelöscht. Wenn das Element als domänenbasierter Attributwert eines anderen Elements verwendet wird, tritt ein Fehler beim Löschen auf. Eine Alternative finden Sie in **ImportType6**.<br /><br /> **5**: Deaktivieren des Elements basierend auf dem **Code** wert. Alle Attribute, Hierarchie- und Auflistungsmitgliedschaften sowie Transaktionen werden beibehalten, sind aber nicht mehr auf der Benutzeroberfläche verfügbar. Wenn das Element als domänenbasierter Attributwert anderer Elemente verwendet wird, werden die verknüpften Werte auf NULL festgelegt. ImportType 5 ist nur für Blattelemente vorgesehen.<br /><br /> **6**: Dauerhaftes Löschen des Elements basierend auf dem **Code** wert. Alle Attribute, Hierarchie- und Auflistungsmitgliedschaften sowie Transaktionen werden dauerhaft gelöscht. Wenn das Element als domänenbasierter Attributwert anderer Elemente verwendet wird, werden die verknüpften Werte auf NULL festgelegt. ImportType 6 ist nur für Blattelemente vorgesehen.|  
|**ImportStatus_ID**<br /><br /> Erforderlich|Der Status des Importvorgangs.|**0** geben Sie an, um anzuzeigen, dass der Datensatz für den Stagingprozess bereit ist.<br /><br /> **1**: wird automatisch zugewiesen und gibt an, dass der Stagingprozess für den Datensatz erfolgreich war.<br /><br /> **2**: wird automatisch zugewiesen, und gibt an, dass der Stagingprozess für den Datensatz nicht erfolgreich war.|  
|**Batch_ID**<br /><br /> Wird nur vom Webdienst benötigt|Ein automatisch zugewiesener Bezeichner, der Datensätze für das Staging gruppiert. Alle Elemente im Batch werden diesem Bezeichner zugewiesen, der in der [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] -Benutzeroberfläche in der **ID** -Spalte angezeigt wird.|Wenn der Batch nicht verarbeitet wurde, ist dieses Feld leer.|  
|**BatchTag**<br /><br /> Erforderlich, außer vom Webdienst|Ein eindeutiger Name für den Batch (bis zu 50 Zeichen).||  
|**ErrorCode**|Zeigt einen Fehlercode an. Informationen zu Datensätzen mit einer **ImportStatus_ID** von **2**, finden Sie unter [Fehler des Stagingprozesses &#40;Master Data Services&#41;](../master-data-services/staging-process-errors-master-data-services.md).||  
|**Code**<br /><br /> Erforderlich, es sei denn, Codes werden automatisch für **ImportType1** oder **2** generiert. Weitere Informationen finden Sie unter [Automatische Codeerstellung &#40;Master Data Services&#41;](../master-data-services/automatic-code-creation-master-data-services.md).|Ein eindeutiger Code für das Element.||  
|**Name**<br /><br /> Optional|Ein Name für das Element.||  
|**NewCode**|Nur verwenden, wenn Sie den Elementcode ändern.||  
|\<Attribute name>|Für jedes Attribut in der Entität ist eine Spalte vorhanden. Verwenden Sie dieses mit einem **ImportType** von **0** oder **2**. Geben Sie für Freiformattribute den neuen Text oder Zeichenfolgenwert für das Attribut an. Für domänenbasierte Attribute geben Sie den Code für das Element an, das als Attribut verwendet wird. Bei Linkattributen muss die URL mit **https://** beginnen.<br /><br /> Hinweis: Sie können keine Dateiattribute bereitstellen.||  
  
## <a name="see-also"></a>Weitere Informationen  
 [Übersicht: Importieren von Daten aus Tabellen &#40;Master Data Services&#41;](../master-data-services/overview-importing-data-from-tables-master-data-services.md)   
 [Anzeigen von Fehlern, die während des Stagings &#40;Master Data Services auftreten&#41;](../master-data-services/view-errors-that-occur-during-staging-master-data-services.md)   
 [Fehler des Stagingprozesses &#40;Master Data Services&#41;](../master-data-services/staging-process-errors-master-data-services.md)  
  
  
