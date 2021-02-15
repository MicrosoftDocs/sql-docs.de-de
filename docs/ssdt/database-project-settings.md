---
title: Datenbankprojekteinstellungen
description: In diesem Artikel erhalten Sie Informationen zu Datenbankprojekteinstellungen. Außerdem erfahren Sie, wie mithilfe dieser Einstellungen Aspekte der Datenbank-, Debug- und Buildkonfigurationen gesteuert werden.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.DebugProperties
- sql.data.tools.dacsettings.dialog
- sql.data.tools.dbsqlclrlanguagevb
- SQL.DATA.TOOLS.DBADVANCEDBUILDSETTINGSCS
- sql.data.tools.dbadvancedbuildsettingscs
- ql.data.tools.criticalerror.dialog
- sql.data.tools.dbassemblysigningchangekeypassword
- sql.data.tools.advanceddeploymentsystemdefined.dialog
- sql.data.tools.advanceddatabasesettings.dialog
- sql.data.tools.extendedpropertiesvalueeditor.dialog
- sql.data.tools.dbsqlclrlanguagecs
- ql.data.tools.dbassemblysigningpasswordneeded
- sql.data.tools.vbadvancedsettings.dialog
- sql.data.tools.dbsqlclr
- sql.data.tools.SqlCmdVariablesProperties
- sql.data.tools.dbassemblysigning
- sql.data.tools.advanceddeploymentsettings.dialog
- sql.data.tools.advancedpublishsettings.dialog
- sql.data.tools.BuildActionProperties
- sql.data.tools.serverselectionpolicy.dialog
- sql.data.tools.dbadvancedbuildsettingsvb
- sql.data.tools.dbprojectwizard.buildeventcommandline
- sql.data.tools.dbreferencepaths
- sql.data.tools.GeneralProperties
- sql.data.tools.BuildProperties
- sql.data.tools.DeployProperties
- sql.data.tools.csadvancedsettings.dialog
- sql.data.tools.dbassemblyinfo
- sql.data.tools.extendedpropertieseditor.dialog
ms.assetid: 34418730-1aaa-4948-aee2-8f1e62cda85c
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 99552883fb06c01a862ddc973ff3747d11a89ee8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100079551"
---
# <a name="database-project-settings"></a>Datenbankprojekteinstellungen

Mit Datenbankprojekteinstellungen werden Aspekte der Datenbank-, Debug- und Buildkonfigurationen gesteuert. Diese Einstellungen werden in die folgenden Kategorien eingeteilt.  
  
-   [Projekteinstellungen](#bkmk_proj_settings)  
  
-   [Erweiterte Transact-SQL-Überprüfung](#bkmk_evf)  
  
-   [SQLCLR](#bkmk_sqlclr)  
  
-   [SQLCLR- und SQLCLR-Build](#bkmk_sqlclr_sqlclrbuild)  
  
-   [Build](#bkmk_build)  
  
-   [SQLCMD-Variablen](#bkmk_sqlcmd_variables)  
  
-   [Buildereignisse](#bkmk_build_events)  
  
-   [Debuggen](#bkmk_debug)  
  
-   [Verweispfade](#bkmk_ref_paths)  
  
-   [Codeanalyse](#bkmk_code_analysis)  
  
### <a name="to-configure-properties-for-your-database-project"></a>So konfigurieren Sie die Eigenschaften für das Datenbankprojekt  
  
1.  Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Datenbankprojekt, für das Sie Eigenschaften konfigurieren möchten, und wählen Sie **Eigenschaften** aus.  
  
    Alternativ können Sie auf den Knoten **Eigenschaften** des Projekts im **Projektmappen-Explorer** doppelklicken.  
  
2.  Das Eigenschaftenblatt für das Datenbankprojekt wird angezeigt.  
  
3.  Klicken Sie auf die Registerkarte **Projekteinstellungen** . Jetzt können Sie die allgemeinen Eigenschaften der Eigenschaften des Datenbankprojekts konfigurieren. Im linken Fensterbereich sind verschiedene Registerkarten (für unterschiedliche Kategorien) verfügbar.  
  
## <a name="project-settings"></a><a name="bkmk_proj_settings"></a>Projekteinstellungen  
Die Einstellungen in der folgenden Tabelle gelten für alle Konfigurationen dieses Datenbankprojekts.  
  
|Feld|Standardwert|BESCHREIBUNG|  
|---------|-----------------|---------------|  
|Zielplattform|Microsoft SQL Server 2012|Gibt die Version von SQL Server an, die das Ziel dieses Datenbankprojekts ist.|  
|Erweiterte Transact\-SQL-Überprüfung für gemeinsame Objekte aktivieren|Nicht aktiviert, wenn Sie ein neues Projekt erstellen.<br /><br />Aktiviert, wenn Sie ein Projekt aus dem SQL Server-Objekt-Explorer erstellen, das mit SQL Azure verbunden ist, eine Instanz der Azure SQL-Datenbank in das Projekt importieren oder die Zielplattform eines Projekts in SQL Azure ändern.|Wenn diese Option aktiviert ist, werden Fehler im Projekt, dessen Überprüfung durch den SQL Server-Compiler fehlgeschlagen ist, gemeldet. Wenn Sie die Zielplattform in SQL Azure ändern, wird die erweiterte Überprüfung aktiviert. Die Option wird nicht deaktiviert, wenn Sie die Zielplattform ändern.<br /><br />Sie können diese Option für andere Versionen von SQL Server aktivieren, die Überprüfung ist jedoch auf teilweise eigenständige Datenbanken von Microsoft SQL Server 2012 sowie auf SQL Azure beschränkt. Nicht die gesamte Transact\-SQL-Syntax wird für alle Versionen von SQL Server unterstützt.<br /><br />Weitere Informationen finden Sie unter [Erweiterte Transact-SQL-Überprüfung](#bkmk_evf) weiter unten in diesem Thema.|  
|Ausgabetypen|||  
|Datenebenenanwendung (DACPAC-Datei)|Aktiviert und gesperrt. In der Buildausgabe eines Datenbankprojekts wird beim Erstellen des Projekts immer ein DACPAC-Paket erzeugt.|Wenn Sie die SQL Server Data Tools-Version (SSDT) mit der Option „Zusätzliche DACPAC-Downleveldatei (v2.0) erstellen“ verwenden, aktivieren Sie die Option, wenn das Paket mit SQL Server Management Studio oder dem SQL Azure-Verwaltungsportal kompatibel sein soll. Sie können ein DACPAC-Paket direkt aus SSDT bereitstellen. Eine DACPAC-Datei der Version 2.0 kann jedoch über SQL Server Management Studio nur zum Zeitpunkt der Freigabe von SQL Server Data Tools bereitgestellt werden.|  
|Skript erstellen (.sql-Datei)||Legt fest, ob beim Erstellen des Projekts ein vollständiges SQL-CREATE-Skript für alle Objekte im Projekt erstellt und im Ordner "bin\debug" abgelegt wird. Sie können mithilfe des Befehls **Projekt veröffentlichen** oder mit dem SQL Compare-Hilfsprogramm ein Skript für inkrementelle Updates erstellen.|  
|Allgemein|||  
|Standardschema|dbo|Gibt das Standardschema an, in dem sowohl SQLCLR-Objekte als auch Transact\-SQL-Objekte erstellt werden. Sie können diese Einstellung überschreiben, indem Sie das Schema direkt für Objekte angeben.|  
|Schemanamen in Dateinamen einschließen|nein|Gibt an, ob das Schema als Präfix in Dateinamen enthalten ist (z. B. "dbo.Products.table.sql"). Wenn dieses Kontrollkästchen deaktiviert ist, werden Dateinamen für Objekte im Format „Objektname.Objekttyp.sql“ erstellt (z.B. „Products.table.sql“).|  
|Groß-/Kleinschreibung für Bezeichner überprüfen|ja|Gibt an, ob bei der Erstellung des Projekts die Groß-/Kleinschreibung für Bezeichner in den SQL-Objekten im Projekt überprüft wird. Diese Option wird auf Datenbankprojekte angewendet, in denen eine Sortierung mit Berücksichtigung der Groß- und Kleinschreibung für die Datenbank angegeben wird.|  
|Datenbankeinstellungen|Standardeinstellungen, die auf den Standardkonfigurationseinstellungen für eine Datenbank basieren|Beispiele für Einstellungen, die angegeben werden können, sind Einstellungen für die Sortierungsmethode und Datenbankebenen für eine SQL Server-Datenbank.|  
  
## <a name="extended-transact-sql-verification"></a><a name="bkmk_evf"></a>Erweiterte Transact-SQL-Überprüfung  
  
> [!IMPORTANT]  
> Die Funktion zur erweiterten Transact-SQL-Überprüfung wird aus der nächsten Featureversion von SQL Server Data Tools und dem nächsten Hauptrelease von Visual Studio entfernt.  
  
Die erweiterte Transact-SQL-Überprüfung ist eine Funktion innerhalb des Datenbankprojektsystems, mit deren Hilfe Entwickler ihr Datenbankprojekt zur Buildzeit vom Transact-SQL-Compilerdienst für Microsoft SQL Server 2014 überprüfen lassen können, indem der Projektcode mit dem Parser und Interpreter von SQL Server Engine überprüft wird.  
  
### <a name="transact-sql-compiler-service"></a>Transact-SQL Compiler Service  
Der Transact-SQL-Compilerdienst ist eine Komponente, die auf der Microsoft SQL Server 2012-Datenbank-Engine basiert. Dieser Dienst ist in der Lage, die Syntax und Semantik von DDL-Anweisungen mit derselben Genauigkeit wie eine Microsoft SQL Server 2012-Datenbank-Engine zu überprüfen. Dies bedeutet auch, dass der Compilerdienst keine Syntax oder Funktionen unterstützt, die in Microsoft SQL Server 2012 als veraltet markiert wurden. Weitere Informationen zu veralteten Funktionen finden Sie unter [Nicht mehr unterstützte Datenbank-Engine-Funktionalität in SQL Server 2012](../database-engine/discontinued-database-engine-functionality-in-sql-server.md).  
  
Zur Validierung des Datenbankprojekts erstellt der Compilerdienst eine teilweise eigenständige Datenbank und simuliert die Ausführung der DDL-Anweisungen für die Datenbank. Weitere Informationen finden Sie unter [Teilweise eigenständige Datenbanken](/previous-versions/sql/sql-server-2012/ff929071(v=sql.110)).  
  
Der Compilerdienst weist zwei unterschiedliche Einschränkungen auf.  
  
Funktionen, die auf der Datenbank- oder Instanzkonfiguration basieren, z. B.:  
  
-   drei- oder vierteilige Objektverweise  
  
-   FileTables  
  
-   Change Tracking  
  
-   Rowsetfunktionen – OPENROWSET, OPENQUERY, OPENDATASOURCE  
  
-   Semantische Volltextsuche  
  
Funktionen, die derzeit keine Validierung unterstützen, z. B.:  
  
-   Service Broker  
  
-   Partitionierte Schemas mit benutzerdefinierten Dateigruppen  
  
-   SQL Azure-Metadatensortierung (der Compilerdienst verwendet die Metadatensortierung für teilweise eigenständige Datenbanken von SQL Server 2012 - Latin1_General_100_CI_AS_KS_WS_SC)  
  
### <a name="enablingdisabling-extended-verification"></a>Aktivieren/Deaktivieren der erweiterten Überprüfung  
Bei Datenbankprojekten, die direkt aus einer Azure SQL-Datenbank erstellt wurden, bzw. bei Projekten, deren Zielplattform auf SQL Azure festgelegt ist, wird die erweiterte Transact-SQL-Überprüfung standardmäßig aktiviert. Es wird empfohlen, die erweiterte Überprüfung bei der Entwicklung für SQL Azure oder bei Verwendung einer Datenbank im Anwendungsbereich zu verwenden, die für SQL Server 2012 konzipiert ist. Weitere Informationen zu Datenbanken im Anwendungsbereich finden Sie unter [Teilweise eigenständige Datenbanken](/previous-versions/sql/sql-server-2012/ff929071(v=sql.110)).  
  
Die erweiterte Überprüfungsfunktion kann auch verwendet werden, wenn eine Datenbank im Anwendungsbereich für SQL Server 2008/R2 entwickelt wird, um Kompatibilität mit Microsoft SQL Server 2012 und SQL Azure zu erzielen.  
  
##### <a name="to-enable-or-disable-extended-verification-at-the-project-level"></a>So aktivieren oder deaktivieren Sie die erweiterte Überprüfung auf Projektebene  
  
1.  Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf die Projektdatei, und klicken Sie dann auf **Eigenschaften**.  
  
2.  Aktivieren oder deaktivieren Sie in den **Projekteinstellungen** unter **Zielplattform** die Option **Erweiterte Transact-SQL-Überprüfung für gemeinsame Objekte aktivieren**.  
  
##### <a name="to-disable-extended-verification-at-the-file-level"></a>So deaktivieren Sie die erweiterte Überprüfung auf Dateiebene  
  
1.  Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf eine SQL-Datei.  
  
    > [!NOTE]  
    > Zum Deaktivieren der Funktion für die erweiterte Transact\-SQL-Überprüfung auf Dateiebene muss die Eigenschaft **Buildvorgang** der Datei auf **Build** festgelegt werden.  
  
2.  Ändern Sie unter **Eigenschaften** die Eigenschaft **Erweiterte T-SQL-Überprüfung** in **False**.  
  
    ![Dateieigenschaften](../ssdt/media/ssdt-evf.gif "Dateieigenschaften")  
  
### <a name="special-considerations-for-collations"></a>Spezielle Überlegungen zu Sortierungen  
Weitere Informationen zu Sortierungen in teilweise eigenständigen Datenbanken finden Sie unter [Sortierungen in eigenständigen Datenbanken](/previous-versions/sql/sql-server-2012/ff929080(v=sql.110)).  
  
## <a name="sqlclr"></a><a name="bkmk_sqlclr"></a>SQLCLR  
Weitere Informationen zu den Assemblyoptionen finden Sie unter [Dialogfeld "Assemblyinformationen"](/visualstudio/ide/reference/assembly-information-dialog-box?queryresult=true).  
  
Weitere Informationen zum Signieren finden Sie im Abschnitt **Signieren von Assemblys** im Thema [Seite "Signierung", Projekt-Designer](/visualstudio/ide/reference/signing-page-project-designer?queryresult=true) .  
  
## <a name="sqlclr-and-sqlclr-build"></a><a name="bkmk_sqlclr_sqlclrbuild"></a>SQLCLR- und SQLCLR-Build  
Die Eigenschaftenseiten **SQLCLR** und **SQLCLR Build** enthalten viele Einstellungen zum Verwenden von SQL CLR-Objekten im Projekt. Insbesondere enthält die Eigenschaftenseite **SQLCLR** eine Berechtigungsstufeneinstellung zum Festlegen von Berechtigungen für die SQLCLR-Assembly. Sie enthält außerdem die Einstellung „DDL generieren“, um zu steuern, ob DDL (Dynamic Data Language) für die dem Projekt hinzugefügten SQLCLR-Objekte generiert wird. Die Eigenschaftenseite **SQLCLR Build** enthält alle Compileroptionen, die Sie festlegen können, um die Kompilierung des SQLCLR-Codes im Projekt zu konfigurieren.  
  
Die Eigenschaftenseite **SQLCLR Build** enthält erweiterte Buildeinstellungen zum Erstellen von SQL CLR-Objekten. Je nach der für die Programmierung der SQL CLR-Objekte verwendeten Sprache (VB oder C#) sind unterschiedliche Optionen verfügbar.  
  
1.  Wenn das Objekt in C# geschrieben ist, können Sie auf die Optionen zugreifen, indem Sie auf der Eigenschaftenseite **SQLCLR-Build** auf die Schaltfläche **Erweitert** klicken. Beschreibungen für C#-Optionen finden Sie unter [Dialogfeld „Erweiterte Buildeinstellungen“ (C#)](/visualstudio/ide/reference/advanced-build-settings-dialog-box-csharp).  
  
2.  Wenn das Objekt in VB geschrieben ist, können Sie zunächst in der Dropdownliste **Sprache** "VB" auswählen und dann auf die Schaltfläche **Erweitert** klicken. Beschreibungen für VB-Optionen finden Sie unter [Dialogfeld „Erweiterte Compilereinstellungen“ (Visual Basic)](/visualstudio/ide/reference/advanced-compiler-settings-dialog-box-visual-basic).  
  

## <a name="build"></a><a name="bkmk_build"></a>Build  
Sie können für jedes Datenbankprojekt in der Projektmappe eine Buildkonfiguration auswählen. Standardmäßig ist eine einzige Konfiguration vorhanden, Sie können jedoch benutzerdefinierte Konfigurationen hinzufügen. Dies empfiehlt sich beispielsweise, wenn Sie eine benutzerdefinierte Konfiguration benötigen, in der Sie die Datenbank immer löschen und neu erstellen können. In Projektmappen mit unterschiedlichen Projekttypen können Sie eine benutzerdefinierte Projektmappenkonfiguration erstellen, die für jedes Projekt eine bestimmte Buildkonfiguration enthält.  
  
#### <a name="to-specify-a-build-configuration-for-a-solution"></a>So geben Sie eine Buildkonfiguration für eine Projektmappe an  
  
1.  Klicken Sie im **Projektmappen-Explorer** auf den Projektmappenknoten, für den Sie eine Buildkonfiguration angeben möchten.  
  
2.  Klicken Sie im Menü **Build** auf **Konfigurations-Manager**. Das Dialogfeld **Konfigurations-Manager** wird angezeigt.  
  
    Geben Sie die Konfigurationseinstellungen an, die Sie für die einzelnen Projekte in der Projektmappe verwenden möchten.  
  
#### <a name="to-specify-a-build-configuration-for-a-database-project"></a>So geben Sie eine Buildkonfiguration für ein Datenbankprojekt an  
  
1.  Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Datenbankprojekt, für das Sie eine Buildkonfiguration angeben möchten, und wählen Sie **Eigenschaften** aus.  
  
2.  Geben Sie auf der Registerkarte **Build** über die Dropdownliste **Konfiguration** die Konfigurationseinstellungen an, die Sie für dieses Projekt verwenden möchten.  
  
Die Einstellungen in der folgenden Tabelle gelten für alle Buildkonfigurationen dieses Datenbankprojekts.  
  
|Feld|Standardwert|BESCHREIBUNG|  
|---------|-----------------|---------------|  
|Buildausgabepfad|bin\Debug\|Gibt an, wo die Buildausgabe generiert wird, wenn Sie das Datenbankprojekt erstellen oder bereitstellen. Einen relativen Pfad müssen Sie relativ zum Pfad des Datenbankprojekts angeben. Wenn der Pfad nicht vorhanden, wird er erstellt.|  
|Name der Buildausgabedatei|*DatabaseProjectName*|Gibt den Namen für die Ausgabe an, die beim Erstellen des Datenbankprojekts generiert wird.|  
|Transact\-SQL-Warnungen als Fehler behandeln|Nein|Gibt an, ob eine Transact\-SQL-Warnung dazu führen soll, dass der Build- und Bereitstellungsprozess abgebrochen wird. Wenn dieses Kontrollkästchen deaktiviert ist, werden Warnungen angezeigt, Erstellung und Bereitstellung werden aber fortgesetzt. Diese Einstellung ist für das Projekt, nicht für den Benutzer, spezifisch und wird in der SQLPROJ-Datei gespeichert.|  
|Transact\-SQL-Warnungen unterdrücken|Leer|Gibt eine durch Kommas oder Semikolons getrennte Liste mit Warnungsnummern an, die unterdrückte Warnungen bezeichnen.<br /><br />Unterdrückte Warnungen werden nicht im Fenster **Fehlerliste** angezeigt und wirken sich nicht auf den Erfolg des Buildvorgangs aus, auch nicht bei aktiviertem Kontrollkästchen **Transact\-SQL-Warnungen als Fehler behandeln**.|  
  
## <a name="sqlcmd-variables"></a><a name="bkmk_sqlcmd_variables"></a>SQLCMD-Variablen  
In SQL Server-Datenbankprojekten können Sie mit SQLCMD-Variablen angeben, dass die dynamische Ersetzung für das Debuggen und Veröffentlichen verwendet werden soll. Sie geben den Variablennamen und die Variablenwerte ein, und während des Buildvorgangs werden die Werte ersetzt. Wenn keine lokalen Werte vorhanden sind, wird der Standardwert verwendet. Durch die Eingabe dieser Variablen in den Projekteigenschaften werden sie automatisch beim Veröffentlichen angeboten, und sie werden in Veröffentlichungsprofilen gespeichert. Sie können die Projektwerte der Variablen in der Veröffentlichung über die Schaltfläche "Werte laden" aufrufen.  
  
Vergewissern Sie sich, dass die richtigen Variablen in den Projekteigenschaften eingegeben sind, denn diese Variablen werden nicht anhand eines Skripts im Projekt überprüft, und die im Skript verwendeten Variablen werden nicht automatisch aufgefüllt.  
  
Außerdem können Sie durch die Veröffentlichung über die Befehlszeile diese Werte in der Befehlszeile oder mit einem Profil überschreiben.  
  
## <a name="build-events"></a><a name="bkmk_build_events"></a>Buildereignisse  
Mit dieser Einstellung können Sie eine Befehlszeile angeben, die vor der Ausführung des Buildvorgangs ausgeführt wird, sowie eine Befehlszeile, die nach Abschluss des Buildvorgangs ausgeführt wird.  
  
|Feld|Standardwert|BESCHREIBUNG|  
|---------|-----------------|---------------|  
|Befehlszeile für Präbuildereignis|Keine|Gibt die vor der Erstellung des Projekts auszuführende Befehlszeile an. Klicken Sie auf **Präbuild bearbeiten**, um die Befehlszeile zu ändern.|  
|Befehlszeile für Postbuildereignis|Keine|Gibt die nach der Erstellung des Projekts auszuführende Befehlszeile an. Klicken Sie auf **Postbuild bearbeiten**, um die Befehlszeile zu ändern.|  
|Postbuildereignis ausführen|Bei erfolgreichem Erstellen|Gibt an, ob die Postbuildbefehlszeile immer, nur bei erfolgreicher Erstellung oder nur dann ausgeführt werden soll, wenn die Projektausgabe (das Buildskript) durch die Erstellung aktualisiert wurde.|  
  
## <a name="debug"></a><a name="bkmk_debug"></a>Debuggen  
Mit diesen Einstellungen können Sie das Debuggen des Datenbankprojekts steuern.  
  
|Feld|Standardwert|BESCHREIBUNG|  
|---------|-----------------|---------------|  
|Startvorgang|Keine|Gibt ein Skript oder externes Programm an, das beim Debuggen des Projekts ausgeführt werden soll.|  
|Zielverbindungszeichenfolge|Data Source=(localdb)\\*SolutionName*;Initial Catalog=*DatabaseProjectName*;Integrated Security=True;Pooling=False;Connect Timeout=30|Gibt die Verbindungsinformationen für den Datenbankserver an, der für die angegebene Buildkonfiguration verwendet werden soll. Die Standardverbindungszeichenfolge bezieht sich auf eine dynamisch erstellte SQL Server LocalDB-Instanz und -Datenbank.|  
|Datenbankeigenschaften bereitstellen|Ja|Gibt an, ob die Einstellungen in DatabaseProperties.DatabaseProperties bereitgestellt oder aktualisiert werden, wenn Sie das Datenbankprojekt bereitstellen.|  
|Datenbank immer neu erstellen|Nein|Gibt an, ob die Datenbank gelöscht und neu erstellt wird, anstatt ein inkrementelles Upgrade durchzuführen. Dieses Kontrollkästchen können Sie beispielsweise aktivieren, wenn Sie Datenbankkomponententests für eine neue Bereitstellung der Datenbank ausführen möchten. Wenn dieses Kontrollkästchen deaktiviert wird, wird die vorhandene Datenbank nicht gelöscht und neu erstellt, sondern aktualisiert.|  
|Inkrementelle Bereitstellung blockieren, wenn Datenverlust auftreten könnte|Ja|Gibt an, ob die Bereitstellung angehalten wird, wenn ein Update Datenverluste verursachen kann. Wenn dieses Kontrollkästchen aktiviert ist, verursachen Änderungen, die zu Datenverlusten führen, das Beenden der Bereitstellung mit einem Fehler, der den Datenverlust verhindert. Die Bereitstellung wird beispielsweise beendet, wenn eine `varchar(50)` -Spalte in `varchar(30)`geändert wird.<br /><br />**HINWEIS:** Die Bereitstellung wird nur blockiert, wenn die Tabellen, in denen es zu einem Datenverlust kommen könnte, Daten enthalten. Die Bereitstellung wird fortgesetzt, wenn keine Daten verloren gehen können.|  
|DROP-Objekte im Ziel, aber nicht im Projekt|Nein|Gibt an, ob Objekte, die sich in der Zieldatenbank, aber nicht im Datenbankprojekt befinden, im Rahmen des Bereitstellungsskripts gelöscht werden sollen. Sie können einige Dateien im Projekt ausschließen, um sie vorübergehend aus dem Buildskript zu entfernen. Sie können jedoch die vorhandenen Versionen solcher Objekte in der Zieldatenbank belassen. Dieses Kontrollkästchen hat keine Auswirkung, wenn das Kontrollkästchen **Datenbank immer neu erstellen** aktiviert ist, da die Datenbank gelöscht wird.|  
|CLR-Typen nicht mit ALTER ASSEMBLY-Anweisungen aktualisieren|Nein|Gibt an, ob CLR (Common Language Runtime)-Typen mit ALTER ASSEMBLY-Anweisungen aktualisiert werden oder ob stattdessen das Objekt, das den CLR-Typ instanziiert, gelöscht und beim Bereitstellen von Änderungen neu erstellt wird.|  
|Erweitert...|Nein|Befehlsschaltfläche, die Ihnen das Angeben von Optionen ermöglicht, die Ereignisse und das Verhalten der Bereitstellung steuern.|  
  
## <a name="reference-paths"></a><a name="bkmk_ref_paths"></a>Verweispfade  
Sie können diese Seite verwenden, um Servervariablen und Datenbankvariablen zu definieren, die einem datenbankübergreifenden Verweis zugeordnet sind. Außerdem können Sie die Werte dieser Variablen angeben. Weitere Informationen finden Sie unter [Verwenden von Verweisen in Datenbankprojekten](/previous-versions/visualstudio/visual-studio-2010/bb386242(v=vs.100)).  
  
## <a name="code-analysis"></a><a name="bkmk_code_analysis"></a>Codeanalyse  
Mithilfe der Codeanalyse können Sie potenzielle Probleme in den Skripts ermitteln, z. B. Entwurfs-, Benennungs- und Leistungsprobleme. Regeln für Datenbankprojekte sind in vordefinierten Regelsätzen organisiert, die auf bestimmte Bereiche abzielen. Sie können die einzelnen Regeln auf der Registerkarte **Codeanalyse** der Eigenschaftenseite **Projekteigenschaften** aktivieren oder deaktivieren. Auf derselben Registerkarte können Sie festlegen, dass die Codeanalyse bei jedem Erstellen eines Projekts automatisch ausgeführt werden soll. Zudem können Sie angeben, ob Warnungen als Fehler behandelt werden sollen.  
  
Um die Codeanalyse manuell auszuführen, klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und klicken Sie auf **Codeanalyse ausführen** aus. Warnungen der Codeanalyse werden im Fenster **Fehlerliste** aufgelistet. Sie können auf eine Warnung doppelklicken, um zum Quellcode zu navigieren, der das betreffende Problem enthält. Außerdem können Sie über das Kontextmenü **Hilfe zu Fehlern anzeigen** weitere Informationen und mögliche Korrekturen für eine Warnung aufrufen. Weitere Informationen zur Codeanalyse finden Sie unter [Analysieren von Datenbankcode zum Verbessern der Codequalität](/previous-versions/visualstudio/visual-studio-2010/dd172133(v=vs.100)).  
