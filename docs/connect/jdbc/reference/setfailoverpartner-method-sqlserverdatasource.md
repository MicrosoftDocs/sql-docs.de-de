---
description: setFailoverPartner-Methode (SQLServerDataSource)
title: setFailoverPartner-Methode (SQLServerDataSource) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSource.setFailoverPartner
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 5310b7c2-9d10-474f-ad3a-218fe5da694b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c47da87d002d67f98cf80f4cd5db19510c744a03
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178979"
---
# <a name="setfailoverpartner-method-sqlserverdatasource"></a>setFailoverPartner-Methode (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Legt den Namen des Failoverservers fest, der in einer Datenbankspiegelungskonfiguration verwendet wird.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
public void setFailoverPartner(java.lang.String serverName)  
```  
  
#### <a name="parameters"></a>Parameter  
 *serverName*  
  
 Eine **Zeichenfolge** mit dem Namen des Failoverservers.  
  
## <a name="remarks"></a>Bemerkungen  
 Der von dieser Methode festgelegte Wert wird im Falle eines Fehlers beim erstmaligen Herstellen einer Verbindung mit dem Prinzipalserver verwendet. Sobald die Verbindung hergestellt wurde, wird dieser Wert ignoriert. In Verbindung mit dieser Methode muss auch die [setDatabaseName](../../../connect/jdbc/reference/setdatabasename-method-sqlserverdatasource.md)-Methode verwendet werden, da andernfalls eine Ausnahme ausgelöst wird.  
  
 Bei festgelegtem Failoverservernamen wird das Angeben der Portnummer des Failoverservers vom Treiber nicht unterstützt. Das Aufrufen der [setServerName](../../../connect/jdbc/reference/setservername-method-sqlserverdatasource.md)-Methode und der [setInstanceName](../../../connect/jdbc/reference/setinstancename-method-sqlserverdatasource.md)-Methode mit der [setFailoverPartner](../../../connect/jdbc/reference/setfailoverpartner-method-sqlserverdatasource.md)-Methode wird jedoch unterstützt.  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQLServerDataSource-Elemente](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [SQLServerDataSource-Klasse](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
