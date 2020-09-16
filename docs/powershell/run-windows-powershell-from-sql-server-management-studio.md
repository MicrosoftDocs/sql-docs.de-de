---
title: Ausführen von Windows PowerShell über SQL Server Management Studio | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie eine Windows PowerShell-Sitzung im Objekt-Explorer im SQL Server Management Studio starten, wobei der Pfad zum Objektspeicherort Ihrer Wahl voreingestellt ist.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: sql-server-powershell
ms.topic: conceptual
ms.assetid: 1f841825-da1f-4062-9a81-3cdbab03845b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 3e244789ced3150c02a07749884dc5f3f0a77b9c
ms.sourcegitcommit: a9f16d7819ed0e2b7ad8f4a7d4d2397437b2bbb2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2020
ms.locfileid: "88714208"
---
# <a name="run-windows-powershell-from-sql-server-management-studio"></a>Ausführen von Windows PowerShell über SQL Server Management Studio
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Sie können Windows PowerShell-Sitzungen aus dem **Objekt-Explorer** in [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]starten. [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] startet Windows PowerShell, lädt das Modul **SqlServer** und legt den Pfadkontext auf den zugeordneten Knoten in der Struktur **Objekt-Explorer** fest.  
  

> [!NOTE]
> Es gibt zwei SQL Server PowerShell-Module: **SqlServer** und **SQLPS**. Das **SQLPS**-Modul ist zwar in der SQL Server-Installation (für die Abwärtskompatibilität) enthalten, wird jedoch nicht mehr aktualisiert. Das **SqlServer**-Modul ist das aktuellste PowerShell-Modul. Das **SqlServer**-Modul enthält aktualisierte Versionen der Cmdlets in **SQLPS** sowie neue Cmdlets zur Unterstützung der neuesten SQL-Funktionen.  
> Vorherige Versionen des **SqlServer**-Moduls *waren* in SQL Server Management Studio (SSMS) enthalten, allerdings nur in den Versionen 16.x. Das **SqlServer**-Modul muss über den PowerShell-Katalog installiert werden, damit PowerShell mit SSMS 17.0 und höher verwendet werden kann.
> Informationen zur Installation des **SqlServer**-Moduls finden Sie unter [Installieren von SQL Server PowerShell](download-sql-server-ps-module.md).



Wenn Sie die Ausführung von PowerShell für ein Objekt im **Objekt-Explorer** angeben, startet [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] eine Windows PowerShell-Sitzung, in denen die [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -PowerShell-Snap-Ins geladen und registriert wurden. Der Pfad für die Sitzung ist auf den Speicherort des Objekts voreingestellt, auf das Sie in Objekt-Explorer mit der rechten Maustaste geklickt haben. Wenn Sie beispielsweise im Objekt-Explorer mit der rechten Maustaste auf das Datenbankobjekt [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] klicken und dann **PowerShell starten**auswählen, wird der Windows PowerShell-Pfad folgendermaßen festgelegt:  
  
```  
SQLSERVER:\SQL\MyComputer\MyInstance\Databases\AdventureWorks2012>  
```  
  
## <a name="run-powershell"></a>Ausführen von PowerShell  
 **So führen Sie PowerShell in SQL Server Management Studio aus**  
  
1.  Öffnen Sie den **Objekt-Explorer**.  
  
2.  Navigieren Sie zum Knoten für das zu verarbeitende Objekt.  
  
3.  Klicken Sie mit der rechten Maustaste auf das Objekt, und wählen Sie **PowerShell starten**aus.  
  
## <a name="permissions"></a>Berechtigungen  
 Wenn PowerShell über [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] geöffnet wurde, wird die Sitzung nicht mit Administratorrechten ausgeführt, wodurch einige Aktivitäten wie Aufrufe der WMI verhindert werden können.  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQL Server-PowerShell](sql-server-powershell.md)  
  
  
