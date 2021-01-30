---
description: SQL_C_TCHAR
title: SQL_C_TCHAR | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- sql_c_tchar [ODBC]
- pseudo-type identifiers [ODBC], SQL_C_TCHAR
- data types [ODBC], pseudo-type identifiers
ms.assetid: 9e27c8bd-ee15-4ce9-b70a-34cf1bf16f4c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a399570a15f04d8ace61664a445c5283d629bf6a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193049"
---
# <a name="sql_c_tchar"></a>SQL_C_TCHAR
Der SQL_C_TCHAR Typbezeichner identifiziert nicht tatsächlich einen Datentyp. Es handelt sich um ein Makro, das in der Header Datei für die Unicode-Konvertierung vorhanden ist. Sie wird durch SQL_C_CHAR oder SQL_C_WCHAR ersetzt, abhängig von der Einstellung der Unicode- **#define**. Es ist nützlich für eine Anwendung, die Zeichendaten überträgt, die als ANSI-und Unicode-Anwendung kompiliert werden.
