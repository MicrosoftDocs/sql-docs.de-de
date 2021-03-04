---
title: Hinweise zur Reihenfolge bei Massenkopiervorgängen
description: In diesem Artikel wird beschrieben, wie Hinweise zur Reihenfolge bei Massenkopiervorgängen verwendet werden.
ms.date: 06/15/2020
dev_langs:
- csharp
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: johnnypham
ms.author: v-jopha
ms.reviewer: v-daenge
ms.openlocfilehash: ba652ab9eb1cbe6564a8f1b2aa5d7309cba86ac4
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837107"
---
# <a name="order-hints-for-bulk-copy-operations"></a>Hinweise zur Reihenfolge bei Massenkopiervorgängen

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../../includes/driver_adonet_download.md)]

Massenkopiervorgänge bieten im Vergleich zu anderen Methoden für das Laden von Daten in eine SQL Server-Tabelle erhebliche Leistungsvorteile. Durch Hinweise zur Reihenfolge bei Massenkopiervorgängen kann die Leistung weiter gesteigert werden. Das Angeben von Hinweisen zur Reihenfolge bei Massenkopiervorgängen kann die Einfügezeit für sortiere Daten in Tabellen mit gruppierten Indizes verringern.

Standardmäßig geht der Masseneinfügevorgang davon aus, dass die eingehenden Daten nicht sortiert sind. SQL Server erzwingt eine vorläufige Sortierung dieser Daten, bevor ein Massenladen durchgeführt wird. Wenn Sie wissen, dass die eingehenden Daten bereits sortiert sind, können Sie Hinweise zur Reihenfolge verwenden, um dem Massenkopiervorgang die Sortierreihenfolge beliebiger Zielspalten mitzuteilen, die Teil eines gruppierten Indexes sind.
  
## <a name="adding-order-hints-to-a-bulk-copy-operation"></a>Hinzufügen von Hinweisen zur Reihenfolge bei Massenkopiervorgängen  
Im folgenden Beispiel werden Daten aus einer Quelltabelle in der Beispieldatenbank **AdventureWorks** in eine Zieltabelle derselben Datenbank massenkopiert. Ein SQlBulkCopyColumnOrderHint-Objekt wird erstellt, um die Sortierreihenfolge für die Spalte **ProductNumber** in der Zieltabelle zu definieren. Der Hinweis zur Sortierreihenfolge wird dann der SqlBulkCopy-Instanz hinzugefügt, die dann wiederum das Argument für den entsprechenden Hinweis zur Sortierreihenfolge an die resultierende `INSERT BULK`-Abfrage anfügt.

[!code-csharp [SqlBulkCopy.ColumnOrderHint#1](~/../sqlclient/doc/samples/SqlBulkCopy_ColumnOrderHint.cs#1)]

## <a name="next-steps"></a>Nächste Schritte
- [Massenkopiervorgänge in SQL Server](bulk-copy-operations-sql-server.md)
