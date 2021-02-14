---
title: Initialisieren von Objekten benutzerdefinierter Assemblys | Microsoft-Dokumentation
ms.date: 03/03/2017
description: Informieren Sie sich über das Initialisieren benutzerdefinierter Klassen mit den Werten, die Ihnen durch die globalen Objektauflistungen des Berichts zur Verfügung stehen.
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: custom-assemblies
ms.topic: reference
helpviewer_keywords:
- initializing custom assemblies [Reporting Services]
- custom assemblies [Reporting Services], initializing
- OnInit method
ms.assetid: 26fd74dc-d02f-40f7-aeb3-50ce05e9e6b9
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 24009d6b4e7d1fbfe107d16b828f16454dff7bc0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100064525"
---
# <a name="initializing-custom-assembly-objects"></a>Initialisieren von Objekten benutzerdefinierter Assemblys
  In einigen Fällen müssen Sie die Werte der Eigenschaften und Felder in Ihren benutzerdefinierten Assemblyklassen beim Instanziieren initialisieren. Wahrscheinlich müssen Sie die benutzerdefinierten Klassen mit den Werten initialisieren, die Ihnen von den globalen Objektauflistungen des Berichts zur Verfügung stehen. Hierzu überschreiben Sie die **OnInit**-Methode des **Code**-Objekts eines Berichts. Verwenden Sie das **Code**-Element der Berichtsdefinition, um auf **OnInit** zuzugreifen. Es gibt zwei Techniken, um Eigenschafts- oder Feldwerte der Klassen in einer benutzerdefinierten Assembly zu initialisieren, die Sie in Ihrem Bericht verwenden möchten: Sie können entweder mithilfe von **OnInit** eine neue Instanz der Klasse deklarieren und erstellen, oder Sie können eine öffentlich verfügbare Methode mithilfe von **OnInit** abrufen.  
  
## <a name="global-object-collections-and-initialization"></a>Globale Objektauflistungen und Initialisierung  
 Zum Initialisieren der benutzerdefinierten Klassenvariablen stehen Ihnen mehrere Auflistungen zur Verfügung. Sie können die **Globals**-Auflistung und **die User**-Auflistung verwenden. Die Auflistungen **Parameters**, **Fields** und **ReportItems** stehen Ihnen zum Zeitpunkt des Berichtslebensdauerzyklus nicht zur Verfügung, wenn die **OnInit**-Methode aufgerufen wird. Sie müssen den **Report**-Objektverweis einschließen, um die freigegebenen Auflistungen **Globals** oder **User** zu verwenden. Wenn Sie beispielsweise Ihre benutzerdefinierte Klasse ausgehend von der aktuellen Sprache des Benutzers initialisieren möchten, der auf den Bericht zugreift, könnte Ihr **Code**-Element folgendermaßen aussehen:  
  
```  
<Code>  
   Dim m_myClass As MyClass  
  
   Protected Overrides Sub OnInit()  
      m_myClass = new MyClass(Report.User!Language, _  
         Report.Globals!ExecutionTime)  
   End Sub  
</Code>  
```  
  
 Eine Möglichkeit, die Eigenschaften- und Feldwerte einer Klasse zu initialisieren (wie bereits zuvor gezeigt), besteht darin, die neue Klasse zu deklarieren und eine neue Instanz davon zu erstellen, indem Sie einen überschriebenen Konstruktor aufrufen.  
  
 Eine andere Möglichkeit zur Initialisierung von Eigenschaften- und Feldwerten in den Klassen Ihrer benutzerdefinierten Assemblys besteht im Aufrufen einer öffentlich verfügbaren Methode, die Sie von der **OnInit**-Methode aus definieren. Sie müssen zuerst in der Berichtsdefinitionsdatei einen Instanznamen für die Klasse hinzufügen. Sobald Sie den entsprechenden Assemblyverweis und den Instanznamen hinzugefügt haben, können Sie die Initialisierungsmethode aufrufen, um die Eigenschaften- und Feldwerte für Ihre Klasse zu initialisieren. Ihre **OnInit**-Methode könnte folgendermaßen aussehen:  
  
```  
<Code>  
   Protected Overrides Sub OnInit()  
      m_myClass.MyInitializationMethod(Report.User!Language, _  
         Report.Globals!ExecutionTime)  
   End Sub  
</Code>  
```  
  
 Weitere Informationen über das Hinzufügen eines Assemblyverweises und eines Instanznamens für die benutzerdefinierte Klasse finden Sie unter [Hinzufügen eines Assemblyverweises zu einem Bericht (SSRS)](../../reporting-services/report-design/add-an-assembly-reference-to-a-report-ssrs.md).  
  
 Weitere Informationen zur Verwendung der globalen Objektauflistungen finden Sie unter [Integrierte Auflistungen in Ausdrücken (Berichts-Generator und SSRS)](../../reporting-services/report-design/built-in-collections-in-expressions-report-builder.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Verwenden benutzerdefinierter Assemblys mit Berichten](../../reporting-services/custom-assemblies/using-custom-assemblies-with-reports.md)  
  
  
