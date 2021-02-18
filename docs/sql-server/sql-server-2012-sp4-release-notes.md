---
title: Versionsanmerkungen zu SQL Server 2012 Service Pack | Microsoft-Dokumentation
description: Dieser Artikel enthält die aggregierten Versionshinweise zu den vier Service Packs für SQL Server 2012. Jedes Service Pack baut auf den vorherigen Service Packs auf.
ms.prod: sql
ms.technology: release-landing
ms.custom: ''
ms.date: 07/22/2020
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: 67cb8b3e-3d82-47f4-840d-0f12a3bff565
author: rothja
ms.author: jroth
monikerRange: = sql-server-2016
ms.openlocfilehash: 83a7828c93d6863e99d88d3cce45654e8f73abbd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341119"
---
# <a name="sql-server-2012-service-pack-release-notes"></a>Anmerkungen zu dieser Version von SQL Server 2012 Service Pack
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]
Dieses Thema enthält die aggregierten Versionsanmerkungen zu den vier Service Packs für SQL Server 2012. Jedes Service Pack baut auf den vorherigen Service Packs auf.

Die Service Packs sind ausschließlich online und nicht auf dem Installationsmedium verfügbar, Sie können sie wie folgt herunterladen:
- [SQL Server 2012 SP4](https://go.microsoft.com/fwlink/?linkid=846937)
- [SQL Server 2012 SP3](https://support.microsoft.com/help/3072779/sql-server-2012-service-pack-3-release-information)
- [SQL Server 2012 SP2](https://support.microsoft.com/KB/2958429)
- [SQL Server 2012 SP1](https://go.microsoft.com/fwlink/p/?LinkID=268158)

## <a name="service-pack-4-release-notes"></a>Versionsanmerkungen zu Service Pack 4

### <a name="download-pages"></a>Downloadseiten

- [SQL Server 2012 SP4 Feature Pack](https://www.microsoft.com/download/details.aspx?id=56041)
- [SQL Server 2012 SP4 Patchinstallation](https://www.microsoft.com/download/details.aspx?id=56040)
- [SQL Server 2012 SP4 Express](https://www.microsoft.com/download/details.aspx?id=56042)


### <a name="performance-and-scale-improvements"></a>Verbesserungen der Leistung und Skalierung
- **Verbesserte Prozedur zum Cleanup des Verteilungs-Agents:** Aufgrund einer zu großen Verteilungsdatenbank kam es zu Blockierungen und Deadlocks. Im Rahmen einer verbesserten Cleanup-Prozedur sollen einige davon beseitigt werden. 
- **Dynamische Skalierung des Speicherobjekts:** Partitionieren Sie Speicherobjekte dynamisch auf der Grundlage der Anzahl von Knoten und Kernen, um sie auf moderner Hardware zu skalieren. Mithilfe der dynamischen Promotion sollen potentielle Engpässe vermieden und threadsichere Speicherobjekte automatisch partitioniert werden. Unpartitionierte Speicherobjekte können dynamisch höher gestuft werden, um anhand der Knoten partitioniert zu werden. Die Anzahl der Partitionen entspricht der Anzahl der NUMA-Knoten. Speicherobjekte, die anhand der Knoten partitioniert werden, können noch höher gestuft werden, um anhand der CPU partitioniert zu werden. Dabei entspricht die Anzahl der Partitionen der Anzahl der CPU.
- **Aktivieren Sie > 8TB for Buffer Pool (> 8TB für Pufferpool):** Aktivieren Sie 128-TB virtuellen Adressraum für die Verwendung des Pufferpools.
- **Cleanup der Änderungsnachverfolgung**: Verbesserte Leistung und Effizienz des Cleanup der Änderungsnachverfolgung für Nebentabellen bei der Änderungsnachverfolgung. 

### <a name="supportability-and-diagnostics-improvements"></a>Verbesserungen der Unterstützung und Diagnose
- **Unterstützung von vollständigen Speicherabbildungen für Replikations-Agents:** Wenn Replikations-Agents derzeit auf Ausnahmefehler stoßen, erstellen sie standardmäßig ein Miniabbild der Ausnahmesymptome. Standardmäßig sind bei Ausnahmefehlern komplexe Problembehandlungsvorgänge erforderlich. Mit SP4 wird ein neuer Registrierungsschlüssel eingeführt, der die Erstellung eines vollständigen Speicherabbilds für Replikations-Agents unterstützt.
- **Erweiterte Diagnose in Showplan XML:** Showplan XML wurde dahingehend erweitert, dass nun Informationen über aktivierte Ablaufverfolgungsflags, Teile des Arbeitsspeichers zur Optimierung des Joins geschachtelter Schleifen sowie über die CPU-Zeit und die verstrichene Zeit zur Verfügung stehen. 
- **Verbesserte Korrelation zwischen XE- und DMV-Diagnosen:** Query_hash- und query_plan_hash-Felder werden verwendet, um Abfragen eindeutig zu identifizieren. Sie werden von der DMV als varbinary(8) und von XEvent als UINT64 definiert. Da der SQL Server nicht über „unsignierten bigint“ verfügt, können Umwandlungen nicht immer erfolgreich vorgenommen werden. Mit dieser Verbesserung werden neue Aktions- bzw. Filterspalten für XEvent eingeführt, die query_hash und query_plan_hash weitestgehend entsprechen, es sei denn, sie werden als INT64 definiert, wodurch Abfragen zwischen XE und DMV besser korreliert werden können. 
- **Bessere Diagnose der Speicherzuweisung und -auslastung:** Neuer query_memory_grant_usage XEvent (Backport von Server 2016 SP1)
- **Hinzufügen einer Protokollnachverfolgung zu Schritten der SSL-Aushandlung:** Fügen Sie bitweise Ablaufverfolgungsinformationen für erfolgreiche bzw. fehlgeschlagene Aushandlungen, einschließlich Protokolle, hinzu. Dies kann bei der Problembehandlung von Konnektivitätsszenarios hilfreich sein, wenn beispielsweise TLS 1.2 bereitgestellt wird.
- **Festlegen eines angemessenen Kompatibilitätsgrads für die Verteilungsdatenbank :** Nach der Installation des Service Packs ändert sich der Kompatibilitätsgrad der Verteilungsdatenbank auf 90. Die Änderung des Kompatibilitätsgrads ist auf ein Problem bei der gespeicherten Prozedur sp_vupgrade_replication zurückzuführen. Das Service Pack wurde nun dahingehend geändert, dass es nun einen angemessenen Kompatibilitätsgrad für die Verteilungsdatenbank bestimmt. 
- **Neuer Datenbankkonsolenbefehl (DBCC) zum Klonen einer Datenbank:** „Clone database“ (Datenbank klonen) ist ein neuer DBCC-Befehl, mit dem es Hauptbenutzern wie CSS ermöglicht wird, Probleme mit bestehenden Produktionsdatenbanken zu behandeln, indem Schemata und Metadaten ohne die Daten geklont werden. Der Aufruf wird mit der DBCC-Klondatenbank (‘source_database_name’, ‘clone_database_name’) ausgeführt. Geklonte Datenbanken dürfen nicht in Produktionsumgebungen verwendet werden. Verwenden Sie den folgenden Befehl, wenn Sie überprüfen möchten, ob eine Datenbank nach dem Aufruf zum Klonen generiert wurde: Wählen Sie DATABASEPROPERTYEX('clonedb', 'isClone') (DATABASEPROPERTYEX('clonedb', 'isClone')) aus. Der Rückgabewert 1 bedeutet TRUE und 0 FALSE. 
- **Informationen zur TempDB-Datei und Dateigröße im SQL-Fehlerprotokoll:** Drucken Sie die Anzahl der Dateien aus, und lösen Sie eine Warnung aus, wenn sich die Größe und die automatische Vergrößerung von TempDB-Dateien beim Starten unterscheiden.
- **IFI-Supportnachrichten im SQL Server-Fehlerprotokoll:** Geben Sie im Fehlerprotokoll an, dass die schnelle Datenbankdateiinitialisierung aktiviert bzw. deaktiviert ist.
- **Neue DMF zum Ersetzen des DBCC INPUTBUFFER:** Eine neue dynamische Verwaltungsfunktion, sys.dm_input_buffer, die session_id als Parameter verwendet, wird eingeführt, um den DBCC INPUTBUFFER zu ersetzen.
- **Erweiterung für XEvents zum Lesen von Routingfehlern für eine Verfügbarkeitsgruppe:** Derzeit wird read_only_rout_fail XEvent nur ausgelöst, wenn zwar eine Routingliste besteht, jedoch zu keinem der Server auf dieser Liste eine Verbindung hergestellt werden kann. Diese Verbesserung beinhaltet zusätzliche Informationen zur Unterstützung bei der Problembehandlung und Erweiterungen zu den Codepunkten, bei denen XEvent ausgelöst wird. 
- **Verbesserte Handhabung von Service Broker mit Failover einer Verfügbarkeitsgruppe:** Wenn Service Broker für Datenbanken der Verfügbarkeitsgruppe derzeit aktiviert ist, bleiben bei einem Failover der Verfügbarkeitsgruppe alle Service Broker-Verbindungen, die dem primären Replikat entstammen, geöffnet. Mit dieser Verbesserung werden all diese offenen Verbindungen während eines Failover der Verfügbarkeitsgruppe geschlossen.
- **Partitionierung mit der automatischen Soft-NUMA**: Mit der Version SQL 2014 SP2 wird die Partitionierung mit der automatischen [Soft-NUMA](../database-engine/configure-windows/soft-numa-sql-server.md) eingeführt, wenn das Ablaufverfolgungsflag 8079 auf Serverebene aktiviert ist. Wenn das Ablaufverfolgungsflag 8079 beim Starten aktiviert ist, fragt SQL Server 2014 SP2 das Hardwarelayout ab und konfiguriert soft-NUMA automatisch auf Systemen, die mindestens acht CPU pro NUMA-Knoten vermelden. Die automatische soft-NUMA ist Hyperthread-fähig (HT/logischer Prozessor). Durch die Partitionierung und Erstellung von weiteren Knoten wird die Hintergrundverarbeitung skaliert, indem die Anzahl von Listenern, Skalierungen sowie Netzwerk- und Verschlüsselungsfunktionen erhöht wird. Wir empfehlen Ihnen, die Leistung der Arbeitsauslastung mit der automatischen soft-NUMA vor der Aktivierung im Rahmen der Produktion zunächst zu testen.

## <a name="service-pack-3-release-notes"></a>Versionsanmerkungen zu Service Pack 3

### <a name="download-pages"></a>Downloadseiten
- [SQL Server 2012 SP3 Feature Pack](https://go.microsoft.com/fwlink/?linkid=615935)
- [SQL Server 2012 SP3 Express](https://go.microsoft.com/fwlink/?linkid=692144)

Genauere Informationen zum Identifizieren des Speicherorts und Namens der basierend auf Ihrer aktuell installierten Version zu herunterladenden Datei finden Sie im Abschnitt „Select the correct file to download“ (Auswählen der richtigen Datei zum Herunterladen) unter [SQL Server 2012 Service Pack 3 release information (Versionsinformationen zu SQL Server 2012 Service Pack 3)](https://support.microsoft.com/help/3072779/sql-server-2012-service-pack-3-release-information).

## <a name="service-pack-2-release-notes"></a>Versionsanmerkungen zu Service Pack 2
  
### <a name="download-pages"></a>Downloadseiten 
- [SQL Server 2012 SP2 Feature Pack](https://www.microsoft.com/download/details.aspx?id=43340)
- [SQL Server 2012 SP2 Express](https://go.microsoft.com/fwlink/?LinkID=401007)

Identifizieren Sie anhand der unten stehenden Tabelle und entsprechend der derzeit installierten Version den Speicherort und den Namen der herunterzuladenden Datei. Downloadseiten enthalten Systemanforderungen und grundlegende Installationsanweisungen.  

|Aktuell installierte Version...|Gewünschter Vorgang...|Download und Installation von...|  
|---|---|---|   
|32-Bit-Installationen:|||  
|32-Bit-Version einer beliebigen Edition von SQL Server 2012|Upgrade auf die 32-Bit-Version von SQL Server 2012 SP2|**SQLServer2012SP2-KB2958429-** <arch> **-** <lang id> **.exe** von der [Downloadseite für SQL Server 2012 SP2](https://go.microsoft.com/fwlink/?LinkID=401006)|  
|Eine 32-Bit-Version von SQL Server 2012 RTM Express|Upgrade auf die 32-Bit-Version von SQL Server 2012 Express SP2|**SQLEXPR_** <arch> **_** <lang> **.msi** von der [Downloadseite für SQL Server 2012 SP2 Express](https://go.microsoft.com/fwlink/?LinkID=401007)|  
|32-Bit-Version nur der Client- und Verwaltbarkeitstools für SQL Server 2012 (einschließlich SQL Server 2012 Management Studio)|Upgrade der Client- und Verwaltbarkeitstools auf die 32-Bit-Version von SQL Server 2012 SP2|**SQLEXPRWT_** <arch> **_** <lang> **.msi** von der [Downloadseite für SQL Server 2012 SP2 Express](https://go.microsoft.com/fwlink/?LinkID=401007)|  
|Eine 32-Bit-Version von SQL Server 2012 Management Studio Express|Upgrade auf die 32-Bit-Version von SQL Server 2012 SP2 Management Studio Express|**SQLManagementStudio_** <arch> **_** <lang> **.msi** von der [Downloadseite für SQL Server 2012 SP2 Express](https://go.microsoft.com/fwlink/?LinkID=401007)|  
|32-Bit-Version einer beliebigen Edition von SQL Server 2012 und 32-Bit-Version der Client- und Verwaltbarkeitstools (einschließlich SQL Server 2012 RTM Management Studio)|Upgrade aller Produkte auf die 32-Bit-Version von SQL Server 2012 SP2|**SQLEXPRADV_** <arch> **_** <lang> **.msi** von der [Downloadseite für SQL Server 2012 SP2 Express](https://go.microsoft.com/fwlink/?LinkID=401007)|  
|32-Bit-Version eines oder mehrerer Tools des [Microsoft SQL Server 2012 RTM Feature Pack](https://www.microsoft.com/download/details.aspx?id=56041) oder des [Microsoft SQL Server 2012 SP1 Feature Pack](https://www.microsoft.com/download/details.aspx?id=35575)|Upgrade der Tools auf die 32-Bit-Version des Microsoft SQL Server 2012 SP2 Feature Pack|Eines oder mehrere Tools von der [Downloadseite für Microsoft SQL Server 2012 SP2 Feature Pack](https://www.microsoft.com/download/details.aspx?id=43340)|  
|64-Bit-Installationen:|||  
|64-Bit-Version einer beliebigen Edition von SQL Server 2012|Upgrade auf die 64-Bit-Version von SQL Server 2012 SP2|SQLServer2012SP2-KB2958429-<arch>-<langid>.exe von der [Downloadseite für SQL Server 2012 SP2](https://go.microsoft.com/fwlink/?LinkID=401006)|  
|64-Bit-Version von SQL Server 2012 RTM Express|Upgrade auf die 64-Bit-Version von SQL Server 2012 SP2|**SQLEXPR_** <arch> **_** <lang> **.msi** von der [Downloadseite für SQL Server 2012 SP2 Express](https://go.microsoft.com/fwlink/?LinkID=401007)|  
|64-Bit-Version ausschließlich der Client- und Verwaltbarkeitstools für SQL Server 2012 (einschließlich SQL Server 2012 Management Studio)|Upgrade der Client- und Verwaltbarkeitstools auf die 64-Bit-Version von SQL Server 2012 SP2|**SQLEXPRWT_** <arch> **_** <lang> **.msi** von der [Downloadseite für SQL Server 2012 SP2 Express](https://go.microsoft.com/fwlink/?LinkID=401007)|  
|64-Bit-Version von SQL Server 2012 Management Studio Express|Upgrade auf die 64-Bit-Version von SQL Server 2012 SP2 Management Studio Express|**SQLManagementStudio_** <arch> **_** <lang> **.msi** von der [Downloadseite für SQL Server 2012 SP2 Express](https://go.microsoft.com/fwlink/?LinkID=401007)|  
|64-Bit-Version eines oder mehrerer Tools des [Microsoft SQL Server 2012 RTM Feature Pack](https://www.microsoft.com/download/details.aspx?id=56041) oder des [Microsoft SQL Server 2012 SP1 Feature Pack](https://www.microsoft.com/download/details.aspx?id=35575)|Upgrade der Tools auf die 64-Bit-Version des Microsoft SQL Server 2012 SP2 Feature Pack|Eines oder mehrere Tools von der [Downloadseite für Microsoft SQL Server 2012 SP2 Feature Pack](https://www.microsoft.com/download/details.aspx?id=43340)|   


## <a name="service-pack-1-release-notes"></a>Versionsanmerkungen zu Service Pack 1

### <a name="download-pages"></a>Downloadseiten  
- [SQL Server 2012 SP1 Feature Pack](https://go.microsoft.com/fwlink/p/?LinkID=268158)
- [SQL Server 2012 SP1 Express](https://www.microsoft.com/download/details.aspx?id=35579)


Bestimmen Sie mithilfe der folgenden Tabelle, welche Datei heruntergeladen und installiert werden muss. Stellen Sie sicher, dass die Systemanforderungen erfüllt sind, bevor Sie das Service Pack installieren. Die Systemanforderungen sind auf den Downloadseiten angegeben, die mit den Links in der Tabelle verknüpft sind.  

|Aktuell installierte Version...|Gewünschter Vorgang...|Download und Installation von...|  
|---|---|---|  
|**32-Bit-Installationen:**|||  
|32-Bit-Version einer beliebigen Edition von SQL Server 2012|Upgrade auf die 32-Bit-Version von SQL Server 2012 SP1|SQLServer2012SP1-KB2674319-x86-ENU.exe über [diesen Link](https://go.microsoft.com/fwlink/p/?LinkID=268158)|  
|Eine 32-Bit-Version von SQL Server 2012 RTM Express|Upgrade auf die 32-Bit-Version von SQL Server 2012 Express SP1|SQLServer2012SP1-KB2674319-x86-ENU.exe über [diesen Link](https://go.microsoft.com/fwlink/p/?LinkID=268158)|  
|32-Bit-Version nur der Client- und Verwaltbarkeitstools für SQL Server 2012 (einschließlich SQL Server 2012 Management Studio)|Upgrade der Client- und Verwaltbarkeitstools auf die 32-Bit-Version von SQL Server 2012 SP1|SQLManagementStudio_x86_ENU.exe über [diesen Link](https://go.microsoft.com/fwlink/p/?LinkID=267905)|  
|Eine 32-Bit-Version von SQL Server 2012 Management Studio Express|Upgrade auf die 32-Bit-Version von SQL Server 2012 SP1 Management Studio Express|SQLManagementStudio_x86_ENU.exe über [diesen Link](https://go.microsoft.com/fwlink/p/?LinkID=267905)|  
|32-Bit-Version einer beliebigen Edition von SQL Server 2012 **und** 32-Bit-Version der Client- und Verwaltbarkeitstools (einschließlich SQL Server 2012 RTM Management Studio)|Upgrade aller Produkte auf die 32-Bit-Version von SQL Server 2012 SP1|SQLServer2012SP1-KB2674319-x86-ENU.exe über [diesen Link](https://go.microsoft.com/fwlink/p/?LinkID=268158)|  
|32-Bit-Version von mindestens einem Tool aus dem [Feature Pack für Microsoft SQL Server 2012 RTM](https://www.microsoft.com/download/details.aspx?id=44272)|Upgrade der Tools auf die 32-Bit-Version des Feature Packs für Microsoft SQL Server 2012 SP1|Mindestens eine Datei aus dem [Feature Pack für Microsoft SQL Server 2012 SP1](https://www.microsoft.com/download/details.aspx?id=35575)|  
|Keine 32-Bit-Installation von SQL Server 2012|Installation von Server 2012 in der 32-Bit-Version einschließlich SP1 (neue Instanz mit vorinstalliertem SP1)|SQLServer2012SP1-FullSlipstream-x86-ENU.exe **und** SQLServer2012SP1-FullSlipstream-x86-ENU.box über [diesen Link](https://go.microsoft.com/fwlink/p/?LinkID=268158)|  
|Keine 32-Bit-Installation von SQL Server 2012 Management Studio|Installation von SQL Server 2012 Management Studio in der 32-Bit-Version einschließlich SP1|SQLManagementStudio_x86_ENU.exe über [diesen Link](https://go.microsoft.com/fwlink/p/?LinkId=267905)|  
|Keine 32-Bit-Version von SQL Server 2012 RTM Express|Installation von SQL Server 2012 Express in der 32-Bit-Version einschließlich SP1|SQLEXPR32_x86_ENU.exe über [diesen Link](https://go.microsoft.com/fwlink/p/?LinkId=267905)|  
|Eine 32-Bit-Installation von **SQL Server 2008** oder **SQL Server 2008 R2**|**Direktes Upgrade** auf die 32-Bit-Version von SQL Server 2012 einschließlich SP1|SQLServer2012SP1-FullSlipstream-x86-ENU.exe **und** SQLServer2012SP1-FullSlipstream-x86-ENU.box über [diesen Link](https://go.microsoft.com/fwlink/p/?LinkID=268158)|  
|**64-Bit-Installationen:**|||  
|64-Bit-Version einer beliebigen Edition von SQL Server 2012|Upgrade auf die 64-Bit-Version von SQL Server 2012 SP1|SQLServer2012SP1-KB2674319-x64-ENU.exe über [diesen Link](https://go.microsoft.com/fwlink/p/?LinkID=268158)|  
|64-Bit-Version von SQL Server 2012 RTM Express|Upgrade auf die 64-Bit-Version von SQL Server 2012 SP1|SQLServer2012SP1-KB2674319-x64-ENU.exe über [diesen Link](https://go.microsoft.com/fwlink/p/?LinkID=268158)|  
|64-Bit-Version ausschließlich der Client- und Verwaltbarkeitstools für SQL Server 2012 (einschließlich SQL Server 2012 Management Studio)|Upgrade der Client- und Verwaltbarkeitstools auf die 64-Bit-Version von SQL Server 2012 SP1|SQLManagementStudio_x64_ENU.exe über [diesen Link](https://go.microsoft.com/fwlink/p/?LinkID=267905)|  
|64-Bit-Version von SQL Server 2012 Management Studio Express|Upgrade auf die 64-Bit-Version von SQL Server 2012 SP1 Management Studio Express|SQLManagementStudio_x64_ENU.exe über [diesen Link](https://go.microsoft.com/fwlink/p/?LinkID=267905)|  
|64-Bit-Version einer beliebigen Edition von SQL Server 2012 **und** 64-Bit-Version der Client- und Verwaltbarkeitstools (einschließlich SQL Server 2012 RTM Management Studio)|Upgrade aller Produkte auf die 64-Bit-Version von SQL Server 2012 SP1|SQLServer2012SP1-KB2674319-x64-ENU.exe über [diesen Link](https://go.microsoft.com/fwlink/p/?LinkID=268158)|  
|64-Bit-Version von mindestens einem Tool aus dem [Feature Pack für Microsoft SQL Server 2012 RTM](https://www.microsoft.com/download/details.aspx?id=44272)|Upgrade der Tools auf die 64-Bit-Version des Feature Packs für Microsoft SQL Server 2012 SP1|Mindestens eine Datei aus dem [Feature Pack für Microsoft SQL Server 2012 SP1](https://www.microsoft.com/download/details.aspx?id=35575)|  
|Keine 64-Bit-Installation von SQL Server 2012|Installation von Server 2012 in der 64-Bit-Version einschließlich SP1 (neue Instanz mit vorinstalliertem SP1)|SQLServer2012SP1-FullSlipstream-x64-ENU.exe **und** SQLServer2012SP1-FullSlipstream-x64-ENU.box über [diesen Link](https://go.microsoft.com/fwlink/p/?LinkID=268158)|  
|Keine 64-Bit-Installation von SQL Server 2012 Management Studio|Installation von SQL Server 2012 Management Studio in der 64-Bit-Version einschließlich SP1|SQLManagementStudio_x64_ENU.exe über [diesen Link](https://go.microsoft.com/fwlink/p/?LinkID=267905)|  
|Keine 64-Bit-Version von SQL Server 2012 RTM Express|Installation von SQL Server 2012 Express in der 64-Bit-Version einschließlich SP1|SQLEXPR_x64_ENU.exe über [diesen Link](https://go.microsoft.com/fwlink/p/?LinkID=267905)|  
|Eine 64-Bit-Installation von **SQL Server 2008** oder **SQL Server 2008 R2**|**Direktes Upgrade** auf die 64-Bit-Version von SQL Server 2012 einschließlich SP1|SQLServer2012SP1-FullSlipstream-x64-ENU.exe **und** SQLServer2012SP1-FullSlipstream-x64-ENU.box über [diesen Link](https://go.microsoft.com/fwlink/p/?LinkID=268158)|  

### <a name="known-issues-fixed-in-this-service-pack"></a>Bekannte Probleme, die in diesem Service Pack behoben wurden  
Eine vollständige Liste von Fehlern und bekannten Problemen, die in diesem Service Pack behoben wurden, finden Sie in diesem [KB-Artikel](https://support.microsoft.com/kb/2674319).   

### <a name="reinstalling--instances-of-sql-server-failover-cluster-fails-if-you-use-the-same-ip-address"></a>Die Neuinstallation von SQL Server-Failoverclusterinstanzen verursacht einen Fehler, wenn dieselbe IP-Adresse verwendet wird  
**Problem:** Wenn Sie bei der Installation einer SQL Server-Failoverclusterinstanz eine falsche IP-Adresse angeben, tritt ein Fehler auf. Wenn Sie nach der Deinstallation der fehlerhaften Instanz versuchen, die SQL Server-Failoverclusterinstanz mit demselben Instanznamen und der richtigen IP-Adresse erneut zu installieren, schlägt die Installation fehl. Dies liegt daran, dass noch die doppelte Ressourcengruppe aus der vorherigen Installation vorhanden ist.  
  
**Problemumgehung:** Um dieses Problem zu beheben, verwenden Sie während der Neuinstallation einen anderen Instanznamen, oder löschen Sie die Ressourcengruppe vor der Neuinstallation manuell. Weitere Informationen finden Sie unter [Hinzufügen oder Entfernen von Knoten in einem SQL Server-Failovercluster](failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md). 
  
### <a name="analysis-services-and-powerpivot"></a>Analysis Services und PowerPivot  
  
##### <a name="powerpivot-configuration-tool-does-not-create-the-powerpivot-gallery"></a>Der PowerPivot-Katalog wird vom PowerPivot-Konfigurationstool nicht erstellt  
**Problem:** Das PowerPivot-Konfigurationstool stellt eine Teamwebsite bereit, und daher wird der PowerPivot-Katalog nicht erstellt.  
  
**Problemumgehung:** Erstellen Sie eine neue App (Bibliothek).  
  
1.  Überprüfen Sie, ob die Websitesammlungsfunktion **Funktion zur PowerPivot-Integration für Websitesammlungen** aktiv ist.  
  
2.  Klicken Sie auf der Seite **Websiteinhalt** einer vorhandenen Website auf **App hinzufügen**.  
  
3.  Klicken Sie auf **PowerPivot-Katalog**.  
  
#### <a name="to-use-powerpivot-for-excel-with-excel-2013-you-must-use-the-add-in-that-is-installed-with-excel"></a>Zur Verwendung von PowerPivot für Excel mit Excel 2013 müssen Sie das mit Excel installierte Add-In verwenden  
**Problem:** Bei Office 2010 ist PowerPivot für Excel ein eigenständiges Add-In, das von [https://www.microsoft.com/bi/powerpivot.aspx](https://www.microsoft.com/bi/powerpivot.aspx) heruntergeladen werden kann. Alternativ kann es auch vom [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=29074)heruntergeladen werden. Beachten Sie, dass zwei Versionen des PowerPivot-Add-Ins als Download verfügbar sind: Eine, die im Lieferumfang von SQL Server 2008 R2 enthalten ist, und eine andere, die im Lieferumfang von SQL Server 2012 enthalten ist. Im Falle von Office 2013 ist PowerPivot für Excel jedoch im Lieferumfang von Office enthalten und wird zusammen mit Excel installiert. Während die SQL Server 2008 R2- und SQL Server 2012-Versionen von PowerPivot für Excel 2010 nicht mit Excel 2013 kompatibel sind, können Sie weiterhin PowerPivot für Excel 2010 auf dem Clientcomputer installieren, wenn Sie Excel 2010 parallel zu Excel 2013 ausführen möchten. Da die beiden Excel-Versionen gleichzeitig vorhanden sein können, gilt dies auch für die entsprechenden PowerPivot-Add-Ins.  
  
**Problemumgehung:** Aktivieren Sie das COM-Add-In, um PowerPivot für Excel 2013 zu verwenden. Wählen Sie in Excel 2013 **Datei** | **Optionen** | **Add-Ins** aus. Wählen Sie im Dropdownfeld **Verwalten** die Option **COM-Add-Ins** aus, und klicken Sie auf **Ausführen**. Wählen Sie unter **COM-Add-Ins** die Option **Microsoft Office PowerPivot für Excel 2013** aus, und klicken Sie auf **OK**.  
  
### <a name="reporting-services"></a>Reporting Services  
  
#### <a name="install-and-configure-sharepoint-server-2013-prior-to-installing-reporting-services"></a>SharePoint Server 2013 muss vor der Installation von Reporting Services installiert und konfiguriert werden  
**Problem:** Führen Sie die folgenden erforderlichen Schritte durch, **bevor** Sie SQL Server Reporting Services (SSRS) installieren.  
  
1.  Führen Sie das Vorbereitungstool für SharePoint 2013-Produkte aus.  
  
2.  Installieren Sie SharePoint Server 2013.  
  
3.  Führen Sie den Konfigurations-Assistenten für SharePoint 2013-Produkte oder die entsprechenden Konfigurationsschritte aus, um die SharePoint-Farm zu konfigurieren.  
  
**Problemumgehung:**  Wenn Sie den [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] SharePoint-Modus vor der Konfiguration der SharePoint-Farm installiert haben, richtet sich die erforderliche Problemumgehung danach, welche weiteren Komponenten installiert sind.  
  
#### <a name="power-view-in-sharepoint-server-2013-requires-microsoftanalysisservicesspclientdll"></a>Power View erfordert „Microsoft.AnalysisServices.SPClient.dll“ in SharePoint Server 2013  
**Problem:** Die erforderliche Komponente **Microsoft.AnalysisServices.SPClient.dll** wird von [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] nicht installiert. Wenn Sie die Vorschauversion von SharePoint Server 2013 und [!INCLUDE[ssSQL11SP1](../includes/sssql11sp1-md.md)][!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] im SharePoint-Modus installieren, ohne das Installationspaket für PowerPivot für SharePoint 2013 **spPowerPivot.msi** herunterzuladen und zu installieren, ist Power View nicht funktionsfähig und zeigt folgende Symptome.  
  
**Symptome:** Beim Erstellen eines Power View-Berichts wird eine mit der folgenden Fehlermeldung vergleichbare Fehlermeldung ausgegeben:  
  
-   "Es kann keine Verbindung mit der Datenquelle hergestellt werden..."  
  
Die internen Fehlerdetails enthalten eine mit der folgenden vergleichbare Meldung:  
  
-   "Der Wert 'SharePoint Principal' wird für die User Identity-Eigenschaft der Verbindungszeichenfolge nicht unterstützt."  
  
**Problemumgehung:** Installieren Sie das Installationspaket für PowerPivot für SharePoint 2013 (**spPowerPivot.msi**) unter SharePoint Server 2013. Das Installationspaket ist als Teil des [!INCLUDE[ssSQL11SP1](../includes/sssql11sp1-md.md)] Feature Packs verfügbar. Das Feature Pack kann vom [!INCLUDE[msCoName](../includes/msconame-md.md)] Download Center unter [SQL Server 2012 SP1 Feature Pack](https://www.microsoft.com/download/details.aspx?id=35575)heruntergeladen werden.  
  
#### <a name="power-view-sheets-in-a-powerpivot-workbook-are-deleted-after-a-scheduled-data-refresh"></a>Power View-Blätter in einer PowerPivot-Arbeitsmappe werden nach einer geplanten Datenaktualisierung gelöscht  
**Problem:** Wenn Sie im PowerPivot-Add-In für SharePoint **Scheduled Data Refresh** (Geplante Datenaktualisierung) für eine Power View-Arbeitsmappe verwenden, werden alle Power View-Blätter gelöscht.  
  
**Problemumgehung**: Erstellen Sie eine PowerPivot-Arbeitsmappe, die nur das Datenmodell enthält, um **Scheduled Data Refresh** (Geplante Datenaktualisierung) mit Power View-Arbeitsmappen zu verwenden. Erstellen Sie eine separate Arbeitsmappe mit Excel-Tabellen und Power View-Blättern, die mit der PowerPivot-Arbeitsmappe verknüpft ist, in der das Datenmodell enthalten ist. Nur die PowerPivot-Arbeitsmappe mit dem Datenmodell sollte für die geplante Datenaktualisierung verwendet werden.  
  
### <a name="data-quality-services"></a>Data Quality Services  
  
#### <a name="dqs-available-in-the-incorrect-edition-of-sql-server-2012"></a>DQS ist in der falschen Edition von SQL Server 2012 verfügbar  
**Problem:** Im [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] RTM-Release ist die Data Quality Services-Funktion (DQS) in SQL Server-Editionen verfügbar, die nicht Enterprise, Business Intelligence oder Developer sind. Nach der Installation von SQL Server 2012 SP1 ist DQS in allen Editionen außer Enterprise, Business Intelligence und Developer nicht verfügbar.  
  
**Problemumgehung**: Wenn Sie DQS in einer nicht unterstützten Edition verwenden, führen Sie entweder ein Upgrade auf eine unterstützte Edition aus, oder entfernen Sie die Abhängigkeit von dieser Funktion aus den Anwendungen.  
  
### <a name="sql-server-express"></a>SQL Server Express  
  
#### <a name="full-version-of-sql-server-management-studio-available-in-sql-server-2012-express-sp1"></a>In SQL Server 2012 Express SP1 ist die Vollversion von SQL Server Management Studio verfügbar  
Die SQL Server 2012 Express Service Pack 1 (SP1)-Version umfasst anstelle von SQL Server 2012 Management Studio Express die Vollversion von SQL Server 2012 Management Studio (die zuvor nur auf der SQL Server 2012-DVD verfügbar war). Unter [SQL Server 2012 Express Service Pack 1](https://go.microsoft.com/fwlink/p/?linkid=267905)können Sie SQL Server 2012 Express SP1 herunterladen und installieren.  
  
### <a name="change-data-capture-service-and-designer-for-oracle-by-attunity"></a>Change Data Capture Service und Designer für Oracle von Attunity  
  
#### <a name="upgrading-the-cdc-service-and-designer"></a>Aktualisieren von CDC Service und Designer  
**Problem:** Wenn zu dem Zeitpunkt, zu dem Sie SQL Server 2012 SP1 installieren, der Change Data Capture Designer für Oracle und Change Data Capture Service für Oracle von Attunity auf dem Computer installiert sind, werden diese Komponenten durch die Installation von SP1 nicht aktualisiert.  
  
**Problemumgehung:** So aktualisieren Sie die CDC-Komponenten auf die neueste Version:  
  
1.  Laden Sie die MSI-Dateien für den Change Data Capture Service für Oracle von Attunity von der [Downloadseite für das SQL Server 2012 SP1-Feature Pack](https://www.microsoft.com/download/details.aspx?id=35575)herunter.  
  
2.  Führen Sie die MSI-Datei aus.  
  
### <a name="sql-server-data-tier-application-framework-dacfx"></a>SQL Server Data-Tier Application Framework (DACFx)  
**Unterstützung für direkte Upgrades**  
  
Diese DACFx-Version (Data-Tier Application Framework) unterstützt direkte Upgrades von Vorgängerversionen. Somit müssen vorherige DACFx-Installationen vor dem Upgrade auf diese Version nicht entfernt werden. Zukünftige Versionen von DACFx finden Sie [hier](/previous-versions/sql/sql-server-2016/dn702988(v=sql.130)).  
  
**Unterstützung für den selektiven XML-Index**  
  
SQL Server 2012 SP1 bietet Unterstützung für den [selektiven XML-Index (SXI)](../relational-databases/xml/selective-xml-indexes-sxi.md), eine neue SQL Server-Funktion, die eine neuen leistungsfähigeren und effizienteren Ansatz zur Indizierung von XML-Spaltendaten darstellt.  
  
DACFx unterstützt jetzt SXI-Indizes in allen DAC-Szenarien und -Clienttools. SXI wird erst ab der neuesten SSDT-Version unterstützt. Die RTM-Version von SSDT und die SSDT-Version von September 2012 unterstützen kein SXI.  
  
**Unterstützung für das native BCP-Datenformat**  
  
Früher wurde das JSON-Datenformat verwendet, um Tabellendaten in DACPAC- und BACPAC-Paketen zu speichern. Ab diesem Update gewährleistet das systemeigene BCP-Format die Persistenz von Daten. Diese Neuerung bringt DACFx eine höhere Genauigkeit bei der Übernahme von SQL Server-Datentypen. Darüber hinaus schließt sie Unterstützung für SQL_Variant-Typen sowie eine verbesserte Datenbereitstellungsleistung bei umfangreichen Datenbanken ein.  
  
**Beibehaltung des Zustands von CHECK-Einschränkungen bei der Erstellung/Bereitstellung von Paketen**  
  
Früher wurde der Zustand von CHECK-Einschränkungen (WITH CHECK/NOCHECK), der für Tabellen im Datenbankschema definiert wurde, nicht beibehalten bzw. nicht in DACPAC-Dateien gespeichert. Dieses Verhalten kann zu Problemen bei der Paketbereitstellung führen, wenn CHECK-Einschränkungen durch bestehende Tabellendaten verletzt werden. Ab diesem Update speichert DACFx den aktuellen Zustand von CHECK-Einschränkungen in der DACPAC-Datei, wenn diese aus einer Datenbank extrahiert wird, und stellt den Zustand bei der Paketbereitstellung ordnungsgemäß wieder her.  
  
**Updates auf „SqlPackage.exe“ (DACFx-Befehlszeilentool)**  
  
-   Extrahieren einer DACPAC-Datei mit Daten: erstellt eine Datenbankmomentaufnahme (DACPAC-Datei) von einer SQL Server- oder Azure SQL-Livedatenbank, die zusätzlich zum Datenbankschema Daten aus Benutzertabellen enthält. Diese Pakete können mithilfe der Veröffentlichungsaktion „SqlPackage.exe“ auf einer neuen oder bestehenden SQL Server- oder Azure SQL-Datenbank veröffentlicht werden. Die im Paket enthaltenen Daten ersetzen die bestehenden Daten in der Zieldatenbank.  
  
-   Exportieren einer BACPAC-Datei: erstellt eine logische Sicherungsdatei (BACPAC) einer SQL Server- oder Azure SQL-Livedatenbank, die das Datenbankschema und die Benutzerdaten enthält. Diese können verwendet werden, um eine Datenbank von einer lokalen SQL Server-Datenbank zu einer Azure SQL-Datenbank zu migrieren. Mit Azure kompatible Datenbanken können exportiert und später zwischen unterstützten Versionen von SQL Server importiert werden.  
  
-   Importieren einer BACPAC-Datei: importiert eine BACPAC-Datei, um eine neue SQL Server- oder Azure SQL-Datenbank zu erstellen bzw. eine leere Datenbank mit Daten aufzufüllen.  
  
Eine vollständige Dokumentation zu SqlPackage.exe auf MSDN finden Sie [hier](../tools/sqlpackage/sqlpackage.md).  
  
**Paketkompatibilität**  
  
Mit dieser Version werden mehrere Szenarien für die Aufwärtskompatibilität von DAC-Paketen eingeführt.  
  
-   Mit dieser Version erstellte DAC-Pakete, die keine SXI-Elemente oder -Tabellendaten enthalten, können von DACFx-Vorgängerversionen (SQL Server 2012 RTM, SQL Server 2012 CU1 und DACFx von September 2012) verwendet werden.  
  
-   Alle von DACFx-Vorgängerversionen erstellten DAC-Pakete können von dieser Version verwendet werden.  
  
## <a name="see-also"></a>Weitere Informationen
- [Installieren von SQL Server 2012-Wartungsupdates](/previous-versions/sql/sql-server-2012/hh479746(v=sql.110))
- [So identifizieren Sie die SQL Server-Version und -Edition](https://support.microsoft.com/help/321185)
- [Installieren von SQL Server 2012-Wartungsupdates](/previous-versions/sql/sql-server-2012/hh479746(v=sql.110))
- [So identifizieren Sie die SQL Server-Version und -Edition](https://support.microsoft.com/help/321185) 
- [Ermitteln der Version und Edition von SQL Server](https://support.microsoft.com/kb/321185)  
- [Von den Editionen von SQL Server 2014 unterstützte Features](./editions-and-components-of-sql-server-2016.md)  

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]
