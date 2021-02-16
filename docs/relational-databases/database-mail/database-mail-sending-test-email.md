---
description: Senden einer Test-E-Mail mit Datenbank-E-Mail
title: Senden einer Test-E-Mail mit Datenbank-E-Mail | Microsoft-Dokumentation
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
ms.openlocfilehash: 58b9594c7069347f7e458a5a375bdaf2e2d29103
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272351"
---
# <a name="send-a-test-email-with-database-mail"></a>Senden einer Test-E-Mail mit Datenbank-E-Mail  
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

Verwenden Sie das Dialogfeld „Test-E-Mail senden“, um zu testen, ob E-Mails mit einem bestimmten Profil gesendet werden können.

## <a name="permissions"></a>Berechtigungen

Sie müssen Mitglied der festen Serverrolle „sysadmin“ sein, um das Dialogfeld „Test-E-Mail senden“ zu verwenden. Benutzer, die nicht Mitglied der festen Serverrolle „sysadmin“ sind, können Datenbank-E-Mail mithilfe der Prozedur [sp_send_dbmail](../system-stored-procedures/sp-send-dbmail-transact-sql.md) testen.

## <a name="procedure"></a>Verfahren

1. Stellen Sie mithilfe des Objekt-Explorers in [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md) eine Verbindung mit einer Instanz der SQL Server-Datenbank-Engine her, auf der Datenbank-E-Mail konfiguriert ist, erweitern Sie den Knoten „Verwaltung“, klicken Sie mit der rechten Maustaste auf „Datenbank-E-Mail“, und wählen Sie dann „Test-E-Mail senden“ aus. Wenn keine Datenbank-E-Mail-Profile vorhanden sind, wird der Benutzer mithilfe eines Dialogfelds aufgefordert, ein Profil zu erstellen, und der Assistent zum Konfigurieren von Datenbank-E-Mail wird geöffnet.
1. Wählen Sie im Dialogfeld **Test-E-Mail senden** von <instance name> im Feld „Datenbank-E-Mail-Profil“ das Profil aus, das getestet werden soll.
1. Geben Sie im Feld **An** den E-Mail-Namen des Empfängers der Test-E-Mail ein.
1. Geben Sie im Feld **Betreff** die Betreffzeile für die Test-E-Mail ein. Ändern Sie den Standardbetreff, damit Sie Ihre E-Mail bei der Problembehandlung besser identifizieren können.
1. Geben Sie im Feld **Text** den Text der Test-E-Mail ein. Ändern Sie den Standardbetreff, damit Sie Ihre E-Mail bei der Problembehandlung besser identifizieren können.
1. Wählen Sie **Test-E-Mail senden** aus, um die Test-E-Mail an die Datenbank-E-Mail-Warteschlange zu senden.
1. Beim Senden der Test-E-Mail wird das Dialogfeld „Test-E-Mail von Datenbank-E-Mail“ geöffnet. Notieren Sie die im Feld „Gesendete E-Mail“ angezeigte Zahl. Hierbei handelt es sich um die „mailitem_id“ der Testnachricht. Wählen Sie „OK“ aus.
1. Klicken Sie auf der Symbolleiste auf „Neue Abfrage“, um das Fenster „Abfrage-Editor“ zu öffnen. Führen Sie die folgende T-SQL-Anweisung aus, um den Status der Test-E-Mail-Nachricht zu ermitteln:

    ```sql
    SELECT * FROM msdb.dbo.sysmail_allitems 
    WHERE mailitem_id = <the mailitem_id from the previous step> ;
    ```

    In der „sent_status“-Spalte wird angegeben, ob die Test-E-Mail-Nachricht gesendet wurde.

1. Wenn Fehler aufgetreten sind, führen Sie die folgende Anweisung aus, um die Fehlermeldung anzuzeigen:

    ```sql
    SELECT * FROM msdb.dbo.sysmail_event_log 
    WHERE mailitem_id = <the mailitem_id from the previous step> ;
    ```


##  <a name="see-also"></a><a name="RelatedContent"></a> Siehe auch 
  
-   [Konfigurationsobjekte für Datenbank-E-Mail](../../relational-databases/database-mail/database-mail-configuration-objects.md)
-   [Messagingobjekte für Datenbank-E-Mail](../../relational-databases/database-mail/database-mail-messaging-objects.md)
-   [Externes Datenbank-E-Mail-Programm](../../relational-databases/database-mail/database-mail-external-program.md)
-   [Datenbank-E-Mail-Protokoll und -Überwachung](../../relational-databases/database-mail/database-mail-log-and-audits.md)
-   [Konfigurieren von SQL Server-Agent-Mail zum Verwenden von Datenbank-E-Mails](../../relational-databases/database-mail/configure-sql-server-agent-mail-to-use-database-mail.md)
  
  
