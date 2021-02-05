---
description: SQLServerResultSet-Klasse
title: SQLServerResultSet-Klasse | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: eaffcff1-286c-459f-83da-3150778480c9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 729293afce0e8b6057e3348ea3d72e409a0cba1e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179898"
---
# <a name="sqlserverresultset-class"></a>SQLServerResultSet-Klasse
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Stellt ein JDBC-Resultset dar.  
  
 **Paket:** com.microsoft.sqlserver.jdbc  
  
 **Implementiert:** [ISQLServerResultSet](../../../connect/jdbc/reference/isqlserverresultset-interface.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
public final class SQLServerResultSet  
```  
  
## <a name="remarks"></a>Bemerkungen  
 Zwei Arten von Resultsets stehen zur Verfügung: clientseitige und serverseitige Resultsets.  
  
 Clientseitige Resultsets werden verwendet, wenn die Ergebnisse in den Clientprozessspeicher passen. Diese Ergebnisse bieten die höchste Geschwindigkeit und werden von [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] vollständig aus der Datenbank gelesen. Bei diesen Resultsets entstehen für die Datenbank keine zusätzlichen Lasten, da keine serverseitigen Cursor erstellt werden müssen. Allerdings können diese Resultsettypen nicht aktualisiert werden.  
  
 Serverseitige Resultsets können verwendet werden, wenn die Ergebnisse nicht in den Clientprozessspeicher passen oder wenn das Resultset aktualisierbar sein muss. Bei dieser Art von Resultset wird vom JDBC-Treiber ein serverseitiger Cursor erstellt, und die Zeilen des Resultsets werden transparent abgerufen, während der Benutzer einen Bildlauf ausführt.  
  
 Die SQLServerResultSet-Klasse bietet eine Vielzahl von Methoden, mit denen sich das Resultset mit beliebigen systemeigenen Java-Datentypen sowie mit vielen Java-Objekttypen aktualisieren lässt.  
  
 Diese Klasse unterstützt das Entpacken in die SQLServerResultSet-Klasse, die ISQLServerResultSet-Schnittstelle und die java.sql.ResultSet-Schnittstelle. Weitere Informationen finden Sie unter [Wrapper und Schnittstellen](../../../connect/jdbc/wrappers-and-interfaces.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQLServerResultSet-Elemente](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [API-Referenz für den JDBC-Treiber](../../../connect/jdbc/reference/jdbc-driver-api-reference.md)  
  
  
