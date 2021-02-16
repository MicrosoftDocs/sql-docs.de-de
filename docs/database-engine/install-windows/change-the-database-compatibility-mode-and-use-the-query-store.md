---
title: Verwenden des Abfragespeichers nach dem Upgrade
description: In diesem Artikel wird das Verwenden des Abfragespeichers zum Einrichten einer Baseline und zum Ändern des Datenbank-Kompatibilitätsgrads während eines SQL Server-Upgrades erläutert.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- query plans [SQL Server], migrating
- upgrading SQL Server, migrating query plans
- plan guides [SQL Server], migrating query plans
ms.assetid: 7e02a137-6867-4f6a-a45a-2b02674f7e65
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: ed9557558c820b3c1c6f633efc90d1237693a534
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347862"
---
# <a name="change-the-database-compatibility-level-and-use-the-query-store"></a>Ändern des Datenbank-Kompatibilitätsgrads und Verwenden des Abfragespeichers

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

In [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher werden einige Änderungen erst wirksam, nachdem der [Datenbank-Kompatibilitätsgrad](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md) geändert wurde. Dies hat verschiedene Gründe:  
  
- Da das Upgraden einen unidirektionalen Vorgang darstellt (ein Downgrade des Dateiformats ist nicht möglich), ist es sinnvoll, das Aktivieren neuer Funktionen als separaten Datenbankvorgang durchzuführen. Es ist möglich, eine Einstellung auf einen früheren Datenbank-Kompatibilitätsgrad zurückzusetzen.  Das neue Modell reduziert die Anzahl von Vorgängen, die während einer Ausfallzeit durchgeführt werden müssen.  
  
- Änderungen am Abfrageprozessor können komplexe Auswirkungen haben. Obwohl eine „positive“ Änderung am System für die meisten Workloads sehr gut sein kann, könnte sie an anderer Stelle für eine wichtige Abfrage eine unzulässige Regression verursachen. Durch die Trennung dieser Logik vom Upgradevorgang können Features, wie z. B. der Abfragespeicher, Planauswahlregressionen auf Produktionsservern schnell abschwächen oder sogar komplett vermeiden.  
  
> [!IMPORTANT]  
> Die folgenden Verhaltensweisen werden für [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] erwartet, wenn eine Datenbank angefügt oder wiederhergestellt wird, und werden nach einem direkten Upgrade erwartet:
> - War der Kompatibilitätsgrad einer Benutzerdatenbank vor dem Upgrade 100 oder höher, wird er nach dem Upgrade beibehalten.    
> - War der Kompatibilitätsgrad einer Benutzerdatenbank vor dem Upgrade 90, wird er auf 100 gesetzt, was dem niedrigsten unterstützten Kompatibilitätsgrad in [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] entspricht.    
> - Der Kompatibilitätsgrad der Datenbanken „tempdb“, „model“, „msdb“ und „Resource“ wird nach dem Upgrade auf den aktuellen Kompatibilitätsgrad festgelegt.   
> - Die „master“-Systemdatenbank behält den Kompatibilitätsgrad von vor dem Upgrade bei.    
  
Der Upgradevorgang zum Aktivieren neuer Abfrageprozessorfunktionen steht im Zusammenhang mit dem Wartungsmodell des Produkts nach der Einführung.  Einige dieser Fixes werden unter dem [Ablaufverfolgungsflag 4199](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md#4199) zur Verfügung gestellt.  Kunden, die Fixes benötigen, können diese abonnieren, ohne unerwartete Regressionen für andere Kunden zu verursachen. Das Wartungsmodell für Abfrageprozessor-Hotfixes nach der Einführung ist [hier](https://support.microsoft.com/kb/974006)dokumentiert. Ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] bedeutet das Verschieben in einen neuen Kompatibilitätsgrad, dass das Ablaufverfolgungsflag 4199 nicht mehr benötigt wird, da diese Problembehebungen (Fixes) nun standardmäßig im aktuellsten Kompatibilitätsgrad aktiviert sind. Im Rahmen des Upgradevorgangs ist es daher wichtig, sich zu vergewissern, dass 4199 nicht aktiviert ist, sobald der Upgradevorgang abgeschlossen ist.  

> [!NOTE]
> Das Ablaufverfolgungsflag 4199 ist jedoch weiterhin erforderlich, um alle neuen Abfrageprozessor-Problembehebungen zu aktivieren, die nach RTM freigegeben wurden (sofern zutreffend).
  
Der empfohlene Workflow für eine Aktualisierung des Abfrageprozessors auf die neueste Version des Codes ist in den „Verwendungsszenarien für den Abfragespeicher“ unter [Aufrechterhalten einer stabilen Leistung während des Upgrades auf das neuere SQL Server](../../relational-databases/performance/query-store-usage-scenarios.md#CEUpgrade) dokumentiert (siehe weiter unten).  
  
![Diagramm: Empfohlener Workflow für die Aktualisierung des Abfrageprozessors auf die neueste Version des Codes](../../relational-databases/performance/media/query-store-usage-5.png "query-store-usage-5") 

Ab [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] v18 können Benutzer im Abfrageoptimierungs-Assistenten durch den empfohlenen Workflow geführt werden. Weitere Informationen finden Sie unter [Upgraden von Datenbanken mit dem Abfrageoptimierungs-Assistenten](../../relational-databases/performance/upgrade-dbcompat-using-qta.md).
 
## <a name="see-also"></a>Weitere Informationen  
[Anzeigen oder Ändern des Kompatibilitätsgrads einer Datenbank](../../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md)     
[Verwendungsszenarien für den Abfragespeicher](../../relational-databases/performance/query-store-usage-scenarios.md)     
[ALTER DATABASE-Kompatibilitätsgrad &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)     
[Upgraden von Datenbanken mit dem Abfrageoptimierungs-Assistenten](../../relational-databases/performance/upgrade-dbcompat-using-qta.md)        
  
