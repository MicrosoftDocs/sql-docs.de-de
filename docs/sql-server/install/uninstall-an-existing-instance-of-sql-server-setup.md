---
title: Deinstallieren einer vorhandenen Instanz
description: In diesem Artikel erfahren Sie, wie Sie eine eigenständige SQL Server-Instanz deinstallieren. Dabei wird auch das System vorbereitet, sodass Sie SQL Server neu installieren können.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- removing instances of SQL Server
- uninstalling instances of SQL Server
- removing SQL Server
- instances of SQL Server, uninstalling
- uninstalling SQL Server
ms.assetid: 3c64b29d-61d7-4b86-961c-0de62261c6a1
author: cawrites
ms.author: chadam
ms.openlocfilehash: 6a5b77c8e67d6512e691f748265e7901319a5e4f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336084"
---
# <a name="uninstall-an-existing-instance-of-sql-server-setup"></a>Deinstallieren einer vorhandenen SQL Server-Instanz (Setup)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  In diesem Artikel wird beschrieben, wie eine eigenständige Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] deinstalliert wird. Mit den Schritten in diesem Artikel bereiten Sie das System außerdem für die erneute Installation von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vor.  
  
 > [!NOTE]
 > Zur Deinstallation eines [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Failovercluster verwenden Sie die vom [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Setup bereitgestellte Funktion Knoten entfernen, um jeden Knoten einzeln zu entfernen. Weitere Informationen finden Sie unter [Hinzufügen oder Entfernen von Knoten in einem SQL Server-Failovercluster &#40;Setup&#41;](../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md).  

## <a name="considerations"></a>Überlegungen

- Um SQL Server zu deinstallieren, müssen Sie ein lokaler Administrator mit der Berechtigung zur Anmeldung als Dienst sein. 
- Wenn Ihr Computer über die *mindestens* erforderliche Menge an physischem Arbeitsspeicher verfügt, setzen Sie die Größe der Auslagerungsdatei auf die doppelte Menge des physischen Arbeitsspeichers herauf. Unzureichender virtueller Arbeitsspeicher kann zu einer unvollständigen Entfernung von SQL Server führen. 
- Auf einem System mit mehreren SQL Server-Instanzen wird der SQL Server-Browserdienst erst bei der Entfernung der letzten Instanz von SQL Server deinstalliert. Der SQL Server-Browserdienst kann manuell über **Programme und Features** in der **Systemsteuerung** entfernt werden. 
- Beim Deinstallieren von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] werden tempdb-Datendateien gelöscht, die während des Installationsvorgangs hinzugefügt wurden. Dateien mit dem Namensmuster „tempdb_mssql_*.ndf“ werden gelöscht, wenn sie im Verzeichnis der Systemdatenbank vorhanden sind. 
  

  
## <a name="prepare"></a>Vorbereiten  
  
1.  **Sichern Sie die Daten.** Erstellen Sie entweder [vollständige Sicherungen](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md) aller Datenbanken, einschließlich der Systemdatenbanken, oder kopieren Sie die MDF- und LDF-Dateien manuell an einen separaten Speicherort. Die **master**-Datenbank enthält alle Informationen für den Server auf Systemebene, wie etwa Anmeldeinformationen und Schemas. Die **MSDB**-Datenbank enthält Auftragsinformationen, wie etwa Aufträge für den SQL Server-Agent, den Sicherungsverlauf und Wartungspläne. Weitere Informationen zu Systemdatenbanken finden Sie unter [Systemdatenbanken](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md). 
  
    Die Dateien, die Sie speichern müssen, umfassen die im Folgenden aufgeführten Datenbankdateien:  

    * master.mdf
    * msdbdata.mdf
    * Tempdb.mdf
    * mastlog.ldf
    * msdblog.ldf
    * Templog.ldf
    * model.mdf
    * Mssqlsystemresource.mdf
    * ReportServer[$InstanceName]
    * modellog.ldf
    * Mssqlsustemresource.ldf
    * ReportServer[$InstanceName]TempDB

    > [!NOTE]
    > Die ReportServer-Datenbanken sind in SQL Server Reporting Services enthalten.   

 
1.  **Beenden Sie alle** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-**Dienste.** Beenden Sie alle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Dienste vor dem Deinstallieren von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Komponenten. Aktive Verbindungen können eine erfolgreiche Deinstallation verhindern.  
  
1.  **Verwenden Sie ein Konto mit den entsprechenden Berechtigungen.** Melden Sie sich am Server an, indem Sie das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Dienstkonto oder ein Konto mit entsprechenden Berechtigungen verwenden. Beispielsweise können Sie sich mit einem Konto beim Server anmelden, das Mitglied der lokalen Administratorgruppe ist.  
  
## <a name="uninstall"></a>Deinstallieren 

# <a name="windows-10--2016-"></a>[Windows 10/2016+](#tab/Windows10)

Führen Sie die folgenden Schritte aus, um SQL Server unter Windows 10, Windows Server 2016, Windows Server 2019 und höher zu deinstallieren: 

1. Um den Entfernungsprozess einzuleiten, navigieren Sie im Startmenü zu **Einstellungen** und wählen dann **Apps** aus. 
1. Suchen Sie im Suchfeld nach `sql`. 
1. Wählen Sie **Microsoft SQL Server (Version) (Bit)** aus. Beispiel: `Microsoft SQL Server 2017 (64-bit)`.
1. Wählen Sie **Deinstallieren** aus.
 
    ![Deinstallieren von SQL Server](media/uninstall-an-existing-instance-of-sql-server-setup/uninstall-sql-server-windows-10.png)

1. Wählen Sie im SQL Server-Dialogfeld-Popup **Entfernen** aus, um den Microsoft SQL Server-Installations-Assistenten zu starten. 

    ![Entfernen von SQL Server](media/uninstall-an-existing-instance-of-sql-server-setup/remove-sql-2017.png)
  
1.  Wählen Sie im Dropdownfeld auf der Seite **Instanz auswählen** die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aus, die Sie entfernen möchten, oder legen Sie fest, dass nur die freigegebenen Funktionen sowie die Verwaltungstools von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] entfernt werden sollen. Wählen Sie zum Fortsetzen des Vorgangs **Weiter** aus.  
  
1.  Geben Sie auf der Seite **Funktionen auswählen** die Funktionen an, die aus der angegebenen Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] entfernt werden sollen.  
  
1.  Überprüfen Sie auf der Seite **Deinstallation kann jetzt ausgeführt werden** die Liste der Komponenten und Funktionen, die deinstalliert werden sollen. Klicken Sie auf **Entfernen**, um mit der Deinstallation zu beginnen.  
 
1. Aktualisieren Sie das Fenster **Apps und Features**, um zu überprüfen, ob die SQL Server-Instanz erfolgreich entfernt wurde, und bestimmen Sie, ob noch Komponenten von SQL Server vorhanden sind. Entfernen Sie auch diese Komponenten über dieses Fenster, wenn Sie dies wünschen. 

# <a name="windows-2008---2012-r2"></a>[Windows 2008–2012 R2](#tab/windows2012)

Führen Sie die folgenden Schritte aus, um SQL Server unter Windows Server 2008, Windows Server 2012 und Windows 2012 R2 zu deinstallieren: 

1. Um den Entfernungsprozess einzuleiten, navigieren Sie zur **Systemsteuerung**, und wählen Sie dann **Programme und Features** aus.
1. Klicken Sie mit der rechten Maustaste auf **Microsoft SQL Server (Version) (Bit)** , und wählen Sie **Deinstallieren** aus. Beispiel: `Microsoft SQL Server 2012 (64-bit)`.  
  
    ![Deinstallieren von SQL Server](media/uninstall-an-existing-instance-of-sql-server-setup/uninstall-sql-server-windows-2012.png)

1. Wählen Sie im SQL Server-Dialogfeld-Popup **Entfernen** aus, um den Microsoft SQL Server-Installations-Assistenten zu starten. 

    ![Entfernen von SQL Server](media/uninstall-an-existing-instance-of-sql-server-setup/remove-sql-2012.png)
  
1.  Wählen Sie im Dropdownfeld auf der Seite **Instanz auswählen** die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aus, die Sie entfernen möchten, oder legen Sie fest, dass nur die freigegebenen Funktionen sowie die Verwaltungstools von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] entfernt werden sollen. Wählen Sie zum Fortsetzen des Vorgangs **Weiter** aus.  
  
1.  Geben Sie auf der Seite **Funktionen auswählen** die Funktionen an, die aus der angegebenen Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] entfernt werden sollen.  
  
1.  Überprüfen Sie auf der Seite **Deinstallation kann jetzt ausgeführt werden** die Liste der Komponenten und Funktionen, die deinstalliert werden sollen. Klicken Sie auf **Entfernen**, um mit der Deinstallation zu beginnen.  
 
1. Aktualisieren Sie das Fenster **Programme und Features**, um zu überprüfen, ob die SQL Server-Instanz erfolgreich entfernt wurde, und bestimmen Sie, ob noch Komponenten von SQL Server vorhanden sind. Entfernen Sie auch diese Komponenten über dieses Fenster, wenn Sie dies wünschen. 

---

  
## <a name="in-the-event-of-failure"></a>Im Fall eines Fehlers  

Wenn beim Entfernungsprozess ein Fehler auftritt, überprüfen Sie die [SQL Server-Setupprotokolldateien](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md), um die Grundursache zu bestimmen. 

Der KB-Artikel [How to identify SQL Server setup issues in the setup log files](https://support.microsoft.com/kb/955396/en-us) (Identifizieren von SQL Server-Setupproblemen in den Setupprotokolldateien) kann Ihnen bei der Untersuchung weiterhelfen. Er bezieht sich zwar auf SQL Server 2008, die beschriebene Methodik lässt sich aber auf jede Version von SQL Server anwenden. 

  
## <a name="see-also"></a>Weitere Informationen  
 [Lesen und Anzeigen der Setupprotokolldateien von SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)  
  
  
