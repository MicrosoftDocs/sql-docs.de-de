---
title: Shared Memory-Eigenschaften
description: In diesem Artikel erfahren Sie, wie Sie das Shared Memory-Protokoll aktivieren oder deaktivieren, das Clients verwenden können, um eine Verbindung zu einer SQL Server-Instanz herzustellen, die auf demselben Computer ausgeführt wird.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- shared memory [SQL Server]
ms.assetid: dc1704da-eacd-4d26-b529-c996f958ca4b
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 8f5d913a9517ef441b50ea4bc3e8cbac5791ce49
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349644"
---
# <a name="shared-memory-properties"></a>Shared Memory-Eigenschaften
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Verwenden Sie die Seite **Protokoll** des Dialogfeldes **Eigenschaften von Shared Memory** , um das Shared Memory-Protokoll zu aktivieren oder zu deaktivieren. Shared Memory ist das am einfachsten zu verwendende Protokoll. Es weist keine konfigurierbaren Einstellungen auf. Da Clients, die das Shared Memory-Protokoll verwenden, nur eine Verbindung mit einer [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz herstellen können, die auf demselben Computer ausgeführt wird, ist dieses Protokoll bei den meisten Datenbankaktivitäten nicht hilfreich. Verwenden Sie das Shared Memory-Protokoll zur Problembehandlung, wenn Sie vermuten, dass die anderen Protokolle nicht ordnungsgemäß konfiguriert sind.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] muss neu gestartet werden, um das Protokoll zu aktivieren oder zu deaktivieren.  
  
## <a name="options"></a>Tastatur  
 **Aktiviert**  
 Mögliche Werte sind **Yes** und **No**. Das Shared Memory-Protokoll ist standardmäßig aktiviert.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Auswählen eines Netzwerkprotokolls](/previous-versions/sql/sql-server-2016/ms187892(v=sql.130))   
 [Erstellen einer gültigen Verbindungszeichenfolge mithilfe des Shared Memory-Protokolls](../../tools/configuration-manager/creating-a-valid-connection-string-using-shared-memory-protocol.md)  
  
