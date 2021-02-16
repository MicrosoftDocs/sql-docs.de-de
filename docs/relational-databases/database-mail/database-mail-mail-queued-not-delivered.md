---
description: 'Datenbank-E-Mail: E-Mail in Warteschlange, nicht versandt'
title: 'Datenbank-E-Mail: E-Mail in Warteschlange, nicht versandt | Microsoft-Dokumentation'
ms.custom: ''
ms.date: 04/22/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- architecture [SQL Server], Database Mail
- Database Mail [SQL Server], architecture
- Database Mail [SQL Server], components
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: ebf8096c4b679cc6d5ac01b164bb9ae4bb014e6e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352945"
---
# <a name="database-mail-mail-queued-not-delivered"></a>Datenbank-E-Mail: E-Mail in Warteschlange, nicht versandt 
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

In diesem Thema wird beschrieben, wie die Problembehandlung ausgeführt wird, wenn E-Mail-Nachrichten erfolgreich in der Warteschlange angeordnet werden, aber nicht übermittelt werden.

## <a name="diagnose-the-problem"></a>Diagnostizieren des Problems 

Das externe Datenbank-E-Mail-Programm protokolliert E-Mail-Aktivitäten in der **msdb**-Datenbank.

Vergewissern Sie sich zuerst, dass Datenbank-E-Mail aktiviert ist. Dazu verwenden Sie die Option [Database Mail XPs](../../database-engine/configure-windows/database-mail-xps-server-configuration-option.md) der gespeicherten Systemprozedur **sp_configure** wie im folgenden Beispiel:

```sql 
EXEC sp_configure 'show advanced', 1;  
RECONFIGURE; 
EXEC sp_configure; 
GO
```

Führen Sie anschließend in der **msdb**-Datenbank die folgende Anweisung aus, um den Status der E-Mail-Warteschlange zu prüfen:

```sql
sysmail_help_queue_sp @queue_type = 'Mail' ;
```

Eine ausführliche Erläuterung der Spalten finden Sie unter [sysmail_help_queue_sp (Transact-SQL)](../system-stored-procedures/sysmail-help-queue-sp-transact-sql.md#result-set).

Überprüfen Sie, ob in der **sysmail_event_log**-Sicht Aktivitäten vorhanden sind. Die Sicht sollte einen Eintrag enthalten, der besagt, dass das externe Programm Datenbank-E-Mail gestartet wurde. Falls in der **sysmail_event_log**-Sicht kein Eintrag vorhanden ist, lesen Sie den Abschnitt zum Symptom [Nachrichten in Warteschlange, keine Einträge](database-mail-common-errors.md#database-mail-queued-no-entries-in-sysmail_event_log-or-windows-application-event-log) in **sysmail_event_log**. Falls in der **sysmail_event_log**-Sicht Fehler vorhanden sind, führen Sie eine Problembehandlung des betreffenden Fehlers aus.

Falls in der **sysmail_event_log**-Sicht Einträge vorhanden sind, überprüfen Sie den Status der Meldungen in der **sysmail_allitems**-Sicht.

## <a name="message-status-unsent"></a>Meldungsstatus „unsent“ 

Der Status **unsent** zeigt an, dass das [externe Datenbank-E-Mail-Programm](database-mail-external-program.md) die E-Mail-Nachricht noch nicht verarbeitet hat. Das externe Programm Datenbank-E-Mail ist möglicherweise beim Verarbeiten von Nachrichten in den Rückstand geraten. Die Rate, mit der das externe Programm Nachrichten verarbeitet, hängt von Netzwerkbedingungen, dem Wiederholungstimeout, dem Nachrichtenumfang und der Kapazität des SMTP-Servers ab. Falls das Problem weiterhin besteht, sollten Sie eventuell mehrere Profile verwenden, um die Nachrichten auf mehrere SMTP-Server zu verteilen.

Überprüfen Sie das letzte Änderungsdatum auf erfolgreich übermittelte Nachrichten. Falls die letzte erfolgreiche Übermittlung vor einiger Zeit erfolgte, überprüfen Sie die „sysmail_event_log“-Sicht, um sicherzustellen, dass das externe Programm von Service Broker erfolgreich gestartet wurde. Falls das externe Programm beim letzten Versuch nicht gestartet werden konnte, stellen Sie sicher, dass sich das externe Datenbank-E-Mail-Programm im richtigen Verzeichnis befindet und dass das Dienstkonto für SQL Server die Berechtigung zum Ausführen der ausführbaren Datei besitzt.

   > [!NOTE]
   > Sie können alte, ungesendete Nachrichten löschen, indem Sie warten, bis die unzustellbaren Nachrichten als älteste Nachrichten in der Warteschlange enthalten sind. Anschließend können diese Nachrichten mithilfe von [sysmail_delete_mailitems_sp](../system-stored-procedures/sysmail-delete-mailitems-sp-transact-sql.md) gelöscht werden.

## <a name="message-status-retrying"></a>Meldungsstatus „retrying“

Der Status retrying zeigt an, dass von Datenbank-E-Mail versucht wurde, die Nachricht an den SMTP-Server zu übermitteln, dass dies aber nicht möglich war. In der Regel liegt dies an einer Unterbrechung im Netzwerk, einem Fehler auf dem SMTP-Server oder einem falsch konfigurierten Datenbank-E-Mail-Konto. Die Nachricht sollte schließlich erfolgreich versendet werden oder fehlschlagen und eine Meldung im Ereignisprotokoll verursachen.

## <a name="message-status-sent"></a>Meldungsstatus „sent“

Der Status **sent** zeigt an, dass das externe Datenbank-E-Mail-Programm die E-Mail-Nachricht erfolgreich an den SMTP-Server übermittelt hat. Falls die Nachricht nicht am Ziel angekommen ist, wurde die Nachricht von Datenbank-E-Mail vom SMTP-Server angenommen, konnte aber nicht an den endgültigen Empfänger übermittelt werden. Überprüfen Sie die Protokolle des SMTP-Servers, oder wenden Sie sich an den Administrator des SMTP-Servers. Sie können den SMTP-Server auch testen, indem Sie einen anderen Client (z. B. Outlook Express) verwenden.

## <a name="message-status-failed"></a>Meldungsstatus „failed“

Der Status „failed“ zeigt an, dass das externe Datenbank-E-Mail-Programm die Nachricht nicht an den SMTP-Server übermitteln konnte. In diesem Fall enthält die **sysmail_event_log**-Sicht ausführliche Informationen vom externen Programm. Eine Beispielabfrage, die **sysmail_faileditems** und **sysmail_event_log** verknüpft, um ausführliche Fehlermeldungen abzurufen, finden Sie unter [Überprüfen des Status von mit Datenbank-E-Mail gesendeten E-Mail-Nachrichten](check-the-status-of-e-mail-messages-sent-with-database-mail.md). Die häufigsten Ursachen für einen Fehler sind eine falsche Zieladresse oder Netzwerkprobleme, die verhindern, dass das externe Programm eines der Failoverkonten erreicht. Probleme auf dem SMTP-Server können ebenfalls dazu führen, dass der Server E-Mail ablehnt. Ändern Sie mithilfe des Assistenten zum Konfigurieren von Datenbank-E-Mail den **Protokolliergrad** in **Ausführlich**, und senden Sie eine Test-E-Mail, um den Fehlerpunkt zu lokalisieren.



##  <a name="see-also"></a><a name="RelatedContent"></a> Siehe auch
  
-  [Übersicht über Datenbank-E-Mail](database-mail.md)

  
  
