---
title: Fehler des Stagingprozesses
description: In diesem Artikel werden Fehlercodes erläutert, die während des Stagingprozesses in Master Data Services allen verarbeiteten Datensätzen hinzugefügt wurden.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- staging process [Master Data Services], error messages
ms.assetid: 0d9be0dd-638f-4dd4-92b2-253fda655455
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 84a5a66cbb8bacb3e1df896fedfa3a051e2f4dbb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336271"
---
# <a name="staging-process-errors-master-data-services"></a>Fehler des Stagingprozesses (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Nach Abschluss des Stagingprozesses verfügen alle in den Stagingtabellen verarbeitete Datensätze über einen Werte in der Spalte ErrorCode. Diese Werte sind in der folgenden Tabelle aufgeführt.  
  
|Code|Fehler|Tritt auf, wenn/Details|Gilt für Tabelle|  
|----------|-----------|--------------------------|----------------------|  
|210001|Der gleiche Elementcode ist in der Stagingtabelle mehrmals vorhanden.|In Ihrer Stagingbatch ist der gleiche Elementcode mehrmals vorhanden. Keines der Elemente wird erstellt oder aktualisiert.|Leaf<br /><br /> Konsolidiert<br /><br /> Relationship|  
|210003|Die Attributwerte verweisen auf ein Element, das nicht vorhanden oder inaktiv ist.|Wenn Sie domänenbasierte Attribute bereitstellen, müssen Sie den Code und nicht den Namen verwenden. Gilt für **ImportType0**, **1** und **2**.|Leaf<br /><br /> Konsolidiert|  
|210006|Der Elementcode ist inaktiv.|**Importtype**  =  **1** und Sie haben einen Element Code angegeben, der nicht vorhanden ist.|Leaf<br /><br /> Konsolidiert<br /><br /> Relationship|  
|210032|Der Hierarchiename fehlt oder ist nicht gültig.|Die explizite Hierarchie wurde nicht gefunden, oder der **HierarchyName** -Wert war leer.|Konsolidiert<br /><br /> Relationship|  
|210035|Da keine Geschäftsregel zur Codegenerierung vorhanden ist, wird der **MemberCode** benötigt.|Beim Erstellen oder Aktualisieren von Elementen ist ein **MemberCode** immer erforderlich, außer wenn Sie die automatische Codegenerierung verwenden. Weitere Informationen finden Sie unter [automatische Code Erstellung &#40;Master Data Services&#41;](../master-data-services/automatic-code-creation-master-data-services.md).|Leaf<br /><br /> Konsolidiert|  
|210036|Da eine Geschäftsregel zur Codegenerierung vorhanden ist, wird der **MemberCode** nicht benötigt.|Beim Erstellen oder Aktualisieren von Elementen ist ein **MemberCode** nicht erforderlich, wenn Sie die automatische Codegenerierung verwenden. Sie können jedoch einen Code angeben, wenn Sie dies möchten. Weitere Informationen finden Sie unter [automatische Code Erstellung &#40;Master Data Services&#41;](../master-data-services/automatic-code-creation-master-data-services.md).|Leaf<br /><br /> Konsolidiert|  
|210041|„ROOT“ ist kein gültiger Elementcode.|Der Wert für die **Mitgliedschaft** enthält das Wort "root".|Leaf<br /><br /> Konsolidiert<br /><br /> Relationship|  
|210042|„MDMUNUSED“ ist kein gültiger Elementcode.|Der Wert für die **Mitgliedschaft** enthält das Wort "mdmunused".|Leaf<br /><br /> Konsolidiert<br /><br /> Relationship|  
|210052|Der MemberCode kann nicht deaktiviert werden, da er als domänenbasierter Attributwert verwendet wird.|Bei **Import Type**  =  **3** oder **4** schlägt das Staging fehl, wenn der Member als Attribut Wert für andere Elemente verwendet wird. Legen Sie den Wert entweder mithilfe von **ImportType5****6** auf NULL fest, oder ändern Sie die Werte vor dem Ausführen des Stagingprozesses.|Leaf<br /><br /> Konsolidiert|  
|300002|Der Elementcode ist nicht gültig.|Beziehungen: Entweder ist der übergeordnete oder der untergeordnete Elementcode nicht vorhanden.<br /><br /> Blatt oder konsolidiert: **Import Type**  =  **3** oder **4** und der Element Code ist nicht vorhanden.|Leaf<br /><br /> Konsolidiert<br /><br /> Relationship|  
|300004|Der Elementcode ist bereits vorhanden.|**Importtype**  =  **1** und Sie haben einen Element Code verwendet, der bereits in der Entität vorhanden ist.|Leaf<br /><br /> Konsolidiert|  
|210011|Wenn **RelationshipType****1** ist, kann der **ParentCode** kein Blattelement sein.|Stellen Sie sicher, dass der **ParentCode** -Wert ein konsolidierter Elementcode ist.|Relationship|  
|210015|Der Elementcode für eine Hierarchie und einen Batch ist in der Stagingtabelle mehrmals vorhanden.|Sie haben für eine explizite Hierarchie im gleichen Batch mehrmals den Speicherort des gleichen Elements angegeben.|Relationship|  
|210016|Die Beziehung konnte nicht erstellt werden, da sie einen Zirkelverweis verursachen würde.|Dieser Fehler tritt auf, wenn Sie versuchen, ein untergeordnetes Element als übergeordnetes Element zuzuweisen.|Relationship|  
|210046|Das Element kann kein gleichgeordnetes Element von Root sein.|Dies tritt auf, wenn **RelationshipType**  =  **2** (gleich geordnet) und entweder der **Parametercode** oder **childcode** **root** sind. Elemente können sich nicht auf der gleichen Ebene wie der Stammknoten befinden. Sie können nur untergeordnete Elemente sein.|Relationship|  
|210047|Das Element kann kein gleichgeordnetes Element von "Nicht verwendet" sein.|Dies tritt auf, wenn **RelationshipType**  =  **2** (gleich geordnet) und entweder der Parametercode oder **childcode** nicht **verwendet** werden.  Elemente können nur untergeordnete Elemente des Knotens "Nicht verwendet" sein.|Relationship|  
|210048|**ParentCode** und **ChildCode** können nicht identisch sein.|Der **ParentCode** -Wert ist identisch mit dem **ChildCode** -Wert. Diese Werte müssen unterschiedlich sein.|Relationship|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Anzeigen von Fehlern, die während des Stagings &#40;Master Data Services auftreten&#41;](../master-data-services/view-errors-that-occur-during-staging-master-data-services.md)   
 [Übersicht: Importieren von Daten aus Tabellen &#40;Master Data Services&#41;](../master-data-services/overview-importing-data-from-tables-master-data-services.md)  
  
  
