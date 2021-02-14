---
title: Erstellen einer Assembly für eine benutzerdefinierte statische Codeanalyseregel für SQL Server
description: Hier erfahren Sie, wie Sie SQL Server-Codeanalyseregeln erstellen. Richten Sie eine Regel ein, um die WAITFOR DELAY-Anweisung in gespeicherten Prozeduren, Triggern und Funktionen zu vermeiden.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: f7b6ed8c-a4e0-4e33-9858-a8aa40aef309
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 3cae3183bef3d6bd40a9d30805f3923df8d70679
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082341"
---
# <a name="walkthrough-authoring-a-custom-static-code-analysis-rule-assembly-for-sql-server"></a>Exemplarische Vorgehensweise – Erstellen einer Assembly für eine benutzerdefinierte statische Codeanalyseregel für SQL Server

Diese exemplarische Vorgehensweise veranschaulicht die erforderlichen Schritte zum Erstellen einer SQL Server-Codeanalyseregel. Die in dieser exemplarischen Vorgehensweise erstellte Regel wird dazu verwendet, WAITFOR DELAY-Anweisungen in gespeicherten Prozeduren, Triggern und Funktionen zu verhindern.  
  
In dieser exemplarischen Vorgehensweise erstellen Sie eine benutzerdefinierte Regel für die statische Codeanalyse von Transact\-SQL mithilfe der folgenden Prozesse:  
  
1. Erstellen einer Klassenbibliothek, Aktivieren von Signaturen für das betreffende Projekt und Hinzufügen der erforderlichen Verweise.  
  
2. Erstellen einer benutzerdefinierten Visual C\#-Regelklasse  
  
3. Erstellen von zwei Visual C\#-Hilfsklassen  
  
4. Kopieren der sich ergebenden DLL, die Sie erstellt haben, in das Erweiterungsverzeichnis, um sie zu installieren.  
  
5. Überprüfen, ob die neue Regel zur Codeanalyse aktiv ist.  
  
**Voraussetzungen**
  
Zum Abschließen dieser exemplarischen Vorgehensweise benötigen Sie Folgendes:  
  
- Sie müssen eine Version von Visual Studio installiert haben, die SQL Server Data Tools enthält und die Entwicklung in Visual C\# oder Visual Basic unterstützt.  
  
- Sie müssen über ein SQL Server-Projekt verfügen, das SQL Server-Objekte enthält.  
  
- Eine Instanz von SQL Server, auf der Sie ein Datenbankprojekt bereitstellen können.  
  
> [!NOTE]  
> Diese exemplarische Vorgehensweise ist für Benutzer gedacht, die bereits mit den SQL Server-Funktionen von SQL Server Data Tools vertraut sind. Außerdem wird von Ihnen erwartet, dass Sie mit den Visual Studio-Konzepten vertraut sind, wie etwa dem Erstellen einer Klassenbibliothek und dem Verwenden des Code-Editors zum Hinzufügen von Code zu einer Klasse.  
  
## <a name="creating-a-custom-code-analysis-rule-for-sql-server"></a>Erstellen einer benutzerdefinierten Codeanalyseregel für SQL Server  

Erstellen Sie zunächst eine Klassenbibliothek. So erstellen Sie ein Klassenbibliotheksprojekt:  
  
1. Erstellen Sie ein Visual C\#- oder Visual Basic-Klassenbibliotheksprojekt mit der Bezeichnung „SampleRules“.  
  
2. Benennen Sie die Datei „Class1.cs“ in „AvoidWaitForDelayRule.cs“ um.  
  
3. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektknoten, und klicken Sie dann auf **Verweis hinzufügen**.  
  
4. Wählen Sie auf der Registerkarte „Frameworks“ die Option „System.ComponentModel.Composition“ aus.  
  
5. Klicken Sie auf **Durchsuchen**, und navigieren Sie zum Verzeichnis „C:\Programme(x86)\\Microsoft SQL Server\120\SDK\Assemblies“, wählen Sie „Microsoft.SqlServer.TransactSql.ScriptDom.dll“ aus, und klicken Sie dann auf „OK“.  
  
6. Erstellen Sie im nächsten Schritt die erforderlichen DACFx-Verweise. Klicken Sie auf **Durchsuchen**, und navigieren Sie zum Verzeichnis „<Visual Studio Install Dir>\Common7\IDE\Extensions\\Microsoft\SQLDB\DAC\120“. Wählen Sie die Einträge „Microsoft.SqlServer.Dac.dll“, „Microsoft.SqlServer.Dac.Extensions.dll“ und „Microsoft.Data.Tools.Schema.Sql.dll“ aus, klicken Sie auf **Hinzufügen**, und klicken Sie dann auf **OK**.  
  
    DACFx-Binärdateien werden jetzt in Ihrem Visual Studio-Installationsverzeichnis installiert. Bei Visual Studio 2012 befindet sich <Visual Studio Install Dir> in der Regel im Verzeichnis „C:\Programme (x86)\\MicrosoftVisual Studio 11.0“. Bei Visual Studio 2013 ist dies in der Regel das Verzeichnis „C:\Programme (x86)\\MicrosoftVisual Studio 12.0“.  
  
Im nächsten Schritt fügen Sie die Unterstützungsklassen hinzu, die von der Regel verwendet werden.  
  
## <a name="creating-the-custom-code-analysis-rule-supporting-classes"></a>Erstellen der Unterstützungsklassen für die benutzerdefinierte Codeanalyseregel

Bevor Sie die Klasse für die eigentliche Regel erstellen, fügen Sie dem Projekt eine Besucherklasse und eine Attributklasse hinzu. Diese Klassen können zum Erstellen zusätzlicher benutzerdefinierten Regeln nützlich sein.  
  
Die erste Klasse, die Sie definieren müssen, ist die Klasse „WaitForDelayVisitor“, die aus [TSqlConcreteFragmentVisitor](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.tsqlconcretefragmentvisitor) abgeleitet ist. Diese Klasse ermöglicht den Zugriff auf die WAITFOR DELAY-Anweisungen im Modell. Besucherklassen verwenden die [ScriptDom](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom)-APIs, die von SQL Server bereitgestellt werden. In dieser API ist Transact\-SQL-Code als abstrakte Syntaxstruktur (AST, Abstract Syntax Tree) dargestellt, und Besucherklassen können nützlich sein, wenn Sie bestimmte Syntaxobjekte finden möchten, wie etwa WAITFOR DELAY-Anweisungen. Diese sind anhand des Objektmodells schwierig zu finden, da sie keiner bestimmten Objekteigenschaft oder -beziehung zugeordnet sind, aber mithilfe des Besuchermusters und der [ScriptDom](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom)-API sind sie leicht zu finden.  
  
### <a name="defining-the-waitfordelayvisitor-class"></a>Definieren der WaitForDelayVisitor-Klasse  
  
1. Wählen Sie im **Projektmappen-Explorer** das Projekt „SampleRules“ aus.  
  
2. Wählen Sie im Menü **Projekt** den Eintrag **Klasse hinzufügen** aus. Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.  
  
3. Geben Sie im Textfeld **Name** den Wert „WaitForDelayVisitor.cs“ ein, und klicken Sie dann auf die Schaltfläche **Hinzufügen**. Die Datei „WaitForDelayVisitor.cs“ wird dem Projekt im **Projektmappen-Explorer** hinzugefügt.  
  
4. Öffnen Sie die Datei „WaitForDelayVisitor.cs“, und aktualisieren Sie den Inhalt, sodass er dem folgenden Code entspricht:  
  
    ```  
    using System.Collections.Generic;  
    using Microsoft.SqlServer.TransactSql.ScriptDom;  
    namespace SampleRules {  
        class WaitForDelayVistor {}  
    }  
    ```  
  
5. Ändern Sie in der Klassendeklaration den Zugriffsmodifizierer auf intern, und leiten Sie die Klasse aus „TSqlConcreteFragmentVisitor“ ab:  
  
    ```  
    internal class WaitForDelayVisitor : TSqlConcreteFragmentVisitor {}  
    ```  
  
6. Fügen Sie den folgenden Code hinzu, um die Listenmitgliedsvariable zu definieren:  
  
    ```  
    public IList<WaitForStatement> WaitForDelayStatements { get; private set; }  
    ```  
  
7. Definieren Sie den Klassenkonstruktor durch Hinzufügen des folgenden Codes:  
  
    ```  
    public WaitForDelayVisitor() {  
       WaitForDelayStatments = new List<WaitForStatement>();  
    }  
    ```  
  
8. Überschreiben Sie die Methode „ExplicitVisit“, indem Sie den folgenden Code hinzufügen:  
  
    ```  
    public override void ExplicitVisit(WaitForStatement node) {  
       // We are only interested in WAITFOR DELAY occurrences  
       if (node.WaitForOption == WaitForOption.Delay)  
          WaitForDelayStatments.Add(node);  
    }  
    ```  
  
    Diese Methode besucht die WAITFOR-Anweisungen im Modell und fügt jene, für die die Option DELAY angegeben ist, der Liste der WAITFOR DELAY-Anweisungen hinzu. Die Schlüsselklasse, auf die hier verwiesen wird, ist [WaitForStatement](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.waitforstatement).  
  
9. Klicken Sie im Menü **Datei** auf **Speichern**.  
  
Die zweite Klasse ist „LocalizedExportCodeAnalysisRuleAttribute.cs“. Dies ist eine Erweiterung der integrierten Klasse Microsoft.SqlServer.Dac.CodeAnalysis.ExportCodeAnalysisRuleAttribute, die vom Framework bereitgestellt wird, und unterstützt das Lesen von „DisplayName“ und „Description“, die von Ihrer Regel verwendet werden, aus einer Ressourcendatei. Dies ist eine nützliche Regel, wenn Sie sich jemals dafür entscheiden, Ihre Regeln in mehreren Sprachen zu verwenden.  
  
### <a name="defining-the-localizedexportcodeanalysisruleattribute-class"></a>Definieren der Klasse „LocalizedExportCodeAnalysisRuleAttribute“  
  
1. Wählen Sie im **Projektmappen-Explorer** das Projekt „SampleRules“ aus.  
  
2. Wählen Sie im Menü **Projekt** den Eintrag **Klasse hinzufügen** aus. Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.  
  
3. Geben Sie im Textfeld **Name** den Wert „LocalizedExportCodeAnalysisRuleAttribute.cs“ ein, und klicken Sie dann auf die Schaltfläche **Hinzufügen**. Die Datei wird dem Projekt im **Projektmappen-Explorer** hinzugefügt.  
  
4. Öffnen Sie die Datei, und aktualisieren Sie den Inhalt, sodass er dem folgenden Code entspricht:  
  
    ```  
    using Microsoft.SqlServer.Dac.CodeAnalysis;  
    using System;  
    using System.Globalization;  
    using System.Reflection;  
    using System.Resources;  
  
    namespace SampleRules  
    {  
  
        internal class LocalizedExportCodeAnalysisRuleAttribute : ExportCodeAnalysisRuleAttribute  
        {  
            private readonly string _resourceBaseName;  
            private readonly string _displayNameResourceId;  
            private readonly string _descriptionResourceId;  
  
            private ResourceManager _resourceManager;  
            private string _displayName;  
            private string _descriptionValue;  
  
            /// <summary>  
            /// Creates the attribute, with the specified rule ID, the fully qualified  
            /// name of the resource file that will be used for looking up display name  
            /// and description, and the Ids of those resources inside the resource file.  
            /// </summary>  
            public LocalizedExportCodeAnalysisRuleAttribute(  
                string id,  
                string resourceBaseName,  
                string displayNameResourceId,  
                string descriptionResourceId)  
                : base(id, null)  
            {  
                _resourceBaseName = resourceBaseName;  
                _displayNameResourceId = displayNameResourceId;  
                _descriptionResourceId = descriptionResourceId;  
            }  
  
            /// <summary>  
            /// Rules in a different assembly would need to overwrite this  
            /// </summary>  
            /// <returns></returns>  
            protected virtual Assembly GetAssembly()  
            {  
                return GetType().Assembly;  
            }  
  
            private void EnsureResourceManagerInitialized()  
            {  
                var resourceAssembly = GetAssembly();  
  
                try  
                {  
                    _resourceManager = new ResourceManager(_resourceBaseName, resourceAssembly);  
                }  
                catch (Exception ex)  
                {  
                    var msg = String.Format(CultureInfo.CurrentCulture, RuleResources.CannotCreateResourceManager, _resourceBaseName, resourceAssembly);  
                    throw new RuleException(msg, ex);  
                }  
            }  
  
            private string GetResourceString(string resourceId)  
            {  
                EnsureResourceManagerInitialized();  
                return _resourceManager.GetString(resourceId, CultureInfo.CurrentUICulture);  
            }  
  
            /// <summary>  
            /// Overrides the standard DisplayName and looks up its value inside a resources file  
            /// </summary>  
            public override string DisplayName  
            {  
                get  
                {  
                    if (_displayName == null)  
                    {  
                        _displayName = GetResourceString(_displayNameResourceId);  
                    }  
                    return _displayName;  
                }  
            }  
  
            /// <summary>  
            /// Overrides the standard Description and looks up its value inside a resources file  
            /// </summary>  
            public override string Description  
            {  
                get  
                {  
                    if (_descriptionValue == null)  
                    {   
                        _descriptionValue = GetResourceString(_descriptionResourceId);  
                    }  
                    return _descriptionValue;  
                }  
            }  
        }  
    }  
    ```  
  
Im nächsten Schritt fügen Sie eine Ressourcendatei hinzu, die den Regelnamen, die Regelbeschreibung und die Kategorie definiert, unter denen die Regel auf der Oberfläche zur Regelkonfiguration angezeigt wird.  
  
### <a name="to-add-a-resource-file-and-three-resource-strings"></a>So fügen Sie eine Ressourcendatei und drei Ressourcenzeichenfolgen hinzu  
  
1. Wählen Sie im **Projektmappen-Explorer** das Projekt „SampleRules“ aus.  
  
2. Wählen Sie im Menü **Projekt** den Eintrag **Neues Element hinzufügen** aus. Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.  
  
3. Klicken Sie in der Liste **Installierte Vorlagen** auf **Allgemein**.  
  
4. Doppelklicken Sie im Detailbereich auf **Ressourcendatei**.  
  
5. Geben Sie unter **Name** „RuleResources.resx“ ein. Der Ressourcen-Editor wird ohne definierte Ressourcen angezeigt.  
  
6. Definieren Sie wie folgt vier Ressourcenzeichenfolgen:  
  
    |Name|Wert|  
    |--------|---------|  
    |AvoidWaitForDelay_ProblemDescription|Die WAITFOR DELAY-Anweisung wurde in {0} gefunden.|  
    |AvoidWaitForDelay_RuleName|Vermeiden von „WaitFor Delay“-Anweisungen in gespeicherten Prozeduren, Funktionen und Triggern.|  
    |CategorySamples|SamplesCategory|  
    |CannotCreateResourceManager|Der ResourceManager für {0} kann nicht aus {1} erstellt werden.|  
  
7. Klicken Sie im Menü **Datei** auf **RuleResources.resx speichern**.  
  
Im nächsten Schritt definieren Sie eine Klasse, die auf die Ressourcen in der Ressourcendatei verweist, die von Visual Studio zum Anzeigen von Informationen über Ihre Regel auf der Benutzeroberfläche verwendet werden.  
  
### <a name="defining-the-sampleconstants-class"></a>Definieren der „SampleConstants“-Klasse  
  
1. Wählen Sie im **Projektmappen-Explorer** das Projekt „SampleRules“ aus.  
  
2. Wählen Sie im Menü **Projekt** den Eintrag **Klasse hinzufügen** aus. Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.  
  
3. Geben Sie im Textfeld **Name** „SampleRuleConstants.cs“ ein, und klicken Sie auf die Schaltfläche **Hinzufügen**. Die Datei „SampleRuleConstants.cs“ wird dem Projekt im **Projektmappen-Explorer** hinzugefügt.  
  
4. Öffnen Sie die Datei „SampleRuleConstants.cs“, und fügen Sie der Datei die folgenden Using-Anweisungen hinzu:  
  
    ```  
    namespace SampleRules  
    {   
        internal static class RuleConstants  
        {  
            /// <summary>  
            /// The name of the resources file to use when looking up rule resources  
            /// </summary>  
            public const string ResourceBaseName = "Public.Dac.Samples.Rules.RuleResources";  
  
            /// <summary>  
            /// Lookup name inside the resources file for the select asterisk rule name  
            /// </summary>  
            public const string AvoidWaitForDelay_RuleName = "AvoidWaitForDelay_RuleName";  
            /// <summary>  
            /// Lookup ID inside the resources file for the select asterisk description  
            /// </summary>  
            public const string AvoidWaitForDelay_ProblemDescription = "AvoidWaitForDelay_ProblemDescription";  
  
            /// <summary>  
            /// The design category (should not be localized)  
            /// </summary>  
            public const string CategoryDesign = "Design";  
  
            /// <summary>  
            /// The performance category (should not be localized)  
            /// </summary>  
            public const string CategoryPerformance = "Design";  
        }  
    }  
    ```  
  
5. Klicken Sie auf **Datei** > **Speichern**.  
  
## <a name="creating-the-custom-code-analysis-rule-class"></a>Erstellen der benutzerdefinierten Codeanalyseregel-Klasse

Jetzt , da Sie die Hilfsklassen hinzugefügt haben, die von der benutzerdefinierten Codeanalyseregel verwendet werden, erstellen Sie eine benutzerdefinierte Regelklasse, die Sie „AvoidWaitForDelayRule“ nennen. Die benutzerdefinierte AvoidWaitForDelayRule-Regel wird verwendet, um Datenbankentwickler beim Vermeiden von WAITFOR DELAY-Anweisungen in gespeicherten Prozeduren, Triggern und Funktionen zu unterstützen.  
  
### <a name="creating-the-avoidwaitfordelayrule-class"></a>Erstellen der Klasse „AvoidWaitForDelayRule“  
  
1. Wählen Sie im **Projektmappen-Explorer** das Projekt „SampleRules“ aus.  
  
2. Wählen Sie im Menü **Projekt** den Eintrag **Klasse hinzufügen** aus. Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.  
  
3. Geben Sie im Textfeld **Name** „AvoidWaitForDelayRule.cs“ ein, und klicken Sie dann auf **Hinzufügen**. Die Datei „AvoidWaitForDelayRule.cs“ wird dem Projekt im **Projektmappen-Explorer** hinzugefügt.  
  
4. Öffnen Sie die Datei „AvoidWaitForDelayRule.cs“, und fügen Sie der Datei die folgenden Using-Anweisungen hinzu:  
  
    ```  
    using Microsoft.SqlServer.Dac.CodeAnalysis;  
    using Microsoft.SqlServer.Dac.Model;  
    using Microsoft.SqlServer.TransactSql.ScriptDom;  
    using System;  
    using System.Collections.Generic;  
    using System.Globalization;   
    namespace SampleRules {  
        class AvoidWaitForDelayRule {}  
    }  
    ```  
  
5. Ändern Sie in der Deklaration der AvoidWaitForDelayRule-Klasse den Zugriffsmodifizierer in öffentlich:  
  
    ```  
    /// <summary>  
    /// This is a rule that returns a warning message   
    /// whenever there is a WAITFOR DELAY statement appears inside a subroutine body.  
    /// This rule only applies to stored procedures, functions and triggers.  
    /// </summary>  
    public sealed class AvoidWaitForDelayRule  
    ```  
  
6. Leiten Sie die Klasse „AvoidWaitForDelayRule“ von der Basisklasse „Microsoft.SqlServer.Dac.CodeAnalysis.SqlCodeAnalysisRule“ ab:  
  
    ```  
    public sealed class AvoidWaitForDelayRule : SqlCodeAnalysisRule  
    ```  
  
7. Fügen Sie „LocalizedExportCodeAnalysisRuleAttribute“ Ihrer Klasse hinzu.  
  
    Mithilfe von „LocalizedExportCodeAnalysisRuleAttribute“ kann der Codeanalysedienst benutzerdefinierte Regeln zur Codeanalyse erkennen. Nur Klassen, die mit dem „ExportCodeAnalysisRuleAttribute“ (oder einem Attribut, das von diesem erbt) gekennzeichnet sind, können in der Codeanalyse verwendet werden.  
  
    „LocalizedExportCodeAnalysisRuleAttribute“ stellt einige erforderliche Metadaten bereit , die vom Dienst verwendet werden. Dies schließt eine eindeutige ID für diese Regel ein, einen Anzeigenamen, der auf der Visual Studio-Benutzeroberfläche angezeigt wird, und eine Beschreibung, die von Ihrer Regel beim Identifizieren von Problemen verwendet werden kann.  
  
    ```  
    [LocalizedExportCodeAnalysisRule(AvoidWaitForDelayRule.RuleId,  
        RuleConstants.ResourceBaseName,  
        RuleConstants.AvoidWaitForDelay_RuleName,   
        RuleConstants.AvoidWaitForDelay_ProblemDescription  
        Category = RuleConstants.CategoryPerformance,   
        RuleScope = SqlRuleScope.Element)]   
    public sealed class AvoidWaitForDelayRule : SqlCodeAnalysisRule  
    {  
       /// <summary>  
       /// The Rule ID should resemble a fully-qualified class name. In the Visual Studio UI  
       /// rules are grouped by "Namespace + Category", and each rule is shown using "Short ID: DisplayName".  
       /// For this rule, that means the grouping will be "Public.Dac.Samples.Performance", with the rule  
       /// shown as "SR1004: Avoid using WaitFor Delay statements in stored procedures, functions and triggers."  
       /// </summary>  
       public const string RuleId = "RuleSamples.SR1004";  
    }  
    ```  
  
    Die Eigenschaft „RuleScope“ sollte „Microsoft.SqlServer.Dac.CodeAnalysis.SqlRuleScope.Element“ sein, da diese Regel spezifische Elemente analysieren wird. Die Regel wird für jedes passende Element im Modell ein Mal aufgerufen. Wenn Sie ein gesamtes Modell analysieren möchten, können Sie stattdessen „Microsoft.SqlServer.Dac.CodeAnalysis.SqlRuleScope.Model“ verwenden.  
  
8. Fügen Sie einen Konstruktor hinzu, der „Microsoft.SqlServer.Dac.CodeAnalysis.SqlAnalysisRule.SupportedElementTypes“ einrichtet. Dies ist für Regeln mit einem Gültigkeitsbereich auf Elementebene erforderlich. Er definiert die Arten von Elementen, auf die diese Regel angewendet werden soll. In diesem Fall wird die Regel auf gespeicherte Prozeduren, Trigger und Funktionen angewendet. Beachten Sie, dass die Klasse „Microsoft.SqlServer.Dac.Model.ModelSchema“ alle verfügbaren Elementtypen auflistet, die analysiert werden können.  
  
    ```  
    public AvoidWaitForDelayRule()  
    {  
       // This rule supports Procedures, Functions and Triggers. Only those objects will be passed to the Analyze method  
       SupportedElementTypes = new[]  
       {  
          // Note: can use the ModelSchema definitions, or access the TypeClass for any of these types  
          ModelSchema.ExtendedProcedure,  
          ModelSchema.Procedure,  
          ModelSchema.TableValuedFunction,  
          ModelSchema.ScalarFunction,  
  
          ModelSchema.DatabaseDdlTrigger,  
          ModelSchema.DmlTrigger,  
          ModelSchema.ServerDdlTrigger  
       };  
    }  
    ```  
  
9. Fügen Sie eine Außerkraftsetzung für die Methode Microsoft.SqlServer.Dac.CodeAnalysis.SqlAnalysisRule.Analyze (Microsoft.SqlServer.Dac.CodeAnalysis.SqlRuleExecutionContext) hinzu, die „Microsoft.SqlServer.Dac.CodeAnalysis.SqlRuleExecutionContext“ als Eingabeparameter verwendet. Diese Methode gibt eine Liste der potenziellen Probleme zurück.  
  
    Die Methode ruft Microsoft.SqlServer.Dac.Model.TSqlModel, Microsoft.SqlServer.Dac.Model.TSqlObject und [TSqlFragment](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.tsqlfragment) aus dem Kontextparameter ab. Die „WaitForDelayVisitor“-Klasse wird anschließend verwendet, um eine Liste aller WAITFOR DELAY-Anweisungen im Modell abzurufen.  
  
    Für jede [WaitForStatement](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.waitforstatement) in dieser Liste wird eine „Microsoft.SqlServer.Dac.CodeAnalysis.SqlRuleProblem“ erstellt.  
  
    ```  
    /// <summary>  
    /// For element-scoped rules the Analyze method is executed once for every matching   
    /// object in the model.   
    /// </summary>  
    /// <param name="ruleExecutionContext">The context object contains the TSqlObject being   
    /// analyzed, a TSqlFragment  
    /// that's the AST representation of the object, the current rule's descriptor, and a   
    /// reference to the model being  
    /// analyzed.  
    /// </param>  
    /// <returns>A list of problems should be returned. These will be displayed in the Visual   
    /// Studio error list</returns>  
    public override IList<SqlRuleProblem> Analyze(  
        SqlRuleExecutionContext ruleExecutionContext)  
    {  
         IList<SqlRuleProblem> problems = new List<SqlRuleProblem>();  
  
         TSqlObject modelElement = ruleExecutionContext.ModelElement;  
  
         // this rule does not apply to inline table-valued function  
         // we simply do not return any problem in that case.  
         if (IsInlineTableValuedFunction(modelElement))  
         {  
             return problems;  
         }  
  
         string elementName = GetElementName(ruleExecutionContext, modelElement);  
  
         // The rule execution context has all the objects we'll need, including the   
         // fragment representing the object,  
         // and a descriptor that lets us access rule metadata  
         TSqlFragment fragment = ruleExecutionContext.ScriptFragment;  
         RuleDescriptor ruleDescriptor = ruleExecutionContext.RuleDescriptor;  
  
         // To process the fragment and identify WAITFOR DELAY statements we will use a   
         // visitor   
         WaitForDelayVisitor visitor = new WaitForDelayVisitor();  
         fragment.Accept(visitor);  
         IList<WaitForStatement> waitforDelayStatements = visitor.WaitForDelayStatements;  
  
         // Create problems for each WAITFOR DELAY statement found   
         // When creating a rule problem, always include the TSqlObject being analyzed. This   
         // is used to determine  
         // the name of the source this problem was found in and a best guess as to the   
         // line/column the problem was found at.  
         //  
         // In addition if you have a specific TSqlFragment that is related to the problem   
         //also include this  
         // since the most accurate source position information (start line and column) will   
         // be read from the fragment  
         foreach (WaitForStatement waitForStatement in waitforDelayStatements)  
         {  
            SqlRuleProblem problem = new SqlRuleProblem(  
                String.Format(CultureInfo.CurrentCulture,   
                    ruleDescriptor.DisplayDescription, elementName),  
                modelElement,  
                waitForStatement);  
            problems.Add(problem);  
        }  
        return problems;  
    }  
  
    private static string GetElementName(  
        SqlRuleExecutionContext ruleExecutionContext,   
        TSqlObject modelElement)  
    {  
        // Get the element name using the built in DisplayServices. This provides a number of   
        // useful formatting options to  
        // make a name user-readable  
        var displayServices = ruleExecutionContext.SchemaModel.DisplayServices;  
        string elementName = displayServices.GetElementName(  
            modelElement, ElementNameStyle.EscapedFullyQualifiedName);  
        return elementName;  
    }  
  
    private static bool IsInlineTableValuedFunction(TSqlObject modelElement)  
    {  
        return TableValuedFunction.TypeClass.Equals(modelElement.ObjectType)  
                       && FunctionType.InlineTableValuedFunction ==   
            modelElement.GetMetadata<FunctionType>(TableValuedFunction.FunctionType);  
    }  
    ```  
  
10. Klicken Sie auf **Datei** > **Speichern**.  
  
### <a name="building-the-class-library"></a>Erstellen der Klassenbibliothek  
  
1. Klicken Sie im Menü **Projekt** auf **SampleRules Properties** (Eigenschaften von „SampleRules“).  
  
2. Klicken Sie auf die Registerkarte **Signierung** .  
  
3. Klicken Sie auf **Assembly signieren**.  
  
4. Klicken Sie unter **Schlüsseldatei mit starkem Namen auswählen:** auf **<New>** .  
  
5. Geben Sie im Dialogfeld **Schlüssel für einen starken Namen erstellen** unter **Schlüsseldateiname** „MyRefKey“ ein.  
  
6. (optional) Sie können ein Kennwort für Ihre Schlüsseldatei mit starkem Namen angeben.  
  
7. Klicken Sie auf **OK**.  
  
8. Klicken Sie im Menü **Datei** auf **Alle speichern**.  
  
9. Klicken Sie im Menü **Build** auf **Projektmappe erstellen**.  
  
Als Nächstes müssen Sie die Assembly installieren, damit sie geladen wird, wenn Sie SQL Server-Projekte erstellen und bereitstellen.  
  
## <a name="install-a-static-code-analysis-rule"></a>Installieren einer Regel für die statische Codeanalyse

Zum Installieren einer Erstellungsregel müssen Sie die Assembly und die zugehörige PDB-Datei in den Erweiterungsordner kopieren.  
  
### <a name="to-install-the-samplerules-assembly"></a>So installieren Sie die „SampleRules“-Assembly

Im nächsten Schritt kopieren Sie die Assemblyinformationen in das Verzeichnis „Extensions“. Beim Start von Visual Studio werden alle Erweiterungen im Verzeichnis „<Visual Studio Install Dir>\Common7\IDE\Extensions\\Microsoft\SQLDB\DAC\120\Extensions“ und in dessen Unterverzeichnissen identifiziert und für die Verwendung zur Verfügung gestellt.  
  
Bei Visual Studio 2012 befindet sich <Visual Studio Install Dir> in der Regel im Verzeichnis „C:\Programme (x86)\\MicrosoftVisual Studio 11.0“. Bei Visual Studio 2013 ist dies in der Regel das Verzeichnis „C:\Programme (x86)\\MicrosoftVisual Studio 12.0“.  
  
Kopieren Sie die Assemblydatei „SampleRules.dll“ aus dem Ausgabeverzeichnis in das Verzeichnis „<Visual Studio Install Dir>\Common7\IDE\Extensions\\Microsoft\SQLDB\DAC\120\Extensions“. Der Pfad Ihrer kompilierten DLL-Datei lautet standardmäßig IhrProjektmappenpfad\IhrProjektpfad\bin\Debug oder IhrProjektmappenpfad\IhrProjektpfad\bin\Release.  
  
Die Regel sollte jetzt installiert sein und angezeigt werden, wenn Sie Visual Studio neu starten. Als Nächstes starten Sie eine neue Visual Studio-Sitzung und erstellen ein Datenbankprojekt.  
  
### <a name="starting-a-new-visual-studio-session-and-creating-a-database-project"></a>Starten einer neuen Visual Studio-Sitzung und Erstellen eines Datenbankprojekts  
  
1. Starten Sie eine zweite Visual Studio-Sitzung.  
  
2. Klicken Sie auf **Datei** > **Neu** > **Projekt**.  
  
3. Erweitern Sie im Dialogfeld **Neues Projekt** unter **Installierte Vorlagen** den Knoten **SQL Server**, und klicken Sie dann auf **SQL Server-Datenbankprojekt**.  
  
4. Geben Sie im Feld **Name** „SampleRulesDB“ ein, und klicken Sie auf **OK**.  
  
Die neue Regel wird schließlich im SQL Server-Projekt angezeigt. So zeigen Sie die neue Codeanalyseregel „AvoidWaitForRule“ an:  
  
1. Wählen Sie im **Projektmappen-Explorer** das Projekt „SampleRulesDB“ aus.  
  
2. Klicken Sie im Menü **Projekt** auf **Eigenschaften**. Die Eigenschaftenseite für „SampleRulesDB“ wird angezeigt.  
  
3. Klicken Sie auf **Codeanalyse**. Jetzt sollte eine neue Kategorie mit dem Namen „RuleSamples.CategorySamples“ angezeigt werden.  
  
4. Erweitern Sie „RuleSamples.CategorySamples“. Folgendes sollte angezeigt werden: „SR1004: Avoid WAITFOR DELAY statement in stored procedures, triggers, and functions“ (Vermeiden Sie die WAITFOR DELAY-Anweisung in gespeicherten Prozeduren, Triggern und Funktionen).  
  
## <a name="see-also"></a>Weitere Informationen

[Übersicht der Erweiterbarkeit um Regeln für die Datenbank-Codeanalyse](../ssdt/overview-of-extensibility-for-database-code-analysis-rules.md)