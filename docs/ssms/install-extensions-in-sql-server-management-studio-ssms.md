---
description: Installieren von Erweiterungen in SQL Server Management Studio (SSMS)
title: Installieren von Erweiterungen in SQL Server Management Studio (SSMS)
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
keywords:
- Erweiterungen
- VSIX
- Installieren der Erweiterung
- Installieren von VSIX
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan
ms.custom: seo-lt-2019
ms.date: 07/29/2020
ms.openlocfilehash: d9fe47b283eb95ed624b68bc9c22abd9258bad15
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344715"
---
# <a name="install-extensions-in-sql-server-management-studio-ssms"></a>Installieren von Erweiterungen in SQL Server Management Studio (SSMS)

[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

SSMS-Erweiterungen (SQL Server Management Studio) werden mit C# durch die Workload „Visual Studio-Erweiterungsentwicklung“ in Visual Studio erstellt. SSMS 18.x basiert auf der Shell von Visual Studio 2017 und unterliegt den Einschränkungen dieser Umgebung.

Eine Erweiterungsinstallation in SSMS 18.x kann durch Bereitstellung von VSIX über Visual Studio oder ein unabhängiges verwaltetes Paketinstallationsprogramm erreicht werden.  Im Folgenden wird die Bereitstellung von Visual Studio beschrieben.

> [!NOTE]
> SQL Server Management Studio-Erweiterungen können unter SSMS 18.x nicht über VSIXInstaller installiert werden.
  
## <a name="visual-studio-deployment-of-an-extension-for-ssms-18x"></a>Visual Studio-Bereitstellung einer Erweiterung für SSMS 18.x

Die manuelle Installation der Erweiterung erfolgt durch Kopieren der zugehörigen Erweiterungsdateien (VSIX) in den Standardordner für SSMS-Erweiterungen.  SSMS überprüft diesen Ordner beim Start automatisch auf Erweiterungen.  Die VSIX-Bereitstellung kann von Visual Studio zur Buildzeit des Projekts abgeschlossen werden. 

  
1.  Suchen Sie Ihren SSMS-Installations- und Standarderweiterungsordner.  Bei den Standardeinstellungen für die SSMS-Installation ist der Speicherort des Ordners ```C:\Program Files (x86)\Microsoft SQL Server Management Studio 18\Common7\IDE\Extensions\```.  


2. Starten Sie Visual Studio als Administrator.

3.  Der Dateikopiervorgang kann von Visual Studio zur Buildzeit abgeschlossen werden, indem Sie das Kontrollkästchen zum Kopieren des VSIX-Inhalts an folgenden Speicherort auf der Registerkarte „VSIX“ im Eigenschaftenfenster des Projekts aktivieren. Geben Sie in das Textfeld unter dem Kontrollkästchen den obigen Speicherort des Ordners ein, wobei ein Ordner für diese Erweiterung angefügt wird.  Beispiel: ```C:\Program Files (x86)\Microsoft SQL Server Management Studio 18\Common7\IDE\Extensions\SampleExtension```
  
![Projekteigenschaftenfenster: VSIX-Einstellungen mit drei Kontrollkästchen und einem Textfeld](./media/install-extensions/vsix_ssms.png)

4. Erstellen Sie das Erweiterungsprojekt. Bei einer erfolgreichen Erstellung werden die Erweiterungsdateien in den SSMS-Erweiterungsordner übertragen.

5.  Starten Sie SSMS, und testen Sie die Funktionalität der Erweiterung.
  
