---
title: Ausführen von SQL-Abfragen (SQLXML)
description: Erfahren Sie, wie Sie SQL-Abfragen mit verwalteten SQLXML-Klassen ausführen.
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- queries [SQLXML], SQLXML Managed Classes
- SQLXML Managed Classes, executing SQL queries
- Managed Classes [SQLXML], executing SQL queries
- ExecuteToStream method
- SQL queries [SQLXML]
ms.assetid: a561ae83-a8b6-4b9b-a819-9b86839546b4
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9a20d41c1002f9e17ef10dbb86feaef100ce293f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97414262"
---
# <a name="executing-sql-queries-sqlxml-managed-classes"></a>Ausführen von SQL-Abfragen (verwaltete SQLXML-Klassen)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  In diesem Beispiel wird Folgendes veranschaulicht:  
  
-   Erstellen von Parametern (SqlXmlParameter-Objekte).  
  
-   Zuweisen von Werten zu den Eigenschaften (Name und Wert) von SqlXmlParameter-Objekten.  
  
 In diesem Beispiel wird eine einfache SQL-Abfrage ausgeführt, um Vornamen, Nachnamen und Geburtsdatum des Mitarbeiters abzufragen, dessen Nachname als Parameterwert übergeben wird. Bei Angabe des-Parameters (*LastName*) wird nur die Value-Eigenschaft festgelegt. Die Name-Eigenschaft ist nicht festgelegt, da der-Parameter in dieser Abfrage positiontional ist und kein Name erforderlich ist.  
  
 Die CommandType-Eigenschaft des SqlXmlCommand-Objekts ist standardmäßig **SQL**. Deshalb wird die Eigenschaft nicht explizit festgelegt.  
  
> [!NOTE]  
>  Im Code müssen Sie den Namen der Instanz von Microsoft [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] in der Verbindungszeichenfolge bereitstellen.  
  
 Dies ist der C#-Code.  
  
```  
using System;  
  
using Microsoft.Data.SqlXml;  
using System.IO;  
class Test  
{  
      static string ConnString = "Provider=SQLOLEDB;Server=(local);database=AdventureWorks;Integrated Security=SSPI";  
      public static int testParams()  
      {  
         Stream strm;  
         SqlXmlParameter p;  
         SqlXmlCommand cmd = new SqlXmlCommand(ConnString);        
         cmd.CommandText = "SELECT FirstName, LastName FROM Person.Contact WHERE LastName=? For XML Auto";  
         p = cmd.CreateParameter();  
         p.Value = "Achong";  
         string strResult;  
         try   
         {  
            strm = cmd.ExecuteStream();  
            strm.Position = 0;  
            using(StreamReader sr = new StreamReader(strm))  
            {  
               Console.WriteLine(sr.ReadToEnd());  
            }  
         }  
         catch (SqlXmlException e)  
         {  
            //in case of an error, this prints error returned.  
            e.ErrorStream.Position=0;  
            strResult=new StreamReader(e.ErrorStream).ReadToEnd();  
            System.Console.WriteLine(strResult);  
         }  
  
         return 0;  
   }  
public static int Main(String[] args)  
{  
    testParams();  
    return 0;  
}  
}  
```  
  
### <a name="to-test-the-application"></a>So testen Sie die Anwendung  
  
1.  Speichern Sie den in diesem Thema bereitgestellten C#-Code (DocSample.cs) in einem Ordner.  
  
2.  Kompilieren Sie den Code. Verwenden Sie zur Kompilierung des Codes an der Eingabeaufforderung die folgende Zeichenfolge:  
  
    ```  
    csc /reference:Microsoft.Data.SqlXML.dll DocSample.cs  
    ```  
  
     Dadurch wird eine ausführbare Datei (DocSample.exe) erstellt.  
  
3.  Führen Sie DocSample.exe an der Eingabeaufforderung aus.  

 Um das Beispiel zu testen, muss auf dem Computer [!INCLUDE[msCoName](../../../includes/msconame-md.md)] .NET Framework installiert sein.  
  
 Statt die SQL-Abfragen als Befehlstext anzugeben, können Sie eine Vorlage festlegen (wie im folgenden Codefragment dargestellt), die ein Updategram ausführt (ebenfalls eine Vorlage), um einen Kundendatensatz einzufügen. Sie können Vorlagen und Updategrams in Dateien angeben und Dateien ausführen. Weitere Informationen finden Sie unter [Ausführen von Vorlagen Dateien mit der CommandText-Eigenschaft](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-template-files-by-using-the-commandtext-property.md).  
  
```  
SqlXmlCommand cmd = new SqlXmlCommand("Provider=SQLOLEDB;Data Source=SqlServerName;Initial Catalog=Database; Integrated Security=SSPI;");  
Stream stm;  
cmd.CommandType = SqlXmlCommandType.UpdateGram;  
cmd.CommandText = "<ROOT xmlns:sql='urn:schemas-microsoft-com:xml-sql' xmlns:updg='urn:schemas-microsoft-com:xml-updategram'>" +  
      "<updg:sync>" +  
       "<updg:before/>" +  
       "<updg:after>" +  
         "<Customer CustomerID='aaaaa' CustomerName='Some Name' CustomerTitle='SomeTitle' />" +  
       "</updg:after>" +  
       "</updg:sync>" +  
       "</ROOT>";  
  
stm = cmd.ExecuteStream();  
stm = null;  
cmd = null;  
```  
  
## <a name="using-executetostream"></a>Verwenden von ExecuteToStream  
 Wenn Sie über einen vorhandenen Stream verfügen, können Sie die executedestream-Methode verwenden, anstatt ein Stream-Objekt zu erstellen und die Execute-Methode zu verwenden. Der Code aus dem vorherigen Beispiel wird hier überarbeitet, um die executedestream-Methode zu verwenden:  
  
```  
using System;  
using Microsoft.Data.SqlXml;  
using System.IO;  
class Test  
{  
   static string ConnString = "Provider=SQLOLEDB;Server=SqlServerName;database=AdventureWorks;Integrated Security=SSPI;";  
   public static int testParams()  
   {  
      SqlXmlParameter p;  
      MemoryStream ms = new MemoryStream();  
      StreamReader sr = new StreamReader(ms);  
      ms.Position = 0;  
      SqlXmlCommand cmd = new SqlXmlCommand(ConnString);  
      cmd.CommandText = "select FirstName, LastName from Person.Contact where LastName = ? For XML Auto";  
      p = cmd.CreateParameter();  
      p.Value = "Achong";  
      cmd.ExecuteToStream(ms);  
      ms.Position = 0;  
      Console.WriteLine(sr.ReadToEnd());  
      return 0;        
   }  
   public static int Main(String[] args)  
   {  
      testParams();     
      return 0;  
   }  
}  
```  
  
> [!NOTE]  
>  Sie können auch die executexmlreadermethod verwenden, die ein XmlReader-Objekt zurückgibt. Weitere Informationen finden Sie unter [Ausführen von SQL-Abfragen mithilfe der ExecuteXmlReader-Methode](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-sql-queries-by-using-the-executexmlreader-method.md).  
  
  
