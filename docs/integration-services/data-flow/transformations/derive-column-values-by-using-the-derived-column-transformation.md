---
description: Ableiten von Spaltenwerten mithilfe der Transformation für abgeleitete Spalten
title: Ableiten von Spaltenwerten mithilfe der Transformation für abgeleitete Spalten | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- columns [Integration Services]
- derived columns
- columns [Integration Services], values
- Derived Column transformation
ms.assetid: 28b07746-fc6f-42b2-b741-9de6fac3f29c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 06ecff11e6fc32ad59e6457ec2404292a9c22955
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353724"
---
# <a name="derive-column-values-with-the-derived-column-transformation"></a>Ableiten von Spaltenwerten mithilfe der Transformation für abgeleitete Spalten

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Das Paket muss bereits mindestens einen Datenflusstask und eine Quelle enthalten, damit Sie eine Transformation für abgeleitete Spalten hinzufügen und konfigurieren können.  
  
 Die Transformation für abgeleitete Spalten verwendet Ausdrücke, um vorhandene Werte zu aktualisieren oder Werte neuen Spalten hinzuzufügen. Wenn Sie das Hinzufügen von neuen Werten zu neuen Spalten auswählen, wird mithilfe des Dialogfelds **Transformations-Editor für abgeleitete Spalte** der Ausdruck ausgewertet, und danach werden die Metadaten der Spalten definiert. Wenn beispielsweise ein Ausdruck zwei Spalten verkettet, die beide den DT_WSTR-Datentyp sowie eine Länge von 50 aufweisen und sich ein Leerzeichen zwischen den zwei Spaltenwerten befindet, dann weist die neue Spalte den DT_WSTR-Datentyp und eine Länge von 101 auf. Sie können den Datentyp neuer Spalten aktualisieren. Als einzige Vorraussetzung muss der Datentyp mit den eingefügten Daten kompatibel sein. Wenn beispielsweise das Dialogfeld **Transformations-Editor für abgeleitete Spalte** eine Fehlermeldung generiert, müssen Sie einer Spalte mit einem integer-Datentyp einen Datenwert zuweisen. Abhängig von dem ausgewählten Datentyp, können Sie die Länge, Genauigkeit, Dezimalstellen und Codepage jeder Spalte angeben.  
  
### <a name="to-derive-column-values"></a>So leiten Sie Spaltenwerte ab  
  
1.  Öffnen Sie in [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)] das [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)]-Projekt mit dem gewünschten Paket.  
  
2.  Doppelklicken Sie im Projektmappen-Explorer auf das Paket, um es zu öffnen.  
  
3.  Klicken Sie auf die Registerkarte **Datenfluss** , und ziehen Sie dann aus **Toolbox** die Transformation für abgeleitete Spalten auf die Entwurfsoberfläche.  
  
4.  Verbinden Sie die Transformation für abgeleitete Spalten mit dem Datenfluss, indem Sie den Konnektor von der Quelle oder der vorherigen Transformation auf die Transformation für abgeleitete Spalten ziehen.  
  
5.  Doppelklicken Sie auf die Transformation für abgeleitete Spalten.  
  
6.  Erstellen Sie in **Transformations-Editor für abgeleitete Spalte** die Ausdrücke, die als Bedingungen verwendet werden sollen, indem Sie Variablen, Spalten, Funktionen und Operatoren in die **Ausdruck** -Spalte im Raster ziehen. Sie können den Ausdruck auch in die **Ausdruck** -Spalte eingeben.  
  
    > [!NOTE]  
    >  Falls der Ausdruck ungültig ist, wird der Ausdruckstext hervorgehoben dargestellt, und die Fehler werden in einer QuickInfo in der Spalte beschrieben.  
  
7.  Wählen Sie in der Liste **Abgeleitete Spalte** **\<add as new column>** aus, um das Auswertungsergebnis des Ausdrucks in eine neue Spalte zu schreiben, oder wählen Sie eine vorhandene Spalte aus, um sie mit dem Auswertungsergebnis zu aktualisieren.  
  
     Wenn Sie eine neue Spalte auswählen, wird mithilfe des Dialogfelds **Transformations-Editor für abgeleitete Spalte** der Ausdruck ausgewertet und ein Datentyp einer Spalte zugewiesen, abhängig von Datentyp, Länge, Genauigkeit, Dezimalzahlen und Codepage.  
  
8.  Falls Sie eine neue Spalte verwenden, wählen Sie in der Liste **Datentyp** einen Datentyp aus. Aktualisieren Sie in Abhängigkeit vom ausgewählten Datentyp die Werte in den Spalten **Länge**, **Genauigkeit**, **Dezimalstellen** und **Codepage** . Metadaten von vorhandenen Spalten können nicht geändert werden.  
  
9. Optional können Sie die Werte in der **Name der abgeleiteten Spalte** -Spalte ändern.  
  
10. Um die Fehlerausgabe zu konfigurieren, klicken Sie auf **Fehlerausgabe konfigurieren**. Weitere Informationen finden Sie unter [Debugging Data Flow](../../../integration-services/troubleshooting/debugging-data-flow.md).  
  
11. Klicken Sie auf **OK**.  
  
12. Klicken Sie im Menü **Datei** auf **Ausgewählte Elemente speichern** , um das aktualisierte Paket zu speichern.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Derived Column Transformation](../../../integration-services/data-flow/transformations/derived-column-transformation.md)   
 [SQL Server Integration Services-Datentypen](../../../integration-services/data-flow/integration-services-data-types.md)   
 [SQL Server Integration Services-Transformationen](../../../integration-services/data-flow/transformations/integration-services-transformations.md)   
 [SQL Server Integration Services-Pfade](../../../integration-services/data-flow/integration-services-paths.md)   
 [Datenflusstask](../../../integration-services/control-flow/data-flow-task.md)   
 [Integration Services-Ausdrücke &#40;SSIS&#41;](../../../integration-services/expressions/integration-services-ssis-expressions.md)  
  
  
