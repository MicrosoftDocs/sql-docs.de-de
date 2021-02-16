---
title: Die Konfigurationsoption „ADR Preallocation Factor“ | Microsoft-Dokumentation
description: In diesem Artikel wird die SQL Server-Konfigurationseinstellung für „ADR Preallocation Factor“ beschrieben.
ms.custom: ''
ms.date: 06/01/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- ADR Preallocation Factor
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 9469b5de278c01b7b256ee348380ebd1ae19cde4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347458"
---
# <a name="adr-preallocation-factor-configuration-option"></a>Die Konfigurationsoption „ADR Preallocation Factor“

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Eingeführt in SQL Server 2019.

Diese Konfigurationseinstellung ist für die [beschleunigte Datenbankwiederherstellung](../../relational-databases/accelerated-database-recovery-concepts.md) erforderlich.

Die beschleunigte Datenbankwiederherstellung (Accelerated Database Recovery, ADR) verwaltet Datenversionen zu Wiederherstellungszwecken. Diese Versionen werden als Teil verschiedener DML-Vorgänge (Data Manipulation Language = Datenbearbeitungssprache) generiert. Sie werden in einer internen Tabelle gespeichert, die als persistenter Versionsspeicher (PVS) bezeichnet wird. 

## <a name="remarks"></a>Bemerkungen  

Die Leistung kann beeinträchtigt werden, wenn Seiten für die PVS-Tabelle als Teil von DML-Benutzervorgängen im Vordergrund zugeordnet werden. Damit dies vermieden wird, gibt es einen Hintergrundthread, der Seiten vorab zuordnet und für DML-Transaktionen bereithält. Die bestmögliche Leistung wird erzielt, wenn der Hintergrundthread vorab genügend Seiten zuordnet und der prozentuale Anteil der PVS-Zuordnungen im Vordergrund nahe 0 ist. Das Fehlerprotokoll enthält Einträge mit dem Tag `PreallocatePVS`, wenn der Prozentsatz zu hoch ist und die Leistung beeinträchtigt.

Die Anzahl der Seiten, die der Hintergrundthread vorab zuordnet, basiert auf verschiedenen heuristischen Workloadverfahren. In der Regel werden aber Seiten in Blöcken von 512 Seiten gleichzeitig zugeordnet. Die Option „ADR Preallocation Factor“ ist ein Vielfaches eines solchen Blocks. Standardmäßig ist der Faktor 4. Das bedeutet, dass bei Bedarf 2048 Seiten gleichzeitig vorab zugeordnet werden können. 

Während der Hintergrundthread Workloadmuster berücksichtigt, kann dieser Faktor bei Bedarf erhöht werden, um die Leistung zu verbessern.

> [!CAUTION]
> Wenn es zu viele PVS-Vorabzuordnungen gibt, konkurrieren diese mit anderen Zuordnungen im System. Dies kann zu Leistungseinbußen führen.
>
> Bevor Sie diese Einstellung ändern, sollten Sie die Gesamtleistung des Systems testen.

## <a name="examples"></a>Beispiele  

Im folgenden Beispiel wird der Faktor für die Vorabzuordnung auf 4 festgelegt.

```tsql
sp_configure 'show advanced options', 1;
RECONFIGURE;
GO 
sp_configure 'ADR Preallocation Factor', 4;
RECONFIGURE;
GO
```

## <a name="see-also"></a>Weitere Informationen  

- [Serverkonfigurationsoptionen &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)
- [Verbesserte Wiederherstellung von Datenbanken](../../relational-databases/accelerated-database-recovery-concepts.md)
- [Verwalten der beschleunigten Datenbankwiederherstellung](../../relational-databases/accelerated-database-recovery-management.md)