---
title: Erstellen und Veröffentlichen einer Geschäftsregel
description: Erfahren Sie, wie Sie eine Geschäftsregel in Master Data Services erstellen, um die Genauigkeit der Master Daten sicherzustellen. Nachdem Sie eine Regel erstellt haben, veröffentlichen Sie diese, um die Regel anzuwenden.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- business rules [Master Data Services], creating
- creating business rules [Master Data Services]
ms.assetid: 6961d636-4d69-468e-81f7-8d0be6a4a039
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: fbdb07d5a9bbff92b98fca913e2fcf142a1cbe6c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339088"
---
# <a name="create-and-publish-a-business-rule-master-data-services"></a>Erstellen und Veröffentlichen einer Geschäftsregel (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Erstellen Sie in [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]eine Geschäftsregel, um die Genauigkeit der Masterdaten sicherzustellen. Nachdem Sie eine Regel erstellt haben, müssen Sie diese veröffentlichen, damit Sie sie auf Daten anwenden können.  
  
## <a name="prerequisites"></a>Voraussetzungen  
 So führen Sie diese Prozedur aus  
  
-   Sie müssen über die Berechtigung verfügen, auf den Funktionsbereich **System Verwaltung** zuzugreifen.  
  
-   Sie müssen ein Modelladministrator sein. Weitere Informationen finden Sie unter [Administratoren &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
### <a name="to-create-and-publish-a-business-rule"></a>So erstellen und veröffentlichen Sie eine Geschäftsregel  
  
1.  Klicken Sie in [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]auf **Systemverwaltung**.  
  
2.  Zeigen Sie auf der Menüleiste auf **Verwalten** , und klicken Sie auf **Geschäftsregeln**.  
  
3.  Wählen Sie auf der Seite **Geschäftsregeln** aus der Dropdownliste **Modell** ein Modell aus.  
  
4.  Wählen Sie in der Dropdownliste **Entität** eine Entität aus.  
  
5.  Wählen Sie in der Dropdownliste **Member Types** (Elementtypen) einen Typ des Elements aus, auf das die Geschäftsregel angewendet werden soll.  
  
6.  Klicken Sie auf **Hinzufügen**.  
  
7.  Geben Sie in das Feld **Name** einen Namen für die neue Geschäftsregel ein.  
  
8.  Optional können Sie im Feld **Beschreibung** die Beschreibung der Geschäftsregel eingeben.  
  
9. Aktivieren Sie optional die Option **Benachrichtigungen senden** , und wählen Sie aus der Dropdownliste einen Benutzer oder eine Gruppe aus, an den bzw. die die E-Mail-Benachrichtigung gesendet wird.  
  
    > [!NOTE]  
    >  Benachrichtigungen werden nur für Regeln gesendet, die eine Überprüfungsaktion einschließen.  
  
10. Klicken Sie im **IF** -Abschnitt auf **Hinzufügen**. Ein Bereich wird angezeigt.  
  
11. Wählen Sie ein Attribut aus der Dropdownliste **Attribut** aus.  
  
12. Wählen Sie eine Bedingung aus der Dropdownliste **Operator** aus.  
  
13. Füllen Sie alle Pflichtfelder aus.  
  
14. Klicken Sie auf die Schaltfläche **Save** . Eine neue Zeile wird dem **IF** -Raster hinzugefügt.  
  
    > [!TIP]  
    >  Sie können Elemente aus Ihrer Geschäftsregel löschen, indem Sie mit der rechten Maustaste auf jedes Element klicken und **Löschen** auswählen.  
  
15. Fügen Sie der Regel option mehrere Bedingungen hinzu. Weitere Informationen finden Sie unter [Hinzufügen mehrerer Bedingungen zu einer Geschäftsregel &#40;Master Data Services&#41;](../master-data-services/add-multiple-conditions-to-a-business-rule-master-data-services.md).  
  
16. Klicken Sie im **THEN** -Abschnitt auf **Hinzufügen** . Ein Bereich wird angezeigt.  
  
17. Wählen Sie ein Attribut aus der Dropdownliste **Attribut** aus.  
  
18. Wählen Sie eine Aktion aus der Dropdownliste **Operator** aus.  
  
19. Füllen Sie alle Pflichtfelder aus.  
  
20. Klicken Sie auf **Speichern**. Eine neue Zeile wird dem **Then** -Raster hinzugefügt.  
  
21. Optional können Sie eine **ELSE** -Aktion hinzufügen, indem Sie folgende Schritte ausführen.  
  
    1.  Klicken Sie unter dem **ELSE** -Abschnitt auf **Hinzufügen**. Ein Bereich wird angezeigt.  
  
    2.  Wählen Sie ein Attribut aus der Dropdownliste **Attribut** aus.  
  
    3.  Wählen Sie eine Aktion aus der Dropdownliste **Operator** aus.  
  
    4.  Füllen Sie alle Pflichtfelder aus.  
  
    5.  Klicken Sie auf **Speichern**. Eine neue Zeile wird dem **ELSE** -Raster hinzugefügt.  
  
22. Klicken Sie auf **Speichern**. Eine neue Zeile wird dem Geschäftsregelraster hinzugefügt.  
  
23. Klicken Sie auf **Alle veröffentlichen**.  
  
24. Klicken Sie im Bestätigungsdialogfeld auf **OK**. Der Wert in der Spalte **Geschäftsregelstatus** ist **Aktiv**.  
  
## <a name="grid-columns"></a>Rasterspalten  
 Für jede erstelle Geschäftsregel wird dem Raster eine Zeile mit sechs Spalten hinzugefügt. Das sind die Spalten.  
  
|Name|BESCHREIBUNG|  
|----------|-----------------|  
|Status|Wenn Sie auf **Speichern** klicken, wird das folgende Bild angezeigt, das angibt, dass die Geschäftsregel aktualisiert wird.<br /><br /> ![mds_BR_refresh](../master-data-services/media/mds-br-refresh.png "mds_BR_refresh")<br /><br /> Falls beim Erstellen oder Bearbeiten einer Geschäftsregel Fehler auftreten, wird das folgende Bild angezeigt.<br /><br /> ![mds_br_error](../master-data-services/media/mds-br-error.png "mds_br_error")<br /><br /> Falls der Status „OK“ lautet, wird das folgende Bild angezeigt.<br /><br /> ![mds_BR_success](../master-data-services/media/mds-br-success.png "mds_BR_success")|  
|Name|Der Geschäftsregelname|  
|Beschreibung|Die Beschreibung der Geschäftsregel.|  
|Geschäftsregelstatus|Einer der folgenden Geschäftsregelstatus: : „Keine Regel definiert“, „Aktiv“, „Ausgeschlossen“, „ausstehende Änderungen“, „ausstehender Ausschluss“ und „ausstehende Löschung“.|  
|Ausgeschlossen|Gibt an, ob die Geschäftsregel ausgeschlossen ist.|  
|Benachrichtigung|Gibt den Benutzer oder eine Gruppe an, an den bzw. die die E-Mail-Benachrichtigung gesendet werden soll.|  
  
## <a name="next-steps"></a>Nächste Schritte  
  
-   Führen Sie zum Anwenden von Geschäftsregeln auf Daten eine der folgenden Prozeduren aus:  
  
    -   [Überprüfen von bestimmten Elementen auf Geschäftsregeln &#40;Master Data Services&#41;](../master-data-services/validate-specific-members-against-business-rules-master-data-services.md)  
  
    -   [Überprüfen einer Version anhand von Geschäftsregeln &#40;Master Data Services&#41;](../master-data-services/validate-a-version-against-business-rules-master-data-services.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Konfigurieren von Geschäftsregeln für das Senden von Benachrichtigungen &#40;Master Data Services&#41;](../master-data-services/configure-business-rules-to-send-notifications-master-data-services.md)   
 [Ändern Sie den Namen einer Geschäftsregel &#40;Master Data Services&#41;](../master-data-services/change-a-business-rule-name-master-data-services.md)   
 [Hinzufügen mehrerer Bedingungen zu einer Geschäftsregel &#40;Master Data Services&#41;](../master-data-services/add-multiple-conditions-to-a-business-rule-master-data-services.md)  
  
  
