---
description: ALTER EXTERNAL DATA SOURCE (Transact-SQL)
title: ALTER EXTERNAL DATA SOURCE (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/26/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER EXTERNAL DATA SOURCE
- ALTER_EXTERNAL_DATA_SOURCE
dev_langs:
- TSQL
helpviewer_keywords:
- polybase, alter external data source statement
- ALTER EXTERNAL DATA SOURCE statement
ms.assetid: a34b9e90-199d-46d0-817a-a7e69387bf5f
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 28b287969222e47501fd9e4fd18fef091a02a683
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337972"
---
# <a name="alter-external-data-source-transact-sql"></a>ALTER EXTERNAL DATA SOURCE (Transact-SQL)
[!INCLUDE [sqlserver2016-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdbmi-asa-pdw.md)]

  Ändert eine externe Datenquelle, die zum Erstellen einer externen Tabelle verwendet wird. Bei der externen Datenquelle kann es sich um Hadoop oder Azure Blob Storage (WASBS) für SQL Server und Azure Blob Storage (WASBS) oder um Azure Data Lake Storage (ABFSS/ADL) für [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] handeln. 

## <a name="syntax"></a>Syntax  

```syntaxsql
-- Modify an external data source
-- Applies to: SQL Server (2016 or later) and APS
ALTER EXTERNAL DATA SOURCE data_source_name SET
    {   
        LOCATION = '<prefix>://<path>[:<port>]' [,] |
        RESOURCE_MANAGER_LOCATION = <'IP address;Port'> [,] |
        CREDENTIAL = credential_name
    }  
    [;]  

-- Modify an external data source pointing to Azure Blob storage
-- Applies to: SQL Server (starting with 2017)
ALTER EXTERNAL DATA SOURCE data_source_name
    SET
        LOCATION = 'https://storage_account_name.blob.core.windows.net'
        [, CREDENTIAL = credential_name ] 

-- Modify an external data source pointing to Azure Blob storage or Azure Data Lake storage
-- Applies to: Azure Synapse Analytics
ALTER EXTERNAL DATA SOURCE data_source_name
    SET
        [LOCATION = '<location prefix>://<location path>']
        [, CREDENTIAL = credential_name ] 
```

## <a name="arguments"></a>Argumente  
 Data_source_name gibt den benutzerdefinierten Namen für die Datenquelle an. Der Name muss eindeutig sein.

 „LOCATION = '<prefix>://<path>[:<port>]'“: Dient zum Angeben des Konnektivitätsprotokolls, des Pfads und des Ports der externen Datenquelle. Weitere Informationen über gültige Speicherortoptionen finden Sie unter [CREATE EXTERNAL DATA SOURCE &#40;Transact-SQL&#41;](create-external-data-source-transact-sql.md#location--prefixpathport).

 RESOURCE_MANAGER_LOCATION = '\<IP address;Port>' (gilt nicht für [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]) gibt den Speicherort des Hadoop-Ressourcen-Managers an. Wenn angegeben, kann der Abfrageoptimierer festlegen, dass Daten für eine PolyBase-Abfrage mithilfe der Berechnungsfunktionen von Hadoop vorverarbeitet werden. Dies ist eine kostenbasierte Entscheidung. Dies wird Prädikatweitergabe genannt und kann die Menge der zwischen Hadoop und SQL übertragenen Daten deutlich reduzieren und damit die Abfrageleistung verbessern.

 CREDENTIAL = Credential_Name gibt die benannten Anmeldeinformationen an. Informationen hierzu finden Sie unter [CREATE DATABASE SCOPED CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/create-database-scoped-credential-transact-sql.md).

TYP = [HADOOP | BLOB_STORAGE]   
**Gilt für:** [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)]
Nur bei Massenvorgängen muss `LOCATION` die gültige URL für den Azure Blob Storage sein. Fügen Sie weder **/** , Dateinamen noch Shared Access Signature-Parameter am Ende der `LOCATION`-URL ein.
Die verwendeten Anmeldeinformationen müssen mithilfe von `SHARED ACCESS SIGNATURE` als Identität erstellt werden. Weitere Informationen zu SAS finden Sie unter [Verwenden von Shared Access Signatures (SAS)](/azure/storage/storage-dotnet-shared-access-signature-part-1).

  

## <a name="remarks"></a>Bemerkungen
 Es kann immer jeweils nur eine Quelle geändert werden. Gleichzeitige Anforderungen zur Änderung derselben Quelle führen dazu, dass eine Anweisung warten muss. Unterschiedliche Quellen können jedoch gleichzeitig geändert werden. Diese Anweisung kann gleichzeitig mit anderen Anweisungen ausgeführt werden.

## <a name="permissions"></a>Berechtigungen  
 Erfordert eine ALTER ANY EXTERNAL DATA SOURCE-Berechtigung.
 > [!IMPORTANT]  
 > Mit der Berechtigung ALTER ANY EXTERNAL DATA SOURCE besitzt jeder Prinzipal die Fähigkeit, beliebige externe Datenquellenobjekte zu erstellen und zu ändern. Damit ist auch der Zugriff auf alle datenbankweit gültigen Anmeldeinformationen der Datenbank möglich. Da es sich hierbei um eine weitreichende Berechtigung handelt, darf sie nur vertrauenswürdigen Prinzipalen innerhalb des Systems erteilt werden.


## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird der Speicherort und der Ressourcen-Manager-Speicherort einer vorhandenen Datenquelle geändert.

```sql  
ALTER EXTERNAL DATA SOURCE hadoop_eds SET
     LOCATION = 'hdfs://10.10.10.10:8020',
     RESOURCE_MANAGER_LOCATION = '10.10.10.10:8032'
    ;
```

 Im folgenden Beispiel werden die Anmeldeinformationen zur Verbindung mit einer vorhandenen Datenquelle geändert.

```sql 
ALTER EXTERNAL DATA SOURCE hadoop_eds SET
   CREDENTIAL = new_hadoop_user
    ;
```
 Im folgenden Beispiel werden die Anmeldeinformationen in einen neuen Speicherort geändert. Bei diesem Beispiel handelt es sich um einer externe Datenquelle für [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]. 

```sql  
ALTER EXTERNAL DATA SOURCE AzureStorage_west SET
   LOCATION = 'wasbs://loadingdemodataset@updatedproductioncontainer.blob.core.windows.net',
   CREDENTIAL = AzureStorageCredential
```