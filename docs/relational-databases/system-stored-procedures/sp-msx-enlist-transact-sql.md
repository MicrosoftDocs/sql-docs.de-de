---
description: sp_msx_enlist (Transact-SQL)
title: sp_msx_enlist (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_msx_enlist_TSQL
- sp_msx_enlist
dev_langs:
- TSQL
helpviewer_keywords:
- sp_msx_enlist
ms.assetid: ceb3b2bc-0cc4-48d8-9bdc-6a809556e35f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9435c4b8628bb119b0ac81126e50e37a67650532
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89541628"
---
# <a name="sp_msx_enlist-transact-sql"></a>sp_msx_enlist (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Fügt den aktuellen Server der Liste der verfügbaren Server auf dem Masterserver hinzu.  
  
> [!CAUTION]  
>  Die gespeicherte Prozedur **sp_msx_enlist** bearbeitet die Registrierung. Die Registrierung sollte nicht manuell bearbeitet werden, da durch ungeeignete oder fehlerhafte Änderungen schwerwiegende Konfigurationsprobleme auf dem System verursacht werden können. Nur erfahrene Benutzer sollten deshalb den Registrierungs-Editor zum Bearbeiten der Registrierung verwenden. Weitere Informationen finden Sie in der Dokumentation zu Microsoft Windows.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_msx_enlist [@msx_server_name =] 'msx_server'   
     [, [@location =] 'location']  
```  
  
## <a name="arguments"></a>Argumente  
`[ @msx_server_name = ] 'msx_server'` Der Name des multiserververwaltungsservers (Master). *msx_server* ist vom Datentyp **sysname**und hat keinen Standardwert.  
  
`[ @location = ] 'location'` Der Speicherort des hinzu zufügenden Zielservers. *location* ist vom Datentyp **nvarchar(100)** und hat den Standardwert NULL.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="result-sets"></a>Resultsets  
 Keine  
  
## <a name="permissions"></a>Berechtigungen  
 Die Berechtigungen zum Ausführen dieser Prozedur werden standardmäßig Mitgliedern der festen Server Rolle **sysadmin** zugewiesen.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird der aktuelle Server auf dem Masterserver `AdventureWorks1` eingetragen. Der Speicherort für den aktuellen Server lautet `Building 21, Room 309, Rack 5`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_msx_enlist N'AdventureWorks1',   
    N'Building 21, Room 309, Rack 5' ;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [sp_msx_defect &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-msx-defect-transact-sql.md)   
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [xp_cmdshell &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/xp-cmdshell-transact-sql.md)  
  
  
