---
description: PWDENCRYPT (Transact-SQL)
title: PWDENCRYPT (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- PWDENCRYPT
- PWDENCRYPT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- PWDENCRYPT function [Transact-SQL]
ms.assetid: 333e9a43-1099-4b9b-b941-4b0b016f47f3
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 2bc5c778ea6dc591099538b974bf71f4d85c4f0a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182352"
---
# <a name="pwdencrypt-transact-sql"></a>PWDENCRYPT (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Kennworthash des Eingabewerts zurück, der die aktuelle Version des Kennworthashalgorithmus verwendet.  
  
 PWDENCRYPT ist eine ältere Funktion und wird in zukünftigen Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]möglicherweise nicht mehr unterstützt werden. Verwenden Sie stattdessen [HASHBYTES](../../t-sql/functions/hashbytes-transact-sql.md) . HASHBYTES stellt mehr Hashalgorithmen bereit.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
PWDENCRYPT ( 'password' )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *password*  
 Das zu verschlüsselnde Kennwort. *password* ist **sysname**  
  
## <a name="return-types"></a>Rückgabetypen  
 **varbinary(128)**  
  
## <a name="permissions"></a>Berechtigungen  
 PWDENCRYPT steht für die Öffentlichkeit zur Verfügung.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Sicherheitsfunktionen &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)   
 [PWDCOMPARE &#40;Transact-SQL&#41;](../../t-sql/functions/pwdcompare-transact-sql.md)  
  
  
