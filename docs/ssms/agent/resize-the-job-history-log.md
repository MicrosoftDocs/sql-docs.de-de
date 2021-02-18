---
description: Resize the Job History Log
title: Resize the Job History Log
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- jobs [SQL Server Agent], history
- resizing job history log
- size [SQL Server], job history log
- logs [SQL Server], jobs
- SQL Server Agent jobs, history
- historical information [SQL Server], jobs
ms.assetid: ddee1ce8-9d1b-4017-9894-bf7256aed95d
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 61b6bef074842a297a46df9ba548602527924873
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339818"
---
# <a name="resize-the-job-history-log"></a>Resize the Job History Log

[!INCLUDE[applies-to-version/_ssnoversion.md](../../includes/applies-to-version/sqlserver.md)]

> [!IMPORTANT]  
> In [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance) werden derzeit die meisten, aber nicht alle, SQL Server-Agent-Features unterstützt. Details dazu finden Sie unter [T-SQL-Unterschiede zwischen Azure SQL Managed Instance und SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

In diesem Thema wird beschrieben, wie Sie Größenbeschränkungen für Auftragsverlaufsprotokolle von [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] festlegen.

- **Vorbereitungen:**  

    [Sicherheit](#Security)  

- **So legen Sie Größenbeschränkungen für den Auftragsverlauf fest mit**  

    [SQL Server Management Studio](#SSMS)

## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Vorbereitungen  

### <a name="security"></a><a name="Security"></a>Sicherheit

Ausführliche Informationen finden Sie unter [Implementieren der SQL Server-Agent-Sicherheit](../../ssms/agent/implement-sql-server-agent-security.md).  

## <a name="using-sql-server-management-studio"></a><a name="SSMS"></a>Verwenden von SQL Server Management Studio

*So ändern Sie die Größe des Auftragsverlaufsprotokolls nur auf der Größe basierend*

1. Stellen Sie im **Objekt-Explorer** eine Verbindung mit einer Instanz von [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]her, und erweitern Sie dann diese Instanz.

2. Klicken Sie mit der rechten Maustaste auf **SQL Server-Agent**, und klicken Sie dann auf **Eigenschaften**.

3. Wählen Sie die Seite **Verlauf** aus, und bestätigen Sie dann, dass **Größe des Auftragsverlaufsprotokolls beschränken** aktiviert ist.

4. Geben Sie im Feld **Maximale Länge des Auftragsverlaufsprotokolls** die Anzahl von Zeilen ein, die maximal für das Auftragsverlaufsprotokoll zulässig sind.

5. Geben Sie im Feld **Maximale Zeilenanzahl pro Auftrag in Auftragsverlauf** die Anzahl von Zeilen ein, die maximal für das Auftragsverlaufsprotokoll eines Auftrags zulässig sind.

**So ändern Sie die Größe des Auftragsverlaufsprotokolls basierend auf der Zeit**

1. Stellen Sie im **Objekt-Explorer** eine Verbindung zu einer Instanz von [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]her, und erweitern Sie dann diese Instanz.  

2. Klicken Sie mit der rechten Maustaste auf **SQL Server-Agent**, und klicken Sie dann auf **Eigenschaften**.

3. Klicken Sie auf die Seite **Verlauf** und dort dann auf **Agent-Verlauf entfernen**.

4. Wählen Sie die entsprechende Anzahl der **Tage**, **Wochen** oder **Monate** aus.