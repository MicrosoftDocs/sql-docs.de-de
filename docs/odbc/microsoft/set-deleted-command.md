---
description: SET DELETED-Befehl
title: Löschbefehl festlegen | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SET DELETED command [ODBC]
ms.assetid: 6b5e0086-156d-471d-8e7f-6c5fa9686cd5
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6447e2d0c77e1a161df58ca070fae8c624c196a8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208635"
---
# <a name="set-deleted-command"></a>SET DELETED-Befehl
Gibt an, ob für den Löschvorgang markierte Datensätze verarbeitet werden und ob Sie zur Verwendung in anderen Befehlen verfügbar sind.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
SET DELETED ON | OFF  
```  
  
## <a name="arguments"></a>Argumente  
 EIN  
 (Standardwert für den Treiber; der Standardwert für Visual FoxPro ist off.) Gibt an, dass Befehle, die auf Datensätzen (einschließlich Datensätzen in verknüpften Tabellen) mithilfe eines Bereichs angewendet werden, zum Löschen markierte Datensätze ignorieren.  
  
 OFF  
 Gibt an, dass auf Datensätze, die zum Löschen markiert sind, über Befehle zugegriffen werden kann, die mit einem Bereich auf Datensätze (einschließlich Datensätze in verknüpften Tabellen  
  
## <a name="remarks"></a>Bemerkungen  
 Abfragen, die gelöschte () zum Testen des Status von Datensätzen verwenden, können mithilfe der Visual FoxPro-Funktion "Rushmore" optimiert werden, wenn die Tabelle auf "Deleted" () indiziert ist.  
  
> [!IMPORTANT]  
>  Set Deleted wird ignoriert, wenn der Standardbereich für den Befehl der aktuelle Datensatz ist oder wenn Sie einen Bereich eines einzelnen Datensatzes einschließen. Der Index ignoriert die gelöschten Sätze immer und indiziert alle Datensätze in der Tabelle.  
  
## <a name="see-also"></a>Weitere Informationen  
 [DELETE (SQL-Befehl)](../../odbc/microsoft/delete-sql-command.md)
