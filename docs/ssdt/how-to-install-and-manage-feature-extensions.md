---
title: Installieren und Verwalten von Funktionserweiterungen
description: In diesem Artikel erfahren Sie, wie Sie Featureerweiterungen installieren, um die Funktionalität von SQL Server Data Tools zu erweitern. Zudem wird erläutert, wo die verschiedenen Erweiterungstypen installiert werden.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 9cdc8cd5-c36f-4bee-a191-87ed457803e7
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 04/26/2017
ms.openlocfilehash: c469770ab64401498b35038f7b20e6f245793804
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100020743"
---
# <a name="how-to-install-and-manage-feature-extensions"></a>Gewusst wie: Installieren und Verwalten von Funktionserweiterungen

Sie können Regeln zum Analysieren von Datenbankcode, Bedingungen für Datenbankkomponententests und Erstellungs-/Bereitstellungs-Contributors hinzufügen, um die Funktionalität von Visual Studio-Editionen, einschließlich SQL Server Data Tools, zu erweitern. Bevor eine Funktionserweiterung verwendet werden kann, muss sie jedoch installiert werden, unabhängig davon, ob die zu installierende Erweiterung von Ihnen oder einer anderen Person erstellt wurde.  
  
Wo Ihre Erweiterung installiert wird, hängt von Erweiterungstyp und dem beabsichtigten Einsatzort ab. In den neuesten Editionen von Visual Studio wurde der Installationsspeicherort einiger Komponenten vom SQL Server-Installationsverzeichnis in das Visual Studio-Verzeichnis verschoben. Dies erleichtert die Koexistenz mehrerer Versionen der Software, bringt jedoch mit sich, dass Sie Ihre Erweiterung möglicherweise an mehreren Speicherorten installieren müssen, wenn Sie sie in verschiedenen Versionen von SQL Server Data Tools sowie in der Befehlszeile verwenden möchten.  
  
### <a name="installing-extensions-for-use-inside-visual-studio"></a>Installieren von Erweiterungen für die interne Verwendung in Visual Studio  
  
|Erweiterungstyp|Installationsspeicherort|  
|------------------|--------------------|  
|Benutzerdefinierte Testbedingung für SQL Server-Komponententests|<Visual Studio Install Dir>\Common7\IDE\Extensions\\Microsoft\SQLDB\TestConditions|  
|Erstellungs-Contributors<br /><br />Bereitstellungs-Contributors<br /><br />Regeln für die statische Codeanalyse|<Visual Studio Install Dir>\Common7\IDE\Extensions\\Microsoft\SQLDB\DAC\120\Extensions|  
  
Das <Visual Studio Install Dir> unterscheidet sich je nach verwendeter Visual Studio-Version und dem gewählten Installationspfad. Bei Visual Studio 2012 befindet es sich in der Regel im Verzeichnis „C:\Programme (x86)\\MicrosoftVisual Studio 11.0“. Bei Visual Studio 2013 befindet es sich in der Regel im Verzeichnis „C:\Programme (x86)\\MicrosoftVisual Studio 12.0“.  
  
Erweiterungen können außerdem als Teil der Befehlszeilendienste ausgeführt werden:  
  
|Erweiterungstyp|Befehlszeilendienst|Installationsordner|  
|------------------|------------------------|------------------|  
|Benutzerdefinierte Testbedingung für SQL Server-Komponententests|MSBuild/MSTest kann zum Ausführen von Komponententests an der Developer-Eingabeaufforderung für Visual Studio 2013 und in ähnlichen Befehlszeilentools verwendet werden.|Gleich wie für die interne Ausführung in Visual Studio.|  
|Erstellungs-Contributors<br /><br />Bereitstellungs-Contributors|[SqlPackage.exe](../tools/sqlpackage/sqlpackage.md) oder durch Verwendung von Deploy- oder Publish-Zielen von MSBuild beim Erstellen eines Datenbankprojekts.|MSBuild: Gleich wie für die interne Ausführung in Visual Studio.<br /><br />[SqlPackage.exe](../tools/sqlpackage/sqlpackage.md): Gleich wie zuvor, sofern im Visual Studio-Verzeichnis gespeichert.<br /><br />Wenn sich „SqlPackage.exe“ und andere DACFx-DLLs außerhalb dieses Verzeichnisses befinden, sollten Erweiterungen entweder im gleichen Verzeichnis oder unter „C:\Programme (x86)\\MicrosoftSQL Server\120\DAC\bin\Extensions“ platziert werden.|  
|Regeln für die statische Codeanalyse|MSBuild kann zum Erstellen des Projekts und zum Ausführen der statischen Codeanalyse verwendet werden.<br /><br />Darüber hinaus können Sie die Codeanalyse mithilfe einer CodeAnalysisService-API aus eigenen Anwendungen heraus ausführen. Die Suchregeln für Erweiterungen funktionieren in diesem Fall genau wie bei der Verwendung von „SqlPackage.exe“.|Das gleiche gilt für Erstellungs- und Bereitstellung-Contributors|  
  
> [!NOTE]  
> Sie müssen über Administratorberechtigungen auf dem Computer verfügen, um auf die Installationsverzeichnisse unter dem Ordner „Programme“ zuzugreifen. Wenn Sie nicht über die entsprechenden Berechtigungen verfügen, wenden Sie sich an Ihren Netzwerkadministrator.  
  
**Sicherheitshinweise**  
  
Vor der Installation einer Erweiterung, die nicht von Ihnen erstellt wurde, sollten Sie sich die folgenden Risiken vergegenwärtigen:  
  
-   Das Installationsprogramm für die Erweiterung könnte Schadsoftware sein, die je nach Ihren Installationsberechtigungen Zugriff auf geschützte Ressourcen erlangen könnte.  
  
-   Auch die Erweiterung selbst kann schädlich sein und Kontrolle über geschützte Ressourcen erlangen, sofern der Benutzer, der die Erweiterung verwendet, über ausreichende Berechtigungen verfügt.  
  
Um das Risiko zu minimieren, sollten Sie nur Erweiterungen aus bekannten Quellen installieren. Falls Sie eine Erweiterung aus einer nicht vertrauenswürdigen Quelle erhalten, sollten Sie den Quellcode der Erweiterung sowie deren Installationsprogramm (falls vorhanden) vor der Installation und Verwendung der Erweiterung überprüfen.  
  
## <a name="to-install-a-custom-feature-extension"></a>So installieren Sie eine benutzerdefinierte Funktionserweiterung  
Kopieren Sie die signierte Assembly (.dll) in den richtigen Installationsordner. Schließen Sie Visual Studio, und öffnen Sie es wieder. Die Erweiterung sollte jetzt verfügbar sein.  
  
## <a name="see-also"></a>Weitere Informationen  
[Erweitern der Datenbankfunktionen](../ssdt/extending-the-database-features.md)  
  
