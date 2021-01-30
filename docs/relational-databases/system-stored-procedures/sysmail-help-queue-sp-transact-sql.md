---
description: sysmail_help_queue_sp (Transact-SQL)
title: sysmail_help_queue_sp (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_help_queue_sp
- sysmail_help_queue_sp_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_help_queue_sp
ms.assetid: 94840482-112c-4654-b480-9b456c4c2bca
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6aece593fd32d5ee563c7b1fe37dc039aa3eec5c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181930"
---
# <a name="sysmail_help_queue_sp-transact-sql"></a>sysmail_help_queue_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Es gibt zwei Warteschlangen in der Datenbank-E-Mail: die E-Mail-Warteschlange und die Statuswarteschlange. In der E-Mail-Warteschlange werden E-Mail-Elemente gespeichert, die darauf warten, gesendet zu werden. In der Statuswarteschlange wird der Status von Elementen gespeichert, die bereits gesendet wurden. Mit dieser gespeicherten Prozedur können Sie den Status der E-Mail- oder der Statuswarteschlange anzeigen. Wenn der Parameter **\@ queue_type** nicht angegeben ist, gibt die gespeicherte Prozedur eine Zeile für jede Warteschlange zurück.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sysmail_help_queue_sp  [ @queue_type = ] 'queue_type'  
```  
  
## <a name="arguments"></a>Argumente  
`[ @queue_type = ] 'queue_type'` Optionales Argument löscht e-Mails des als *queue_type* angegebenen Typs. *queue_type* ist vom Datentyp **nvarchar(6)** und besitzt keinen Standardwert. Gültige Einträge sind **mail** und **status**.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="result-set"></a>Resultset  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**queue_type**|**nvarchar(6)**|Der Typ der Warteschlange. Mögliche Werte sind **mail** und **status**.|  
|**length**|**int**|Die Anzahl der E-Mail-Elemente in der angegebenen Warteschlange.|  
|**state**|**nvarchar (64)**|Der Status des Überwachungsservers. Mögliche Werte sind **INACTIVE** (Warteschlange ist inaktiv), **NOTIFIED** (Warteschlange wurde benachrichtigt, dass Empfang auftritt) und **RECEIVES_OCCURRING** (Warteschlange empfängt).|  
|**last_empty_rowset_time**|**DateTime**|Das Datum und die Uhrzeit, an dem bzw. zu der die Warteschlange zuletzt leer war. Die Angabe erfolgt im 24-Stunden-Format und in der GMT-Zeitzone.|  
|**last_activated_time**|**DateTime**|Das Datum und die Uhrzeit, an dem bzw. zu der die Warteschlange zuletzt aktiviert war. Die Angabe erfolgt im 24-Stunden-Format und in der GMT-Zeitzone.|  
  
## <a name="remarks"></a>Bemerkungen  
 Wenn Sie Probleme mit der Datenbank-E-Mail behandeln, verwenden Sie **sysmail_help_queue_sp** , um anzuzeigen, wie viele Elemente sich in der Warteschlange befinden, wie der Status der Warteschlange lautet und wann die Warteschlange zuletzt aktiviert wurde.  
  
## <a name="permissions"></a>Berechtigungen  
 Standardmäßig können nur Mitglieder der festen Serverrolle **sysadmin** auf diese Prozedur zugreifen.  
  
## <a name="examples"></a>Beispiele  
 Das folgende Beispiel gibt sowohl die E-Mail- als auch die Statuswarteschlange zurück.  
  
```  
EXECUTE msdb.dbo.sysmail_help_queue_sp ;  
GO  
```  
  
 Dies ist ein Beispielresultset, das auf Zeilenlänge umformatiert wurde.  
  
```  
queue_type length      state              last_empty_rowset_time  last_activated_time  
---------- -------- ------------------ ----------------------- -----------------------  
mail       0        RECEIVES_OCCURRING 2005-10-07 21:14:47.010 2005-10-10 20:52:51.517  
status     0        INACTIVE           2005-10-07 21:04:47.003 2005-10-10 21:04:47.003  
  
(2 row(s) affected)  
  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Datenbank-E-Mail](../../relational-databases/database-mail/database-mail.md)  
  
  
