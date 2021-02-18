---
title: Erstellen einer domänenunabhängigen Verfügbarkeitsgruppe
description: Mit diesen Schritten erstellen Sie eine Verfügbarkeitsgruppe, die einen Arbeitsgruppencluster verwendet. Dadurch kann SQL Server 2016 (und höher) eine Always On-Verfügbarkeitsgruppe auf einem WSFC bereitstellen, für die Active Directory Domain Services nicht erforderlich sind und somit nicht jeder Server derselben Domäne angehören muss.
ms.custom: seodec18
ms.date: 09/25/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], domain independent
ms.assetid: ''
author: cawrites
ms.author: chadam
ms.openlocfilehash: 060e9e635aa7be4dd3c00e11b0f3a36073cdaa54
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100349279"
---
# <a name="create-a-domain-independent-availability-group"></a>Erstellen einer domänenunabhängigen Verfügbarkeitsgruppe
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

Always On-Verfügbarkeitsgruppen (AGs) erfordern einen zugrunde liegenden Windows Server-Failovercluster (WSFC). Das Bereitstellen eines WSFC über Windows Server 2012 R2 erfordert, dass die Server, die an einem WSFC teilnehmen (auch als Knoten bekannt) mit derselben Domäne verknüpft sind. Weitere Informationen zu Active Directory Domain Services (AD DS), finden Sie [hier](/previous-versions/windows/it-pro/windows-server-2003/cc759073(v=ws.10)).

Die AD DS- sowie WSFC-Abhängigkeit ist komplexer als das, was zuvor mit einer Datenbankspiegelungskonfiguration (DBM) bereitgestellt wurde, da DBM über mehrere Rechenzentren hinweg mit Zertifikaten ohne solche Abhängigkeiten bereitgestellt werden.  Eine herkömmliche Verfügbarkeitsgruppe, die sich über mehrere Datencenter erstreckt, erfordert, dass alle Server mit derselben Active Directory-Domäne verknüpft sein sollen. Andere Domänen, auch vertrauenswürdige Domänen, funktionieren nicht. Alle Server müssen Knoten desselben WSFC sein. In der folgenden Abbildung wird diese Konfiguration veranschaulicht. SQL Server 2016 hat ebenfalls Verfügbarkeitsgruppen verteilt, die ebenfalls dieses Ziel auf eine andere Weise erreichen können.


![Der WSFC, der sich über zwei Rechenzentren erstreckt, die mit derselben Domäne verbunden sind][1]

In Windows Server 2012 R2 wurde ein [von Active Directory getrennter Cluster](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn265970(v=ws.11)) eingeführt, eine spezielle Form eines Windows Server-Failoverclusters, der mit Verfügbarkeitsgruppen verwendet werden kann. Diese Art von WSFC benötigt noch immer die Knoten, um mit der gleichen Active Directory-Domäne verbunden zu sein. In diesem Fall verwendet der WSFC jedoch DNS, aber nicht die Domäne. Da eine Domäne noch beteiligt ist, bietet der von Active Directory getrennte Cluster noch immer keine vollständige Erfahrung ohne Domänen.

In Windows Server 2016 wurde eine neue Art eines Windows Server-Failoverclusters eingeführt, basierend auf dem von Active Directory getrennten Cluster: ein Workgroupcluster. Ein Workgroupcluster lässt zu, dass SQL Server 2016 eine Verfügbarkeitsgruppe auf einem WSFC platziert, der AD DS nicht benötigt. SQL Server erfordert die Verwendung von Zertifikaten für die Endpunktsicherheit, ebenso wie das Datenbankspiegelungsszenario Zertifikate erfordert.  Diese Art einer Verfügbarkeitsgruppe wird als domänenunabhängige Verfügbarkeitsgruppe bezeichnet. Das Bereitstellen einer Verfügbarkeitsgruppe mit einem zugrunde liegenden Workgroupcluster unterstützt die folgenden Kombinationen für die Knoten, aus denen der WSFC besteht:
- Keine Knoten sind mit einer Domäne verknüpft.
- Alle Knoten sind mit unterschiedlichen Domänen verknüpft.
- Knoten werden mit einer Kombination aus mit Domänen verbundenen und nicht mit Domänen verbundenen Knoten gemischt.

Die nächste Abbildung zeigt ein Beispiel einer domänenunabhängigen Verfügbarkeitsgruppe, wo die Knoten im Rechenzentrum 1 domänenverbunden sind, die Knoten im Rechenzentrum 2 jedoch nur DNS nutzen. In diesem Fall legen Sie das DNS-Suffix auf allen Servern fest, die zu Knoten des WSFC werden. Für jede Anwendung und jeder Server, die bzw. der Zugriff auf die Verfügbarkeitsgruppe hat, muss die gleiche DNS-Informationen angezeigt werden.


![Workgroupcluster mit zwei Knoten, die mit einer Domäne verknüpft sind][2]

Eine domänenunabhängige Verfügbarkeitsgruppe ist nicht nur für Multisite- oder Notfallwiederherstellungsszenarios vorhanden. Sie kann in einem einzelnen Rechenzentrum bereitgestellt und sogar mit einer [Standardverfügbarkeitsgruppe](basic-availability-groups-always-on-availability-groups.md) (auch als Verfügbarkeitsgruppe der Standard Edition bekannt) verwendet werden, um eine ähnliche Architektur bereitzustellen, wie die, die mithilfe der Datenbankspiegelung mit Zertifikaten erreicht wurde, so wie hier dargestellt.


![Überblick über eine Verfügbarkeitsgruppe in der Standard Edition][3]

Das Bereitstellen einer domänenunanbhängigen Verfügbarkeitsgruppe hat einige bekannte Vorbehalte:
- Die einzigen verfügbaren Zeugentypen, die für das Quorum vorhanden sind, sind die Datenträger und die [Cloud](/windows-server/failover-clustering/deploy-cloud-witness), die in Windows Server 2016 neu ist. Datenträger sind problematisch, da es für die Verfügbarkeitsgruppe aller Wahrscheinlichkeit nach keine Verwendung eines freigegebenen Datenträgers gibt.
- Die zugrunde liegende Variante des Workgroupclusters eines WSFCs kann nur mithilfe von PowerShell erstellt werden, kann dann jedoch mit dem Failovercluster-Manager verwaltet werden.
- Wenn Kerberos erforderlich ist, müssen Sie einen Standard-WSFC bereitstellen, der einer Active Directory-Domäne angefügt ist. Eine domänenunabhängige Verfügbarkeitsgruppe ist sicherlich keine Option.
- Während ein Listener konfiguriert werden kann, muss er in DNS registriert werden, um verwendet werden zu können. Wie oben bereits erwähnt, besteht keine Kerberos-Unterstützung für den Listener.
- Anwendungen, die eine Verbindung mit SQL Server haben, sollten in erster Linie die SQL Server-Authentifizierung nutzen, da Domänen möglicherweise nicht vorhanden oder nicht für die Zusammenarbeit konfiguriert sind. 
- Zertifikate werden in der Konfiguration der Verfügbarkeitsgruppe verwendet.

## <a name="set-and-verify-the-dns-suffix-on-all-replica-servers"></a>Festlegen und Überprüfen des DNS-Suffix auf allen Replikatservern

Ein allgemeines DNS-Suffix ist für den Workgroupcluster einer domänenunabhängigen Verfügbarkeitsgruppe erforderlich. Um das DNS-Suffix auf jedem Windows-Server, der ein Replikat für die Verfügbarkeitsgruppe hosten wird, festzulegen und zu überprüfen, befolgen Sie die folgenden Anweisungen:

1. Wählen Sie mithilfe der Windows-Taste + X „System“ aus.
2. Wenn der Computername und der vollständige Computername identisch sind, wurde das DNS-Suffix nicht festgelegt. Wenn der Computername z.B. ALLAN ist, sollte der Wert für den vollständigen Computernamen nicht nur ALLAN sein. Dies sollte etwa wie ALLAN.SQLHA.LAB aussehen. SQLHA.LAB ist das DNS-Suffix. Der Wert für die Arbeitsgruppe müsste WORKGROUP sein. Wenn Sie das DNS-Suffix festlegen müssen, wählen Sie „Einstellungen ändern“.
3. Klicken Sie im Dialogfeld „Systemeigenschaften“ auf der Registerkarte „Computername“ auf „Ändern“.
4. Klicken Sie auf dem Dialogfeld „Computernamen- bzw. -domänenänderungen“ auf „Weitere“.
5. Geben Sie auf dem DNS-Suffix und im Dialogfeld „NetBIOS-Computername“ das allgemeine DNS-Suffix als primäres DNS-Suffix ein. 
6. Klicken Sie auf „OK“, um das DNS-Suffix und das NetBIOS-Computername-Dialogfeld zu schließen.
7. Klicken Sie auf „OK“, um das Dialogfeld „Computernamen- bzw. -domänenänderungen“ zu schließen.
8. Sie werden aufgefordert, den Server neu zu starten, damit die Änderungen in Kraft treten können. Klicken Sie auf „OK“, um das Dialogfeld „Computernamen- bzw. -domänenänderungen“ zu schließen.
9. Klicken Sie auf „Schließen“, um das Dialogfeld „Systemeigenschaften“ zu schließen.
10. Sie werden zu einem Neustart aufgefordert. Wenn Sie nicht sofort neu starten möchten, klicken Sie auf „Später neu starten“. Klicken Sie andernfalls auf „Jetzt neu starten“.
11. Nachdem der Server neu gestartet wurde, stellen Sie sicher, dass das allgemeine DNS-Suffix konfiguriert ist, indem Sie erneut einen Blick auf das System werfen.

![Erfolgreiche Konfiguration des DNS-Suffix][4]

  > [!NOTE]
  > Wenn Sie mehrere Subnetze verwenden und über ein statisches DNS verfügen, benötigen Sie einen Prozess, um den dem Listener zugeordneten DNS-Eintrag zu aktualisieren, bevor Sie einen Failover durchführen, da sonst der Netzwerkname nicht online geht.

## <a name="create-a-domain-independent-availability-group"></a>Erstellen einer domänenunabhängigen Verfügbarkeitsgruppe

Die Erstellung einer domänenunabhängigen Verfügbarkeitsgruppe kann derzeit von SQL Server Management Studio nicht vollständig durchgeführt werden. Während die Erstellung der domänenunabhängigen Verfügbarkeitsgruppe im Grunde dasselbe wie die Erstellung einer normalen Verfügbarkeitsgruppe ist, sind manche Aspekte (wie das Erstellen der Zertifikate) nur mit Transact-SQL möglich. Im folgenden Beispiel wird von der Konfiguration einer Verfügbarkeitsgruppe mit zwei Replikaten ausgegangen: ein primäres und ein sekundäres Replikat. 

1. [Mithilfe der Anweisungen unter diesem Link](https://techcommunity.microsoft.com/t5/Failover-Clustering/Workgroup-and-Multi-domain-clusters-in-Windows-Server-2016/ba-p/372059) stellen Sie einen Workgroupcluster bereit, der aus allen Servern besteht, die in der Verfügbarkeitsgruppe vorhanden sein werden. Stellen Sie sicher, dass das allgemeine DNS-Suffix bereits konfiguriert ist, bevor Sie den Workgroupcluster konfigurieren.
2. [Aktivieren Sie die Funktion „Always On-Verfügbarkeitsgruppen“](./enable-and-disable-always-on-availability-groups-sql-server.md) auf jeder Instanz, die an einer oder mehreren Verfügbarkeitsgruppen teilnehmen soll. Der Neustart jeder SQL Server-Instanz ist so erforderlich.
3. Jede Instanz, die das primäre Replikat hostet, erfordert einen Datenbankhauptschlüssel. Wenn nicht bereits ein Hauptschlüssel vorhanden ist, führen Sie den folgenden Befehl aus:

   ```sql
   CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'Strong Password';
   GO
   ```

4. Erstellen sie auf der Instanz, die das primäre Replikat sein wird, das Zertifikat, das jeweils für eingehende Verbindungen auf den sekundären Replikaten und für die Sicherung des Endpunkts auf dem primären Replikat verwendet wird.

   ```sql
   CREATE CERTIFICATE InstanceA_Cert 
   WITH SUBJECT = 'InstanceA Certificate';
   GO
   ``` 

5. Sichern Sie das Zertifikat. Sie können es bei Bedarf auch weiter mit einem privaten Schlüssel sichern. In diesem Beispiel wird kein privater Schlüssel verwendet.

   ```sql
   BACKUP CERTIFICATE InstanceA_Cert 
   TO FILE = 'Backup_path\InstanceA_Cert.cer';
   GO
   ```

6. Wiederholen Sie die Schritte 4 und 5, um Zertifikate aus jedem sekundären Replikat zu erstellen und zu sichern. Verwenden Sie hierzu die passenden Namen für die Zertifikate, z.B. „InstanceB_Cert“.
7. Sie müssen auf dem primären Replikat einen Anmeldenamen für jedes sekundäre Replikat der Verfügbarkeitsgruppe erstellen. Diesem Anmeldename wird die Berechtigung zur Verbindung mit dem Endpunkt gewährt, der von der domänenunabhängigen Verfügbarkeitsgruppe verwendet wird. Beispielsweise für ein Replikat mit der Bezeichnung „InstanceB“:

   ```sql
   CREATE LOGIN InstanceB_Login WITH PASSWORD = 'Strong Password';
   GO
   ```

8. Erstellen Sie auf jedem sekundären Replikat einen Anmeldenamen für das primäre Replikat. Diesem Anmeldenamen werden Berechtigungen für die Verbindung mit dem Endpunkt erteilt. Beispielsweise auf einem Replikat mit der Bezeichnung „InstanceB“:

   ```sql
   CREATE LOGIN InstanceA_Login WITH PASSWORD = 'Strong Password';
   GO
   ```

9. Erstellen Sie für alle Instanzen einen Benutzer für jeden Anmeldenamen, der erstellt wurde. Dieser wird beim Wiederherstellen der Zertifikate verwendet. Geben Sie beispielsweise Folgendes ein, um einen Benutzer für das primäre Replikat zu erstellen:

   ```sql
   CREATE USER InstanceA_User FOR LOGIN InstanceA_Login;
   GO
   ```

10. Erstellen Sie für jedes Replikat, das möglicherweise ein primäres Replikat ist, einen Anmeldenamen und Benutzer auf allen entsprechenden sekundären Replikaten.
11. Stellen Sie auf jeder Instanz die Zertifikate für die anderen Instanzen wieder her, für die ein Anmeldename und Benutzer erstellt wurde. Stellen Sie alle sekundären Replikatzertifikate auf dem primären Replikat wieder her. Stellen Sie auf jedem sekundären Replikat das Zertifikat des primären Replikats wieder her. Wiederholen Sie diesen Vorgang für jedes andere Replikat, das das primäre Replikat sein kann. Beispiel:

   ```sql
   CREATE CERTIFICATE [InstanceB_Cert]
   AUTHORIZATION InstanceB_User
   FROM FILE = 'Restore_path\InstanceB_Cert.cer'
   ```

12. Erstellen Sie den Endpunkt, der von der Verfügbarkeitsgruppe auf jeder Instanz verwendet wird, die ein Replikat sein wird. Für Verfügbarkeitsgruppen muss der Endpunkt über einen Typ DATABASE_MIRRORING verfügen. Der Endpunkt nutzt das in Schritt 4 erstellte Zertifikat für diese Instanz zur Authentifizierung. Eine Beispielsyntax zum Erstellen eines Endpunkts mithilfe eines Zertifikats ist unten dargestellt. Verwenden Sie die passende Verschlüsselungsmethode und andere Optionen, die relevant für Ihre Umgebung sind. Weitere Informationen zu den verfügbaren Optionen finden Sie unter [CREATE ENDPOINT (Transact-SQL)](../../../t-sql/statements/create-endpoint-transact-sql.md).

   ```sql
   CREATE ENDPOINT DIAG_EP
   STATE = STARTED
   AS TCP (   
    LISTENER_PORT = 5022,
    LISTENER_IP = ALL
         )
   FOR DATABASE_MIRRORING (
    AUTHENTICATION = CERTIFICATE InstanceX_Cert,
    ROLE = ALL
         )
   ```

13. Weisen Sie jedem in dieser Instanz in Schritt 8 erstellten Anmeldenamen Rechte zu, sodass dieser eine Verbindung mit dem Endpunkt herstellen kann. 

   ```sql
   GRANT CONNECT ON ENDPOINT::DIAG_EP TO [InstanceX_Login];
   GO
   ```

14. Sobald die zugrunde liegenden Zertifikate und die Endpunktsicherheit konfiguriert ist, erstellen Sie die Verfügbarkeitsgruppe mit Ihrer bevorzugten Methode. Es wird empfohlen, eine manuelle Sicherung sowie eine Kopie und Wiederherstellung der Sicherung auszuführen, die zum Initialisieren des sekundären Replikats verwendet wird, oder verwenden Sie alternativ [automatisches Seeding](automatically-initialize-always-on-availability-group.md). Das Verwenden des Assistenten zum Initialisieren des sekundären Replikats beinhaltet die Verwendung der Server Message Block-Dateien (SMB), die möglicherweise nicht funktionieren, wenn Sie einen Workgroupcluster nutzen, der nicht mit der Domäne verbunden ist.
15. Wenn Sie einen Listener erstellen, stellen Sie sicher, dass der Name und die IP-Adresse jeweils in DNS registriert sind.

### <a name="next-steps"></a>Nächste Schritte 

- [Verwenden des Assistenten zum Hinzufügen von Datenbanken zu Verfügbarkeitsgruppen (SQL Server)](use-the-availability-group-wizard-sql-server-management-studio.md)

- [Verwenden des Dialogfelds Neue Verfügbarkeitsgruppe (SQL Server Management Studio)](use-the-new-availability-group-dialog-box-sql-server-management-studio.md)
 
- [Erstellen einer Verfügbarkeitsgruppe mit Transact-SQL](create-an-availability-group-transact-sql.md)

<!--Image references-->
[1]: ./media/diag-wsfc-two-data-centers-same-domain.png
[2]: ./media/diag-workgroup-cluster-two-nodes-joined.png
[3]: ./media/diag-high-level-view-ag-standard-edition.png
[4]: ./media/diag-successful-dns-suffix.png
