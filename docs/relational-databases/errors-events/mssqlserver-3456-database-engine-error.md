---
description: MSSQLSERVER_3456
title: MSSQLSERVER_3456 | Microsoft-Dokumentation
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 3456 (Database Engine error)
ms.assetid: d11b2b2c-3ae4-4023-b82f-05b561bfacce
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: a4809d1ba5e22e8e8d9a9f0d574f2690731e62a8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201582"
---
# <a name="mssqlserver_3456"></a>MSSQLSERVER_3456
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Details  
  
| attribute | Wert |  
| :-------- | :---- |  
|Produktname|SQL Server|  
|Ereignis-ID|3456|  
|Ereignisquelle|MSSQLSERVER|  
|Komponente|SQLEngine|  
|Symbolischer Name|REC_REDOLSNMISMATCH|  
|Meldungstext|Der Protokolleintrag %S_LSN für die Transaktions-ID %S_XID auf Seite %S_PGID, %.*ls-Datenbank (Datenbank-ID %d) konnte nicht rückgängig gemacht werden. Seite: LSN = %S_LSN, Typ = %ld. Protokoll: OpCode = %ld, Kontext %ld, PrevPageLSN: %S_LSN. Stellen Sie die Datenbank von einer Sicherung wieder her, oder reparieren Sie die Datenbank.|  
  
## <a name="explanation"></a>Erklärung  
Der Wiederherstellungsvorgang konnte das Transaktionsprotokoll nicht wiederholen. Durch diesen Fehler wurde die Datenbank in den SUSPECT-Status geschaltet. Die primäre Dateigruppe und möglicherweise weitere Dateigruppen sind fehlerverdächtig und u. U. beschädigt. Die Datenbank kann während Starts von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nicht wiederhergestellt werden und ist daher nicht verfügbar. Eine Aktion seitens des Benutzers ist erforderlich, um das Problem zu beheben.  
  
Falls dieser Fehler für **tempdb** auftritt, wird die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz heruntergefahren.  
  
## <a name="user-action"></a>Benutzeraktion  
Dieser Fehler kann auf vorübergehende Systemschwierigkeiten zurückzuführen sein, die beim Versuch, die Serverinstanz zu starten oder eine Datenbank wiederherzustellen, aufgetreten sind. Es kann jedoch auch ein dauerhafter Fehler vorliegen, der bei jedem Versuch auftritt, die Datenbank zu starten. Um Informationen zur Ursache zu erhalten, überprüfen Sie das Windows-Ereignisprotokoll auf einen vorangehenden Fehler, der Aufschluss über den aktuellen Fehler geben könnte.  
  
Um Informationen zur Ursache dieses Auftretens von Fehler 3456 zu erhalten, überprüfen Sie das Windows-Ereignisprotokoll auf einen vorangehenden Fehler, der Aufschluss über den aktuellen Fehler geben könnte. Die entsprechende Benutzeraktion hängt davon ab, ob die Informationen im Windows-Ereignisprotokoll angeben, dass der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Fehler durch eine vorübergehende Bedingung oder einen dauerhaften Fehler verursacht wurde. Informationen zu den Benutzeraktionen zum Beheben von Fehler 3456 finden Sie in der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Onlinedokumentation.  
  
## <a name="see-also"></a>Weitere Informationen  
[ALTER DATABASE &#40;Transact-SQL&#41;](~/t-sql/statements/alter-database-transact-sql-set-options.md)  
[DBCC CHECKDB &#40;Transact-SQL&#41;](~/t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)  
[Vollständige Datenbankwiederherstellungen &#40;einfaches Wiederherstellungsmodell&#41;](~/relational-databases/backup-restore/complete-database-restores-simple-recovery-model.md)  
[MSSQLSERVER_824](~/relational-databases/errors-events/mssqlserver-824-database-engine-error.md)  
[sys.databases &#40;Transact-SQL&#41;](~/relational-databases/system-catalog-views/sys-databases-transact-sql.md)  
  
