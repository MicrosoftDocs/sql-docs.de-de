---
description: Transformation für UNION ALL
title: Transformation für UNION ALL | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.unionalltrans.f1
- sql13.dts.designer.unionalltransformation.f1
helpviewer_keywords:
- merging datasets [Integration Services]
- combining datasets
- Union All transformation
- datasets [Integration Services], merging
ms.assetid: 942e4b90-9c41-4e9c-a6f3-80b3afe57f2f
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 59756c7690301d35a1ad53b9db5bde441ce74c24
ms.sourcegitcommit: cfa04a73b26312bf18d8f6296891679166e2754d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2020
ms.locfileid: "92194612"
---
# <a name="union-all-transformation"></a>Transformation für UNION ALL

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Die Transformation für UNION ALL kombiniert mehrere Eingaben zu einer einzigen Ausgabe. Beispielsweise können die Ausgaben von fünf verschiedenen Flatfilequellen Eingaben für die Transformation für UNION ALL sein und zu einer einzigen Ausgabe kombiniert werden.  
  
## <a name="inputs-and-outputs"></a>Eingaben und Ausgaben  
 Die Transformationseingaben werden der Transformationsausgabe nacheinander hinzugefügt. Die Zeilen werden nicht neu angeordnet. Falls das Paket eine sortierte Ausgabe erfordert, sollten Sie anstelle der Transformation für UNION ALL die Transformation für Zusammenführen verwenden.  
  
 Die erste Eingabe, die Sie mit der Transformation für UNION ALL verbinden, ist die Eingabe, von der die Transformation die Transformationsausgabe erstellt. Die Spalten in den Eingaben, die Sie anschließend mit der Transformation verbinden, werden den Spalten in der Transformationsausgabe zugeordnet.  
  
 Um Eingaben zusammenzuführen, ordnen Sie Eingabespalten Ausgabespalten zu. Eine Spalte von mindestens einer Eingabe muss jeder Ausgabespalte zugeordnet werden. Für die Zuordnung von zwei Spalten müssen die Metadaten der Spalten übereinstimmen. Beispielsweise müssen die zugeordneten Spalten denselben Datentyp aufweisen.  
  
 Wenn die zugeordneten Spalten Zeichenfolgendaten enthalten und die Ausgabespalte kürzer als die Eingabespalte ist, wird die Ausgabespalte automatisch verlängert, damit die Eingabespalten Platz haben. Für Eingabespalten, die keinen Ausgabespalten zugeordnet werden, werden in den Ausgabespalten NULL-Werte festgelegt.  
  
 Diese Transformation weist mehrere Eingaben und eine Ausgabe auf. Eine Fehlerausgabe wird nicht unterstützt.  
  
## <a name="configuration-of-the-union-all-transformation"></a>Konfiguration der Transformation für UNION ALL  
 Sie können Eigenschaften mit dem [!INCLUDE[ssIS](../../../includes/ssis-md.md)] -Designer oder programmgesteuert festlegen.  
  
 Weitere Informationen zu den Eigenschaften, die Sie programmgesteuert festlegen können, finden Sie unter [Common Properties](../set-the-properties-of-a-data-flow-component.md).  
  
 Klicken Sie auf eines der folgenden Themen, um weitere Informationen zum Festlegen von Eigenschaften anzuzeigen:  
  
-   [Festlegen der Eigenschaften einer Datenflusskomponente](../../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md)  
  
## <a name="union-all-transformation-editor"></a>Transformations-Editor für UNION ALL
  Mithilfe des Dialogfelds **Transformations-Editor für UNION ALL** können Sie mehrere Eingaberowsets zu einem einzigen Ausgaberowset zusammenführen. Durch das Einschließen der Transformation für UNION ALL in einen Datenfluss können Sie Daten aus mehreren Datenflüssen zusammenführen, komplexe Datasets durch Verschachteln der Transformationen für UNION ALL erstellen und Zeilen erneut zusammenführen, nachdem Sie Fehler in den Daten korrigiert haben.  
  
### <a name="options"></a>Optionen  
 **Name der Ausgabespalte**  
 Geben Sie einen Alias für jede Spalte ein. Standardmäßig wird der Name der Eingabespalte von der ersten (Referenz-)Eingabe verwendet. Sie können jedoch auch einen beschreibenden Namen angeben, sofern dieser eindeutig ist.  
  
 **Eingabe 1 für UNION ALL**  
 Wählen Sie eine Eingabespalte aus der Liste der verfügbaren Eingabespalten in der ersten (Referenz-)Eingabe aus. Die Metadaten der zugeordneten Spalten müssen übereinstimmen.  
  
 **Eingabe n für UNION ALL**  
 Wählen Sie eine Eingabespalte aus der Liste der verfügbaren Eingabespalten in den folgenden Eingaben aus. Die Metadaten der zugeordneten Spalten müssen übereinstimmen.  
  
## <a name="related-tasks"></a>Related Tasks  
 [Zusammenführen von Daten mithilfe der Transformation für UNION ALL](../../../integration-services/data-flow/transformations/merge-data-by-using-the-union-all-transformation.md)  
  
