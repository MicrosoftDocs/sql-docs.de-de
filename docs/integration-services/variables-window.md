---
description: Variablen (Fenster)
title: Fenster „Variablen“ | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.variables.f1
- sql13.dts.designer.variableoptionswindow.f1
helpviewer_keywords:
- Variables Window dialog box
ms.assetid: f405e5ce-ef69-4c58-8c7d-a3d44dfe9ab0
author: chugugrace
ms.author: chugu
ms.openlocfilehash: aa326be8b57eed58f0aa52876d0d32b889320c58
ms.sourcegitcommit: cfa04a73b26312bf18d8f6296891679166e2754d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2020
ms.locfileid: "92193739"
---
# <a name="variables-window"></a>Variablen (Fenster)

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


  Verwenden Sie das Fenster **Variablen** , um benutzerdefinierte Variablen zu erstellen und zu ändern sowie um Systemvariablen anzuzeigen.  
  
 Das Fenster **Variablen** befindet sich in **standardmäßig im SSIS-Designer unterhalb des Bereichs** Verbindungs-Manager [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)]. Wenn das Fenster **Variablen** nicht angezeigt wird, klicken Sie im Menü **SSIS** auf **Variablen**, um das Fenster einzublenden.  
  
 Sie können das Fenster **Variablen** auch anzeigen, indem Sie im Dialogfeld **Optionen** auf der Seite **Tastatur** den Befehl View.Variables einer beliebigen Tastenkombination zuordnen.  
  
> [!NOTE]
>  Die Werte der Eigenschaften **Name** und **Namespace** müssen mit einem Buchstaben beginnen, wie in Unicode-Standard 2.0 definiert ist, oder mit einem Unterstrich (_). Bei den nachfolgenden Zeichen kann es sich um Buchstaben oder Zahlen gemäß Unicode-Standard 2.0 oder um einem Unterstrich (\_) handeln.  
  
## <a name="options"></a>Optionen  
 **Variable hinzufügen**  
 Fügt eine benutzerdefinierte Variable hinzu.  
  
 **Variable verschieben**  
 Klicken Sie in der Liste auf eine Variable, und klicken Sie dann auf **Variable verschieben** , um den Gültigkeitsbereich der Variablen zu ändern. Wählen Sie im Dialogfeld **Neuen Bereich auswählen** das Paket oder einen Container, Task oder Ereignishandler im Paket aus, um den Gültigkeitsbereich der Variablen zu ändern.  
  
 Weitere Informationen zu Variablenbereichen finden Sie unter [Integration Services-Variablen &#40;SSIS&#41;](../integration-services/integration-services-ssis-variables.md).  
  
 **Variable löschen**  
 Wählen Sie in der Liste eine Variable aus, und klicken Sie auf **Variable löschen**.  
  
 **Rasteroptionen**  
 Klicken Sie auf diese Option, um das Dialogfeld **Variable Rasteroptionen** zu öffnen. Hier können Sie die Spaltenauswahl ändern und Filter auf das Fenster **Variablen** anwenden. Weitere Informationen finden Sie unter [Variable Rasteroptionen]().  
  
 **Name**  
 Zeigt den Variablennamen an. Bei benutzerdefinierten Variablen können Sie den Namen der Variablen aktualisieren.  
  
 **Umfang**  
 Zeigt den Bereich der Variablen an. Eine Variable verfügt entweder über den Bereich des gesamten Pakets oder den Bereich eines Containers bzw. Tasks. Der Bereich der Variable muss ausreichend sein, sodass die Variable für alle anderen Tasks oder Komponenten sichtbar ist, die ihren Wert lesen oder festlegen müssen.  
  
 Sie können den Bereich ändern, indem Sie auf die Variable klicken und dann im Fenster **Variablen** auf **Variable verschieben** klicken.  
  
 **Datentyp**  
 Zeigt den Datentyp der Variablen an. Bei benutzerdefinierten Variablen können Sie einen Datentyp in der Liste auswählen.  
  
> [!NOTE]  
>  Wenn Sie der Variable einen Ausdruck zuweisen, können Sie den Datentyp nicht ändern.  
  
 **Wert**  
 Zeigt den Wert der Variablen an. Bei benutzerdefinierten Variablen können Sie den Wert aktualisieren. Dieser Wert kann ein Literal oder ein Ausdruck sein, und der Wert kann eine mehrzeilige Zeichenfolge sein. Um der Variable einen Ausdruck zuzuweisen, klicken Sie auf die Schaltfläche mit den Auslassungspunkten neben der Spalte **Ausdruck** im Fenster **Variablen** .  
  
 **Namespace**  
 Zeigt den Namen des Namespaces an. Benutzerdefinierte Variablen werden anfangs im **User** -Namespace erstellt. Sie können den Namespace jedoch im Feld **Namespace** ändern. Klicken Sie zum Anzeigen dieser Spalte auf **Rasteroptionen**.  
  
 **Ereignis bei Änderung des Variablenwertes auslösen**  
 Gibt an, ob das **OnVariableValueChanged** -Ereignis ausgelöst werden soll, wenn ein Wert geändert wird. Bei benutzerdefinierten und Systemvariablen können Sie den Wert aktualisieren. Standardmäßig wird diese Spalte nicht im Fenster **Variablen** angezeigt. Klicken Sie zum Anzeigen dieser Spalte auf **Rasteroptionen**.  
  
 **Beschreibung**  
 Anzeigen der Variablenbeschreibung. Sie können die Beschreibung für benutzerdefinierte Variablen ändern. Standardmäßig wird diese Spalte nicht im Fenster **Variablen** angezeigt. Klicken Sie zum Anzeigen dieser Spalte auf **Rasteroptionen**.  
  
 **Ausdruck**  
 Anzeigen des der Variable zugewiesenen Ausdrucks. Klicken Sie auf die Schaltfläche mit den Auslassungspunkten, um einen Ausdruck zuzuweisen.  
  
 Wenn Sie einer Variablen einen Ausdruck zuweisen, wird ein spezieller Symbolmarker neben der Variablen angezeigt. Dieser spezielle Symbolmarker wird auch neben Verbindungs-Managern und Tasks angezeigt, für die Ausdrücke festgelegt wurden.  

## <a name="variable-grid-options-dialog-box"></a>Dialogfeld „Variable Rasteroptionen“
 Im Dialogfeld **Variable Rasteroptionen** können Sie die Spalten auswählen, die im Fenster **Variablen** angezeigt werden, und die Filter auswählen, die auf die Liste der Variablen angewendet werden sollen. Weitere Informationen über die Eigenschaften der zugehörigen Variablen finden Sie unter [Integration Services-Variablen &#40;SSIS&#41;](../integration-services/integration-services-ssis-variables.md).  
  
### <a name="options-for-filter"></a>Optionen für Filter  
 **Systemvariablen anzeigen**  
 Wählen Sie diese Option aus, um Systemvariablen im Fenster **Variablen** aufzulisten. Systemvariablen sind vordefiniert. Systemvariablen können nicht hinzugefügt oder gelöscht werden. Sie können die Eigenschafteneinstellung **RaiseChangedEvent** ändern.  
  
 Diese Liste ist mit Farbcodes versehen. Systemvariablen werden grau, benutzerdefinierte Variablen werden schwarz angezeigt.  
  
 **Variablen aller Bereiche anzeigen**  
 Wählen Sie diese Option aus, um Variablen innerhalb des Bereichs des Pakets und innerhalb des Bereichs von Containern, Tasks und Ereignishandlern im Paket anzuzeigen. Deaktivieren Sie diese Option, um nur Variablen innerhalb des Bereichs des Pakets und innerhalb des Bereichs eines ausgewählten Containers, Tasks oder Ereignishandlers anzuzeigen.  
  
 Weitere Informationen zu Variablenbereichen finden Sie unter [Integration Services-Variablen &#40;SSIS&#41;](../integration-services/integration-services-ssis-variables.md).  
  
### <a name="options-for-columns"></a>Optionen für Spalten  
 Wählen Sie die Spalten aus, die im Fenster **Variablen** angezeigt werden sollen.  
  
-   **Umfang**  
  
-   **Datentyp**  
  
-   **Wert**  
  
-   **Namespace**  
  
-   **Ereignis bei Änderung des Variablenwertes auslösen**  
  
-   **Beschreibung**  
  
-   **Ausdruck**  
  
## <a name="see-also"></a>Weitere Informationen  
 [Integration Services-Variablen &#40;SSIS&#41;](../integration-services/integration-services-ssis-variables.md)   
 [Verwenden von Variablen in Paketen](./integration-services-ssis-variables.md)   
 [Integration Services-Ausdrücke &#40;SSIS&#41;](../integration-services/expressions/integration-services-ssis-expressions.md)   
 [Generieren von Dumpdateien für die Paketausführung](../integration-services/troubleshooting/generating-dump-files-for-package-execution.md)  
  
