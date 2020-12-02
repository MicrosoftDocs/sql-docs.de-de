---
description: Erstellen eines Profils für Datenbank-E-Mail
title: Erstellen eines Profils für Datenbank-E-Mail | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/01/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- Database Mail [SQL Server], public profiles
- profiles [SQL Server], Database Mail
- public profiles [Database Mail]
ms.assetid: 58ae749d-6ada-4f9c-bf00-de7c7a992a2d
author: stevestein
ms.author: sstein
ms.openlocfilehash: f3ee012fe4bcd7fa1cd98c51f537035fc6148938
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/25/2020
ms.locfileid: "96125242"
---
# <a name="create-a-database-mail-profile"></a>Erstellen eines Profils für Datenbank-E-Mail
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
   Verwenden Sie den **Assistenten zum Konfigurieren von Datenbank-E-Mail** oder [!INCLUDE[tsql](../../includes/tsql-md.md)], um öffentliche und private Datenbank-E-Mail-Profile zu erstellen. Weitere Informationen über E-Mail-Profile finden Sie unter [Profile für Datenbank-E-Mail](database-mail-configuration-objects.md).
  
-   **Vorbereitungen:** [Voraussetzungen](#Prerequisites), [Sicherheit](#Security)  
  
-   **So erstellen Sie private Profile für Datenbank-E-Mail mit:**  [Assistenten zum Konfigurieren von Datenbank-E-Mail](#SSMSProcedure), [Transact-SQL](#PrivateProfile)  
  
-   **So erstellen Sie öffentliche Profile für Datenbank-E-Mail mit:**  [Assistenten zum Konfigurieren von Datenbank-E-Mail](#SSMSProcedure), [Transact-SQL](#PublicProfile)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Voraussetzungen  
 Erstellen Sie mindestens ein Datenbank-E-Mail-Konto für das Profil. Weitere Informationen zum Erstellen von Datenbank-E-Mail-Konten finden Sie unter [Erstellen eines Kontos für Datenbank-E-Mail](../../relational-databases/database-mail/create-a-database-mail-account.md).  
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
 Mit einem öffentlichen Profil kann jeder Benutzer mit Zugriff auf die **msdb** -Datenbank E-Mail mithilfe dieses Profils senden. Ein privates Profil kann von einem Benutzer oder einer Rolle verwendet werden. Durch Gewähren des Rollenzugriffs auf Profile wird eine leichter zu verwaltende Architektur geschaffen. Um E-Mail zu senden, müssen Sie Mitglied der **DatabaseMailUserRole** in der **msdb** -Datenbank sein und Zugriff auf mindestens ein Datenbankprofil besitzen.  
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Der Benutzer, der die Profilkonten erstellt und gespeicherte Prozeduren ausführt, sollte Mitglied der festen Serverrolle "sysadmin" sein.  
  
##  <a name="using-database-mail-configuration-wizard"></a><a name="SSMSProcedure"></a> Verwenden des Assistenten zum Konfigurieren von Datenbank-E-Mail  
 **So erstellen Sie ein Profil für Datenbank-E-Mail**  
  
-   Stellen Sie im Objekt-Explorer eine Verbindung mit der Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] her, auf der Datenbank-E-Mail konfiguriert werden soll, und erweitern Sie die Serverstruktur.  
  
-   Erweitern Sie den Knoten **Verwaltung** .  
  
-   Doppelklicken Sie auf Datenbank-E-Mail, um den Assistenten zum Konfigurieren von Datenbank-E-Mail zu öffnen.  
  
-   Aktivieren Sie auf der Seite **Konfigurationsaufgabe auswählen** die Option **Konten und Profile für Datenbank-E-Mail verwalten** , und klicken Sie auf **Weiter**.  
  
-   Aktivieren Sie auf der Seite **Profile und Konten verwalten** , aktivieren Sie die Option **Neues Profil erstellen** , und klicken Sie auf **Weiter**.  
  
-   Geben Sie auf der Seite **Neues Profil** den Profilnamen und die Beschreibung ein, und fügen Sie die ins Profil einzuschließenden Konten hinzu. Klicken Sie anschließend auf **Weiter**.  
  
-   Überprüfen Sie auf der Seite **Assistenten abschließen** die auszuführenden Aktionen, und klicken Sie auf **Fertig stellen** , um die Erstellung des neuen Profils abzuschließen.  
  
-   **So konfigurieren Sie ein privates Profil für Datenbank-E-Mail:**  
  
    -   Öffnen Sie den Assistenten zum Konfigurieren von Datenbank-E-Mail.  
  
    -   Aktivieren Sie auf der Seite **Konfigurationsaufgabe auswählen** die Option **Konten und Profile für Datenbank-E-Mail verwalten** , und klicken Sie auf **Weiter**.  
  
    -   Aktivieren Sie auf der Seite **Profile und Konten verwalten** die Option **Profilsicherheit verwalten** , und klicken Sie auf **Weiter**.  
  
    -   Aktivieren Sie auf der Registerkarte **Private Profile** das Kontrollkästchen für das zu konfigurierende Profil, und klicken Sie auf **Weiter**.  
  
    -   Überprüfen Sie auf der Seite **Assistenten abschließen** die auszuführenden Aktionen, und klicken Sie auf **Fertig stellen** , um die Konfiguration des Profils abzuschließen.  
  
-   **So konfigurieren Sie ein öffentliches Profil für Datenbank-E-Mail:**  
  
    -   Öffnen Sie den Assistenten zum Konfigurieren von Datenbank-E-Mail.  
  
    -   Aktivieren Sie auf der Seite **Konfigurationsaufgabe auswählen** die Option **Konten und Profile für Datenbank-E-Mail verwalten** , und klicken Sie auf **Weiter**.  
  
    -   Aktivieren Sie auf der Seite **Profile und Konten verwalten** die Option **Profilsicherheit verwalten** , und klicken Sie auf **Weiter**.  
  
    -   Aktivieren Sie auf der Registerkarte **Öffentliche Profile** das Kontrollkästchen für das zu konfigurierende Profil, und klicken Sie auf **Weiter**.  
  
    -   Überprüfen Sie auf der Seite **Assistenten abschließen** die auszuführenden Aktionen, und klicken Sie auf **Fertig stellen** , um die Konfiguration des Profils abzuschließen.  
  
## <a name="using-transact-sql"></a>Verwenden von Transact-SQL  
  
###  <a name="to-create-a-database-mail-private-profile"></a><a name="PrivateProfile"></a> So erstellen Sie private Profile für Datenbank-E-Mail  
  
-   Stellen Sie eine Verbindung mit der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz her.  
  
-   Führen Sie die gespeicherte Systemprozedur [sysmail_add_profile_sp &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sysmail-add-profile-sp-transact-sql.md) wie folgt durch, um ein neues Profil zu erstellen.  
  
     **EXECUTEmsdb.dbo.sysmail_add_profile_sp**  
  
     *\@profile_name* = '*Profilname*'  
  
     *\@description* = '*Beschreibung*'  
  
     Dabei steht *\@profile_name* für den Namen des Profils und *\@description* für die Beschreibung des Profils. Dieser Parameter ist optional.  
  
-   Führen Sie für jedes Konto die gespeicherte Prozedur [sysmail_add_profileaccount_sp &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sysmail-add-profileaccount-sp-transact-sql.md) wie folgt durch:  
  
     **EXECUTEmsdb.dbo.sysmail_add_profileaccount_sp**  
  
     *\@profile_name* = '*Name des Profils*'  
  
     *\@account_name* = '*Name des Kontos*'  
  
     *\@sequence_number* = '*Die Sequenznummer des Kontos innerhalb des Profils* '  
  
     Dabei steht *\@profile_name* für den Namen des Profils und *\@account_name* für den Namen des Kontos, das zum Profil hinzugefügt werden soll. *\@sequence_number* gibt die Reihenfolge an, in der die Konten im Profil verwendet werden.  
  
-   Erteilen Sie für jede Datenbankrolle oder jeden Benutzer, der E-Mails über dieses Profil sendet, Zugriff auf das Profil. Führen Sie dazu die gespeicherte Prozedur [sysmail_add_profileaccount_sp &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sysmail-add-principalprofile-sp-transact-sql.md) wie folgt aus:  
  
     **EXECUTEmsdb.sysmail_add_principalprofile_sp**  
  
     *\@profile_name* = '*Name des Profils*'  
  
     *\@ principal_name* = '*Name des Datenbankbenutzers oder der Rolle*'  
  
     *\@is_default* = '*Standardprofilstatus* '  
  
     Dabei steht *\@profile_name* für den Namen des Profils und *\@principal_name* für den Datenbankbenutzer oder die Datenbankrolle. *\@is_default* legt fest, ob dieses Profil das Standardprofil für den Datenbankbenutzer oder die Datenbankrolle darstellt.  
  
 Im folgenden Beispiel werden ein Datenbank-E-Mail-Konto und ein privates Konto für Datenbank-E-Mail erstellt. Anschließend wird das Konto zum Profil hinzugefügt und der Datenbankrolle **DBMailUsers** in der **msdb** -Datenbank Zugriff auf das Profil erteilt.  
  
```  
-- Create a Database Mail account  
EXECUTE msdb.dbo.sysmail_add_account_sp  
    @account_name = 'AdventureWorks Administrator',  
    @description = 'Mail account for administrative e-mail.',  
    @email_address = 'dba@Adventure-Works.com',  
    @replyto_address = 'danw@Adventure-Works.com',  
    @display_name = 'AdventureWorks Automated Mailer',  
    @mailserver_name = 'smtp.Adventure-Works.com' ;  
  
-- Create a Database Mail profile  
EXECUTE msdb.dbo.sysmail_add_profile_sp  
    @profile_name = 'AdventureWorks Administrator Profile',  
    @description = 'Profile used for administrative mail.' ;  
  
-- Add the account to the profile  
EXECUTE msdb.dbo.sysmail_add_profileaccount_sp  
    @profile_name = 'AdventureWorks Administrator Profile',  
    @account_name = 'AdventureWorks Administrator',  
    @sequence_number =1 ;  
  
-- Grant access to the profile to the DBMailUsers role  
EXECUTE msdb.dbo.sysmail_add_principalprofile_sp  
    @profile_name = 'AdventureWorks Administrator Profile',  
    @principal_name = 'ApplicationUser',  
    @is_default = 1 ;  
```  
  
###  <a name="to-create-a-database-mail-public-profile"></a><a name="PublicProfile"></a> So erstellen Sie öffentliches Profil für Datenbank-E-Mail  
  
-   Stellen Sie eine Verbindung mit der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz her.  
  
-   Führen Sie die gespeicherte Systemprozedur [sysmail_add_profile_sp &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sysmail-add-profile-sp-transact-sql.md) wie folgt durch, um ein neues Profil zu erstellen.  
  
     **EXECUTEmsdb.dbo.sysmail_add_profile_sp**  
  
     *\@profile_name* = '*Profilname*'  
  
     *\@description* = '*Beschreibung*'  
  
     Dabei steht *\@profile_name* für den Namen des Profils und *\@description* für die Beschreibung des Profils. Dieser Parameter ist optional.  
  
-   Führen Sie für jedes Konto die gespeicherte Prozedur [sysmail_add_profileaccount_sp &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sysmail-add-profileaccount-sp-transact-sql.md) wie folgt durch:  
  
     **EXECUTEmsdb.dbo.sysmail_add_profileaccount_sp**  
  
     *\@profile_name* = '*Name des Profils*'  
  
     *\@account_name* = '*Name des Kontos*'  
  
     *\@sequence_number* = '*Die Sequenznummer des Kontos innerhalb des Profils* '  
  
     Dabei steht *\@profile_name* für den Namen des Profils und *\@account_name* für den Namen des Kontos, das zum Profil hinzugefügt werden soll. *\@sequence_number* gibt die Reihenfolge an, in der die Konten im Profil verwendet werden.  
  
-   Führen Sie zum Erteilen öffentlichen Zugriffs die gespeicherte Prozedur [sysmail_add_profileaccount_sp &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sysmail-add-principalprofile-sp-transact-sql.md) wie folgt aus:  
  
     **EXECUTEmsdb.sysmail_add_principalprofile_sp**  
  
     *\@profile_name* = '*Name des Profils*'  
  
     *\@ principal_name* = '**öffentlich** oder **0**'  
  
     *\@is_default* = '*Standardprofilstatus* '  
  
     Dabei steht *\@profile_name* für den Namen des Profils, und *\@principal_name* weist darauf hin, dass es sich um ein öffentliches Profil handelt. *\@is_default* legt fest, ob dieses Profil das Standardprofil für den Datenbankbenutzer oder die Datenbankrolle darstellt.  
  
 Im folgenden Beispiel werden ein Datenbank-E-Mail-Konto und ein privates Profil für Datenbank-E-Mail erstellt. Anschließend wird das Konto zum Profil hinzugefügt und öffentlicher Zugriff auf das Profil erteilt.  
  
```  
-- Create a Database Mail account  
  
EXECUTE msdb.dbo.sysmail_add_account_sp  
    @account_name = 'AdventureWorks Public Account',  
    @description = 'Mail account for use by all database users.',  
    @email_address = 'db_users@Adventure-Works.com',  
    @replyto_address = 'danw@Adventure-Works.com',  
    @display_name = 'AdventureWorks Automated Mailer',  
    @mailserver_name = 'smtp.Adventure-Works.com' ;  
  
-- Create a Database Mail profile  
  
EXECUTE msdb.dbo.sysmail_add_profile_sp  
    @profile_name = 'AdventureWorks Public Profile',  
    @description = 'Profile used for administrative mail.' ;  
  
-- Add the account to the profile  
  
EXECUTE msdb.dbo.sysmail_add_profileaccount_sp  
    @profile_name = 'AdventureWorks Public Profile',  
    @account_name = 'AdventureWorks Public Account',  
    @sequence_number =1 ;  
  
-- Grant access to the profile to all users in the msdb database  
  
EXECUTE msdb.dbo.sysmail_add_principalprofile_sp  
    @profile_name = 'AdventureWorks Public Profile',  
    @principal_name = 'public',  
    @is_default = 1 ;  
```  
  
  
