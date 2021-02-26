---
title: Vor dem Installieren des Failoverclusterings | Microsoft-Dokumentation
description: In diesem Artikel werden Planungsüberlegungen für die Vorbereitung der Installation eines SQL Server-Failoverclusters beschrieben, z. B. in Bezug auf die Hardware, das Betriebssystem und die Konfiguration.
ms.custom: ''
ms.date: 08/24/2016
ms.reviewer: ''
ms.prod: sql
ms.technology: failover-cluster-instance
ms.topic: how-to
helpviewer_keywords:
- clusters [SQL Server], preinstallation checklist
- installing failover clusters
- failover clustering [SQL Server], preinstallation checklist
ms.assetid: a655225d-8c54-4b30-95fd-31f588167899
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2c40647cad7568789a2909adf79d8cb12508532d
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100341157"
---
# <a name="before-installing-failover-clustering"></a>Vor dem Installieren des Failoverclusterings
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Bevor Sie einen SQL Server-Failovercluster installieren, müssen Sie die Hardware und das Betriebssystem auswählen, unter dem SQL Server ausgeführt werden soll. Außerdem müssen Sie das Windows Server Failover Clustering (WSFC) konfigurieren und Überlegungen zu Netzwerk, Sicherheit und anderer Software überprüfen, die auf dem Failovercluster ausgeführt werden soll.  
  
 Wenn ein Windows-Cluster über ein lokales Laufwerk verfügt und der zugehörige Laufwerkbuchstabe auch für mindestens einen Clusterknoten als freigegebenes Laufwerk verwendet wird, kann [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] auf diesem Laufwerk nicht installiert werden. Diese Einschränkung gilt sowohl für SQL Server-Failoverclusterinstanzen als auch für eigenständige Instanzen auf einem Server, der Teil einer Windows-Failoverclusterinstanz ist.
  
 Die folgenden Themen enthalten weitere Informationen zu den Konzepten, Funktionen und Tasks, die sich auf das Failoverclustering in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] beziehen.  
  
|Beschreibung der Themen|Thema|  
|-----------------------|-----------|  
|Beschreibt Konzepte des Failoverclusterings mit [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] und stellt Links zu relevanten Inhalten und Tasks bereit.|[AlwaysOn-Failoverclusterinstanzen &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md)|  
|Beschreibt Konzepte zur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Failoverrichtlinie und stellt Links zum Konfigurieren der Failoverrichtlinie in Anpassung an die Anforderungen Ihrer Organisation bereit.|[Failover Policy for Failover Cluster Instances](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md)|  
|Beschreibt, wie ein vorhandener [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Failovercluster verwaltet wird.|[Verwaltung und Wartung von Failoverclusterinstanzen](../../../sql-server/failover-clusters/windows/failover-cluster-instance-administration-and-maintenance.md)|  
|Erläutert, wie Sie [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] auf einem Windows Server-Failovercluster (WSFC) installieren.|[Verwenden von SQL Server Analysis Services in einem Cluster](/previous-versions/sql/sql-server-2012/dn736073(v=msdn.10))|  
  
 
  
##  <a name="best-practices"></a><a name="BestPractices"></a> Bewährte Methoden  
  
-   Lesen Sie die [Versionshinweise](../../sql-server-version-15-release-notes.md) für [!INCLUDE[SQL Server 2019](../../../includes/sssql19-md.md)].
  
-   Installieren Sie erforderliche Software. Installieren Sie vor dem Ausführen des Setups zum Installieren oder Aktualisieren die folgenden erforderlichen Komponenten, um die Installationsdauer zu verkürzen. Sie können die erforderliche Software auf jedem Failoverclusterknoten installieren und die Knoten anschließend einmal neu starten, bevor Sie Setup ausführen.  
  
    -   Windows PowerShell wird nicht mehr vom Setup für [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] installiert. Windows PowerShell ist eine erforderliche Komponente zum Installieren von [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)][!INCLUDE[ssDE](../../../includes/ssde-md.md)] -Komponenten und [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]. Wenn Windows PowerShell nicht auf dem Computer vorhanden ist, können Sie die Komponente aktivieren, indem Sie die Anweisungen auf der Seite [Windows Management Framework](/powershell/scripting/windows-powershell/wmf/overview) befolgen.  
  
    -   .NET Framework 3.5 SP1 wird vom [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Setup nicht mehr installiert; diese Version kann jedoch für die Installation von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] unter älteren Windows-Betriebssystemen erforderlich sein. Weitere Informationen finden Sie unter [SQL Server 2019: Hardware- und Softwareanforderungen](../../install/hardware-and-software-requirements-for-installing-sql-server-ver15.md).  
  
    -   **[!INCLUDE[msCoName](../../../includes/msconame-md.md)] Updatepaket:** Damit während des Setups aufgrund der .NET Framework 4-Installation kein Neustart erfolgt, ist für das Setup von [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] ein einzige Replikat die Installation eines [!INCLUDE[msCoName](../../../includes/msconame-md.md)]-Updates auf dem Computer erforderlich.  Wird [!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)] unter Windows 7 SP1 oder [!INCLUDE[firstref_longhorn](../../../includes/firstref-longhorn-md.md)] SP2 installiert, ist dieses Update bereits enthalten. Wenn Sie die Installation unter einem älteren Windows-Betriebssystem ausführen, laden Sie es von [Microsoft Update für .NET Framework 4.0 unter Windows Vista und Windows Server 2008](https://go.microsoft.com/fwlink/?LinkId=198093)herunter.  
  
    -   .NET Framework 4: In einem Clusterbetriebssystem wird .NET Framework 4 von Setup installiert. Um die Installationsdauer zu reduzieren, können Sie .NET Framework 4 installieren, bevor Sie das Setup ausführen.  
  
    -   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Sie können diese Dateien installieren, indem Sie SqlSupport.msi ausführen, das sich auf dem Installationsmedium befindet.  
  
-   Stellen Sie sicher, dass auf dem WSFC-Cluster keine Antivirensoftware installiert ist. Weitere Informationen finden Sie im [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Knowledge Base-Artikel über [mögliche Probleme mit Clusterdiensten durch Antivirensoftware](/troubleshoot/windows-server/high-availability/not-cluster-aware-antivirus-software-cause-issue).  
  
-   Beim Benennen einer Clustergruppe für die Failoverclusterinstallation dürfen Sie keine der folgenden Zeichen für den Namen der Clustergruppe verwenden:  
  
    -   Kleiner als-Operator (\<)  
  
    -   Größer als-Operator (>)  
  
    -   Doppelte Anführungszeichen (")  
  
    -   Einfaches Anführungszeichen (')  
  
    -   Kaufmännisches Und-Zeichen (&)  
  
     Überprüfen Sie auch die vorhandenen Clustergruppennamen auf nicht unterstützte Zeichen.  
  
-   Stellen Sie sicher, dass die Konfigurationen aller Clusterknoten identisch sind, einschließlich COM+, Laufwerkbuchstaben und Benutzer in der Administratorengruppe.  
  
-   Überprüfen Sie, ob Sie die Systemprotokolle in allen Knoten gelöscht und die Systemprotokolle erneut angezeigt haben. Stellen Sie sicher, dass die Protokolle keine Fehlermeldungen aufweisen, bevor Sie den Vorgang fortsetzen.  
  
-   Bevor Sie einen [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Failovercluster installieren oder aktualisieren, müssen Sie alle Anwendungen und Dienste deaktivieren, die ggf. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Komponenten während der Installation verwenden. Schalten Sie die Datenträgerressourcen jedoch nicht offline.  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Setup legt automatisch die Abhängigkeiten zwischen der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Clustergruppe und den Datenträgern im Failovercluster fest. Legen Sie vor dem Ausführen von Setup keine Abhängigkeiten für Datenträger fest.  
  
    -   Während der Installation des [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Failoverclusters wird ein Computerobjekt (Active Directory-Computerkonten) für den [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Netzwerkressourcennamen erstellt. In einem [!INCLUDE[firstref_longhorn](../../../includes/firstref-longhorn-md.md)] -Cluster benötigt das Clusternamenkonto (Computerkonto des Clusters) Berechtigungen zum Erstellen von Computerobjekten. Weitere Informationen finden Sie unter [Konfigurieren von Konten in Active Directory](https://technet.microsoft.com/library/cc731002\(WS.10\).aspx).  
  
    -   Wenn Sie die SMB-Dateifreigabe als Speicheroption verwenden, muss das [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Setupkonto über die Berechtigung "SeSecurityPrivilege" auf dem Dateiserver verfügen. Diese Berechtigung weisen Sie zu, indem Sie die Konsole für lokale Sicherheitsrichtlinien auf dem Dateiserver verwenden, um das [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Setupkonto den Berechtigungen **Überwachungs- und Sicherheitsprotokolle verwalten** hinzuzufügen.  
  
##  <a name="verify-your-hardware-solution"></a><a name="Hardware"></a> Überprüfen der Hardwarelösung  
  
-   Wenn die Clusterlösung geografisch verstreute Clusterknoten enthält, müssen weitere Elemente überprüft werden, z. B. die Netzwerklatenzzeit und die Unterstützung für freigegebene Datenträger.  
  
    -   Weitere Informationen zu [!INCLUDE[firstref_longhorn](../../../includes/firstref-longhorn-md.md)] und [!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)]finden Sie unter [Prüfen der Hardware auf einen Failovercluster](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732035(v=ws.10)) und unter [Supportrichtlinie für Failovercluster](https://go.microsoft.com/fwlink/?LinkId=196818).  
  
-   Überprüfen Sie, ob der Datenträger, auf dem [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] installiert wird, komprimiert oder verschlüsselt ist. Wenn Sie versuchen, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] auf einem komprimierten Laufwerk oder verschlüsselten Laufwerk zu installieren, erzeugt das [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Setup einen Fehler.  
  
-   SAN-Konfigurationen werden auch in den Editionen von [!INCLUDE[firstref_longhorn](../../../includes/firstref-longhorn-md.md)] und [!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)] Advanced Server und Datacenter Server unterstützt. In der Kategorie "Cluster-/Multiclustergerät" des Windows-Katalogs und der Hardwarekompatibilitätsliste wird die Gruppe der SAN-fähigen Speichervorrichtungen aufgelistet, die getestet wurden und als SAN-Speichereinheiten mit mehreren angefügten WSFC-Clustern unterstützt werden. Führen Sie eine Clustervalidierung durch, nachdem die zertifizierten Komponenten ermittelt wurden.  
  
-   Die SMB-Dateifreigabe wird ebenfalls für die Installation von Datendateien unterstützt. Weitere Informationen finden Sie unter [Storage Types for Data Files](../../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md#StorageTypes).  
  
    > [!WARNING]  
    >  Wenn Sie Windows File Server als SMB-Dateifreigabespeicher verwenden, muss das [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Setupkonto über die Berechtigung "SeSecurityPrivilege" auf dem Dateiserver verfügen. Diese Berechtigung weisen Sie zu, indem Sie die Konsole für lokale Sicherheitsrichtlinien auf dem Dateiserver verwenden, um das [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Setupkonto den Berechtigungen **Überwachungs- und Sicherheitsprotokolle verwalten** hinzuzufügen.  
    >   
    >  Wenn Sie einen anderen SMB-Dateifreigabespeicher als Windows File Server verwenden, wenden Sie sich bezüglich einer entsprechenden Einstellung auf der Dateiserverseite an den Speicheranbieter.  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] unterstützt Einbindungspunkte.  
  
     Mithilfe eines eingebundenen Volumes oder Einbindungspunkts können Sie mit einem einzelnen Laufwerkbuchstaben auf zahlreiche Datenträger oder Volumes verweisen. Wenn Sie der Laufwerkbuchstabe D: vorhanden ist, der auf einen regulären Datenträger oder ein reguläres Volume verweist, können Sie zusätzliche Datenträger oder Volumes als Verzeichnisse unter dem Laufwerkbuchstaben D: verbinden oder einlegen, ohne dass für die zusätzlichen Datenträger oder Volumes eigene Laufwerkbuchstaben erforderlich sind.  
  
     Zu Einbindungspunkten für das Failoverclustering von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] können weitere Überlegungen angestellt werden:  
  
    -   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Setup muss das Basislaufwerk eines bereitgestellten Laufwerkes über einen zugeordneten Laufwerkbuchstaben verfügen. Für Installationen von Failoverclustern muss es sich bei diesem Basislaufwerk um ein gruppiertes Laufwerk handeln. Volume-GUIDs werden in dieser Version nicht unterstützt.  
  
    -   Das Basislaufwerk, das Laufwerk mit dem Laufwerkbuchstaben, kann nicht von mehreren Failoverclusterinstanzen gemeinsam verwendet werden. Dies ist eine normale Einschränkung für Failovercluster, jedoch ist dies keine Einschränkung auf eigenständigen Servern mit mehreren Instanzen.  
  
    -   Die gruppierten Installationen von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sind auf die Anzahl der verfügbaren Laufwerkbuchstaben beschränkt. Vorausgesetzt, dass Sie nur einen Laufwerkbuchstaben für das Betriebssystem verwenden und alle anderen Laufwerkbuchstaben als normale Clusterlaufwerke oder Hostingeinbindungspunkte für Clusterlaufwerke verfügbar sind, sind Sie auf ein Maximum von 25 Instanzen von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pro Failovercluster beschränkt.  
  
        > [!TIP]  
        >  Durch die Verwendung der SMB-Dateifreigabeoption kann die Beschränkung auf 25 Instanzen außer Kraft gesetzt werden. Wenn Sie die SMB-Dateifreigabe als Speicheroption verwenden, können bis zu 50 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Failoverclusterinstanzen installiert werden.  
  
    -   Nach der Einbindung zusätzlicher Laufwerke wird keine Laufwerkformatierung unterstützt.  
  
-   Bei der[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Failoverclusterinstallation wird nur der lokale Datenträger zum Installieren der tempdb-Dateien unterstützt. Stellen Sie sicher, dass der für die tempdb-Daten und die Protokolldateien angegebene Pfad auf allen Clusterknoten gültig ist. Sind die tempdb-Verzeichnisse auf dem Failoverzielknoten während des Failovers nicht verfügbar, wird die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Ressource nicht online geschaltet. Weitere Informationen finden Sie unter [Speichertypen für Datendateien](../../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md#StorageTypes) und [Konfiguration der Datenbank-Engine – Datenverzeichnisse](../../../database-engine/install-windows/install-sql-server.md).  
  
-   Wenn Sie einen [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Failovercluster auf Komponenten mit iSCSI-Technologie (Internet Small Computer System Interface) bereitstellen, ist es empfehlenswert, mit entsprechender Vorsicht vorzugehen. Weitere Informationen finden Sie unter [Unterstützung für SQL Server auf iSCSI-Technologiekomponenten](/troubleshoot/sql/admin/support-iscsi-technology-components).  
  
-   Weitere Informationen finden Sie unter [Microsoft SQL Server-Supportrichtlinie für Microsoft Clustering](/troubleshoot/sql/failover-clusters/support-policy-clustered-configurations).  
  
-   Weitere Informationen zur ordnungsgemäßen Konfiguration von Quorumlaufwerken finden Sie unter [Quorum-Laufwerkskonfigurationsinformation](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770620(v=ws.10)).  
  
-   Wenn Sie einen [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Failovercluster installieren möchten und die Quellinstallationsdateien sowie der Cluster von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] in verschiedenen Domänen enthalten sind, müssen Sie die Installationsdateien in die aktuelle Domäne kopieren, die für den [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Failovercluster verfügbar ist.  
  
##  <a name="review-security-considerations"></a><a name="Security"></a> Überprüfen der Sicherheitsüberlegungen  
  
-   Installieren Sie zum Verwenden der Verschlüsselung das Serverzertifikat mit dem vollqualifizierten DNS-Namen des WSFC-Clusters auf allen Knoten im [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Failovercluster. Wenn Sie beispielsweise über einen Cluster mit zwei Knoten mit den Namen "Test1.DomainName.com" und "Test2.DomainName.com" und eine [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Failoverclusterinstanz mit dem Namen "Virtsql" verfügen, müssen Sie ein Zertifikat für "Virtsql.DomainName.com" abrufen und das Zertifikat auf den Knoten test1 und test2 installieren. Dann können Sie das Kontrollkästchen **Protokollverschlüsselung erzwingen** im [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Konfigurations-Manager aktivieren, um den Failovercluster für die Verschlüsselung zu konfigurieren.  
  
    > [!IMPORTANT]  
    >  Aktivieren Sie das Kontrollkästchen **Protokollverschlüsselung erzwingen** erst dann, wenn Sie die Zertifikate auf allen teilnehmenden Knoten in der Failoverclusterinstanz installiert haben.  
  
-   Bei [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Installationen in gleichzeitigen Konfigurationen mit früheren Versionen müssen die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Dienste Konten verwenden, die der globalen Domänengruppe angehören müssen. Darüber hinaus dürfen Konten, die von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Diensten genutzt werden, nicht der lokalen Administratorgruppe angehören. Wenn diese Richtlinien nicht eingehalten werden, führt dies zu unerwarteten Ergebnissen im Hinblick auf die Sicherheit.  
  
-   Zum Erstellen eines Failoverclusters müssen Sie lokaler Administrator sein, der über die Berechtigung verfügt, sich als Dienst anzumelden und als Teil des Betriebssystems auf allen Knoten der Failoverclusterinstanz zu agieren.  
  
-   Unter [!INCLUDE[nextref_longhorn](../../../includes/nextref-longhorn-md.md)]werden Dienst-SIDs automatisch zur Verwendung auf [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] -Diensten generiert. Bei [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] -Failoverclusterinstanzen, die von früheren Versionen von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]aktualisiert wurden, werden die vorhandenen Domänengruppen und die Konfigurationen der Zugriffssteuerungsliste beibehalten.  
  
-   Domänengruppen müssen sich innerhalb derselben Domäne wie die Computerkonten befinden. Wenn sich der Computer, auf dem [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] installiert wird, in der SQLSVR-Domäne befindet, die MYDOMAIN untergeordnet ist, müssen Sie eine Gruppe in der SQLSVR-Domäne angeben. Die SQLSVR-Domäne kann Benutzerkonten aus MYDOMAIN enthalten.  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] kann nicht installiert werden, wenn es sich bei den Clusterknoten um Domänencontroller handelt.  
  
-   Lesen Sie [Security Considerations for a SQL Server Installation](../../../sql-server/install/security-considerations-for-a-sql-server-installation.md).  
  
-   Informationen zum Aktivieren der Kerberos-Authentifizierung in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]finden Sie in der [Knowledge Base unter](https://www.betaarchive.com/wiki/index.php?title=Microsoft_KB_Archive/319723) Verwenden der Kerberos-Authentifizierung in SQL Server [!INCLUDE[msCoName](../../../includes/msconame-md.md)] .  

-   Die SQL Server-Failoverclusterinstanz (FCI) erfordert, dass die Clusterknoten einer Domäne beigetreten sind. Die folgenden Konfigurationen werden **nicht unterstützt**: 
    *   SQL-FCI in Arbeitsgruppenclustern. 
    *   SQL-FCI in Clustern mit mehreren Domänen.   
    *   SQL-FCI in Domänen- und Arbeitsgruppenclustern. 

  
##  <a name="review-network-port-and-firewall-considerations"></a><a name="Network"></a> Überprüfen der Überlegungen zu Netzwerken, Ports und Firewall  
  
-   Überprüfen Sie, ob Sie NetBIOS für alle privaten Netzwerkkarten deaktiviert haben, bevor Sie das [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Setup starten.  
  
-   Netzwerkname und IP-Adresse des Computers mit [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sollten zu keinem anderen Zweck, wie z. B. Dateifreigabe, verwendet werden. Wenn Sie eine Ressource für die Dateifreigabe erstellen möchten, verwenden Sie einen anderen, eindeutigen Netzwerknamen und eine andere, eindeutige IP-Adresse für die Ressource.  
  
    > [!IMPORTANT]  
    >  Es wird empfohlen, keine Dateifreigaben auf Datenlaufwerken zu verwenden, da sie Auswirkungen auf das Verhalten und die Leistung von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] haben können.  
  
-   Obwohl [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sowohl Named Pipes als auch TCP/IP-Sockets über TCP/IP innerhalb eines Clusters unterstützt, wird die Verwendung von TCP/IP-Sockets in einer gruppierten Konfiguration empfohlen.  
  
-   Beachten Sie, dass ISA Server nicht von Windows Clustering und daher auch nicht auf [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Failoverclustern unterstützt wird.  
  
-   Der Remoteregistrierungsdienst muss aktiviert sein und ausgeführt werden.  
  
-   Die Remoteverwaltung muss aktiviert sein.  
  
- Verwenden Sie für SQL Server-Instanzen, die keinen Standardport verwenden, die Netzwerkkonfiguration des SQL Server-Konfigurations-Manager, um den Port zu bestimmen, der von der SQL Server-Instanz verwendet wird, für die Sie die Blockierung aufheben möchten. Aktivieren Sie in der Firewall den TCP-Port für IPALL, wenn Sie über den [SQL Server-Browser-Dienst](../../../tools/configuration-manager/sql-server-browser-service.md), der eine andere IP-Adresse als die gruppierte Instanz verwendet, und den UDP-Port 1434 verwenden möchten, um eine Verbindung mit Ihrer SQL Server-Instanz herzustellen. 
  
-   Bei Setupvorgängen für Failovercluster wird eine Regel zum Überprüfen der Bindungsreihenfolge von Netzwerken befolgt. Auch wenn die Bindungsreihenfolgen korrekt erscheinen, können die NIC-Konfigurationen im System möglicherweise deaktiviert oder inaktiv sein. Inaktive NIC-Konfigurationen können sich auf die Bindingsreihenfolge auswirken und zum Ausgeben einer Warnung durch die Regel für die Bindungsreihenfolge führen. Um diese Situation zu vermeiden, führen Sie die folgenden Schritte aus, um deaktivierte Netzwerkkarten zu ermitteln und zu entfernen:  
  
    1.  Geben Sie an einer Eingabeaufforderung Folgendes ein: set devmgr_Show_Nonpersistent_Devices=1.  
  
    2.  Geben Sie den folgenden Befehl ein, und führen Sie ihn aus: start Devmgmt.msc.  
  
    3.  Erweitern Sie die Liste der Netzwerkkarten. Nur die physischen Karten sollten in der Liste aufgeführt werden. Wenn eine Netzwerkkarte deaktiviert ist, gibt Setup einen Fehler für die Regel der Netzwerkbindungsreihenfolge aus. In der Systemsteuerung wird unter Netzwerkverbindungen ebenfalls angezeigt, dass die Karte deaktiviert wurde. Überprüfen Sie, ob in der Systemsteuerung unter Netzwerkeinstellungen die gleiche Liste aktivierter physischer Karten wie in devmgmt.msc angezeigt wird.  
  
    4.  Entfernen Sie deaktivierte Netzwerkkarten, bevor Sie das Setup für SQL Server ausführen.  
  
    5.  Kehren Sie nach Beendigung von Setup zu den Netzwerkverbindungen in der Systemsteuerung zurück, und deaktivieren Sie sämtliche Netzwerkadapter, die derzeit nicht verwendet werden.  
  
##  <a name="verify-your-operating-system"></a><a name="OS_Support"></a> Überprüfen des Betriebssystems  
 Stellen Sie sicher, dass das Betriebssystem ordnungsgemäß installiert ist und für die Unterstützung von Failoverclustering entworfen wurde. In der folgenden Tabelle werden die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Editionen und die Betriebssysteme aufgelistet, die sie unterstützen.  
  
|[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Edition|Windows Server 2019 Datacenter|Windows Server 2019 Standard|Windows Server 2016 Datacenter|Windows Server 2016 Standard |  
|---------------------------------------|------------------------------------------------|-------------------------------------------------------|----------------------------------------------|-----------------------------------------------------|  
|[!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)] Enterprise (64-Bit) x64*|Ja|Ja|Ja|Ja|  
|[!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)] Enterprise (32-Bit)|Ja|Ja|||  
|[!INCLUDE[sssql16-md](../../../includes/sssql16-md.md)] Enterprise |Ja|Ja|Ja|Ja|
|[!INCLUDE[sssql16-md](../../../includes/sssql16-md.md)] Standard |Ja|Ja|Ja|Ja|
|[!INCLUDE[sssql17-md](../../../includes/sssql17-md.md)] Enterprise |Ja|Ja|Ja|Ja|
|[!INCLUDE[sssql17-md](../../../includes/sssql17-md.md)] Standard |Ja|Ja|Ja|Ja|
|[!INCLUDE[sssql19-md](../../../includes/sssql19-md.md)] Enterprise |Ja|Ja|Ja|Ja|
|[!INCLUDE[sssql19-md](../../../includes/sssql19-md.md)] Standard |Ja|Ja|Ja|Ja|

 *[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Cluster werden im WOW-Modus nicht unterstützt. Dies schließt Upgrades von früheren Versionen von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Failoverclustern ein, die ursprünglich unter WOW installiert wurden. Für diese Upgrades ist nur eine parallele Installation der neuen Version mit anschließender Migration möglich.  

  
##  <a name="additional-considerations-for-multi-subnet-configurations"></a><a name="MultiSubnet"></a> Zusätzliche Überlegungen zu Multisubnetz-Konfigurationen  
 In den folgenden Abschnitten werden die Anforderungen beschrieben, die bei der Installation eines [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Multisubnetz-Failoverclusters berücksichtigt werden müssen. Da bei der Multisubnetz-Konfiguration Cluster über mehrere Subnetze erstellt werden, werden auch mehrere IP-Adressen verwendet und u. U. Änderungen an den Abhängigkeiten der IP-Adressressourcen vorgenommen.  
  
### <a name="ssnoversion-edition-and-operating-system-considerations"></a>[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Edition und zum Betriebssystem  
  
-   Weitere Informationen zu den Editionen von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , die einen [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Multisubnetz-Failovercluster unterstützen, finden Sie unter [Von den SQL Server 2016-Editionen unterstützte Funktionen](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
-   Um einen [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Multisubnetz-Failovercluster zu erstellen, müssen Sie zuerst den [!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)] -Failovercluster für mehrere Standorte in mehreren Subnetzen erstellen.  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Failovercluster stellt bei einem Failover mithilfe des Windows Server-Failoverclusters sicher, dass die IP-Abhängigkeitsbedingungen gültig sind.  
  
-   [!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)] erfordert, dass sich alle Clusterserver in der gleichen Active Directory-Domäne befinden. Daher müssen sich alle Clusterknoten eines [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Multisubnetz-Failoverclusters in der gleichen Active Directory-Domäne befinden, auch wenn sie Teil unterschiedlicher Subnetze sind.  
  
#### <a name="ip-address-and-ip-address-resource-dependencies"></a>IP-Adressen und Abhängigkeiten von IP-Adressressourcen  
  
1.  Die Abhängigkeit von IP-Adressressourcen wird in einer Multisubnetz-Konfiguration auf OR festgelegt. Weitere Informationen finden Sie unter [Erstellen eines neuen SQL Server-Failoverclusters &#40;Setup&#41;](../../../sql-server/failover-clusters/install/create-a-new-sql-server-failover-cluster-setup.md).  
  
2.  Die gleichzeitige Verwendung von AND- und OR-IP-Adressabhängigkeiten wird nicht unterstützt. „\<IP1> AND \<IP2> OR \<IP3>“ wird beispielsweise nicht unterstützt.  
  
3.  Mehr als eine IP-Adresse pro Subnetz wird nicht unterstützt.  
  
     Wenn Sie mehrere IP-Adressen für ein Subnetz konfigurieren, kann es zu Fehlern bei der Clientverbindung während des Starts von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] kommen.  
  
#### <a name="related-content"></a>Verwandte Inhalte  
 Weitere Informationen über [!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)] -Failover für mehrere Standorte finden Sie auf der [Windows Server 2008 R2-Failoverclustering-Website](https://technet.microsoft.com/library/ff182338\(v=WS.10\).aspx) und unter [Entwurf für einen Clusterdienst oder eine Clusteranwendung in einem Failovercluster für mehrere Standorte](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd197430(v=ws.10)).  
  
##  <a name="configure-windows-server-failover-cluster"></a><a name="WSFC"></a> Konfigurieren von Windows Server-Failoverclustern  
  
-   [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Cluster Service (WSFC) muss auf mindestens einem Knoten des Serverclusters konfiguriert werden. Außerdem müssen Sie [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Enterprise, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Business Intelligence oder [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Standard in Verbindung mit WSFC ausführen. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Enterprise werden Failovercluster mit bis zu 16 Knoten unterstützt. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Business Intelligence und [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Standard unterstützen Failovercluster mit zwei Knoten.  
  
-   Die Ressourcen-DLL für den [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Dienst exportiert zwei vom WSFC-Cluster-Manager verwendete Funktionen, um die Verfügbarkeit der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Ressource zu überprüfen. Weitere Informationen finden Sie unter [Failoverrichtlinie für Failoverclusterinstanzen](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md).  
  
-   WSFC muss mithilfe von IsAlive überprüfen können, ob die Failoverclusterinstanz ausgeführt wird. Hierzu muss über eine vertrauenswürdige Verbindung eine Verbindung mit dem Server hergestellt werden. Standardmäßig ist das Konto, unter dem der Clusterdienst ausgeführt wird, auf den Knoten im Cluster nicht als Administrator konfiguriert, und die Gruppe BUILTIN\Administratoren verfügt nicht über die Berechtigung zum Anmelden an [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Diese Einstellungen werden nur dann geändert, wenn Sie die Berechtigungen für die Clusterknoten ändern.  
  
-   Konfigurieren Sie Domain Name Service (DNS) oder Windows Internet Name Service (WINS). Ein DNS-Server oder WINS-Server muss in der Umgebung ausgeführt werden, in der der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Failovercluster installiert wird. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Setup erfordert eine DDNS-Registrierung (Dynamic Domain Name Service) des virtuellen Verweises der IP-Schnittstelle von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Die DNS-Serverkonfiguration sollte Clusterknoten die dynamische Registrierung einer Online-IP-Adresszuordnung mit dem Netzwerknamen ermöglichen. Wenn die dynamische Registrierung nicht abgeschlossen werden kann, kann das Setup nicht erfolgreich ausgeführt werden, und es wird ein Rollback der Installation ausgeführt. Weitere Informationen finden Sie [in diesem Knowledge Base-Artikel](https://mskb.pkisolutions.com/kb/947048)  
  
##  <a name="install-msconame-distributed-transaction-coordinator"></a><a name="MSDTC"></a> Installieren von [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Distributed Transaction Coordinator  
 Bevor Sie [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] in einem Failovercluster installieren, müssen Sie bestimmen, ob die MSDTC-Clusterressource ( [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Distributed Transaction Coordinator) erstellt werden muss. Wenn Sie nur das [!INCLUDE[ssDE](../../../includes/ssde-md.md)]installieren, ist die MSDTC-Clusterressource nicht erforderlich. Wenn Sie das [!INCLUDE[ssDE](../../../includes/ssde-md.md)] und SSIS, die Arbeitsstationskomponenten, installieren, oder wenn Sie verteilte Transaktionen verwenden, müssen Sie MSDTC installieren. Für Nur- [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]-Instanzen ist MSDTC nicht erforderlich.  
  
 Unter [!INCLUDE[firstref_longhorn](../../../includes/firstref-longhorn-md.md)] und [!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)]können Sie mehrere Instanzen von MSDTC auf einem einzelnen Failovercluster installieren. Die erste Instanz von MSDTC, die installiert wird, ist die Clusterstandardinstanz von MSDTC. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] nutzt eine Instanz von MSDTC, die in der lokalen Clusterressourcengruppe von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] installiert wurde, durch die automatische Verwendung der Instanz von MSDTC. Einzelne Anwendungen können jedoch einer beliebigen Instanz von MSDTC auf dem Cluster zugeordnet werden.  
  
 Für die Auswahl durch [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]werden folgende Regeln auf eine Instanz von MSDTC angewendet:  
  
-   MSDTC in der lokalen Gruppe verwenden, andernfalls  
  
-   Zugeordnete Instanz von MSDTC verwenden, andernfalls  
  
-   MSDTC-Standardinstanz des Clusters verwenden, andernfalls  
  
-   MSDTC-Instanz auf lokalem Computer verwenden  
  
> [!IMPORTANT]  
>  Bei einem Fehler in der MSDTC-Instanz der lokalen Clustergruppe von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] versucht [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] nicht automatisch, die Standardclusterinstanz oder die lokale Computerinstanz von MSDTC zu verwenden. Sie müssten die fehlerhafte Instanz von MSDTC vollständig aus der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Gruppe entfernen, um eine andere Instanz von MSDTC zu verwenden. Wenn Sie eine Zuordnung für [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] erstellen und ein Fehler in der zugeordneten Instanz von MSDTC auftritt, kommt es analog zu Fehlern bei den verteilten Transaktionen. Wenn [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] eine andere Instanz von MSDTC verwenden soll, müssen Sie der lokalen Clustergruppe von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] eine Instanz von MSDTC hinzufügen oder die Zuordnung löschen.  
  
### <a name="configure-msconame-distributed-transaction-coordinator"></a>Konfigurieren von [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Distributed Transaction Coordinator  
 Nachdem Sie das Betriebssystem installiert und den Cluster konfiguriert haben, müssen Sie MS DTC mithilfe der Clusterverwaltung für die Ausführung in einem Cluster konfigurieren. Wenn MSDTC nicht für die Ausführung in einem Cluster konfiguriert wird, führt dies nicht zu einer Blockierung des [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Setups, aber die Funktionen der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Anwendung können beeinträchtigt sein, falls MSDTC nicht ordnungsgemäß konfiguriert wurde.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Hardware- und Softwareanforderungen für die Installation von SQL Server 2016](../../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)   
 [Überprüfen der Parameter für die Systemkonfigurationsprüfung](../../../database-engine/install-windows/check-parameters-for-the-system-configuration-checker.md)   
 [Verwaltung und Wartung von Failoverclusterinstanzen](../../../sql-server/failover-clusters/windows/failover-cluster-instance-administration-and-maintenance.md)  
  
