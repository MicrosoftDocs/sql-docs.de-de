---
description: 'Verteilte Microsoft SQL Server-Abfragen: OLE DB-Konnektivität'
title: Erstellen eines Verbindungsserveranbieters
ms.date: 07/01/2019
ms.prod: sql
ms.technology: ''
ms.prod_service: database-engine
ms.reviewer: MikeRayMSFT
ms.topic: conceptual
author: pmasl
ms.author: pelopes
manager: rothj
ms.custom: seo-dt-2019
ms.openlocfilehash: 75134031d7567dcc5a653d28ecb1c92a26891d23
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080411"
---
# <a name="microsoft-sql-server-distributed-queries-ole-db-connectivity"></a>Verteilte Microsoft SQL Server-Abfragen: OLE DB-Konnektivität

In diesem Artikel wird beschrieben, wie der Abfrageprozessor von Microsoft SQL Server mit einem OLE DB-Anbieter interagiert, um verteilte und heterogene Abfragen zu ermöglichen. Dies ist in erster Linie für Entwickler von OLE DB-Anbietern gedacht und setzt ein fundiertes Verständnis der OLE DB-Spezifikation voraus. Der Schwerpunkt liegt auf der OLE DB-Schnittstelle zwischen dem SQL Server-Abfrageprozessor und dem OLE DB-Anbieter und nicht auf der Funktionalität für verteilte Abfragen selbst. Eine vollständige Beschreibung der Funktionalität für verteilte Abfragen finden Sie unter [Verbindungsserver](../../relational-databases/linked-servers/linked-servers-database-engine.md).

## <a name="overview-and-terminology"></a>Übersicht und Terminologie

 Mit verteilten Abfragen können SQL Server-Benutzer auf Daten auf Servern außerhalb von SQL Server zugreifen. Dieser kann sich innerhalb anderer Server, die SQL Server ausführen, oder anderer Datenquellen befinden, die eine OLE DB-Schnittstelle zur Verfügung stellen. OLE DB stellt eine Möglichkeit für einheitlichen Zugriff auf Tabellendaten aus heterogenen Datenquellen bereit.

Im Rahmen dieses Artikels kann es sich bei einer verteilten Abfrage um eine beliebige `SELECT`-, `INSERT`-. `UPDATE`- oder `DELETE`-Anweisung handeln, die auf Tabellen und Rowsets aus mindestens einer externen OLE DB-Datenquelle verweist.

Eine Remotetabelle ist eine Tabelle, die in einer OLE DB-Datenquelle gespeichert ist und außerhalb des SQL Server ausführenden Servers liegt, der die Abfrage ausführt. Eine verteilte Abfrage greift auf mindestens eine Remotetabelle zu.

### <a name="ole-db-provider-categories"></a>Kategorien von OLE DB-Anbietern

Im Folgenden werden OLE DB-Anbieter basierend auf ihren Funktionen aus der Sicht verteilter Abfragen von SQL Server kategorisiert. Diese schließen sich gemäß der Definition nicht gegenseitig aus. Ein jeweiliger Anbieter kann mehreren der folgenden Kategorien angehören:

- SQL-Befehlsanbieter

- Indexanbieter

- Einfache Tabellenanbieter

- Nicht-SQL-Befehlsanbieter

#### <a name="sql-command-providers"></a>SQL-Befehlsanbieter

Anbieter, die das `Command`-Objekt in einem SQL-Standarddialekt unterstützen, der von SQL Server erkannt wird, gehören dieser Kategorie an. Die spezifischen Anforderungen, die ein OLE DB-Anbieter erfüllen muss, um von SQL Server als SQL-Befehlsanbieter behandelt zu werden lauten wie folgt:

- Der Anbieter muss das `Command`-Objekt und alle erforderlichen OLE DB-Schnittstellen unterstützen: `ICommand`, `ICommandText`, `IColumnsInfo`, `ICommandProperties` und `IAccessor`.

- Der vom Anbieter unterstützte SQL-Dialekt muss mindestens SQL Subminimum sein. Der Dialekt muss mit der Eigenschaft `DBPROP_SQLSUPPORT` vom Anbieter angegeben werden.

Der Microsoft OLE DB-Anbieter für SQL Server und der Microsoft OLE DB-Anbieter für ODBC sind Beispiele für SQL-Befehlsanbieter.

#### <a name="index-providers"></a>Indexanbieter

Indexanbieter sind Anbieter, die Indizes gemäß OLE DB unterstützen und zur Verfügung stellen sowie die indexbasierte Suche von Basistabellen ermöglichen. Die spezifischen Anforderungen, die ein OLE DB-Anbieter erfüllen muss, um von SQL Server als Indexanbieter behandelt zu werden lauten wie folgt:

- Der Anbieter muss die `IDBSchemaRowset`-Schnittstelle mit den TABLES-, COLUMNS- und INDEXES-Schemarowsets unterstützen.

- Der Anbieter muss das Öffnen eines Rowsets in einem Index mithilfe von `IOpenRowset` durch Angabe des Indexnamens und des zugehörigen Basistabellennamens unterstützen.

- Das `Index`-Objekt muss alle erforderlichen Schnittstellen unterstützen: `IRowset`, `IRowsetIndex`, `IAccessor`, `IColumnsInfo`, `IRowsetInfo` und `IConvertTypes`.

- Rowsets, die (mit `IOpenRowset`) für die indizierte Basistabelle geöffnet werden, müssen die `IRowsetLocate`-Schnittstelle für die Positionierung in einer Zeile anhand eines Lesezeichens unterstützen.

Wenn der OLE DB-Anbieter die obigen Voraussetzungen erfüllt, können Benutzer die Anbieteroption `Index As Access Path` festlegen, um SQL Server die Verwendung des Index des Anbieters zum Auswerten von Abfragen zu ermöglichen. Standardmäßig versucht SQL Server nicht die Indizes des Anbieters zu verwenden, es sei denn, diese Option ist festgelegt.

>[!NOTE]
>SQL Server unterstützt verschiedene Optionen, die sich darauf auswirken, wie SQL Server auf einen OLE DB-Anbieter zugreift. Das Dialogfeld `Linked Server Properties` von SQL Server Enterprise Manager kann zum Festlegen dieser Optionen verwendet werden.

#### <a name="simple-table-providers"></a>Einfache Tabellenanbieter

Hierbei handelt es sich um Anbieter, die die Öffnung eines Rowsets für eine Basistabelle mithilfe der Schnittstelle `IOpenRowset` zur Verfügung stellen. Bei diesen Anbietern handelt es sich weder um SQL-Befehlsanbieter noch um Indexanbieter. Stattdessen handelt es sich um die einfachste Anbieterklasse, für die verteilte SQL Server-Abfragen durchgeführt werden können.

Für diese Anbieter kann SQL Server Tabellenscans nur während der Auswertung verteilter Abfragen durchführen.

#### <a name="non-sql-command-providers"></a>Nicht-SQL-Befehlsanbieter

Anbieter, die das `Command`-Objekt und dessen erforderlichen Schnittstellen unterstützen, aber keinen von SQL Server anerkannten Dialekt unterstützen, fallen in diese Kategorie.

Der Microsoft OLE DB-Anbieter für Indexdienst und der [Microsoft OLE DB-Anbieter für Microsoft Active Directory Service](../../ado/guide/appendixes/microsoft-ole-db-provider-for-microsoft-active-directory-service.md) sind Beispiele für Nicht-SQL-Befehlsanbieter.

### <a name="transact-sql-subset"></a>Transact-SQL-Teilmenge

Alle der folgenden Klassen von Transact-SQL-Anweisungen wird für verteilte Abfragen unterstützt, wenn der Anbieter die erforderlichen OLE DB-Schnittstellen unterstützt.

- Abgesehen von `SELECT` INTO-Anweisungen mit einer Remotetabelle als Zieltabelle sind alle `SELECT`-Anweisungen zulässig.

- `INSERT`-Anweisungen sind für Remotetabellen zulässig, wenn der Anbieter die erforderlichen Schnittstellen für INSERT unterstützt. Weitere Informationen zu OLE DB-Anforderungen für INSERT finden Sie später in diesem Artikel im Abschnitt zur \"INSERT-Anweisung\".

- `UPDATE`- und DELETE-Anweisungen können auf Remotetabellen angewendet werden, sofern der Anbieter die OLE DB-Schnittstellenanforderungen für die angegebene Tabelle erfüllt. Informationen zu den OLE DB-Schnittstellenanforderungen und -bedingungen für Updates und Löschen von Remotetabellen finden Sie weiter unten in diesem Artikel im Abschnitt zu \"UPDATE- und DELETE-Anweisungen\".

### <a name="cursor-support"></a>Cursorunterstützung

Sowohl Momentaufnahme- als auch Keysetcursor werden für verteilte Abfragen unterstützt, wenn der Anbieter die erforderliche OLE DB-Funktionalität unterstützt. Dynamische Cursor werden für verteilte Abfragen nicht unterstützt. Benutzeranforderungen von dynamischen Cursors für verteilte Abfragen werden auf Keysetcursor herabgestuft.

Momentaufnahmecursor werden zum Öffnungszeitpunkt des Cursor aufgefüllt, und das Resultset bleibt unverändert. UPDATE-, INSERT- und DELETE-Anweisungen für die zugrunde liegenden Tabellen werden im Cursor nicht dargestellt.

Keysetcursor werden zum Öffnungszeitpunkt des Cursors aufgefüllt, und das Resultset bleibt während der gesamten Lebensdauer des Cursors unverändert. UPDATE- und DELETE-Anweisungen für zugrunde liegende Tabellen sind jedoch im Cursor sichtbar, wenn die Zeilen besucht werden. INSERT-Anweisungen für zugrunde liegende Tabellen, die sich auf die Cursormitgliedschaft auswirken können, sind nicht sichtbar.

Eine Remotetabelle kann über einen Cursor aktualisiert oder gelöscht werden, der für eine verteilte Abfrage definiert ist und auf die Remotetabelle verweist, wenn der Anbieter die Bedingungen für Updates und Löschungen für die Remotetabelle erfüllt (z. B. `UPDATE` \| DELETE `<remote-table>` `WHERE` CURRENT OF `<cursor-name>`). Weitere Informationen finden Sie weiter unten in diesem Artikel im Abschnitt zu \"UPDATE- und DELETE-Anweisungen\".

#### <a name="keyset-cursor-support-requirements"></a>Anforderungen für die Keysetcursorunterstützung

Ein Keysetcursor wird für verteilte Abfragen unterstützt, wenn alle Anforderungen der Transact-SQL-Syntax erfüllt sind und eines der folgenden Szenarios vorliegt:

- Der OLE DB-Anbieter unterstützt wiederverwendbare Lesezeichen für alle Remotetabellen in der Abfrage. Wiederverwendbare Lesezeichen können von einem Rowset in einer Tabelle genutzt und für ein anderes Rowset in derselben Tabelle verwendet werden. Die Unterstützung wiederverwendbarer Lesezeichen wird vom Schemarowset TABLES_INFO von `IDBSchemaRowset` angegeben, indem die Spalte BOOKMARK_DURABILITY auf BMK_DURABILITY_INTRANSACTION oder eine höhere Dauerhaftigkeit festgelegt wird.

- Alle Remotetabellen stellen einen eindeutigen Schlüssel über das INDEXES-Rowset der `IDBSchemaRowset`-Schnittstelle zur Verfügung. Ein Indexeintrag mit einer auf VARIANT_TRUE festgelegten UNIQUE-Spalte sollte vorhanden sein.

Keysetcursor werden nicht für verteilte Abfragen unterstützt, die die *OpenQuery*-Funktion umfassen.

#### <a name="updatable-keyset-cursor-requirements"></a>Anforderungen für aktualisierbare Keysetcursor

Eine Remotetabelle kann über einen Keysetcursor aktualisiert oder gelöscht werden, der für eine verteilte Abfrage definiert ist (z. B. `UPDATE` \| DELETE `<remote-table>` `WHERE` CURRENT OF `<cursor-name>`). Es folgen die Bedingungen, unter denen aktualisierbare Cursor für verteilte Abfragen zulässig sind:

- Aktualisierbare Cursor sind zulässig, wenn der Anbieter auch die Bedingungen für Updates und Löschungen der Remotetabelle erfüllt. Weitere Informationen finden Sie weiter unten in diesem Artikel im Abschnitt zu \"UPDATE- und DELETE-Anweisungen\".

- Alle aktualisierbaren Keysetcursorvorgänge müssen sich mit der Isolationsstufe „Repeatable Read“ oder einer höheren Isolationsstufe in einer benutzerdefinierten Transaktion befinden. Außerdem muss der Anbieter verteilte Transaktionen mit der `ITransactionJoin`-Schnittstelle unterstützen.

## <a name="ole-db-provider-interaction-phases"></a>OLE DB-Anbieterinteraktionsphasen

 Es gibt sechs Vorgänge, die in allen Szenarios für die Ausführung verteilter Abfragen gängig sind:

- Vorgänge zum Herstellen der Verbindung und Abrufen von Eigenschaften legen fest, wie SQL Server die Verbindung mit OLE DB-Anbietern herstellt und welche Anbietereigenschaften verwendet werden.

- Vorgänge für die Auflösung von Tabellennamen und zum Abrufen von Metadaten legen fest, wie SQL Server den Remotetabellennamen in das entsprechende Datenobjekt im Anbieter auflöst (dies wird auf zwei Arten angegeben: als Name, der auf einem Verbindungsserver basiert, oder als Ad-hoc-Name). Dazu gehören auch die Tabellenmetadaten, die SQL Server vom Anbieter abruft, um eine verteilte Abfrage zu kompilieren und zu optimieren.

- Vorgänge für die Transaktionsverwaltung legen alle transaktionsbezogenen Interaktionen mit dem OLE DB-Anbieter fest.

- Vorgänge für die Verarbeitung von Datentypen legen fest, wie OLE DB-Datentypen von SQL Server verarbeitet werden, wenn Daten eines OLE DB-Anbieters verarbeitet oder Daten an einen OLE DB-Anbieter exportiert werden, während eine verteilte Abfrage verarbeitet wird.

- Vorgänge für die Fehlerbehandlung legen fest, wie SQL Server erweiterte Fehlerinformationen vom Anbieter verwendet.

- Sicherheitsvorgänge legen fest, wie die Sicherheit von SQL Server mit der Sicherheit des Anbieters interagiert.

### <a name="connection-establishment-and-property-retrieval"></a>Herstellung der Verbindung und Abruf von Eigenschaften

SQL Server unterstützt zwei Benennungskonventionen für Remotedatenobjekte: Verbindungsserverbasierte vierteilige Namen und Ad-hoc-Namen mit der `OPENROWSET`-Funktion.

#### <a name="linked-server-based-names"></a>Verbindungsserverbasierte Namen

Ein Verbindungsserver fungiert als Abstraktion einer OLE DB-Datenquelle. Ein Verbindungsserverbasierter Name ist ein vierteiliger Name im Format: `<linked-server>.<catalog>`. Bei `<schema>.<object>` entspricht `<linked-server>` dem Namen des Verbindungsservers. SQL Server interpretiert `<linked-server>`, um den OLE DB-Anbieter und die Verbindungsattribute abzuleiten, die die Datenquelle für den Anbieter identifizieren. Die anderen drei Namensteile werden von der OLE DB-Datenquelle interpretiert, um die spezifische Remotetabelle zu identifizieren. :::

#### <a name="ad-hoc-names"></a>Ad-hoc-Namen

Ein Ad-hoc-Name ist ein Name, der auf der `OPENROWSET`- oder `OPENDATASOURCE`-Funktion basiert. Jedes Mal, wenn die Remotetabelle in einer verteilten Abfrage referenziert wird, enthält dieser Name alle Verbindungsinformationen (d. h. der zu verwendende OLE DB-Anbieter, die zum Identifizieren der Datenquelle erforderlichen Attribute, die Benutzer-ID und das Kennwort).

Die Verwendung von Ad-hoc-Namen ist standardmäßig nur für Mitglieder mit der sysadmin-Rolle zulässig. Die Anbieteroption `DisallowAdhocAccess` sollte auf `0` festgelegt sein, damit Ad-hoc-Namen für einen OLE DB-Anbieter verwendet werden können.

Wenn ein Verbindungsservername verwendet wird, extrahiert SQL Server den OLE DB-Anbieternamen und die Initialisierungseigenschaften des Anbieters aus der Definition des Verbindungsservers. Wenn ein Ad-hoc-Name verwendet wird, extrahiert SQL Server dieselben Informationen aus den Argumenten der `OPENROWSET`-Funktion.

Ausführliche Anweisungen zum Einrichten eines Verbindungsservers mit einem vierteiligen Namen und einer auf Ad-hoc-Namen basierenden Syntax finden Sie unter [Erstellen von Verbindungsservern](create-linked-servers-sql-server-database-engine.md).

### <a name="connecting-to-an-ole-db-provider"></a>Herstellen einer Verbindung mit einem OLE DB-Anbieter

Die folgenden allgemeinen Schritte werden beim Herstellen einer Verbindung mit einem OLE DB-Anbieter von SQL Server durchgeführt:

1. SQL Server erstellt ein Datenquellenobjekt.

   SQL Server verwendet die `ProgID` des Anbieters, um das Datenquellenobjekt zu instanziieren. Die ProgID wird als `provider_name`-Parameter einer Verbindungsserverkonfiguration oder bei Ad-hoc-Namen als erstes Argument der `OPENROWSET`-Funktion festgelegt.

   SQL Server instanziiert das Datenquellenobjekt des Anbieters über die OLE DB-Dienstkomponentenschnittstelle `IDataInitialize`. Dies ermöglicht dem Dienstkomponenten-Manager, die Dienste über den nativen Funktionen des Anbieters zu aggregieren, z. B. die Unterstützung für Updates und das Scrollen. Des Weiteren ermöglicht die Instanziierung des Anbieters über `IDataInitialize` der OLE DB-Dienstkomponente, Verbindungen mit dem Anbieter zu einem Pool zusammenzufassen, wodurch der Mehraufwand für die Verbindung und Initialisierung reduziert wird.

   Ein Anbieter kann für die Instanziierung im selben Prozess wie SQL Server oder einem eigenen Prozess konfiguriert werden. Die Instanziierung in einem separaten Prozess schützt den SQL Server-Prozess vor Fehlern im Anbieter. Gleichzeitig wird dem Marshallen von OLE DB-Aufrufen von SQL Server außerhalb des Prozess ein Leistungsmehraufwand zugeschrieben. Ein Anbieter kann für die Instanziierung innerhalb oder außerhalb des Prozesses konfiguriert werden, indem die Anbieteroption `Allow In Process` festgelegt wird. Weitere Informationen finden Sie in den [Optionen für Anbieter](../../ado/guide/appendixes/microsoft-ole-db-provider-for-sql-server.md).

   Weitere Informationen über die OLE DB-Dienstkomponenten und Sitzungspools finden Sie in der OLE DB-Dokumentation für Anbieteranforderungen.

2. Die Datenquelle wird initialisiert.

   Nachdem das Datenquellenobjekt erstellt wurde, legt die `IDBProperties`-Schnittstelle die Initialisierungseigenschaft DBPROP_INIT_TIMEOUT fest, wenn die Serverkonfigurationsoption `remote login timeout` einen höheren Wert als 0 (null) aufweist. Diese Eigenschaft ist erforderlich.

   Diese Eigenschaften werden festgelegt, wenn sie in der Verbindungsserverdefinition oder im zweiten Argument der `OPENROWSET`-Funktion angegeben oder impliziert werden:

   - `DBPROP_INIT_PROVIDERSTRING`

   - `DBPROP_INIT_DATASOURCE`

   - `DBPROP_INIT_LOCATION`

   - `DBPROP_INIT_CATALOG`

   - `DBPROP_AUTH_USERID`

   - `DBPROP_AUTH_PASSWORD`

   Sobald diese Eigenschaften festgelegt wurden, wird `IDBInitialize::Initialize` aufgerufen, um das Datenquellenobjekt mit den festgelegten Eigenschaften zu initialisieren.

3. SQL Server erfasst anbieterspezifische Informationen.

   SQL Server erfasst mehrere Anbietereigenschaften, die für Auswertung verteilter Abfragen verwendet werden sollen. Diese Eigenschaften werden durch Aufruf von `IDBProperties::GetProperties` abgerufen. Alle dieser Eigenschaften sind optional. Wenn jedoch alle relevanten Eigenschaften unterstützt werden, kann SQL Server die Funktionen des Anbieters in vollem Umfang ausschöpfen. `DBPROP_SQLSUPPORT` ist beispielsweise erforderlich, um zu ermitteln, ob SQL Server Abfragen an den Anbieter senden kann. Wenn diese Eigenschaft nicht unterstützt wird, verwendet SQL Server den Remoteanbieter nicht als SQL-Befehlsanbieter, auch wenn es sich dabei um einen solchen handelt. In der folgenden Tabelle gibt die Spalte „Standardwert“ an, welchen Wert SQL Server erwartet, wenn der Anbieter die Eigenschaft nicht unterstützt.

Eigenschaft| Standardwert| Verwendung |
|:----|:----|:----|
|`DBPROP_DBMSNAME`|Keine|Wird für Fehlermeldungen verwendet.|
|`DBPROP_DBMSVER` |Keine|Wird für Fehlermeldungen verwendet.|
|`DBPROP_PROVIDERNAME`|Keine|Wird für Fehlermeldungen verwendet.|
|`DBPROP_PROVIDEROLEDBVER1`|1.5|Wird zum Bestimmen der Verfügbarkeit von 2.0-Features verwendet.
|`DBPROP_CONCATNULLBEHAVIOR`|Keine|Wird verwendet, um zu ermitteln, ob das `NULL`-Verkettungsverhalten des Anbieter dem von SQL Server entspricht.|
|`DBPROP_NULLCOLLATION`|Keine|Lässt die Verwendung von Sortierung/Index nur zu, wenn `NULLCOLLATION` mit dem Sortierungsverhalten der SQL Server-Instanz „NULL“ übereinstimmt.|
|`DBPROP_OLEOBJECTS`|Keine|Bestimmt, ob der Anbieter strukturierte Speicherschnittstellen für LOB-Spalten unterstützt.|
|`DBPROP_STRUCTUREDSTORAGE`|Keine|Bestimmt, welche strukturierten Speicherschnittstellen für LOB-Typen unterstützt werden (`ILockBytes`, `Istream` oder `ISequentialStream`).|
|`DBPROP_MULTIPLESTORAGEOBJECTS`|False|Bestimmt, ob mehrere LOB-Spalten gleichzeitig geöffnet sein können.|
|`DBPROP_SQLSUPPORT`|Keine|Bestimmt, ob SQL-Abfragen an den Anbieter gesendet werden können.|
|`DBPROP_CATALOGLOCATION`|`DBPROPVAL_CL_START`|Wird zum Erstellen mehrteiliger Tabellennamen verwendet.
|`SQLPROP_DYNAMICSQL`|False|SQL Server-spezifische Eigenschaft: Rückgabe von `VARIANT_TRUE` gibt an, dass `?`-Parametermarker für die Ausführung parametrisierter Abfragen unterstützt werden.
|`SQLPROP_NESTEDQUERIES`|False|SQL Server-spezifische Eigenschaft: Rückgabe von `VARIANT_TRUE` gibt an, dass der Anbieter geschachtelte `SELECT`-Anweisungen in der `FROM`-Klausel unterstützt.
|`SQLPROP_GROUPBY`|False|SQL Server-spezifische Eigenschaft: Rückgabe von `VARIANT_TRUE` gibt an, dass der Anbieter die GROUP BY-Klausel in der `SELECT`-Anweisung gemäß dem SQL-92-Standard unterstützt.
|`SQLPROP_DATELITERALS `|False|SQL Server-spezifische Eigenschaft: Rückgabe von `VARIANT_TRUE` gibt an, dass der Anbieter datetime-Literale gemäß der Transact-SQL-Syntax von SQL Server unterstützt.
|`SQLPROP_ANSILIKE `|False|SQL Server-spezifische Eigenschaft: Diese Eigenschaft ist für Anbieter von Interesse, die die minimale SQL-Ebene und den Operator `LIKE` gemäß der SQL-92-Einstiegsebene unterstützen (\'%\' und \'_\' werden als Platzhalterzeichen verwendet).
|`SQLPROP_SUBQUERIES `|False|SQL Server-Eigenschaft: Diese Eigenschaft ist für Anbieter von Interesse, die die minimale SQL-Ebene unterstützen. Diese Eigenschaft gibt an, dass der Anbieter Unterabfragen gemäß der SQL-92-Einstiegsebene unterstützt. Dies schließt Unterabfragen in der `SELECT`-Liste und der `WHERE`-Klausel mit Unterstützung für korrelierte Unterabfragen sowie `IN`-, `EXISTS`-, `ALL`- und `ANY`-Operatoren mit ein.
|`SQLPROP_INNERJOIN`|False|SQL Server-spezifische Eigenschaft: Diese Eigenschaft ist für Anbieter von Interesse, die die minimale SQL-Ebene unterstützen. Diese Eigenschaft gibt an, dass Joins mit mehreren Tabellen in der `FROM`-Klausel unterstützt werden. ------ ---

Die folgenden drei Literale werden aus `IDBInfo::GetLiteralInfo` abgerufen: `DBLITERAL_CATALOG_SEPARATOR`, `DBLITERAL_SCHEMA_SEPARATOR` (zum Erstellen eines vollständigen Objektnamens aus Namensteilen des Katalogs, Schemas und Objekts) und `DBLITERAL_QUOTE` (zum Kennzeichnen von Bezeichnernamen in einer SQL-Abfrage, die an den Anbieter gesendet wird).

Wenn der Anbieter keine Trennzeichenliterale unterstützt, verwendet SQL Server einen Punkt („.“) als Standardtrennzeichen. Wenn der Anbieter nur das Katalogtrennzeichen unterstützt, aber nicht das Schematrennzeichen, verwendet SQL Server das Katalogtrennzeichen auch als Schematrennzeichen. Wenn der Anbieter `DBLITERAL_QUOTE` nicht unterstützt, verwendet SQL Server ein einzelnes Anführungszeichen (`'`) als Anführungszeichen.

>[!NOTE]
>Wenn die Trennzeichenliterale für Namen des Anbieters nicht mit diesen Standardwerten übereinstimmen, muss der Anbieter sie über `IDBInfo` für SQL Server zur Verfügung stellen, um über vierteilige Namen auf die Tabellen zuzugreifen. Wenn diese Literale nicht zur Verfügung gestellt werden, können nur Pass-Through-Abfragen für einen solchen Anbieter verwendet werden.

Informationen zur Verfügbarkeit der Eigenschaften `SQLPROP_DYNAMICSQL` und `SQLPROP_NESTEDQUERIES` finden Sie unter [SQL Server Specific Properties (Spezifische SQL Server-Eigenschaften)](#appendixc).

### <a name="table-name-resolution-and-meta-data-retrieval"></a>Auflösung von Tabellennamen und Abrufen von Metadaten

SQL Server löst einen jeweiligen Remotetabellennamen in eine verteilte Abfrage in eine spezifische Tabelle oder Ansicht in einer OLE DB-Datenquelle auf. Sowohl das auf dem Verbindungsserver basierende als auch das auf Ad-hoc-Namen basierende Benennungsschema führt zu einem dreiteiligen Namen der vom Anbieter interpretiert wird. Bei Namen, die auf dem Verbindungsserver basieren, bestehen die letzten drei Teile des vierteiligen Namens aus den Katalog-, Schema- und Objektnamen. Bei Ad-hoc-Namen gibt das dritte Argument der `OPENROWSET`-Funktion einen dreiteiligen Namen an, der die Katalog-, Schema- und Objektnamen beschreibt. Katalog- und Schemaname können leer sein. (Ein vierteiliger Name mit einem leeren Katalognamen und Schemanamen würde wie folgt aussehen: `<server-name>...<object-name>`.) In diesem Fall verwendet SQL Server `NULL` als entsprechenden Wert, nach dem in den Schemarowsettabellen gesucht wird.

Die von SQL Server angewandten Namensauflösungsregeln und Schritte für den Metadatenabruf hängen davon ab, ob der Anbieter die `IDBSchemaRowset`-Schnittstelle für das `Session`-Objekt unterstützt.

Wenn `IDBSchemaRowset` unterstützt wird, werden die Schemarowsets `TABLES`, `COLUMNS`, `INDEXES` und `TABLES_INFO` über die `IDBSchemaRowset`-Schnittstelle verwendet. (Das `TABLES_INFO`-Schemarowset wird in OLE DB 2.0 definiert.) SQL Server schränkt die von der `IDBSchemaRowset`-Schnittstelle zurückgegebenen Schemarowsets auf die Schemarowsets ein, die mit den angegebenen Namensteilen der Remotetabelle übereinstimmen. Die folgenden Regeln beziehen sich auf die vom Anbieter unterstützten Einschränkungen der Schemarowsets und wie diese von SQL Server zum Abrufen der Metadaten einer Remotetabelle verwendet werden:

- Die Einschränkungen für die Spalten `TABLE_NAME` und `COLUMN_NAME` sind immer erforderlich.

- Wenn der Anbieter eine Einschränkung für `TABLE_CATALOG` (oder `TABLE_SCHEMA`) unterstützt, verwendet SQL Server diese Einschränkung für `TABLE_CATALOG` (oder `TABLE_SCHEMA`). Wenn der Katalogname (oder Schemaname) nicht im Remotetabellenname angegeben ist, wird ein `NULL`-Wert als entsprechender Einschränkungswert verwendet. Wenn ein Katalogname (oder Schemaname) angegeben ist, muss der Anbieter die entsprechende Einschränkung für `TABLE_CATALOG` (oder `TABLE_SCHEMA`) unterstützen.

- Der Anbieter muss die Einschränkung für die `TABLE_SCHEMA`-Spalte sowohl in `TABLES` als auch in `COLUMNS` oder in keinen der beiden unterstützen. Der Anbieter muss die Katalognameneinschränkung für die beiden Rowsets `TABLES` und `COLUMNS` oder für keines der beiden unterstützen.

- Wenn Einschränkungen für INDEXES unterstützt werden, muss der Anbieter die Schemaeinschränkung für die Rowsets `TABLES` und INDE`XES or support them on neither. The provider must either support catalog name restriction on both `TABLES` and `INDEXES oder keines der beiden unterstützen.

SQL Server ruft die Spalten `TABLE_CATALOG`, `TABLE_SCHEMA`, `TABLE_NAME`, `TABLE_TYPE` und `TABLE_GUID` aus dem Schemarowset `TABLES` ab, indem Einschränkungen gemäß der oben beschriebenen Regeln festgelegt werden.

SQL Server ruft die Spalten `TABLE_CATALOG`, `TABLE_SCHEMA`, `TABLE_NAME`, `COLUMN_NAME`, `COLUMN_GUID`, `ORDINAL_POSITION`, `COLUMN_FLAGS`, `IS_NULLABLE`, `DATA_TYPE`, `TYPE_GUID`, `CHARACTER_MAXIMUM_LENGTH`, `NUMERIC_PRECISION` und `NUMERIC_SCALE` aus dem Schemarowset `COLUMNS` ab. `COLUMN_NAME`, `DATA_TYPE` und `ORDINAL_POSITION` müssen gültige Werte ungleich NULL zurückgeben. Wenn `DATA_TYPE``DBTYPE_NUMERIC` oder `DBTYPE_DECIMAL` entspricht, müssen die entsprechenden Werte von `NUMERIC_PRECISION` und `NUMERIC_SCALE` gültige Nicht-NULL-Werte sein.

SQL Server sucht im optionalen Schemarowset `INDEXES` nach Indizes für die angegebene Remotetabelle, indem Einschränkungen gemäß der oben beschriebenen Regeln festgelegt werden. SQL Server ruft dann die Spalten `TABLE_CATALOG`, `TABLE_SCHEMA`, `TABLE_NAME`, `INDEX_CATALOG`, `INDEX_SCHEMA`, `INDEX_NAME`, `PRIMARY_KEY`, `UNIQUE`, `CLUSTERED`, `FILL_FACTOR`, `ORDINAL_POSITION`, `COLUMN_NAME`, `COLLATION`, `CARDINALITY` und `PAGES` aus den gefundenen übereinstimmenden Indexeinträgen ab.

SQL Server sucht im optionalen `TABLES_INFO`-Rowset nach weiteren Informationen zur angegebenen Remotetabelle, z. B. Lesezeichenunterstützung sowie Typ und Länge des Lesezeichens. Abgesehen von der Spalte `DESCRIPTION` des Rowsets `TABLES_INFO` werden alle Spalten verwendet. Die Informationen im Rowset `TABLES_INFO` wird wie folgt verwendet:

- Die Spalte `BOOKMARK_DURABILITY` wird zum Implementieren effizienterer Keysetcursor verwendet. Wenn diese Spalte den Wert `BMK_DURABILITY_INTRANSACTION` oder einen höheren Dauerhaftigkeitswert enthält, verwendet SQL Server auf Lesezeichen basiertes Abrufen und Updates von Remotetabellenzeilen zum Implementieren eines Keysetcursors.

- Die Spalten `BOOKMARK_TYPE`, `BOOKMARK_DATA TYPE` und `BOOKMARK_MAXIMUM_LENGTH` werden zum Ermitteln der Lesezeichenmetadaten zu Kompilierungszeit der Abfrage verwendet. Wenn diese Spalten nicht unterstützt werden, öffnet SQL Server das Basistabellenrowset während der Kompilierung mit `IOpenRowset`, um die Lesezeicheninformationen abzurufen.

Wenn `IDBSchemaRowset` nicht unterstützt wird und der Remotetabellenname einen Katalog- oder Schemanamen enthält, erfordert SQL Server `IDBSchemaRowset` und gibt einen Fehler zurück. Wenn jedoch weder Katalog- noch Schemaname angegeben ist, öffnet SQL Server das Rowset, das der Remotetabelle entspricht, und ruft Spaltenmetadaten aus der obligatorischen `IColumnsInfo`-Schnittstelle des Rowsetobjekts ab.

SQL Server öffnet das der Tabelle entsprechende Rowset mithilfe eines Aufrufs von `IOpenRowset::OpenRowset`. Der Tabellenname, der für `OPENROWSET` bereitgestellt wird, wird aus Teilen der Namen von Katalog, Schema und Objekt zusammengesetzt.

- Alle Namensteile (`catalog`, `schema`, `object name`) werden mit dem Anführungszeichen des Anbieters (`DBLITERAL_QUOTE`) aufgeführt und anschließend mit den Zeichen `DBLITERAL_CATALOG_SEPARATOR` und `DBLITERAL_SCHEMA_SEPARATOR` dazwischen verkettet. Diese Namenserstellung erfolgt gemäß den OLE DB-Regeln in `IOpenRowset`.

- Die Spaltenmetadaten für die Tabelle wird mithilfe von `IColumnsInfo::GetColumnInfo` abgerufen, nachdem das Rowsetobjekt geöffnet wurde.

Wenn `IDBSchemaRowset` nicht mit den Rowsets TABLES, COLUMNS und TABLES_INFO unterstützt wird, öffnen SQL Server das Rowset für die Basistabelle zweimal: einmal während der Kompilierung der Abfrage zum Abrufen von Metadaten, und ein weiteres Mal während der Ausführung der Abfrage. Anbieter, die beim Öffnen des Rowsets Nebenwirkungen auslösen können (z. B. Ausführen von Code, der den Zustand eines Echtzeitgeräts ändert, Senden von E-Mails, Ausführen von arbiträrem vom Benutzer bereitgestellten Codes), müssen sich dieses Verhaltens bewusst sein.

### <a name="statistics-retrieval"></a>Abrufen von Statistiken

Wenn der Anbieter Verteilungsstatistiken für die Basistabellen unterstützt, verwendet SQL Server diese Statistiken. Es gibt zwei Arten von Statistiken, die für den SQL Server-Abfrageprozessor von Interesse sind:

- **Spaltenkardinalität (oder Tupelkardinalität):** Dies ist die Anzahl eindeutiger Werte, die sich in einer Spalte (oder einer Kombination von Spalten) einer Tabelle befinden. Dies kann zum Einschätzen der Selektivität von Prädikaten für die Spalte(n) verwendet werden. Ein Anbieter, der Verteilungsstatistiken unterstützt, sollte mindestens einen Kardinalitätstyp unterstützt.

- **Histogramme:** Wenn die Verteilung von Werten nicht einheitlich ist, reicht die Anzahl eindeutiger Werte nicht aus, um die Selektivität von Prädikaten einzuschätzen. In diesem Fall kann ein Histogramm bereitgestellt werden, das genauere Informationen zur Verteilung von Spaltenwerten in einer Tabelle bereitstellt.

Mit Statistiken kann der SQL Server-Abfrageoptimierer die Kardinalitäten von intermediate-Vorgängen in einer Abfrage besser einschätzen, wodurch bessere Ausführungspläne für diese generiert werden können.

Der OLE DB-Anbieter sollte Verteilungsstatistiken wie folgt unterstützen:

- **Erforderlich:** Unterstützung der Eigenschaften `DBPROP_TABLESTATISTICS` (1), die angibt, ob Spalten- oder Tupelkardinalitäten unterstützt werden und ob Histogramme unterstützt werden, und `DBPROP_OPENROWSETSUPPORT` (2), die mithilfe von `DBPROPVAL_ORS_HISTOGRAM` angibt, ob Histogramme unterstützt werden.

- **Erforderlich:** Das Schemarowset `TABLE_STATISTICS`. Das Schemarowset `TABLE_STATISTICS` führt die in einer jeweiligen Datenbank verfügbaren Statistiken auf. Es enthält auch die Spalten- und Tupelkardinalitäten im Schemarowset selbst und gibt an, ob Histogramme in den spezifischen Spalten unterstützt werden. Damit SQL Server Statistiken nutzen kann, müssen die folgenden Spalten im Schemarowset enthalten sein: `TABLE_NAME`, `STATISTICS_NAME`, `STATISTICS_TYPE`, `COLUMN_NAME` und `ORDINAL_POSITION`. Mindestens eine `COLUMN_CARDINALITY`- oder `TUPLE_CARDINALITY`-Spalte ist erforderlich. Wenn Histogramme unterstützt werden, ist auch `NO_OF_RANGES` erforderlich.

- **Optional:** Wenn der Anbieter Histogramme unterstützt, kann optional auch eine Erweiterung der `IOpenRowset::OpenRowset`-Methode unterstützt werden, die das Öffnen eines Histogrammrowsets durch Angabe des `DBID`-Werts der entsprechenden Statistik ermöglicht.

Ausführliche Informationen zu den Schnittstellen für Statistiken finden Sie in der OLE DB 2.6-Spezifikation.

### <a name="constraints"></a>Einschränkungen

Der SQL Server-Abfrageoptimierer verwendet auch die `CHECK`-Einschränkungen, die für die Basistabellen in einer Remotedatenquelle definiert sind, wenn der OLE DB-Anbieter das OLE DB 2.6-Schemarowset `CHECK_CONSTRAINTS_BY_TABLE` unterstützt. Die `CHECK_CLAUSE`-Spalte des Schemarowsets sollte das Klauselprädikat `CHECK` in SQL-92-konformer Syntax zurückgeben. Der Abfrageoptimierer verwendet Einschränkungsinformationen, um Prädikate zu entfernen oder zu vereinfachen, bei denen bekannt ist, dass sie aufgrund einer fehlenden CHECK-Einschränkung der Tabelle immer den Wert FALSE oder TRUE aufweisen.

### <a name="transaction-management"></a>Transaktionsverwaltung

SQL Server unterstützt den transaktionsbasierten Zugriff auf verteilte Daten mithilfe der OLE DB-Schnittstellen `ITransactionLocal` (für lokale Transaktionen) und `ITransactionJoin` (für verteilte Transaktionen) des Anbieters. Durch Starten einer lokalen Transaktion für den Anbieter garantiert SQL Server atomische Schreibvorgänge. Mithilfe verteilter Transaktionen stellt SQL Server sicher, dass Transaktionen, die mehrere Knoten umfassen, bei allen Knoten dasselbe Ergebnis (entweder Commit oder Abbruch) aufweisen. Wenn der Anbieter die erforderlichen auf OLE DB-Transaktionen bezogenen Schnittstellen nicht unterstützt, werden Updatevorgänge für diesen Anbieter abhängig von lokalen Transaktionskontext nicht unterstützt.

In der folgenden Tabelle wird beschrieben, was geschieht, wenn der Benutzer eine verteilte Abfrage mit den vom Anbieter bereitgestellten Funktionen und einem lokalen Ausführungskontext ausführt. Ein Lesevorgang für einen Anbieter bezieht sich entweder auf eine `SELECT`-Anweisung oder darauf, dass die Remotetabelle in die Eingabe einer `SELECT INTO`-, `INSERT`-, `UPDATE`- oder `DELETE`-Anweisung gelesen wird. Ein Schreibvorgang für einen Anbieter bezieht sich auf eine `INSERT`-, `UPDATE`- oder `DELETE`-Anweisung mit einer Remotetabelle als Zieltabelle.

Ergebnisse einer verteilten Abfrage basierend auf Anbieterfunktionen und Transaktionskontext:

|Ausführung der verteilten Abfrage|Anbieter unterstützt `ITransactionLocal` nicht|Anbieter unterstützt `ITransactionLocal`, aber nicht `ITransactionJoin`|Anbieter unterstützt sowohl `ITransactionLocal` als auch `ITransactionJoin`|
|:-----|:-----|:-----|:-----|
| Allein in einer Transaktion (keine Benutzertransaktion)|Standardmäßig sind nur Lesevorgänge zulässig. Wenn die Anbieterebenenoption `Nontransacted Updates` aktiviert ist, sind Schreibvorgänge zulässig. (Wenn diese Option aktiviert ist, kann SQL Server keine Atomarität und Konsistenz für die Daten des Anbieters gewährleisten. Dies kann zu dem Nebeneffekt führen, dass Schreibvorgänge in der Remotedatenquelle reflektiert werden, ohne dass diese rückgängig gemacht werden können.)| Alle Anweisungen sind für Remotedaten zulässig. Keysetcursor sind schreibgeschützt. Die lokale Transaktion wird auf dem Anbieter mit der Transaktionsisolationsstufe der aktuellen SQL Server-Sitzung gestartet und nach erfolgreicher Auswertung der Anweisung committet. (Die Standardisolationsstufe für SQL Server-Sitzungen lautet `READ COMMITTED`, es sei denn, sie wurde mit der Anweisung `SET TRANSACTION ISOLATION LEVEL` geändert. Der Anbieter muss die angeforderte Isolationsstufe unterstützen.) | Alle Anweisungen sind zulässig. Keysetcursor sind schreibgeschützt. Die lokale Transaktion wird auf dem Anbieter mit der Transaktionsisolationsstufe der aktuellen SQL Server-Sitzung gestartet und nach erfolgreicher Auswertung der Anweisung committet. |
| In einer Benutzertransaktion (d. h. zwischen `BEGIN TRAN` oder `BEGIN DISTRIBUTED TRAN` und `COMMIT`) | Wenn die Isolationsstufe der Transaktion `READ COMMITTED` (Standard) oder niedriger lautet, sind Lesevorgänge zulässig. Wenn die Isolationsstufe höher ist, werden keine verteilten Abfragen zugelassen. | Nur Lesevorgänge sind zulässig. Neue verteilte Transaktionen werden auf dem Anbieter mit der Isolationsstufe der aktuellen SQL Server-Sitzung gestartet. |Alle Anweisungen sind zulässig. Neue verteilte Transaktionen werden auf dem Anbieter mit der Isolationsstufe der aktuellen SQL Server-Sitzung gestartet und committet, wenn die Benutzertransaktion committet wird. Für Datenänderungsanweisungen startet SQL Server standardmäßig eine geschachtelte Transaktion unter der verteilten Transaktion, damit die Datenänderungsanweisung unter bestimmten Fehlerbedingungen gestoppt werden kann, ohne die umgebende Transaktion zu beenden. Wenn die Option `XACT_ABORT SET` aktiviert ist, erfordert SQL Server keine Unterstützung von geschachtelten Transaktionen und beendet die umgebende Transaktion, wenn während der Datenänderungsanweisung Fehler auftreten. |
|  |  |  |

### <a name="data-type-handling-in-distributed-queries"></a>Verarbeitung des Datentyps in verteilten Abfragen

OLE DB-Anbieter stellen ihre Daten im Rahmen der in OLE DB definierten Datentypen zur Verfügung (in OLE DB durch `DBTYPE` angegeben). SQL Server verarbeitet externe Daten innerhalb des Servers als native SQL Server-Typen. Dies führt zu einer Zuordnung von OLE DB-Datentypen zu nativen SQL Server-Typen und umgekehrt, während die Daten von SQL Server verarbeitet oder exportiert werden. Sofern nicht anders angegeben, erfolgt diese Zuordnung implizit.

Datentypen in verteilten Abfragen werden mithilfe einer von zwei Zuordnungsmethoden verteilt:

- Die Zuordnung auf der Verbraucherseite ordnet OLE DB-Datentypen zu nativen SQL Server-Datentypen auf der Verbraucherseite zu, wenn Remotetabellen in `SELECT`-Anweisungen und in der Eingabe von INSERT-, UPDATE- und DELETE-Anweisungen auftreten.

- Die Zuordnung auf der Exportseite ordnet SQL Server-Datentypen zu OLE DB-Datentypen auf der Exportseite zu, wenn eine Remotetabelle als Zieltabelle einer `INSERT`- oder `UPDATE`-Anweisung auftritt.

Zuordnungstabelle für Datentypen von SQL Server und OLE DB

| OLE DB-Typ | `DBCOLUMNFLAG` | SQL Server-Datentyp |
|-----|-----|-----|
|`DBTYPE_I1`*| |`numeric(3, 0)`|
|`DBTYPE_I2`| |`smallint`|
|`DBTYPE_I4`| |`int`|
|`DBTYPE_I8`| |`numeric(19,0)`|
|`DBTYPE_UI1`| |`tinyint`|
|`DBTYPE_UI2`*| |`numeric(5,0)`|
|`DBTYPE_UI4`*| |`numeric(10,0)`|
|`DBTYPE_UI8`*| |`numeric(20,0)`|
|`DBTYPE_R4`| |`float`|
|`DBTYPE_R8`| |`real`|
|`DBTYPE_NUMERIC`| |`numeric`|
|`DBTYPE_DECIMAL`| |`decimal`|
|`DBTYPE_CY`| |`money`|
|`DBTYPE_BSTR`| `DBCOLUMNFLAGS_ISFIXEDLENGTH=true`<br>oder<br> Maximale Länge > 4.000 Zeichen|ntext|
|`DBTYPE_BSTR`| `DBCOLUMNFLAGS_ISFIXEDLENGTH=true`|NCHAR|
|`DBTYPE_BSTR`| `DBCOLUMNFLAGS_ISFIXEDLENGTH=false`|NVARCHAR|
|`DBTYPE_IDISPATCH`| |Fehler|
|`DBTYPE_ERROR`| |Fehler|
|`DBTYPE_BOOL`| |`bit`|
|`DBTYPE_VARIANT`*| |NVARCHAR|
|`DBTYPE_IUNKNOWN`| |Fehler|
|`DBTYPE_GUID`| |`uniqueidentifier`|
|`DBTYPE_BYTES`|`DBCOLUMNFLAGS_ISLONG=true` <br>oder<br> Maximale Länge > 8.000 Zeichen|`image`|
|`DBTYPE_BYTES`|`DBCOLUMNFLAGS_ISROWVER=true`, `DBCOLUMNFLAGS_ISFIXEDLENGTH=true`, Spaltengröße = 8 <br>oder<br> Maximale Länge wurde nicht gemeldet. | `timestamp` |
|`DBTYPE_BYTES`| `DBCOLUMNFLAGS_ISFIXEDLENGTH=true` | `binary` |
|`DBTYPE_BYTES`| `DBCOLUMNFLAGS_ISFIXEDLENGTH=true` | `varbinary`|
|`DBTYPE_STR`| `DBCOLUMNFLAGS_ISFIXEDLENGTH=true` | `char`|
|`DBTYPE_STR`| `DBCOLUMNFLAGS_ISFIXEDLENGTH=true` | `varchar` |
|`DBTYPE_STR`| `DBCOLUMNFLAGS_ISLONG=true` <br>oder<br> Maximale Länge > 8.000 Zeichen <br>oder<br>   Maximale Länge wurde nicht gemeldet. | `text`|
|`DBTYPE_WSTR`| `DBCOLUMNFLAGS_ISFIXEDLENGTH=true` |`nchar`|
|`DBTYPE_WSTR` | `DBCOLUMNFLAGS_ISFIXEDLENGTH=false`|`nvarchar`|
|`DBTYPE_WSTR`| `DBCOLUMNFLAGS_ISLONG=true` <br>oder<br> Maximale Länge > 4.000 Zeichen <br>oder<br>   Maximale Länge wurde nicht gemeldet. | `ntext`|
|`DBTYPE_UDT`| |Fehler|
|`DBTYPE_DATE`* | | `datetime` |
|`DBTYPE_DBDATE` | | `datetime` (explizite Konvertierung erforderlich)|
|`DBTYPE_DBTIME`| | `datetime` (explizite Konvertierung erforderlich)|
|`DBTYPE_DBTIMESTAMP`* | | `datetime`|
|`DBTYPE_ARRAY` | |Fehler|
|`DBTYPE_BYREF` | | Wird ignoriert. |
|`DBTYPE_VECTOR` | |Fehler|
|`DBTYPE_RESERVED`| |Fehler|

\*Gibt eine Art Übersetzung in die Darstellung des SQL Server-Typs an, da es keinen äquivalenten Datentyp in SQL Server gibt. Solche Konvertierungen können zu Genauigkeitsverlust, Überlauf oder Unterlauf führen. Die impliziten Standardzuordnungen können in Zukunft geändert werden, wenn die entsprechenden Datentypen von zukünftigen SQL Server-Versionen unterstützt werden.

>[!NOTE]
>`numeric(p,s)` gibt den SQL Server-Datentyp `numeric` mit der Genauigkeit `p` und der Skalierung `s` an. Die maximal zulässige Genauigkeit für `DBTYPE_NUMERIC` und `DBTYPE_DECIMAL` ist 38. Der Anbieter muss die Bindung an die `DBTYPE_BSTR`-Spalte als `DBTYPE_WSTR` beim Erstellen eines Accessors unterstützen. `DBTYPE_VARIANT`-Spalten werden als `nvarchar`-Unicode-Zeichenfolgen verarbeitet. Hierzu muss der Anbieter die Konvertierung von `DBTYPE_VARIANT` in `DBTYPE_WSTR` unterstützen. Es wird erwartet, dass der Anbieter diese Konvertierung gemäß der Definition in OLE DB implementiert. Weitere Informationen finden Sie unter [Data Types of the OLE DB Specification (Datentypen der OLE DB-Spezifikation)](#appendixa).

#### <a name="interpreting-data-type-mapping"></a>Interpretieren der Datentypzuordnung

Die Zuordnung zu einem SQL Server-Typ wird anhand des OLE DB-Datentyps und den DBCOLUMNFLAGS-Werten bestimmt, die den Spalten- oder Skalarwert beschreiben. Beim Schemarowset `COLUMNS` werden diese Werte von den Spalten `DATA_TYPE` und `COLUMN_FLAGS` dargestellt. Bei der Schnittstelle `IColumnsInfo::GetColumnInfo` wird diese Information von den Membern `wType` und `dwFlags` der Struktur `DBCOLUMNINFO` dargestellt.

Suchen Sie in der Tabelle nach dem entsprechenden SQL Server-Typ, um die Zuordnung auf der Verbraucherseite für eine Spalte mit einem spezifischen `DBTYPE`- und `DBCOLUMNFLAG`-Wert zu verwenden. Die Typregeln für Spalten aus Remotetabellen in Ausdrücken können mit der folgenden Regel beschrieben werden:

>Ein jeweiliger Remotespaltenwert ist in einem Transact-SQL-Ausdruck gültig, wenn der entsprechende zugeordnete SQL Server-Typ in der Tabelle im gleichen Kontext gültig ist.

Die Tabelle und die Regel definieren:

- Vergleiche und Ausdrücke.

Im Allgemeinen ist `X <op> <remote-column>` ein gültiger Ausdruck, wenn `<op>` ein gültiger Operator für den Datentyp von `X` und den Datentyp ist, dem `<remote-column>` zugeordnet wird.

- Explizite Konvertierungen.

`Convert(X, <remote-column>)` ist zulässig, wenn der Typ `DBTYPE` von `<remote-column>` dem nativen Datentyp `Y` zugeordnet wird (gemäß der obigen Tabelle) und die explizite Konvertierung von `Y` in `X` zulässig ist.

Wenn Benutzer mehr Remotedaten in einen nicht standardmäßigen nativen Datentyp konvertieren möchten, müssen sie eine explizite Konvertierung verwenden.

Ordnen Sie native SQL Server-Datentypen zu OLE DB-Datentypen mithilfe derselben Tabelle zu, um die Zuordnung auf der Exportseite für die Anweisungen `UPDATE` und `INSERT` für Remotetabellen zu verwenden. Die Zuordnung des SQL Server-Typ `S1` zu einem jeweiligen `T`-OLE DB-Typ ist zulässig, wenn eines der folgenden Szenarios vorliegt:

- Die entsprechende Zuordnung ist in der Zuordnungstabelle enthalten.

- Es liegt eine zulässige implizite Konvertierung von `S1` in den anderen SQL Server-Typ `S2` vor, durch die `S2` in der Zuordnungstabelle dem Typ `T` zugeordnet wird.

#### <a name="large-object-lob-handling"></a>Behandlung großer Objekte (Large Object, LOB)

Wenn Spalten der Typen `DBTYPE_STR`, `DBTYPE_WSTR` oder `DBTYPE_BSTR` auch `DBCOLUMNFLAGS_ISLONG` enthalten oder ihre maximale Länge 4.000 Zeichen überschreitet (bzw. keine maximale Länge gemeldet wird), behandelt SQL Server sie wie in der Zuordnungstabelle angegeben entsprechend als `text`- oder `ntext`-Spalte. Ähnlich gilt für `DBTYPE_BYTES`-Spalten, dass die Spalten wie `image`-Spalten behandelt werden, wenn `DBCOLUMNFLAGS_ISLONG` festgelegt ist oder die maximale Länge über 8.000 Byte liegt (oder wenn die maximale Länge nicht gemeldet wird). Die Spalten `Text`, `ntext` und `image` werden als LOB-Spalten bezeichnet.

SQL Server stellt die vollständige Text- und Bildfunktion für LOBs eines OLE DB-Anbieters nicht frei. `TEXTPTRS` wird für LOBs von OLE DB-Anbietern nicht unterstützt. Daher werden auch keine der zugehörigen Funktionen unterstützt (z. B. die Systemfunktion `TEXTPTR` und die Anweisungen `READTEXT`, `WRITETEXT` und `UPDATETEXT`). `SELECT`-Anweisungen, die ganze LOB-Spalten abrufen, sowie die Anweisungen `UPDATE` und `INSERT` für ganze LOB-Spalten in Remotetabellen werden unterstützt.

SQL Server verwendet die strukturierten Speicherschnittstellen für LOB-Spalten, wenn diese vom Anbieter unterstützt werden. Die strukturierten Speicherschnittstellen werden folgenderweise in Reihenfolge zunehmender Bevorzugung und Funktionalität aufgeführt: `ISequentialStream`, `Istream` und `ILockBytes`. Wenn mindestens eine dieser Schnittstellen unterstützt wird, muss der Anbieter DBPROPVAL_OO_BLOB als Wert der `DBPROP_OLEOBJECTS`-Eigenschaft zurückgeben, wenn diese über die `IDBProperties`-Schnittstelle abgefragt wird. Der Anbieter sollte die unterstützten Schnittstellen außerdem in der Eigenschaft `DBPROP_STRUCTUREDSTORAGE` angeben.

Wenn der Anbieter keine der strukturierten Speicherschnittstellen für LOB-Spalten unterstützt, erzeugt SQL Server diese Schnittstelle selbst und stellt sie dennoch über die `text`-, `ntext`- oder `image`-Spalten zur Verfügung.

#### <a name="accessing-lob-columns"></a>Zugreifen auf LOB-Spalten

Wenn der Anbieter eine der strukturierten Speicherschnittstellen unterstützt, führt SQL Server die folgenden Schritte zum Abrufen von LOB-Spalten während der Abfrageausführung durch:

1. Bevor das Rowset über `IOpenRowset::OpenRowset` geöffnet wird, fordert SQL Server Unterstützung für mindestens eine der strukturierten Speicherschnittstellen (`ISequentialStream`, `Istream` oder `ILockBytes`) für die LOB-Spalten an. Die erste vom Anbieter unterstützte Schnittstelle ist erforderlich. Weitere Schnittstellen werden mit der Eigenschaft \"SETIFCHEAP\" angefordert, indem das Element *dwOptions* der entsprechenden DBPROP-Struktur auf DBPROPOPTIONS_SETIFCHEAP festgelegt wird. Wenn ein Anbieter beispielsweise `ISequentialStream` und `ILockBytes` unterstützt, ist `ISequentialStream` erforderlich und `ILockBytes` wird mit der Eigenschaft \"SETIFCHEAP\" angefordert.

4. Sobald das Rowset geöffnet wurde, verwendet SQL Server `IRowsetInfo::GetProperties`, um die tatsächlich verfügbaren Schnittstellen im Rowset zu identifizieren. Die letzte oder bevorzugte Schnittstelle, die vom Anbieter zurückgegeben wird, wird verwendet. Wenn SQL Server einen Accessor für die LOB-Spalte erstellt, wird die Spalte mit dem *iid*-Element der DBOBJECT-Struktur in der für die Schnittstelle festgelegten Bindung als DBTYPE_IUNKNOWN gebunden.

#### <a name="reading-from-lob-columns"></a>Lesen aus LOB-Spalten

Verwenden Sie den Schnittstellenzeiger für die angeforderte strukturierte Speicherschnittstelle, die im Zeilenpuffer von `IRowset::GetData` zum Auslesen der LOB-Spalte zurückgegeben wird. Wenn der Anbieter mehrere gleichzeitig geöffnete LOBs nicht unterstützt (d. h. wenn `DBPROP_MULTIPLE_STORAGEOBJECTS` nicht unterstützt wird) und die Zeile mehrere LOB-Spalten enthält, kopiert SQL Server die LOB-Spalten in eine lokale Arbeitstabelle.

#### <a name="update-and-insert-statements-on-lob-columns"></a>`UPDATE`- und `INSERT`-Anweisungen für LOB-Spalten

SQL Server übergibt einen Zeiger auf ein neues Speicherobjekt an den Anbieter, anstatt die vom Anbieter bereitgestellte Schnittstelle zum Ändern des Speicherobjekts zu verwenden. Für jede LOB-Spalte wird der Wert, der aktualisiert oder in ein Speicherobjekt eingefügt wird, mit der ausgewählten strukturierten Speicherschnittstelle erstellt. Je nachdem, ob es sich um einen `UPDATE`- oder `INSERT`-Vorgang handelt, wird ein Speicherobjekt über `IRowsetChange::SetData` oder `IRowsetChange::InsertRow` an den Anbieter übergeben.

### <a name="error-handling"></a>Fehlerbehandlung

Wenn ein bestimmter Methodenaufruf für einen OLE DB-Anbieter einen Fehlercode zurückgibt, sucht SQL Server nach den erweiterten Fehlerinformationen des Anbieters, bevor dem Benutzer Informationen zu den Fehlerbedingungen zurückgegeben werden.

SQL Server verwendet das OLE DB-Fehlerobjekt gemäß OLE DB. Einige der allgemeinen Schritte lauten wie folgt:

1. Wenn ein Methodenaufruf einen Fehlercode vom Anbieter zurückgibt, sucht SQL Server nach der `ISupportErrorInfo`-Schnittstelle. Wenn diese Schnittstelle unterstützt wird, ruft SQL Server `ISupportErrorInfo::InterfaceSupportsErrorInfo` auf, um zu überprüfen, ob Fehlerobjekte von der Schnittstelle unterstützt werden, die den Fehlercode erzeugt hat.

<!-- -->

5. Wenn Fehlerobjekte von der Schnittstelle unterstützt werden, ruft SQL Server die `GetErrorInfo`-Funktion auf, um einen `IErrorInfo`-Schnittstellenzeiger auf das aktuelle Fehlerobjekt abzurufen.

6. SQL Server verwendet die `IErrorInfo`-Schnittstelle, um einen Zeiger auf die `IErrorRecords`-Schnittstelle abzurufen.

7. SQL Server verwendet `IErrorRecords`, um alle Fehlerdatensätze im Objekt zu durchlaufen und den Text der Fehlermeldung der einzelnen Datensätze abzurufen.

Weitere Informationen zur Verwendung des Fehlerobjekts des Anbieters finden Sie in der OLE DB-Dokumentation.

### <a name="security"></a>Sicherheit

Wenn ein Benutzer eine Verbindung mit einem OLE DB-Anbieter herstellt, fordert der Anbieter in der Regel eine Benutzer-ID und ein Kennwort an, es sei denn, der Benutzer möchte als integrierter Sicherheitsbenutzer authentifiziert werden. Bei verteilten Abfragen agiert SQL Server als Benutzer des OLE DB-Anbieters für die SQL Server-Anmeldung, die die verteilte Abfrage ausführt. SQL Server ordnet die aktuelle SQL Server-Anmeldung einer Benutzer-ID und einem Kennwort auf dem Verbindungsserver zu.

Diese Zuordnungen können vom Benutzer eines jeweiligen Verbindungsservers festgelegt werden und von den gespeicherten Systemprozeduren `sp_addlinkedsrvlogin` und `sp_droplinkedsrvlogin` eingerichtet und verwaltet werden. Wenn die Initialisierungsgruppeneigenschaften DBPROP_AUTH_USERID und DBPROP_AUTH_PASSWORD über `IDBProperties::SetProperties` festgelegt werden, werden die von der Zuordnung festlege Benutzer-ID und das Kennwort beim Herstellen der Verbindung an den Anbieter übermittelt.

Wenn ein Client die Verbindung mit SQL Server über Windows-Authentifizierung herstellt und die Anmeldung eine mithilfe von `sp_addlinkedsrvlogin` eingerichtete `self`-Zuordnung aufweist, versucht SQL Server die Identität des Sicherheitskontexts des Clients anzunehmen und legt die `DBPROP_AUTH_INTEGRATED`-Eigenschaft beim Herstellen der Verbindung für den Anbieter fest. Dieser Vorgang wird als *Delegierung* bezeichnet.

Nachdem der Sicherheitskontext für die Verbindung ermittelt wurde, liegen die Authentifizierung dieses Sicherheitskontexts und die Berechtigungsüberprüfung des Kontexts für Datenobjekte in der Datenquelle in der Verantwortung des OLE DB-Anbieters.

Weitere Informationen finden Sie unter [`sp_addlinkedsrvlogin`](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md) und [`sp_droplinkedsrvlogin`](../../relational-databases/system-stored-procedures/sp-droplinkedsrvlogin-transact-sql.md).

## <a name="query-execution-scenarios"></a>Szenarios der Abfrageausführung

 Beim Auswerten einer verteilten Abfrage interagiert SQL Server mit dem OLE DB-Anbieter auf mindestens einer der folgenden Weisen:

- Remoteabfrage

- Indizierter Zugriff

- Reine Tabellenscans

- `UPDATE`- und DELETE-Anweisungen

- `INSERT`-Anweisung

- Pass-Through-Abfragen

### <a name="remote-query"></a>Remote Query

SQL Server generiert eine SQL-Abfrage, die einen Teil der ursprünglichen Abfrage auswertet, die vom Anbieter vollständig ausgewertet werden kann. Dieses Szenario ist nur für SQL-Befehlsanbieter möglich. Das Ausmaß der Übertragung von Vorgängen von SQL Server an den Anbieter mithilfe von Push durch das Generieren einer SQL-Abfrage hängt von der vom Anbieter unterstützten SQL-Grammatik ab. Der Anbieter sollte die Ebene der SQL-Unterstützung durch Folgendes angeben:

1. Durch Angeben der Unterstützung der minimalen SQL-Grammatik, ODBC Core oder der SQL-92-Einstiegsstufe über die `DBPROP_SQLSUPPORT`-Eigenschaft. Die minimale SQL-Syntaxebene ist eine neue Ebene, die in SQL Server unterstützt wird, weshalb SQL Server Remoteabfragen an einfache Anbieter senden kann, die eine einfache Teilmenge von SQL unterstützen. Diese Ebene umfasst eine einfache `SELECT`-Anweisung, die keine Unterabfragen, mehrere Tabellen in der `FROM`-Klausel (daher keine Joins) und „GROUP BY“ umfasst. Informationen zur Teilmenge der SQL-Grammatik, die von SQL Server zum Generieren von Remoteabfragen für Anbieter jeder dieser Syntaxebenen verwendet wird, finden Sie unter [SQL-Teilmenge für das Generieren von Remoteabfragen](#appendixb).

1. Durch die Unterstützung verschiedener spezifischer SQL Server-Eigenschaften zum Angeben der Unterstützung für einzelne SQL-Features, die wie von „DBPROP_SQLSUPPORT“ berichtet nicht anderweitig in der Syntaxebene enthalten sind. Die Liste der Eigenschaften und deren Verwendung durch SQL Server werden weiter unten in diesem Abschnitt beschrieben.

SQL Server verwendet eine parametrisierte Abfrageausführung mit einem Fragezeichen (?) als Parametermarker in der Transact-SQL-Zeichenfolge. Die Ausführung parametrisierte Abfragen wird für SQL Server-, Microsoft Jet- und Oracle OLE DB-Anbieter verwendet. Für andere Anbieter wird die Ausführung parametrisierter Abfragen verwendet, wenn der Anbieter `ICommandWithParameters` für das `Command`-Objekt unterstützt und mindestens eine der folgenden Bedingungen erfüllt wird:

- Der Anbieter gibt die ODBC Core-Ebene der SQL Server-Unterstützung über die `DBPROP_SQLSUPPORT`-Eigenschaft an.

- Der Anbieter gibt die Unterstützung für den Fragezeichenparametermarker (?) durch die Unterstützung der SQL Server-spezifischen Eigenschaft „SQLPROP_DYNCMICSQL“ durch `IDBPProperties` an. Weitere Informationen finden Sie im nächsten Abschnitt zu den Anbietereigenschaften.

- Der Administrator legt die Anbieteroption `Dynamic Parameters` für den Anbieter fest, damit SQL Server parametrisierte Abfragen generieren kann.

Wenn SQL Server den remote auszuführenden SQL-Text generiert, werden die Tabellen- und Spaltennamen wie über das `DBLITERAL_QUOTE`-Literal der `IDBInfo`-Schnittstelle angegeben in die Zeichenangabe des Anbieters eingeschlossen. Wenn dieses Literal nicht unterstützt wird, werden Tabellen- und Spaltennamen nicht eingeschlossen.

Wenn der Anbieter die Ausführung parametrisierter Abfragen unterstützt, zieht SQL Server die Ausführung einer parametrisierten Abfragestrategie zum Auswerten eines Joins einer Remotetabelle mit einer lokalen Tabelle in Betracht. Die parametrisierte Abfrage wird für Parameterwerte wiederholt ausgeführt, die aus den einzelnen Zeilen der lokalen Tabelle generiert werden. Diese Strategie reduziert die Anzahl der Zeilen, die vom Anbieter abgerufen werden und bringt Vorteile bei der Verknüpfung einer lokalen Tabelle mit einer kleinen Anzahl an Zeilen mit einer Remotetabelle mit einer großen Anzahl an Zeilen mit sich. Diese Remotejoinstrategie kann durch den Joinoptimiererhinweis `REMOTE` erzwungen werden. Weitere Informationen zur Ausführung parametrisierter Abfragen finden Sie unter [Vorgehensweise: Ausführen parametrisierter Abfragen](../../connect/php/how-to-perform-parameterized-queries.md).

Im Folgenden finden Sie die Schritte für höhere Ebenen für den Anbieter im Remoteabfrageszenario.

1. SQL Server erstellt mithilfe von `IDBCreateCommand::CreateCommand` ein `Command`-Objekt aus dem `Session`-Objekt.

9. Wenn die Serverkonfigurationsoption `Remote Query Timeout` auf einen Wert > 0 festgelegt ist, legt SQL Server die Eigenschaft `DBPROP_COMMANDTIMEOUT` für das Objekt `Command` mithilfe von `ICommandProperties::SetProperties` auf denselben Wert fest. `ICommand::SetCommandText` muss aufgerufen werden, um den Befehlstext auf die generierte Transact-SQL-Zeichenfolge festzulegen.

10. SQL Server ruft `ICommandPrepare::Prepare` zum Vorbereiten des Befehls ab. Wenn der Anbieter diese Schnittstelle nicht unterstützt, fährt SQL Server mit Schritt 4 fort.

11. Wenn es sich bei der generierten Abfrage um eine parametrisierte Abfrage handelt, verwendet SQL Server `ICommandWithParameters::SetParameterInfo` zum Beschreiben der Parameter und `IAccessor::CreateAccessor` zum Erstellen von Accessoren für die Parameter.

12. SQL Server ruft `ICommand::Execute` zum Ausführen des Befehls und Erstellen des Rowsets ab.

13. SQL Server verwendet die `IRowset`-Schnittstelle zum Navigieren und Verarbeiten der Zeilen der Tabelle. Verwenden Sie `IRowset::GetNextRows` zum Fetchen von Zeilen, `IRowset::RestartPosition` zum Ändern der Position an den Anfang des Rowsets und `IRowset::ReleaseRows` zum Freigeben von Zeilen.

#### <a name="provider-properties-of-interest-for-remote-query-execution"></a>Wichtige Anbietereigenschaften für die Remoteabfrageausführung

Wenn der Anbieter SQL-Features unterstützt, die nicht durch die in „DBPROP_SQLSUPPORT“ beschriebene Syntaxebene abgedeckt werden, können diese mithilfe verschiedener anbieterspezifischer Eigenschaften angegeben werden.

- SQLPROP_GROUPBY. Diese Eigenschaft ist für Anbieter von Interesse, die die minimale SQL-Ebene unterstützen. Diese Eigenschaft gibt an, dass der Anbieter die Klauseln „GROUP BY“ und „HAVING“ in der `SELECT`-Anweisung unterstützt. Außerdem gibt diese Eigenschaft auch an, dass der Anbieter die fünf Aggregatfunktionen „MIN“, „MAX“, „SUM“, „COUNT“ und „AVG“ unterstützt. „DISTINCT“ wird für das Argument dieser Aggregatfunktionen möglicherweise nicht vom Anbieter unterstützt.

- SQLPROP_SUBQUERIES. Diese Eigenschaft ist für Anbieter von Interesse, die die minimale SQL-Ebene unterstützen. Sie gibt an, dass der Anbieter (wie in der SQL-92-Einstiegsebene angegeben) Unterabfragen unterstützt. Dies schließt Unterabfragen in der `SELECT`-Liste und der `WHERE`-Klausel mit Unterstützung für korrelierte Unterabfragen sowie `IN`-, `EXISTS`-, `ALL`- und `ANY`-Operatoren mit ein.

- SQLPROP_DATELITERALS. Diese Eigenschaft ist für alle Anbieter von Interesse (einschließlich derjenigen, die die SQL-92-Einstiegsebene unterstützen). Unterstützung für die Standardliteralsyntax für datetime-Literale ist nicht Teil der SQL-92-Einstiegsebene. Diese SQL Server-spezifische Eigenschaft gibt an, dass der Anbieter (gemäß SQL-92-Standard) die datetime-Literalsyntax unterstützt.

- SQLPROP_ANSILIKE. Diese Eigenschaft ist für Anbieter von Interesse, die die minimale SQL-Ebene unterstützen. Diese Eigenschaft gibt an, dass der Anbieter (gemäß der SQL-92-Einstiegsebene) den `LIKE`-Operator unterstützt (\'%\' und \'_\' als Platzhalterzeichen). Dies wird für einen Anbieter verwendet, der die minimale SQL-Ebene unterstützt, da die minimale SQL-Ebene `LIKE` nicht unterstützt.

- SQLPROP_INNERJOIN. Diese Eigenschaft ist für Anbieter von Interesse, die die minimale SQL-Ebene unterstützen. Sie gibt an, dass mehrere Tabellen in der `FROM`-Klausel unterstützt werden. Dies wird für einen Anbieter verwendet, der nur die minimale SQL-Ebene unterstützt, da die minimale SQL-Ebene Joins nicht unterstützt. Dies gibt nicht die Unterstützung für explizite Joinschlüsselwörter und äußere Verknüpfungen an. Es wird nur angegeben, dass implizite Joins durch eine Liste von Tabellen in der `FROM`-Klausel unterstützt werden.

- SQLPROP_DYNAMICSQL. Diese Eigenschaft gibt die Unterstützung für `?` als Parametermarker an. Der Parametermarker sollte anstelle des Skalarelements in einer `WHERE`-Klausel oder der `SELECT`-Liste unterstützt werden. Die Unterstützung von `?`-Parametermarkern ermöglicht SQL Server das Senden parametrisierter Abfragen an den Anbieter.

- SQLPROP_NESTEDQUERIES. Diese Eigenschaft gibt die Unterstützung für geschachtelte „SELECT“-Anweisungen in der `FROM`-Klausel an (beispielsweise `SELECT * FROM (SELECT * FROM T))`). In vielen Fällen verwendet SQL Server geschachtelte `SELECT`-Anweisungen in der `FROM`-Klausel einer Abfrage, wenn die remote auszuführenden Abfragezeichenfolgen generiert werden. Da die Unterstützung geschachtelter `SELECT`-Anweisungen für die SQL-92-Einstiegsebene nicht erforderlich ist, werden von SQL Server keine Abfragen mit geschachtelten `SELECT`-Anweisungen an den Anbieter delegiert, es sei denn, der Anbieter legt diese Eigenschaft ebenfalls fest. Alternativ kann der Administrator die `Nested Queries`-Anbieteroption für den Anbieter auch so festlegen, dass SQL Server geschachtelte Abfragen für den Anbieter generiert.

Der Anbieter kann diese Eigenschaften mithilfe mehrerer SQL Server-spezifischer Eigenschaften namens `SQLPROPSET_OPTHINTS` und definierter `PROPID`-Werte unterstützen. Die Eigenschaften, die `SQLPROPSET_OPTHINTS` umfasst, und die beiden Eigenschaften werden mit den folgenden Konstanten definiert:

```
extern const GUID `SQLPROPSET_OPTHINTS` = { 0x2344480c, 0x33a7, 0x11d1, { 0x9b, 0x1a, 0x0, 0x60, 0x8, 0x26, 0x8b, 0x9e } };
enum SQLPROPERTIES {
SQLPROP_NESTEDQUERIES = 0x4,
SQLPROP_DYNAMICSQL = 0x5,
SQLPROP_GROUPBY = 0x6,
SQLPROP_DATELITERALS = 0x7,
SQLPROP_ANSILIKE = 0x8,
SQLPROP_INNERJOIN = 0x9,
SQLPROP_SUBQUERIES = 0x10
};
```

#### <a name="character-set-and-sort-order-implications"></a>Zeichensatz- und Sortierreihenfolgenimplikationen

SQL Server unterstützt das Angeben einer Sortierung für Zeichendaten auf Spaltenebene. Die Sortierung umfasst sowohl den Zeichensatz als auch die Sortierreihenfolgenspezifikation für nicht von Unicode unterstütze Zeichendaten (`char`- und `varchar`-Spalten). Bei Unicode-Daten (`nchar`- und `nvarchar`-Spalten) gibt die Sortierung nur die Sortierreihenfolge an.

SQL Server delegiert Zeichenfolgenvergleiche nur dann an den Anbieter, wenn der Zeichensatz (für nicht von Unicode unterstützte Daten), die Sortierreihenfolge und die vom Verbindungsserver verwendete Zeichenfolgenvergleichssemantik mit den vom lokalen Server verwendeten Optionen identisch sind.

Im Fall von SQL Server-Verbindungsservern bestimmt SQL Server automatisch die Sortierungskompatibilität. Bei anderen Anbietern muss der Administrator SQL Server die Sortierung von Zeichendaten von einem angegebenen Verbindungsserver angeben. In SQL Server wird eine neue Verbindungsserveroption namens `Collation Name` unterstützt. Wenn der Administrator festlegt, dass die vom Verbindungsserver übernommene Sortierungssemantik mit der SQL Server-Standardsortierung identisch ist, kann die `Collation Name`-Option auf diesen Sortierungsnamen festgelegt werden. Die Option `Collation Name` kann mithilfe der gespeicherten Systemprozedur `sp_serveroption` festgelegt werden. Diese Option sollte nur dann festgelegt werden, wenn die beiden folgenden Bedingungen erfüllt sind:

- Die Remotesortierreihenfolge und der Zeichensatz sind identisch mit der angegebenen SQL Server-Sortierung.

- Die vom OLE DB-Anbieter verwendete Zeichenfolgenvergleichssemantik folgt den SQL-92-Standardspezifikationen oder der gleichwertigen Vergleichssemantik von SQL Server.

Die in SQL Server 7.0 unterstützte Option „Kompatibel mit Sortierung“ wird aus Gründen der Abwärtskompatibilität auch weiterhin unterstützt. Wenn die Option auf TRUE festgelegt ist, entspricht dies dem Festlegen der Option „Sortierungsname“ auf die Standardsortierung der SQL Server-Masterdatenbank. Neue Anwendungen sollten die Option „Sortierungsname“ anstelle der Option „Kompatibel mit Sortierung“ verwenden.

### <a name="indexed-access"></a>Indizierter Zugriff

SQL Server verwendet einen vom Anbieter verfügbar gemachten Index zum Auswerten bestimmter Prädikate der verteilten Abfrage. Dieses Szenario ist nur für Indexanbieter möglich. Der Benutzer muss dann die Anbieteroption `Index as Access Path` festlegen. Im Folgenden finden Sie die wichtigsten grundlegenden Schritte, die SQL Server während der Verwendung eines Index zum Ausführen einer Abfrage für den Anbieter ausführt:

1. SQL Server öffnet über `IOpenRowset::OpenRowset` das Indexrowset mit dem vollständigen Tabellennamen und Indexnamen. Die vollständigen Tabellen- und Indexnamen werden (wie zuvor im Remoteabfrageszenario beschrieben) generiert.

1. Das Basistabellenrowset wird über `IOpenRowset::OpenRowset` mit dem vollständigen Tabellennamen geöffnet.

1. Die Bereiche für das Indexrowset werden basierend auf dem Abfrageprädikat durch `IRowsetIndex::SetRange` festgelegt.

1. Zeilen des Indexrowsets werden durch `IRowset` im Indexrowset überprüft.

1. SQL Server verwendet die Textmarkenspalte aus den abgerufenen Indexzeilen, um die entsprechenden Zeilen aus dem Basistabellenrowset durch `IRowsetLocate::GetRowsByBookmark` zu fetchen.

Die Rowseteigenschaften `DBPROP_IRowsetLocate` und `DBPROP_BOOKMARKS` sind für das Rowset erforderlich, das für die Basistabelle geöffnet wurde:

### <a name="pure-table-scans"></a>Reine Tabellenscans

SQL Server scannt die gesamte Remotetabelle des Anbieters und führt alle Abfrageauswertungen lokal durch. Das der Tabelle entsprechende Rowset wird mithilfe des Aufrufs von `IOpenRowset::OpenRowset` geöffnet. SQL Server erstellt den in `OPENROWSET` bereitgestellten Tabellennamen aus den Katalog-, Schema- und Objektnamensteilen wie folgt:

1. Jeder Namensteil wird in die Zeichenangabe des Anbieters (`DBLITERAL_QUOTE`) eingeschlossen und dann mit dem zwischen den beiden eingebetteten `DBLITERAL_CATALOG_SEPARATOR`-Zeichen verkettet.

1. Nachdem das Rowsetobjekt geöffnet wurde, verwendet SQL Server die `IColumnsInfo`-Schnittstelle, um zu überprüfen, ob die Metadaten der Ausführungszeit den Metadaten der Kompilierzeit für die Tabelle entsprechen.

1. SQL Server verwendet die `IRowset`-Schnittstelle zum Navigieren und Verarbeiten der Zeilen der Tabelle. Verwenden Sie `IRowset::GetNextRows` zum Fetchen von Zeilen, `IRowset::RestartPosition` zum Ändern der Position an den Anfang des Rowsets und `IRowset::ReleaseRows` zum Freigeben von Zeilen.

### <a name="update-and-delete-statements"></a>`UPDATE`- und `DELETE`-Anweisungen

Die folgenden Bedingungen müssen erfüllt sein, damit eine Remotetabelle aktualisiert oder aus einer von SQL Server verteilten Abfrage gelöscht werden kann:

- Der Anbieter muss Lesezeichen für das Rowset unterstützen, das über `IOpenRowset` in der aktualisierten oder gelöschten Tabelle geöffnet wurde.

- Der Anbieter muss die Schnittstellen `IRowsetLocate` und `IRowsetChange` im Rowset unterstützen, das über `IOpenRowset` für die aktualisierte oder gelöschte Tabelle geöffnet wurde.

- Die `IRowsetChange`-Schnittstelle muss das Update (`SetData`) unterstützen und Methoden (`DeleteRows`) löschen.

- Wenn der Anbieter `ITransactionLocal`, `UPDATE` und `DELETE` nicht unterstützt, sind Anweisungen nur dann zulässig, wenn die Option `Non-transacted` für diesen Anbieter festgelegt ist und die Anweisung nicht Teil einer Benutzertransaktion ist.

- Wenn der Anbieter `ITransactionJoin` nicht unterstützt, ist eine `UPDATE`- oder `DELETE`-Anweisung nur dann zulässig, wenn es nicht Teil einer Benutzertransaktion ist.

Die folgenden Rowseteigenschaften sind für das Rowset erforderlich, das für die aktualisierte Tabelle geöffnet wurde: `DBPROP_IRowsetLocate`, `DBPROP_IRowsetChange` und `DBPROP_BOOKMARKS`. Die Rowseteigenschaft `DBPROP_UPDATABILITY` wird auf `DBPROPVAL_UP_CHANGE` oder `DBPROPVAL_UP_DELETE` festgelegt, je nachdem, ob es sich um einen `UPDATE`- oder `DELETE`-Vorgang handelt.

Die folgenden wichtigen Schritte für den Anbieter werden für die Verarbeitung eines `UPDATE`- oder `DELETE`-Vorgangs ausgeführt:

1. SQL Server öffnet das Basistabellenrowset über die `IOpenRowset`-Schnittstelle. SQL Server erfordert die oben erwähnten Eigenschaften für das Rowset.

1. SQL Server bestimmt die qualifizierenden Zeilen, die aktualisiert oder gelöscht werden sollen.

1. SQL Server verwendet die Textmarken, um die qualifizierenden Zeilen über die `IRowsetLocate`-Schnittstelle zu positionieren.

1. Verwenden Sie `IRowsetChange::SetData` für `UPDATE`-Vorgänge oder `IRowsetChange::DeleteRows` für Löschvorgänge, um die erforderlichen Änderungen an den qualifizierenden Zeilen auszuführen.

### <a name="insert-statement"></a>`INSERT`-Anweisung

Die Bedingungen für die Unterstützung von `INSERT`-Anweisungen für eine Remotetabelle sind weniger streng als für `UPDATE`- und `DELETE`-Anweisungen:

- Der Anbieter muss `IRowsetChange::InsertRow` für das Rowset in der Basistabelle unterstützen, in die eingefügt wird.

- Wenn der Anbieter `ITransactionLocal` nicht unterstützt, sind `INSERT`-Anweisungen nur dann zulässig, wenn die Option `Non-transacted updates` für diesen Verbindungsserver festgelegt ist und die Anweisung nicht Teil einer Benutzertransaktion ist.

- Wenn der Anbieter `ITransactionJoin` nicht unterstützt, sind `INSERT`-Anweisungen nur dann zulässig, wenn sie nicht Teil einer Benutzertransaktion sind.

SQL Server verwendet `IOpenRowset::OpenRowset` zum Öffnen eines Rowsets in der Basistabelle und ruft `IRowsetChange::InsertRow` zum Einfügen neuer Zeilen in das Basisrowset ab.

### <a name="pass-through-queries"></a>Pass-Through-Abfragen

Dieses Szenario ähnelt dem Szenario zu Remoteabfragen mit der Ausnahme, dass der an `ICommand` übergebene Befehlstext eine Befehlszeichenfolge ist, die vom Benutzer gesendet und nicht von SQL Server interpretiert wird. SQL Server verwendet `DBGUID_DEFAULT` beim Abrufen von `ICommandText::SetCommandText` als Dialektidentifizierer. `DBGUID_DEFAULT` gibt an, dass der Anbieter den Standarddialekt verwenden soll. Wenn dieser Befehlstext mehr als ein Resultset zurückgibt (wenn der Befehl beispielsweise eine gespeicherte Prozedur aufruft, die mehrere Resultsets zurückgibt), verwendet SQL Server nur das erste Resultset aus dem Befehl.

Eine Liste aller von SQL Server verwendeten OLE DB-Schnittstellen finden Sie unter [Von SQL Server verwendete OLE DB-Schnittstellen](#appendixa).

### <a name="conclusion"></a>Zusammenfassung

Microsoft SQL Server bietet die stabilsten Tools für den Zugriff auf Daten aus heterogenen Datenquellen. Durch grundlegende Kenntnisse im Umgang mit den von SQL Server verfügbar gemachten OLE DB-Schnittstellen haben Entwickler ein hohes Maß an Kontrolle und Optionen hinsichtlich verteilter Abfragen.

## <a name="ole-db-interfaces-consumed-by-sql-server"></a><a name="appendixa"></a> Von SQL Server verwendete OLE DB-Schnittstellen

In der folgenden Tabelle werden alle OLE DB-Schnittstellen aufgelistet, die von SQL Server verwendet werden. Die Spalte „Erforderlich“ gibt an, ob die Schnittstelle Teil der minimalen von SQL Server benötigten OLE DB-Funktionalität oder optional ist. Wenn eine angegebene Schnittstelle nicht als erforderlich markiert ist, kann SQL Server zwar auch weiterhin auf den Anbieter zugreifen, aber einige bestimmte SQL Server-Funktionalitäten und Optimierungen sind dann für den Anbieter nicht möglich.

Im Fall der optionalen Schnittstellen gibt die Spalte „Szenarien“ ein oder mehrere der sechs Szenarios an, die die angegebene Schnittstelle verwenden. Die `IRowsetChange`-Schnittstelle in Basistabellenrowsets ist beispielsweise eine optionale Schnittstelle. Diese Schnittstelle wird in den `UPDATE`-, „DELETE“- und `INSERT`-Anweisungsszenarios verwendet. Wenn diese Schnittstelle nicht unterstützt wird, können „UPDATE“-, „DELETE“- und `INSERT`-Anweisungen nicht für diesen Anbieter unterstützt werden. Einige der anderen optionalen Schnittstellen sind in der Spalte „Szenarien“ als \"Leistung\" gekennzeichnet. Dies gibt an, dass die Schnittstellen eine bessere allgemeine Leistung aufweisen. Wenn die `IDBSchemaRowset`-Schnittstelle beispielsweise nicht unterstützt wird, muss SQL Server das Rowset zweimal öffnen: einmal für die Metadaten und einmal für die Ausführung der Abfrage. Durch die Unterstützung von `IDBSchemaRowset` wird die Leistung von SQL Server verbessert.

|Object|Schnittstelle|Erforderlich|Kommentare|Szenarien|
|:-----|:-----|:-----|:-----|:-----|
|Datenquellenobjekt|`IDBInitialize`|Ja|Initialisieren und Einrichten von Daten und Sicherheitskontext.| |
| |`IDBCreateSession`|Ja|Erstellen eines Datenbanksitzungsobjekts.| |
| |`IDBProperties`|Ja|Abrufen von Informationen über Möglichkeiten des Anbieters, Festlegen von Initialisierungseigenschaften und erforderlicher Eigenschaften: DBPROP_INIT_TIMEOUT.| |
| |`IDBInfo`|Nein|Wird zum Abrufen eines Zeichenliterals, Katalogs, Namens, Teils, Separators, Zeichens usw. verwendet.|Remoteabfrage.|
|Datenbanksitzungsobjekt|`IDBSchemaRowset`|Nein|Abrufen von Tabellen-/Spaltenmetadaten. Erforderliche Rowsets: `TABLES`, `COLUMNS`, `PROVIDER_TYPES`; andere verwendete Rowsets falls verfügbar: `INDEXES`, `TABLE_STATISTICS`.|Leistung, indizierter Zugriff.|
| |`IOpenRowset`|Ja|Öffnen eines Rowsets für eine Tabelle, einen Index oder ein Histogramm.| |
| |`IGetDataSource`|Ja|Wird verwendet, um von einem Datenbanksitzungsobjekt zu einem Datenquellenobjekt zurückzukehren.| |
| |`IDBCreateCommand`|Nein|Wird verwendet, um ein(e) Befehlsobjekt(abfrage) für Anbieter zu erstellen, die Abfragen unterstützen.|Remoteabfrage, Pass-Through-Abfrage.|
| |`ITransactionLocal`|Nein|Wird für transaktive Updates verwendet.|`UPDATE`-, `DELETE`- und `INSERT`-Anweisungen.|
| |`ITransactionJoin`|Nein|Wird zur Unterstützung verteilter Transaktionen verwendet.|`UPDATE`-, `DELETE`- und `INSERT`-Anweisungen, falls in Benutzertransaktion.|
|Rowsetobjekt|IRowset|Ja|Überprüfen von Zeilen.| |
| |`IAccessor`|Ja|Binden an Zeilen in einem Rowset.| |
| |`IColumnsInfo`|Ja|Abrufen von Informationen über Spalten in einem Rowset.| |
| |`IRowsetInfo`|Ja|Abrufen von Informationen über Rowseteigenschaften.| |
| |`IRowsetLocate`|Nein|Wird für `UPDATE`/`DELETE`-Vorgänge und indexbasierte Suchvorgänge benötigt; wird zum Suchen nach Zeilen nach Lesezeichen verwendet.|Indizierter Zugriff, `UPDATE`- und `DELETE`-Anweisungen.|
| |`IRowsetChange`|Nein|Erforderlich für `INSERTS`/`UPDATES`/ `DELETES` in einem Rowset. Rowsets für Basistabellen sollten diese Schnittstelle für `INSERT`-, `UPDATE`- und `DELETE`-Anweisungen unterstützen.|`UPDATE`-, `DELETE`- und `INSERT`-Anweisungen.|
| |`IConvertType`|Ja|Wird zum Überprüfen verwendet, ob das Rowset bestimmte Datentypkonvertierungen seiner Spalten unterstützt.| |
|Index|`IRowset`|Ja|Überprüfen von Zeilen.|Indizierter Zugriff, Leistung.|
| |`IAccessor`|Ja|Binden an Zeilen in einem Rowset.|Indizierter Zugriff, Leistung.|
| |`IColumnsInfo`|Ja|Abrufen von Informationen über Spalten in einem Rowset.|Indizierter Zugriff, Leistung.|
| |`IRowsetInfo`|Ja|Abrufen von Informationen über Rowseteigenschaften.|Indizierter Zugriff, Leistung.|
| |`IRowsetIndex`|Ja|Wird nur für Rowsets in einem Index benötigt; verwendet für Indizierungsfunktionen (Bereich festlegen, Suchen).|Indizierter Zugriff, Leistung.|
|Get-Help|`ICommand`|Ja| |Remoteabfrage, Pass-Through-Abfrage.|
| |`ICommandText`|Ja|Wird zum Definieren des Abfragetexts verwendet.|Remoteabfrage, Pass-Through-Abfrage.|
| |`IColumnsInfo`|Ja|Wird zum Abfragen von Spaltenmetadaten für Abfrageergebnisse verwendet.|Remoteabfrage, Pass-Through-Abfrage.|
| |`ICommandProperties`|Ja|Wird zum Angeben erforderlicher Eigenschaften für Rowsets verwendet, die vom Befehl zurückgegeben werden.|Remoteabfrage, Pass-Through-Abfrage.|
| |`ICommandWithParameters`|Nein|Wird zur Ausführung einer parametrisierten Abfrage verwendet.|Remoteabfrage, Leistung.|
| |`ICommandPrepare`|Nein|Wird zum Vorbereiten eines Befehls verwendet, um Metadaten zu erhalten (wird in Pass-Through-Abfragen verwendet, falls diese verfügbar sind).|Remoteabfrage, Leistung.|
|Error-Objekt|`IErrorRecords`|Ja|Wird zum Abrufen eines Zeigers auf eine `IErrorInfo`-Schnittstelle verwendet, die einem einzelnen Fehlerdatensatz entspricht.| |
| |`IErrorInfo`|Ja|Wird zum Abrufen eines Zeigers auf eine `IErrorInfo`-Schnittstelle verwendet, die einem einzelnen Fehlerdatensatz entspricht.| |
|Ein beliebiges Objekt|`ISupportErrorInfo`|Nein|Wird zum Überprüfen verwendet, ob eine bestimmte Schnittstelle Error-Objekte unterstützt.| |
|  |  |  |  |  |

>[!NOTE]
>Die `Index`-, `Command`- und `Error`-Objekte sind nicht obligatorisch. Wenn diese jedoch unterstützt werden, sind die aufgelisteten Schnittstellen obligatorisch (wie in der Spalte „Erforderlich“ angegeben).

## <a name="sql-subset-used-for-generating-remote-queries"></a><a name="appendixb"></a>SQL-Teilmengen zum Generieren von Remoteabfragen

Die SQL-Teilmenge, die der SQL Server-Abfrageprozessor für einen SQL-Befehlsanbieter generiert, hängt (wie von der `DBPROP_SQLSUPPORT`-Eigenschaft angegeben) von der vom Anbieter unterstützten Syntaxebene ab.

SQL-Befehlsanbieter, die die SQL-Einstiegsebene oder ODBC Core unterstützen

SQL Server verwendet die folgende Teilmenge der SQL-Sprache für von SQL-Befehlsanbietern ausgewertete Abfragen, die entweder die SQL-92-Einstiegsstufe oder ODBC Core unterstützen:

1. `SELECT`-Anweisungen mit den Klauseln `SELECT`, `FROM`, `WHERE`, `GROUP BY`, `UNION`, `UNION ALL`, `ORDER BY DESC`, `ASC` und `HAVING`.

1. `UNION` und `UNION ALL` werden nur für Anbieter generiert, die die SQL-92-Einstiegsstufe und nicht ODBC Core unterstützen.

1. `SELECT`-Klausel:

   - Skalare Unterabfragen in der `SELECT`-Liste.

   - Spaltenaliase ohne das `AS`-Schlüsselwort.

1. `FROM`-Klausel:

   - Explizite Joinschlüsselwörter werden nicht verwendet; durch Kommata getrennte Tabellennamen werden zum Angeben innerer Joins verwendet, und äußere Verknüpfungen werden in Remoteabfragen nicht angegeben.

   - Geschachtelte Abfragen in der Form `FROM` (`<alias>`) `<nested query>`.

   - Tabellenaliase ohne das AS-Schlüsselwort.

1. Die `WHERE`-Klausel verwendet Unterabfragen mit `NOT` `EXISTS`, `ANY` und `ALL`.

1. Ausdrücke:

   - Verwendete Aggregatfunktionen: `MIN([DISTINCT])`, `MAX([DISTINCT])`, `COUNT([DISTINCT])`, `SUM([DISTINCT])`, `AVG([DISTINCT])` und `COUNT(*)`.

   - Vergleichsoperatoren: `<`, `=`, `<=`, `>`, `<>`, `>=`, `IS NULL` und `IS NOT NULL`.

   - Boolesche Operatoren: `AND`, `OR` und `NOT`.

 - Arithmetische Operatoren: `+`, `-`, `*` und `/`.

1. Konstanten:

- Numerische und Geldliterale sind immer von `( )` umgeben.

- Zeichenliterale werden in `' '` eingeschlossen.

SQL-Befehlsanbieter, die die SQL-Mindeststufe unterstützen

SQL Server generiert mithilfe der folgenden Grammatik SQL für SQL-Befehlsanbieter, die die SQL-Mindeststufe unterstützen.

Diese Grammatik wurde mithilfe der in ODBC 3.0 beschriebenen minimalen SQL-Grammatik abgeleitet. Alle Unterschiede zu dieser Grammatik sind hervorgehoben. Bei den in `*bold italics`* angezeigten Elementen handelt es sich um die in ODBC 3.0 beschriebenen zur minimalen SQL-Grammatik hinzugefügten Elemente. Bei den in grün angezeigten gelöschten Elementen handelt es sich um die in dieser Grammatik gelöschten Elemente.

*select-statement* ::=

`SELECT [ALL | DISTINCT] *select-list* FROM *table-reference-list*[WHERE *search-condition*] [order-by-clause]`

`SELECT`-Klausel

select-list ::= `*` | `select-sublist [, select-sublist]...`

select-sublist ::= expression * [`alias`]*

`*alias ::= user-defined-name`*

`FROM clause`

table-reference-list ::= table-reference

table-identifier ::= user-defined-name

table-name ::= table-identifier

table-reference ::= table-name

`WHERE clause`

search-condition ::= boolean-term \[OR search-condition\]

boolean-term ::= boolean-factor \[AND boolean-term\]

boolean-factor ::= \[NOT\] boolean-primary

boolean-primary ::= comparison-predicate \| ( search-condition )

comparison-predicate ::= expression comparison-operator expression

*\| `expression IS \[NOT\] NULL`*

comparison-operator ::= `< \| >` \| `<= \| >`= \| = \| `<>`

`ORDER BY clause`

order-by-clause ::= ORDER BY sort-specification \[, sort-specification\]\..

sort-specification ::= { \| column-name } \[ASC \| DESC\]

`Common syntactic elements`

expression ::= term \| expression {+\|--} term

term ::= factor \| term {\*\|/} factor

factor ::= \[+\|--\] primary

primary ::= column-name

\|-Literal

\| ( expression )

column-name ::= \[table-name.\]column-identifier

literal ::= character-string-literal

*\| integer-literal*

*\| exact-numeric-literal*

character-string-literal ::= \'{character}...\'

(„character“ ist ein beliebiges Zeichen im Zeichensatz des Treibers/der Datenquelle. Verwenden Sie zwei literale Anführungszeichen (\'\'), um ein einzelnes Anführungszeichen (\') in ein Zeichenfolgenliteral einzufügen.)

`*integer-literal ::=* \[*+ \| -*\] *unsigned-integer`*

`*exact-numeric-literal::=* \[*+ \| -*\] *unsigned-integer* \[*period unsigned-integer*\]`

`*\| period unsigned-integer`*

base-table-name ::= base-table-identifier

base-table-identifier ::= user-defined-name

column-identifier ::= user-defined-name

user-defined-name ::= letter\[digit \| letter \| _\]\..

unsigned-integer ::= {digit}...

digit ::= 0 \| 1 \| 2 \| 3 \| 4 \| 5 \| 6 \| 7 \| 8 \| 9

period ::= . 

## <a name="sql-server-specific-properties"></a><a name="appendixc"></a>SQL Server-spezifische Eigenschaften

```
enum SQLPROPERTIES
       {
       SQLPROP_NOHPNEEDED = 0x1,
       SQLPROP_FREETHREADED = 0x2,
       SQLPROP_UMSENABLED = 0x3,
       SQLPROP_NESTEDQUERIES = 0x4,
       SQLPROP_DYNAMICSQL = 0x5,
       SQLPROP_GROUPBY = 0x6,
       SQLPROP_DATELITERALS = 0x7,
       SQLPROP_ANSILIKE = 0x8,
       SQLPROP_INNERJOIN = 0x9,
       SQLPROP_SUBQUERIES = 0x10, 
       SQLPROP_PARALLELSCAN = 0x11,
       SQLPROP_COLUMNCOLLATION = 0x12,
       SQLPROP_CARDINALITY = 0x13,
       SQLPROP_SIMPLEUPDATES = 0x14,
       SQLPROP_SQLLIKE = 0x15,
       SQLPROP_BITREMOTING = 0x16,
       SQLPROP_UNICODELITERALS = 0x17,
       SQLPROP_USELATESTCOLLATIONVERSION = 0x18
       };

```
