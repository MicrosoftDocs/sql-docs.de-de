---
title: Scale Out-Unterstützung für Hochverfügbarkeit | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die SQL Server Integration Services (SSIS) Scale Out-Masterseite für Hochverfügbarkeit konfigurieren.
ms.custom: performance
ms.date: 05/23/2018
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: haoqian
ms.author: haoqian
ms.openlocfilehash: dd53bf6f045687f7d79f6c0e836490adb60cda44
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348858"
---
# <a name="scale-out-support-for-high-availability"></a>Scale Out-Unterstützung für Hochverfügbarkeit

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



In SSIS Scale Out wird die Hochverfügbarkeit auf Workerseite über ausführende Pakete mit mehreren Scale Out-Workern bereitgestellt.

Die Hochverfügbarkeit auf Scale Out-Masterseite erfolgt über [Always On for SSIS Catalog (Always On für den SSIS-Katalog)](../catalog/ssis-catalog.md#always-on-for-ssis-catalog-ssisdb) und Windows-Failovercluster. In dieser Lösung werden in einem Windows-Failovercluster werden mehrere Instanzen des Scale Out-Masters gehostet. Wenn der Scale Out-Masterdienst oder die SSISDB auf dem Primärknoten ausfallen, akzeptieren der Dienst oder die SSISDB auf dem Sekundärknoten weiterhin Benutzeranforderungen, und beide kommunizieren mit den Scale Out-Workern.

Alternativ kann die Hochverfügbarkeit auf Scale Out-Masterseite mithilfe einer SQL Server-Failoverclusterinstanz eingerichtet werden. Weitere Informationen finden Sie unter [Scale Out support for high availability via SQL Server failover cluster instance (Scale Out-Unterstützung für Hochverfügbarkeit über eine SQL Server-Failoverclusterinstanz)](scale-out-failover-cluster-instance.md).

Führen Sie die folgenden Schritte aus, um die Hochverfügbarkeit auf der Scale Out-Masterseite mit Always On für den SSIS-Katalog einzurichten:

## <a name="1-prerequisites"></a>1. Voraussetzungen
Erstellen Sie einen Windows-Failovercluster. Weitere Anweisungen finden Sie im Blogbeitrag [Installing the Failover Cluster Feature and Tools for Windows Server 2012 (Installation des Failoverclusterfeatures und des Tools für Windows Server 2012)](https://techcommunity.microsoft.com/t5/failover-clustering/installing-the-failover-cluster-feature-and-tools-in-windows/ba-p/371733). Installieren Sie die Funktion und die Tools auf allen Clusterknoten.

## <a name="2-install-scale-out-master-on-the-primary-node"></a>2. Installieren des Scale Out-Masters auf dem Primärknoten
Installieren Sie SQL Server Database Engine Services, Integration Services und den Scale Out-Master auf dem Primärknoten für den Scale Out-Master. 

Führen Sie bei der Installation die folgenden Schritte aus:

### <a name="21-set-the-account-running-scale-out-master-service-to-a-domain-account"></a>2.1 Legen Sie das Konto, das den Scale Out-Masterdienst ausführt, auf ein Domänenkonto fest
Dieses Konto sollte zukünftig Zugriff auf die SSISDB auf dem Sekundärknoten im Windows-Failovercluster haben. Da für den Scale Out-Masterdienst und die SSISDB unabhängig voneinander Failover ausgeführt werden können, befinden sie sich nach der Failoverausführung möglicherweise nicht auf demselben Knoten.

![Serverkonfiguration für Hochverfügbarkeit](media/ha-server-config.PNG)

### <a name="22-include-the-dns-host-name-for-the-scale-out-master-service-in-the-cns-of-the-scale-out-master-certificate"></a>2.2 Nehmen Sie den DNS-Hostnamen des Scale Out-Masterdiensts in den allgemeinen Namen des Scale Out-Masterzertifikats auf

Dieser Hostname entspricht dem Scale Out-Masterendpunkt, der als allgemeiner Clusterdienst im Failovercluster erstellt wird (siehe Schritt 7).   (Achten Sie darauf, einen DNS-Hostnamen anzugeben, keinen Servernamen.)

![Masterkonfiguration für Hochverfügbarkeit](media/ha-master-config.PNG)

## <a name="3-install-scale-out-master-on-the-secondary-node"></a>3. Installieren des Scale Out-Masters auf dem Sekundärknoten
Installieren Sie SQL Server Database Engine Services, Integration Services und den Scale Out-Master auf dem Sekundärknoten für den Scale Out-Master. 

Verwenden Sie dasselbe Scale Out-Masterzertifikat wie für den Primärknoten. Exportieren Sie das TLS/SSL-Zertifikat für den Scale Out-Master auf dem Primärknoten mit einem privaten Schlüssel, und installieren Sie es im Stammzertifikatspeicher des lokalen Computers auf dem Sekundärknoten. Wählen Sie dieses Zertifikat bei der Installation des Scale Out-Masters auf dem Sekundärknoten aus.

![Masterkonfiguration für Hochverfügbarkeit 2](media/ha-master-config2.PNG)

> [!NOTE]
> Sie können mehrere Sicherungskopien des Scale Out-Masters einrichten, indem Sie diese Vorgänge für die Scale Out-Master auf weiteren Sekundärknoten wiederholen.

## <a name="4-set-up-and-configure-ssisdb-support-for-always-on"></a>4. Einrichten und Konfigurieren der SSISDB-Unterstützung für Always On

Führen Sie die Anweisungen zur Einrichtung und Konfiguration der SSISDB-Unterstützung für Always On durch, die unter [Always On for SSIS Catalog (SSISDB) (Always On für den SSIS-Katalog (SSISDB))](../catalog/ssis-catalog.md#always-on-for-ssis-catalog-ssisdb) beschrieben werden.

Außerdem müssen Sie einen Verfügbarkeitsgruppenlistener für die Verfügbarkeitsgruppe erstellen, zu der Sie SSISDB hinzufügen. Vgl. [Create or Configure an Availability Group Listener (Erstellen oder Konfigurieren eines Verfügbarkeitsgruppenlisteners)](../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md).

## <a name="5-update-the-scale-out-master-service-configuration-file"></a>5. Aktualisieren Sie die Konfigurationsdatei des Scale Out-Masterdiensts
Aktualisieren Sie die Konfigurationsdatei des Scale Out-Masterdiensts (`\<drive\>:\Program Files\Microsoft SQL Server\140\DTS\Binn\MasterSettings.config`) auf dem Primär- und dem Sekundärknoten. Aktualisieren Sie **SqlServerName** auf *[DNS des Verfügbarkeitsgruppenlisteners],[Port]* .

## <a name="6-enable-package-execution-logging"></a>6. Aktualisieren Sie die Ausführungsprotokollierung.

Sie können sich bei SSISDB mit der Anmelde-ID **##MS_SSISLogDBWorkerAgentLogin##** anmelden, für die ein automatisches Kennwort generiert wurde. Führen Sie die folgenden Schritte aus, damit die Protokollierung für alle SSISDB-Replikate funktioniert.

### <a name="61-change-the-password-of-ms_ssislogdbworkeragentlogin-on-the-primary-sql-server"></a>6.1 Ändern Sie das Kennwort von **##MS_SSISLogDBWorkerAgentLogin##** auf dem primären SQL Server

### <a name="62-add-the-login-to-the-secondary-sql-server"></a>6.2. Fügen Sie dem sekundären SQL Server die Anmelde-ID hinzu

### <a name="63-update-the-connection-string-used-for-logging"></a>6.3 Aktualisieren Sie die Verbindungszeichenfolge, die für die Protokollierung verwendet wird
Rufen Sie die gespeicherte Prozedur `[catalog].[update_logdb_info]` mithilfe der folgenden Parameterwerte auf:

-   `@server_name = '[Availability Group Listener DNS name],[Port]'`

-   `@connection_string = 'Data Source=[Availability Group Listener DNS name],[Port];Initial Catalog=SSISDB;User Id=##MS_SSISLogDBWorkerAgentLogin##;Password=[Password]];'`

## <a name="7-configure-the-scale-out-master-service-role-of-the-windows-server-failover-cluster"></a>7. Konfigurieren der Rolle des Scale Out-Masterdiensts des Windows Server-Failoverclusters

1.  Stellen Sie im Failovercluster-Manager eine Verbindung mit dem Cluster für Scale Out her. Wählen Sie den Cluster aus. Wählen Sie im Menü zunächst **Aktion** und dann **Rolle konfigurieren** aus.

2.  Wählen Sie im Dialogfeld **Assistent für Hochverfügbarkeit** auf der Seite **Rolle auswählen** **allgemeiner Dienst** aus. Wählen Sie den SQL Server Integration Services Scale Out-Master 14.0 auf der Seite **Dienst auswählen** aus.

3.  Geben Sie auf der Seite **Clientzugriffspunkt** den DNS-Hostnamen des Scale Out-Masterdiensts ein.

    ![Hochverfügbarkeitsassistent 1](media/ha-wizard1.PNG)

4.  Beenden Sie den Assistenten.

Auf virtuellen Azure-Computern erfordert diese Konfiguration zusätzliche Schritte. Eine vollständige Erläuterung dieser Konzepte und dieser Schritte ist nicht Gegenstand dieses Artikels.

1.  Sie müssen eine Azure-Domäne einrichten. Windows Server-Failoverclustering erfordert, dass alle Computer im Cluster Mitglieder derselben Domäne sind. Weitere Informationen finden Sie unter [Enable Azure Active Directory Domain Services using the Azure portal (Aktivieren von Azure Active Directory Domain Services über das Azure-Portal)](/azure/active-directory-domain-services/create-instance).

2. Sie müssen einen Azure Load Balancer einrichten. Dies ist für den Verfügbarkeitsgruppenlistener erforderlich. Weitere Informationen finden Sie unter [Tutorial: Ausgleichen der internen Datenverkehrslast mithilfe eines Lastenausgleichs im Tarif „Basic“ über das Azure-Portal](/azure/load-balancer/tutorial-load-balancer-basic-internal-portal).

## <a name="8-update-the-scale-out-master-address-in-ssisdb"></a>8. Aktualisieren der Scale Out-Masteradresse in SSISDB

Führen Sie auf dem primären SQL Server die gespeicherte Prozedur `[catalog].[update_master_address]` mithilfe des Parameterwerts `@MasterAddress = N'https://[Scale Out Master service DNS host name]:[Master Port]'` aus. 

## <a name="9-add-the-scale-out-workers"></a>9. Hinzufügen des Scale Out-Workers

Jetzt können Sie Scale Out-Worker mithilfe des [Integration Services Scale Out-Managers](integration-services-ssis-scale-out-manager.md) hinzufügen. Geben Sie auf der Verbindungsseite `[SQL Server Availability Group Listener DNS name],[Port]` ein.

## <a name="upgrade-scale-out-in-high-availability-environment"></a>Upgrade von Scale Out in einer Hochverfügbarkeitsumgebung
Befolgen Sie für ein Upgrade von Scale Out in einer Hochverfügbarkeitsumgebung die Schritte [zum Upgrade von Always On für den SSIS-Katalog](../catalog/ssis-catalog.md#Upgrade). Aktualisieren Sie Scale Out-Master und Scale Out-Worker auf jedem Computer, und erstellen Sie die Failoverclusterrolle von Windows Server in Schritt 7 mit einer neuen Version des Scale Out-Masterdiensts.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in den folgenden Artikeln:
-   [Scale Out-Master von Integration Services (SSIS)](integration-services-ssis-scale-out-master.md)
-   [Scale Out-Worker von Integration Services (SSIS)](integration-services-ssis-scale-out-worker.md)