---
title: Häufig gestellte Fragen zu SQL Server für Linux
description: Dieser Artikel bietet Antworten auf häufig gestellte Fragen zu SQL Server für Linux.
author: VanMSFT
ms.author: vanto
ms.date: 10/23/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 68f135dfbc2b03cfb27c7ab8b14345f0ffcc32db
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348137"
---
# <a name="sql-server-on-linux-frequently-asked-questions-faq"></a>Häufig gestellte Fragen zu SQL Server für Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

In den folgenden Abschnitten werden häufig gestellte Fragen zu SQL Server für Linux beantwortet.

## <a name="general-questions"></a>Allgemeine Fragen

1. **Welche Linux-Plattformen werden unterstützt?**

   SQL Server wird derzeit auf Red Hat Enterprise Server, SUSE Linux Enterprise Server und Ubuntu unterstützt. Außerdem wird die Ausführung von SQL Server in einem Docker-Container unterstützt. Die neuesten Informationen über die unterstützten Versionen finden Sie unter [Supported platforms (Unterstützte Plattformen)](sql-server-linux-setup.md#supportedplatforms).

1. **Wird SQL Server für Linux auf anderen Plattformen funktionieren?**

   SQL Server wird unter Linux für die oben aufgeführten Verteilungen getestet und unterstützt. Andere Linux-Verteilungen sind eng verwandt und können SQL Server möglicherweise ausführen (z. B. ist CentOS eng mit Red Hat Enterprise Server verwandt). Wenn Sie SQL Server jedoch unter einem nicht unterstützten Betriebssystem installieren möchten, finden Sie Informationen zu den Auswirkungen auf die Unterstützung unter [Technical support policy for Microsoft SQL Server (Richtlinien für die technische Unterstützung von Microsoft SQL Server)](https://support.microsoft.com/help/4047326/support-policy-for-microsoft-sql-server) im Abschnitt **Unterstützungsrichtlinie**. Beachten Sie auch, dass einige von der Community verwalteten Linux-Verteilungen über keine formale Möglichkeit verfügen, unterstützt zu werden, wenn das zugrunde liegende Betriebssystem das Problem ist.

1. **Ist SQL Server für Linux identisch mit SQL Server unter Windows?**

   Die zugrunde liegende Datenbank-Engine für SQL Server ist unter Linux und Windows identisch. Einige Features werden derzeit jedoch nicht unter Linux unterstützt. Eine Liste von Features, die unter Linux nicht unterstützt werden, finden Sie unter [Nicht unterstützte Features und Dienste](sql-server-linux-editions-and-components-2019.md#Unsupported). Überprüfen Sie außerdem die [bekannten Probleme](sql-server-linux-release-notes.md#known-issues). Sofern andere SQL Server-Features und -Dienste nicht in dieser Liste aufgeführt werden, werden sie unter Linux unterstützt.

1. **Wie lautet die Unterstützungsrichtlinie für SQL Server?**

   Die Unterstützungsrichtlinie können Sie unter [Technical Support Policy for SQL Server (Richtlinien für die technische Unterstützung von SQL Server)](https://support.microsoft.com/help/4047326/support-policy-for-microsoft-sql-server).

1. **Ich verfüge bereits über Kenntnisse über SQL Server unter Windows. Gibt es Ressourcen, mit denen ich die Verwendung von SQL Server für Linux erlernen kann?**

   In den [Schnellstarts](sql-server-linux-setup.md#platforms) finden Sie ausführliche Anweisungen zum Installieren von SQL Server für Linux und zum Ausführen von Transact-SQL-Abfragen. In anderen Tutorials finden Sie weitere Anweisungen zur Verwendung von SQL Server für Linux. Eine Drittanbieterliste von Tipps finden Sie unter [MSSQLTIPS list of SQL Server on Linux Tips (MSSQLTIPS-Liste von Tipps für SQL Server für Linux)](https://www.mssqltips.com/sql-server-tip-category/226/sql-server-on-linux/).

## <a name="licensing"></a>Lizenzierung

1. **Wie funktioniert die Lizenzierung unter Linux?**

   SQL Server wird unter Linux auf gleiche Weise wie unter Windows lizenziert. Tatsächlich lizenzieren Sie zunächst SQL Server und wählen anschließend aus, für welche Plattform Sie diese Lizenz verwenden möchten. Weitere Informationen finden Sie unter [Lizenzierung von SQL Server](https://www.microsoft.com/sql-server/sql-server-2017-pricing).

1. **Welche SQL Server-Edition sollte ich auswählen, wenn ich SQL Server bereits erworben habe?**

   Wenn Sie den Befehl „mssql-conf setup“ ausführen, werden Ihnen die folgenden Optionen angezeigt:
   
   ```
   Choose an edition of SQL Server:
      1. Evaluation (free, no production use rights, 180-day limit)
      2. Developer (free, no production use rights)
      3. Express (free)
      4. Web (PAID)
      5. Standard (PAID)
      6. Enterprise (PAID)
      7. Enterprise Core (PAID)
      8. I bought a license through a retail sales channel and have a product key to enter.
   ```
     
   Wenn Sie Ihre Lizenz im Rahmen der Volumenlizenzierung eines Enterprise Agreements oder über Ihr MSDN-Abonnement erhalten haben, müssen Sie die Optionen 4 bis 7 auswählen. In diesem Schritt werden Sie nicht dazu aufgefordert, Ihre Lizenz einzugeben, jedoch müssen Sie die entsprechende Lizenz für Ihre Konfiguration bereits erworben haben. Wenn Sie die Standard Edition über einen Retail Channel erworben haben, wählen Sie die Option 8 aus. Bei dieser Option werden Sie dazu aufgefordert einen Schlüssel einzugeben. 

1. **Wie kann ich die installierte Version und Edition von SQL Server für Linux überprüfen?**

   Stellen Sie eine Verbindung mit der SQL Server-Instanz mithilfe eines Clienttools wie **sqlcmd**, **mssql-cli** oder Visual Studio Code her. Führen Sie anschließend die folgende Transact-SQL-Abfrage aus, um die Version und Edition von SQL Server zu überprüfen, die Sie ausführen: 

   ```sql
   SELECT @@VERSION
   SELECT SERVERPROPERTY('Edition')
   ```

## <a name="installation"></a>Installation

1. **Wie installiere ich SQL Server auf meinen Linux-Servern?**

   Microsoft verwaltet Paketrepositorys zum Installieren von SQL Server und unterstützt die Installation mithilfe nativer Paket-Manager wie YUM, Zypper und APT. Informationen zur schnellen Installation finden Sie in einem der [Schnellstarts](sql-server-linux-setup.md#platforms).

1. **Kann ich SQL Server auf Linux-Subsystemen für Windows 10 installieren?**

   Nein. Linux unter Windows 10 wird derzeit nicht als Plattform für SQL Server und verwandte Tools unterstützt.

1. **Welche Linux-Dateisysteme kann SQL Server für Datendateien verwenden?**

   Derzeit unterstützt SQL Server für Linux EXT4 und XFS. Die Unterstützung für andere Dateisysteme wird bei Bedarf in Zukunft hinzugefügt.

1. **Kann ich die Installationspakete herunterladen, um SQL Server offline zu installieren?**

   Ja. Weitere Informationen hierzu finden Sie bei den Paketdownloadlinks in den [Versionshinweisen](sql-server-linux-release-notes.md). Sehen Sie sich außerdem die [Anweisungen für offline Installationen](sql-server-linux-setup.md#offline) an.

1. **Kann ich SQL Server für Linux unbeaufsichtigt installieren?**

   Ja. In der [Installationsanleitung für SQL Server für Linux](sql-server-linux-setup.md#unattended) finden Sie eine Erläuterung der unbeaufsichtigten Installation. Sehen Sie sich die Beispielskripts für [Red Hat](sample-unattended-install-redhat.md), [SUSE Linux Enterprise Server](sample-unattended-install-suse.md) und [Ubuntu](sample-unattended-install-ubuntu.md) an. Sie können auch [dieses Beispielskript](/archive/blogs/sqlcat/unattended-install-and-configuration-for-sql-server-2017-on-linux) überprüfen, das vom SQL Server-Kundenberatungsteam erstellt wurde.

## <a name="tools"></a>Tools

1. **Kann ich den SQL Server Management Studio-Client unter Windows verwenden, um auf SQL Server für Linux zuzugreifen?**

   Ja, Sie können all Ihre vorhandenen Tools verwenden, die unter Windows funktionieren, um auf SQL Server für Linux zuzugreifen. Dazu gehören Microsoft-Tools wie SSMS (SQL Server Management Studio), SSDT (SQL Server Data Tools) und OSS sowie Tools von Drittanbietern.

1. **Gibt es ein Tool wie SSMS für Linux?**

   Das neue Azure Data Studio ist ein plattformübergreifendes Tool zum Verwalten von SQL Server. Weitere Informationen finden Sie unter [What is Azure Data Studio (Was ist Azure Data Studio)](../azure-data-studio/what-is-azure-data-studio.md).

1. **Sind Befehle wie „sqlcmd“ und „bcp“ unter Linux verfügbar?**

   Ja, [sqlcmd und bcp](sql-server-linux-setup-tools.md) sind unter Linux, macOS und Windows nativ verfügbar. Zusätzlich können Sie das neue Befehlszeilentool [mssql-scripter](https://github.com/Microsoft/mssql-scripter) unter Linux, macOS oder Windows verwenden, um T-SQL-Skripts für Ihre SQL-Datenbank zu generieren, die an einem beliebigen Ort ausgeführt wird. Sehen Sie sich auch die Vorschauversion von [mssql-cli](https://blogs.technet.microsoft.com/dataplatforminsider/2017/12/12/try-mssql-cli-a-new-interactive-command-line-tool-for-sql-server/) an.

1. **Ist es möglich, den Aktivitätsmonitor bei einer Verbindung über SSMS unter Windows für eine Instanz unter Linux anzuzeigen?**

   Ja, Sie können SSMS unter Windows verwenden, um eine Remoteverbindung herzustellen und um Tools und Features wie Aktivitätsmonitorbefehle in einer Linux-Instanz auszuführen.

1. **Welche Tools stehen zur Überwachung der Leistung von SQL Server für Linux zur Verfügung?**

   Sie können [dynamische Systemverwaltungssichten](../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md) verwenden, um verschiedene Typen von Informationen über SQL Server, einschließlich Informationen über Linux-Prozesse, zu erfassen. Sie können [Abfragedatenspeicher](../relational-databases/performance/monitoring-performance-by-using-the-query-store.md) zur Verbesserung der Abfrageleistung verwenden. Andere Tools, z. B. das integrierte [Leistungsdashboard](/archive/blogs/sql_server_team/new-in-ssms-performance-dashboard-built-in), funktionieren remote in SSMS unter Windows.

   > [!TIP]
   > Eine Möglichkeit zur Verbesserung der Leistung besteht darin, Ihr Linux-Betriebssystem und die SQL Server-Instanz ordnungsgemäß zu konfigurieren. Weitere Informationen finden Sie unter [Bewährte Methoden für die Leistung und Konfigurationsrichtlinien für SQL Server für Linux](sql-server-linux-performance-best-practices.md).

## <a name="administration"></a>Verwaltung

1. **Hat Microsoft eine App wie SQL Server-Konfigurations-Manager für Linux erstellt?**

   Ja, es gibt ein Konfigurationstool für SQL Server für Linux: [mssql-conf](sql-server-linux-configure-mssql-conf.md).

1. **Unterstützt SQL Server für Linux mehrere Instanzen auf demselben Host?**

   Es wird empfohlen, mehrere Container auf einem Host auszuführen, um mehrere verschiedene Instanzen zu betreiben. Dies kann mühelos mit Docker erreicht werden, jedoch muss jeder Container an einem anderen Port lauschen. Weitere Informationen finden Sie unter [Run multiple SQL Server containers (Ausführen mehrerer SQL Server-Container)](./sql-server-linux-docker-container-deployment.md#multiple).

1. **Wird die Active Directory-Authentifizierung unter Linux unterstützt?**

   Ja. Weitere Informationen finden Sie unter [Active Directory Authentication with SQL Server on Linux (Active Directory-Authentifizierung mit SQL Server für Linux)](sql-server-linux-active-directory-authentication.md).

1. **Werden Always On und Clustering unter Linux unterstützt?**

   Failoverclustering und Hochverfügbarkeit werden unter Linux mithilfe von Pacemaker erzielt. Weitere Informationen hierzu finden Sie unter [Business continuity and database recovery - SQL Server on Linux (SQL Server für Linux: Geschäftskontinuität und Datenbankwiederherstellung)](sql-server-linux-business-continuity-dr.md).

1. **Ist es möglich, die Replikation aus Linux in Windows und umgekehrt zu konfigurieren?**

   Leseskalierungsreplikate können für die unidirektional Datenreplikation zwischen Windows und Linux verwendet werden.

1. **Ist es möglich, vorhandene Datenbanken in ältere Versionen von SQL Server zwischen Windows und Linux zu migrieren?**

   Ja, es gibt [mehrere Methoden](sql-server-linux-migrate-overview.md), mit denen dies erreicht werden kann.

1. **Kann ich meine Daten von Oracle und anderen Datenbank-Engines zu SQL Server für Linux migrieren?**

   Ja. SSMA unterstützt die Migration aus mehreren Typen von Datenbank-Engines: Microsoft Access, DB2, MySQL, Oracle und SAP ASE (ehemals SAP Sybase ASE). Ein Beispiel zur Verwendung von SSMA finden Sie unter [Migrate an Oracle schema to SQL Server on Linux with the SQL Server Migration Assistant (Migrieren eines Oracle-Schemas zu SQL Server für Linux mithilfe von SQL Server Migration Assistant)](../ssma/oracle/sql-server-linux-convert-from-oracle.md?toc=/sql/toc/toc.json).

1. **Welche Berechtigungen sind für SQL Server-Dateien erforderlich?**

   Alle Dateien im `/var/opt/mssql`-Dateiordner sollten sich im Besitz des **mssql**-Benutzers befinden und der **mssql**-Gruppe angehören. Sowohl der **mssql**-Benutzer als auch die -Gruppe sollte über Lese- und Schreibberechtigungen für alle Dateien und Verzeichnisse verfügen. Beachten Sie die folgenden Szenarios im Zusammenhang mit Datei- und Verzeichnisberechtigungen:

   * Berechtigungen für den mssql-Besitzer und die -Gruppe sind für eingebettete Netzwerkfreigaben erforderlich, die zum Speichern von SQL Server-Dateien verwendet werden.
   * Wenn Sie Datenbankdateien oder Sicherungen in einem nicht standardmäßigen Verzeichnis finden, müssen Sie die Berechtigungen auch für dieses Verzeichnis festlegen.
   * Wenn Sie den umask-Wert „0022“ des Standardstammverzeichnisses ändern, schlägt die SQL Server-Konfiguration nach der Installation fehl. Sie müssen dann alle erforderlichen Berechtigungen manuell für das SQL Server-Startkonto einrichten.

1. **Kann ich den Besitz der SQL Server-Dateien und -Verzeichnisse des installierten mssql-Kontos und der -Gruppe ändern?**

   Das Ändern des Besitzes des SQL Server-Verzeichnisses und der -Dateien der Standardinstallation wird nicht unterstützt. Das mssql-Konto und die -Gruppe werden spezifisch für SQL Server verwendet und verfügen über keinen interaktiven Anmeldezugriff.
   
 1. **Werden symbolische Links für Daten- und Protokollverzeichnisse von SQL Server unterstützt?** 
    
    Nein, symbolische Links für Daten- und Protokollverzeichnisse von SQL Server werden nicht unterstützt. Informationen zum Ändern der Daten- und Protokollverzeichnisse finden Sie unter [Ändern des Standardspeicherorts für das Verzeichnis der Datenbank- oder Protokolldateien](sql-server-linux-configure-mssql-conf.md#datadir).
    
[!INCLUDE[Get Help Options](../includes/paragraph-content/get-help-options.md)]