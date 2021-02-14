---
title: Berichtsteile im Berichts-Designer | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie paginierte Berichtselemente einzeln auf einem Berichtsserver oder auf einer SharePoint-Website veröffentlichen, damit Sie und andere sie in anderen Berichten im Berichts-Generator wiederverwenden können.
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
f1_keywords:
- sql13.rtp.rptdesigner.components.f1
ms.assetid: 0c34311d-05d6-4bd2-b452-545fa95f8e7f
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 873d299829171f516da4a9eeacce37786b99347b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100062825"
---
# <a name="report-parts-in-report-designer-ssrs"></a>Berichtsteile im Berichts-Designer (SSRS)

  Nachdem Sie Tabellen, Diagramme und andere paginierte Berichtselemente in einem Projekt erstellt haben, können Sie sie im Berichts-Designer auf einem Berichtsserver oder einer in einen Berichtsserver integrierten SharePoint-Website als *Berichtsteile* veröffentlichen, damit sie von Ihnen und weiteren Benutzern in anderen Berichten wiederverwendet werden können.  
  
 Im Allgemeinen weisen Berichtsteile im Berichts-Designer und Berichts-Generator die gleiche Funktionsweise auf. Informationen zu grundlegenden Funktionen finden Sie unter [Berichtsteile (Berichts-Generator und SSRS)](../../reporting-services/report-design/report-parts-report-builder-and-ssrs.md).  
  
 Bei der Arbeit mit Berichtsteilen im Berichts-Designer gibt es grundlegende Unterschiede. Ein Hauptunterschied ist der Workflow. Der Berichts-Generator ermöglicht die gemeinsame Erstellung: Ein Benutzer erstellt einen Berichtsteil und veröffentlicht ihn. Ein anderer Benutzer kann ihn wiederverwenden, ändern und erneut veröffentlichen. In Berichts-Designer ist die Veröffentlichung unidirektional: Ein Benutzer kann einen Berichtsteil in Berichts-Designer veröffentlichen, und ein anderer Benutzer kann diesen erneut verwenden. Ein vorhandener Berichtsteil kann jedoch nicht in einem Bericht in Berichts-Designer wiederverwendet werden. In diesem Thema werden die Unterschiede nach einer kurzen Übersicht über Berichtsteile näher erläutert.  
  
##  <a name="life-cycle-of-report-part-publishing"></a><a name="ComponentWorkflow"></a> Lebenszyklus der Berichtsteilveröffentlichung  
 ![rs_ComponentCreation](../../reporting-services/report-design/media/rs-componentcreation.gif "rs_ComponentCreation")  
  
1.  Person A erstellt im Berichts-Designer ein Projekt, das einen Bericht mit einem Diagramm enthält, das von einem eingebetteten Dataset abhängig ist.  
  
2.  Person A kennzeichnet das Diagramm mit dem eingebetteten Dataset für die Veröffentlichung. Der Berichts-Designer weist ihm eine eindeutige ID zu. Person A stellt anschließend den Bericht auf dem Berichtsserver bereit. Der Berichts-Designer veröffentlicht das Diagramm.  
  
3.  Person B erstellt im Berichts-Generator einen leeren Bericht und fügt ihm das Diagramm hinzu. Das Diagramm ist jetzt zusammen mit dem eingebetteten Dataset Teil des Berichts von Person B. Person B kann die im Bericht enthaltenen Diagramm- und Datasetinstanzen ändern. Dies hat weder Auswirkungen auf die Diagramm- und Datasetinstanzen auf dem Berichtsserver, noch wird die Beziehung zwischen den Instanzen im Bericht und auf dem Berichtsserver unterbrochen.  
  
     ![rs_BIDScomponentupdate](../../reporting-services/report-design/media/rs-bidscomponentupdate.gif "rs_BIDScomponentupdate")  
  
4.  Person A ändert im Berichts-Designer das Diagramm im ursprünglichen Bericht.  
  
5.  Person A stellt den Bericht, durch den das Diagramm auf dem Server erneut veröffentlicht wird, erneut bereit, wodurch das Diagramm auf dem Server aktualisiert wird.  
  
6.  Person B akzeptiert im Berichts-Generator das aktualisierte Diagramm vom Server. Dadurch werden die Änderungen, die Person B am Diagramm im Bericht von Person B vorgenommen hatte, überschrieben.  
  
##  <a name="publishing-report-parts"></a><a name="PublishingComponents"></a> Veröffentlichen von Berichtsteilen  
 Beim Veröffentlichen wird einem Berichtsteil vom Berichts-Designer eine eindeutige ID zugewiesen. Diese ID wird, unabhängig von sonstigen Änderungen, die Sie am Berichtsteil vornehmen, von diesem Zeitpunkt an beibehalten. Durch die ID wird das ursprüngliche Berichtselement im Bericht mit dem Berichtsteil verknüpft. Wird der Berichtsteil von anderen Berichtsautoren im Berichts-Generator wiederverwendet, wird auch der Berichtsteil in deren Berichten durch die ID mit dem Berichtsteil verknüpft.  
  
 Die folgenden Berichtselemente können als Berichtsteile veröffentlicht werden:  
  
-   Diagramme  
  
-   Messgeräte  
  
-   Bilder und eingebettete Bilder  
  
-   Karten  
  
-   Parameter  
  
-   Rechtecke  
  
-   Tabellen  
  
-   Matrizen  
  
-   Listen  
  
 Wenn Sie einen Berichtsteil veröffentlichen, in dem Daten angezeigt werden, z. B. eine Tabelle, Matrix oder ein Diagramm, können Sie als Grundlage für den Berichtsteil ein freigegebenes Dataset verwenden. Andernfalls wird beim Veröffentlichen des Berichtsteils das Dataset, von dem der Berichtsteil abhängig ist, als eingebettetes Dataset gespeichert. Eingebettete Datasets können auf eingebetteten Datenquellen basieren, allerdings werden keine Anmeldeinformationen in eingebetteten Datenquellen gespeichert. Wenn der Berichtsteil von einem eingebetteten Dataset abhängig ist, das eine eingebettete Datenquelle verwendet, muss folglich jeder Benutzer, der diesen Berichtsteil wiederverwendet, die Anmeldeinformationen für die eingebettete Datenquelle angeben. Um dies zu vermeiden, sollten Sie als Grundlage für eingebettete und freigegebene Datasets freigegebene Datenquellen mit gespeicherten Anmeldeinformationen verwenden. Weitere Informationen finden Sie unter [Berichtsteile und Datasets](../../reporting-services/report-data/report-parts-and-datasets-in-report-builder.md).  
  
 Die Veröffentlichung eines Berichtsteils im Berichts-Designer umfasst zwei Schritte:  
  
1.  Kennzeichnen Sie die zu veröffentlichenden Berichtselemente im Dialogfeld **Berichtsteile veröffentlichen** .  
  
2.  Stellen Sie den Bericht bereit.  
  
 Wenn Sie den Bericht bereitstellen, wird der Berichtsteil auf einer SharePoint-Website oder einem Berichtsserver veröffentlicht und kann von anderen Benutzern wiederverwendet werden. Zum Veröffentlichen eines Berichtsteils benötigen Sie eine Verbindung mit einem Berichtsserver sowie ausreichende Berechtigungen für den Server, wenn Sie den Bericht bereitstellen.  
  
  
##  <a name="reusing-report-parts"></a><a name="SearchReuseComponents"></a> Wiederverwenden von Berichtsteilen  
 Im Gegensatz zum Berichts-Generator können Sie keine Berichtsteile in einem anderen Projekt als dem suchen und wiederverwenden, in dem er erstellt wurde.  
  
 Berichtsautoren, die im Berichts-Generator arbeiten, können Berichtsteile suchen und wiederverwenden, die Sie in Berichten veröffentlichen, die diese Autoren erstellen.  
  
##  <a name="republishing-report-parts"></a><a name="RepublishingComponents"></a> Erneutes Veröffentlichen von Berichtsteilen  
 Im Berichts-Designer sollten Sie einen vorhandenen Berichtsteil innerhalb des Berichts aktualisieren, in dem Sie den Berichtsteil erstellt haben. Im Berichts-Generator können Berichtsautoren den Berichtsteil wiederverwenden und als neuen Berichtsteil veröffentlichen, ohne den von Ihnen veröffentlichten Berichtsteil zu ersetzen. Sofern ausreichende Berechtigungen vorhanden sind, kann der von Ihnen veröffentlichte Berichtsteil auch aktualisiert werden. Jeder Benutzer mit ausreichenden Berechtigungen für einen Ordner auf einer Website oder einem Server kann die dort gespeicherten Berichtsteile aktualisieren. Vorherige Updates werden durch das letzte Update überschrieben.  
  
 Sie können den Berichtsteil ändern und dann auf der Website oder dem Server erneut veröffentlichen. Berichtsautoren im Berichts-Generator, die diesen Berichtsteil einem Bericht hinzugefügt haben, werden über die Änderung informiert, wenn sie diesen Bericht das nächste Mal öffnen. Sie können die Änderungen annehmen oder ablehnen.  
  
 Sie können auch einen Bericht, den Sie bereits veröffentlicht haben, als neu veröffentlichen. Klicken Sie im Dialogfeld "Berichtsteile veröffentlichen" auf "Als neuen Berichtsteil veröffentlichen". Dieser neue Berichtsteil hat eine neue eindeutige ID und keine Beziehung zum alten Berichtsteil.  

## <a name="next-steps"></a>Nächste Schritte

[Managing Report Parts (Verwalten von Berichtsteilen)](../../reporting-services/report-design/managing-report-parts.md)  

Haben Sie dazu Fragen? [Stellen Sie eine Frage im Reporting Services-Forum](https://go.microsoft.com/fwlink/?LinkId=620231)
