---
title: Option „status“ im Verwaltungstool
titleSuffix: SQL Server Distributed Replay
description: Dieser Artikel beschreibt die Befehlszeilenoption „status“ und die entsprechende Syntax des Distributed Replay-Verwaltungstools von SQL Server, das den aktuellen Status anzeigt.
ms.prod: sql
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: ea89386e-1598-4412-8b37-680d14b2a5b6
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 2dc1f88454d40d6c6b4efdcaa51df24741d9df88
ms.sourcegitcommit: a9f16d7819ed0e2b7ad8f4a7d4d2397437b2bbb2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2020
ms.locfileid: "88714168"
---
# <a name="status-option-distributed-replay-administration-tool"></a>Option Status (Verwaltungstool Distributed Replay)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Das [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Verwaltungstool Distributed Replay, **DReplay.exe**, ist ein Befehlszeilentool, das Sie für die Kommunikation mit dem Distributed Replay-Controller verwenden können. In diesem Thema werden die Befehlszeilenoption **status** und die entsprechende Syntax beschrieben.  
  
 Mit der **status** -Option wird der Controller abgefragt und der aktuelle Status angezeigt.  
  
 ![Artikellinksymbol](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") Weitere Informationen zu den Syntaxkonventionen für das Verwaltungstool finden Sie unter [Transact-SQL-Syntaxkonventionen &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  
  
## <a name="syntax"></a>Syntax  
  
```  
  
dreplay status [-m controller] [-f status_interval]  
```  
  
#### <a name="parameters"></a>Parameter  
 **-m** _Controller_  
 Gibt den Computernamen des Controllers an. Sie können mit "`localhost`" oder "`.`" auf den lokalen Computer verweisen.  
  
 Wenn der **-m** -Parameter nicht angegeben ist, wird der lokale Computer verwendet.  
  
 **-f** _status_interval_  
 Gibt die Häufigkeit (in Sekunden) für die Anzeige des Status an.  
  
 Wenn der **-f** -Parameter nicht angegeben wird, ist das Standardintervall 30 Sekunden.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird der aktuelle Status alle 60 Sekunden angezeigt. Der Wert `localhost` gibt an, dass der Controllerdienst auf demselben Computer wie das Verwaltungstool ausgeführt wird.  
  
```  
dreplay status -m localhost -f 60  
```  
  
## <a name="permissions"></a>Berechtigungen  
 Sie müssen das Verwaltungstool als interaktiver Benutzer mit einem lokalen Benutzerkonto oder Domänenbenutzerkonto ausführen. Um ein lokales Benutzerkonto zu verwenden, müssen das Verwaltungstool und der Controller auf demselben Computer ausgeführt werden.  
  
 Weitere Informationen finden Sie unter [Distributed Replay Security](../../tools/distributed-replay/distributed-replay-security.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQL Server Distributed Replay](../../tools/distributed-replay/sql-server-distributed-replay.md)   
 [Transact-SQL-Debugger](../../ssms/scripting/transact-sql-debugger.md?view=sql-server-ver15)  
  
