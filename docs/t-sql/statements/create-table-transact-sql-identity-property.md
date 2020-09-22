---
description: CREATE TABLE (Transact-SQL) IDENTITY (Eigenschaft)
title: IDENTITY (Eigenschaft) (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- IDENTITY_TSQL
- IDENTITY
dev_langs:
- TSQL
helpviewer_keywords:
- IDENTITY property
- columns [SQL Server], creating
- identity columns [SQL Server], IDENTITY property
- autonumbers, identity numbers
ms.assetid: 8429134f-c821-4033-a07c-f782a48d501c
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6ebc5291ae6be0de2c1ea8961e05c4238db17b2d
ms.sourcegitcommit: ac9feb0b10847b369b77f3c03f8200c86ee4f4e0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/16/2020
ms.locfileid: "90688846"
---
# <a name="create-table-transact-sql-identity-property"></a>CREATE TABLE (Transact-SQL) IDENTITY (Eigenschaft)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Erstellt eine Identitätsspalte in einer Tabelle. Diese Eigenschaft wird in den [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen CREATE TABLE und ALTER TABLE verwendet.  
  
> [!NOTE]  
>  Die IDENTITY-Eigenschaft unterscheidet sich von der SQL-DMO-Eigenschaft **IDENTITY**, die die IDENTITY-Eigenschaft für Zeilen einer Spalte verfügbar macht.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql  
IDENTITY [ (seed , increment) ]
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *seed*  
 Der Wert, der für die erste in die Tabelle geladene Zeile verwendet wird.  
  
 *increment*  
 Der inkrementelle Wert, der zum Identitätswert der zuvor geladenen Zeile addiert wird.

 > [!NOTE]
 > In Azure Synapse Analytics sind Werte für die Identität aufgrund der verteilten Architektur des Data Warehouse nicht inkrementell. Weitere Informationen finden Sie unter [Erstellen von Ersatzschlüsseln im Synapse SQL-Pool mit IDENTITY](/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-tables-identity#allocation-of-values).
  
 Sie müssen entweder sowohl den Ausgangswert als auch den inkrementellen Wert oder keinen von beiden angeben. Wurden Ausgangswert und inkrementeller Wert nicht angegeben, ist der Standardwert (1,1).  
  
## <a name="remarks"></a>Bemerkungen  
 Identitätsspalten können zum Generieren von Schlüsselwerten verwendet werden. Die Identitätseigenschaft für eine Spalte garantiert Folgendes:  
  
-   Jeder neue Wert wird auf Grundlage des aktuellen Seeds und Inkrements generiert.  
  
-   Jeder neue Wert für eine bestimmte Transaktion unterscheidet sich von anderen gleichzeitigen Transaktionen für die Tabelle.  
  
 Die Identitätseigenschaft für eine Spalte garantiert nicht Folgendes:  
  
-   **Eindeutigkeit des Werts**: Die Eindeutigkeit muss mit einer **PRIMARY KEY**- oder **UNIQUE**-Einschränkung bzw. einem **UNIQUE**-Index erzwungen werden. - 
 
> [!NOTE]
> Azure Synapse Analytics unterstützt weder **PRIMARY KEY**- oder **UNIQUE**-Einschränkungen noch einen **UNIQUE**-Index. Weitere Informationen finden Sie unter [Erstellen von Ersatzschlüsseln im Synapse SQL-Pool mit IDENTITY](/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-tables-identity#what-is-a-surrogate-key).

-   **Aufeinanderfolgende Werte innerhalb einer Transaktion**: Bei einer Transaktion, durch die mehrere Zeilen eingefügt werden, ist nicht sichergestellt, dass Sie aufeinanderfolgende Werte für die Zeilen erhalten, da für die Tabelle möglicherweise andere gleichzeitige Einfügungsvorgänge stattfinden. Wenn Werte fortlaufend sein müssen, sollte die Transaktion eine exklusive Sperre für die Tabelle oder die Isolationsstufe **SERIALIZABLE** verwenden.  
  
-   **Aufeinanderfolgende Werte nach Serverneustart oder anderen Fehlern** -[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] kann Identitätswerte aus Leistungsgründen zwischenspeichern. Einige der zugewiesenen Werte können während eines Datenbankausfalls oder Serverneustarts verloren gehen. Dies kann zu Lücken im Identitätswert beim Einfügen führen. Wenn Lücken nicht zulässig sind, sollte die Anwendung ihren eigenen Mechanismus verwenden, um Schlüsselwerte zu generieren. Die Verwendung eines Sequenzgenerators mit der **NOCACHE**-Option kann die Lücken auf Transaktionen beschränken, für die nie ein Commit ausgeführt wird.  
  
-   **Wiederverwendung von Werten**: Für eine bestimmte Identitätseigenschaft mit spezifischem Ausgangswert/Inkrement werden die Identitätswerte von der Engine nicht wiederverwendet. Wenn eine bestimmte INSERT-Anweisung fehlschlägt oder für die INSERT-Anweisung ein Rollback ausgeführt wird, gehen die verwendeten Identitätswerte verloren und werden nicht erneut generiert. Es können Lücken entstehen, wenn die nachfolgenden Identitätswerte generiert werden.  
  
 Diese Einschränkungen sind beabsichtigt, um die Leistung zu verbessern. Ferner sind sie in vielen Situationen akzeptabel. Wenn Sie Identitätswerte aufgrund dieser Einschränkungen nicht verwenden können, sollten Sie eine separate Tabelle mit einem aktuellen Wert erstellen und den Zugriff auf die Tabelle und die Nummernzuweisung für die Anwendung verwalten.  
  
 Wenn eine Tabelle mit einer Identitätsspalte für die Replikation veröffentlicht wird, muss die Identitätsspalte entsprechend dem verwendeten Replikationstyp verwaltet werden. Weitere Informationen finden Sie unter [Replizieren von Identitätsspalten](../../relational-databases/replication/publish/replicate-identity-columns.md).  
  
 Es kann nur eine Identitätsspalte pro Tabelle erstellt werden.  
  
 In speicheroptimierten Tabellen müssen sowohl der Ausgangswert als auch das Inkrement auf 1,1 festgelegt werden. Wenn Sie den Seed oder das Inkrement auf einen Wert festlegen, der nicht 1 ist, wird der folgende Fehler ausgelöst: The use of seed and increment values other than 1 is not supported with memory optimized tables. (Die Verwendung von Seed- und Inkrementwerten, die nicht 1 betragen, wird für arbeitsspeicheroptimierte Tabellen nicht unterstützt.)  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-using-the-identity-property-with-create-table"></a>A. Verwenden der IDENTITY-Eigenschaft mit CREATE TABLE  
 Im folgenden Beispiel wird eine neue Tabelle erstellt. Dabei wird die `IDENTITY`-Eigenschaft zum automatischen Erhöhen der Identifikationsnummer verwendet.  
  
```sql  
USE AdventureWorks2012;  
  
IF OBJECT_ID ('dbo.new_employees', 'U') IS NOT NULL  
   DROP TABLE new_employees;  
GO  
CREATE TABLE new_employees  
(  
 id_num int IDENTITY(1,1),  
 fname varchar (20),  
 minit char(1),  
 lname varchar(30)  
);  
  
INSERT new_employees  
   (fname, minit, lname)  
VALUES  
   ('Karin', 'F', 'Josephs');  
  
INSERT new_employees  
   (fname, minit, lname)  
VALUES  
   ('Pirkko', 'O', 'Koskitalo');  
```  
  
### <a name="b-using-generic-syntax-for-finding-gaps-in-identity-values"></a>B. Verwenden generischer Syntax zum Auffinden von Lücken in Identitätswerten  
 Im folgenden Beispiel wird generische Syntax zum Auffinden von Lücken in Identitätswerten dargestellt. Diese Lücken entstehen, wenn Daten entfernt werden.  
  
> [!NOTE]  
>  Der erste Teil des folgenden [!INCLUDE[tsql](../../includes/tsql-md.md)]-Skripts dient lediglich zur Veranschaulichung. Sie können das [!INCLUDE[tsql](../../includes/tsql-md.md)]-Skript ausführen, das mit folgendem Kommentar beginnt: `-- Create the img table`.  
  
```sql 
-- Here is the generic syntax for finding identity value gaps in data.  
-- The illustrative example starts here.  
SET IDENTITY_INSERT tablename ON;  
DECLARE @minidentval column_type;  
DECLARE @maxidentval column_type;  
DECLARE @nextidentval column_type;  
SELECT @minidentval = MIN($IDENTITY), @maxidentval = MAX($IDENTITY)  
    FROM tablename  
IF @minidentval = IDENT_SEED('tablename')  
   SELECT @nextidentval = MIN($IDENTITY) + IDENT_INCR('tablename')  
   FROM tablename t1  
   WHERE $IDENTITY BETWEEN IDENT_SEED('tablename') AND   
      @maxidentval AND  
      NOT EXISTS (SELECT * FROM tablename t2  
         WHERE t2.$IDENTITY = t1.$IDENTITY +   
            IDENT_INCR('tablename'))  
ELSE  
   SELECT @nextidentval = IDENT_SEED('tablename');  
SET IDENTITY_INSERT tablename OFF;  
-- Here is an example to find gaps in the actual data.  
-- The table is called img and has two columns: the first column   
-- called id_num, which is an increasing identification number, and the   
-- second column called company_name.  
-- This is the end of the illustration example.  
  
-- Create the img table.  
-- If the img table already exists, drop it.  
-- Create the img table.  
IF OBJECT_ID ('dbo.img', 'U') IS NOT NULL  
   DROP TABLE img;  
GO  
CREATE TABLE img (id_num INT IDENTITY(1,1), company_name sysname);  
INSERT img(company_name) VALUES ('New Moon Books');  
INSERT img(company_name) VALUES ('Lucerne Publishing');  
-- SET IDENTITY_INSERT ON and use in img table.  
SET IDENTITY_INSERT img ON;  
  
DECLARE @minidentval SMALLINT;  
DECLARE @nextidentval SMALLINT;  
SELECT @minidentval = MIN($IDENTITY) FROM img  
 IF @minidentval = IDENT_SEED('img')  
    SELECT @nextidentval = MIN($IDENTITY) + IDENT_INCR('img')  
    FROM img t1  
    WHERE $IDENTITY BETWEEN IDENT_SEED('img') AND 32766 AND  
      NOT    EXISTS (SELECT * FROM img t2  
          WHERE t2.$IDENTITY = t1.$IDENTITY + IDENT_INCR('img'))  
 ELSE  
    SELECT @nextidentval = IDENT_SEED('img');  
SET IDENTITY_INSERT img OFF;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [DBCC CHECKIDENT &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkident-transact-sql.md)   
 [IDENT_INCR &#40;Transact-SQL&#41;](../../t-sql/functions/ident-incr-transact-sql.md)   
 [@@IDENTITY &#40;Transact-SQL&#41;](../../t-sql/functions/identity-transact-sql.md)   
 [IDENTITY &#40;Funktion&#41; &#40;Transact-SQL&#41;](../../t-sql/functions/identity-function-transact-sql.md)   
 [IDENT_SEED &#40;Transact-SQL&#41;](../../t-sql/functions/ident-seed-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [SET IDENTITY_INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/set-identity-insert-transact-sql.md)   
 [Replizieren von Identitätsspalten](../../relational-databases/replication/publish/replicate-identity-columns.md)  
  
  
