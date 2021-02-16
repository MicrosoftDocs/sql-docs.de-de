---
description: Analysieren von nicht standardmäßigen Textdateiformaten mit der Skriptkomponente
title: Analysieren von nicht standardmäßigen Textdateiformaten mit der Skriptkomponente | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- text file reading [Integration Services]
- Script component [Integration Services], non-standard text file formats
- transformations [Integration Services], components
- Script component [Integration Services], examples
ms.assetid: 1fda034d-09e4-4647-9a9f-e8d508c2cc8f
author: chugugrace
ms.author: chugu
ms.openlocfilehash: c48454ceec4f05f6b2386e4abb96092bbbbcb140
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344859"
---
# <a name="parsing-non-standard-text-file-formats-with-the-script-component"></a>Analysieren von nicht standardmäßigen Textdateiformaten mit der Skriptkomponente

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Wenn Ihre Quelldaten in einem nicht standardmäßigen Format angeordnet sind, dann könnte es u. U. praktischer sein, Ihre gesamte Parser-Logik in einem einzigen Skript zu konsolidieren, anstatt mehrere [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Transformationen miteinander zu verketten, um das gleiche Ergebnis zu erzielen.  
  
 [Beispiel 1: Analysieren von nach Zeilen getrennten Datensätzen](#example1)  
  
 [Beispiel 2: Teilen von übergeordneten und untergeordneten Datensätzen](#example2)  
  
> [!NOTE]  
>  Wenn Sie eine Komponente erstellen möchten, die Sie einfacher in mehreren Datenflusstasks und Paketen wiederverwenden können, empfiehlt es sich, den Code in diesem Skriptkomponentenbeispiel als Ausgangspunkt für eine benutzerdefinierte Datenflusskomponente zu verwenden. Weitere Informationen finden Sie unter [Entwickeln einer benutzerdefinierten Datenflusskomponente](../../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md).  
  
##  <a name="example-1-parsing-row-delimited-records"></a><a name="example1"></a> Beispiel 1: Analysieren von nach Zeilen getrennten Datensätzen  
 In diesem Beispiel wird gezeigt, wie eine Textdatei, in der sich jede Datenspalte auf einer separaten Zeile befindet, mithilfe der Skriptkomponente in eine Zieltabelle konvertiert werden kann.  
  
 Weitere Informationen zum Konfigurieren der Skriptkomponente für die Verwendung als Transformation im Datenfluss finden Sie unter [Erstellen einer synchronen Transformation mit der Skriptkomponente](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-synchronous-transformation-with-the-script-component.md) und [Erstellen einer asynchronen Transformation mit der Skriptkomponente](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-an-asynchronous-transformation-with-the-script-component.md) .  
  
#### <a name="to-configure-this-script-component-example"></a>So konfigurieren Sie dieses Skriptkomponentenbeispiel  
  
1.  Erstellen und speichern Sie eine Textdatei mit dem Namen **rowdelimiteddata.txt**, die die folgenden Quelldaten enthält:  
  
    ```  
    FirstName: Nancy  
    LastName: Davolio  
    Title: Sales Representative  
    City: Seattle  
    StateProvince: WA  
  
    FirstName: Andrew  
    LastName: Fuller  
    Title: Vice President, Sales  
    City: Tacoma  
    StateProvince: WA  
  
    FirstName: Steven  
    LastName: Buchanan  
    Title: Sales Manager  
    City: London  
    StateProvince:  
  
    ```  
  
2.  Öffnen Sie [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] , und stellen Sie eine Verbindung zu einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]her.  
  
3.  Wählen Sie eine Zieldatenbank aus, und öffnen Sie ein neues Abfragefenster. Führen Sie im Abfragefenster das folgende Skript aus, um die Zieltabelle zu erstellen:  
  
    ```sql
    create table RowDelimitedData  
    (  
    FirstName varchar(32),  
    LastName varchar(32),  
    Title varchar(32),  
    City varchar(32),  
    StateProvince varchar(32)  
    )  
  
    ```  
  
4.  Öffnen Sie [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], und erstellen Sie ein neues [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Paket mit dem Namen ParseRowDelim.dtsx.  
  
5.  Fügen Sie einen Verbindungs-Manager für Flatfiles zum Paket hinzu, benennen Sie ihn RowDelimitedData und konfigurieren Sie ihn, um eine Verbindung mit der Datei rowdelimiteddata.txt, die Sie in einem vorherigen Schritt erstellt haben, herzustellen.  
  
6.  Fügen Sie einen OLE DB-Verbindungsmanager zum Paket hinzu und konfigurieren Sie ihn, um eine Verbindung zu der Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und zur Datenbank, in der Sie die Zieltabelle erstellt haben, herzustellen.  
  
7.  Fügen Sie einen Datenflusstask zum Paket hinzu, und klicken Sie auf die Registerkarte **Datenfluss** des SSIS-Designers.  
  
8.  Fügen Sie eine Flatfilequelle zum Datenfluss hinzu und konfigurieren Sie sie, um den RowDelimitedData-Verbindungs-Manager zu verwenden. Wählen Sie auf der Seite **Spalten** des **Quellen-Editors für Flatfiles** die einzige verfügbare externe Spalte aus.  
  
9. Fügen Sie eine Skriptkomponente zum Datenfluss hinzu, und konfigurieren Sie sie als Transformation. Verbinden Sie die Ausgabe der Flatfilequelle mit der Skriptkomponente.  
  
10. Doppelklicken Sie auf die Skriptkomponente, um den **Transformations-Editor für Skripterstellung** anzuzeigen.  
  
11. Wählen Sie auf der Seite **Eingabespalten** des **Transformations-Editors für Skripterstellung** die einzelne verfügbare Eingabespalte.  
  
12. Wählen Sie auf der Seite **Eingaben und Ausgaben** des **Transformations-Editors für Skripterstellung** den Ausgang 0 aus, und legen Sie **SynchronousInputID** auf „Keine“ fest. Erstellen Sie 5 Ausgabespalten, die alle eine Typzeichenfolge [DT_STR] mit einer Länge von 32 aufweisen:  
  
    -   FirstName  
  
    -   LastName  
  
    -   Title  
  
    -   City  
  
    -   StateProvince  
  
13. Klicken Sie im **Transformations-Editor für Skripterstellung** auf der Seite **Skript** auf **Skript bearbeiten**, und geben Sie den in der **ScriptMain**-Klasse des Beispiels angegebenen Code ein. Schließen Sie die Skriptentwicklungsumgebung und den **Transformations-Editor für Skripterstellung**.  
  
14. Fügen Sie zum Datenfluss ein SQL Server-Ziel hinzu. Konfigurieren Sie es, um den OLE DB-Verbindungs-Manager und die RowDelimitedData-Tabelle zu verwenden. Verbinden Sie die Ausgabe der Skriptkomponente mit diesem Ziel.  
  
15. Führen Sie das Paket aus. Untersuchen Sie nach der Beendung des Pakets die Datensätze in der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Zieltabelle.  
  
```vb  
Public Overrides Sub Input0_ProcessInputRow(ByVal Row As Input0Buffer)  
  
    Dim columnName As String  
    Dim columnValue As String  
  
    ' Check for an empty row.  
    If Row.Column0.Trim.Length > 0 Then  
        columnName = Row.Column0.Substring(0, Row.Column0.IndexOf(":"))  
        ' Check for an empty value after the colon.  
        If Row.Column0.Substring(Row.Column0.IndexOf(":")).TrimEnd.Length > 1 Then  
            ' Extract the column value from after the colon and space.  
            columnValue = Row.Column0.Substring(Row.Column0.IndexOf(":") + 2)  
            Select Case columnName  
                Case "FirstName"  
                    ' The FirstName value indicates a new record.  
                    Me.Output0Buffer.AddRow()  
                    Me.Output0Buffer.FirstName = columnValue  
                Case "LastName"  
                    Me.Output0Buffer.LastName = columnValue  
                Case "Title"  
                    Me.Output0Buffer.Title = columnValue  
                Case "City"  
                    Me.Output0Buffer.City = columnValue  
                Case "StateProvince"  
                    Me.Output0Buffer.StateProvince = columnValue  
            End Select  
        End If  
    End If  
  
End Sub  
```  
  
```csharp  
public override void Input0_ProcessInputRow(Input0Buffer Row)  
    {  
  
        string columnName;  
        string columnValue;  
  
        // Check for an empty row.  
        if (Row.Column0.Trim().Length > 0)  
        {  
            columnName = Row.Column0.Substring(0, Row.Column0.IndexOf(":"));  
            // Check for an empty value after the colon.  
            if (Row.Column0.Substring(Row.Column0.IndexOf(":")).TrimEnd().Length > 1)  
            // Extract the column value from after the colon and space.  
            {  
                columnValue = Row.Column0.Substring(Row.Column0.IndexOf(":") + 2);  
                switch (columnName)  
                {  
                    case "FirstName":  
                        // The FirstName value indicates a new record.  
                        this.Output0Buffer.AddRow();  
                        this.Output0Buffer.FirstName = columnValue;  
                        break;  
                    case "LastName":  
                        this.Output0Buffer.LastName = columnValue;  
                        break;  
                    case "Title":  
                        this.Output0Buffer.Title = columnValue;  
                        break;  
                    case "City":  
                        this.Output0Buffer.City = columnValue;  
                        break;  
                    case "StateProvince":  
                        this.Output0Buffer.StateProvince = columnValue;  
                        break;  
                }  
            }  
        }  
  
    }  
```  
  
##  <a name="example-2-splitting-parent-and-child-records"></a><a name="example2"></a> Beispiel 2: Teilen von übergeordneten und untergeordneten Datensätzen  
 In diesem Beispiel wird gezeigt, wie eine Textdatei, in der eine Trennzeichenzeile einer übergeordneten Datenzeile vorausgeht, auf die eine endlose Anzahl an untergeordneten Datenzeilen folgt, mithilfe der Skriptkomponente in ordnungsgemäß normalisierte übergeordnete und untergeordnete Zieltabellen konvertiert werden kann. Dieses einfache Beispiel kann in abgewandelter Form problemlos auf Quelldateien angewendet werden, die mehr als eine Zeile oder Spalte für jeden über- und untergeordneten Datensatz verwenden, vorausgesetzt, der Anfang und das Ende jedes Datensatzes kann bestimmt werden.  
  
> [!CAUTION]  
>  Dieses Beispiel dient nur Demonstrationszwecken. Wenn Sie das Beispiel mehr als einmal ausführen, fügt es doppelte Schlüsselwerte in die Zieltabelle ein.  
  
 Weitere Informationen zum Konfigurieren der Skriptkomponente für die Verwendung als Transformation im Datenfluss finden Sie unter [Erstellen einer synchronen Transformation mit der Skriptkomponente](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-synchronous-transformation-with-the-script-component.md) und [Erstellen einer asynchronen Transformation mit der Skriptkomponente](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-an-asynchronous-transformation-with-the-script-component.md) .  
  
#### <a name="to-configure-this-script-component-example"></a>So konfigurieren Sie dieses Skriptkomponentenbeispiel  
  
1.  Erstellen und speichern Sie eine Textdatei mit dem Namen **parentchilddata.txt**, die die folgenden Quelldaten enthält:  
  
    ```  
    **********  
    PARENT 1 DATA  
    child 1 data  
    child 2 data  
    child 3 data  
    child 4 data  
    **********  
    PARENT 2 DATA  
    child 5 data  
    child 6 data  
    child 7 data  
    child 8 data  
    **********  
  
    ```  
  
2.  Öffnen Sie [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , und stellen Sie eine Verbindung zu einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]her.  
  
3.  Wählen Sie eine Zieldatenbank aus, und öffnen Sie ein neues Abfragefenster. Führen Sie im Abfragefenster das folgende Skript aus, um die Zieltabellen zu erstellen:  
  
    ```sql
    CREATE TABLE [dbo].[Parents]([ParentID] [int] NOT NULL,  
    [ParentRecord] [varchar](32) NOT NULL,  
     CONSTRAINT [PK_Parents] PRIMARY KEY CLUSTERED   
    ([ParentID] ASC))  
    GO  
    CREATE TABLE [dbo].[Children]([ChildID] [int] NOT NULL,  
    [ParentID] [int] NOT NULL,  
    [ChildRecord] [varchar](32) NOT NULL,  
     CONSTRAINT [PK_Children] PRIMARY KEY CLUSTERED   
    ([ChildID] ASC))  
    GO  
    ALTER TABLE [dbo].[Children] ADD CONSTRAINT [FK_Children_Parents] FOREIGN KEY([ParentID])  
    REFERENCES [dbo].[Parents] ([ParentID])  
  
    ```  
  
4.  Öffnen Sie [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], und erstellen Sie ein neues [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Paket mit dem Namen SplitParentChild.dtsx.  
  
5.  Fügen Sie einen Verbindungs-Manager für Flatfiles zum Paket hinzu, benennen Sie ihn ParentChildData und konfigurieren Sie ihn, um eine Verbindung mit der Datei parentchilddata.txt file, die Sie in einem vorherigen Schritt erstellt haben, herzustellen.  
  
6.  Fügen Sie einen OLE DB-Verbindungsmanager zum Paket hinzu und konfigurieren Sie ihn, um ihn zu der Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und zur Datenbank, in der Sie die Zieltabellen erstellt haben, hinzuzufügen.  
  
7.  Fügen Sie einen Datenflusstask zum Paket hinzu, und klicken Sie auf die Registerkarte **Datenfluss** des SSIS-Designers.  
  
8.  Fügen Sie eine Flatfilequelle zum Datenfluss hinzu und konfigurieren Sie sie, um den ParentChildData-Verbindungs-Manager zu verwenden. Wählen Sie auf der Seite **Spalten** des **Quellen-Editors für Flatfiles** die einzige verfügbare externe Spalte aus.  
  
9. Fügen Sie eine Skriptkomponente zum Datenfluss hinzu, und konfigurieren Sie sie als Transformation. Verbinden Sie die Ausgabe der Flatfilequelle mit der Skriptkomponente.  
  
10. Doppelklicken Sie auf die Skriptkomponente, um den **Transformations-Editor für Skripterstellung** anzuzeigen.  
  
11. Wählen Sie auf der Seite **Eingabespalten** des **Transformations-Editors für Skripterstellung** die einzelne verfügbare Eingabespalte.  
  
12. Wählen Sie auf der Seite **Eingaben und Ausgaben** im **Transformations-Editor für Skripterstellung** den Ausgang 0, benennen Sie ihn in „ParentRecords“ um, und legen Sie **SynchronousInputID** auf „Keine“ fest. Erstellen Sie 2 Ausgabespalten:  
  
    -   ParentID (der Primärschlüssel) vom Typ 4-Byte-Ganzzahl mit Vorzeichen [DT_I4]  
  
    -   ParentRecord vom Typ String [DT_STR] mit einer Länge von 32.  
  
13. Erstellen Sie eine zweite Ausgabe, und nennen Sie sie ChildRecords. Die Eigenschaft **SynchronousInputID** der neuen Ausgabe ist bereits auf „Keine“ festgelegt. Erstellen Sie 3 Ausgabespalten:  
  
    -   ChildID (der Primärschlüssel) vom Typ 4-Byte-Ganzzahl mit Vorzeichen [DT_I4]  
  
    -   ParentID (der Fremdschlüssel), ebenfalls vom Typ 4-Byte-Ganzzahl mit Vorzeichen [DT_I4]  
  
    -   ChildRecord vom Typ String [DT_STR] mit einer Länge von 50.  
  
14. Klicken Sie im **Transformations-Editor für Skripterstellung** auf der Seite **Skript** auf **Skript bearbeiten**. Geben Sie in der **ScriptMain**-Klasse den im Beispiel gezeigten Code ein. Schließen Sie die Skriptentwicklungsumgebung und den **Transformations-Editor für Skripterstellung**.  
  
15. Fügen Sie zum Datenfluss ein SQL Server-Ziel hinzu. Verbinden Sie den ParentRecords-Ausgang der Skriptkomponente mit diesem Ziel. Konfigurieren Sie den OLE DB-Verbindungs-Manager und die übergeordnete Tabelle.  
  
16. Fügen Sie zum Datenfluss ein weiteres SQL Server-Ziel hinzu. Verbinden Sie die ChildRecords-Ausgabe der Skriptkomponente mit diesem Ziel. Konfigurieren Sie es, um den OLE DB-Verbindungs-Manager und die Children-Tabelle zu verwenden.  
  
17. Führen Sie das Paket aus. Untersuchen Sie nach der Beendung des Pakets die übergeordneten und untergeordneten Datensätze in den beiden [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Zieltabellen.  
  
```vb  
Public Overrides Sub Input0_ProcessInputRow(ByVal Row As Input0Buffer)  
  
    Static nextRowIsParent As Boolean = False  
    Static parentCounter As Integer = 0  
    Static childCounter As Integer = 0  
  
    ' If current row starts with separator characters,  
    '  then following row contains new parent record.  
    If Row.Column0.StartsWith("***") Then  
        nextRowIsParent = True  
    Else  
        If nextRowIsParent Then  
            ' Current row contains parent record.  
            parentCounter += 1  
            Me.ParentRecordsBuffer.AddRow()  
            Me.ParentRecordsBuffer.ParentID = parentCounter  
            Me.ParentRecordsBuffer.ParentRecord = Row.Column0  
            nextRowIsParent = False  
        Else  
            ' Current row contains child record.  
            childCounter += 1  
            Me.ChildRecordsBuffer.AddRow()  
            Me.ChildRecordsBuffer.ChildID = childCounter  
            Me.ChildRecordsBuffer.ParentID = parentCounter  
            Me.ChildRecordsBuffer.ChildRecord = Row.Column0  
        End If  
    End If  
  
End Sub  
```  
  
```csharp  
public override void Input0_ProcessInputRow(Input0Buffer Row)  
    {  
  
    int static_Input0_ProcessInputRow_childCounter = 0;  
    int static_Input0_ProcessInputRow_parentCounter = 0;  
    bool static_Input0_ProcessInputRow_nextRowIsParent = false;  
  
        // If current row starts with separator characters,   
        // then following row contains new parent record.   
        if (Row.Column0.StartsWith("***"))  
        {  
            static_Input0_ProcessInputRow_nextRowIsParent = true;  
        }  
        else  
        {  
            if (static_Input0_ProcessInputRow_nextRowIsParent)  
            {  
                // Current row contains parent record.   
                static_Input0_ProcessInputRow_parentCounter += 1;  
                this.ParentRecordsBuffer.AddRow();  
                this.ParentRecordsBuffer.ParentID = static_Input0_ProcessInputRow_parentCounter;  
                this.ParentRecordsBuffer.ParentRecord = Row.Column0;  
                static_Input0_ProcessInputRow_nextRowIsParent = false;  
            }  
            else  
            {  
                // Current row contains child record.   
                static_Input0_ProcessInputRow_childCounter += 1;  
                this.ChildRecordsBuffer.AddRow();  
                this.ChildRecordsBuffer.ChildID = static_Input0_ProcessInputRow_childCounter;  
                this.ChildRecordsBuffer.ParentID = static_Input0_ProcessInputRow_parentCounter;  
                this.ChildRecordsBuffer.ChildRecord = Row.Column0;  
            }  
        }  
  
    }  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Erstellen einer synchronen Transformation mit der Skriptkomponente](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-synchronous-transformation-with-the-script-component.md)   
 [Erstellen einer asynchronen Transformation mit der Skriptkomponente](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-an-asynchronous-transformation-with-the-script-component.md)  
  
  
