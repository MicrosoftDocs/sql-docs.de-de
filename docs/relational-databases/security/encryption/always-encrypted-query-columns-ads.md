---
description: Abfragen von Spalten mithilfe von Always Encrypted mit Azure Data Studio
title: Abfragen von Spalten mithilfe von Always Encrypted mit Azure Data Studio | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 826832a9653c0f3966a2919f7e32beaa938ae6d5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345459"
---
# <a name="query-columns-using-always-encrypted-with-azure-data-studio"></a>Abfragen von Spalten mithilfe von Always Encrypted mit Azure Data Studio
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

In diesem Artikel wird beschrieben, wie Sie mit [Azure Data Studio](../../../azure-data-studio/what-is-azure-data-studio.md) mithilfe von [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md) verschlüsselte Spalten abfragen. Azure Data Studio bietet folgende Möglichkeiten:
- Abrufen von in verschlüsselten Spalten gespeicherten Chiffretextwerten 
- Abrufen von in verschlüsselten Spalten gespeicherten Klartextwerten  
- Senden von Klartextwerten an verschlüsselte Spalten (z.B. in `INSERT`- oder `UPDATE`-Anweisungen und als Nachschlageparameter von `WHERE`-Klauseln in `SELECT`-Anweisungen). 

## <a name="retrieving-ciphertext-values-stored-in-encrypted-columns"></a>Abrufen von in verschlüsselten Spalten gespeicherten Chiffretextwerten    
In diesem Abschnitt wird beschrieben, wie in verschlüsselten Spalten gespeicherte Daten als Chiffretext abgerufen werden.

### <a name="steps"></a>Schritte
1. Vergewissern Sie sich, dass Always Encrypted für die Datenbankverbindung des Abfragefensters deaktiviert ist, in dem Sie eine `SELECT`-Abfrage zum Abrufen von Chiffretextwerten ausführen. Weitere Informationen finden Sie unter [Aktivieren und Deaktivieren von Always Encrypted für eine Datenbankverbindung](#enabling-and-disabling-always-encrypted-for-a-database-connection) weiter unten.     
1. Führen Sie Ihre `SELECT`-Abfrage aus. Alle aus verschlüsselten Spalten abgerufenen Daten werden als (verschlüsselte) Binärwerte zurückgegeben.   

### <a name="example"></a>Beispiel
Sofern `SSN` eine verschlüsselte Spalte in der Tabelle `Patients` ist, ruft die folgende Abfrage die binären Chiffretextwerte ab, wenn Always Encrypted für die Datenbankverbindung deaktiviert ist.   

![Screenshot: Abfrage „SELECT * FROM [dbo].[Patients]“ und die Ergebnisse der Abfrage, dargestellt als binäre Chiffretextwerte](../../../relational-databases/security/encryption/media/always-encrypted-ads-query-ciphertext.png)
 
## <a name="retrieving-plaintext-values-stored-in-encrypted-columns"></a>Abrufen von in verschlüsselten Spalten gespeicherten Klartextwerten    
In diesem Abschnitt wird beschrieben, wie in verschlüsselten Spalten gespeicherte Daten als Chiffretext abgerufen werden.

### <a name="prerequisites"></a>Voraussetzungen
- Azure Data Studio Version 17.1 oder höher.
- Sie benötigen Zugriff auf die Spaltenhauptschlüssel und die Metadaten zu den Schlüsseln, die die Spalten schützen, für die Sie die Abfrage ausführen. Ausführliche Informationen finden Sie weiter unten unter [Berechtigungen zum Abfragen verschlüsselter Spalten](#permissions-for-querying-encrypted-columns).
- Spaltenhauptschlüssel müssen in Azure Key Vault oder im Windows-Zertifikatspeicher gespeichert sein. Azure Data Studio unterstützt keine anderen Schlüsselspeicher.

### <a name="steps"></a>Schritte
1.  Aktivieren Sie Always Encrypted für die Datenbankverbindung des Abfragefensters, in dem Sie eine `SELECT`-Abfrage zum Abrufen und Entschlüsseln Ihrer Daten ausführen. Dadurch wird der (von Azure Data Studio verwendete) [Microsoft .NET-Datenanbieter für SQL Server](../../../connect/ado-net/sql/sqlclient-support-always-encrypted.md) angewiesen, die verschlüsselten Spalten im Abfrageresultset zu entschlüsseln. Weitere Informationen finden Sie unter [Aktivieren und Deaktivieren von Always Encrypted für eine Datenbankverbindung](#enabling-and-disabling-always-encrypted-for-a-database-connection) weiter unten.
1.  Führen Sie Ihre `SELECT`-Abfrage aus. Aus verschlüsselten Spalten abgerufene Daten werden als Klartextwerte der ursprünglichen Datentypen zurückgegeben.
 
### <a name="example"></a>Beispiel
Wenn SSN eine verschlüsselte Spalte in der Tabelle `Patients` ist, gibt die unten gezeigte Abfrage Klartextwerte zurück, sofern Always Encrypted für die Datenbankverbindung aktiviert ist und Sie Zugriff auf den Spaltenhauptschlüssel haben, der für die Spalte `SSN` konfiguriert ist.   

![Screenshot: Abfrage „SELECT * FROM [dbo].[Patients]“ und die Ergebnisse der Abfrage, dargestellt als Nur-Text-Werte](../../../relational-databases/security/encryption/media/always-encrypted-ads-query-plaintext.png)
 
## <a name="sending-plaintext-values-targeting-encrypted-columns"></a>Senden von Klartextwerten an verschlüsselte Spalten       
In diesem Abschnitt wird beschrieben, wie eine Abfrage ausgeführt wird, die Werte sendet, welche eine verschlüsselte Spalte als Ziel haben. Beispielsweise eine Abfrage, mit der ein Wert in einer verschlüsselten Spalte eingefügt oder aktualisiert wird oder mit der nach einem solchen Wert gefiltert wird:

### <a name="prerequisites"></a>Voraussetzungen
- Azure Data Studio Version 18.1 oder höher.
- Sie benötigen Zugriff auf die Spaltenhauptschlüssel und die Metadaten zu den Schlüsseln, die die Spalten schützen, für die Sie die Abfrage ausführen. Ausführliche Informationen finden Sie weiter unten unter [Berechtigungen zum Abfragen verschlüsselter Spalten](#permissions-for-querying-encrypted-columns).
- Spaltenhauptschlüssel müssen in Azure Key Vault oder im Windows-Zertifikatspeicher gespeichert sein. Azure Data Studio unterstützt keine anderen Schlüsselspeicher.

### <a name="steps"></a>Schritte
1. Aktivieren Sie Always Encrypted für die Datenbankverbindung des Abfragefensters, in dem Sie eine `SELECT`-Abfrage zum Abrufen und Entschlüsseln Ihrer Daten ausführen. Dadurch wird der (von Azure Data Studio verwendete) [Microsoft .NET-Datenanbieter für SQL Server](../../../connect/ado-net/sql/sqlclient-support-always-encrypted.md) angewiesen, Abfrageparameter zu verschlüsseln, die verschlüsselte Spalten zum Ziel haben, sowie die aus verschlüsselten Spalten abgerufenen Ergebnisse zu entschlüsseln. Weitere Informationen finden Sie unter [Aktivieren und Deaktivieren von Always Encrypted für eine Datenbankverbindung](#enabling-and-disabling-always-encrypted-for-a-database-connection) weiter unten. 
1. Aktivieren Sie die Parametrisierung für Always Encrypted für das Abfragefenster. Details finden Sie weiter unten unter [Parametrisierung für Always Encrypted](#parameterization-for-always-encrypted) .
1. Deklarieren Sie eine Transact-SQL-Variable, und initialisieren Sie sie mit einem Wert, der an die Datenbank gesendet werden soll (Einfügen, Aktualisieren oder Filtern nach). 
1. Führen Sie die Abfrage aus, um den Wert der Transact-SQL-Variablen an die Datenbank zu senden. Azure Data Studio wandelt die Variable in einen Abfrageparameter um und verschlüsselt dessen Wert, ehe er an die Datenbank gesendet wird.   

### <a name="example"></a>Beispiel
Wenn `SSN` eine verschlüsselte `char(11)`-Spalte in der Tabelle `Patients` ist, versucht das nachfolgende Skript, eine Zeile zu finden, die `'795-73-9838'` in der Spalte "SSN" enthält. Die Ergebnisse werden zurückgegeben, wenn Always Encrypted für die Datenbankverbindung aktiviert ist, die Parametrisierung für Always Encrypted für das Abfragefenster aktiviert ist und Sie Zugriff auf den für die Spalte `SSN` konfigurierten Spaltenhauptschlüssel haben.   

![Screenshot: Abfrage „DECLARE @SSN char(11) = '795-73-9838' SELECT * FROM [dbo].[Patients] WHERE [SSN] = @SSN“ und die Ergebnisse der Abfrage](../../../relational-databases/security/encryption/media/always-encrypted-ads-query-parameters.png)

## <a name="permissions-for-querying-encrypted-columns"></a>Berechtigungen zum Abfragen verschlüsselter Spalten

Zum Ausführen von Abfragen für verschlüsselte Spalten, einschließlich Abfragen, die Daten in Chiffretext abrufen, benötigen Sie die Berechtigungen **VIEW ANY COLUMN MASTER KEY DEFINITION** und **VIEW ANY COLUMN ENCRYPTION KEY DEFINITION** in der Datenbank.

Zusätzlich zu den oben aufgeführten Berechtigungen benötigen Sie zum Entschlüsseln von Abfrageergebnissen oder Verschlüsseln von Abfrageparametern (die durch parametrisierte Transact-SQL-Anweisungen erstellt wurden) auch Zugriff auf den Spaltenhauptschlüssel, der die Zielspalten schützt:

- **Zertifikatspeicher – lokaler Computer:** Sie benötigen **Lesezugriff** auf das als Spaltenhauptschlüssel verwendete Zertifikat oder Administratorrechte auf dem Computer.   
- **Azure Key Vault:** Sie benötigen die Berechtigungen **Abrufen**, **Schlüssel entpacken** und **Überprüfen** für den Schlüsseltresor, der den Spaltenhauptschlüssel enthält.

Weitere Informationen finden Sie unter [Create and Store Column Master Keys (Always Encrypted)](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)(Erstellen und Speichern von Spaltenhauptschlüsseln (Always Encrypted)).

## <a name="enabling-and-disabling-always-encrypted-for-a-database-connection"></a>Aktivieren und Deaktivieren von Always Encrypted für eine Datenbankverbindung   

Wenn Sie in Azure Data Studio eine Verbindung mit einer Datenbank herstellen, können Sie Always Encrypted für die Datenbankverbindung entweder aktivieren oder deaktivieren. Always Encrypted ist standardmäßig deaktiviert. 

Durch Aktivieren von Always Encrypted für eine Datenbankverbindung wird der [Microsoft .NET-Datenanbieter für SQL Server](../../../connect/ado-net/sql/sqlclient-support-always-encrypted.md), der von Azure Data Studio verwendet wird, aufgefordert, die folgenden Aufgaben transparent auszuführen:   
-   Entschlüsseln aller Werte, die aus verschlüsselten Spalten abgerufen und in Abfrageergebnissen zurückgegeben werden   
-   Verschlüsseln der Werte der parametrisierten Transact-SQL-Variablen für verschlüsselte Spalten in der Zieldatenbank   

Wenn Sie Always Encrypted für eine Verbindung nicht aktivieren, wird der Microsoft .NET-Datenanbieter für SQL Server nicht versuchen, Abfrageparameter zu verschlüsseln oder Ergebnisse zu entschlüsseln.

Sie können Always Encrypted aktivieren oder deaktivieren, wenn Sie eine Verbindung mit einer Datenbank herstellen. Allgemeine Informationen zum Herstellen einer Verbindung mit einer Datenbank finden Sie unter:
- [Schnellstart: Herstellen einer Verbindung mit und Abfragen von SQL Server mit Azure Data Studio](../../../azure-data-studio/quickstart-sql-server.md)
- [Schnellstart: Verwenden von Azure Data Studio, um eine Verbindung mit einer Azure SQL-Datenbank-Instanz herzustellen und sie abzufragen](../../../azure-data-studio/quickstart-sql-database.md)

So aktivieren (oder deaktivieren) Sie Always Encrypted:
1. Klicken Sie im Dialogfeld **Verbindung** auf **Erweitert...** .
2. Um Always Encrypted für die Verbindung zu aktivieren, legen Sie das Feld **Always Encrypted** auf **Aktiviert** fest. Wenn Sie Always Encrypted deaktivieren möchten, lassen Sie entweder das Feld **Always Encrypted** leer, oder setzen Sie es auf **Deaktiviert**.
3. Klicken Sie auf **OK**, um **Erweiterte Eigenschaften** zu schließen.

![Dieses kurze Video zeigt die Schritte zum Aktivieren von Always Encrypted für die Verbindung.](../../../relational-databases/security/encryption/media/always-encrypted-ads-connect.gif)

Wenn Sie Anweisungen ausführen möchten, die eine serverseitige Secure Enclave nutzen, müssen Sie bei Verwendung von [Always Encrypted mit Secure Enclaves](always-encrypted-enclaves.md) zusätzlich zum Aktivieren von Always Encrypted für die Verbindung ein Protokoll zum Nachweis von Enclaves sowie eine URL zum Nachweis von Enclaves angeben. Ausführliche Informationen finden Sie unter [Abfragen von Spalten mit Always Encrypted mit Secure Enclaves](always-encrypted-enclaves-query-columns.md#prerequisites-for-running-t-sql-statements-using-enclaves-in-azure-data-studio).

> [!TIP]
> Um für ein vorhandenes Abfragefenster zwischen aktiviertem und deaktiviertem Always Encrypted zu wechseln, klicken Sie auf **Trennen**, klicken Sie dann auf **Verbinden**, und führen Sie die obigen Schritte aus, um die Verbindung mit der Datenbank mit den gewünschten Werten des Felds **Always Encrypted** wiederherzustellen. 

> [!NOTE] 
> Die Schaltfläche **Verbindung ändern** in einem Abfragefenster unterstützt derzeit nicht das Umschalten zwischen aktiviertem und deaktiviertem Always Encrypted.

## <a name="parameterization-for-always-encrypted"></a>Parametrisierung für Always Encrypted

„Parametrisierung für Always Encrypted“ ist ein Feature in Azure Data Studio Version 18.1. und höher, das Transact-SQL-Variablen automatisch in Abfrageparameter ([SqlParameter Class](/dotnet/api/microsoft.data.sqlclient.sqlparameter)-Instanzen) konvertiert. Dies ermöglicht dem zugrunde liegenden [Microsoft .NET-Datenanbieter für SQL Server](../../../connect/ado-net/sql/sqlclient-support-always-encrypted.md) das Erkennen von Daten für verschlüsselte Spalten sowie das Verschlüsseln dieser Daten, ehe sie an die Datenbank gesendet werden.
  
Ohne Parametrisierung übergibt der Microsoft .NET-Datenanbieter für SQL Server jede Anweisung, die Sie im Abfragefenster erstellen, als nicht parametrisierte Abfrage. Wenn die Abfrage Literale oder Transact-SQL-Variablen für verschlüsselte Spalten enthält, kann der .NET Framework-Datenanbieter für SQL Server diese nicht erkennen und verschlüsseln, ehe die Abfrage an die Datenbank gesendet wird. Daher misslingt die Abfrage aufgrund eine Typkonflikts (zwischen dem Literal oder der Transact-SQL-Variablen in Klartext und der verschlüsselten Spalte). Die folgende Abfrage misslingt beispielsweise ohne Parametrisierung, sofern die Spalte `SSN` verschlüsselt ist.   

```sql
DECLARE @SSN CHAR(11) = '795-73-9838'
SELECT * FROM [dbo].[Patients]
WHERE [SSN] = @SSN
```

### <a name="enabling-and-disabling-parameterization-for-always-encrypted"></a>Aktivieren oder Deaktivieren der Parametrisierung für Always Encrypted

„Parametrisierung für Always Encrypted“ ist standardmäßig deaktiviert.

So aktivieren bzw. deaktivieren Sie die Parametrisierung für Always Encrypted:

1. Wählen Sie **Datei** > **Einstellungen** > **Einstellungen** (**Code** > **Einstellungen** > **Einstellungen** auf dem Mac) aus.
2. Navigieren Sie zu **Daten** > **Microsoft SQL Server**.
3. Aktivieren bzw. deaktivieren **Parametrisierung für Always Encrypted**.
4. Schließen Sie das Fenster **Einstellungen**.

![Dieses kurze Video zeigt, wie Sie die Parametrisierung für Always Encrypted aktivieren/deaktivieren.](../../../relational-databases/security/encryption/media/always-encrypted-ads-parameterization.gif)

> [!NOTE]
> Die Parametrisierung für Always Encrypted funktioniert nur in einer Abfrage, die Datenbankverbindungen verwendet, für die Always Encrypted aktiviert ist (siehe [Aktivieren und Deaktivieren von Always Encrypted für eine Datenbankverbindung](#enabling-and-disabling-always-encrypted-for-a-database-connection)). Transact-SQL-Variablen werden nicht parametrisiert, wenn das Abfragefenster eine Datenbankverbindung ohne aktiviertes Always Encrypted verwendet.

### <a name="how-parameterization-for-always-encrypted-works"></a>Funktionsweise von „Parametrisierung für Always Encrypted“

Wenn für ein Abfragefenster sowohl das Feature „Parametrisierung für Always Encrypted“ als auch Always Encrypted aktiviert ist, versucht Azure Data Studio, Transact-SQL-Variablen zu parametrisieren, die die folgenden Voraussetzungen erfüllen:

- Sind in der gleichen Anweisung deklariert und initialisiert (Inline-Initialisierung). Variablen, die mit getrennten `SET` -Anweisungen deklariert wurden, werden nicht parametrisiert.
- Sind mithilfe eines einzelnen Literals initialisiert. Variablen, die mithilfe von Ausdrücken initialisiert wurden, die Operatoren oder Funktionen enthalten, werden nicht parametrisiert.

Im Folgenden finden Sie Beispiele für Variablen, die von Azure Data Studio parametrisiert werden.

```sql
DECLARE @SSN char(11) = '795-73-9838';
   
DECLARE @BirthDate date = '19990104';
DECLARE @Salary money = $30000;
```

Im Folgenden finden Sie einige Beispiele für Variablen, die Azure Data Studio nicht zu parametrisieren versucht:

```sql
DECLARE @Name nvarchar(50); --Initialization separate from declaration
SET @Name = 'Abel';

DECLARE @StartDate date = GETDATE(); -- a function used instead of a literal

DECLARE @NewSalary money = @Salary * 1.1; -- an expression used instead of a literal
```
 
Voraussetzungen für eine erfolgreiche Parametrisierung:   
- Der Typ des Literals, der für die Initialisierung der zu parametrisierenden Variablen verwendet wird, muss dem Typ in der Variablendeklaration entsprechen.   
- Wenn der deklarierte Typ der Variablen ein „date“- oder „time“-Typ ist, muss die Variable mithilfe einer Zeichenfolge in einem der folgenden ISO 8601-kompatiblen Formate initialisiert werden.   

Es folgen Beispiele von Transact-SQL-Variablendeklarationen, die zu Parametrisierungsfehlern führen:

```sql
DECLARE @BirthDate date = '01/04/1999' -- unsupported date format   
   
DECLARE @Number int = 1.1 -- the type of the literal does not match the type of the variable   
```

Azure Data Studio nutzt Intellisense, um Sie zu informieren, welche Variablen erfolgreich parametrisiert werden können und welche Parametrisierungsversuche fehlschlagen (samt Grund).   

Eine Deklaration einer Variablen, die erfolgreich parametrisiert werden kann, wird im Abfragefenster mit einer Unterstreichung gekennzeichnet, die auf eine zugrunde liegende Informationsmeldung verweist. Wenn Sie mit der Maus auf eine Deklarationsanweisung zeigen, die mit einer zugrunde liegenden Informationsmeldung gekennzeichnet wurde, wird die Meldung mit den Ergebnissen des Parametrisierungsvorgangs, einschließlich der Werte der Haupteigenschaften des resultierenden [SqlParameter Class](/dotnet/api/microsoft.data.sqlclient.sqlparameter)-Objekts, angezeigt (die Variable ist zugeordnet zu: [SqlDbType](/dotnet/api/microsoft.data.sqlclient.sqlparameter.dbtype), [Size](/dotnet/api/microsoft.data.sqlclient.sqlparameter.size), [Precision](/dotnet/api/microsoft.data.sqlclient.sqlparameter.precision), [Scale](/dotnet/api/microsoft.data.sqlclient.sqlparameter.scale) und [SqlValue](/dotnet/api/microsoft.data.sqlclient.sqlparameter.sqlvalue)). Eine vollständige Liste aller Variablen, die erfolgreich parametrisiert wurden, finden Sie in der Ansicht **Probleme**. Um die Ansicht **Probleme** zu öffnen, wählen Sie **Ansicht** > **Probleme** aus.    



Wenn Azure Data Studio versucht hat, eine Variable zu parametrisieren, die Parametrisierung jedoch fehlgeschlagen ist, wird die Deklaration der Variablen mit einer Fehlerunterstreichung gekennzeichnet. Wenn Sie den Mauszeiger über der Deklarationsanweisung bewegen, die mit einer Fehlerunterstreichung markiert wurde, erhalten Sie Informationen zum Fehler. In der Ansicht **Probleme** wird auch die vollständige Liste der Parametrisierungsfehler für alle Variablen angezeigt.

 
> [!NOTE]
> Da Always Encrypted eine beschränkte Teilmenge von Typumwandlungen unterstützt, ist es in vielen Fällen erforderlich, dass der Datentyp einer Transact-SQL-Variablen dem Typ der Spalte in der Zieldatenbank entspricht. Angenommen, der Typ der Spalte `SSN` in der Tabelle `Patients` ist `char(11)`. Die folgende Abfrage misslingt, da der Typ der Variablen `@SSN` (der `nchar(11)` lautet) nicht dem Typ der Spalte entspricht.   

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
> Ohne Parametrisierung wird die gesamte Abfrage, einschließlich Typumwandlungen, innerhalb von SQL Server/Azure SQL-Datenbank verarbeitet. Bei aktivierter Parametrisierung werden einige Typumwandlungen vom Microsoft .NET-Datenanbieter für SQL Server in Azure Data Studio ausgeführt. Aufgrund der Unterschiede zwischen dem Typsystem von Microsoft .NET und dem von SQL Server (z. B. unterschiedliche Genauigkeit einiger Typen wie „float“) kann eine Abfrage mit aktivierter Parametrisierung andere Ergebnisse liefern als eine Abfrage, die ohne aktivierte Parametrisierung ausgeführt wird. 

## <a name="next-steps"></a>Nächste Schritte
- [Entwickeln von Anwendungen mit Always Encrypted](always-encrypted-client-development.md)


## <a name="see-also"></a>Weitere Informationen
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)