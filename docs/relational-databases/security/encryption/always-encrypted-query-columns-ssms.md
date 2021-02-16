---
title: Abfragen von Spalten mithilfe von Always Encrypted mit SQL Server Management Studio | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Spalten in Always Encrypted mithilfe von SQL Server Management Studio abgefragt werden. Abrufen von in verschlüsselten Spalten gespeicherten Chiffretextwerten oder Textwerten
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Always Encrypted, configure with SSMS
ms.assetid: 29816a41-f105-4414-8be1-070675d62e84
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ff7924bf1262ee29977d16548b6548c112bc8b5d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345448"
---
# <a name="query-columns-using-always-encrypted-with-sql-server-management-studio"></a>Abfragen von Spalten mithilfe von Always Encrypted mit SQL Server Management Studio
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

In diesem Artikel wird beschrieben, wie Sie mithilfe von [SQL Server Management Studio (SSMS)](../../../ssms/download-sql-server-management-studio-ssms.md) mit [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md) verschlüsselte Spalten abfragen. Mit SSMS können Sie folgende Aktionen ausführen:
- Abrufen von in verschlüsselten Spalten gespeicherten Chiffretextwerten 
- Abrufen von in verschlüsselten Spalten gespeicherten Klartextwerten   
- Senden von Klartextwerten an verschlüsselte Spalten (z.B. in `INSERT`- oder `UPDATE`-Anweisungen und als Nachschlageparameter von `WHERE`-Klauseln in `SELECT`-Anweisungen).

## <a name="retrieving-ciphertext-values-stored-in-encrypted-columns"></a>Abrufen von in verschlüsselten Spalten gespeicherten Chiffretextwerten    
Für die Ausführung von SELECT-Abfragen, die in verschlüsselten Spalten gespeicherte Chiffretextdaten abrufen (ohne dabei die Daten zu entschlüsseln), benötigen Sie keinen Zugriff auf datenschützende Spaltenhauptschlüssel. So rufen Sie in SSMS Werte aus einer verschlüsselten Spalte als Chiffretext ab:

1. Stellen Sie sicher, dass Sie auf die Metadaten zu den Schlüsseln zugreifen können, die die Spalten schützen, für die Sie Ihre Abfrage ausführen. Sie müssen zwar nicht auf die eigentlichen Spaltenhauptschlüssel zugreifen können, jedoch benötigen Sie Berechtigungen auf Datenbankebene, um Metadatenobjekte von Spaltenhauptschlüsseln und Spaltenverschlüsselungsschlüsseln in Datenbanken anzuzeigen. Ausführliche Informationen finden Sie weiter unten unter [Berechtigungen zum Abfragen verschlüsselter Spalten](#permissions-for-querying-encrypted-columns).
1. Vergewissern Sie sich, dass Always Encrypted für die Datenbankverbindung des Fensters „Abfrage-Editor“ deaktiviert ist, in dem Sie eine `SELECT`-Abfrage zum Abrufen von Chiffretextwerten ausführen. Weitere Informationen finden Sie unter [Aktivieren und Deaktivieren von Always Encrypted für eine Datenbankverbindung](#en-dis) weiter unten.     
1. Führen Sie Ihre `SELECT`-Abfrage aus. Alle aus verschlüsselten Spalten abgerufenen Daten werden als (verschlüsselte) Binärwerte zurückgegeben.   

### <a name="example"></a>Beispiel
Sofern `SSN` eine verschlüsselte Spalte in der Tabelle `Patients` ist, ruft die folgende Abfrage die binären Chiffretextwerte ab, wenn Always Encrypted für die Datenbankverbindung deaktiviert ist.   

![Screenshot: Abfrage „SELECT [SSN] FROM [dbo].[Patients]“ und die Ergebnisse der Abfrage, dargestellt als binäre Chiffretextwerte](../../../relational-databases/security/encryption/media/always-encrypted-ciphertext.png)
 
## <a name="retrieving-plaintext-values-stored-in-encrypted-columns"></a>Abrufen von in verschlüsselten Spalten gespeicherten Klartextwerten    
So rufen Sie Werte aus einer verschlüsselten Spalte als Klartext ab (um die Werte zu entschlüsseln)   
1. Stellen Sie sicher, dass Sie auf die Spaltenhauptschlüssel und die Metadaten zu den Schlüsseln zugreifen können, die die Spalten schützen, gegen die Sie Ihre Abfrage ausführen. Ausführliche Informationen finden Sie weiter unten unter [Berechtigungen zum Abfragen verschlüsselter Spalten](#permissions-for-querying-encrypted-columns).
1.  Vergewissern Sie sich, dass Always Encrypted für die Datenbankverbindung des Fensters „Abfrage-Editor“ aktiviert ist, in dem Sie eine `SELECT`-Abfrage zum Abrufen und Entschlüsseln Ihrer Daten ausführen. Dadurch wird der (von SSMS verwendete) .NET Framework-Datenanbieter für SQL Server angewiesen, die verschlüsselten Spalten im Abfrageresultset zu entschlüsseln. Weitere Informationen finden Sie unter [Aktivieren und Deaktivieren von Always Encrypted für eine Datenbankverbindung](#en-dis) weiter unten.
1.  Führen Sie Ihre `SELECT`-Abfrage aus. Aus verschlüsselten Spalten abgerufene Daten werden als Klartextwerte der ursprünglichen Datentypen zurückgegeben.
 
### <a name="example"></a>Beispiel
Wenn SSN eine verschlüsselte Spalte `char(11)` in der Tabelle `Patients` ist, gibt die unten gezeigte Abfrage Klartextwerte zurück, sofern Always Encrypted für die Datenbankverbindung aktiviert ist und Sie Zugriff auf den Spaltenhauptschlüssel haben, der für die Spalte `SSN` konfiguriert ist.   

![Screenshot: Abfrage „SELECT [SSN] FROM [Clinic].[dbo].[Patients]“ und die Ergebnisse der Abfrage, dargestellt als Nur-Text-Werte](../../../relational-databases/security/encryption/media/always-encrypted-plaintext.png)
 
## <a name="sending-plaintext-values-targeting-encrypted-columns"></a>Senden von Klartextwerten an verschlüsselte Spalten       
So führen Sie eine Abfrage aus, die einen Wert an eine verschlüsselte Spalte sendet, z.B. eine Abfrage, die einen in einer verschlüsselten Spalte gespeicherten Wert einfügt, aktualisiert oder danach filtert:

1. Stellen Sie sicher, dass Sie auf die Spaltenhauptschlüssel und die Metadaten zu den Schlüsseln zugreifen können, die die Spalten schützen, gegen die Sie Ihre Abfrage ausführen. Weitere Informationen finden Sie weiter unten unter [Berechtigungen zum Abfragen verschlüsselter Spalten](#permissions-for-querying-encrypted-columns).
1.  Vergewissern Sie sich, dass Always Encrypted für die Datenbankverbindung des Fensters „Abfrage-Editor“ aktiviert ist, in dem Sie eine `SELECT`-Abfrage zum Abrufen und Entschlüsseln Ihrer Daten ausführen. Dadurch wird der (von SSMS verwendete) .NET Framework-Datenanbieter für SQL Server angewiesen, die verschlüsselten Spalten im Abfrageresultset zu entschlüsseln. Weitere Informationen finden Sie unter [Aktivieren und Deaktivieren von Always Encrypted für eine Datenbankverbindung](#en-dis) weiter unten.
1. Stellen Sie sicher, dass „Parametrisierung für Always Encrypted“ für das Fenster „Abfrage-Editor“ aktiviert ist. (Erfordert mindestens SSMS Version 17.0) Deklarieren Sie eine Transact-SQL-Variable, und initialisieren Sie sie mit einem Wert, der an die Datenbank gesendet werden soll (Einfügen, Aktualisieren oder Filtern nach). Details finden Sie weiter unten unter [Parametrisierung für Always Encrypted](#param) .

1. Führen Sie die Abfrage aus, um den Wert der Transact-SQL-Variablen an die Datenbank zu senden. SSMS wandelt die Variable in einen Abfrageparameter um und verschlüsselt dessen Wert, ehe er an die Datenbank gesendet wird.   

### <a name="example"></a>Beispiel
Wenn `SSN` eine verschlüsselte Spalte `char(11)` in der Tabelle `Patients` ist, versucht das folgende Skript eine Zeile zu finden, die `'795-73-9838'` in der Spalte „SSN“ enthält und den Wert der Spalte `LastName` zurückgibt. Vorausgesetzt wird, dass Always Encrypted für die Datenbankverbindung aktiviert ist, dass „Parametrisierung für Always Encrypted“ für das Fenster „Abfrage-Editor“ aktiviert ist und dass Sie Zugriff auf den Spaltenhauptschlüssel haben, der für die Spalte `SSN` konfiguriert ist.   

![Screenshot: Abfrage „DECLARE @SSN CHAR(11) = '795-73-9838' SELECT [LastName] FROM [dbo].[Patients] WHERE [SSN] = @SSN“ und die Ergebnisse der Abfrage](../../../relational-databases/security/encryption/media/always-encrypted-patients.png)

## <a name="permissions-for-querying-encrypted-columns"></a>Berechtigungen zum Abfragen verschlüsselter Spalten

Zum Anwenden von Abfragen auf verschlüsselte Spalten, einschließlich Abfragen zum Abrufen von Daten in Chiffretext, benötigen Sie für die Datenbank die Berechtigungen `VIEW ANY COLUMN MASTER KEY DEFINITION` und `VIEW ANY COLUMN ENCRYPTION KEY DEFINITION` .

Zusätzlich zu den oben aufgeführten Berechtigungen benötigen Sie zum Entschlüsseln von Abfrageergebnissen oder Verschlüsseln von Abfrageparametern (die durch parametrisierte Transact-SQL-Anweisungen erstellt wurden) auch Zugriff auf den Spaltenhauptschlüssel, der die Zielspalten schützt:

- **Zertifikatspeicher – Lokaler Computer**: Sie benötigen `Read`-Zugriff auf das Zertifikat, das als Spaltenhauptschlüssel verwendet wird, oder Administratorrechte auf dem Computer.   
- **Azure Key Vault**: Sie benötigen die Berechtigungen `get`, `unwrapKey` und `verify` für den Tresor, der den Spaltenhauptschlüssel enthält.
- **Schlüsselspeicheranbieter (KSP)** : Die erforderlichen Berechtigungen und Anmeldeinformationen, zu deren Eingabe Sie möglicherweise aufgefordert werden, wenn Sie einen Schlüsselspeicher oder einen Schlüssel verwenden, hängen von der Konfiguration des Speichers und des Schlüsselspeicheranbieters ab.   
- **Kryptografiedienstanbieter (CSP)** : Die erforderlichen Berechtigungen und Anmeldeinformationen, zu deren Eingabe Sie möglicherweise aufgefordert werden, wenn Sie einen Schlüsselspeicher oder einen Schlüssel verwenden, hängen von der Konfiguration des Speichers und des Kryptografiedienstanbieters ab.

Weitere Informationen finden Sie unter [Create and Store Column Master Keys (Always Encrypted)](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)(Erstellen und Speichern von Spaltenhauptschlüsseln (Always Encrypted)).

## <a name="enabling-and-disabling-always-encrypted-for-a-database-connection"></a><a name="en-dis"></a> Aktivieren und Deaktivieren von Always Encrypted für eine Datenbankverbindung   
Wenn Sie in SSMS eine Verbindung mit einer Datenbank herstellen, können Sie Always Encrypted für die Datenbankverbindung entweder aktivieren oder deaktivieren. Always Encrypted ist standardmäßig deaktiviert. 

Durch Aktivieren von Always Encrypted für eine Datenbankverbindung wird der .NET Framework-Datenanbieter für SQL Server, der von SQL Server Management Studio verwendet wird, aufgefordert, die folgenden Aufgaben transparent auszuführen:   
-   Entschlüsseln aller Werte, die aus verschlüsselten Spalten abgerufen und in Abfrageergebnissen zurückgegeben werden   
-   Verschlüsseln der Werte der parametrisierten Transact-SQL-Variablen für verschlüsselte Spalten in der Zieldatenbank   

Wenn Sie Always Encrypted für eine Verbindung nicht aktivieren, wird der von SSMS verwendete .NET Framework-Datenanbieter für SQL Server nicht versuchen, die Abfrageparameter zu verschlüsseln oder Ergebnisse zu entschlüsseln.

Sie können Always Encrypted aktivieren oder deaktivieren, wenn Sie eine neue Verbindung erstellen oder eine vorhandene Verbindung mithilfe des Dialogfelds **Verbindung mit Server herstellen** ändern. 

So aktivieren (oder deaktivieren) Sie Always Encrypted:
1. Öffnen Sie das Dialogfeld **Verbindung mit Server herstellen** (Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit einer SQL Server-Instanz](../../../ssms/quickstarts/ssms-connect-query-sql-server.md#connect-to-a-sql-server-instance)).
1. Klicken Sie auf **Optionen>>** .
1. Bei Verwendung von SSMS 18 oder höher:
    1. Wählen Sie die Registerkarte **Always Encrypted** aus.
    1. Wählen Sie **Always Encrypted aktivieren (Spaltenverschlüsselung)** aus, um Always Encrypted zu aktivieren. Stellen Sie sicher, dass **Always Encrypted aktivieren (Spaltenverschlüsselung)** nicht ausgewählt ist, wenn Sie Always Encrypted deaktivieren wollen.
1. Bei Verwendung von SSMS 17 oder älter:
    1. Wählen Sie die Registerkarte **Weitere Eigenschaften** aus.
    1. Geben Sie `Column Encryption Setting = Enabled` ein, um Always Encrypted zu aktivieren. Geben Sie zum Deaktivieren von Always Encrypted `Column Encryption Setting = Disabled` an, oder entfernen Sie die Einstellung **Spaltenverschlüsselungseinstellung** von der Registerkarte **Zusätzliche Eigenschaften** (der Standardwert ist **Deaktiviert**).   
 1. Klicken Sie auf **Verbinden**.

Wenn Sie Anweisungen ausführen möchten, die eine serverseitige Secure Enclave nutzen, müssen Sie bei Verwendung von [Always Encrypted mit Secure Enclaves](always-encrypted-enclaves.md) zusätzlich zum Aktivieren von Always Encrypted für die Verbindung eine URL zum Nachweis von Enclaves angeben. Ausführliche Informationen finden Sie unter [Voraussetzungen für das Ausführen von T-SQL-Anweisungen mithilfe von Enclaves in SSMS](always-encrypted-enclaves-query-columns.md#prerequisites-for-running-t-sql-statements-using-enclaves-in-ssms).

> [!TIP]
> So schalten Sie zwischen dem Aktivieren und Deaktivieren von Always Encrypted für ein vorhandenes Fenster „Abfrage-Editor“ um:   
> 1.    Klicken Sie im Fenster „Abfrage-Editor“ mit der rechten Maustaste auf eine beliebige Stelle.
> 2.    Wählen Sie **Verbindung** > **Verbindung ändern...** aus. Dadurch wird das Dialogfeld **Verbindung mit Server herstellen** für die aktuelle Verbindung für das Abfrage-Editor-Fenster geöffnet. 
> 2.    Aktivieren oder deaktivieren Sie Always Encrypted, indem Sie die oben stehenden Schritte ausführen, und klicken Sie auf **Verbinden**.  
   
## <a name="parameterization-for-always-encrypted"></a><a name="param"></a>Parametrisierung für Always Encrypted   
 
„Parametrisierung für Always Encrypted“ ist ein Feature in SQL Server Management Studio, das Transact-SQL-Variablen automatisch in Abfrageparameter (Instanzen der [„SqlParameter“-Klasse](/dotnet/api/system.data.sqlclient.sqlparameter)) konvertiert. (Erfordert mindestens SSMS Version 17.0) Dies ermöglicht dem zugrunde liegenden .NET Framework-Datenanbieter für SQL Server das Erkennen von Daten für verschlüsselte Spalten und das Verschlüsseln dieser Daten, ehe sie an die Datenbank gesendet werden. 
  
Ohne Parametrisierung übergibt der .NET Framework-Datenanbieter jede Ihrer Anweisungen, die Sie im Abfrage-Editor erstellen, als nicht parametrisierte Abfrage. Wenn die Abfrage Literale oder Transact-SQL-Variablen für verschlüsselte Spalten enthält, kann der .NET Framework-Datenanbieter für SQL Server diese nicht erkennen und verschlüsseln, ehe die Abfrage an die Datenbank gesendet wird. Daher misslingt die Abfrage aufgrund eine Typkonflikts (zwischen dem Literal oder der Transact-SQL-Variablen in Klartext und der verschlüsselten Spalte). Die folgende Abfrage misslingt beispielsweise ohne Parametrisierung, sofern die Spalte `SSN` verschlüsselt ist.   

```sql
DECLARE @SSN NCHAR(11) = '795-73-9838'
SELECT * FROM [dbo].[Patients]
WHERE [SSN] = @SSN
```

### <a name="enabling-and-disabling-parameterization-for-always-encrypted"></a>Aktivieren oder Deaktivieren der Parametrisierung für Always Encrypted

„Parametrisierung für Always Encrypted“ ist standardmäßig deaktiviert.

So aktivieren Sie „Parametrisierung für Always Encrypted“ für das aktuelle Fenster „Abfrage-Editor“

1. Wählen Sie im Hauptmenü **Abfrage** aus.
2. Wählen Sie **Abfrageoptionen...** aus.
3. Navigieren Sie zu **Ausführung** > **Erweitert**.
4. Aktivieren bzw. deaktivieren **Parametrisierung für Always Encrypted**.
5. Klicken Sie auf **OK**.

So aktivieren oder deaktivieren Sie „Parametrisierung für Always Encrypted“ für künftige „Abfrage-Editor“-Fenster

1. Wählen Sie im Hauptmenü **Tools** aus.
2. Wählen Sie **Optionen...** aus.
3. Navigieren Sie zu **Abfrageausführung** > **SQL Server** > **Erweitert**.
4. Aktivieren bzw. deaktivieren **Parametrisierung für Always Encrypted**.
5. Klicken Sie auf **OK**.

Bei Ausführung einer Abfrage im Fenster „Abfrage-Editor“, das eine Datenbankverbindung mit aktiviertem Always Encrypted aufweist, ohne dass die Parametrisierung für das Fenster „Abfrage-Editor“ aktiviert ist, werden Sie zur Aktivierung aufgefordert.

> [!NOTE]
> Die Parametrisierung für Always Encrypted funktioniert nur in Abfrage-Editor-Fenstern mit Datenbankverbindungen, für die Always Encrypted aktiviert ist (siehe [Aktivieren und Deaktivieren der Parametrisierung für Always Encrypted](#enabling-and-disabling-parameterization-for-always-encrypted)). Transact-SQL-Variablen werden nicht parametrisiert, wenn das Fenster „Abfrage-Editor“ eine Datenbankverbindung ohne aktiviertes Always Encrypted aufweist.

### <a name="how-parameterization-for-always-encrypted-works"></a>Funktionsweise von „Parametrisierung für Always Encrypted“

Wenn sowohl „Parametrisierung für Always Encrypted“ als auch das Always Encrypted-Verhalten für die Datenbankverbindung im Fenster „Abfrage-Editor“ aktiviert sind, versucht SQL Server Management Studio die Parametrisierung von Transact-SQL-Variablen, die die folgenden Bedingungen erfüllen:

- Sind in der gleichen Anweisung deklariert und initialisiert (Inline-Initialisierung). Variablen, die mit getrennten `SET` -Anweisungen deklariert wurden, werden nicht parametrisiert.
- Sind mithilfe eines einzelnen Literals initialisiert. Variablen, die mithilfe von Ausdrücken initialisiert wurden, die Operatoren oder Funktionen enthalten, werden nicht parametrisiert.

Es folgen Beispiele von Variablen, die von SQL Server Management Studio parametrisiert werden.

```sql
DECLARE @SSN char(11) = '795-73-9838';
   
DECLARE @BirthDate date = '19990104';
DECLARE @Salary money = $30000;
```

Und hier sehen Sie einige Beispiele von Variablen, bei denen SQL Server Management Studio keine Parametrisierung versucht:

```sql
DECLARE @Name nvarchar(50); --Initialization separate from declaration
SET @Name = 'Abel';

DECLARE @StartDate date = GETDATE(); -- a function used instead of a literal

DECLARE @NewSalary money = @Salary * 1.1; -- an expression used instead of a literal
```
 
Voraussetzungen für eine erfolgreiche Parametrisierung:   
- Der Typ des Literals, der für die Initialisierung der zu parametrisierenden Variablen verwendet wird, muss dem Typ in der Variablendeklaration entsprechen.   
- Wenn die deklarierten Variablen vom Typ „date“ oder „time“ sind, müssen diese mithilfe einer Zeichenfolge in einem der folgenden [ISO 8601-kompatiblen Formate](../../../t-sql/functions/cast-and-convert-transact-sql.md#date-and-time-styles) initialisiert werden.    

Es folgen Beispiele von Transact-SQL-Variablendeklarationen, die zu Parametrisierungsfehlern führen:   
```sql
DECLARE @BirthDate date = '01/04/1999' -- unsupported date format   
   
DECLARE @Number int = 1.1 -- the type of the literal does not match the type of the variable   
```
SQL Server Management Studio nutzt Intellisense, um Sie zu informieren, welche Variablen erfolgreich parametrisiert werden können und welche Parametrisierungsversuche misslingen (samt Grund).   

Eine Deklaration einer Variablen, die erfolgreich parametrisiert werden kann, wird im Abfrage-Editor mit einer Warnunterstreichung markiert. Wenn Sie den Mauszeiger über einer Deklarationsanweisung bewegen, die mit einer Warnunterstreichung markiert wurde, werden die Ergebnisse des Parametrisierungsvorgangs, einschließlich der Werte der Haupteigenschaften des resultierenden [SqlParameter](/dotnet/api/system.data.sqlclient.sqlparameter) -Objekts (dem die Variable zugeordnet ist) angezeigt: [SqlDbType](/dotnet/api/system.data.sqlclient.sqlparameter.sqldbtype), [Size](/dotnet/api/system.data.sqlclient.sqlparameter.size), [Precision](/dotnet/api/system.data.sqlclient.sqlparameter.precision), [Scale](/dotnet/api/system.data.sqlclient.sqlparameter.scale) und [SqlValue](/dotnet/api/system.data.sqlclient.sqlparameter.sqlvalue). Eine vollständige Liste aller Variablen, die erfolgreich parametrisiert wurden, finden Sie auf der Registerkarte **Warnung** der Ansicht **Fehlerliste** . Zum Öffnen der Ansicht **Fehlerliste** wählen im Hauptmenü **Ansicht** und dann **Fehlerliste** aus.    

Wenn SQL Server Management Studio versucht hat, eine Variable zu parametrisieren, aber die Parametrisierung misslungen ist, wird die Deklaration der Variablen mit einer Fehlerunterstreichung gekennzeichnet. Wenn Sie den Mauszeiger über der Deklarationsanweisung bewegen, die mit einer Fehlerunterstreichung markiert wurde, erhalten Sie Informationen zum Fehler. In der Ansicht **Fehlerliste** sehen Sie auf der Registerkarte **Fehler** die vollständige Liste von Parametrisierungsfehlern für alle Variablen. Zum Öffnen der Ansicht **Fehlerliste** wählen im Hauptmenü **Ansicht** und dann **Fehlerliste** aus.   

Das nachstehende Bildschirmfoto zeigt ein Beispiel von sechs Variablendeklarationen. SQL Server Management Studio hat die ersten drei Variablen erfolgreich parametrisiert. Die letzten drei Variablen haben nicht die Bedingungen für die Parametrisierung erfüllt, weshalb SQL Server Management Studio nicht versucht hat, sie zu parametrisieren (ihre Deklarationen sind nicht gekennzeichnet).

![Screenshot: Beispiel für sechs Variablendeklarationen, drei davon erfolgreich parametrisiert und drei mit Fehlern und den zugehörigen Warnmeldungen](../../../relational-databases/security/encryption/media/always-encrypted-parameter-warnings.png)
 
Ein weiteres nachstehendes Beispiel zeigt zwei Variablen, die die Bedingungen für die Parametrisierung erfüllt haben, doch der Parametrisierungsversuch ist misslungen, weil die Variablen falsch initialisiert waren.    
 
![Screenshot: Beispiel für zwei Variablendeklarationen, bei denen letztendlich ein Fehler auftritt, und die zugehörigen Fehlermeldungen](../../../relational-databases/security/encryption/media/always-encrypted-error.png)
 
> [!NOTE]
> Da Always Encrypted eine beschränkte Teilmenge von Typumwandlungen unterstützt, ist es in vielen Fällen erforderlich, dass der Datentyp einer Transact-SQL-Variablen dem Typ der Spalte in der Zieldatenbank entspricht. Angenommen, der Typ der Spalte `SSN` in der Tabelle `Patients` ist `char(11)`. Die folgende Abfrage misslingt, da der Typ der Variablen `@SSN` (der `nchar(11)`ist) nicht dem Typ der Spalte entspricht.   

```sql
DECLARE @SSN nchar(11) = '795-73-9838'
SELECT * FROM [dbo].[Patients]
WHERE [SSN] = @SSN;
```

```output
Msg 402, Level 16, State 2, Line 5   
The data types char(11) encrypted with (encryption_type = 'DETERMINISTIC', 
encryption_algorithm_name = 'AEAD_AES_256_CBC_HMAC_SHA_256', column_encryption_key_name = 'CEK_Auto1', 
column_encryption_key_database_name = 'Clinic') collation_name = 'Latin1_General_BIN2' 
and nchar(11) encrypted with (encryption_type = 'DETERMINISTIC', 
encryption_algorithm_name = 'AEAD_AES_256_CBC_HMAC_SHA_256', column_encryption_key_name = 'CEK_Auto1', 
column_encryption_key_database_name = 'Clinic') are incompatible in the equal to operator.
```

> [!NOTE]
> Ohne Parametrisierung wird die gesamte Abfrage, einschließlich Typumwandlungen, innerhalb von SQL Server/Azure SQL-Datenbank verarbeitet. Bei aktivierter Parametrisierung erfolgen einige Typumwandlungen durch .NET Framework innerhalb von SQL Server Management Studio. Aufgrund der Unterschiede zwischen dem Typsystem von .NET Framework und dem von SQL Server (z.B. verschiedene Genauigkeit einiger Typen wie „float“) kann eine Abfrage mit aktivierter Parametrisierung andere Ergebnisse als die Abfrage liefern, die ohne aktivierte Parametrisierung ausgeführt wird. 

## <a name="next-steps"></a>Nächste Schritte
- [Entwickeln von Anwendungen mit Always Encrypted](always-encrypted-client-development.md)


## <a name="see-also"></a>Weitere Informationen
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)