---
description: sp_helparticledts (Transact-SQL)
title: sp_helparticledts (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sp_helparticledts
- sp_helparticledts_TSQL
helpviewer_keywords:
- sp_helparticledts
ms.assetid: cd1aed60-e056-4ff3-86ee-62b19433d890
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9e768bd98561338d392c5fc4c65c00c9ab5c7f59
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89541777"
---
# <a name="sp_helparticledts-transact-sql"></a>sp_helparticledts (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Ruft Informationen zu den benutzerdefinierten Tasknamen ab, die beim Erstellen eines Transformationsabonnements mithilfe von [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual Basic verwendet werden sollen. Diese gespeicherte Prozedur wird auf dem Verleger für die Veröffentlichungs Datenbank ausgeführt.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_helparticledts [ @publication = ] 'publication', [ @article = ] 'article'  
```  
  
## <a name="arguments"></a>Argumente  
`[ @publication = ] 'publication'` Der Name der Veröffentlichung. *Publication* ist vom **Datentyp vom Datentyp sysname**und hat keinen Standardwert.  
  
`[ @article = ] 'article'` Der Name eines Artikels in der Veröffentlichung. der *Artikel* ist vom **Datentyp vom Datentyp sysname**und hat keinen Standardwert.  
  
## <a name="result-sets"></a>Resultsets  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**pre_script_ignore_error_task_name**|**sysname**|Taskname für den Programmiertask, der auftritt, bevor die Momentaufnahmedaten kopiert werden. Die Programmausführung soll beim Auftreten eines Skriptfehlers fortgesetzt werden.|  
|**pre_script_task_name**|**sysname**|Taskname für den Programmiertask, der auftritt, bevor die Momentaufnahmedaten kopiert werden. Die Programmausführung wird beim Auftreten eines Fehlers beendet.|  
|**transformation_task_name**|**sysname**|Taskname für den Programmiertask, wenn der Task Datengesteuerte Abfrage verwendet wird.|  
|**post_script_ignore_error_task_name**|**sysname**|Taskname für den Programmiertask, der auftritt, nachdem die Momentaufnahmedaten kopiert wurden. Die Programmausführung soll beim Auftreten eines Skriptfehlers fortgesetzt werden.|  
|**post_script_task_name**|**sysname**|Taskname für den Programmiertask, der auftritt, nachdem die Momentaufnahmedaten kopiert wurden. Die Programmausführung wird beim Auftreten eines Fehlers beendet.|  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="remarks"></a>Hinweise  
 **sp_helparticledts** wird bei der Momentaufnahme-und Transaktions Replikation verwendet.  
  
 Beim Benennen von Tasks in einem Replikations-DTS-Programm (Data Transformation Services) müssen für die Replikations-Agents bestimmte Namenskonventionen eingehalten werden. Für benutzerdefinierte Tasks, wie z. B. den Task SQL ausführen, stellt der Name eine verkettete Zeichenfolge dar, die aus dem Artikelnamen, einem Präfix und einem optionalen Teil besteht. Wenn Sie den Code schreiben und sich nicht sicher sind, wie die Tasknamen lauten sollen, dann weist das Resultset die zu verwendenden Tasknamen zu.  
  
## <a name="permissions"></a>Berechtigungen  
 Nur Mitglieder der festen Server Rolle **sysadmin** und der festen Daten Bank Rolle **db_owner** können **sp_helparticledts**ausführen.  
  
  
