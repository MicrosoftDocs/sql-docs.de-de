---
title: Konfigurieren der Sammlung von Nutzungs- und Diagnosedaten für SQL Server für Linux
description: Hier wird beschrieben, wie Kundennutzungs- und Diagnosedaten von SQL Server unter Linux gesammelt und konfiguriert werden.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 31f391360636bb27bf346ee76614c409738347e2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347321"
---
# <a name="configure-usage--diagnostic-data-collection-for-sql-server-on-linux"></a>Konfigurieren der Sammlung von Nutzungs- und Diagnosedaten für SQL Server für Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Microsoft SQL Server erfasst standardmäßig Informationen darüber, wie Kunden die Anwendung verwenden. Dies bedeutet, dass SQL Server Informationen zur Installationserfahrung, Nutzung und Leistung sammelt. Mit diesen Informationen kann Microsoft besser an die Bedürfnisse der Kunden anpassen. Microsoft erfasst z.B. Informationen zu Fehlercodes von Kunden, sodass wir damit verknüpfte Probleme beheben, die Dokumentation zu SQL Server verbessern und bestimmen können, ob wir dem Produkt weitere Funktionen hinzufügen müssen, um die Benutzererfahrung zu optimieren.

Dieses Dokument enthält ausführliche Informationen dazu, welche Arten von Informationen gesammelt werden, und wie Microsoft SQL Server für Linux konfiguriert wird, um diese gesammelten Informationen an Microsoft zu senden. SQL Server 2017 enthält eine Datenschutzerklärung, in der erläutert wird, welche Informationen von den Benutzern gesammelt werden. Weitere Informationen finden Sie in den [Datenschutzbestimmungen](../sql-server/sql-server-privacy.md).

Microsoft sendet nicht die folgenden Informationen mit diesem Mechanismus:

- Werte aus Benutzertabellen
- Anmeldeinformationen oder andere Authentifizierungsinformationen
- Personenbezogene Informationen (Personally Identifiable Information, PII)

SQL Server 2017 erfasst und sammelt kontinuierlich Informationen zur Installationserfahrung des Einrichtungsvorgangs, damit wir schnell Installationsprobleme erkennen und beheben können, die der Kunde hat. SQL Server 2017 kann so konfiguriert werden, dass keine Informationen (pro Serverinstanz) mit **mssql-conf** an Microsoft gesendet werden. „mssql-conf“ ist ein Konfigurationsskript, das mit SQL Server 2017 für Red Hat Enterprise Linux, SUSE Linux Enterprise Server und Ubuntu installiert wird.

> [!NOTE]
> Sie können das Senden von Informationen an Microsoft nur in bezahlten Versionen von SQL Server deaktivieren.

## <a name="disable-usage-and-diagnostic-data-collection"></a>Deaktivieren der Nutzungs- und Diagnosedatensammlung

Mit dieser Option können Sie ändern, ob SQL Server die Nutzungs- und Diagnosedatensammlung an Microsoft sendet. Standardmäßig ist dieser Wert auf „true“ festgelegt. Führen Sie die folgenden Befehle aus, um den Wert zu ändern:

> [!IMPORTANT]
> Sie können die Nutzungs- und Diagnosedatensammlung für kostenlose Editionen von SQL Server, Express und Developer nicht deaktivieren.

### <a name="on-red-hat-suse-and-ubuntu"></a>Unter Red Hat, SUSE und Ubuntu

1. Führen Sie das mssql-conf-Skript als Rootbenutzer mit dem Befehl **set** für **telemetry.customerfeedback** aus. Im folgenden Beispiel wird die Nutzungs- und Diagnosedatensammlung durch Angabe von **false** deaktiviert.

   ```bash
   sudo /opt/mssql/bin/mssql-conf set telemetry.customerfeedback false
   ```

1. Starten Sie den SQL Server-Dienst neu:

   ```bash
   sudo systemctl restart mssql-server
   ```
   
### <a name="on-docker"></a>Unter Docker
Zum Deaktivieren der Nutzungs- und Diagnosedatensammlung in Docker muss Docker [Ihre Daten persistent speichern](./sql-server-linux-docker-container-deployment.md). 

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

1. Fügen Sie eine `mssql.conf`-Datei mit den Zeilen `[telemetry]` und `customerfeedback = false` im Hostverzeichnis hinzu:
 
   ```bash
   echo '[telemetry]' >> <host directory>/mssql.conf
   ```

   ```bash
   echo 'customerfeedback = false' >> <host directory>/mssql.conf
   ```

2. Ausführen des Containerimages

   ```bash
   docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -v <host directory>:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2017-latest
   ```

   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v <host directory>:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2017-latest
   ```

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

1. Fügen Sie eine `mssql.conf`-Datei mit den Zeilen `[telemetry]` und `customerfeedback = false` im Hostverzeichnis hinzu:

   ```bash
   echo '[telemetry]' >> <host directory>/mssql.conf
   ```

   ```bash
   echo 'customerfeedback = false' >> <host directory>/mssql.conf
   ```

2. Ausführen des Containerimages

   ```bash
   docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -v <host directory>:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2019-GA-ubuntu-16.04
   ```

   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v <host directory>:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2019-GA-ubuntu-16.04
   ```

::: moniker-end

## <a name="local-audit-for-sql-server-on-linux-usage-and-diagnostic-data-collection"></a>Lokale Überwachung für Nutzungs- und Diagnosedatensammlung in SQL Server für Linux

Microsoft SQL Server 2017 enthält internetfähige Funktionen, die Daten über Ihren Computer oder Ihr Gerät („Standardcomputerinformationen“) erfassen und an Microsoft senden können. Die Komponente „Lokale Überwachung“ der SQL Server-Nutzungs- und Diagnosedatensammlung kann die vom Dienst erfassten Daten, die die an Microsoft zu sendenden Daten (Protokolle) darstellen, in einen bestimmten Ordner schreiben. Der Zweck der lokalen Überwachung besteht darin, dass es Benutzern gestattet wird, alle Daten hinsichtlich Zustimmung, behördlicher Bestimmungen oder aus Datenschutzgründen anzuzeigen, die Microsoft mithilfe dieses Features erfasst.

In SQL Server für Linux kann die lokale Überwachung auf Instanzebene für die SQL Server-Datenbank-Engine konfiguriert werden. Andere SQL Server-Komponenten und SQL Server-Tools besitzen keine Funktion zur lokalen Überwachung für die Nutzungs- und Diagnosedatensammlung.

### <a name="enable-local-audit"></a>Aktivieren der lokalen Überwachung

Mit dieser Option wird die lokale Überwachung aktiviert. Zusätzlich können Sie das Verzeichnis festlegen, in dem die Protokolle für lokale Überwachungen erstellt werden.

1. Erstellen Sie ein Zielverzeichnis für neue Protokolle von lokalen Überwachungen. Im folgenden Beispiel wird das Verzeichnis **/tmp/audit** erstellt:

   ```bash
   sudo mkdir /tmp/audit
   ```

2. Ändern Sie Besitzer und Gruppe des Verzeichnisses in den **mssql**-Benutzer:

   ```bash
   sudo chown mssql /tmp/audit
   sudo chgrp mssql /tmp/audit
   ```

3. Führen Sie das mssql-conf-Skript als Rootbenutzer mit dem **set**-Befehl für **telemetry.userrequestedlocalauditdirectory** aus:

   ```bash
   sudo /opt/mssql/bin/mssql-conf set telemetry.userrequestedlocalauditdirectory /tmp/audit
   ```

4. Starten Sie den SQL Server-Dienst neu:

   ```bash
   sudo systemctl restart mssql-server
   ```
   
### <a name="on-docker"></a>Unter Docker
Um die lokale Überwachung für Docker zu aktivieren, muss Docker [Ihre Daten persistent speichern](./sql-server-linux-docker-container-deployment.md). 

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

1. Das Zielverzeichnis für neue Protokolle der lokalen Überwachung befindet sich im Container. Erstellen Sie ein Zielverzeichnis für neue Protokolle der lokalen Überwachung im Hostverzeichnis auf Ihrem Computer. Im folgenden Beispiel wird ein neues Verzeichnis **/audit** erstellt:

   ```bash
   sudo mkdir <host directory>/audit
   ```

1. Fügen Sie eine `mssql.conf`-Datei mit den Zeilen `[telemetry]` und `userrequestedlocalauditdirectory = <host directory>/audit` im Hostverzeichnis hinzu:
 
   ```bash
   echo '[telemetry]' >> <host directory>/mssql.conf
   ```

   ```bash
   echo 'userrequestedlocalauditdirectory = <host directory>/audit' >> <host directory>/mssql.conf
   ```

1. Ausführen des Containerimages

   ```bash
   docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -v <host directory>:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2017-latest
   ```

   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v <host directory>:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2017-latest
   ```

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

1. Das Zielverzeichnis für neue Protokolle der lokalen Überwachung befindet sich im Container. Erstellen Sie ein Zielverzeichnis für neue Protokolle der lokalen Überwachung im Hostverzeichnis auf Ihrem Computer. Im folgenden Beispiel wird ein neues Verzeichnis **/audit** erstellt:

   ```bash
   sudo mkdir <host directory>/audit
   ```

1. Fügen Sie eine `mssql.conf`-Datei mit den Zeilen `[telemetry]` und `userrequestedlocalauditdirectory = <host directory>/audit` im Hostverzeichnis hinzu:
 
   ```bash
   echo '[telemetry]' >> <host directory>/mssql.conf
   ```

   ```bash
   echo 'userrequestedlocalauditdirectory = <host directory>/audit' >> <host directory>/mssql.conf
   ```

1. Ausführen des Containerimages

   ```bash
   docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -v <host directory>:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2019-GA-ubuntu-16.04
   ```

   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v <host directory>:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2019-GA-ubuntu-16.04
   ```

::: moniker-end

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu SQL Server für Linux finden Sie in der [Übersicht für SQL Server für Linux](sql-server-linux-overview.md).