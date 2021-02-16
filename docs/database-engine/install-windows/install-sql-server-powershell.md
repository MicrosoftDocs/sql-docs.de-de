---
title: Installieren von SQL Server PowerShell | Microsoft-Dokumentation
description: In diesem Artikel werden die SQL Server PowerShell-Komponenten beschrieben, die beim Setup installiert werden, wenn Sie SQL Server-Features auswählen, die PowerShell-Unterstützung erfordern.
ms.custom: ''
ms.date: 09/05/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: 854c0b2f-02d2-46a4-a8cc-6b7a5d191cf8
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: d42d3744307b27f1244e04f266365c9ba34941ae
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348358"
---
# <a name="install-sql-server-powershell"></a>Installieren von SQL Server PowerShell
[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Setup konfiguriert PowerShell-Komponenten automatisch.  

Die Software, die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Unterstützung für Windows PowerShell bietet, installieren Sie mithilfe von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Setup. Wenn Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Funktionen auswählen, die PowerShell-Unterstützung erfordern, installiert Setup die folgenden [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] PowerShell-Komponenten:  
  
- Die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] PowerShell-Snap-Ins. Die Snap-Ins sind DLL-Dateien, die zwei Arten der Windows PowerShell-Unterstützung für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]implementieren:  
  
  - Ein Satz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Cmdlets. Cmdlets sind Befehle, die eine bestimmte Aktion implementieren. Beispielsweise führt **Invoke-Sqlcmd** ein [!INCLUDE[tsql](../../includes/tsql-md.md)] - oder XQuery-Skript aus, das auch vom **sqlcmd** -Hilfsprogramm ausgeführt werden kann. **Invoke-PolicyEvaluation** meldet, ob [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Objekte den richtlinienbasierten Verwaltungsrichtlinien entsprechen.  
  
  - Ein [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anbieter. Mit dem Anbieter können Sie in der Hierarchie der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Objekte mithilfe eines Pfads navigieren, der einem Dateisystempfad ähnelt. Jedes Objekt ist einer Klasse der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Object-Modelle zugeordnet. Sie können die Methoden und die Eigenschaften der Klasse zur Arbeit mit den Objekten verwenden. Wenn Sie z. B. cd zu einem Datenbankobjekt in einem Pfad ausführen, können Sie die Methoden und Eigenschaften der Microsoft.SqlServer.Management.SMO.Database-Klasse verwenden, um die Datenbank zu verwalten.  
 
- Das **sqlps** -Modul, das in Windows PowerShell-Sitzungen importiert wird, um die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Snap-Ins zu laden.  
 
- [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] unterstützt das Starten von Windows PowerShell-Sitzungen aus der Struktur des Objekt-Explorers. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent unterstützt Windows PowerShell-Auftragsschritte.  
  
In Windows Server 2012 und höher sowie Windows 8 und höher ist PowerShell bereits installiert und konfiguriert. Informationen zum Installieren von Windows PowerShell finden Sie auf der Seite [Installieren von Windows PowerShell](/powershell/scripting/install/installing-windows-powershell).  

Weitere Informationen finden Sie unter   

- [SQL Server-PowerShell](../../powershell/sql-server-powershell.md)  
  
