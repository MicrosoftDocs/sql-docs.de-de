---
title: Ausführen von Transact-SQL-Anweisungen mit Secure Enclaves
description: Ausführen von Anweisungen in der Datendefinitionssprache (Data Definition Language, DDL) zum Konfigurieren der Verschlüsselung für Ihre Spalte oder zum Verwalten von Indizes für verschlüsselte Spalten sowie zum Abfragen verschlüsselter Spalten
ms.custom: ''
ms.date: 01/15/2021
ms.reviewer: vanto
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15 || = sqlallproducts-allversions'
ms.openlocfilehash: 130d2a9f2d92a77ab2d2e033f8e5f4fb9f88ef9f
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837661"
---
# <a name="run-transact-sql-statements-using-secure-enclaves"></a>Ausführen von Transact-SQL-Anweisungen mit Secure Enclaves

[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

[Always Encrypted mit Secure Enclaves](always-encrypted-enclaves.md) ermöglicht es einigen Transact-SQL-Anweisungen (T-SQL), vertrauliche Berechnungen in verschlüsselten Datenbankspalten in einer serverseitigen Secure Enclave auszuführen.

## <a name="statements-using-secure-enclaves"></a>Anweisungen mit Secure Enclaves

Die folgenden Arten von T-SQL-Anweisungen verwenden Secure Enclaves.

### <a name="ddl-statements-using-secure-enclaves"></a>DDL-Anweisungen mit Secure Enclaves

Für die folgenden Arten von [DDL](../../../t-sql/statements/statements.md#data-definition-language)-Anweisungen werden Secure Enclaves benötigt.

- [ALTER TABLE column_definition (Transact-SQL)](../../../t-sql/statements/alter-table-column-definition-transact-sql.md)-Anweisungen, die direkte kryptografische Vorgänge mithilfe von Enclave-fähigen Schlüsseln auslösen. Weitere Informationen hierzu finden Sie unter [Konfigurieren einer direkten Spaltenverschlüsselung mithilfe von Always Encrypted mit Secure Enclaves](always-encrypted-enclaves-configure-encryption.md).
- [CREATE INDEX (Transact-SQL)](../../../t-sql/statements/create-index-transact-sql.md)- und [ALTER INDEX (Transact-SQL)](../../../t-sql/statements/alter-index-transact-sql.md)-Anweisungen, die Indizes in Enclave-fähigen Spalten mithilfe von zufälliger Verschlüsselung erstellen oder ändern. Weitere Informationen finden Sie unter [Erstellen und Verwenden von Indizes in Spalten mithilfe von Always Encrypted mit Secure Enclaves](always-encrypted-enclaves-create-use-indexes.md).
  
### <a name="dml-statements-using-secure-enclaves"></a>DML-Anweisungen mit Secure Enclaves

Für die folgenden Anweisungen in der [Datenbearbeitungssprache](../../../t-sql/statements/statements.md#data-manipulation-language) (Data Manipulation Language, DML) oder Abfragen von Enclave-fähigen Spalten mithilfe von zufälliger Verschlüsselung werden Secure Enclaves benötigt:

- Abfragen, die mindestens einen der folgenden Transact-SQL-Operatoren verwenden, die in Secure Enclaves unterstützt werden:
  - [Comparison Operators (Vergleichsoperatoren)](../../../mdx/comparison-operators.md)
  - [BETWEEN (Transact-SQL)](../../../t-sql/language-elements/between-transact-sql.md)
  - [IN (Transact-SQL)](../../../t-sql/language-elements/in-transact-sql.md)
  - [LIKE (Transact-SQL)](../../../t-sql/language-elements/like-transact-sql.md)
  - [DISTINCT](../../../t-sql/queries/select-transact-sql.md#c-using-distinct-with-select)
  - [Joins](../../performance/joins.md)[!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)] unterstützt nur Joins geschachtelter Schleifen. [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] unterstützt Joins geschachtelter Schleifen, Hashjoins und Zusammenführungsjoins.
  - [SELECT - ORDER BY-Klausel (Transact-SQL):](../../../t-sql/queries/select-order-by-clause-transact-sql.md) Wird in [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] unterstützt. Nicht unterstützt in [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)]
  - [SELECT - GROUP BY-Klausel (Transact-SQL):](../../../t-sql/queries/select-group-by-transact-sql.md) Wird in [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] unterstützt. Nicht unterstützt in [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)]
- Abfragen, mit denen Zeilen eingefügt, aktualisiert oder gelöscht werden, die wiederum das Einfügen und/oder Entfernen eines Indexschlüssels in einen/aus einem Index für eine Enclave-fähige Spalte auslösen. Weitere Informationen finden Sie unter [Erstellen und Verwenden von Indizes in Spalten mithilfe von Always Encrypted mit Secure Enclaves](always-encrypted-enclaves-create-use-indexes.md).

> [!NOTE]
> Vorgänge für Indizes und vertrauliche DML-Abfragen mithilfe von Enclaves werden nur für Enclave-fähige Spalten unterstützt, für die die zufällige Verschlüsselung verwendet wird. Die deterministische Verschlüsselung wird nicht unterstützt.
>
> In [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)] muss für vertrauliche Abfragen, die Enclaves in Zeichenfolgenspalten (`char`, `nchar`) verwenden, eine binary2-Sortierung (BIN2) für die Spalten konfiguriert werden. In [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] müssen BIN2- oder UTF-8-Sortierungen verwendet werden.

### <a name="dbcc-commands-using-secure-enclaves"></a>DBCC-Befehle mit Secure Enclaves

Für [DBCC-Verwaltungsbefehle (Transact-SQL)](../../../t-sql/database-console-commands/dbcc-transact-sql.md), die das Überprüfen der Integrität von Indizes umfassen, werden möglicherweise ebenfalls Secure Enclaves benötigt, wenn die Datenbank Indizes für Enclave-fähige Spalten enthält, die die zufällige Verschlüsselung verwenden. Beispielsweise [DBCC CHECKDB (Transact-SQL)](../../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) und [DBCC CHECKTABLE (Transact-SQL)](../../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md).

## <a name="prerequisites-for-running-statements-using-secure-enclaves"></a>Voraussetzungen für das Ausführen von Anweisungen mit Secure Enclaves

Ihre Umgebung muss die folgenden Anforderungen erfüllen, damit das Ausführen von Anweisungen unterstützt wird, die eine Secure Enclave verwenden.

- Die [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Instanz oder die Datenbank und der Server in [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] müssen ordnungsgemäß für die Unterstützung von Enclaves und Nachweisen konfiguriert werden. Weitere Informationen finden Sie unter [Einrichten von Secure Enclaves und Nachweisen](configure-always-encrypted-enclaves.md#set-up-the-secure-enclave-and-attestation).
- Sie benötigen eine Nachweis-URL aus Ihrer Umgebung vom Dienstadministrator, der für Nachweise zuständig ist.

  - Wenn Sie [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] und den Host-Überwachungsdienst (Host Guardian Service, HGS) verwenden, finden Sie weitere Informationen unter [Ermitteln und Teilen der Nachweis-URL für HGS](always-encrypted-enclaves-host-guardian-service-deploy.md#step-6-determine-and-share-the-hgs-attestation-url).
  - Wenn Sie [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] und Microsoft Azure Attestation verwenden, finden Sie weitere Informationen unter [Ermitteln der Nachweis-URL für Ihre Nachweisrichtlinie](./always-encrypted-enclaves.md?view=sql-server-ver15#secure-enclave-attestation).

- Wenn Sie über Ihre Anwendung eine Verbindung mit der Datenbank herstellen, muss diese einen Clienttreiber verwenden, der Always Encrypted mit Secure Enclaves unterstützt. Die Anwendung muss eine Verbindung mit der Datenbank herstellen. Hierbei muss Always Encrypted für die Datenbankverbindung aktiviert, und das Nachweisprotokoll und die Nachweis-URL müssen ordnungsgemäß konfiguriert sein. Ausführliche Informationen finden Sie unter [Entwickeln von Anwendungen mithilfe von Always Encrypted mit Secure Enclaves](always-encrypted-enclaves-client-development.md).
- Wenn Sie SQL Server Management Studio (SSMS) oder Azure SQL Data Studio verwenden, müssen Sie Always Encrypted aktivieren und das Nachweisprotokoll und die Nachweis-URL konfigurieren, wenn Sie eine Verbindung mit der Datenbank herstellen. Weitere Informationen hierzu finden Sie in den nachfolgenden Abschnitten.

> [!NOTE]
> Wenn Sie zwischengespeicherte Spaltenverschlüsselungsschlüssel verwenden, müssen Sie für die folgenden Vorgänge weder mithilfe von Always Encrypted eine Verbindung mit der Datenbank herstellen noch Nachweise konfigurieren: DDL-Abfragen, die Indizes erstellen oder ändern, DML-Abfragen, die Indizes aktualisieren, und DBCC-Befehle, die die Indexintegrität überprüfen. Weitere Informationen finden Sie unter [Aufrufen der Indizierungsvorgänge mit zwischengespeicherten Spaltenverschlüsselungsschlüsseln](always-encrypted-enclaves-create-use-indexes.md#invoke-indexing-operations-using-cached-column-encryption-keys).

### <a name="prerequisites-for-running-t-sql-statements-using-enclaves-in-ssms"></a>Voraussetzungen für das Ausführen von T-SQL-Anweisungen mithilfe von Enclaves in SSMS

Mindestversion für SQL Server Management Studio:

- SSMS 18.3 bei Verwendung von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]
- SSMS 18.8 bei Verwendung von [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)]

Achten Sie darauf, Ihre Anweisungen über ein Abfragefenster auszuführen, das eine Verbindung mit Always Encrypted verwendet, für die die richtige Nachweis-URL konfiguriert ist.

1. Geben Sie im Dialogfeld **Mit Server verbinden** Ihren Servernamen, eine Authentifizierungsmethode und Ihre Anmeldeinformationen an.
2. Klicken Sie auf **Optionen >>** , und wählen Sie die Registerkarte **Always Encrypted** aus.
3. Aktivieren Sie das Kontrollkästchen **Always Encrypted (Spaltenverschlüsselung) aktivieren**, und geben Sie die Nachweis-URL für die Enclave an. Zum Beispiel: `https://hgs.bastion.local/Attestation` oder `https://contososqlattestation.uks.attest.azure.net/attest/SgxEnclave`.

    ![Herstellen einer Verbindung mit dem Server mit einem Nachweis unter Verwendung von SSMS](./media/always-encrypted-enclaves/column-encryption-setting.png)

4. Klicken Sie auf **Verbinden**.
5. Wenn Sie aufgefordert werden, die Parametrisierung für Always Encrypted-Abfragen zu aktivieren, klicken Sie auf **Aktivieren**, wenn Sie parametrisierte DML-Abfragen ausführen möchten. Weitere Informationen finden Sie unter [Parametrisierung für Always Encrypted](always-encrypted-query-columns-ssms.md#param).

Weitere Informationen finden Sie unter [Aktivieren und Deaktivieren von Always Encrypted für eine Datenbankverbindung](always-encrypted-query-columns-ssms.md#en-dis).

### <a name="prerequisites-for-running-t-sql-statements-using-enclaves-in-azure-data-studio"></a>Voraussetzungen für das Ausführen von T-SQL-Anweisungen mithilfe von Enclaves in Azure Data Studio

Es wird empfohlen, mindestens Version **1.23** zu verwenden.

Achten Sie darauf, Ihre Anweisungen über ein Abfragefenster auszuführen, das eine Verbindung mit Always Encrypted verwendet, für die sowohl das richtige Nachweisprotokoll als auch die richtige Nachweis-URL konfiguriert ist.

1. Klicken Sie im Dialogfeld **Verbindung** auf **Erweitert...** .
2. Um Always Encrypted für die Verbindung zu aktivieren, legen Sie das Feld **Always Encrypted** auf **Aktiviert** fest.
3. Geben Sie das Nachweisprotokoll und die Nachweis-URL an.
    - Wenn Sie [!INCLUDE [sssql19-md](../../../includes/sssql19-md.md)] verwenden, legen Sie für **Attestation Protocol** (Nachweisprotokoll) **Host-Überwachungsdienst** fest, und geben Sie die Nachweis-URL für den Host-Überwachungsdienst in das Feld **Enclave Attestation URL** (Nachweis-URL für die Enclave) ein.
    - Wenn Sie [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] verwenden, legen Sie **Attestation Protocol** (Nachweisprotokoll) auf **Azure Attestation** fest, und geben Sie die Nachweis-URL für Ihre Richtlinie in Microsoft Azure Attestation in das Feld **Enclave Attestation URL** (Nachweis-URL für die Enclave) ein.

    ![Herstellen einer Verbindung mit dem Server mit einem Nachweis unter Verwendung von Azure Data Studio](./media/always-encrypted-enclaves/azure-data-studio-connect-with-enclaves.png)

4. Klicken Sie auf **OK**, um **Erweiterte Eigenschaften** zu schließen.

Weitere Informationen finden Sie unter [Aktivieren und Deaktivieren von Always Encrypted für eine Datenbankverbindung](always-encrypted-query-columns-ads.md#enabling-and-disabling-always-encrypted-for-a-database-connection).

Wenn Sie parametrisierte DML-Abfragen ausführen möchten, müssen Sie ebenfalls die [Parametrisierung für Always Encrypted](always-encrypted-query-columns-ads.md#parameterization-for-always-encrypted) aktivieren.

## <a name="examples"></a>Beispiele

Dieser Abschnitt enthält Beispiele für DML-Abfragen mit Enclaves. 

In den Beispielen wird das folgende Schema verwendet:

```sql
CREATE SCHEMA [HR];
GO

CREATE TABLE [HR].[Jobs](
 [JobID] [int] IDENTITY(1,1) PRIMARY KEY,
 [JobTitle] [nvarchar](50) NOT NULL,
 [MinSalary] [money] ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL,
 [MaxSalary] [money] ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL
);
GO

CREATE TABLE [HR].[Employees](
 [EmployeeID] [int] IDENTITY(1,1) PRIMARY KEY,
 [SSN] [char](11) ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL,
 [FirstName] [nvarchar](50) NOT NULL,
 [LastName] [nvarchar](50) NOT NULL,
 [Salary] [money] ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL,
 [JobID] [int] NULL,
 FOREIGN KEY (JobID) REFERENCES [HR].[Jobs] (JobID)
);
GO
```

### <a name="exact-match-search"></a>Suchen nach genauen Übereinstimmungen

Die folgende Abfrage führt eine Suche nach genauen Einstellungen für die verschlüsselte `SSN`-Zeichenfolgenspalte durch.

```sql
DECLARE @SSN char(11) = '795-73-9838';
SELECT * FROM [HR].[Employees] WHERE [SSN] = @SSN;
GO
```

### <a name="pattern-matching-search"></a>Abgleichen von Mustern

Die folgende Abfrage führt einen Musterabgleich für die verschlüsselte `SSN`-Zeichenfolgenspalte durch. Hierbei wird nach Mitarbeitern gesucht, deren Sozialversicherungsnummern mit den angegebenen Ziffern enden.

```sql
DECLARE @SSN char(11) = '795-73-9838';
SELECT * FROM [HR].[Employees] WHERE [SSN] = @SSN;
GO
```

### <a name="range-comparison"></a>Bereichsvergleich

Die folgende Abfrage führt einen Bereichsvergleich für die verschlüsselte `Salary`-Spalte durch und sucht Mitarbeiter, deren Gehälter im angegebenen Bereich liegen.

```sql
DECLARE @MinSalary money = 40000;
DECLARE @MaxSalary money = 45000;
SELECT * FROM [HR].[Employees] WHERE [Salary] > @MinSalary AND [Salary] < @MaxSalary;
GO
```

### <a name="joins"></a>Joins

Die folgende Abfrage führt einen Join zwischen `Employees` und `Jobs`-Tabellen mithilfe der verschlüsselten `Salary`-Spalte aus. Die Abfrage ruft Mitarbeiter ab, deren Gehälter außerhalb eines bestimmten Bereichs für seinen Job liegt.

```sql
SELECT * FROM [HR].[Employees] e
JOIN [HR].[Jobs] j
ON e.[JobID] = j.[JobID] AND e.[Salary] > j.[MaxSalary] OR e.[Salary] < j.[MinSalary];
GO
```

### <a name="sorting"></a>Sortierung

Mit der folgenden Abfrage werden die Mitarbeiterdatensätze basierend auf der verschlüsselten `Salary`-Spalte sortiert, sodass 10 Mitarbeiter mit den höchsten Gehältern abgerufen werden.
> [!NOTE]
> Das Sortieren verschlüsselter Spalten wird in [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)], aber nicht in [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)]unterstützt.

```sql
SELECT TOP(10) * FROM [HR].[Employees]
ORDER BY [Salary] DESC;
GO
```

## <a name="next-steps"></a>Nächste Schritte

- [Entwickeln von Anwendungen mithilfe von Always Encrypted mit Secure Enclaves](always-encrypted-enclaves-client-development.md)

## <a name="see-also"></a>Weitere Informationen

- [Behandeln von häufig auftretenden Problemen bei Always Encrypted mit Secure Enclaves](always-encrypted-enclaves-troubleshooting.md)
- [Tutorial: Erste Schritte mit Always Encrypted mit Secure Enclaves in SQL Server](../tutorial-getting-started-with-always-encrypted-enclaves.md)
- [Tutorial: Erste Schritte mit Always Encrypted mit Secure Enclaves in Azure SQL-Datenbank](/azure/azure-sql/database/always-encrypted-enclaves-getting-started)
- [Konfigurieren einer direkten Spaltenverschlüsselung mithilfe von Always Encrypted mit Secure Enclaves](always-encrypted-enclaves-configure-encryption.md)
- [Erstellen und Verwenden von Indizes in Spalten mithilfe von Always Encrypted mit Secure Enclaves](always-encrypted-enclaves-create-use-indexes.md)