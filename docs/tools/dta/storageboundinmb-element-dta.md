---
title: StorageBoundInMB-Element (DTA)
description: Im Hilfsprogramm „DTA“ gibt das Element „StorageBoundInMB“ die maximale Größe des Speicherplatzes an, der von der Optimierungsempfehlung des Datenbankoptimierungsratgebers beansprucht werden kann.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
dev_langs:
- XML
helpviewer_keywords:
- StorageBoundInMB element
ms.assetid: a8374910-bf68-4edb-b464-53a3a705e7f4
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 0343b4854611e4645cce007036aec28175f7f9dc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349475"
---
# <a name="storageboundinmb-element-dta"></a>StorageBoundInMB-Element (DTA)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Gibt die maximale Größe des Speicherplatzes in Megabyte an, der von der Optimierungsempfehlung des Datenbankoptimierungsratgebers (Index und Partitionierung) beansprucht werden kann.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
<DTAInput>  
...code removed...  
    <TuningOptions>  
      <StorageBoundInMB>...</ StorageBoundInMB >  
```  
  
## <a name="element-characteristics"></a>Elementmerkmale  
  
|Merkmal|BESCHREIBUNG|  
|--------------------|-----------------|  
|**Datentyp und -länge**|**unsignedInt**, unbegrenzte Länge.|  
|**Standardwert**|Keine.|  
|**Vorkommen**|Optional. Kann nur einmalig für das **TuningOptions** -Element verwendet werden.|  
  
## <a name="element-relationships"></a>Elementbeziehungen  
  
|Beziehung|Elemente|  
|------------------|--------------|  
|**Übergeordnetes Element**|[TuningOptions-Element &#40;DTA&#41;](../../tools/dta/tuningoptions-element-dta.md)|  
|**Untergeordnete Elemente**|Keine|  
  
## <a name="remarks"></a>Bemerkungen  
 Wenn mehrere Datenbanken optimiert werden, werden bei der Berechnung des Speicherplatzes Empfehlungen für alle Datenbanken berücksichtigt. Standardmäßig nimmt der Datenbankoptimierungsratgeber die kleinere der folgenden Speichergrößen an:  
  
-   Das Dreifache der aktuellen Rohdatengröße, einschließlich der Gesamtgröße von Heaps und gruppierten Indizes für Tabellen.  
  
-   Der freie Speicherplatz auf allen angefügten Laufwerken plus die Rohdatengröße.  
  
 Nicht im Standardspeicherplatz enthalten sind nicht gruppierte Indizes und indizierte Sichten.  
  
 Wenn der für das **StorageBoundInMB** -Element angegebene Wert den tatsächlichen Speicherplatz überschreitet, meldet der Datenbankoptimierungsratgeber einen Fehler, fährt aber mit der Optimierung fort. Nach der Optimierung können Sie Speicherplatz hinzufügen, wenn Sie die Empfehlung implementieren möchten.  
  
## <a name="example"></a>Beispiel  
  
## <a name="description"></a>BESCHREIBUNG  
 Im folgenden Codebeispiel wird veranschaulicht, wie 1500 Megabytes als maximaler Speicherplatz festgelegt werden, der von einer Empfehlung zur Optimierung belegt werden kann:  
  
## <a name="code"></a>Code  
  
```  
<DTAInput>  
  <Server>...</Server>  
  <Workload>...</Workload>  
  <TuningOptions>  
    <StorageBoundInMB>1500</StorageBoundInMB>  
...code removed here...  
</DTAInput>  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [XML-Eingabedateireferenz &#40;Datenbankoptimierungsratgeber&#41;](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  
