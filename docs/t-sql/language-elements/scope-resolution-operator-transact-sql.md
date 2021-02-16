---
description: ':: (Bereichsauflösung) (Transact-SQL)'
title: ':: (Bereichsauflösung) (Transact-SQL) | Microsoft-Dokumentation'
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 764d8f91-957b-4037-997b-a9b6b533c504
author: cawrites
ms.author: chadam
ms.openlocfilehash: f683825863a66a14fa715ce507469be218d100f3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100023141"
---
# <a name="-scope-resolution-transact-sql"></a>:: (Bereichsauflösung) (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx_md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Der Bereichsauflösungsoperator **::** ermöglicht den Zugriff auf statische Elemente eines Verbunddatentyps. Ein Verbunddatentyp enthält mehrere einfache Datentypen und Methoden. Verbunddatentypen beinhalten integrierte CLR-Typen und benutzerdefinierte SQLCLR-Typen (User-Defined Types, UDTs).  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird der Zugriff auf das `GetRoot()`-Element des `hierarchyid`-Typs mithilfe des Bereichsauflösungsoperators dargestellt.  
  
```sql  
DECLARE @hid hierarchyid;  
SELECT @hid = hierarchyid::GetRoot();  
PRINT @hid.ToString();  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `/`  
  
## <a name="see-also"></a>Weitere Informationen  
 [Operatoren &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)  
 
