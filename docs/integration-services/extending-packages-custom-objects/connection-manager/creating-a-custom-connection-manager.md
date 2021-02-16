---
description: Erstellen eines benutzerdefinierten Verbindungs-Managers
title: Erstellen eines benutzerdefinierten Verbindungs-Managers | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- custom connection managers [Integration Services], creating
ms.assetid: e83f8e02-ace4-42e0-b979-2f6be1460985
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 2316b7f61767e1befd400bef378b1e3775272136
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348945"
---
# <a name="creating-a-custom-connection-manager"></a>Erstellen eines benutzerdefinierten Verbindungs-Managers

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Die durchzuführenden Schritte zum Erstellen eines benutzerdefinierten Verbindungs-Managers ähneln denen jedes anderen benutzerdefinierten Objekts für [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)]:  
  
-   Erstellen Sie eine neue Klasse, die von der Basisklasse erbt. Bei einem Verbindungs-Manager ist die Basisklasse <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase>.  
  
-   Weisen Sie das Attribut zu, das den Typ des Objekts für die Klasse identifiziert. Bei einem Verbindungs-Manager ist das Attribut <xref:Microsoft.SqlServer.Dts.Runtime.DtsConnectionAttribute>.  
  
-   Überschreiben Sie die Implementierung der Methoden und Eigenschaften der Basisklasse. Bei einem Verbindungs-Manager gehören dazu die <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.ConnectionString%2A>-Eigenschaft sowie die <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.AcquireConnection%2A>-Methode und die <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.ReleaseConnection%2A>-Methode.  
  
-   Entwickeln Sie optional eine individuelle Benutzeroberfläche. Bei einem Verbindungs-Manager ist dazu eine Klasse erforderlich, die die <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsConnectionManagerUI>-Schnittstelle implementiert.  
  
> [!NOTE]  
>  Die meisten Tasks, Quellen und Ziele, die in [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] integriert wurden, können nur mit bestimmten Typen integrierter Verbindungs-Manager verwendet werden. Daher können diese Beispiele nicht mit den integrierten Tasks und Komponenten getestet werden.  
  
## <a name="getting-started-with-a-custom-connection-manager"></a>Erste Schritte mit einem benutzerdefinierten Verbindungs-Manager  
  
### <a name="creating-projects-and-classes"></a>Erstellen von Projekten und Klassen  
 Da alle verwalteten Verbindungs-Manager von der <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase>-Basisklasse abgeleitet sind, besteht der erste Schritt beim Erstellen eines benutzerdefinierten Verbindungs-Managers darin, in Ihrer bevorzugten verwalteten Programmiersprache ein Klassenbibliotheksprojekt anzulegen und eine Klasse zu generieren, die von der Basisklasse erbt. In dieser abgeleiteten Klasse überschreiben Sie die Methoden und Eigenschaften der Basisklasse, um die benutzerdefinierten Funktionen zu implementieren.  
  
 Erstellen Sie in der gleichen Lösung ein zweites Klassenbibliotheksprojekt für die individuelle Benutzeroberfläche. Für eine einfache Bereitstellung sollten Sie eine eigene Assembly für die Benutzeroberfläche verwenden, da Sie so den Verbindungs-Manager oder seine Benutzeroberfläche unabhängig aktualisieren und erneut bereitstellen können.  
  
 Konfigurieren Sie beide Projekte für das Signieren der Assemblys, die bei der Erstellung erzeugt werden, mit einer Schlüsseldatei mit starkem Namen.  
  
### <a name="applying-the-dtsconnection-attribute"></a>Zuweisen des 'DtsConnection'-Attributs  
 Weisen Sie das <xref:Microsoft.SqlServer.Dts.Runtime.DtsConnectionAttribute>-Attribut der Klasse zu, die Sie erstellt haben, um sie als Verbindungs-Manager zu kennzeichnen. Dieses Attribut stellt Entwurfszeitinformationen bereit, z. B. Name, Beschreibung und Verbindungstyp des Verbindungs-Managers. Die Eigenschaften <xref:Microsoft.SqlServer.Dts.Runtime.DtsConnectionAttribute.ConnectionType%2A> und **Description** entsprechen den Spalten **Typ** und **Beschreibung**, die im Dialogfeld **SSIS-Verbindungs-Manager hinzufügen** angezeigt werden. Das Dialogfeld wird beim Konfigurieren von Verbindungen für ein Paket in [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)] geöffnet.  
  
 Verwenden Sie die <xref:Microsoft.SqlServer.Dts.Runtime.DtsConnectionAttribute.UITypeName%2A>-Eigenschaft, um den Verbindungs-Manager mit der individuellen Benutzeroberfläche zu verknüpfen. Um das für diese Eigenschaft erforderliche öffentliche Schlüsseltoken zu erhalten, können Sie **sn.exe -t** verwenden. Damit zeigen Sie das öffentliche Schlüsseltoken aus der Schlüsselpaardatei (.snk) an, die Sie für das Signieren der Benutzeroberflächenassembly verwenden möchten.  
  
```vb  
<DtsConnection(ConnectionType:="SQLVB", _  
  DisplayName:="SqlConnectionManager (VB)", _  
  Description:="Connection manager for Sql Server", _  
  UITypeName:="SqlConnMgrUIVB.SqlConnMgrUIVB,SqlConnMgrUIVB,Version=1.0.0.0,Culture=neutral,PublicKeyToken=<insert public key token here>")> _  
Public Class SqlConnMgrVB  
  Inherits ConnectionManagerBase  
  . . .  
End Class  
```  
  
```csharp  
[DtsConnection(ConnectionType = "SQLCS",  
  DisplayName = "SqlConnectionManager (CS)",  
  Description = "Connection manager for Sql Server",  
  UITypeName = "SqlConnMgrUICS.SqlConnMgrUICS,SqlConnMgrUICS,Version=1.0.0.0,Culture=neutral,PublicKeyToken=<insert public key token here>")]  
public class SqlConnMgrCS :  
ConnectionManagerBase  
{  
  . . .  
}  
```  
  
## <a name="building-deploying-and-debugging-a-custom-connection-manager"></a>Erstellen, Bereitstellen und Debuggen eines benutzerdefinierten Verbindungs-Managers  
 Die Schritte zum Erstellen, Bereitstellen und Debuggen eines benutzerdefinierten Verbindungs-Managers in [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] ähneln denen für andere Typen benutzerdefinierter Objekte. Weitere Informationen finden Sie unter [Building, Deploying, and Debugging Custom Objects](../../../integration-services/extending-packages-custom-objects/building-deploying-and-debugging-custom-objects.md) (Erstellen, Bereitstellen und Debuggen von benutzerdefinierten Objekten).    
  
## <a name="see-also"></a>Weitere Informationen  
 [Codieren eines benutzerdefinierten Verbindungs-Managers](../../../integration-services/extending-packages-custom-objects/connection-manager/coding-a-custom-connection-manager.md)   
 [Entwickeln einer Benutzeroberfläche für einen benutzerdefinierten Verbindungs-Manager](../../../integration-services/extending-packages-custom-objects/connection-manager/developing-a-user-interface-for-a-custom-connection-manager.md)  
  
  
