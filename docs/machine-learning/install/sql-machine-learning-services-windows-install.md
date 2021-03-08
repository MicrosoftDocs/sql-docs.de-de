---
title: Installieren unter Windows
description: Erfahren Sie, wie Sie SQL Server-Machine Learning Services unter Windows installieren. Sie können Machine Learning Services verwenden, um Python- und R-Skripts in einer Datenbank auszuführen.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/17/2021
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017'
ms.openlocfilehash: 44993ae527e99d615a9be620e3d930aa520c8980
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839507"
---
# <a name="install-sql-server-machine-learning-services-python-and-r-on-windows"></a>Installieren von SQL Server Machine Learning Services (Python und R) unter Windows

[!INCLUDE [SQL Server 2017 and later](../../includes/applies-to-version/sqlserver2017.md)]

Erfahren Sie, wie Sie SQL Server-Machine Learning Services unter Windows installieren. Sie können Machine Learning Services verwenden, um Python- und R-Skripts in einer Datenbank auszuführen.

## <a name="pre-install-checklist"></a><a name="bkmk_prereqs"> </a> Prüfliste vor der Installation

+ Es ist eine Datenbank-Engine-Instanz erforderlich. Sie können nicht nur R- oder Python-Funktionen installieren, aber Sie können diese einer vorhandenen Instanz inkrementell hinzufügen.

+ Für die Geschäftskontinuität werden für Machine Learning Services [Always On-Verfügbarkeitsgruppen](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md) unterstützt. Installieren Sie auf jedem Knoten Machine Learning Services, und konfigurieren Sie Pakete.

+ Das Installieren von Machine Learning Services auf einer [Always On-Failoverclusterinstanz (FCI)](../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md) wird in SQL Server 2017 *nicht unterstützt*. Bei SQL Server 2019 und höheren Versionen wird dies jedoch unterstützt.
 
+ Installieren Sie Machine Learning Services nicht auf einem Domänencontroller. Bei dem Teil des Setups, der sich auf Machine Learning Services bezieht, tritt ein Fehler auf.

+ Installieren Sie **Freigegebene Funktionen** > **Machine Learning Server (eigenständig)** nicht auf dem gleichen Computer, auf dem eine Datenbankinstanz ausgeführt wird. Ein eigenständiger Server wird die gleichen Ressourcen nutzen, wodurch die Leistung beider Installationen reduziert wird.

+ Eine parallele Installation mit anderen Versionen von R und Python wird zwar unterstützt, jedoch nicht empfohlen. Dies wird unterstützt, da die SQL Server-Instanz eigene Kopien der Open-Source-Distributionen von R und Anaconda verwendet. Es wird jedoch nicht empfohlen, da das Ausführen von Code, der R und Python auf dem SQL Server-Computer außerhalb von SQL Server verwendet, zu unterschiedlichen Problemen führen kann:
    
  + Durch die Verwendung einer anderen Bibliothek und anderer ausführbarer Dateien werden inkonsistente Ergebnisse erzeugt, die von der Ausführung in SQL Server abweichen.
  + R- und Python-Skripts, die in externen Bibliotheken ausgeführt werden, können nicht von SQL Server verwaltet werden, was zu Ressourcenkonflikten führt.

::: moniker range=">=sql-server-ver15"
> [!NOTE]
> Machine Learning Services wird standardmäßig auf **SQL Server-Big Data-Clustern** installiert. Wenn Sie einen **Big Data-Cluster** verwenden, müssen Sie die Schritte in diesem Artikel nicht ausführen. Weitere Informationen finden Sie unter [Verwenden von Machine Learning Services (Python und R) in Big Data-Clustern](../../big-data-cluster/machine-learning-services.md).
::: moniker-end

> [!IMPORTANT]
> Stellen Sie nach Abschluss des Setups sicher, dass Sie die in diesem Artikel beschriebenen Schritte nach der Konfiguration durchführen. Zu diesen Schritten gehören das Aktivieren von SQL Server für die Verwendung externer Skripts und das Hinzufügen von Konten, die für SQL Server zum Ausführen Ihrer R- und Python-Aufträge erforderlich sind. Konfigurationsänderungen erfordern in der Regel einen Neustart der Instanz oder einen Neustart des Launchpad-Diensts.

## <a name="get-the-installation-media"></a>Abrufen der Installationsmedien

[!INCLUDE[GetInstallationMedia](../../includes/getssmedia.md)]

::: moniker range="=sql-server-2017"
Weitere Informationen zu den SQL Server-Editionen, die die Python- und R-Integration in Machine Learning Services unterstützen, finden Sie unter [Editionen und unterstützte Funktionen von SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md).
::: moniker-end

::: moniker range="=sql-server-ver15"
Weitere Informationen zu den SQL Server-Editionen, die die Python- und R-Integration in Machine Learning Services unterstützen, finden Sie unter [Editionen und unterstützte Funktionen von SQL Server 2019 (15.x)](../../sql-server/editions-and-components-of-sql-server-version-15.md).
::: moniker-end

## <a name="run-setup"></a>Ausführen von Setup

Bei lokalen Installationen müssen Sie das Setup als Administrator ausführen. Wenn Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] von einer Remotefreigabe installieren, müssen Sie ein Domänenkonto verwenden, das Lese- und Ausführungsberechtigungen auf der Remotefreigabe hat.

1. Starten Sie den Setup-Assistenten für SQL Server.
  
1. Klicken Sie auf der Registerkarte **Installation** auf **Neue eigenständige SQL Server-Installation oder Hinzufügen von Funktionen zu einer vorhandenen Installation**.

   ::: moniker range="=sql-server-2017"
   ![Neue eigenständige SQL Server-Installationen](media/2017setup-installation-page-mlsvcs.png)
   ::: moniker-end

   ::: moniker range="=sql-server-ver15"
   ![Neue eigenständige SQL Server-Installationen](media/2019setup-installation-page-mlsvcs.png)
   ::: moniker-end

1. Wählen Sie diese Optionen auf der Seite **Funktionsauswahl** aus:

   ::: moniker range="=sql-server-2017"

   - **Datenbank-Engine-Dienste**
     
     Sie müssen eine Instanz der Datenbank-Engine installieren, um R und Python mit SQL Server verwenden zu können. Sie können entweder eine Standardinstanz oder eine benannte Instanz verwenden.

   - **Machine Learning Services (datenbankintern)**
     
     Mit dieser Option werden die Datenbankdienste installiert, die die Skriptausführung von R und Python unterstützen.

   ::: moniker-end

   ::: moniker range="=sql-server-ver15"

   - **Datenbank-Engine-Dienste**
     
     Sie müssen eine Instanz der Datenbank-Engine installieren, um R oder Python mit SQL Server verwenden zu können. Sie können entweder eine Standardinstanz oder eine benannte Instanz verwenden.

   - **Machine Learning Services (datenbankintern)**
     
     Mit dieser Option werden die Datenbankdienste installiert, die die Skriptausführung von R und Python unterstützen.

   ::: moniker-end

   - **R**
     
     Wählen Sie diese Option aus, um die Microsoft R-Pakete, -Interpreter und Open-Source-R hinzuzufügen. 
     
   - **Python**
     
     Wählen Sie diese Option aus, um die Microsoft Python-Pakete, die ausführbare Python 3.5-Datei und ausgewählte Bibliotheken aus der Anaconda-Distribution hinzuzufügen.
     
   ::: moniker range="=sql-server-ver15"
   Informationen zum Installieren und Verwenden von Java finden Sie unter [Installieren von SQL Server-Spracherweiterungen unter Windows](../../language-extensions/install/windows-java.md).
   ::: moniker-end
   
   ::: moniker range="=sql-server-2017"
   ![Funktionsoptionen für R und Python](media/2017setup-features-page-mls-rpy.PNG "Setupoptionen für R und Python")
   ::: moniker-end
   
   ::: moniker range="=sql-server-ver15"
   ![Funktionsoptionen für R und Python](media/2019setup-features-page-mls-rpy.png "Setupoptionen für R und Python")
   ::: moniker-end
   
   > [!NOTE]
   > 
   > Wählen Sie nicht die Option **Machine Learning Server (eigenständig)** aus. Die Option unter **Freigegebene Funktionen** zum Installieren von Machine Learning Server ist für die Verwendung auf einem separaten Computer vorgesehen.

::: moniker range="=sql-server-2017"

4. Klicken Sie auf der Seite **Zustimmung zur Installation von Microsoft R Open** zunächst auf **Akzeptieren** und anschließend auf **Weiter**. 

Die Lizenzbedingungen decken Folgendes ab:
+ Microsoft R Open
+ Open-Source-R-Basispakete und -Tools
+ Erweiterte R-Pakete und Konnektivitätsanbieter aus dem Microsoft-Entwicklungsteam.

1. Klicken Sie auf der Seite **Zustimmung zur Installation von Python** zunächst auf **Akzeptieren** und anschließend auf **Weiter**. Der Open-Source-Lizenzvertrag von Python umfasst auch Anaconda- und ähnliche Tools sowie einige neue Python-Bibliotheken des Microsoft-Entwicklungsteams.

   > [!NOTE]
   >  Falls der Computer, den Sie verwenden, keinen Internetzugriff hat, können Sie das Setup zu diesem Zeitpunkt anhalten, um die Installationsprogramme separat herunterzuladen. Weitere Informationen finden Sie unter [Installieren von Machine Learning-Komponenten ohne Internetzugang](../install/sql-ml-component-install-without-internet-access.md).

1. Stellen Sie auf der Seite **Installationsbereit** sicher, dass die folgenden Auswahlmöglichkeiten aktiviert sind, und klicken Sie auf **Installieren**.
  
   + -Datenbank-Engine-Dienste
   + Machine Learning-Dienste (datenbankintern)
   + R, Python oder beide

   Notieren Sie sich den Speicherort des Ordners unter dem Pfad `..\Setup Bootstrap\Log`, in dem die Konfigurationsdateien gespeichert werden. Nach Abschluss des Setups können Sie die installierten Komponenten in der Zusammenfassungsdatei überprüfen.

1. Wenn Sie nach Abschluss des Setups dazu aufgefordert werden, starten Sie jetzt den Computer neu. Wenn Sie den Setupvorgang abgeschlossen haben, sollten Sie unbedingt die vom Installations-Assistenten angezeigte Meldung lesen. Weitere Informationen finden Sie unter [View and Read SQL Server Setup Log Files](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).

::: moniker-end

::: moniker range="=sql-server-ver15"

1. Klicken Sie auf der Seite **Zustimmung zur Installation von Microsoft R Open** zunächst auf **Akzeptieren** und anschließend auf **Weiter**. Dieser Lizenzvertrag umfasst Microsoft R Open mit einer Distribution der Open Source-R-Basispakete und -Tools einschließlich erweiterter R-Pakete und Konnektivitätsanbieter des Microsoft-Entwicklungsteams.

2. Klicken Sie auf der Seite **Zustimmung zur Installation von Python** zunächst auf **Akzeptieren** und anschließend auf **Weiter**. Der Open-Source-Lizenzvertrag von Python umfasst auch Anaconda- und ähnliche Tools sowie einige neue Python-Bibliotheken des Microsoft-Entwicklungsteams.

3. Stellen Sie auf der Seite **Installationsbereit** sicher, dass die folgenden Auswahlmöglichkeiten aktiviert sind, und klicken Sie auf **Installieren**.
  
   + -Datenbank-Engine-Dienste
   + Machine Learning-Dienste (datenbankintern)
   + R und/oder Python

   Notieren Sie sich den Speicherort des Ordners unter dem Pfad `..\Setup Bootstrap\Log`, in dem die Konfigurationsdateien gespeichert werden. Nach Abschluss des Setups können Sie die installierten Komponenten in der Zusammenfassungsdatei überprüfen.

4. Wenn Sie nach Abschluss des Setups dazu aufgefordert werden, starten Sie jetzt den Computer neu. Wenn Sie den Setupvorgang abgeschlossen haben, sollten Sie unbedingt die vom Installations-Assistenten angezeigte Meldung lesen. Weitere Informationen finden Sie unter [View and Read SQL Server Setup Log Files](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).

::: moniker-end

## <a name="set-environment-variables"></a>Festlegen von Umgebungsvariablen

Wenn Sie nur das R-Feature integrieren möchten, sollten Sie die Umgebungsvariable **MKL_CBWR** über die Intel Math Kernel Library-Berechnungen auf [ensure consistent output](https://software.intel.com/articles/introduction-to-the-conditional-numerical-reproducibility-cnr) (Konsistente Ausgabe sicherstellen) festlegen.

1. Klicken Sie in der Systemsteuerung auf **System und Sicherheit** > **System** > **Erweiterte Systemeinstellungen** > **Umgebungsvariablen**.

2. Erstellen Sie eine neue Benutzer- oder Systemvariable. 

   + Legen Sie den Variablenname auf `MKL_CBWR` fest.
   + Legen Sie den Variablenwert auf `AUTO` fest.

Für diesen Schritt ist ein Neustart des Servers erforderlich. Wenn Sie im Begriff sind, die Skriptausführung zu aktivieren, können Sie den Neustart anhalten, bis die gesamte Konfiguration abgeschlossen ist.

<a name="bkmk_enableFeature"></a>

## <a name="enable-script-execution"></a>Aktivieren der Skriptausführung

1. Öffnen Sie [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. 

    > [!TIP]
    > Sie können die entsprechende Version von folgender Seite herunterladen und installieren: [Herunterladen von SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md)
    > 
    > Sie können auch [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md) verwenden, das administrative Aufgaben und Abfragen für SQL Server unterstützt.
  
2. Stellen Sie eine Verbindung mit der Instanz her, auf der Sie Machine Learning Services installiert haben. Klicken Sie auf **Neue Abfrage**, um ein Abfragefenster zu öffnen, und führen Sie den folgenden Befehl aus:

    ```sql
    sp_configure
    ```

    Der Wert für die Eigenschaft `external scripts enabled` sollte an diesem Punkt **0** betragen. Dieses Feature ist standardmäßig deaktiviert. Die Funktion muss explizit von einem Administrator aktiviert werden, bevor Sie R- oder Python-Skripts ausführen können.
    
3.  Führen Sie die folgende Anweisung aus, um die externe Funktion für die Skripterstellung zu aktivieren:
    
    ```sql
    EXEC sp_configure  'external scripts enabled', 1
    RECONFIGURE WITH OVERRIDE
    ```
    
    Führen Sie RECONFIGURE kein zweites Mal für Python aus, wenn Sie die Funktion für die R-Sprache bereits aktiviert haben. Die zugrunde liegende Erweiterungsplattform unterstützt beide Sprachen.

## <a name="restart-the-service"></a>Starten Sie den Dienst neu.

Wenn die Installation abgeschlossen ist, starten Sie die Datenbank-Engine neu.

Durch den Neustart des Diensts wird auch der zugehörige [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)]-Dienst automatisch neu gestartet.

Sie können den Dienst neu starten, indem Sie mit der rechten Maustaste auf den Befehl **Neu starten** für die Instanz in SSMS klicken, den Bereich **Dienste** in der Systemsteuerung oder den [SQL Server-Konfigurations-Manager](../../relational-databases/sql-server-configuration-manager.md) verwenden.

## <a name="verify-installation"></a>Überprüfen der Installation

Gehen Sie folgendermaßen vor, um zu überprüfen, ob alle zum Starten eines externen Skripts verwendeten Komponenten ausgeführt werden.

1. Öffnen Sie in SQL Server Management Studio ein neues Abfragefenster, und führen Sie den folgenden Befehl aus:
    
   ```sql
   EXECUTE sp_configure  'external scripts enabled'
   ```

   Der **run_value** ist auf 1 festgelegt.
    
2. Öffnen Sie den Bereich **Dienste** oder den SQL Server-Konfigurations-Manager, und überprüfen Sie, ob der **SQL Server-Launchpad-Dienst** ausgeführt wird. Sie sollten über einen Dienst für jede Datenbank-Engine-Instanz verfügen, auf der R oder Python installiert ist. Weitere Informationen zu dem Dienst finden Sie unter [Erweiterbarkeitsframework](../concepts/extensibility-framework.md). 
   
3. Wenn Launchpad ausgeführt wird, können Sie einfache R- und Python-Skripts ausführen, um zu überprüfen, ob externe Skriptruntimes mit SQL Server kommunizieren können.

   Öffnen Sie ein neues **Abfragefenster** in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], und führen Sie ein Skript wie das folgende aus:
   + Für R
   
     ```sql
     EXEC sp_execute_external_script  @language =N'R',
     @script=N'
     OutputDataSet <- InputDataSet;
     ',
     @input_data_1 =N'SELECT 1 AS hello'
     WITH RESULT SETS (([hello] int not null));
     GO
     ```
     
   + Für Python
     
     ```sql
     EXEC sp_execute_external_script  @language =N'Python',
     @script=N'
     OutputDataSet = InputDataSet;
     ',
     @input_data_1 =N'SELECT 1 AS hello'
     WITH RESULT SETS (([hello] int not null));
     GO
     ```
   
   **Ergebnisse**

   Die Ausführung des Skripts kann einige Zeit in Anspruch nehmen, wenn die externe Skriptruntime zum ersten Mal geladen wird. Die Ergebnisse sollten etwa wie folgt aussehen:

   | Hello |
   |----|
   | 1|

> [!NOTE]
> Im Python-Skript verwendete Spalten oder Überschriften werden nicht automatisch zurückgegeben. Sie müssen das Schema für das Rückgabedataset angeben, um Spaltennamen für die Ausgabe hinzuzufügen. Verwenden Sie hierzu den WITH RESULTS-Parameter der gespeicherten Prozedur, benennen Sie die Spalten, und geben Sie den SQL-Datentyp an.
>
> Sie können beispielsweise die folgende Zeile hinzufügen, um einen beliebigen Spaltennamen zu generieren: `WITH RESULT SETS ((Col1 AS int))`

::: moniker range=">=sql-server-2017"

<a name="apply-cu"></a>

## <a name="apply-updates"></a>Anwenden von Updates

### <a name="existing-installation"></a>Vorhandene Installation

Wenn Sie Machine Learning Services zu einer vorhandenen SQL Server-Instanz hinzugefügt haben und zuvor ein kumulatives Update (CU) angewendet haben, können die Versionen Ihrer Datenbank-Engine und des Features Machine Learning Services unterschiedlich sein. Dies kann zu unerwartetem Verhalten oder Fehlern führen. 

Führen Sie die folgenden Schritte aus, um die Machine Learning Services auf die gleiche Version wie Ihre Datenbank-Engine umzustellen.

1. Ermitteln Sie das kumulative Update (CU), das Sie für die Datenbank-Engine haben. Führen Sie diese T-SQL-Anweisung aus:

   ```sql
   SELECT @@VERSION
   ```
 
   Hier ist eine Beispielausgabe für SQL Server 2019, kumulatives Update (CU) 8:
 
   *Microsoft SQL Server 2019 (RTM-CU8-GDR) (KB4583459) - 15.0.4083.2 (X64)   Nov  2 2020 18:35:09   Copyright (C) 2019 Microsoft Corporation  Developer Edition (64-bit) on Windows 10 Enterprise 10.0 <X64> (Build 19042: ) (Hypervisor)*

   Weitere Informationen finden Sie unter [Bestimmen der Version, Edition und Updateebene von SQL Server und zugehöriger Komponenten](https://docs.microsoft.com/troubleshoot/sql/general/determine-version-edition-update-level#machine-learning-services).

1. Laden Sie das [kumulative Update (CU)](../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md) herunter, das Sie für die Datenbank-Engine installiert haben.

1. Führen Sie die Installation des kumulativen Updates (CU) aus, und folgen Sie den Anweisungen zur Installation des CU für Machine Learning Services.

### <a name="new-installation"></a>Neue Installation

Wenn Sie Machine Learning Services mit einer neuen Installation der SQL Server-Datenbank-Engine installieren, empfehlen wir, das neueste kumulative Update sowohl für die Datenbank-Engine als auch für die Komponenten für maschinelles Lernen anzuwenden.

Auf Geräten, die mit dem Internet verbunden sind, werden kumulative Updates in der Regel über Windows Update angewendet. Sie können jedoch auch die nachfolgenden Schritte für kontrollierte Updates verwenden. Wenn Sie das Update für die Datenbank-Engine anwenden, ruft das Setup kumulative Updates für alle R- oder Python-Funktionen ab, die Sie auf derselben Instanz installiert haben. 

Getrennte Server erfordern zusätzliche Schritte. Weitere Informationen finden Sie unter [Installieren auf Computern ohne Internetzugriff > Kumulative Updates anwenden](sql-ml-component-install-without-internet-access.md#apply-cu).

1. Beginnen Sie mit einer bereits installierten Baseline-Instanz: SQL Server (erste Version).

2. Navigieren Sie zur Liste der kumulativen Updates: [Neueste Updates für Microsoft SQL Server](../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md).

3. Wählen Sie das neueste kumulative Update aus. Eine ausführbare Datei wird automatisch heruntergeladen und extrahiert.

4. Führen Sie das Setup aus. Akzeptieren Sie die Lizenzbedingungen, und überprüfen Sie auf der Seite für die Funktionsauswahl die Funktionen, für die kumulative Updates angewendet werden. Es sollte jede für die aktuelle Instanz installierte Funktion einschließlich Machine Learning-Funktionen angezeigt werden. Das Setup lädt die CAB-Dateien herunter, die zum Aktualisieren aller Funktionen erforderlich sind.

   ![Zusammenfassung installierter Funktionen](media/cumulative-update-feature-selection.png)

5. Folgen Sie den weiteren Anweisungen des Assistenten, und akzeptieren Sie die Lizenzbedingungen für die R- und Python-Distributionen. 

::: moniker-end

## <a name="additional-configuration"></a>Zusätzliche Konfiguration

Wenn der Schritt zur externen Skriptüberprüfung erfolgreich war, können Sie R- oder Python-Befehle von SQL Server Management Studio, Visual Studio Code oder einem anderen Client ausführen, der T-SQL-Anweisungen an den Server senden kann.

Überprüfen Sie die zusätzlichen Konfigurationsschritte in diesem Abschnitt, wenn beim Ausführen des Befehls ein Fehler aufgetreten ist. Möglicherweise müssen Sie für den Dienst oder die Datenbank zusätzliche geeignete Konfigurationen vornehmen.

Auf Instanzebene kann eine zusätzliche Konfiguration Folgendes umfassen:

* [Firewallkonfiguration für SQL Server- Machine Learning Services](../../machine-learning/security/firewall-configuration.md)
* [Aktivieren zusätzlicher Netzwerkprotokolle](../../database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md)
* [Aktivieren von Remoteverbindungen](../../database-engine/configure-windows/configure-the-remote-access-server-configuration-option.md)
* [Erstellen von Anmeldeinformationen für SQLRUserGroup](../../machine-learning/security/create-a-login-for-sqlrusergroup.md)
* [Verwalten von Datenträgerkontingenten](/windows/desktop/fileio/managing-disk-quotas), um das Ausführen externer Skripts zu vermeiden, die Speicherplatz verbrauchen

::: moniker range=">=sql-server-ver15"
Der Isolationsmechanismus hat sich in SQL Server 2019 unter Windows geändert. Dieser Mechanismus wirkt sich auf **SQLRUserGroup**, Firewallregeln, Dateiberechtigungen und die implizite Authentifizierung aus. Weitere Informationen finden Sie unter [Isolationsänderungen für Machine Learning Services](sql-server-machine-learning-services-2019.md).
::: moniker-end

<a name="bkmk_configureAccounts"></a> 
<a name="permissions-external-script"></a> 

Für die Datenbank benötigen Sie möglicherweise die folgenden Konfigurationsupdates:

* [Benutzern die Berechtigung für SQL Server-Machine Learning Services erteilen](../../machine-learning/security/user-permission.md)

> [!NOTE]
> Ob die zusätzliche Konfiguration erforderlich ist, hängt vom Sicherheitsschema, in dem Sie SQL Server installiert haben, sowie von Ihren Erwartungen bezüglich des Herstellens einer Verbindung mit der Datenbank und dem Ausführen externer Skripts seitens der Benutzer ab.

## <a name="suggested-optimizations"></a>Empfohlene Optimierungen

Da nun alles funktioniert, möchten Sie möglicherweise auch den Server für die Unterstützung von Machine Learning optimieren oder vorab trainierte Modelle für maschinelles Lernen installieren.

::: moniker range="=sql-server-2017"

### <a name="add-more-worker-accounts"></a>Hinzufügen weiterer Geschäftskonten

Wenn Sie erwarten, dass viele Benutzer gleichzeitig Skripts ausführen werden, können Sie die Anzahl der Workerkonten erhöhen, die dem Launchpad-Dienst zugewiesen sind. Weitere Informationen finden Sie unter [Skalieren der gleichzeitige Ausführung externer Skripts in SQL Server-Machine Learning Services](../administration/scale-concurrent-execution-external-scripts.md).
::: moniker-end

### <a name="optimize-the-server-for-script-execution"></a>Optimieren des Servers für die Skriptausführung

Die Standardeinstellungen für das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Setup dienen zur Optimierung des Lastenausgleichs des Servers für eine Vielzahl von Diensten, die von der Datenbank-Engine unterstützt werden, einschließlich ETL-Prozesse (Extrahieren, Transformieren und Laden), Reporting, Überwachung und Anwendungen, die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Daten verwenden. In den Standardeinstellungen können Ressourcen für Machine Learning-Vorgänge, insbesondere für speicherintensive Vorgänge, eingeschränkt oder gedrosselt sein.

Es wird empfohlen, dass Sie zum Konfigurieren eines externen Ressourcenpools den SQL Server-Resource Governor verwenden, um sicherzustellen, dass Machine Learning-Aufgaben über die entsprechende Priorität und die nötigen Ressourcen verfügen. Eventuell ist es auch sinnvoll, die Größe des Speichers zu ändern, der der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbank-Engine zugewiesen ist, oder die Anzahl der Konten zu erhöhen, die unter dem [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)]-Dienst ausgeführt werden.

- Informationen zum Konfigurieren eines Ressourcenpools zum Verwalten externer Ressourcen finden Sie unter [Erstellen eines externen Ressourcenpools](../../t-sql/statements/create-external-resource-pool-transact-sql.md).
  
- Informationen zum Ändern des für die Datenbank reservierten Arbeitsspeichers finden Sie unter [Konfigurationsoptionen für den Serverarbeitsspeicher](../../database-engine/configure-windows/server-memory-server-configuration-options.md).
  
- Informationen zum Ändern der Anzahl von R-Konten, die durch [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)] gestartet werden können, finden Sie unter [Skalieren der gleichzeitigen Ausführung externer Skripts in SQL Server-Machine Learning Services](../administration/scale-concurrent-execution-external-scripts.md).

Wenn Sie die Standard Edition verwenden und nicht über den Resource Governor verfügen, können Sie zum Verwalten von Serverressourcen dynamische Verwaltungssichten (Dynamic Management Views, DMVs) und erweiterte Ereignisse sowie die Windows-Ereignisüberwachung verwenden.

### <a name="install-additional-python-and-r-packages"></a>Installieren zusätzlicher Python- und R-Pakete

Die Python- und R-Lösungen, die Sie für SQL Server erstellen, können grundlegende Funktionen, Funktionen aus den proprietären mit SQL Server installierten Paketen sowie Funktionen aus Drittanbieterpaketen aufrufen, die mit der von SQL Server installierten Open-Source-Version von Python und R kompatibel sind.

Pakete, die Sie von SQL Server verwenden möchten, müssen in der Standardbibliothek installiert sein, die von der Instanz verwendet wird. Wenn Sie eine separate Installation von R oder Python auf dem Computer haben, oder wenn Pakete in Benutzerbibliotheken installiert sind, können Sie diese Pakete von T-SQL nicht verwenden.

Sie können zum Installieren und Verwalten zusätzlicher Pakete Benutzergruppen für die Freigabe von Paketen auf Datenbankebene einrichten oder Datenbankrollen so konfigurieren, dass Benutzer ihre eigenen Pakete installieren können. Weitere Informationen finden Sie unter [Installieren von Python-Paketen](../package-management/install-additional-python-packages-on-sql-server.md) und [Installieren neuer R-Pakete](../package-management/install-additional-r-packages-on-sql-server.md).

## <a name="next-steps"></a>Nächste Schritte

Python-Entwickler können in den folgenden Tutorials erfahren, wie Python mit SQL Server verwendet werden kann:

+ [Python-Tutorial: Bereitstellen eines linearen Regressionsmodells in SQL Server Machine Learning Services](../tutorials/python-ski-rental-linear-regression-deploy-model.md)
+ [Python-Tutorial: Kategorisieren von Kunden mithilfe von K-Means-Clustering mit SQL Server Machine Learning Services](../tutorials/python-clustering-model.md)

R-Entwickler können mit einigen einfachen Beispielen loslegen und die Grundlagen der Funktionen von R unter SQL Server kennenlernen. Informationen zu den nächsten Schritten finden Sie unter den folgenden Links:

+ [Schnellstart: Ausführen von R in T-SQL](../tutorials/quickstart-r-create-script.md)
+ [Tutorial: Datenbankinterne Analysen für R-Entwickler](../tutorials/r-taxi-classification-introduction.md)
