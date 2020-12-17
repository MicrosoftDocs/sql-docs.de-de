---
title: Exportieren eines Berichts über URL-Zugriff | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie einen Bericht mithilfe des URL-Zugriffs exportieren. Dazu geben Sie über den URL-Parameter „rs:Format“ das Format an, in dem der Bericht gerendert werden soll.
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
helpviewer_keywords:
- formats [Reporting Services], URL rendering
- URL access [Reporting Services], rendering formats
ms.assetid: 6a3b7fc3-3d91-4d12-8371-42ea12e74517
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: d61f509fa07528b71cd7cbb23488bc14f33959ae
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97472491"
---
# <a name="export-a-report-using-url-access"></a>Exportieren von Berichten über URL-Zugriff
  Sie können mit dem *rs:Format* -URL-Parameter optional das Format angeben, in dem ein Bericht gerendert werden soll.  Die Formate HTML4.0 und HTM5 (Renderingerweiterung) werden im Browser und für andere Formate gerendert, der Browser fordert zum Speichern der Berichtsausgabe in einer lokalen Datei auf.  
  
 Um beispielsweise von einem im einheitlichen Modus ausgeführten Berichtsserver direkt eine PDF-Kopie eines Berichts abzurufen, geben Sie Folgendes an:  
  
```  
https://myrshost/ReportServer?/myreport&rs:Format=PDF  
```  

::: moniker range="=sql-server-2016"
  
 Zum Abrufen von einem Berichtsserver im integrierten SharePoint-Modus geben Sie Folgendes an:  
  
```  
https://myspsite/subsite/_vti_bin/reportserver?https://myspsite/subsite/myrereport.rdl&rs:Format=PDF  
```  
 
::: moniker-end
 
 Beispielsweise exportiert der folgende URL-Befehl in Ihrem Browser einen PPTX-Bericht von einer benannten Instanz des Berichtsservers:  
  
```  
https://servername/ReportServer_THESQLINSTANCE/Pages/ReportViewer.aspx?%2freportfolder%2freport+name+with+spaces&rs:Format=pptx  
```  
  
 Gültige Werte für diesen Parameter sind von den Berichtrenderingerweiterungen abhängig, die auf dem Berichtsserver installiert sind, auf den zugegriffen wird. Häufig verwendete Erweiterungen sind HTML4.0, MHTML, IMAGE, EXCELOPENXML (XLSX), WORDOPENXML (DOCX), CSV, PDF, XML und NULL. Wenn eine bestimmte Renderingerweiterung nicht auf dem Berichtsserver installiert ist, wird der Bericht nicht gerendert. Es wird ein Fehler generiert und im Browser angezeigt.  
  
 Wenn Sie den *Format* -Parameter nicht als Teil der URL aufnehmen, erkennt der Berichtsserver den Browser und rendert den Bericht im entsprechenden HTML-Format.  
  
## <a name="see-also"></a>Weitere Informationen  
 [URL-Zugriff &#40;SSRS&#41;](../reporting-services/url-access-ssrs.md)   
 [URL Access Parameter Reference (URL-Zugriffsparameterverweis)](../reporting-services/url-access-parameter-reference.md)  
  
  
