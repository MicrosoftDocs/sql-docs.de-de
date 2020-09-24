---
title: Herunterladen und Installieren von „Sqlpackage“
description: Herunterladen und Installieren von „sqlpackage“ für Windows, MacOS oder Linux
ms.custom: tools|sos
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
author: pensivebrian
ms.author: broneill
ms.reviewer: alayu; sstein
ms.date: 06/20/2018
ms.openlocfilehash: 40c95546496b6b79aeb95bc63db7750646f833fc
ms.sourcegitcommit: c74bb5944994e34b102615b592fdaabe54713047
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90990143"
---
# <a name="download-and-install-sqlpackage"></a>Herunterladen und Installieren von „Sqlpackage“

„sqlpackage“ wird auf Windows, macOS und Linux ausgeführt.

Laden Sie die neueste.NET Framework-Version und die MacOS- und Linux-Vorschau herunter und installieren Sie sie:

|Plattform|Download|Veröffentlichungsdatum|Version|Entwickeln
|:---|:---|:---|:---|:---|
|Windows|[MSI-Installationsprogramm](https://go.microsoft.com/fwlink/?linkid=2143544)|18. September 2020| 18.6 | 15.0.4897.1 |
|macOS .NET Core |[zip-Datei](https://go.microsoft.com/fwlink/?linkid=2143659)|18. September 2020| 18.6| 15.0.4897.1 |
|Linux .NET Core |[zip-Datei](https://go.microsoft.com/fwlink/?linkid=2143497)|18. September 2020| 18.6| 15.0.4897.1 |
|Windows .NET Core |[zip-Datei](https://go.microsoft.com/fwlink/?linkid=2143496)|18. September 2020| 18.6| 15.0.4897.1 |

Weitere Informationen über die neueste Version finden Sie in den [Versionshinweisen](release-notes-sqlpackage.md). Informationen zum Herunterladen zusätzlicher Sprachen finden Sie im Abschnitt [Verfügbare Sprachen](#available-languages).

[!INCLUDE[Freshness](../includes/paragraph-content/fresh-note-steps-feedback.md)]

## <a name="get-sqlpackage-for-windows"></a>Abrufen von „sqlpackage“ für Windows

Dieses Release von sqlpackage enthält das standardmäßige Windows Installationsprogramm und ein zip-Datei: 

1. Laden Sie das Installationsprogramm [DacFramework.msi für Windows](https://go.microsoft.com/fwlink/?linkid=2143544) herunter, und führen Sie es aus.
2. Öffnen Sie ein Eingabeaufforderungsfenster, und führen Sie die sqlpackage.exe aus.
    - „sqlpackage“ wird im Ordner ```C:\Program Files\Microsoft SQL Server\150\DAC\bin``` installiert.

## <a name="get-sqlpackage-net-core-for-windows"></a>Abrufen von „sqlpackage .NET Core“ für Windows

1. Laden Sie [„sqlpackage“ für Windows](https://go.microsoft.com/fwlink/?linkid=2143496) herunter.
2. Extrahieren Sie die Datei indem Sie mit der rechten Maustaste im Windows Explorer auf die Datei klicken und dann zuerst „Alle extrahieren…“ und dann das Zielverzeichnis auswählen.
3. Öffnen Sie ein neues Terminalfenster, und wechseln Sie mithilfe des Befehls „cd“ zu dem Speicherort, wo sqlpackage extrahiert wurde:

   ```cmd
   > sqlpackage
   ```

## <a name="get-sqlpackage-net-core-for-macos"></a>Abrufen von „sqlpackage .NET Core“ für macOS

1. Laden Sie [„sqlpackage“ für MacOS](https://go.microsoft.com/fwlink/?linkid=2143659) herunter.
2. Um die Datei zu extrahieren und „sqlpackage“ zu starten, öffnen Sie ein neues Terminalfenster und geben Sie die folgenden Befehle ein:

   ```bash
   $ mkdir sqlpackage
   $ unzip ~/Downloads/sqlpackage-osx-<version string>.zip -d ~/sqlpackage 
   $ echo 'export PATH="$PATH:~/sqlpackage"' >> ~/.bash_profile
   $ source ~/.bash_profile
   $ sqlpackage
   ```

   > [!NOTE]
   > Die Sicherheitseinstellungen müssen möglicherweise geändert werden, um „sqlpackage“ unter macOS auszuführen. Verwenden Sie die folgenden Befehle, um über die Befehlszeile mit Gatekeeper in Interaktion zu treten.

   **Vor dem Ausführen von „sqlpackage“:**
   ```bash
   $ sudo spctl --master-disable
   ```

   **Nach dem Ausführen von „sqlpackage“:**
   ```bash
   $ sudo spctl --master-enable
   ```

## <a name="get-sqlpackage-net-core-for-linux"></a>Abrufen von „sqlpackage .NET Core“ für Linux

1. Laden Sie [„sqlpackage“ für Linux](https://go.microsoft.com/fwlink/?linkid=2143497) herunter, indem Sie eines der Installationsprogramme oder das tar.gz-Archiv verwenden:
2. Um die Datei zu extrahieren und „sqlpackage“ zu starten, öffnen Sie ein neues Terminalfenster und geben Sie die folgenden Befehle ein:

   ```bash
   $ cd ~
   $ mkdir sqlpackage
   $ unzip ~/Downloads/sqlpackage-linux-<version string>.zip -d ~/sqlpackage 
   $ echo "export PATH=\"\$PATH:$HOME/sqlpackage\"" >> ~/.bashrc
   $ chmod a+x ~/sqlpackage/sqlpackage
   $ source ~/.bashrc
   $ sqlpackage
   ```

   > [!NOTE]
   > Bei Debian, Red hat und Ubuntu fehlen möglicherweise Abhängigkeiten. Verwenden Sie die folgenden Befehle, um diese Abhängigkeiten je nach Ihrer Linux-Version zu installieren:

   **Debian:**

   ```bash
   $ sudo apt-get install libunwind8
   ```

   **Redhat:**

   ```bash
   $ yum install libunwind
   $ yum install libicu
   ```

   **Ubuntu:**

   ```bash
   $ sudo apt-get install libunwind8

   # install the libicu library based on the Ubuntu version
   $ sudo apt-get install libicu52      # for 14.x
   $ sudo apt-get install libicu55      # for 16.x
   $ sudo apt-get install libicu57      # for 17.x
   $ sudo apt-get install libicu60      # for 18.x
   ```

## <a name="uninstall-sqlpackage"></a>Deinstallieren von sqlpackage

Wenn Sie „sqlpackage“ mit dem Windows-Installationsprogramm installiert haben, deinstallieren Sie es auf die gleiche Weise, wie Sie eine Windows-Anwendung entfernen.

Wenn Sie „sqlpackage“ über eine ZIP-Datei oder ein anderes Archiv installiert haben, dann löschen Sie die Dateien.

## <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme

sqlpackage wird unter Windows, macOS und Linux ausgeführt und wurde mit .NET Core 3.1 erstellt.  Die [Anforderungen für das .NET Core 3.1-Betriebssystem](https://github.com/dotnet/core/blob/master/release-notes/3.1/3.1-supported-os.md) gelten für sqlpackage.

### <a name="windows-x64"></a>Windows (x64)

- Windows 10 (1607 und höher)
- Windows 8.1
- Windows 7 SP1
- Windows Server Core
- Windows Server 2012 R2
- Windows Server 2016
- Windows Server 2019

### <a name="macos"></a>macOS

- macOS 10.15 Catalina
- macOS 10.14 Mojave
- macOS 10.13 High Sierra

### <a name="linux-x64"></a>Linux (x64)

- Red Hat Enterprise Linux 7 und höher
- SUSE Linux Enterprise Server v12 SP2 und höher
- Ubuntu 16.04 und höher

## <a name="available-languages"></a>Verfügbare Sprachen

Diese Version von sqlpackage kann in folgenden Sprachen installiert werden:

sqlpackage-Fenster:  
[Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x40a)

sqlpackage .NET Core Windows:  
[Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x40a)

sqlpackage .NET Core macOS:  
[Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x40a)

sqlpackage .NET Core Linux:  
[Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x40a)

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu [sqlpackage](sqlpackage.md)

[Datenschutzerklärung von Microsoft](https://go.microsoft.com/fwlink/?LinkId=521839)
