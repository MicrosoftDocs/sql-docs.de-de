---
description: Set the Polling Interval for Target Servers
title: Set the Polling Interval for Target Servers
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- interval for polling [SQL Server]
- target servers [SQL Server], polling interval
- polling interval [SQL Server]
ms.assetid: 4ffbbefa-77fb-442e-a77c-cb8c6cab9f3c
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 9ae0186143d0bf3d9fff36d9d26a04f1cab0203f
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88418046"
---
# <a name="set-the-polling-interval-for-target-servers"></a>Set the Polling Interval for Target Servers
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Azure SQL Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance) werden derzeit die meisten, aber nicht alle, SQL Server-Agent-Features unterstützt. Details dazu finden Sie unter [T-SQL-Unterschiede zwischen Azure SQL Managed Instance und SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

In diesem Artikel wird beschrieben, wie die Frequenz für den [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent festgelegt wird, mit der Informationen aktualisiert werden, die von der Masterdatenbank an die Zielserver übertragen werden. Ein Auftrag ist eine festgelegte Reihe von Aktionen, die der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent ausführt. Ein Multiserverauftrag ist ein Auftrag, der von einem Masterserver auf mindestens einem Zielserver ausgeführt wird.  
  
-   **Vorbereitungen:**  [Sicherheit](#Security)  
  
-   **Festlegen des Abrufintervalls für Zielserver mit:** [SQL Server Management Studio](#SSMS), [Transact-SQL](#TSQL)  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Vorbereitungen  
Auf jedem Server kann gleichzeitig eine Instanz des gleichen Auftrags ausgeführt werden. Jeder Zielserver ruft in regelmäßigen Abständen den Masterserver ab, lädt eine Kopie aller neuen Aufträge herunter, die dem Zielserver zugewiesen wurden, und trennt dann die Verbindung. Der Zielserver führt den Auftrag lokal aus und stellt dann erneut eine Verbindung mit dem Masterserver her, um den Auftragsergebnisstatus hochzuladen.  
  
> [!NOTE]  
> Wenn der Zielserver versucht, den Auftragsstatus durch Hochladen zu übertragen, und dabei nicht auf den Masterserver zugreifen kann, bleibt der Auftragsstatus so lange im Spooler (in der Warteschlange), bis der Masterserver wieder zur Verfügung steht.  
  
### <a name="security"></a><a name="Security"></a>Sicherheit  
Ausführliche Informationen finden Sie unter [Implement SQL Server Agent Security](../../ssms/agent/implement-sql-server-agent-security.md) und [Choose the Right SQL Server Agent Service Account for Multiserver Environments](../../ssms/agent/choose-the-right-sql-server-agent-service-account-for-multiserver-environments.md).  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMS"></a>Verwenden von SQL Server Management Studio  
**So legen Sie das Abrufintervall für Zielserver fest**  
  
1.  Stellen Sie im **Objekt-Explorer** eine Verbindung mit einer Instanz von [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]her, und erweitern Sie dann diese Instanz.  
  
2.  Klicken Sie mit der rechten Maustaste auf **SQL Server-Agent**, zeigen Sie auf **Multiserververwaltung**, und klicken Sie dann auf **Zielserver verwalten**.  
  
3.  Klicken Sie auf der Registerkarte **Status des Zielservers** auf **Anweisungen bereitstellen**.  
  
4.  Wählen Sie in der Liste **Anweisungstyp** die Option **Abrufintervall festlegen**aus.  
  
5.  Geben Sie im Feld **Abrufintervall** die Anzahl von Sekunden zwischen 10 und 28.800 ein, die verstreichen müssen, bevor der Zielserver den Masterserver abruft.  
  
6.  Führen Sie unter **Empfänger**eine der folgenden Aktionen aus:  
  
    1.  Klicken Sie auf **Alle Zielserver** , wenn für alle Zielserver dasselbe Abrufintervall gilt.  
  
    2.  Klicken Sie auf **Diese Zielserver** , wenn nicht für alle Zielserver dasselbe Abrufintervall gilt, und wählen Sie dann die Zielserver aus, für die dieses Abrufintervall verwendet werden soll.  
  
## <a name="using-transact-sql"></a><a name="TSQL"></a>Verwenden von Transact-SQL  
**So legen Sie das Abrufintervall für Zielserver fest**  
  
1.  Stellen Sie im Objekt-Explorer eine Verbindung mit einer Instanz von Database Engine (Datenbankmodul) her, und erweitern Sie dann diese Instanz.  
  
2.  Klicken Sie auf der Symbolleiste auf **Neue Abfrage**.  
  
3.  Verwenden Sie im Abfragefenster die gespeicherte Systemprozedur [sp_post_msx_operation (Transact-SQL)](https://msdn.microsoft.com/085deef8-2709-4da9-bb97-9ab32effdacf) , um das Abrufintervall für Zielserver festzulegen.  
  
## <a name="see-also"></a>Weitere Informationen  
[sysdownloadlist](../../relational-databases/system-tables/dbo-sysdownloadlist-transact-sql.md)  
  
