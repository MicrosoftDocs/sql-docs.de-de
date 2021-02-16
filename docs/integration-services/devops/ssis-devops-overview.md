---
title: Übersicht über DevOps für SQL Server Integration Services | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mithilfe von SSIS DevOps-Tools CI/CD in SQL Server Integration Services erstellen.
ms.date: 12/06/2019
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f44c8c3677f76ebc96b6e0385aa839a9e20aa2f8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100030608"
---
# <a name="sql-server-integration-services-ssis-devops-tools-azure-devops-extension"></a>SSIS-DevOps-Tools (SQL Server Integration Services) Azure DevOps-Erweiterung

Die Erweiterung [SSIS DevOps Tools](https://marketplace.visualstudio.com/items?itemName=SSIS.ssis-devops-tools) ist im **Azure DevOps Marketplace** verfügbar.

Wenn Sie noch nicht über eine **Azure DevOps-Organisation** verfügen, registrieren Sie sich zunächst bei [Azure Pipelines](/azure/devops/pipelines/get-started/pipelines-sign-up?view=azure-devops&preserve-view=true). Fügen Sie anschließend gemäß [dieser Anleitung](/azure/devops/marketplace/overview?tabs=browser&view=azure-devops&preserve-view=true#add-an-extension) die Erweiterung **SSIS DevOps-Tools** hinzu.

**SSIS DevOps Tools** umfasst den **SSIS-Buildtask**, den **SSIS-Bereitstellungstask** sowie den **Konfigurationstask für SSIS-Kataloge**.

- Der **[SSIS-Buildtask](#ssis-build-task)** unterstützt das Erstellen von DTPROJ-Dateien in einem Projekt- oder Paketbereitstellungsmodell.

- Der **[SSIS-Bereitstellungstask](#ssis-deploy-task)** unterstützt die Bereitstellung einzelner oder mehrerer ISPAC-Dateien für lokale SSIS-Kataloge und Azure-SSIS IR. Des Weiteren unterstützt er die Bereitstellung von SSISDeploymentManifest-Dateien und der zugehörigen Dateien für lokale oder Azure-Dateifreigaben.

- Der **[Konfigurationstask für SSIS-Kataloge](#ssis-catalog-configuration-task)** unterstützt die Konfiguration der Ordner, Projekte und Umgebungen eines SSIS-Katalogs mithilfe einer Konfigurationsdatei im JSON-Format. Dieser Task unterstützt die folgenden Szenarios:
    - Ordner
        - Sie können Ordner erstellen.
        - Aktualisieren von Ordnerbeschreibungen
    - Project
        - Konfigurieren von Parameterwerten („literal“ und „referenced“) wird unterstützt
        - Hinzufügen von Umgebungsverweisen
    - Environment
        - Erstellen von Umgebungen
        - Aktualisieren von Umgebungsbeschreibungen
        - Erstellen oder Aktualisieren von Umgebungsvariablen

## <a name="ssis-build-task"></a>SSIS-Buildtask

![Buildtask](media/ssis-build-task.png)

### <a name="properties"></a>Eigenschaften

#### <a name="project-path"></a>Projektpfad

Pfad des Projektordners oder der Projektdatei, die erstellt werden sollen. Wird ein Ordnerpfad angegeben, durchsucht der SSIS-Buildtask alle DTPROJ-Dateien in diesem Ordner rekursiv und erstellt alle.

Der Projektpfad darf nicht *leer* sein. Legen Sie ihn als **.** fest, um den Build im Stammordner des Repositorys zu erstellen.

#### <a name="project-configuration"></a>Projektkonfiguration

Name der Projektkonfiguration, die für den Build verwendet werden soll. Wird keine Projektkonfiguration angegeben, wird als Standardeinstellung die erste definierte Projektkonfiguration in jeder DTPROJ-Datei verwendet.

#### <a name="output-path"></a>Ausgabepfad

Pfad eines separaten Ordners zum Speichern von Buildergebnissen, die über den Task [Veröffentlichen von Buildartefakten](/azure/devops/pipelines/tasks/utility/publish-build-artifacts?view=azure-devops&preserve-view=true) als Buildartefakt veröffentlicht werden können.

### <a name="limitations-and-known-issues"></a>Einschränkungen und bekannte Probleme

- Der SSIS-Buildtask verwendet den Visual Studio- und SSIS-Designer, der für Build-Agents obligatorisch ist. Wenn Sie den SSIS-Buildtask also in der Pipeline ausführen möchten, wählen Sie für von Microsoft gehostete Agents **vs2017-win2016** aus. Oder installieren Sie für selbstgehostete Agents den Visual Studio- und SSIS-Designer (entweder VS2017 und SSDT2017 oder VS2019 und SSIS-Projekterweiterungen).

- Wenn Sie SSIS-Projekte mit beliebigen Standardkomponenten (wie etwa SSIS Azure Feature Pack oder andere Drittanbieterkomponenten) erstellen möchten, müssen Sie diese auf dem Computer installieren, auf dem der Pipeline-Agent ausgeführt wird.  Bei von Microsoft gehosteten Agents können Benutzer einen [PowerShell-Skripttask](/azure/devops/pipelines/tasks/utility/powershell?view=azure-devops&preserve-view=true) oder [Befehlszeilen-Skripttask](/azure/devops/pipelines/tasks/utility/command-line?view=azure-devops&preserve-view=true) hinzufügen, um die Komponenten vor Ausführung des SSIS-Buildtasks herunterzuladen und zu installieren. Hier finden Sie das PowerShell-Beispielskript zum Installieren von Azure Feature Pack: 

```powershell
wget -Uri https://download.microsoft.com/download/E/E/0/EE0CB6A0-4105-466D-A7CA-5E39FA9AB128/SsisAzureFeaturePack_2017_x86.msi -OutFile AFP.msi

start -Wait -FilePath msiexec -Args "/i AFP.msi /quiet /l* log.txt"

cat log.txt
```

- Die Schutzebenen **EncryptSensitiveWithPassword** und **EncryptAllWithPassword** werden vom SSIS-Buildtask nicht unterstützt. Stellen Sie sicher, dass diese beiden Schutzebenen von keinem SSIS-Projekt in der Codebasis verwendet werden, da der SSIS-Buildtask ansonsten während der Ausführung nicht mehr reagiert und zu einem Timeout führt.

## <a name="ssis-deploy-task"></a>SSIS-Bereitstellungstask

![Bereitstellungstask](media/ssis-deploy-task.png)

### <a name="properties"></a>Eigenschaften

#### <a name="source-path"></a>Quellpfad

Pfad der ISPAC- oder SSISDeploymentManifest-Quelldateien, die Sie bereitstellen möchten. Bei diesem Pfad kann es sich um einen Ordner- oder Dateipfad handeln.

#### <a name="destination-type"></a>Zieltyp

Typ des Ziels. Zurzeit unterstützt der SSIS-Bereitstellungstask die folgenden beiden Zieltypen:

- *Dateisystem*: Stellen Sie SSISDeploymentManifest-Dateien und die zugehörigen Dateien für ein angegebenes Dateisystem bereit. Es werden sowohl lokale als auch Azure-Dateifreigaben unterstützt.
- *SSISDB*: Stellen Sie ISPAC-Dateien für einen angegebenen SSIS-Katalog bereit, der lokal auf SQL Server oder Azure-SSIS Integration Runtime gehostet werden kann.

#### <a name="destination-server"></a>Zielserver

Name des SQL Server-Zielservers. Dabei kann es sich um den Namen einer lokalen Instanz von SQL Server, Azure SQL-Datenbank oder Azure SQL Managed Instance handeln. Diese Eigenschaft ist nur sichtbar, wenn der Zieltyp „SSISDB“ lautet.

#### <a name="destination-path"></a>Zielpfad

Pfad des Zielordners, in dem die Quelldatei bereitgestellt wird. Beispiel:

- /SSISDB/\<folderName\>
- \\\\\<machineName\>\\\<shareFolderName\>\\\<optionalSubfolderName\>

Mit dem SSIS-Bereitstellungstask wird der Ordner und der Unterordner erstellt, sofern diese noch nicht vorhanden sind.

#### <a name="authentication-type"></a>Authentifizierungsart

Authentifizierungstyp für den Zugriff auf den angegebenen Zielserver. Diese Eigenschaft ist nur sichtbar, wenn der Zieltyp „SSISDB“ lautet. In der Regel werden die folgenden Authentifizierungstypen unterstützt:

- Windows-Authentifizierung
- SQL Server-Authentifizierung
- Active Directory-Kennwortauthentifizierung
- Integrierte Active Directory-Authentifizierung

Ob der jeweilige Authentifizierungstyp wirklich unterstützt wird, hängt jedoch vom Typ des Zielservers und des Agents ab. Die folgende Tabelle enthält eine detaillierte Unterstützungsmatrix:

|Typ des Zielservers|Von Microsoft gehosteter Agent|Selbstgehosteter Agent|
|---------|---------|---------|
|Lokale SQL Server-Instanz oder virtueller Computer |–|Windows-Authentifizierung|
|Azure SQL|SQL Server-Authentifizierung <br> Active Directory-Kennwortauthentifizierung|SQL Server-Authentifizierung <br> Active Directory-Kennwortauthentifizierung <br> Integrierte Active Directory-Authentifizierung|

#### <a name="domain-name"></a>Domänenname

Domänenname für den Zugriff auf das angegebene Dateisystem. Diese Eigenschaft ist nur sichtbar, wenn der Zieltyp „Dateisystem“ lautet.
Verfügt das Benutzerkonto zur Ausführung des selbstgehosteten Agents über Lese-/Schreibzugriff auf den angegebenen Zielpfad, können Sie das Feld leer lassen.

#### <a name="username"></a>Username

Benutzername für den Zugriff auf das angegebene Dateisystem oder SSISDB. Diese Eigenschaft ist sichtbar, wenn der Zieltyp „Dateisystem“ oder der Authentifizierungstyp „SQL Server-Authentifizierung“ oder „Active Directory-Kennwort“ lautet.
Lautet der Zieltyp „Dateisystem“ und verfügt das Benutzerkonto zur Ausführung des selbstgehosteten Agents über Lese-/Schreibzugriff auf den angegebenen Zielpfad, können Sie das Feld leer lassen.

#### <a name="password"></a>Kennwort

Kennwort für den Zugriff auf das angegebene Dateisystem oder SSISDB. Diese Eigenschaft ist sichtbar, wenn der Zieltyp „Dateisystem“ oder der Authentifizierungstyp „SQL Server-Authentifizierung“ oder „Active Directory-Kennwort“ lautet.
Lautet der Zieltyp „Dateisystem“ und verfügt das Benutzerkonto zur Ausführung des selbstgehosteten Agents über Lese-/Schreibzugriff auf den angegebenen Zielpfad, können Sie das Feld leer lassen.

#### <a name="overwrite-existing-projects-or-ssisdeploymentmanifest-files-of-the-same-names"></a>Vorhandene Projekte oder SSISDeploymentManifest-Dateien mit demselben Namen überschreiben

Geben Sie an, ob vorhandene Projekte oder SSISDeploymentManifest-Dateien mit demselben Namen überschrieben werden sollen. Bei „Nein“ überspringt der SSIS-Bereitstellungstask die Bereitstellung dieser Projekte oder Dateien.

#### <a name="continue-deployment-when-error-occurs"></a>Bereitstellung bei Auftreten eines Fehlers fortsetzen

Geben Sie an, ob die Bereitstellung der verbleibenden Projekte oder Dateien fortgesetzt werden soll, wenn ein Fehler auftritt. Bei „Nein“ wird der SSIS-Bereitstellungstask bei einem Fehler sofort beendet.

### <a name="limitations-and-known-issues"></a>Einschränkungen und bekannte Probleme

Folgende Szenarios werden vom SSIS-Bereitstellungstask zurzeit nicht unterstützt:

- Konfigurieren der Umgebung im SSIS-Katalog
- Bereitstellen von ISPAC-Dateien in Azure SQL Server oder einer verwalteten Azure SQL-Instanz – hierbei ist nur eine mehrstufige Authentifizierung (Multi-Factor Authentication, MFA) zulässig
- Bereitstellen von Paketen für MSDB oder SSIS-Paketspeicher

## <a name="ssis-catalog-configuration-task"></a>Konfigurationstask für SSIS-Kataloge

![Katalogkonfigurationstask](media/ssis-catalog-configuation-task.png)

### <a name="properties"></a>Eigenschaften

#### <a name="configuration-file-source"></a>Configuration file source (Quelle der Konfigurationsdatei)

Die Quelle der JSON-Konfigurationsdatei für den SSIS-Katalog. Sie kann inline oder über einen Dateipfad angegeben werden.

Unter folgenden Hyperlinks finden Sie weitere Informationen zum [Angeben der JSON-Konfiguration](#define-configuration-json):

- [Beispiel für eine Inlineeinbettung der JSON-Konfiguration](#a-sample-inline-configuration-json)
- [Beschreibung des JSON-Schemas](#json-schema)

#### <a name="configuration-json-file-path"></a>Configuration JSON file path (Pfad zur JSON-Konfigurationsdatei)

Der Pfad zur JSON-Konfigurationsdatei für den SSIS-Katalog. Diese Eigenschaft ist nur sichtbar, wenn „Dateipfad“ als Quelle der Konfigurationsdatei ausgewählt ist.

Wenn Sie [Pipelinevariablen](/azure/devops/pipelines/process/variables) in der JSON-Konfiurationsdatei verwenden möchten, müssen Sie vor diesem Task einen [Dateitransformationstask](/azure/devops/pipelines/tasks/utility/file-transform?view=azure-devops&preserve-view=true) hinzufügen, um Konfigurationswerte durch Pipelinevariablen zu ersetzen. Weitere Informationen finden Sie unter [JSON-Variablenersetzung](/azure/devops/pipelines/tasks/transforms-variable-substitution?tabs=Classic&view=azure-devops&preserve-view=true#json-variable-substitution).

#### <a name="inline-configuration-json"></a>Inline configuration JSON (Inline eingebettete JSON-Konfiguration)

Inline eingebettete JSON-Konfiguration für den SSIS-Katalog. Diese Eigenschaft ist nur sichtbar, wenn „Inline“ als Quelle der Konfiguration ausgewählt ist. Pipelinevariablen können direkt verwendet werden.

#### <a name="roll-back-configuration-when-error-occurs"></a>Roll back configuration when error occurs (Bei einem Fehler Rollback für die Konfiguration ausführen)

Hiermit legen Sie fest, ob für die von diesem Task vorgenommene Konfiguration bei einem Fehler ein Rollback ausgeführt werden soll.

#### <a name="target-server"></a>Zielserver

Der Name des SQL Server-Zielservers. Dabei kann es sich um den Namen einer lokalen Instanz von SQL Server, Azure SQL-Datenbank oder Azure SQL Managed Instance handeln.

#### <a name="authentication-type"></a>Authentifizierungsart

Der Authentifizierungstyp für den Zugriff auf den angegebenen Zielserver. In der Regel werden die folgenden Authentifizierungstypen unterstützt:

- Windows-Authentifizierung
- SQL Server-Authentifizierung
- Active Directory-Kennwortauthentifizierung
- Integrierte Active Directory-Authentifizierung

Ob der jeweilige Authentifizierungstyp wirklich unterstützt wird, hängt jedoch vom Typ des Zielservers und des Agents ab. Die folgende Tabelle enthält eine detaillierte Unterstützungsmatrix:

|Typ des Zielservers|Von Microsoft gehosteter Agent|Selbstgehosteter Agent|
|---------|---------|---------|
|Lokale SQL Server-Instanz oder virtueller Computer |–|Windows-Authentifizierung|
|Azure SQL|SQL Server-Authentifizierung <br> Active Directory-Kennwortauthentifizierung|SQL Server-Authentifizierung <br> Active Directory-Kennwortauthentifizierung <br> Integrierte Active Directory-Authentifizierung|

#### <a name="username"></a>Username

Der Benutzername für den Zugriff auf den SQL Server-Zielserver. Diese Eigenschaft ist nur sichtbar, wenn der Authentifizierungstyp „SQL Server-Authentifizierung“ oder „Active Directory: Kennwort“ lautet.

#### <a name="password"></a>Kennwort

Das Kennwort für den Zugriff auf den SQL Server-Zielserver. Diese Eigenschaft ist nur sichtbar, wenn der Authentifizierungstyp „SQL Server-Authentifizierung“ oder „Active Directory: Kennwort“ lautet.

### <a name="define-configuration-json"></a>Angeben der JSON-Konfigurationsdatei

Das Schema einer JSON-Konfiguration hat drei Ebenen:

- catalog
- folder
- Projekt und Umgebung

![Katalogkonfigurationsschema](media/catalog-configuration-schema.png)

#### <a name="a-sample-inline-configuration-json"></a>Beispiel für eine Inlineeinbettung der JSON-Konfiguration

```json
{
  "folders": [
    {
      "name": "devopsdemo",
      "description": "devops demo folder",
      "projects": [
        {
          "name": "catalog devops",
          "parameters": [
            {
              "name": "password",
              "container": "Package.dtsx",
              "value": "passwd",
              "valueType": "referenced"
            },
            {
              "name": "serverName",
              "container": "catalog devops",
              "value": "localhost",
              "valueType": "literal"
            }
          ],
          "references": [
            {
              "environmentName": "test",
              "environmentFolder": "devopsdemo"
            },
            {
              "environmentName": "test",
              "environmentFolder": "."
            }
          ]
        }
      ],
      "environments": [
        {
          "name": "test",
          "description": "test",
          "variables": [
            {
              "name": "passwd",
              "type": "string",
              "description": "",
              "value": "$(SSISDBServerAdminPassword)",
              "sensitive": true
            },
            {
              "name": "serverName",
              "type": "string",
              "description": "",
              "value": "$(TargetServerName)",
              "sensitive": false
            }
          ]
        }
      ]
    }
  ]
}
```

#### <a name="json-schema"></a>JSON-Schema

##### <a name="catalog-attributes"></a>Katalogattribute

|Eigenschaft  |Beschreibung  |Notizen  |
|---------|---------|---------|
|Ordner  |Ein Array von Ordnerobjekten. Jedes Objekt enthält Konfigurationsinformationen für einen Katalogordner.|Weitere Informationen zum Schema von Ordnerobjekten finden Sie unter *Ordnerattribute*.|

##### <a name="folder-attributes"></a>Ordnerattribute

|Eigenschaft  |Beschreibung  |Hinweise  |
|---------|---------|---------|
|name  |Der Name des Katalogordners.|Sofern der Ordner noch nicht vorhanden ist, wird er erstellt.|
|description|Eine Beschreibung des Katalogordners.|Der Wert *NULL* wird übersprungen.|
|projects|Ein Array von Projektobjekten. Jedes Objekt enthält Konfigurationsinformationen für ein Projekt.|Weitere Informationen zum Schema von Projektobjekten finden Sie unter *Projektattribute*.|
|environments|Ein Array von Umgebungsobjekten. Jedes Objekt enthält Konfigurationsinformationen für eine Umgebung.|Weitere Informationen zum Schema von Umgebungsattributen finden Sie unter *Umgebungsattribute*.|

##### <a name="project-attributes"></a>Projektattribute

|Eigenschaft  |Beschreibung  |Hinweise  |
|---------|---------|---------|
|name|Der Name des Projekts. |Das Projektobjekt wird übersprungen, wenn das Projekt nicht im übergeordneten Ordner vorhanden ist.|
|parameters|Ein Array von Parameterobjekten. Jedes Objekt enthält Konfigurationsinformationen für einen Parameter.|Weitere Informationen zum Schema von Parameterobjekten finden Sie unter *Parameterattribute*.|
|references|Ein Array von Verweisobjekten. Jedes Objekt stellt einen Umgebungsverweis auf das Zielprojekt dar.|Weitere Informationen zu Verweisobjekten finden Sie unter *Verweisattribute*.|

##### <a name="parameter-attributes"></a>Parameterattribute

|Eigenschaft  |Beschreibung  |Hinweise  |
|---------|---------|---------|
|name|Der Name des Parameters.|<li>Der Parameter kann ein Projektparameter oder ein Paketparameter sein. <li>Wenn er nicht vorhanden ist, wird der Parameter übersprungen. <li>Wenn es sich bei dem Parameter um eine Eigenschaft des Verbindungs-Managers handelt, muss der Name das Format **CM.\<Connection Manager Name>.\<Property Name>** aufweisen. |
|Container|Der Container des Parameters.|<li>Wenn der Parameter ein Projektparameter ist, muss *container* dem Projektnamen entsprechen. <li>Wenn es sich um einen Paketparameter handelt, muss *container* dem Namen des Pakets mit der Erweiterung **.dtsx** entsprechen.|
|value|Wert des Parameters|<li>Wenn *valueType* auf *referenced* festgelegt ist: Der Wert ist ein Verweis auf eine Umgebungsvariable des Typs *String*. <li> Wenn *valueType* auf *literal* festgelegt ist: Dieses Attribut unterstützt alle gültigen JSON-Werte des Typs *Boolean*, *Zahl* und *String*. <li> Der Wert wird in den Typ des Zielparameters konvertiert. Wenn die Konvertierung nicht möglich ist, tritt ein Fehler auf.<li> Der Wert *NULL* ist ungültig. Der Task überspringt dieses Parameterobjekt und gibt eine Warnung aus.|
|valueType|Der Typ des Parameterwerts.|Gültige Typen sind: <br> *literal:* Das Attribut *value* stellt einen Literalwert dar. <br> *referenced:* Das Attribut *value* stellt einen Verweis auf eine Umgebungsvariable dar.|

##### <a name="reference-attributes"></a>Verweisattribute

|Eigenschaft  |Beschreibung  |Notizen  |
|---------|---------|---------|
|environmentFolder|Der Ordnername der Umgebung.|Sofern der Ordner noch nicht vorhanden ist, wird er erstellt. <br> Der Wert kann „ . “ entsprechen. Dies steht für den übergeordneten Ordner des Projekts, der auf die Umgebung verweist.|
|environmentName|Der Name der Umgebung, auf die verwiesen wird.|Sofern sie noch nicht vorhanden ist, wird die Umgebung erstellt.|

##### <a name="environment-attributes"></a>Umgebungsattribute

|Eigenschaft  |Beschreibung  |Hinweise  |
|---------|---------|---------|
|name|Der Name der Umgebung.|Sofern sie noch nicht vorhanden ist, wird die Umgebung erstellt.|
|description|Die Beschreibung der Umgebung.|Der Wert *NULL* wird übersprungen.|
|variables|Ein Array von Variablenobjekten.|Jedes Objekt enthält Konfigurationsinformationen für eine Umgebungsvariable. Weitere Informationen zum Schema eines Variablenobjekten finden Sie unter *Variablenattribute*.|

##### <a name="variable-attributes"></a>Variablenattribute

|Eigenschaft  |Beschreibung  |Hinweise  |
|---------|---------|---------|
|name|Der Name der Umgebungsvariablen.|Sofern sie noch nicht vorhanden ist, wird die Umgebungsvariable erstellt.|
|type|Der Datentyp der Umgebungsvariablen.|Gültige Typen sind: <br> *boolean* <br> *Byte* <br> *datetime* <br> Decimal <br> *double* <br> *int16* <br> *int32* <br> *int64* <br> *sbyte* <br> *single* <br> *string* <br> *uint32* <br> *uint64*|
|description|Die Beschreibung der Umgebungsvariablen.|Der Wert *NULL* wird übersprungen.|
|value|Der Wert der Umgebungsvariablen.|Dieses Attribut unterstützt alle gültigen JSON-Werte des Typs „Boolean“, „Zahl“ und „String“.<br> Der Wert wird in den Typ des vom Attribut **type** angegebenen Werts konvertiert. Wenn die Konvertierung fehlschlägt, tritt ein Fehler auf.<br>Der Wert *NULL* ist ungültig. Der Task überspringt dieses Umgebungsvariablenobjekt und gibt eine Warnung aus.|
|sensitive|Gibt an, ob der Wert der Umgebungsvariablen vertraulich ist.|Gültige Eingaben sind: <br> *true* <br> *false*|

## <a name="release-notes"></a>Versionshinweise

### <a name="version-103"></a>Version 1.0.3

Veröffentlichungsdatum: 21. Oktober 2020

- Hiermit wird das Angeben eines Verbindungszeichenfolgen-Suffix für den SSIS-Bereitstellungstask und den Konfigurationstask für SSIS-Kataloge ermöglicht.

### <a name="version-102"></a>Version 1.0.2

Veröffentlichungsdatum: 26. Mai 2020

- Ein Problem wurde behoben, durch das der Konfigurationstask für SSIS-Kataloge in einigen Fällen nicht erfolgreich durchgeführt wurde, nachdem die Konfiguration abgeschlossen wurde.

### <a name="version-101"></a>Version 1.0.1

Veröffentlichungsdatum: 9. Mai 2020

- Es wurde ein Problem behoben, bei dem der Buildtask von SSIS immer die gesamte Lösung erstellt hat, auch wenn nur eine einzige DTPROJ-Datei als Projektpfad angegeben wurde.

### <a name="version-100"></a>Version 1.0.0

Veröffentlichungsdatum: 8. Mai 2020

- Allgemein verfügbares Release
- Eine Einschränkung für die .NET Framework-Mindestversion auf dem Agent wurde hinzugefügt. Die Mindestversion ist derzeit .NET Framework 4.6.2.
- Die Beschreibung des SSIS-Buildtasks und des SSIS-Bereitstellungstasks wurde optimiert.

### <a name="version-020-preview"></a>Version 0.2.0, Vorschauversion

Veröffentlichungsdatum: 31. März 2020

- Der Konfigurationstask für SSIS-Kataloge wurde hinzugefügt.

### <a name="version-013-preview"></a>Version 0.1.3 (Vorschau)

Veröffentlichungsdatum: 19. Januar 2020

- Ein Problem wurde behoben, durch das die Bereitstellung von „ispac“ verhindert wurde, wenn der ursprüngliche Dateiname geändert wurde.

### <a name="version-012-preview"></a>Version 0.1.2 (Vorschauversion)

Veröffentlichungsdatum: 13. Januar 2020

- Im Protokoll für den SSIS-Bereitstellungstask werden nun detailliertere Informationen zu Ausnahmen angezeigt, wenn das Ziel eine SSIS-Datenbank ist.
- Der fehlerhafte Beispielzielpfad im Hilfetext für die Eigenschaft „Zielpfad“ des SSIS-Bereitstellungstask wurde korrigiert.

### <a name="version-011-preview"></a>Version 0.1.1 (Vorschau)

Veröffentlichungsdatum: 6. Januar 2020

- Es wurde eine Einschränkung der Mindestanforderung für die Agent-Version hinzugefügt. Die minimale Agent-Version für dieses Produkt ist derzeit 2.144.0.
- Ein falscher Anzeigetext für den SSIS-Bereitstellungstask wurde korrigiert.
- Einige Fehlermeldungen wurden präzisiert.

### <a name="version-010-preview"></a>Version 0.1.0 (Vorschau)

Veröffentlichungsdatum: 5. Dezember 2019

Erstrelease von SSIS DevOps-Tools. Dies ist ein Vorschaurelease.

## <a name="next-steps"></a>Nächste Schritte

- Laden Sie die [SSIS DevOps-Erweiterung](https://marketplace.visualstudio.com/items?itemName=SSIS.ssis-devops-tools) herunter.
