---
title: Isolationsänderungen für Windows
description: In diesem Artikel werden die Änderungen am Isolationsmechanismus in Machine Learning Services in SQL Server 2019 unter Windows beschrieben. Diese Änderungen wirken sich auf SQLRUserGroup, Firewallregeln, Dateiberechtigungen und die implizite Authentifizierung aus.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 03/05/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-ver15'
ms.openlocfilehash: ace298ff0e69ef3e3ee964c15d894280d6ef1b99
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082387"
---
# <a name="sql-server-2019-on-windows-isolation-changes-for-machine-learning-services"></a>SQL Server 2019 unter Windows: Isolationsänderungen für Machine Learning Services
[!INCLUDE [SQL Server 2019 - Windows only](../../includes/applies-to-version/sqlserver2019-windows-only.md)]

In diesem Artikel werden die Änderungen am Isolationsmechanismus in Machine Learning Services in SQL Server 2019 unter Windows beschrieben. Diese Änderungen wirken sich auf **SQLRUserGroup**, Firewallregeln, Dateiberechtigungen und die implizite Authentifizierung aus.

Weitere Informationen finden Sie unter [Installieren von SQL Server Machine Learning Services unter Windows](sql-machine-learning-services-windows-install.md).

## <a name="changes-to-isolation-mechanism"></a>Änderungen am Isolationsmechanismus

Bei SQL Server 2019-Setup wurde unter Windows der Isolationsmechanismus für externe Prozesse geändert. Im Rahmen dieser Änderung wurden lokale Workerkonten durch [AppContainer](/windows/desktop/secauthz/appcontainer-isolation) ersetzt. Hierbei handelt es sich um eine Isolationstechnologie für Clientanwendungen, die unter Windows ausgeführt werden. 

Aufgrund der Änderung sind für den Administrator keine bestimmten Aktionselemente vorhanden. Auf einem neuen oder aktualisierten Server gehorchen alle externen Skripts und Codes, die über [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) ausgeführt werden, automatisch dem neuen Isolationsmodell. 

Die wichtigsten Änderungen in diesem Release:

+ Lokale Benutzerkonten unter **SQL Restricted User Group (SQLRUserGroup)** (unter SQL eingeschränkte Benutzergruppe) werden nicht mehr erstellt oder zum Ausführen externer Prozesse verwendet. Diese werden durch AppContainer ersetzt.
+ Die Mitgliedschaft in **SQLRUserGroup** wurde geändert. Eine Mitgliedschaft besteht nun nicht mehr aus mehreren lokalen Benutzerkonten, sondern nur noch aus dem SQL Server-Launchpad-Dienstkonto. R- und Python-Prozesse werden nun isoliert durch AppContainer unter der Launchpad-Dienstidentität ausgeführt.

Obwohl sich das Isolationsmodell geändert hat, bleiben Installations-Assistent und Befehlszeilenparameter in SQL Server 2019 unverändert. Weitere Informationen zur Installation finden Sie unter [Installieren von SQL Server Machine Learning Services](sql-machine-learning-services-windows-install.md).

## <a name="about-appcontainer-isolation"></a>Informationen zur AppContainer-Isolation

In früheren Releases enthielt **SQLRUserGroup** einen Pool mit lokalen Windows-Benutzerkonten (MSSQLSERVER00-MSSQLSERVER20), die zum Isolieren und Ausführen externer Prozesse verwendet wurden. Wenn ein externer Prozess ausgeführt werden musste, hat der SQL Server-Launchpad-Dienst dazu ein verfügbares Konto verwendet. 

In SQL Server 2019 werden von Setup keine lokalen Workerkonten mehr erstellt. Stattdessen wird die Isolation durch [AppContainer](/windows/desktop/secauthz/appcontainer-isolation) erreicht. Wenn zur Laufzeit in einer gespeicherten Prozedur oder Abfrage eingebettete Skripts oder Codes erkannt werden, ruft SQL Server Launchpad mit der Anforderung eines erweiterungsspezifischen Startprogramms auf. Launchpad ruft die entsprechende Laufzeitumgebung in einem Prozess unter seiner Identität auf und instanziiert einen AppContainer, um ihn aufzunehmen. Diese Änderung hat den Vorteil, dass keine lokalen Konten und Kennwörter mehr verwaltet werden müssen. Zudem kann dank dem Wegfall der Abhängigkeit von lokalen Benutzerkonten dieses Feature nun in Installationen verwendet werden, bei denen lokale Benutzerkonten nicht zulässig sind.

Gemäß der Implementierung durch SQL Server handelt es sich bei AppContainern um einen internen Mechanismus. Obwohl sich AppContainer im Prozessmonitor physisch nicht nachweisen lassen, sind sie dennoch in Firewallregeln für ausgehenden Datenverkehr zu finden, die beim Setup erstellt werden, um zu verhindern, dass Prozesse Netzwerkaufrufe ausführen.

## <a name="firewall-rules-created-by-setup"></a>Von Setup erstellte Firewallregeln

Standardmäßig werden ausgehende Verbindungen von SQL Server durch Erstellen von Firewallregeln deaktiviert. Bisher haben diese Regeln auf lokalen Benutzerkonten basiert, bei denen im Rahmen des Setups eine Ausgangsregel für **SQLRUserGroup** erstellt wurde, die für die Mitglieder den Netzwerkzugriff verweigert hat (jedes Workerkonto wurde gemäß der Regel als lokales Prinzip aufgeführt). 

Aufgrund des Wechsels zu AppContainer gibt es nun neue Firewallregeln, die auf AppContainer-SIDs basieren: jeweils eine Regel für jede der 20 von SQL Server-Setup erstellten AppContainer. Die Namenskonvention für den Namen der Firewallregel lautet **Netzwerkzugriff für AppContainer-00 in SQL Server-Instanz MSSQLSERVER blockieren**, wobei 00 für die Nummer des AppContainers (standardmäßig 00-20) steht und MSSQLSERVER der Name der SQL Server-Instanz ist. 

> [!Note]
> Wenn Netzwerkaufrufe erforderlich sind, können Sie die Ausgangsregeln in der Windows-Firewall deaktivieren.

<a name="file-permissions"></a>

## <a name="file-permissions"></a>Dateiberechtigungen

Standardmäßig haben externe Python- und R-Skripts nur eine Berechtigung in Form von Lesezugriff auf ihre Arbeitsverzeichnisse. 

Wenn Ihre Python- oder R-Skripts auf andere Verzeichnisse zugreifen müssen, müssen Sie dem Dienstbenutzerkonto **NT Service\MSSQLLaunchpad** und **ALLEN ANWENDUNGSPAKETEN** in diesem Verzeichnis entweder **Lesen und Ausführen**- und/oder **Schreib**-Berechtigungen erteilen.

Befolgen Sie die untenstehenden Schritte, um Zugriff zu gewähren.

1. Klicken Sie im Datei-Explorer mit der rechten Maustaste auf den Ordner, den Sie als Arbeitsverzeichnis verwenden möchten, und klicken Sie auf **Eigenschaften**.
1. Klicken Sie auf **Sicherheit**, und klicken Sie auf **Bearbeiten…** , um die Berechtigungen zu ändern.
1. Klicken Sie auf **Hinzufügen…** .
1. Stellen Sie sicher, dass es sich bei **Suchpfad:** um den Namen des lokalen Computers handelt.
1. Geben Sie **ALLE ANWENDUNGSPAKETE** unter **Geben Sie die Namen der auszuwählenden Objekte ein** ein, und klicken Sie auf **Namen überprüfen**. Klicken Sie auf **OK**.
1. Klicken Sie unter der Spalte **Zulassen** auf die Option **Lesen und Ausführen**.
1. Klicken Sie unter der Spalte **Zulassen** auf die Option **Schreiben**, wenn Sie Leseberechtigungen gewähren möchten.
1. Klicken Sie auf **OK** und dann noch mal auf **OK**.

### <a name="program-file-permissions"></a>Berechtigungen für Programmdateien

Wie bei früheren Releases stellt **SQLRUserGroup** weiterhin die Berechtigungen zum Lesen und Ausführen von ausführbaren Dateien in den SQL Server-Verzeichnissen **Binn**, **R_SERVICES** und **PYTHON_SERVICES** bereit. In diesem Release enthält **SQLRUserGroup** als einziges Mitglied das SQL Server-Launchpad-Dienstkonto.  Wenn durch den Launchpad-Dienst eine R- oder Python-Ausführungsumgebung aufgerufen wird, wird der Prozess als LaunchPad-Dienst ausgeführt.

## <a name="implied-authentication"></a>Implizite Authentifizierung

Für die *implizite Authentifizierung* ist wie bisher eine zusätzliche Konfiguration erforderlich, wenn das Skript oder der Code zum Abrufen von Daten oder Ressourcen erneut eine Verbindung mit SQL Server über eine vertrauenswürdige Authentifizierung herstellen muss. Im Rahmen der zusätzlichen Konfiguration muss für **SQLRUserGroup** eine Datenbankanmeldung erstellt werden. Diese Gruppe enthält nun nicht mehr mehrere Workerkonten, sondern nur noch das SQL Server-Launchpad-Dienstkonto. Weitere Informationen zu diesem Task finden Sie unter [Hinzufügen von SQLRUserGroup als Datenbankbenutzer](../security/create-a-login-for-sqlrusergroup.md).


## <a name="symbolic-link-created-by-setup"></a>Symbolische Verknüpfung erstellt durch Setup

Im Rahmen des SQL Server-Setups wird für die aktuellen Standardverzeichnisse **R_SERVICES** und **PYTHON_SERVICES** eine symbolische Verknüpfung erstellt. Wenn Sie nicht möchten, dass diese Verknüpfung erstellt wird, können Sie „allen Anwendungspaketen“ eine Leseberechtigung für den Ordner in der Hierarchie weiter oben gewähren.


## <a name="see-also"></a>Weitere Informationen

+ [Installieren von SQL Server-Machine Learning Services unter Windows](sql-machine-learning-services-windows-install.md)
+ [Installieren von SQL Server-Machine Learning Services unter Linux](../../linux/sql-server-linux-setup-machine-learning.md)