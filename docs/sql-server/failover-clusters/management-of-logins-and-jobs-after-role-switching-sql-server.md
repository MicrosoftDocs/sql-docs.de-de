---
title: Verwalten von Anmeldungen und Aufträgen nach einem Spiegelungsfailover
description: Erfahren Sie, wie Sie Anmeldungen und Aufträge verwalten, nachdem Sie ein Failover der gespiegelten Datenbank von der primären zur sekundären Datenbank durchgeführt haben.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: high-availability
ms.topic: how-to
helpviewer_keywords:
- role switching [SQL Server]
ms.assetid: fc2fc949-746f-40c7-b5d4-3fd51ccfbd7b
author: cawrites
ms.author: chadam
ms.openlocfilehash: 1e410b42c5e53b479f28febee1ccd716d68437d8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354010"
---
# <a name="management-of-logins-and-jobs-after-role-switching-sql-server"></a>Verwaltung von Anmeldenamen und Aufträgen nach einem Rollenwechsel (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
   Bei der Bereitstellung einer Lösung für hohe Verfügbarkeit oder Notfallwiederherstellung für eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbank ist wichtig, relevante Informationen, die in der **master**- oder **msdb**-Datenbank für die Datenbank gespeichert sind, zu reproduzieren. Zu relevanten Informationen gehören normalerweise die Aufträge der primären/Prinzipaldatenbank sowie die Anmeldenamen der Benutzer oder Prozesse, die eine Verbindung mit der Datenbank herstellen müssen. Diese Informationen sollten auf jeder [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz dupliziert werden, die eine sekundäre/Spiegeldatenbank hostet. Nach dem Rollenwechsel sollten die Informationen möglichst in der neuen primären/Prinzipaldatenbank programmgesteuert reproduziert werden.  
  
## <a name="logins"></a>Anmeldungen  
 Anmeldenamen, denen eine Berechtigung für den Zugriff auf die Prinzipaldatenbank zugewiesen wurde, sollten auf jeder Serverinstanz reproduziert werden, die eine Kopie der Datenbank hostet. Wenn die primäre/Prinzipalrolle gewechselt wird, können ausschließlich Benutzer, deren Anmeldenamen auf der neuen primären/Prinzipalserverinstanz enthalten sind, auf die neue primäre/Prinzipaldatenbank zugreifen. Benutzer, deren Anmeldenamen nicht auf der neuen primären/Prinzipalserverinstanz definiert wurden, sind verwaist und können nicht auf die Datenbank zugreifen.  
  
 Wenn ein Benutzer verwaist ist, erstellen Sie den Anmeldenamen auf der neuen primären/Prinzipalserverinstanz und führen [sp_change_users_login](../../relational-databases/system-stored-procedures/sp-change-users-login-transact-sql.md)aus. Weitere Informationen finden Sie unter [Problembehandlung bei verwaisten Benutzern &#40;SQL Server&#41;](../../sql-server/failover-clusters/troubleshoot-orphaned-users-sql-server.md)aus.  
  
###  <a name="logins-of-applications-that-use-sql-server-authentication-or-a-local-windows-login"></a><a name="SSauthentication"></a> Anmeldenamen von Anwendungen, die die SQL Server-Authentifizierung oder eine lokale Windows-Anmeldung verwenden  
 Wenn eine Anwendung die SQL Server-Authentifizierung oder eine lokale Windows-Anmeldung verwendet, können nicht übereinstimmende SIDs verhindern, dass der Anmeldename der Anwendung auf einer Remoteinstanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]aufgelöst wird. Aufgrund der nicht übereinstimmenden SIDs wird der Anmeldename auf der Remoteserverinstanz als verwaister Benutzer behandelt. Dieses Problem kann auftreten, wenn von einer Anwendung nach einem Failover eine Verbindung mit einer gespiegelten oder Protokollversand-Datenbank hergestellt wird bzw. wenn eine Verbindung mit einer Replikationsabonnenten-Datenbank hergestellt wird, die von einer Sicherung initialisiert wurde.  
  
 Um dieses Problem zu vermeiden, sollten Sie Vorbeugemaßnahmen ergreifen, wenn Sie eine solche Anwendung für die Verwendung einer Datenbank einrichten, die auf einer Remoteinstanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]gehostet wird. Eine Vorbeugungsmaßnahme besteht darin, die Anmeldenamen und Kennwörter von der lokalen Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf die Remoteinstanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]zu übertragen. Weitere Informationen zur Vermeidung dieses Problems finden Sie im KB-Artikel 918992 [Übertragen von Benutzernamen und Kennwörtern zwischen Instanzen von SQL Server](https://support.microsoft.com/kb/918992/).  
  
> [!NOTE]  
>  Dieses Problem betrifft lokale Windows-Konten auf unterschiedlichen Computern. Bei Domänenkonten tritt das Problem jedoch nicht auf, da die SID auf allen Computern identisch ist.  
  
 Weitere Informationen finden Sie unter [Verwaiste Benutzer bei Datenbankspiegelung und Protokollversand](/archive/blogs/sqlserverfaq/orphaned-users-with-database-mirroring-and-log-shipping) (Blog zur Datenbank-Engine).  
  
## <a name="jobs"></a>Aufträge  
 Aufträge, wie z. B. Sicherungsaufträge, erfordern besondere Aufmerksamkeit. Nach einem Rollenwechsel muss der Datenbankbesitzer oder der Systemadministrator die Aufträge für die neue primäre/Prinzipaldatenbank gewöhnlich erneut erstellen.  
  
 Wenn die frühere primäre/Prinzipalserverinstanz verfügbar ist, sollten Sie die ursprünglichen Aufträge auf dieser Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]löschen. Sie sollten beachten, dass Aufträge in der aktuellen Spiegeldatenbank fehlerhaft sind, weil sich die Datenbank im Status RESTORING befindet und dadurch nicht verfügbar ist.  
  
> [!NOTE]  
>  Unterschiedliche Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sind möglicherweise anders konfiguriert und verfügen u. U. über abweichende Laufwerkbuchstaben. Bei den Aufträgen für die einzelnen Partner müssen derartige Unterschiede berücksichtigt werden.  
  
## <a name="see-also"></a>Siehe auch  
 [Verwalten von Metadaten beim Bereitstellen einer Datenbank auf einer anderen Serverinstanz &#40;SQL Server&#41;](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md)   
 [Problembehandlung bei verwaisten Benutzern &#40;SQL Server&#41;](../../sql-server/failover-clusters/troubleshoot-orphaned-users-sql-server.md)  
  
