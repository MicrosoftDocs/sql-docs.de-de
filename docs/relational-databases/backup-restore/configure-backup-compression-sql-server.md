---
title: Konfigurieren von Sicherungskomprimierung (SQL Server) | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie die das Standardverhalten auf Serverebene beim Erstellen einer einzelnen Sicherung oder beim Planen einer Reihe von Routinesicherungen in SQL Server überschreiben.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
ms.assetid: 430905eb-d218-458c-bd48-aeee6fbb7446
author: cawrites
ms.author: chadam
ms.openlocfilehash: 31376d9ff735c30a0374acade23e3a8a89804baa
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/23/2020
ms.locfileid: "96127028"
---
# <a name="configure-backup-compression-sql-server"></a>Konfigurieren von Sicherungskomprimierung (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Die Sicherungskomprimierung wird bei der Installation standardmäßig deaktiviert. Das Standardverhalten für die Sicherungskomprimierung wird auf Serverebene durch die Konfigurationsoption **backup compression default** definiert. Sie können jedoch die Standardeinstellung auf Serverebene überschreiben, wenn Sie eine einzelne Sicherung erstellen oder eine Reihe von Routinesicherungen einplanen. Zum Ändern der Standardeinstellung auf Serverlevel gehen Sie unter [Anzeigen oder Konfigurieren der Serverkonfigurationsoption Standardeinstellung für die Sicherungskomprimierung](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md).  
  
## <a name="override-the-backup-compression-default"></a>Überschreiben der Standardeinstellung für die Sicherungskomprimierung  
 Sie können das Verhalten der Sicherungskomprimierung für eine einzelne Sicherung, einen Sicherungsauftrag oder eine Protokollversandkonfiguration ändern.  
  
-   **[!INCLUDE[tsql](../../includes/tsql-md.md)]**  
  
     Verwenden Sie WITH NO_COMPRESSION oder WITH COMPRESSION in der [BACKUP](../../t-sql/statements/backup-transact-sql.md)-Anweisung, um die Standardeinstellung für die Serversicherungskomprimierung zu überschreiben.  
  
     Bei einer Protokollversandkonfiguration können Sie das Verhalten der Sicherungskomprimierung für Protokollsicherungen mithilfe von [sp_add_log_shipping_primary_database](../../relational-databases/system-stored-procedures/sp-add-log-shipping-primary-database-transact-sql.md)[sp_change_log_shipping_primary_database &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-change-log-shipping-primary-database-transact-sql.md) steuern.  
  
-   **[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]**  
  
     Informationen zum Anzeigen oder Konfigurieren der Standardoption für die Sicherungskomprimierung für eine Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], gehen Sie unter [Anzeigen oder Konfigurieren der Serverkonfigurationsoption Standardeinstellung für die Sicherungskomprimierung](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md).  
  
     Sie können die Standardeinstellung für die Serversicherungskomprimierung beim Erstellen einer Sicherung überschreiben, indem Sie die Optionen **Sicherung komprimieren** oder **Sicherung nicht komprimieren** in einem der folgenden Dialogfelder angeben:  
  
    -   [Datenbank sichern (Seite Optionen)](../../relational-databases/backup-restore/back-up-database-backup-options-page.md)  
  
         Beim Sichern einer Datenbank können Sie die Sicherungskomprimierung für eine einzelne Datenbank, Datei oder Protokollsicherung steuern.  
  
    -   [Verwenden des Wartungsplanungs-Assistenten](../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md)  
  
         Mit dem Wartungsplanungs-Assistenten können Sie die Sicherungskomprimierung bei jeder Gruppe von geplanten vollständigen oder differenziellen Datenbank- bzw. Protokollsicherungen steuern.  
  
    -   Integration Services (SSIS) [Task "Datenbank sichern"](../../integration-services/control-flow/back-up-database-task.md)  
  
         Sie können das Verhalten der Sicherungskomprimierung steuern, wenn Sie ein Paket für die Sicherung einer einzelnen oder mehrerer Datenbanken erstellen.  
  
    -   [Sicherungseinstellungen für den Transaktionsprotokollversand](../../relational-databases/databases/log-shipping-transaction-log-backup-settings.md)  
  
         Sie können das Sicherungskomprimierungsverhalten von Protokollsicherungen steuern.  
  
  
## <a name="see-also"></a>Weitere Informationen  
 [Sicherungskomprimierung &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-compression-sql-server.md)  
  
  
