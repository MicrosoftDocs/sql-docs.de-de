---
description: Ändern der Spaltenreihenfolge einer Tabelle
title: Ändern der Spaltenreihenfolge einer Tabelle | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/15/2018
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- columns [SQL Server], change order in a table
- column order, change
ms.assetid: cd99ef56-9085-431a-a0fc-58e7add5399f
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 78da6d646d810462b01718523c4917705f533229
ms.sourcegitcommit: 331b8495e4ab37266945c81ff5b93d250bdaa6da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2020
ms.locfileid: "88646690"
---
# <a name="change-column-order-in-a-table"></a>Ändern der Spaltenreihenfolge einer Tabelle

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

  Sie können mit [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] die Reihenfolge der Spalten im Tabellen-Designer in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]ändern.  
  
> [!CAUTION]  
>  Das Ändern der Spaltenreihenfolge in einer Tabelle kann sich auf den Code und die Anwendungen auswirken, die von einer bestimmten Spaltenreihenfolge abhängig sind. Dies schließt Abfragen, Sichten, gespeicherte Prozeduren, benutzerdefinierte Funktionen und Clientanwendungen ein. Bedenken Sie sorgfältig die Konsequenzen, bevor Sie Änderungen an der Spaltenreihenfolge vornehmen. Best Practice ist, in der Anwendung oder auf der Abfrageebene die Reihenfolge anzugeben, in der die Spalten zurückgegeben werden sollen. Sie sollten sich nicht darauf verlassen, dass bei Verwendung von SELECT * alle Spalten in der Reihenfolge, in der sie in der Tabelle definiert worden sind, zurückgegeben werden. Geben Sie die Spalten in Abfragen und Anwendungen immer namentlich in der Reihenfolge an, in der sie angezeigt werden sollen.  
  
 **In diesem Thema**  
  
-   **So ändern Sie die Spaltenreihenfolge mit:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
  
#### <a name="to-change-the-column-order"></a>So ändern Sie die Spaltenreihenfolge  
  
1.  Klicken Sie im **Objekt-Explorer**mit der rechten Maustaste auf die Tabelle mit Spalten, die Sie neu anordnen möchten, und klicken Sie auf **Entwerfen**.  
  
2.  Markieren Sie das Feld links neben dem Namen der Spalte, deren Reihenfolge Sie ändern möchten.  
  
3.  Ziehen Sie die Spalte an eine andere Position innerhalb der Tabelle.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
 **So ändern Sie die Spaltenreihenfolge**  
  
 Diese Aufgabe wird für die Verwendung mit Transact-SQL-Anweisungen nicht unterstützt.  
  
###  <a name="TsqlExample"></a>  
