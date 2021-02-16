---
title: " Timeout für die Wiederholung der ADR-Bereinigung (Serverkonfigurationsoption) | Microsoft-Dokumentation"
description: In diesem Artikel wird die SQL Server-Konfigurationseinstellung beschrieben, mit der Sie das Timeout für die Wiederholung der ADR-Bereinigung festlegen können.
ms.custom: ''
ms.date: 06/01/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- ADR cleaner retry timeout (min)
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 05502bbf760b0f8aa18d81739ce89e19e92397cb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347470"
---
# <a name="adr-cleaner-retry-timeout-min-configuration-option"></a>Timeout für die Wiederholung der ADR-Bereinigung (Serverkonfigurationsoption)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Eingeführt in SQL Server 2019.

Diese Konfigurationseinstellung ist für die [beschleunigte Datenbankwiederherstellung](../../relational-databases/accelerated-database-recovery-concepts.md) erforderlich. Die Bereinigung ist ein asynchroner Prozess, der periodisch reaktiviert wird und nicht benötigte Seitenversionen bereinigt.

Gelegentlich treten bei der Bereinigung Probleme beim Abrufen von Sperren auf Objektebene aufgrund von Konflikten mit der Benutzerworkload auf. Diese Seiten werden in einer separaten Liste nachverfolgt. Mit dem Timeout für die Wiederholung der ADR-Bereinigung (Standardwert = 15 Minuten) wird gesteuert, wie lange der Objektsperrenabruf und die Seitenbereinigung wiederholt werden sollen, bevor die Bereinigung abgebrochen wird. Eine Bereinigung mit hundertprozentigem Erfolg abzuschließen, ist entscheidend, um den Zuwachs abgebrochener Transaktionen in der Liste der abgebrochenen Transaktionen unter Kontrolle zu behalten. Kann die separate Liste im vorgeschriebenen Timeout nicht bereinigt werden, wird der aktuelle Bereinigungsvorgang abgebrochen und der nächste gestartet.

## <a name="remarks"></a>Bemerkungen  

Die Bereinigung ist in SQL Server 2019 ein Singlethreadvorgang. Das bedeutet, dass in einer SQL Server-Instanz nur jeweils eine Datenbank ausgeführt werden kann. Verfügt die Instanz über mehrere Benutzerdatenbanken, bei denen ADR aktiviert ist, sollten Sie das Timeoutintervall nicht zu sehr erhöhen, da ansonsten die Bereinigung einer Datenbank durch die Wiederholungsversuche für die Bereinigung einer anderen Datenbank verzögert werden könnte.

## <a name="examples"></a>Beispiele

In den folgenden Beispielen wird das Timeout für die Wiederholung der Bereinigung festgelegt:

```tsql
sp_configure 'show advanced options', 1;  
RECONFIGURE;
GO 
sp_configure 'ADR cleaner retry timeout', 15;  
RECONFIGURE;  
GO  
```  

## <a name="see-also"></a>Weitere Informationen  

- [Serverkonfigurationsoptionen &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)
- [Verbesserte Wiederherstellung von Datenbanken](../../relational-databases/accelerated-database-recovery-concepts.md)
- [Verwalten der beschleunigten Datenbankwiederherstellung](../../relational-databases/accelerated-database-recovery-management.md)