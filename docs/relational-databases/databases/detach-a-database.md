---
title: Trennen einer Datenbank | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie in SQL Server mithilfe von SQL Server Management Studio oder Transact-SQL eine Datenbank trennen. Dateien können wieder angefügt oder an einen anderen Server angefügt werden.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
f1_keywords:
- sql13.swb.detachdatabase.f1
helpviewer_keywords:
- database detaching [SQL Server]
- detaching databases [SQL Server]
ms.assetid: f63d4107-13e4-4bfe-922d-5e4f712e472d
author: stevestein
ms.author: sstein
ms.openlocfilehash: c20fa84b3921cdd76db17d1657ba6c6f8ae0be86
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350910"
---
# <a name="detach-a-database"></a>Trennen einer Datenbank
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In diesem Thema wird beschrieben, wie eine Datenbank in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mit [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[tsql](../../includes/tsql-md.md)]getrennt wird. Die getrennten Dateien bleiben gespeichert und können mithilfe von CREATE DATABASE (mit der FOR ATTACH- oder FOR ATTACH_REBUILD_LOG-Option) erneut angefügt werden. Die Dateien können auf einen anderen Server verschoben und dort angefügt werden.  
  
 **In diesem Thema**  
  
-   **Vorbereitungen:**  
  
     [Einschränkungen](#Restrictions)  
  
     [Security](#Security)  
  
-   **So trennen Sie eine Datenbank mit**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Einschränkungen  
 Eine Liste der Einschränkungen finden Sie unter [Anfügen und Trennen von Datenbanken &#40;SQL Server&#41;](../../relational-databases/databases/database-detach-and-attach-sql-server.md)getrennt wird.  
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Erfordert die Mitgliedschaft in der festen Datenbankrolle "db_owner".  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
  
#### <a name="to-detach-a-database"></a>So trennen Sie eine Datenbank  
  
1.  Stellen Sie im Objekt-Explorer von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] eine Verbindung zu der Instanz von [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] her, und erweitern Sie dann die Instanz.  
  
2.  Erweitern Sie **Datenbanken**, und wählen Sie den Namen der zu trennenden Benutzerdatenbank aus.  
  
3.  Klicken Sie mit der rechten Maustaste auf den Datenbanknamen, zeigen Sie auf **Tasks**, und klicken Sie dann auf **Trennen**. Das Dialogfeld **Datenbank trennen** wird angezeigt.  
  
     **Zu trennende Datenbanken**  
     Führt die zu trennenden Datenbanken auf.  
  
     **Database Name**  
     Zeigt den Namen der zu trennenden Datenbank an.  
  
     **Verbindungen löschen**  
     Trennt die Verbindungen zu der angegebenen Datenbank.  
  
    > [!NOTE]  
    >  Sie können eine Datenbank mit aktiven Verbindungen nicht trennen.  
  
     **Statistikaktualisierung**  
     Standardmäßig werden durch den Trennvorgang beim Trennen der Datenbank die veralteten Optimierungsstatistiken beibehalten. Um die vorhandenen Optimierungsstatistiken zu aktualisieren, aktivieren Sie dieses Kontrollkästchen.  
  
     **Volltextkataloge beibehalten**  
     Standardmäßig werden während des Trennvorgangs alle der Datenbank zugeordneten Volltextkataloge beibehalten. Um sie zu entfernen, deaktivieren Sie das Kontrollkästchen **Volltextkataloge beibehalten** . Diese Option wird nur beim Aktualisieren einer Datenbank von [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]angezeigt.  
  
     **Status**  
     Zeigt einen der folgenden Statuswerte an: **Ready** (Bereit) der **Not ready** (Nicht bereit).  
  
     **Meldung**  
     Unter **Meldung** können folgende Informationen zur Datenbank angezeigt werden:  
  
    -   Wenn eine Datenbank an einer Replikation beteiligt ist, hat der **Status** den Wert **Nicht bereit** , und unter **Meldung** wird **Die Datenbank wurde repliziert** angezeigt.  
  
    -   Wenn eine Datenbank über eine oder mehrere Verbindungen verfügt, weist der **Status** den Wert **Nicht bereit** auf, und in der Spalte **Meldung** wird _<Anzahl_aktiver_Verbindungen>_ **Aktive Verbindung(en)** angezeigt, z. B.: **1 Aktive Verbindung(en)** . Bevor Sie die Datenbank trennen können, müssen Sie durch Auswählen der Option **Verbindungen löschen** alle aktiven Verbindungen trennen.  
  
     Weitere Informationen zu einer Meldung erhalten Sie, indem Sie auf den Linktext klicken, um den Aktivitätsmonitor zu öffnen.  
  
4.  Wenn Sie zum Trennen der Datenbank bereit sind, klicken Sie auf **OK**.  
  
> [!NOTE]  
>  Die jetzt getrennte Datenbank bleibt im **Datenbanken** -Knoten des Objekt-Explorers sichtbar, bis die Ansicht aktualisiert wird. Sie können die Ansicht jederzeit aktualisieren: Klicken Sie in den Objekt-Explorer-Bereich, und wählen Sie in der Menüleiste **Ansicht** und dann **Aktualisieren** aus.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
  
#### <a name="to-detach-a-database"></a>So trennen Sie eine Datenbank  
  
1.  Stellen Sie eine Verbindung mit dem [!INCLUDE[ssDE](../../includes/ssde-md.md)]her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**. Im folgenden Beispiel wird die AdventureWorks2012-Datenbank getrennt, wobei "skipchecks" auf "true" festgelegt ist.  
  
```  
EXEC sp_detach_db 'AdventureWorks2012', 'true';  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Anfügen und Trennen von Datenbanken &#40;SQL Server&#41;](../../relational-databases/databases/database-detach-and-attach-sql-server.md)   
 [sp_detach_db &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-detach-db-transact-sql.md)  
  
  
