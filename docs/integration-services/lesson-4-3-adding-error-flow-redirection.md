---
description: 'Lektion 4.3: Hinzufügen der Fehlerflussumleitung'
title: 'Schritt 3: Hinzufügen der Fehlerflussumleitung | Microsoft-Dokumentation'
ms.custom: ''
ms.date: 01/07/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: 5683a45d-9e73-4cd5-83ca-fae8b26b488c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: ac63d922423bcbb9c0c7f513d34a5834b3f0c1b3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352024"
---
# <a name="lesson-4-3-add-error-flow-redirection"></a>Lektion 4.3: Hinzufügen der Fehlerflussumleitung

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



Wie in der vorhergehenden Aufgabe gezeigt, kann von der Lookup Currency Key-Transformation keine Übereinstimmung generiert werden, wenn von der Transformation die beschädigte Beispielflatfile verarbeitet werden soll, weil Letztere einen Fehler verursacht. Da die Transformation die Standardeinstellungen für die Fehlerausgabe verwendet, führt jeder Fehler dazu, dass die Transformation fehlschlägt. Wenn die Transformation fehlschlägt, schlägt auch der Rest des Pakets fehl.  
  
Anstatt ein Fehlschlagen der Transformation zuzulassen, können Sie die Komponente so konfigurieren, dass die fehlerverursachende Zeile mithilfe der Fehlerausgabe zu einem anderen Verarbeitungspfad umgeleitet wird. Die Nutzung eines separaten Verarbeitungspfads bietet mehr Optionen. Sie können beispielsweise die Daten bereinigen und dann die fehlerverursachende Zeile neu verarbeiten. Alternativ können Sie die fehlerverursachende Zeile mit den zugehörigen Fehlerinformationen speichern, um später eine Überprüfung und Neuverarbeitung durchzuführen.  
  
In dieser Aufgabe konfigurieren Sie die Lookup Currency Key-Transformation so, dass alle fehlerhaften Zeilen in die Fehlerausgabe umgeleitet werden. In der Fehlerverzweigung des Datenflusses schreibt [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] diese Zeilen in eine Datei.  
  
Standardmäßig enthalten die beiden zusätzlichen Spalten **ErrorCode** und **ErrorColumn** in einer [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]-Fehlerausgabe nur einen numerischen Fehlercode und die ID der Spalte, in der der Fehler aufgetreten ist. In dieser Aufgabe greifen Sie mithilfe einer Skriptkomponente auf die [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]-API zu und rufen eine Beschreibung des Fehlers ab, bevor das Paket die fehlerverursachenden Zeilen in die Datei schreibt.  
  
## <a name="configure-an-error-output"></a>Konfigurieren einer Fehlerausgabe  
  
1.  Erweitern Sie in der **SSIS-Toolbox** die Option **Allgemein**, und ziehen Sie anschließend **Skriptkomponente** auf die Entwurfsoberfläche der Registerkarte **Datenfluss** . Legen Sie **Skript** rechts von der **Lookup Currency Key** -Transformation ab.  
  
2.  Klicken Sie im Dialogfeld **Skriptkomponententyp auswählen** auf **Transformation** und anschließend auf **OK**.  
  
3.  Klicken Sie auf die **Lookup Currency Key**-Transformation, und ziehen Sie anschließend den roten Pfeil auf die neue Transformation **Skript**, um die beiden Komponenten zu verbinden.  
  
    Der rote Pfeil stellt die Fehlerausgabe der **Lookup Currency Key** -Transformation dar. Indem Sie den roten Pfeil zum Verbinden der Transformation mit der Skriptkomponente verwenden, werden alle Verarbeitungsfehler in die Skriptkomponente umgeleitet. Diese verarbeitet dann die Fehler und sendet sie an das Ziel.  
  
4.  Markieren Sie im Dialogfeld **Fehlerausgabe konfigurieren** in der **Fehler**-Spalte **Zeile umleiten**, und klicken Sie anschließend auf **OK**.  
  
5.  Klicken Sie auf der **Datenfluss**-Entwurfsoberfläche in der neu hinzugefügten **Skriptkomponente** auf den Namen **Skriptkomponente**, und ändern Sie den Namen in **Fehlerbeschreibung abrufen**.  
  
6.  Doppelklicken Sie auf die **Get Error Description** -Transformation.  
  
7.  Wählen Sie im Dialogfeld **Transformations-Editor für Skripterstellung** auf der Seite **Eingabespalten** die **ErrorCode** -Spalte aus.  
  
8.  Erweitern Sie auf der Seite **Eingaben und Ausgaben** das Element **Ausgabe 0**, klicken Sie auf **Ausgabespalten** und anschließend auf **Spalte hinzufügen**.  
  
9. Geben Sie in der **Name**-Eigenschaft *Fehlerbeschreibung* ein, und legen Sie die **DataType**-Eigenschaft auf **Unicode string [DT_WSTR]** (Unicode-Zeichenfolge [DT_WSTR]) fest.  
  
10. Überprüfen Sie auf der Seite **Skript**, ob die **LocaleID**-Eigenschaft auf **Englisch (USA)** festgelegt ist.
  
11. Klicken Sie auf **Skript bearbeiten**, um [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[vsprvs](../includes/vsprvs-md.md)] Tools for Applications (VSTA) zu öffnen. Geben Sie in der Methode **Input0_ProcessInputRow** den folgenden Code ein, oder fügen Sie ihn ein:  
  
    [Visual Basic]  
  
    ```vb  
    Row.ErrorDescription =   
      Me.ComponentMetaData.GetErrorDescription(Row.ErrorCode)  
    ```  
  
    [Visual C#]  
  
    ```cs
    Row.ErrorDescription = this.ComponentMetaData.GetErrorDescription(Row.ErrorCode);  
    ```  
  
    Die vollständige Unterroutine enthält den folgenden Code:  
  
    [Visual Basic]  
  
    ```vb
    Public Overrides Sub Input0_ProcessInputRow(ByVal Row As Input0Buffer)  
  
      Row.ErrorDescription =   
        Me.ComponentMetaData.GetErrorDescription(Row.ErrorCode)  
  
    End Sub  
    ```  
  
    [Visual C#]  
  
    ```cs
    public override void Input0_ProcessInputRow(Input0Buffer Row)  
        {  
  
            Row.ErrorDescription = this.ComponentMetaData.GetErrorDescription(Row.ErrorCode);  
  
        }  
    ```  
  
12. Klicken Sie im Menü **Erstellen** auf **Projektmappe erstellen**, um das Skript zu erstellen und die Änderungen zu speichern, und schließen Sie anschließend VSTA.  
  
13. Klicken Sie auf **OK**, um das Dialogfeld **Transformations-Editor für Skripterstellung** zu schließen.  
  
## <a name="go-to-next-task"></a>Weiter zur nächsten Aufgabe
[Schritt 4: Hinzufügen eines Flatfileziels](../integration-services/lesson-4-4-adding-a-flat-file-destination.md)  
  
  
  
