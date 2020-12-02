---
description: Erkennen einer leeren flachen Datei mit dem Skripttask
title: Erkennen einer leeren flachen Datei mit dem Skripttask | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- flat files
- Script task [Integration Services], empty flat files
- SSIS Script task, empty flat files
- Script task [Integration Services], examples
ms.assetid: 1b4defb8-886a-483d-8056-d1b91d37bc90
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d5c1c5473eb307480b772d2cecd31b3ebb2a9bed
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/25/2020
ms.locfileid: "96122989"
---
# <a name="detecting-an-empty-flat-file-with-the-script-task"></a>Erkennen einer leeren flachen Datei mit dem Skripttask

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Die Flatfilequelle ermittelt vor dem Verarbeitungsversuch nicht, ob eine Flatfile Datenzeilen enthält. Möglicherweise möchten Sie die Effizienz eines Pakets verbessern, insbesondere bei Paketen, die eine Iteration durch zahlreiche Faltfiles durchführen. Dazu können Sie die Dateien auslassen, die keine Datenzeilen enthalten. Der Skripttask kann nach leeren Flatfiles suchen, bevor das Paket mit der Verarbeitung des Datenflusses beginnt.  
  
> [!NOTE]  
>  Wenn Sie einen Task erstellen möchten, den Sie einfacher in mehreren Paketen wiederverwenden können, empfiehlt es sich, den Code in diesem Skripttaskbeispiel als Ausgangspunkt für einen benutzerdefinierten Task zu verwenden. Weitere Informationen finden Sie unter [Entwickeln eines benutzerdefinierten Tasks](../../integration-services/extending-packages-custom-objects/task/developing-a-custom-task.md).  
  
## <a name="description"></a>BESCHREIBUNG  
 Im folgenden Beispiel wird mithilfe von Methoden des **System.IO**-Namespace das in einem Verbindungs-Manager für Flatfiles angegebene Flatfile getestet, um zu ermitteln, ob die Datei leer ist, oder ob sie nur erwartete Nichtdatenzeilen wie z.B. Spaltenkopfzeilen oder eine leere Zeile enthält. Das Skript prüft zuerst die Größe der Datei; bei einer Größe von 0 (null) Bytes ist die Datei leer. Ist die Datei größer als 0 (null), liest das Skript die Zeilen aus der Datei so lange, bis keine weiteren Zeilen mehr vorhanden sind, oder bis die Anzahl der Zeilen höher ist als die erwartete Anzahl der Nichtdatenzeilen. Ist die Anzahl der Zeilen der Datei kleiner oder gleich der erwarteten Anzahl der Nichtdatenzeilen, dann wird davon ausgegangen, dass die Datei leer ist. Das Ergebnis wird dann als boolescher Wert in einer Benutzervariablen zurückgegeben, deren Wert für die Verzweigung in der Paketablaufsteuerung verwendet werden kann. Die **FireInformation**-Methode zeigt das Ergebnis auch im Fenster **Ausgabe** der [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] Tools for Applications (VSTA) an.  
  
#### <a name="to-configure-this-script-task-example"></a>So konfigurieren Sie dieses Skripttaskbeispiel  
  
1.  Erstellen und konfigurieren Sie einen Verbindungs-Manager für Flatfiles mit dem Namen **EmptyFlatFileTest**.  
  
2.  Erstellen Sie eine ganzzahlige Variable mit dem Namen `FFNonDataRows` und legen sie ihren Wert auf die Anzahl der in der Flatfile erwarteten Nichtdatenzeilen fest.  
  
3.  Erstellen Sie eine boolesche Variable mit dem Namen `FFIsEmpty`.  
  
4.  Fügen Sie die `FFNonDataRows`-Variable der Eigenschaft **ReadOnlyVariables** des Skripttasks hinzu.  
  
5.  Fügen Sie die `FFIsEmpty`-Variable der Eigenschaft **ReadWriteVariables** des Skripttasks hinzu.  
  
6.  Importieren Sie in Ihrem Code den **System.IO**-Namespace.  
  
 Bei der Iteration durch Dateien mit einem Foreach-Schleifen-Editor müssen Sie anstelle der Verwendung eines einzelnen Flatfile-Verbindungs-Managers den unten aufgeführten Beispielcode ändern, um den Namen und Pfad der Datei von der Variable, in der der Enumerationswert gespeichert ist, und nicht vom Verbindungsmanager, zu erhalten.  
  
### <a name="code"></a>Code  
  
```vb  
Public Sub Main()  
  
  Dim nonDataRows As Integer = _  
      DirectCast(Dts.Variables("FFNonDataRows").Value, Integer)  
  Dim ffConnection As String = _  
      DirectCast(Dts.Connections("EmptyFlatFileTest").AcquireConnection(Nothing), _  
      String)  
  Dim flatFileInfo As New FileInfo(ffConnection)  
  ' If file size is 0 bytes, flat file does not contain data.  
  Dim fileSize As Long = flatFileInfo.Length  
  If fileSize > 0 Then  
    Dim lineCount As Integer = 0  
    Dim line As String  
    Dim fsFlatFile As New StreamReader(ffConnection)  
    Do Until fsFlatFile.EndOfStream  
      line = fsFlatFile.ReadLine  
      lineCount += 1  
      ' If line count > expected number of non-data rows,  
      '  flat file contains data (default value).  
      If lineCount > nonDataRows Then  
        Exit Do  
      End If  
      ' If line count <= expected number of non-data rows,  
      '  flat file does not contain data.  
      If lineCount <= nonDataRows Then  
        Dts.Variables("FFIsEmpty").Value = True  
      End If  
    Loop  
  Else  
    Dts.Variables("FFIsEmpty").Value = True  
  End If  
  
  Dim fireAgain As Boolean = False  
  Dts.Events.FireInformation(0, "Script Task", _  
      String.Format("{0}: {1}", ffConnection, _  
      Dts.Variables("FFIsEmpty").Value.ToString), _  
      String.Empty, 0, fireAgain)  
  
  Dts.TaskResult = ScriptResults.Success  
  
End Sub  
```  
  
```csharp  
public void Main()  
        {  
  
            int nonDataRows = (int)(Dts.Variables["FFNonDataRows"].Value);  
            string ffConnection = (string)(Dts.Connections["EmptyFlatFileTest"].AcquireConnection(null) as String);  
            FileInfo flatFileInfo = new FileInfo(ffConnection);  
            // If file size is 0 bytes, flat file does not contain data.  
            long fileSize = flatFileInfo.Length;  
            if (fileSize > 0)  
            {  
  
                int lineCount = 0;  
                string line;  
                StreamReader fsFlatFile = new StreamReader(ffConnection);  
                while (!(fsFlatFile.EndOfStream))  
                {  
                    Console.WriteLine (fsFlatFile.ReadLine());  
                    lineCount += 1;  
                    // If line count > expected number of non-data rows,  
                    //  flat file contains data (default value).  
                    if (lineCount > nonDataRows)  
                    {  
                        break;  
                    }  
                    // If line count <= expected number of non-data rows,  
                    //  flat file does not contain data.  
                    if (lineCount <= nonDataRows)  
                    {  
                        Dts.Variables["FFIsEmpty"].Value = true;  
                    }  
                }  
            }  
            else  
            {  
                Dts.Variables["FFIsEmpty"].Value = true;  
            }  
  
            bool fireAgain = false;  
            Dts.Events.FireInformation(0, "Script Task", String.Format("{0}: {1}", ffConnection, Dts.Variables["FFIsEmpty"].Value), String.Empty, 0, ref fireAgain);  
  
            Dts.TaskResult = (int)ScriptResults.Success;  
  
        }  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Skripttask-Beispiele](../../integration-services/extending-packages-scripting-task-examples/script-task-examples.md)  
  
  
