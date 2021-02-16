---
title: Erste Schritte mit SQL Server-Sicherheit unter Linux
description: Informieren sich über die Sicherheitsfeatures von SQL Server für Linux, um herauszufinden, in welchen Bereichen Sie weitere Untersuchungen anstellen müssen.
author: VanMSFT
ms.author: vanto
ms.date: 10/02/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: ecc72850-8b01-492e-9a27-ec817648f0e0
ms.openlocfilehash: f4201aa6c07d4ae96d44d8aa443b23e275e1f9f2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346408"
---
# <a name="walkthrough-for-the-security-features-of-sql-server-on-linux"></a>Exemplarische Vorgehensweise für die Sicherheitsfeatures von SQL Server für Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Wenn Sie ein Linux-Benutzer sind, der noch nicht mit SQL Server vertraut ist, lernen Sie mit den folgenden Aufgaben einige der Sicherheitsaufgaben kennen. Diese Features gelten nicht ausschließlich und speziell nur für Linux, aber mit diesem Artikel können Sie sich einen Überblick über die Bereiche verschaffen, die Sie näher untersuchen sollten. In jedem Beispiel wird ein Link zu der ausführlichen Dokumentation für diesen Bereich bereitgestellt.

> [!NOTE]
>  In den folgenden Beispielen wird die Beispieldatenbank **AdventureWorks2014** verwendet. Anweisungen zum Abrufen und Installieren dieser Beispieldatenbank finden Sie unter [Migrieren einer SQL Server-Datenbank-Instanz von Windows zu Linux mit Sicherung und Wiederherstellung](sql-server-linux-migrate-restore-database.md).


## <a name="create-a-login-and-a-database-user"></a>Erstellen eines Anmeldenamens und eines Datenbankbenutzers 

Gewähren Sie anderen Benutzern Zugriff auf SQL Server, indem Sie mithilfe der [CREATE LOGIN](../t-sql/statements/create-login-transact-sql.md)-Anweisung einen Anmeldenamen in der Masterdatenbank erstellen. Beispiel:

```
CREATE LOGIN Larry WITH PASSWORD = '************';  
```

> [!NOTE]
>  Verwenden Sie immer ein sicheres Kennwort anstelle der Sternchen im vorherigen Befehl.

Anmeldenamen können eine Verbindung mit SQL Server herstellen und Zugriff (mit eingeschränkten Berechtigungen) auf die Masterdatenbank haben. Zum Herstellen einer Verbindung mit einer Benutzerdatenbank benötigt ein Anmeldename eine entsprechende Identität auf Datenbankebene, die als Datenbankbenutzer bezeichnet wird. Benutzer sind für jede Datenbank spezifisch und müssen separat in jeder Datenbank erstellt werden, um Ihnen Zugriff zu gewähren. Im folgenden Beispiel wird in der AdventureWorks2014-Datenbank mit der [CREATE USER](../t-sql/statements/create-user-transact-sql.md)-Anweisung ein Benutzer namens Larry erstellt, der mit dem Anmeldenamen „Larry“ verknüpft ist. Obwohl Anmeldename und Benutzer miteinander verknüpft sind (einander zugeordnet), handelt es sich dabei um unterschiedliche Objekte. Der Anmeldename ist ein Prinzipal auf Serverebene. Der Benutzer ist ein Prinzipal auf Datenbankebene.

```
USE AdventureWorks2014;
GO
CREATE USER Larry;
GO
```

- Mit einem SQL Server-Administratorkonto kann eine Verbindung mit einer beliebigen Datenbank hergestellt werden, und es können mehr Anmeldenamen und Benutzer in einer beliebigen Datenbank erstellt werden.  
- Eine Person, die eine Datenbank erstellt, wird zum Datenbankbesitzer, der eine Verbindung mit der Datenbank herstellen kann. Datenbankbesitzer können mehr Benutzer erstellen.

Später können Sie andere Anmeldenamen dazu autorisieren, weitere Anmeldenamen zu erstellen, indem Sie Ihnen die `ALTER ANY LOGIN`-Berechtigung erteilen. Innerhalb einer Datenbank können Sie andere Benutzer dazu autorisieren, weitere Benutzer zu erstellen, indem Sie Ihnen die `ALTER ANY USER`-Berechtigung erteilen. Beispiel:   

```
GRANT ALTER ANY LOGIN TO Larry;   
GO   
   
USE AdventureWorks2014;   
GO   
GRANT ALTER ANY USER TO Jerry;    
GO   
```

Nun kann der Anmeldename Larry weitere Anmeldungen erstellen, und der Benutzer Jerry kann weitere Benutzer erstellen.


## <a name="granting-access-with-least-privileges"></a>Gewähren von Zugriff mit geringsten Rechten

Die ersten Personen, die eine Verbindung mit einer Benutzerdatenbank herstellen, sind die Administrator- und Datenbankbesitzerkonten. Diese Benutzer verfügen jedoch über alle Berechtigungen, die für die Datenbank verfügbar sind. Dies sind mehr Berechtigungen, als die meisten Benutzer haben sollten. 

Wenn Sie gerade erst beginnen, können Sie mithilfe der integrierten *festen Datenbankrollen* allgemeine Kategorien von Berechtigungen zuweisen. Beispielsweise kann die feste Datenbankrolle `db_datareader` alle Tabellen in der Datenbank lesen, aber keine Änderungen vornehmen. Gewähren Sie die Mitgliedschaft in einer festen Datenbankrolle mit der [ALTER ROLE](../t-sql/statements/alter-role-transact-sql.md)-Anweisung. Im folgenden Beispiel wird der Benutzer `Jerry` der festen Datenbankrolle `db_datareader` hinzugefügt.   
   
```   
USE AdventureWorks2014;   
GO   
   
ALTER ROLE db_datareader ADD MEMBER Jerry;   
```   

Eine Liste der festen Datenbankrollen finden Sie unter [Rollen auf Datenbankebene](../relational-databases/security/authentication-access/database-level-roles.md).

Wenn Sie später bereit sind, präziseren Datenzugriff (sehr empfohlen) zu konfigurieren, erstellen Sie mithilfe der [CREATE ROLE](../t-sql/statements/create-role-transact-sql.md)-Anweisung eigene benutzerdefinierte Datenbankrollen. Weisen Sie dann Ihren benutzerdefinierten Rollen bestimmte präzise Berechtigungen zu.

Die folgenden Anweisungen erstellen z.B. eine Datenbankrolle mit dem Namen `Sales` und geben der `Sales`-Gruppe die Möglichkeit, Zeilen der `Orders`-Tabelle anzuzeigen, zu aktualisieren und zu löschen, und anschließend wird der Benutzer `Jerry` der `Sales`-Rolle hinzugefügt.   
   
```   
CREATE ROLE Sales;   
GRANT SELECT ON Object::Sales TO Orders;   
GRANT UPDATE ON Object::Sales TO Orders;   
GRANT DELETE ON Object::Sales TO Orders;   
ALTER ROLE Sales ADD MEMBER Jerry;   
```

Weitere Informationen zum Berechtigungssystem finden Sie unter [Erste Schritte mit Berechtigungen für die Datenbank-Engine](../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md).


## <a name="configure-row-level-security"></a>Konfigurieren der Sicherheit auf Zeilenebene  

Mit der [Sicherheit auf Zeilenebene](../relational-databases/security/row-level-security.md) können Sie in einer Datenbank den Zugriff auf Zeilen für den Benutzer einschränken, der eine Abfrage ausführt. Dieses Feature ist sehr nützlich, wenn Sie beispielsweise sicherstellen möchten, dass Kunden nur auf ihre eigenen Daten oder Mitarbeiter nur auf Daten zugreifen können, die für ihre eigene Abteilung relevant sind.   

In den folgenden Schritten erfahren Sie, wie Sie zwei Benutzer mit unterschiedlichen Zugriffsrechten auf Zeilenebene für die Tabelle `Sales.SalesOrderHeader` einrichten. 

Erstellen Sie zwei Benutzerkonten, um die Sicherheit auf Zeilenebene zu testen:    
   
```   
USE AdventureWorks2014;   
GO   
   
CREATE USER Manager WITHOUT LOGIN;     
   
CREATE USER SalesPerson280 WITHOUT LOGIN;    
```

Erteilen Sie den Benutzern Lesezugriff auf die Tabelle `Sales.SalesOrderHeader`:    
   
```   
GRANT SELECT ON Sales.SalesOrderHeader TO Manager;      
GRANT SELECT ON Sales.SalesOrderHeader TO SalesPerson280;
```
   
Erstellen Sie ein neues Schema und eine Inline-Tabellenwertfunktion. Die Funktion gibt 1 zurück, wenn eine Zeile in der Spalte `SalesPersonID` der ID einer `SalesPerson`-Anmeldung entspricht oder wenn der Benutzer, der die Abfrage ausführt, der Managerbenutzer ist.   
   
```     
CREATE SCHEMA Security;   
GO   
   
CREATE FUNCTION Security.fn_securitypredicate(@SalesPersonID AS int)     
    RETURNS TABLE   
WITH SCHEMABINDING   
AS     
   RETURN SELECT 1 AS fn_securitypredicate_result    
WHERE ('SalesPerson' + CAST(@SalesPersonId as VARCHAR(16)) = USER_NAME())     
    OR (USER_NAME() = 'Manager');    
```

Erstellen Sie eine Sicherheitsrichtlinie, und fügen Sie der Tabelle diese Funktion als FILTER- und BLOCK-Prädikat hinzu:  

```
CREATE SECURITY POLICY SalesFilter   
ADD FILTER PREDICATE Security.fn_securitypredicate(SalesPersonID)    
  ON Sales.SalesOrderHeader,   
ADD BLOCK PREDICATE Security.fn_securitypredicate(SalesPersonID)    
  ON Sales.SalesOrderHeader   
WITH (STATE = ON);   
```

Führen Sie den folgenden Code aus, um die Tabelle `SalesOrderHeader` mit jedem Benutzer abzufragen. Überprüfen Sie dann, ob `SalesPerson280` nur die 95 Zeilen aus den Vertriebsdaten des Benutzers angezeigt werden, während `Manager` alle Tabellenzeilen sehen kann.  

```    
EXECUTE AS USER = 'SalesPerson280';   
SELECT * FROM Sales.SalesOrderHeader;    
REVERT; 
 
EXECUTE AS USER = 'Manager';   
SELECT * FROM Sales.SalesOrderHeader;   
REVERT;   
```
 
Ändern Sie die Sicherheitsrichtlinie, um die Richtlinie zu deaktivieren.  Nun können beide Benutzer alle Zeilen anzeigen. 

```
ALTER SECURITY POLICY SalesFilter   
WITH (STATE = OFF);    
``` 


## <a name="enable-dynamic-data-masking"></a>Aktivieren der dynamischen Datenmaskierung

Mithilfe der [dynamischen Datenmaskierung](../relational-databases/security/dynamic-data-masking.md) kann der Zugriff auf vertrauliche Daten auf die Benutzer einer Anwendung beschränkt werden, indem bestimmte Spalten vollständig oder teilweise maskiert werden. 

Führen Sie eine `ALTER TABLE`-Anweisung aus, um der Spalte `EmailAddress` in der Tabelle `Person.EmailAddress` eine Maskierungsfunktion hinzuzufügen: 
 
```
USE AdventureWorks2014;
GO
ALTER TABLE Person.EmailAddress    
ALTER COLUMN EmailAddress    
ADD MASKED WITH (FUNCTION = 'email()');
``` 
 
Erstellen Sie den neuen Benutzer `TestUser` mit der `SELECT`-Berechtigung für die Tabelle, und führen Sie dann eine Abfrage als `TestUser` aus, um die maskierten Daten anzuzeigen:   

```  
CREATE USER TestUser WITHOUT LOGIN;   
GRANT SELECT ON Person.EmailAddress TO TestUser;    
 
EXECUTE AS USER = 'TestUser';   
SELECT EmailAddressID, EmailAddress FROM Person.EmailAddress;       
REVERT;    
```
 
Überprüfen Sie, ob die Maskierungsfunktion die E-Mail-Adresse im ersten Datensatz wie folgt ändert. Von:
  
|EmailAddressID |EmailAddress |  
|----|---- |   
|1 |ken0@adventure-works.com |    
 
into 

|EmailAddressID |EmailAddress |  
|----|---- |   
|1 |kXXX@XXXX.com |   


## <a name="enable-transparent-data-encryption"></a>Transparent Data Encryption aktivieren

Ein Risiko von Datenbanken ist, dass die Datenbankdateien von Ihrem Festplattenlaufwerk gestohlen werden. Dies kann passieren, wenn ein Angreifer aufgrund der Handlungen eines unvorsichtigen Mitarbeiters oder durch Diebstahl des Computers (z. B. eines Laptops), auf dem die Dateien gespeichert sind, erhöhten Zugriff auf Ihr System erhält.

Mit Transparent Data Encryption (TDE) werden die Datendateien verschlüsselt, wenn sie auf dem Festplattenlaufwerk gespeichert werden. Die Masterdatenbank der SQL Server-Datenbank-Engine verfügt über den Verschlüsselungsschlüssel, sodass die Datenbank-Engine die Daten bearbeiten kann. Ohne den Schlüssel können die Datenbankdateien nicht gelesen werden. Höherrangige Administratoren können den Schlüssel verwalten, sichern und neu erstellen. Dies bedeutet, dass die Datenbank verschoben werden kann, jedoch nur von ausgewählten Personen. Wenn TDE konfiguriert ist, wird die Datenbank `tempdb` ebenfalls automatisch verschlüsselt. 

Da die Datenbank-Engine die Daten lesen kann, schützt TDE weder vor unbefugtem Zugriff durch Administratoren des Computers, der Arbeitsspeicher direkt auslesen kann, noch vor unbefugtem Zugriff auf SQL Server über ein Administratorkonto.

### <a name="configure-tde"></a>Konfigurieren von TDE

- Erstellen Sie einen Hauptschlüssel
- Erstellen oder beziehen Sie ein vom Hauptschlüssel geschütztes Zertifikat
- Erstellen Sie einen Verschlüsselungsschlüssel für die Datenbank, und schützen Sie ihn durch das Zertifikat
- Legen Sie fest, dass für die Datenbank Verschlüsselung verwendet wird

Zum Konfigurieren von TDE ist die `CONTROL`-Berechtigung für die Masterdatenbank und die `CONTROL`-Berechtigung für die Benutzerdatenbank erforderlich. TDE wird in der Regel von einem Administrator konfiguriert. 

Im folgenden Beispiel wird die Verschlüsselung und Entschlüsselung der `AdventureWorks2014` -Datenbank gezeigt, wobei ein auf dem Server `MyServerCert`installiertes Zertifikat verwendet wird.


```
USE master;  
GO  

CREATE MASTER KEY ENCRYPTION BY PASSWORD = '**********';  
GO  

CREATE CERTIFICATE MyServerCert WITH SUBJECT = 'My Database Encryption Key Certificate';  
GO  

USE AdventureWorks2014;  
GO
  
CREATE DATABASE ENCRYPTION KEY  
WITH ALGORITHM = AES_256  
ENCRYPTION BY SERVER CERTIFICATE MyServerCert;  
GO
  
ALTER DATABASE AdventureWorks2014  
SET ENCRYPTION ON;   
```

Wenn Sie TDE entfernen möchten, führen Sie `ALTER DATABASE AdventureWorks2014 SET ENCRYPTION OFF;` aus.   

Die Verschlüsselungs- und Entschlüsselungsvorgänge werden von SQL Server in geplanten Hintergrundthreads ausgeführt. Sie können den Status dieser Vorgänge mithilfe der in der Liste weiter unten in diesem Thema genannten Katalogsichten und dynamischen Verwaltungssichten anzeigen.   

> [!WARNING]
>  Sicherungsdateien von Datenbanken, für die TDE aktiviert wurde, werden ebenfalls mithilfe des Verschlüsselungsschlüssels für die Datenbank verschlüsselt. Darum muss bei der Wiederherstellung dieser Sicherungen das Zertifikat, das zum Verschlüsseln des Verschlüsselungsschlüssels für die Datenbank verwendet wurde, verfügbar sein. Dies bedeutet, dass Sie zusätzlich zur Sicherung der Datenbank auch Sicherungskopien der Serverzertifikate aufbewahren müssen, um einem Datenverlust vorzubeugen. Ist das Zertifikat nicht mehr verfügbar, kann es zu einem Datenverlust kommen. Weitere Informationen finden Sie unter [SQL Server Certificates and Asymmetric Keys](../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md).  

Weitere Informationen zu TDE finden Sie unter [Transparent Data Encryption (TDE)](../relational-databases/security/encryption/transparent-data-encryption.md).   


## <a name="configure-backup-encryption"></a>Konfigurieren der Sicherungsverschlüsselung
In SQL Server können die Daten während der Erstellung einer Sicherung verschlüsselt werden. Indem der Verschlüsselungsalgorithmus und der Verschlüsseler (ein Zertifikat oder ein asymmetrischer Schlüssel) beim Erstellen einer Sicherung angegeben werden, können Sie eine verschlüsselte Sicherungsdatei erstellen.    
  
> [!WARNING]
> Es ist sehr wichtig, das Zertifikat oder den asymmetrischen Schlüssel zu sichern – vorzugsweise an einem anderen Speicherort als die Sicherungsdatei, die zum Verschlüsseln verwendet wurde. Ohne das Zertifikat oder den asymmetrischen Schlüssel können Sie keine Sicherung wiederherstellen, sodass die Sicherungsdatei unbrauchbar ist. 
 
 
Im folgenden Beispiel werden ein Zertifikat und anschließend eine durch das Zertifikat geschützte Sicherung erstellt.

```
USE master;  
GO  
CREATE CERTIFICATE BackupEncryptCert   
   WITH SUBJECT = 'Database backups';  
GO 
BACKUP DATABASE [AdventureWorks2014]  
TO DISK = N'/var/opt/mssql/backups/AdventureWorks2014.bak'  
WITH  
  COMPRESSION,  
  ENCRYPTION   
   (  
   ALGORITHM = AES_256,  
   SERVER CERTIFICATE = BackupEncryptCert  
   ),  
  STATS = 10  
GO  
```

Weitere Informationen finden Sie unter [Sicherungsverschlüsselung](../relational-databases/backup-restore/backup-encryption.md).


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den Sicherheitsfeatures in SQL Server finden Sie unter [Sicherheitscenter für die SQL Server-Datenbank-Engine und Azure SQL-Datenbank](../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md).