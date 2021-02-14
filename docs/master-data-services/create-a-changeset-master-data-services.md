---
description: Erstellen eines Changesets (Master Data Services)
title: Erstellen eines Changesets
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: cfad6f1c-9125-4896-b5f5-a4b9f9593cc4
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 7aa771612709a47f5dce24038af6307dc41d95e7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272731"
---
# <a name="create-a-changeset-master-data-services"></a>Erstellen eines Changesets (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Ein Changeset ist eine Auflistung der ausstehenden Änderungen an den Masterdaten. Wenn für die Entität eine Genehmigung der Änderungen erforderlich ist, müssen die ausstehenden Änderungen in einem Changeset gespeichert und dann für die Genehmigung durch den Administrator übermittelt werden.  
  
## <a name="prerequisites"></a>Voraussetzungen  
  
-   Sie müssen über die Berechtigung für den Zugriff auf den Funktionsbereich "Explorer" verfügen. Weitere Informationen finden Sie unter [Funktions Bereichs Berechtigungen &#40;Master Data Services&#41;](../master-data-services/functional-area-permissions-master-data-services.md)  
  
-   Sie benötigen mindestens die Berechtigung zum Lesen der Entität oder eines ihrer Attribute.  
  
## <a name="to-create-a-local-changeset"></a>So erstellen Sie ein lokales Changeset  
  
1.  [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]Wählen Sie auf der Startseite das Modell und die Version aus, und klicken Sie dann auf **Explorer**.  
  
2.  Klicken Sie im Menü **Entitäten** auf eine Entität.  
  
3.  Wählen Sie im rechten Bereich **Changesets** , und klicken Sie auf **Erstellen**.  
  
4.  Geben Sie einen Namen für das Changeset ein, und klicken Sie anschließend auf **Speichern**.  
  
     Der Name des Changesets muss innerhalb eines Modells eindeutig sein.  
  
## <a name="to-create-a-changeset-for-approval"></a>So erstellen Sie ein Changeset für die Genehmigung  
  
1.  [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]Wählen Sie auf der Startseite das Modell und die Version aus, und klicken Sie dann auf **Explorer**.  
  
2.  Klicken Sie im Menü **Entitäten** auf eine Entität.  
  
3.  Ändern Sie die Entität, und klicken Sie auf **OK**.  
  
4.  Das Dialogfeld **Changeset auswählen** wird angezeigt.  
  
5.  Klicken Sie auf **Neu**, geben Sie einen Namen für das Changeset ein, und klicken Sie anschließend auf **Speichern**. Der Changesetname muss innerhalb eines Modells eindeutig sein.  
  
6.  Klicken Sie auf **Vorhanden** , und wählen Sie das Changeset aus der Liste aus, um ein vorhandenes Changeset zu verwenden. Nur Changesets mit dem Status "offen" oder "abgelehnt" sind verfügbar.  
  
## <a name="next-steps"></a>Nächste Schritte  
 [Anwenden und Aktualisieren eines Changesets &#40;Master Data Services&#41;](../master-data-services/apply-and-update-a-changeset-master-data-services.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Ein Changeset &#40;Master Data Services Commit oder übermitteln&#41;](../master-data-services/commit-or-submit-a-changeset-master-data-services.md)   
 [Genehmigen oder Ablehnen eines Changesets &#40;Master Data Services&#41;](../master-data-services/approve-or-reject-a-changeset-master-data-services.md)  
  
  
