---
title: Installieren der SQL Server-Replikation | Microsoft-Dokumentation
description: Installieren Sie Replikationskomponenten mithilfe des Installations-Assistenten für SQL Server oder in einem Eingabeaufforderungsfenster.
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- components [SQL Server replication]
- command line installations [SQL Server replication]
- installing replication
- replication [SQL Server], installing
- command prompt [SQL Server replication]
ms.assetid: c50ad078-060b-4a8d-ad45-9e31a8d85729
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 30c66668feb75097cde8c508adf85ef3f18bb097
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348352"
---
# <a name="install-sql-server-replication"></a>Installieren der SQL Server-Replikation

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

Die Replikationskomponenten können mithilfe des Installations-Assistenten für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder über die Eingabeaufforderung installiert werden. Installieren Sie die Replikation, wenn Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]installieren oder wenn Sie eine vorhandene Instanz ändern.  
  
Nachdem die Replikationskomponenten installiert wurden, müssen Sie den Server konfigurieren, um die Replikation verwenden zu können. Weitere Informationen finden Sie unter [Konfigurieren der Verteilung](../../relational-databases/replication/configure-distribution.md) in der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Onlinedokumentation.  
  
>[!IMPORTANT]  
>Wenn Sie Replikationskomponenten während der Änderung einer vorhandenen Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]installieren, müssen Sie den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent nach Abschluss der Installation beenden und neu starten. Dadurch wird sichergestellt, dass der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent die Subsysteme des Replikations-Agents erkennt und Replikations-Agents in Auftragsschritten aufrufen kann.  
  
## <a name="installing-replication-by-using-setup"></a>Installieren der Replikation mithilfe des Setups  
**So installieren Sie die Replikation, wenn Sie eine neue Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]**  
  
- Wählen Sie zum Installieren der Replikationskomponenten, einschließlich der Replikationsverwaltungsobjekte (Replication Management Objects, RMO), auf der Seite **Funktionsauswahl** des Installations-Assistenten die Option **SQL Server-Replikation** aus.  
  
## <a name="installing-replication-from-the-command-prompt"></a>Installieren der Replikation an der Eingabeaufforderung  
 **So installieren Sie die Replikation, wenn Sie eine neue Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]**  
  
- Siehe [Installieren von SQL Server von der Eingabeaufforderung](./install-sql-server-from-the-command-prompt.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Installieren von SQL Server](../../database-engine/install-windows/install-sql-server.md)   
 [Installieren von SQL Server von der Eingabeaufforderung](./install-sql-server-from-the-command-prompt.md)   
 [Von den SQL Server 2017-Editionen unterstützte Features](../../sql-server/editions-and-components-of-sql-server-2017.md) und [Von den SQL Server 2019-Editionen unterstützte Features](../../sql-server/editions-and-components-of-sql-server-version-15.md)
  
