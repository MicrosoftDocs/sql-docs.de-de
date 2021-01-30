---
description: sp_monitor (Transact-SQL)
title: sp_monitor (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_monitor_TSQL
- sp_monitor
dev_langs:
- TSQL
helpviewer_keywords:
- sp_monitor
ms.assetid: cb628496-2f9b-40e4-b018-d0831c4cb018
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f046c3e6fe81c7dac489fcdd88237ecb2d3747d0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195464"
---
# <a name="sp_monitor-transact-sql"></a>sp_monitor (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Zeigt Statistiken zu an [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_monitor  
```  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="result-sets"></a>Resultsets  
  
|Spaltenname|BESCHREIBUNG|  
|-----------------|-----------------|  
|**last_run**|Zeit **sp_monitor** zuletzt ausgeführt.|  
|**current_run**|Der Zeitraum, **sp_monitor** ausgeführt wird.|  
|**Sekunden**|Anzahl der seit dem Ausführen **sp_monitor** verstrichenen Sekunden.|  
|**cpu_busy**|Die Anzahl von Sekunden, während derer von der CPU des Servercomputers für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Vorgänge ausgeführt wurden.|  
|**io_busy**|Die Anzahl von Sekunden, während derer von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Eingabe- und Ausgabevorgänge ausgeführt wurden.|  
|**Gesch**|Die Anzahl von Sekunden, während derer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sich im Leerlauf befand.|  
|**packets_received**|Die Anzahl von Eingabepaketen, die von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gelesen wurden.|  
|**packets_sent**|Die Anzahl der von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] geschriebenen Ausgabepakete.|  
|**packet_errors**|Die Anzahl von Fehlern, die von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] beim Lesen und Schreiben von Paketen festgestellt wurden.|  
|**total_read**|Die Anzahl von Lesevorgängen durch [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**total_write**|Die Anzahl von Schreibvorgängen durch [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**total_errors**|Die Anzahl von Fehlern, die von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] beim Lesen und Schreiben festgestellt wurden.|  
|**connections**|Die Anzahl von Anmeldungen oder versuchten Anmeldungen an [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
  
## <a name="remarks"></a>Bemerkungen  
 In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] werden mithilfe einer Reihe von Funktionen quantitative Angaben über die ausgeführten Vorgänge gespeichert. Beim Ausführen **sp_monitor** werden die aktuellen Werte angezeigt, die von diesen Funktionen zurückgegeben werden, und es wird angezeigt, wie stark Sie sich seit der letzten Ausführung der Prozedur geändert haben.  
  
 Für jede Spalte wird die Statistik im Format *Number*(*Number*)-*Number*% oder *Number*(*Number*) ausgegeben. Die erste *Zahl* bezieht sich auf die Anzahl von Sekunden (für **CPU_BUSY**, **IO_BUSY** und **Leerlauf**) oder die Gesamtzahl (für die anderen Variablen) seit dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Neustart von. Die *Zahl* in Klammern bezieht sich auf die Anzahl der Sekunden oder die Gesamtzahl seit dem letzten Ausführen **sp_monitor** . Der Prozentsatz ist der Prozentsatz der Zeit seit dem letzten Ausführen **sp_monitor** . Wenn der Bericht z. b. **CPU_BUSY** als 4250 (215)-68% anzeigt, ist die CPU ausgelastet, 4250 Sekunden seit dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] letzten Start, 215 Sekunden seit der letzten Betriebs **sp_monitor** und 68 Prozent der Gesamtzeit seit dem letzten Ausführen **sp_monitor** .  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die Mitgliedschaft in der festen Serverrolle **sysadmin** .  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel werden Informationen zur Auslastung von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ausgegeben.  
  
```console
USE master  
EXEC sp_monitor  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  

```console
last_run       current_run                   seconds
-----------    --------------------------    ---------
Mar 29 1998    11:55AM Apr 4 1998 2:22 PM    561

cpu_busy           io_busy     idle
---------------    ---------   --------------
190(0)-0%          187(0)-0%   148(556)-99%

packets_received       packets_sent    packet_errors
----------------       ------------    -------------
16(1)                  20(2)           0(0)

total_read     total_write   total_errors    connections
-----------    -----------   -------------   -----------
141(0)         54920(127)    0(0)            4(0)
```
  
## <a name="see-also"></a>Weitere Informationen  
 [sp_who &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md)   
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
