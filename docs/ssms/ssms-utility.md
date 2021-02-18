---
description: SSMS-Hilfsprogramm
title: SSMS-Hilfsprogramm
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Management Studio [SQL Server], opening
- command prompt utilities [SQL Server], sqlwb
- sqlwb utility
- Management Studio command line
- opening SQL Server Management Studio
ms.assetid: aafda520-9e2a-4e1e-b936-1b165f1684e8
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 07/24/2020
ms.openlocfilehash: 102a07f1b26b8c1e31807227686dd8f1f6856237
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344185"
---
# <a name="ssms-utility"></a>SSMS-Hilfsprogramm

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Mit dem **SSMS**-Hilfsprogramm wird [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] geöffnet. Bei entsprechender Angabe stellt **Ssms** zudem eine Verbindung mit einem Server her und öffnet Abfragen, Skripts, Dateien, Projekte und Lösungen.

Sie können Dateien angeben, die Abfragen, Projekte oder Lösungen enthalten. Für Dateien, die Abfragen enthalten, wird automatisch eine Verbindung mit einem Server hergestellt, wenn Verbindungsinformationen bereitgestellt werden und der Dateityp diesem Servertyp zugeordnet ist. Beispielsweise wird für SQL-Dateien ein SQL-Abfrage-Editorfenster in [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] und für MDX-Dateien ein MDX-Abfrage-Editorfenster in [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] geöffnet. **SQL Server-Lösungen und -Projekte** werden in [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] geöffnet.

> [!NOTE]
> Das Hilfsprogramm **Ssms** führt keine Abfragen aus. Zum Ausführen von Abfragen in der Befehlszeile verwenden Sie das **sqlcmd** -Hilfsprogramm. 

## <a name="syntax"></a>Syntax

```syntaxsql
Ssms
[scriptfile] [projectfile] [solutionfile] 
[-S servername] [-d databasename] [-G] [-U username] [-E] [-nosplash] [-log [filename]?] [-?] 
```

## <a name="arguments"></a>Argumente

*scriptfile* gibt eine oder mehrere zu öffnende Skriptdateien an. Der Parameter muss den vollständigen Pfad zu den Dateien enthalten. 

*projectfile* gibt ein zu öffnendes Skriptprojekt an. Der Parameter muss den vollständigen Pfad zur Skriptprojektdatei enthalten. 

*solutionfile* gibt eine zu öffnende Lösung an. Der Parameter muss den vollständigen Pfad zur Lösungsdatei enthalten. 

[ **-S** _servername_] Servername

[ **-d** _databasename_] Name der Datenbank

[ **-G**] Herstellen einer Verbindung mithilfe von Active Directory-Authentifizierung. Der Verbindungstyp wird angegeben, indem **-U** eingefügt wird.

> [!Note]
> **Active Directory: universell mit MFA-Unterstützung** wird aktuell nicht unterstützt.

[ **-U**_username_] Benutzername für die Verbindung mit der SQL-Authentifizierung

> [!Note]
> **-P** wurde in SSMS, Version 18.0, entfernt.
>
> Problemumgehung: Versuchen Sie, mithilfe der Benutzeroberfläche eine Verbindung mit dem Server herzustellen, und speichern Sie Ihr Kennwort.

[ **-E**] gibt an, dass die Verbindung mithilfe der Windows-Authentifizierung hergestellt werden soll.

[ **-nosplash**] verhindert, dass [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] den Begrüßungsbildschirm beim Öffnen anzeigt. Verwenden Sie diese Option, wenn Sie eine Verbindung zum Computer mit [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] herstellen und hierfür Terminaldienste über eine Verbindung mit begrenzter Bandbreite einsetzen. Bei diesem Argument wird die Groß- und Kleinschreibung nicht beachtet. Es kann vor oder nach anderen Argumenten angegeben werden.

[ **-log** _[filename]?_ ] protokolliert [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]-Aktivitäten für die Problembehandlung in der angegebenen Datei.

[ **-?** ] zeigt die Befehlszeilenhilfe an.

## <a name="remarks"></a>Bemerkungen

Alle Optionen sind optional und werden durch Leerzeichen voneinander getrennt. Dateien stellen hier eine Ausnahme dar, da sie durch Kommas getrennt werden. Wenn Sie keine Schalter angeben, wird **Ssms** Ssms [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] so geöffnet, wie es in den Einstellungen unter **Optionen** im Menü **Extras** angegeben ist. Wenn die Option **Beim Start** der Seite **Umgebung/Allgemein** beispielsweise **Neues Abfragefenster öffnen** festlegt, öffnen **SSMS** ein leeres Abfrage-Editorfenster.

Die Option **-log** muss nach allen anderen Optionen am Ende der Befehlszeile angegeben werden. Das filename-Argument ist optional. Wenn ein Dateiname angegeben wird und die Datei nicht vorhanden ist, wird sie erstellt. Wenn die Datei beispielsweise aufgrund unzureichender Schreibberechtigungen nicht erstellt werden kann, wird die Datei stattdessen in den Ordner für nicht lokalisierte Anwendungsdaten (APPDATA) geschrieben (siehe unten). Wenn das filename-Argument nicht angegeben wird, werden zwei Dateien in den Ordner für nicht lokalisierte Anwendungsdaten des aktuellen Benutzers geschrieben. Der Ordner für nicht lokalisierte Anwendungsdaten in SQL Server kann anhand der APPDATA-Umgebungsvariablen ermittelt werden. Für SQL Server 2012 lautet der Ordner beispielsweise \<system drive>:\Users\\<Benutzername\>\AppData\Roaming\Microsoft\AppEnv\10.0\\. Die beiden Dateien erhalten standardmäßig den Namen ActivityLog.xml und ActivityLog.xsl. Die erste Datei enthält die Aktivitätsprotokolldaten, und die zweite Datei ist ein XML-Stylesheet, mit dem die XML-Datei auf bequeme Weise angezeigt werden kann. Führen Sie folgende Schritte durch, um die Protokolldatei in Ihrem standardmäßig verwendeten XML-Viewer (z. B. Internet Explorer) anzuzeigen: Klicken Sie auf „Start“ und dann auf „Ausführen“. Geben Sie dann „\<system drive>:\Users\\<Benutzername\>\AppData\Roaming\Microsoft\AppEnv\10.0\ActivityLog.xml“ in das angezeigte Feld ein, und drücken Sie die EINGABETASTE.

Dateien, die Abfragen enthalten, fordern eine Verbindung mit einem Server an, wenn Verbindungsinformationen bereitgestellt werden und der Dateityp diesem Servertyp zugeordnet ist. Beispielsweise wird für SQL-Dateien ein SQL-Abfrage-Editorfenster in [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] und für MDX-Dateien ein MDX-Abfrage-Editorfenster in [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] geöffnet. **SQL Server-Lösungen und -Projekte** werden in [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] geöffnet.

In der folgenden Tabelle werden Servertypen zu Dateierweiterungen zugeordnet.

| Servertyp | Durchwahl |
|-------------|-----------|
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]|SQL|
|SQL Server Analysis Services|MDX<br /><br /> XMLA|

## <a name="examples"></a>Beispiele

Mit dem folgenden Skript wird [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] mit den Standardeinstellungen von der Eingabeaufforderung aus geöffnet.

```console
  Ssms
```

Mit den folgenden Skripts wird [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] über eine Eingabeaufforderung mithilfe von *Active Directory: Integriert* geöffnet:

```console
Ssms.exe -S servername.database.windows.net -G
```

Mit dem folgenden Skript wird [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] von der Eingabeaufforderung aus geöffnet. Es wird die Windows-Authentifizierung verwendet, im Code-Editor wird der Server `ACCTG and the database AdventureWorks2012,` ausgewählt, und der Begrüßungsbildschirm wird nicht angezeigt:

```console
Ssms -E -S ACCTG -d AdventureWorks2012 -nosplash
```

Mit dem folgenden Skript wird [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] von der Eingabeaufforderung aus geöffnet. Anschließend wird das Skript MonthEndQuery geöffnet.

```console
Ssms "C:\Documents and Settings\username\My Documents\SQL Server Management Studio Projects\FinanceScripts\FinanceScripts\MonthEndQuery.sql"
```

Mit dem folgenden Skript wird [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] von der Eingabeaufforderung aus geöffnet. Anschließend wird das Projekt NewReportsProject auf dem Computer `developer`geöffnet:

```console
Ssms "\\developer\fin\ReportProj\ReportProj\NewReportProj.ssmssqlproj"
```

Mit dem folgenden Skript wird [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] von der Eingabeaufforderung aus geöffnet. Anschließend wird die Lösung MonthlyReports geöffnet: 

```console
Ssms "C:\solutionsfolder\ReportProj\MonthlyReports.ssmssln"
```

## <a name="see-also"></a>Weitere Informationen

[Verwenden von SQL Server Management Studio](./sql-server-management-studio-ssms.md)