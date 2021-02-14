---
description: Bearbeiten eines Modellbereitstellungspakets
title: Bearbeiten eines Modellbereitstellungspakets
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 6b0fdb7d-83dd-4392-9011-4ae642c471f1
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: e1ad7283c99b87b56d2fe509dbbcc8fe3e4e9497
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341778"
---
# <a name="edit-a-model-deployment-package"></a>Bearbeiten eines Modellbereitstellungspakets

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  In diesem Thema wird beschrieben, wie ausgewählte Teile eines Modells statt eines ganzen Modells in MDS bereitgestellt werden. Hierzu bearbeiten Sie mit dem Modellpaketeditor ein MDS-Modellpaket.  
  
 Der Modellpaketeditor-Assistent ermöglicht es Ihnen, bestimmte Entitäten, abgeleitete Hierarchien, Abonnementsichten und Geschäftsregeln in einem Modell auszuwählen, die Sie in ein MDS-Paket einschließen und dann später bereitstellen möchten. Sie können die Teile des Modells auslassen, die Sie nicht bereitstellen wollen. Wenn Sie eine Entität auswählen, werden alle abhängigen Objekte in dieser Entität automatisch auch ausgewählt.  
  
 Sie wählen mithilfe des Modellpaketeditors Teile eines Modells in einer Paketdatei, die entweder vom Tool MDSModelDeploy (welches eine Paketdatei erstellt, die Objekte und Daten umfasst) oder dem Modellbereitstellungs-Assistenten (welcher eine Datei erstellt, die nur die Modellstruktur enthält) erstellt wurde, aus. Nachdem Sie das Modell im Paket bearbeitet haben, verwenden Sie das Tool MDSModelDeploy, um Objekte und Daten bereitzustellen, oder den Modellbereitstellungs-Assistent, um nur die Modellstruktur bereitzustellen.  
  
## <a name="prerequisites"></a>Voraussetzungen  
 So führen Sie diese Prozedur aus  
  
-   Sie müssen ein Modelladministrator sein. Weitere Informationen finden Sie unter [Administratoren &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
-   Es muss ein Modellpaket vorhanden sein, das Sie bearbeiten können. Weitere Informationen finden Sie unter [Bereitstellen von Modellen &#40;Master Data Services&#41;](../master-data-services/deploying-models-master-data-services.md) und [Erstellen eines Modellbereitstellungspakets mithilfe des Assistenten](../master-data-services/create-a-model-deployment-package-by-using-the-wizard.md) oder [Erstellen eines Modellbereitstellungspakets mit MDSModelDeploy](../master-data-services/create-a-model-deployment-package-by-using-mdsmodeldeploy.md).  
  
### <a name="to-edit-a-model-deployment-package"></a>So bearbeiten Sie ein Modellbereitstellungspaket  
  
1.  Wechseln Sie im Windows-Explorer auf dem MDS-Server zu *Laufwerk*:\Programme\Microsoft SQL Server\130\Master Data Services\Configuration.  
  
2.  Führen Sie ModelPackageEditor.exe aus.  
  
3.  Klicken Sie im Modellpaketeditor-Assistenten auf **Durchsuchen**, wechseln Sie zu dem Ordner, der die Pakete enthält, wählen Sie ein Paket aus, und klicken Sie dann auf **Öffnen**. Klicken Sie auf **Weiter**.  
  
4.  Wählen Sie die Entitäten, abgeleiteten Hierarchien, Abonnementsichten oder Geschäftsregeln, die Sie bereitstellen möchten, aus. Heben Sie die Auswahl für die auf, die Sie nicht bereitstellen wollen. Klicken Sie auf **Weiter**.  
  
5.  Überprüfen Sie die bereitzustellende Auswahlliste. Um Änderungen vorzunehmen, klicken Sie auf **Zurück** und wiederholen Schritt 4.  
  
6.  Klicken Sie auf **Durchsuchen**, wechseln Sie in den Ordner, in dem Sie das Teilpaket speichern möchten, und geben Sie anschließend den Dateinamen des Teilpakets ein (mit der Erweiterung „.pkg“). Klicken Sie auf **Speichern**.  
  
7.  Klicken Sie auf **Fertig stellen**.  
  
## <a name="next-steps"></a>Nächste Schritte  
  
-   [Bereitstellen eines Modellbereitstellungspakets mithilfe des Assistenten](../master-data-services/deploy-a-model-deployment-package-by-using-the-wizard.md)  
  
-   [Bereitstellen eines Modellbereitstellungspakets mit MDSModelDeploy](../master-data-services/deploy-a-model-deployment-package-by-using-mdsmodeldeploy.md)  
  
  
