---
title: Fehlgeschlagener Vorgang zum Hinzufügen einer Datei zu einer Verfügbarkeitsgruppe
description: In diesem Artikel wird die Problembehandlung bei fehlgeschlagenen add-file-Vorgängen in einer AlwaysOn-Verfügbarkeitsgruppe behandelt. Dateipfade können sich zwischen den Systemen unterscheiden, die das primäre und das sekundäre Replikat hosten.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- secondary databases [SQL Server], Availability Groups
- Availability Groups [SQL Server], troubleshooting
ms.assetid: 31ceaebf-864b-4dd0-9112-0d047b0316ad
author: cawrites
ms.author: chadam
ms.openlocfilehash: 0260d63c44c3006186b773752e0df8482f055bbc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348978"
---
# <a name="troubleshoot-a-failed-add-file-operation-always-on-availability-groups"></a>Problembehandlung bei einem fehlgeschlagenen Vorgang zum Hinzufügen einer Datei (Always On-Verfügbarkeitsgruppen)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  In einigen Bereitstellungen von Always On-Verfügbarkeitsgruppen unterscheiden sich Dateipfade zwischen dem System, das das primäre Replikat hostet, und Systemen, die ein sekundäres Replikat hosten. Wenn der Dateipfad eines Vorgangs zum Hinzufügen einer Datei auf einem sekundären Replikat nicht vorhanden ist, dann ist dieser Vorgang auf der primären Datenbank erfolgreich. Der Vorgang zum Hinzufügen einer Datei bewirkt jedoch, dass die sekundäre Datenbank angehalten wird. Dies bewirkt dann, dass das sekundäre Replikat den Status NOT SYNCHRONIZING erhält.  
  
> [!NOTE]  
>  Außerdem sollte der Dateipfad (einschließlich des Laufwerkbuchstabens) einer sekundären Datenbank nach Möglichkeit mit dem Pfad der entsprechenden primären Datenbank übereinstimmen.  
  
## <a name="problem-resolution"></a>Problemlösung  
 Um dieses Problem zu beheben, muss der Datenbankbesitzer die folgenden Schritte ausführen:  
  
1.  Entfernen Sie die sekundäre Datenbank aus der Verfügbarkeitsgruppe. Weitere Informationen finden Sie unter [Entfernen einer sekundären Datenbank aus einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-secondary-database-from-an-availability-group-sql-server.md).  
  
2.  Stellen Sie in der vorhandenen sekundären Datenbank eine vollständige Sicherung der Dateigruppe wieder her, die die zur sekundären Datenbank hinzugefügte Datei enthält. Verwenden Sie hierzu WITH NORECOVERY und WITH MOVE (geben Sie den Dateipfad auf der Serverinstanz an, die das sekundäre Replikat hostet). Weitere Informationen finden Sie unter [Wiederherstellen einer Datenbank an einem neuen Speicherort &#40;SQL Server&#41;](../../../relational-databases/backup-restore/restore-a-database-to-a-new-location-sql-server.md).  
  
3.  Sichern Sie das Transaktionsprotokoll, das den Vorgang zum Hinzufügen der Datei auf der primären Datenbank enthält, und stellen Sie die Protokollsicherung auf der sekundären Datenbank manuell mit WITH NORECOVERY und WITH MOVE wieder her.  
  
4.  Bereiten Sie die sekundäre Datenbank auf das erneute Hinzufügen zur Verfügbarkeitsgruppe vor, indem Sie mit WITH NO RECOVERY alle sonstigen ausstehenden Protokollsicherungen von der primären Datenbank wiederherstellen.  
  
5.  Fügen Sie die sekundäre Datenbank wieder zur Verfügbarkeitsgruppe hinzu. Weitere Informationen finden Sie unter [Verknüpfen einer sekundären Datenbank mit einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)aktiviert sind, eine Always On-Verfügbarkeitsgruppe zu erstellen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Übersicht über AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Manuelles Vorbereiten einer sekundären Datenbank auf eine Verfügbarkeitsgruppe (SQL Server)](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)   
 [Problembehandlung bei verwaisten Benutzern &#40;SQL Server&#41;](../../../sql-server/failover-clusters/troubleshoot-orphaned-users-sql-server.md)   
 [Problembehandlung für die AlwaysOn-Verfügbarkeitsgruppenkonfiguration &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md)
