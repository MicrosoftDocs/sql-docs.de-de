---
description: HOST_ID (Transact-SQL)
title: HOST_ID (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- HOST_ID
- HOST_ID_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- IDs [SQL Server], workstations
- HOST_ID function
- workstation IDs [SQL Server]
- identification numbers [SQL Server], workstations
ms.assetid: 36ba56d4-20d7-4cd1-aa2a-e40a6c0a4e39
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 099026a22a03c563b8ea176d5fed14d7d936ffc6
ms.sourcegitcommit: cc23d8646041336d119b74bf239a6ac305ff3d31
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2020
ms.locfileid: "91116344"
---
# <a name="host_id-transact-sql"></a>HOST_ID (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt die Arbeitsstation-ID zurück. Die Arbeitsstation-ID ist die Prozess-ID (PID) der Anwendung auf dem Clientcomputer, die eine Verbindung mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] herstellt.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
HOST_ID ()  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Rückgabetypen
 **char(10)**  
  
## <a name="remarks"></a>Bemerkungen  
 Wenn der Parameter für eine Systemfunktion optional ist, wird von der aktuellen Datenbank, dem aktuellen Hostcomputer, dem aktuellen Serverbenutzer oder dem aktuellen Datenbankbenutzer ausgegangen. Auf integrierte Funktionen müssen immer runde Klammern folgen.  
  
 Systemfunktionen können in der SELECT-Liste, in einer WHERE-Klausel und überall dort verwendet werden, wo ein Ausdruck zulässig ist.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird eine Tabelle erstellt, bei der `HOST_ID()` in einer `DEFAULT`-Definition verwendet wird, um die Terminal-ID von Computern aufzuzeichnen, die Zeilen in eine Tabelle zur Auftragsaufzeichnung einfügen.  
  
```sql  
CREATE TABLE Orders  
   (OrderID     INT       PRIMARY KEY,  
    CustomerID  NCHAR(5)  REFERENCES Customers(CustomerID),  
    TerminalID  CHAR(8)   NOT NULL DEFAULT HOST_ID(),  
    OrderDate   DATETIME  NOT NULL,  
    ShipDate    DATETIME  NULL,  
    ShipperID   INT       NULL REFERENCES Shippers(ShipperID));  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Ausdrücke &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Systemfunktionen &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-category-transact-sql.md)  
  
  
