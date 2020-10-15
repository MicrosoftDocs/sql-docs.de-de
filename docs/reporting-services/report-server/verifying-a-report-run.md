---
title: Überprüfen einer Berichtsausführung | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Protokolldateien verwenden oder auf Statusinformationen verweisen, die mit einem Bericht angezeigt werden, um eine Berichtsausführung im Berichts-Manager zu überprüfen.
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- auditing [Reporting Services]
- verifying report execution
- logs [Reporting Services], verifying report run
- report execution data [Reporting Services]
- status information [Reporting Services]
- report processing [Reporting Services], verifying execution
- checking report execution
ms.assetid: 18a98f2f-6b40-454e-9b37-568ed1a96458
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 89f3381fdd26cc2fccc5aec34049e7d97fce4ef3
ms.sourcegitcommit: a41e1f4199785a2b8019a419a1f3dcdc15571044
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2020
ms.locfileid: "91987478"
---
# <a name="verifying-a-report-run"></a>Überprüfen einer Berichtsausführung
  Mit Protokolldateien oder den Statusinformationen, die zusammen mit einem Bericht im Berichts-Manager angezeigt werden, können Sie Informationen zum Status der Berichtsverarbeitung anzeigen.  
  
## <a name="sources-of-report-execution-data"></a>Quellen für Berichtausführungsdaten  
 Die Berichtsausführungsprotokolle stellen umfangreiche Informationen zur Berichtsausführung bereit. Hierzu zählen der Name des Berichts, der Name des Benutzers, der den Bericht ausgeführt hat, der Zeitpunkt der Berichtsausführung sowie die verwendete Übermittlungserweiterung für die Weitergabe des Berichts. Um diese Daten anzuzeigen und zu analysieren, kopieren Sie das Berichtsausführungsprotokoll in Datenbanktabellen, für die problemlos Abfragen ausgeführt und Berichte erstellt werden können.  
  
 Protokolldateien enthalten eine Vielzahl von Einträgen zur Berichtsausführung und zu anderen Serveraktivitäten. Da Protokolldateien so viele Daten enthalten, sollten Sie den Berichts-Manager verwenden, wenn Sie nur überprüfen möchten, wann der Bericht zuletzt ausgeführt wurde. Falls Sie zusätzliche Informationen benötigen, müssen Sie die Protokolldateien ansehen.  
  
> [!NOTE]  
>  Im Berichts-Manager werden die Verarbeitungszeiten von Berichten, die bei Bedarf ausgeführt wurden, nicht angezeigt.  
  
 In der folgenden Tabelle ist beschrieben, wie Sie das Datum und die Uhrzeit der Verarbeitung für verschiedene Berichtstypen anzeigen.  
  
|Berichtstyp|Datum und Uhrzeit ist hier zu finden|Vorgehensweise zum Anzeigen der Informationen|  
|-----------------------------|-----------------------------------------------|-----------------------------------------------|  
|Ein Bericht, der als Berichtsmomentaufnahme ausgeführt wird.|Auf der Seite Inhalt. Weitere Informationen finden Sie unter [Inhalt (Seite) (Berichts-Manager)](/previous-versions/sql/sql-server-2016/ms186470(v=sql.130)).|1) Suchen Sie den Ordner, in dem der Bericht gespeichert ist.<br /><br /> 2) Wählen Sie für den Ordner die Sicht „Details“ aus.<br /><br /> 3) Notieren Sie sich das Datum und die Uhrzeit in der Spalte **Wann ausgeführt** .|  
|Eine Momentaufnahme im Berichtsverlauf.|Auf der Eigenschaftenseite Verlauf. Weitere Informationen finden Sie unter [Momentaufnahmeoptionen (Eigenschaftenseite) (Berichts-Manager)](/previous-versions/sql/sql-server-2016/ms189952(v=sql.130)).|1) Öffnen Sie den Bericht.<br /><br /> 2) Klicken Sie auf die Seite **Eigenschaften** .<br /><br /> 3) Klicken Sie auf die Registerkarte **Verlauf** .<br /><br /> 4) Notieren Sie sich das Datum und die Uhrzeit in der Spalte **Wann ausgeführt** .|  
|Ein zwischengespeicherter Bericht.|Im Zeitplan, der zum Erstellen und Aktualisieren des zwischengespeicherten Berichts verwendet wird.|1) Öffnen Sie den Bericht.<br /><br /> 2) Klicken Sie auf die Seite **Eigenschaften** .<br /><br /> 3) Klicken Sie auf die Registerkarte **Ausführung** .<br /><br /> 4) Öffnen Sie den Zeitplan.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Reporting Services-Protokolldateien und Quellen](../../reporting-services/report-server/reporting-services-log-files-and-sources.md)   
 [Festlegen von Berichtsverarbeitungseigenschaften](../../reporting-services/report-server/set-report-processing-properties.md)   
 [Berichts-Manager (einheitlicher SSRS-Modus)](../web-portal-ssrs-native-mode.md)  
  
