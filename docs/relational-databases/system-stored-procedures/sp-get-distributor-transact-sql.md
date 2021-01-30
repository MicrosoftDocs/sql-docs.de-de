---
description: sp_get_distributor (Transact-SQL)
title: sp_get_distributor (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_get_distributor
- sp_get_distributor_TSQL
helpviewer_keywords:
- sp_get_distributor
ms.assetid: f0134448-bc17-4f2f-bd81-619351ce56ac
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 82c9e743cf3c1ddce87642fb3320b74eb5f7b6e1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99105591"
---
# <a name="sp_get_distributor-transact-sql"></a>sp_get_distributor (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Bestimmt, ob ein Verteiler auf einem Server installiert ist. Diese gespeicherte Prozedur wird auf dem Computer, auf dem nach dem Verteiler gesucht wird, für jede Datenbank ausgeführt.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_get_distributor   
```  
  
## <a name="result-sets"></a>Resultsets  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**lierter**|**int**|**0** = Nein; **1** = ja|  
|**Verteilungs Server**|**sysname**|Name des Verteilerservers|  
|**installierte Verteilungs Datenbank**|**int**|**0** = Nein; **1** = ja|  
|**is distribution publisher**|**int**|**0** = Nein; **1** = ja|  
|**hat Remote Verteilungs Verleger**|**int**|**0** = Nein; **1** = ja|  
  
## <a name="remarks"></a>Bemerkungen  
 **sp_get_distributor** wird in erster Linie von der [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] in Momentaufnahme-, Transaktions-und Mergereplikation verwendet.  
  
## <a name="permissions"></a>Berechtigungen  
 Jeder Benutzer kann **sp_get_distributor** ausführen. Ein Resultset ungleich NULL wird zurückgegeben, wenn diese gespeicherte Prozedur von Mitgliedern der festen Daten bankrollen **db_owner** oder **replmonitor** in der Verteilungs Datenbank oder von Mitgliedern der festen Daten Bank Rolle **db_owner** für mindestens eine veröffentlichte Datenbank ausgeführt wird. Ein Resultset ungleich NULL wird auch zurückgegeben, wenn diese gespeicherte Prozedur von Benutzern in der Veröffentlichungs Zugriffsliste (Publication Access List, PAL) für mindestens eine veröffentlichte Datenbank oder in der PAL der Verteilungs Datenbank für einen nicht-SQL Server Verleger ausgeführt wird. Außerdem kann **sp_get_distributor** ausgeführt werden.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Konfigurieren der Veröffentlichung und der Verteilung](../../relational-databases/replication/configure-publishing-and-distribution.md)   
 [Informationsskript für Verleger und Verteiler](../../relational-databases/replication/administration/distributor-and-publisher-information-script.md)   
 [Gespeicherte Automatisierungsprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
