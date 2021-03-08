---
title: Installieren von PolyBase unter Windows
description: In diesem Artikel erfahren Sie, wie Sie PolyBase als Einzelknoten oder PolyBase-Erweiterungsgruppe installieren. Für die Installation können Sie einen Installations-Assistenten oder die Eingabeaufforderung verwenden. Schließlich müssen Sie PolyBase aktivieren.
ms.date: 02/05/2021
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
helpviewer_keywords:
- PolyBase, installation
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>= sql-server-2016'
ms.openlocfilehash: ca5c7911444dcc5cdd87787a80ee0d9e572797ef
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101838087"
---
# <a name="install-polybase-on-windows"></a>Installieren von PolyBase unter Windows

[!INCLUDE [SQL Server Windows Only ](../../includes/applies-to-version/sql-windows-only.md)]

Besuchen Sie [SQL Server Evaluation](https://www.microsoft.com/evalcenter/evaluate-sql-server-2016), um eine Testversion von SQL Server zu installieren. 
   
## <a name="prerequisites"></a>Voraussetzungen  
   
- Eine 64-Bit-Edition von SQL Server Evaluation  
   
- Microsoft .NET Framework 4.5.  

- Mindestgröße des Arbeitsspeichers: 4 GB 
   
- Mindestgröße des Festplattenspeichers: 2 GB
  
- Empfohlen: Mindestens 16 GB RAM
   
- Für PolyBase muss TCP/IP aktiviert sein, um ordnungsgemäß funktionieren zu können. TCP/IP ist standardmäßig in allen Editionen von SQL Server aktiviert. Davon ausgenommen sind nur die Editionen Developer und Express SQL Server. Damit PolyBase auch in den Developer- und Express-Versionen ordnungsgemäß funktionieren kann, müssen Sie die TCP/IP-Konnektivität aktivieren. Weitere Informationen finden Sie unter [Aktivieren oder Deaktivieren eines Servernetzwerkprotokolls](../../database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md).


>[!NOTE] 
> PolyBase kann nur auf einer SQL Server-Instanz pro Computer installiert werden.


>[!NOTE]
>Damit Sie PolyBase verwenden können, müssen Sie über Berechtigungen auf Systemadministrator- oder CONTROL SERVER-Ebene für die Datenbank verfügen.

>[!IMPORTANT]
>Wenn Sie die Weitergabeberechnungsfunktion für Hadoop verwenden möchten, muss der Hadoop-Zielcluster über die Kernkomponenten von HDFS (Hadoop Distributed File System), YARN und MapReduce verfügen. Dabei muss der Auftragsverlaufserver aktiviert sein. PolyBase übermittelt die Weitergabeabfrage über MapReduc und ruft den Status über den Auftragsverlaufserver ab. Wenn keine dieser Komponenten vorhanden ist, tritt bei der Abfrage ein Fehler auf.
  
## <a name="single-node-or-polybase-scale-out-group"></a>Einzelknoten oder PolyBase-Erweiterungsgruppe

Entscheiden Sie sich vor der Installation von PolyBase auf Ihren SQL Server-Instanzen zwischen einer Einzelknoteninstallation und einer [PolyBase-Erweiterungsgruppe](../../relational-databases/polybase/polybase-scale-out-groups.md).

Wenn Sie sich für eine PolyBase-Erweiterungsgruppe entscheiden, müssen Sie sicherstellen, dass folgende Anforderungen erfüllt sind:

- Alle Computer befinden sich in derselben Domäne.
- Sie verwenden während der PolyBase-Installation dasselbe Dienstkonto und Kennwort.
- Ihre SQL Server-Instanzen können über das Netzwerk miteinander kommunizieren.
- Alle SQL Server-Instanzen weisen die gleiche Version von SQL Server auf.

Wenn Sie PolyBase einmal installiert haben, können Sie dies nicht mehr ändern. Dabei spielt es keine Rolle, ob Sie eine eigenständige Version oder eine Erweiterungsgruppe installieren. Sie müssen das Feature deinstallieren und wieder neu installieren, um diese Einstellung zu ändern.

## <a name="use-the-installation-wizard"></a>Verwenden des Installationsassistenten
   
1. Führen Sie das Setupprogramm von SQL Server aus.   
   
2. Klicken Sie erst auf **Installation** und dann auf **New Standalone SQL Server installation or add features**(Neue eigenständige SQL Server-Installation oder Features hinzufügen).  
   
3. Wählen Sie auf der Seite „Funktionsauswahl“ **PolyBase Query Service for External Data** (PolyBase-Abfragedienst für externe Daten).  

   ![PolyBase-Dienste](../../relational-databases/polybase/media/install-wizard.png "PolyBase-Dienste")  
   
   >[!NOTE]
   >SQL Server 2019 PolyBase enthält jetzt eine zusätzliche Option **Java-Connector für HDFS-Datenquellen**. Weitere Informationen zu diesem Feature finden Sie im [Blog zu SQL Server-Previewfunktionen](https://cloudblogs.microsoft.com/sqlserver/2019/04/24/sql-server-2019-community-technology-preview-2-5-is-now-available/).
   
4. Konfigurieren Sie auf der Seite „Serverkonfiguration“ die **SQL Server-PolyBase-Engine** und den **SQL Server PolyBase-Datenverschiebungsdienst** so, dass beide unter demselben Domänenkonto ausgeführt werden.  

   >[!IMPORTANT]
   >In einer PolyBase-Erweiterungsgruppe müssen die PolyBase-Engine und der PolyBase-Datenverschiebungsdienst auf allen Knoten unter dem gleichen Domänenkonto ausgeführt werden. Weitere Informationen finden Sie unter [PolyBase scale-out groups (PolyBase-Erweiterungsgruppen)](#enable).

5. Wählen Sie auf der PolyBase-Konfigurationsseite eine der beiden Optionen aus. Weitere Informationen finden Sie unter [PolyBase scale-out groups (PolyBase-Erweiterungsgruppen)](../../relational-databases/polybase/polybase-scale-out-groups.md).  
   
   - Verwenden Sie die SQL Server-Instanz als eigenständige, für PolyBase aktivierte Instanz.  
   
     Wählen Sie diese Option aus, um die SQL Server-Instanz als eigenständigen Hauptknoten zu verwenden.  
   
   - Verwenden Sie die SQL Server-Instanz als Teil der PolyBase-Erweiterungsgruppe. Bei dieser Option wird die Firewall geöffnet, um eingehende Verbindungen zuzulassen. Verbindungen mit der SQL Server-Datenbank-Engine, der SQL Server-PolyBase-Engine und dem SQL Server PolyBase-Datenverschiebungsdienst sowie dem SQL-Browser werden zugelassen. Die Firewall erlaubt außerdem eingehende Verbindungen von anderen Knoten in einer PolyBase-Erweiterungsgruppe.  
   
     Bei dieser Option werden auch Firewallverbindungen des Microsoft Distributed Transaction Coordinator (MSDTC) zugelassen und die MSDTC-Registrierungseinstellungen geändert.  
   
6. Geben Sie auf der Seite PolyBase-Konfiguration den Portbereich mit mindestens sechs Ports an. SQL Server-Setup weist die ersten sechs verfügbaren Ports aus diesem Bereich zu.  

   >[!IMPORTANT]
   > Nach der Installation müssen Sie das [PolyBase-Feature aktivieren](#enable).


##  <a name="use-a-command-prompt"></a><a name="installing"></a> Verwenden einer Eingabeaufforderung.

Verwenden Sie die Werte in dieser Tabelle, um Installationsskripte zu erstellen. Die SQL Server-PolyBase-Engine und der SQL Server PolyBase-Datenverschiebungsdienst müssen unter demselben Konto ausgeführt werden. In einer PolyBase-Gruppe mit horizontaler Skalierung müssen PolyBase-Dienste auf allen Knoten unter demselben Domänenkonto ausgeführt werden.  
   
<!--SQL Server 2016/2017-->
::: moniker range="= sql-server-2016 || = sql-server-2017"

|SQL Server-Komponente|Parameter und Werte|BESCHREIBUNG|  
|--------------------------|--------------------------|-----------------|  
|SQL Server-Setupsteuerung|**Erforderlich**<br /><br /> /FEATURES=PolyBase|Wählt die PolyBase-Funktion aus.|  
|SQL Server PolyBase-Engine|**Optional**<br /><br /> /PBENGSVCACCOUNT|Gibt das Konto für den Engine-Dienst an. Der Standardwert ist **NT Authority\NETWORK SERVICE**.|  
|SQL Server PolyBase-Engine|**Optional**<br /><br /> /PBENGSVCPASSWORD|Gibt das Kennwort für das Engine-Dienstkonto an.|  
|SQL Server PolyBase-Engine|**Optional**<br /><br /> /PBENGSVCSTARTUPTYPE|Gibt den Startmodus für die PolyBase-Engine an: „Automatisch“ (Standard), „Deaktiviert“ und „Manuell“.|  
|SQL Server PolyBase-Datenverschiebungsdienst |**Optional**<br /><br /> /PBDMSSVCACCOUNT|Gibt das Konto für den Datenverschiebungsdienst an. Der Standardwert ist **NT Authority\NETWORK SERVICE**.|  
|SQL Server PolyBase-Datenverschiebungsdienst |**Optional**<br /><br /> /PBDMSSVCPASSWORD|Gibt das Kennwort für das Datenverschiebungskonto an.|  
|SQL Server PolyBase-Datenverschiebungsdienst |**Optional**<br /><br /> /PBDMSSVCSTARTUPTYPE|Gibt den Startmodus für den Datenverschiebungsdienst an: „Automatisch“ (Standard), „Deaktiviert“ und „Manuell“.|  
|PolyBase|**Optional**<br /><br /> /PBSCALEOUT|Gibt an, ob die SQL Server-Instanz als Teil einer PolyBase-Berechnungsgruppe verwendet wird. <br />Unterstützte Werte: TRUE, FALSE|  
|PolyBase|**Optional**<br /><br /> /PBPORTRANGE|Gibt einen Portbereich für PolyBase-Dienste mit mindestens sechs Ports an. Beispiel:<br /><br /> `/PBPORTRANGE=16450-16460`|  

::: moniker-end
<!--SQL Server 2019-->
::: moniker range=">= sql-server-ver15 "

|SQL Server-Komponente|Parameter und Werte|BESCHREIBUNG|  
|--------------------------|--------------------------|-----------------|  
|SQL Server-Setupsteuerung|**Erforderlich**<br /><br /> /FEATURES=PolyBaseCore, PolyBaseJava, PolyBase | PolyBaseCore installiert Unterstützung für alle PolyBase-Features mit Ausnahme der Hadoop-Konnektivität. PolyBaseJava aktiviert die Hadoop-Konnektivität. PolyBase installiert beides. |  
|SQL Server PolyBase-Engine|**Optional**<br /><br /> /PBENGSVCACCOUNT|Gibt das Konto für den Engine-Dienst an. Der Standardwert ist **NT Authority\NETWORK SERVICE**.|  
|SQL Server PolyBase-Engine|**Optional**<br /><br /> /PBENGSVCPASSWORD|Gibt das Kennwort für das Engine-Dienstkonto an.|  
|SQL Server PolyBase-Engine|**Optional**<br /><br /> /PBENGSVCSTARTUPTYPE|Gibt den Startmodus für die PolyBase-Engine an: „Automatisch“ (Standard), „Deaktiviert“ und „Manuell“.|  
|SQL Server PolyBase-Datenverschiebungsdienst |**Optional**<br /><br /> /PBDMSSVCACCOUNT|Gibt das Konto für den Datenverschiebungsdienst an. Der Standardwert ist **NT Authority\NETWORK SERVICE**.|  
|SQL Server PolyBase-Datenverschiebungsdienst |**Optional**<br /><br /> /PBDMSSVCPASSWORD|Gibt das Kennwort für das Datenverschiebungskonto an.|  
|SQL Server PolyBase-Datenverschiebungsdienst |**Optional**<br /><br /> /PBDMSSVCSTARTUPTYPE|Gibt den Startmodus für den Datenverschiebungsdienst an: „Automatisch“ (Standard), „Deaktiviert“ und „Manuell“.|  
|PolyBase|**Optional**<br /><br /> /PBSCALEOUT|Gibt an, ob die SQL Server-Instanz als Teil einer PolyBase-Berechnungsgruppe verwendet wird. <br />Unterstützte Werte: TRUE, FALSE|  
|PolyBase|**Optional**<br /><br /> /PBPORTRANGE|Gibt einen Portbereich für PolyBase-Dienste mit mindestens sechs Ports an. Beispiel:<br /><br /> `/PBPORTRANGE=16450-16460`|  

::: moniker-end

Nach der Installation müssen Sie das [PolyBase-Feature aktivieren](#enable).



**Beispiel**

Hier wird ein Beispielskript für das Setup dargestellt.  

```cmd
   
Setup.exe /Q /ACTION=INSTALL /IACCEPTSQLSERVERLICENSETERMS /FEATURES=SQLEngine,PolyBase   
/INSTANCENAME=MSSQLSERVER /SQLSYSADMINACCOUNTS="\<fabric-domain>\Administrator"   
/INSTANCEDIR="C:\Program Files\Microsoft SQL Server" /PBSCALEOUT=TRUE   
/PBPORTRANGE=16450-16460 /SECURITYMODE=SQL /SAPWD="<StrongPassword>"   
/PBENGSVCACCOUNT="<DomainName>\<UserName>" /PBENGSVCPASSWORD="<StrongPassword>"   
/PBDMSSVCACCOUNT="<DomainName>\<UserName>" /PBDMSSVCPASSWORD="<StrongPassword>"  
   
```  

## <a name="enable-polybase"></a><a id="enable"></a> Aktivieren von PolyBase

Nach der Installation muss PolyBase aktiviert werden, um auf die Features zugreifen zu können. Verwenden Sie dafür den folgenden Transact-SQL-Befehl: Bei Instanzen von SQL Server 2019, die während der Installation von Big Data-Clustern bereitgestellt wurden, ist diese Einstellung standardmäßig aktiviert.


```sql
exec sp_configure @configname = 'polybase enabled', @configvalue = 1;
RECONFIGURE;
```
## <a name="post-installation-notes"></a>Vorgehensweise nach Abschluss der Installation  

PolyBase installiert drei Benutzerdatenbanken, und zwar DWConfiguration, DWDiagnostics und DWQueue. Diese Datenbanken sind für die Verwendung durch PolyBase bestimmt. Nehmen Sie keine Änderungen vor, und löschen Sie sie nicht.  

> [!CAUTION]
> Das Hinzufügen von PolyBase zu einer vorhandenen Installation von SQL Server installiert die Features auf der Versionsebene des Installationsmediums, die hinter der Versionsebene anderer Features von SQL Server liegen kann. Dies kann zu unerwartetem Verhalten oder Fehlern führen. Stellen Sie stets im Anschluss an die Installation des Features PolyBase das neue Feature auf die gleiche Versionsebene um. Installieren Sie bei Bedarf Service Packs (SPs), kumulative Updates (CUs) und/oder allgemeine Vertriebsversionen (General Distribution Releases, GDRs). Informationen zum Bestimmen der Version PolyBase finden Sie unter [Ermitteln von Version, Edition und Updateebene von SQL Server und zugehöriger Komponenten](https://docs.microsoft.com/troubleshoot/sql/general/determine-version-edition-update-level#polybase).
   
### <a name="how-to-confirm-installation"></a><a id="confirminstall"></a> Bestätigen der Installation  

Führen Sie den folgenden Befehl aus. Wenn PolyBase installiert ist, wird 1 zurückgegeben. Andernfalls ist der Wert 0.  

```sql  
SELECT SERVERPROPERTY ('IsPolyBaseInstalled') AS IsPolyBaseInstalled;  
```  

### <a name="firewall-rules"></a>Firewallregeln  

Beim Setup von SQL Server PolyBase werden die folgenden Firewallregeln auf dem Computer erstellt:  
   
- SQL Server PolyBase – Datenbankmodul – \<SQLServerInstanceName> (TCP-In)  
   
- SQL Server PolyBase – PolyBase-Dienste – \<SQLServerInstanceName> (TCP-In)  

- SQL Server-PolyBase – SQL-Browser – (eingehendes UDP)  
   
Diese Regeln werden aktiviert, wenn Sie bei der Installation die SQL Server-Instanz als Teil der PolyBase-Erweiterungsgruppe verwenden. Die Firewall wird geöffnet, um eingehende Verbindungen zuzulassen. Diese werden für die SQL Server-Datenbank-Engine, die SQL Server-PolyBase-Engine und den SQL Server PolyBase-Datenverschiebungsdienst sowie den SQL-Browser zugelassen. Wenn der Firewalldienst des Computers bei der Installation nicht ausgeführt wird, kann das SQL Server-Setup diese Regeln nicht aktivieren. Starten Sie in diesem Fall den Firewalldienst des Computers, und aktivieren Sie diese Regeln nach der Installation.  
   
#### <a name="to-enable-the-firewall-rules"></a>So aktivieren Sie die Firewallregeln  

1. Öffnen Sie die **Systemsteuerung**.  

2. Klicken Sie erst auf **System and Security** (System und Sicherheit) und dann auf **Windows-Firewall**.  
   
3. Klicken Sie erst auf **Erweiterte Einstellungen** und dann auf **Eingangsregeln**.  
   
4. Klicken Sie erst mit der rechten Maustaste auf die deaktivierte Regel und dann mit der linken auf **Regel aktivieren**.  
   
### <a name="polybase-service-accounts"></a>PolyBase-Dienstkonten

Deinstallieren Sie das PolyBase-Feature, und installieren Sie es erneut, um die Dienstkonten für die PolyBase-Engine und den PolyBase-Datenverschiebungsdienst zu ändern.

## <a name="next-steps"></a>Nächste Schritte  

Siehe [PolyBase configuration](../../relational-databases/polybase/polybase-configuration.md).
