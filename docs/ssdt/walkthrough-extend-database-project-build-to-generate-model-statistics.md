---
title: Erweitern von Datenbankprojekten zum Generieren von Modellstatistiken
description: In diesem Artikel erfahren Sie, wie Sie einen Build Contributor erstellen, installieren und testen, der beim Erstellen eines Datenbankprojekts Statistiken von SQL-Datenbankmodellen ausgibt.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: d44935ce-63bf-46df-976a-5a54866c8119
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 5e3905a3ded61ac6ef99f9ada4e94ffaab77a36c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100016680"
---
# <a name="walkthrough-extend-database-project-build-to-generate-model-statistics"></a>Exemplarische Vorgehensweise: Erweitern von Datenbankprojekten zum Generieren von Modellstatistiken

Sie können einen Erstellungs-Contributor erstellen, um benutzerdefinierte Aktionen durchzuführen, wenn Sie ein Datenbankprojekt erstellen. In dieser exemplarischen Vorgehensweise erstellen Sie einen Erstellungs-Contributor mit der Bezeichnung ModelStatistics, von dem Statistiken über das SQL-Datenbankmodell ausgegeben werden, wenn Sie ein Datenbankprojekt erstellen. Da von diesem Erstellungs-Contributor beim Erstellen Parameter übernommen werden, sind einige zusätzliche Schritte erforderlich.  
  
In dieser exemplarischen Vorgehensweise führen Sie folgende Hauptaufgaben aus:  
  
-   [Erstellen eines Erstellungs-Contributors](#CreateBuildContributor)  
  
-   [Installieren des Erstellungs-Contributors](#InstallBuildContributor)  
  
-   [Testen des Erstellungs-Contributors](#TestBuildContributor)  
  
## <a name="prerequisites"></a>Voraussetzungen  
Zum Abschließen dieser exemplarischen Vorgehensweise benötigen Sie Folgendes:  
  
-   Sie müssen eine Version von Visual Studio installiert haben, die SQL Server Data Tools (SSDT) enthält und die Entwicklung in C# oder VB unterstützt.  
  
-   Sie müssen über ein SQL-Projekt verfügen, das SQL-Objekte enthält.  
  
> [!NOTE]  
> Diese exemplarische Vorgehensweise ist für Benutzer gedacht, die bereits mit den SQL-Funktionen von SSDT vertraut sind. Außerdem wird von Ihnen erwartet, dass Sie mit den grundlegenden Visual Studio-Konzepten vertraut sind, wie etwa dem Erstellen einer Klassenbibliothek und dem Verwenden des Code-Editor zum Hinzufügen von Code zu einer Klasse.  
  
## <a name="build-contributor-background"></a>Hintergrund des Erstellungs-Contributors  
Die Erstellungs-Contributors werden während des Projekterstellens und nach dem Generieren des Modells ausgeführt, das das Projekt darstellt, aber vor dem Speichern des Projekts auf dem Datenträger. Sie können für zahlreiche Szenarien verwendet werden, wie etwa  
  
-   Prüfen der Modellinhalte und Melden von Validierungsfehlern an den Aufrufer. Dies kann durch Hinzufügen von Fehlern zu einer Liste erfolgen, die als Parameter an die OnExecute-Methode übermittelt wird.  
  
-   Generieren von Modellstatistiken und Melden an den Benutzer. Dies wird im Beispiel hier dargestellt.  
  
Der Haupteinstiegspunkt für Erstellungs-Contributors ist die OnExecute-Methode. Alle Klassen, die von BuildContributor erben, müssen diese Methode implementieren. Ein BuildContributorContext-Objekt wird an diese Methode weitergeleitet. Dieses enthält alle relevanten Daten für den Build, wie etwa das Modell der Datenbank, Build-Eigenschaften und Argumente/Dateien, die von Erstellungs-Contributors verwendet werden.  
  
**TSqlModel und die Datenbankmodell-API**  
  
Das hilfreichste Objekt ist das Datenbankmodell, das von einem TSqlModel-Objekt dargestellt wird. Dies ist eine logische Darstellung einer Datenbank, einschließlich aller Tabellen, Ansichten und anderer Elemente sowie der Beziehungen zwischen diesen. Es gibt stark typisiertes Schema, das zum Abfragen nach spezifischen Elementtypen und Untersuchen interessanter Beziehungen verwendet werden kann. Im Code der exemplarischen Vorgehensweise sehen Sie Beispiele für die Verwendung.  
  
Hier sind einige der Befehle, die vom Beispiel-Contributor in dieser exemplarischen Vorgehensweise verwendet werden:  
  
|**Klasse**|**Methode/Eigenschaft**|**Beschreibung**|  
|-------------|------------------------|-------------------|  
|[TSqlModel](/dotnet/api/microsoft.sqlserver.dac.model.tsqlmodel)|GetObjects()|Fragt das Modell für Objekte ab und ist der Haupteinstiegspunkt für die Modell-API. Nur Typen auf der obersten Ebene wie etwa Tabelle oder Ansicht können abgerufen werden. Typen wie etwa Spalten können nur durch Durchsuchen des Modells gefunden werden. Falls keine ModelTypeClass-Filter angegeben werden, werden alle Typen auf oberster Ebene zurückgegeben.|  
|[TSqlObject](/dotnet/api/microsoft.sqlserver.dac.model.tsqlobject)|GetReferencedRelationshipInstances()|Sucht Beziehungen zu Elementen, auf die vom aktuellen TSqlObject verwiesen wird. Für eine Tabelle werden hiermit beispielsweise Objekte wie Spalten einer Tabelle zurückgegeben. In diesem Fall kann ein ModelRelationshipClass-Filter verwendet werden, um exakte Beziehung anzugeben, die abgefragt werden sollen (Die Verwendung des Filters „Table.Columns“ würde beispielsweise sicherstellen, dass nur Spalten zurückgegeben werden).<br /><br />Es gibt zahlreiche ähnliche Methoden wie etwa GetReferencingRelationshipInstances, GetChildren und GetParent. Weitere Informationen finden Sie in der API-Dokumentation.|  
  
**Eindeutiges Identifizieren Ihres Contributors**  
  
Während des Erstellungsprozesses werden benutzerdefinierte Contributors aus dem Standarderweiterungsverzeichnis geladen. Erstellungscontributors werden anhand eines [ExportBuildContributor](/dotnet/api/microsoft.sqlserver.dac.deployment.exportbuildcontributorattribute)-Attributs gekennzeichnet. Dieses Attribut muss angegeben werden, damit die Contributors ermittelt werden können. Dieses Attribut sollte folgendem ähnlich sehen:  
  
```  
[ExportBuildContributor("ExampleContributors.ModelStatistics", "1.0.0.0")]  
  
```  
  
In diesem Fall muss der erste Parameter des Attributs ein eindeutiger Bezeichner sein. Dieser wird verwendet, um den Contributor in Projektdateien zu kennzeichnen. Die beste Vorgehensweise ist die Kombination des Namespace der Bibliothek (in dieser exemplarischen Vorgehensweise „ExampleContributors“) mit dem Klassennamen (in dieser exemplarischen Vorgehensweise „ModelStatistics“), um den Bezeichner zu produzieren. Sie werden sehen, wie dieser Namespace zum Angeben verwendet wird, dass Ihr Contributor später in der exemplarischen Vorgehensweise verwendet werden soll.  
  
## <a name="create-a-build-contributor"></a><a name="CreateBuildContributor"></a>Erstellen eines Erstellungs-Contributors  
Zum Erstellen eines Erstellungs-Contributors führen Sie folgende Aufgaben aus:  
  
-   Erstellen Sie ein Klassenbibliotheksprojekt, und fügen Sie die erforderlichen Verweise hinzu.  
  
-   Definieren Sie eine Klasse mit der Bezeichnung „ModelStatistics“, die von [BuildContributor](/dotnet/api/microsoft.sqlserver.dac.deployment.buildcontributor) erbt.  
  
-   Überschreiben Sie die OnExecute-Methode.  
  
-   Fügen Sie einige private Hilfemethoden hinzu.  
  
-   Erstellen Sie die resultierende Assembly.  
  
#### <a name="to-create-a-class-library-project"></a>So erstellen Sie ein Klassenbibliotheksprojekt  
  
1.  Erstellen Sie ein Visual Basic- oder ein Visual C#-Klassenbibliotheksprojekt mit der Bezeichnung MyBuildContributor.  
  
2.  Benennen Sie die Datei „Class1.cs“ in „ModelStatistics.cs“ um.  
  
3.  Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektknoten, und klicken Sie dann auf **Verweis hinzufügen**.  
  
4.  Wählen Sie den Eintrag **System.ComponentModel.Composition** aus, und klicken Sie dann auf **OK**.  
  
5.  Erforderliche SQL-Verweise hinzufügen: Klicken Sie mit der rechten Maustaste auf den Projektknoten, und klicken Sie dann auf **Verweis hinzufügen**. Klicken Sie auf die Schaltfläche **Durchsuchen** . Navigieren Sie zum Ordner **C:\Programme (x86)\Microsoft SQL Server\110\DAC\Bin**. Wählen Sie die Einträge **Microsoft.SqlServer.Dac.dll**, **Microsoft.SqlServer.Dac.Extensions.dll** und **Microsoft.Data.Tools.Schema.Sql.dll** , und klicken Sie dann auf **OK**.  
  
    Beginnen Sie als Nächstes, der Klasse Code hinzuzufügen.  
  
#### <a name="to-define-the-modelstatistics-class"></a>So definieren Sie die ModelStatistics-Klasse  
  
1.  Von der ModelStatistics-Klasse wird das Datenbankmodell verarbeitet, das an die OnExecute-Methode übertragen wird, und ein XML-Bericht erstellt, der die Inhalte des Modells detailliert aufführt.  
  
    Aktualisieren Sie im Code-Editor die ModelStatistics.cs-Datei entsprechend folgendem Objekt:  
  
    ```csharp  
    using System;  
    using System.Collections.Generic;  
    using System.IO;  
    using System.Linq;  
    using System.Xml.Linq;  
    using Microsoft.Data.Schema;  
    using Microsoft.Data.Schema.Build;  
    using Microsoft.Data.Schema.Extensibility;  
    using Microsoft.Data.Schema.SchemaModel;  
    using Microsoft.Data.Schema.Sql;  
  
    namespace ExampleContributors  
    {  
    /// <summary>  
        /// A BuildContributor that generates statistics about a model and saves this to the output directory.  
        /// Will only run if a "GenerateModelStatistics=true" contributor argument is set in the project file, or a targets file.   
        /// Statistics can be sorted by "none, "name" or "value", with "none" being the default sort behavior.  
        ///   
        /// To set contributor arguments in a project file, add the following:  
        ///   
        /// <PropertyGroup>  
        ///     <ContributorArguments Condition="'$(Configuration)' == 'Debug'">  
        /// $(ContributorArguments);ModelStatistics.GenerateModelStatistics=true;ModelStatistics.SortModelStatisticsBy="name";  
        ///     </ContributorArguments>  
        /// <PropertyGroup>      
        ///   
        /// This will generate model statistics when building in Debug mode only - remove the condition to generate in all build modes.  
        /// </summary>  
        [ExportBuildContributor("ExampleContributors.ModelStatistics", "1.0.0.0")]  
        public class ModelStatistics : BuildContributor  
        {  
            public const string GenerateModelStatistics = "ModelStatistics.GenerateModelStatistics";  
            public const string SortModelStatisticsBy = "ModelStatistics.SortModelStatisticsBy";  
            public const string OutDir = "ModelStatistics.OutDir";  
            public const string ModelStatisticsFilename = "ModelStatistics.xml";  
            private enum SortBy { None, Name, Value };  
            private static Dictionary<string, SortBy> SortByMap = new Dictionary<string, SortBy>(StringComparer.OrdinalIgnoreCase)  
            {  
                { "none", SortBy.None },  
                { "name", SortBy.Name },  
                { "value", SortBy.Value },  
            };  
  
            private SortBy _sortBy = SortBy.None;  
  
            /// <summary>  
            /// Override the OnExecute method to perform actions when you build a database project.  
            /// </summary>  
            protected override void OnExecute(BuildContributorContext context, IList<ExtensibilityError> errors)  
            {  
                // handle related arguments, passed in as part of  
                // the context information.  
                bool generateModelStatistics;  
                ParseArguments(context.Arguments, errors, out generateModelStatistics);  
  
                // Only generate statistics if requested to do so  
                if (generateModelStatistics)  
                {  
                    // First, output model-wide information, such  
                    // as the type of database schema provider (DSP)  
                    // and the collation.  
                    StringBuilder statisticsMsg = new StringBuilder();  
                    statisticsMsg.AppendLine(" ")  
                                 .AppendLine("Model Statistics:")  
                                 .AppendLine("===")  
                                 .AppendLine(" ");  
                    errors.Add(new ExtensibilityError(statisticsMsg.ToString(), Severity.Message));  
  
                    var model = context.Model;  
  
                    // Start building up the XML that will later  
                    // be serialized.  
                    var xRoot = new XElement("ModelStatistics");  
  
                    SummarizeModelInfo(model, xRoot, errors);  
  
                    // First, count the elements that are contained   
                    // in this model.  
                    IList<TSqlObject> elements = model.GetObjects(DacQueryScopes.UserDefined).ToList();  
                    Summarize(elements, element => element.ObjectType.Name, "UserDefinedElements", xRoot, errors);  
  
                    // Now, count the elements that are defined in  
                    // another model. Examples include built-in types,  
                    // roles, filegroups, assemblies, and any   
                    // referenced objects from another database.  
                    elements = model.GetObjects(DacQueryScopes.BuiltIn | DacQueryScopes.SameDatabase | DacQueryScopes.System).ToList();  
                    Summarize(elements, element => element.ObjectType.Name, "OtherElements", xRoot, errors);  
  
                    // Now, count the number of each type  
                    // of relationship in the model.  
                    SurveyRelationships(model, xRoot, errors);  
  
                    // Determine where the user wants to save  
                    // the serialized XML file.  
                    string outDir;  
                    if (context.Arguments.TryGetValue(OutDir, out outDir) == false)  
                    {  
                        outDir = ".";  
                    }  
                    string filePath = Path.Combine(outDir, ModelStatisticsFilename);  
                    // Save the XML file and tell the user  
                    // where it was saved.  
                    xRoot.Save(filePath);  
                    ExtensibilityError resultArg = new ExtensibilityError("Result was saved to " + filePath, Severity.Message);  
                    errors.Add(resultArg);  
                }  
            }  
  
            /// <summary>  
            /// Examine the arguments provided by the user  
            /// to determine if model statistics should be generated  
            /// and, if so, how the results should be sorted.  
            /// </summary>  
            private void ParseArguments(IDictionary<string, string> arguments, IList<ExtensibilityError> errors, out bool generateModelStatistics)  
            {  
                // By default, we don't generate model statistics  
                generateModelStatistics = false;  
  
                // see if the user provided the GenerateModelStatistics   
                // option and if so, what value was it given.  
                string valueString;  
                arguments.TryGetValue(GenerateModelStatistics, out valueString);  
                if (string.IsNullOrWhiteSpace(valueString) == false)  
                {  
                    if (bool.TryParse(valueString, out generateModelStatistics) == false)  
                    {  
                        generateModelStatistics = false;  
  
                        // The value was not valid from the end user  
                        ExtensibilityError invalidArg = new ExtensibilityError(  
                            GenerateModelStatistics + "=" + valueString + " was not valid.  It can be true or false", Severity.Error);  
                        errors.Add(invalidArg);  
                        return;  
                    }  
                }  
  
                // Only worry about sort order if the user requested  
                // that we generate model statistics.  
                if (generateModelStatistics)  
                {  
                    // see if the user provided the sort option and  
                    // if so, what value was provided.  
                    arguments.TryGetValue(SortModelStatisticsBy, out valueString);  
                    if (string.IsNullOrWhiteSpace(valueString) == false)  
                    {  
                        SortBy sortBy;  
                        if (SortByMap.TryGetValue(valueString, out sortBy))  
                        {  
                            _sortBy = sortBy;  
                        }  
                        else  
                        {  
                            // The value was not valid from the end user  
                            ExtensibilityError invalidArg = new ExtensibilityError(  
                                SortModelStatisticsBy + "=" + valueString + " was not valid.  It can be none, name, or value", Severity.Error);  
                            errors.Add(invalidArg);  
                        }  
                    }  
                }  
            }  
  
            /// <summary>  
            /// Retrieve the database schema provider for the  
            /// model and the collation of that model.  
            /// Results are output to the console and added to the XML  
            /// being constructed.  
            /// </summary>  
            private static void SummarizeModelInfo(TSqlModel model, XElement xContainer, IList<ExtensibilityError> errors)  
            {  
                // use a Dictionary to accumulate the information  
                // that will later be output.  
                var info = new Dictionary<string, string>();  
  
                // Two things of interest: the database schema  
                // provider for the model, and the language id and  
                // case sensitivity of the collation of that  
                // model  
                info.Add("Version", model.Version.ToString());  
  
                TSqlObject options = model.GetObjects(DacQueryScopes.UserDefined, DatabaseOptions.TypeClass).FirstOrDefault();  
                if (options != null)  
                {  
                    info.Add("Collation", options.GetProperty<string>(DatabaseOptions.Collation));  
                }  
  
                // Output the accumulated information and add it to   
                // the XML.  
                OutputResult("Basic model info", info, xContainer, errors);  
            }  
  
            /// <summary>  
            /// For a provided list of model elements, count the number  
            /// of elements for each class name, sorted as specified  
            /// by the user.  
            /// Results are output to the console and added to the XML  
            /// being constructed.  
            /// </summary>  
            private void Summarize<T>(IList<T> set, Func<T, string> groupValue, string category, XElement xContainer, IList<ExtensibilityError> errors)  
            { // Use a Dictionary to keep all summarized information  
                var statistics = new Dictionary<string, int>();  
  
                // For each element in the provided list,  
                // count items based on the specified grouping  
                var groups =  
                    from item in set  
                    group item by groupValue(item) into g  
                    select new { g.Key, Count = g.Count() };  
  
                // order the groups as requested by the user  
                if (this._sortBy == SortBy.Name)  
                {  
                    groups = groups.OrderBy(group => group.Key);  
                }  
                else if (this._sortBy == SortBy.Value)  
                {  
                    groups = groups.OrderBy(group => group.Count);  
                }  
  
                // build the Dictionary of accumulated statistics  
                // that will be passed along to the OutputResult method.  
                foreach (var item in groups)  
                {  
                    statistics.Add(item.Key, item.Count);  
                }  
  
                statistics.Add("subtotal", set.Count);  
                statistics.Add("total items", groups.Count());  
  
                // output the results, and build up the XML  
                OutputResult(category, statistics, xContainer, errors);  
            }  
  
            /// <summary>  
            /// Iterate over all model elements, counting the  
            /// styles and types for relationships that reference each   
            /// element  
            /// Results are output to the console and added to the XML  
            /// being constructed.  
            /// </summary>  
            private static void SurveyRelationships(TSqlModel model, XElement xContainer, IList<ExtensibilityError> errors)  
            {  
                // get a list that contains all elements in the model  
                var elements = model.GetObjects(DacQueryScopes.All);  
                // We are interested in all relationships that  
                // reference each element.  
                var entries =  
                    from element in elements  
                    from entry in element.GetReferencedRelationshipInstances(DacExternalQueryScopes.All)  
                    select entry;  
  
                // initialize our counting buckets  
                var composing = 0;  
                var hierachical = 0;  
                var peer = 0;  
  
                // process each relationship, adding to the   
                // appropriate bucket for style and type.  
                foreach (var entry in entries)  
                {  
                    switch (entry.Relationship.Type)  
                    {  
                        case RelationshipType.Composing:  
                            ++composing;  
                            break;  
                        case RelationshipType.Hierarchical:  
                            ++hierachical;  
                            break;  
                        case RelationshipType.Peer:  
                            ++peer;  
                            break;  
                        default:  
                            break;  
                    }  
                }  
  
                // build a dictionary of data to pass along  
                // to the OutputResult method.  
                var stat = new Dictionary<string, int>  
                {  
                    {"Composing", composing},  
                    {"Hierarchical", hierachical},  
                    {"Peer", peer},  
                    {"subtotal", entries.Count()}  
                };  
  
                OutputResult("Relationships", stat, xContainer, errors);  
            }  
  
            /// <summary>  
            /// Performs the actual output for this contributor,  
            /// writing the specified set of statistics, and adding any   
            /// output information to the XML being constructed.  
            /// </summary>  
            private static void OutputResult<T>(string category, Dictionary<string, T> statistics, XElement xContainer, IList<ExtensibilityError> errors)  
            {  
                var maxLen = statistics.Max(stat => stat.Key.Length) + 2;  
                var format = string.Format("{{0, {0}}}: {{1}}", maxLen);  
  
                StringBuilder resultMessage = new StringBuilder();  
                //List<ExtensibilityError> args = new List<ExtensibilityError>();  
                resultMessage.AppendLine(category);  
                resultMessage.AppendLine("-----------------");  
  
                // Remove any blank spaces from the category name  
                var xCategory = new XElement(category.Replace(" ", ""));  
                xContainer.Add(xCategory);  
  
                foreach (var item in statistics)  
                {  
                    //Console.WriteLine(format, item.Key, item.Value);  
                    var entry = string.Format(format, item.Key, item.Value);  
                    resultMessage.AppendLine(entry);  
                    // Replace any blank spaces in the element key with  
                    // underscores.  
                    xCategory.Add(new XElement(item.Key.Replace(' ', '_'), item.Value));  
                }  
                resultMessage.AppendLine(" ");  
                errors.Add(new ExtensibilityError(resultMessage.ToString(), Severity.Message));  
            }  
        }  
    }  
  
    ```  
  
    Als Nächstes erstellen Sie die Klassenbibliothek.  
  
### <a name="to-sign-and-build-the-assembly"></a>So signieren und erstellen Sie die Assembly  
  
1.  Klicken Sie im Menü **Projekt** auf **Eigenschaften von MyBuildContributor**.  
  
2.  Klicken Sie auf die Registerkarte **Signierung** .  
  
3.  Klicken Sie auf **Assembly signieren**.  
  
4.  Klicken Sie unter **Schlüsseldatei mit starkem Namen auswählen:** auf **<New>** .  
  
5.  Geben Sie im Dialogfeld **Schlüssel für einen starken Namen erstellen** unter **Schlüsseldateiname** **MyRefKey** ein.  
  
6.  (optional) Sie können ein Kennwort für Ihre Schlüsseldatei mit starkem Namen angeben.  
  
7.  Klicken Sie auf **OK**.  
  
8.  Klicken Sie im Menü **Datei** auf **Alle speichern**.  
  
9. Klicken Sie im Menü **Build** auf **Projektmappe erstellen**.  
  
    Als Nächstes müssen Sie die Assembly installieren, damit sie geladen wird, wenn Sie SQL-Projekte erstellen.  
  
## <a name="install-a-build-contributor"></a><a name="InstallBuildContributor"></a>Installieren des Erstellungs-Contributors  
Zum Installieren eines Erstellungs-Contributors müssen Sie die Assembly und die zugehörige PDB-Datei in den Erweiterungsordner kopieren.  
  
#### <a name="to-install-the-mybuildcontributor-assembly"></a>So installieren Sie die MyBuildContributor-Assembly  
  
1.  Im nächsten Schritt kopieren Sie die Assemblyinformationen in das Verzeichnis „Extensions“. Beim Start von Visual Studio werden alle Erweiterungen im Ordner %Programme%\Microsoft SQL Server\110\DAC\Bin\Extensions und dessen Unterverzeichnissen identifiziert und für die Verwendung zur Verfügung gestellt.  
  
2.  Kopieren Sie die **MyBuildContributor.dll**-Assemblydatei aus dem Ausgabeverzeichnis in das Verzeichnis %Programme%\Microsoft SQL Server\110\DAC\Bin\Extensions.  
  
    > [!NOTE]  
    > Der Pfad Ihrer kompilierten DLL-Datei lautet standardmäßig IhrProjektmappenpfad\IhrProjektpfad\bin\Debug oder IhrProjektmappenpfad\IhrProjektpfad\bin\Release.  
  
## <a name="run-or-test-your-build-contributor"></a><a name="TestBuildContributor"></a>Ausführen oder Testen eines Erstellungs-Contributors  
Zum Ausführen oder Testen eines Erstellungs-Contributors führen Sie folgende Aufgaben aus:  
  
-   Fügen Sie der SQLPROJ-Datei, die Sie zu erstellen planen, Eigenschaften hinzu.  
  
-   Erstellen Sie das Datenbankprojekt mithilfe von MSBuild und durch Angeben der entsprechenden Parameter.  
  
### <a name="add-properties-to-the-sql-project-sqlproj-file"></a>So fügen Sie der SQL Project (.sqlproj)-Datei Eigenschaften hinzu  
Sie müssen die SQL-Projektdatei immer aktualisieren, um die ID der Contributor festzulegen, die Sie ausführen möchten. Da dieser Erstellungs-Contributor zusätzlich Befehlszeilenparameter von MSBuild akzeptiert, müssen Sie das SQL-Projekt bearbeiten, um Benutzern zu ermöglichen, dass diese Parameter MSBuild durchlaufen.  
  
Dazu haben Sie zwei Möglichkeiten:  
  
-   Sie können die SQLPROJ-Datei manuell bearbeiten, um die erforderlichen Argumente hinzuzufügen. Sie können sich dafür entscheiden, falls Sie den Erstellungs-Contributor nicht für zahlreiche Projekte wiederverwenden möchten. Falls Sie diese Option wählen, fügen Sie der SQLPROJ-Datei nach dem ersten Importknoten in der Datei die folgenden Anweisungen hinzu.  
  
    ```  
    /// <PropertyGroup>  
    ///     <ContributorArguments Condition="'$(Configuration)' == 'Debug'">  
    ///         $(ContributorArguments);ModelStatistics.GenerateModelStatistics=true;ModelStatistics.SortModelStatisticsBy="name";  
    ///     </ContributorArguments>  
    /// <PropertyGroup>  
  
    ```  
  
-   Die zweite Methode ist das Erstellen einer Zieledatei, die die erforderlichen Contributorargumente enthält. Dies ist hilfreich, wenn Sie denselben Contributor für mehrere Projekte verwenden, da er die Standardwerte enthält.  
  
    Erstellen Sie in diesem Fall eine Zieledatei im MSBuild-Erweiterungspfad:  
  
    1.  Navigieren Sie zu %Programme%\MSBuild\\.  
  
    2.  Erstellen Sie einen neuen Ordner namens „MyContributors“, in dem Ihre Zieldateien gespeichert werden.  
  
    3.  Erstellen Sie eine neue Datei „MyContributors.targets“ in diesem Verzeichnis, fügen Sie den folgenden Text hinzu, und speichern Sie dann die Datei:  
  
        ```  
        <?xml version="1.0" encoding="utf-8"?>  
  
        <Project xmlns="https://schemas.microsoft.com/developer/msbuild/2003">  
          <PropertyGroup>  
            <BuildContributors>$(BuildContributors);ExampleContributors.ModelStatistics</BuildContributors>  
            <ContributorArguments Condition="'$(Configuration)' == 'Debug'">$(ContributorArguments);ModelStatistics.GenerateModelStatistics=true;ModelStatistics.SortModelStatisticsBy=name;</ContributorArguments>  
          </PropertyGroup>  
        </Project>  
        ```  
  
    4.  Importieren Sie die Zieldatei innerhalb der SQLPROJ-Datei eines Projekts, in der Sie Build Contributors ausführen möchten, indem Sie in der SQLPROJ-Datei nach dem \<Import Project="$(MSBuildExtensionsPath)\Microsoft\VisualStudio\v$(VisualStudioVersion)\SSDT\Microsoft.Data.Tools.Schema.SqlTasks.targets" \/>-Knoten in der Datei die folgende Anweisung hinzufügen:  
  
        ```  
        <Import Project="$(MSBuildExtensionsPath)\MyContributors\MyContributors.targets " />  
        ```  
  
Nach dem Durchführen einer dieser Vorgehensweisen können Sie MSBuild verwenden, um die Parameter für Befehlszeilenbuilds zu übertragen.  
  
> [!NOTE]  
> Sie müssen die Eigenschaft „BuildContributors“ stets aktualisieren, damit Ihre Contributor-ID angegeben wird. Dies ist dieselbe ID, wie im Attribut „ExportBuildContributor“ in der Contributorquelldatei verwendet wird. Ohne diese wird Ihr Contributor nicht ausgeführt, wenn Sie das Projekt erstellen. Die Eigenschaft „ContributorArguments“ muss nur aktualisiert werden, wenn Argumente erforderlich sind, dass Ihr Contributor ausgeführt werden kann.  
  
### <a name="build-the-sql-project"></a>Erstellen Sie das SQL-Projekt.  
  
##### <a name="to-rebuild-your-database-project-by-using-msbuild-and-generate-statistics"></a>So erstellen Sie Ihr Datenbankprojekt mithilfe von MSBuild neu und generieren Statistiken  
  
1.  Klicken Sie in Visual Studio mit der rechten Maustaste auf Ihre Projekt, und wählen Sie „Neu erstellen”. Dadurch wird das Projekt neu erstellt. Sie sollten die generierten Modellstatistiken sehen, die die Ausgabe in der Erstellungsausgabe enthalten und in ModelStatistics.xml gespeichert werden. Beachten Sie, dass Sie möglicherweise im Projektmappen-Explorer „Alle Dateien anzeigen“ wählen müssen, um die XML-Datei zu sehen.  
  
2.  Klicken Sie im **Startmenü** auf **Alle Programme**, klicken Sie auf **Microsoft Visual Studio<Visual Studio Version>**, auf **Visual Studio Tools** und dann auf **Visual Studio-Eingabeaufforderung (<Visual Studio Version>)**.  
  
3.  Navigieren Sie in der Eingabeaufforderung zu dem Ordner, der Ihr SQL-Projekt enthält.  
  
4.  Geben Sie an der Eingabeaufforderung den folgenden Befehl ein:  
  
    ```  
    MSBuild /t:Rebuild MyDatabaseProject.sqlproj /p:BuildContributors=$(BuildContributors);ExampleContributors.ModelStatistics /p:ContributorArguments=$(ContributorArguments);GenerateModelStatistics=true;SortModelStatisticsBy=name;OutDir=.\;  
    ```  
  
    Ersetzen Sie *MyDatabaseProject* durch den Namen des zu erstellenden Datenbankprojekts. Falls Sie das Projekt nach dem letzten Erstellen geändert haben, können Sie /t:Build anstelle von /t:Rebuild verwenden.  
  
    In der Ausgabe sehen Sie Erstellungsinformationen wie Folgende:  
  
```  
Model Statistics:  
===  
  
Basic model info  
-----------------  
    Version: Sql110  
  Collation: SQL_Latin1_General_CP1_CI_AS  
  
UserDefinedElements  
-----------------  
  DatabaseOptions: 1  
         subtotal: 1  
      total items: 1  
  
OtherElements  
-----------------  
                Assembly: 1  
       BuiltInServerRole: 9  
           ClrTypeMethod: 218  
  ClrTypeMethodParameter: 197  
         ClrTypeProperty: 20  
                Contract: 6  
                DataType: 34  
                Endpoint: 5  
               Filegroup: 1  
             MessageType: 14  
                   Queue: 3  
                    Role: 10  
                  Schema: 13  
                 Service: 3  
                    User: 4  
         UserDefinedType: 3  
                subtotal: 541  
             total items: 16  
  
Relationships  
-----------------  
     Composing: 477  
  Hierarchical: 6  
          Peer: 19  
      subtotal: 502  
  
```  
  
1.  Öffnen Sie ModelStatistics.xml, und untersuchen Sie den Inhalt.  
  
    Die gemeldeten Ergebnisse sind auch in der XML-Datei enthalten.  
  
## <a name="next-steps"></a>Nächste Schritte  
Sie können weitere Tools erstellen, um die Verarbeitung der Ausgabe-XML-Datei durchzuführen. Dies ist nur ein Beispiel eines Erstellungs-Contributors. Sie können beispielsweise einen Erstellungs-Contributor erstellen, um eine Datenwörterbuch-Datei als Teil Ihres Builds auszugeben.  
  
## <a name="see-also"></a>Weitere Informationen  
[Anpassen der Datenbankerstellung und -bereitstellung durch Erstellungs- und Bereitstellungs-Contributors](../ssdt/use-deployment-contributors-to-customize-database-build-and-deployment.md)  
[Exemplarische Vorgehensweise: Erweitern der Bereitstellung eines Datenbankprojekts zum Analysieren des Bereitstellungsplans](../ssdt/walkthrough-extend-database-project-deployment-to-analyze-the-deployment-plan.md)  
