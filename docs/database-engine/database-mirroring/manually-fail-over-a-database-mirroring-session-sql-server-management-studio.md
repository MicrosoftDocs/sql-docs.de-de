---
title: Manuelles Failover für eine Datenbank-Spiegelungssitzung (SQL Server Management Studio) | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie ein manuelles Failover auf einen Spiegelserver mithilfe von SQL Server Management Studio initiieren. Die Spiegeldatenbank wird dann zur Prinzipaldatenbank.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- failover [SQL Server], database mirroring
- manual failover [SQL Server]
- database mirroring [SQL Server], failover
ms.assetid: 4ecf9c63-b3a4-4c54-b553-5bc37973232b
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 6f50c8c9de9309069eb45f24bc62532030e18199
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342281"
---
# <a name="manually-fail-over-a-database-mirroring-session-sql-server-management-studio"></a>Manueller Failover für eine Datenbank-Spiegelungssitzung (SQL Server Management Studio)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Wenn die gespiegelte Datenbank synchronisiert wird, also den Status SYNCHRONIZED aufweist, kann der Datenbankbesitzer ein manuelles Failover zu dem gespiegelten Server initiieren.  
  
 Während eines manuellen Failovers werden die Prinzipal- und Spiegelserverrollen für die Datenbank getauscht, auf der das Failover auftritt. Die Spiegelungsdatenbank wird zur Prinzipaldatenbank, die Prinzipaldatenbank zum Spiegel. Beispielsweise wird in der folgenden Tabelle das Tauschen von zwei Spiegelungspartnern durch ein manuelles Failover dargestellt: `SQLDBENGINE0_1` und `SQLDBENGINE0_2`.  
  
|Server|Vor dem Failover|Nach dem Failover|  
|------------|---------------------|--------------------|  
|`SQLDBENGINE0_1`|PRINCIPAL|MIRROR|  
|`SQLDBENGINE0_2`|MIRROR|PRINCIPAL|  
  
 Beachten Sie, dass die Serverrollen für andere Datenbank-Spiegelungssitzungen nicht davon betroffen sind. Weitere Informationen finden Sie unter [Rollenwechsel während einer Datenbank-Spiegelungssitzung &#40;SQL Server&#41;](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md)herunter.  
  
### <a name="to-manually-fail-over-database-mirroring"></a>Sie führen Sie ein manuelles Failover für Datenbankspiegelungen durch  
  
1.  Stellen Sie eine Verbindung zur Prinzipalserverinstanz her, und klicken Sie im Bereich **Objekt-Explorer** auf den Servernamen, um die Serverstruktur zu erweitern.  
  
2.  Erweitern Sie **Datenbanken**, und wählen Sie die Datenbank aus, für die ein Failover durchgeführt werden soll.  
  
3.  Klicken Sie mit der rechten Maustaste auf die Datenbank, wählen Sie **Tasks** aus, und klicken Sie dann auf **Spiegeln**. Dadurch wird die Seite **Spiegelung** im Dialogfeld **Datenbankeigenschaften** geöffnet.  
  
4.  Klicken Sie auf **Failover**.  
  
     Es wird ein Bestätigungsdialogfeld angezeigt.  Der Prinzipalserver versucht als erstes, mithilfe der Windows-Authentifizierung eine Verbindung mit dem Spiegelserver herzustellen. Wenn die Windows-Authentifizierung nicht funktioniert, zeigt der Prinzipalserver das Dialogfeld **Verbindung mit Server herstellen** an. Wenn der Spiegelserver die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Authentifizierung verwendet, wählen Sie im Feld **Authentifizierung** die Option **SQL Server-Authentifizierung** aus. Geben Sie im Textfeld **Anmeldename** das Anmeldekonto an, mit dem auf dem Spiegelserver eine Verbindung hergestellt werden soll, und geben Sie im Textfeld **Kennwort** das Kennwort für dieses Konto an.  
  
     Nach erfolgreicher Ausführung des Failovers, wird das Dialogfeld **Datenbankeigenschaften** geschlossen. Die Spiegelungsdatenbank wird zur Prinzipaldatenbank, die Prinzipaldatenbank zum Spiegel.  
  
     Falls ein Fehler beim Failover auftritt, wird eine Fehlermeldung angezeigt, und das Dialogfeld bleibt geöffnet.  
  
    > [!IMPORTANT]  
    >  Wenn Sie Eigenschaften seit dem Öffnen der Seite **Spiegelung** geändert haben, werden diese Änderungen nicht gespeichert.  
  
     Das Dialogfeld wird automatisch geschlossen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Datenbankeigenschaften &#40;Seite Wird gespiegelt&#41;](../../relational-databases/databases/database-properties-mirroring-page.md)   
 [Datenbankspiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [Ausführen des manuellen Failovers einer Datenbank-Spiegelungssitzung (Transact-SQL)](../../database-engine/database-mirroring/manually-fail-over-a-database-mirroring-session-transact-sql.md)   
 [Anhalten oder Fortsetzen einer Datenbank-Spiegelungssitzung (SQL Server)](../../database-engine/database-mirroring/pause-or-resume-a-database-mirroring-session-sql-server.md)   
 [Entfernen der Datenbankspiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/remove-database-mirroring-sql-server.md)  
  
  
