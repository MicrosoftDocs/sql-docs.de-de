---
description: CREATE QUEUE (Transact-SQL)
title: CREATE QUEUE (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2021
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- QUEUE_TSQL
- CREATE QUEUE
- QUEUE
- CREATE_QUEUE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE QUEUE statement
- internal activation [Service Broker]
- stored procedure activation [Service Broker]
- message retention [Service Broker]
- unavailable queues [Service Broker]
- activation stored procedures [Service Broker]
- queues [Service Broker], creating
ms.assetid: fce80faf-2bdc-475d-8ca1-31438ed41fb0
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 12bedec8dcd525767bc54eb3b1af221a17c658fa
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464946"
---
# <a name="create-queue-transact-sql"></a>CREATE QUEUE (Transact-SQL)

[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

Erstellt eine neue Warteschlange in einer Datenbank. Fügt einer Warteschlange gespeicherte Nachrichten hinzu. Wenn eine Nachricht für einen Dienst eintrifft, platziert [!INCLUDE[ssSB](../../includes/sssb-md.md)] die Nachricht in der dem Dienst zugeordneten Warteschlange.

![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Syntax

```syntaxsql
CREATE QUEUE <object>
   [ WITH
     [ STATUS = { ON | OFF } [ , ] ]
     [ RETENTION = { ON | OFF } [ , ] ]
     [ ACTIVATION (
         [ STATUS = { ON | OFF } , ]
           PROCEDURE_NAME = <procedure> ,
           MAX_QUEUE_READERS = max_readers ,
           EXECUTE AS { SELF | 'user_name' | OWNER }
            ) [ , ] ]
     [ POISON_MESSAGE_HANDLING (
         [ STATUS = { ON | OFF } ] ) ]
    ]
     [ ON { filegroup | [ DEFAULT ] } ]
[ ; ]

<object> ::=
{ database_name.schema_name.queue_name | schema_name.queue_name | queue_name }

<procedure> ::=
{ database_name.schema_name.stored_procedure_name | schema_name.stored_procedure_name | stored_procedure_name }

```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente

*database_name* (Objekt) ist der Name der Datenbank, in der die neue Warteschlange erstellt werden soll. *database_name* muss dem Namen einer vorhandenen Datenbank entsprechen. Wird *database_name* nicht bereitgestellt, wird die Warteschlange in der aktuellen Datenbank erstellt.

*schema_name* (Objekt) ist der Name des Schemas, zu dem die neue Warteschlange gehört. Standardmäßig handelt es sich bei dem Schema um das Standardschema für den Benutzer, der die Anweisung ausführt. Wird die CREATE QUEUE-Anweisung von einem Mitglied der festen Serverrolle sysadmin oder einem Mitglied der festen Datenbankrollen db_dbowner oder db_ddladmin in der durch *database_name* angegebenen Datenbank ausgeführt, kann *schema_name* ein anderes als das dem Anmeldenamen der aktuellen Verbindung zugeordnete Schema angeben. Andernfalls muss es sich bei *schema_name* um das Standardschema für den Benutzer handeln, der die Anweisung ausführt.

*queue_name* ist der Name der zu erstellenden Warteschlange. Dieser Name muss den Richtlinien für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Bezeichner entsprechen.

STATUS (Warteschlange) gibt an, ob die Warteschlange verfügbar ist (ON) oder nicht (OFF). Ist die Warteschlange nicht verfügbar, können der Warteschlange keine Nachrichten hinzugefügt oder aus ihr entfernt werden. Sie können die Warteschlange im nicht verfügbaren Status erstellen, damit Nachrichten erst dann in der Warteschlange ankommen, wenn die Warteschlange mit einer ALTER QUEUE-Anweisung zur Verfügung gestellt wird. Wird diese Klausel nicht angegeben, ist die Standardeinstellung ON, und die Warteschlange ist verfügbar.

RETENTION gibt die Aufbewahrungseinstellung für die Warteschlange an. Ist RETENTION = ON, werden alle Nachrichten, die für Konversationen mit dieser Warteschlange gesendet oder empfangen werden, in der Warteschlange beibehalten, bis die Konversationen beendet sind. Dies ermöglicht es Ihnen, Nachrichten zu Überwachungszwecken oder zur Ausführung von kompensierenden Transaktionen beim Auftreten eines Fehlers beizubehalten. Wird diese Klausel nicht angegeben, wird die Beibehaltungseinstellung standardmäßig auf OFF festgelegt.

> [!NOTE]
> Das Festlegen von RETENTION = ON kann die Leistung reduzieren. Diese Einstellung sollte nur verwendet werden, wenn sie für die Anwendung erforderlich ist.

ACTIVATION gibt Informationen über die gespeicherte Prozedur an, die Sie für die Verarbeitung von Nachrichten in dieser Warteschlange starten müssen.

STATUS (Aktivierung) gibt an, ob [!INCLUDE[ssSB](../../includes/sssb-md.md)] die gespeicherte Prozedur startet. Ist STATUS = ON, startet die Warteschlange die mit PROCEDURE_NAME angegebene gespeicherte Prozedur, wenn die Anzahl der zurzeit ausgeführten Prozeduren kleiner als MAX_QUEUE_READERS ist und wenn Nachrichten schneller in der Warteschlange ankommen, als die gespeicherten Prozeduren Nachrichten empfangen. Ist STATUS = OFF, startet die Warteschlange die gespeicherte Prozedur nicht. Wird diese Klausel nicht angegeben, ist die Standardeinstellung ON.

PROCEDURE_NAME = \<procedure> Gibt den Namen der gespeicherten Prozedur an, die für die Verarbeitung von Nachrichten in dieser Warteschlange gestartet werden soll. Dieser Wert muss ein [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Bezeichner sein.

*database_name*(Prozedur) ist der Name der Datenbank, die die gespeicherte Prozedur enthält.

*schema_name*(Prozedur) ist der Name des Schemas, das die gespeicherte Prozedur enthält.

*procedure_name* ist der Name der gespeicherten Prozedur.

MAX_QUEUE_READERS =*max_readers* gibt die maximale Anzahl von Instanzen der gespeicherten Aktivierungsprozedur an, die von der Warteschlange gleichzeitig gestartet werden. Der Wert von *max_readers* muss eine Zahl zwischen **0** und **32767** sein.

EXECUTE AS gibt das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbank-Benutzerkonto an, unter dem die gespeicherte Aktivierungsprozedur ausgeführt wird. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] muss zum Zeitpunkt des Startens der gespeicherten Prozedur durch die Warteschlange die Berechtigungen für diesen Benutzer überprüfen können. Bei einem Domänenbenutzer muss der Server mit der Domäne verbunden sein, wenn die Prozedur gestartet wird. Andernfalls erzeugt die Aktivierung einen Fehler. Bei einem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Benutzer kann der Server immer die Berechtigungen überprüfen.

SELF gibt an, dass die gespeicherte Prozedur als der aktuelle Benutzer ausgeführt wird. (Der Datenbankprinzipal, der diese CREATE QUEUE-Anweisung ausführt.)

'*user_name*' ist der Name des Benutzers, als der die gespeicherte Prozedur ausgeführt wird. Der *user_name*-Parameter muss ein gültiger [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Benutzer sein, der als [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Bezeichner angegeben wird. Der aktuelle Benutzer muss über die IMPERSONATE-Berechtigung für den mit *user_name* angegebenen Benutzer verfügen.

OWNER gibt an, dass die gespeicherte Prozedur als der Besitzer der Warteschlange ausgeführt wird.

POISON_MESSAGE_HANDLING gibt an, ob die Behandlung nicht verarbeitbarer Nachrichten für die Warteschlange aktiviert ist. Der Standardwert ist ON.

Eine Warteschlange, für die die Behandlung nicht verarbeitbarer Nachrichten auf OFF festgelegt ist, wird erst nach fünf aufeinander folgenden Transaktionsrollbacks deaktiviert. Daher ist es möglich, dass von der Anwendung ein System für die Behandlung nicht verarbeitbarer Nachrichten definiert wird.

ON *Dateigruppe |* [**DEFAULT**] gibt die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Dateigruppe an, in der diese Warteschlange erstellt werden soll. Sie können mit dem *filegroup*-Parameter eine Dateigruppe identifizieren oder mit dem DEFAULT-Bezeichner die Standarddateigruppe für die Service Broker-Datenbank verwenden. Im Kontext dieser Klausel ist DEFAULT kein Schlüsselwort und muss als Bezeichner begrenzt sein. Wird keine Dateigruppe angegeben, verwendet die Warteschlange die Standarddateigruppe für die Datenbank.

## <a name="remarks"></a>Bemerkungen

Eine Warteschlange kann das Ziel einer SELECT-Anweisung sein. Der Inhalt einer Warteschlange kann jedoch nur mithilfe von Anweisungen geändert werden, die für [!INCLUDE[ssSB](../../includes/sssb-md.md)]-Konversationen verwendet werden, wie beispielsweise SEND, RECEIVE und END CONVERSATION. Eine Warteschlange kann nicht das Ziel einer INSERT-, UPDATE-, DELETE- oder TRUNCATE-Anweisung sein.

Eine Warteschlange ist möglicherweise kein temporäres Objekt. Daher sind Warteschlangennamen, die mit **#** beginnen, ungültig.

Das Erstellen einer Warteschlange im inaktiven Status ermöglicht es Ihnen, die Infrastruktur für einen Dienst einzurichten, bevor der Empfang von Nachrichten in der Warteschlange zugelassen wird.

[!INCLUDE[ssSB](../../includes/sssb-md.md)] beendet gespeicherte Aktivierungsprozeduren nicht, wenn sich keine Nachrichten in der Warteschlange befinden. Eine gespeicherte Aktivierungsprozedur sollte beendet werden, wenn eine kurze Zeit lang keine Nachrichten in der Warteschlange verfügbar sind.

Berechtigungen für die gespeicherte Aktivierungsprozedur werden überprüft, wenn [!INCLUDE[ssSB](../../includes/sssb-md.md)] die gespeicherte Prozedur startet, nicht, wenn die Warteschlange erstellt wird. Die CREATE QUEUE-Anweisung überprüft nicht, ob der in der EXECUTE AS-Klausel angegebene Benutzer über die Berechtigung verfügt, die in der PROCEDURE NAME-Klausel angegebene gespeicherte Prozedur auszuführen.

Ist eine Warteschlange nicht verfügbar, speichert [!INCLUDE[ssSB](../../includes/sssb-md.md)] Nachrichten für Dienste, die die Warteschlange verwenden, in der Übertragungswarteschlange für die Datenbank. Die sys.transmission_queue-Katalogsicht stellt eine Sicht der Übertragungswarteschlange bereit.

Eine Warteschlange ist ein Objekt, dessen Besitzer ein Schema ist. Warteschlangen werden in der sys.objects-Katalogsicht angezeigt.

In der folgenden Tabelle werden die Spalten in einer Warteschlange aufgelistet.

|Spaltenname|Datentyp|BESCHREIBUNG|
|-----------------|---------------|-----------------|
|status|**tinyint**|Status der Nachricht. Die RECEIVE-Anweisung gibt alle Nachrichten zurück, die den Status **1** haben. Wenn die Nachrichtenbeibehaltung aktiviert ist, wird der Status auf 0 festgelegt. Wenn die Nachrichtenbeibehaltung deaktiviert ist, wird die Meldung aus der Warteschlange gelöscht. Nachrichten in der Warteschlange können einen der folgenden Werte enthalten:<br /><br /> **0** = Empfangene Nachricht wurde beibehalten<br /><br /> **1** = Bereit zu empfangen<br /><br /> **2** = Noch nicht abgeschlossen<br /><br /> **3** = Gesendete Nachricht wurde beibehalten|
|priority|**tinyint**|Die Prioritätsebene, die der Nachricht zugewiesen wird.|
|queuing_order|**bigint**|Fortlaufende Nummer der Nachricht in der Warteschlange.|
|conversation_group_id|**uniqueidentifier**|Bezeichner für die Konversationsgruppe, zu der diese Nachricht gehört.|
|conversation_handle|**uniqueidentifier**|Handle der Konversation, von der diese Nachricht ein Teil ist.|
|message_sequence_number|**bigint**|Sequenznummer der Nachricht in der Konversation.|
|service_name|**nvarchar(512)**|Name des Diensts, an den die Konversation gerichtet ist.|
|service_id|**int**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Objektbezeichner des Diensts, an den die Konversation gerichtet ist.|
|service_contract_name|**nvarchar(256)**|Name des Vertrags, dem die Konversation entspricht.|
|service_contract_id|**int**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Objektbezeichner des Vertrags, dem die Konversation entspricht.|
|message_type_name|**nvarchar(256)**|Name des Nachrichtentyps, der die Nachricht beschreibt.|
|message_type_id|**int**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Objektbezeichner des Nachrichtentyps, der die Nachricht beschreibt.|
|validation|**nchar(2)**|Für die Nachricht verwendete Überprüfung.<br /><br /> E=Leer<br /><br /> N = Keine<br /><br /> X=XML|
|message_body|**varbinary(max)**|Inhalt der Nachricht.|
|message_enqueue_time|**datetime**|Uhrzeit, zu der die Nachricht in die Warteschlange eingereiht wurde.|

## <a name="permissions"></a>Berechtigungen

Die Berechtigung zum Erstellen einer Warteschlange liegt bei Mitgliedern der festen Datenbankrollen `db_ddladmin` oder `db_owner` bzw. der festen Serverrolle `sysadmin`.

Die `REFERENCES`-Berechtigung für eine Warteschlange wird standardmäßig dem Besitzer der Warteschlange, den Mitgliedern der festen Datenbankrolle `db_ddladmin` oder `db_owner` bzw. den Mitgliedern der festen Serverrolle `sysadmin` zugewiesen.

`RECEIVE`-Berechtigungen für eine Warteschlange liegen standardmäßig beim Besitzer der Warteschlange, bei Mitgliedern der festen Datenbankrolle `db_owner` oder bei Mitgliedern der festen Serverrolle `sysadmin`.

## <a name="examples"></a>Beispiele

### <a name="a-creating-a-queue-with-no-parameters"></a>A. Erstellen einer Warteschlange ohne Parameter

Im folgenden Beispiel wird eine Warteschlange erstellt, die für den Empfang von Nachrichten verfügbar ist. Für die Warteschlange wird keine gespeicherte Aktivierungsprozedur angegeben.

```sql
CREATE QUEUE ExpenseQueue ;
```

### <a name="b-creating-an-unavailable-queue"></a>B. Erstellen einer nicht verfügbaren Warteschlange

Im folgenden Beispiel wird eine Warteschlange erstellt, die nicht für den Empfang von Nachrichten verfügbar ist. Für die Warteschlange wird keine gespeicherte Aktivierungsprozedur angegeben.

```sql
CREATE QUEUE ExpenseQueue WITH STATUS=OFF ;
```

### <a name="c-creating-a-queue-and-specify-internal-activation-information"></a>C. Erstellen einer Warteschlange und Angeben von internen Aktivierungsinformationen

Im folgenden Beispiel wird eine Warteschlange erstellt, die für den Empfang von Nachrichten verfügbar ist. Die Warteschlange startet die gespeicherte Prozedur `expense_procedure`, wenn eine Nachricht in der Warteschlange angeordnet wird. Die gespeicherte Prozedur wird als der Benutzer `ExpenseUser` ausgeführt. Die Warteschlange startet ein Maximum von `5` Instanzen der gespeicherten Prozedur.

```sql
CREATE QUEUE ExpenseQueue
    WITH STATUS=ON,
    ACTIVATION (
        PROCEDURE_NAME = expense_procedure
        , MAX_QUEUE_READERS = 5
        , EXECUTE AS 'ExpenseUser' ) ;
```

### <a name="d-creating-a-queue-on-a-specific-filegroup"></a>D: Erstellen einer Warteschlange in einer bestimmten Dateigruppe

Im folgenden Beispiel wird eine Warteschlange in der `ExpenseWorkFileGroup`-Dateigruppe erstellt.

```sql
CREATE QUEUE ExpenseQueue
    ON ExpenseWorkFileGroup ;
```

### <a name="e-creating-a-queue-with-multiple-parameters"></a>E. Erstellen einer Warteschlange mit mehreren Parametern

Im folgenden Beispiel wird eine Warteschlange in der `DEFAULT`-Dateigruppe erstellt. Die Warteschlange ist nicht verfügbar. Nachrichten werden in der Warteschlange beibehalten, bis die Konversation endet, zu der sie gehören. Wenn die Warteschlange über ALTER QUEUE zur Verfügung gestellt wird, startet die Warteschlange die gespeicherte Prozedur `2008R2.dbo.expense_procedure` für die Verarbeitung von Nachrichten. Die gespeicherte Prozedur wird als der Benutzer ausgeführt, der die `CREATE QUEUE`-Anweisung ausgeführt hat. Die Warteschlange startet ein Maximum von `10` Instanzen der gespeicherten Prozedur.

```sql
CREATE QUEUE ExpenseQueue
    WITH STATUS = OFF
      , RETENTION = ON
      , ACTIVATION (
          PROCEDURE_NAME = AdventureWorks2012.dbo.expense_procedure
          , MAX_QUEUE_READERS = 10
          , EXECUTE AS SELF )
    ON [DEFAULT];
```

## <a name="see-also"></a>Weitere Informationen

- [ALTER QUEUE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-queue-transact-sql.md)
- [CREATE SERVICE &#40;Transact-SQL&#41;](../../t-sql/statements/create-service-transact-sql.md)
- [DROP QUEUE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-queue-transact-sql.md)
- [RECEIVE &#40;Transact-SQL&#41;](../../t-sql/statements/receive-transact-sql.md)
- [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)
