---
description: DBCC DROPCLEANBUFFERS (Transact-SQL)
title: DBCC DROPCLEANBUFFERS (Transact-SQL)
ms.custom: ''
ms.date: 07/16/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROPCLEANBUFFERS
- DBCC_DROPCLEANBUFFERS_TSQL
- DROPCLEANBUFFERS_TSQL
- DBCC DROPCLEANBUFFERS
dev_langs:
- TSQL
helpviewer_keywords:
- clean buffers
- cold buffer cache
- buffer pools [SQL Server]
- dropping buffers
- removing buffers
- DBCC DROPCLEANBUFFERS statement
ms.assetid: a4121927-f2ce-4926-aa2c-9b1519dac048
author: pmasl
ms.author: umajay
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c463accb2ddc329855abafcd6a4f7aea19ce75b4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340965"
---
# <a name="dbcc-dropcleanbuffers-transact-sql"></a>DBCC DROPCLEANBUFFERS (Transact-SQL)

[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

Entfernt alle leeren Puffer aus dem Pufferpool und alle Columnstore-Objekte aus dem Columnstore-Objektpool.
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntax

Syntax für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und [!INCLUDE[ssSOD](../../includes/sssodfull-md.md)]:

```syntaxsql
DBCC DROPCLEANBUFFERS [ WITH NO_INFOMSGS ]  
```  
Syntax für [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]:

```syntaxsql  
DBCC DROPCLEANBUFFERS ( COMPUTE | ALL ) [ WITH NO_INFOMSGS ]  
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 WITH NO_INFOMSGS  
 Alle Informationsmeldungen werden unterdrückt. Informationsmeldungen werden bei [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] immer unterdrückt.  
  
 COMPUTE  
 Löschen Sie den Datencache im Arbeitsspeicher jedes Computeknotens.  
  
 ALL  
 Löschen Sie den Datencache im Arbeitsspeicher jedes Computerknotens und des Steuerknotens. Wenn Sie keinen Wert angeben, handelt es sich dabei um den Standardwert.  
  
## <a name="remarks"></a>Bemerkungen  
Verwenden Sie DBCC DROPCLEANBUFFERS, um Abfragen mit einem frischen Puffercache zu testen, ohne den Server herunterzufahren und neu zu starten.
Sie müssen zunächst mithilfe von CHECKPOINT einen neuen Puffercache erzeugen, um leere Puffer aus dem Pufferpool und Columnstore-Objekte aus dem Columnstore-Objektpool zu entfernen. Dadurch wird erzwungen, dass alle modifizierten Seiten der aktuellen Datenbank auf den Datenträger geschrieben und die Puffer geleert werden. Anschließend können Sie durch Ausführen des DBCC DROPCLEANBUFFERS-Befehls alle Puffer aus dem Pufferpool entfernen.
  
## <a name="result-sets"></a>Resultsets  
DBCC DROPCLEANBUFFERS gibt bei [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Folgendes zurück:
  
```
DBCC execution completed. If DBCC printed error messages, contact your system administrator.  
```  
  
## <a name="permissions"></a>Berechtigungen  
Erfordert die Mitgliedschaft in der festen Serverrolle `sysadmin` für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].  
Erfordert die Mitgliedschaft in der festen Serverrolle `DB_OWNER` für [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)].  
  
## <a name="see-also"></a>Weitere Informationen  
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[CHECKPOINT &#40;Transact-SQL&#41;](../../t-sql/language-elements/checkpoint-transact-sql.md)  
  
  
