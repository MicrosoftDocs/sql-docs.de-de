---
title: Erstellen eines Datenbank-Hauptschlüssels | Microsoft-Dokumentation
description: Erstellen Sie einen Datenbankhauptschlüssel in SQL Server mithilfe von Transact-SQL. Stellen Sie sicher, dass Sie über die erforderlichen Berechtigungen verfügen.
ms.custom: ''
ms.date: 09/12/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- database master key [SQL Server], creating
ms.assetid: 8cb24263-e97d-4e4d-9429-6cf494a4d5eb
author: jaszymas
ms.author: jaszymas
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f97dbd4425b990cd2ec433afa817081683b38f45
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/02/2021
ms.locfileid: "99251415"
---
# <a name="create-a-database-master-key"></a>Erstellen eines Datenbank-Hauptschlüssels

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
In diesem Thema wird beschrieben, wie ein Datenbank-Hauptschlüssel in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] mit [!INCLUDE[tsql](../../../includes/tsql-md.md)]erstellt wird.

## <a name="security"></a>Sicherheit

### <a name="permissions"></a>Berechtigungen

Erfordert die CONTROL-Berechtigung für die Datenbank.

## <a name="using-transact-sql"></a>Verwenden von Transact-SQL

### <a name="to-create-a-database-master-key"></a>So erstellen Sie einen Datenbank-Hauptschlüssel

1. Wählen Sie ein Kennwort aus, mit dem die in der Datenbank gespeicherte Kopie des Datenbank-Hauptschlüssels verschlüsselt wird.
2. Stellen Sie im **Objekt-Explorer** eine Verbindung mit einer [!INCLUDE[ssDE](../../../includes/ssde-md.md)]-Instanz her.
3. Erweitern Sie **Systemdatenbanken**, klicken Sie mit der rechten Maustaste auf `master`, und klicken Sie anschließend auf **Neue Abfrage**.
4. Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**.

   ```sql
     -- Creates the master key.
     -- The key is encrypted using the password "23987hxJ#KL95234nl0zBe".  
     CREATE MASTER KEY ENCRYPTION BY PASSWORD = '23987hxJ#KL95234nl0zBe';  

   ```

Weitere Informationen finden Sie unter [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-master-key-transact-sql.md).
