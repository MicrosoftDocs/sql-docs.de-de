---
title: Massenzugriff auf Daten in Azure Blob Storage
description: Transact-SQL-Beispiele, die BULK INSERT und OPENROWSET für den Zugriff auf Daten in einem Azure Blob Storage-Konto verwenden.
ms.date: 10/22/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- bulk importing [SQL Server], from Azure blob storage
- Azure blob storage, bulk import to SQL Server
- BULK INSERT, Azure blob storage
- OPENROWSET, Azure blob storage
ms.assetid: f7d85db3-7a93-400e-87af-f56247319ecd
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=sql-server-2017||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: b067b668cfdf0dc42596d7fb6858240ad8d944e9
ms.sourcegitcommit: 4d370399f6f142e25075b3714e5c2ce056b1bfd0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91867713"
---
# <a name="examples-of-bulk-access-to-data-in-azure-blob-storage"></a>Beispiele für Massenzugriff auf Daten in Azure Blob Storage

[!INCLUDE[sqlserver2017-asdb](../../includes/applies-to-version/sqlserver2017-asdb.md)]

Die Anweisungen `BULK INSERT` und `OPENROWSET` können direkt auf eine Datei in Azure Blob Storage zugreifen. In den folgenden Beispielen werden Daten aus einer CSV-Datei (Comma Separated Value: durch Trennzeichen getrennte Werte) (mit dem Namen `inv-2017-01-19.csv`) verwendet, die in einem Container (mit dem Namen `Week3`) gespeichert ist, der in einem Speicherkonto (mit dem Namen `newinvoices`) gespeichert ist. Der Pfad zur Datei kann verwendet werden, ist aber nicht in diesen Beispielen enthalten.

Für den Massenzugriff auf Azure Blob Storage über SQL Server wird mindestens [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1 benötigt.

> [!IMPORTANT]
> Bei allen Pfaden zum Container und zu den Dateien in Blob Storage wird die Groß-/Kleinschreibung beachtet (`CASE SENSITIVE`). Wenn ein Pfad nicht korrekt ist, wird möglicherweise die folgende Fehlermeldung zurückgegeben: „Massenladen ist nicht möglich. Die Datei „file.csv“ ist nicht vorhanden, oder Sie besitzen keine Rechte für den Dateizugriff."

## <a name="create-the-credential"></a>Anmeldeinformationen erstellen

Alle nachfolgenden Beispiele erfordern für die komplette Datenbank gültige Anmeldeinformationen mit einem Verweis auf eine Shared Access Signature.

> [!IMPORTANT]
> Die externe Datenquelle muss mit für die komplette Datenbank gültige Anmeldeinformationen erstellt werden, die die `SHARED ACCESS SIGNATURE`-Identität verwenden. Informationen zum Erstellen einer Shared Access Signature für das Speicherkonto finden Sie unter der Eigenschaft **Shared Access Signature** auf der Eigenschaftenseite des Speicherkontos im Azure-Portal. Weitere Informationen zu SAS finden Sie unter [Verwenden von Shared Access Signatures (SAS)](/azure/storage/storage-dotnet-shared-access-signature-part-1). Weitere Informationen finden Sie unter [ (Erstellen von datenbankweit gültigen Anmeldeinformationen)](../../t-sql/statements/create-database-scoped-credential-transact-sql.md).

Erstellen Sie datenbankweit gültige Anmeldeinformationen mithilfe von `IDENTITY`, die auf `SHARED ACCESS SIGNATURE` festgelegt sein muss. Verwenden Sie das SAS-Token, das für das Blobspeicherkonto generiert wurde. Vergewissern Sie sich, dass Ihr SAS-Token keine führende `?` aufweist, dass Sie mindestens über Leseberechtigung für das Objekt verfügen, das geladen werden soll, und dass der Ablaufzeitraum gültig ist (alle Datumsangaben werden in UTC-Zeit angegeben).

Beispiel:

```sql
CREATE DATABASE SCOPED CREDENTIAL UploadInvoices
WITH IDENTITY = 'SHARED ACCESS SIGNATURE',
SECRET = 'sv=2018-03-28&ss=b&srt=sco&sp=rwdlac&se=2019-08-31T02:25:19Z&st=2019-07-30T18:25:19Z&spr=https&sig=KS51p%2BVnfUtLjMZtUTW1siyuyd2nlx294tL0mnmFsOk%3D';
```

## <a name="accessing-data-in-a-csv-file-referencing-an-azure-blob-storage-location"></a>Zugriff auf Daten in einer CSV-Datei, die auf einen Speicherort von Azure Blob Storage verweisen

Im folgenden Beispiel wird eine externe Datenquelle verwendet, die auf ein Azure-Speicherkonto mit dem Namen `MyAzureInvoices` verweist.

```sql
CREATE EXTERNAL DATA SOURCE MyAzureInvoices
    WITH (
        TYPE = BLOB_STORAGE,
        LOCATION = 'https://newinvoices.blob.core.windows.net',
        CREDENTIAL = UploadInvoices
    );
```

Die `OPENROWSET`-Anweisung fügt den Containernamen (`week3`) zur Dateibeschreibung hinzu. Die Datei heißt `inv-2017-01-19.csv`.

```sql
SELECT * FROM OPENROWSET(
   BULK 'week3/inv-2017-01-19.csv',
   DATA_SOURCE = 'MyAzureInvoices',
   FORMAT = 'CSV',
   FORMATFILE='invoices.fmt',
   FORMATFILE_DATA_SOURCE = 'MyAzureInvoices'
   ) AS DataFile;   
```

Verwenden Sie den Container und die Dateibeschreibung mithilfe von `BULK INSERT`:

```sql
BULK INSERT Colors2
FROM 'week3/inv-2017-01-19.csv'
WITH (DATA_SOURCE = 'MyAzureInvoices',
      FORMAT = 'CSV');
```

## <a name="accessing-data-in-a-csv-file-referencing-a-container-in-an-azure-blob-storage-location"></a>Zugriff auf Daten in einer CSV-Datei, die auf einen Container an einem Speicherort von Azure BLOB-Speicher verweisen

Im folgenden Beispiel wird eine externe Datenquelle verwendet, die auf einen Container (mit dem Namen `week3`) in einem Azure-Speicherkonto verweist.

```sql
CREATE EXTERNAL DATA SOURCE MyAzureInvoicesContainer
    WITH (
        TYPE = BLOB_STORAGE,
        LOCATION = 'https://newinvoices.blob.core.windows.net/week3',
        CREDENTIAL = UploadInvoices
    );
```

Die `OPENROWSET`-Anweisung enthält den Containernamen nicht in der Dateibeschreibung:

```sql
SELECT * FROM OPENROWSET(
   BULK 'inv-2017-01-19.csv',
   DATA_SOURCE = 'MyAzureInvoicesContainer',
   FORMAT = 'CSV',
   FORMATFILE='invoices.fmt',
   FORMATFILE_DATA_SOURCE = 'MyAzureInvoices'
   ) AS DataFile;
```

Verwenden Sie den Containernamen mithilfe von `BULK INSERT` nicht in der Dateibeschreibung:

```sql
BULK INSERT Colors2
FROM 'inv-2017-01-19.csv'
WITH (DATA_SOURCE = 'MyAzureInvoicesContainer',
      FORMAT = 'CSV');
```

## <a name="see-also"></a>Weitere Informationen

- [CREATE DATABASE SCOPED CREDENTIAL](../../t-sql/statements/create-database-scoped-credential-transact-sql.md)
- [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md)
- [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md)
- [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md)