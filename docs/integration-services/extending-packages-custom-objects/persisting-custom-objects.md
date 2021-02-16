---
description: Beibehalten von benutzerdefinierten Objekten
title: Beibehalten von benutzerdefinierten Objekten | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- custom objects [Integration Services], persisting
ms.assetid: 97c19716-6447-4c1c-b277-cc2e6c1e6a6c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 99f1f97ef2165aff97f9f45d667c95677180815f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354360"
---
# <a name="persisting-custom-objects"></a>Beibehalten von benutzerdefinierten Objekten

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Sie müssen keine benutzerdefinierte Persistenz für die von Ihnen erstellten benutzerdefinierten Objekte einfügen, solange ihre Eigenschaften nur einfache Datentypen wie **integer** und **string** verwenden. Die Standardimplementierung der Persistenz speichert die Metadaten Ihres Objekts zusammen mit den Werten aller Eigenschaften.  
  
 Falls Ihr Objekt jedoch Eigenschaften hat, die komplexe Datentypen verwenden, oder wenn Sie benutzerdefinierte Verarbeitung beim Laden und Speichern von Eigenschaftswerten durchführen möchten, können Sie die <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentPersist>-Schnittstelle und ihre Methoden <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentPersist.LoadFromXML%2A> und <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentPersist.SaveToXML%2A> implementieren. In diesen Methoden laden Sie aus der XML-Definition des Pakets (oder speichern darin) ein XML-Fragment, das die Eigenschaften des Objekts und ihre aktuellen Werte enthält. Das Format dieses XML-Fragments wird nicht definiert; es muss nur wohlgeformtes XML sein.  
  
> [!IMPORTANT]  
>  Beim Implementieren von benutzerdefinierter Persistenz müssen Sie alle Eigenschaften des Objekts dauerhaft machen, einschließlich der beiden übernommenen Eigenschaften und der hinzugefügten benutzerdefinierten Eigenschaften.  
  
## <a name="example"></a>Beispiel  
 Obwohl das SQL Server Custom Connection Manager-Beispiel keine benutzerdefinierte Persistenz für seine drei Eigenschaften des Typs **string** erfordert, zeigt der folgende Code ein Beispiel für den benutzerdefinierten Code, der erforderlich ist, um den Verbindungs-Manager und dessen Eigenschaften persistent zu speichern. Die Klasse, die diesen Code enthält, muss die <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentPersist>-Schnittstelle implementieren.  
  
```vb  
Private Const PERSIST_ELEMENT As String = "SqlConnectionManager"  
Private Const PERSIST_SERVER As String = "Server"  
Private Const PERSIST_DATABASE As String = "Database"  
Private Const PERSIST_CONNECTIONSTRING As String = "ConnectionString"  
  
Public Sub LoadFromXML(ByVal node As System.Xml.XmlElement, _  
  ByVal infoEvents As Microsoft.SqlServer.Dts.Runtime.IDTSInfoEvents) _  
  Implements Microsoft.SqlServer.Dts.Runtime.IDTSComponentPersist.LoadFromXML  
  
  Dim propertyNode As XmlNode  
  
  ' Make sure that the correct node is being loaded.  
  If node.Name <> PERSIST_ELEMENT Then  
    Throw New Exception("Persisted element is not of type " & PERSIST_ELEMENT)  
  End If  
  
  ' Load the three properties of the object from XML into variables.  
  For Each propertyNode In node.ChildNodes  
    Select Case propertyNode.Name  
      Case PERSIST_SERVER  
        _serverName = propertyNode.InnerText  
      Case PERSIST_DATABASE  
        _databaseName = propertyNode.InnerText  
      Case PERSIST_CONNECTIONSTRING  
        _connectionString = propertyNode.InnerText  
    End Select  
  Next  
  
End Sub  
  
Public Sub SaveToXML(ByVal doc As System.Xml.XmlDocument, _  
  ByVal infoEvents As Microsoft.SqlServer.Dts.Runtime.IDTSInfoEvents) _  
  Implements Microsoft.SqlServer.Dts.Runtime.IDTSComponentPersist.SaveToXML  
  
  Dim elementRoot As XmlElement  
  Dim propertyNode As XmlNode  
  
  ' Create a new node to persist the object and its properties.  
  elementRoot = doc.CreateElement(String.Empty, PERSIST_ELEMENT, String.Empty)  
  
  ' Save the three properties of the object from variables into XML.  
  propertyNode = doc.CreateNode(XmlNodeType.Element, PERSIST_SERVER, String.Empty)  
  propertyNode.InnerText = _serverName  
  elementRoot.AppendChild(propertyNode)  
  
  propertyNode = doc.CreateNode(XmlNodeType.Element, PERSIST_DATABASE, String.Empty)  
  propertyNode.InnerText = _databaseName  
  elementRoot.AppendChild(propertyNode)  
  
  propertyNode = doc.CreateNode(XmlNodeType.Element, PERSIST_CONNECTIONSTRING, String.Empty)  
  propertyNode.InnerText = _connectionString  
  elementRoot.AppendChild(propertyNode)  
  
  doc.AppendChild(elementRoot)  
  
End Sub  
```  
  
```csharp  
private const string PERSIST_ELEMENT = "SqlConnectionManager";  
private const string PERSIST_SERVER = "Server";  
private const string PERSIST_DATABASE = "Database";  
private const string PERSIST_CONNECTIONSTRING = "ConnectionString";  
  
public void LoadFromXML(System.Xml.XmlElement node,  
  Microsoft.SqlServer.Dts.Runtime.IDTSInfoEvents infoEvents)  
{  
  
  // Make sure that the correct node is being loaded.  
  if (node.Name != PERSIST_ELEMENT)  
  {  
    throw new Exception("Persisted element is not of type " + PERSIST_ELEMENT);  
  }  
  
  // Save the three properties of the object from variables into XML.  
  foreach (XmlNode propertyNode in node.ChildNodes)  
  {  
    switch (propertyNode.Name)  
    {  
      case PERSIST_SERVER:  
        _serverName = propertyNode.InnerText;  
        break;  
      case PERSIST_DATABASE:  
        _databaseName = propertyNode.InnerText;  
        break;  
      case PERSIST_CONNECTIONSTRING:  
        _connectionString = propertyNode.InnerText;  
        break;  
    }  
  }  
  
}  
  
public void SaveToXML(System.Xml.XmlDocument doc,  
  Microsoft.SqlServer.Dts.Runtime.IDTSInfoEvents infoEvents)  
{  
  
  XmlElement elementRoot;  
  XmlNode propertyNode;  
  
  // Create a new node to persist the object and its properties.  
  elementRoot = doc.CreateElement(String.Empty, PERSIST_ELEMENT, String.Empty);  
  
  // Save the three properties of the object from variables into XML.  
  propertyNode = doc.CreateNode(XmlNodeType.Element, PERSIST_SERVER, String.Empty);  
  propertyNode.InnerText = _serverName;  
  elementRoot.AppendChild(propertyNode);  
  
  propertyNode = doc.CreateNode(XmlNodeType.Element, PERSIST_DATABASE, String.Empty);  
  propertyNode.InnerText = _databaseName;  
  elementRoot.AppendChild(propertyNode);  
  
  propertyNode = doc.CreateNode(XmlNodeType.Element, PERSIST_CONNECTIONSTRING, String.Empty);  
  propertyNode.InnerText = _connectionString;  
  elementRoot.AppendChild(propertyNode);  
  
  doc.AppendChild(elementRoot);  
  
}  
```  
 
## <a name="see-also"></a>Siehe auch  
 [Entwickeln benutzerdefinierter Objekte für Integration Services](../../integration-services/extending-packages-custom-objects/developing-custom-objects-for-integration-services.md)   
 [Erstellen, Bereitstellen und Debuggen von benutzerdefinierten Objekten](../../integration-services/extending-packages-custom-objects/building-deploying-and-debugging-custom-objects.md)  
  
  
