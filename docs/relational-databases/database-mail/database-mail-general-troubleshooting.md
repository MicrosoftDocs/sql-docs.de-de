---
description: Allgemeine Schritte zur Problembehandlung für Datenbank-E-Mail
title: Allgemeine Schritte zur Problembehandlung für die Datenbank-E-Mail
ms.date: 06/03/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- architecture [SQL Server], Database Mail
- Database Mail [SQL Server], architecture
- Database Mail [SQL Server], components
author: MashaMSFT
ms.author: mathoma
ms.custom: seo-dt-2019
ms.openlocfilehash: 2e9b7f5b5aec4f1b111983f893783e46ad14e021
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352954"
---
# <a name="general-database-mail-troubleshooting-steps"></a>Allgemeine Schritte zur Problembehandlung für Datenbank-E-Mail 
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

Beim Behandeln von Problemen mit der Datenbank-E-Mail werden die folgenden allgemeinen Bereiche des Systems Datenbank-E-Mail überprüft. Diese Verfahren sind in logischer Reihenfolge aufgeführt, können jedoch in beliebiger Folge ausgewertet werden.

## <a name="permissions"></a>Berechtigungen

Sie müssen Mitglied der festen Serverrolle „sysadmin“ sein, um Probleme mit allen Aspekten von Datenbank-E-Mail zu behandeln. Benutzer, die nicht Mitglied der festen Serverrolle „sysadmin“ sind, können nur Informationen zu den E-Mails abrufen, die sie selbst gesendet haben, nicht jedoch zu E-Mails, die von anderen gesendet wurden.

## <a name="is-database-mail-enabled"></a>Ist Datenbank-E-Mail aktiviert?

1. Stellen Sie in SQL Server Management Studio in einem Abfrage-Editorfenster eine Verbindung mit einer Instanz von SQL Server her, und führen Sie dann den folgenden Code aus:

    ```sql
    sp_configure 'show advanced', 1; 
    GO
    RECONFIGURE;
    GO
    sp_configure;
    GO
    ```

   Vergewissern Sie sich im Ergebnisbereich, dass „run_value“ für [Database Mail XPs](../../database-engine/configure-windows/database-mail-xps-server-configuration-option.md) auf „1“ festgelegt ist.
   Lautet „run_value“ nicht „1“, ist Datenbank-E-Mail nicht aktiviert. Die Datenbank-E-Mail wird nicht automatisch aktiviert, damit möglichst wenige Funktionen eine Angriffsfläche für böswillige Benutzer bieten. Weitere Informationen finden Sie unter [Grundlegendes zur Oberflächenkonfiguration](../security/surface-area-configuration.md).

1. Wenn Sie beschließen, die Datenbank-E-Mail zu aktivieren, führen Sie den folgenden Code aus:

    ```sql
    sp_configure 'Database Mail XPs', 1; 
    GO
    RECONFIGURE;
    GO
    ```

1. Führen Sie den folgenden Code aus, um den Standardzustand der Prozedur „sp_configure“ wiederherzustellen, in dem keine erweiterten Optionen angezeigt werden:

    ```sql 
    sp_configure 'show advanced', 0; 
    GO
    RECONFIGURE;
    GO
    ```

## <a name="are-users-properly-configured-to-send-mail"></a>Sind Benutzer ordnungsgemäß zum Senden von E-Mails konfiguriert?

1. Zum Senden von Datenbank-E-Mail müssen Benutzer ein Mitglied der Datenbankrolle „DatabaseMailUserRole“ in der msdb-Datenbank sein. Mitglieder der festen Serverrolle „sysadmin“ und der Rolle „msdbdb_owner“ sind automatisch Mitglied der Rolle „DatabaseMailUserRole“. Führen Sie die folgende Anweisung aus, um alle anderen Mitglieder der Rolle „DatabaseMailUserRole“ anzuzeigen:

    ```sql
    EXEC msdb.sys.sp_helprolemember 'DatabaseMailUserRole';
    ```

1. Verwenden Sie die folgende Anweisung, um der Rolle „DatabaseMailUserRole“ Benutzer hinzuzufügen:

    ```sql
    USE msdb;
    GO
    
    sp_addrolemember @rolename = 'DatabaseMailUserRole'
    ,@membername = '<database user>';
    ```

1. Um Datenbank-E-Mails zu senden, müssen Benutzer Zugriff auf mindestens ein Datenbank-E-Mail-Profil haben. Führen Sie die folgende Anweisung aus, um die Benutzer (Prinzipale) und die Profile anzuzeigen, auf die diese zugreifen können.

    ```sql
    EXEC msdb.dbo.sysmail_help_principalprofile_sp;
    ```

1. Verwenden Sie den Assistenten zum Konfigurieren der Datenbank-E-Mail, um Profile zu erstellen und Benutzern den Zugriff auf Profile zu gewähren.
 
## <a name="is-database-mail-started"></a>Ist Datenbank-E-Mail gestartet?

1. Das [externe Datenbank-E-Mail-Programm](database-mail-external-program.md) wird aktiviert, wenn zu verarbeitende E-Mail-Nachrichten vorhanden sind. Wenn keine zu sendenden Nachrichten für den angegebenen Timeoutzeitraum vorhanden sind, wird das Programm beendet. Führen Sie die folgende Anweisung aus, um zu überprüfen, ob die Datenbank-E-Mail-Aktivierung gestartet wurde:

    ```sql
    EXEC msdb.dbo.sysmail_help_status_sp;
    ```
1. Wenn die Datenbank-E-Mail-Aktivierung nicht gestartet wurde, führen Sie zum Aktivieren die folgende Anweisung aus:

    ```sql
    EXEC msdb.dbo.sysmail_start_sp;
    ```

1. Wenn das externe Datenbank-E-Mail-Programm gestartet wurde, überprüfen Sie den Status der E-Mail-Warteschlange mit der folgenden Anweisung:

    ```sql
    EXEC msdb.dbo.sysmail_help_queue_sp @queue_type = 'mail';
    ```
  
   Die E-Mail-Warteschlange muss den Status RECEIVES_OCCURRING aufweisen. Der Status der Warteschlange kann sich von einem Moment zum anderen ändern. Wenn die E-Mail-Warteschlange nicht den Status RECEIVES_OCCURRING aufweist, starten Sie die Warteschlange erneut. Stoppen Sie die Warteschlange mit der folgenden Anweisung:
   
```sql
EXEC msdb.dbo.sysmail_stop_sp;
```

Dann starten Sie die Warteschlange mit der folgenden Anweisung:

```sql
EXEC msdb.dbo.sysmail_start_sp;
```

  > [!NOTE]
  >  Legen Sie die Anzahl der E-Mails in der E-Mail-Warteschlange mithilfe der Spalte „length“ im Resultset von „sysmail_help_queue_sp“ fest.

## <a name="do-problems-affect-some-or-all-accounts"></a>Wirken sich Probleme auf einige oder alle Konten aus?

1. Wenn Sie festgestellt haben, dass einige, aber nicht alle Profile E-Mails senden können, liegt möglicherweise ein Problem mit den von den betroffenen Profilen verwendeten Konten für Datenbank-E-Mails vor. Führen Sie die folgende Anweisung aus, um festzustellen, welche Konten erfolgreich E-Mails senden können:

    ```sql
    SELECT sent_account_id, sent_date FROM msdb.dbo.sysmail_sentitems;
    ```

1. Wenn ein nicht funktionierendes Profil keines der aufgeführten Konten verwendet, ist es möglich, dass alle dem Profil zur Verfügung stehenden Konten nicht einwandfrei arbeiten. Zum Überprüfen einzelner Konten erstellen Sie mit dem Assistenten zum Konfigurieren von Datenbank-E-Mail ein neues Profil mit nur einem Konto, und verwenden Sie dann das Dialogfeld „Test-E-Mail senden“, um mithilfe des neuen Kontos eine E-Mail zu senden. 
1. Führen Sie die folgende Anweisung aus, um die Fehlermeldungen der Datenbank-E-Mail anzuzeigen:

    ```sql
    SELECT * FROM msdb.dbo.sysmail_event_log;
    ```

   > [!NOTE]
   > Datenbank-E-Mail betrachtet eine E-Mail als versandt, wenn diese erfolgreich an einen SMTP-Mailserver übertragen wurde. Nachfolgende Fehler, wie eine ungültige E-Mail-Empfängeradresse, können immer noch verhindern, dass die E-Mail zugestellt wird, sind jedoch im Datenbank-E-Mail-Protokoll nicht enthalten.

## <a name="retry-mail-delivery"></a>Wiederholen der E-Mail-Übermittlung

1. Wenn Sie festgestellt haben, dass die Datenbank-E-Mail nicht einwandfrei arbeitet, weil der SMTP-Server nicht zuverlässig erreicht werden kann, können Sie die Rate der erfolgreich übermittelten E-Mails erhöhen. Erhöhen Sie dazu die Anzahl der Versuche der Datenbank-E-Mail beim Senden jeder Nachricht. Starten Sie den Assistenten zum Konfigurieren von Datenbank-E-Mail, und aktivieren Sie die Option „Systemparameter anzeigen oder ändern“. Sie können dem Profil wahlweise auch mehrere Konten zuordnen, sodass die Datenbank-E-Mail im Falle eines Failovers vom ersten Konto das Failoverkonto zum Senden von E-Mails verwendet.
1. Auf der Seite „Systemparameter konfigurieren“ bedeuten die Standardwerte von fünf Mal für „Wiederholungsversuche für das Konto“ und 60 Sekunden für „Wiederholungsverzögerung für das Konto“, dass die Nachrichtenübermittlung fehlschlägt, wenn der SMTP-Server nicht innerhalb von fünf Minuten erreicht werden kann. Erhöhen Sie diese Parameter, um die Zeitdauer bis zum Fehlschlagen der Nachrichtenübermittlung zu verlängern.

    > [!NOTE]
    > Wenn eine große Anzahl Nachrichten gesendet wird, kann durch große Standardwerte die Zuverlässigkeit erhöht werden. Dadurch wird jedoch auch die Verwendung von Ressourcen deutlich erhöht, da die Übermittlung vieler Nachrichten immer wieder versucht wird. Beheben Sie das eigentliche Problem, indem Sie das Problem mit dem Netzwerk oder dem SMTP-Server beheben, das bzw. der verhindert, dass von der Datenbank-E-Mail sofort eine Verbindung mit dem SMTP-Server hergestellt wird.



##  <a name="see-also"></a><a name="RelatedContent"></a> Siehe auch
  
-   [Konfigurationsobjekte für Datenbank-E-Mail](../../relational-databases/database-mail/database-mail-configuration-objects.md)  
  
-   [Messagingobjekte für Datenbank-E-Mail](../../relational-databases/database-mail/database-mail-messaging-objects.md)  
  
-   [Externes Datenbank-E-Mail-Programm](../../relational-databases/database-mail/database-mail-external-program.md)  
  
-   [Datenbank-E-Mail-Protokoll und -Überwachung](../../relational-databases/database-mail/database-mail-log-and-audits.md)  
  
-   [Konfigurieren von SQL Server-Agent-Mail zum Verwenden von Datenbank-E-Mails](../../relational-databases/database-mail/configure-sql-server-agent-mail-to-use-database-mail.md)  
  
  
  
