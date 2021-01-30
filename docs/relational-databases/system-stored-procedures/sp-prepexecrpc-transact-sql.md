---
description: sp_prepexecrpc (Transact-SQL)
title: sp_prepexecrpc (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/02/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cursor_prepexecrpc
- sp_cursor_prepexecrpc_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_prepexecrpc
ms.assetid: 35d686f2-ef31-4eaa-baa9-9cef5d6c87c2
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c3190e97f177811bc5c90e770544d9c4da1f104b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183094"
---
# <a name="sp_prepexecrpc-transact-sql"></a>sp_prepexecrpc (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Bereitet einen Aufruf einer parametrisierten, mithilfe eines RPC-Bezeichners angegebenen gespeicherten Prozedur vor und führt sie aus. sp_prepexecrpc wird von ID = 14 in einem Tabular Data Stream-Paket (TDS) aufgerufen.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_prepexecrpc handle OUTPUT, RPCCall  
    [ , bound_param ] [ ,...n]]  
```  
  
## <a name="arguments"></a>Argumente  
 *bewältigen*  
 Der von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]generierte, vorbereitete Handlebezeichner. *handle* ist ein erforderlicher Parameter mit einem **int** -Rückgabewert.  
  
 *RPCCall*  
 Definiert den Aufruf für die gespeicherte Prozedur mit kanonischer ODBC-Syntax. *RPCCall* ist ein erforderlicher Parameter, der einen Eingabewert für eine **ntext** -Zeichenfolge erfordert.  
  
 *bound_param*  
 Gibt die optionale Verwendung zusätzlicher Parameter an. *bound_param* erfordert einen Eingabewert eines beliebigen Datentyps, um die zusätzlichen verwendeten Parameter festzulegen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
