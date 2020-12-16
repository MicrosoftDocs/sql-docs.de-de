---
description: CREATE DIAGNOSTICS SESSION (Transact-SQL)
title: CREATE DIAGNOSTICS SESSION (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 662d019e-f217-49df-9e2f-b5662fa0342d
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016'
ms.openlocfilehash: 840adb3ae36e03e73106b2c2536fcba978dc78d4
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97462341"
---
# <a name="create-diagnostics-session-transact-sql"></a>CREATE DIAGNOSTICS SESSION (Transact-SQL)
[!INCLUDE [pdw](../../includes/applies-to-version/pdw.md)]

  Mithilfe von Diagnosesitzungen können Sie detaillierte benutzerdefinierte Diagnoseinformationen zur System- oder Abfrageleistung speichern.  
  
 Diagnosesitzungen werden in der Regel dazu verwendet, die Leistung einer bestimmten Abfrage zu debuggen oder das Verhalten einer bestimmten Appliancekomponente während des Appliancevorgangs zu überwachen.  
  
> [!NOTE]  
>  Sie sollten mit XML vertraut sein, um Diagnosesitzungen zu verwenden.  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
-- Creating a new diagnostics session:  
CREATE DIAGNOSTICS SESSION diagnostics_name AS N'{<session_xml>}';  
  
<session_xml>::  
<Session>  
   [ <MaxItemCount>max_item_count_num</MaxItemCount> ]  
   <Filter>  
      { \<Event Name="event_name"/>  
         [ <Where>\<filter_property_name Name="value" ComparisonType="comp_type"/></Where> ] [ ,...n ]  
      } [ ,...n ]  
   </Filter> ]   
   <Capture>  
      \<Property Name="property_name"/> [ ,...n ]  
   </Capture>  
<Session>  
  
-- Retrieving results for a diagnostics session:  
SELECT * FROM master.sysdiag.diagnostics_name ;  
  
-- Removing results for a diagnostics session:  
DROP DIAGNOSTICS SESSION diagnostics_name ;  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *diagnostics_name*  
 Der Name der Diagnosesitzung. Namen von Diagnosesitzungen dürfen nur die Zeichen a-z, A-Z und die Zahlen 0-9 enthalten. Darüber hinaus müssen Namen von Diagnosesitzungen mit einem Zeichen beginnen. *diagnostics_name* ist auf 127 Zeichen beschränkt.  
  
 *max_item_count_num*  
 Die Anzahl der zu persistierenden Ereignisse in einer Sicht. Wenn beispielsweise 100 angegeben wird, werden die 100 aktuellsten Ereignisse, die den Filterkriterien entsprechen, in der Diagnosesitzung persistiert. Wenn weniger als 100 übereinstimmende Ereignisse gefunden werden, enthält die Diagnosesitzung weniger als 100 Ereignisse. *max_item_count_num* muss mindestens einen Wert von 100 und darf maximal den Wert 100.000 aufweisen.  
  
 *event_name*  
 Definiert die tatsächlichen Ereignisse, die in der Diagnosesitzung gesammelt werden sollen.  *event_name* ist eines der Ereignisse, die in [sys.pdw_diag_events](../../relational-databases/system-catalog-views/sys-pdw-diag-events-transact-sql.md) aufgelistet sind, wobei Folgendes gilt: `sys.pdw_diag_events.is_enabled='True'`.  
  
 *filter_property_name*  
 Der Name der Eigenschaft, auf die Ergebnisse beschränkt werden sollen. Wenn Sie beispielsweise eine Einschränkung basierend auf der Sitzungs-ID vornehmen möchten, muss für *filter_property_name**SessionId* angegeben werden. Eine Liste mit möglichen Werten für *filter_property_name* finden Sie unter *property_name* weiter unten.  
  
 *value*  
 Ein Wert, der für *filter_property_name* ausgewertet werden soll. Der Werttyp muss mit dem Eigenschaftentyp übereinstimmen. Wenn beispielsweise der Eigenschaftentyp „decimal“ ist, muss der Typ von *value* „decimal“ sein.  
  
 *comp_type*  
 Der Vergleichstyp. Mögliche Werte sind: Equals, EqualsOrGreaterThan, EqualsOrLessThan, GreaterThan, LessThan, NotEquals, Contains, RegEx  
  
 *property_name*  
 Eine Eigenschaft, die mit dem Ereignis verknüpft ist.  Eigenschaftsnamen können Teil des Capture-Tags sein oder als Teil der Filterkriterien verwendet werden.  
  
|Eigenschaftenname|BESCHREIBUNG|  
|-------------------|-----------------|  
|UserName|Ein Benutzer(anmelde)name.|  
|SessionID|Eine Sitzungs-ID.|  
|QueryId|Eine Abfrage-ID.|  
|CommandType|Ein Befehlstyp.|  
|CommandText|Text innerhalb eines verarbeiteten Befehls.|  
|OperationType|Der Vorgangstyp für das Ereignis.|  
|Duration|Die Dauer des Ereignisses.|  
|SPID|Die Prozess-ID des Diensts.|  
  
## <a name="remarks"></a>Bemerkungen  
 Pro Benutzer sind maximal 10 Diagnosesitzungen gleichzeitig erlaubt. Eine Liste Ihrer aktuellen Sitzungen finden Sie unter [sys.pdw_diag_sessions](../../relational-databases/system-catalog-views/sys-pdw-diag-sessions-transact-sql.md). Dort können Sie nicht benötigte Sitzungen mithilfe von `DROP DIAGNOSTICS SESSION` löschen.  
  
 Diagnosesitzungen sammeln weiter Metadaten, bis sie gelöscht werden.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die **ALTER SERVER STATE**-Berechtigung.  
  
## <a name="locking"></a>Sperren  
 Ruft eine gemeinsame Sperre für die Tabelle der Diagnosesitzungen ab.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-creating-a-diagnostics-session"></a>A. Erstellen einer Diagnosesitzung  
 In diesem Beispiel wird eine Diagnosesitzung erstellt, um Statistiken zur Leistung der Datenbank-Engine aufzuzeichnen. Im Beispiel wird eine Diagnosesitzung erstellt, die auf Ausführungs- und Endereignisse von Engine-Abfragen sowie ein blockierendes DMS-Ereignis achtet. Zurückgegeben werden der Befehlstext, der Computername, die Anforderungs-ID (Abfrage-ID) und die Sitzung, in der das Ereignis erstellt wurde.  
  
```sql  
CREATE DIAGNOSTICS SESSION MYDIAGSESSION AS N'  
<Session>  
   <MaxItemCount>100</MaxItemCount>  
   <Filter>  
      <Event Name="EngineInstrumentation:EngineQueryRunningEvent" />  
      <Event Name="DmsCoreInstrumentation:DmsBlockingQueueEnqueueBeginEvent" />  
      <Where>  
         <SessionId Value="381" ComparisonType="NotEquals" />  
      </Where>  
   </Filter>  
   <Capture>  
      <Property Name="Query.CommandText" />  
      <Property Name="MachineName" />  
      <Property Name="Query.QueryId" />  
      <Property Name="Alias" />  
      <Property Name="Duration" />  
      <Property Name="Session.SessionId" />  
   </Capture>  
</Session>';  
```  
  
 Führen Sie nach dem Erstellen der Diagnosesitzung eine Abfrage aus.  
  
```sql  
SELECT COUNT(EmployeeKey) FROM AdventureWorksPDW2012..FactSalesQuota;  
```  
  
 Zeigen Sie dann die Ergebnisse der Diagnosesitzung an, indem Sie diese über das sysdiag-Schema auswählen.  
  
```sql  
SELECT * FROM master.sysdiag.MYDIAGSESSION;  
```  
  
 Beachten Sie, dass das sysdiag-Schema eine Sicht mit dem Namen Ihrer Diagnosesitzung enthält.  
  
 Um nur die Aktivität für Ihre Verbindung anzuzeigen, fügen Sie die `Session.SPID`-Eigenschaft hinzu, und fügen Sie `WHERE [Session.SPID] = @@spid;` zur Abfrage hinzu.  
  
 Wenn Sie mit der Diagnosesitzung fertig sind, löschen Sie diese mithilfe des Befehls **DROP DIAGNOSTICS**.  
  
```sql  
DROP DIAGNOSTICS SESSION MYDIAGSESSION;  
```  
  
### <a name="b-alternative-diagnostic-session"></a>B. Alternative Diagnosesitzung  
 Ein zweites Beispiel mit etwas anderen Eigenschaften.  
  
```sql  
-- Determine the session_id of your current session  
SELECT TOP 1 session_id();  
-- Replace \<*session_number*> in the code below with the numbers in your session_id  
CREATE DIAGNOSTICS SESSION PdwOptimizationDiagnostics AS N'  
<Session>  
   <MaxItemCount>100</MaxItemCount>  
   <Filter>  
      <Event Name="EngineInstrumentation:MemoGenerationBeginEvent" />  
      <Event Name="EngineInstrumentation:MemoGenerationEndEvent" />  
      <Event Name="DSQLInstrumentation:OptimizationBeginEvent" />  
      <Event Name="DSQLInstrumentation:OptimizationEndEvent" />  
      <Event Name="DSQLInstrumentation:BuildRelOpContextTreeBeginEvent" />  
      <Event Name="DSQLInstrumentation:PostPlanGenModifiersEndEvent" />  
      <Where>  
         <SessionId Value="\<*session_number*>" ComparisonType="Equals" />  
      </Where>  
   </Filter>  
   <Capture>  
      <Property Name="Session.SessionId" />  
      <Property Name="Query.QueryId" />  
      <Property Name="Query.CommandText" />  
      <Property Name="Name" />  
      <Property Name="DateTimePublished" />  
      <Property Name="DateTimePublished.Ticks" />  
  </Capture>  
</Session>';  
```  
  
 Führen Sie eine Abfrage wie z.B folgende aus:  
  
```sql  
USE ssawPDW;  
GO  
SELECT * FROM dbo.FactFinance;  
```  
  
 Die folgende Abfrage gibt die Autorisierungszeitsteuerung zurück:  
  
```sql  
SELECT *   
FROM master.sysdiag.PdwOptimizationDiagnostics   
ORDER BY DateTimePublished;  
```  
  
 Wenn Sie mit der Diagnosesitzung fertig sind, löschen Sie diese mithilfe des Befehls **DROP DIAGNOSTICS**.  
  
```sql  
DROP DIAGNOSTICS SESSION PdwOptimizationDiagnostics;  
```  
  
  
