---
title: Verwenden eines externen Datasets mit Reporting Services | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie benutzerdefinierte Datenverarbeitungserweiterungen in den Reporting Services erstellen, damit Sie benutzerdefinierte DataSet-Objekte integrieren können, die von externen Anwendungen erstellt werden.
ms.date: 03/06/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- DataSet objects [Reporting Services]
- data processing extensions [Reporting Services], custom DataSet objects
- custom DataSet objects [Reporting Services]
- external DataSet objects [Reporting Services]
ms.assetid: 11daa013-ec17-4760-80e3-6d84cd8d5722
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: f34dee113abaf59f54db5983d39353bb45fc0641
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100060945"
---
# <a name="using-an-external-dataset-with-reporting-services"></a>Verwenden eines externen Datasets mit Reporting Services
  Das **DataSet**-Objekt ist wesentlich für die Unterstützung getrennter, verteilter Datenszenarios mit [!INCLUDE[vstecado](../../../includes/vstecado-md.md)]. Das **DataSet**-Objekt ist eine speicherresidente Datendarstellung, die unabhängig von der Datenquelle ein konsistentes relationales Programmiermodell zur Verfügung stellt. Es kann mit mehreren verschiedenen Datenquellen verwendet werden, mit XML-Daten oder zur Verwaltung lokaler Daten in einer Anwendung. Das **DataSet**-Objekt stellt ein komplettes Dataset dar, einschließlich verknüpfter Tabellen, Einschränkungen und Beziehungen zwischen den Tabellen. Da das **DataSet**-Objekt sehr vielseitig beim Speichern und Verfügbarmachen von Daten ist, kann es sein, dass Ihre Daten häufig verarbeitet und in ein **DataSet**-Objekt umgewandelt werden müssen, bevor diese Daten in Berichten erfasst werden können.  
  
 Mit den [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]-Datenverarbeitungserweiterungen können Sie jegliche benutzerdefinierten **DataSet**-Objekte integrieren, die von externen Anwendungen erstellt wurden. Hierzu erstellen Sie eine benutzerdefinierte Datenverarbeitungserweiterung in [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)], die als Brücke zwischen Ihrem **DataSet**-Objekt und dem Berichtsserver dient. Der Hauptteil des Codes für die Verarbeitung dieses **DataSet**-Objekts ist in der **DataReader**-Klasse enthalten, die Sie erstellen.  
  
 Der erste Schritt, das **DataSet**-Objekt für den Berichtsserver zugänglich zu machen, besteht in der Implementierung einer anbieterspezifischen Methode in Ihrer **DataReader**-Klasse, die Werte für ein **DataSet**-Objekt liefern kann. In folgendem Beispiel wird gezeigt, wie statische Daten in ein **DataSet**-Objekt geladen werden, indem eine anbieterspezifische Methode in Ihrer **DataReader**-Klasse verwendet wird.  
  
```vb  
'Private members of the DataReader class  
Private m_dataSet As System.Data.DataSet  
Private m_currentRow As Integer  
  
'Method to create a dataset  
Friend Sub CreateDataSet()  
   ' Create a dataset.  
   Dim ds As New System.Data.DataSet("myDataSet")  
   ' Create a data table.   
   Dim dt As New System.Data.DataTable("myTable")  
   ' Create a data column and set various properties.   
   Dim dc As New System.Data.DataColumn()  
   dc.DataType = System.Type.GetType("System.Decimal")  
   dc.AllowDBNull = False  
   dc.Caption = "Number"  
   dc.ColumnName = "Number"  
   dc.DefaultValue = 25  
   ' Add the column to the table.   
   dt.Columns.Add(dc)  
   ' Add 10 rows and set values.   
   Dim dr As System.Data.DataRow  
   Dim i As Integer  
   For i = 0 To 9  
      dr = dt.NewRow()  
      dr("Number") = i + 1  
      ' Be sure to add the new row to the DataRowCollection.   
      dt.Rows.Add(dr)  
   Next i  
  
   ' Fill the dataset.  
   ds.Tables.Add(dt)  
  
   ' Use a private variable to store the dataset in your  
   ' DataReader.  
   m_dataSet = ds  
  
   ' Set the current row to -1.  
   m_currentRow = - 1  
End Sub 'CreateDataSet  
```  
  
```csharp  
// Private members of the DataReader class  
private System.Data.DataSet m_dataSet;  
private int m_currentRow;  
  
// Method to create a dataset  
internal void CreateDataSet()  
{  
   // Create a dataset.  
   System.Data.DataSet ds = new System.Data.DataSet("myDataSet");  
   // Create a data table.   
   System.Data.DataTable dt = new System.Data.DataTable("myTable");  
   // Create a data column and set various properties.   
   System.Data.DataColumn dc = new System.Data.DataColumn();   
   dc.DataType = System.Type.GetType("System.Decimal");   
   dc.AllowDBNull = false;   
   dc.Caption = "Number";   
   dc.ColumnName = "Number";   
   dc.DefaultValue = 25;   
   // Add the column to the table.   
   dt.Columns.Add(dc);   
   // Add 10 rows and set values.   
   System.Data.DataRow dr;   
   for(int i = 0; i < 10; i++)  
   {   
      dr = dt.NewRow();   
      dr["Number"] = i + 1;   
      // Be sure to add the new row to the DataRowCollection.   
      dt.Rows.Add(dr);  
   }  
  
   // Fill the dataset.  
   ds.Tables.Add(dt);  
  
   // Use a private variable to store the dataset in your  
   // DataReader.  
   m_dataSet = ds;  
  
   // Set the current row to -1.  
   m_currentRow = -1;  
}  
```  
  
```csharp  
public bool Read()  
{  
   m_currentRow++;  
   if (m_currentRow >= m_dataSet.Tables[0].Rows.Count)   
   {  
      return (false);  
   }   
   else   
   {  
      return (true);  
   }  
}  
  
public int FieldCount  
{  
   // Return the count of the number of columns, which in  
   // this case is the size of the column metadata  
   // array.  
   get { return m_dataSet.Tables[0].Columns.Count; }  
}  
  
public string GetName(int i)  
{  
   return m_dataSet.Tables[0].Columns[i].ColumnName;  
}  
  
public Type GetFieldType(int i)  
{  
   // Return the actual Type class for the data type.  
   return m_dataSet.Tables[0].Columns[i].DataType;  
}  
  
public Object GetValue(int i)  
{  
   return m_dataSet.Tables[0].Rows[m_currentRow][i];  
}  
  
public int GetOrdinal(string name)  
{  
   // Look for the ordinal of the column with the same name and return it.  
   // Returns -1 if not found.  
   return m_dataSet.Tables[0].Columns[name].Ordinal;  
}  
```  
  
 Sobald Sie Ihr Dataset erstellt oder abgerufen haben, können Sie das **DataSet**-Objekt in Ihrer Implementierung der Elemente **Read**, **GetValue**, **GetName**, **GetOrdinal**, **GetFieldType** und **FieldCount** der Klasse **DataReader** verwenden.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Erweiterungen für Reporting Services](../../../reporting-services/extensions/reporting-services-extensions.md)   
 [Implementieren von Datenverarbeitungserweiterungen](../../../reporting-services/extensions/data-processing/implementing-a-data-processing-extension.md)   
 [Reporting Services Extension Library (Reporting Services-Erweiterungsbibliothek)](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
