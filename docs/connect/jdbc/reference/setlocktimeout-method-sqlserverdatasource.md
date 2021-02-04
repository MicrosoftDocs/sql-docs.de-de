---
description: setLockTimeout-Methode (SQLServerDataSource)
title: setLockTimeout-Methode (SQLServerDataSource) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSource.setLockTimeout
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 10dca5aa-1851-4326-9ae9-7a8430d12d11
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 98f9ee813d8f1b4999d44b5ec56a65efe35888e3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178896"
---
# <a name="setlocktimeout-method-sqlserverdatasource"></a>setLockTimeout-Methode (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Legt einen Wert vom Typ **int** fest, der angibt, wie lange von der Datenbank bis zum Melden eines Sperrtimeouts gewartet wird (in Millisekunden).  
  
## <a name="syntax"></a>Syntax  
  
```  
  
public void setLockTimeout(int lockTimeout)  
```  
  
#### <a name="parameters"></a>Parameter  
 *lockTimeout*  
  
 Ein Wert vom Typ **int**, der die Wartedauer in Millisekunden enthält.  
  
## <a name="remarks"></a>Bemerkungen  
 Das Sperrtimeout ist die Anzahl von Millisekunden bis zur Meldung eines Sperrtimeouts durch die Datenbank. Der Standardwert -1 bedeutet, dass unbegrenzt gewartet wird. Wird dieser Wert angegeben, stellt er den Standardwert für alle Anweisungen der Verbindung dar.  
  
> [!NOTE]  
>  Bei dem Wert "0" wird nicht gewartet. Ist die lockTimeout-Eigenschaft nicht festgelegt, wird von der [getLockTimeout](../../../connect/jdbc/reference/getlocktimeout-method-sqlserverdatasource.md)-Methode der Standardwert „-1“ zurückgegeben.  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQLServerDataSource-Elemente](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [SQLServerDataSource-Klasse](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
