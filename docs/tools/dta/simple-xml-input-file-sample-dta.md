---
title: Beispiel für eine einfache XML-Eingabedatei (DTA)
description: Dieser Artikel enthält ein Beispiel für eine XML-Eingabedatei, die zum Optimieren von Arbeitsauslastungen mit dem Datenbankoptimierungsratgeber verwendet wird.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
dev_langs:
- XML
helpviewer_keywords:
- sample applications [DTA]
ms.assetid: 5b00e4eb-1742-43ec-98d8-d84216b6b840
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 1b475ce68b38283903ecd7295a89b18fce6b9636
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354417"
---
# <a name="simple-xml-input-file-sample-dta"></a>Beispiel für eine einfache XML-Eingabedatei (DTA)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Kopieren Sie dieses Beispiel einer einfachen XML-Eingabedatei, und fügen Sie sie in Ihren XML-Editor oder Text-Editor ein, um sie für die Optimierung der Arbeitsauslastungen zu verwenden. Ersetzen Sie anschließend die Werte für die **Server**-, **Database**-, **Schema**-, **Table**-, **Workload**- und **TuningOptions** -Elemente durch die Werte für Ihre bestimmte Optimierungssitzung. Weitere Informationen zu den Attributen und untergeordneten Elementen, die Sie zusammen mit diesen Elementen verwenden können, finden Sie unter [XML-Eingabedateireferenz &#40;Datenbankoptimierungsratgeber&#41;](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md). Im folgenden Beispiel wird nur eine Teilmenge der verfügbaren Optionen für Attribute und untergeordnete Elemente verwendet.  
  
## <a name="code"></a>Code  
 [!code-xml[InputFileSamples#SimpleXMLInputFile](../../tools/dta/codesnippet/xml/simple-xml-input-file-sa_1.xml)]  
  
## <a name="see-also"></a>Weitere Informationen  
 [Starten und Verwenden des Datenbankoptimierungsratgebers](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md)   
 [XML-Eingabedateireferenz &#40;Datenbankoptimierungsratgeber&#41;](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  
