---
title: Erstellen einer gültigen Verbindungszeichenfolge mithilfe des Shared Memory-Protokolls
description: Hier erfahren Sie, wann für Verbindungen mit SQL Server das Shared Memory-Protokoll verwendet wird und wie Sie eine gültige Verbindungszeichenfolge für dieses Protokoll erstellen.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- connection strings [Database Engine], shared memory
- aliases [SQL Server], shared memory
ms.assetid: 5fff42e8-377f-4b40-b0c8-b02393f8a1af
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 9b67ee3355916447015da000dea1286060467ee3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349806"
---
# <a name="creating-a-valid-connection-string-using-shared-memory-protocol"></a>Erstellen einer gültigen Verbindungszeichenfolge mithilfe des Shared Memory-Protokolls
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Verbindungen mit [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] von einem Client auf dem gleichen Computer verwenden das Protokoll des gemeinsam genutzten Speicherbereichs. Shared Memory verfügt über keine konfigurierbaren Eigenschaften. Es wird immer zuerst versucht, Shared Memory zu verwenden; es ist nicht möglich, dieses Protokoll von der obersten Position der Liste **Aktivierte Protokolle** in der Liste **Eigenschaften der Clientprotokolle** zu verschieben. Das Shared Memory-Protokoll kann deaktiviert werden, was insbesondere bei der Problembehandlung eines der anderen Protokolle nützlich ist.  
  
 Sie können keinen Alias mithilfe des Shared Memory-Protokolls erstellen. Allerdings wird bei aktiviertem Shared Memory über den namentlichen Verbindungsaufbau zu [!INCLUDE[ssDE](../../includes/ssde-md.md)] eine Shared Memory-Verbindung hergestellt. Für Shared Memory-Verbindungszeichenfolgen wird das Format `lpc:<servername>[\instancename]`verwendet.  
  
## <a name="connecting-to-the-local-server"></a>Herstellen einer Verbindung mit dem lokalen Server  
 Beim Herstellen einer Verbindung mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , das auf dem gleichen Computer wie der Client ausgeführt wird, können Sie **(local)** als Servernamen verwenden. Aus Gründen der Mehrdeutigkeit wird dies nicht empfohlen, kann aber nützlich sein, wenn vom Client bekannt ist, dass er auf dem vorgesehenen Computer ausgeführt wird. Beim Erstellen einer Anwendung für mobile Benutzer mit getrennter Verbindung (beispielsweise für Verkaufspersonal, wobei [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf Laptops ausgeführt und zum Speichern von Projektdaten verwendet wird) würde beispielsweise die Verbindung eines Clients zu **(local)** immer zu dem auf dem Laptop ausgeführten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] hergestellt. Anstelle von **(local)** kann das Wort **localhost** oder ein Punkt ( **.** ) verwendet werden.  
  
## <a name="verifying-your-connection-protocol"></a>Überprüfen Ihres Verbindungsprotokolls  
 Von der folgenden Abfrage wird das Protokoll zurückgegeben, das für die aktuelle Verbindung verwendet wird.  
  
```  
SELECT net_transport   
FROM sys.dm_exec_connections   
WHERE session_id = @@SPID;  
  
```  
  
## <a name="examples"></a>Beispiele:  
 Die folgenden Namen werden mit dem lokalen Computer mithilfe des Shared Memory-Protokolls verbunden, falls dieses aktiviert ist:  
  
 `<servername>`  
  
 `<servername>\<instancename>`  
  
 `(local)`  
  
 `localhost`  
  
 Sie können keinen Alias für eine Shared Memory-Verbindung erstellen.  
  
> [!NOTE]  
>  Die Angabe einer IP-Adresse im Feld **Server** führt zu einer TCP/IP-Verbindung.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Erstellen einer gültigen Verbindungszeichenfolge mithilfe von TCP/IP](../../tools/configuration-manager/creating-a-valid-connection-string-using-tcp-ip.md)   
 [Erstellen einer gültigen Verbindungszeichenfolge mithilfe von Named Pipes](/previous-versions/sql/sql-server-2016/ms189307(v=sql.130))   
 [Auswählen eines Netzwerkprotokolls](/previous-versions/sql/sql-server-2016/ms187892(v=sql.130))  
  
