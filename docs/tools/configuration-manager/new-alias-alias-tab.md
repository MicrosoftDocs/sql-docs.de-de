---
title: Neuer Alias (Registerkarte Alias)
description: In diesem Artikel erfahren Sie, wie Sie einen Alias erstellen, also einen alternativen Namen für eine SQL Server-Instanz, der verwendet wird, wenn eine Verbindung zu dieser Instanz hergestellt wird. Außerdem können Sie sich Beispiele ansehen, wann Aliasse verwendet werden.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 785eb6fb-f67e-449d-b1c8-c38dfbb95ef6
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: d2a34ab643359f7afca304b64328638675f8a742
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349768"
---
# <a name="new-alias-alias-tab"></a>Neuer Alias (Registerkarte Alias)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Bei einem Alias handelt es sich um einen alternativen Namen, der zum Herstellen einer Verbindung verwendet werden kann. In dem Alias eingeschlossen werden erforderliche Elemente einer Verbindungszeichenfolge. Diese Elemente werden mit einem vom Benutzer ausgewählten Namen offen gelegt. Verwenden Sie im Dialogfeld **Alias – Neu** die Seite **Alias** , um die Elemente der Verbindungszeichenfolge für einen Alias anzugeben. Informationen zum Ändern der Verbindungszeichenfolge eines vorhandenen Alias finden Sie unter [&#60;Alias&#62;-Eigenschaften &#40;Registerkarte „Alias“&#41;](../../tools/configuration-manager/alias-properties-alias-tab.md).  
  
 Alle Werte im **Eigenschaften** -Raster müssen nicht vervollständigt werden. Gültige Kombinationen variieren abhängig vom ausgewählten Protokoll. Beispiele für gültige Kombinationen finden Sie in der folgenden Liste.  
  
 **Aliasname**  
 Der Name (Alias), der als Referenz auf diese Verbindung verwendet werden soll.  
  
 **Pipename** / **Portnr.**  
 Zusätzliche Elemente der Verbindungszeichenfolge. Der Name dieses Dialogfelds unterscheidet sich je nach ausgewähltem Protokoll.  
  
 **Protokoll**  
 Das für die Verbindung verwendete Protokoll.  
  
 **Server**  
 Der Name der Instanz von [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], mit der eine Verbindung hergestellt wird  
  
## <a name="when-to-use-an-alias"></a>Wann ein Alias verwendet werden sollte  
 Standardmäßig wird von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] eine Verbindung mit einer lokalen Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mithilfe des **Shared Memory** -Protokolls, sowie mit einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf einem anderen Computer mithilfe von **TCP/IP** oder **Named Pipes** hergestellt. Erstellen Sie einen Alias, wenn Sie TCP/IP oder Named Pipes verwenden und eine benutzerdefinierte Verbindungszeichenfolge zur Verfügung stellen oder einen anderen Namen als den Servernamen für die Verbindung verwenden möchten.  
  
### <a name="examples"></a>Beispiele  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lauscht nicht wird am Standard-TCP/IP-Port 1433. Sie sollten deshalb eine Verbindungszeichenfolge mit einer anderen Portnummer zur Verfügung stellen.  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wird an der standardmäßige Named Pipe nicht gelauscht. Sie möchten deshalb eine Verbindungszeichenfolge mit einem anderen Pipenamen zur Verfügung stellen.  
  
-   Eine Anwendung ist auf die Verbindung mit einer Datenbank auf dem Server `ACCT`festgelegt. Diese Datenbank wurde aber als Instanz `ACCT` auf einem Server mit dem Namen `CENTRAL`konsolidiert. Das Ändern der Anwendung ist nicht einfach. Erstellen Sie einen Alias mit dem Namen `ACCT`mit einer Verbindungszeichenfolge, die auf `CENTRAL\ACCT`zeigt.  
  
## <a name="creating-a-valid-connection-string"></a>Erstellen einer gültigen Verbindungszeichenfolge  
 In den folgenden Themen finden Sie Beschreibungen und Beispiele für gültige Kombinationen von Aliaseigenschaften:  
  
-   [Erstellen einer gültigen Verbindungszeichenfolge mithilfe des Shared Memory-Protokolls](../../tools/configuration-manager/creating-a-valid-connection-string-using-shared-memory-protocol.md)  
  
-   [Erstellen einer gültigen Verbindungszeichenfolge mithilfe von TCP/IP](../../tools/configuration-manager/creating-a-valid-connection-string-using-tcp-ip.md)  
  
-   [Erstellen einer gültigen Verbindungszeichenfolge mithilfe von Named Pipes](/previous-versions/sql/sql-server-2016/ms189307(v=sql.130))  
  
