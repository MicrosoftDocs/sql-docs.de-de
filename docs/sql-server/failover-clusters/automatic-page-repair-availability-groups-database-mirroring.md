---
title: Automatische Seitenreparatur für Verfügbarkeitsgruppen und Datenbankspiegelung
description: In diesem Artikel erhalten Sie Informationen zum automatischen Reparieren von bestimmten Arten von Seitenbeschädigungen, wenn eine Datenbank Mitglied einer Always-On-Verfügbarkeitsgruppe oder einer Datenbankspiegelung ist.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: high-availability
ms.topic: troubleshooting
helpviewer_keywords:
- automatic page repair
- Availability Groups [SQL Server], automatic page repair
- database mirroring [SQL Server], automatic page repair
- suspect pages [SQL Server]
ms.assetid: cf2e3650-5fac-4f34-b50e-d17765578a8e
author: cawrites
ms.author: chadam
ms.openlocfilehash: 111a5ad98210207fcdf55d3656953ddda6ac38c0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346185"
---
# <a name="automatic-page-repair-availability-groups-database-mirroring"></a>Automatische Seitenreparatur (Verfügbarkeitsgruppen: Datenbankspiegelung)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Automatische Seitenreparatur wird von Datenbankspiegelung und [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]unterstützt. Wenn bestimmte Fehlertypen eine Seite beschädigen und sie unlesbar machen, versucht ein Datenbank-Spiegelungspartner (Prinzipal oder Spiegel) oder ein Verfügbarkeitsreplikat (primär oder sekundär), die Seite automatisch wiederherzustellen. Der Partner/das Replikat, der/das die Seite nicht lesen kann, fordert eine neue Kopie der Seite von seinem Partner oder einem anderen Replikat an. Wenn die Anforderung erfolgreich ist, wird die nicht lesbare Seite durch die lesbare Kopie ersetzt. Dadurch wird der Fehler normalerweise behoben.  
  
 Im Allgemeinen behandeln Datenbankspiegelung und [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] E/A-Fehler auf dieselbe Weise. Die wenigen Unterschiede werden hier explizit beschrieben.  
  
> [!NOTE]  
>  Die automatische Seitenreparatur unterscheidet sich von der DBCC-Reparatur. Bei einer automatischen Seitenreparatur bleiben alle Daten erhalten. Beim Beheben von Fehlern mithilfe der Option DBCC REPAIR_ALLOW_DATA_LOSS ist es jedoch möglicherweise erforderlich, dass einige Seiten (und somit Daten) gelöscht werden.  
  
-   [Fehlertypen, die zu einem automatischen Seitenreparaturversuch führen](#ErrorTypes)  
  
-   [Seitentypen, die nicht automatisch repariert werden können](#UnrepairablePageTypes)  
  
-   [Behandeln von E/A-Fehlern in der Prinzipaldatenbank/primären Datenbank](#PrimaryIOErrors)  
  
-   [Behandeln von E/A-Fehlern in der Spiegeldatenbank/sekundären Datenbank](#SecondaryIOErrors)  
  
-   [Bewährte Methode für Entwickler](#DevBP)  
  
-   [Vorgehensweise: Anzeigen von automatischen Seitenreparatur-Versuchen](#ViewAPRattempts)  
  
##  <a name="error-types-that-cause-an-automatic-page-repair-attempt"></a><a name="ErrorTypes"></a> Error Types That Cause an Automatic Page-Repair Attempt  
 Bei der automatischen Seitenreparatur bei einer Datenbankspiegelung werden nur Seiten in einer Datendatei repariert, bei denen bei einem Vorgang einer der in der folgenden Tabelle aufgeführten Fehler aufgetreten ist.  
  
|Fehlernummer|BESCHREIBUNG|Instanzen, die zu einer automatischen Seitenreparatur führen|  
|------------------|-----------------|---------------------------------------------------------|  
|823|Maßnahme wird nur ergriffen, wenn das Betriebssystem eine zyklische Redundanzprüfung (CRC, Redundancy Check) ausgeführt hat, bei der in den Daten ein Fehler gefunden wurde.|ERROR_CRC. Der Wert des Betriebssystems für diesen Fehler ist 23.|  
|824|Logische Fehler.|Logische Datenfehler, z. B. unterbrochener Schreibvorgang oder fehlerhafte Prüfsumme auf einer Seite.|  
|829|Eine Seite wurde mit "Wiederherstellung steht aus" gekennzeichnet.|Alle.|  
  
 Um aktuelle CRC-Fehler vom Typ 823 und Fehler vom Typ 824 anzuzeigen, rufen Sie die Tabelle [suspect_pages](../../relational-databases/system-tables/suspect-pages-transact-sql.md) in der [msdb](../../relational-databases/databases/msdb-database.md) -Datenbank auf.  

  
##  <a name="page-types-that-cannot-be-automatically-repaired"></a><a name="UnrepairablePageTypes"></a> Page Types That Cannot Be Automatically Repaired  
 Die automatische Seitenreparatur kann die folgenden Steuerelementseitentypen nicht reparieren:  
  
-   Dateiheaderseite (Seiten-ID 0).  
  
-   Seite 9 (die Startseite der Datenbank)  
  
-   Zuordnungsseiten: GAM-Seiten (Global Allocation Map), SGAM-Seiten (Shared Global Allocation Map) und PFS-Seiten (Page Free Space).  
  
 
##  <a name="handling-io-errors-on-the-principalprimary-database"></a><a name="PrimaryIOErrors"></a> Handling I/O Errors on the Principal/Primary Database  
 In der Prinzipaldatenbank/primären Datenbank wird die automatische Seitenreparatur nur ausgeführt, wenn sich die Datenbank im Status SYNCHRONIZED befindet und der Prinzipalserver/primäre Server noch Protokolldatensätze für die Datenbank an den Spiegelserver/sekundären Server sendet. Im Prinzip werden bei einer automatischen Seitenreparatur die folgenden Aktionen in dieser Reihenfolge ausgeführt:  
  
1.  Wenn in der Prinzipaldatenbank/primären Datenbank auf einer Datenseite ein Lesefehler auftritt, fügt der Prinzipalserver/primäre Server in die Tabelle [suspect_pages](../../relational-databases/system-tables/suspect-pages-transact-sql.md) eine Zeile mit dem entsprechenden Fehlerstatus ein. Zur Datenbankspiegelung fordert der Prinzipalserver dann eine Kopie der Seite vom Spiegelserver aus an. Für [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]überträgt der primäre Server die Anforderung an alle sekundären Server und ruft die Seite vom Server ab, der als Erster antwortet. In der Anforderung werden die Seiten-ID und die LSN angegeben, die sich derzeit am Ende des geleerten Protokolls befindet. Die Seite wird mit *Wiederherstellung steht aus* gekennzeichnet. Das bedeutet, dass während der automatischen Seitenreparatur kein Zugriff auf die Seite möglich ist. Bei dem Versuch, während der Seitenreparatur auf die Seite zuzugreifen, wird der Fehler 829 (<localizedText>Wiederherstellung steht aus</localizedText>) ausgegeben.  
  
2.  Nach Erhalt der Seitenanforderung wartet der Spiegelserver/sekundäre Server, bis das Protokoll bis zu der in der Anforderung angegebenen LSN wiederholt wurde. Dann versucht der Spiegelserver/sekundäre Server die Seite in seiner Kopie der Datenbank aufzurufen. Wenn der Zugriff möglich ist, sendet der Spiegelserver/sekundäre Server die Kopie der Seite an den Prinzipalserver/primären Server. Andernfalls gibt der Spiegelserver/sekundäre Server einen Fehler an den Prinzipalserver/primären Server zurück, und die automatische Seitenreparatur schlägt fehl.  
  
3.  Der Prinzipalserver/primäre Server verarbeitet die Antwort, die die neue Kopie der Seite enthält.  
  
4.  Nachdem mithilfe der automatischen Seitenreparatur eine fehlerverdächtige Seite repariert werden konnte, wird die Seite in der Tabelle **suspect_pages** als wiederhergestellt (**event_type** = 5) gekennzeichnet.  
  
5.  Wenn durch den Seiten-E/A-Fehler [verzögerte Transaktionen](../../relational-databases/backup-restore/deferred-transactions-sql-server.md)verursacht wurden, versucht der Prinzipalserver/primäre Server nach der Reparatur der Seite, diese Transaktionen aufzulösen.  
  
 
##  <a name="handling-io-errors-on-the-mirrorsecondary-database"></a><a name="SecondaryIOErrors"></a> Handling I/O Errors on the Mirror/Secondary Database  
 E/A-Fehler auf Datenseiten, die auf der Spiegeldatenbank/sekundären Datenbank auftreten, werden im Allgemeinen von Datenbankspiegelung und [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]auf dieselbe Weise behandelt.  
  
1.  Falls der Spiegel bei der Datenbankspiegelung beim Wiederholen eines Protokolldatensatzes einen oder mehrere Seiten-E/A-Fehler feststellt, wird die Spiegelungssitzung in den Status SUSPENDED versetzt. Falls ein sekundäres Replikat bei [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]beim Wiederholen eines Protokolldatensatzes ein oder mehrere Seiten-E/A-Fehler feststellt, wird die sekundäre Datenbank in den Status SUSPENDED versetzt. Zu diesem Zeitpunkt fügt der Spiegelserver/sekundäre Server in die Tabelle **suspect_pages** eine Zeile mit dem entsprechenden Fehlerstatus ein. Der Spiegelserver/sekundäre Server fordert dann eine Kopie der Seite vom Prinzipalserver/primären Server an.  
  
2.  Der Prinzipalserver/primäre Server versucht, die Seite in seiner Kopie der Datenbank aufzurufen. Wenn der Zugriff auf die Seite möglich ist, sendet der Prinzipalserver/primäre Server die Kopie der Seite an den Spiegelserver/sekundären Server.  
  
3.  Erhält der Spiegelserver/sekundäre Server Kopien aller angeforderten Seiten, unternimmt er den Versuch, die Spiegelungssitzung fortzusetzen. Wenn mithilfe der automatischen Seitenreparatur eine fehlerverdächtige Seite repariert werden konnte, wird die Seite in der Tabelle **suspect_pages** als wiederhergestellt (**event_type** = 4) gekennzeichnet.  
  
     Wenn ein Spiegelserver/sekundärer Server eine vom Prinzipalserver/primären Server angeforderte Seite nicht erhält, tritt bei der automatischen Seitenreparatur ein Fehler auf. Bei der Datenbankspiegelung bleibt die Spiegelungssitzung angehalten. Bei [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]bleibt die sekundäre Datenbank angehalten. Wenn die Spiegelungssitzung oder sekundäre Datenbank manuell fortgesetzt wird, werden die beschädigten Seiten während der nächsten Synchronisierungsphase erneut gefunden.  
  
 
##  <a name="developer-best-practice"></a><a name="DevBP"></a> Developer Best Practice  
 Eine automatische Seitenreparatur ist ein asynchroner Prozess, der im Hintergrund ausgeführt wird. Daher tritt bei einem Datenbankvorgang, bei dem eine nicht lesbare Seite angefordert wird, ein Fehler auf, und der Fehlercode für den Zustand wird zurückgegeben, der den Fehler ausgelöst hat. Beim Entwickeln einer Anwendung für eine gespiegelte Datenbank oder eine Verfügbarkeitsdatenbank sollten Sie Ausnahmen für fehlerhafte Vorgänge abfangen. Wenn der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Fehlercode 823, 824 oder 829 lautet, sollten Sie den Vorgang später wiederholen.  
  

##  <a name="how-to-view-automatic-page-repair-attempts"></a><a name="ViewAPRattempts"></a> Vorgehensweise: View Automatic Page-Repair Attempts (Vorgehensweise: Anzeigen von automatischen Seitenreparaturversuchen)  
 Die folgenden dynamischen Verwaltungssichten geben Zeilen für die letzten automatischen Seitenreparatur-Versuche auf einer angegebenen Verfügbarkeitsdatenbank oder gespiegelten Datenbank mit einem Maximum von 100 Zeilen pro Datenbank zurück.  
  
-   **Always On-Verfügbarkeitsgruppen:**  
  
     [sys.dm_hadr_auto_page_repair &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-hadr-auto-page-repair-transact-sql.md)  
  
     Gibt eine Zeile für jede versuchte automatische Seitenreparatur in einer beliebigen Verfügbarkeitsdatenbank auf einem Verfügbarkeitsreplikat zurück, das von der Serverinstanz für eine beliebige Verfügbarkeitsgruppe gehostet wird.  
  
-   **Datenbankspiegelung:**  
  
     [sys.dm_db_mirroring_auto_page_repair &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/database-mirroring-sys-dm-db-mirroring-auto-page-repair.md)  
  
     Gibt eine Zeile für jede automatische Seitenreparatur für jede gespiegelte Datenbank der Serverinstanz zurück.  
  
 
## <a name="see-also"></a>Weitere Informationen  
 [Verwalten der suspect_pages-Tabelle &#40;SQL Server&#41;](../../relational-databases/backup-restore/manage-the-suspect-pages-table-sql-server.md)   
 [Übersicht über AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Datenbankspiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)  
  
  


