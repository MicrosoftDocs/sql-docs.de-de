---
description: setSendStringParametersAsUnicode-Methode (SQLServerDataSource)
title: Methode „setSendStringParametersAsUnicode“ (SQLServerDataSource) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLServerDataSource.setSendStringParametersAsUnicode
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 49198d63-76cb-4843-8d04-e49b1fbb6916
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b8957b431a3aabce4ed4564867c052ef63502350
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88458352"
---
# <a name="setsendstringparametersasunicode-method-sqlserverdatasource"></a>setSendStringParametersAsUnicode-Methode (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Legt einen Wert vom Typ **boolean** fest, mit dem angegeben wird, ob das Senden von Zeichenfolgenparametern an den Server im Unicode-Format aktiviert ist.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
public void setSendStringParametersAsUnicode(boolean sendStringParametersAsUnicode)  
```  
  
#### <a name="parameters"></a>Parameter  
 *sendStringParametersAsUnicode*  
  
 Der Wert ist **true**, wenn Zeichenfolgenparameter im Unicode-Format an den Server gesendet werden. Andernfalls lautet der Wert **false**.  
  
## <a name="remarks"></a>Bemerkungen  
 Ist die sendStringParametersAsUnicode-Eigenschaft auf **true** (Standardeinstellung) festgelegt, werden Zeichenfolgenparameter im Unicode-Format an den Server gesendet. Ist die sendStringParametersAsUnicode-Eigenschaft auf **false** festgelegt, werden Zeichenfolgenparameter nicht im Unicode-Format, sondern im ASCII-/MBCS-Format an den Server gesendet. Ist die sendStringParametersAsUnicode-Eigenschaft nicht festgelegt, wird von [getSendStringParametersAsUnicode](../../../connect/jdbc/reference/getsendstringparametersasunicode-method-sqlserverdatasource.md) der Standardwert **true** zurückgegeben.  
  
 Weitere Informationen zur sendStringParametersAsUnicode-Verbindungseigenschaft finden Sie unter [Festlegen von Verbindungseigenschaften](../../../connect/jdbc/setting-the-connection-properties.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQLServerDataSource-Elemente](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [SQLServerDataSource-Klasse](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
