---
title: Eigenschaften der Clientprotokolle (Registerkarte Reihenfolge)
description: In diesem Artikel erfahren Sie, wie Sie Clientprotokolle aktivieren oder deaktivieren. Außerdem wird erläutert, wie Sie die Verwendungsreihenfolge von Protokollen neu festlegen, nach der sich Clients beim Herstellen einer Verbindung mit SQL Server richten.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- client protocols [SQL Server]
ms.assetid: 64fd7135-1756-4885-bed9-9ab8997ecc6c
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 18c8dfab9d2fef64ccd7e441eeab50eca3fdce97
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349850"
---
# <a name="client-protocols-properties-order-tab"></a>Eigenschaften der Clientprotokolle (Registerkarte Reihenfolge)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Verwenden Sie die Seite **Reihenfolge** im Dialogfeld **Eigenschaften der Clientprotokolle**, um die Clientprotokolle anzuzeigen und zu aktivieren.  
  
 Klicken Sie auf ein Protokoll, und klicken Sie dann auf **Aktivieren** oder **Deaktivieren** , um das ausgewählte Protokoll in die Liste **Deaktivierte Protokolle** oder **Aktivierte Protokolle** zu verschieben.  
  
 Protokolle werden in der Reihenfolge verwendet, in der sie aufgelistet sind, wobei zunächst das erste Protokoll in der Liste, dann das zweite Protokoll usw. verwendet wird. Verschieben Sie Protokolle in der Liste **Aktivierte Protokolle** nach oben oder nach unten, indem Sie auf die Schaltfläche mit dem Aufwärtspfeil bzw. Abwärtspfeil klicken. Beim Herstellen einer Verbindung mit [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] von einem Client auf diesem Computer wird zuerst immer das **Shared Memory**-Protokoll verwendet, falls dieses aktiviert ist.  
  
> [!NOTE]  
>  Diese Einstellungen werden nicht von [!INCLUDE[msCoName](../../includes/msconame-md.md)] .NET SqlClient verwendet. In der Protokollreihenfolge für .NET SqlClient steht TCP an der ersten Stelle, dann folgen Named Pipes. Dies kann nicht geändert werden.  
  
## <a name="options"></a>Tastatur  
 **Deaktivierte Protokolle**  
 Hiermit werden die Protokolle aufgelistet, die installiert sind, aber zum jetzigen Zeitpunkt nicht verwendet werden.  
  
 **Aktivierte Protokolle**  
 Hiermit werden die Protokolle aufgelistet, die für [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Clients auf diesem Computer verfügbar sind.  
  
 **>**  
 Aktiviert das aktuell hervorgehobene Protokoll im Feld **Deaktivierte Protokolle** und verschiebt es in das Feld **Aktivierte Protokolle** .  
  
 **\<**  
 Deaktiviert das aktuell hervorgehobene Protokoll im Feld **Aktivierte Protokolle** und verschiebt es in das Feld **Deaktivierte Protokolle** .  
  
 Pfeil nach oben  
 Verschiebt das markierte Protokoll in der Liste nach oben. Damit wird die Priorität erhöht, nach der von der Netzwerkbibliothek versucht wird, das ausgewählte Protokoll für Verbindungen zu verwenden.  
  
 Pfeil nach unten  
 Verschiebt das markierte Protokoll in der Liste nach unten. Damit wird die Priorität herabgesetzt, nach der von der Netzwerkbibliothek versucht wird, das ausgewählte Protokoll für Verbindungen zu verwenden.  
  
 **Shared Memory-Protokoll aktivieren**  
 Aktiviert das Shared Memory-Protokoll. Dieses wird (falls aktiviert) immer zuerst herangezogen, wenn von einem Client eine Verbindung zu [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf diesem Computer hergestellt wird.  
  
> [!NOTE]  
>  Wenn das Protokoll mithilfe eines Präfix oder als Teil der Verbindungszeichenfolge angegeben ist, wird nur das angegebene Protokoll versucht.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Auswählen eines Netzwerkprotokolls](/previous-versions/sql/sql-server-2016/ms187892(v=sql.130))  
  
