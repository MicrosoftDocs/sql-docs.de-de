---
title: Lokale Überwachung für die Sammlung von Nutzungs- und -Diagnosedaten
description: In diesem Artikel erhalten Sie Informationen zur lokalen Überwachung, die von SQL Server verwendet wird, um Nutzungs- und Diagnosedaten zu sammeln und an Microsoft zu senden.
ms.custom: seo-lt-2019
ms.date: 03/27/2019
ms.prod: sql
ms.prod_service: security
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Local Audit
ms.assetid: a0665916-7789-4f94-9086-879275802cf3
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 6333f571ea271420b4044f7e3cf562d2f1edcc15
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353539"
---
# <a name="local-audit-for-sql-server-usage-and-diagnostic-data-collection-ceip"></a>Lokale Überwachung für SQL Server-Nutzungs- und -Diagnosedatensammlung (CEIP)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

## <a name="introduction"></a>Einführung

Microsoft SQL Server enthält internetfähige Features, die Daten über Ihren Computer oder Ihr Gerät erfassen und senden können. Die zugehörigen Informationen werden als *Standardcomputerinformationen* bezeichnet. Die Komponente „Lokale Überwachung“ der [SQL Server-Nutzungs- und -Diagnosedatensammlung](usage-and-diagnostic-data-configuration-for-sql-server.md) schreibt die vom Dienst erfassten Daten, die die an Microsoft zu sendenden Daten (Protokolle) darstellen, in einen bestimmten Ordner. Der Zweck der lokalen Überwachung besteht darin, dass Kunden die von Microsoft mit dem Feature erfassten Daten einsehen können, um die Compliance sowie die Einhaltung von behördlichen Bestimmungen oder Datenschutzbestimmungen zu überprüfen.  

Seit SQL Server 2016 CU2 und CU3 kann die lokale Überwachung auf der Instanzebene für SQL Server-Datenbank-Engine und Analysis Services (SSAS) konfiguriert werden. In SQL Server 2016 CU4, SQL Server 2016 SP1 und späteren Releases ist die lokale Überwachung auch für SQL Server Integration Services (SSIS) aktiviert. Andere SQL Server-Komponenten, die beim Setup installiert werden, und SQL Server-Tools, die nach dem Setup heruntergeladen oder installiert werden, bieten keine Funktion zur lokalen Überwachung der Nutzungs- und Diagnosedatensammlung.

## <a name="remarks"></a>Bemerkungen

 - Das Entfernen oder Deaktivieren des CEIP-Diensts für SQL wird nicht unterstützt. 
 - Das Entfernen von CEIP-Ressourcen für SQL aus der Clustergruppe wird nicht unterstützt. 

Informationen zur Deaktivierung der Datensammlung finden Sie unter [Aktivieren oder Deaktivieren der lokalen Überwachung](#turning-local-audit-on-or-off).

## <a name="prerequisites"></a>Voraussetzungen 

Nachfolgend sind die erforderlichen Komponenten zum Aktivieren der lokalen Überwachung auf jeder SQL Server-Instanz aufgeführt: 

1. Die Instanz wird auf SQL Server 2016 RTM CU2 oder höher gepatcht. Für Integration Services wird die Instanz auf SQL 2016 RTM CU4, SQL 2016 SP1 oder höher gepatcht.

1. Benutzer muss ein Systemadministrator oder eine Rolle mit Zugriff zum Hinzufügen und Ändern von Registrierungsschlüsseln, Erstellen von Ordnern, Verwalten der Ordnersicherheit und Beenden/Starten eines Windows-Diensts sein.  

## <a name="pre-configuration-steps-prior-to-turning-on-local-audit"></a>Vorkonfigurationsschritte vor der Aktivierung der lokalen Überwachung

Vor der Aktivierung der lokalen Überwachung muss ein Systemadministrator folgende Voraussetzungen erfüllen:

1. Er muss den SQL Server-Instanznamen und das Anmeldekonto für den SQL Server-CEIP-Dienst kennen. 

1. Konfigurieren Sie einen neuen Ordner für die Dateien der lokalen Überwachung.

1. Er muss Berechtigungen für das Anmeldekonto des SQL Server-CEIP-Diensts gewähren.

1. Er muss eine Registrierungsschlüsseleinstellung erstellen, um das Zielverzeichnis für die lokale Überwachung zu konfigurieren. 


### <a name="get-the-sql-server-ceip-service-logon-account"></a>Abrufen des Anmeldekontos für den SQL Server-CEIP-Dienst 

Führen Sie die folgenden Schritte aus, um das Anmeldekonto für den SQL Server-CEIP-Dienst abzurufen:
 
1. Starten Sie die Konsole **Dienste**. Wählen Sie dazu die **Windows-Taste + R** auf der Tastatur aus, um das Dialogfeld **Ausführen** zu öffnen. Geben Sie dann *services.msc* in das Textfeld ein, und wählen **OK** aus, um die Konsole **Dienste** zu starten.  

2. Navigieren Sie zu dem entsprechenden Dienst. Suchen Sie z. B. für die Datenbank-Engine nach dem **SQL Server-CEIP-Dienst** **(*Name Ihrer Instanz*)** . Suchen Sie für Analysis Services nach **SQL Server Analysis Services-CEIP** **(*Name Ihrer Instanz*)** . Suchen Sie für Integration Services nach **SQL Server Integration Services-CEIP-Dienst**.

3. Klicken Sie mit der rechten Maustaste auf den Dienst, und wählen Sie **Eigenschaften** aus. 

4. Wählen Sie die Registerkarte **Anmelden** aus. Das Anmeldekonto wird in **Dieses Konto** aufgelistet. 

### <a name="configure-a-new-folder-for-the-local-audit-files"></a>Konfigurieren Sie einen neuen Ordner für die Dateien der lokalen Überwachung.    

Erstellen Sie einen neuen Ordner (Verzeichnis für die lokale Überwachung), in den die Protokolle von der lokalen Überwachung geschrieben werden. Der vollständige Pfad zum Verzeichnis der lokalen Überwachung für eine Standardinstanz der Datenbank-Engine wäre beispielsweise: *C:\\SQLCEIPAudit\\MSSQLSERVER\\DB\\* . 
 
  >[!NOTE] 
  >Konfigurieren Sie den Verzeichnispfad für die lokale Überwachung außerhalb des SQL Server-Installationspfads, um zu vermeiden, dass die Überwachungsfunktionen und das Patchen zu potenziellen Problemen mit SQL Server führen.

|Entwurfsentscheidung|Empfehlung|  
|-----------------|----------|  
|Speicherplatzverfügbarkeit |Planen Sie für mittlere Arbeitsauslastungen mit ungefähr 10 Datenbanken ca. 2 MB an Speicherplatz pro Datenbank und Instanz ein.|  
|Separate Verzeichnisse | Erstellen sie für jede Instanz ein Verzeichnis. Verwenden Sie z. B. *C:\\SQLCEIPAudit\\MSSQLSERVER\\DB\\* für eine SQL Server-Instanz namens `MSSQLSERVER`. Dies vereinfacht die Dateiverwaltung.
|Separate Ordner |Verwenden Sie für jeden Dienst einen bestimmten Ordner. Verwenden Sie z. B. für einen angegebenen Instanznamen einen Ordner für die Datenbank-Engine. Wenn eine Instanz von Analysis Services denselben Instanznamen verwendet, erstellen Sie einen separaten Ordner für Analysis Services. Wenn sowohl Datenbank-Engine- als auch Analysis Services-Instanzen für denselben Ordner konfiguriert sind, werden alle Daten von der lokalen Überwachung für beide Instanzen in dieselbe Protokolldatei geschrieben.| 
|Gewähren von Berechtigungen für das Anmeldekonto des SQL Server-CEIP-Diensts|Aktivieren Sie die Zugriffsberechtigungen **Ordnerinhalt auflisten**, **Lesen** und **Schreiben** für das Anmeldekonto des SQL Server-CEIP-Diensts.|


### <a name="grant-permissions-to-the-sql-server-ceip-service-logon-account"></a>Gewähren von Berechtigungen für das Anmeldekonto des SQL Server-CEIP-Diensts
  
1. Navigieren Sie in **Datei-Explorer** zum Speicherort, in dem sich der neue Ordner befindet.

1. Klicken Sie mit der rechten Maustaste auf den neuen Ordner, und wählen Sie **Eigenschaften** aus. 

1. Wählen Sie auf der Registerkarte **Sicherheit** die Option **Bearbeiten** und dann „Berechtigung verwalten“ aus.

1. Wählen Sie **Hinzufügen** aus, und geben Sie die Anmeldeinformationen des SQL Server-CEIP-Diensts ein. Beispiel: `NT Service\SQLTELEMETRY`.

1. Wählen Sie **Namen überprüfen** aus, um den bereitgestellten Namen zu überprüfen, und wählen Sie dann **OK** aus.

1. Wählen Sie im Dialogfeld **Berechtigung** das Anmeldekonto für den SQL Server-CEIP-Dienst aus, und wählen Sie **Ordnerinhalt auflisten**, **Lesen** und **Schreiben** aus.

1. Wählen Sie **OK** aus, um die Berechtigungsänderungen sofort anzuwenden. 
  
### <a name="create-a-registry-key-setting-to-configure-local-audit-target-directory"></a>Erstellen einer Registrierungsschlüsseleinstellung, um das Zielverzeichnis für die lokale Überwachung zu konfigurieren

1. Starten Sie „regedit“.

1. Navigieren Sie zu dem entsprechenden CPE-Pfad:

   | Version | ***Datenbank-Engine*** – Registrierungsschlüssel |
   | :------ | :----------------------------- |
   | 2016    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\MSSQL **13**.*Name-Ihrer-Instanz*\\CPE |
   | 2017    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\MSSQL **14**.*Name-Ihrer-Instanz*\\CPE |
   | 2019    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\MSSQL **15**.*Name-Ihrer-Instanz*\\CPE |
   | &nbsp; | &nbsp; |

   | Version | ***Analysis Services*** – Registrierungsschlüssel |
   | :------ | :------------------------------- |
   | 2016    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\MSAS **13**.*Name-Ihrer-Instanz*\\CPE |
   | 2017    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\MSAS **14**.*Name-Ihrer-Instanz*\\CPE |
   | 2019    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\MSAS **15**.*Name-Ihrer-Instanz*\\CPE |  
   | &nbsp; | &nbsp; |

   | Version | ***Integration Services*** – Registrierungsschlüssel |
   | :------ | :---------------------------------- |
   | 2016    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\**130** |
   | 2017    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\**140** |
   | 2019    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\**150** |
   | &nbsp; | &nbsp; |

1. Klicken Sie im mit der rechten Maustaste auf den CPE-Pfad, und wählen Sie dann **Neu** aus. Wählen Sie **Zeichenfolgenwert** aus.

1. Benennen Sie den neuen Registrierungsschlüssel `UserRequestedLocalAuditDirectory`. 
 
## <a name="turning-local-audit-on-or-off"></a>Aktivieren oder Deaktivieren der lokalen Überwachung

Nachdem Sie die Schritte zur Vorkonfiguration abgeschlossen haben, können Sie die lokale Überwachung aktivieren. Verwenden Sie dazu ein Systemadministratorkonto oder eine ähnliche Rolle mit Zugriff auf die Änderungsfunktion für Registrierungsschlüssel, um die lokale Überwachung mithilfe der folgenden Schritte zu aktivieren oder zu deaktivieren. 

1. Starten Sie **regedit**.  

1. Navigieren Sie zu dem entsprechenden CPE-[Pfad](#create-a-registry-key-setting-to-configure-local-audit-target-directory). 

1. Klicken Sie mit der rechten Maustaste auf **UserRequestedLocalAuditDirectory**, und wählen Sie *Ändern* aus. 

1. Geben Sie den Pfad für die lokale Überwachung ein, um diese zu aktivieren. Beispiel: *C:\\SQLCEIPAudit\\MSSQLSERVER\\DB\\*.
 
    Um die lokale Überwachung zu deaktivieren, löschen Sie den Wert in **UserRequestedLocalAuditDirectory**.

1. Schließen Sie **regedit**. 

Das SQL Server-CEIP sollte die Einstellung für die lokale Überwachung sofort erkennen, wenn der Dienst bereits ausgeführt wird. Um den SQL Server CEIP-Dienst zu starten, kann ein Systemadministrator oder eine Person mit der Berechtigung zum Starten oder Beenden von Windows-Diensten die folgenden Schritte ausführen: 

1. Starten Sie die Konsole **Dienste**. Wählen Sie dazu die **Windows-Taste + R** auf der Tastatur aus, um das Dialogfeld **Ausführen** zu öffnen. Geben Sie dann *services.msc* in das Textfeld ein, und wählen **OK** aus, um die Konsole **Dienste** zu starten.  

1. Navigieren Sie zu dem entsprechenden Dienst. 

    - Für die Datenbank-Engine verwenden Sie **SQL Server CEIP-Dienst (*Name-Ihrer-Instanz*)**.     
    - Verwenden Sie für Analysis Services **SQL Server Analysis Services CEIP (*Name Ihrer Instanz*)**.
    - Für Integration Services: 
        - Für SQL 2016 verwenden Sie *SQL Server Integration Services CEIP-Dienst 13.0*.
        - Für SQL 2017 verwenden Sie *SQL Server Integration Services CEIP-Dienst 14.0*.
    - Für SQL 2019 verwenden Sie den *SQL Server Integration Services-CEIP-Dienst 15.0*.

1. Klicken Sie mit der rechten Maustaste auf den Dienst, und wählen Sie „Neu starten“ aus. 

1. Vergewissern Sie sich, dass der Dienst den Status **Wird ausgeführt** hat. 

Die lokale Überwachung generiert eine Protokolldatei pro Tag. Die Protokolldateien erhalten Dateinamen im Format `<YYYY-MM-DD>.json`. Beispiel: *2016-07-12.json*. Wenn für den Tag im ausgewählten Verzeichnis eine Datei vorhanden ist, werden die Einträge von der lokalen Überwachung an die Datei angefügt. Andernfalls wird für den Tag eine neue Datei erstellt. 

  >[!NOTE]
  > Nach der Aktivierung der lokalen Überwachung kann es bis zu fünf Minuten dauern, bis die Protokolldatei zum ersten Mal geschrieben wird. 

## <a name="maintenance"></a>Wartung 

1. Um die Nutzung von Speicherplatz beim Schreiben von Dateien durch die lokale Überwachung einzuschränken, richten Sie eine Richtlinie oder einen regulären Auftrag ein. So kann das Verzeichnis für die lokale Überwachung bereinigt werden, um ältere, überflüssige Dateien zu entfernen.  

2. Schützen Sie das Verzeichnis der lokalen Überwachung, sodass nur geeignete Personen darauf zugreifen können. Beachten Sie, dass die Protokolldateien Informationen enthalten, die in [Konfigurieren von SQL Server 2016 zum Senden von Feedback an Microsoft](https://support.microsoft.com/kb/3153756) erläutert werden. Der entsprechende Zugriff auf diese Datei sollte die meisten Personen in Ihrem Unternehmen daran hindern, die Datei zu lesen.  

## <a name="data-dictionary-of-local-audit-output-data-structure"></a>Datenwörterbuch für die Ausgabedatenstruktur der lokalen Überwachung 

- Die Protokolldateien der lokalen Überwachung besitzen das JSON-Format und enthalten einen Satz von Objekten (Zeilen), die Datenpunkte darstellen, die zu **emitTime** an Microsoft gesendet werden.
- Jede Zeile entspricht einem bestimmten Schema, das durch **schemaVersion** identifiziert wird.
- Jede Zeile stellt eine Ausgabe einer SQLCEIP-Dienstsitzung dar, die als **sessionID** identifiziert wird.
- Zeilen werden in Folge ausgegeben und durch **sequence** identifiziert.
- Jede Datenpunktzeile enthält die Ausgabe für ein **queryIdentifier**, wobei es sich um eine T-SQL-Abfrage, eine XE-Sitzung oder eine handeln kann, die mit einer Art von Ablaufverfolgung verknüpft ist, die als **traceName** identifiziert wird.
- **queryIdentifiers** werden zusammen mit **querySetVersion** gruppiert und erhalten eine Versionsangabe.
- **data** enthält die Ausgabe der entsprechenden Abfrageausführung, die **queryTimeInTicks** benötigt hat.
- Für **queryIdentifiers** für T-SQL-Abfragen ist die T-SQL-Abfragedefinition in der Abfrage gespeichert.

| Logische Informationshierarchie für die lokale Überwachung | Verbundene Spalten |
| ------ | -------|
| Header | emitTime, schemaVersion 
| Machine | operatingSystem 
| Instanz | instanceUniqueID, correlationID, clientVersion 
| Sitzung | sessionID, traceName 
| Abfrage | sequence, querySetVersion, queryIdentifier, query, queryTimeInTicks 
| Daten |  data 

### <a name="namevalue-pairs-definition-and-examples"></a>Definition und Beispiele für Name-Wert-Paare 

Die nachfolgend aufgeführten Spalten stellen die Reihenfolge für die Dateiausgabe der lokalen Überwachung dar. Unidirektionaler Hash mit SHA-256 dient zum Anonymisieren von Werten für eine Reihe der nachfolgenden Spalten.  

| Name | BESCHREIBUNG | Beispielwerte
|-------|--------| ----------|
|instanceUniqueID| Anonymisierter Instanzbezeichner | 888770C4D5A8C6729F76F33D472B28883AE518C92E1999888B171A085059FD 
|schemaVersion| Schemaversion von SQLCEIP |  3 
|emitTime |Ausgabezeit für Datenpunkt in UTC | 2016-09-08T17:20:22.1124269Z 
|sessionID | Sitzungsbezeichner für SQLCEIP-Dienst | 89decf9a-ad11-485c-94a7-fefb3a02ed86 
|correlationId | Platzhalter für einen zusätzlichen Bezeichner | 0 
|sequence | Sequenznummer der Datenpunkte, die innerhalb der Sitzung gesendet wurden | 15 
|clientVersion | SQL Server-Instanzversion | 13.0.2161.3 ((SQL16_RTM_QFE-CU).160907-1223) 
|operatingSystem | Betriebssystemversion, auf der die SQL Server-Instanz installiert ist | Microsoft Windows Server 2012 R2 Datacenter 
|querySetVersion | Version einer Gruppe von Abfragedefinitionen | 1.0.0.0 
|traceName | Ablaufverfolgungskategorien: (SQLServerXeQueries, SQLServerPeriodicQueries, SQLServerOneSettingsException) | SQLServerPeriodicQueries 
|queryIdentifier | Ein Bezeichner der Abfrage | SQLServerProperties.002 
|data   | Die Ausgabe der für queryIdentifier erfassten Informationen als Ausgabe einer T-SQL-Abfrage, XE-Sitzung oder der Anwendung |  [{"Collation": "SQL_Latin1_General_CP1_CI_AS","SqlFTinstalled": "0" "SqlIntSec": "1","IsSingleUser": "0","SqlFilestreamMode": "0","SqlPbInstalled": "0","SqlPbNodeRole": "","SqlVersionMajor": "13","SqlVersionMinor": "0","SqlVersionBuild": "2161","ProductBuildType": "","ProductLevel": "RTM","ProductUpdateLevel": "CU2","ProductUpdateReference": "KB3182270","ProductRevision": "3","SQLEditionId": "-1534726760","IsClustered": "0","IsHadrEnabled": "0","SqlAdvAInstalled": "0","PacketReceived": "1210","Version": "Microsoft SQL Server 2016 (RTM-CU2) (KB3182270) - 13.0.2161.3 (X64) \n\tSep  7 2016 14:24:16 \n\tCopyright (c) Microsoft Corporation\n\tStandard Edition (64-Bit) unter Windows Server 2012 R2 Datacenter 6.3 \u003cX64\u003e (Build 9600: ) (Hypervisor)\n"}],
|Abfrage| Die auf queryIdentifier bezogene T-SQL-Abfragedefinition, die Daten erzeugt, falls zutreffend.        Diese Komponente wird nicht vom SQL Server CEIP-Dienst hochgeladen. Sie ist in der lokalen Überwachung nur als Referenz für Kunden enthalten.| SELECT\n      SERVERPROPERTY(\u0027Collation\u0027) AS [Collation],\n      SERVERPROPERTY(\u0027IsFullTextInstalled\u0027) AS [SqlFTinstalled],\n      SERVERPROPERTY(\u0027IsIntegratedSecurityOnly\u0027) AS [SqlIntSec],\n      SERVERPROPERTY(\u0027IsSingleUser\u0027) AS [IsSingleUser],\n      SERVERPROPERTY (\u0027FileStreamEffectiveLevel\u0027) AS [SqlFilestreamMode],\n      SERVERPROPERTY(\u0027IsPolyBaseInstalled\u0027) AS [SqlPbInstalled],\n      SERVERPROPERTY(\u0027PolyBaseRole\u0027) AS [SqlPbNodeRole],\n      SERVERPROPERTY(\u0027ProductMajorVersion\u0027) AS [SqlVersionMajor],\n      SERVERPROPERTY(\u0027ProductMinorVersion\u0027) AS [SqlVersionMinor],\n      SERVERPROPERTY(\u0027ProductBuild\u0027) AS [SqlVersionBuild],\n      SERVERPROPERTY(\u0027ProductBuildType\u0027) AS ProductBuildType,\n      SERVERPROPERTY(\u0027ProductLevel\u0027) AS ProductLevel,\n      SERVERPROPERTY(\u0027ProductUpdateLevel\u0027) AS ProductUpdateLevel,\n      SERVERPROPERTY(\u0027ProductUpdateReference\u0027) AS ProductUpdateReference,\n      RIGHT(CAST(SERVERPROPERTY(\u0027ProductVersion\u0027) AS NVARCHAR(30)),CHARINDEX(\u0027.\u0027, REVERSE(CAST(SERVERPROPERTY(\u0027ProductVersion\u0027) AS NVARCHAR(30)))) - 1) AS ProductRevision,\n      SERVERPROPERTY(\u0027EditionID\u0027) AS SQLEditionId,\n      SERVERPROPERTY(\u0027IsClustered\u0027) AS IsClustered,\n      SERVERPROPERTY(\u0027IsHadrEnabled\u0027) AS IsHadrEnabled,\n      SERVERPROPERTY(\u0027IsAdvancedAnalyticsInstalled\u0027) AS [SqlAdvAInstalled],\n      @@PACK_RECEIVED AS PacketReceived,\n      @@VERSION AS Version
|queryTimeInTicks | Die Dauer zum Ausführen der Abfrage mit der folgenden Ablaufverfolgungskategorie: (SQLServerXeQueries, SQLServerPeriodicQueries) |  0 
 
### <a name="trace-categories"></a>Ablaufverfolgungskategorien 
Derzeit werden die folgenden Ablaufverfolgungskategorien erfasst: 

- **SQLServerXeQueries**: Enthält Datenpunkte, die über die Sitzung für erweiterte Ereignisse erfasst wurden.
- **SQLServerPeriodicQueries**: Enthält Datenpunkte, die über regelmäßige Abfragen erfasst werden, die in einer SQL Server-Instanz ausgeführt werden.
- **SQLServerPerDBPeriodicQueries**: Enthält Datenpunkte, die über regelmäßige Abfragen erfasst werden, die in einer SQL Server-Instanz für bis zu 30 Datenbanken ausgeführt werden.
- **SQLServerOneSettingsException**: Enthält Ausnahmenachrichten hinsichtlich der Aktualisierung von Schema- und/oder Abfragesätzen.
- **DigitalProductID**: Enthält Datenpunkte zum Aggregieren anonymisierter (SHA-256) digitaler Produkt-IDs mit Hash von SQL Server-Instanzen. 

### <a name="local-audit-file-examples"></a>Dateibeispiele für die lokale Überwachung



Es folgt ein Auszug aus einer JSON-Dateiausgabe der lokalen Überwachung.

```JSON
[
  {
    "instanceUniqueId": "888770C4D5A8C6729F76F33D472B28883AE518C92E1999888B171A085059FD",
    "isSSEIInstance": "0",
    "schemaVersion": "5",
    "emitTime": "2018-05-04T15:27:59.7031518Z",
    "sessionId": "c3cd1b56-ab61-462f-8363-8881779aa223",
    "correlationId": 0,
    "sequence": 18,
    "clientVersion": "14.0.3025.34 ((SQLServer2017-CU6).180410-0033)",
    "isInternalMachine": "1",
    "operatingSystem": "Microsoft Windows 10 Enterprise",
    "querySetVersion": "14.0.3025.34",
    "traceName": "SQLServerPeriodicQueries",
    "queryIdentifier": "SQLServerProperties.002",
    "data": [
      {
        "Collation": "SQL_Latin1_General_CP1_CI_AS",
        "SqlFTinstalled": "0",
        "SqlIntSec": "1",
        "IsSingleUser": "0",
        "SqlFilestreamMode": "2",
        "SqlPbInstalled": "1",
        "SqlPbNodeRole": "Head",
        "SqlVersionMajor": "14",
        "SqlVersionMinor": "0",
        "SqlVersionBuild": "3025",
        "ProductBuildType": "",
        "ProductLevel": "RTM",
        "ProductUpdateLevel": "CU6",
        "ProductUpdateReference": "KB4101464",
        "ProductRevision": "34",
        "SQLEditionId": "1872460670",
        "IsClustered": "0",
        "IsHadrEnabled": "0",
        "SqlAdvAInstalled": "1",
        "PacketReceived": "422",
        "Version": "Microsoft SQL Server 2017 (RTM-CU6) (KB4101464) - 14.0.3025.34 (X64) \n\tApr  9 2018 18:00:41 \n\tCopyright (C) 2017 Microsoft Corporation\n\tEnterprise Edition: Core-based Licensing (64-bit) on Windows 10 Enterprise 10.0 <X64> (Build 16299: )\n"
      }
    ],
    "query": "SELECT\n      SERVERPROPERTY('Collation') AS [Collation],\n      SERVERPROPERTY('IsFullTextInstalled') AS [SqlFTinstalled],\n      SERVERPROPERTY('IsIntegratedSecurityOnly') AS [SqlIntSec],\n      SERVERPROPERTY('IsSingleUser') AS [IsSingleUser],\n      SERVERPROPERTY ('FileStreamEffectiveLevel') AS [SqlFilestreamMode],\n      SERVERPROPERTY('IsPolyBaseInstalled') AS [SqlPbInstalled],\n      SERVERPROPERTY('PolyBaseRole') AS [SqlPbNodeRole],\n      SERVERPROPERTY('ProductMajorVersion') AS [SqlVersionMajor],\n      SERVERPROPERTY('ProductMinorVersion') AS [SqlVersionMinor],\n      SERVERPROPERTY('ProductBuild') AS [SqlVersionBuild],\n      SERVERPROPERTY('ProductBuildType') AS ProductBuildType,\n      SERVERPROPERTY('ProductLevel') AS ProductLevel,\n      SERVERPROPERTY('ProductUpdateLevel') AS ProductUpdateLevel,\n      SERVERPROPERTY('ProductUpdateReference') AS ProductUpdateReference,\n      RIGHT(CAST(SERVERPROPERTY('ProductVersion') AS NVARCHAR(30)),CHARINDEX('.', REVERSE(CAST(SERVERPROPERTY('ProductVersion') AS NVARCHAR(30)))) - 1) AS ProductRevision,\n      SERVERPROPERTY('EditionID') AS SQLEditionId,\n      SERVERPROPERTY('IsClustered') AS IsClustered,\n      SERVERPROPERTY('IsHadrEnabled') AS IsHadrEnabled,\n      SERVERPROPERTY('IsAdvancedAnalyticsInstalled') AS [SqlAdvAInstalled],\n      @@PACK_RECEIVED AS PacketReceived,\n      @@VERSION AS Version",
    "queryTimeInTicks": 0
  },
  {
    "instanceUniqueId": "8884F770C4D5A8C6729F76F33D472B28883AE518C92E1999888B171A085059FD",
    "isSSEIInstance": "0",
    "schemaVersion": "5",
    "emitTime": "2018-05-04T15:28:00.9025999Z",
    "sessionId": "c3cd1b56-ab61-462f-8363-8881779aa223",
    "correlationId": 0,
    "sequence": 23,
    "clientVersion": "14.0.3025.34 ((SQLServer2017-CU6).180410-0033)",
    "isInternalMachine": "1",
    "operatingSystem": "Microsoft Windows 10 Enterprise",
    "querySetVersion": "14.0.3025.34",
    "traceName": "SQLServerPeriodicQueries",
    "queryIdentifier": "OsSysInfo.003",
    "data": [
      {
        "LogicalCPUCount": "8",
        "HyperthreadRatio": "8",
        "PhysicalMemoryMB": "32710.902343",
        "SQLServerStartTime": "05/04/2018 08:22:30",
        "AffinityTypeDesc": "AUTO",
        "VirtualMachineType": "0",
        "SocketCount": "1",
        "CoresPerSocket": "4",
        "NumaNodeCount": "1",
        "ContainerType": "0",
        "ContainerDescription": "NONE"
      }
    ],
    "query": "SELECT\n      cpu_count AS LogicalCPUCount,\n      hyperthread_ratio AS HyperthreadRatio,\n      physical_memory_kb/1024.0 AS PhysicalMemoryMB,\n      sqlserver_start_time AS SQLServerStartTime,\n      affinity_type_desc AS AffinityTypeDesc,\n      virtual_machine_type AS VirtualMachineType,\n      socket_count as SocketCount,\n      cores_per_socket as CoresPerSocket,\n      numa_node_count as NumaNodeCount,\n      container_type as ContainerType,\n      container_type_desc as ContainerDescription\n      FROM sys.dm_os_sys_info WITH(nolock)",
    "queryTimeInTicks": 0
  }
]
```
## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

**Wie lesen DBAs die Protokolldateien der lokalen Überwachung?**
Diese Protokolldateien werden im JSON-Format geschrieben. Jede Zeile ist ein JSON-Objekt, das Nutzungs-/Diagnosedaten darstellt, die zu Microsoft hochgeladen werden. Die Feldnamen sollten selbsterklärend sein.

**Was geschieht, wenn der DBA die Nutzungs- und Diagnosedatensammlung deaktiviert?**
Es wird keine Datei für die lokale Überwachung geschrieben.

**Was geschieht, wenn keine Verbindung mit dem Internet besteht bzw. der Computer sich hinter der Firewall befindet?**
Nutzungs- und Diagnosedaten von SQL Server werden nicht an Microsoft gesendet. Es wird weiterhin versucht, die Protokolle der lokalen Überwachung zu schreiben, wenn dies ordnungsgemäß konfiguriert wurde.

**Wie deaktivieren DBAs die lokale Überwachung?**
Entfernen Sie den Registrierungsschlüsseleintrag „UserRequestedLocalAuditDirectory“.

**Wer kann die Protokolldateien der lokalen Überwachung lesen?**
Jeder in Ihrem Unternehmen mit Zugriff auf das Verzeichnis für die lokale Überwachung.

**Wie verwalten DBAs die Protokolldateien, die in das vorgesehene Verzeichnis geschrieben werden?**
Das Bereinigen der Dateien im Verzeichnis muss von den Datenbankadministratoren selbst verwaltet werden, damit die Verwendung von zu viel Speicherplatz vermieden wird.

**Gibt es einen Client oder ein Tool, mit dem diese JSON-Ausgabe gelesen werden kann?**
Die Ausgabe kann mit Editor, Visual Studio oder einem beliebigen JSON-Reader gelesen werden.
Alternativ können Sie die JSON-Datei lesen und die Daten in einer SQL Server-Instanz analysieren, wie unten dargestellt. Weitere Informationen zum Lesen von JSON-Datei in SQL Server finden Sie unter [Importieren von JSON-Dateien in SQL Server mithilfe von OPENROWSET (BULK) und OPENJSON (Transact-SQL)](/archive/blogs/sqlserverstorageengine/bulk-importing-json-files-into-sql-server).

```Transact-SQL
DECLARE @JSONFile AS VARCHAR(MAX)

-- Read the JSON file into variable 
SELECT @JSONFile = BulkColumn 
FROM OPENROWSET (BULK 'C:\SQLCEIPAudit\MSSQLSERVER\2016-09-08.json', SINGLE_CLOB) MyFile 

-- Check if the JSON file has been read properly and if it's in a JSON format
SELECT 
    @JSONFile LocalAuditOutput, 
    ISJSON(@JSONFile) IsFileInJSONFormat

-- Get the query identifier, query and the data (output of the query)   
SELECT 
    sequence,
    queryIdentifier,
    query,
    data
FROM OPENJSON(@JSONFile) 
    WITH (sessionId VARCHAR(64)
         ,sequence INT
         ,queryIdentifier VARCHAR(128)
         ,query VARCHAR(MAX)
         ,data NVARCHAR(MAX) AS JSON)
-- Get specific details about the output of "DatabaseProperties.001" query  
SELECT 
    QueryIdentifier,
    DatabaseID,
    CompatibilityLevel,
    IsQueryStoreOn
FROM OPENJSON(@JSONFile) 
    WITH (sessionId VARCHAR(64)
         ,sequence INT
         ,queryIdentifier VARCHAR(128)
         ,query VARCHAR(MAX)
         ,data NVARCHAR(MAX) AS JSON) 
    CROSS APPLY OPENJSON(data) 
        WITH (   DatabaseID varchar(128) '$.database_id'
                ,CompatibilityLevel varchar(128) '$.compatibility_level'
                ,IsQueryStoreOn varchar(128) '$.QS'
             )
WHERE queryIdentifier = 'DatabaseProperties.001'
```

## <a name="see-also"></a>Weitere Informationen:
[Lokale Überwachung für SSMS-Nutzungs- und -Diagnosedatensammlung](../ssms/sql-server-management-studio-telemetry-ssms.md)