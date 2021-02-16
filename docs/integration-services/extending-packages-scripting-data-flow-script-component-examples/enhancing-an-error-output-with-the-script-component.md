---
description: Erweitern einer Fehlerausgabe mit der Skriptkomponente
title: Erweitern einer Fehlerausgabe mit der Skriptkomponente | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/04/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- transformations [Integration Services], components
- Script component [Integration Services], examples
- error outputs [Integration Services], enhancing
- Script component [Integration Services], transformation components
ms.assetid: f7c02709-f1fa-4ebd-b255-dc8b81feeaa5
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 0e3fee4ea309a5cc48b938abd50f846f16358412
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344904"
---
# <a name="enhancing-an-error-output-with-the-script-component"></a>Erweitern einer Fehlerausgabe mit der Skriptkomponente

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Standardmäßig enthalten die beiden zusätzlichen Spalten in einer [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Fehlerausgabe – ErrorCode und ErrorColumn – nur numerische Codes, die eine Fehlernummer darstellen, und die ID der Spalte, in der der Fehler auftrat. Diese numerischen Werte sind ohne die entsprechende Fehlerbeschreibung und den Spaltennamen nur von begrenztem Nutzen.  
  
 In diesem Thema wird beschrieben, wie Sie mithilfe der Skriptkomponente den im Datenfluss vorhandenen Fehlerausgabedaten die Fehlerbeschreibung und den Spaltennamen hinzufügen. Im Beispiel wird die Fehlerbeschreibung einem bestimmten vordefinierten [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Fehlercode hinzugefügt. Dazu wird die <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.GetErrorDescription%2A>-Methode der <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100>-Schnittstelle verwendet, die von der <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.ComponentMetaData%2A>-Eigenschaft der Skriptkomponente bereitgestellt wird. Dann wird im Beispiel mithilfe der <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData130.GetIdentificationStringByID%2A>-Methode derselben Schnittstelle der Spaltenname hinzugefügt, der der erfassten Herkunfts-ID entspricht.  
  
> [!NOTE]  
>  Wenn Sie eine Komponente erstellen möchten, die Sie einfacher in mehreren Datenflusstasks und Paketen wiederverwenden können, empfiehlt es sich, den Code in diesem Skriptkomponentenbeispiel als Ausgangspunkt für eine benutzerdefinierte Datenflusskomponente zu verwenden. Weitere Informationen finden Sie unter [Entwickeln einer benutzerdefinierten Datenflusskomponente](../../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md).  
  
## <a name="example"></a>Beispiel  
 In diesem Beispiel wird gezeigt, wie mit einer Skriptkomponente, die als Transformation konfiguriert ist, den im Datenfluss vorhandenen Fehlerausgabedaten die Fehlerbeschreibung und der Spaltenname hinzugefügt werden.  
  
 Weitere Informationen zum Konfigurieren der Skriptkomponente für die Verwendung als Transformation im Datenfluss finden Sie unter [Erstellen einer synchronen Transformation mit der Skriptkomponente](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-synchronous-transformation-with-the-script-component.md) und [Erstellen einer asynchronen Transformation mit der Skriptkomponente](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-an-asynchronous-transformation-with-the-script-component.md) .  
  
#### <a name="to-configure-this-script-component-example"></a>So konfigurieren Sie dieses Skriptkomponentenbeispiel  
  
1.  Konfigurieren Sie vor dem Erstellen der neuen Skriptkomponente eine Upstreamkomponente im Datenfluss, um Zeilen zur zugehörigen Fehlerausgabe umzuleiten, sobald ein Fehler auftritt oder Daten abgeschnitten werden. Zu Testzwecken sollten Sie eine Komponente so konfigurieren, dass auf jeden Fall Fehler auftreten, beispielsweise, indem Sie eine Transformation zum Suchen zwischen zwei Tabellen konfigurieren, und die Suche einen Fehler meldet.  
  
2.  Fügen Sie der Datenfluss-Designeroberfläche eine neue Skriptkomponente hinzu, und konfigurieren Sie sie als Transformation.  
  
3.  Verbinden Sie die Fehlerausgabe der Upstreamkomponente mit der neuen Skriptkomponente.  
  
4.  Öffnen Sie den **Transformations-Editor für Skripterstellung**, und wählen Sie auf der Seite **Skript** für die **ScriptLanguage**-Eigenschaft die Skriptsprache aus.  
  
5.  Klicken Sie auf **Skript bearbeiten**, um die IDE der [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]-Tools for Applications (VSTA) zu öffnen und den unten dargestellten Beispielcode einzufügen.  
  
6.  Schließen Sie VSTA.  
  
7.  Wählen Sie im Transformations-Editor für Skripterstellung auf der Seite **Eingabespalten** die Spalten ErrorCode und ErrorColumn aus.  
  
8.  Fügen Sie auf der Seite **Eingaben und Ausgaben** zwei neue Spalten hinzu.  
  
    -   Fügen Sie eine neue Ausgabespalte des Typs **Zeichenfolge** mit dem Namen **ErrorDescription** hinzu. Vergrößern Sie die Standardlänge der neuen Spalte auf 255, damit auch lange Meldungen angezeigt werden können.  
  
    -   Fügen Sie eine weitere neue Ausgabespalte des Typs **Zeichenfolge** mit dem Namen **ColumnName** hinzu. Vergrößern Sie die Standardlänge der neuen Spalte auf 255, damit auch lange Werte angezeigt werden können.  
  
9. Schließen Sie den **Transformations-Editor für Skripterstellung**.  
  
10. Fügen Sie die Ausgabe der Skriptkomponente an ein geeignetes Ziel an. Ein Flatfileziel ist die einfachste Möglichkeit zur Konfiguration bei Ad-hoc-Tests.  
  
11. Führen Sie das Paket aus.  

```vb
Public Class ScriptMain      ' VB
    Inherits UserComponent
    Public Overrides Sub Input0_ProcessInputRow(ByVal Row As Input0Buffer)

        Row.ErrorDescription = _
            Me.ComponentMetaData.GetErrorDescription(Row.ErrorCode)

        Dim componentMetaData130 = TryCast(Me.ComponentMetaData, IDTSComponentMetaData130)

        If componentMetaData130 IsNot Nothing Then

            If Row.ErrorColumn = 0 Then
                ' 0 means no specific column is identified by ErrorColumn, this time.
                Row.ColumnName = "Check the row for a violation of a foreign key constraint."
            ELSE If Row.ErrorColumn = -1 Then
                ' -1 means you are using Table Lock for a Memory Optimised destination table which is not supported.
                Row.ColumnName = "Table lock is not compatible with Memory Optimised tables."
            Else
                Row.ColumnName = componentMetaData130.GetIdentificationStringByID(Row.ErrorColumn)
            End If
        End If
    End Sub
End Class
```

```csharp
public class ScriptMain:      // C#
    UserComponent
{
    public override void Input0_ProcessInputRow(Input0Buffer Row)
    {
        Row.ErrorDescription = this.ComponentMetaData.GetErrorDescription(Row.ErrorCode);

        var componentMetaData130 = this.ComponentMetaData as IDTSComponentMetaData130;
        if (componentMetaData130 != null)
        {
            // 0 means no specific column is identified by ErrorColumn, this time.
            if (Row.ErrorColumn == 0)
            {
                Row.ColumnName = "Check the row for a violation of a foreign key constraint.";
            }
            // -1 means you are using Table Lock for a Memory Optimised destination table which is not supported.
            else if (Row.ErrorColumn == -1)
            {
                Row.ColumnName = "Table lock is not compatible with Memory Optimised tables.";
            }
            else
            {
                Row.ColumnName = componentMetaData130.GetIdentificationStringByID(Row.ErrorColumn);
            }
        }
    }
}
```

## <a name="see-also"></a>Siehe auch  
 [Fehlerbehandlung in Daten](../../integration-services/data-flow/error-handling-in-data.md)   
 [Verwenden von Fehlerausgaben in einer Datenflusskomponente](../../integration-services/extending-packages-custom-objects/data-flow/using-error-outputs-in-a-data-flow-component.md)   
 [Erstellen einer synchronen Transformation mit der Skriptkomponente](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-synchronous-transformation-with-the-script-component.md)   
  
  
