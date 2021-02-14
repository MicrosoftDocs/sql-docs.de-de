---
title: Installieren der Java-Spracherweiterung unter Windows
titleSuffix: SQL Server Language Extensions
description: Erfahren Sie, wie Sie die Java-Spracherweiterung für SQL Server unter Windows installieren.
author: dphansen
ms.author: davidph
ms.date: 11/11/2020
ms.topic: how-to
ms.prod: sql
ms.technology: language-extensions
monikerRange: '>=sql-server-ver15'
ms.openlocfilehash: f45e4265425562ef5efca8766be84f774b45b3a5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100070780"
---
# <a name="install-sql-server-java-language-extension-on-windows"></a>Installieren der Java-Spracherweiterung für SQL Server unter Windows

[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

Erfahren Sie, wie Sie die Komponente [Java-Spracherweiterung](../java-overview.md) für SQL Server unter Windows installieren. Die Java-Spracherweiterung gehört zu den [SQL Server-Spracherweiterungen](../language-extensions-overview.md).

> [!NOTE]
> In diesem Artikel wird die Installation der Java-Spracherweiterung für SQL Server unter Windows vorgestellt. Informationen für Linux finden Sie unter [Installieren von Java-Spracherweiterungen für SQL Server unter Linux](../../linux/sql-server-linux-setup-language-extensions-java.md).

<a name="prerequisites"></a>

## <a name="pre-install-checklist"></a>Prüfliste vor der Installation

+ Die Java-Spracherweiterung muss über das SQL Server 2019-Setup installiert werden.

+ Es ist eine Datenbank-Engine-Instanz erforderlich. Sie können nicht ausschließlich die Features der Java-Spracherweiterung installieren, diese aber einer vorhandenen Instanz schrittweise hinzufügen.

+ Für die Aufrechterhaltung der Geschäftskontinuität werden [Always On-Verfügbarkeitsgruppen](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md) für Spracherweiterungen unterstützt. Sie müssen auf jedem Knoten Spracherweiterungen installieren und Pakete konfigurieren.

+ In SQL Server 2019 wird die Installation der Java-Spracherweiterung in einem Failovercluster unterstützt.

+ Installieren Sie SQL Server-Spracherweiterungen oder die Java-Spracherweiterung nicht auf einem Domänencontroller. Ansonsten schlägt derjenige Teil des Setups, der die Spracherweiterungen betrifft, fehl.

+ Spracherweiterungen und [Machine Learning Services](../../machine-learning/index.yml) werden standardmäßig auf Big Data-Clustern für SQL Server installiert. Wenn Sie Big Data-Cluster verwenden, müssen Sie die Schritte in diesem Artikel nicht ausführen. Weitere Informationen finden Sie unter [Verwenden von Machine Learning Services (Python und R) in Big Data-Clustern](../../big-data-cluster/machine-learning-services.md).

> [!IMPORTANT]
> Stellen Sie nach Abschluss des Setups sicher, dass Sie die in diesem Artikel beschriebenen Schritte nach der Konfiguration durchführen. Zu diesen Schritten gehören das Aktivieren von SQL Server für die Verwendung von externem Code und das Hinzufügen von Konten, die SQL Server für das Ausführen Ihres Java-Codes benötigt. Konfigurationsänderungen erfordern in der Regel einen Neustart der Instanz oder einen Neustart des Launchpad-Diensts.

<a name="java-jre-jdk"></a>

## <a name="java-jre-or-jdk"></a>Java JRE oder JDK

Es gibt zwei Möglichkeiten, Java zu installieren und mit SQL Server zu nutzen:

1. Verwenden Sie die Java-Standardruntime, Zulu Open JRE Version 11.0.3. Diese Runtime wird unterstützt und ist in der SQL Server-Installation enthalten.

1. Verwenden Sie Ihre bevorzugte Java-Verteilung anstelle der Java-Standardruntime.

    Java 11 ist die derzeit unterstützte Version unter Windows. Die Java Runtime Environment (JRE) ist die Mindestanforderung, aber das Java Development Kit (JDK) ist nützlich, wenn Sie den Java-Compiler und die Entwicklungspakete benötigen. Da das JDK alles beinhaltet, ist die JRE bei der Installation des JDK nicht erforderlich. Unter Windows empfiehlt es sich, das JDK nach Möglichkeit unter dem Standardordner `/Program Files/` zu installieren. Andernfalls ist eine zusätzliche Konfiguration erforderlich, um ausführbare Dateien Berechtigungen zu erteilen. Weitere Informationen finden Sie im Abschnitt über das [Erteilen von Berechtigungen (Windows)](#perms-nonwindows) in diesem Dokument.

    > [!NOTE]
    > Da Java abwärtskompatibel ist, funktionieren frühere Versionen möglicherweise, die für SQL Server 2019 unterstützte und getestete Version ist jedoch Java 11.

## <a name="get-the-installation-media"></a>Abrufen der Installationsmedien

[!INCLUDE[GetInstallationMedia](../../includes/getssmedia.md)]

## <a name="run-setup"></a>Ausführen von 'Setup'

Bei lokalen Installationen müssen Sie das Setup als Administrator ausführen. Wenn Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] von einer Remotefreigabe installieren, müssen Sie ein Domänenkonto verwenden, das Lese- und Ausführungsberechtigungen auf der Remotefreigabe hat.

1. Starten Sie den Setup-Assistenten für SQL Server 2019.
  
1. Klicken Sie auf der Registerkarte **Installation** auf **Neue eigenständige SQL Server-Installation oder Hinzufügen von Funktionen zu einer vorhandenen Installation**.

    ![SQL Server 2019-Installation](../media/sql-install.png) 

1. Wählen Sie diese Optionen auf der Seite **Funktionsauswahl** aus:
  
    - **Datenbank-Engine-Dienste**
  
        Sie müssen eine Instanz der Datenbank-Engine installieren, um Spracherweiterungen mit SQL Server verwenden zu können. Sie können entweder eine Standardinstanz oder eine benannte Instanz verwenden.
  
    - **Machine Learning-Dienste und -Spracherweiterungen**
  
        Mit dieser Option wird die Spracherweiterungskomponente installiert, die die Java-Codeausführung unterstützt.

        - Wenn Sie die Java-Standardruntime, Zulu Open JRE 11.0.3, installieren möchten, wählen Sie **Machine Learning-Dienste und -Spracherweiterungen** sowie **Java** aus.

        - Wenn Sie Ihre eigene Java-Runtime verwenden möchten, wählen Sie **Machine Learning-Dienste und -Spracherweiterungen** aus. Wählen Sie nicht **Java** aus.

        Wenn Sie R und Python verwenden möchten, finden Sie Informationen unter [Installieren von SQL Server Machine Learning Services unter Windows](../../machine-learning/install/sql-machine-learning-services-windows-install.md).

    ![Featureoptionen für Spracherweiterungen](../media/sql-install-feature-selection.png)

1. Wenn Sie im vorherigen Schritt **Java** ausgewählt haben, um die Java-Standardruntime zu installieren, wird die Seite **Java-Installationsspeicherort** angezeigt.

    Wählen Sie **In dieser Installation enthaltene Open JRE-Version 11.0.3 installieren** aus.

    ![Auswählen des Java-Installationsspeicherorts](../media/sql-install-openjdk.png)

    > [!NOTE]
    > Die Option **Speicherort einer anderen Version angeben, die auf diesem Computer installiert ist** wird für Spracherweiterungen nicht verwendet.

1. Stellen Sie auf der Seite **Installationsbereit** sicher, dass die folgenden Auswahlmöglichkeiten aktiviert sind, und klicken Sie auf **Installieren**.
  
    + -Datenbank-Engine-Dienste
    + Machine Learning-Dienste und -Spracherweiterungen

    Notieren Sie sich den Speicherort des Ordners unter dem Pfad `..\Setup Bootstrap\Log`, in dem die Konfigurationsdateien gespeichert werden. Nach Abschluss des Setups können Sie die installierten Komponenten in der Zusammenfassungsdatei überprüfen.

6. Wenn Sie nach Abschluss des Setups dazu aufgefordert werden, starten Sie jetzt den Computer neu. Wenn Sie den Setupvorgang abgeschlossen haben, sollten Sie unbedingt die vom Installations-Assistenten angezeigte Meldung lesen. Weitere Informationen finden Sie unter [View and Read SQL Server Setup Log Files](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).

## <a name="add-the-jre_home-variable"></a>Hinzufügen der Variablen JRE_HOME

`JRE_HOME` ist eine Systemumgebungsvariable, die den Speicherort des Java-Interpreters angibt. In diesem Schritt erstellen Sie hierfür in Windows eine Systemumgebungsvariable.

1. Suchen und kopieren Sie den JRE-Basispfad.

    Für die standardmäßige Java-Runtime Zulu JRE 11.0.3 lautet der JRE-Basispfad beispielsweise `C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\Binn\AZUL-OpenJDK-JRE\`.

    Der JDK- oder JRE-Speicherort kann sich von dem oben genannten Beispielpfad unterscheiden. Dies hängt von Ihrem SQL Server-Installationspfad oder davon ab, ob Sie eine andere Java-Runtime ausgewählt haben. Selbst wenn Sie ein JDK installiert haben, wird häufig im Rahmen dieser Installation ein JRE-Unterordner erstellt. Verweisen Sie in einem solchen Fall auf den JRE-Ordner. Die Java-Erweiterung versucht, die `jvm.dll` aus dem Pfad `%JRE_HOME%\bin\server` zu laden.

1. Öffnen Sie in der Systemsteuerung **System und Sicherheit** und anschließend **System**, und wählen Sie dann **Erweiterte Systemeigenschaften** aus.

1. Legen Sie **Umgebungsvariablen** fest.

1. Erstellen Sie eine neue Systemvariable für `JRE_HOME` mit dem Wert des JDK-/JRE-Pfads (aus Schritt 1).

1. Starten Sie [Launchpad](../concepts/extensibility-framework.md#launchpad) neu.

    1. Öffnen Sie den [SQL Server-Konfigurations-Manager](../../relational-databases/sql-server-configuration-manager.md).

    1. Klicken Sie unter „SQL Server-Dienste“ mit der rechten Maustaste auf „SQL Server-Launchpad“, und wählen Sie **Neu starten** aus.

<a name="perms-nonwindows"></a>

## <a name="grant-access-to-non-default-jre-folder"></a>Gewähren des Zugriffs auf einen nicht standardmäßigen JRE-Ordner

Wenn Sie nicht die standardmäßige Zulu Open JRE installiert haben, die in SQL Server enthalten ist, und das JDK bzw. die JRE nicht unter „Programme“ installiert haben, müssen Sie die folgenden Schritte ausführen. Führen Sie die **icacls**-Befehle von einer Befehlszeile mit *erhöhten Rechten* aus, um für den JRE-Zugriff Zugriff auf die **SQLRUsergroup** und die SQL Server-Dienstkonten (in **ALL_APPLICATION_PACKAGES**) zu gewähren. Die Befehle gewähren rekursiv Zugriff auf alle Dateien und Ordner unter dem angegebenen Verzeichnispfad.

1. Vergeben Sie SQLRUserGroup-Berechtigungen.

    Hängen Sie für eine benannte Instanz den Namen der Instanz an SQLRUsergroup an (z. B. `SQLRUsergroupINSTANCENAME`).

    ```cmd
    icacls "<PATH to JRE>" /grant "SQLRUsergroup":(OI)(CI)RX /T
    ```
    
    Sie können diesen Schritt überspringen, wenn Sie das JDK/die JRE in Windows im Standardordner unter „Programme“ installiert haben.

1. Vergeben Sie AppContainer-Berechtigungen.

    ```cmd
    icacls “<PATH to JRE>” /grant *S-1-15-2-1:(OI)(CI)RX /T
    ```

    > [!NOTE]
    > Der obige Befehl erteilt Berechtigungen für die Computer-SID **S-1-15-2-1**, was **ALLEN ANWENDUNGSPAKETEN** auf einer englischen Version von Windows entspricht. Alternativ können Sie `icacls "<PATH to JRE>" /grant "ALL APPLICATION PACKAGES":(OI)(CI)RX /T` in einer englischen Version von Windows verwenden.

## <a name="restart-the-service"></a>Starten Sie den Dienst neu.

Starten Sie nach Abschluss der Installation die Datenbank-Engine neu, bevor Sie mit dem nächsten Schritt zum Aktivieren der Skriptausführung fortfahren.

Durch den Neustart des Diensts wird auch der zugehörige SQL Server-Launchpad-Dienst automatisch neu gestartet.

Sie können den Dienst neu starten, indem Sie mit der rechten Maustaste auf den Befehl **Neu starten** für die Instanz in SSMS klicken, den Bereich **Dienste** in der Systemsteuerung oder den [SQL Server-Konfigurations-Manager](../../relational-databases/sql-server-configuration-manager.md) verwenden.

## <a name="enable-script-execution"></a>Aktivieren der Skriptausführung

1. Öffnen Sie [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. 

1. Stellen Sie eine Verbindung mit der Instanz her, auf der Sie die Spracherweiterungen installiert haben. Klicken Sie auf **Neue Abfrage**, um ein Abfragefenster zu öffnen, und führen Sie den folgenden Befehl aus:

    ```sql
    sp_configure
    ```

    Der Wert für die Eigenschaft `external scripts enabled` sollte an diesem Punkt **0** betragen. Diese Funktion ist standardmäßig deaktiviert. Sie muss explizit von einem Administrator aktiviert werden, damit Sie Java-Code ausführen können.

1. Führen Sie die folgende Anweisung aus, um die externe Funktion für die Skripterstellung zu aktivieren:

    ```sql
    EXEC sp_configure 'external scripts enabled', 1
    RECONFIGURE WITH OVERRIDE
    ```

    Führen Sie RECONFIGURE kein zweites Mal für Spracherweiterungen aus, wenn Sie die Funktion bereits für Machine Learning Services aktiviert haben. Die zugrunde liegende Erweiterungsplattform unterstützt beides.

<a name="register_external_language"></a>

## <a name="register-external-language"></a>Registrieren einer externen Sprache

Für jede Datenbank, in der Sie Spracherweiterungen verwenden möchten, müssen Sie die externe Sprache über den Befehl [CREATE EXTERNAL LANGUAGE](../../t-sql/statements/create-external-language-transact-sql.md) registrieren.

Im folgenden Beispiel wird eine externe Sprache mit dem Namen Java einer Datenbank auf SQL Server unter Windows hinzugefügt.

```SQL
CREATE EXTERNAL LANGUAGE Java
FROM (CONTENT = N'<path-to-zip>', FILE_NAME = 'javaextension.dll');
GO
```

Weitere Informationen finden Sie unter [CREATE EXTERNAL LANGUAGE](../../t-sql/statements/create-external-language-transact-sql.md).

## <a name="verify-installation"></a>Überprüfen der Installation

Überprüfen Sie den Installationsstatus der Instanz in den Setupprotokollen.

Gehen Sie folgendermaßen vor, um zu überprüfen, ob alle zum Starten eines externen Skripts verwendeten Komponenten ausgeführt werden.

1. Öffnen Sie in SQL Server Management Studio oder in Azure Data Studio ein neues Abfragefenster, und führen Sie die folgende Anweisung aus:

    ```sql
    EXEC sp_configure 'external scripts enabled'
    ```

    Der **run_value-** ist nun auf 1 festgelegt.

1. Öffnen Sie den Bereich **Dienste** oder den SQL Server-Konfigurations-Manager, und überprüfen Sie, ob der **SQL Server-Launchpad-Dienst** ausgeführt wird. Sie sollten über einen Dienst für jede Datenbank-Engine-Instanz verfügen, auf der Spracherweiterungen installiert sind. Weitere Informationen zu dem Dienst finden Sie unter [Erweiterbarkeitsframework](../concepts/extensibility-framework.md).

## <a name="additional-configuration"></a>Zusätzliche Konfiguration

Wenn der Überprüfungsschritt erfolgreich war, können Sie Java-Code aus SQL Server Management Studio, Azure Data Studio, Visual Studio Code oder einem anderen Client ausführen, der T-SQL-Anweisungen an den Server senden kann.

Überprüfen Sie die zusätzlichen Konfigurationsschritte in diesem Abschnitt, wenn beim Ausführen des Befehls ein Fehler aufgetreten ist. Möglicherweise müssen Sie für den Dienst oder die Datenbank zusätzliche geeignete Konfigurationen vornehmen.

Auf Instanzebene kann eine zusätzliche Konfiguration Folgendes umfassen:

+ [Firewallkonfiguration für SQL Server- Machine Learning Services](../../machine-learning/security/firewall-configuration.md)
+ [Aktivieren zusätzlicher Netzwerkprotokolle](../../database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md)
+ [Aktivieren von Remoteverbindungen](../../database-engine/configure-windows/configure-the-remote-access-server-configuration-option.md)
+ [Erstellen von Anmeldeinformationen für SQLRUserGroup](../../machine-learning/security/create-a-login-for-sqlrusergroup.md)

<a name="bkmk_configureAccounts"></a>
<a name="permissions-external-script"></a>

Für die Datenbank benötigen Sie möglicherweise die folgenden Konfigurationsupdates:

+ [Benutzern die Berechtigung für SQL Server-Machine Learning Services erteilen](../../machine-learning/security/user-permission.md)
+ [Benutzern die Berechtigung zum Ausführen einer bestimmten Sprache erteilen](../../t-sql/statements/create-external-language-transact-sql.md#permissions)

> [!NOTE]
> Ob eine zusätzliche Konfiguration erforderlich ist, hängt vom Sicherheitsschema, in dem Sie SQL Server installiert haben, sowie von Ihren Erwartungen bezüglich des Herstellens einer Verbindung mit der Datenbank und dem Ausführen externer Skripts seitens der Benutzer ab.

## <a name="suggested-optimizations"></a>Empfohlene Optimierungen

Da nun alles funktioniert, möchten Sie möglicherweise auch zur Unterstützung der Java-Spracherweiterung den Server optimieren.

### <a name="optimize-the-server-for-java-language-extension"></a>Optimieren des Servers für die Java-Spracherweiterung

Die Standardeinstellungen für das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Setup dienen zur Optimierung des Lastenausgleichs des Servers für eine Vielzahl von Diensten, die von der Datenbank-Engine unterstützt werden, einschließlich ETL-Prozesse (Extrahieren, Transformieren und Laden), Reporting, Überwachung und Anwendungen, die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Daten verwenden. Daher können in den Standardeinstellungen Ressourcen für Spracherweiterungen eingeschränkt oder gedrosselt sein, insbesondere für speicherintensive Vorgänge.

Es wird empfohlen, dass Sie zum Konfigurieren eines externen Ressourcenpools den SQL Server-Resource Governor verwenden, um sicherzustellen, dass Spracherweiterungsaufgaben über die entsprechende Priorität und die nötigen Ressourcen verfügen. Eventuell ist es auch sinnvoll, die Größe des Speichers zu ändern, der der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbank-Engine zugewiesen ist, oder die Anzahl der Konten zu erhöhen, die unter dem [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)]-Dienst ausgeführt werden.

+ Informationen zum Konfigurieren eines Ressourcenpools zum Verwalten externer Ressourcen finden Sie unter [Erstellen eines externen Ressourcenpools](../../t-sql/statements/create-external-resource-pool-transact-sql.md).
  
+ Informationen zum Ändern des für die Datenbank reservierten Arbeitsspeichers finden Sie unter [Konfigurationsoptionen für den Serverarbeitsspeicher](../../database-engine/configure-windows/server-memory-server-configuration-options.md).
  
Wenn Sie die Standard Edition verwenden und nicht über den Resource Governor verfügen, können Sie zum Verwalten von Serverressourcen dynamische Verwaltungssichten (DMVs) und erweiterte Ereignisse sowie die Windows-Ereignisüberwachung verwenden.

## <a name="next-steps"></a>Nächste Schritte

Java-Entwickler können mit einigen einfachen Beispielen loslegen und die Grundlagen der Funktionen von Java unter SQL Server kennenlernen. Informationen zum nächsten Schritt finden Sie unter dem folgenden Link:

+ [Tutorial: Reguläre Ausdrücke in Java](../tutorials/search-for-string-using-regular-expressions-in-java.md)