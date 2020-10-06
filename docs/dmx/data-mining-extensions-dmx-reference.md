---
description: Data Mining-Erweiterungen (DMX) - Referenz
title: Referenz zu Data Mining-Erweiterungen (DMX) | Microsoft-Dokumentation
ms.date: 06/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: dmx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: bd6830e46f10ba20642d1975f293b72b768f7e5b
ms.sourcegitcommit: c7f40918dc3ecdb0ed2ef5c237a3996cb4cd268d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "91726251"
---
# <a name="data-mining-extensions-dmx-reference"></a>Data Mining-Erweiterungen (DMX) - Referenz
[!INCLUDE[ssas](../includes/applies-to-version/ssas.md)]

  Data Mining-Erweiterungen (DMX) sind eine Sprache, die Sie verwenden können, um Data Mining Modelle in zu erstellen und mit Ihnen zu arbeiten [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] . Mit DMX können Sie die Struktur neuer Data Mining-Modelle erstellen, diese Modelle trainieren sowie die Modelle durchsuchen, verwalten und für Vorhersagen verwenden. DMX besteht aus DDL-Anweisungen (Data Definition Language, Datendefinitionssprache), DML-Anweisungen (Data Manipulation Language, Datenbearbeitungssprache) sowie aus Funktionen und Operatoren.  
  
## <a name="microsoft-ole-db-for-data-mining-specification"></a>Spezifikation von Microsoft OLE DB für Data Mining  
 Die Data Mining Funktionen in [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] werden erstellt, um die [!INCLUDE[msCoName](../includes/msconame-md.md)] OLE DB für Data Mining-Spezifikation zu erfüllen.  
  
 Die [!INCLUDE[msCoName](../includes/msconame-md.md)] OLE DB für die Data Mining-Spezifikation definiert Folgendes:  
  
-   Eine Struktur, die die Informationen aufnimmt, die ein Data Mining-Modell definieren  
  
-   Eine Sprache zum Erstellen und Verwenden von Data Mining-Modellen  
  
 Die Spezifikation definiert die Basis von Data Mining als das virtuelle Data Mining-Modellobjekt. Das Data Mining-Modellobjekt kapselt alle Informationen, die zu einem bestimmten Miningmodell bekannt sind. Das Data Mining-Modellobjekt ist wie eine SQL-Tabelle strukturiert, mit Spalten, Datentypen und Metainformationen, die das Modell beschreiben. Diese Struktur ermöglicht es Ihnen, mit der Sprache DMX, die eine Erweiterung von SQL ist, Modelle zu erstellen und zu verwenden.  
  
 **Weitere Informationen finden Sie unter:** [Mining Strukturen &#40;Analysis Services Data Mining-&#41;](/analysis-services/data-mining/mining-structures-analysis-services-data-mining)  
  
##  <a name="dmx-statements"></a><a name="BKMK_DMXStatements"></a> DMX-Anweisungen  
 Mit den DMX-Anweisungen können Sie Data Mining-Modelle erstellen, verarbeiten, kopieren, durchsuchen und für Vorhersagen verwenden. Es gibt zwei Arten von Anweisungen in DMX: Datendefinitionsanweisungen und Datenbearbeitungsanweisungen. Sie können jede Art von Anweisung zum Ausführen unterschiedlicher Arten von Aufgaben verwenden.  
  
 In den folgenden Abschnitten finden Sie weitere Informationen zum Verwenden von DMX-Anweisungen:  
  
-   [Datendefinitionsanweisungen](#BKMK_DDL)  
  
-   [Daten Bearbeitungsanweisungen](#BKMK_DML)  
  
-   [Grundlagen der Abfrage](#BKMK_Queries)  
  
###  <a name="data-definition-statements"></a><a name="BKMK_DDL"></a> Daten Definitions Anweisungen  
 Datendefinitionsanweisungen verwenden Sie in DMX dazu, neue Miningstrukturen und -modelle zu erstellen und zu definieren, Miningmodelle und Miningstrukturen zu importieren und zu exportieren sowie vorhandene Modelle aus einer Datenbank zu löschen. Datendefinitionsanweisungen in DMX sind Teil der Datendefinitionssprache (Data Definition Language, DDL).  
  
 Mit den Datendefinitionsanweisungen in DMX können Sie folgende Aufgaben ausführen:  
  
-   Erstellen Sie mithilfe der [CREATE MINING STRUCTURE](../dmx/create-mining-structure-dmx.md) -Anweisung eine Mining Struktur, und fügen Sie der Mining Struktur mithilfe der [ALTER MINING STRUCTURE](../dmx/alter-mining-structure-dmx.md) -Anweisung ein Mining Modell hinzu.  
  
-   Erstellen Sie gleichzeitig ein Mining Modell und eine zugehörige Mining Struktur, indem Sie die [Create Mining Model](../dmx/create-mining-model-dmx.md) -Anweisung verwenden, um ein leeres Data Mining Modell Objekt zu erstellen.  
  
-   Exportieren Sie mithilfe der [Export](../dmx/export-dmx.md) -Anweisung ein Mining Modell und eine zugehörige Mining Struktur in eine Datei. Importieren Sie mithilfe der [Import](../dmx/import-dmx.md) -Anweisung ein Mining Modell und eine zugehörige Mining Struktur aus einer Datei, die durch die Export-Anweisung erstellt wurde.  
  
-   Kopieren Sie die Struktur eines vorhandenen Mining Modells in ein neues Modell, und trainieren Sie es mit den gleichen Daten, indem [Sie die SELECT INTO](../dmx/select-into-dmx.md) -Anweisung verwenden.  
  
-   Entfernen Sie ein Mining Modell vollständig aus einer Datenbank, indem Sie die [Drop Mining Model](../dmx/drop-mining-model-dmx.md) -Anweisung verwenden. Entfernen Sie eine Mining Struktur und alle zugehörigen Mining Modelle vollständig aus der Datenbank, indem Sie die [Drop Mining Structure](../dmx/drop-mining-structure-dmx.md) -Anweisung verwenden.  
  
 Weitere Informationen zu den Data Mining Aufgaben, die Sie mithilfe von DMX-Anweisungen ausführen können, finden Sie unter [Data Mining Extensions &#40;DMX-&#41;-Anweisungs Referenz](../dmx/data-mining-extensions-dmx-statements.md).  
  
 [Zurück zu den DMX-Anweisungen](#BKMK_DMXStatements)  
  
###  <a name="data-manipulation-statements"></a><a name="BKMK_DML"></a> Daten Bearbeitungsanweisungen  
 Mit den Datenbearbeitungsanweisungen in DMX können Sie vorhandene Miningmodelle verwenden, die Modelle durchsuchen sowie Vorhersagen aus ihnen erstellen. Datenbearbeitungsanweisungen in DMX sind Teil der Datenbearbeitungssprache (Data Manipulation Language, DML).  
  
 Mit den Datenbearbeitungsanweisungen in DMX können Sie folgende Aufgaben ausführen:  
  
-   Trainieren eines Mining Modells mithilfe der [INSERT INTO](../dmx/insert-into-dmx.md) -Anweisung. Diese Anweisung fügt nicht die tatsächlichen Quelldaten in ein Data Mining-Modellobjekt ein, sondern erstellt eine Abstraktion, die das Miningmodell beschreibt, das der Algorithmus erstellt. Die Quell Abfrage für eine INSERT INTO-Anweisung wird in beschrieben [\<source data query>](../dmx/source-data-query.md) .  
  
-   Erweitern Sie die SELECT-Anweisung, um die Informationen zu durchsuchen, die während des Modell Trainings berechnet und im Data Mining Modell gespeichert werden, z. b. Statistiken der Quelldaten. Im folgenden sind die Klauseln aufgeführt, die Sie einschließen können, um die Leistung der SELECT-Anweisung zu erweitern:  
  
    -   [Wählen Sie unterschiedliche &#60;Modell &#62; &#40;DMX aus&#41;](../dmx/select-distinct-from-model-dmx.md)  
  
    -   [Wählen Sie aus &#60;Modell&#62; aus. Inhalt &#40;DMX-&#41;](../dmx/select-from-model-content-dmx.md)  
  
    -   [Wählen Sie aus &#60;Modell&#62; aus. Fälle &#40;DMX-&#41;](../dmx/select-from-model-cases-dmx.md)  
  
    -   [Wählen Sie aus &#60;Modell&#62; aus. SAMPLE_CASES &#40;DMX-&#41;](../dmx/select-from-model-sample-cases-dmx.md)  
  
    -   [Wählen Sie aus &#60;Modell&#62; aus. DIMENSION_CONTENT &#40;DMX-&#41;](../dmx/select-from-model-dimension-content-dmx.md)  
  
-   Erstellen Sie Vorhersagen, die auf einem vorhandenen Mining Modell basieren, indem Sie die [Vorhersage Join](../dmx/select-from-model-prediction-join-dmx.md) -Klausel der SELECT-Anweisung verwenden. Die Quell Abfrage für eine Vorhersage JOIN-Anweisung wird in beschrieben [\<source data query>](../dmx/source-data-query.md) .  
  
-   Entfernen Sie alle trainierten Daten aus einem Modell oder einer Struktur mithilfe der Anweisung [Delete &#40;DMX&#41;](../dmx/delete-dmx.md) .  
  
 Weitere Informationen zu den Data Mining Aufgaben, die Sie mithilfe von DMX-Anweisungen ausführen können, finden Sie unter [Data Mining Extensions &#40;DMX-&#41;-Anweisungs Referenz](../dmx/data-mining-extensions-dmx-statements.md).  
  
 [Zurück zu den DMX-Anweisungen](#BKMK_DMXStatements)  
  
###  <a name="dmx-query-fundamentals"></a><a name="BKMK_Queries"></a> Grundlagen der DMX-Abfrage  
 Die SELECT-Anweisung ist die Grundlage für die meisten DMX-Abfragen. Abhängig von den Klauseln, die Sie in der jeweiligen Anweisung verwenden, können Sie Miningmodelle durchsuchen, kopieren oder für Vorhersagen verwenden. Die Vorhersage Abfrage verwendet das Formular SELECT, um Vorhersagen auf Grundlage vorhandener Mining Modelle zu erstellen. Funktionen erweitern Ihre Möglichkeiten zum Durchsuchen und Abfragen von Miningmodellen über die systeminternen Möglichkeiten des Data Mining-Modells hinaus.  
  
 Mit DMX-Funktionen können Sie Informationen abrufen, die während des Trainings eines Modells ermittelt wurden, sowie neue Informationen berechnen. Sie können diese Funktionen für viele Zwecke verwenden, so z. B. zum Zurückgeben von Statistiken, die die zugrunde liegenden Daten oder die Genauigkeit einer Vorhersage beschreiben, oder zum Zurückgeben einer erweiterten Erläuterung einer Vorhersage.  
  
 **Weitere**  **Informationen finden** Sie Untergrund Legendes [zur DMX-SELECT-Anweisung](../dmx/understanding-the-dmx-select-statement.md), [allgemeine Vorhersagefunktionen &#40;DMX-&#41;](../dmx/general-prediction-functions-dmx.md), [Struktur und Verwendung von DMX-Vorhersage Abfragen](../dmx/structure-and-usage-of-dmx-prediction-queries.md), [Data Mining-Erweiterungen &#40;DMX-&#41; Funktionsreferenz](../dmx/data-mining-extensions-dmx-function-reference.md)  
  
 [Zurück zu den DMX-Anweisungen](#BKMK_DMXStatements)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Data Mining-Erweiterungen &#40;DMX-&#41; Funktionsreferenz](../dmx/data-mining-extensions-dmx-function-reference.md)   
 [Data Mining-Erweiterungen &#40;DMX-&#41; Operator Verweis](../dmx/data-mining-extensions-dmx-operator-reference.md)   
 [Data Mining-Erweiterungen &#40;DMX-&#41;-Anweisungs Referenz](../dmx/data-mining-extensions-dmx-statements.md)   
 [Data Mining-Erweiterungen &#40;DMX-&#41; Syntax Konventionen](../dmx/data-mining-extensions-dmx-syntax-conventions.md)   
 [Data Mining-Erweiterungen &#40;DMX-&#41; Syntax Elemente](../dmx/data-mining-extensions-dmx-syntax-elements.md)   
 [Allgemeine Vorhersagefunktionen &#40;DMX-&#41;](../dmx/general-prediction-functions-dmx.md)   
 [Struktur und Verwendung von DMX-Vorhersage Abfragen](../dmx/structure-and-usage-of-dmx-prediction-queries.md)   
 [Understanding the DMX Select Statement (Grundlegendes zur SELECT-Anweisung)](../dmx/understanding-the-dmx-select-statement.md)  
  
