---
title: Named Pipes-Eigenschaften
description: Verwenden Sie die Seite „Protokoll“ im Dialogfeld „Named Pipes Properties“ (Eigenschaften von Named Pipes), um die Named Pipe anzuzeigen oder zu ändern, auf die SQL Server beim Verwenden des Named-Pipes-Protokolls lauscht.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- pipes [SQL Server]
- listening [SQL Server], pipes
- pipes [SQL Server], listening on pipes
- Named Pipes [SQL Server], listening on pipes
ms.assetid: a5fd5b8e-f889-485b-89e3-d4010ec4c6ec
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 1ad975d9b53b201d3975377ca32e2a06d8ee30b1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349782"
---
# <a name="named-pipes-properties"></a>Named Pipes-Eigenschaften
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Verwenden Sie die Seite **Protokoll** im Dialogfeld **Named Pipes-Eigenschaften**, um die Named Pipe anzuzeigen oder zu ändern, an der [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lauscht, wenn Sie das Named Pipes-Protokoll verwenden.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] muss neu gestartet werden, um das Protokoll zu aktivieren oder zu deaktivieren oder die Named Pipe zu ändern.  
  
## <a name="options"></a>Tastatur  
 **Aktiviert**  
 Mögliche Werte sind **Yes** und **No**.  
  
 **Pipename**  
 Gibt die Named Pipe an, an der von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gelauscht wird. Standardmäßig lauscht [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] folgende: `\\.\pipe\sql\query` nach der Standardinstanz und `\\.\pipe\MSSQL$<instancename>\sql\query` nach einer benannten Instanz. Dieses Feld ist auf 2047 Zeichen begrenzt.  
  
## <a name="creating-an-alternate-named-pipe"></a>Erstellen einer alternativen Named Pipe  
 Zum Ändern der Named Pipe geben Sie im Feld **Pipename** einen neuen Pipenamen ein. Beenden Sie anschließend [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], und führen Sie einen Neustart aus. Da **sql\query** bekannt ist als Named Pipe, die von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]verwendet wird, kann eine Änderung der Pipe das Risiko eines Angriffs durch böswillige Programme reduzieren.  
  
### <a name="example"></a>Beispiel  
 Geben Sie **\\\\.\pipe\unit\app** ein, um an der Pipe **unit\app** zu lauschen.  
  
 Geben Sie **\\\\.\pipe\acct** ein, um an der Pipe **acct** zu lauschen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Aktivieren oder Deaktivieren eines Servernetzwerkprotokolls](../../database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md)   
 [Auswählen eines Netzwerkprotokolls](/previous-versions/sql/sql-server-2016/ms187892(v=sql.130))   
 [Erstellen einer gültigen Verbindungszeichenfolge mithilfe von Named Pipes](/previous-versions/sql/sql-server-2016/ms189307(v=sql.130))  
  
