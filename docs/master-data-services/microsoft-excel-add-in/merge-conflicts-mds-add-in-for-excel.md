---
description: Konflikte zusammenführen (MDS-Add-In für Excel)
title: Konflikte zusammenführen
ms.custom: microsoft-excel-add-in
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: cf95978f-a2c5-4325-8606-dbd4e88741b8
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 3cb483ab4a9e83289bdc250fc442657a50cb44e5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343603"
---
# <a name="merge-conflicts-mds-add-in-for-excel"></a>Konflikte zusammenführen (MDS-Add-In für Excel)

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Wenn das [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] -Add-In für Excel auf dem Server von einem anderen Benutzer geändert wurde, schlägt das Veröffentlichen mit einem Konfliktfehler fehl. Um diesen Fehler zu beheben, können Sie die Konflikte zusammenführen und die Änderungen erneut veröffentlichen.  
  
## <a name="prerequisites"></a>Voraussetzungen  
 So führen Sie diese Prozedur aus  
  
-   Sie müssen über die Berechtigung für den Zugriff auf den Funktionsbereich **Explorer** verfügen.  
  
-   Sie müssen mindestens die Berechtigung "Aktualisieren" für das Blattmodellobjekt für die Entität besitzen, die Sie aktualisieren.  
  
-   Im aktiven Arbeitsblatt müssen von MDS verwaltete Daten vorhanden sein.  
  
-   Das aktive Arbeitsblatt muss einen Konfliktfehler aufweisen, nachdem Sie versucht haben, Ihre Änderungen zu veröffentlichen.  
  
### <a name="to-merge-conflicts"></a>So führen Sie Konflikte zusammen  
  
1.  Wählen Sie im Arbeitsblatt die Zeile oder Zelle, die den Konfliktfehler aufweist.  
  
2.  Wählen Sie in der Menügruppe **Veröffentlichen und überprüfen** die Option **Konflikte zusammenführen** , um das Dialogfeld **Konflikte zusammenführen** zu öffnen.  
  
3.  Im Dialogfeld **Konflikte zusammenführen** haben Sie folgende Möglichkeiten:  
  
    -   Sie können **Neueste** auswählen und auf **Anwenden** klicken, um die ausstehenden Änderungen rückgängig zu machen und die neueste Version vom Server herunterzuladen.  
  
    -   Sie können **Ursprüngliche** auswählen und auf **Anwenden** klicken, um die ursprüngliche Version auf das Arbeitsblatt anzuwenden.  
  
    -   Sie können **Ihre** auswählen und auf **Anwenden** klicken, um die vorhandenen lokalen Änderungen beizubehalten.  
  
4.  Nachdem Sie auf **Anwenden** geklickt haben, können Sie zusätzliche Änderungen vornehmen und erneut veröffentlichen. Sie können auch auf **Abbrechen** klicken, um die Aktualisierung abzubrechen und die neueste Version erneut vom Server herunterzuladen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Übersicht: Importieren von Daten aus Excel &#40;MDS-Add-In für Excel&#41;](../../master-data-services/microsoft-excel-add-in/overview-importing-data-from-excel-mds-add-in-for-excel.md)  
  
  
