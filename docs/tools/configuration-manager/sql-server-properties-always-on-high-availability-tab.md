---
title: SQL Server-Eigenschaften (Registerkarte „Always On High Availability Tab“ (Hohe Verfügbarkeit bei Always On))
description: Aktivieren Sie in SQL Server das Feature „Always On-Verfügbarkeitsgruppen“, um die Verfügbarkeitsgruppen als Hochverfügbarkeits- und Notfallwiederherstellungslösung zu verwenden.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: d8630923-a600-4f1c-aca1-027453a3ec82
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 2de80bdd32d62d43a3316365029ec46a165e2df6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345110"
---
# <a name="sql-server-properties-always-on-high-availability-tab"></a>SQL Server-Eigenschaften (Registerkarte „Always On High Availability Tab“ (Hohe Verfügbarkeit bei Always On))
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Verwenden Sie die Registerkarte **Always On High Availability Tab** (Hohe Verfügbarkeit bei Always On) im Dialogfeld **SQL Server-Eigenschaften** im [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Konfigurations-Manager, um die Funktion für Always On-Verfügbarkeitsgruppen in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]zu aktivieren oder zu deaktivieren. Die Aktivierung von Always On-Verfügbarkeitsgruppen ist eine Voraussetzung dafür, dass eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz Verfügbarkeitsgruppen als Lösung für Hochverfügbarkeit und Notfallwiederherstellung verwenden kann.  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Voraussetzungen  
 Damit eine Serverinstanz für Always On-Verfügbarkeitsgruppen aktiviert werden kann, muss sie die folgenden Voraussetzungen erfüllen:  
  
-   Die Serverinstanz muss sich auf einem WSFC-Knoten (Windows Server Failover Clustering) befinden.  
  
-   Instanzen müssen sich im gleichen WSFC-Cluster befinden, damit sie derselben Verfügbarkeitsgruppe angehören. Eine Verfügbarkeitsgruppe kann sich nicht über mehrere WSFC-Cluster erstrecken.  
  
-   Auf der Serverinstanz muss eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Edition mit [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]-Unterstützung ausgeführt werden.  
  
-   Always On-Verfügbarkeitsgruppen sollten jeweils nur für eine Serverinstanz aktiviert werden. Warten Sie nach der Aktivierung von Always On-Verfügbarkeitsgruppen, bis der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Dienst neu gestartet wurde, bevor Sie die nächste Serverinstanz aktivieren.  
  
> [!NOTE]  
>  Informationen zur Funktionsunterstützung sowie zu zusätzlichen Voraussetzungen, Einschränkungen und Empfehlungen zu [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]finden Sie in der [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] -Onlinedokumentation.  
  
## <a name="dialog-options"></a>Dialogfeldoptionen  
 **Name des Windows-Failoverclusters**  
 Zeigt den Namen des WSFC-Clusters an, in dem der lokale Computer einen Knoten darstellt.  
  
 **Aktivieren von AlwaysOn-Verfügbarkeitsgruppen**  
 Verwenden Sie dieses Kontrollkästchen, um Always On-Verfügbarkeitsgruppen auf dieser [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz wie folgt zu aktivieren oder deaktivieren:  
  
-   Wenn dieses Kontrollkästchen leer ist, sind Always On-Verfügbarkeitsgruppen derzeit deaktiviert. Aktivieren Sie dieses Kontrollkästchen, um Always On-Verfügbarkeitsgruppen zu aktivieren, klicken Sie auf **OK**, und starten Sie den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Dienst manuell erneut.  
  
-   Wenn dieses Kontrollkästchen bereits aktiviert ist, sind Always On-Verfügbarkeitsgruppen derzeit aktiviert. Um Always On-Verfügbarkeitsgruppen zu deaktivieren, deaktivieren Sie das Kontrollkästchen, und klicken Sie auf **OK**. Dadurch wird die Serverinstanz neu gestartet.  
  
    > [!TIP]  
    >  Nach der Deaktivierung von Always On-Verfügbarkeitsgruppen sollten Sie alle lokalen Verfügbarkeitsreplikate aus der Serverinstanz entfernen. Nachdem Sie das letzte Replikat einer bestimmten Verfügbarkeitsgruppe entfernt haben, sollten Sie auch die Gruppe entfernen.  
  
## <a name="ui-element-list"></a>Liste der Benutzeroberflächenelemente  
  
> [!NOTE]  
>  Weitere Informationen zu den Schritten, die im Anschluss an die Deaktivierung von Always On-Verfügbarkeitsgruppen erforderlich sind, sowie Informationen zum Erstellen und Konfigurieren von Verfügbarkeitsgruppen finden Sie in der [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] -Onlinedokumentation.  
  
  
