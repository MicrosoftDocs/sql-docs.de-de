---
title: Erste Schritte mit Report Viewer-Steuerelementen
description: Die Report Viewer-Steuerelemente können verwendet werden, um RDL-Berichte von Reporting Services in Web- und WinForms-Apps zu integrieren.
ms.custom: seo-lt-2019
ms.date: 09/01/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: application-integration
ms.topic: conceptual
ms.assetid: 01a821c4-2920-400c-be03-93d26c749bb1
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 009c70da7365cc232dc5b00da6b4f1f62bfca8e2
ms.sourcegitcommit: 04fb4c2d7ccddd30745b334b319d9d2dd34325d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/09/2020
ms.locfileid: "89569960"
---
# <a name="integrate-reporting-services-using-the-report-viewer-controls---get-started"></a>Integrieren von Reporting Services mit den ReportViewer-Steuerelementen – Erste Schritte

Die Report Viewer-Steuerelemente können verwendet werden, um RDL-Berichte von Reporting Services in Web- und WinForms-Apps zu integrieren. Detaillierte Informationen zu aktuellen Updates finden Sie im [Änderungsprotokoll](changelog.md).

## <a name="add-the-report-viewer-control-to-a-new-web-project"></a>Hinzufügen des ReportViewer-Steuerelements zu einem neuen Webprojekt

1. Erstellen Sie eine **leere ASP.NET-Website**, oder öffnen Sie ein vorhandenes ASP.NET-Projekt.

    Sie können .NET Framework 4.6 oder eine höhere Version verwenden.

    ![Screenshot: Erstellen einer neuen, leeren ASP.NET-Website](../../reporting-services/application-integration/media/ssrs-create-new-aspnet-project-4-6.png)

2. Installieren Sie das NuGet-Paket für Report Viewer-Steuerelemente über die **Manager-Konsole für NuGet-Pakete**.

    ```
    Install-Package Microsoft.ReportingServices.ReportViewerControl.WebForms
    ```
3. Fügen Sie dem Projekt eine neue ASPX-Seite hinzu, und registrieren Sie die Report Viewer-Steuerelementassembly zur Verwendung auf der Seite.

    ```
    <%@ Register assembly="Microsoft.ReportViewer.WebForms, Version=15.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91" namespace="Microsoft.Reporting.WebForms" tagprefix="rsweb" %>
    ```
    
4. Fügen Sie **ScriptManagerControl** zu der Seite hinzu.

5. Fügen Sie das Report Viewer-Steuerelement zu der Seite hinzu. Der untenstehende Ausschnitt kann aktualisiert werden, um auf einen gehosteten Bericht auf einem Remoteberichtsserver zu verweisen.

    ```
    <rsweb:ReportViewer ID="ReportViewer1" runat="server" ProcessingMode="Remote">
      <ServerReport ReportPath="" ReportServerUrl="" />
    </rsweb:ReportViewer>
    ```
    
Die letzte Seite sollte wie folgt aussehen:

```html
<%@ Page Language="C#" AutoEventWireup="true" CodeBehind="WebForm1.aspx.cs" Inherits="Sample" %>

<%@ Register assembly="Microsoft.ReportViewer.WebForms, Version=15.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91" namespace="Microsoft.Reporting.WebForms" tagprefix="rsweb" %>

<!DOCTYPE html>

<html xmlns="http://www.w3.org/1999/xhtml">
<head runat="server">
    <meta http-equiv="X-UA-Compatible" content="IE=edge" /> 
    <title></title>
</head>
<body>
    <form id="form1" runat="server">
    <asp:ScriptManager runat="server"></asp:ScriptManager>        
        <rsweb:ReportViewer ID="ReportViewer1" runat="server" ProcessingMode="Remote">
            <ServerReport ReportServerUrl="https://AContosoDepartment/ReportServer" ReportPath="/LatestSales" />
        </rsweb:ReportViewer>
    </form>
</body>
</html>
```

## <a name="update-an-existing-project-to-use-the-report-viewer-control"></a>Aktualisieren eines vorhandenen Projekts zur Verwendung des ReportViewer-Steuerelements

Stellen Sie sicher, dass Sie alle Assemblyverweise auf Version *15.0.0.0* aktualisieren, einschließlich der web.config-Datei und sämtlicher ASPX-Seiten, die auf das Anzeigesteuerelement verweisen.

### <a name="sample-webconfig-changes"></a>Beispiel für web.config-Änderungen

```xml
<?xml version="1.0"?>
<!--
  For more information on how to configure your ASP.NET application, please visit
  https://go.microsoft.com/fwlink/?LinkId=169433
  -->
<configuration>
  <system.web>
    <compilation debug="true" targetFramework="4.6">
      <assemblies>
        <!-- All assemblies updated to version 15.0.0.0. -->
        <add assembly="Microsoft.ReportViewer.Common, Version=15.0.0.0, Culture=neutral, PublicKeyToken=89845DCD8080CC91"/>
        <add assembly="Microsoft.ReportViewer.DataVisualization, Version=15.0.0.0, Culture=neutral, PublicKeyToken=89845DCD8080CC91"/>
        <add assembly="Microsoft.ReportViewer.Design, Version=15.0.0.0, Culture=neutral, PublicKeyToken=89845DCD8080CC91"/>
        <add assembly="Microsoft.ReportViewer.ProcessingObjectModel, Version=15.0.0.0, Culture=neutral, PublicKeyToken=89845DCD8080CC91"/>
        <add assembly="Microsoft.ReportViewer.WebDesign, Version=15.0.0.0, Culture=neutral, PublicKeyToken=89845DCD8080CC91"/>
        <add assembly="Microsoft.ReportViewer.WebForms, Version=15.0.0.0, Culture=neutral, PublicKeyToken=89845DCD8080CC91"/>
        <add assembly="Microsoft.ReportViewer.WinForms, Version=15.0.0.0, Culture=neutral, PublicKeyToken=89845DCD8080CC91"/>
      </assemblies>
      <buildProviders>
        <!-- Version updated to 15.0.0.0. -->
        <add extension=".rdlc"
          type="Microsoft.Reporting.RdlBuildProvider, Microsoft.ReportViewer.WebForms, Version=15.0.0.0, Culture=neutral, PublicKeyToken=89845DCD8080CC91"/>
      </buildProviders>
    </compilation>
    <httpRuntime targetFramework="4.6"/>
    <httpHandlers>
      <!-- Version updated to 15.0.0.0 -->
      <add path="Reserved.ReportViewerWebControl.axd" verb="*"
        type="Microsoft.Reporting.WebForms.HttpHandler, Microsoft.ReportViewer.WebForms, Version=15.0.0.0, Culture=neutral, PublicKeyToken=89845DCD8080CC91"
        validate="false"/>
    </httpHandlers>
  </system.web>
  <system.webServer>
    <validation validateIntegratedModeConfiguration="false"/>
    <modules runAllManagedModulesForAllRequests="true"/>
    <handlers>
      <!-- Version updated to 15.0.0.0 -->
      <add name="ReportViewerWebControlHandler" verb="*" path="Reserved.ReportViewerWebControl.axd" preCondition="integratedMode"
        type="Microsoft.Reporting.WebForms.HttpHandler, Microsoft.ReportViewer.WebForms, Version=15.0.0.0, Culture=neutral, PublicKeyToken=89845DCD8080CC91"/>
    </handlers>
  </system.webServer>
</configuration>
```

### <a name="sample-aspx"></a>Beispiel für eine ASPX-Datei

```
<%@ Page Language="C#" AutoEventWireup="true" CodeBehind="WebForm1.aspx.cs" Inherits="SampleAspx" %>

<!-- Update version to 15.0.0.0 -->
<%@ Register assembly="Microsoft.ReportViewer.WebForms, Version=15.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91" namespace="Microsoft.Reporting.WebForms" tagprefix="rsweb" %>

<!DOCTYPE html>
```

## <a name="add-the-report-viewer-control-to-a-new-windows-forms-project"></a>Hinzufügen des Report Viewer-Steuerelements zu einem neuen Windows Forms-Projekt

1. Erstellen Sie eine neue **Windows Forms-Anwendung**, oder öffnen Sie ein vorhandenes Projekt.

    Sie können .NET Framework 4.6 oder eine höhere Version verwenden.
    
    ![Screenshot: Erstellen einer neuen Windows Forms-Anwendung](../../reporting-services/application-integration/media/ssrs-create-new-winforms-project-4-6.png)

2. Installieren Sie das NuGet-Paket für Report Viewer-Steuerelemente über die **Manager-Konsole für NuGet-Pakete**.

    ```
    Install-Package Microsoft.ReportingServices.ReportViewerControl.WinForms
    ```
3. Fügen Sie entweder ein neues Steuerelement aus Code hinzu oder [fügen Sie der Toolbox das Steuerelement hinzu](#add-the-control-to-visual-studio-toolbar).

    ```csharp
    private Microsoft.Reporting.WinForms.ReportViewer reportViewer1;
    
    private void InitializeComponent()
    {
        this.reportViewer1 = new Microsoft.Reporting.WinForms.ReportViewer();
        this.SuspendLayout();
        // 
        // reportViewer1
        // 
        this.reportViewer1.Location = new System.Drawing.Point(168, 132);
        this.reportViewer1.Name = "reportViewer1";
        this.reportViewer1.ServerReport.BearerToken = null;
        this.reportViewer1.Size = new System.Drawing.Size(396, 246);
        this.reportViewer1.TabIndex = 0;
        // 
        // Form1
        // 
        this.Controls.Add(this.reportViewer1);
    }
    ```

## <a name="how-to-set-100-height-on-the-report-viewer-control"></a>Festlegen einer Höhe von 100 % für das Report Viewer-Steuerelement

Wenn Sie die Höhe des Anzeigesteuerelements auf 100 % festlegen, muss das übergeordnete Element über eine definierte Höhe verfügen, oder alle Vorgängerelemente müssen prozentuale Höhenwerte aufweisen.

### <a name="set-the-height-of-all-the-ancestors-to-100"></a>Festlegen der Höhe aller Vorgängerelemente auf 100 %

```html
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
<head runat="server">
    <style>
        html,body,form,#div1 {
            height: 100%; 
        }
    </style>
   </head>
<body>
    <form id="form1" runat="server">
    <div id="div1" >
            <asp:ScriptManager runat="server"></asp:ScriptManager>
        <rsweb:ReportViewer ID="ReportViewer1" runat="server" ProcessingMode="Remote" Height="100%" Width="100%">
            <ServerReport ReportServerUrl="https://test/ReportServer" ReportPath="/testreport" />
        </rsweb:ReportViewer>
    </div>
    </form>
</body>
</html>
```

### <a name="set-the-parents-height-attribute"></a>Festlegen des Höhe-Attributs des übergeordneten Elements

Weitere Informationen zu den Längen der Viewports in Prozent finden Sie unter [Viewport-percentage lengths (Längen der Viewports in Prozent)](http://www.w3.org/TR/css3-values/#viewport-relative-lengths).

```html
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
<head runat="server">
</head>
<body>
    <form id="form1" runat="server">
    <div style="height:100vh;">
            <asp:ScriptManager runat="server"></asp:ScriptManager>
        <rsweb:ReportViewer ID="ReportViewer1" runat="server" ProcessingMode="Remote" Height="100%" Width="100%">
            <ServerReport ReportServerUrl="https://test/ReportServer" ReportPath="/testreport" />
        </rsweb:ReportViewer>
    </div>
    </form>
</body>
</html>
```

## <a name="add-the-control-to-visual-studio-toolbar"></a>Hinzufügen des Steuerelements zur Visual Studio-Symbolleiste

Das Report Viewer-Steuerelement wird jetzt als NuGet-Paket ausgeliefert und wird in der Visual Studio-Toolbox nicht mehr standardmäßig angezeigt. Sie können das Steuerelement manuell zur Toolbox hinzufügen.

1. Installieren Sie wie zuvor erwähnt das NuGet-Paket entweder für WinForms oder für WebForms.

2. Entfernen Sie das Report Viewer-Steuerelement aus der Toolbox.

    ![Screenshot: Löschen des ReportViewer-Steuerelements](../../reporting-services/application-integration/media/ssrs-remove-old-rvcontrol-toolbox.png)

3. Klicken Sie mit der rechten Maustaste auf eine beliebige Stelle in der Toolbox, und klicken Sie anschließend auf **Elemente auswählen...** .

    ![Screenshot: Option „Elemente auswählen“ in der Toolbox](../../reporting-services/application-integration/media/ssrs-toolbox-choose-item.png)
    
4. Klicken Sie unter den **.NET Framework-Komponenten** auf **Durchsuchen**.

    ![Screenshot: Schaltfläche „Durchsuchen“ im Dialogfeld „.NET Framework-Komponenten“](../../reporting-services/application-integration/media/ssrs-toolbox-browse.png)

5. Klicken Sie in dem von Ihnen installierten NuGet-Paket auf **Microsoft.ReportViewer.WinForms.dll** oder **Microsoft.ReportViewer.WebForms.dll**.

    > [!NOTE] 
    > Das NuGet-Paket wird im Projektmappenverzeichnis Ihres Projekts installiert. Der Pfad zur DLL ähnelt einem der folgenden Pfade: `{Solution Directory}\packages\Microsoft.ReportingServices.ReportViewerControl.Winforms.{version}\lib\net40` oder `{Solution Directory}\packages\Microsoft.ReportingServices.ReportViewerControl.WebForms.{version}\lib\net40`.

6. Das neue Steuerelement sollte jetzt in der Toolbox angezeigt werden. Sie können es nun in eine andere Registerkarte in der Toolbox verschieben, wenn Sie möchten.

    ![Screenshot: Neues ReportViewer-Steuerelement in der Toolbox](../../reporting-services/application-integration/media/ssrs-toolbox-rvcontrol.png)

## <a name="common-issues"></a>Häufige Probleme
    
Das Anzeigesteuerelement wurde für moderne Browser entwickelt. Das Steuerelement funktioniert möglicherweise nicht wie erwartet, wenn der Browser die Seite mit dem IE-Kompatibilitätsmodus rendert. Für Intranetsites ist möglicherweise ein Metatag erforderlich, um das standardmäßige Browserverhalten außer Kraft zu setzen.

```html
<meta http-equiv="X-UA-Compatible" content="IE=edge" />
```

## <a name="nugetorg-pages"></a>Seiten auf NuGet.org

Nachstehend finden Sie Links zu Artikeln auf der Website NuGet.org, die Informationen zu den WebForm- und WinForm-Versionen des Report Viewer-Steuerelements enthalten:

- Microsoft.ReportingServices.ReportViewerControl.WebForms [https://www.nuget.org/packages/Microsoft.ReportingServices.ReportViewerControl.WebForms/](https://www.nuget.org/packages/Microsoft.ReportingServices.ReportViewerControl.WebForms/)
- Microsoft.ReportingServices.ReportViewerControl.Winforms [https://www.nuget.org/packages/Microsoft.ReportingServices.ReportViewerControl.WinForms/](https://www.nuget.org/packages/Microsoft.ReportingServices.ReportViewerControl.WinForms/)


## <a name="forum-feedback"></a>Forum-Feedback

Informieren Sie das Team in den [Reporting Services-Foren](https://docs.microsoft.com/answers/topics/sql-server-reporting-services.html) über Probleme.

## <a name="see-also"></a>Weitere Informationen

[Datensammlung im Report Viewer-Steuerelement](../../reporting-services/application-integration/integrating-reporting-services-using-reportviewer-controls-data-collection.md)  

