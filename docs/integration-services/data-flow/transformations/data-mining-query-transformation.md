---
description: Transformation für Data Mining-Abfragen
title: Transformation für Data Mining-Abfragen | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.dataminingquerytrans.f1
- sql13.dts.designer.dmquerytransformation.miningmodel.f1
- sql13.dts.designer.dmquerytransformation.query.f1
helpviewer_keywords:
- Data Mining Query transformation
- prediction queries [Integration Services]
ms.assetid: 7960133b-a3e1-48af-ba43-55ed78c38e71
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8791836a066b658fb6775ce1cbf30f30c7d7d4a1
ms.sourcegitcommit: cfa04a73b26312bf18d8f6296891679166e2754d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2020
ms.locfileid: "92192650"
---
# <a name="data-mining-query-transformation"></a>Transformation für Data Mining-Abfragen

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Die Transformation für Data Mining-Abfragen führt Vorhersageabfragen für Data Mining-Modelle aus. Diese Transformation enthält einen Abfrage-Generator zum Erstellen von DMX-Abfragen (Data Mining Extensions). Mit dem Abfrage-Generator können Sie mithilfe der DMX-Sprache benutzerdefinierte Anweisungen erstellen, um Transformationseingabedaten mit einem vorhandenen Miningmodell zu vergleichen. Weitere Informationen finden Sie unter [Data Mining-Erweiterungen &#40;DMX&#41; – Referenz](../../../dmx/data-mining-extensions-dmx-reference.md).  
  
 Eine Transformation kann mehrere Vorhersageabfragen ausführen, falls die Modelle mit der gleichen Data Mining-Struktur erstellt wurden. Weitere Informationen finden Sie unter [Data Mining-Abfragetools](/analysis-services/data-mining/data-mining-query-tools).  
  
## <a name="configuration-of-the-data-mining-query-transformation"></a>Konfiguration der Transformation für Data Mining-Abfragen  
 Die Transformation für Data Mining-Abfragen stellt mithilfe eines Verbindungs-Managers für [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] eine Verbindung mit dem [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] -Projekt oder der Instanz von [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] her, die die Miningstruktur und die Miningmodelle enthält. Weitere Informationen finden Sie unter [Analysis Services Connection Manager](../../../integration-services/connection-manager/analysis-services-connection-manager.md).  
  
 Diese Transformation weist je eine Eingabe und eine Ausgabe auf. Eine Fehlerausgabe wird nicht unterstützt.  
  
 Sie können Eigenschaften mit dem [!INCLUDE[ssIS](../../../includes/ssis-md.md)] -Designer oder programmgesteuert festlegen.  
  
 Das Dialogfeld **Erweiterter Editor** enthält die Eigenschaften, die programmgesteuert festgelegt werden können. Klicken Sie auf eines der folgenden Themen, um weitere Informationen zu den Eigenschaften zu erhalten, die Sie im Dialogfeld **Erweiterter Editor** oder programmgesteuert festlegen können:  
  
-   [Common Properties](../set-the-properties-of-a-data-flow-component.md)  
  
-   [Benutzerdefinierte Eigenschaften von Transformationen](../../../integration-services/data-flow/transformations/transformation-custom-properties.md)  
  
 Weitere Informationen zum Festlegen der Eigenschaften finden Sie unter [Festlegen der Eigenschaften einer Datenflusskomponente](../../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md).  
  
## <a name="data-mining-query-transformation-editor-mining-model-tab"></a>Transformations-Editor für Data Mining-Abfragen (Registerkarte Miningmodell)
  Mithilfe der Registerkarte **Miningmodell** des Dialogfelds **Transformations-Editor für Data Mining-Abfragen** können Sie die Miningstruktur und deren Miningmodelle angeben.  
  
### <a name="options"></a>Tastatur  
 **Connection**  
 Wählen Sie mithilfe des Listenfelds eine vorhandene Analysis Services-Verbindung aus, oder erstellen Sie mit der Schaltfläche **Neu** wie nachstehend beschrieben eine neue Verbindung.  
  
 **Neu**  
 Erstellen Sie mithilfe des Dialogfelds **Analysis Services-Verbindungs-Manager hinzufügen** eine neue Verbindung.  
  
 **Miningstruktur**  
 Wählen Sie aus der Liste der verfügbaren Miningmodellstrukturen eine Struktur aus.  
  
 **Miningmodelle**  
 Zeigen Sie die Liste der mit der ausgewählten Datenminingstruktur verknüpften Miningmodelle an.  
  
## <a name="data-mining-query-transformation-editor-query-tab"></a>Transformations-Editor für Data Mining-Abfragen (Registerkarte Abfrage)
  Auf der Registerkarte **Abfrage** des Dialogfelds **Transformations-Editor für Data Mining-Abfragen** können Sie eine Vorhersageabfrage erstellen.  
  
### <a name="options"></a>Optionen  
 **Data Mining-Abfrage**  
 Geben Sie die DMX-Abfrage (Data Mining Extension, Data Mining-Erweiterung) direkt in das Textfeld ein.  
  
 **Neue Abfrage erstellen**  
 Klicken Sie auf **Neue Abfrage erstellen** , um mithilfe des grafischen Abfrage-Generators eine DMX-Abfrage zu erstellen.  
