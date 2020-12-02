---
description: Angeben von berechneten Spalten in einer Tabelle
title: Angeben von berechneten Spalten in einer Tabelle | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- computed columns, define
ms.assetid: 731a4576-09c1-47f0-a8f6-edd0b55679f4
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ee1d32ce60064d6ab42b04a9aeddec4a6252d479
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/26/2020
ms.locfileid: "88488598"
---
# <a name="specify-computed-columns-in-a-table"></a>Angeben von berechneten Spalten in einer Tabelle

[!INCLUDE[tsql-appliesto-ss-asdb-xxxx-xxx-md](../../includes/applies-to-version/sql-asdb.md)]

Eine berechnete Spalte ist eine virtuelle Spalte, die nicht physisch in der Tabelle gespeichert ist, es sei denn, die Spalte wurde (mit PERSISTED) als persistente Spalte markiert. Der Ausdruck für eine berechnete Spalte kann aus Daten anderer Spalten einen Wert für die Spalte berechnen, der er zuwiesen ist. Sie können mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[tsql](../../includes/tsql-md.md)] einen Ausdruck für eine berechnete Spalte in [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] angeben.

**In diesem Thema**

- **Vorbereitungen:**

   [Einschränkungen](#Limitations)

   [Security](#Security)

- **So geben Sie eine berechnete Spalte an mit:**

   [SQL Server Management Studio](#SSMSProcedure)

   [Transact-SQL](#TsqlProcedure)

## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen

### <a name="limitations-and-restrictions"></a><a name="Limitations"></a> Einschränkungen

- Eine berechnete Spalte kann nicht als DEFAULT- oder FOREIGN KEY-Einschränkungsdefinition oder mit einer NOT NULL-Einschränkungsdefinition verwendet werden. Eine berechnete Spalte kann jedoch als Schlüsselspalte in einem Index oder als Teil einer PRIMARY KEY- oder UNIQUE-Einschränkung verwendet werden, wenn der Wert der berechneten Spalte durch einen deterministischen Ausdruck definiert ist und der Datentyp des Ergebnisses in Indexspalten zulässig ist. Falls die Tabelle z. B. die Spalten "a" und "b" vom Datentyp int aufweist, kann die berechnete Spalte "a + b" indiziert werden. Dagegen kann die berechnete Spalte "a + DATEPART(dd, GETDATE())" nicht indiziert werden, da sich der Wert in späteren Aufrufen ändern kann.
- Eine berechnete Spalte kann nicht das Ziel einer INSERT- oder UPDATE-Anweisung sein.

### <a name="security"></a><a name="Security"></a> Sicherheit

#### <a name="permissions"></a><a name="Permissions"></a> Berechtigungen

Erfordert die ALTER-Berechtigung für die Tabelle.

## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio

### <a name="to-add-a-new-computed-column"></a><a name="NewColumn"></a> So fügen Sie eine neue berechnete Spalte hinzu

1. Erweitern Sie in **Objekt-Explorer** die Tabelle, der Sie die neue berechnete Spalte hinzufügen möchten. Klicken Sie mit der rechten Maustaste auf **Spalten** , und wählen Sie **Neue Spalte** aus.
2. Geben Sie den Spaltennamen ein, und akzeptieren Sie den Standarddatentyp (**nchar**(10)). [!INCLUDE[ssDE](../../includes/ssde-md.md)] ermittelt den Datentyp der berechneten Spalte, indem die Regeln zur Rangfolge von Datentypen auf Ausdrücke angewendet werden, die in der Formel angegeben wurden. Wenn die Formel beispielsweise auf eine Spalte vom Typ **money** und eine Spalte vom Typ **int** verweist, weist die berechnete Spalte den Typ **money** auf, da dieser Datentyp die höhere Rangfolge hat. Weitere Informationen finden Sie unter [Rangfolge der Datentypen &#40;Transact-SQL&#41;](../../t-sql/data-types/data-type-precedence-transact-sql.md).
3. Erweitern Sie auf der Registerkarte **Spalteneigenschaften** die **BerechneteSpalteSpezifikation** -Eigenschaft.
4. Geben Sie für die untergeordnete Eigenschaft **(Formel)** in der Datenblattzelle auf der rechten Seite den Ausdruck für diese Spalte ein. In einer `SalesTotal` -Spalte kann die eingegebene Formel z. B. `SubTotal+TaxAmt+Freight`lauten, wodurch für jede Tabellenzeile der Wert in diesen Spalten hinzugefügt wird.

   > [!IMPORTANT]
   > Wenn in einer Formel zwei Ausdrücke verschiedener Datentypen kombiniert sind, geben die Rangfolgeregeln für Datentypen an, dass der Datentyp mit der niedrigeren Rangfolge in den Datentyp mit der höheren Rangfolge konvertiert wird. Wenn es sich bei der Konvertierung nicht um eine unterstützte implizite Konvertierung handelt, wird der Fehler "`Error validating the formula for column column_name.`" zurückgegeben. Verwenden Sie die CAST-Funktion oder CONVERT-Funktion, um den Datentypkonflikt aufzulösen. Wenn beispielsweise eine Spalte vom Typ **nvarchar** mit einer Spalte vom Typ **int** kombiniert wird, muss der ganzzahlige Typ in **nvarchar** konvertiert werden, wie in der folgenden Formel `('Prod'+CONVERT(nvarchar(23),ProductID))`dargestellt. Weitere Informationen finden Sie unter [CAST und CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md).

5. Geben Sie an, ob die Daten persistent gespeichert werden sollen, indem Sie im Dropdownfeld für die untergeordnete Eigenschaft **IsPersisted** die Option **Ja** oder **Nein** wählen.

6. Klicken Sie im Menü **Datei** auf _Tabellenname_ **speichern**.

#### <a name="to-add-a-computed-column-definition-to-an-existing-column"></a>So fügen Sie einer vorhandenen Spalte die Definition einer berechneten Spalte hinzu

1. Klicken Sie im **Objekt-Explorer** mit der rechten Maustaste auf die Tabelle mit der Spalte, für die Sie den Ordner **Spalten** ändern und erweitern möchten.
2. Klicken Sie mit der rechten Maustaste auf die Spalte, für die Sie die Formel einer berechneten Spalte angeben möchten, und klicken Sie auf **Löschen**. Klicken Sie auf **OK**.
3. Fügen Sie eine neue Spalte hinzu, und geben Sie die Formel der berechneten Spalte an, indem Sie die vorangehenden Schritte ausführen, um eine neue berechnete Spalte hinzuzufügen.

## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL

### <a name="to-add-a-computed-column-when-creating-a-table"></a>So fügen Sie eine berechnete Spalte beim Erstellen einer Tabelle hinzu

Im folgenden Beispiel wird eine Tabelle mit einer berechneten Spalte erstellt, die den Wert in der Spalte `QtyAvailable` mit dem Wert in der Spalte `UnitPrice` multipliziert.

```sql
CREATE TABLE dbo.Products
   (
      ProductID int IDENTITY (1,1) NOT NULL
      , QtyAvailable smallint
      , UnitPrice money
      , InventoryValue AS QtyAvailable * UnitPrice
    )
;
-- Insert values into the table.
INSERT INTO dbo.Products (QtyAvailable, UnitPrice)
   VALUES (25, 2.00), (10, 1.5)
;
-- Display the rows in the table.
SELECT ProductID, QtyAvailable, UnitPrice, InventoryValue
FROM dbo.Products
;
```

### <a name="to-add-a-new-computed-column-to-an-existing-table"></a>So fügen Sie einer vorhandenen Tabelle eine neue berechnete Spalte hinzu

Im folgenden Beispiel wird der im vorangehenden Beispiel erstellten Tabelle eine neue Spalte hinzugefügt.

```sql
ALTER TABLE dbo.Products ADD RetailValue AS (QtyAvailable * UnitPrice * 1.5)
;
```

Fügen Sie optional das PERSISTED-Argument hinzu, um die berechneten Werte physisch in der Tabelle zu speichern:

```sql
ALTER TABLE dbo.Products ADD RetailValue AS (QtyAvailable * UnitPrice * 1.5) PERSISTED
;
```

### <a name="to-change-an-existing-column-to-a-computed-column"></a>So ändern Sie eine vorhandene Spalte in eine berechnete Spalte

Im folgenden Beispiel wird die im vorangehenden Beispiel hinzugefügte Spalte geändert.

```sql
ALTER TABLE dbo.Products DROP COLUMN RetailValue
;
GO
ALTER TABLE dbo.Products ADD RetailValue AS (QtyAvailable * UnitPrice * 1.5)
;
```

Weitere Informationen finden Sie unter [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md).
