---
title: CREATE TRIGGER (Transact-SQL)
description: Transact-SQL-Referenz für die CREATE TRIGGER-Anweisung, die zum Erstellen eines DML-, DDL- oder LOGON-Triggers verwendet wird.
ms.date: 10/30/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE TRIGGER
- TRIGGER
- CREATE_TRIGGER_TSQL
- TRIGGER_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- recursive DML triggers [SQL Server]
- CREATE TRIGGER statement
- multiple triggers
- deferred name resolution, DML triggers
- DML triggers, creating
- nested triggers
- server-scoped triggers
- DDL triggers, creating
- triggers [SQL Server], creating
- database-scoped triggers [SQL Server]
ms.assetid: edeced03-decd-44c3-8c74-2c02f801d3e7
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.reviewer: mathoma
ms.openlocfilehash: ff81e043894af70e9a51991cec000fede3c31521
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345266"
---
# <a name="create-trigger-transact-sql"></a>CREATE TRIGGER (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Erstellt einen DML-, DDL- oder LOGON-Trigger. Ein Trigger ist eine spezielle Art von gespeicherter Prozedur, die automatisch ausgeführt wird, wenn ein Ereignis auf dem Datenbankserver auftritt. DML-Trigger werden ausgeführt, wenn ein Benutzer versucht, Daten mithilfe eines DML-Ereignisses (Data Manipulation Language, Datenbearbeitungssprache) zu ändern. DML-Ereignisse sind INSERT-, UPDATE- oder DELETE-Anweisungen für eine Tabelle oder Sicht. Diese Trigger werden ausgelöst, sobald ein beliebiges gültiges Ereignis ausgelöst wird, unabhängig davon, ob Tabellenzeilen betroffen sind oder nicht. Weitere Informationen finden Sie unter [DML Triggers](../../relational-databases/triggers/dml-triggers.md).  
  
DDL-Trigger werden als Reaktion auf verschiedene DDL-Ereignisse (Data Definition Language, Datendefinitionssprache) ausgeführt. Diese Ereignisse entsprechen im Wesentlichen den Anweisungen CREATE, ALTER und DROP von [!INCLUDE[tsql](../../includes/tsql-md.md)] sowie bestimmten gespeicherten Systemprozeduren, die DDL-ähnliche Vorgänge ausführen. 

LOGON-Trigger werden als Reaktion auf das LOGON-Ereignis ausgelöst, das wiederum ausgelöst wird, wenn eine Benutzersitzung eingerichtet wird. Sie können Trigger direkt aus [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen oder aus Methoden von Assemblys erstellen, die in der [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] Common Language Runtime (CLR) erstellt und auf eine Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] hochgeladen werden. Mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] können Sie mehrere Trigger für jede konkrete Anweisung erstellen.  
  
> [!IMPORTANT]  
>  Bösartiger Code innerhalb von Triggern kann unter ausgeweiteten Privilegien ausgeführt werden. Weitere Informationen dazu, wie Sie diese Bedrohung minimieren, finden Sie unter [Verwalten der Triggersicherheit](../../relational-databases/triggers/manage-trigger-security.md).  
  
> [!NOTE]  
>  Die Integration der .NET Framework-CLR in SQL Server wird in diesem Artikel erläutert. Die Integration der CLR gilt nicht für Azure SQL-Datenbank.  
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="sql-server-syntax"></a>SQL Server-Syntax  
  
```syntaxsql
-- SQL Server Syntax  
-- Trigger on an INSERT, UPDATE, or DELETE statement to a table or view (DML Trigger)  
  
CREATE [ OR ALTER ] TRIGGER [ schema_name . ]trigger_name   
ON { table | view }   
[ WITH <dml_trigger_option> [ ,...n ] ]  
{ FOR | AFTER | INSTEAD OF }   
{ [ INSERT ] [ , ] [ UPDATE ] [ , ] [ DELETE ] }   
[ WITH APPEND ]  
[ NOT FOR REPLICATION ]   
AS { sql_statement  [ ; ] [ ,...n ] | EXTERNAL NAME <method specifier [ ; ] > }  
  
<dml_trigger_option> ::=  
    [ ENCRYPTION ]  
    [ EXECUTE AS Clause ]  
  
<method_specifier> ::=  
    assembly_name.class_name.method_name  
  
```  
  
```syntaxsql
-- SQL Server Syntax  
-- Trigger on an INSERT, UPDATE, or DELETE statement to a 
-- table (DML Trigger on memory-optimized tables)  
  
CREATE [ OR ALTER ] TRIGGER [ schema_name . ]trigger_name   
ON { table }   
[ WITH <dml_trigger_option> [ ,...n ] ]  
{ FOR | AFTER }   
{ [ INSERT ] [ , ] [ UPDATE ] [ , ] [ DELETE ] }   
AS { sql_statement  [ ; ] [ ,...n ] }  
  
<dml_trigger_option> ::=  
    [ NATIVE_COMPILATION ]  
    [ SCHEMABINDING ]  
    [ EXECUTE AS Clause ]  
  
```  
  
```syntaxsql
-- Trigger on a CREATE, ALTER, DROP, GRANT, DENY, 
-- REVOKE or UPDATE statement (DDL Trigger)  
  
CREATE [ OR ALTER ] TRIGGER trigger_name   
ON { ALL SERVER | DATABASE }   
[ WITH <ddl_trigger_option> [ ,...n ] ]  
{ FOR | AFTER } { event_type | event_group } [ ,...n ]  
AS { sql_statement  [ ; ] [ ,...n ] | EXTERNAL NAME < method specifier >  [ ; ] }  
  
<ddl_trigger_option> ::=  
    [ ENCRYPTION ]  
    [ EXECUTE AS Clause ]  
  
```  
  
```syntaxsql
-- Trigger on a LOGON event (Logon Trigger)  
  
CREATE [ OR ALTER ] TRIGGER trigger_name   
ON ALL SERVER   
[ WITH <logon_trigger_option> [ ,...n ] ]  
{ FOR| AFTER } LOGON    
AS { sql_statement  [ ; ] [ ,...n ] | EXTERNAL NAME < method specifier >  [ ; ] }  
  
<logon_trigger_option> ::=  
    [ ENCRYPTION ]  
    [ EXECUTE AS Clause ]  
  
```  
  
## <a name="azure-sql-database-syntax"></a>Azure SQL-Datenbank-Syntax  
  
```syntaxsql
-- Azure SQL Database Syntax   
-- Trigger on an INSERT, UPDATE, or DELETE statement to a table or view (DML Trigger)  
  
CREATE [ OR ALTER ] TRIGGER [ schema_name . ]trigger_name   
ON { table | view }   
 [ WITH <dml_trigger_option> [ ,...n ] ]   
{ FOR | AFTER | INSTEAD OF }   
{ [ INSERT ] [ , ] [ UPDATE ] [ , ] [ DELETE ] }   
  AS { sql_statement  [ ; ] [ ,...n ] [ ; ] > }  
  
<dml_trigger_option> ::=   
        [ EXECUTE AS Clause ]  
  
```  
  
```syntaxsql
-- Azure SQL Database Syntax  
-- Trigger on a CREATE, ALTER, DROP, GRANT, DENY, 
-- REVOKE, or UPDATE STATISTICS statement (DDL Trigger)   
  
CREATE [ OR ALTER ] TRIGGER trigger_name   
ON { DATABASE }   
 [ WITH <ddl_trigger_option> [ ,...n ] ]   
{ FOR | AFTER } { event_type | event_group } [ ,...n ]   
AS { sql_statement  [ ; ] [ ,...n ]  [ ; ] }  
  
<ddl_trigger_option> ::=   
    [ EXECUTE AS Clause ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
OR ALTER  
**Gilt für**: Azure [!INCLUDE[ssSDS](../../includes/sssds-md.md)], [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1). 
  
Ändert den Trigger nur, wenn dieser bereits vorhanden ist. 
  
*schema_name*  
Der Name des Schemas, zu dem ein DML-Trigger gehört. DML-Trigger werden auf das Schema der Tabelle oder der Sicht begrenzt, in denen sie erstellt werden. *schema_name* kann für DDL- oder LOGON-Trigger nicht angegeben werden.  
  
*trigger_name*  
Der Name des Triggers. Ein *trigger_name* muss den Regeln für [Bezeichner](../../relational-databases/databases/database-identifiers.md) entsprechen, und *trigger_name* darf nicht mit # oder ## beginnen.  
  
*table* | *view*  
Die Tabelle oder Sicht, für die der DML-Trigger ausgeführt wird. Diese Tabelle oder Sicht wird manchmal als Triggertabelle oder Triggersicht bezeichnet. Die Angabe des vollqualifizierten Namens der Tabelle oder Sicht ist optional. Sie können nur mit einem INSTEAD OF-Trigger auf eine Sicht verweisen. Sie können DML-Trigger nicht für lokale oder globale temporäre Tabellen definieren.  
  
DATABASE  
Wendet den Bereich eines DDL-Triggers auf die aktuelle Datenbank an. Wenn angegeben, wird der Trigger jedes Mal ausgelöst, wenn in der aktuellen Datenbank *event_type* oder *event_group* auftritt.  
  
ALL SERVER  
**Gilt für**:  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.  
  
Wendet den Bereich eines DDL- oder LOGON-Triggers auf den aktuellen Server an. Wenn angegeben, wird der Trigger jedes Mal ausgelöst, wenn auf dem aktuellen Server *event_type* oder *event_group* auftritt.  
  
WITH ENCRYPTION  
**Gilt für**:  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.  
  
Verbirgt den Text der CREATE TRIGGER-Anweisung. Durch das Verwenden von WITH ENCRYPTION kann verhindert werden, dass der Trigger als Teil der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Replikation veröffentlicht wird. WITH ENCRYPTION kann nicht für CLR-Trigger angegeben werden.  
  
EXECUTE AS  
Gibt den Sicherheitskontext an, unter dem der Trigger ausgeführt wird. Sie können steuern, welches Benutzerkonto die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwendet, um so die Berechtigungen für beliebige Datenbankobjekte zu überprüfen, auf die der Trigger verweist.  
  
Diese Option ist für Trigger in speicheroptimierten Tabellen erforderlich.  
  
Weitere Informationen finden Sie unter [EXECUTE AS-Klausel &#40; Transact-SQL &#41; ](../../t-sql/statements/execute-as-clause-transact-sql.md).  
  
NATIVE_COMPILATION  
Gibt an, dass die Trigger nativ kompiliert werden.  
  
Diese Option ist für Trigger in speicheroptimierten Tabellen erforderlich.  
  
SCHEMABINDING  
Stellt sicher, dass Tabellen, auf die durch einen Trigger verwiesen wird, nicht gelöscht oder geändert werden können.  
  
Diese Option ist für Trigger in speicheroptimierten Tabellen erforderlich und wird nicht für Trigger in herkömmlichen Tabellen unterstützt.  
  
FOR | AFTER  
FOR und AFTER geben an, dass der DML-Trigger nur dann ausgelöst wird, nachdem alle Vorgänge, die in der den Trigger auslösenden SQL-Anweisung festgelegt sind, erfolgreich gestartet wurden. Alle referenziellen CASCADE-Aktionen und Einschränkungsüberprüfungen müssen ebenfalls erfolgreich ausgeführt worden sein, bevor dieser Trigger ausgelöst wird.  
  
Sie können keine AFTER-Trigger für Ansichten definieren.  
  
INSTEAD OF  
Gibt an, dass der DML-Trigger *anstelle* der auslösenden SQL-Anweisung gestartet wird, wodurch die Aktionen der auslösenden Anweisungen überschrieben werden. Sie können nicht INSTEAD OF für DDL- oder LOGON-Trigger angeben.  
  
Sie können maximal nur einen INSTEAD OF-Trigger pro INSERT-, UPDATE- oder DELETE-Anweisung für eine Tabelle oder Sicht definieren. Sie können auch Sichten auf Sichten definieren, wobei jede Sicht über einen eigenen INSTEAD OF-Trigger verfügt.  
  
Sie können nicht INSTEAD OF-Trigger für aktualisierbare Sichten definieren, die WITH CHECK OPTION verwenden. Dies löst einen Fehler aus, falls ein INSTEAD OF-Trigger einer aktualisierbaren Sicht hinzugefügt wird, in der WITH CHECK OPTION angegeben ist. Sie entfernen diese Option mithilfe von ALTER VIEW, bevor Sie den INSTEAD OF-Trigger definieren.  
  
{ [ DELETE ] [ , ] [ INSERT ] [ , ] [ UPDATE ] }  
Gibt die Anweisungen zur Datenänderung an, die den DML-Trigger aktivieren, wenn Sie ihn für diese Tabelle oder Sicht auszuführen versuchen. Geben Sie mindestens eine Option an. Verwenden Sie diese Optionen in der Triggerdefinition in beliebiger Kombination und Reihenfolge.  
  
Für INSTEAD OF-Trigger können Sie die Option DELETE nicht für Tabellen mit einer referenziellen Beziehung untereinander verwenden, wenn für ON DELETE die Option CASCADE angegeben ist. Ebenso ist die Option UPDATE nicht für Tabellen mit einer referenziellen Beziehung untereinander zulässig, wenn für ON UPDATE die Aktion CASCADE angegeben ist.  
  
WITH APPEND  
**Gilt für**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] bis [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].  
  
Gibt an, dass ein weiterer Trigger eines vorhandenen Typs hinzugefügt werden soll. WITH APPEND kann nicht mit INSTEAD OF-Triggern verwendet werden, oder falls der AFTER-Trigger explizit angegeben ist. Verwenden Sie WITH APPEND aus Gründen der Abwärtskompatibilität nur, wenn FOR angegeben ist, ohne INSTEAD OF oder AFTER. Sie können WITH APPEND nicht angeben, wenn Sie EXTERNAL NAME verwenden (d.h., wenn der Trigger ein CLR-Trigger ist).  
  
*event_type*  
Der Name eines [!INCLUDE[tsql](../../includes/tsql-md.md)]-Sprachereignisses, nach dessen Start ein DDL-Trigger ausgelöst wird. Gültige Ereignisse für DDL-Trigger werden unter [DDL-Ereignisse](../../relational-databases/triggers/ddl-events.md) aufgeführt.  
  
*event_group*  
Der Name einer vordefinierten Gruppe von [!INCLUDE[tsql](../../includes/tsql-md.md)]-Sprachereignissen. Der DDL-Trigger wird nach dem Start eines beliebigen [!INCLUDE[tsql](../../includes/tsql-md.md)]-Sprachereignisses ausgelöst, das zu *event_group* gehört. Gültige Ereignisgruppen für DDL-Trigger werden unter [DDL-Ereignisgruppen](../../relational-databases/triggers/ddl-event-groups.md) aufgeführt.  
  
Nach dem Ausführen von CREATE TRIGGER dient *event_group* außerdem als Makro, das der sys.trigger_events-Katalogsicht die verarbeitbaren Ereignistypen hinzufügt.  
  
NOT FOR REPLICATION  
**Gilt für**:  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.  
  
Gibt an, dass der Trigger nicht ausgeführt werden sollte, wenn ein Replikations-Agent die vom Trigger betroffene Tabelle ändert.  
  
*sql_statement*  
Die Triggerbedingungen und -aktionen. Triggerbedingungen geben zusätzliche Kriterien an, die bestimmen, ob der Versuch, DML-, DDL- oder LOGON-Ereignisse auszulösen, die Triggeraktionen auslöst.  
  
Die in den [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen angegebenen Triggeraktionen treten in Kraft, wenn versucht wird, den Vorgang auszuführen.  
  
Trigger können beliebig viele [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen jeglicher Art enthalten, einschließlich Ausnahmen. Weitere Informationen finden Sie in den Hinweisen. Ein Trigger ist dafür konzipiert, Daten auf der Grundlage einer Datenänderungs- oder Definitionsanweisung zu prüfen oder zu ändern, jedoch nicht dafür, Daten an den Benutzer zurückzugeben. Die [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen in einem Trigger enthalten häufig [Sprachkonstrukte zur Ablaufsteuerung](~/t-sql/language-elements/control-of-flow.md).  
  
DML-Trigger verwenden die gelöschten und eingefügten logischen (konzeptionellen) Tabellen. Strukturell ähneln sie der Tabelle, für die der Trigger definiert wurde, d.h. der Tabelle, für die versucht wurde, die Benutzeraktion auszuführen. Die gelöschten und eingefügten Tabellen enthalten die alten oder die neuen Werte der Zeilen, die möglicherweise durch die Benutzeraktion geändert werden. Um beispielsweise alle Werte in der `deleted`-Tabelle abzurufen, verwenden Sie:  
  
```sql  
SELECT * FROM deleted;  
```  
  
Weitere Informationen finden Sie unter [Verwenden der Tabellen „inserted“ und „deleted“](../../relational-databases/triggers/use-the-inserted-and-deleted-tables.md).  
  
DDL- und LOGON-Trigger zeichnen Informationen zu dem auslösenden Ereignis auf, indem sie die Funktion [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md) verwenden. Weitere Informationen finden Sie unter [Verwenden der EVENTDATA-Funktion](../../relational-databases/triggers/use-the-eventdata-function.md).  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ermöglicht das Aktualisieren der Spalten **text**, **ntext** oder **image** mithilfe des INSTEAD OF-Triggers für Tabellen oder Sichten.  
  
> [!IMPORTANT]
>  Die Datentypen **ntext**, **text** und **image** werden in einer zukünftigen Version von [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] entfernt. Vermeiden Sie die Verwendung dieser Datentypen bei neuen Entwicklungen, und planen Sie die Änderung von Anwendungen, in denen sie aktuell verwendet werden. Verwenden Sie stattdessen [nvarchar(max)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md), [varchar(max)](../../t-sql/data-types/char-and-varchar-transact-sql.md)und [varbinary(max)](../../t-sql/data-types/binary-and-varbinary-transact-sql.md) . Die Trigger AFTER und INSTEAD OF unterstützen die Daten **varchar(max)** , **nvarchar(max)** und **varbinary(max)** in den Tabellen „inserted“ und „deleted“.  
  
Bei Triggern in speicheroptimierten Tabellen ist auf der obersten Ebene nur ein ATOMIC-Block als *sql_statement* erlaubt. Das im ATOMIC-Block erlaubte T-SQL ist durch das in nativen Prozeduren zulässige T-SQL beschränkt.  
  
\< method_specifier > 
**Gilt für**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.  
  
Gibt für einen CLR-Trigger die Methode einer Assembly an, die an den Trigger gebunden werden soll. Die Methode darf keine Argumente enthalten und muss "void" zurückgeben. *class_name* muss ein gültiger [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Bezeichner sein und als Klasse mit Assemblysichtbarkeit in der Assembly vorhanden sein. Wenn die Klasse über einen mit einem Namespace qualifizierten Namen verfügt, der '.' verwendet, um die einzelnen Bestandteile des Namespace voneinander zu trennen, muss im Klassennamen [ ] oder " " als Trennzeichen verwendet werden. Bei der Klasse darf es sich nicht um eine geschachtelte Klasse handeln.  
  
> [!NOTE]  
>  Standardmäßig ist die Möglichkeit, dass [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] CLR-Code ausführt, deaktiviert. Sie können Datenbankobjekte, die auf verwaltete Codemodule verweisen, erstellen, ändern und löschen. Diese Verweise werden jedoch nur dann in einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ausgeführt, wenn die Option [clr enabled](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md) mithilfe von [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) aktiviert wird.  
  
## <a name="remarks-for-dml-triggers"></a>Hinweise zu DML-Triggern  
DML-Trigger werden häufig zum Erzwingen von Geschäftsregeln und Datenintegrität verwendet. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bietet deklarative referenzielle Integrität (DRI) durch die ALTER TABLE- und CREATE TABLE-Anweisungen. DRI stellt jedoch keine datenbankübergreifende referenzielle Integrität sicher. Referenzielle Integrität bezieht sich auf die Regeln über die Beziehungen zwischen den Primär- und den Fremdschlüsseln von Tabellen. Um referenzielle Integrität zu erzwingen, verwenden Sie die PRIMARY KEY- und FOREIGN KEY-Einschränkungen in ALTER TABLE und CREATE TABLE. Wenn es für die Triggertabelle Einschränkungen gibt, werden diese geprüft, nachdem der INSTEAD OF-Trigger ausgeführt wurde und bevor der AFTER-Trigger ausgeführt wird. Falls eine Verletzung der Einschränkungen vorliegt, wird für die Aktionen des INSTEAD OF-Triggers ein Rollback ausgeführt. Der AFTER-Trigger wird nicht ausgelöst.  
  
Sie können den ersten und letzten für eine Tabelle auszuführenden AFTER-Trigger mit „sp_settriggerorder“ angeben. Sie können in einer Tabelle für jeden INSERT-, UPDATE- und DELETE-Vorgang nur einen einzigen ersten und letzten AFTER-Trigger angeben. Sind für eine Tabelle weitere AFTER-Trigger vorhanden, werden diese nach dem Zufallsprinzip ausgeführt.  
  
Wenn eine ALTER TRIGGER-Anweisung den ersten oder letzten Trigger ändert, wird das erste oder letzte für den geänderten Trigger festgelegte Attribut gelöscht, und Sie müssen den Reihenfolgewert mit „sp_settriggerorder“ zurücksetzen.  
  
Ein AFTER-Trigger wird nur dann ausgeführt, wenn die den Trigger auslösende SQL-Anweisung erfolgreich ausgeführt wurde. Diese erfolgreiche Ausführung umfasst alle referenziellen kaskadierenden Aktionen und Einschränkungsüberprüfungen, die mit dem aktualisierten oder gelöschten Objekt verknüpft sind. Ein AFTER-Trigger löst nicht rekursiv einen INSTEAD OF-Trigger für dieselbe Tabelle aus.  
  
Falls ein für eine Tabelle definierter INSTEAD OF-Trigger eine Anweisung für die Tabelle ausführt, die normalerweise den INSTEAD OF-Trigger erneut auslösen würde, wird der Trigger nicht rekursiv aufgerufen. Stattdessen wird die Anweisung so verarbeitet, als ob in der Tabelle kein INSTEAD OF-Trigger vorhanden wäre, und die Kette der Einschränkungsvorgänge und AFTER-Triggerausführungen wird gestartet. Beispiel: Ein Trigger ist als INSTEAD OF INSERT-Trigger für eine Tabelle definiert. Außerdem führt der Trigger eine INSERT-Anweisung für dieselbe Tabelle aus; die vom INSTEAD OF-Trigger gestartete INSERT-Anweisung ruft den Trigger nicht erneut auf. Die vom Trigger gestartete INSERT-Anweisung startet das Ausführen der Einschränkungsaktionen und löst alle für die Tabelle definierten AFTER INSERT-Trigger aus.  
  
Wenn ein für eine Sicht definierter INSTEAD OF-Trigger eine Anweisung für die Sicht ausführt, die normalerweise den INSTEAD OF-Trigger erneut auslösen würde, wird der Trigger nicht rekursiv aufgerufen. Stattdessen wird die Anweisung als Änderungen an den zugrunde liegenden Basistabellen der Sicht aufgelöst. In diesem Fall muss die Sichtdefinition alle Einschränkungen für eine aktualisierbare Sicht erfüllen. Eine Definition zu aktualisierbaren Sichten finden Sie unter [Modify Data Through a View (Ändern von Daten über eine Sicht)](../../relational-databases/views/modify-data-through-a-view.md).  
  
Beispiel: Ein Trigger ist als INSTEAD OF UPDATE-Trigger für eine Sicht definiert. Außerdem führt der Trigger eine UPDATE-Anweisung aus, die auf dieselbe Sicht verweist; die vom INSTEAD OF-Trigger gestartete UPDATE-Anweisung ruft den Trigger nicht erneut auf. Die von dem Trigger gestartete UPDATE-Anweisung wird für die Sicht so verarbeitet, als ob in der Sicht kein INSTEAD OF-Trigger vorhanden wäre. Die von der UPDATE-Anweisung geänderten Spalten müssen in eine einzige Basistabelle aufgelöst werden. Jede Änderung an einer zugrunde liegenden Basistabelle startet die Kette der definierten Einschränkungen und löst die für die Tabelle definierten AFTER-Trigger aus.  
  
### <a name="testing-for-update-or-insert-actions-to-specific-columns"></a>Testen auf UPDATE- oder INSERT-Aktionen in angegebenen Spalten  
Sie können einen [!INCLUDE[tsql](../../includes/tsql-md.md)]-Trigger erstellen, der auf der Grundlage von UPDATE- oder INSERT-Änderungen an den angegebenen Spalten bestimmte Aktionen ausführt. Verwenden Sie hierzu [UPDATE()](../../t-sql/functions/update-trigger-functions-transact-sql.md) oder [COLUMNS_UPDATED](../../t-sql/functions/columns-updated-transact-sql.md) innerhalb des Triggertexts. UPDATE() testet auf UPDATE- oder INSERT-Versuche in einer Spalte. COLUMNS_UPDATED testet, ob UPDATE- oder INSERT-Aktionen für mehrere Spalten ausgeführt wurden. Diese Funktion gibt ein Bitmuster zurück, das angibt, welche Spalten eingefügt oder aktualisiert wurden.  
  
### <a name="trigger-limitations"></a>Beschränkungen bei der Verwendung von Triggern  
CREATE TRIGGER muss die erste Anweisung in einem Batch sein und kann sich nur auf eine Tabelle beziehen.  
  
Ein Trigger kann nur in der aktuellen Datenbank erstellt werden; er darf jedoch auf Objekte außerhalb der aktuellen Datenbank verweisen.  
  
Wenn der Name des Triggerschemas angegeben ist, um den Trigger zu kennzeichnen, kennzeichnen Sie den Tabellennamen auf die gleiche Weise.  
  
Es ist möglich, in derselben CREATE TRIGGER-Anweisung dieselbe Triggeraktion für mehrere Benutzeraktionen festzulegen (beispielsweise INSERT und UPDATE).  
  
INSTEAD OF DELETE/UPDATE-Trigger können nicht für eine Tabelle definiert werden, die einen Fremdschlüssel hat, für den ON DELETE/UPDATE CASCADE angegeben ist.  
  
In einem Trigger kann jede beliebige SET-Anweisung angegeben werden. Die ausgewählte SET-Option bleibt während der Ausführung des Triggers in Kraft und kehrt dann zur vorherigen Einstellung zurück.  
  
Wenn ein Trigger ausgelöst wird, werden die Ergebnisse wie bei einer gespeicherten Prozedur an die aufrufende Anwendung zurückgegeben. Um zu verhindern, dass Ergebnisse aufgrund einer Triggerauslösung an eine Anwendung zurückgegeben werden, verwenden Sie in einem Trigger keine SELECT-Anweisungen, die Ergebnisse oder Anweisungen zurückgeben, die Variablenzuweisungen durchführen. Ein Trigger, der entweder SELECT-Anweisungen enthält, die Ergebnisse an den Benutzer zurückgeben, oder Anweisungen, die Variablen zuweisen, erfordert eine besondere Behandlung. Sie müssten die zurückgegebenen Ergebnisse in alle Anwendungsprogramme schreiben, in denen Änderungen an der Triggertabelle zulässig sind. Wenn Variablenzuweisungen in einem Trigger erfolgen müssen, verwenden Sie eine SET NOCOUNT-Anweisung am Anfang des Triggers, um die Rückgabe von Resultsets zu verhindern.  
  
Obwohl eine TRUNCATE TABLE-Anweisung mit einer DELETE-Anweisung vergleichbar ist, löst sie keine Trigger aus, da die Löschung einzelner Zeilen nicht protokolliert wird. Allerdings müssen nur die Benutzer, die die Berechtigung zur Ausführung einer TRUNCATE TABLE-Anweisung besitzen, beachten, dass ein DELETE-Trigger durch eine TRUNCATE TABLE-Anweisung unbeabsichtigt umgangen werden kann.  
  
Die WRITETEXT-Anweisung, ob protokolliert oder nicht protokolliert, aktiviert keinen Trigger.  
  
Die folgenden [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen sind in einem DML-Trigger nicht zulässig:  

- ALTER DATABASE
- CREATE DATABASE
- DROP DATABASE
- RESTORE DATABASE
- RESTORE LOG
- RECONFIGURE

Darüber hinaus sind die folgenden [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen im Text des DML-Triggers nicht zulässig, wenn dieser für eine Tabelle oder Sicht verwendet wird, die das Ziel der den Trigger auslösenden Aktion ist.  
  
- CREATE INDEX (einschließlich CREATE SPATIAL INDEX und CREATE XML INDEX)
- ALTER INDEX
- DROP INDEX
- DROP TABLE
- DBCC DBREINDEX
- ALTER PARTITION FUNCTION
- ALTER TABLE bei Verwendung für die folgenden Aufgaben:
    - Hinzufügen, Ändern oder Löschen von Spalten
    - Wechseln zwischen Partitionen
    - Hinzufügen oder Löschen von PRIMARY KEY- oder UNIQUE-Einschränkungen

> [!NOTE]  
>  Da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] benutzerdefinierte Trigger für Systemtabellen nicht unterstützt, sollten Sie für Systemtabellen keine benutzerdefinierten Trigger erstellen. 

### <a name="optimizing-dml-triggers"></a>Optimieren von DML-Triggern
Trigger funktionieren in Transaktionen (impliziert oder anderweitig), und während sie offen sind, sperren sie Ressourcen. Die Sperre wird beibehalten, bis die Transaktion bestätigt (mit COMMIT) oder abgelehnt wurde (mit ROLLBACK). Je länger ein Trigger ausgeführt wird, desto höher ist die Wahrscheinlichkeit, dass ein anderer Prozess blockiert wird. Schreiben Sie Trigger daher so, dass ihre Dauer so gering wie möglich ist. Eine Möglichkeit, die Dauer zu reduzieren, ist, einen Trigger freizugeben, wenn eine DML-Anweisung 0 (null) Zeilen ändert. 

Zum Freigeben des Triggers für einen Befehl, der keine Zeilen ändert, verwenden Sie die Systemvariable [ROWCOUNT_BIG](../functions/rowcount-big-transact-sql.md). 

Der folgende T-SQL-Codeausschnitt veranschaulicht die Freigabe des Triggers für einen Befehl, der keine Zeilen ändert. Dieser Code sollte am Anfang jedes DML-Triggers vorhanden sein:

```sql
IF (ROWCOUNT_BIG() = 0)
RETURN;
```
  
  
## <a name="remarks-for-ddl-triggers"></a>Hinweise zu DDL-Triggern  
DDL-Trigger starten, ebenso wie Standardtrigger, gespeicherte Prozeduren als Reaktion auf ein Ereignis. Im Gegensatz zu Standardtriggern führen sie diese jedoch nicht als Reaktion auf UPDATE-, INSERT- oder DELETE-Anweisungen für eine Tabelle oder Sicht aus. Stattdessen führen sie sie primär als Reaktion auf DDL-Anweisungen (Data Definition Language, Datendefinitionssprache) aus. Zu den Anweisungstypen zählen CREATE, ALTER, DROP, GRANT, DENY, REVOKE und UPDATE STATISTICS. Bestimmte gespeicherte Systemprozeduren, die DDL-ähnliche Vorgänge ausführen, können ebenfalls DDL-Trigger auslösen.  
  
> [!IMPORTANT]  
>  Testen Sie die verwendeten DDL-Trigger, um ihre Antworten auf die Ausführung von gespeicherten Systemprozeduren zu ermitteln. So lösen beispielsweise die CREATE TYPE-Anweisung und die gespeicherten Prozeduren „sp_addtype“ und „sp_rename“ einen DDL-Trigger aus, der für ein CREATE_TYPE-Ereignis erstellt wird.  
  
Weitere Informationen zu DDL-Triggern finden Sie unter [DDL-Trigger](../../relational-databases/triggers/ddl-triggers.md).  
  
DDL-Trigger werden nicht als Antwort auf Ereignisse ausgelöst, die sich auf lokale oder globale temporäre Tabellen und gespeicherte Prozeduren auswirken.  
  
Im Gegensatz zu DML-Triggern haben DDL-Trigger keinen Schemabereich. Aus diesem Grund können Sie Funktionen wie OBJECT_ID, OBJECT_NAME, OBJECTPROPERTY und OBJECTPROPERTYEX nicht verwenden, um Metadaten aus DDL-Triggern abzufragen. Verwenden Sie stattdessen die Katalogsichten. Weitere Informationen finden Sie unter [Abrufen von Informationen zu DDL-Triggern](../../relational-databases/triggers/get-information-about-ddl-triggers.md).  
  
> [!NOTE]  
>  DDL-Trigger mit Serverbereich werden im Ordner **Trigger** vom [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]-Objekt-Explorer angezeigt. Dieser Ordner befindet sich unter dem Ordner **Serverobjekte** . Datenbankbezogene DDL-Trigger werden im Ordner **Datenbanktrigger** angezeigt. Dieser Ordner befindet sich unter dem Ordner **Programmierbarkeit** der entsprechenden Datenbank.  
  
## <a name="logon-triggers"></a>Logon-Trigger  
Logon-Trigger führen gespeicherte Prozeduren als Antwort auf ein LOGON-Ereignis aus. Dieses Ereignis findet statt, wenn eine Benutzersitzung mit einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] hergestellt wird. Logon-Trigger werden ausgelöst, nachdem die Authentifizierungsphase der Anmeldung abgeschlossen ist, aber bevor die Benutzersitzung hergestellt wird. Aus diesem Grund werden alle Meldungen, die aus dem Trigger stammen und normalerweise den Benutzer erreichen (z.B. Fehlermeldungen und Meldungen aus der PRINT-Anweisung) zum [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Fehlerprotokoll umgeleitet. Weitere Informationen finden Sie unter [Logon-Trigger](../../relational-databases/triggers/logon-triggers.md).  
  
Logon-Trigger werden nicht ausgelöst, wenn die Authentifizierung nicht ausgeführt werden kann.  
  
Verteilte Transaktionen werden in einem Logon-Trigger nicht unterstützt. Fehler 3969 wird zurückgegeben, wenn ein Logon-Trigger ausgelöst wird, der eine verteilte Transaktion enthält.  
  
### <a name="disabling-a-logon-trigger"></a>Deaktivieren eines Logon-Triggers  
Ein Logon-Trigger kann effektiv erfolgreiche Verbindungen zu [!INCLUDE[ssDE](../../includes/ssde-md.md)] für alle Benutzer verhindern, einschließlich Elementen der festen Serverrolle **sysadmin** . Wenn ein LOGON-Trigger Verbindungen verhindert, können die Mitglieder der festen Serverrolle **sysadmin** über die dedizierte Administratorverbindung eine Verbindung herstellen oder durch Starten des [!INCLUDE[ssDE](../../includes/ssde-md.md)] s im minimalen Konfigurationsmodus (-f). Weitere Informationen finden Sie unter [Startoptionen für den Datenbank-Engine-Dienst](../../database-engine/configure-windows/database-engine-service-startup-options.md).  
  
## <a name="general-trigger-considerations"></a>Allgemeine Überlegungen zu Triggern  
  
### <a name="returning-results"></a>Zurückgeben von Ergebnissen  
Die Möglichkeit, Ergebnisse aus Triggern zurückzugeben, wird in einer künftigen Version von SQL Server entfernt. Durch Trigger, die Resultsets zurückgeben, kann es in Anwendungen, die hierfür nicht konzipiert wurden, zu unerwartetem Verhalten kommen. Vermeiden Sie deshalb bei Neuentwicklungen, Resultsets aus Triggern zurückzugeben, und planen Sie die Änderung von Anwendungen, in denen dies derzeit geschieht. Legen Sie die Option [Ergebnisse von Triggern nicht zulassen](../../database-engine/configure-windows/disallow-results-from-triggers-server-configuration-option.md) auf 1 fest, um zu verhindern, dass Trigger Resultsets zurückgeben.  
  
Logon-Trigger lassen nie zu, dass Resultsets zurückgegeben werden. Dieses Verhalten ist nicht konfigurierbar. Wenn ein Logon-Trigger ein Resultset generiert, kann der Trigger nicht gestartet werden, und der Anmeldeversuch, der den Trigger ausgelöst hat, wird abgelehnt.  
  
### <a name="multiple-triggers"></a>Mehrere Trigger  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ermöglicht Ihnen das Erstellen mehrerer Trigger für jedes DML-, DDL- oder LOGON-Ereignis. Wenn zum Beispiel CREATE TRIGGER FOR UPDATE für eine Tabelle ausgeführt wird, die bereits über einen UPDATE-Trigger verfügt, wird ein zusätzlicher UPDATE-Trigger erstellt. In früheren Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] war pro Tabelle nur ein Trigger für jedes INSERT-, UPDATE- oder DELETE-Datenänderungsereignis zulässig.  
  
### <a name="recursive-triggers"></a>Rekursive Trigger  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unterstützt außerdem den rekursiven Aufruf von Triggern, wenn die RECURSIVE_TRIGGERS-Einstellung mithilfe von ALTER DATABASE aktiviert wurde.  
  
Rekursive Trigger ermöglichen die folgenden Arten von Rekursion:  
  
-   Indirekte Rekursion  
  
     Bei der indirekten Rekursion aktualisiert eine Anwendung die T1-Tabelle. Dadurch wird der TR1-Trigger ausgelöst, der die T2-Tabelle aktualisiert. Trigger TR2 löst dann Tabelle T1 aus und aktualisiert sie.  
  
-   Direkte Rekursion  
  
     Bei der direkten Rekursion aktualisiert die Anwendung Tabelle T1. Dadurch wird der TR1-Trigger ausgelöst, der die T1-Tabelle aktualisiert. Da die T1-Tabelle aktualisiert wurde, wird der TR1-Trigger erneut ausgelöst usw.  
  
Im folgenden Beispiel werden sowohl die indirekte als auch die direkte Triggerrekursion verwendet. Angenommen, für die T1-Tabelle wurden zwei Updatetrigger, TR1 und TR2, definiert. Der TR1-Trigger aktualisiert die T1-Tabelle rekursiv. Eine UPDATE-Anweisung führt TR1 und TR2 je einmal aus. Darüber hinaus löst der Start von TR1 die Ausführung von TR1 (rekursiv) und TR2 aus. Die inserted- und die deleted-Tabelle für einen bestimmten Trigger enthalten Zeilen, die nur der UPDATE-Anweisung entsprechen, die den Trigger aufgerufen hat.  
  
> [!NOTE]  
>  Das obige Verhalten tritt nur dann ein, wenn die RECURSIVE_TRIGGERS-Einstellung mithilfe von ALTER DATABASE aktiviert wurde. Es gibt keine vorgeschriebene Reihenfolge für die Ausführung mehrerer, für ein bestimmtes Ereignis definierter Trigger. Jeder Trigger sollte unabhängig sein.  
  
Durch Deaktivierung der RECURSIVE_TRIGGERS-Einstellung werden nur die direkten Rekursionen verhindert. Um die indirekte Rekursion zu deaktivieren, legen Sie die Serveroption Geschachtelte Trigger mithilfe von sp_configure auf 0 fest.  
  
Führt einer der Trigger eine ROLLBACK TRANSACTION-Anweisung aus, werden unabhängig von der Schachtelungsebene keine weiteren Trigger ausgeführt.  
  
### <a name="nested-triggers"></a>Geschachtelte Trigger  
Sie können Trigger auf ein Maximum von 32 Ebenen schachteln. Falls ein Trigger eine Tabelle ändert, für die es einen anderen Trigger gibt, wird der zweite Trigger aktiviert und kann dann seinerseits einen dritten Trigger aufrufen usw. Wenn ein Trigger in der Kette eine Endlosschleife auslöst, wird die zulässige Schachtelungsebenenzahl überschritten und der Trigger abgebrochen. Wenn ein [!INCLUDE[tsql](../../includes/tsql-md.md)]-Trigger verwalteten Code startet, indem er auf eine CLR-Routine, einen -Typ oder ein -Aggregat verweist, zählt dieser Verweis als eine Ebene der auf 32 begrenzten Schachtelungsebenen. Methoden, die aus verwaltetem Code aufgerufen werden, werden nicht mitgezählt.  
  
Um geschachtelte Trigger zu deaktivieren, legen Sie die Option "Geschachtelte Trigger" von "sp_configure" auf 0 (deaktiviert) fest. Die Standardkonfiguration unterstützt geschachtelte Trigger. Wenn geschachtelte Trigger deaktiviert wurden, sind rekursive Trigger ebenfalls deaktiviert, unabhängig von der durch ALTER DATABASE festgelegten RECURSIVE_TRIGGERS-Einstellung.  
  
Der erste AFTER-Trigger, der in einem INSTEAD OF-Trigger geschachtelt ist, wird auch dann ausgelöst, wenn die Serverkonfigurationsoption **geschachtelte Trigger** 0 (null) ist. Bei dieser Einstellung werden jedoch nachfolgende AFTER-Trigger nicht ausgelöst. Überprüfen Sie Ihre Anwendungen auf geschachtelte Trigger, um festzustellen, ob die Anwendungen Ihren Geschäftsregeln entsprechen, wenn für die Serverkonfigurationsoption **geschachtelte Trigger** 0 (null) festgelegt ist. Wenn dies nicht der Fall ist, nehmen Sie die entsprechenden Änderungen vor.  
  
### <a name="deferred-name-resolution"></a>Verzögerte Namensauflösung  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lässt zu, dass gespeicherte [!INCLUDE[tsql](../../includes/tsql-md.md)]-Prozeduren, -Trigger und -Batches auf Tabellen verweisen, die zur Kompilierzeit noch nicht vorhanden sind. Diese Option wird verzögerte Namensauflösung genannt.  
  
## <a name="permissions"></a>Berechtigungen  
Zum Erstellen eines DML-Triggers ist die ALTER-Berechtigung für die Tabelle oder Sicht erforderlich, für die der Trigger erstellt wird.  
  
Zum Erstellen eines DDL-Triggers mit einem Serverbereich (ON ALL SERVER) oder eines LOGON-Triggers ist die CONTROL SERVER-Berechtigung auf dem Server erforderlich. Zum Erstellen eines DDL-Triggers mit Datenbankbereich (ON DATABASE) ist die ALTER ANY DATABASE DDL TRIGGER-Berechtigung in der aktuellen Datenbank erforderlich.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-using-a-dml-trigger-with-a-reminder-message"></a>A. Verwenden eines DML-Triggers mit einer Erinnerungsmeldung  
Der folgende DML-Trigger gibt jedes Mal, wenn jemand versucht, Daten in die `Customer`-Tabelle in der [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]-Datenbank einzufügen bzw. vorhandene Daten zu ändern, eine Meldung an den Client aus.  
  
```sql  
CREATE TRIGGER reminder1  
ON Sales.Customer  
AFTER INSERT, UPDATE   
AS RAISERROR ('Notify Customer Relations', 16, 10);  
GO  
```  
  
### <a name="b-using-a-dml-trigger-with-a-reminder-e-mail-message"></a>B. Verwenden eines DML-Triggers mit einer E-Mail-Erinnerungsnachricht  
Im folgenden Beispiel wird eine E-Mail-Nachricht an die angegebene Person (`MaryM`) gesendet, wenn die `Customer`-Tabelle geändert wird.  
  
```sql  
CREATE TRIGGER reminder2  
ON Sales.Customer  
AFTER INSERT, UPDATE, DELETE   
AS  
   EXEC msdb.dbo.sp_send_dbmail  
        @profile_name = 'AdventureWorks2012 Administrator',  
        @recipients = 'danw@Adventure-Works.com',  
        @body = 'Don''t forget to print a report for the sales force.',  
        @subject = 'Reminder';  
GO  
```  
  
### <a name="c-using-a-dml-after-trigger-to-enforce-a-business-rule-between-the-purchaseorderheader-and-vendor-tables"></a>C. Verwenden eines DML-AFTER-Triggers zum Erzwingen einer Geschäftsregel zwischen den Tabellen PurchaseOrderHeader und Vendor  
Da sich CHECK-Einschränkungen nur auf Spalten beziehen, für die die Einschränkung auf Spalten- oder Tabellenebene definiert wurden, müssen Sie tabellenübergreifende Einschränkungen (in diesem Fall Geschäftsregeln) als Trigger definieren.  
  
Im folgenden Beispiel wird ein DML-Trigger in der AdventureWorks2012-Beispieldatenbank erstellt. Bei dem Versuch, eine neue Bestellung in die Tabelle `PurchaseOrderHeader` einzufügen, überprüft der Trigger, ob die Bonität eines Herstellers gut ist (nicht 5). Um die Bonität des Herstellers zu ermitteln, muss auf die `Vendor`-Tabelle verwiesen werden. Ist die Bonität zu niedrig, wird eine Meldung angezeigt, und die Bestellung wird nicht durchgeführt.  
  
```sql  
-- This trigger prevents a row from being inserted in the Purchasing.PurchaseOrderHeader 
-- table when the credit rating of the specified vendor is set to 5 (below average).  
  
CREATE TRIGGER Purchasing.LowCredit ON Purchasing.PurchaseOrderHeader  
AFTER INSERT  
AS  
IF (ROWCOUNT_BIG() = 0)
RETURN;
IF EXISTS (SELECT *  
           FROM Purchasing.PurchaseOrderHeader AS p   
           JOIN inserted AS i   
           ON p.PurchaseOrderID = i.PurchaseOrderID   
           JOIN Purchasing.Vendor AS v   
           ON v.BusinessEntityID = p.VendorID  
           WHERE v.CreditRating = 5  
          )  
BEGIN  
RAISERROR ('A vendor''s credit rating is too low to accept new  
purchase orders.', 16, 1);  
ROLLBACK TRANSACTION;  
RETURN   
END;  
GO  
  
-- This statement attempts to insert a row into the PurchaseOrderHeader table  
-- for a vendor that has a below average credit rating.  
-- The AFTER INSERT trigger is fired and the INSERT transaction is rolled back.  
  
INSERT INTO Purchasing.PurchaseOrderHeader (RevisionNumber, Status, EmployeeID,  
VendorID, ShipMethodID, OrderDate, ShipDate, SubTotal, TaxAmt, Freight)  
VALUES (  
2  
,3  
,261  
,1652  
,4  
,GETDATE()  
,GETDATE()  
,44594.55  
,3567.564  
,1114.8638 );  
GO  
  
```  
  
### <a name="d-using-a-database-scoped-ddl-trigger"></a>D: Verwenden eines DDL-Triggers mit Datenbankbereich  
Im folgenden Beispiel wird ein DDL-Trigger verwendet, um zu verhindern, dass ein Synonym aus einer Datenbank gelöscht wird.  
  
```sql  
CREATE TRIGGER safety   
ON DATABASE   
FOR DROP_SYNONYM  
AS   
IF (@@ROWCOUNT = 0)
RETURN;
   RAISERROR ('You must disable Trigger "safety" to remove synonyms!', 10, 1)  
   ROLLBACK  
GO  
DROP TRIGGER safety  
ON DATABASE;  
GO  
```  
  
### <a name="e-using-a-server-scoped-ddl-trigger"></a>E. Verwenden eines DDL-Triggers mit Serverbereich  
Im folgenden Beispiel wird ein DDL-Trigger verwendet, um eine Meldung auszugeben, wenn ein CREATE DATABASE-Ereignis auf der aktuellen Serverinstanz eintritt. Mithilfe der `EVENTDATA`-Funktion wird der Text der entsprechenden [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung abgerufen. Weitere Beispiele für die Verwendung von EVENTDATA in DDL-Triggern finden Sie unter [Verwenden der EVENTDATA-Funktion](../../relational-databases/triggers/use-the-eventdata-function.md).  
  
**Gilt für**:  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.  
  
```sql  
CREATE TRIGGER ddl_trig_database   
ON ALL SERVER   
FOR CREATE_DATABASE   
AS   
    PRINT 'Database Created.'  
    SELECT EVENTDATA().value('(/EVENT_INSTANCE/TSQLCommand/CommandText)[1]','nvarchar(max)')  
GO  
DROP TRIGGER ddl_trig_database  
ON ALL SERVER;  
GO  
```  
  
### <a name="f-using-a-logon-trigger"></a>F. Verwenden eines LOGON-Triggers  
Im folgenden Beispiel für LOGON-Trigger wird ein Anmeldeversuch bei [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] als Mitglied des Anmeldenamens *login_test* abgewiesen, wenn unter diesem Anmeldenamen bereits drei Benutzersitzungen ausgeführt werden.  
  
**Gilt für**:  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.  
  
```sql  
USE master;  
GO  
CREATE LOGIN login_test WITH PASSWORD = '3KHJ6dhx(0xVYsdf' MUST_CHANGE,  
    CHECK_EXPIRATION = ON;  
GO  
GRANT VIEW SERVER STATE TO login_test;  
GO  
CREATE TRIGGER connection_limit_trigger  
ON ALL SERVER WITH EXECUTE AS 'login_test'  
FOR LOGON  
AS  
BEGIN  
IF ORIGINAL_LOGIN()= 'login_test' AND  
    (SELECT COUNT(*) FROM sys.dm_exec_sessions  
            WHERE is_user_process = 1 AND  
                original_login_name = 'login_test') > 3  
    ROLLBACK;  
END;  
  
```  
  
### <a name="g-viewing-the-events-that-cause-a-trigger-to-fire"></a>G. Anzeigen der Ereignisse, die einen Trigger auslösen  
Im folgenden Beispiel werden die `sys.triggers`- und die `sys.trigger_events`-Katalogsichten abgefragt, um zu ermitteln, welche [!INCLUDE[tsql](../../includes/tsql-md.md)]-Sprachereignisse bewirken, dass der `safety`-Trigger ausgelöst wird. Der Trigger `safety` wird in Beispiel „D“ erstellt, wie oben dargestellt.  
  
```sql  
SELECT TE.*  
FROM sys.trigger_events AS TE  
JOIN sys.triggers AS T ON T.object_id = TE.object_id  
WHERE T.parent_class = 0 AND T.name = 'safety';  
GO  
```  

    

## <a name="see-also"></a>Weitere Informationen  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [ALTER TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/alter-trigger-transact-sql.md)   
 [COLUMNS_UPDATED &#40;Transact-SQL&#41;](../../t-sql/functions/columns-updated-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [DROP TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-trigger-transact-sql.md)   
 [ENABLE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/enable-trigger-transact-sql.md)   
 [DISABLE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/disable-trigger-transact-sql.md)   
 [TRIGGER_NESTLEVEL &#40;Transact-SQL&#41;](../../t-sql/functions/trigger-nestlevel-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.dm_sql_referenced_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md)   
 [sys.dm_sql_referencing_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md)   
 [sys.sql_expression_dependencies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)   
 [sp_help &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)   
 [sp_helptrigger &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptrigger-transact-sql.md)   
 [sp_helptext &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md)   
 [sp_rename &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)   
 [sp_settriggerorder &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-settriggerorder-transact-sql.md)   
 [UPDATE&#40;&#41; &#40;Transact-SQL&#41;](../../t-sql/functions/update-trigger-functions-transact-sql.md)   
 [Abrufen von Informationen zu DML-Triggern](../../relational-databases/triggers/get-information-about-dml-triggers.md)   
 [Abrufen von Informationen zu DDL-Triggern](../../relational-databases/triggers/get-information-about-ddl-triggers.md)   
 [sys.triggers &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md)   
 [sys.trigger_events &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-trigger-events-transact-sql.md)   
 [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [sys.assembly_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-assembly-modules-transact-sql.md)   
 [sys.server_triggers &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-triggers-transact-sql.md)   
 [sys.server_trigger_events &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-trigger-events-transact-sql.md)   
 [sys.server_sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-sql-modules-transact-sql.md)   
 [sys.server_assembly_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-assembly-modules-transact-sql.md)  
  
  


