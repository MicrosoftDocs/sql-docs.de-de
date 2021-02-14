---
description: Bearbeiten und Löschen eines Indexes (Master Data Services)
title: Bearbeiten und Löschen eines Indexes
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: f8fb2a63-f9ae-4b9d-b26f-2024d9af15c5
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: c6fa616af0fcc16b4898194bcdbb82e9382ba98d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341758"
---
# <a name="edit-and-delete-an-index-master-data-services"></a>Bearbeiten und Löschen eines Indexes (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Sie können einen Index, den Sie mit Attributen erstellt haben, bearbeiten und löschen.  
  
## <a name="prerequisites"></a>Voraussetzungen  
 So führen Sie diese Prozedur aus  
  
-   Sie müssen über die Berechtigung verfügen, auf den Funktionsbereich "Systemverwaltung" zuzugreifen. Weitere Informationen finden Sie unter [Berechtigungen für Funktionsbereiche &#40;Master Data Services&#41;](../master-data-services/functional-area-permissions-master-data-services.md).  
  
-   Sie müssen ein Modelladministrator sein. Weitere Informationen finden Sie unter [Administratoren &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
 **So bearbeiten Sie einen Index**  
  
1.  Klicken Sie im Master Data Manager auf **Systemverwaltung**.  
  
2.  Wählen Sie auf der Seite **Modell verwalten** im Raster ein Modell aus, und klicken Sie dann auf **Entitäten**.  
  
3.  Wählen Sie auf der Seite **Entitäten verwalten** im Raster die Entität mit dem zu bearbeitenden Index aus.  
  
4.  Klicken Sie auf **Indizes**.  
  
5.  Wählen Sie auf der Seite **Index verwalten** den zu bearbeitenden Index aus, und klicken Sie anschließend auf **Bearbeiten**.  
  
6.  Geben Sie im Feld **Name** den neuen Indexnamen ein.  
  
7.  Aktivieren oder deaktivieren Sie das Kontrollkästchen **IsUnique** .  
  
8.  Bearbeiten Sie die Liste der zugewiesenen Attribute durch Hinzufügen und Entfernen von Attributen in der Liste.  
  
9. Klicken Sie auf **Speichern**.  
  
 **So löschen Sie einen Index**  
  
1.  Wählen Sie auf der Seite **Modell verwalten** im Raster ein Modell aus, und klicken Sie dann auf **Entitäten**.  
  
2.  Wählen Sie auf der Seite **Entitäten verwalten** im Raster die Entität mit dem zu löschenden Index aus.  
  
3.  Klicken Sie auf **Indizes**.  
  
4.  Wählen Sie auf der Seite **Index verwalten** den zu löschenden Index aus, und klicken Sie anschließend auf **Löschen**.  
  
5.  Klicken Sie in den Feldern für die Bestätigungsmeldung auf **OK**.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Erstellen Sie einen Index &#40;Master Data Services&#41;](../master-data-services/create-an-index-master-data-services.md)   
 [Benutzerdefinierter Index &#40;Master Data Services&#41;](../master-data-services/custom-index-master-data-services.md)  
  
  
