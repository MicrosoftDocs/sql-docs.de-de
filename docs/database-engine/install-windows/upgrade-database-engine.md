---
title: Upgrade einer Datenbank-Engine | Microsoft-Dokumentation
description: Der Artikel enthält Links zu Ressourcen, die Ihnen beim Upgrade der SQL Server-Datenbank-Engine von einem früheren Release von SQL Server auf SQL Server 2019 helfen.
ms.custom: ''
ms.date: 11/04/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- compatibility [SQL Server], databases
- compatibility levels [SQL Server], after upgrade
- Database Engine [SQL Server], upgrading
ms.assetid: 3c036813-36cf-4415-a0c9-248d0a433859
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 4db158cf3e5e007ce1dade79dc49f3d34dace78f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341818"
---
# <a name="upgrade-database-engine"></a>Aktualisieren der Datenbank-Engine

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
  Die Artikel in diesem Abschnitt unterstützen Sie dabei, ein Upgrade der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbank-Engine von einer früheren Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] durchzuführen.  
  
1.  [Auswählen einer Upgrademethode für die Datenbank-Engine:](../../database-engine/install-windows/choose-a-database-engine-upgrade-method.md) Bevor Sie ein Upgrade durchführen, sollten Sie die verschiedenen Upgrademethoden kennen. Dieser Artikel beschreibt die Upgrademethoden und die jeweils zugehörigen Schritte.  
  
2.  [Planen und Testen des Upgradeplans für die Datenbank-Engine:](../../database-engine/install-windows/plan-and-test-the-database-engine-upgrade-plan.md) Nachdem Sie sich einen Überblick über die Upgrademethoden verschafft haben, können Sie eine geeignete Upgrademethode für Ihre Umgebung entwickeln und vor dem Upgrade der vorhandenen Umgebung testen. Dieser Artikel beschreibt das Entwickeln und Testen eines Upgradeplans.  
  
3.  [Abschließen des Datenbank-Engine-Upgrades:](../../database-engine/install-windows/complete-the-database-engine-upgrade.md) Nachdem das Upgrade für die Datenbank-Engine durchgeführt und die Datenbanken online gestellt wurden, sind noch weitere Schritte erforderlich. Beispielsweise müssen Sie eine neue Sicherung anfertigen, die Datenbankfunktionen so aktualisieren, dass neue Features unterstützt werden, und die Volltextkataloge neu auffüllen. Diese Schritte werden in diesem Artikel erläutert.  
  
4.  Durchführen eines Upgrades für den [Datenbank-Kompatibilitätsgrad](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#compatibility-levels-and-database-engine-upgrades) (**gilt für:**  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]): Wenn Ihre Datenbanken online sind und mit der neuen Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] verwendet werden, müssen Sie möglicherweise für den Funktionsmodus der Datenbanken ein Upgrade durchführen, um neue Features zu aktivieren. Dazu ändern Sie den Datenbank-Kompatibilitätsgrad. Dieser Vorgang kann manuell oder mithilfe des Abfrageoptimierungs-Assistenten durchgeführt werden. 

    - [Ändern des Datenbank-Kompatibilitätsmodus und Verwenden des Abfragespeichers:](../../database-engine/install-windows/change-the-database-compatibility-mode-and-use-the-query-store.md) Nachdem Sie den Datenbank-Kompatibilitätsgrad manuell geändert haben, verwenden Sie den Abfragespeicher, um die Leistung zu überwachen und mögliche Regressionen zu ermitteln. In diesem Artikel werden die empfohlenen Schritte und der Workflow beschrieben.  

    - [Ändern des Datenbank-Kompatibilitätsmodus mithilfe des Abfrageoptimierungs-Assistenten:](../../relational-databases/performance/upgrade-dbcompat-using-qta.md) Wenn Sie die Änderungen nicht manuell vornehmen möchten, können Sie den **Abfrageoptimierungs-Assistenten** verwenden, der Sie durch die empfohlenen Schritte zur Änderung des Datenbank-Kompatibilitätsgrads führt. In diesem Artikel wird dieser Vorgang erläutert. Außerdem werden Anweisungen für den Workflow des Abfrageoptimierungs-Assistenten bereitgestellt.  

    Weitere Informationen zu neuen Features und verbesserten Verhalten, die nach dem Ändern des Datenbank-Kompatibilitätsgrads verfügbar sind, finden Sie unter [Unterschiede zwischen Kompatibilitätsgraden](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#compatibility-levels-and-stored-procedures).

5.  [Nutzen Sie die Vorteile der neuen Features von SQL Server](https://www.microsoft.com/sql-server/sql-server-2019). Nachdem Sie die vorherigen Schritte abgeschlossen haben, können Sie abschließend einige der neuen Verbesserungen der Datenbank-Engine nutzen. Dieser Artikel schlägt einige dieser Erweiterungen vor und stellt Links zu weiteren Informationen bereit.  
  
  
