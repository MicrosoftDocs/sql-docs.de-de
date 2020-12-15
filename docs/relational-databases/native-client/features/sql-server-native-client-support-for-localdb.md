---
title: Unterstützung für LocalDB
description: Erfahren Sie, wie Sie eine Verbindung mit einer Datenbank in einer localdb-Instanz herstellen. Dies ist eine vereinfachte Version von SQL Server, die von SQL Server Native Client unterstützt wird.
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
ms.assetid: 127569d1-a9f7-49bf-a561-c084986a8871
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5a971836cd6b3dce74debb9ee0f150f6d8e20e6b
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97462031"
---
# <a name="sql-server-native-client-support-for-localdb"></a>SQL Server Native Client-Unterstützung für LocalDB
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Ab [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)]ist eine vereinfachte Version von SQL Server mit dem Namen LocalDB verfügbar. In diesem Thema wird erläutert, wie in einer LocalDB-Instanz eine Verbindung mit einer Datenbank hergestellt wird.  
  
## <a name="remarks"></a>Bemerkungen  
 Weitere Informationen zu LocalDB, einschließlich der Installation von LocalDB und der Konfiguration der LocalDB-Instanz, finden Sie unter:  
  
-   [SQL Server Express LocalDB-Verweis](../../../relational-databases/sql-server-express-localdb-reference.md)  
  
-   [SQL Server 2016 Express LocalDB](../../../database-engine/configure-windows/sql-server-express-localdb.md)  
  
 Zusammenfassend erlaubt LocalDB Ihnen Folgendes:  
  
-   Verwenden Sie **sqllocaldb.exe i** , um den Namen der Standardinstanz zu ermitteln.  
  
-   Verwenden Sie das Schlüsselwort der **AttachDBFilename** -Verbindungszeichenfolge, um anzugeben, welche Datenbankdatei der Server anfügen soll. Wenn Sie **AttachDBFilename** verwenden und den Namen der Datenbank nicht mit dem Schlüsselwort der **Database** -Verbindungszeichenfolge angeben, wird die Datenbank aus der LocalDB-Instanz entfernt, wenn die Anwendung geschlossen wird.  
  
-   Geben Sie in der Verbindungszeichenfolge eine LocalDB-Instanz an:  
  
```  
SERVER=(localdb)\v11.0  
```  
  
 Falls notwendig können Sie eine LocalDB-Instanz mit "sqllocaldb.exe" erstellen. Sie können auch "sqlcmd.exe" verwenden, um Datenbanken in einer LocalDB-Instanz hinzuzufügen und zu ändern. Beispiel: **sqlcmd -S (localdb)\v11.0**.  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQL Server Native Client-Funktionen](../../../relational-databases/native-client/features/sql-server-native-client-features.md)  
  
