---
title: 'Tutorial: Datenbankoptimierungsratgeber'
description: Der Datenbankoptimierungsratgeber untersucht, wie Abfragen verarbeitet werden, und gibt Empfehlungen zum Verbessern der Abfrageverarbeitungsleistung durch das Ändern von Datenbankstrukturen.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- Database Engine Tuning Advisor [SQL Server], tutorials
- tutorials [Database Engine Tuning Advisor]
ms.assetid: 3b54cbbe-d8c6-424d-92f1-aa58179f4da8
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 4f1de68f73e54af6e50672f78b0a8c90999a4115
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353427"
---
# <a name="tutorial-database-engine-tuning-advisor"></a>Tutorial: Datenbankoptimierungsratgeber

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Willkommen beim Lernprogramm zum Datenbankoptimierungsratgeber. Der Datenbankoptimierungsratgeber prüft, wie Abfragen in den von Ihnen angegebenen Datenbanken verarbeitet werden. Er gibt dann eine Empfehlung ab, wie Sie die Verarbeitung von Abfragen verbessern können, indem Sie Datenbankstrukturen ändern, wie z. B. Indizes, indizierte Sichten und die Partitionierung.  
  
Der Datenbankoptimierungsratgeber bietet zwei Benutzeroberflächen: eine grafische Benutzeroberfläche (GUI) und das Befehlszeilenhilfsprogramm **dta** . Über die grafische Benutzeroberfläche können schnell die Ergebnisse von Optimierungssitzungen angezeigt werden. Das Hilfsprogramm **dta** ermöglicht die einfache Übernahme von Funktionen des Datenbankoptimierungsratgebers in Skripts für die automatische Optimierung. Darüber hinaus kann der Datenbankoptimierungsratgeber XML-Eingaben annehmen und bietet somit eine bessere Steuerung des Optimierungsprozesses.  
  
## <a name="what-you-will-learn"></a>Lernziele  
In diesem Lernprogramm erfahren Sie, wie Sie in der grafischen Benutzeroberfläche des Datenbankoptimierungsratgebers navigieren. Außerdem werden Sie einige einfache Aufgaben mithilfe der grafischen Benutzeroberfläche und mit dem Hilfsprogramm **dta** ausführen. Es enthält die folgenden Lektionen:  
  
[Lektion 1: Grundlagen zur Navigation im Datenbankoptimierungsratgeber](../../tools/dta/lesson-1-basic-navigation-in-database-engine-tuning-advisor.md)  
In dieser Lektion machen Sie sich mit der grafischen Benutzeroberfläche des neuen Datenbankoptimierungsratgebers vertraut und lernen, wie Sie die Anzeigeoptionen und das Layout festlegen.  
  
[Lektion 2: Verwenden des Datenbankoptimierungsratgebers](../../tools/dta/lesson-2-using-database-engine-tuning-advisor.md)  
In dieser Lektion erfahren Sie, wie Sie einfache Optimierungsaufgaben mit der grafischen Benutzeroberfläche des Datenbankoptimierungsratgebers ausführen.  
  
[Lektion 3: Verwenden des Befehlszeilenprogramms dta](../../tools/dta/lesson-3-using-the-dta-command-prompt-utility.md)  
In dieser Lektion lernen Sie, wie Sie das Befehlszeilenprogramm **dta** starten und wie Sie einige einfache Optimierungsbefehle ausführen.  
  
## <a name="requirements"></a>Requirements (Anforderungen)  
Dieses Lernprogramm richtet sich an Datenbankadministratoren, die noch nicht mit der grafischen Benutzeroberfläche des Datenbankoptimierungsratgebers oder mit dem Befehlszeilenhilfsprogramm **dta** vertraut sind, die jedoch mit Datenbankkonzepten und -strukturen vertraut sind, wie z.B. mit Indizes und indizierten Sichten.  
  
Sie müssen [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mit den [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Beispieldatenbanken installieren. Aus Sicherheitsgründen werden die Beispieldatenbanken standardmäßig nicht installiert. Informationen zur Installation der Beispieldatenbanken finden Sie unter [Installieren der SQL Server-Beispiele und -Beispieldatenbanken](https://sqlserversamples.codeplex.com).  
  
## <a name="after-you-finish-this-tutorial"></a>Weiterführende Informationen nach Abschluss dieses Lernprogramms  
Wenn Sie die Lektionen in diesem Lernprogramm durchgearbeitet haben, finden Sie unter folgenden Themen weitere Informationen zum Datenbankoptimierungsratgeber:  
  
-   [Database Engine Tuning Advisor](../../relational-databases/performance/database-engine-tuning-advisor.md) enthält Beschreibungen zum Ausführen von Tasks mit diesem Tool.  
  
-   [dta Utility](../../tools/dta/dta-utility.md) enthält Referenzmaterial zum Eingabeaufforderungs-Hilfsprogramm und der optionalen XML-Datei, mit der Sie die Ausführung des Hilfsprogramms steuern können.  
  
## <a name="next-lesson"></a>Nächste Lektion  
[Lektion 1: Grundlagen zur Navigation im Datenbankoptimierungsratgeber](../../tools/dta/lesson-1-basic-navigation-in-database-engine-tuning-advisor.md)  
  
  
  
