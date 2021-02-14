---
title: Suchen nach Berichtsteilen und Festlegen eines Standardordners (Berichts-Generator) | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Berichtsteile wie Tabellen und Diagramme Ihrem Bericht mithilfe des Berichtsteilkatalogs im Berichts-Generator hinzufügen.
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: 5cf38068-65d1-4fe8-81f3-a404d8fbc663
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: fb998c464acaf6296ef4b0b188f33e5798c757a5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100074809"
---
# <a name="browse-for-report-parts-and-set-a-default-folder-report-builder-and-ssrs"></a>Suchen nach Berichtsteilen und Festlegen eines Standardordners (Berichts-Generator und SSRS)
Die einfachste Möglichkeit, einen paginierten [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Bericht zu erstellen, besteht darin, vorhandene Berichtsteile wie Tabellen und Diagramme dem Bericht mithilfe des Berichtsteilkatalogs hinzuzufügen. Wenn Sie dem Bericht einen Berichtsteil hinzufügen, werden damit auch alle für die Verarbeitung erforderlichen Elemente hinzugefügt. Beispielsweise ist jeder Berichtsteil von einem Dataset (d. h., einer Abfrage und einer Verbindung zu einer Datenquelle) abhängig. Nachdem Sie dem Bericht den Berichtsteil hinzugefügt haben, können Sie diesen nach Bedarf ändern.  
  
 Sie können einen Standardordner für das Veröffentlichen von Berichtsteilen auf dem Berichtsserver oder auf einer in einen Berichtsserver integrierten SharePoint-Website festlegen.  
  
 Weitere Informationen finden Sie unter [Berichtsteile &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/report-parts-report-builder-and-ssrs.md).  
  
## <a name="to-browse-for-report-parts"></a>So suchen Sie nach Berichtsteilen  
  
1.  Klicken Sie im Menü **Einfügen** auf **Berichtsteile**.  
  
     Wurde noch keine Verbindung hergestellt, klicken Sie auf **Stellen Sie eine Verbindung mit einem Berichtsserver her**, und geben Sie den Namen des Berichtsservers ein.  
  
    > [!NOTE]  
    >  Für die Suche nach Berichtsteilen muss eine Verbindung mit einem Berichtsserver bestehen.  
  
2.  Sie können die Suche eingrenzen, indem Sie Details zum Berichtsteil angeben. Geben Sie Namen und Beschreibung ganz oder teilweise in das Feld **Suchen** ein, oder klicken Sie auf **Kriterien hinzufügen** , und fügen Sie Werte für einige oder alle diese Felder hinzu:  
  
    -   Erstellt von  
  
    -   Erstellungsdatum  
  
    -   Datum der letzten Änderung  
  
    -   Zuletzt geändert von  
  
    -   Serverordner  
  
    -   type  
  
     Klicken Sie zum Suchen nach einem Bild beispielsweise auf **Kriterien hinzufügen** und anschließend auf **Typ**. Aktivieren Sie im Dropdownfeld das Kontrollkästchen **Bild** , drücken Sie die EINGABETASTE, und klicken Sie anschließend auf die Suchlupe.  
  
    > [!NOTE]  
    >  Suchen Sie für die Werte **Erstellt von** und **Zuletzt geändert von** nach dem Benutzernamen der Person, wie er auf dem Berichtsserver angegeben ist.  
  
## <a name="to-set-a-default-folder-for-report-parts"></a>So legen Sie einen Standardordner für Berichtsteile fest  
  
1.  Klicken Sie auf **Berichts-Generator**, und klicken Sie dann auf **Optionen**.  
  
2.  Geben Sie im Dialogfeld **Optionen** auf der Registerkarte **Einstellungen** einen Ordnernamen in das Textfeld **Berichtselemente standardmäßig in diesem Ordner veröffentlichen** ein.  
  
 Der Berichts-Generator erstellt diesen Ordner, wenn er noch nicht vorhanden ist und Sie über die Berechtigung verfügen, Ordner auf dem Berichtsserver zu erstellen.  
  
 Sie müssen den Berichts-Generator nicht neu starten, damit diese Einstellung wirksam wird.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Berichtsteile &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/report-parts-report-builder-and-ssrs.md)   
 [Berichtsteile und Datasets in Berichts-Generator](../../reporting-services/report-data/report-parts-and-datasets-in-report-builder.md)   
 [Veröffentlichen und erneutes Veröffentlichen von Berichtsteilen &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/publish-and-republish-report-parts-report-builder-and-ssrs.md)