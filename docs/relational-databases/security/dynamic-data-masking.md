---
title: Dynamische Datenmaskierung | Microsoft-Dokumentation
description: Erfahren Sie mehr über dynamische Datenmaskierung, mit der die Offenlegung vertraulicher Daten eingeschränkt wird, indem sie für nicht berechtigte Benutzer maskiert werden. Dies kann die Sicherheit in SQL Server erheblich vereinfachen.
ms.date: 05/02/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
ms.assetid: a62f4ff9-2953-42ca-b7d8-1f8f527c4d66
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 0a05c7af0bc8b8846e3b4ab5c1e3472249350e7b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344395"
---
# <a name="dynamic-data-masking"></a>Dynamische Datenmaskierung
[!INCLUDE [SQL Server 2016 ASDB, ASDBMI, ASDW ](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

![Dynamische Datenmaskierung](../../relational-databases/security/media/dynamic-data-masking.png)

Die dynamische Datenmaskierung (DDM) beschränkt die Offenlegung vertraulicher Daten, indem sie für nicht berechtigte Benutzer maskiert werden. Sie kann den Entwurf und die Sicherheitscodierung in Ihrer Anwendung erheblich vereinfachen.  

Mit der dynamischen Datenmaskierung können Sie unbefugten Zugriff auf sensible Daten verhindern, indem Kunden festlegen können, wie viele sensible Daten mit minimaler Auswirkung auf die Anwendungsschicht offengelegt werden. DDM kann für designierte Datenbankfelder zum Ausblenden sensibler Daten in den Resultsets von Abfragen konfiguriert werden. Mit der dynamischen Datenmaskierung (DDM) werden Daten in der Datenbank nicht geändert. Die dynamische Datenmaskierung ist für vorhandene Anwendungen einfach zu verwenden, da Maskierungsregeln auf die Abfrageergebnisse angewendet werden. Viele Clientanwendungen können sensible Daten maskieren, ohne vorhandene Abfragen zu ändern.

* Eine zentrale Datenmaskierungsrichtlinie wirkt sich direkt auf vertrauliche Felder in der Datenbank aus.
* Festlegen berechtigter Benutzer oder Rollen, die Zugriff auf vertrauliche Daten haben.
* DDM umfasst Funktionen zur vollständigen und partiellen Maskierung sowie eine willkürliche Maske für numerische Daten.
* Einfache [!INCLUDE[tsql_md](../../includes/tsql-md.md)]-Befehle dienen zum Definieren und Verwalten von Masken.

Der Zweck der dynamischen Datenmaskierung besteht darin, die Offenlegung sensibler Daten zu beschränken, die Benutzer an der Anzeige der Daten hindert, die keinen Zugriff auf diese erhalten sollten. Die dynamische Datenmaskierung ist nicht darauf ausgerichtet, Datenbankbenutzer daran zu hindern, eine direkte Verbindung zur Datenbank herzustellen und umfassende Abfragen auszuführen, die Teile der vertraulichen Daten verfügbar machen. Die dynamische Datenmaskierung ist eine Ergänzung zu anderen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Sicherheitsfeatures (Überwachung, Verschlüsselung, Sicherheit auf Zeilenebene usw.), und es wird dringend empfohlen, dieses Feature in Verbindung mit diesen anderen Features zu verwenden, um die sensiblen Daten in der Datenbank besser zu schützen.  
  
Die dynamische Datenmaskierung steht in [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)]zur Verfügung und wird mithilfe von [!INCLUDE[tsql](../../includes/tsql-md.md)] -Befehlen konfiguriert. Weitere Informationen zum Konfigurieren der dynamischen Datenmaskierung über das Azure-Portal finden Sie unter [Erste Schritte mit der dynamischen Datenmaskierung für SQL-Datenbank (Azure-Portal)](/azure/azure-sql/database/dynamic-data-masking-overview).  
  
## <a name="defining-a-dynamic-data-mask"></a>Definieren einer dynamischen Datenmaske
 Eine Maskierungsregel kann für eine Spalte in einer Tabelle definiert werden, um die Daten in dieser Spalte zu verschleiern. Es stehen vier Arten von Masken zur Verfügung.  
  
|Funktion|BESCHREIBUNG|Beispiele|  
|--------------|-----------------|--------------|  
|Standard|Die vollständige Maskierung gemäß der Datentypen der festgelegten Felder.<br /><br /> Verwenden Sie XXXX oder weniger X-Platzhalter für Zeichenfolge-Datentypen, wenn die Größe des Felds weniger als vier Zeichen umfasst (**char**, **nchar**,  **varchar**, **nvarchar**, **text**, **ntext**).  <br /><br /> Verwenden Sie für numerische Datentypen den Wert 0 (null) (**bigint**, **bit**, **decimal**, **int**, **money**, **numeric**, **smallint**, **smallmoney**, **tinyint**, **float**, **real**).<br /><br /> Verwenden Sie für die Datentypen für Datum und Uhrzeit 01.01.1900 00:00:00.0000000 (**date**, **datetime2**, **datetime**, **datetimeoffset**, **smalldatetime**, **time**).<br /><br />Verwenden Sie für binäre Datentypen ein Einzelbyte des ASCII-Werts 0 (**binary**, **varbinary**, **image**).|Beispielsyntax der Spaltendefinition: `Phone# varchar(12) MASKED WITH (FUNCTION = 'default()') NULL`<br /><br /> Beispiel für die alter-Syntax: `ALTER COLUMN Gender ADD MASKED WITH (FUNCTION = 'default()')`|  
|Email|Eine Maskierungsmethode, die den ersten Buchstaben einer E-Mail-Adresse und das konstante Suffix „.com“ in Form einer E-Mail-Adresse verfügbar macht. `aXXX@XXXX.com`.|Beispielsyntax der Definition: `Email varchar(100) MASKED WITH (FUNCTION = 'email()') NULL`<br /><br /> Beispiel für die alter-Syntax: `ALTER COLUMN Email ADD MASKED WITH (FUNCTION = 'email()')`|  
|Zufällig|Eine zufällige Maskierungsfunktion, die für alle numerischen Typen verwendet werden kann, um den ursprünglichen Wert mit einem zufälligen Wert innerhalb eines bestimmten Bereichs zu maskieren.|Beispielsyntax der Definition: `Account_Number bigint MASKED WITH (FUNCTION = 'random([start range], [end range])')`<br /><br /> Beispiel für die alter-Syntax: `ALTER COLUMN [Month] ADD MASKED WITH (FUNCTION = 'random(1, 12)')`|  
|Benutzerdefinierte Zeichenfolge|Eine Maskierungsmethode, die den ersten und letzten Buchstaben verfügbar macht und in der Mitte eine benutzerdefinierte Zeichenfolge zur Auffüllung hinzufügt. `prefix,[padding],suffix`<br /><br /> Hinweis: Wenn der ursprüngliche Wert zu kurz ist, um die gesamte Maske zu vervollständigen, wird ein Teil des Präfixes oder Suffixes nicht verfügbar gemacht.|Beispielsyntax der Definition: `FirstName varchar(100) MASKED WITH (FUNCTION = 'partial(prefix,[padding],suffix)') NULL`<br /><br /> Beispiel für die alter-Syntax: `ALTER COLUMN [Phone Number] ADD MASKED WITH (FUNCTION = 'partial(1,"XXXXXXX",0)')`<br /><br /> Weiteres Beispiel:<br /><br /> `ALTER COLUMN [Phone Number] ADD MASKED WITH (FUNCTION = 'partial(5,"XXXXXXX",0)')`|  
  
## <a name="permissions"></a>Berechtigungen  
 Sie benötigen keine speziellen Berechtigungen zum Erstellen einer Tabelle mit einer dynamischen Datenmaske. Lediglich die Standardschemaberechtigungen **CREATE TABLE** und **ALTER** sind erforderlich.  
  
 Das Hinzufügen, Ersetzen oder Entfernen der Maske einer Spalte erfordert die Berechtigungen **ALTER ANY MASK** und **ALTER** für die Tabelle. Es empfiehlt sich, die Berechtigung **ALTER ANY MASK** einem Sicherheitsbeauftragten zu erteilen.  
  
 Benutzer mit der Berechtigung **SELECT** für eine Tabelle können die Tabellendaten anzeigen. Spalten, die als maskiert definiert sind, zeigen die maskierten Daten an. Erteilen Sie die Berechtigung **UNMASK** einem Benutzer, damit dieser Daten ohne Maskierung aus den Spalten abrufen kann, für die eine Maskierung definiert ist.  
  
 Die Berechtigung **CONTROL** für eine Datenbank umfasst sowohl die Berechtigung **ALTER ANY MASKE** als auch **UNMASK** .  
  
## <a name="best-practices-and-common-use-cases"></a>Bewährte Methoden und Einsatzbereiche  
  
-   Das Erstellen einer Maske für eine Spalte verhindert keine Aktualisierungen für diese Spalte. Obwohl die Benutzer beim Abfragen der maskierten Spalte auch maskierte Daten erhalten, können dieselben Benutzer die Daten aktualisieren, wenn sie über die entsprechenden Schreibberechtigungen verfügen. Eine ordnungsgemäße Zugriffssteuerungsrichtlinie sollte weiterhin verwendet werden, um Aktualisierungsberechtigungen einzuschränken.  
  
-   Die Verwendung von `SELECT INTO` oder `INSERT INTO` zum Kopieren von Daten aus einer maskierten Spalte in eine andere Tabelle führt zu maskierten Daten in der Zieltabelle.  
  
-   Die dynamische Datenmaskierung wird beim Ausführen von Import- und Exportvorgängen in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] angewendet. Eine Datenbank mit maskierten Spalten führt zu einer exportierten Datendatei mit maskierten Daten (vorausgesetzt, sie wird durch einen Benutzer ohne **UNMASK**-Berechtigungen exportiert), und die importierte Datenbank enthält statisch maskierte Daten.  
  
## <a name="querying-for-masked-columns"></a>Abfragen von maskierten Spalten  
 Verwenden Sie die **sys.masked_columns** -Ansicht zum Abfragen von Tabellenspalten, auf die eine Maskierungsfunktion angewendet wird. Diese Ansicht erbt von der **sys.columns** -Ansicht. Sie gibt alle Spalten in der **sys.columns** -Ansicht sowie die Spalten **is_masked** und **masking_function** zurück, wobei angegeben wird, ob die Spalte maskiert ist. Ist dies der Fall, gibt die Ansicht die definierte Maskierungsfunktion an. Diese Ansicht zeigt nur die Spalten an, auf die eine Maskierungsfunktion angewendet wird.  
  
```sql 
SELECT c.name, tbl.name as table_name, c.is_masked, c.masking_function  
FROM sys.masked_columns AS c  
JOIN sys.tables AS tbl   
    ON c.[object_id] = tbl.[object_id]  
WHERE is_masked = 1;  
```  
  
## <a name="limitations-and-restrictions"></a>Einschränkungen  
 Eine Maskierungsregel kann für die folgenden Spaltentypen nicht definiert werden:  
  
-   Verschlüsselte Spalten (Always Encrypted)  
  
-   FILESTREAM  
  
-   COLUMN_SET oder eine Sparsespalte, die Teil eines Spaltensatzes ist.  
  
-   Eine Maske kann nicht für eine berechnete Spalte konfiguriert werden, aber wenn die berechnete Spalte von einer Spalte mit MASKE abhängig ist, gibt die berechnete Spalte maskierte Daten zurück.  
  
-   Eine Spalte mit Datenmaskierung darf kein Schlüssel für einen Volltextindex sein.  
  
 Für Benutzer ohne die **UNMASK** -Berechtigung funktionieren die als veraltet markierten Anweisungen **READTEXT**, **UPDATETEXT** und **WRITETEXT** nicht ordnungsgemäß für eine Spalte, die für die dynamische Datenmaskierung konfiguriert ist. 
 
 Das Hinzufügen einer dynamischen Datenmaske wird als Schemaänderung für die zugrunde liegende Tabelle implementiert und kann deshalb nicht für eine Spalte mit Abhängigkeiten ausgeführt werden. Um diese Einschränkung zu umgehen, können Sie zunächst die Abhängigkeiten entfernen und anschließend die dynamische Datenmaske hinzufügen. Danach erstellen Sie die Abhängigkeiten erneut. Wenn z.B. die Abhängigkeit dadurch entsteht, dass ein Indexes von dieser Spalte abhängt, können Sie den Index löschen und anschließend die Maske hinzufügen und dann den abhängigen Index wieder herstellen.
 

## <a name="security-note-bypassing-masking-using-inference-or-brute-force-techniques"></a>Sicherheitshinweis: Umgehen der Maskierung mithilfe von Rückschluss- oder Brute-Force-Verfahren

Die dynamische Datenmaskierung dient zur Vereinfachung der Anwendungsentwicklung durch die Einschränkung der Gefährdung der Daten in einer Reihe von vordefinierten Abfragen, die von der Anwendung verwendet werden. Die dynamische Datenmaskierung kann auch dazu dienen, eine versehentliche Offenlegung sensibler Daten beim direkten Zugriff auf eine Produktionsdatenbank zu verhindern. Es ist wichtig zu beachten, dass nicht berechtigte Benutzer mit Berechtigungen für Ad-hoc-Abfragen entsprechende Verfahren anwenden können, um Zugriff auf die eigentlichen Daten zu erhalten. Wenn es erforderlich ist, diesen Ad-hoc-Zugriff zu gewähren, sollten alle Datenbankaktivitäten überwacht werden, um dieses Szenario zu vermeiden.
 
Betrachten Sie dazu als Beispiel einen Datenbankprinzipal, der über ausreichende Berechtigungen zum Ausführen von Ad-hoc-Abfragen für die Datenbank verfügt und versucht, die zugrunde liegenden Daten zu „erraten“ und letztlich die tatsächlichen Werte abzuleiten. Nehmen Sie an, es liegt eine definierte Maske für die `[Employee].[Salary]` -Spalte vor, und dieser Benutzer stellt eine direkte Verbindung zur Datenbank her und beginnt mit dem Raten von Werten, wodurch letztendlich der `[Salary]` -Wert einer Reihe von Mitarbeitern gefolgert wird:
 

```sql
SELECT ID, Name, Salary FROM Employees
WHERE Salary > 99999 and Salary < 100001;
```

>    |  Id | Name| Gehalt |   
>    | ----- | ---------- | ------ | 
>    |  62543 | Jana Hoffmann | 0 | 
>    |  91245 | Johan Lorenz | 0 |  

Dies zeigt, dass die dynamische Datenmaskierung nicht als isoliertes Measure verwendet werden sollte, um vertrauliche Daten nicht vollständig vor Benutzern zu schützen, die Ad-hoc-Abfragen für die Datenbank ausführen. Sie ist dazu geeignet, die versehentliche Offenlegung von Daten zu vermeiden, aber sie schützt nicht vor der bösen Absicht, die zugrunde liegenden Daten abzuleiten.
 
Es ist wichtig, die Berechtigungen für die Datenbank ordnungsgemäß zu verwalten und stets das Prinzip der minimal erforderlichen Berechtigungen zu verfolgen. Beachten Sie auch, dass die Überwachung aktiviert ist, um alle Aktivitäten zu verfolgen, die für die Datenbank ausgeführt werden

  
## <a name="examples"></a>Beispiele  
  
### <a name="creating-a-dynamic-data-mask"></a>Erstellen einer dynamischen Datenmaske  
 Mit dem folgenden Beispiel wird eine Tabelle mit drei verschiedenen Typen von dynamischen Datenmasken erstellt. Das Beispiel füllt die Tabelle und wählt sie aus, um das Ergebnis anzuzeigen.  
  
```sql
CREATE TABLE Membership  
  (MemberID int IDENTITY PRIMARY KEY,  
   FirstName varchar(100) MASKED WITH (FUNCTION = 'partial(1,"XXXXXXX",0)') NULL,  
   LastName varchar(100) NOT NULL,  
   Phone varchar(12) MASKED WITH (FUNCTION = 'default()') NULL,  
   Email varchar(100) MASKED WITH (FUNCTION = 'email()') NULL);  
  
INSERT Membership (FirstName, LastName, Phone, Email) VALUES   
('Roberto', 'Tamburello', '555.123.4567', 'RTamburello@contoso.com'),  
('Janice', 'Galvin', '555.123.4568', 'JGalvin@contoso.com.co'),  
('Zheng', 'Mu', '555.123.4569', 'ZMu@contoso.net');  
SELECT * FROM Membership;  
```  
  
 Es wird ein neuer Benutzer erstellt und diesem die Berechtigung **SELECT** für die Tabelle erteilt. Abfragen werden für durch die `TestUser` -Ansicht maskierte Daten ausgeführt.  
  
```sql 
CREATE USER TestUser WITHOUT LOGIN;  
GRANT SELECT ON Membership TO TestUser;  
  
EXECUTE AS USER = 'TestUser';  
SELECT * FROM Membership;  
REVERT;  
```  
  
 Das Ergebnis veranschaulicht die Masken durch Ändern der Daten von  
  
 `1    Roberto     Tamburello    555.123.4567    RTamburello@contoso.com`  
  
 into  
  
 `1    RXXXXXXX    Tamburello    xxxx            RXXX@XXXX.com`  
  
### <a name="adding-or-editing-a-mask-on-an-existing-column"></a>Hinzufügen oder Bearbeiten einer Maske für eine vorhandene Spalte  
 Verwenden Sie die **ALTER TABLE** -Anweisung zum Hinzufügen einer Maske zu einer vorhandenen Spalte in der Tabelle oder zum Bearbeiten der Maske für die betreffende Spalte.  
Im folgenden Beispiel wird eine Maskierungsfunktion zur Spalte `LastName` hinzugefügt:  
  
```sql  
ALTER TABLE Membership  
ALTER COLUMN LastName ADD MASKED WITH (FUNCTION = 'partial(2,"XXX",0)');  
```  
  
 Im folgenden Beispiel wird eine Maskierungsfunktion für die Spalte `LastName` geändert:  

```sql  
ALTER TABLE Membership  
ALTER COLUMN LastName varchar(100) MASKED WITH (FUNCTION = 'default()');  
```  
  
### <a name="granting-permissions-to-view-unmasked-data"></a>Erteilen von Berechtigungen zum Anzeigen von Daten ohne Maskierung  
 Das Erteilen der **UNMASK** -Berechtigung ermöglicht dem `TestUser` das Anzeigen der Daten ohne Maskierung.  
  
```sql
GRANT UNMASK TO TestUser;  
EXECUTE AS USER = 'TestUser';  
SELECT * FROM Membership;  
REVERT;   
  
-- Removing the UNMASK permission  
REVOKE UNMASK TO TestUser;  
```  
  
### <a name="dropping-a-dynamic-data-mask"></a>Löschen einer dynamischen Datenmaske  
 Die folgende Anweisung löscht die Maske für die Spalte `LastName` , die im vorherigen Beispiel erstellt wurde:  
  
```sql  
ALTER TABLE Membership   
ALTER COLUMN LastName DROP MASKED;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [column_definition &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-column-definition-transact-sql.md)   
 [sys.masked_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-masked-columns-transact-sql.md)   
 [Erste Schritte mit der dynamischen Datenmaskierung für die SQL-Datenbank (Azure-Portal)](/azure/azure-sql/database/dynamic-data-masking-overview)
