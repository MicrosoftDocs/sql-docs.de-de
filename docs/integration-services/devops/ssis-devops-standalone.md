---
title: Eigenständige SSIS-DevOps-Tools (SQL Server Integration Services) | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mithilfe von eigenständigen SSIS-DevOps-Tools CI/CD in SQL Server Integration Services erstellen.
ms.date: 10/16/2020
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: chugugrace
ms.author: chugu
ms.openlocfilehash: a445e38d796113826f45199cf19b4f59a6e9dc16
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045100"
---
# <a name="standalone-sql-server-integration-service-ssis-devops-tools-preview"></a>Eigenständige SSIS-DevOps-Tools (SQL Server Integration Service) (Vorschauversion)

Eigenständige **SSIS-DevOps-Tools** bieten eine Reihe von ausführbaren Dateien für SSIS-CI/CD-Aufgaben. Ohne Abhängigkeit von einer Visual Studio-Installation oder einer SSIS-Runtime lassen sich diese ausführbaren Dateien leicht in jede CI/CD-Plattform integrieren. Dies sind die bereitgestellten ausführbaren Dateien:

- SSISBuild.exe: Erstellen von SSIS-Projekten in einem Modell zur Projektbereitstellung oder Paketbereitstellung.
- SSISDeploy.exe: Bereitstellen von ISPAC-Dateien in SSIS-Katalogen oder von DTSX-Dateien mit ihren Abhängigkeiten im Dateisystem.

## <a name="installation"></a>Installation

.NET Framework 4.6.2 oder höher ist erforderlich.

Laden Sie das neueste Installationsprogramm aus dem [Download-Center](https://aka.ms/AA9xp65) herunter, und führen Sie dann die Installation über den Assistenten oder die Befehlszeile aus:

- Installieren per Assistent

Doppelklicken Sie auf die zu installierende EXE-Datei, und geben Sie dann einen Ordner an, in dem ausführbaren Dateien und die Abhängigkeiten extrahiert werden sollen.

![Installationspfad](media/installation-location.png)

- Installieren per Befehlszeile

```
SSISDevOpsTools.exe /Q /C /T:<full path>
```

![Installationsbefehlszeile](media/installation-command-line.png)


## <a name="ssisbuildexe"></a>SSISBuild.exe

**Syntax**

```
SSISBuild.exe -project|-p:<dtproj file path> [-configuration|-c:<configuration name>] [-projectPassword|-pp:<project password>] [-stripSensitive|-ss] [-output|-o:<output path>] [-log|-l:<log level>[;<log path>]] [-quiet|-q] [-help|-h|-?]
```

**Parameter**

|Parameter|BESCHREIBUNG|
|---------|---------|
|-project \|-p:\<dtproj file path>|Dateipfad der zu erstellenden DTPROJ-Datei.|
|-configuration\|-c:\<configuration name>|Name der Projektkonfiguration, die für den Build verwendet werden soll. Wird keine Projektkonfiguration angegeben, wird als Standardeinstellung die erste definierte Projektkonfiguration in der DTPROJ-Datei verwendet.|
|-projectPassword\|-pp:\<project password>|Das Kennwort des SSIS-Projekts und seiner Pakete. Dieses Argument ist nur gültig, wenn die Schutzebene des SSIS-Projekts und der Pakete EncryptSensitiveWithPassword oder EncryptAllWithPassword ist. Im Fall des Paketbereitstellungsmodells müssen alle Pakete das gleiche, in diesem Argument angegebene Kennwort verwenden.|
|-stripSensitive\|-ss|Konvertieren Sie die Schutzebene des SSIS-Projekts in DontSaveSensitve. Für die Schutzebene EncryptSensitiveWithPassword oder EncryptAllWithPassword muss das Argument „-projectPassword“ ordnungsgemäß festgelegt sein. Diese Option ist nur für das Projektbereitstellungsmodell gültig.|
|-output\|-o:\<output path>|Der Ausgabepfad des Buildartefakts. Der Wert dieses Arguments überschreibt den Standardausgabepfad in der Projektkonfiguration.|
|-log\|-l:\<log level>[;\<log path>]|Protokollbezogene Einstellungen. <li>Protokolliergrad: Nur Protokolle mit gleichem oder höherem Protokolliergrad werden in die Protokolldatei geschrieben. Es gibt vier Protokolliergrade (von niedrig zu hoch): DIAG, INFO, WRN, ERR. Ohne Angabe ist der Standardprotokolliergrad INFO. <li> Protokollpfad: Der Pfad der Datei, in der Protokolle gespeichert werden sollen. Ohne Pfadangabe wird keine Protokolldatei generiert.|
|-quiet\|-q|In der Standardausgabe werden keine Protokolle angezeigt.|
|-help\|-h\|-?|Ausführliche Syntaxinformationen zu diesem Befehlszeilen-Hilfsprogramm anzeigen.|

**Beispiele**

- Ein DTPROJ mit der ersten definierten Projektdefinition erstellen und nicht mit Kennwort verschlüsseln:    
    ```
    SSISBuild.exe -p:"C:\projects\demo\demo.dtproj"
    ```

- Ein DTPROJ mit der Konfiguration „DevConfiguration“ erstellen, mit einem Kennwort verschlüsseln und die Buildartefakte in einem bestimmten Ordner ausgeben:
    ```
    SSISBuild.exe -p:C:\projects\demo\demo.dtproj -c:DevConfiguration -pp:encryptionpassword -o:D:\folder
    ```

- Ein DTPROJ mit der Konfiguration „DevConfiguration“ erstellen, mit einem Kennwort verschlüsseln, seine vertraulichen Daten verbergen und Protokolliergrad DIAG anwenden:
    ```
    SSISBuild.exe -p:C:\projects\demo\demo.dtproj -c:DevConfiguration -pp:encryptionpassword -ss -l:diag
    ```

## <a name="ssisdeployexe"></a>SSISDeploy.exe

**Syntax**

```
SSISDeploy.exe -source|-s:<source path> -destination|-d:<type>;<path>[;server] [-authType|-at:<auth type name>] [-connectionStringSuffix|-css:<connection string suffix>] [-projectPassword|-pp:<project password>] [-username|-u:<username>] [-password|-p:<password>] [-log|-l:<log level>[;<log path>]] [-quiet|-q] [-help|-h|-?]
```

**Parameter**

|Parameter|BESCHREIBUNG|
|---------|---------|
|-source\|-s:\<source path>|Lokaler Dateipfad der bereitzustellenden Artefakte. ISPAC, DTSX, der Pfad des Ordners für DTSX und SSISDeploymentManifest sind zulässig.|
|-destination\|-d:\<type>;\<path>[;server]|Zieltyp, Pfad des Zielordners und Servername des SSIS-Katalogs, in dem die Quelldatei bereitgestellt werden soll. Aktuell werden die folgenden zwei Zieltypen unterstützt: <li> *CATALOG*: eine oder mehrere ISPAC-Dateien werden im angegebenen SSIS-Katalog bereitgestellt. Der Pfad des CATALOG-Ziels sollte in diesem Format angegeben werden: <br> /SSISDB/\<folder name>[/\<project name>] <br> Der optionale <project name> ist nur gültig, wenn die Quelle einen einzelnen ISPAC-Dateipfad angibt. Als CATALOG-Ziel muss der Servername angegeben werden. <li> *FILE*: SSIS-Pakete oder -Dateien werden in einer einzelnen oder mehreren SSISDeploymentManifest-Dateien unter dem angegebenen Pfad im Dateisystem bereitgestellt. Der Pfad des FILE-Ziels kann der Pfad eines lokalen Ordners oder eines Netzwerkordners in diesem Format sein: <br>\\\\\<machine name>\\\<folder name>[\\\<sub folder name>\...]|
|-authType\|-at:\<auth type name>|Authentifizierungstyp für den Zugriff auf SQL Server Für das CATALOG-Ziel zwingend erforderlich. Folgende Typen werden unterstützt: <li> WIN:  Windows-Authentifizierung <li> SQL:  SQL Server-Authentifizierung <li> ADPWD:  Active Directory-Kennwortauthentifizierung <li> ADINT:  Integrierte Active Directory-Authentifizierung|
|-connectionStringSuffix\|-css:\<connection string suffix> |Suffix der Verbindungszeichenfolge, die zum Herstellen einer Verbindung mit dem SSIS-Katalog verwendet wird.|
|-projectPassword\|-pp:\<project password> |Das Kennwort zum Entschlüsseln der ISPAC-oder DTSX-Dateien.|
|-username\|-u:\<username>  |Benutzername für den Zugriff auf den angegebenen SSIS-Katalog oder das Dateisystem. Für den Dateisystemzugriff ist ein Präfix mit dem Domänennamen zulässig.|
|-password\|-p:\<password>  |Kennwort für den Zugriff auf den angegebenen SSIS-Katalog oder das Dateisystem.|
|-log\|-l:\<log level>[;\<log path>] |Protokolleinstellungen für die Ausführung dieses Hilfsprogramms. <li> Protokolliergrad: Nur Protokolle mit gleichem oder höherem Protokolliergrad werden in die Protokolldatei geschrieben. Es gibt vier Protokolliergrade (von niedrig zu hoch): DIAG, INFO, WRN, ERR. Ohne Angabe ist der Standardprotokolliergrad INFO. <li> Protokollpfad: Der Pfad der Datei, in der Protokolle gespeichert werden sollen. Ohne Pfadangabe wird keine Protokolldatei generiert.|
|-quiet\|-q |In der Standardausgabe werden keine Protokolle angezeigt.|
|-help\|-h\|-?  |Ausführliche Syntaxinformationen zu diesem Befehlszeilen-Hilfsprogramm anzeigen.|

**Beispiele**

- Bereitstellen eines einzelnen ISPAC, das nicht mit einem Kennwort verschlüsselt ist, im SSIS-Katalog mit Windows-Authentifizierung.
    ```
    SSISDeploy.exe -s:D:\myfolder\demo.ispac -d:catalog;/SSISDB/destfolder;myssisserver -at:win
    ```

- Bereitstellen eines einzelnen ISPAC, das mit einem Kennwort verschlüsselt ist, mit SQL-Authentifizierung und Umbenennung des Projekts.
    ```
    SSISDeploy.exe -s:D:\myfolder\test.ispac -d:catalog;/SSISDB/folder/testproj;myssisserver -at:sql -u:sqlusername -p:sqlpassword -pp:encryptionpassword
    ```

- Bereitstellen eines einzelnen SSISDeploymentManifest und der zugehörigen Dateien auf einer Azure-Dateifreigabe.
    ```
    SSISDeploy.exe -s:D:\myfolder\mypackage.SSISDeploymentManifest -d:file;\\myssisshare.file.core.windows.net\destfolder -u:Azure\myssisshare -p:storagekey
    ```

- Bereitstellen eines Ordners mit DTSX-Dateien im lokalen Dateisystem.
    ```
    SSISDeploy.exe -s:D:\myfolder -d:file;\\myssisshare\destfolder
    ```

## <a name="release-notes"></a>Anmerkungen zu diesem Release

### <a name="version-012-preview"></a>Version 0.1.2 (Vorschauversion)

Veröffentlichungsdatum: 14. Januar 2021

- Es wurde ein Problem behoben, bei dem „SSISBuild.exe“ ein Projekt mit einer NullReference-Ausnahme nicht erstellen kann, wenn die Metadaten des Paketparameters in der SSIS-Projektdatei und dem SSIS-Paket nicht übereinstimmen.
- Es wurde ein Problem behoben, bei dem das Paket nicht ausgeführt werden konnte, und die Fehlermeldung „Failed to decrypt protected XML node“ (Fehler beim Entschlüsseln des geschützten XML-Knotens) ausgegeben wurde, obwohl das Paket erfolgreich mit „SSISDeploy.exe“ für SSISDB bereitgestellt werden konnte. Dabei enthält das SSIS-Projekt das Paket, das mit EncryptSensitiveWithUserKey verschlüsselt ist und CM mit sensiblen Daten enthält.

### <a name="version-011-preview"></a>Version 0.1.1 (Vorschau)

Veröffentlichungsdatum: 11. November 2020

- Behoben: „SSISDeploy.exe“ kann bei der Bereitstellung von „ispac“ im SSIS-Katalog keine Assembly laden

### <a name="version-010-preview"></a>Version 0.1.0 (Vorschau)

Veröffentlichungsdatum: 16. Oktober 2020

Erste Vorschauversion der eigenständigen SSIS-DevOps-Tools.

## <a name="next-steps"></a>Nächste Schritte

- Abrufen der [eigenständigen SSIS-DevOps-Tools](https://aka.ms/AA9xp65)
- Suchen Sie bei Fragen die [Q&A](https://marketplace.visualstudio.com/items?itemName=SSIS.ssis-devops-tools&ssr=false#qna)-Website auf.
