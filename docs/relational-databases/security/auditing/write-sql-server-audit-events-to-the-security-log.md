---
title: Schreiben von SQL-Serverüberwachungsereignissen in das Sicherheitsprotokoll | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie SQL Server-Überwachungsereignisse in das Windows-Sicherheitsprotokoll schreiben. Außerdem lernen Sie die Einschränkungen bei der Verwendung dieses Protokolls kennen.
ms.custom: ''
ms.date: 09/21/2017
ms.prod: sql
ms.prod_service: security
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- logs [SQL Server], Security Log
- server audit [SQL Server]
- audits [SQL Server], writing to Security Log
- security logs [SQL Server]
ms.assetid: 6fabeea3-7a42-4769-a0f3-7e04daada314
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: abdd25c6166b40436584a1260d4b1b09d40900c4
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186061"
---
# <a name="write-sql-server-audit-events-to-the-security-log"></a>Schreiben von SQL-Serverüberwachungsereignissen in das Sicherheitsprotokoll  
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

In einer Umgebung mit hoher Sicherheit ist das Windows-Sicherheitsprotokoll der geeignete Speicherort für Ereignisse, die Objektzugriffe aufzeichnen. Andere Überwachungsspeicherorte werden unterstützt, können aber leichter manipuliert werden.  
  
 Es gibt drei Hauptanforderungen für das Schreiben von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Serverüberwachungen in das Windows-Sicherheitsprotokoll:  
  
-   Die Einstellungen für die Überwachung von Objektzugriffsversuchen müssen so konfiguriert sein, dass die Ereignisse aufgezeichnet werden. Das Tool für Überwachungsrichtlinien (`auditpol.exe`) macht eine Vielzahl von Einstellungen für Unterrichtlinien in der Kategorie **Überwachung von Objektzugriffsversuchen** verfügbar. Um [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] die Überwachung des Objektzugriffs zu ermöglichen, konfigurieren Sie die Einstellung **automatisch generiert** .  
-   Das Konto, unter dem der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Dienst ausgeführt wird, muss über die Berechtigung zum **Generieren von Sicherheitsüberwachungen** verfügen, um in das Windows-Sicherheitsprotokoll schreiben zu können. Standardmäßig verfügen die Konten LOCAL SERVICE und NETWORK SERVICE über diese Berechtigung. Dieser Schritt ist nicht erforderlich, wenn [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] unter einem dieser Konten ausgeführt wird.  
-   Vergeben Sie an das [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Dienstkonto die Vollberechtigung zum Zugriff auf die Registrierungsstruktur `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\EventLog\Security`.  

  > [!IMPORTANT]  
  > [!INCLUDE[ssnoteregistry-md](../../../includes/ssnoteregistry-md.md)]   
  
Die Windows-Überwachungsrichtlinie kann sich auf die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Überwachung auswirken, wenn sie so konfiguriert wurde, dass sie in das Windows-Sicherheitsprotokoll schreibt. In diesem Fall besteht bei einer falschen Konfiguration der Überwachungsrichtlinie die Gefahr, dass Ereignisse verloren gehen. Das Windows-Sicherheitsprotokoll ist standardmäßig so konfiguriert, dass ältere Ereignisse überschrieben werden. Hierdurch werden immer die neuesten Ereignisse beibehalten. Wurde das Windows-Sicherheitsprotokoll jedoch so festgelegt, dass ältere Ereignisse nicht überschrieben werden, löst das System das Windows-Ereignis 1104 aus, sobald das Sicherheitsprotokoll voll ist. In diesem Fall geschieht Folgendes:  
-   Es werden keine weiteren Sicherheitsereignisse aufgezeichnet.  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] kann nicht erkennen, dass das System keine Ereignisse mehr im Sicherheitsprotokoll aufzeichnen kann, sodass Überwachungsereignisse möglicherweise verloren gehen.  
-   Nachdem der Administrator das Sicherheitsprotokoll korrigiert hat, wird die Protokollierung wieder wie gewohnt ausgeführt.  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Einschränkungen  
 Administratoren des [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Computers sollten sich bewusst sein, dass lokale Einstellungen für das Sicherheitsprotokoll durch eine Domänenrichtlinie überschrieben werden können. In diesem Fall überschreibt die Domänenrichtlinie möglicherweise die Einstellung für die Unterkategorie (**auditpol /get /subcategory:"application generated"** ). Dies kann sich auf die Fähigkeit von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] auswirken, Ereignisse zu protokollieren. Dabei kann nicht nachvollzogen werden, dass die Ereignisse, die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] zu überwachen versucht, nicht aufgezeichnet werden.  
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Sie müssen Windows-Administrator sein, um diese Einstellungen konfigurieren zu können.  
  
##  <a name="to-configure-the-audit-object-access-setting-in-windows-using-auditpol"></a><a name="auditpolAccess"></a> So konfigurieren Sie die Einstellung für die Überwachung von Objektzugriffsversuchen in Windows mit "auditpol"  
  
1.  Öffnen Sie eine Eingabeaufforderung mit Administratorberechtigungen.  
  
    1.  Zeigen Sie im Menü **Start** auf **Alle Programme**, zeigen Sie auf **Zubehör**, klicken Sie mit der rechten Maustaste auf **Eingabeaufforderung**, und klicken Sie dann auf **Als Administrator ausführen**.  
  
    2.  Wenn das Dialogfeld **Benutzerkontensteuerung** geöffnet wird, klicken Sie auf **Weiter**.  
  
2.  Führen Sie die folgende Anweisung aus, um die Überwachung von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]zu aktivieren.  
  
    ```  
    auditpol /set /subcategory:"application generated" /success:enable /failure:enable  
    ```  
  
3.  Schließen Sie das Eingabeaufforderungsfenster.  
  
##  <a name="to-grant-the-generate-security-audits-permission-to-an-account-using-secpol"></a><a name="secpolAccess"></a> So erteilen Sie die Berechtigung zum Generieren von Sicherheitsüberwachungen für ein Konto mit "secpol"  
  
1.  Klicken Sie in allen Windows-Betriebssystemen im Menü **Start** auf **Ausführen**.  
  
2.  Geben Sie **secpol.msc** ein, und klicken Sie dann auf **OK**. Wenn das Dialogfeld **Benutzerzugriffssteuerung** angezeigt wird, klicken Sie auf **Weiter**.  
  
3.  Erweitern Sie im Tool "Lokale Sicherheitsrichtlinie" die **Sicherheitseinstellungen**, erweitern Sie **Lokale Richtlinien**, und klicken Sie dann auf **Zuweisen von Benutzerrechten**.  
  
4.  Doppelklicken Sie im Ergebnisbereich auf **Generieren von Sicherheitsüberwachungen**.  
  
5.  Klicken Sie auf der Registerkarte **Lokale Sicherheitseinstellung** auf **Benutzer oder Gruppe hinzufügen**.  
  
6.  Geben Sie im Dialogfeld **Benutzer, Computer oder Gruppen auswählen** entweder den Namen des Benutzerkontos, z. B. **Domäne1\Benutzer1** ein, und klicken Sie dann auf **OK**, oder klicken Sie auf **Erweitert** , und suchen Sie nach dem Konto.  
  
7.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
8.  Schließen Sie das Tool "Sicherheitsrichtlinie".  
  
9. Starten Sie [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] neu, um diese Einstellung zu aktivieren.  
  
##  <a name="to-configure-the-audit-object-access-setting-in-windows-using-secpol"></a><a name="secpolPermission"></a> So konfigurieren Sie die Einstellung für die Überwachung von Objektzugriffsversuchen in Windows mit "secpol"  
  
1.  Wenn das Betriebssystem eine frühere Version als [!INCLUDE[wiprlhext](../../../includes/wiprlhext-md.md)] oder Windows Server 2008 ist, klicken Sie im Menü **Start** auf **Ausführen**.  
  
2.  Geben Sie **secpol.msc** ein, und klicken Sie dann auf **OK**. Wenn das Dialogfeld **Benutzerzugriffssteuerung** angezeigt wird, klicken Sie auf **Weiter**.  
  
3.  Erweitern Sie im Tool "Lokale Sicherheitsrichtlinie" die **Sicherheitseinstellungen**, erweitern Sie **Lokale Richtlinien**, und klicken Sie dann auf **Überwachungsrichtlinie**.  
  
4.  Doppelklicken Sie im Ergebnisbereich auf **Objektzugriffsversuche überwachen**.  
  
5.  Wählen Sie im Bereich **Diese Versuche überwachen** auf der Registerkarte **Lokale Sicherheitseinstellung** sowohl **Erfolg** als auch **Fehler** aus.  
  
6.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
7.  Schließen Sie das Tool "Sicherheitsrichtlinie".  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQL Server Audit &#40;Datenbank-Engine&#41;](../../../relational-databases/security/auditing/sql-server-audit-database-engine.md)  
  
  
