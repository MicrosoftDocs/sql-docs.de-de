---
title: Installieren von SQL Server 2016 R Services
titleSuffix: ''
description: Hier erfahren Sie, wie Sie SQL Server 2016 R Services unter Windows installieren. Sie können R Services verwenden, um R-Skripts in der Datenbank auszuführen.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 08/06/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: contperf-fy20q4
monikerRange: =sql-server-2016
ms.openlocfilehash: 56efa5151e4d72019cb126c69e3d3138829dbbd2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082451"
---
# <a name="install-sql-server-2016-r-services"></a>Installieren von SQL Server 2016 R Services

[!INCLUDE[SQL Server 2016 only](../../includes/applies-to-version/sqlserver2016-only.md)]

Hier erfahren Sie, wie Sie SQL Server 2016 R Services unter Windows installieren. Sie können R Services verwenden, um R-Skripts in der Datenbank auszuführen.

> [!NOTE]
> In SQL Server 2017 und höher ist R in [Machine Learning Services](../sql-server-machine-learning-services.md) zusammen mit Python enthalten. Wenn Sie die R benötigen und über SQL Server 2017 oder höher verfügen, finden Sie Informationen zum Hinzufügen des Features unter [Installieren von SQL Server-Machine Learning Services](sql-machine-learning-services-windows-install.md).

<a name="bkmk_prereqs"></a>

## <a name="pre-install-checklist"></a>Prüfliste vor der Installation

+ Es ist eine Datenbank-Engine-Instanz erforderlich. Sie können nicht nur R installieren, aber Sie können die Sprache inkrementell zu einer vorhandenen Instanz hinzufügen.

+ Für die Aufrechterhaltung der Geschäftskontinuität werden [Always On-Verfügbarkeitsgruppen](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md) für R Services unterstützt. Sie müssen auf jedem Knoten R Services installieren und Pakete konfigurieren.

+ Installieren Sie R Services nicht in Always On-Failoverclusterinstanzen (FCI) für SQL Server. Der Sicherheitsmechanismus, der zum Isolieren von R-Prozessen verwendet wird, ist mit einer Always On-FCI-Umgebung in SQL Server nicht kompatibel.

+ Installieren Sie R Services nicht auf einem Domänencontroller. Das Setup für R Services schlägt in diesem Fall fehl.

+ Installieren Sie **Freigegebene Features** > **R Server (Standalone)** nicht auf einem Computer, auf dem eine datenbankinterne Instanz ausgeführt wird.

+ Eine parallele Installation mit anderen Versionen von R wird zwar unterstützt, jedoch nicht empfohlen. Dies wird unterstützt, da die SQL Server-Instanz eigene Kopien der Open-Source-Distributionen von R verwendet. Das Ausführen von Code, der R auf dem SQL Server-Computer außerhalb von SQL Server verwendet, kann jedoch zu unterschiedlichen Problemen führen:

  + Da Sie eine andere Bibliothek und eine andere ausführbare Datei verwenden, erhalten Sie andere Ergebnisse als bei der Ausführung in SQL Server.
  + R-Skripts, die in externen Bibliotheken ausgeführt werden, können nicht von SQL Server verwaltet werden, was zu Ressourcenkonflikten führt.
  
> [!IMPORTANT]
> Stellen Sie nach Abschluss des Setups sicher, dass Sie die in diesem Artikel beschriebenen zusätzlichen Schritte nach der Konfiguration durchführen. Zu diesen Schritten zählen das Aktivieren von SQL Server für die Verwendung externer Skripts und das Hinzufügen von Konten, die für SQL Server zum Ausführen Ihrer R-Aufträge erforderlich sind. Konfigurationsänderungen erfordern in der Regel einen Neustart der Instanz oder einen Neustart des Launchpad-Diensts.

## <a name="get-the-installation-media"></a>Abrufen der Installationsmedien

[!INCLUDE[GetInstallationMedia](../../includes/getssmedia.md)]

<a name="bkmk_ga_instalpatch"></a>

### <a name="install-patch-requirement"></a>Installieren einer Patchanforderung

Microsoft hat ein Problem bei der speziellen Version von Microsoft VC++ 2013 Runtime-Binärdateien erkannt, die von SQL Server als vorausgesetzte Komponenten installiert werden. Wenn dieses Update an den VC++ Runtime-Binärdateien nicht installiert wird, können bei SQL Server in bestimmten Szenarios Stabilitätsprobleme auftreten. Bevor Sie SQL Server installieren, sollten Sie entsprechend den Anweisungen unter [Versionsanmerkungen zu SQL Server](../../sql-server/sql-server-2016-release-notes.md#bkmk_ga_instalpatch) vorgehen, um festzustellen, ob Ihr Computer einen Patch für die VC-Runtime-Binärdateien benötigt.  

<a name="bkmk2016top"></a>

## <a name="run-setup"></a>Ausführen von Setup

Bei lokalen Installationen müssen Sie das Setup als Administrator ausführen. Wenn Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] von einer Remotefreigabe installieren, müssen Sie ein Domänenkonto verwenden, das Lese- und Ausführungsberechtigungen auf der Remotefreigabe hat.

1. Starten Sie den Setup-Assistenten für SQL Server 2016.

1. Klicken Sie auf der Registerkarte **Installation** auf **Neue eigenständige SQL Server-Installation oder Hinzufügen von Funktionen zu einer vorhandenen Installation**.

    ![Installation von R Services (datenbankintern)](media/2016-setup-installation-rsvcs.png "Einstieg in die Installation einer Datenbank-Engine mit R Services")

1. Wählen Sie folgende Optionen auf der Seite **Funktionsauswahl** aus:

    + Wählen Sie **Datenbank-Engine-Dienste** aus. Die Datenbank-Engine ist für jede Instanz erforderlich, in der Machine Learning verwendet wird.
    + Wählen Sie **R Services (datenbankintern)** aus. Durch diese Option wird die datenbankinterne Verwendung von R unterstützt.

    ![Featureauswahl für R Services](media/2016setup-rsvcs-features.png "Auswahl der Features für R Services (datenbankintern)")

    > [!IMPORTANT]
    > Installieren Sie R Server und R Services nicht gleichzeitig. 

1. Klicken Sie auf der Seite **Zustimmung zur Installation von Microsoft R Open** auf **Annehmen**.
  
    Dieser Lizenzvertrag ist erforderlich, um Microsoft R Open herunterzuladen. Dort ist eine Verteilung der Open-Source-Basispakete und -Tools für R enthalten sowie erweiterte R-Pakete und Konnektivitätsanbieter des Microsoft-R-Entwicklungsteams.
  
1. Nachdem Sie den Lizenzvertrag akzeptiert haben, wird der Installer kurz vorbereitet. Klicken Sie auf **Weiter**, sobald die Schaltfläche verfügbar ist.

1. Stellen Sie auf der Seite **Installationsbereit** sicher, dass die folgenden Elemente ausgewählt sind, und klicken Sie auf **Installieren**.

    + -Datenbank-Engine-Dienste
    + R Services (In-Database)

1. Wenn Sie nach Abschluss des Setups dazu aufgefordert werden, starten Sie jetzt den Computer neu. Wenn Sie den Setupvorgang abgeschlossen haben, sollten Sie unbedingt die vom Installations-Assistenten angezeigte Meldung lesen. Weitere Informationen finden Sie unter [View and Read SQL Server Setup Log Files](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).

## <a name="set-environment-variables"></a>Festlegen von Umgebungsvariablen

Wenn Sie nur das R-Feature integrieren möchten, sollten Sie die Umgebungsvariable **MKL_CBWR** über die Intel Math Kernel Library-Berechnungen auf [ensure consistent output](https://software.intel.com/articles/introduction-to-the-conditional-numerical-reproducibility-cnr) (Konsistente Ausgabe sicherstellen) festlegen.

1. Klicken Sie in der Systemsteuerung auf **System und Sicherheit** > **System** > **Erweiterte Systemeinstellungen** > **Umgebungsvariablen**.

1. Erstellen Sie eine neue Benutzer- oder Systemvariable.

    + Legen Sie den Variablenname auf `MKL_CBWR` fest.
    + Legen Sie den Variablenwert auf `AUTO` fest.

Für diesen Schritt ist ein Neustart des Servers erforderlich. Sie können den Neustart anhalten, bis die gesamte Konfiguration abgeschlossen ist.

<a name="bkmk_enableFeature"></a>

##  <a name="enable-script-execution"></a>Aktivieren der Skriptausführung

1. Öffnen Sie das [SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md) oder [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md).

1. Stellen Sie eine Verbindung mit der Instanz her, auf der Sie R Services installiert haben. Klicken Sie auf **Neue Abfrage**, um ein Abfragefenster zu öffnen, und führen Sie den folgenden Befehl aus:

   ```sql
   sp_configure
   ```

    Der Wert für die Eigenschaft `external scripts enabled` sollte an diesem Punkt **0** betragen. Dies liegt daran, dass die Funktion standardmäßig deaktiviert ist. Das Feature muss explizit von einem Administrator aktiviert werden, bevor Sie R-Skripts ausführen können.

1. Führen Sie die folgende Anweisung aus, um die externe Funktion für die Skripterstellung zu aktivieren:
  
    ```sql
    EXEC sp_configure  'external scripts enabled', 1
    RECONFIGURE WITH OVERRIDE
    ```
  
## <a name="restart-the-service"></a>Starten Sie den Dienst neu.

Starten Sie nach Abschluss der Installation die Datenbank-Engine neu, bevor Sie mit dem nächsten Schritt fortfahren, und aktivieren Sie die Skriptausführung.

Durch den Neustart des Diensts wird auch der zugehörige [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)]-Dienst automatisch neu gestartet.

Sie können den Dienst neu starten, indem Sie für die Instanz in SSMS mit der rechten Maustaste auf den Befehl **Neu starten** klicken oder den [SQL Server-Konfigurations-Manager](../../relational-databases/sql-server-configuration-manager.md) verwenden.

## <a name="verify-installation"></a>Überprüfen der Installation

Gehen Sie folgendermaßen vor, um zu überprüfen, ob alle zum Starten eines externen Skripts verwendeten Komponenten ausgeführt werden.

1. Öffnen Sie in SQL Server Management Studio ein neues Abfragefenster, und führen Sie den folgenden Befehl aus:

    ```sql
    EXEC sp_configure 'external scripts enabled'
    ```

    Der Wert **Run_value** sollte jetzt auf 1 festgelegt werden.

1. Öffnen Sie den SQL Server-Konfigurations-Manager, und überprüfen Sie, ob der **SQL Server-Launchpad-Dienst** ausgeführt wird. Sie sollten für jede Datenbank-Engine-Instanz über einen Dienst verfügen, auf der R installiert ist. Weitere Informationen zu dem Dienst finden Sie unter [Erweiterbarkeitsframework](../concepts/extensibility-framework.md).

1. Wenn das Launchpad ausgeführt wird, sollten Sie einfache R-Skripts ausführen können, um zu überprüfen, ob externe Skriptruntimes mit SQL Server kommunizieren können.

    Öffnen Sie ein neues **Abfragefenster** in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder Azure Data Studio, und führen Sie ein Skript wie das folgende aus:

    ```sql
    EXEC sp_execute_external_script  @language =N'R',
    @script=N'
    OutputDataSet <- InputDataSet;
    ',
    @input_data_1 =N'SELECT 1 AS hello'
    WITH RESULT SETS (([hello] int not null));
    GO
    ```

    Die Ausführung des Skripts kann einige Zeit in Anspruch nehmen, wenn die externe Skriptruntime zum ersten Mal geladen wird. Die Ergebnisse sollten etwa wie folgt aussehen:

    | Hello |
    |----|
    | 1|

<a name="apply-cu"></a>

## <a name="apply-updates"></a>Anwenden von Updates

Es wird empfohlen, dass Sie das neueste Service Pack und kumulative Update sowohl auf die Datenbank-Engine als auch auf die Machine Learning-Komponenten anwenden.

Auf Geräten, die mit dem Internet verbunden sind, werden kumulative Updates in der Regel über Windows Update angewendet. Sie können jedoch auch die nachfolgenden Schritte für kontrollierte Updates verwenden. Wenn Sie das Update für die Datenbank-Engine anwenden, ruft das Setup die kumulativen Updates für alle R-Bibliotheken ab, die Sie auf derselben Instanz installiert haben. 

Auf getrennten Servern sind zusätzliche Schritte erforderlich. Weitere Informationen finden Sie unter [Installieren auf Computern ohne Internetzugriff > Kumulative Updates anwenden](sql-ml-component-install-without-internet-access.md#apply-cu).

1. Beginnen Sie mit einer bereits installierten Baseline-Instanz: erstes Release von SQL Server 2016, SQL Server 2016 SP1 oder SQL Server 2016 SP2

1. Navigieren Sie zur kumulativen Updateliste: [Neueste Updates für Microsoft SQL Server](../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md)

1. Wählen Sie (von den nicht bereits als Baselineinstanz installierten Versionen) das neueste Service Pack und das kumulative Update aus. Eine ausführbare Datei wird automatisch heruntergeladen und extrahiert.

1. Führen Sie das Setup aus. Akzeptieren Sie die Lizenzbedingungen, und überprüfen Sie auf der Seite für die Funktionsauswahl die Funktionen, für die kumulative Updates angewendet werden. Es sollte jedes für die aktuelle Instanz installierte Feature einschließlich R Services angezeigt werden. Das Setup lädt die CAB-Dateien herunter, die zum Aktualisieren aller Funktionen erforderlich sind.

1. Folgen Sie den weiteren Anweisungen des Assistenten, und akzeptieren Sie die Lizenzbedingungen für die R-Verteilung.

> [!NOTE]
> Das kumulative Update (CU) 14 und höher für SQL Server 2016 SP2 enthält eine neuere Version der R-Runtime. Weitere Informationen finden Sie unter [Ändern der Language Runtime-Standardversion](change-default-language-runtime-version.md).

<a name="bkmk_FollowUp"></a>

## <a name="additional-configuration"></a>Zusätzliche Konfiguration

Wenn der Schritt zur externen Skriptüberprüfung erfolgreich war, können Sie R-Befehle von SQL Server Management Studio, Azure Data Studio oder einem anderen Client ausführen, der T-SQL-Anweisungen an den Server senden kann.

Überprüfen Sie die zusätzlichen Konfigurationsschritte in diesem Abschnitt, wenn beim Ausführen des Befehls ein Fehler aufgetreten ist. Möglicherweise müssen Sie für den Dienst oder die Datenbank zusätzliche geeignete Konfigurationen vornehmen.

Auf Instanzebene kann eine zusätzliche Konfiguration Folgendes umfassen:

* [Firewallkonfiguration für SQL Server- Machine Learning Services](../../machine-learning/security/firewall-configuration.md)
* [Aktivieren oder Deaktivieren eines Servernetzwerkprotokolls](../../database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md)
* [Konfigurieren der Serverkonfigurationsoption Remotezugriff](../../database-engine/configure-windows/configure-the-remote-access-server-configuration-option.md)
* [Verwalten von Datenträgerkontingenten](/windows/desktop/fileio/managing-disk-quotas), um das Ausführen externer Skripts zu vermeiden, die Speicherplatz verbrauchen

<a name="bkmk_configureAccounts"></a>
<a name="bkmk_AllowLogon"></a>

Für die Datenbank benötigen Sie möglicherweise die folgenden Konfigurationsupdates:

* [Benutzern die Berechtigung für SQL Server-Machine Learning Services erteilen](../../machine-learning/security/user-permission.md)
* [Hinzufügen der SQLRUserGroup als Datenbankbenutzer](../../machine-learning/security/create-a-login-for-sqlrusergroup.md)

> [!NOTE]
> Nicht alle aufgeführten Änderungen sind erforderlich. Es ist möglich, dass keine davon erforderlich ist. Die Anforderungen hängen vom Sicherheitsschema ab, in dem Sie SQL Server installiert haben, sowie von Ihren Erwartungen bezüglich des Herstellens einer Verbindung mit der Datenbank und dem Ausführen externer Skripts seitens der Benutzer. Weitere Hinweise zur Installation finden Sie hier: [Installieren von SQL Server-Machine Learning Services](../install/sql-machine-learning-services-windows-install.md)

## <a name="suggested-optimizations"></a>Empfohlene Optimierungen

Sie sollten auch die Server optimieren, um maschinelles Lernen mit R oder das Installieren vorab trainierter Modelle zu unterstützen.

### <a name="add-more-worker-accounts"></a>Hinzufügen weiterer Geschäftskonten

Wenn Sie wissen, dass Sie R intensiv verwenden oder dass viele Benutzer gleichzeitig Skripts ausführen werden, können Sie die Anzahl der Workerkonten erhöhen, die dem Launchpad-Dienst zugewiesen sind. Weitere Informationen finden Sie unter [Skalieren der gleichzeitige Ausführung externer Skripts in SQL Server-Machine Learning Services](../administration/scale-concurrent-execution-external-scripts.md).

<a name="bkmk_optimize"></a>

### <a name="optimize-the-server-for-external-script-execution"></a>Optimieren des Servers für die Ausführung externer Skripts

Die Standardeinstellungen für das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Setup dienen zur Optimierung des Lastenausgleichs des Servers für eine Vielzahl von Diensten, die von der Datenbank-Engine unterstützt werden, einschließlich ETL-Prozesse (Extrahieren, Transformieren und Laden), Reporting, Überwachung und Anwendungen, die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Daten verwenden. Daher können in den Standardeinstellungen insbesondere für speicherintensive Vorgänge Ressourcen für Machine Learning-Vorgänge eingeschränkt oder gedrosselt sein.

Es wird empfohlen, dass Sie zum Konfigurieren eines externen Ressourcenpools den SQL Server-Resource Governor verwenden, um sicherzustellen, dass Machine Learning-Aufgaben über die entsprechende Priorität und die nötigen Ressourcen verfügen. Eventuell ist es auch sinnvoll, die Größe des Speichers zu ändern, der der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbank-Engine zugewiesen ist, oder die Anzahl der Konten zu erhöhen, die unter dem [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)]-Dienst ausgeführt werden.

- Informationen zum Konfigurieren eines Ressourcenpools zum Verwalten externer Ressourcen finden Sie unter [Erstellen eines externen Ressourcenpools](../../t-sql/statements/create-external-resource-pool-transact-sql.md).
  
- Informationen zum Ändern des für die Datenbank reservierten Arbeitsspeichers finden Sie unter [Konfigurationsoptionen für den Serverarbeitsspeicher](../../database-engine/configure-windows/server-memory-server-configuration-options.md).
  
- Informationen zum Ändern der Anzahl von R-Konten, die durch [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)] gestartet werden können, finden Sie unter [Skalieren der gleichzeitigen Ausführung externer Skripts in SQL Server-Machine Learning Services](../administration/scale-concurrent-execution-external-scripts.md).

Wenn Sie die Standard Edition verwenden und nicht über den Resource Governor verfügen, können Sie zum Verwalten von Serverressourcen, die von R verwendet werden, dynamische Verwaltungssichten (DMVs) und erweiterte Ereignisse sowie die Windows-Ereignisüberwachung nutzen.

### <a name="install-additional-r-packages"></a>Installieren zusätzlicher R-Pakete

Die R-Lösungen, die Sie für SQL Server erstellen, können grundlegende Funktionen, Funktionen aus den proprietären mit SQL Server installierten Paketen sowie Funktionen aus Drittanbieterpaketen aufrufen, die mit der von SQL Server installierten Open-Source-Version von R kompatibel sind.

Pakete, die Sie von SQL Server verwenden möchten, müssen in der Standardbibliothek installiert sein, die von der Instanz verwendet wird. Wenn Sie eine separate Installation von R auf dem Computer haben oder Pakete in Benutzerbibliotheken installiert sind, werden Sie diese Pakete über T-SQL nicht verwenden können.

Der Installations- und Verwaltungsprozess für R-Pakete unterscheidet sich in SQL Server 2016 und SQL Server 2017. In SQL Server 2016 muss ein Datenbankadministrator die R-Pakete installieren, die die Benutzer benötigen. In SQL Server 2017 können Sie Benutzergruppen für die Freigabe von Paketen auf Datenbankebene einrichten oder Datenbankrollen so konfigurieren, dass Benutzer ihre eigenen Pakete installieren können. Weitere Informationen finden Sie unter [Installieren von Paketen mit R-Tools](../package-management/install-r-packages-standard-tools.md).

## <a name="next-steps"></a>Nächste Schritte

R-Entwickler können mit einigen einfachen Beispielen loslegen und die Grundlagen der Funktionen von R unter SQL Server kennenlernen. Informationen zu den nächsten Schritten finden Sie unter den folgenden Links:

+ [Schnellstart: Ausführen von R in T-SQL](../tutorials/quickstart-r-create-script.md)
+ [R-Turorials für SQL Machine Learning](../tutorials/r-tutorials.md)
+ [Machine-Learning-Dokumentation für SQL](../index.yml)