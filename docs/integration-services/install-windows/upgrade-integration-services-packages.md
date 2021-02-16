---
description: Aktualisieren von Integration Services-Paketen
title: Durchführen eines Upgrades für Integration Services-Pakete | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- Integration Services, migrating
- migrating packages [Integration Services]
ms.assetid: 68dbdf81-032c-4a73-99f6-41420e053980
author: MikeRayMSFT
ms.author: mikeray
manager: erikre
ms.openlocfilehash: f064880f44b25a96ab5f93e15ee27c361182ba81
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352039"
---
# <a name="upgrade-integration-services-packages"></a>Aktualisieren von Integration Services-Paketen

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Wenn Sie eine Instanz von [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] auf die aktuelle Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]aktualisieren, werden die vorhandenen [!INCLUDE[ssISversion10](../../includes/ssisversion10-md.md)] -Pakete nicht automatisch auf das Paketformat aktualisiert, das von der aktuellen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Version verwendet wird. Sie müssen eine Upgrademethode auswählen und die Pakete manuell aktualisieren.  
  
 Informationen zum Durchführen eines Upgrades für Pakete beim Konvertieren eines Projekts in das Projektbereitstellungsmodell finden Sie unter [Bereitstellen von SQL Server Integration Services-Projekten und Paketen (SSIS)](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md).
  
## <a name="selecting-an-upgrade-method"></a>Auswählen einer Upgrademethode  
 Sie können verschiedene Methoden verwenden, um [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]-, [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]-, [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]- oder [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] -Pakete zu aktualisieren. Bei einigen dieser Methoden wird das Upgrade nur temporär ausgeführt. Bei anderen wird das Upgrade dauerhaft ausgeführt. In der folgenden Tabelle wird jede dieser Methoden beschrieben, und es wird angegeben, ob das Upgrade temporär oder dauerhaft ausgeführt wird.  
  
> [!NOTE]  
>  Wenn Sie ein [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]-, [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]-, [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]- oder [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] -Paket mithilfe des Hilfsprogramms **dtexec** (dtexec.exe) ausführen, das mit der aktuellen Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]installiert wird, verlängert sich die Ausführungszeit für das Upgrade des temporären Pakets. Dabei hängt es von der Paketgröße ab, um welchen Zeitraum sich die Ausführungszeit verlängert. Zur Vermeidung einer längeren Ausführungszeit wird empfohlen, das Paket vor der Ausführung zu aktualisieren.  
  
|Upgrademethode|Typ des Upgrades|  
|--------------------|---------------------|  
|Verwenden Sie das Hilfsprogramm **dtexec** (dtexec.exe), das mit der aktuellen Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installiert wird, um ein [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]-, [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]-, [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]- oder [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] -Paket auszuführen.<br /><br /> Weitere Informationen finden Sie [hier](../../integration-services/packages/dtexec-utility.md).|Das Paketupgrade ist vorübergehend.<br /><br /> Der Änderungen können nicht gespeichert werden.|  
|Öffnen Sie eine [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]-, [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]-, [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]- oder [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] -Paketdatei in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)].|Das Paketupgrade wird dauerhaft ausgeführt, wenn Sie das Paket speichern. Wenn Sie es nicht speichern, wird das Paketupgrade temporär ausgeführt.|  
|Fügen Sie ein [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]-, [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]-, [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]- oder [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] -Paket einem vorhandenen Projekt in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]hinzu.|Das Paketupgrade ist dauerhaft.|  
|Öffnen Sie eine Projektdatei aus [!INCLUDE[ssISversion10](../../includes/ssisversion10-md.md)] oder höher in [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)], und aktualisieren Sie mehrere Pakete im Projekt mit dem [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Paketupgrade-Assistenten.<br /><br /> Weitere Informationen finden Sie unter [Aktualisieren von Integration Services-Paketen mit dem SSIS-Paketupgrade-Assistenten](../../integration-services/install-windows/upgrade-integration-services-packages-using-the-ssis-package-upgrade-wizard.md) und [SSIS Paketupgrade-Assistent (F1-Hilfe)](../../integration-services/ssis-package-upgrade-wizard-f1-help.md).|Das Paketupgrade ist dauerhaft.|  
|Verwenden Sie das Hilfsprogramm <xref:Microsoft.SqlServer.Dts.Runtime.Application.Upgrade%2A> -Methode, um ein oder mehrere [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Pakete zu aktualisieren.|Das Paketupgrade ist dauerhaft.|  
  
## <a name="custom-applications-and-custom-components"></a>Benutzerdefinierte Anwendungen und benutzerdefinierte Komponenten  
 [!INCLUDE[ssISversion2005](../../includes/ssisversion2005-md.md)] -Komponenten können nicht mit der aktuellen Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 Sie können die aktuelle Version der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Tools verwenden, um Pakete auszuführen und zu verwalten, die benutzerdefinierte [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]-, [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]-, [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]- oder [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)][!INCLUDE[ssIS](../../includes/ssis-md.md)] -Komponenten enthalten. Den folgenden Dateien wurden vier Bindungsumleitungsregeln hinzugefügt, um die Umleitung der Laufzeitassemblys von Version 10.0.0.0 ([!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]), Version 11.0.0.0 ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) oder Version 12.0.0.0 ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) an Version 15.0.0.0 ([!INCLUDE[ssSQL19](../../includes/sssql19-md.md)]) zu erleichtern.  
  
-   DTExec.exe.config  
  
-   dtshost.exe.config  
  
-   DTSWizard.exe.config  
  
-   DTUtil.exe.config  
  
-   DTExecUI.exe.config  
  
 Um mit [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] Pakete zu entwerfen, die benutzerdefinierte [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]-, [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]-, [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]- oder [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]-Komponenten enthalten, müssen Sie die Datei „devenv.exe.config“ im Verzeichnis „ *\<drive>* :\Programme\Microsoft Visual Studio 10.0\Common7\IDE“ ändern.  
  
 Zur Verwendung dieser Pakete mit Kundenanwendungen, die mit der Laufzeit für [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)]erstellt werden, schließen Sie die Umleitungsregeln in den Konfigurationsabschnitt der Datei *.exe.config für die ausführbare Datei ein. Die Laufzeitassemblys werden durch die Regeln zu Version 15.0.0.0 ([!INCLUDE[ssSQL19](../../includes/sssql19-md.md)]) umgeleitet. Weitere Informationen zur Umleitung von Assemblyversionen finden Sie unter [\<assemblyBinding>-Element für \<runtime>](/dotnet/framework/configure-apps/file-schema/runtime/assemblybinding-element-for-runtime).  
  
### <a name="locating-the-assemblies"></a>Suchen der Assemblys  
 In [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)]wurden die [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Assemblys auf .NET 4.0 aktualisiert. Es ist ein separater globaler Assemblycache für .NET 4 unter „ *\<drive>* :\Windows\Microsoft.NET\assembly“ verfügbar. Normalerweise befinden sich alle [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Assemblys unter diesem Pfad im Ordner GAC_MSIL.  
  
 Wie in früheren Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] befinden sich die zentralen DLL-Dateien für die [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Erweiterbarkeit unter „ *\<drive>* :\Programme\Microsoft SQL Server\130\SDK\Assemblies“.  
  
## <a name="understanding-sql-server-package-upgrade-results"></a>Grundlegendes zu den Ergebnissen des SQL Server-Paketupgrades  
 Während des Paketupgrades werden die meisten Komponenten und Funktionen in [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]-, [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]-, [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]- oder [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] -Paketen nahtlos in ihre Äquivalente der aktuellen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Version konvertiert. Allerdings gibt es einige Komponenten und Funktionen, die entweder nicht aktualisiert werden oder zu Upgradeergebnissen führen, über die Sie sich im Klaren sein müssen. In der folgenden Tabelle werden diese Komponenten und Funktionen aufgeführt.  
  
> [!NOTE]  
>  Führen Sie Upgrade Advisor aus, um zu ermitteln, in welchen Paketen die in der Tabelle aufgeführten Probleme aufgetreten sind.  
  
|Komponente oder Funktion|Upgradeergebnisse|  
|--------------------------|---------------------|  
|Verbindungszeichenfolgen|Die Namen bestimmter Anbieter für [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]-, [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]-, [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]- oder [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] -Pakete haben sich geändert und erfordern andere Werte in den Verbindungszeichenfolgen. Führen Sie zum Aktualisieren der Verbindungszeichenfolgen einen der folgenden Schritte aus:<br /><br /> Aktualisieren Sie das Paket mit dem [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Paketupgrade-Assistenten, und aktivieren Sie das Kontrollkästchen **Verbindungszeichenfolgen zum Verwenden neuer Anbieternamen aktualisieren** .<br /><br /> Aktivieren Sie in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]im Dialogfeld **Optionen** auf der Seite Allgemein die Option Verbindungszeichenfolgen zum Verwenden neuer Anbieternamen aktualisieren. Weitere Informationen zu dieser Option finden Sie auf der Seite „Allgemein“.<br /><br /> Öffnen Sie das Paket in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], und ändern Sie manuell den Text der ConnectionString-Eigenschaft.<br /><br /> Hinweis: Mithilfe der vorherigen Prozeduren ist es nicht möglich, eine Verbindungszeichenfolge zu aktualisieren, wenn diese entweder in einer Konfigurationsdatei oder einer Datenquellendatei gespeichert wird oder wenn ein Ausdruck die **ConnectionString** -Eigenschaft festlegt. Um die Verbindungszeichenfolge in diesen Fällen zu aktualisieren, müssen Sie die Datei oder den Ausdruck manuell aktualisieren.<br /><br /> Weitere Informationen zu Datenquellen finden Sie unter [Datenquellen](../../integration-services/connection-manager/data-sources.md).|  
  
### <a name="scripts-that-depend-on-adodbdll"></a>Skripts, die von "ADODB.dll" abhängen  
 Skripttask- und Skriptkomponentenskripts, die explizit auf "ADODB.dll" verweisen, können auf Computern, auf denen [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] nicht installiert ist, weder aktualisiert noch ausgeführt werden. Zum Aktualisieren dieser Skripttask- und Skriptkomponentenskripts sollten Sie die Abhängigkeit auf „ADODB.dll“ entfernen.  Ado.Net ist die empfohlene Alternative für verwalteten Code, beispielsweise VB- und C#-Skripts.  
  
