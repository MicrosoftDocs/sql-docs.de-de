---
title: Durchführen eines Upgrades für eine benutzerdefinierte Visual Studio 2010-Testbedingung von einer älteren Version
description: In diesem Artikel wird erläutert, wie Sie eine benutzerdefinierte Visual Studio 2010-Testbedingung für SQL Server Data Tools aktualisieren. Außerdem erfahren Sie, welche Änderungen vorgenommen werden müssen und wie Sie die Bedingung installieren.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 44c895a3-dee0-4032-a60f-812f5fe3c713
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 2e0f9813db19e06729f7164f0ca419801364a365
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018050"
---
# <a name="how-to-upgrade-a-visual-studio-2010-custom-test-condition-from-a-previous-release-to-sql-server-data-tools"></a>Gewusst wie: Durchführen eines Upgrades für eine benutzerdefinierten Visual Studio 2010-Testbedingung von einem älteren Release auf SQL Server Data Tools

Zur Verwendung einer Testbedingung, die in einer Version vor SQL Server Data Tools erstellt wurde, muss ein Upgrade durchgeführt werden:  
  
-   [Aktualisieren von Verweisen](#UpdateReferences)  
  
-   [Aktualisieren von Klassenattributen und Typverweisen](#UpdateClassAttributesandTypeReference)  
  
-   [Installieren der Testbedingung mit durchgeführtem Upgrade](#ApplytheNewRegistrationProcess)  
  
## <a name="update-references"></a><a name="UpdateReferences"></a>Aktualisieren von Verweisen  
So aktualisieren Sie die Projektverweise  
  
1.  Nur für Visual Basic: Klicken Sie im **Projektmappen-Explorer** auf **Alle Dateien anzeigen**.  
  
2.  Erweitern Sie im **Projektmappen-Explorer** den Knoten **Verweise**.  
  
3.  Klicken Sie mit der rechten Maustaste auf die folgenden Assemblyverweise, und klicken Sie auf **Entfernen**:  
  
    1.  Microsoft.Data.Schema.UnitTesting  
  
    2.  Microsoft.Data.Schema  
  
4.  Klicken Sie auf das Menü **Projekt**, oder klicken Sie mit der rechten Maustaste im **Projektmappen-Explorer** auf den Projektordner, und klicken Sie dann auf **Verweis hinzufügen**.  
  
5.  Klicken Sie auf die Registerkarte **.NET**.  
  
6.  Klicken Sie in der Liste **Komponentenname** auf **System.ComponentModel.Composition** und dann auf **OK**.  
  
7.  Fügen Sie die erforderlichen Assemblyverweise hinzu. Klicken Sie mit der rechten Maustaste auf den Projektknoten, und klicken Sie dann auf **Verweis hinzufügen**. Klicken Sie auf **Durchsuchen**, und navigieren Sie zum Ordner „C:\Programme (x86)\\Microsoft SQL Server\110\DAC\Bin“. Wählen Sie „Microsoft.Data.Tools.Schema.Sql.dll“ aus, klicken Sie auf „Hinzufügen“, und klicken Sie dann auf „OK“.  
  
8.  Klicken Sie im Menü **Projekt** auf **Projekt entladen**.  
  
9. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das **Projekt**, und wählen Sie `project_name` **.csproj** **bearbeiten** aus.  
  
10. Fügen Sie nach dem Import von `Microsoft.CSharp.targets` folgende Import-Anweisung hinzu:  
  
    ```  
    <Import Project="$(MSBuildExtensionsPath32)\Microsoft\VisualStudio\v10.0\SSDT\Microsoft.Data.Tools.Schema.Sql.UnitTesting.targets" Condition="'$(VisualStudioVersion)' == ''" />  
  
    <Import Project="$(MSBuildExtensionsPath32)\Microsoft\VisualStudio\v$(VisualStudioVersion)\SSDT\Microsoft.Data.Tools.Schema.Sql.UnitTesting.targets" Condition="'$(VisualStudioVersion)' != ''" />  
    ```  
  
11. Speichern Sie die Datei, und schließen Sie sie. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie **Projekt erneut laden** aus.  
  
12. Öffnen Sie die Testbedingungsklasse, und entfernen Sie alle using-Anweisungen, die mit **Microsoft.Data.Schema** beginnen. Diese Aufgabe lässt sich am einfachsten ausführen, indem Sie mit der rechten Maustaste auf die Datei klicken und dann **Using-Direktiven organisieren** und **Entfernen und Sortieren** auswählen. Die folgenden Using-Anweisungen müssen entfernt werden:  
  
    ```  
    using Microsoft.Data.Schema.UnitTesting;  
    using Microsoft.Data.Schema.UnitTesting.Conditions;  
    using Microsoft.Data.Schema.Extensibility;  
    using Microsoft.Data.Schema;  
    ```  
  
13. Fügen Sie der Datei die folgenden Using-Anweisungen hinzu, falls sie noch nicht vorhanden sind:  
  
    ```  
    using System.ComponentModel;  
    using Microsoft.Data.Tools.Schema.Sql.UnitTesting;  
    using Microsoft.Data.Tools.Schema.Sql.UnitTesting.Conditions;  
    ```  
  
Die Testbedingung verwendet nun Assemblyverweise für SQL Server-Komponententests.  
  
## <a name="update-class-attributes-and-type-references"></a><a name="UpdateClassAttributesandTypeReference"></a>Aktualisieren von Klassenattributen und Typverweisen  
Ersetzen Sie die älteren Klassenattribute für die Komponententests durch ein neues Attribut. Die Erweiterbarkeit von SQL Server-Komponententests basiert jetzt auf MEF (Managed Extensibility Framework). Außerdem müssen einige Typverweise aktualisiert werden.  
  
### <a name="update-class-attributes"></a>Aktualisieren von Klassenattributen  
Aktualisieren Sie den Code wie folgt:  
  
1.  Entfernen Sie alle `DatabaseSchemaProviderCompatibility`-Attribute, da sie für die Erweiterbarkeitsfunktion der Vorgängerversion erforderlich waren und in SQL Server-Komponententests nicht unterstützt werden.  
  
2.  Entfernen Sie das `DisplayName`-Attribut. Der Anzeigename ist im neuen Attribut enthalten.  
  
3.  Fügen Sie das neue `ExportTestCondition`-Attribut hinzu. Dieses Attribut muss vorhanden sein, damit die Testbedingung von SQL Server-Komponententests ermittelt und verwendet werden kann. `ExportTestCondition` ersetzt das/die `DatabaseSchemaProviderCompatibility`-Attribut(e). `ExportTestCondition` verwendet zwei Parameter:  
  
    -   `DisplayName` ist der erste Parameter. Er ersetzt das `DisplayName`-Attribut und wird zur Beschreibung aller Testbedingungen dieses Typs verwendet.  
  
    -   Der zweite Parameter wird verwendet, um die Erweiterung eindeutig zu identifizieren. Sie können den Typ einfach mithilfe von `typeof(NewTestCondition)` übergeben, da dieser eindeutig sein sollte. Optional kann auch eine Zeichenfolgen-ID übergeben werden.  
  
4.  Die Klassendefinition sollte wie folgt geändert werden:  
  
    Vorher:  
  
    ```  
    [DatabaseSchemaProviderCompatibility(typeof(DatabaseSchemaProvider))]  
    [DatabaseSchemaProviderCompatibility(null)]  
    [DisplayName("NewTestCondition")]  
    public class NewTestCondition:TestCondition  
    {  
       // Additional implementation to be added here  
    }  
    ```  
  
    Nachher:  
  
    ```  
    [ExportTestCondition("NewTestCondition ", typeof(NewTestCondition))]  
    public class NewTestCondition:TestCondition  
    {  
       // Additional implementation to be added here  
    }  
    ```  
  
### <a name="update-type-references"></a>Aktualisieren von Typverweisen  
Einige Typnamen wurden im Komponententestframework von SQL Server geändert. Um den Code für die Verwendung der neuen Typnamen zu aktualisieren, verwenden Sie im Menü **Bearbeiten** die Option **Suchen und Ersetzen**. Typnamen beginnen jetzt mit **Sql**. Klassennamen sollten wie folgt aktualisiert werden:  
  
|Alter Typname|Neuer Typname|  
|-----------------|-----------------|  
|`ExecutionResult`|`SqlExecutionResult`|  
  
## <a name="install-the-upgraded-test-condition"></a><a name="ApplytheNewRegistrationProcess"></a>Installieren der Testbedingung mit durchgeführtem Upgrade  
In Datenbankkomponententests früherer Versionen war es u. U. erforderlich, die Testbedingung im globalen Assemblycache zu installieren oder eine XML-Datei mit den Assemblyinformationen zu erstellen. In den Komponententests von SQL Server sind diese zusätzlichen Schritte nicht mehr erforderlich. (Weitere Informationen finden Sie unter [Kompilieren des Projekts und Installieren der Testbedingung](../ssdt/walkthrough-use-custom-test-condition-to-verify-stored-procedure-results.md#xxx).  
  
Stellen Sie sicher, dass die Assembly signiert und kompiliert ist, nachdem Sie die Verweise aktualisiert haben.  
  
Als Nächstes kopieren Sie die Assemblydatei aus dem Standardausgabeverzeichnis „Eigene Dokumente\Visual Studio 2010\Projects\\<yoursolutionname>\\<yourprojectname>\bin\Debug\\)“ in das Verzeichnis „%Programme%\Microsoft Visual Studio <Version>\Common7\IDE\Extensions\Microsoft\SQLDB\TestConditions“. Beim Start von Visual Studio werden alle Erweiterungen im Verzeichnis TestConditions identifiziert und für die Verwendung in der Sitzung zur Verfügung gestellt:  
  
## <a name="upgrade-existing-tests-that-need-to-use-the-new-test-condition"></a>Aktualisieren vorhandener Tests, die die neue Testbedingung verwenden müssen  
Ermitteln Sie alle Testprojekte, die die alte Testbedingung verwenden, stattdessen aber die neue Bedingung verwenden sollten. Stellen Sie sicher, dass diese Testprojekte aktualisiert werden. Weitere Informationen finden Sie unter [Durchführen eines Upgrades für ein älteres Testprojekt mit Datenbankkomponententests](../ssdt/upgrade-an-older-test-project-containing-database-unit-tests.md).  
  
Entfernen Sie den Assemblyverweis auf die alte Testbedingung.  
  
Fügen Sie dem Projekt einen neuen SQL Server-Komponententest hinzu, um einen Assemblyverweis auf die Testbedingung, für die ein Upgrade durchgeführt wurde, im Projekt zu erstellen. Zum Erstellen des Verweises muss eine Testklasse hinzugefügt werden. Nachdem der Verweis hinzugefügt wurde, können Sie die Testklasse löschen.  
  
## <a name="see-also"></a>Weitere Informationen  
[Benutzerdefinierte Testbedingungen für SQL Server-Komponententests](../ssdt/custom-test-conditions-for-sql-server-unit-tests.md)  
  
