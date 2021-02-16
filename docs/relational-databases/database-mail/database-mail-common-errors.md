---
description: Häufige Fehler mit Datenbank-E-Mail
title: Häufige Fehler mit Datenbank-E-Mail | Microsoft-Dokumentation
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
ms.openlocfilehash: 0df7b437ff897966502291975a99d56a64003f08
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272361"
---
# <a name="common-errors-with-database-mail"></a>Häufige Fehler mit Datenbank-E-Mail 
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

In diesem Artikel werden einige häufige Fehler, die bei Datenbank-E-Mail auftreten können, und deren Lösungen beschrieben.

## <a name="could-not-find-stored-procedure-sp_send_dbmail"></a>Gespeicherte Prozedur „sp_send_dbmail“ wurde nicht gefunden
Die gespeicherte Prozedur [sp_send_dbmail](../system-stored-procedures/sp-send-dbmail-transact-sql.md) ist in der msdb-Datenbank installiert. Führen Sie entweder **sp_send_dbmail** aus der msdb-Datenbank aus, oder geben Sie einen dreiteiligen Namen für die gespeicherte Prozedur an.

Beispiel:
```sql
EXEC msdb.dbo.sp_send_dbmail ...
```

Oder:

```sql
USE msdb;
GO
EXEC dbo.sp_send_dbmail ...
```

Verwenden Sie den [Assistenten zum Konfigurieren von Datenbank-E-Mail](configure-database-mail.md) zum Aktivieren und Konfigurieren von Datenbank-E-Mail.

## <a name="profile-not-valid"></a>Profil nicht gültig
Für diese Fehlermeldung gibt es zwei mögliche Ursachen: Entweder ist das angegebene Profil nicht vorhanden, oder der Benutzer, der [sp_send_dbmail (Transact-SQL)](../system-stored-procedures/sp-send-dbmail-transact-sql.md) ausführt, hat keine Zugriffsberechtigung für das Profil.

Um Berechtigungen für ein Profil zu überprüfen, führen Sie die gespeicherte Prozedur [sysmail_help_principalprofile_sp (Transact-SQL)](../system-stored-procedures/sysmail-help-principalprofile-sp-transact-sql.md) mit dem Namen des Profils aus. Verwenden Sie die gespeicherte Prozedur [sysmail_add_principalprofile_sp (Transact-SQL)](../system-stored-procedures/sysmail-help-principalprofile-sp-transact-sql.md) oder den [Assistenten zum Konfigurieren von Datenbank-E-Mail](configure-database-mail.md), um einem msdb-Benutzer oder einer msdb-Gruppe die Berechtigung zum Zugriff auf ein Profil zu gewähren.

## <a name="permission-denied-on-sp_send_dbmail"></a>Berechtigung für „sp_send_dbmail“ verweigert

In diesem Thema wird beschrieben, wie eine Fehlermeldung behandelt wird, die besagt, dass der Benutzer, der versucht, Datenbank-E-Mails zu senden, keine Berechtigung zum Ausführen von „sp_send_dbmail“ besitzt.

Die Fehlermeldung lautet wie folgt:

```
EXECUTE permission denied on object 'sp_send_dbmail', 
database 'msdb', schema 'dbo'.
```

Um Datenbank-E-Mails zu senden, müssen Benutzer ein Benutzer in der msdb-Datenbank und Mitglied der Datenbankrolle „DatabaseMailUserRole“ in der msdb-Datenbank sein. Um dieser Rolle msdb-Benutzer oder -Gruppen hinzuzufügen, verwenden Sie SQL Server Management Studio, oder führen Sie die folgende Anweisung für den Benutzer oder die Rolle aus, der bzw. die Datenbank-E-Mails senden muss.

```sql
EXEC msdb.dbo.sp_addrolemember @rolename = 'DatabaseMailUserRole'
    ,@membername = '<user or role name>';
GO
```
Weitere Informationen finden Sie unter [sp_addrolemember](../system-stored-procedures/sp-addrolemember-transact-sql.md) und [sp_droprolemember](../system-stored-procedures/sp-droprolemember-transact-sql.md).

## <a name="database-mail-queued-no-entries-in-sysmail_event_log-or-windows-application-event-log"></a>Datenbank-E-Mail in der Warteschlange, keine Einträge in „sysmail_event_log“ oder im Windows-Anwendungsereignisprotokoll 

Datenbank-E-Mail erfordert Service Broker für das Anordnen von E-Mail-Nachrichten in Warteschlangen. Wenn Datenbank-E-Mail beendet wird oder die Service Broker-Nachrichtenübermittlung in der **msdb**-Datenbank nicht aktiviert ist, reiht Datenbank-E-Mail Nachrichten in die Datenbankwarteschlange ein, kann die Nachrichten jedoch nicht übermitteln. In diesem Fall verbleiben die Service Broker-Nachrichten in der Service Broker-E-Mail-Warteschlange. Service Broker aktiviert nicht das externe Programm. Daher gibt es keine Protokolleinträge in **sysmail_event_log** und keine Aktualisierungen des Elementstatus in **sysmail_mailitems** und den zugehörigen Sichten.

Führen Sie die folgende Anweisung aus, um zu überprüfen, ob Service Broker in der **msdb**-Datenbank aktiviert ist:

```sql
SELECT is_broker_enabled FROM sys.databases WHERE name = 'msdb';
```

Der Wert 0 (Null) zeigt an, dass die Service Broker-Nachrichtenübermittlung in der msdb-Datenbank nicht aktiviert ist. Zur Behebung des Problems aktivieren Sie Service Broker in der Datenbank mit dem folgenden Transact-SQL-Befehl:

```sql
USE master ;
GO

ALTER DATABASE msdb SET ENABLE_BROKER ;
GO
``` 

Datenbank-E-Mail basiert auf verschiedenen intern gespeicherten Prozeduren. Um den Oberflächenbereich zu reduzieren, werden diese gespeicherten Prozeduren bei neuen Installationen von SQL Server deaktiviert. Verwenden Sie zum Aktivieren dieser gespeicherten Prozeduren die Option [Database Mail XPs](../../database-engine/configure-windows/database-mail-xps-server-configuration-option.md) der gespeicherten Systemprozedur **sp_configure** wie im folgenden Beispiel:

```sql
EXEC sp_configure 'show advanced options', 1;  
RECONFIGURE;
EXEC sp_configure 'Database Mail XPs', 1;  
RECONFIGURE  
GO  

Database Mail may be stopped in the **msdb** database. To check status of Database Mail, execute the following statement:

```sql
EXECUTE dbo.sysmail_help_status_sp;
```

Führen Sie den folgenden Befehl in der msdb-Datenbank aus, um Datenbank-E-Mail in einer Mailhost-Datenbank zu starten:

```sql
EXECUTE dbo.sysmail_start_sp;
```

Service Broker untersucht die Dialogfeld-Lebensdauer für Nachrichten, wenn es aktiviert ist. Daher schlagen Nachrichten, die sich länger als die konfigurierte Dialogfeld-Lebensdauer in der Service Broker-Übertragungswarteschlange befinden, sofort fehl. Datenbank-E-Mail aktualisiert den Status fehlgeschlagener Nachrichten in der [sysmail_allitems](../system-catalog-views/sysmail-allitems-transact-sql.md)-Sicht und zugehörigen Sichten. Sie müssen entscheiden, ob die E-Mail-Nachrichten erneut gesendet werden sollen. Weitere Informationen zum Konfigurieren der von Datenbank-E-Mail verwendeten Dialogfeld-Lebensdauer finden Sie unter [sysmail_configure_sp (Transact-SQL)](../system-stored-procedures/sysmail-configure-sp-transact-sql.md).



##  <a name="see-also"></a><a name="RelatedContent"></a> Siehe auch
  
-  [Übersicht über Datenbank-E-Mail](database-mail.md)
-  [Erstellen eines Profils für Datenbank-E-Mail](create-a-database-mail-profile.md)
  
  
