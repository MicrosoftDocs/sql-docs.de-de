---
description: SET TEXTSIZE (Transact-SQL)
title: SET TEXTSIZE (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 04/12/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- TEXTSIZE_TSQL
- TEXTSIZE
- SET_TEXTSIZE_TSQL
- SET TEXTSIZE
dev_langs:
- TSQL
helpviewer_keywords:
- SET TEXTSIZE statement
- SELECT statement [SQL Server], text size returned
- size [SQL Server], text and image data
- TEXTSIZE option
- text size returned [SQL Server]
ms.assetid: 787154a6-39a6-4dd6-a6d0-67b4364f95d5
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2b89190f7e6e60f2ed81f4b9c4b6ada685ad4f9f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189461"
---
# <a name="set-textsize-transact-sql"></a>SET TEXTSIZE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Hiermit wird die Größe der **varchar(max)** -, **nvarchar(max)** -, **varbinary(max)** -, **text**-, **ntext**- und **image**-Daten angegeben, die von einer SELECT-Anweisung an den Client zurückgegeben werden.  
  
> [!IMPORTANT]
>  Die Datentypen **ntext**, **text** und **image** werden in einer zukünftigen Version von [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] entfernt. Vermeiden Sie die Verwendung dieser Datentypen bei neuen Entwicklungen, und planen Sie die Änderung von Anwendungen, in denen sie aktuell verwendet werden. Verwenden Sie stattdessen **nvarchar(max)**, **varchar(max)** und **varbinary(max)** .  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
SET TEXTSIZE { number }   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *Zahl*  
 Entspricht der Länge der **varchar(max)**-, **nvarchar(max)**-, **varbinary(max)**-, **text**-, **ntext**- oder **image**-Daten in Byte. *number* entspricht einer ganzen Zahl mit einem maximalen Wert von 2.147.483.647 (2 GB).  Ein Wert von –1 gibt eine unbegrenzte Größe an. Ein Wert von 0 setzt die Größe auf den Standardwert von 4 KB zurück.  
  
 Der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client (10.0 und höher) und der ODBC-Treiber für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] geben beim Herstellen einer Verbindung automatisch `-1` (unbegrenzt) an.  
  
 **Ältere Treiber als [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2008:** Der ODBC-Treiber von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client und der OLE DB-Anbieter (Version 9) von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] legen beim Herstellen einer Verbindung für TEXTSIZE automatisch den Wert 2.147.483.647 fest.  
  
## <a name="remarks"></a>Hinweise  
 Das Festlegen von SET TEXTSIZE wirkt sich auf die @@TEXTSIZE-Funktion aus.  
  
 Die Einstellung von TEXTSIZE wird zur Ausführungszeit und nicht zur Analysezeit festgelegt.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die Mitgliedschaft in der **public** -Rolle.  
  
## <a name="see-also"></a>Weitere Informationen  
 [@@TEXTSIZE &#40;Transact-SQL&#41;](../../t-sql/functions/textsize-transact-sql.md)   
 [Datentypen &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [SET-Anweisungen &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)  
  
  
