---
title: Herunterladen und Installieren von Azure Data Studio
description: Laden Sie Azure Data Studio für Windows, macOS oder Linux herunter, und installieren Sie das Tool. In diesem Artikel finden Sie Veröffentlichungstermine, Versionsnummern, Systemanforderungen und Downloadlinks.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: overview
author: yualan
ms.author: alayu
ms.reviewer: maghan
ms.custom: seodec18
ms.date: 2/10/2021
ms.openlocfilehash: bb7bd4db8a8241e184b41170fe46664955612ec1
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100342511"
---
# <a name="download-and-install-azure-data-studio"></a>Herunterladen und Installieren von Azure Data Studio

Azure Data Studio ist ein plattformübergreifendes Datenbanktool für Datenexperten, das die lokalen und cloudbasierten Datenplattformen unter Windows, macOS und Linux nutzt.

Azure Data Studio bietet eine moderne Editor-Benutzeroberfläche mit IntelliSense, Codeausschnitten und Integration der Quellcodeverwaltung sowie ein integriertes Terminal. Das Tool wurde speziell für die Bedürfnisse der Benutzer von Datenplattformen konzipiert und bietet eine integrierte Diagrammdarstellung von Abfrageresultsets sowie anpassbare Dashboards. Weitere Informationen zu Azure Data Studio finden Sie unter [Was ist Azure Data Studio](what-is-azure-data-studio.md).

## <a name="download-the-latest-release"></a>Herunterladen der neuesten Version

| Plattform | Download | Veröffentlichungsdatum | Version |
|----------|----------|--------------|---------|
| Windows | [Benutzer-Installationsprogramm (empfohlen)](https://go.microsoft.com/fwlink/?linkid=2150927)<br>[System-Installationsprogramm](https://go.microsoft.com/fwlink/?linkid=2150928)<br>[.zip](https://go.microsoft.com/fwlink/?linkid=2151312) | 10. Februar 2021 | 1.25.3 |
| macOS | [.zip](https://go.microsoft.com/fwlink/?linkid=2151311) | 10. Februar 2021 | 1.25.3 |
| Linux | [.deb](https://go.microsoft.com/fwlink/?linkid=2151506)<br>[.rpm](https://go.microsoft.com/fwlink/?linkid=2151407)<br>[.tar.gz](https://go.microsoft.com/fwlink/?linkid=2151508) | 10. Februar 2021 | 1.25.3 |

**Weitere Informationen über die neueste Version finden Sie in den [Versionshinweisen](./release-notes-azure-data-studio.md).**

## <a name="get-azure-data-studio-for-windows"></a>Azure Data Studio für Windows

[!INCLUDE [ssms-ads-install](../includes/ssms-azure-data-studio-install.md)]

Dieses Release von Azure Data Studio enthält den standardmäßigen Windows Installer und eine ZIP-Datei.

Das *Benutzer-Installationsprogramm* wird empfohlen, da dafür keine Administratorrechte erforderlich sind, was Installationen und Upgrades vereinfacht. Für das Benutzerinstallationsprogramm sind keine Administratorrechte erforderlich, da sich der Speicherort im lokalen Ordner „AppData“ (LOCALAPPDATA) des Benutzers befindet. Zudem bietet das Benutzer-Installationsprogramm eine einfachere Aktualisierung im Hintergrund. Weitere Informationen finden Sie unter [Benutzersetup für Windows](https://code.visualstudio.com/updates/v1_26#_user-setup-for-windows).

**Benutzer-Installationsprogramm** (empfohlen)

1. Laden Sie das [Installationsprogramm für *Benutzer* für Windows für Azure Data Studio](https://go.microsoft.com/fwlink/?linkid=2150927) herunter.
2. Starten Sie die Anwendung Azure Data Studio.

**System-Installationsprogramm**

1. Laden Sie das [Installationsprogramm für *Systeme* für Windows für Azure Data Studio](https://go.microsoft.com/fwlink/?linkid=2150928) herunter.
2. Starten Sie die Anwendung Azure Data Studio.

**zip-Datei**

1. Laden Sie die [ZIP-Datei von Azure Data Studio von Windows](https://go.microsoft.com/fwlink/?linkid=2151312) herunter.
2. Navigieren Sie zur heruntergeladenen Datei, und extrahieren Sie sie.
3. Ausführen von `\azuredatastudio-windows\azuredatastudio.exe`

## <a name="get-azure-data-studio-for-macos"></a>Azure Data Studio für macOS

1. Laden Sie [Azure Data Studio für macOS](https://go.microsoft.com/fwlink/?linkid=2151311) herunter.
2. Doppelklicken Sie auf die ZIP-Datei, um die Inhalte zu erweitern.
3. Wenn Sie Azure Data Studio im *Launchpad* zur Verfügung stellen wollen, können Sie *Azure Data Studio.app* in den Ordner *Programme* ziehen.

## <a name="get-azure-data-studio-for-linux"></a>Azure Data Studio für Linux

1. Laden Sie Azure Data Studio für Linux herunter, indem Sie eines der Installationsprogramme oder das Archiv „tar.gz“ verwenden:
    - [.deb](https://go.microsoft.com/fwlink/?linkid=2151506)
    - [.rpm](https://go.microsoft.com/fwlink/?linkid=2151407)
    - [.tar.gz](https://go.microsoft.com/fwlink/?linkid=2151508)
1. Öffnen Sie ein neues Terminalfenster, und geben Sie die folgenden Befehle ein, um die Datei zu extrahieren und Azure Data Studio zu starten:

   **Debian-Installation:**

   ```bash
   cd ~
   sudo dpkg -i ./Downloads/azuredatastudio-linux-<version string>.deb

   azuredatastudio
   ```

   **rpm-Installation:**

   ```bash
   cd ~
   yum install ./Downloads/azuredatastudio-linux-<version string>.rpm

   azuredatastudio
   ```

   **tar.gz-Installation:**

   ```bash
   cd ~
   cp ~/Downloads/azuredatastudio-linux-<version string>.tar.gz ~ 
   tar -xvf ~/azuredatastudio-linux-<version string>.tar.gz 
   echo 'export PATH="$PATH:~/azuredatastudio-linux-x64"' >> ~/.bashrc
   source ~/.bashrc
   azuredatastudio
   ```

   > [!NOTE]
   > Bei Debian, Red hat und Ubuntu fehlen möglicherweise Abhängigkeiten. Verwenden Sie die folgenden Befehle, um diese Abhängigkeiten je nach Ihrer Linux-Version zu installieren:

   **Debian:**

   ```bash
   sudo apt-get install libunwind8
   ```

   **Redhat:**

   ```bash
   yum install libXScrnSaver
   ```

   **Ubuntu:**

   ```bash
   sudo apt-get install libxss1

   sudo apt-get install libgconf-2-4

   sudo apt-get install libunwind8
   ```

## <a name="download-insiders-build-of-azure-data-studio"></a>Herunterladen des Insiders-Builds von Azure Data Studio

Im Allgemeinen sollten Benutzer das stabile Release von Azure Data Studio oben herunterladen. Wenn Sie jedoch die Betafunktionen testen und uns dazu Feedback senden möchten, können Sie den [Insiders-Build von Azure Data Studio](https://github.com/microsoft/azuredatastudio#try-out-the-latest-insiders-build-from-main) herunterladen.

## <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme

Azure Data Studio kann unter Windows, macOS und Linux ausgeführt werden und wird auf den folgenden Plattformen unterstützt:

### <a name="windows"></a>Windows

- Windows 10 (64-Bit)
- Windows 8.1 (64-Bit)
- Windows 8 (64-Bit)
- Windows 7 (SP1)
- Windows Server 2019
- Windows Server 2016
- Windows Server 2012 R2 (64-Bit)
- Windows Server 2012 (64-Bit)
- Windows Server 2008 R2 (64-Bit)

### <a name="macos"></a>macOS

- macOS 10.15 Catalina
- macOS 10.14 Mojave
- macOS 10.13 High Sierra
- macOS 10.12 Sierra
- macOS 11.1 Big Sur

### <a name="linux"></a>Linux

- Red Hat Enterprise Linux 7.4
- Red Hat Enterprise Linux 7.3
- SUSE Linux Enterprise Server v12 SP2
- Ubuntu 16.04

## <a name="recommended-system-requirements"></a>Empfohlene Systemanforderungen

| Empfohlen/Minimum | CPU-Kerne | Speicher/RAM |
|---------------------|-----------|------------|
|     Empfohlen     |     4     |   8 GB     |
|     Minimum         |     2     |   4 GB     |

## <a name="check-for-updates"></a>Suchen nach Updates

Klicken Sie unten links im Fenster auf das Zahnradsymbol und dann auf **Nach Updates suchen**, um nach aktuellen Updates zu suchen.

Updates für Offlineumgebungen können durch [Installieren der neuesten Version](#download-and-install-azure-data-studio) direkt über eine zuvor installierte Version angewendet werden. Es ist nicht notwendig, frühere Versionen von Azure Data Studio zu deinstallieren. Der Installer aktualisiert eine aktuell installierte Anwendung, falls vorhanden.

## <a name="supported-sql-offerings"></a>Unterstützte SQL-Angebote

- Diese Version von Azure Data Studio funktioniert mit allen [unterstützten Versionen von [!INCLUDE [sssql14-md](../includes/sssql14-md.md)] - [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)]](https://support.microsoft.com/lifecycle?C2=1044) und bietet Unterstützung für die Arbeit mit den neuesten Cloudfunktionen in Azure SQL-Datenbank und Azure Synapse Analytics. Azure Data Studio bietet auch Unterstützung für die Vorschau der verwalteten Azure SQL-Datenbank-Instanz.

## <a name="move-user-settings"></a>Übernehmen von Benutzereinstellungen

Wie Sie SQL Operations Studio auf Azure Data Studio aktualisieren und Sie Ihre Einstellungen, Tastenkombinationen und Codeausschnitte beibehalten möchten, befolgen Sie die nachstehenden Schritte.

*Wenn Sie Azure Data Studio bereits nutzen oder SQL Operations Studio nie installiert oder angepasst haben, können Sie diesen Abschnitt ignorieren.*

1. Klicken Sie unten links auf das Zahnradsymbol und dann auf **Settings** (Einstellungen).

   ![Bearbeiten Sie die Einstellungen in Azure Data Studio.](./media/download/open-settings.png)

2. Klicken Sie mit der rechten Maustaste oben auf die Registerkarte **User Settings** (Benutzereinstellungen) und dann auf **Reveal in Explorer** (Im Explorer anzeigen).

   ![Starten Sie den Explorer, mit dem Sie zum lokalen Dateisystem gelangen.](./media/download/reveal-in-explorer.png)

3. Kopieren Sie alle Dateien in diesem Ordner, und speichern Sie sie an einem leicht zugänglichen Ort auf Ihrem lokalen Laufwerk, z. B. im Ordner „Dokumente“.

   ![Verwenden Sie die Dateien, und kopieren Sie sie an Ihren Speicherort.](./media/download/copy-settings.png)

4. Führen Sie in der neuen Version von Azure Data Studio die Schritte 1-2 durch, und fügen Sie für Schritt 3 die gespeicherten Inhalte in den Ordner ein. Sie können die Einstellungen, Tastenzuordnungen und Codeausschnitte auch manuell an ihre jeweiligen Speicherorte kopieren.

5. Löschen Sie beim Außerkraftsetzen einer vorhandenen Installation zunächst das alte Installationsverzeichnis, um zu verhindern, dass im Ressourcen-Explorer beim Herstellen einer Verbindung mit Ihrem Azure-Konto Fehler auftreten.

## <a name="unattended-install-for-windows"></a>Unbeaufsichtigte Installation für Windows

Sie können Azure Data Studio auch mithilfe eines Eingabeaufforderungsskripts installieren.

Wenn Sie Azure Data Studio im Hintergrund ohne GUI-Aufforderungen installieren möchten, und Sie sich auf der Windows-Plattform befinden, führen Sie die folgenden Schritte aus.

1. Starten Sie die Eingabeaufforderung mit erweiterten Berechtigungen.

2. Geben Sie den folgenden Befehl in die Eingabeaufforderung ein.

    ```console
    <path where the azuredatastudio-windows-user-setup-x.xx.x.exe file is located> /VERYSILENT /MERGETASKS=!runcode>
    ```

    Beispiel:

    ```console
    %systemdrive%\azuredatastudio-windows-user-setup-1.24.0.exe /VERYSILENT /MERGETASKS=!runcode
    ```

    > [!Note]
    > Das Beispiel funktioniert auch mit der Systeminstallationsdatei.
    > 
    > ```console
    > <path where the azuredatastudio-windows-setup-x.xx.x.exe file is located> /VERYSILENT /MERGETASKS=!runcode>
    > ```

    Sie können auch */SILENT* anstelle von */SILENT* übergeben, um die Setupbenutzeroberfläche zu sehen.

3. Wenn alle Schritt erfolgreich durchgeführt wurden, können Sie sehen, dass Azure installiert wurde.

## <a name="uninstall-azure-data-studio"></a>Deinstallieren von Azure Data Studio

Wenn Sie Azure Data Studio mit dem Windows Installer installiert haben, deinstallieren Sie das Tool auf die gleiche Weise, wie Sie eine Windows-Anwendung entfernen.

Wenn Sie Azure Data Studio über eine ZIP-Datei oder ein anderes Archiv installiert haben, dann löschen Sie die Dateien.

## <a name="next-steps"></a>Nächste Schritte

In den folgenden Schnellstarts finden Sie Hinweise zu den ersten Schritten:

- [What is Azure Data Studio (Was ist Azure Data Studio?)](what-is-azure-data-studio.md)
- [Azure Data Studio-Versionshinweise](release-notes-azure-data-studio.md)
- [Herstellen einer Verbindung mit und Abfragen von SQL Server](quickstart-sql-server.md)
- [Herstellen einer Verbindung mit und Abfragen von Azure SQL-Datenbank](quickstart-sql-database.md)
- [Herstellen einer Verbindung mit und Abfragen von Azure Synapse Analytics](quickstart-sql-dw.md)

[!INCLUDE[get-help-sql-tools](../includes/paragraph-content/get-help-sql-tools.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]

[Datenschutzerklärung von Microsoft](https://go.microsoft.com/fwlink/?LinkId=521839) und [usage data collection (Erfassung von Nutzungsdaten)](usage-data-collection.md)
