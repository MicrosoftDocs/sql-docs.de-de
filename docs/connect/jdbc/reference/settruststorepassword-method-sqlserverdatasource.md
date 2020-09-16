---
description: setTrustStorePassword-Methode (SQLServerDataSource)
title: setTrustStorePassword-Methode (SQLServerDataSource) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- setTrustStorePassword Method (SQLServerDataSource)
apilocation:
- setTrustStorePassword Method (SQLServerDataSource)
apitype: Assembly
ms.assetid: fa87cbde-71cc-4f21-bc07-f8ba2b6a0a3f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6ded82d7db8e7dade5c203a348375812b4e04dfc
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88478626"
---
# <a name="settruststorepassword-method-sqlserverdatasource"></a>setTrustStorePassword-Methode (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Legt das Kennwort fest, das zum Überprüfen der Integrität der trustStore-Daten verwendet wird.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
public void setTrustStorePassword(java.lang.String trustStorePassword)  
```  
  
#### <a name="parameters"></a>Parameter  
 *trustStorePassword*  
  
 Eine **Zeichenfolge** mit dem Kennwort, das zum Überprüfen der Integrität der trustStore-Daten verwendet wird.  
  
## <a name="remarks"></a>Bemerkungen  
 Die trustStorePassword-Eigenschaft kann zusammen mit der trustStore-Eigenschaft angegeben werden, und deren Wert wird zum Prüfen der Integrität der trustStore-Datei verwendet.  
  
 Wenn die trustStore-Eigenschaft festgelegt ist, die trustStorePassword-Eigenschaft jedoch nicht festgelegt wurde, wird die Integrität von "trustStore" nicht überprüft.  
  
 Wenn die trustStore-Eigenschaft und die trustStorePassword-Eigenschaft nicht angegeben wurden, verwendet der Treiber die Java Virtual Machine (JVM)-Systemeigenschaften "javax.net.ssl.trustStore" und "javax.net.ssl.trustStorePassword". Wenn die Systemeigenschaft "javax.net.ssl.trustStorePassword" nicht angegeben wird, wird die Integrität von trustStore nicht überprüft.  
  
 Wenn die trustStore-Eigenschaft nicht festgelegt ist, die trustStorePassword-Eigenschaft jedoch festgelegt ist, verwendet der JDBC-Treiber die von "javax.net.ssl.trustStore" angegebene Datei als Vertrauensspeicher, und die Integrität des Vertrauensspeichers wird mithilfe des angegebenen trustStorePassword überprüft. Dies kann erforderlich sein, wenn in der Clientanwendung das Kennwort nicht in der JVM-Systemeigenschaft gespeichert werden soll.  
  
 Weitere Informationen zum Festlegen der Verbindungseigenschaften finden Sie unter [Setting the Connection Properties (Festlegen von Verbindungseigenschaften)](../../../connect/jdbc/setting-the-connection-properties.md).  
  
 Ab JDBC Driver 3.0 muss SQLServerDataSource.setTrustStorePassword vor dem Herstellen der Verbindung aufgerufen werden, wenn Sie SQLServerDataSource.setTrustStorePassword vor dem Binden der Datenquelleneigenschaften festlegen. Weitere Informationen finden Sie unter [SQLServerDataSource.getReference](../../../connect/jdbc/reference/getreference-method-sqlserverdatasource.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQLServerDataSource-Elemente](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [SQLServerDataSource-Klasse](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
