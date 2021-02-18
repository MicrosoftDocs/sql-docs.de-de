---
title: Konfigurieren von Firewallregeln vor dem Ausführen des T-SQL-Debuggers
description: Hier erfahren Sie, wie Sie die Windows-Firewallregeln vor dem Ausführen des T-SQL-Debuggers (Transact-SQL) konfigurieren, wenn in SQL Server Management Studio eine Verbindung zu einem SQL-Server besteht.
ms.custom: seo-lt-2019
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- vs.debug.error.sqlde_accessdenied
- vs.debug.error.sqlde_register_failed
- vs.debug.error.sqlde_firewall.remotemachines
helpviewer_keywords:
- Transact-SQL debugger, remote connections
- Windows Firewall [Database Engine], Transact-SQL debugger
- Transact-SQL debugger, Windows Firewall
- Transact-SQL debugger, configuring
- ports [SQL Server], Transact-SQL debugger
- TCP/IP [SQL Server], port numbers
ms.assetid: f50e0b0d-eaf0-4f4a-be83-96f5be63e7ea
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.date: 10/20/2016
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4a55de0f9f6a6b8ae80161adaa0252b3d5c4a774
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354690"
---
# <a name="configure-firewall-rules-before-running-the-tsql-debugger"></a>Konfigurieren von Firewallregeln vor dem Ausführen des TSQL-Debuggers

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Sie müssen Windows-Firewall-Regeln so konfigurieren, dass das Debuggen von [!INCLUDE[tsql](../../includes/tsql-md.md)] aktiviert ist, wenn eine Verbindung mit einer Instanz von [!INCLUDE[ssDE](../../includes/ssde-md.md)] hergestellt wird, die auf einem anderen Computer als der [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Abfrage-Editor ausgeführt wird.

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]

## <a name="configuring-the-transact-sql-debugger"></a>Konfigurieren des Transact-SQL-Debuggers

Der [!INCLUDE[tsql](../../includes/tsql-md.md)] -Debugger umfasst sowohl serverseitige als auch clientseitige Komponenten. Die serverseitigen Debuggerkomponenten werden mit jeder Instanz der Datenbank-Engine von [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 2 (SP2) oder höher installiert. Die clientseitigen Debuggerkomponenten sind inbegriffen:

- Wenn Sie die clientseitigen Tools von [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] oder höher installieren.

- Wenn Sie Microsoft Visual Studio 2010 oder höher installieren.

- Wenn Sie [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] vom Webdownload installieren.

Es gibt keine Konfigurationsvoraussetzungen für die Ausführung des [!INCLUDE[tsql](../../includes/tsql-md.md)] -Debuggers, wenn [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] auf demselben Computer wie die Instanz von [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]ausgeführt wird. Um jedoch den [!INCLUDE[tsql](../../includes/tsql-md.md)] -Debugger auszuführen, wenn eine Verbindung mit einer Remoteinstanz von [!INCLUDE[ssDE](../../includes/ssde-md.md)]besteht, müssen auf beiden Computern in der Windows-Firewall Programm- und Portausnahmen aktiviert sein. Diese Regeln werden möglicherweise durch [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Setup erstellt. Wenn Ihnen beim Versuch, eine Remotedebugsitzung zu öffnen, Fehler angezeigt werden, stellen Sie sicher, dass die folgenden Firewallregeln auf dem Computer definiert sind.

Verwenden Sie die Anwendung **Windows-Firewall mit erweiterter Sicherheit** , um die Firewallregeln zu verwalten. Öffnen Sie in sowohl [!INCLUDE[win7](../../includes/win7-md.md)] als auch [!INCLUDE[winserver2008r2](../../includes/winserver2008r2-md.md)]**Systemsteuerung**, öffnen Sie **Windows-Firewall** und wählen Sie **Erweiterte Einstellungen** aus. In [!INCLUDE[winserver2008r2](../../includes/winserver2008r2-md.md)] können Sie auch **Dienst-Manager** öffnen, im linken Bereich **Konfiguration** sowie **Windows-Firewall mit erweiterter Sicherheit** erweitern.

> [!CAUTION]
>  Wenn Sie Regeln in der Windows-Firewall aktivieren, kann dies dazu führen, dass Ihr Computer Sicherheitsrisiken ausgesetzt ist, die von der Firewall normalerweise geblockt werden. Durch das Aktivieren von Regeln für Remotedebugging wird die Blockierung der in diesem Thema aufgeführten Ports und Programme aufgehoben.

## <a name="firewall-rules-on-the-server"></a>Firewallregeln für den Server
Verwenden Sie auf dem Computer, auf dem die Instanz von [!INCLUDE[ssDE](../../includes/ssde-md.md)]ausgeführt wird, **Windows-Firewall mit erweiterter Sicherheit** , um die folgenden Informationen anzugeben:

- Fügen Sie eine eingehende Programmregel für sqlservr.exe hinzu. Sie müssen eine Regel für jede Instanz besitzen, die Remotedebugsitzungen unterstützen muss.

   1. Klicken Sie im linken Bereich von **Windows-Firewall** mit erweiterter Sicherheit mit der rechten Maustaste auf **Eingehende Regeln**, und wählen Sie dann im Aktionsbereich **Neue Regel** aus.

   2. Wählen Sie im Dialogfeld **Regeltyp** die Option **Programm** aus, und klicken Sie anschließend auf **Weiter**.

   3. Wählen Sie im Dialogfeld **Programm** die Option **Dieser Programmpfad:** aus, und geben Sie den vollständigen Pfad zu sqlservr.exe für diese Instanz ein. Standardmäßig wird „sqlservr.exe“ im Verzeichnis „C:\Programme\Microsoft SQL Server\MSSQL13.*Instanzname*\MSSQL\Binn“ installiert, wobei *Instanzname* für die Standardinstanz MSSQLSERVER lautet. Für eine beliebige benannte Instanz wird der jeweilige Instanzname eingefügt.

   4. Wählen Sie im Dialogfeld **Aktion** die Option **Verbindung zulassen** aus, und klicken Sie auf **Weiter**.

   5. Wählen Sie im Dialogfeld **Profil** beliebige Profile aus, die die Verbindungsumgebung des Computers beschreiben, wenn Sie eine Debugsitzung für die Instanz öffnen möchten, und klicken Sie auf **Weiter**.

   6. Geben Sie im Dialogfeld **Name** einen Namen und eine Beschreibung für die Regel ein und klicken Sie auf **Fertig stellen**.

   7. Klicken Sie in der Liste **Eingehende Regeln** mit der rechten Maustaste auf die Regel, die Sie erstellt haben, und wählen Sie dann im Aktionsbereich **Eigenschaften** aus.

   8. Wählen Sie die Registerkarte **Protokolle und Ports** aus.

   9. Wählen Sie **TCP** im Feld **Protokolltyp:** aus, wählen Sie **Dynamische RPC-Ports** im Feld **Lokaler Port:** aus, klicken Sie auf **Anwenden** und dann auf **OK**.

- Fügen Sie eine eingehende Programmregel für svchost.exe hinzu, um DCOM-Kommunikation von Remotedebuggersitzungen zu ermöglichen.

   1. Klicken Sie im linken Bereich von **Windows-Firewall** mit erweiterter Sicherheit mit der rechten Maustaste auf **Eingehende Regeln**, und wählen Sie dann im Aktionsbereich **Neue Regel** aus.

   2. Wählen Sie im Dialogfeld **Regeltyp** die Option **Programm** aus, und klicken Sie anschließend auf **Weiter**.

   3. Wählen Sie im Dialogfeld **Programm****Dieser Programmpfad:** aus, und geben Sie den vollständigen Pfad zu svchost.exe ein. Standardmäßig ist svchost.exe in %systemroot%\System32\svchost.exe. installiert.

   4. Wählen Sie im Dialogfeld **Aktion** die Option **Verbindung zulassen** aus, und klicken Sie auf **Weiter**.

   5. Wählen Sie im Dialogfeld **Profil** beliebige Profile aus, die die Verbindungsumgebung des Computers beschreiben, wenn Sie eine Debugsitzung für die Instanz öffnen möchten, und klicken Sie auf **Weiter**.

   6. Geben Sie im Dialogfeld **Name** einen Namen und eine Beschreibung für die Regel ein und klicken Sie auf **Fertig stellen**.

   7. Klicken Sie in der Liste **Eingehende Regeln** mit der rechten Maustaste auf die Regel, die Sie erstellt haben, und wählen Sie dann im Aktionsbereich **Eigenschaften** aus.

   8. Wählen Sie die Registerkarte **Protokolle und Ports** aus.

   9. Wählen Sie **TCP** im Feld **Protokolltyp:** aus, wählen Sie **RPC-Endpunktzuordnung** im Feld **Lokaler Port:** aus, klicken Sie auf **Anwenden** und dann auf **OK**.

- Wenn die Domänenrichtlinie eine Netzwerkkommunikation über IPSec erfordert, müssen Sie auch eingehende Regeln für das Öffnen von UDP-Port 4500 und den UDP-Port 500 hinzufügen.

## <a name="firewall-rules-on-the-client"></a>Firewallregeln für den Client

Auf dem Computer, der den [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Abfrage-Editor ausführt, ist für das SQL Server-Setup oder [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] -Setup möglicherweise die Windows-Firewall konfiguriert, um Remotedebugging zu ermöglichen.

Wenn Ihnen beim Versuch, eine Remotedebugsitzung zu öffnen, Fehler angezeigt werden, können Sie die Programm- und Portausnahmen manuell mit **Windows-Firewall mit erweiterter Sicherheit** konfigurieren, um Firewallregeln zu konfigurieren:

- Fügen Sie einen Programmeintrag für svchost hinzu:

   1. Klicken Sie im linken Bereich von **Windows-Firewall** mit erweiterter Sicherheit mit der rechten Maustaste auf **Eingehende Regeln**, und wählen Sie dann im Aktionsbereich **Neue Regel** aus.

   2. Wählen Sie im Dialogfeld **Regeltyp** die Option **Programm** aus, und klicken Sie anschließend auf **Weiter**.

   3. Wählen Sie im Dialogfeld **Programm****Dieser Programmpfad:** aus, und geben Sie den vollständigen Pfad zu svchost.exe ein. Standardmäßig ist svchost.exe in %systemroot%\System32\svchost.exe. installiert.

   4. Wählen Sie im Dialogfeld **Aktion** die Option **Verbindung zulassen** aus, und klicken Sie auf **Weiter**.

   5. Wählen Sie im Dialogfeld **Profil** beliebige Profile aus, die die Verbindungsumgebung des Computers beschreiben, wenn Sie eine Debugsitzung für die Instanz öffnen möchten, und klicken Sie auf **Weiter**.

   6. Geben Sie im Dialogfeld **Name** einen Namen und eine Beschreibung für die Regel ein und klicken Sie auf **Fertig stellen**.

   7. Klicken Sie in der Liste **Eingehende Regeln** mit der rechten Maustaste auf die Regel, die Sie erstellt haben, und wählen Sie dann im Aktionsbereich **Eigenschaften** aus.

   8. Wählen Sie die Registerkarte **Protokolle und Ports** aus.

   9. Wählen Sie **TCP** im Feld **Protokolltyp:** aus, wählen Sie **RPC-Endpunktzuordnung** im Feld **Lokaler Port:** aus, klicken Sie auf **Anwenden** und dann auf **OK**.

- Fügen Sie einen Programmeintrag für die Anwendung hinzu, die den [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Abfrage-Editor hostet. Wenn Sie Remotedebugsitzungen von sowohl [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] als auch [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] auf dem gleichen Computer öffnen müssen, müssen Sie eine Programmregel für beide hinzufügen:

   1. Klicken Sie im linken Bereich von **Windows-Firewall** mit erweiterter Sicherheit mit der rechten Maustaste auf **Eingehende Regeln**, und wählen Sie dann im Aktionsbereich **Neue Regel** aus.

   2. Wählen Sie im Dialogfeld **Regeltyp** die Option **Programm** aus, und klicken Sie anschließend auf **Weiter**.

   3. Wählen Sie im Dialogfeld **Programm****Dieser Programmpfad:** aus, und geben Sie einen dieser drei Werte ein.

       - Geben Sie für [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]den vollständigen Pfad zu ssms.exe ein. Standardmäßig wird "ssms.exe" unter "C:\Programme (x86)\Microsoft SQL Server\130\Tools\Binn\Management Studio" installiert.

       - Geben Sie für [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] den vollständigen Pfad zu devenv.exe ein.

            1. Standardmäßig befindet sich der devenv.exe für Visual Studio 2010 unter C:\Programme (x86)\Microsoft Visual Studio 10.0\Common7\IDE.

            2. Standardmäßig befindet sich der devenv.exe für Visual Studio 2012 unter C:\Programme (x86)\Microsoft Visual Studio 11.0\Common7\IDE

            3. Sie können den Pfad zu ssms.exe über die Verknüpfung suchen, mit der Sie [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]starten. Sie können den Pfad zu devenv.exe über die Verknüpfung suchen, mit der Sie [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]starten. Klicken Sie mit der rechten Maustaste auf die Verknüpfung und wählen Sie **Eigenschaften** aus. Die ausführbare Datei und der Pfad sind im Feld **Ziel** aufgeführt.

   4. Wählen Sie im Dialogfeld **Aktion** die Option **Verbindung zulassen** aus, und klicken Sie auf **Weiter**.

   5. Wählen Sie im Dialogfeld **Profil** beliebige Profile aus, die die Verbindungsumgebung des Computers beschreiben, wenn Sie eine Debugsitzung für die Instanz öffnen möchten, und klicken Sie auf **Weiter**.

   6. Geben Sie im Dialogfeld **Name** einen Namen und eine Beschreibung für die Regel ein und klicken Sie auf **Fertig stellen**.

   7. Klicken Sie in der Liste **Eingehende Regeln** mit der rechten Maustaste auf die Regel, die Sie erstellt haben, und wählen Sie dann im Aktionsbereich **Eigenschaften** aus.

   8. Wählen Sie die Registerkarte **Protokolle und Ports** aus.

   9. Wählen Sie **TCP** im Feld **Protokolltyp:** aus, wählen Sie **Dynamische RPC-Ports** im Feld **Lokaler Port:** aus, klicken Sie auf **Anwenden** und dann auf **OK**.

## <a name="requirements-for-starting-the-debugger"></a>Anforderungen zum Starten des Debuggers

Beim Starten des [!INCLUDE[tsql](../../includes/tsql-md.md)] -Debuggers müssen außerdem immer die folgenden Anforderungen erfüllt sein:

- [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] muss unter einem Windows-Konto ausgeführt werden, das Mitglied der festen Serverrolle sysadmin ist.

- Das [!INCLUDE[ssDE](../../includes/ssde-md.md)] Abfrage-Editor-Fenster muss entweder mithilfe einer Windows-Authentifizierung oder mithilfe eines Anmeldenamens für die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Authentifizierung, der Mitglied der festen Serverrolle sysadmin ist, verbunden werden.

- Das [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Abfrage-Editor-Fenster muss mit einer Instanz des [!INCLUDE[ssDE](../../includes/ssde-md.md)] von [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 2 (SP2) oder höher verbunden sein. Sie können den Debugger nicht ausführen, wenn das Abfrage-Editor-Fenster mit einer Instanz verbunden ist, die sich im Einzelbenutzermodus befindet.

- Der Server muss über RPC mit dem Client kommunizieren. Das Konto, mit dem der SQL Server-Dienst ausgeführt wird, muss über Authentifizierungsberechtigungen für den Client verfügen.

## <a name="see-also"></a>Weitere Informationen

- [Transact-SQL-Debugger](./transact-sql-debugger.md)
- [Ausführen des Transact-SQL-Debuggers](./run-the-transact-sql-debugger.md)
- [Schrittweises Durchlaufen von Transact-SQL-Code](./step-through-transact-sql-code.md)
- [Transact-SQL-Debuggerinformationen](./transact-sql-debugger-information.md)
- [Abfrage-Editor der Datenbank-Engine &#40;SQL Server Management Studio&#41;](../f1-help/database-engine-query-editor-sql-server-management-studio.md)