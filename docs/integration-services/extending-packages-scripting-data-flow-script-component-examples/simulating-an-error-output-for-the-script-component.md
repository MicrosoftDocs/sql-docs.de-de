---
description: Simulieren einer Fehlerausgabe für die Skriptkomponente
title: Simulieren einer Fehlerausgabe für die Skriptkomponente | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
helpviewer_keywords:
- Script component [Integration Services], error output
- error outputs [Integration Services], Script component
ms.assetid: f8b6ecff-ac99-4231-a0e7-7ce4ad76bad0
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 88af189347fe799f58939f167e6713c90b3a8275
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344914"
---
# <a name="simulating-an-error-output-for-the-script-component"></a>Simulieren einer Fehlerausgabe für die Skriptkomponente

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Zur automatischen Bearbeitung von Fehlerzeilen können Sie eine Ausgabe in der Skriptkomponente zwar nicht direkt als Fehlerausgabe konfigurieren, aber Sie können die Funktion einer integrierten Fehlerausgabe reproduzieren, indem Sie eine weitere Ausgabe erstellen und im Skript Bedingungslogik verwenden, um Zeilen gegebenenfalls an diese Ausgabe weiterzuleiten. Möglicherweise möchten Sie das Verhalten einer integrierten Fehlerausgabe imitieren, indem Sie zwei zusätzliche Ausgabespalten hinzufügen, sodass Sie die Fehlernummer und die ID der Spalte erhalten, in der der Fehler aufgetreten ist.  
  
 Wenn Sie die Fehlerbeschreibung hinzufügen möchten, die einem spezifischen vordefinierten [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Fehlercode entspricht, können Sie dazu die <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.GetErrorDescription%2A>-Methode der <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100>-Schnittstelle verwenden. Diese wird über die <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.ComponentMetaData%2A>-Eigenschaft der Skriptkomponente bereitgestellt.  
  
## <a name="example"></a>Beispiel  
 Das hier dargestellte Beispiel verwendet eine Skriptkomponente, die als Transformation konfiguriert ist und über zwei synchrone Ausgaben verfügt. Der Zweck der Skriptkomponente besteht darin, Fehlerzeilen aus Adressdaten in der AdventureWorks-Beispieldatenbank herauszufiltern. Dieses fiktive Beispiel geht von der Annahme aus, dass wir eine Werbeaktion für Kunden in Nordamerika vorbereiten und dazu Adressen herausfiltern müssen, die sich nicht in Nordamerika befinden.  
  
#### <a name="to-configure-the-example"></a>So konfigurieren Sie das Beispiel  
  
1.  Erstellen Sie einen Verbindungs-Manager, bevor Sie die neue Skriptkomponente erzeugen, und konfigurieren Sie eine Datenflussquelle, die Adressdaten aus der AdventureWorks-Beispieldatenbank herausfiltert. Für dieses Beispiel hat nur die Spalte CountryRegionName Bedeutung. Sie können einfach die Ansicht Person.vStateCountryProvinceRegion verwenden, oder Sie können Daten auswählen, indem Sie die Tabellen Person.Address, Person.StateProvince und Person.CountryRegion miteinander verknüpfen.  
  
2.  Fügen Sie der Datenfluss-Designeroberfläche eine neue Skriptkomponente hinzu, und konfigurieren Sie sie als Transformation. Öffnen Sie den **Transformations-Editor für Skripterstellung**.  
  
3.  Legen Sie auf der Seite **Skript** die **ScriptLanguage**-Eigenschaft auf die Skriptsprache fest, die Sie zum Codieren des Skripts verwenden möchten.  
  
4.  Klicken Sie auf **Skript bearbeiten**, um [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] Tools for Applications (VSTA) zu öffnen.  
  
5.  Geben Sie den folgenden Beispielcode in die **Input0_ProcessInputRow**-Methode ein, oder fügen Sie ihn mit Kopieren und Einfügen ein.  
  
6.  Schließen Sie VSTA.  
  
7.  Wählen Sie auf der Seite **Eingabespalten** die Spalten aus, die Sie in der Skripttransformation verarbeiten möchten. In diesem Beispiel wird nur die CountryRegionName-Spalte verwendet. Verfügbare Eingabespalten, die Sie nicht auswählen, werden einfach unverändert im Datenfluss durchlaufen.  
  
8.  Fügen Sie auf der Seite **Eingaben und Ausgaben** eine neue, zweite Ausgabe hinzu, und legen Sie den entsprechenden **SynchronousInputID**-Wert auf die ID der Eingabe fest, die gleichzeitig auch der Wert der **SynchronousInputID**-Eigenschaft der Standardausgabe ist. Legen Sie die **ExclusionGroup**-Eigenschaft beider Ausgaben auf denselben Wert ungleich null (z.B. 1) fest, um anzugeben, dass jede Zeile nur an eine der beiden Ausgaben weitergeleitet wird. Geben Sie der neuen Fehlerausgabe einen aussagekräftigen Namen, z. B. "MeineFehlerausgabe".  
  
9. Fügen Sie der neuen Fehlerausgabe zusätzliche Ausgabespalten hinzu, um die gewünschten Fehlerinformationen aufzuzeichnen, zu denen zum Beispiel der Fehlercode, die ID der Spalte mit dem Fehler und möglicherweise die Fehlerbeschreibung zählen können. In diesem Beispiel werden die neuen Spalten, ErrorColumn und ErrorMessage, erstellt. Wenn Sie vordefinierte [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Fehler in Ihren eigenen Implementierungen erkennen, stellen Sie sicher, dass Sie eine ErrorCode-Spalte für die Fehlernummer hinzufügen.  
  
10. Beachten Sie den ID-Wert der Eingabezeile oder -zeilen, die die Skriptkomponente auf Fehlerbedingungen untersucht. In diesem Beispiel wird dieser Spaltenbezeichner dazu verwendet, den ErrorColumn-Wert aufzufüllen.  
  
11. Schließen Sie den **Transformations-Editor für Skripterstellung**.  
  
12. Fügen Sie die Ausgaben der Skriptkomponente an geeignete Ziele an. Flatfileziele sind die einfachste Möglichkeit zur Konfiguration bei Ad-hoc-Tests.  
  
13. Führen Sie das Paket aus.  
  
```vb  
Public Overrides Sub Input0_ProcessInputRow(ByVal Row As Input0Buffer)  
  
  If Row.CountryRegionName <> "Canada" _  
      And Row.CountryRegionName <> "United States" Then  
  
    Row.ErrorColumn = 68 ' ID of CountryRegionName column  
    Row.ErrorMessage = "Address is not in North America."  
    Row.DirectRowToMyErrorOutput()  
  
  Else  
  
    Row.DirectRowToOutput0()  
  
  End If  
  
End Sub  
```  
  
```csharp  
public override void Input0_ProcessInputRow(Input0Buffer Row)  
{  
  
  if (Row.CountryRegionName!="Canada"&&Row.CountryRegionName!="United States")  
  
  {  
    Row.ErrorColumn = 68; // ID of CountryRegionName column  
    Row.ErrorMessage = "Address is not in North America.";  
    Row.DirectRowToMyErrorOutput();  
  
  }  
  else  
  {  
  
    Row.DirectRowToOutput0();  
  
  }  
  
}  
```  
  
## <a name="see-also"></a>Siehe auch  
 [Fehlerbehandlung in Daten](../../integration-services/data-flow/error-handling-in-data.md)   
 [Verwenden von Fehlerausgaben in einer Datenflusskomponente](../../integration-services/extending-packages-custom-objects/data-flow/using-error-outputs-in-a-data-flow-component.md)   
 [Erstellen einer synchronen Transformation mit der Skriptkomponente](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-synchronous-transformation-with-the-script-component.md)  
  
  
