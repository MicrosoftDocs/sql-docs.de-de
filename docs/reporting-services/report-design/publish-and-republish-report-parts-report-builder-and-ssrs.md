---
title: Veröffentlichen und erneutes Veröffentlichen von Berichtsteilen (Berichts-Generator) | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie im Berichts-Generator einen Berichtsteil mit bearbeiteten Metadaten und Standardeinstellungen an einem Standardort veröffentlichen.
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: 92dce484-f39b-403c-9caf-d8772bc3aca3
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: f8848fb3d8d474083e0ce1a10b7d355a2e4b886e
ms.sourcegitcommit: e8f6c51d4702c0046aec1394109bc0503ca182f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2020
ms.locfileid: "87928631"
---
# <a name="publish-and-republish-report-parts-report-builder-and-ssrs"></a>Veröffentlichen und erneutes Veröffentlichen von Berichtsteilen (Berichts-Generator und SSRS)
  Berichtsteile sind Elemente paginierter Berichte, die separat auf einem Berichtsserver veröffentlicht wurden und in anderen paginierten Berichten wieder verwendet werden können. Sie können einen Berichtsteil mit den Standardeinstellungen an einem Standardspeicherplatz veröffentlichen oder Metadaten eines Berichtsteils wie Name und Beschreibung bearbeiten und danach an einem anderen Ort auf dem Berichtsserver speichern. Außerdem können Sie ihn auf einer SharePoint-Website speichern, die in einen Berichtsserver integriert ist, wenn Sie die erforderlichen Berechtigungen dafür haben.  
  
 Sie können nur den Berichtsteil oder den Berichtsteil mit dem davon abhängigen Dataset, das darin eingebettet wurde, veröffentlichen. Sie können jedoch auch das Dataset getrennt davon veröffentlichen. Wenn Sie das Dataset separat veröffentlichen, wird es zu einem freigegebenen Dataset, und der Berichtsteil wird damit verknüpft. Weitere Informationen finden Sie unter [Berichtsteile und Datasets](../../reporting-services/report-data/report-parts-and-datasets-in-report-builder.md).  
  
 Sie können einen vorhandenen Berichtsteil erneut veröffentlichen. Wenn Sie über die entsprechenden Berechtigungen verfügen, können Sie die ursprüngliche Instanz des Berichtsteils auf dem Server überschreiben, auch wenn Sie diese nicht erstellt haben. Sie können ihn auch als neuen Berichtsteil veröffentlichen und am gleichen Speicherort oder einem anderen Speicherort speichern.  
  
## <a name="to-publish-a-report-part"></a>So veröffentlichen Sie einen Berichtsteil  
  
1.  Klicken Sie im Menü von Berichts-Generator auf **Berichtselemente veröffentlichen**.  
  
     Wenn Sie keine Verbindung mit einem Berichtsserver hergestellt haben, werden Sie dazu aufgefordert, eine Verbindung herzustellen. Wenn Sie keine Verbindung mit einem Berichtsserver herstellen können, können Sie auch keine Berichtsteile veröffentlichen.  
  
2.  Um die Berichtsteile mit den Standardeinstellungen am Standardspeicherort zu speichern, klicken Sie auf **Alle Berichtselemente mit Standardeinstellungen verwenden**.  
  
     Klicken Sie andernfalls auf **Überprüfen und ändern Sie Berichtselemente vor der Veröffentlichung**.  
  
3.  Bearbeiten Sie den Namen und die Beschreibung des Berichtsteils: Doppelklicken Sie dazu auf den Namen, um ihn zu bearbeiten, und klicken Sie in das Feld **Beschreibung**, um eine Beschreibung hinzuzufügen.  
  
    > [!NOTE]  
    >  Es empfiehlt sich, einen Namen und eine Beschreibung für den Berichtsteil zu erstellen, damit er bei einer Suche von anderen Personen leichter gefunden werden kann. Die maximale Länge des Namens eines Berichtsteils beträgt 260 Zeichen für den gesamten Pfad einschließlich der Namen der Ordner auf dem Server, gefolgt von dem tatsächlichen Namen des Berichtsteils.  
  
4.  Wenn Sie den Berichtsteil nicht im Standardordner speichern möchten, klicken Sie auf die Schaltfläche **Durchsuchen** .  
  
5.  Klicken Sie auf **Veröffentlichen**, nachdem Sie die Optionen für alle Berichtsteile im Bericht festgelegt haben.  
  
     Nach dem Veröffentlichen der Berichtsteile wird im Dialogfeld angezeigt, welche Berichtsteile erfolgreich veröffentlicht wurden und welche nicht. Details zu den Berichtsteilen, die nicht veröffentlicht werden konnten, können im Dialogfeld **Berichtsteile veröffentlichen** angezeigt werden.  
  
6.  Klicken Sie auf **Schließen**.  
  
## <a name="to-republish-a-report-part"></a>So veröffentlichen Sie einen Berichtsteil erneut  
  
1.  Gehen Sie wie im Folgenden beschrieben vor, um einen Berichtsteil erneut zu veröffentlichen.  
  
2.  Wenn Sie im Dialogfeld **Berichtselemente veröffentlichen** auf **Als neue Kopie des Berichtselements veröffentlichen**klicken, überschreibt Berichts-Generator beim Speichern den vorhandenen Berichtsteil auf dem Server nicht, sondern erstellt stattdessen einen anderen Berichtsteil.  
  
> [!NOTE]  
>  Wenn Sie diesen als neuen Berichtsteil veröffentlichen, verfügt er über eine neue eindeutige ID. Er wird nicht mehr aktualisiert, wenn sich der ursprüngliche Berichtsteil ändert.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Berichtsteile &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/report-parts-report-builder-and-ssrs.md)   
 [Berichtsteile und Datasets in Berichts-Generator](../../reporting-services/report-data/report-parts-and-datasets-in-report-builder.md)   
 [Suchen nach Berichtsteilen und Festlegen eines Standardordners &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/browse-for-report-parts-and-set-a-default-folder-report-builder-and-ssrs.md)  
  
  
