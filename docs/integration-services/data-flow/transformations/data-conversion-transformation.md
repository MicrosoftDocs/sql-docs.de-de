---
description: Transformation für Datenkonvertierung
title: Transformation für Datenkonvertierung | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.dataconversiontrans.f1
- sql13.dts.designer.dataconversiontransformation.f1
helpviewer_keywords:
- converting data types [Integration Services]
- Data Conversion transformation
- data types [Integration Services], converting
ms.assetid: fd515bbc-6f49-4d0c-ae7f-6ea3c3f24a1c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 642e6ad5d7d0b8ad1103571b5868ba16a509876d
ms.sourcegitcommit: cfa04a73b26312bf18d8f6296891679166e2754d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2020
ms.locfileid: "92192665"
---
# <a name="data-conversion-transformation"></a>Transformation für Datenkonvertierung

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Die Transformation für Datenkonvertierung konvertiert die Daten in einer Eingabespalte in einen anderen Datentyp und kopiert sie dann in eine neue Ausgabespalte. Beispielsweise kann ein Paket Daten aus mehreren Quellen extrahieren und anschließend mithilfe dieser Transformation Spalten in den für den Zieldatenspeicher erforderlichen Datentyp konvertieren. Für eine einzelne Eingabespalte können mehrere Konvertierungen ausgeführt werden.  
  
 Mithilfe dieser Transformation kann ein Paket die folgenden Datenkonvertierungstypen ausführen:  
  
-   den Datentyp ändern. Weitere Informationen finden Sie unter [Integration Services Datentypen](../../../integration-services/data-flow/integration-services-data-types.md).  
  
    > [!NOTE]  
    >  Falls Sie Daten in einen date- oder datetime-Datentyp konvertieren, weist das Datum in der Ausgabespalte das ISO-Format auf, auch wenn für das Gebietsschema ein anderes Format angegeben ist.  
  
-   Festlegen der Spaltenlänge der Zeichenfolgendaten sowie der Genauigkeit und der Dezimalstellenanzahl für numerische Daten. Weitere Informationen finden Sie unter [Genauigkeit, Dezimalstellen und Länge &#40;Transact-SQL&#41;](../../../t-sql/data-types/precision-scale-and-length-transact-sql.md).  
  
-   Eingeben einer Codepage. Weitere Informationen finden Sie unter [Comparing String Data](../../../integration-services/data-flow/comparing-string-data.md).  
  
    > [!NOTE]  
    >  Beim Kopieren zwischen Spalten mit einem Zeichenfolgen-Datentyp müssen die beiden Spalten die gleiche Codepage verwenden.  
  
 Falls die Länge einer Ausgabespalte von Zeichenfolgendaten kürzer als die Länge der entsprechenden Eingabespalte ist, werden die Ausgabedaten abgeschnitten. Weitere Informationen finden Sie unter [Fehlerbehandlung in Daten](../../../integration-services/data-flow/error-handling-in-data.md).  
  
 Diese Transformation weist eine Eingabe, eine Ausgabe und eine Fehlerausgabe auf.  
  
## <a name="related-tasks"></a>Related Tasks  
 Eigenschaften können Sie mit dem [!INCLUDE[ssIS](../../../includes/ssis-md.md)] -Designer oder programmgesteuert festlegen. Weitere Informationen zum Verwenden der Transformation für Datenkonvertierung im SSIS-Designer finden Sie unter [Konvertieren von Daten in einen anderen Datentyp mithilfe der Transformation für Datenkonvertierung](../../../integration-services/data-flow/transformations/convert-data-type-by-using-data-conversion-transformation.md). Weitere Informationen zum programmgesteuerten Festlegen der Eigenschaften dieser Transformation finden Sie unter [Allgemeine Eigenschaften](../set-the-properties-of-a-data-flow-component.md) und [Benutzerdefinierte Eigenschaften von Transformationen](../../../integration-services/data-flow/transformations/transformation-custom-properties.md).  
  
## <a name="related-content"></a>Verwandte Inhalte  
 Blogeintrag, [Leistungsvergleich zwischen Datentypkonvertierungstechniken in SSIS 2008](https://techcommunity.microsoft.com/t5/datacat/performance-comparison-between-data-type-conversion-techniques/ba-p/305035), auf blogs.msdn.com.  
  
## <a name="data-conversion-transformation-editor"></a>Transformations-Editor für Datenkonvertierung
  Im Dialogfeld **Transformations-Editor für Datenkonvertierung** können Sie die zu konvertierenden Spalten und den Datentyp, in den die Spalte konvertiert werden soll, auswählen und Konvertierungsattribute festlegen.  
  
> [!NOTE]  
>  Die **FastParse** -Eigenschaft der Ausgabespalten der Transformation für Datenkonvertierung ist im **Transformations-Editor für Datenkonvertierung**nicht verfügbar, kann aber mithilfe des **erweiterten Editors**festgelegt werden. Weitere Informationen zu dieser Eigenschaft finden Sie im Abschnitt "Transformation für Datenkonvertierung" von [Transformation Custom Properties](../../../integration-services/data-flow/transformations/transformation-custom-properties.md).  
  
### <a name="options"></a>Tastatur  
 **Verfügbare Eingabespalten**  
 Wählen Sie mithilfe der Kontrollkästchen die zu konvertierenden Spalten aus. Durch Ihre Auswahl werden der nachfolgenden Tabelle Eingabespalten hinzugefügt.  
  
 **Eingabespalte**  
 Wählen Sie zu konvertierende Spalten aus der Liste der verfügbaren Eingabespalten aus. Ihre Auswahl wird entsprechend in der Auswahl der Kontrollkästchen oben deutlich.  
  
 **Ausgabealias**  
 Geben Sie einen Alias für jede neue Spalte ein. Der Standard lautet **Copy of** , gefolgt vom Namen der Eingabespalte. Sie können jedoch auch einen eindeutigen, beschreibenden Namen auswählen.  
  
 **Datentyp**  
 Wählen Sie einen verfügbaren Datentyp aus der Liste aus. Weitere Informationen finden Sie unter [Integration Services Datentypen](../../../integration-services/data-flow/integration-services-data-types.md).  
  
 **Länge**  
 Legt für Zeichenfolgendaten die Spaltenlänge fest.  
  
 **Genauigkeit**  
 Legt für numerische Daten die Genauigkeit fest.  
  
 **Skalierung**  
 Legt für numerische Daten die Dezimalstellen fest.  
  
 **Codepage**  
 Wählen Sie die geeignete Codepage für Spalten vom Typ DT_STR aus.  
  
 **Fehlerausgabe konfigurieren**  
 Geben Sie mithilfe des Dialogfelds [Fehlerausgabe konfigurieren](../error-handling-in-data.md) an, wie Fehler auf Zeilenebene behandelt werden sollen.  
  
## <a name="see-also"></a>Siehe auch  
 [Schnelle Analyse](../parsing-data.md)   
 [Datenfluss](../../../integration-services/data-flow/data-flow.md)   
 [SQL Server Integration Services-Transformationen](../../../integration-services/data-flow/transformations/integration-services-transformations.md)  
  
