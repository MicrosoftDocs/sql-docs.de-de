---
description: SQLServerXAConnection-Elemente
title: SQLServerXAConnection-Elemente | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 4b61dabd-369b-460c-8450-9fe424f76541
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4676ec4ff2495468f043384d2b3e4761dbd1f725
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179963"
---
# <a name="sqlserverxaconnection-members"></a>SQLServerXAConnection-Elemente
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Die folgenden Tabellen enthalten die Elemente, die von der [SQLServerXAConnection](../../../connect/jdbc/reference/sqlserverxaconnection-class.md)-Klasse verfügbar gemacht werden.  
  
## <a name="constructors"></a>Konstruktoren  
 Keine  
  
## <a name="fields"></a>Felder  
 Keine.  
  
## <a name="inherited-fields"></a>Geerbte Felder  
 Keine.  
  
## <a name="methods"></a>Methoden  
  
|Name|BESCHREIBUNG|  
|----------|-----------------|  
|[addConnectionEventListener](../../../connect/jdbc/reference/addconnectioneventlistener-method-sqlserverpooledconnection.md)|(Geerbt von [SQLServerPooledConnection](../../../connect/jdbc/reference/sqlserverpooledconnection-class.md)) Registriert den angegebenen Ereignislistener, damit dieser benachrichtigt wird, wenn in Zusammenhang mit dem Connection-Objekt ein Ereignis auftritt.|  
|[close](../../../connect/jdbc/reference/close-method-sqlserverpooledconnection.md)|(Geerbt von [SQLServerPooledConnection](../../../connect/jdbc/reference/sqlserverpooledconnection-class.md)) Schließt die physische Verbindung, die von diesem Connection-Objekt dargestellt wird.|  
|[getConnection](../../../connect/jdbc/reference/getconnection-method-sqlserverpooledconnection.md)|(Geerbt von [SQLServerPooledConnection](../../../connect/jdbc/reference/sqlserverpooledconnection-class.md)) Erstellt ein Objekthandle für die physische Verbindung, die von diesem Connection-Objekt dargestellt wird.|  
|[getXAResource](../../../connect/jdbc/reference/getxaresource-method-sqlserverxaconnection.md)|Ruft ein [SQLServerXAResource](../../../connect/jdbc/reference/sqlserverxaresource-class.md)-Objekt ab, mit dem der Transaktions-Manager die Beteiligung dieses [SQLServerXAConnection](../../../connect/jdbc/reference/sqlserverxaconnection-class.md)-Objekts in einer verteilten Transaktion verwaltet.|  
|[removeConnectionEventListener](../../../connect/jdbc/reference/removeconnectioneventlistener-method-sqlserverpooledconnection.md)|(Geerbt von [SQLServerPooledConnection](../../../connect/jdbc/reference/sqlserverpooledconnection-class.md)) Entfernt den angegebenen Ereignislistener.|  
  
## <a name="inherited-methods"></a>Geerbte Methoden  
  
|Klasse geerbt von:|Methoden|  
|---------------------------|-------------|  
|com.microsoft.sqlserver.jdbc.SQLServerPooledConnection|addConnectionEventListener, close, getConnection, removeConnectionEventListener|  
|java.lang.Object|clone, equals, finalize, getClass, hashCode, notify, notifyAll, toString, wait|  
|javax.sql.PooledConnection|addConnectionEventListener, close, getConnection, removeConnectionEventListener|  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQLServerXAConnection-Klasse](../../../connect/jdbc/reference/sqlserverxaconnection-class.md)  
  
  
