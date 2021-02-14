---
title: Erweiterbarkeit der Regeln zur Datenbankcodeanalyse
description: In diesem Artikel erhalten Sie einen Überblick über die verschiedenen Komponenten von Regeln für die Datenbankcodeanalyse und wie diese in SQL Server Data Tools interagieren. Hier erfahren Sie mehr über das Erstellen benutzerdefinierter Regeln.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 62f5c980-18d5-43fe-b443-c9e149d01fc7
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 24b0224e21f6acec7cc346d35935b0fb0ea692dc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080821"
---
# <a name="overview-of-extensibility-for-database-code-analysis-rules"></a>Übersicht der Erweiterbarkeit um Regeln für die Datenbank-Codeanalyse

Visual Studio-Editionen, die SQL Server Data Tools enthalten, beinhalten Regeln für die Codeanalyse, um Transact\-SQL-Warnungen zu Entwurf, Benennung und Leistung in Ihrem Datenbankcode zu melden. Weitere Informationen zur Codeanalyse finden Sie unter [Analysieren von Datenbankcode zum Verbessern der Codequalität](/previous-versions/visualstudio/visual-studio-2010/dd172133(v=vs.100)).  
  
Wenn die integrierten Regeln zur Codeanalyse ein bestimmtes Transact\-SQL-Problem, das Sie berücksichtigen möchten, nicht abdecken, können Sie benutzerdefinierte Regeln zur Analyse von Datenbankcode erstellen. Beispielsweise möchten Sie möglicherweise eine benutzerdefinierte Regel erstellen, die die Verwendung der WAITFOR DELAY-Anweisung verhindert, wie unter [Exemplarische Vorgehensweise: Erstellen einer benutzerdefinierten Regelassembly zur statischen Codeanalyse für SQL Server](../ssdt/walkthrough-author-custom-static-code-analysis-rule-assembly.md) veranschaulicht wird. Verwenden Sie zum Erstellen benutzerdefinierter Regeln zur Datenbankcodeanalyse die Klassen im [CodeAnalysis](/dotnet/api/microsoft.sqlserver.dac.codeanalysis)-Namespace.  
  
Bevor Sie benutzerdefinierte Regeln zur Codeanalyse erstellen, sollten Sie die Architektur der verschiedenen Komponenten von Regeln zur Datenbankcodeanalyse im Grundsatz verstehen.  
  
## <a name="database-code-analysis-rules-components"></a>Komponenten von Regeln zur Datenbankcodeanalyse  
Das folgende Diagramm veranschaulicht die Interaktion der Regeln zur Datenbankcodeanalyse:  
  
![Komponenten von Regeln zur Datenbankcodeanalyse](../ssdt/media/ssdt-database-code-analysis-rules-components.jpg "Komponenten von Regeln zur Datenbankcodeanalyse")  
  
Wenn Sie die Funktion für Regeln zur Datenbankcodeanalyse verwenden, entweder durch direktes Ausführen der statischen Codeanalyse (weitere Informationen finden Sie unter [Gewusst wie: Analysieren von Transact-SQL-Code auf Codefehler](/previous-versions/visualstudio/visual-studio-2010/dd172119(v=vs.100))) oder durch Erstellen eines Builds, werden alle Regeln geladen und entsprechend ihrer Konfiguration in Ihrem Projekt verwendet. Weitere Informationen finden Sie unter [Vorgehensweise: Aktivieren und Deaktivieren bestimmter Regeln für die statische Analyse von Datenbankcode](/previous-versions/visualstudio/visual-studio-2010/dd172131(v=vs.100)). Der Erweiterungs-Manager lädt außerdem alle benutzerdefinierten Regelassemblys, die von Ihnen erstellt und registriert wurden. Weitere Informationen finden Sie unter [Vorgehensweise: Installieren und Verwalten von Funktionserweiterungen](../ssdt/how-to-install-and-manage-feature-extensions.md).  
  
Eine benutzerdefinierte Klasse von Codeanalyseregeln erbt von [SqlCodeAnalysisRule](/dotnet/api/microsoft.sqlserver.dac.codeanalysis.sqlcodeanalysisrule). Die benutzerdefinierte Regelklasse kann über ihren Regelausführungskontext auf eine Reihe von nützlichen Objekten zugreifen. Dazu gehören:  
  
-   Metadaten über die Regel selbst.  
  
-   Das Dac.Model.TSqlModel, das das Schema der Datenbank darstellt, einschließlich aller Modellelemente, der Beziehungen zwischen ihnen und eventueller Eigenschaften der Elemente.  
  
-   Für Regeln, die spezifische Elemente untersuchen, wird das Dac.Model.TSqlObject, das das Schemaelement im Modell darstellt, in den Kontext einbezogen.  
  
-   Viele Schemaobjekte weisen darüber hinaus eine [ScriptDom](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom)-Darstellung auf, auf die über diesen Kontext zugegriffen werden kann. Hierbei handelt es sich um eine AST-basierte Darstellung eines Elements, die beim Erforschen potenzieller Syntaxprobleme hilfreich sein kann, wie etwa dem Vorhandensein von [SelectStarExpression](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.selectstarexpression).  
  
Ein Dac.CodeAnalysis.SqlRuleProblem wird von der Regel erstellt, um alle von ihr möglicherweise gefundenen Probleme darzustellen. Bei der Erstellung werden das relevante Dac.Model.TSqlObject und ein mögliches Element in der [ScriptDom](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom)-Darstellung an den Konstruktor übergeben und dazu verwendet, die Position des Problems in Ihren Quellcodedateien zu bestimmen. Am Ende der Analyse werden alle genannten Probleme an den Fehler-Manager übergeben und in der Fehlerliste angezeigt.  
  
## <a name="see-also"></a>Weitere Informationen  
[Erweitern der Datenbankfunktionen](../ssdt/extending-the-database-features.md)  
