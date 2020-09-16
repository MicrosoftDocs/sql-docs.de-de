---
description: SQLServerXAResource-Klasse
title: SQLServerXAResource-Klasse | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: df957b79-536f-4db7-b6ac-3d59343559fc
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8126ecb70b0985845536d6d023937df6bbd344ff
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88458192"
---
# <a name="sqlserverxaresource-class"></a>SQLServerXAResource-Klasse
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Stellt ein XAResource-Objekt für die Verwaltung verteilter XA-Transaktionen dar.  
  
 **Paket:** com.microsoft.sqlserver.jdbc  
  
 **Erweitert:** java.lang.Object  
  
 **Implementiert:** javax.transaction.xa.XAResource  
  
## <a name="syntax"></a>Syntax  
  
```  
  
public class SQLServerXAResource  
```  
  
## <a name="remarks"></a>Bemerkungen  
 XA-Transaktionen werden in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] mithilfe von [!INCLUDE[msCoName](../../../includes/msconame_md.md)] mithilfe des Managers für verteilte Transaktionen (DTC) implementiert. Die SQLServerXAResource-Klasse ruft eine erweiterte DLL-Datei in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] mit der Bezeichnung „sqljdbc_xa.dll“ auf, die eine Verbindung mit DTC herstellt. Von SQLServerXAResource empfangene XA-Aufrufe (XA_START, XA_END, XA_PREPARE usw.) werden den entsprechenden Aufrufen der DTC-Funktionen zugeordnet.  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQLServerXAResource-Elemente](../../../connect/jdbc/reference/sqlserverxaresource-members.md)   
 [API-Referenz für den JDBC-Treiber](../../../connect/jdbc/reference/jdbc-driver-api-reference.md)  
  
  
