---
title: SQLCMD-Hilfsprogramm
description: Mit dem Hilfsprogramm „sqlcmd“ können Sie Transact-SQL-Anweisungen, Systemprozeduren und Skriptdateien in verschiedenen Modi eingeben. Es verwendet zudem ODBC zum Ausführen von Transact-SQL-Batches.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- statements [SQL Server], command prompt
- QUIT command
- Transact-SQL statements, command prompt
- EXIT command
- sqlcmd commands
- ED command
- sqlcmd utility
- command prompt utilities [SQL Server], sqlcmd
- '!! command'
- stored procedures [SQL Server], command prompt
- system stored procedures [SQL Server], command prompt
- sqlcmd utility, about sqlcmd utility
- scripts [SQL Server], command prompt
- RESET command
- GO command
ms.assetid: e1728707-5215-4c04-8320-e36f161b834a
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 02/24/2021
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017'
ms.openlocfilehash: 1b7d902064ef7a083b706af08730e91319be312f
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101836835"
---
# <a name="sqlcmd-utility"></a>SQLCMD-Hilfsprogramm

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

> Informationen zu SQL Server 2014 und niedrigeren Versionen finden Sie unter [sqlcmd-Hilfsprogramm](/previous-versions/sql/2014/tools/sqlcmd-utility?view=sql-server-2014&preserve-view=true).
>
> Informationen zur Verwendung von sqlcmd unter Linux finden Sie unter [Installieren von sqlcmd und bcp unter Linux](../linux/sql-server-linux-setup-tools.md).

 Mit dem Hilfsprogramm **sqlcmd** können Sie Transact-SQL-Anweisungen, Systemprozeduren und Skriptdateien in verschiedenen Modi eingeben:

- An einer Eingabeaufforderung.
- Im **Abfrage-Editor** im SQLCMD-Modus.
- In einer Windows-Skriptdatei.
- In einem Auftragsschritt eines SQL Server-Agent-Auftrags für ein Betriebssystem (cmd.exe).

Das Hilfsprogramm verwendet ODBC zum Ausführen von Transact-SQL-Batches.

## <a name="download-the-latest-version-of-sqlcmd-utility"></a>Herunterladen der aktuellen Version des sqlcmd-Hilfsprogramms

**[![Download sqlcmd for x64](../ssdt/media/download.png) Microsoft-Befehlszeilen-Hilfsprogramme 15 für SQL Server (x64) herunterladen (2,6 MB)](https://go.microsoft.com/fwlink/?linkid=2142258)**
<br>**[![Download sqlcmd for x86](../ssdt/media/download.png) Microsoft-Befehlszeilen-Hilfsprogramme 15 für SQL Server (x86) herunterladen (2,3 MB)](https://go.microsoft.com/fwlink/?linkid=2142257)**

Die Befehlszeilentools sind allgemein verfügbar (GA-Version), sie werden jedoch mit dem Installationspaket für [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] veröffentlicht.

**Versionsinformationen**

Releasenummer: 15.0.2<br>
Buildnummer: 15.0.2000.5<br>
Veröffentlichungsdatum: 11. September 2020

Die neue Version von SQLCMD unterstützt die Azure AD-Authentifizierung, einschließlich der Multi-Factor Authentication-Unterstützung (MFA) für SQL-Datenbank, Azure Synapse Analytics und Always Encrypted-Features.
Die neue BCP unterstützt die Azure AD-Authentifizierung, einschließlich der Multi-Factor Authentication-Unterstützung (MFA) für SQL-Datenbank und Azure Synapse Analytics.

**Systemanforderungen:** Windows 10, Windows 7, Windows 8, Windows 8.1, Windows Server 2008 – 2019

Für diese Komponente sind sowohl [Windows Installer 4.5](https://windows-installer.soft32.com/) als auch [Microsoft ODBC Driver 17 for SQL Server](../connect/odbc/download-odbc-driver-for-sql-server.md) erforderlich.
 
Führen Sie zum Überprüfen der SQLCMD-Version den Befehl `sqlcmd -?` aus, und vergewissern Sie sich, dass Version 15.0.2000.5 oder höher verwendet wird.

> [!NOTE]
> Sie benötigen mindestens Version 13.1 zur Unterstützung von Always Encrypted (`-g`) und Azure Active Directory-Authentifizierung (`-G`). (Möglicherweise haben Sie mehrere Versionen von „sqlcmd.exe“ auf Ihrem Computer installiert. Achten Sie darauf, dass Sie die richtige Version verwenden. Um die Version zu bestimmen, führen Sie `sqlcmd -?`aus.)

Sie können das sqlcmd-Hilfsprogramm über die Azure Cloud Shell testen, da diese standardmäßig vorinstalliert ist: [![Starten von Cloud Shell](https://shell.azure.com/images/launchcloudshell.png "Cloud Shell starten")](https://shell.azure.com)

Um sqlcmd-Anweisungen in SSMS auszuführen, wählen Sie in der Navigations-Dropdownliste oben im Menü „Abfrage“ den Befehl „SQLCMD-Modus“ aus.  

> [!IMPORTANT]
> [!INCLUDE[ssManStudioFull_md](../includes/ssmanstudiofull-md.md)] (SSMS) verwendet den Microsoft [!INCLUDE[dnprdnshort_md](../includes/dnprdnshort-md.md)]-SqlClient für die Ausführung im regulären Modus und im SQLCMD-Modus des **Abfrage-Editors**. Beim Ausführen von **sqlcmd** über die Befehlszeile verwendet **sqlcmd** den ODBC-Treiber. Da unterschiedliche Standardoptionen gelten können, führt die Ausführung derselben Abfrage im SQLCMD-Modus von [!INCLUDE[ssManStudioFull_md](../includes/ssmanstudiofull-md.md)] und im Hilfsprogramm **sqlcmd** möglicherweise zu unterschiedlichen Ergebnissen.  
>

Aktuell ist für **sqlcmd** kein Leerzeichen zwischen der Befehlszeilenoption und dem Wert erforderlich. In einer zukünftigen Version kann jedoch ein Leerzeichen zwischen der Befehlszeilenoption und dem Wert erforderlich sein.  

 Weitere Themen: 

- [Starten des Hilfsprogramms „sqlcmd“](../ssms/scripting/sqlcmd-start-the-utility.md)
- [Verwenden des Hilfsprogramms sqlcmd](../ssms/scripting/sqlcmd-use-the-utility.md)
  
## <a name="syntax"></a>Syntax

```cmd
sqlcmd
   -a packet_size
   -A (dedicated administrator connection)
   -b (terminate batch job if there is an error)
   -c batch_terminator
   -C (trust the server certificate)
   -d db_name
   -D
   -e (echo input)
   -E (use trusted connection)
   -f codepage | i:codepage[,o:codepage] | o:codepage[,i:codepage]
   -g (enable column encryption)
   -G (use Azure Active Directory for authentication)
   -h rows_per_header
   -H workstation_name
   -i input_file
   -I (enable quoted identifiers)
   -j (Print raw error messages)
   -k[1 | 2] (remove or replace control characters)  
   -K application_intent  
   -l login_timeout  
   -L[c] (list servers, optional clean output)  
   -m error_level  
   -M multisubnet_failover  
   -N (encrypt connection)  
   -o output_file  
   -p[1] (print statistics, optional colon format)  
   -P password  
   -q "cmdline query"  
   -Q "cmdline query" (and exit)  
   -r[0 | 1] (msgs to stderr)  
   -R (use client regional settings)  
   -s col_separator  
   -S [protocol:]server[instance_name][,port]  
   -t query_timeout  
   -u (unicode output file)  
   -U login_id  
   -v var = "value"
   -V error_severity_level
   -w column_width
   -W (remove trailing spaces)
   -x (disable variable substitution)
   -X[1] (disable commands, startup script, environment variables, optional exit)
   -y variable_length_type_display_width
   -Y fixed_length_type_display_width
   -z new_password
   -Z new_password (and exit)
   -? (usage)
```
  
## <a name="command-line-options"></a>Befehlszeilenoptionen

**Anmeldungsvorgänge**

**-A**  
Bewirkt die Anmeldung bei SQL Server über eine dedizierte Administratorverbindung (DAC). Diese Verbindungsart wird verwendet, um Serverprobleme zu behandeln. Diese Verbindung funktioniert nur bei Servercomputern, die DAC unterstützen. Wenn DAC nicht verfügbar ist, generiert **sqlcmd** eine Fehlermeldung und wird dann beendet. Weitere Informationen zu DAC finden Sie unter [Diagnoseverbindung für Datenbankadministratoren](../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md). Die Option „-A“ wird mit der Option „-G“ nicht unterstützt. Wenn Sie mithilfe der Option „-A“ eine Verbindung mit SQL-Datenbank herstellen möchten, müssen Sie SQL-Serveradministrator sein. DAC ist für Azure Active Directory-Administratoren nicht verfügbar.

**-C**  
Dieser Schalter wird vom Client verwendet, um ihn so zu konfigurieren, dass er dem Serverzertifikat ohne Überprüfung implizit vertraut. Diese Option entspricht der ADO.NET-Option `TRUSTSERVERCERTIFICATE = true`.  

**-d** _Datenbankname_  
Gibt beim Starten von **sqlcmd** eine `USE` *Datenbankname*-Anweisung aus. Durch diese Option wird die **sqlcmd** -Skriptvariable SQLCMDDBNAME festgelegt. Dieser Parameter gibt die Ausgangsdatenbank an. Der Standardwert ist die Standarddatenbank-Eigenschaft der Anmelde-ID. Wenn die Datenbank nicht vorhanden ist, wird eine Fehlermeldung generiert und **sqlcmd** beendet.  

**-D**  
Interpretiert den an -S übergebenen Servernamen als einen DSN statt als Hostnamen. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit sqlcmd](../connect/odbc/linux-mac/connecting-with-sqlcmd.md) im Abschnitt _DSN-Unterstützung in sqlcmd und bcp_.

> [!NOTE]
> Die Option „-D“ ist nur für Linux- und MacOS-Clients verfügbar. Auf Windows-Clients wurde damit zuvor eine jetzt veraltete Option bezeichnet, die entfernt wurde und ignoriert wird.

**-l** _Anmeldetimeout_  
Gibt an, wie viele Sekunden bei der Herstellung einer Verbindung mit einem Server verstreichen dürfen, bevor für eine **sqlcmd** -Anmeldung beim ODBC-Treiber ein Timeout eintritt. Durch diese Option wird die **sqlcmd** -Skriptvariable SQLCMDLOGINTIMEOUT festgelegt. Der Standardwert für das Timeout einer **sqlcmd** -Anmeldung beträgt acht Sekunden. Wenn Sie die **-G** -Option zum Herstellen einer Verbindung mit SQL-Datenbank oder Azure Synapse Analytics und zur Authentifizierung mithilfe von Azure Active Directory verwenden, empfiehlt sich ein Timeoutwert von mindestens 30 Sekunden. Der Timeoutwert für den Anmeldungszeitraum muss eine Zahl zwischen 0 und 65534 sein. Wenn der angegebene Wert kein numerischer Wert ist oder außerhalb dieses Bereichs liegt, generiert **sqlcmd** eine Fehlermeldung. Mit dem Wert 0 wird eine unbegrenzte Wartezeit festgelegt.

**-E**  
Verwendet eine vertrauenswürdige Verbindung anstelle eines Benutzernamens und eines Kennworts für die Anmeldung bei SQL Server. Standardmäßig wird von **sqlcmd** die vertrauenswürdige Verbindung verwendet, wenn **-E** nicht angegeben ist.  

Die Option **-E** ignoriert mögliche Umgebungsvariableneinstellungen für Benutzernamen und Kennwörter, z.B. SQLCMDPASSWORD. Wird die Option **-E** zusammen mit der Option **-U** oder der Option **-P** verwendet, wird eine Fehlermeldung generiert.  

**-g**  
Legt „Column Encryption Setting“ auf `Enabled`fest. Weitere Informationen hierzu finden Sie unter [Always Encrypted](../relational-databases/security/encryption/always-encrypted-database-engine.md). Es werden nur Hauptschlüssel unterstützt, die im Windows-Zertifikatspeicher gespeichert sind. Der Schalter -g erfordert mindestens **sqlcmd** Version [13.1](https://go.microsoft.com/fwlink/?LinkID=825643). Um die Version zu bestimmen, führen Sie `sqlcmd -?`aus.

**-G**  
Diese Option wird vom Client beim Herstellen einer Verbindung mit SQL-Datenbank oder Azure Synapse Analytics verwendet, um anzugeben, dass der Benutzer mithilfe der Azure Active Directory-Authentifizierung authentifiziert werden soll. Durch diese Option wird die **sqlcmd** -Skriptvariable „SQLCMDUSEAAD = true“ festgelegt. Der Schalter -G erfordert mindestens **sqlcmd** Version [13.1](https://go.microsoft.com/fwlink/?LinkID=825643). Um die Version zu bestimmen, führen Sie `sqlcmd -?`aus. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit SQL-Datenbank oder Azure Synapse Analytics unter Verwendung der Azure Active Directory-Authentifizierung](/azure/azure-sql/database/authentication-aad-overview). Die Option „-A“ wird mit der Option „-G“ nicht unterstützt.

> [!IMPORTANT]
> Die Option `-G` gilt nur für Azure SQL-Datenbank und Azure Synapse Analytics.
> Die interaktive AAD-Authentifizierung wird unter Linux oder macOS derzeit nicht unterstützt. Die integrierte AAD-Authentifizierung erfordert [Microsoft ODBC-Treiber 17 für SQL Server](../connect/odbc/download-odbc-driver-for-sql-server.md) ab Version 17.6.1 oder höher und eine ordnungsgemäß [konfigurierte Kerberos-Umgebung](../connect/odbc/linux-mac/using-integrated-authentication#configure-kerberos).

- **Azure Active Directory-Benutzername und -Kennwort:** 

    Wenn Sie einen Azure Active Directory-Benutzernamen und das zugehörige Kennwort verwenden möchten, geben Sie die Option **-G** zusammen mit dem Benutzernamen und dem Kennwort an, indem Sie die Optionen **-U** und **-P** bereitstellen.

    ```cmd
    Sqlcmd -S testsrv.database.windows.net -d Target_DB_or_DW -U bob@contoso.com -P MyAADPassword -G
    ```

    Der Parameter „-G“ generiert die folgende Verbindungszeichenfolge im Back-End: 

    ```cmd
     SERVER = Target_DB_or_DW.testsrv.database.windows.net;UID= bob@contoso.com;PWD=MyAADPassword;AUTHENTICATION = ActiveDirectoryPassword 
    ```

- **Integrierte Azure Active Directory-Authentifizierung**

   Wenn Sie die integrierte Azure Active Directory-Authentifizierung verwenden möchten, geben Sie die Option **-G** ohne Benutzername und Kennwort an.
   *Die integrierte AAD-Authentifizierung erfordert[ Microsoft ODBC-Treiber 17 für SQL Server](../connect/odbc/download-odbc-driver-for-sql-server.md) ab Version 17.6.1 und eine[ ordnungsgemäß](../connect/odbc/linux-mac/using-integrated-authentication.md#configure-kerberos)konfigurierte Kerberos-Umgebung*.

    ```cmd
    Sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net -G
    ```

    Dadurch wird die folgende Verbindungszeichenfolge im Back-End generiert: 

    ```cmd
    SERVER = Target_DB_or_DW.testsrv.database.windows.net Authentication = ActiveDirectoryIntegrated; Trusted_Connection=NO
    ```

    > [!NOTE]
    > Die `-E`-Option (Trusted_Connection) kann nicht zusammen mit der `-G`-Option verwendet werden.

- **Azure Active Directory Interactive**

    Durch die interaktive Azure AD-Authentifizierung für Azure SQL-Datenbank und Azure Synapse Analytics können Sie eine interaktive Methode verwenden, die die mehrstufige Authentifizierung unterstützt. Weitere Informationen finden Sie unter [Interaktive Active Directory-Authentifizierung](../ssdt/azure-active-directory.md#active-directory-interactive-authentication). 

   Für die interaktive Azure AD-Authentifizierung sind **sqlcmd** in [Version 15.0.1000.34](#download-the-latest-version-of-sqlcmd-utility) oder höher und [ODBC in Version 17.2 oder höher](../connect/odbc/download-odbc-driver-for-sql-server.md) erforderlich.  

   Geben Sie zum Aktivieren der interaktiven Authentifizierung die Option „-G“ nur mit dem Benutzernamen (-U) und ohne ein Kennwort an.

   Im folgenden Beispiel werden Daten mithilfe des interaktiven Azure AD-Modus exportiert. Hierbei wird ein Benutzername angegeben, der ein AAD-Konto darstellt. Dies ist das gleiche Beispiel, das im vorherigen Abschnitt verwendet wurde: *Azure Active Directory-Benutzername und -Kennwort*.  

   Im interaktiven Modus muss ein Kennwort manuell eingegeben werden. Bei Konten mit mehrstufiger Authentifizierung müssen Sie Ihre konfigurierte MFA-Authentifizierungsmethode vervollständigen.

   ```cmd
   sqlcmd -S testsrv.database.windows.net -d Target_DB_or_DW -G -U alice@aadtest.onmicrosoft.com
   ```

   Der vorherige Befehl generiert die folgende Verbindungszeichenfolge im Back-End:  

   ```cmd
   SERVER = Target_DB_or_DW.testsrv.database.windows.net;UID=alice@aadtest.onmicrosoft.com; AUTHENTICATION = ActiveDirectoryInteractive   
   ```

   Für den Fall, dass ein Azure AD-Benutzer auch ein Benutzer eines Domänenverbunds ist und ein Windows-Konto verwendet, enthält der in der Befehlszeile erforderliche Benutzername dessen Domänenkonto (Beispiel joe@contoso.com unten):

   ```cmd
   sqlcmd -S testsrv.database.windows.net -d Target_DB_or_DW -G -U joe@contoso.com  
   ```

   Der Gastbenutzeralias wird verwendet, wenn Gastbenutzer in einer bestimmten Azure AD-Instanz vorhanden sind und zu einer Gruppe gehören, die in SQL-Datenbank vorhanden ist und über Datenbankberechtigungen zum Ausführen des sqlcmd-Befehls verfügt (z. B. *keith0@adventureworks.com* ).

  >[!IMPORTANT]
  >Bei Verwendung der Optionen `-G` und `-U` mit SQLCMD gibt es ein bekanntes Problem: Wenn die Option `-U` vor die Option `-G` gesetzt wird, kann bei der Authentifizierung ein Fehler auftreten. Verwenden Sie die Option `-G` immer vor der Option `-U`.

**-H** _Arbeitsstationsname_  
 Der Name einer Arbeitsstation. Durch diese Option wird die **sqlcmd** -Skriptvariable SQLCMDWORKSTATION festgelegt. Der Name der Arbeitsstation wird in der **hostname** -Spalte der **sys.sysprocesses** -Katalogsicht aufgeführt, und der Name kann mithilfe der gespeicherten Prozedur **sp_who** zurückgegeben werden. Wenn diese Option nicht angegeben ist, wird standardmäßig der Name des aktuellen Computers angenommen. Dieser Name kann verwendet werden, um verschiedene 

**sqlcmd**-Sitzungen zu identifizieren.  

**-j** Zeigt unformatierte Fehlermeldungen auf dem Bildschirm an.
  
 **-K** _Anwendungsabsicht_  
 Deklariert den Arbeitsauslastungstyp der Anwendung beim Herstellen einer Verbindung mit einem Server. Der einzige derzeit unterstützte Wert ist **ReadOnly**. Wenn **-K** nicht angegeben ist, unterstützt das sqlcmd-Hilfsprogramm keine Konnektivität mit einem sekundären Replikat in einer AlwaysOn-Verfügbarkeitsgruppe. Weitere Informationen finden Sie unter [Aktive sekundäre Replikate: Lesbare sekundäre Replikate (Always On-Verfügbarkeitsgruppen)](../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)  
  
**-M** _Multisubnetzfailover_  
 Geben Sie immer **-M** an, wenn Sie eine Verbindung mit dem Verfügbarkeitsgruppenlistener einer SQL Server-Verfügbarkeitsgruppe oder einer SQL Server-Failoverclusterinstanz herstellen. **-M** gewährleistet eine schnellere Erkennung und Verbindung mit dem (gerade) aktiven Server. Wenn **-M** nicht angegeben ist, ist **-M** deaktiviert. Weitere Informationen finden Sie unter [Listener, Clientkonnektivität, Anwendungsfailover](../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md), [Erstellung und Konfiguration von Verfügbarkeitsgruppen &#40;SQL Server&#41;](../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md), [Failoverclustering und Always On-Verfügbarkeitsgruppen (SQL Server)](../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md) und [Aktive Sekundäre: Lesbare sekundäre Replikate (Always On-Verfügbarkeitsgruppen)](../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md).
  
 **-N**  
 Dieser Schalter wird vom Client verwendet, um eine verschlüsselte Verbindung anzufordern.  
  
 **-P** _Kennwort_  
 Ein vom Benutzer angegebenes Kennwort. Bei Kennwörtern wird nach Groß- und Kleinschreibung unterschieden. Wenn die Option -U verwendet wird, nicht aber die Option **-P** , und die SQLCMDPASSWORD-Umgebungsvariable nicht festgelegt wurde, wird der Benutzer von **sqlcmd** zur Angabe eines Kennworts aufgefordert. Die Verwendung des NULL-Kennworts wird nicht empfohlen, Sie können es jedoch angeben, indem Sie ein Paar zusammenhängender doppelter gerader Anführungszeichen oben für den Parameterwert verwenden:

- **-P ""**

Es wird empfohlen, ein sicheres Kennwort zu verwenden.

#### <a name="use-a-strong-password"></a>[**Verwenden Sie ein sicheres Kennwort!**](../relational-databases/security/strong-passwords.md)

 Die Aufforderung zur Eingabe des Kennworts wird folgendermaßen an der Konsole ausgegeben: `Password:`  
  
 Die Benutzereingabe bleibt verborgen, d. h. es erfolgt keine Anzeige, und der Cursor bleibt an der Anfangsposition.  
  
 Über die SQLCMDPASSWORD-Umgebungsvariable können Sie ein Standardkennwort für die aktuelle Sitzung festlegen. Aus diesem Grund ist die Hartcodierung von Kennwörtern in Batchdateien nicht notwendig.  
  
 Im folgenden Beispiel wird zuerst die SQLCMDPASSWORD-Variable an der Eingabeaufforderung festgelegt und dann auf das Hilfsprogramm **sqlcmd** zugegriffen. Geben Sie an der Eingabeaufforderung Folgendes ein:  
  
 `SET SQLCMDPASSWORD= p@a$$w0rd`  
 Geben Sie Folgendes an der daraufhin angezeigten Eingabeaufforderung ein:  
  
 `sqlcmd`  
  
 Wenn die Kombination aus Benutzername und Kennwort falsch ist, wird eine Fehlermeldung generiert.  
  
**HINWEIS!**  Die OSQLPASSWORD-Umgebungsvariable wurde aus Gründen der Abwärtskompatibilität beibehalten. Die SQLCMDPASSWORD-Umgebungsvariable hat Vorrang vor der OSQLPASSWORD-Umgebungsvariable. Da OSQLPASSWORD nicht mehr freigegeben ist, können die Hilfsprogramme **sqlcmd** und **osql** ohne Störungen nebeneinander verwendet werden. Vorhandene Skripts funktionieren weiterhin.  
  
 Wird die Option **-P** zusammen mit der Option **-E** verwendet, wird eine Fehlermeldung generiert.  
  
 Werden nach der Option **-P** mehrere Argumente angegeben, wird eine Fehlermeldung generiert und das Programm beendet.  
  
 **-S** [*protocol*:]*server*[ **\\** _instance\_name_][ **,** _port_]  
 Gibt die SQL Server-Instanz an, mit der eine Verbindung hergestellt werden soll. Durch die Option wird die **sqlcmd** -Skriptvariable SQLCMDSERVER festgelegt.  
  
 Geben Sie *server_name* an, um eine Verbindung mit der Standardinstanz von SQL Server auf diesem Servercomputer herzustellen. Geben Sie *server_name* [ **\\** _instance\_name_ ] an, um eine Verbindung mit der benannten Instanz von SQL Server auf diesem Servercomputer herzustellen. Wenn kein Servercomputer angegeben wird, stellt **sqlcmd** eine Verbindung mit der Standardinstanz von SQL Server auf dem lokalen Computer her. Diese Option ist erforderlich, wenn **sqlcmd** von einem Remotecomputer im Netzwerk ausgeführt wird.  
  
 *Protokoll* kann Folgendes sein: **tcp** (TCP/IP), **lpc** (Shared Memory) oder **np** (Named Pipes).  
  
 Wenn Sie *server_name* [ **\\** _instance\_name_ ] beim Starten von **sqlcmd** nicht angeben, sucht SQL Server nach der SQLCMDSERVER-Umgebungsvariable und verwendet diese.  
  
> [!NOTE]  
>  Die OSQLSERVER-Umgebungsvariable wurde aus Gründen der Abwärtskompatibilität beibehalten. Die SQLCMDSERVER-Umgebungsvariable hat Vorrang vor der OSQLSERVER-Umgebungsvariablen. Das bedeutet, dass **sqlcmd** und **osql** störungsfrei parallel verwendet werden können und dass alte Skripts weiterhin funktionsfähig sind.  
  
 **-U** _Anmelde-ID_  
 Dies ist der Anmeldename oder der für die eigenständige Datenbank verwendete Benutzername. Für Benutzer einer eigenständigen Datenbank müssen Sie die Option für den Datenbanknamen (-d) angeben.  
  
> [!NOTE]  
>  Die OSQLUSER-Umgebungsvariable steht aus Gründen der Abwärtskompatibilität zur Verfügung. Die SQLCMDUSER-Umgebungsvariable ist bezüglich der OSQLUSER-Umgebungsvariable vorrangig. Dies bedeutet, dass **sqlcmd** und **osql** störungsfrei parallel verwendet werden können. Es bedeutet ferner, dass vorhandene **osql** -Skripts weiterhin funktionieren.  
  
 Wenn weder die Option **-U** noch die Option **-P** angegeben wird, versucht **sqlcmd**, die Verbindung im Microsoft Windows-Authentifizierungsmodus herzustellen. Die Authentifizierung basiert auf dem Windows-Konto des Benutzers, der **sqlcmd** ausführt.  
  
 Wird die Option **-U** zusammen mit der Option **-E** verwendet (weiter unten in diesem Artikel beschrieben), wird eine Fehlermeldung generiert. Werden nach der Option **-U** mehrere Argumente angegeben, wird eine Fehlermeldung generiert und das Programm beendet.  
  
 **-z** _neues_Kennwort_  
 Kennwort ändern:  
  
 `sqlcmd -U someuser -P s0mep@ssword -z a_new_p@a$$w0rd`  
  
 **-Z** _neues_Kennwort_  
 Kennwort ändern und beenden:  
  
 `sqlcmd -U someuser -P s0mep@ssword -Z a_new_p@a$$w0rd`  
  
 **Eingabe-/Ausgabeoptionen**  
  **-f** _Codepage_ | **i:** _Codepage_[ **,o:** _Codepage_] | **o:** _Codepage_[ **,i:** _Codepage_]  
 Gibt die Eingabe- und Ausgabecodepages an. Die Codepagenummer ist ein numerischer Wert, der eine installierte Windows-Codepage angibt.  
  
 Regeln zum Konvertieren von Codepages:  
  
- Wenn keine Codepages angegeben sind, verwendet **sqlcmd** die aktuelle Codepage sowohl für Eingabe- als auch für Ausgabedateien, außer wenn es sich bei der Eingabedatei um eine Unicode-Datei handelt, da in diesem Fall keine Konvertierung erforderlich ist.  
  
- **sqlcmd** erkennt Unicode-Eingabedateien des Formats Big-Endian bzw. Little-Endian automatisch. Wenn die Option **-u** angegeben wurde, handelt es sich bei der Ausgabe stets um Unicode-Dateien des Formats Little-Endian.  
  
- Wenn keine Ausgabedatei angegeben wurde, handelt es sich bei der Ausgabe-Codepage um die Codepage der Konsole. Auf diese Weise kann die Ausgabe richtig auf der Konsole angezeigt werden.  
  
- Wenn mehrere Eingabedateien angegeben wurden, wird davon ausgegangen, dass sie dieselbe Codepage verwenden. Unicode- und Nicht-Unicode-Eingabedateien können gemischt verwendet werden.  
  
 Geben Sie an der Eingabeaufforderung **chcp** ein, um die Codepage von Cmd.exe zu überprüfen.  
  
 **-i** _Eingabedatei_[ **,** _Eingabedatei2_...]  
 Identifiziert die Datei, die einen Batch mit SQL-Anweisungen oder gespeicherten Prozeduren enthält. Sie können mehrere Dateien angeben, die der Reihe nach gelesen und verarbeitet werden. Verwenden Sie keine Leerzeichen zwischen Dateinamen. **sqlcmd** prüft zunächst, ob alle angegebenen Dateien vorhanden sind. Wenn eine oder mehrere Dateien nicht vorhanden sind, wird **sqlcmd** beendet. Die Optionen -i und -Q/-q schließen sich gegenseitig aus.  
  
 Beispiele für Pfade:  

```cmd
-i C:\<filename>  
-i \\<Server>\<Share$>\<filename>  
-i "C:\Some Folder\<file name>"  
```
  
 Dateipfade, die Leerzeichen enthalten, müssen in Anführungszeichen eingeschlossen werden.  
  
 Diese Option kann mehrmals verwendet werden: **-i**_Eingabedatei_ **-I**_I Eingabedatei._  
  
 **-o** _Ausgabedatei_  
 Identifiziert die Datei, die die Ausgabe von **sqlcmd** erhält.  
  
 Ist **-u** angegeben, wird die *Ausgabedatei* im Unicode-Format gespeichert. Wenn der Dateiname ungültig ist, wird eine Fehlermeldung generiert und **sqlcmd** beendet. **sqlcmd** unterstützt keine parallelen Schreibvorgänge mehrerer **sqlcmd** -Prozesse in die gleiche Datei. In diesem Fall wäre die Dateiausgabe beschädigt oder fehlerhaft. Informieren Sie sich auch über die Option **-f**, die für Dateiformate ebenfalls relevant ist. Falls sie noch nicht vorhanden ist, wird die Datei erstellt. Eine Datei mit demselben Namen aus einer früheren **sqlcmd** -Sitzung wird überschrieben. Bei der hier angegebenen Datei handelt es sich nicht um die **stdout** -Datei. Wenn eine **stdout**-Datei angegeben ist, wird diese Datei nicht verwendet.  
  
 Beispiele für Pfade:  

```cmd
-o C:< filename>  
-o \\<Server>\<Share$>\<filename>  
-o "C:\Some Folder\<file name>"  
 ```
 Dateipfade, die Leerzeichen enthalten, müssen in Anführungszeichen eingeschlossen werden.  
  
 **-r**[**0** | **1**]  
 Leitet die Ausgabe der Fehlermeldung auf den Bildschirm um (**stderr**). Wenn Sie keinen Parameter bzw. wenn Sie **0** angeben, werden nur Fehlermeldungen mit dem Schweregrad 11 oder höher umgeleitet. Wenn Sie **1** angeben, wird die gesamte Fehlermeldungsausgabe (einschließlich der Ausgabe von PRINT) umgeleitet. Dies hat keine Wirkung, wenn Sie die Option -o verwenden. Standardmäßig werden Meldungen an **stdout** gesendet.  
  
 **-R**  
 Bewirkt, dass **sqlcmd** aus SQL Server abgerufene numerische Spalten sowie Währungs-, Datums- und Zeitspalten basierend auf dem Gebietsschema des Clients lokalisiert. Standardmäßig werden diese Spalten entsprechend den regionalen Einstellungen des Servers angezeigt.  
  
 **-u**  
 Gibt an, dass *Ausgabedatei* unabhängig vom Format von *Eingabedatei* im Unicode-Format gespeichert wird.  
  
 **Abfrageausführungsoptionen**  
  **-e**  
 Schreibt Eingabeskripts in das Standardausgabegerät (**stdout**).  
  
 **-I**  
 Legt die SET QUOTED_IDENTIFIER-Verbindungsoption auf ON fest. Die Standardeinstellung ist OFF. Weitere Informationen finden Sie unter [SET QUOTED_IDENTIFIER &#40;Transact-SQL&#41;](~/t-sql/statements/set-quoted-identifier-transact-sql.md).  
  
 **-q "** _Befehlszeilenabfrage_ **"**  
 Führt eine Abfrage aus, wenn **sqlcmd** startet, beendet **sqlcmd** jedoch nicht, nachdem die Ausführung der Abfrage abgeschlossen ist. Es können mehrere durch Semikolons getrennte Abfragen ausgeführt werden. Schließen Sie die Abfrage in Anführungszeichen ein, wie im folgenden Beispiel gezeigt wird.  
  
 Geben Sie an der Eingabeaufforderung Folgendes ein:  
  
 `sqlcmd -d AdventureWorks2012 -q "SELECT FirstName, LastName FROM Person.Person WHERE LastName LIKE 'Whi%';"`  
  
 `sqlcmd -d AdventureWorks2012 -q "SELECT TOP 5 FirstName FROM Person.Person;SELECT TOP 5 LastName FROM Person.Person;"`  
  
> [!IMPORTANT]  
>  Verwenden Sie nicht das Abschlusszeichen GO in der Abfrage.  
  
 Wenn **-b** zusammen mit dieser Option angegeben wird, wird **sqlcmd** beim Auftreten eines Fehlers beendet. Der Schalter **-b** wird weiter unten in diesem Artikel beschrieben.  
  
 **-Q "** _Befehlszeilenabfrage_ **"**  
 Führt eine Abfrage aus, wenn **sqlcmd** startet, und beendet **sqlcmd** unmittelbar im Anschluss. Es können mehrere durch Semikolons getrennte Abfragen ausgeführt werden.  
  
 Schließen Sie die Abfrage in Anführungszeichen ein, wie im folgenden Beispiel gezeigt wird.  
  
 Geben Sie an der Eingabeaufforderung Folgendes ein:  
  
 `sqlcmd -d AdventureWorks2012 -Q "SELECT FirstName, LastName FROM Person.Person WHERE LastName LIKE 'Whi%';"`  
  
 `sqlcmd -d AdventureWorks2012 -Q "SELECT TOP 5 FirstName FROM Person.Person;SELECT TOP 5 LastName FROM Person.Person;"`  
  
> [!IMPORTANT]  
>  Verwenden Sie nicht das Abschlusszeichen GO in der Abfrage.  
  
 Wenn **-b** zusammen mit dieser Option angegeben wird, wird **sqlcmd** beim Auftreten eines Fehlers beendet. Der Schalter **-b** wird weiter unten in diesem Artikel beschrieben.  
  
 **-t** _Abfragetimeout_  
 Gibt an, wie viele Sekunden verstreichen, bevor für einen Befehl (oder eine SQL-Anweisung) ein Timeout eintritt. Durch diese Option wird die **sqlcmd** -Skriptvariable SQLCMDSTATTIMEOUT festgelegt. Wenn für *Abfragetimeout* kein Wert angegeben ist, tritt für den Befehl kein Timeout ein. Der Wert für *query* *time_out* muss eine Zahl zwischen 1 und 65534 sein. Wenn der angegebene Wert kein numerischer Wert ist oder außerhalb dieses Bereichs liegt, generiert **sqlcmd** eine Fehlermeldung.  
  
> [!NOTE]  
>  Der tatsächliche Timeoutwert kann einige Sekunden von dem für *Timeout* angegebenen Wert abweichen.  
  
 **-vvar =**  _Wert_[ **var =** _Wert_...]  
 Erstellt eine **sqlcmd**-Skriptvariable, die in einem **sqlcmd** -Skript verwendet werden kann. Setzen Sie den Wert in Anführungszeichen, falls er Leerzeichen enthält. Sie können mehrere Werte für _**var**_= **"** _values_ **"** angeben. Wenn einer der angegebenen Werte fehlerhaft ist, generiert **sqlcmd** eine Fehlermeldung und wird beendet.  
  
 `sqlcmd -v MyVar1=something MyVar2="some thing"`  
  
 `sqlcmd -v MyVar1=something -v MyVar2="some thing"`  
  
 **-x**  
 Bewirkt, dass **sqlcmd** Skriptvariablen ignoriert. Dieser Parameter erweist sich als nützlich, wenn ein Skript mehrere INSERT-Anweisungen enthält, die Zeichenfolgen im selben Format wie reguläre Variablen enthalten können, wie z.B. $(*Variablenname*).  
  
 **Formatierungsoptionen**  
  **-h** _Kopfzeilen_  
 Gibt die Anzahl der Zeilen an, die zwischen den Spaltenüberschriften ausgegeben werden. Standardmäßig werden die Überschriften für jedes Resultset der Abfrage einmal gedruckt. Durch diese Option wird die **sqlcmd** -Skriptvariable SQLCMDHEADERS festgelegt. Mit **-1** können Sie angeben, dass keine Überschriften ausgegeben werden sollen. Ein ungültiger Wert bewirkt, dass **sqlcmd** eine Fehlermeldung generiert und dann beendet wird.  
  
 **-k** [**1** | **2**]  
 Entfernt alle Steuerzeichen aus der Ausgabe, z. B. Tabstoppzeichen und Neue-Zeile-Zeichen. Mit diesem Parameter bleibt die Spaltenformatierung erhalten, wenn Daten zurückgegeben werden. Wenn 1 angegeben wird, werden die Steuerzeichen durch ein einzelnes Leerzeichen ersetzt. Wenn 2 angegeben wird, werden aufeinanderfolgende Steuerzeichen durch ein einzelnes Leerzeichen ersetzt. **-k** ist der Gleiche wie **-k1**.  
  
 **-s** _Spaltentrennzeichen_  
 Gibt das Spaltentrennzeichen an. Der Standardwert ist ein Leerzeichen. Durch diese Option wird die **sqlcmd** -Skriptvariable SQLCMDCOLSEP festgelegt. Um Zeichen verwenden zu können, die für das Betriebssystem eine besondere Bedeutung haben, z. B. das kaufmännische Und-Zeichen (&) oder das Semikolon (;), müssen Sie das Zeichen in Anführungszeichen (") einschließen. Das Spaltentrennzeichen kann ein beliebiges 8-Bit-Zeichen sein.  
  
 **-w** _Spaltenbreite_  
 Gibt die Bildschirmbreite für die Ausgabe an. Durch diese Option wird die **sqlcmd** -Skriptvariable SQLCMDCOLWIDTH festgelegt. Die Spaltenbreite muss eine Zahl größer als 8 und kleiner als 65536 sein. Wenn die angegebene Spaltenbreite außerhalb dieses Bereichs liegt, generiert **sqlcmd** eine Fehlermeldung. Die Standardbreite beträgt 80 Zeichen. Wenn eine Ausgabezeile die angegebene Spaltenbreite überschreitet, wird sie in die nächste Zeile umbrochen.  
  
 **-W**  
 Mit dieser Option werden nachfolgende Leerzeichen aus einer Spalte entfernt. Verwenden Sie diese Option zusammen mit der Option **-s** , um Daten vorzubereiten, die in eine andere Anwendung exportiert werden sollen. Diese Option kann nicht mit der Option **-y** oder **-Y** verwendet werden.  
  
 **-y** _Anzeigebreite_bei_Typen_mit_variabler_Länge_  
 Dadurch wird die **sqlcmd** -Skriptvariable `SQLCMDMAXVARTYPEWIDTH`festgelegt. Der Standardwert ist 256. Sie begrenzt die Anzahl der Zeichen, die für die großen Datentypen variabler Länge zurückgegeben werden:  
  
- **varchar(max)**  
  
- **nvarchar(max)**  
  
- **varbinary(max)**  
  
- **xml**  
  
- **UDT (benutzerdefinierte Datentypen)**  
  
- **text**  
  
- **ntext**  
  
- **image**  
  
> [!NOTE]  
>  UDTs können je nach Implementierung eine feste Länge aufweisen. Wenn die Länge eines UDT mit fester Länge den Wert für *Anzeigebreite* unterschreitet, hat dies keinen Einfluss auf den Wert des zurückgegebenen UDT. Wenn die Länge den Wert für *Anzeigebreite* jedoch überschreitet, wird die Ausgabe abgeschnitten.  
   
  
> [!IMPORTANT]  
>  Verwenden Sie die Option **-y 0** mit äußerster Sorgfalt, da sie je nach Größe der zurückgegebenen Daten zu ernsthaften Leistungsproblemen auf dem Server und im Netzwerk führen kann.  
  
 **-Y** _Anzeigebreite_bei_Typen_mit_fester_Länge_  
 Dadurch wird die **sqlcmd** -Skriptvariable `SQLCMDMAXFIXEDTYPEWIDTH`festgelegt. Der Standardwert ist 0 (unbegrenzt). Er begrenzt die Anzahl der zurückgegebenen Zeichen für die folgenden Datentypen:  
  
- **char(** _n_ **)** , wobei 1<=n<=8000  
  
- **nchar(n** _n_ **)** , wobei 1<=n<=4000  
  
- **varchar(n** _n_ **)** , wobei 1<=n<=8000  
  
- **nvarchar(n** _n_ **)** , wobei 1<=n<=4000  
  
- **varbinary(n** _n_ **)** , wobei 1<=n\<=4000  
  
- **variant**  
  
 **Optionen für die Fehlerberichterstellung**  
  **-b**  
 Gibt an, dass **sqlcmd** beendet und ein DOS ERRORLEVEL-Wert zurückgegeben wird, wenn ein Fehler auftritt. Für die DOS ERRORLEVEL-Variable wird der Wert **1** zurückgegeben, wenn der Schweregrad der SQL Server-Fehlermeldung größer als 10 ist. Andernfalls wird der Wert **0** zurückgegeben. Wenn die Option **-V** zusätzlich zu **-b** festgelegt wurde, meldet **sqlcmd** keinen Fehler, wenn der Schweregrad kleiner ist als die mithilfe von **-V** festgelegten Werte. Mit Eingabeaufforderungs-Batchdateien kann der Wert von ERRORLEVEL getestet und der Fehler entsprechend behoben werden. **sqlcmd** meldet keine Fehler für Schweregrad 10 (Informationsmeldungen).  
  
 Wenn das **sqlcmd** -Skript einen falschen Kommentar bzw. einen Syntaxfehler enthält oder eine Skriptvariable fehlt, wird der ERRORLEVEL-Wert 1 zurückgegeben.  
  
 **-m** _Fehlerebene_  
 Steuert, welche Fehlermeldungen an **stdout** gesendet werden. Fehlermeldungen mit einem Schweregrad, der größer oder gleich diesem Wert ist, werden gesendet. Wenn dieser Wert auf **-1** festgelegt ist, werden alle Meldungen gesendet, einschließlich der Informationsmeldungen. Es sind keine Leerzeichen zwischen **-m** und **-1** zulässig. Beispielsweise ist **-m-1** gültig, **-m -1** jedoch nicht.  
  
 Mit dieser Option wird außerdem die **sqlcmd** -Skriptvariable SQLCMDERRORLEVEL festgelegt. Diese Variable hat den Standardwert 0.  
  
 **-V** _Fehlerschweregrad_  
 Steuert den Schweregrad, der zur Festlegung der ERRORLEVEL-Variable verwendet wird. Für Fehlermeldungen mit einem Schweregrad, der größer oder gleich diesem Wert ist, wird ERRORLEVEL festgelegt. Werte kleiner 0 werden als 0 zurückgegeben. Batch- und CMD-Dateien können verwendet werden, um den Wert der ERRORLEVEL-Variable zu testen.  
  
 **Sonstige Optionen**  
  **-a** _Paketgröße_  
 Fordert ein Paket einer anderen Größe an. Durch diese Option wird die **sqlcmd** -Skriptvariable SQLCMDPACKETSIZE festgelegt. *packet_size* muss einen Wert zwischen 512 und 32767 haben. Der Standardwert lautet 4096. Ein höherer Wert für die Paketgröße kann das Leistungsverhalten beim Ausführen von Skripts verbessern, die zahlreiche SQL-Anweisungen zwischen GO-Befehlen aufweisen. Sie können eine größere Paketgröße anfordern. Wenn die Anforderung abgelehnt wird, verwendet **sqlcmd** jedoch den Standardwert des Servers für die Paketgröße.  
  
 **-c** _Batchterminator_  
 Gibt das Batchabschlusszeichen an. Standardmäßig werden Befehle abgeschlossen und an SQL Server gesendet, indem das Wort „GO“ in eine eigene Zeile eingegeben wird. Wenn Sie das Batchabschlusszeichen neu festlegen, dürfen Sie keine für Transact-SQL reservierten Schlüsselwörter oder Zeichen verwenden, die eine spezielle Bedeutung für das Betriebssystem haben, und zwar auch dann nicht, wenn vor dem Wort bzw. Zeichen ein umgekehrter Schrägstrich steht.  
  
 **-L**[**c**]  
 Listet die lokal konfigurierten Servercomputer sowie die Namen der Servercomputer auf, die Broadcastnachrichten über das Netzwerk senden. Dieser Parameter kann nicht in Verbindung mit anderen Parametern verwendet werden. Es können maximal 3.000 Servercomputer aufgelistet werden. Wenn die Serverliste aufgrund der Puffergröße abgeschnitten wurde, wird eine Warnmeldung angezeigt.  
  
> [!NOTE]  
>  Aufgrund der Beschaffenheit des Broadcastings in Netzwerken ist es möglich, dass **sqlcmd** nicht von allen Servern rechtzeitig eine Antwort empfängt. Daher kann die Liste der zurückgegebenen Server mit jedem Aufruf dieser Option variieren.  
  
 Wenn der optionale Parameter **c** angegeben ist, wird die Ausgabe ohne die Kopfzeile **Server:** angezeigt. Jede Serverzeile wird ohne führende Leerzeichen ausgegeben. Diese Darstellung wird als bereinigte Ausgabe bezeichnet. Durch die einfache Ausgabe wird die Verarbeitungsleistung von Skriptsprachen verbessert.  
  
 **-p**[**1**]  
 Gibt Leistungsstatistiken für jedes Resultset aus. Folgende Ausgabe ist ein Beispiel für das Format von Leistungsstatistiken:  
  
 `Network packet size (bytes): n`  
  
 `x xact[s]:`  
  
 `Clock Time (ms.): total       t1  avg       t2 (t3 xacts per sec.)`  
  
 Hierbei gilt:  
  
 `x` = Anzahl der Transaktionen, die von SQL Server verarbeitet werden.  
  
 `t1` = Gesamtdauer aller Transaktionen.  
  
 `t2` = Durchschnittliche Dauer einer einzelnen Transaktion.  
  
 `t3` = Durchschnittliche Anzahl von Transaktionen pro Sekunde.  
  
 Alle Zeitangaben erfolgen in Millisekunden.  
  
 Wird der optionale Parameter **1** angegeben, wird die Statistik im durch Doppelpunkte getrennten Format ausgegeben, das problemlos in eine Kalkulationstabelle importiert oder von einem Skript verarbeitet werden kann.  
  
 Wird für den optionalen Parameter ein anderer Wert als **1** angegeben, wird ein Fehler generiert und **sqlcmd** beendet.  
  
 **-X**[**1**]  
 Deaktiviert Befehle, die die Systemsicherheit gefährden können, wenn **sqlcmd** aus einer Batchdatei ausgeführt wird. Die deaktivierten Befehle werden trotzdem erkannt; **sqlcmd** gibt eine Warnmeldung aus und setzt die Ausführung fort. Wenn der optionale Parameter **1** angegeben wird, generiert **sqlcmd** eine Fehlermeldung und wird dann beendet. Die folgenden Befehle sind deaktiviert, wenn die Option **-X** verwendet wird:  
  
- **ED**  
  
- **!!** _command_  
  
 Wenn die **-X** -Option angegeben wird, verhindert sie die Übergabe von Umgebungsvariablen an **sqlcmd**. Sie verhindert darüber hinaus die Ausführung des Startskripts, das mithilfe der SQLCMDINI-Skriptvariablen angegeben wurde. Weitere Informationen zu **sqlcmd** Skriptvariablen finden Sie unter [Verwenden von sqlcmd mit Skriptvariablen](~/relational-databases/scripting/sqlcmd-use-with-scripting-variables.md).  
  
 **-?**  
 Zeigt die Version von **sqlcmd** und eine Syntaxzusammenfassung der **sqlcmd** -Optionen an.  
  
## <a name="remarks"></a>Bemerkungen

Die Optionen müssen nicht in der Reihenfolge verwendet werden, in der sie im Abschnitt zur Syntax gezeigt wurden.

Wenn mehrere Ergebnisse zurückgegeben werden, gibt **sqlcmd** eine Leerzeile zwischen den einzelnen Resultsets in einem Batch aus. Außerdem wird die Meldung `<x> rows affected` nicht angezeigt, wenn sie für die ausgeführte Anweisung nicht gilt.

Wenn Sie **sqlcmd** interaktiv verwenden möchten, geben Sie **sqlcmd** an der Eingabeaufforderung und eine oder mehrere der weiter oben in diesem Artikel beschriebenen Optionen ein. Weitere Informationen finden Sie unter [Verwenden des Hilfsprogramms „sqlcmd“](~/relational-databases/scripting/sqlcmd-use-the-utility.md).

> [!NOTE]  
>  Die Optionen **-L**, **-Q**, **-Z** und **-i** bewirken, dass **sqlcmd** nach der Ausführung beendet wird.
  
 Die gesamte Länge der **sqlcmd**-Befehlszeile in der Befehlsumgebung (Cmd.exe) einschließlich aller Argumente und erweiterten Variablen entspricht der Länge, die im Betriebssystem für Cmd.exe bestimmt wurde.
  
## <a name="variable-precedence-low-to-high"></a>Rangfolge der Variablen (vom niedrigsten bis zum höchsten Rang)
  
1.  Umgebungsvariablen auf Systemebene
  
2.  Umgebungsvariablen auf Benutzerebene  
  
3.  Vor der Ausführung von **sqlcmd** an der Eingabeaufforderung festgelegte Befehlsshell ( **SET** X=Y).  
  
4.  **sqlcmd-v** X=Y  
  
5.  **:Setvar** X Y  
  
> [!NOTE]  
>  Öffnen Sie die **Systemsteuerung**, klicken Sie auf **System** und anschließend auf die Registerkarte **Erweitert** , um die Umgebungsvariablen anzuzeigen.  
  
## <a name="sqlcmd-scripting-variables"></a>sqlcmd-Skriptvariablen  
  
|Variable|Damit verbundene Schalter|R/W|Standard|  
|--------------|--------------------|----------|-------------|  
|SQLCMDUSER|-U|R|""|  
|SQLCMDPASSWORD|-P|--|""|  
|SQLCMDSERVER|-S|R|"DefaultLocalInstance"|  
|SQLCMDWORKSTATION|-H|R|"ComputerName"|  
|SQLCMDDBNAME|-d|R|""|  
|SQLCMDLOGINTIMEOUT|-l|R/W|"8" (Sekunden)|  
|SQLCMDSTATTIMEOUT|-t|R/W|"0" = unbegrenzt warten|  
|SQLCMDHEADERS|-H|R/W|"0"|  
|SQLCMDCOLSEP|-S|R/W|" "|  
|SQLCMDCOLWIDTH|-w|R/W|"0"|  
|SQLCMDPACKETSIZE|-a|R|"4096"|  
|SQLCMDERRORLEVEL|-M|R/W|0|  
|SQLCMDMAXVARTYPEWIDTH|-y|R/W|"256"|  
|SQLCMDMAXFIXEDTYPEWIDTH|-y|R/W|"0" = unbegrenzt|  
|SQLCMDEDITOR||R/W|"edit.com"|  
|SQLCMDINI||R|""|
|SQLCMDUSEAAD  | -G | R/W | "" |  
  
 SQLCMDUSER, SQLCMDPASSWORD und SQLCMDSERVER werden festgelegt, wenn **:Connect** verwendet wird.  
  
 Durch R wird angezeigt, dass der Wert nur einmal während der Programminitialisierung festgelegt werden kann.  
  
 Durch R/W wird angezeigt, dass der Wert mithilfe des **setvar** -Befehls geändert werden kann und auf nachfolgende Befehle der neue Wert angewendet wird.  
  
## <a name="sqlcmd-commands"></a>sqlcmd-Befehle  
 Zusätzlich zu den Transact-SQL-Anweisungen in **sqlcmd** sind auch die folgenden Befehle verfügbar:  
  
|||  
|-|-|  
|**GO** [*Anzahl*]|**:List**|  
|[ **:** ] **RESET**|**:Error**|  
|[ **:** ] **ED**|**:Out**|  
|[**:**] **!!**|**:Perftrace**|  
|[ **:** ] **QUIT**|**:Connect**|  
|[ **:** ] **EXIT**|**:On Error**|  
|**:r**|**:Help**|  
|**:ServerList**|**:XML** [**ON** &#124; **OFF**]|  
|**:Setvar**|**:Listvar**|  
  
 Beachten Sie bei der Verwendung von **sqlcmd** -Befehlen Folgendes:  
  
- Mit Ausnahme von GO muss vor allen **sqlcmd** -Befehlen ein Doppelpunkt (:) angegeben werden.  
  
    > [!IMPORTANT]  
    >  Aus Gründen der Abwärtskompatibilität mit bestehenden **osql** -Skripts werden einige der Befehle auch ohne Angabe des Doppelpunkts erkannt, die durch [ **:** ] gekennzeichnet werden.
  
- **sqlcmd** -Befehle werden nur erkannt, wenn sie am Anfang einer Zeile stehen.  
  
- Bei keinem **sqlcmd** -Befehl wird die Groß- und Kleinschreibung beachtet.  
  
- Jeder Befehl muss in einer eigenen Zeile stehen. Auf einen Befehl darf keine Transact-SQL-Anweisung oder ein anderer Befehl folgen.  
  
- Die Befehle werden sofort ausgeführt. Sie werden nicht wie Transact-SQL-Anweisungen im Ausführungspuffer abgelegt.  
  
 **Bearbeitungsbefehle**  
  [ **:** ] **ED**  
 Startet den Text-Editor. Mit diesem Editor kann der aktuelle Transact-SQL-Batch oder der zuletzt ausgeführte Batch bearbeitet werden. Um den zuletzt ausgeführten Batch zu bearbeiten, muss der **ED** -Befehl unmittelbar nach Abschluss der Ausführung des letzten Batches eingegeben werden.  
  
 Der Text-Editor wird durch die SQLCMDEDITOR-Umgebungsvariable definiert. Der Standardeditor ist 'Edit'. Sie können den Editor ändern, indem Sie die SQLCMDEDITOR-Umgebungsvariable festlegen. Wenn Sie beispielsweise den Microsoft-Editor als Editor festlegen möchten, müssen Sie Folgendes über die Eingabeaufforderung eingeben:  
  
 `SET SQLCMDEDITOR=notepad`  
  
 [ **:** ] **RESET**  
 Löscht den Anweisungscache.  
  
 **:List**  
 Gibt den Inhalt des Anweisungscaches aus.  
  
 **Variablen**  
  **:Setvar** \<**var**> [ **"** _Wert_ **"** ]  
 Definiert **sqlcmd** -Skriptvariablen. Skriptvariablen haben das folgende Format: `$(VARNAME)`.  
  
 Bei Variablennamen wird die Groß- und Kleinschreibung nicht beachtet.  
  
 Skriptvariablen können folgendermaßen festgelegt werden:  
  
- Implizit – mithilfe einer Befehlszeilenoption. Beispielsweise wird mit der Option **-l** die **sqlcmd** -Variable SQLCMDLOGINTIMEOUT festgelegt.  
  
- Explizit – mithilfe des **:Setvar** -Befehls.  
  
- Durch die Definition einer Umgebungsvariablen vor der Ausführung von **sqlcmd**.  
  
> [!NOTE]  
>  Die Option **X** verhindert, dass Umgebungsvariablen an **sqlcmd** übergeben werden.  
  
 Wenn eine Variable, die mithilfe von **:Setvar** definiert wurde, denselben Namen wie eine Umgebungsvariable aufweist, ist die mit **:Setvar** definierte Variable vorrangig.  
  
 Variablennamen dürfen keine Leerzeichen enthalten.  
  
 Variablennamen können nicht dieselbe Form wie ein Variablenausdruck, z. B. $(var), haben.  
  
 Wenn der Zeichenfolgenwert der Skriptvariablen Leerzeichen enthält, müssen Sie den Wert in Anführungszeichen einschließen. Wenn für eine Skriptvariable kein Wert angegeben wird, wird die Skriptvariable ausgelassen.  
  
 **:Listvar**  
 Zeigt die Liste der zurzeit festgelegten Skriptvariablen an.  
  
> [!NOTE]  
>  Es werden nur solche Skriptvariablen angezeigt, die von **sqlcmd** oder mithilfe des **:Setvar** -Befehls festgelegt wurden.  
  
 **Ausgabebefehle**  
  **:Error**   
 _**\<**_  _filename_  **_>|_ STDERR|STDOUT**  
 Leitet die gesamte Fehlerausgabe in die mit *Dateiname* angegebene Datei, in **stderr** oder in **stdout** um. Der Befehl **Error** kann mehrmals in einem Skript verwendet werden. Die Fehlerausgabe wird standardmäßig an **stderr** gesendet.  
  
 *file name*  
 Erstellt und öffnet eine Datei, in die die Ausgabe geschrieben wird. Wenn die Datei bereits vorhanden ist, wird sie auf 0 Byte gekürzt. Wenn der Zugriff auf die Datei aufgrund unzureichender Berechtigungen oder anderer Ursachen nicht möglich ist, wird die Ausgabe nicht umgeleitet, sondern an das zuletzt angegebene Ziel oder an das Standardziel gesendet.  
  
 **STDERR**  
 Leitet die Fehlerausgabe in den **stderr** -Datenstrom. Wenn dieser Datenstrom umgeleitet wurde, wird die Fehlerausgabe von dem Ziel empfangen, zu dem der Datenstrom umgeleitet wurde.  
  
 **STDOUT**  
 Leitet die Fehlerausgabe in den **stdout** -Datenstrom. Wenn dieser Datenstrom umgeleitet wurde, wird die Fehlerausgabe von dem Ziel empfangen, zu dem der Datenstrom umgeleitet wurde.  
  
 **:Out \<** _filename_ **>** | **STDERR**| **STDOUT**  
 Erstellt und leitet alle Abfrageergebnisse in der bzw. an die durch *Dateiname* angegebene Datei, an **stderr** oder an **stdout** um. Standardmäßig wird die Ausgabe an **stdout** gesendet. Wenn die Datei bereits vorhanden ist, wird sie auf 0 Byte gekürzt. Der Befehl **Out** kann mehrmals in einem Skript verwendet werden.  
  
 **:Perftrace \<** _filename_ **>** | **STDERR**| **STDOUT**  
 Erstellt und leitet alle Informationen zur Leistungsnachverfolgung in der bzw. an die durch *Dateiname* angegebene Datei, an **stderr** oder **stdout** um. Standardmäßig wird die Ausgabe zur Leistungsnachverfolgung an **stdout** gesendet. Wenn die Datei bereits vorhanden ist, wird sie auf 0 Byte gekürzt. Der Befehl **Perftrace** kann mehrmals in einem Skript verwendet werden.  
  
 **Befehle zur Ausführungssteuerung**  
  **:On Error**[ **exit** | **ignore**]  
 Legt die Aktion fest, die ausgeführt werden soll, wenn ein Fehler während der Skript- oder Batchausführung auftritt.  
  
 Wird die Option **exit** verwendet, wird **sqlcmd** mit dem entsprechenden Fehlerwert beendet.  
  
 Wenn die Option **ignore** verwendet wird, ignoriert **sqlcmd** den Fehler und setzt die Batch- oder Skriptausführung fort. Standardmäßig wird eine Fehlermeldung ausgegeben.  
  
 [ **:** ] **QUIT**  
 Bewirkt, dass **sqlcmd** beendet wird.  
  
 [ **:** ] **EXIT**[ **(** _Anweisung_ **)** ]  
 Gibt Ihnen die Möglichkeit, das Ergebnis einer SELECT-Anweisung als Rückgabewert von **sqlcmd** zu verwenden. Wenn numerisch, wird die erste Spalte der letzten Ergebniszeile in eine 4 Byte lange ganze Zahl (Long) konvertiert. MS-DOS, Linux und Mac übergeben das niedrige Byte an den übergeordneten Prozess oder an die Fehlerebene des Betriebssystems. Windows 200x übergibt die gesamte 4 Bytes lange ganze Zahl. Die Syntax ist:  
  
 `:EXIT(query)`  
  
 Beispiel:  
  
 `:EXIT(SELECT @@ROWCOUNT)`  
  
 Sie können den **EXIT** -Parameter auch als Teil einer Batchdatei einschließen. Geben Sie an der Eingabeaufforderung z. B. Folgendes ein:  
  
 `sqlcmd -Q "EXIT(SELECT COUNT(*) FROM '%1')"`  
  
 Das Hilfsprogramm **sqlcmd** sendet alle zwischen den Klammern **()** stehende Angaben an den Server. Wenn eine gespeicherte Systemprozedur eine Menge auswählt und einen Wert zurückgibt, wird nur die Auswahl zurückgegeben. Eine EXIT **()** -Anweisung ohne Angabe zwischen den Klammern führt alle im Batch vorhergehenden Anweisungen aus und beendet das Hilfsprogramm dann ohne Rückgabewert.  
  
 Wird eine fehlerhafte Abfrage angegeben, wird **sqlcmd** beendet, ohne dass ein Wert zurückgegeben wird.  
  
 Folgende Liste enthält eine Aufstellung von EXIT-Formaten:  
  
- :EXIT  
  
 Führt den Batch nicht aus, beendet das Hilfsprogramm sofort und gibt keinen Wert zurück.  
  
- :EXIT( )  
  
 Führt den Batch aus, beendet dann das Hilfsprogramm und gibt keinen Wert zurück.  
  
- :EXIT(query)  
  
 Führt den Batch mit der darin enthaltenen Abfrage aus und beendet dann das Hilfsprogramm, nachdem die Ergebnisse der Abfrage zurückgegeben wurden.  
  
 Wird RAISERROR in einem **sqlcmd** -Skript verwendet und der Status 127 ausgelöst, wird **sqlcmd** beendet und die entsprechende Meldungs-ID an den Client zurückgegeben. Beispiel:  
  
 `RAISERROR(50001, 10, 127)`  
  
 Dieser Fehler bewirkt, dass das **sqlcmd** -Skript beendet und die Meldungs-ID 50001 an den Client zurückgegeben wird.  
  
 Die Rückgabewerte −1 bis −99 sind für SQL Server reserviert. **sqlcmd** definiert die folgenden zusätzlichen Rückgabewerte:  
  
|Rückgabewerte|BESCHREIBUNG|  
|-------------------|-----------------|  
|-100|Vor dem Auswählen des Rückgabewerts ist ein Fehler aufgetreten.|  
|-101|Beim Auswählen eines Rückgabewerts wurden keine Zeilen gefunden.|  
|-102|Beim Auswählen des Rückgabewerts ist ein Konvertierungsfehler aufgetreten.|  
  
 **GO** [*Anzahl*]  
 GO signalisiert sowohl das Ende eines Batches als auch die Ausführung aller zwischengespeicherten Transact-SQL-Anweisungen. Das Batch wird mehrmals als separate Batches ausgeführt. Sie können eine Variable in einem einzelnen Batch nicht mehr als einmal deklarieren.
  
 **Sonstige Befehle**  
  **:r \<** _filename_ **>**  
 Hiermit werden zusätzliche Transact-SQL-Anweisungen und **sqlcmd**-Befehle aus der durch **\<**_filename_**>** angegebenen Datei analysiert, und das Ergebnis wird in den Anweisungscache geschrieben.  
  
 Wenn die Datei Transact-SQL-Anweisungen enthält, die nicht von **GO** gefolgt sind, müssen Sie **GO** in der ersten Zeile nach **:r** eingeben.  
  
> [!NOTE]  
>  **\<** _filename_ **>** wird relativ zum Startverzeichnis gelesen, in dem **sqlcmd** ausgeführt wurde.  
  
 Die Datei wird gelesen und ausgeführt, nachdem ein Batchabschlusszeichen gefunden wurde. Sie können den Befehl **:r** mehrmals verwenden. Die Datei kann beliebige **sqlcmd** -Befehle enthalten. Dies schließt das Batchabschlusszeichen **GO** ein.  
  
> [!NOTE]  
>  Die im interaktiven Modus angezeigte Zeilenanzahl wird für jeden gefundenen `:r`-Befehl um 1 erhöht. Der `:r`-Befehl wird in der Ausgabe des Listenbefehls angezeigt.  
  
 **:Serverlist**  
 Listet die lokal konfigurierten Server sowie die Namen der Server auf, die Nachrichten über das Netzwerk senden.  
  
 **:Connect**  _Servername_[ **\\** _Instanzname_] [-l *Timeout*] [-U *Benutzername* [-P *Kennwort*]]  
 Stellt eine Verbindung mit einer Instanz von SQL Server her. Schließt außerdem die aktuelle Verbindung.  
  
 Timeoutoptionen:  
  
|Wert|Verhalten|  
|-|-|  
|0|unbegrenzte Wartezeit|  
|n>0|Wartezeit beträgt n Sekunden|  
  
 Die SQLCMDSERVER-Skriptvariable spiegelt die zurzeit aktive Verbindung wider.  
  
 Wenn *Timeout* nicht angegeben wird, gilt standardmäßig der Wert der SQLCMDLOGINTIMEOUT-Variablen.  
  
 Wenn nur *Benutzername* angegeben wird (entweder als Option oder als Umgebungsvariable), wird der Benutzer zur Eingabe eines Kennworts aufgefordert. Benutzer werden nicht aufgefordert, wenn die Umgebungsvariable SQLCMDUSER oder SQLCMDPASSWORD festgelegt wurde. Wenn weder Optionen noch Umgebungsvariablen angegeben werden, wird der Windows-Authentifizierungsmodus für die Anmeldung verwendet. Wenn z. B. mithilfe integrierter Sicherheit eine Verbindung mit der Instanz `instance1` von SQL Server `myserver` hergestellt werden soll, geben Sie folgenden Befehl ein:  
  
 `:connect myserver\instance1`  
  
 Wenn mithilfe von Skriptvariablen eine Verbindung mit der Standardinstanz auf `myserver` hergestellt werden soll, würden Sie Folgendes eingeben:  
  
 `:setvar myusername test`  
  
 `:setvar myservername myserver`  
  
 `:connect $(myservername) $(myusername)`  
  
 [ **:** ] **!!** < *Befehl*>  
 Führt Betriebssystembefehle aus. Um einen Betriebssystembefehl auszuführen, geben Sie zwei Ausrufezeichen ( **!!** ) gefolgt von dem Betriebssystembefehl in eine neue Zeile ein. Beispiel:  
  
 `:!! Dir`  
  
> [!NOTE]  
>  Der Befehl wird auf dem Computer ausgeführt, auf dem **sqlcmd** ausgeführt wird.  
  
 **:XML** [**ON** | **OFF**]  
 Weitere Informationen finden Sie unter [XML-Ausgabeformat](#OutputXML) und [JSON-Ausgabeformat](#OutputJSON) in diesem Artikel.  
  
 **:Help**  
 Listet die **sqlcmd** -Befehle zusammen mit einer kurzen Beschreibung jedes Befehls auf.  
  
### <a name="sqlcmd-file-names"></a>sqlcmd-Dateinamen  
 **sqlcmd** -Eingabedateien können mit der Option **-i** oder dem Befehl **:r** angegeben werden. Ausgabedateien können mit der Option **-o** oder den Befehlen **:Error**, **:Out** und **:Perftrace** angegeben werden. Es folgen einige Richtlinien für das Verwenden dieser Dateien:  
  
- **:Error**, **:Out** und **:Perftrace** sollten separate **\<**_filename_**>** verwenden. Falls derselbe **\<**_filename_**>** verwendet wird, werden die Eingaben der Dateien womöglich vermischt.  
  
- Wenn eine Eingabedatei auf einem Remoteserver einen Laufwerksdateipfad wie „:Out c:\OutputFile.txt“ enthält und von **sqlcmd** auf einem lokalen Computer aufgerufen wird, Die wird Ausgabedatei auf dem lokalen Computer und nicht auf dem Remoteserver erstellt.  
  
- Gültige Dateipfade sind beispielsweise: `C:\<filename>`, `\\<Server>\<Share$>\<filename>` und `"C:\Some Folder\<file name>"`. Verwenden Sie Anführungszeichen, wenn der Pfad ein Leerzeichen enthält.  
  
- Mit jeder neuen **sqlcmd** -Sitzung werden eventuell schon vorhandene gleichnamige Dateien überschrieben.  
  
### <a name="informational-messages"></a>Informationsmeldungen

**sqlcmd** gibt alle vom Server gesendeten Informationsmeldungen aus. Im folgenden Beispiel wird eine Informationsmeldung ausgegeben, nachdem die Transact-SQL-Anweisungen ausgeführt wurden.
  
Geben Sie an der Eingabeaufforderung folgenden Befehl ein:

`sqlcmd`
  
Geben Sie an der sqlcmd-Eingabeaufforderung Folgendes ein:

`USE AdventureWorks2012;`

`GO`

Wenn Sie die EINGABETASTE drücken, wird folgende Informationsmeldung ausgegeben: Der Datenbankkontext wurde in "AdventureWorks2012" geändert.  
  
### <a name="output-format-from-transact-sql-queries"></a>Ausgabeformat von Transact-SQL-Abfragen  
 **sqlcmd** gibt zuerst eine Spaltenüberschrift aus, die die in der SELECT-Liste angegebenen Spaltennamen enthält. Die Spaltennamen werden durch das SQLCMDCOLSEP-Zeichen getrennt. Standardmäßig handelt es sich hierbei um ein Leerzeichen. Wenn der Spaltenname kürzer als die Spaltenbreite ist, wird die Ausgabe bis zur nächsten Spalte mit Leerzeichen aufgefüllt.  
  
 Auf diese Zeile folgt eine Trennlinie, die durch eine Reihe von Bindestrichen dargestellt wird. Die folgende Ausgabe zeigt ein Beispiel.  
  
 Starten Sie **sqlcmd**. Geben Sie an der **sqlcmd** -Eingabeaufforderung folgende Abfrage ein:  
  
 `USE AdventureWorks2012;`  
  
 `SELECT TOP (2) BusinessEntityID, FirstName, LastName`  
  
 `FROM Person.Person;`  
  
 `GO`  
  
 Wenn Sie die EINGABETASTE drücken, wird das folgende Resultset zurückgegeben.  
  
 `BusinessEntityID FirstName    LastName`  
  
 `---------------- ------------ ----------`  
  
 `285              Syed         Abbas`  
  
 `293              Catherine    Abel`  
  
 `(2 row(s) affected)`  
  
 Obwohl die `BusinessEntityID`-Spalte nur vier Zeichen breit ist, wurde sie erweitert, um den längeren Spaltennamen aufzunehmen. Standardmäßig wird die Ausgabe mit dem 80. Zeichen beendet. Dies kann geändert werden, indem Sie die Option **-w** verwenden oder die SQLCMDCOLWIDTH-Skriptvariable festlegen.  
  
###  <a name="xml-output-format"></a><a name="OutputXML"></a> XML-Ausgabeformat  
 Die XML-Ausgabe, die sich aus der FOR XML-Klausel ergibt, wird unformatiert in einem fortlaufenden Datenstrom ausgegeben.  
  
 Verwenden Sie den Befehl `:XML ON`, wenn Sie eine Ausgabe im XML-Format erwarten.  
  
> [!NOTE]  
>  **sqlcmd** gibt Fehlermeldungen im üblichen Format zurück. Beachten Sie, dass die Fehlermeldungen auch im XML-Textstrom im XML-Format ausgegeben werden. Mit `:XML ON`zeigt **sqlcmd** keine Informationsmeldungen an.  
  
 Verwenden Sie den folgenden Befehl, um den XML-Modus zu deaktivieren: `:XML OFF`.  
  
 Der GO-Befehl sollte nicht verwendet werden, bevor der XML OFF-Befehl ausgegeben wurde, da XML OFF bewirkt, dass **sqlcmd** zur zeilenbasierten Ausgabe zurückkehrt.  
  
 Es ist nicht möglich, XML-Daten (Datenstrom) und Rowsetdaten zu mischen. Wenn der XML ON-Befehl nicht verwendet wurde, bevor eine Transact-SQL-Anweisung, die XML-Datenströme ausgibt, ausgeführt wurde, wird die Ausgabe nicht richtig dargestellt. Nachdem der XML ON-Befehl verwendet wurde, können Sie keine Transact-SQL-Anweisungen ausführen, die reguläre Rowsets ausgeben.  
  
> [!NOTE]  
>  Der `:XML`-Befehl unterstützt die SET STATISTICS XML-Anweisung nicht.  
  
###  <a name="json-output-format"></a><a name="OutputJSON"></a> JSON-Ausgabeformat  
 Verwenden Sie den Befehl `:XML ON`, wenn Sie eine Ausgabe im JSON-Format erwarten. Andernfalls enthält die Ausgabe sowohl den Spaltennamen als auch den JSON-Text. Diese Ausgabe ist kein gültiger JSON-Code.  
  
 Verwenden Sie den folgenden Befehl, um den XML-Modus zu deaktivieren: `:XML OFF`.  
  
 Weitere Informationen finden Sie unter [XML-Ausgabeformat](#OutputXML) in diesem Artikel.  

### <a name="using-azure-active-directory-authentication"></a>Verwenden der Azure Active Directory-Authentifizierung

Beispiele zum Verwenden der Azure Active Directory-Authentifizierung:

```cmd
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net  -G  -l 30
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net -G -U bob@contoso.com -P MyAADPassword -l 30
```
  
## <a name="sqlcmd-best-practices"></a>Bewährte Methoden für die Verwendung von sqlcmd

Die folgenden Methoden haben sich dazu bewährt, Sicherheit und Effizienz zu optimieren.  

- Verwenden Sie die integrierte Sicherheit von Windows.  

- Verwenden Sie in automatisierten Umgebungen **-X[1]** .

- Sichern Sie Eingabe- und Ausgabedateien, indem Sie die geeigneten NTFS-Dateisystemberechtigungen verwenden.

- Führen Sie aus Leistungsgründen möglichst alle Aufgaben in einer einzigen **sqlcmd** -Sitzung, nicht in einer Reihe von verschiedenen Sitzungen aus.

- Legen Sie für Batch- bzw. Abfragetimeouts Werte fest, die höher sind als die erwartete Ausführungsdauer.

Maximieren Sie mit folgenden Methoden die Richtigkeit:

- Verwenden Sie **-V16**, um [Nachrichten des Schweregrads 16](../relational-databases/errors-events/database-engine-error-severities.md#levels-of-severity) zu protokollieren.  Nachrichten des Schweregrads 16 verweisen auf allgemeine Fehler, die der Benutzer korrigieren kann.

- Überprüfen Sie nach dem Beenden des Prozesses den Exitcode und die DOS ERRORLEVEL-Variable.  **sqlcmd** gibt normalerweise 0 (null) zurück. Andernfalls wird der ERRORLEVEL-Wert wie von **-V** konfiguriert festgelegt.  Mit anderen Worten: Erwarten Sie nicht, dass ERRORLEVEL mit der Fehlernummer übereinstimmt, die von SQL Server gemeldet wird. Die Fehlernummer ist ein SQL Server-spezifischer Wert, der der Systemfunktion [ **@@ERROR**](../t-sql/functions/error-transact-sql.md) entspricht.  ERRORLEVEL ist ein SQLCMD-spezifischer Wert, der angibt, warum SQLCMD beendet wurde, und wird durch Angabe des Befehlszeilenarguments **-b** beeinflusst.

Die Verwendung von **-V16** in Kombination mit der Überprüfung von Exitcode und DOS ERRORLEVEL kann dazu beitragen, Fehler in automatisierten Umgebungen zu erfassen, insbesondere Quality Gates vor einem Produktionsrelease.

## <a name="next-steps"></a>Nächste Schritte

- [Starten des Hilfsprogramms „sqlcmd“](~/relational-databases/scripting/sqlcmd-start-the-utility.md)
- [Ausführen von Transact-SQL-Skriptdateien mithilfe von sqlcmd](~/relational-databases/scripting/sqlcmd-run-transact-sql-script-files.md)
- [Verwenden des Hilfsprogramms sqlcmd](~/relational-databases/scripting/sqlcmd-use-the-utility.md)
- [Verwenden von sqlcmd mit Skriptvariablen](~/relational-databases/scripting/sqlcmd-use-with-scripting-variables.md)
- [Herstellen einer Verbindung mit der Datenbank-Engine mithilfe von sqlcmd](~/relational-databases/scripting/sqlcmd-connect-to-the-database-engine.md)
- [Bearbeiten von SQLCMD-Skripts mit dem Abfrage-Editor](~/relational-databases/scripting/edit-sqlcmd-scripts-with-query-editor.md)
- [Verwalten von Auftragsschritten](~/ssms/agent/manage-job-steps.md)   
- [Erstellen eines CmdExec-Auftragsschritts](~/ssms/agent/create-a-cmdexec-job-step.md)  

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]