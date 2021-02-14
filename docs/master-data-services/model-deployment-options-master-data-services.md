---
description: Optionen für Modellbereitstellung (Master Data Services)
title: Optionen für Modellbereitstellung
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: cf1b17b4-47d5-4eba-83f9-fb0555806867
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: eb67b14b6f0023a0b3b112718b09bb94ddbebff1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340348"
---
# <a name="model-deployment-options-master-data-services"></a>Optionen für Modellbereitstellung (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Beim Bereitstellen einer Modellpaketdatei in [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]müssen Sie sich entscheiden, ob Sie ein neues oder geklontes Modell bereitstellen oder ein Modell aktualisieren, das zuvor geklont wurde.  
  
## <a name="workflows"></a>Workflows  
 Beim Verwenden von Modellpaketen gibt es zwei primäre Workflows.  
  
-   Erstellen Sie ein Paket eines Modells in einer Testumgebung, und stellen Sie einen Klon des Modells in einer Produktionsumgebung bereit. Stellen Sie im Laufe der Zeit Updates aus der Testumgebung in der Produktionsumgebung bereit.  
  
-   Erstellen Sie ein Paket eines Modells, und stellen Sie es als neues Modell in der gleichen Umgebung bereit. In diesem Fall müssen Sie dem Modell einen neuen Namen geben.  
  
## <a name="options"></a>Tastatur  
 In der MDS-Datenbank verfügt jedes Modellobjekt über einen eindeutigen Bezeichner (ID). Diese IDs sind in Modellbereitstellungspaketen enthalten. Beim Bereitstellen des Pakets müssen Sie angeben, was mit diesen IDs geschehen soll.  
  
 Die folgende Tabelle soll als Entscheidungshilfe beim Bereitstellen eines Modells dienen, indem entweder der Modellbereitstellungs-Assistent der Systemverwaltung oder das MDSModelDeploy-Tool verwendet wird.  
  
|Option|Beschreibung|Hinweise|  
|------------|-----------------|-----------|  
|Neu|Erstellen Sie ein neues Modell mit einem eindeutigen Namen. Für alle Modellobjekte werden neue Bezeichner erstellt.|Wenn Sie ein neues Modell mit neuen Bezeichnern erstellen, können Sie keine Modellbereitstellungstools verwenden, um später Updates auf das Modell anzuwenden. Wenn Sie den Assistenten in der Webanwendung zum Bereitstellen eines Modellpakets verwenden, können Sie nur dann ein neues Modell erstellen, wenn ein Modell mit dem gleichen Namen oder der gleichen ID bereits vorhanden ist.|  
|Klon|Erstellen Sie ein neues Modell, das ein genauer Klon des Modells im Paket ist. Dies funktioniert nur, wenn das Modell (dem Namen oder Bezeichner nach) in der Zielumgebung nicht vorhanden ist. Verwenden Sie „Klonen“, wenn das gleiche Modell in mehreren Umgebungen vorhanden sein soll und Sie das geklonte Modell dann immer entsprechend aktualisieren möchten.|Dies ist das Standardverhalten des Assistenten in der Webanwendung. Falls ein Modell mit dem gleichen Namen oder der gleichen ID bereits vorhanden ist, werden Sie aufgefordert, stattdessen ein neues Modell zu erstellen.|  
|Update|Aktualisieren Sie ein vorhandenes Modell mit dem Modell im Paket. Die Bezeichner müssen in beiden Modellen gleich sein. Wird verwendet, um ein Modell zu aktualisieren, das Sie zuvor geklont haben.|Sie können nur Modelle aktualisieren, die zuvor geklont wurden. (Die Namen und IDs müssen übereinstimmen.)|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Bereitstellen eines Modell Bereitstellungs Pakets mit mdsmodeldeployment](../master-data-services/deploy-a-model-deployment-package-by-using-mdsmodeldeploy.md)   
 [Bereitstellen eines Modell Bereitstellungs Pakets mithilfe des Assistenten](../master-data-services/deploy-a-model-deployment-package-by-using-the-wizard.md)   
 [Bereitstellen von Modellen &#40;Master Data Services&#41;](../master-data-services/deploying-models-master-data-services.md)  
  
  
