---
description: Aktivieren und Deaktivieren von Change Data Capture (SQL Server)
title: Aktivieren und Deaktivieren von Change Data Capture
ms.custom: seo-dt-2019
ms.date: 01/02/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- change data capture [SQL Server], enabling tables
- change data capture [SQL Server], enabling databases
- change data capture [SQL Server], disabling databases
- change data capture [SQL Server], disabling tables
ms.assetid: b741894f-d267-4b10-adfe-cbc14aa6caeb
author: rothja
ms.author: jroth
ms.openlocfilehash: 83e4d748032d4744480d90a977432c70b418ce8b
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88463731"
---
# <a name="enable-and-disable-change-data-capture-sql-server"></a>Aktivieren und Deaktivieren von Change Data Capture (SQL Server)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]
  In diesem Thema wird beschrieben, wie Sie Change Data Capture für Datenbanken und Tabelle aktivieren und deaktivieren können.  
  
## <a name="enable-change-data-capture-for-a-database"></a>Aktivieren von Change Data Capture für eine Datenbank  
 Bevor eine Aufzeichnungsinstanz für einzelne Tabellen erstellt werden kann, muss ein Mitglied der festen Serverrolle **sysadmin** zuerst die Datenbank für Change Data Capture aktivieren. Dies erfolgt durch Ausführen der gespeicherten Prozedur [sys.sp_cdc_enable_db &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-db-transact-sql.md) im Kontext der Datenbank. Um zu bestimmen, ob die Datenbank bereits aktiviert ist, fragen Sie die **is_cdc_enabled** -Spalte in der **sys.databases** -Katalogsicht ab.  
  
 Wenn eine Datenbank für Change Data Capture aktiviert ist, werden das **cdc** -Schema, der **cdc** -Benutzer, Metadatentabellen und andere Systemobjekte für die Datenbank erstellt. Das **cdc** -Schema enthält die Metadatentabellen für Change Data Capture, und sobald die Quelltabellen für Change Data Capture aktiviert wurden, dienen die einzelnen Änderungstabellen als Repository für die Änderungsdaten. Das **cdc** -Schema enthält außerdem zugeordnete Systemfunktionen, die verwendet werden, um Änderungsdaten abzufragen.  
  
 Change Data Capture erfordert die exklusive Verwendung des **cdc** -Schemas und des **cdc** -Benutzers. Wenn entweder ein Schema oder ein Datenbankbenutzer namens *cdc* schon in der Datenbank vorhanden ist, kann diese so lange für Change Data Capture nicht aktiviert werden, bis das Schema oder der Benutzer gelöscht oder umbenannt wird.  
  
 Ein Beispiel für das Aktivieren einer Datenbank finden Sie in der Vorlage "Datenbank für Change Data Capture aktivieren".  
  
> [!IMPORTANT]  
>  Um die Vorlagen in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]zu suchen, rufen Sie **Ansicht**auf, klicken Sie auf **Vorlagen-Explorer**, und wählen Sie dann **SQL Server-Vorlagen**aus. **Change Data Capture** ist ein Unterordner. In diesem Ordner finden Sie alle Vorlagen, auf die in diesem Thema verwiesen wird. Es gibt auch ein **Vorlagen-Explorer** -Symbol auf der [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] -Symbolleiste.  
  
```sql  
-- ====  
-- Enable Database for CDC template   
-- ====  
USE MyDB  
GO  
EXEC sys.sp_cdc_enable_db  
GO  
```  
  
## <a name="disable-change-data-capture-for-a-database"></a>Deaktivieren von Change Data Capture für eine Datenbank  
 Ein Mitglied der festen Serverrolle **sysadmin** kann die gespeicherte Prozedur [sys.sp_cdc_disable_db &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-disable-db-transact-sql.md) im Datenbankkontext ausführen, um Change Data Capture für eine Datenbank zu deaktivieren. Es ist nicht notwendig, einzelne Tabellen zu deaktivieren, bevor Sie die Datenbank deaktivieren. Durch das Deaktivieren der Datenbank werden alle mit ihr verbundenen Change Data Capture-Metadaten entfernt, einschließlich des **cdc** -Benutzers und -Schemas und der Change Data Capture-Aufträge. Durch Change Data Capture erstellte Gatingrollen werden jedoch nicht automatisch entfernt und müssen explizit gelöscht werden. Um zu bestimmen, ob die Datenbank aktiviert ist, fragen Sie die **is_cdc_enabled** -Spalte in der sys.databases-Katalogsicht ab.  
  
 Wenn eine Datenbank, für die Change Data Capture aktiviert ist, gelöscht wird, werden Change Data Capture-Aufträge automatisch entfernt.  
  
 Ein Beispiel für das Deaktivieren einer Datenbank finden Sie in der Vorlage "Datenbank für Change Data Capture deaktivieren".  
  
> [!IMPORTANT]  
>  Um die Vorlagen in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]zu suchen, rufen Sie **Ansicht**auf, klicken Sie auf **Vorlagen-Explorer**, und klicken Sie dann auf **SQL Server-Vorlagen**. **Change Data Capture** ist ein Unterordner, in dem Sie alle Vorlagen finden, auf die in diesem Thema verwiesen wird. Es gibt auch ein **Vorlagen-Explorer** -Symbol auf der [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] -Symbolleiste.  
  
```sql  
-- =======  
-- Disable Database for Change Data Capture template   
-- =======  
USE MyDB  
GO  
EXEC sys.sp_cdc_disable_db  
GO  
```  
  
## <a name="enable-change-data-capture-for-a-table"></a>Aktivieren von Change Data Capture für eine Tabelle  
 Nachdem eine Datenbank für Change Data Capture aktiviert wurde, können Mitglieder der festen Datenbankrolle **db_owner** mithilfe der gespeicherten Prozedur **sys.sp_cdc_enable_table**eine Aufzeichnungsinstanz für einzelne Quelltabellen erstellen. Um zu bestimmen, ob eine Quelltabelle bereits für Change Data Capture aktiviert ist, überprüfen Sie die is_tracked_by_cdc-Spalte in der **sys.tables** -Katalogsicht.  
  
 Wenn Sie eine Aufzeichnungsinstanz erstellen, können Sie die folgenden Optionen angeben:  
  
 **Aufzuzeichnende Spalten in der Quelltabelle**  
  
 Standardmäßig werden alle Spalten in der Quelltabelle als aufgezeichnete Spalten identifiziert. Wenn nur eine Teilmenge der Spalten nachverfolgt werden soll, z. B. aus Gründen des Datenschutzes oder der Leistung, geben Sie diese Teilmenge mithilfe des Parameters *\@captured_column_list* an.  
  
 **Eine Dateigruppe, welche die Änderungstabelle enthält.**  
  
 Standardmäßig befindet sich die Änderungstabelle in der Standarddateigruppe der Datenbank. Zum Steuern der Position einzelner Änderungstabellen kann ein Datenbankbesitzer den Parameter *\@filegroup_name* verwenden, um für die Änderungstabelle der Aufzeichnungsinstanz eine bestimmte Dateigruppe anzugeben. Die benannte Dateigruppe muss bereits vorhanden sein. Im Allgemeinen empfehlen wir, Änderungstabellen in eine von den Quelltabellen getrennte Dateigruppe einzufügen. Ein Beispiel für die Verwendung des Parameters *\@filegroup_name* finden Sie in der Vorlage **Option zum Aktivieren einer Tabelle unter Angabe der Dateigruppe**.  
  
```sql  
-- =========  
-- Enable a Table Specifying Filegroup Option Template  
-- =========  
USE MyDB  
GO  
  
EXEC sys.sp_cdc_enable_table  
@source_schema = N'dbo',  
@source_name   = N'MyTable',  
@role_name     = N'MyRole',  
@filegroup_name = N'MyDB_CT',  
@supports_net_changes = 1  
GO  
```  
  
 **Eine Rolle, mit deren Hilfe der Zugriff auf eine Änderungstabelle gesteuert wird.**  
  
 Der Zweck der benannten Rolle besteht darin, den Zugriff auf die Änderungsdaten zu steuern. Die angegebene Rolle kann eine vorhandene feste Serverrolle oder eine Datenbankrolle sein. Wenn die angegebene Rolle nicht bereits vorhanden ist, wird automatisch eine Datenbankrolle mit diesem Namen erstellt. Mitglieder der Rollen **sysadmin** oder **db_owner** haben vollen Zugriff auf die Daten in den Änderungstabellen. Alle anderen Benutzer müssen über die SELECT-Berechtigung für alle aufgezeichneten Spalten der Quelltabelle verfügen. Ist eine Rolle angegeben, müssen Benutzer, die nicht Mitglieder der Rollen **sysadmin** oder **db_owner** sind, darüber hinaus Mitglieder der angegebenen Rolle sein.  
  
 Wenn Sie keine Gatingrolle verwenden möchten, legen Sie den Parameter *\@role_name* explizit auf NULL fest. Ein Beispiel für das Aktivieren einer Tabelle ohne Gatingrolle finden Sie unter **Enable a Table Without Using a Gating Role** .  
  
```sql  
-- =========  
-- Enable a Table Without Using a Gating Role template   
-- =========  
USE MyDB  
GO  
EXEC sys.sp_cdc_enable_table  
@source_schema = N'dbo',  
@source_name   = N'MyTable',  
@role_name     = NULL,  
@supports_net_changes = 1  
GO  
  
```  
  
 **Eine Funktion zum Abfragen der Nettoänderungen.**  
  
 Eine Aufzeichnungsinstanz umfasst immer eine Tabellenwertfunktion, um alle Änderungstabelleneinträge zurückzugeben, die innerhalb eines definierten Intervalls auftreten. Diese Funktion wird benannt, indem der Name der Aufzeichnungsinstanz an "cdc.fn_cdc_get_all_changes_" angefügt wird. Weitere Informationen finden Sie unter [cdc.fn_cdc_get_all_changes_&#60;capture_instance&#62;  &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-all-changes-capture-instance-transact-sql.md).  
  
 Wird der Parameter *\@supports_net_changes* auf 1 festgelegt, wird auch eine Funktion für Nettoänderungen für die Aufzeichnungsinstanz generiert. Diese Funktion gibt für jede einzelne Zeile, die innerhalb des beim Aufruf angegebenen Intervalls geändert wurde, nur eine Änderung zurück. Weitere Informationen finden Sie unter [cdc.fn_cdc_get_net_changes_&#60;capture_instance&#62; &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-net-changes-capture-instance-transact-sql.md).  
  
 Zur Unterstützung von Abfragen für Nettoänderungen muss die Quelltabelle einen Primärschlüssel oder einen eindeutigen Index aufweisen, damit die Zeilen eindeutig identifiziert werden können. Wird ein eindeutiger Index verwendet, muss dessen Name mithilfe des Parameters *\@index_name* angegeben werden. Die für den Primärschlüssel oder den eindeutigen Index definierten Spalten müssen in der Liste der aufgezeichneten Quellspalten enthalten sein.  
  
 Ein Beispiel zur Erläuterung der Erstellung einer Aufzeichnungsinstanz mit beiden Abfragefunktionen finden Sie in der Vorlage **Enable a Table for All and Net Changes Queries** .  
  
```sql  
-- =============  
-- Enable a Table for All and Net Changes Queries template   
-- =============  
USE MyDB  
GO  
EXEC sys.sp_cdc_enable_table  
@source_schema = N'dbo',  
@source_name   = N'MyTable',  
@role_name     = N'MyRole',  
@supports_net_changes = 1  
GO  
```  
  
> [!NOTE]
>  Wenn Change Data Capture für eine Tabelle mit einem vorhandenen Primärschlüssel aktiviert ist und der Parameter *\@index_name* nicht zum Identifizieren eines alternativen eindeutigen Indexes verwendet wird, verwendet die Change Data Capture-Funktion den Primärschlüssel. Nachfolgende Änderungen am Primärschlüssel sind nicht zulässig, ohne zuerst Change Data Capture für die Tabelle zu deaktivieren. Dies gilt unabhängig davon, ob Unterstützung für Abfragen für Nettoänderungen angefordert wurde, als Change Data Capture konfiguriert wurde. Wenn zum Zeitpunkt der Aktivierung für Change Data Capture für eine Tabelle kein Primärschlüssel vorhanden ist, wird das nachträgliche Hinzufügen eines Primärschlüssels von Change Data Capture ignoriert. Da Change Data Capture keinen Primärschlüssel verwendet, der nach der Aktivierung der Tabelle erstellt wurde, können der Schlüssel und die Schlüsselspalten ohne Einschränkungen entfernt werden.  
  
## <a name="disable-change-data-capture-for-a-table"></a>Deaktivieren von Change Data Capture für eine Tabelle  
 Mitglieder der festen Datenbankrolle **db_owner** können eine Aufzeichnungsinstanz für einzelne Quelltabellen mithilfe der gespeicherten Prozedur **sys.sp_cdc_disable_table**entfernen. Um zu bestimmen, ob eine Quelltabelle derzeit für Change Data Capture aktiviert ist, überprüfen Sie die **is_tracked_by_cdc** -Spalte in der **sys.tables** -Katalogsicht. Wenn nach dem Deaktivieren keine Tabelle für die Datenbank aktiviert sind, werden die Change Data Capture-Aufträge ebenfalls entfernt.  
  
 Wenn eine Tabelle, für die Change Data Capture aktiviert ist, gelöscht wird, werden Change Data Capture-Metadaten, die mit der Tabelle verbunden sind, automatisch entfernt.  
  
 Ein Beispiel für das Deaktivieren einer Tabelle finden Sie in der Vorlage "Eine Aufzeichnungsinstanz für eine Tabelle deaktivieren".  
  
```sql  
-- =====  
-- Disable a Capture Instance for a Table template   
-- =====  
USE MyDB  
GO  
EXEC sys.sp_cdc_disable_table  
@source_schema = N'dbo',  
@source_name   = N'MyTable',  
@capture_instance = N'dbo_MyTable'  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Nachverfolgen von Datenänderungen &#40;SQL Server&#41;](../../relational-databases/track-changes/track-data-changes-sql-server.md)   
 [Über Change Data Capture &#40;SQL Server&#41;](../../relational-databases/track-changes/about-change-data-capture-sql-server.md)   
 [Arbeiten mit Änderungsdaten &#40;SQL Server&#41;](../../relational-databases/track-changes/work-with-change-data-sql-server.md)   
 [Verwalten und Überwachen von Change Data Capture &#40;SQL Server&#41;](../../relational-databases/track-changes/administer-and-monitor-change-data-capture-sql-server.md)  
  
  
