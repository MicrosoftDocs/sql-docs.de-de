---
title: Erstellen einer Entität
description: Erfahren Sie, wie Sie in Master Data Services eine Entität erstellen, die Elemente und ihre Attribute enthält. Sie müssen über Berechtigungen für den Bereich System Verwaltung verfügen.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- entities [Master Data Services], creating
- creating entities [Master Data Services]
ms.assetid: d9a6a51e-7b53-4785-a118-3baeb7ca2d48
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 8d47a78e4a7e706e91312ee2eb4686121c2b2532
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341429"
---
# <a name="create-an-entity-master-data-services"></a>Erstellen einer Entität (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Erstellen Sie in [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]eine Entität, die Elemente und ihre Attribute enthält.  
  
## <a name="prerequisites"></a>Voraussetzungen  
 So führen Sie diese Prozedur aus  
  
-   Sie müssen über die Berechtigung verfügen, auf den Funktionsbereich **System Verwaltung** zuzugreifen.  
  
-   Sie müssen ein Modelladministrator sein. Weitere Informationen finden Sie unter [Administratoren &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
-   Es muss ein Modell vorhanden sein. Weitere Informationen finden Sie unter [Erstellen eines Modells &#40;Master Data Services&#41;](../master-data-services/create-a-model-master-data-services.md).  
  
### <a name="to-create-an-entity"></a>So erstellen Sie eine Entität  
  
1.  Klicken Sie in [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]auf **Systemverwaltung**.  
  
2.  Wählen Sie auf der Seite **Modell verwalten** aus dem Raster das Modell aus, für das die Entität erstellt werden soll, und klicken Sie anschließend auf **Entitäten**.  
  
3.  Klicken Sie auf der Seite **Entität verwalten** auf **Hinzufügen**.  
  
4.  Geben Sie im Feld **Name** den Namen der Entität ein.  
  
5.  (Optional) Geben Sie im Feld **Beschreibung** die Entitätsbeschreibung ein.  
  
6.  (Optional) Geben Sie im Feld **Name der Stagingtabellen** einen Namen für die Stagingtabelle ein.  
  
     Wenn Sie dieses Feld nicht ausfüllen, wird der Entitätsname verwendet.  
  
    > [!TIP]  
    >  Verwenden Sie den Modellnamen als einen Teil des Stagingtabellennamens, z.B. *Modelname_Entityname*. Dies erleichtert die Suche nach den Tabellen in der Datenbank. Weitere Informationen zu den Stagingtabellen finden Sie unter [Übersicht: Importieren von Daten aus Tabellen &#40;Master Data Services&#41;](../master-data-services/overview-importing-data-from-tables-master-data-services.md).
    > [!TIP]
    > Wenn Sie die Standardnamen für Stagingtabellen verwenden, fügt MDS den Namen der Stagingtabellen automatisch Bezeichner an (z.B. „_1“, „_2“), wenn eine Entität desselben Namens in einem anderen Modell vorhanden ist.
  
7.  Wählen Sie im Feld **Transaktionsprotokolltyp** den Transaktionsprotokolltyp aus der Dropdownliste aus.  
  
     Weitere Informationen finden Sie unter [Ändern des Transaktionsprotokolltyps von Entitäten &#40;Master Data Services&#41;](../master-data-services/change-the-entity-transaction-log-type-master-data-services.md).  
  
8.  Dies ist optional. Aktivieren Sie das Kontrollkästchen **Codewerte automatisch erstellen** . Weitere Informationen finden Sie unter [automatische Code Erstellung &#40;Master Data Services&#41;](../master-data-services/automatic-code-creation-master-data-services.md).  
  
9. Dies ist optional. Aktivieren Sie das Kontrollkästchen **Datenkomprimierung aktivieren** . Die Zeilenkomprimierung ist standardmäßig aktiviert. Weitere Informationen finden Sie unter [Datenkomprimierung](../relational-databases/data-compression/data-compression.md).  
  
10. Klicken Sie auf **Speichern**.  
  
## <a name="grid-columns"></a>Rasterspalten  
 Für jede erstellte Entität wird dem Raster eine Zeile mit dreizehn Spalten hinzugefügt. Das sind die Spalten.  
  
|Name|BESCHREIBUNG|  
|----------|-----------------|  
|Status|Der Status der Entität. Wenn Sie auf **Speichern** klicken, wird das folgende Bild angezeigt, das angibt, dass die Entität aktualisiert wird.<br /><br /> ![Symbol für Aktualisierungs Status](../master-data-services/media/mds-statusicon-updating.png "Symbol für Aktualisierungs Status")<br /><br /> Wenn beim Erstellen oder Bearbeiten einer Entität Fehler auftreten, wird das folgende Bild angezeigt.<br /><br /> ![Symbol für Fehlerstatus](../master-data-services/media/mds-statusicon-error.png "Symbol für Fehlerstatus")<br /><br /> Falls der Status „OK“ lautet, wird das folgende Bild angezeigt.<br /><br /> ![Symbol für Status OK](../master-data-services/media/mds-statusicon-ok.png "Symbol für Status OK")|  
|Name|Der Name der Entität.|  
|Beschreibung|Die Entitätsbeschreibung.|  
|Stagingtabelle|Der Präfixname der Tabelle, die zum Speichern von Daten verwendet wird.|  
|Transaktionsprotokolltyp|Der Transaktionsprotokolltyp der Entität.|  
|Automatische Codeerstellung|Gibt an, ob die automatische Codeerstellung aktiviert ist.|  
|Datenkomprimierung|Gibt an, ob die Datenkomprimierung für die Entität aktiviert ist.|  
|Ist Synchronisationsziel|Gibt an, ob die Entität das Ziel einer Synchronisierungsbeziehung ist.|  
|Ist für Hierarchie aktiviert|Gibt an, ob die Entität für explizite Hierarchien aktiviert ist. Diese Spalte zeigt "Ja" an, wenn mindestens eine explizite Hierarchie für die Entität erstellt wird.|  
|Created By|Der Benutzername des Benutzers, der die Entität erstellt hat.|  
|Erstellt am|Das Datum und die Uhrzeit der Entitätserstellung.|  
|Aktualisiert von|Der Benutzername des Benutzers, der die Entität zuletzt aktualisiert hat.|  
|Aktualisiert am|Datum und Uhrzeit, wann die Entität zuletzt aktualisiert wurde.|  
  
## <a name="next-steps"></a>Nächste Schritte  
  
-   [Erstellen eines Textattributs &#40;Master Data Services&#41;](../master-data-services/create-a-text-attribute-master-data-services.md)  
  
-   [Erstellen eines domänenbasierten Attributs &#40;Master Data Services&#41;](../master-data-services/create-a-domain-based-attribute-master-data-services.md)  
  
-   [Erstellen eines Dateiattributs &#40;Master Data Services&#41;](../master-data-services/create-a-file-attribute-master-data-services.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Entitäten &#40;Master Data Services&#41;](../master-data-services/entities-master-data-services.md)   
 [Explizite Hierarchien &#40;Master Data Services&#41;](../master-data-services/explicit-hierarchies-master-data-services.md)   
 [Bearbeiten eines Entitäts &#40;Master Data Services&#41;](../master-data-services/edit-an-entity-master-data-services.md)   
 [Löschen einer Entität &#40;Master Data Services&#41;](../master-data-services/delete-an-entity-master-data-services.md)  
  
  
