---
description: Spalten-Kopieren-Transformation
title: Spalten-Kopieren-Transformation | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.copycolumntrans.f1
- sql13.dts.designer.copymaptransformation.f1
helpviewer_keywords:
- columns [Integration Services], copying
- copying columns
- Copy Column transformation
ms.assetid: 1c72a313-9026-46bc-a57f-c6b3f47346f8
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 27673de93d920cdc6adddd12deadcdc4a774d86c
ms.sourcegitcommit: cfa04a73b26312bf18d8f6296891679166e2754d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2020
ms.locfileid: "92194142"
---
# <a name="copy-column-transformation"></a>Spalten-Kopieren-Transformation

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Die Transformation für das Kopieren von Spalten erstellt neue Spalten, indem Eingabespalten kopiert und der Transformationsausgabe die neuen Spalten hinzugefügt werden. Später können im Datenfluss verschiedene Transformationen auf die Spaltenkopien angewendet werden. Beispielsweise können Sie mit der Transformation für das Kopieren von Spalten eine Kopie einer Spalte erstellen und anschließend mithilfe der Transformation zum Zuordnen der Zeichen die kopierten Daten in Großbuchstaben konvertieren. Sie könnten aber auch mithilfe der Transformation für das Aggregieren Aggregationen auf die neue Spalte anwenden.  
  
## <a name="configuration-of-the-copy-column-transformation"></a>Konfiguration der Transformation für das Kopieren von Spalten  
 Zum Konfigurieren der Transformation für das Kopieren von Spalten geben Sie die zu kopierenden Eingabespalten an. Sie können mehrere Kopien einer Spalte erstellen, oder Kopien mehrerer Spalten in einem Vorgang erstellen.  
  
 Diese Transformation weist je eine Eingabe und eine Ausgabe auf. Eine Fehlerausgabe wird nicht unterstützt.  
  
 Eigenschaften können Sie mit dem [!INCLUDE[ssIS](../../../includes/ssis-md.md)] -Designer oder programmgesteuert festlegen.  
  
 Das Dialogfeld **Erweiterter Editor** enthält die Eigenschaften, die programmgesteuert festgelegt werden können. Klicken Sie auf eines der folgenden Themen, um weitere Informationen zu den Eigenschaften zu erhalten, die Sie im Dialogfeld **Erweiterter Editor** oder programmgesteuert festlegen können:  
  
-   [Common Properties](../set-the-properties-of-a-data-flow-component.md)  
  
-   [Benutzerdefinierte Eigenschaften von Transformationen](../../../integration-services/data-flow/transformations/transformation-custom-properties.md)  
  
 Weitere Informationen zum Festlegen der Eigenschaften finden Sie unter [Festlegen der Eigenschaften einer Datenflusskomponente](../../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md).  
  
## <a name="copy-column-transformation-editor"></a>Transformations-Editor für das Kopieren von Spalten
  Über das Dialogfeld **Transformations-Editor für das Kopieren von Spalten** können Sie zu kopierende Spalten auswählen und den neuen Ausgabespalten Namen zuordnen.  
  
> [!NOTE]  
>  Wenn Sie einfach alle Quelldaten in ein Ziel kopieren, ist die Verwendung der Transformation für das Kopieren von Spalten möglicherweise nicht erforderlich. In einigen Szenarien können Sie eine Quelle direkt mit einem Ziel verbinden, wenn keine Datentransformation erforderlich ist. In solchen Fällen ist zum Erstellen des Pakets häufig die Verwendung des SQL Server-Import/Export-Assistenten zu bevorzugen. Sie können das Paket je nach Bedarf zu einem späteren Zeitpunkt erweitern und neu konfigurieren. Weitere Informationen finden Sie unter [SQL Server Import and Export Wizard](~/integration-services/import-export-data/welcome-to-sql-server-import-and-export-wizard.md).  
  
### <a name="options"></a>Tastatur  
 **Verfügbare Eingabespalten**  
 Wählen Sie mithilfe der Kontrollkästchen die zu kopierenden Spalten aus. Durch Ihre Auswahl werden der nachfolgenden Tabelle Eingabespalten hinzugefügt.  
  
 **Eingabespalte**  
 Wählen Sie zu kopierende Spalten aus der Liste der verfügbaren Eingabespalten aus. Ihre Auswahl wird entsprechend in der Auswahl der Kontrollkästchen in der **Verfügbare Eingabespalten** -Tabelle deutlich.  
  
 **Ausgabealias**  
 Geben Sie einen Alias für jede neue Ausgabespalte ein. Der Standard lautet **Kopie von**, gefolgt von dem Namen der Eingabespalte. Sie können jedoch auch einen eindeutigen, beschreibenden Namen auswählen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Datenfluss](../../../integration-services/data-flow/data-flow.md)   
 [SQL Server Integration Services-Transformationen](../../../integration-services/data-flow/transformations/integration-services-transformations.md)  
  
