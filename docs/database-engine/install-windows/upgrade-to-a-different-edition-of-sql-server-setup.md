---
title: Durchführen eines Upgrades auf eine andere Edition
description: Das SQL Server-Setup unterstützt Editionsupgrades für verschiedene Editionen von SQL Server. Überprüfen Sie die in diesem Artikel bereitgestellten Ressourcen, bevor Sie ein Editionsupgrade beginnen.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: 31d16820-d126-4c57-82cc-27701e4091bc
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: cb7bbbe15bee9219bf78430421bf99ea6b63676f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353821"
---
# <a name="upgrade-to-a-different-edition-of-sql-server-setup"></a>Aktualisieren auf eine andere Edition von SQL Server (Setup)

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Setup unterstützt das Editionsupgrade unter den verschiedenen Editionen von [!INCLUDE[ssNoversion](../../includes/ssnoversion-md.md)]. Weitere Informationen finden Sie unter [Unterstützte Versions- und Editionsupgrades](../../database-engine/install-windows/supported-version-and-edition-upgrades-2017.md). Bevor Sie das Editionsupgrade einer Instanz von [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] initiieren, lesen Sie die folgenden Artikel:  

  [Editionen und unterstützten Features von SQL Server 2019](../../sql-server/editions-and-components-of-sql-server-version-15.md)
- [Editionen und unterstützten Funktionen von SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md)  
- [Editionen und unterstützten Funktionen von SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md)  
- [Rechenkapazitätsgrenzen von bestimmten Editionen von SQL Server](../../sql-server/compute-capacity-limits-by-edition-of-sql-server.md)  
- [Hardware- und Softwareanforderungen für die Installation von SQL Server](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)  
  
> [!NOTE]  
> **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf einer Failoverclusterinstanz:** Die Ausführung eines Editionsupgrades auf einem der Knoten der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Failoverclusterinstanz reicht aus. Dieser Knoten kann entweder aktiv oder passiv sein, und die Engine schaltet während des Editionsupgrades die Ressourcen nicht offline. Nach dem Editionsupgrade muss entweder die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz oder das Failover auf einem anderen Knoten neu gestartet werden.  
  
## <a name="prerequisites"></a>Voraussetzungen  
Bei lokalen Installationen müssen Sie das Setup als Administrator ausführen. Wenn Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] von einer Remotefreigabe installieren, müssen Sie auf der Remotefreigabe ein Domänenkonto mit Leseberechtigungen verwenden.  
  
> [!IMPORTANT]  
> Damit die Änderung der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Edition wirksam wird, müssen Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Dienste neu starten. Dies führt zu einem Ausfall der Anwendung, während die Dienste offline sind.  
  
## <a name="procedure"></a>Verfahren  
  
### <a name="to-upgrade-to-a-different-edition-of-ssnoversion"></a>So führen Sie ein Upgrade auf eine andere Edition von [!INCLUDE[ssNoversion](../../includes/ssnoversion-md.md)] durch  
  
1.  Legen Sie das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Installationsmedium ein. Doppelklicken Sie im Stammordner auf setup.exe, oder starten Sie das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Installationscenter über die Konfigurationstools. Wenn Sie eine Installation über eine Netzwerkfreigabe vornehmen möchten, suchen Sie den Stammordner in der Freigabe, und doppelklicken Sie auf setup.exe.  
  
2.  Um eine vorhandene Instanz von [!INCLUDE[ssNoversion](../../includes/ssnoversion-md.md)] auf eine andere Edition zu aktualisieren, klicken Sie im [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Installationscenter auf **Wartung**, und wählen Sie anschließend **Editionsupgrade** aus.  
  
3.  Wenn Setup-Unterstützungsdateien erforderlich sind, werden diese durch das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Setup installiert. Wenn Sie zum Neustarten des Computers aufgefordert werden, führen Sie einen Neustart durch, bevor Sie den Vorgang fortsetzen.  
  
4.  Die Systemkonfigurationsprüfung führt einen Ermittlungsvorgang auf dem Computer aus. Klicken Sie zum Fortsetzen des Vorgangs auf **OK**.  
  
5.  Klicken Sie auf der Seite Product Key auf ein Optionsfeld, um anzugeben, ob Sie auf eine kostenlose Edition von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]aktualisieren oder über einen PID-Schlüssel für eine Produktionsversion des Produkts verfügen. Weitere Informationen finden Sie unter [Editions and Components of SQL Server (Editionen und Komponenten von SQL Server)](../../sql-server/editions-and-components-of-sql-server-2017.md) und unter [Supported Version and Edition Upgrades (Unterstützte Versions- und Editionsupgrades)](../../database-engine/install-windows/supported-version-and-edition-upgrades.md).  
  
6.  Lesen Sie auf der Seite mit den Lizenzbedingungen den Lizenzvertrag, und aktivieren Sie dann das Kontrollkästchen, um die Lizenzbestimmungen zu akzeptieren. Klicken Sie auf **Weiter**, um den Vorgang fortzusetzen. Klicken Sie auf **Abbrechen**, wenn Sie den Setupvorgang beenden möchten.  
  
7.  Geben Sie auf der Seite Instanz auswählen die zu aktualisierende Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] an.  
  
8.  Auf der Seite Editionsupgraderegeln wird die Konfiguration Ihres Computers vor Beginn des Editionsupgrades überprüft.  
  
9. Auf der Seite Die Edition kann jetzt aktualisiert werden wird eine Strukturansicht der Installationsoptionen angezeigt, die während des Setups angegeben wurden. Klicken Sie zum Fortsetzen des Vorgangs auf **Aktualisieren**.  
  
10. Während des Editionsupgradeprozesses müssen die Dienste neu gestartet werden, damit die neuen Einstellungen übernommen werden. Nach dem Editionsupgrade wird auf der Seite Abgeschlossen ein Link zur zusammenfassenden Protokolldatei für das Editionsupgrade bereitgestellt. Klicken Sie auf **Schließen**, um den Assistenten zu schließen.  
  
11. Auf der Seite Abgeschlossen finden Sie einen Link zur zusammenfassenden Protokolldatei für die Installation und andere wichtige Hinweise.  
  
12. Starten Sie den Computer neu, falls Sie dazu aufgefordert werden. Nachdem das Setup abgeschlossen ist, sollten Sie unbedingt die vom Installations-Assistenten ausgegebene Meldung lesen. Weitere Informationen finden Sie unter [Lesen und Anzeigen der Setupprotokolldateien von SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).  
  
13. Wenn Sie ein Upgrade von [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]durchgeführt haben, müssen Sie zusätzliche Schritte ausführen, bevor Sie die aktualisierte Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]verwenden können:  
  
    -   Aktivieren Sie den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent-Dienst im Dienstkontroll-Manager von Windows (SCM).  
  
    -   Stellen Sie das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent-Dienstkonto mit dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Konfigurations-Manager bereit.  
  
 Wenn Sie von [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]aktualisiert haben, müssen Sie zusätzlich zu den oben beschriebenen Schritten möglicherweise die folgenden Schritte ausführen:  
  
-   Benutzer, die in [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] bereitgestellt wurden, stehen auch nach dem Upgrade zur Verfügung. Dies gilt insbesondere für die Gruppe BUILTIN\Users. Deaktivieren oder entfernen Sie diese Konten nach Bedarf, oder stellen Sie sie erneut bereit. Weitere Informationen finden Sie unter [Konfigurieren von Windows-Dienstkonten und -Berechtigungen](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)betreffen.  
  
-   Die Größe und der Wiederherstellungsmodus für tempdb und Systemdatenbanken bleiben nach dem Upgrade unverändert. Konfigurieren Sie diese Einstellungen bei Bedarf neu. Weitere Informationen finden Sie unter [Sichern und Wiederherstellen von Systemdatenbanken &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md).  
  
-   Vorlagendatenbanken verbleiben nach dem Upgrade auf dem Computer.  

> [!NOTE]  
> Wenn die Prozedur bei der Engine_SqlEngineHealthCheck-Regel fehlschlägt, können Sie die Installationsoption über die Befehlszeile verwenden, um diese spezifische Regel zu überspringen, damit der Upgradevorgang erfolgreich abgeschlossen werden kann. Öffnen Sie eine Eingabeaufforderung, und legen Sie den Pfad fest, unter dem sich die SQL Server-Setupdatei („Setup.exe“) befindet, um die Überprüfung dieser Regel zu überspringen. Geben Sie dann den folgenden Befehl ein: 

```console
setup.exe /q /ACTION=editionupgrade /InstanceName=MSSQLSERVER /PID=<appropriatePid> /SkipRules=Engine_SqlEngineHealthCheck
```


## <a name="see-also"></a>Weitere Informationen  
 [Upgrade von SQL Server](../../database-engine/install-windows/upgrade-sql-server.md)   
 [Abwärtskompatibilität](/previous-versions/sql/sql-server-2016/cc280407(v=sql.130))  
  
