---
title: Codegliederung
description: Hier erfahren Sie, wie Sie die Codegliederungsfunktion in den Abfrage-Editoren von SQL Server Management Studio verwenden, um Code selektiv auszublenden.
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Query Editor [SQL Server Management Studio], outlining code
- Query Editor [SQL Server Management Studio], hiding code
ms.assetid: 556c7dfe-7bc8-4cab-a36f-2b753a05d3f1
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 58b2b9ac1955e2563f7b0df9e9f41d2a27269bfb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354765"
---
# <a name="code-outlining"></a>Codegliederung
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Sie können die Gliederungsfunktion in den [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] -Abfrage-Editoren verwenden, um ausgewählten Code auszublenden, wenn Sie Abfragen bearbeiten. Auf diese Weise können Sie insbesondere in großen Abfragedateien den bearbeiteten Code besser anzeigen.  
  
## <a name="outlining-overview"></a>Übersicht über Gliederungen  
 Standardmäßig wird der gesamte Code angezeigt, wenn Sie ein Abfrage-Editorfenster öffnen. Es ist möglich, Codebereiche zu reduzieren, sodass der Code ausgeblendet wird. Eine vertikale Linie auf der linken Seite des Editorfensters gibt anhand eines Quadrats mit einem Minuszeichen (-) den Beginn eines reduzierbaren Codebereichs an. Wenn Sie auf ein Minuszeichen klicken, wird der Text des Codebereichs durch ein Feld mit drei Punkten (...) ersetzt, und das Minuszeichen wird in ein Pluszeichen (+) geändert. Wenn Sie auf ein Pluszeichen klicken, wird der reduzierte Code angezeigt, und das Pluszeichen wird in ein Minuszeichen (-) geändert. Wenn Sie den Mauszeiger über ein Feld mit drei Punkten bewegen, wird eine QuickInfo mit dem Code im reduzierten Abschnitt angezeigt.  
  
## <a name="system-outline-regions"></a>Gliederungsbereiche des Systems  
 Jeder [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] -Editor generiert einen Satz vorgegebener systemdefinierter Gliederungsbereiche.  
  
 Der MDX- und der DMX-Code-Editor erstellen Gliederungsbereiche für alle mehrzeiligen Anweisungen. Dies ist die einzige von diesen Editoren unterstützte Gliederungsebene.  
  
### <a name="analysis-services-xmla-query-editor-regions"></a>Gliederungsbereiche des XMLA-Abfrage-Editors von Analysis Services  
 Der XMLA-Abfrage-Editor von [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] generiert einen Gliederungsbereich für alle mehrzeiligen XML-Attribute. Der Editor schachtelt die Gliederungsbereiche für geschachtelte Tags. So erstellt der XMLA-Editor z. B. drei Gliederungsbereiche für das folgende Dokument.  
  
 ![XML-Code für Gliederung](../../ssms/scripting/media/editoutlinexmlfull.gif "XML-Code für Gliederung")  
  
 Wenn Sie auf das Minuszeichen in der \<InnerTag>-Zeile klicken, wird wie in der folgenden Abbildung dargestellt nur „InnerTag“ reduziert.  
  
 ![XML-Code mit verborgenem innerem Knoten](../../ssms/scripting/media/editoutlinexmlinnercol.gif "XML-Code mit verborgenem innerem Knoten")  
  
 Wenn Sie den Mauszeiger über das Feld mit den drei Punkten (…) bewegen, wird der Code wie in der folgenden Abbildung gezeigt im reduzierten Bereich in einer QuickInfo angezeigt.  
  
 ![XML-Code mit QuickInfo mit verborgenem Code](../../ssms/scripting/media/editoutlinexmlmouse.gif "XML-Code mit QuickInfo mit verborgenem Code")  
  
 Wenn Sie auf das Minuszeichen in der \<MiddleTag>-Zeile klicken, werden wie in der folgenden Abbildung dargestellt „MiddleTag“ und „InnerTag“ reduziert.  
  
 ![XML-Code mit verborgenen inneren und mittleren Tags](../../ssms/scripting/media/editoutlinexmlmiddlecol.gif "XML-Code mit verborgenen inneren und mittleren Tags")  
  
 Wenn Sie auf das Minuszeichen in der \<OuterTag>-Zeile klicken, werden wie in der folgenden Abbildung dargestellt alle drei Zeilen reduziert.  
  
 ![XML-Code, bei dem alle drei Tags verborgen sind](../../ssms/scripting/media/editoutlinexmloutercol.gif "XML-Code, bei dem alle drei Tags verborgen sind")  
  
### <a name="database-engine-query-editor-regions"></a>Gliederungsbereiche des Datenbank-Engine-Abfrage-Editors  
 Der [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Abfrage-Editor generiert Gliederungsbereiche für jedes Element in der folgenden Hierarchie:  
  
1.  Batches. Der erste Batch ist der Code vom Anfang der Datei bis zum ersten GO-Befehl oder bis zum Ende der Datei, wenn keine GO-Befehle vorhanden sind. Nach dem ersten GO-Befehl gibt es einen Batch von jedem GO-Befehl bis zum nächsten GO-Befehl oder bis zum Ende der Datei.  
  
2.  Blöcke, die durch die folgenden Schlüsselwörter begrenzt sind:  
  
    -   BEGIN - END  
  
    -   BEGIN TRY - END TRY  
  
    -   BEGIN CATCH - END CATCH  
  
3.  Mehrzeilige Anweisungen.  
  
 Der [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Abfrage-Editor erstellt z. B. drei Gliederungsbereiche für die folgende Abfrage:  
  
```  
CREATE PROCEDURE Sales.SampleProc --Outline region 1  
AS  
BEGIN --Outline region 2   
  SELECT GETDATE() AS TimeOfQuery;  
  SELECT * --Outline region 3  
  FROM sys.transmission_queue;  
  SELECT @@VERSION;  
END;  
GO  
```  
  
 Sie können auf das Minuszeichen in der `SELECT *` -Zeile klicken, um nur diese `SELECT` -Anweisung zu reduzieren. Um den gesamten `BEGIN - END` -Block zu reduzieren, klicken Sie auf das Minuszeichen in der `BEGIN` -Zeile. Um den gesamten Batch bis zum `GO` -Befehl zu reduzieren, klicken Sie auf das Minuszeichen in der `CREATE PROCEDURE` -Zeile. Es ist nicht möglich, die `SELECT GETDATE()` -Zeile oder die `SELECT @@VERSION` -Zeile einzeln zu reduzieren, da es sich um einzeilige Anweisungen handelt, die keine Gliederungsbereiche erhalten.  
  
  
