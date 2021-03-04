---
title: Option „Abbrechen“ (Verwaltungstool)
titleSuffix: SQL Server Distributed Replay
description: Dieser Artikel beschreibt die Befehlszeilenoption „cancel“ und die entsprechende Syntax des Distributed Replay-Verwaltungstools von SQL Server.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 21d1d74439b41a6e36287fef957c6c2df24aa1c1
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101836924"
---
# <a name="cancel-option-distributed-replay-administration-tool"></a>Option Abbrechen (Verwaltungstool Distributed Replay)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Das [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay-Verwaltungstool (**DReplay.exe**) ist ein Befehlszeilentool, über das Sie mit dem Distributed Replay-Controller kommunizieren können. In diesem Thema werden die **cancel** -Befehlszeilenoption und die entsprechende Syntax beschrieben.  
  
 Die **cancel** -Option bricht den aktuellen Vorgang ab, der auf dem Controller ausgeführt wird.  
  
 ![Artikellinksymbol](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") Weitere Informationen zu den Syntaxkonventionen für das Verwaltungstool finden Sie unter [Transact-SQL-Syntaxkonventionen &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  
  
## <a name="syntax"></a>Syntax  
  
```  
  
dreplay cancel [-m controller] [-q]   
```  
  
#### <a name="parameters"></a>Parameter
 **-m** *Controller*  
 Der Computername des Controllers. Sie können mit "`localhost`" oder "`.`" auf den lokalen Computer verweisen.  
  
 Wenn der **-m** -Parameter nicht angegeben ist, wird der lokale Computer verwendet.  
  
 **-q**  
 Stiller Modus. Fordert nicht zur Bestätigung auf.  
  
 Der **-q** -Parameter ist optional.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird eine Abbruchanforderung im stillen Modus gesendet. Der Wert `localhost` gibt an, dass der Controllerdienst auf demselben Computer wie das Verwaltungstool ausgeführt wird.  
  
```  
dreplay cancel -m localhost -q  
```  
  
## <a name="permissions"></a>Berechtigungen  
 Sie müssen das Verwaltungstool als interaktiver Benutzer mit einem lokalen Benutzerkonto oder Domänenbenutzerkonto ausführen. Um ein lokales Benutzerkonto zu verwenden, müssen das Verwaltungstool und der Controller auf demselben Computer ausgeführt werden.  
  
 Weitere Informationen finden Sie unter [Distributed Replay Security](../../tools/distributed-replay/distributed-replay-security.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQL Server Distributed Replay](../../tools/distributed-replay/sql-server-distributed-replay.md)  
  
  
