---
title: Anzeigen von Berichten in der Vorschau
description: Erfahren Sie, wie Sie eine Vorschau von Berichten in SQL Server Reporting Services anzeigen, bevor Sie sie in einer Produktionsumgebung veröffentlichen.
author: maggiesMSFT
ms.author: maggies
ms.reviewer: ''
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reports
ms.topic: conceptual
ms.custom: seodec18
ms.date: 12/14/2018
ms.openlocfilehash: 45965ac8d40496fb2cd18c576ca6bb1228ce855b
ms.sourcegitcommit: a41e1f4199785a2b8019a419a1f3dcdc15571044
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2020
ms.locfileid: "91988626"
---
# <a name="preview-reports-in-sql-server-reporting-services-ssrs"></a>Vorschauberichte in SQL Server Reporting Services (SSRS)

  Wenn Sie einen [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]-Bericht entwerfen, sollten Sie sich diesen ansehen, bevor Sie ihn in einer Produktionsumgebung veröffentlichen. Dazu gibt es mehrere Möglichkeiten: Sie können im Berichts-Designer in den Vorschaumodus wechseln, im Berichts-Designer das Vorschaufenster verwenden oder den Bericht auf einem Berichtsserver in einer Testumgebung veröffentlichen.  
  
> [!NOTE]  
> Wenn Sie eine Vorschau für einen Bericht anzeigen, werden die Daten für den Bericht auf dem lokalen Computer in einer Datei zwischengespeichert. Wenn Sie für denselben Bericht erneut eine Vorschau anzeigen, indem Sie dieselbe Abfrage und dieselben Parameter und Anmeldeinformationen verwenden, ruft der Berichts-Designer die zwischengespeicherte Kopie ab, anstatt die Abfrage erneut auszuführen. Die Datendatei wird in demselben Verzeichnis wie die Berichtsdefinitionsdatei unter dem Namen *\<reportname>* .rdl.data gespeichert. Die Datei wird nicht gelöscht, wenn Sie den Berichts-Designer schließen.  
  
## <a name="preview-mode"></a>Vorschaumodus

 Sie können die Vorschau eines Berichts im Berichts-Designer aufrufen, indem Sie auf ![ssrs_ssdt_preview](../../reporting-services/media/ssrs-ssdt-preview.png "ssrs_ssdt_preview") klicken. Dadurch wird der Bericht lokal mit derselben Berichtsverarbeitungs- und Renderingfunktionalität ausgeführt, die auf dem Berichtsserver zur Verfügung steht. Der Bericht wird als interaktives Bild angezeigt, in dem Sie Parameter auswählen, auf Links klicken, die Dokumentstruktur anzeigen und ausgeblendete Bereiche des Berichts erweitern bzw. reduzieren können. Darüber hinaus können Sie den Bericht in jedes installierte Renderingformat exportieren.  
  
## <a name="standalone-preview"></a>Eigenständige Vorschau

 Sie können auch das Berichtsprojekt in einer Debugkonfiguration ausführen, um eine Vorschau des Berichts anzuzeigen, zum Beispiel zum Debuggen von benutzerdefinierten Assemblys, die Sie geschrieben haben. Der Bericht wird im Standardbrowser geöffnet. Ein Berichtsprojekt kann auf drei Arten ausgeführt werden:  
  
- Durch Klicken im Menü **Debuggen** auf **Debuggen starten** .  
  
- Durch Klicken auf die Schaltfläche **Start** in der [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]-Standardsymbolleiste ![ssrs_ssdt_startdebug](../../reporting-services/reports/media/ssrs-ssdt-startdebug.png "ssrs_ssdt_startdebug")  
  
- Durch Drücken von **F5**.  
  
 Falls Sie eine Projektkonfiguration verwenden, die zwar den Bericht erstellt, aber nicht bereitstellt, wird der in der **StartItem** -Eigenschaft angegebene Bericht der aktuellen Konfiguration in einem separaten Vorschaufenster geöffnet. Im Vorschaufenster wird der Bericht genauso und mit derselben Funktionalität angezeigt wie im Vorschaumodus.  
  
> [!NOTE]  
> Vor dem Debuggen eines Berichts müssen Sie ein Startelement festlegen. Beispiel: Wenn Sie den Debugmodus ausführen und der Browser die Seite des Hauptberichtsservers und nicht Ihren Bericht im Vorschaumodus öffnet. Klicken Sie zum Festlegen eines Startelements im Projektmappen-Explorer mit der rechten Maustaste auf das Berichtsprojekt, klicken Sie auf **Eigenschaften**, und wählen Sie unter **StartItem**den Namen des anzuzeigenden Berichts aus.  
  
 Wenn Sie einen bestimmten Bericht in der Vorschau anzeigen möchten, der nicht als Startelement für das Projekt festgelegt ist, wählen Sie eine Konfiguration aus, die den Bericht zwar erstellt, jedoch nicht bereitstellt (z.B. die DebugLocal-Konfiguration). Klicken Sie dann mit der rechten Maustaste auf den Bericht, und klicken Sie anschließend auf **Ausführen**. Sie müssen eine Konfiguration auswählen, die den Bericht nicht bereitstellt. Andernfalls wird der Bericht auf dem Berichtsserver veröffentlicht und nicht lokal in einem Vorschaufenster angezeigt.  
  
## <a name="publish-to-a-test-server"></a>Veröffentlichen auf einem Testserver

 Ferner können Sie Berichte auch testen, indem Sie sie auf einem Testserver veröffentlichen, zum Bericht wechseln und die Vorschau anzeigen. Das Veröffentlichen eines Berichts auf einem Testserver unterscheidet sich nicht vom Veröffentlichen auf einem Produktionsserver. Informationen zum Veröffentlichen eines Berichts finden Sie unter [Veröffentlichen von Berichten auf einem Berichtsserver](../../reporting-services/reports/publishing-reports-to-a-report-server.md).  
  
## <a name="next-steps"></a>Nächste Schritte

 - [Drucken von Berichten (Berichts-Generator und SSRS)](../../reporting-services/report-builder/print-reports-report-builder-and-ssrs.md)
 - [Drucken eines Berichts &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-builder/print-a-report-report-builder-and-ssrs.md)
 - [Veröffentlichen von Berichten](/previous-versions/sql/sql-server-2016/ms159615(v=sql.130))
 - [Verwenden benutzerdefinierter Assemblys mit Berichten](../../reporting-services/custom-assemblies/using-custom-assemblies-with-reports.md)