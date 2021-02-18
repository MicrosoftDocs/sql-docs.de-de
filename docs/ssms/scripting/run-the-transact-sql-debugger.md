---
title: Ausführen des Transact-SQL-Debuggers
description: Hier erfahren Sie, wie Sie den Transact-SQL-Debugger anpassen und verwenden, um Ihren Transact-SQL-Code zu debuggen. Sie können den Debugger auf einer Instanz der Datenbank-Engine ausführen, die sich auf einem anderen Computer befindet.
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Transact-SQL debugger, sysadmin requirement
- Transact-SQL debugger, supported versions
- Query Editor [Database Engine], right-click menu
- debugging [SQL Server], T-SQL debugger
- Transact-SQL debugger, Query Editor shortcut menu
- Transact-SQL debugger, stopping
- Transact-SQL debugger, Debug menu
- debugging [SQL Server]
- Transact-SQL debugger, Debug toolbar
- Transact-SQL debugger, keyboard shortcuts
- Transact-SQL debugger, starting
ms.assetid: 386f6d09-dbec-4dc7-9e8a-cd9a4a50168c
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: cb075a71ec6a4004de4691a53b8b487eadbd65c4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354535"
---
# <a name="run-the-transact-sql-debugger"></a>Ausführen des Transact-SQL-Debuggers

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Sie können den [!INCLUDE[tsql](../../includes/tsql-md.md)] -Debugger starten, nachdem Sie ein [!INCLUDE[ssDE](../../includes/ssde-md.md)] Abfrage-Editor-Fenster geöffnet haben. Anschließend können Sie den [!INCLUDE[tsql](../../includes/tsql-md.md)] -Code im Debugmodus ausführen, bis Sie den Debugger beenden. Sie können Optionen festlegen, um die Ausführung des Debuggers anzupassen.

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]

## <a name="starting-and-stopping-the-debugger"></a>Starten und Beenden des Debuggers

Um den [!INCLUDE[tsql](../../includes/tsql-md.md)] -Debugger zu starten, müssen folgende Anforderungen erfüllt sein:

- Wenn der [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Abfrage-Editor mit einer Instanz von [!INCLUDE[ssDE](../../includes/ssde-md.md)] auf einem anderen Computer verbunden ist, müssen Sie den Debugger für das Remotedebuggen konfiguriert haben. Weitere Informationen finden Sie unter [Konfigurieren von Firewallregeln vor dem Ausführen des TSQL-Debuggers](./configure-firewall-rules-before-running-the-tsql-debugger.md).
  
- [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] muss unter einem Windows-Konto ausgeführt werden, das Mitglied der festen Serverrolle sysadmin ist.

- Das [!INCLUDE[ssDE](../../includes/ssde-md.md)] Abfrage-Editor-Fenster muss entweder mithilfe einer Windows-Authentifizierung oder mithilfe eines Anmeldenamens für die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Authentifizierung, der Mitglied der festen Serverrolle sysadmin ist, verbunden werden.
  
- Das [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Abfrage-Editor-Fenster muss mit einer Instanz des [!INCLUDE[ssDE](../../includes/ssde-md.md)] von [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 2 (SP2) oder höher verbunden sein. Sie können den Debugger nicht ausführen, wenn das Abfrage-Editor-Fenster mit einer Instanz verbunden ist, die sich im Einzelbenutzermodus befindet.  
  
 Wir empfehlen, den [!INCLUDE[tsql](../../includes/tsql-md.md)] -Code aus den folgenden Gründen auf einem Testserver und nicht auf einem Produktionsserver zu debuggen:
  
- Debuggen ist ein Vorgang mit hohen Berechtigungen. Daher dürfen nur Mitglieder der festen Serverrolle sysadmin in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]debuggen.
  
- Debuggingsitzungen werden oft über längere Zeiträume ausgeführt, während Sie die Vorgänge verschiedener [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisungen untersuchen. Sperren, wie z.B. Updatesperren, die von der Sitzung eingerichtet werden, können längere Zeit beibehalten werden, bis die Sitzung beendet wird oder ein Commit oder ein Rollback für die Transaktion ausgeführt wurde.  
  
 Das Starten des [!INCLUDE[tsql](../../includes/tsql-md.md)] -Debuggers versetzt das Abfrage-Editor-Fenster in den Debugmodus. Wenn das Abfrage-Editor-Fenster in den Debugmodus wechselt, hält der Debugger bei der ersten Codezeile an. Sie können den Code schrittweise durchlaufen, die Ausführung bei bestimmten [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisungen unterbrechen und mithilfe der Debuggerfenster den aktuellen Ausführungsstatus anzeigen. Um den Debugger zu starten, klicken Sie entweder auf der Symbolleiste **Abfrage** auf die Schaltfläche **Debuggen** oder im Menü **Debuggen** auf **Debuggen starten** .  
  
 Das Abfrage-Editor-Fenster bleibt im Debugmodus, bis entweder die letzte Anweisung im Abfrage-Editor-Fenster abgeschlossen ist oder Sie den Debugmodus beenden. Sie können den Debugmodus und die Anweisungsausführung mithilfe einer der folgenden Methoden beenden:  
  
- Klicken Sie im Menü **Debuggen** auf **Debuggen beenden**.  
  
- Klicken Sie auf der Symbolleiste **Debuggen** auf die Schaltfläche **Debuggen beenden** .  
  
- Klicken Sie im Menü **Abfrage** auf **Ausführung der Abfrage abbrechen**.  
  
- Klicken Sie auf der Symbolleiste **Abfrage** auf die Schaltfläche **Ausführung der Abfrage abbrechen** .  
  
 Außerdem können Sie den Debugmodus beenden und die Ausführung der verbleibenden [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisungen zulassen, indem Sie im Menü **Debuggen** auf **Alle trennen** klicken.  
  
## <a name="controlling-the-debugger"></a>Steuern des Debuggers

 Sie können die Arbeitsweise des [!INCLUDE[tsql](../../includes/tsql-md.md)] -Debuggers mit den folgenden Menübefehlen, Symbolleisten und Verknüpfungen steuern:  
  
- Mit dem Menü **Debuggen** und der Symbolleiste **Debuggen** . Sowohl das Menü **Debuggen** als auch die Symbolleiste **Debuggen** sind inaktiv, bis der Fokus auf ein offenes Abfrage-Editor-Fenster verschoben wird. Sie bleiben aktiv, bis das aktuelle Projekt geschlossen wird.  
  
- Mit den Tastenkombinationen des Debuggers.  
  
- Mit dem Kontextmenü des Abfrage-Editors. Das Kontextmenü wird angezeigt, wenn Sie in einem Abfrage-Editor-Fenster mit der rechten Maustaste auf eine Zeile klicken. Wenn sich das Abfrage-Editor-Fenster im Debugmodus befindet, zeigt das Kontextmenü Debuggerbefehle an, die für die ausgewählte Zeile oder Zeichenfolge gelten.  
  
- Mit Menüelementen und Kontextbefehlen in den Fenstern, die vom Debugger geöffnet werden, wie z.B. das Fenster **Überwachung** oder **Breakpoints** .  
  
 Die folgende Tabelle enthält die Menübefehle, Symbolleistenschaltflächen und Tastenkombinationen für den Debugger.  
  
|Menübefehle für das Debuggen|Editor-Verknüpfungsbefehl|Symbolleistenschaltfläche|Tastenkombinationen|Aktion|  
|------------------------|-----------------------------|--------------------|-----------------------|------------|  
|**Fenster/Breakpoints**|Nicht verfügbar|**Breakpoints**|STRG+ALT+B|Zeigt das Fenster **Breakpoints** an, in dem Sie Breakpoints anzeigen und verwalten können.|  
|**Fenster/Überwachung/Überwachen 1**|Nicht verfügbar|**Breakpoints/Überwachung/Überwachen 1**|STRG+ALT+W, 1|Zeigt das Fenster **Überwachen 1** an.|  
|**Fenster/Überwachung/Überwachen 2**|Nicht verfügbar|**Breakpoints/Überwachung/Überwachen 2**|STRG+ALT+W, 2|Zeigt das Fenster **Überwachen 2** an.|  
|**Fenster/Überwachung/Überwachen 3**|Nicht verfügbar|**Breakpoints/Überwachung/Überwachen 3**|STRG+ALT+W, 3|Zeigt das Fenster **Überwachen 3** an.|  
|**Fenster/Überwachung/Überwachen 4**|Nicht verfügbar|**Breakpoints/Überwachung/Überwachen 4**|STRG+ALT+W, 4|Zeigt das Fenster **Überwachen 4** an.|  
|**Fenster/Lokal**|Nicht verfügbar|**Breakpoints/Lokal**|STRG+ALT+V, L|Anzeigen des Fensters **Lokal**|  
|**Fenster/Aufrufliste**|Nicht verfügbar|**Breakpoints/Aufrufliste**|STRG+ALT+C|Anzeigen des Fensters **Aufrufliste**|  
|**Fenster/Threads**|Nicht verfügbar|**Breakpoints/Threads**|STRG+ALT+H|Anzeigen des Fensters **Threads**|  
|**Fortsetzen**|Nicht verfügbar|**Fortsetzen**|ALT+F5|Den Vorgang bis zum nächsten Breakpoint ausführen **Weiter** ist erst dann aktiv, wenn der Fokus auf ein Abfrage-Editor-Fenster verschoben wird, das sich im Debugmodus befindet.|  
|**Debuggen**|Nicht verfügbar|**Debuggen**|ALT+F5|Versetzt ein Abfrage-Editor-Fenster in den Debugmodus und führt den Vorgang bis zum ersten Breakpoint aus. Wenn sich Ihr Fokus auf ein Abfrage-Editor-Fenster richtet, das sich im Debugmodus befindet, wird **Debuggen starten** durch **Weiter** ersetzt.|  
|**Alle unterbrechen**|Nicht verfügbar|**Alle unterbrechen**|STRG+ALT+UNTBR|Diese Funktion wird vom [!INCLUDE[tsql](../../includes/tsql-md.md)] -Debugger nicht verwendet.|  
|**Debuggen beenden**|Nicht verfügbar|**Debuggen beenden**|UMSCHALT+F5|Deaktiviert den Debugmodus für ein Abfrage-Editor-Fenster und stellt den normalen Modus wieder her.|  
|**Debuggen**|Nicht verfügbar|Nicht verfügbar|Nicht verfügbar|Beendet den Debugmodus, führt jedoch die übrigen Anweisungen im Abfrage-Editor-Fenster aus.|  
|**Einzelschritt**|Nicht verfügbar|**Einzelschritt**|F11|Führt die nächste Anweisung aus und öffnet außerdem ein neues Abfrage-Editor-Fenster im Debugmodus, wenn die nächste Anweisung eine gespeicherte Prozedur, einen Trigger oder eine Funktion ausführt.|  
|**Überspringen**|Nicht verfügbar|**Überspringen**|F10|Wie **Einzelschritt**, mit dem Unterschied, dass keine Funktionen, gespeicherten Prozeduren oder Trigger debuggt werden.|  
|**Rücksprung**|Nicht verfügbar|**Rücksprung**|UMSCHALT+F11|Führt den restlichen Code in einem Trigger, einer Funktion oder einer gespeicherten Prozedur aus, ohne bei Breakpoints anzuhalten. Der normale Debugmodus wird fortgesetzt, wenn die Steuerung an den Code, der das Modul aufgerufen hat, zurückgegeben wird.|  
|Nicht verfügbar|**Ausführen bis** Cursorposition|Nicht verfügbar|STRG+F10|Führt den gesamten Code von der letzten Halteposition bis zur aktuellen Cursorposition aus, ohne bei Breakpoints anzuhalten.|  
|**Schnellüberwachung**|**Schnellüberwachung**|Nicht verfügbar|STRG+ALT+Q|Zeigt das Fenster **Schnellüberwachung** an.|  
|**Haltepunkt umschalten**|**Breakpoint/Breakpoint einfügen**|Nicht verfügbar|F9|Positioniert einen Breakpoint bei der aktuellen oder ausgewählten [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisung.|  
|Nicht verfügbar|**Breakpoint/Breakpoint löschen**|Nicht verfügbar|Nicht verfügbar|Löscht den Breakpoint aus der ausgewählten Zeile.|  
|Nicht verfügbar|**Breakpoint/Breakpoint deaktivieren**|Nicht verfügbar|Nicht verfügbar|Deaktiviert den Breakpoint in der ausgewählten Zeile. Der Breakpoint bleibt in der Codezeile, beendet aber keine Ausführung, bis er erneut aktiviert wird.|  
|Nicht verfügbar|**Breakpoint/Breakpoint aktivieren**|Nicht verfügbar|Nicht verfügbar|Aktiviert den Breakpoint in der ausgewählten Zeile.|  
|**Alle Breakpoints löschen**|Nicht verfügbar|Nicht verfügbar|STRG+UMSCHALT+F9|Löscht alle Breakpoints.|  
|**Alle Breakpoints deaktivieren**|Nicht verfügbar|Nicht verfügbar|Nicht verfügbar|Deaktiviert alle Breakpoints.|  
|Nicht verfügbar|**Überwachung hinzufügen**|Nicht verfügbar|Nicht verfügbar|Fügt dem **Überwachungsfenster** den ausgewählten Ausdruck hinzu.|  
  
## <a name="see-also"></a>Weitere Informationen

- [Transact-SQL-Debugger](./transact-sql-debugger.md)
- [Schrittweises Durchlaufen von Transact-SQL-Code](./step-through-transact-sql-code.md)
- [Transact-SQL-Debuggerinformationen](./transact-sql-debugger-information.md)
- [Abfrage-Editor der Datenbank-Engine &#40;SQL Server Management Studio&#41;](../f1-help/database-engine-query-editor-sql-server-management-studio.md)
- [Live-Abfragestatistik](../../relational-databases/performance/live-query-statistics.md)