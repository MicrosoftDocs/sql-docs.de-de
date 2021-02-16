---
title: Hinzufügen des sekundären Partners für den Protokollversand
description: In diesem Artikel wird beschrieben, wie Sie mithilfe von SQL Server Management Studio oder Transact-SQL eine sekundäre Datenbank zu einer vorhandenen Protokollversandkonfiguration in SQL Server hinzufügen.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: log-shipping
ms.topic: conceptual
helpviewer_keywords:
- adding secondary databases
- secondary databases [SQL Server], in log shipping
- secondary data files [SQL Server], adding
- log shipping [SQL Server], secondary databases
ms.assetid: b02eba13-f8e6-4684-b7e4-75ea038ea473
author: cawrites
ms.author: chadam
ms.openlocfilehash: b6dd7ac20e514ea9164cc2545323d43b6ba7901f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353785"
---
# <a name="add-a-secondary-database-to-a-log-shipping-configuration-sql-server"></a>Hinzufügen einer sekundären Datenbank zu einer Protokollversandkonfiguration (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In diesem Thema wird erläutert, wie Sie in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[tsql](../../includes/tsql-md.md)]einer vorhandenen Protokollversandkonfiguration eine sekundäre Datenbank hinzufügen.  
  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Die gespeicherten Prozeduren für den Protokollversand erfordern die Mitgliedschaft in der festen Serverrolle **sysadmin** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
  
#### <a name="to-add-a-log-shipping-secondary-database"></a>So fügen Sie eine sekundäre Datenbank für den Protokollversand hinzu  
  
1.  Klicken Sie mit der rechten Maustaste auf die Datenbank, die Sie als primäre Datenbank in der Protokollversandkonfiguration verwenden möchten, und klicken Sie dann auf **Eigenschaften**.  
  
2.  Klicken Sie unter **Seite auswählen** auf **Transaktionsprotokollversand**.  
  
3.  Klicken Sie unter **Sekundäre Serverinstanzen und Datenbanken** auf **Hinzufügen**.  
  
4.  Klicken Sie auf **Verbinden** , und stellen Sie eine Verbindung mit der Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] her, die Sie als sekundären Server verwenden möchten.  
  
5.  Wählen Sie im Feld **Sekundäre Datenbank** eine Datenbank aus der Liste aus, oder geben Sie den Namen der Datenbank ein, die Sie erstellen möchten.  
  
6.  Wählen Sie auf der Registerkarte **Sekundäre Datenbank initialisieren** die Option aus, die Sie zum Initialisieren der sekundären Datenbank verwenden möchten.  
  
7.  Geben Sie auf der Registerkarte **Dateien kopieren** im Ordner **Zielordner für kopierte Dateien** den Pfad des Ordners ein, in den die Transaktionsprotokollsicherungen kopiert werden sollen. Dieser Ordner befindet sich oft auf dem sekundären Server.  
  
8.  Beachten Sie den Zeitplan für das Kopieren im Feld **Zeitplan** unter **Kopierauftrag**. Wenn Sie den Zeitplan für Ihre Installation anpassen möchten, klicken Sie auf **Zeitplan** , und passen Sie den Zeitplan des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agents bei Bedarf an. Dieser Zeitplan sollte ungefähr mit dem Sicherungszeitplan übereinstimmen.  
  
9. Wählen Sie auf der Registerkarte **Wiederherstellen** unter **Datenbankstatus beim Wiederherstellen von Sicherungen** die Option **Kein Wiederherstellungsmodus** oder **Standbymodus** aus.  
  
10. Wenn Sie die Option **Standbymodus** auswählen, legen Sie fest, ob die Benutzer von der sekundären Datenbank getrennt werden sollen, während der Wiederherstellungsvorgang ausgeführt wird.  
  
11. Wenn Sie den Wiederherstellungsprozess auf dem sekundären Server verzögern möchten, wählen Sie unter **Wiederherstellen von Sicherungen verzögern um mindestens** eine Verzögerungszeit aus.  
  
12. Wählen Sie unter **Warnen, wenn keine Wiederherstellung erfolgt in** einen Warnschwellenwert aus.  
  
13. Beachten Sie den Wiederherstellungszeitplan im Feld **Zeitplan** unter **Wiederherstellungsauftrag**. Wenn Sie den Zeitplan für Ihre Installation anpassen möchten, klicken Sie auf **Zeitplan** , und passen Sie den Zeitplan des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agents bei Bedarf an. Dieser Zeitplan sollte ungefähr mit dem Sicherungszeitplan übereinstimmen.  
  
14. Klicken Sie auf **OK**.  
  
15. Klicken Sie im Dialogfeld mit den Datenbankeigenschaften auf **OK** , um den Konfigurationsprozess zu starten.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
  
#### <a name="to-add-a-log-shipping-secondary-database"></a>So fügen Sie eine sekundäre Datenbank für den Protokollversand hinzu  
  
1.  Führen Sie auf dem sekundären Server [sp_add_log_shipping_secondary_primary](../../relational-databases/system-stored-procedures/sp-add-log-shipping-secondary-primary-transact-sql.md) aus, um die Details des primären Servers und der Datenbank zur Verfügung zu stellen. Diese gespeicherte Prozedur gibt die sekundäre ID sowie die IDs des Kopier- und des Wiederherstellungsauftrags zurück.  
  
2.  Führen Sie auf dem sekundären Server [sp_add_jobschedule](../../relational-databases/system-stored-procedures/sp-add-jobschedule-transact-sql.md) aus, um den Zeitplan für den Kopier- und den Wiederherstellungsauftrag festzulegen.  
  
3.  Führen Sie auf dem sekundären Server [sp_add_log_shipping_secondary_database](../../relational-databases/system-stored-procedures/sp-add-log-shipping-secondary-database-transact-sql.md) aus, um eine sekundäre Datenbank hinzuzufügen.  
  
4.  Führen Sie auf dem primären Server [sp_add_log_shipping_primary_secondary](../../relational-databases/system-stored-procedures/sp-add-log-shipping-primary-secondary-transact-sql.md) aus, um die erforderlichen Informationen über die neue sekundäre Datenbank dem primären Server hinzuzufügen.  
  
5.  Aktivieren Sie auf dem sekundären Server den Kopier- und den Wiederherstellungsauftrag. Weitere Informationen finden sie unter [Disable or Enable a Job](../../ssms/agent/disable-or-enable-a-job.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
  
-   [Aktualisieren des Protokollversands auf SQL Server 2016 &#40;Transact-SQL&#41;](../../database-engine/log-shipping/upgrading-log-shipping-to-sql-server-2016-transact-sql.md)  
  
-   [Konfigurieren des Protokollversands &#40;SQL Server&#41;](../../database-engine/log-shipping/configure-log-shipping-sql-server.md)  
  
-   [Entfernen einer sekundären Datenbank aus einer Protokollversandkonfiguration &#40;SQL Server&#41;](../../database-engine/log-shipping/remove-a-secondary-database-from-a-log-shipping-configuration-sql-server.md)  
  
-   [Entfernen des Protokollversands &#40;SQL Server&#41;](../../database-engine/log-shipping/remove-log-shipping-sql-server.md)  
  
-   [Anzeigen des Protokollversandberichts &#40;SQL Server Management Studio&#41;](../../database-engine/log-shipping/view-the-log-shipping-report-sql-server-management-studio.md)  
  
-   [Überwachen des Protokollversands &#40;Transact-SQL&#41;](../../database-engine/log-shipping/monitor-log-shipping-transact-sql.md)  
  
-   [Failover zu einer sekundären Datenbank für den Protokollversand &#40;SQL Server&#41;](../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Informationen zum Protokollversand &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Protokollversandtabellen und gespeicherte Prozeduren](../../database-engine/log-shipping/log-shipping-tables-and-stored-procedures.md)  
  
  
