---
title: Aliaseigenschaften (Registerkarte „Alias“)
description: Mithilfe der Registerkarte „Alias“ des Dialogfelds für Eigenschaften können Sie einen Alias konfigurieren, sodass Sie einen alternativen Namen verwenden können, wenn Sie eine Verbindung zu einer Instanz von SQL Server herstellen.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 2d1498e2-129c-4ce7-88e5-408e4037243c
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 510ee3ceb24f2e61e414282903fd8b9dee667632
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349940"
---
# <a name="ltaliasgt-properties-alias-tab"></a>&lt;Alias&gt;-Eigenschaften (Registerkarte „Alias“)

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

Bei einem Alias handelt es sich um einen alternativen Namen, der zum Herstellen einer Verbindung verwendet werden kann. In dem Alias eingeschlossen werden erforderliche Elemente einer Verbindungszeichenfolge. Diese Elemente werden mit einem vom Benutzer ausgewählten Namen offen gelegt. Verwenden Sie die Seite **Alias** im Dialogfeld **\<**Alias name**> Eigenschaften** zum Anzeigen oder Angeben der Elemente einer Verbindungszeichenfolge eines Alias.

## <a name="options"></a>Tastatur

**Aliasname**

Der Name (Alias), der als Referenz auf diese Verbindung verwendet werden soll.  

**Pipename** / **Portnr.**  

Zusätzliche Elemente der Verbindungszeichenfolge. Der Name dieses Dialogfelds unterscheidet sich je nach ausgewähltem Protokoll. Beispiele finden Sie in der folgenden Liste.  

**Protokoll**

Das für die Verbindung verwendete Protokoll.

**Server**

Dies ist der Name der Instanz von [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], mit der eine Verbindung hergestellt wird.  

## <a name="see-also"></a>Weitere Informationen

- [Erstellen einer gültigen Verbindungszeichenfolge mithilfe des Shared Memory-Protokolls](../../tools/configuration-manager/creating-a-valid-connection-string-using-shared-memory-protocol.md)
- [Erstellen einer gültigen Verbindungszeichenfolge mithilfe von TCP/IP](../../tools/configuration-manager/creating-a-valid-connection-string-using-tcp-ip.md)
- [Erstellen einer gültigen Verbindungszeichenfolge mithilfe von Named Pipes](/previous-versions/sql/sql-server-2016/ms189307(v=sql.130))